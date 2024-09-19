# Gestión de Inventario Dislicores
---

## Miembros 
---

Yenifer Briceño - Data Analytics 
Antonio Astudillo - Data Engineer
Juan Bermudez - Data Scientist
Dante Arola - Data Analytics 
Alexandro Lugo - Data Scientist

## Desafios
---

Nuestra iniciativa se centra en resolver los desafíos relacionados con la gestión de inventarios que enfrenta Dislicores, una distribuidora de bebidas. Mediante el análisis de datos y la implementación de modelos de optimización, buscamos incrementar la eficiencia en la administración de inventarios, mejorar la rotación de productos y el descubrimientos de diferentes KPIs

## Objetivos
---

+ Minimizar los desabastecimientos mediante un seguimiento eficiente del stock.
+ Automatizar los procesos de reposición de inventario.
+ Evaluar y mejorar el rendimiento de proveedores.
+ Identificar productos de baja rotación y ajustar las estrategias de compra.
+ Fomentar una gestión de inventarios más sostenible.
+ Mejorar la planificación a largo plazo mediante pronósticos basados en datos históricos.

## Origenes de los datos 
---

Fuente original: Inventory Analysis Case Study

Precios de compra 2017dic.csv
IniciarInvFINAL12312016.csv
FinInvFINAL12312016.csv
FacturaCompras12312016.csv
ComprasFINAL12312016.csv
VentasFINAL12312016.csv

## Descripción del repositorio
---

+ Automatizacion de Watchdog
+ Diagrama Entidad-Relacion
+ ETL-EDA
+ CSV Finales
+ CSV Iniciales
+ README.md

## Carpeta Automatizacion con la libreria Watchdog 
---

En esta carpeta se encuentra el archivo con extensión (.ipynb), el cual contiene el proceso completo de automatización para la carga incremental de datos utilizando Python. Este proceso está diseñado para transferir los nuevos registros de manera eficiente hacia la base de datos, que está alojada en SQL Server. A través de este archivo, se implementan las distintas etapas de la automatización, asegurando que solo se carguen los datos que no existían previamente en la base de datos, optimizando así el rendimiento y la gestión de los recursos.

## Carpeta Diagrama Entidad-Relacion
---

Esta carpeta contiene una imagen del Diagrama de Entidad-Relación (DER) de la base de datos, generada directamente desde SQL Server Management. La imagen muestra las relaciones entre las tablas y ofrece una visión clara de la estructura de la base de datos.

## Carpeta ETL-EDA
---

En esta carpeta se han cargado los seis archivos (.ipynb) correspondientes a los procesos de Análisis Exploratorio de Datos (EDA) y la Extracción Transformación y Carga (ETL) para cada uno de los dataframes utilizados en la base de datos. Estos archivos documentan los pasos realizados para analizar y preparar los datos antes de su integración a SQL.

## CSV Finales
---

En esta carpeta se almacenarán los seis archivos finales en formato (.csv), que corresponden a cada dataframe después de haber completado el proceso de ETL. Estos archivos contienen los datos ya extraídos, transformados y listos para su posterior análisis o integración en otros sistemas.

## CSV Iniciales 
---

En esta carpeta se guardarán los archivos iniciales en formato (.csv), correspondientes a cada dataframe antes de ser procesados mediante el ETL. Estos archivos contienen los datos originales que serán utilizados como punto de partida para la extracción, transformación y carga.