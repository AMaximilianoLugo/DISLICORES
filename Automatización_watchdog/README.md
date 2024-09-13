Código para Automatización y Carga Incremental de Datos usando Watchdog 🚀
Este script en Python está diseñado para supervisar una carpeta específica en tu sistema de archivos. Utiliza Watchdog para detectar la creación o modificación de archivos CSV en tiempo real y luego carga de forma incremental estos datos en la base de datos InventarioBD alojada en SQL Server.

Librerías utilizadas
watchdog: Monitorea los cambios en el sistema de archivos.
sqlalchemy: Gestiona la conexión y las interacciones con SQL Server.
pandas: Realiza la manipulación y análisis de los datos.
os: Proporciona funciones para interactuar con el sistema operativo.
time: Controla los intervalos de tiempo y pausas en el proceso.
