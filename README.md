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

Para la visión más general se han explorado los siquientes puntos:

 - El df está compuesto por 24 columnas y 25723 filas.
 - El memory usage: 22.8 MB
 - Sólo dos columnas son numéricas: Year y Original order y por tanto, 22 son categóricas.
 - También se ha comprobado que todas las columnas presentan una gran cantidad de valores nulos.
 
   *meter imagen 1
   
   
     

     
     




## Eliminación de duplicados

A la hora de explorar y limpiar los duplicados, hemos tenido en cuenta dos factores:

 1. Muchas filas eran en su totalidad valores Nan, por lo que se han borrado 17.020 filas que no aportaban valor por cumplir dicha condición.

 2. Tras la anterior limpieza, hemos procedido a eliminar las filas duplicadas y tras todo este procedo conjunto el df ha pasado de tener 25723 filas a 6311.
 
Además, tal y como se se ve en la siguiente gráfica, ahora no todas las columnas tienen nulos (o algunas tienen un número relativamente pequeño respecto a la muestra). Por ello, vamos a explorar que columnas son y tratar de identificar la mejor solución para cada una.

*Insertar gráfica 2


## Exploración de columnas.

Antes de comenzar la exploración individual de cada columna, primero realizaremos un análisis más genérico para tener una idea general de la situación de las columnas del df:

La **media de valores** nulos por columna es de 1032, pero hay outliers por lo que el parámetro de guía para seguir explorando debería ser la **mediana**, que es de 48.

Teniendo esta información,como aún tenemos una muestra relativamente grande,vamos a ver la distribución de Nan por filas creando una variable 'nan_filas' y añadiremos una columana **nueva con el número total** de nans que hay en cada fila.  





## Columnas constantes o de baja varianza

## Outliers

## Colinealidad

## DataFrame Final

## BONUS - Conclusiones