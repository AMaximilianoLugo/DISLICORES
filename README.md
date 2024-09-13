## Código para Automatización y Carga Incremental de Datos usando Watchdog 🚀

Este script en Python está diseñado para supervisar una carpeta específica en tu sistema de archivos. Utiliza Watchdog para detectar la creación o modificación de archivos CSV en tiempo real y luego cargar de forma incremental estos datos en la base de datos **InventarioBD** alojada en SQL Server.

### Librerías utilizadas:
- **watchdog**: Monitorea los cambios en el sistema de archivos.
- **sqlalchemy**: Gestiona la conexión y las interacciones con SQL Server.
- **pandas**: Realiza la manipulación y análisis de los datos.
- **os**: Proporciona funciones para interactuar con el sistema operativo.
- **time**: Controla los intervalos de tiempo y pausas en el proceso.

---

## 1. Configuración de la base de datos:

Se especifica la configuración del servidor SQL, el nombre de la base de datos, el usuario y la contraseña. Se utiliza **SQLAlchemy** para crear una cadena de conexión que permite la interacción con la base de datos SQL Server.

```python
# Configuración de la base de datos
database_config = {
    'server': 'dislicores-1.cxc0gcgy4dsz.us-east-2.rds.amazonaws.com,1433',  # Cambia esto por tu servidor
    'database': 'dislicores-1',  # Cambia esto por tu base de datos
    'username': 'admin',  # Cambia esto por tu usuario
    'password': 'KingKong9#_9+',  # Cambia esto por tu contraseña
}

# Crear cadena de conexión
connection_string = f"mssql+pyodbc://{database_config['username']}:{database_config['password']}@{database_config['server']}/{database_config['database']}?driver=ODBC+Driver+17+for+SQL+Server"
engine = create_engine(connection_string)
```


## 2. Definición de la Clase que Maneja los Eventos:

Se crea la clase DataHandler, la cual extiende de FileSystemEventHandler proporcionada por la librería Watchdog. Esta clase incluye métodos para gestionar los eventos que ocurren cuando se crean o modifican archivos (métodos on_created y on_modified). El método process_new_file es responsable de cargar el archivo CSV, procesar la información y actualizar la base de datos con los datos nuevos.

```python
class DataHandler(FileSystemEventHandler):
    def __init__(self, engine):
        self.engine = engine

    def on_created(self, event):
        if event.is_directory or not event.src_path.endswith(".csv"):
            return
        self.process_new_file(event.src_path)

    def on_modified(self, event):
        if event.is_directory or not event.src_path.endswith(".csv"):
            return
        self.process_new_file(event.src_path)

    def process_new_file(self, file_path):
        try:
            new_data = pd.read_csv(file_path)
            new_data = new_data.drop(columns=['Unnamed: 0'], errors='ignore')

            # Convertir columnas que contengan la palabra 'date' a formato datetime
            for column in new_data.columns:
                if 'date' in column.lower():
                    new_data[column] = pd.to_datetime(new_data[column], errors='coerce')

            table_name = os.path.splitext(os.path.basename(file_path))[0]

            # Obtener el nombre de la columna ID específica
            id_column = self.get_id_column(table_name)

            # Verificar que la columna ID exista en el DataFrame
            if id_column not in new_data.columns:
                print(f"Error: La columna '{id_column}' no existe en el archivo {file_path}")
                return

            # Cargar los datos existentes de la base de datos
            existing_data = pd.read_sql(f"SELECT * FROM {table_name}", self.engine)

            # Identificar filas nuevas (basado en id_column)
            new_rows = new_data[~new_data[id_column].isin(existing_data[id_column])]

            # Verificar integridad referencial para 'Inventario_inicialID' si es necesario
            if table_name == 'Tabla_VentasFinal':
                inventario_inicial_ids = pd.read_sql("SELECT Inventario_inicialID FROM Tabla_InventarioInicial", self.engine)
                valid_ids = inventario_inicial_ids['Inventario_inicialID']
                new_rows = new_rows[new_rows['Inventario_inicialID'].isin(valid_ids)]

            # Insertar datos nuevos en la base de datos
            if not new_rows.empty:
                new_rows.to_sql(table_name, self.engine, if_exists='append', index=False)
                print(f"Se han insertado {len(new_rows)} filas nuevas en la tabla {table_name}")
            else:
                print(f"No hay filas nuevas para insertar en la tabla {table_name}")

        except Exception as e:
            print(f"Error al procesar el archivo {file_path}: {e}")

    def get_id_column(self, table_name):
        id_columns = {
            'Tabla_VentasFinal': 'VentasID',
            'Tabla_Detallecompras': 'Detalle_compraID',
            'Tabla_InventarioFinal': 'Inventario_FinalID',
            'Tabla_InventarioInicial': 'Inventario_inicialID',
            'Tabla_Producto': 'MarcaID',
            'Tabla_Compras': 'CompraID',
        }
        return id_columns.get(table_name, 'id')
```

## 3. Función para Procesar Archivos Existentes:
Esta función llamada process_existing_files se encarga de recorrer todos los archivos CSV que ya están en la carpeta de monitoreo, y procesarlos según sea necesario.

```python
def process_existing_files(path, handler):
    for filename in os.listdir(path):
        if filename.endswith(".csv"):
            file_path = os.path.join(path, filename)
            handler.process_new_file(file_path)
```

4. Inicialización y Ejecución del Observador:
En este apartado, se establece el flujo principal del script:

Se especifica el directorio que será monitoreado.
Se crea un objeto de la clase DataHandler para gestionar los eventos.
Los archivos CSV ya presentes en la carpeta se procesan automáticamente.
Se configura el observador de cambios (Observer) para vigilar continuamente el directorio indicado.
Finalmente, el observador comienza a monitorear y permanece activo hasta que se detiene de forma manual.

```python
if __name__ == '__main__':
    path_to_watch = r"C:\Users\Juan Daniel Bermudez\OneDrive\Escritorio\M6\inventory_prueba\CSV_Finales"  # Cambia esto por tu ruta
    event_handler = DataHandler(engine)

    # Procesar archivos existentes en la carpeta
    process_existing_files(path_to_watch, event_handler)

    observer = Observer()
    observer.schedule(event_handler, path=path_to_watch, recursive=True)
    observer.start()
    print(f"Observando cambios en: {path_to_watch}")

    try:
        while True:
            time.sleep(1)
    except KeyboardInterrupt:
        observer.stop()
    observer.join()
    print("El observador ha sido detenido.")
```