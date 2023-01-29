# Project-Shark-Attacks (Data Cleaning)

![Portada](https://github.com/BeaZatarain/Project-Shark-Attacks/blob/main/imagenes/titulo.jpg)



En estre proyecto consiste en realizar un proceso de limpieza de datos de una "caótica" base de datos de ataques de tiburón. 

A su vez, tiene como restricción el borrado de columnas y el requisito añadido de que tras la limpieza, el datafreame contenga al menos, 1500 filas. 

Adicionalmente, este proyecto tiene como objetivo analizar si:
 1. Los tiburones atacan a más personas que a barcos 
 2. Las personas atacadas asumían un riesgo por la actividad que hacían en     el momento del ataque o suele haber más ataques aleatorios.
 3. Como se distribuye la fatalidad en función del ataque.


## Importaciones y carga de datos.

El dataframe sobre el que vamos a trabajar proviene de Kaggle en formato csv y será trabajado en un jupiter notebook. 

Las principales librerías que se han utilizado son Pandas y Numpy.

Además, con el objetivo de agilizar todo el proceso se han importado también:

     import warnings
     warnings.filterwarnings ('ignore')

     pd.set_option('display.max_columns', None)
     pd.set_option('display.max_rows', None)
     
Por último se ha importado el archivo csv con la función de pandas pd.read_csv y se ha procedido a su vez a realizar una copia del df original:

     sharks_ori = pd.read_csv('../Project-Shark-Attacks/data/attacks.csv',      encoding = 'latin1')

     sharks = sharks_ori.copy()


## Exploración general.

En primer lugar, se ha realizado una exploración general del df para tener una visión general de su contenido en cuanto a numero de columnas y filas, número de valores nulos...etc

Para la visión más general se ha utilizado lo siguiente:

     sharks.head() -> para poder visualizar el encabezado con las columnas del dataframe y tener un vistazo general de los datos que contiene          cada una. 
     
     sharks.shape -> (25723, 24) con lo que hemos podido observar que el df tenía 24 columnas y 25723 filas. 
     
     sharks.info(memory_usage='deep')-> para tener a simple vista el número de valores no nulos de cada columna, el tipo de dato de cada columna, dtypes y el memory usage (22.8 MB)
     
     sharks.describe(include='all').T -> para realizar una exploración estadística del df.
     

     
     




## Eliminación de duplicados

## Exploración de columnas.

-fillna/borrado de registros



## Columnas constantes o de baja varianza

## Outliers

## Colinealidad

## DataFrame Final

## BONUS - Conclusiones