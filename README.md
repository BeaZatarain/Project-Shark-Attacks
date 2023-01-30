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

 - El df está compuesto por **24 columnas y 25723 filas**.
 - El memory usage: **22.8 MB**
 - Sólo dos columnas son numéricas: Year y Original order y por tanto, 22 son categóricas.
 - También se ha comprobado que todas las columnas presentan una gran cantidad de valores nulos.
 
![gráfica 1 nan](https://github.com/BeaZatarain/Project-Shark-Attacks/blob/main/imagenes/grafica_nan1.png)
   
   
     

     
     




## Eliminación de duplicados

A la hora de explorar y limpiar los duplicados, hemos tenido en cuenta dos factores:

 1. Muchas **filas eran en su totalidad valores Nan**, por lo que se han borrado 17.020 filas que no aportaban valor por cumplir dicha condición.

 2. Tras la anterior limpieza, hemos procedido a eliminar las **filas duplicadas** y tras todo este procedo conjunto el df ha pasado de tener 25723 filas a 6311.
 
Además, tal y como se se ve en la siguiente gráfica, ahora no todas las columnas tienen nulos (o algunas tienen un número relativamente pequeño respecto a la muestra). Por ello, vamos a explorar que columnas son y tratar de identificar la mejor solución para cada una.

![gráfica 2 nan](https://github.com/BeaZatarain/Project-Shark-Attacks/blob/main/imagenes/grafica_nan2.png)


## Exploración de columnas.

Antes de comenzar la exploración individual de cada columna, primero realizaremos un análisis más genérico para tener una idea general de la situación de las columnas del df:

La **media de valores** nulos por columna es de 1032, pero hay outliers por lo que el parámetro de guía para seguir explorando debería ser la **mediana**, que es de 48.

Teniendo esta información,como aún tenemos una muestra relativamente grande,vamos a ver la distribución de Nan por filas creando una variable 'nan_filas' y añadiremos una columana **nueva con el número total** de nans que hay en cada fila. Los valores que toman esta columna sumatorio de nan por fila tienen como mínimo 2 y como máximo 23 valores nan por fila. 

Con ello, hemos tomado la decisión de eliminar aquellas filas cuya suma tota de nans sea igual a la mediana de la columa. De esta forma, eliminamos todas aquellas filas que tengan al menos 4 valores no nulos. 

Con esta solución general, hemos eliminado 9 filas que no aportaban valor.

Por último, antes de comenzar con la exploración individual de cada columna, hemos **unificado los nombres de las columnas** para evitar problemas de caracteres especiales y case sensitive.

### LIMPIEZA COLUMNAS UNNAMED 22 Y UNNAMED23

En este caso estamos ante dos columnas con casi todos los valores NaN por lo que no aportarán valor al análisis y no nos interesan. Por otro lado, como son columnas categóricas,  nos hemos limitado a rellenar el NaN con 'unknown'.

### LIMPIEZA COLUMNA NAME

De nuevo, la columna name no es una columna que nos interese para nuestro objetivo, por lo que nos limitaremos a rellenar los NaN con 'Unknown'.

### REPASO DE LIMPIEZA DE NAN GENERAL

Tras la limpieza de las tres anteriores columnas, volvemos a realizar un intento de limpieza general de valores nulos. En este caso, hemos calculado el porcentaje medio de valores nulos por columna y hemos tomado la decisión de borrar las filas de las columnas con un porcentaje muy pequeño de Nan respecto al total. En concreto, los que tengan menos de un 1%. 

Con esta decisión, las columnas 'casenumber', 'hrefformula', 'injury','country','hrefformula', 'year','type' e'investigatororsource' estarían sin valores nulos. 


### LIMPIEZA COLUMNA CASENUMBER

La columna casenumber es un ID por lo que cada caso es único, por lo que sustituiremos todos los valores de 1-n.

### LIMPIEZA COLUMNA DATE

Como en todos los valores de casenumber.1 aparece la fecha, crearemos una nueva columna que se llame date_ok (dejando solo la fecha de casenumber.1 ). Si la igualación no consigue dejar sólo lo que sería una fecha, devuelve Nan. Por ello, en los que no tenemos clara la fecha del ataque, borramos las filas.

Por otro lado, aunque todas los registros de nuestra columna siguen la misma estructura de fecha 'YY.MM.DD', en algunos casos el mes y el día aparecen como 0, por lo que también procederemos a limpiar esas fechas al considerarlas como inválidas. Con ello, nos hemos quedado únicamente con los registros que contienen fechas válidas.

Tras la limpieza de esta columna, el df cuenta con 4368 filas y 26 columnas.

### LIMPIEZA COLUMNA YEAR

Para limpiar la columna year habría que quitar '.0', por lo tanto, la solución sería convertir los valores de float a integer ya que 

### LIMPIEZA COLUMNA TYPE


## Columnas constantes o de baja varianza

## Outliers

## Colinealidad

## DataFrame Final

## BONUS - Conclusiones