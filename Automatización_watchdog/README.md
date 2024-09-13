Código para Automatización y Carga Incremental de Datos usando Watchdog 🚀
Este script en Python está diseñado para supervisar una carpeta específica en tu sistema de archivos. Utiliza Watchdog para detectar la creación o modificación de archivos CSV en tiempo real y luego cargar de forma incremental estos datos en la base de datos InventarioBD alojada en SQL Server.

Librerías utilizadas:
watchdog: Monitorea los cambios en el sistema de archivos.
sqlalchemy: Gestiona la conexión y las interacciones con SQL Server.
pandas: Realiza la manipulación y análisis de los datos.
os: Proporciona funciones para interactuar con el sistema operativo.
time: Controla los intervalos de tiempo y pausas en el proceso.

1. Configuración de la base de datos:
Se especifica la configuración del servidor SQL, el nombre de la base de datos, el usuario y la contraseña. Se utiliza SQLAlchemy para crear una cadena de conexión que permite la interacción con la base de datos SQL Server.

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
