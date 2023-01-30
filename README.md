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

Esta columna no tiene Nan, por lo que para la limpieza de esta columna tenemos que tener en cuenta varios factores:

  1. La **variable 'sea disaster'** no guarda ninguna relación a priori con ataques de tiburón, ya que, se definen más como accidentes marítimos de barcos (ejemplo el **Titanic**). Por lo que procederemos a eliminarlos de la columna.
  2. las **variables 'boating' y 'boat'** hacen referencia a ataques de tiburones a barcos no a humanos, por lo que unificaremos los valores en 'boating' (ya que hay más cantidad de esta).
  3.**¿Qué quiere decir Invalid?** hace referencia a accidentes de personas (a veces no se sabe ni siquiera si han sido en el mar) que no se pueden considerar como un ataque de tiburón. Por lo que la decisión tamada es eliminar dichas filas ya que distorsionan la muestra al no estar relacionadas con tiburones.
  4. ¿Qué quiere decir que es **Questionable?** -> sólo tenemos dos casos, parece que significa que había un tiburón relacionado pero que o bien no provocó ninguna herida a una persona o dañó una tabla de surf. Por tanto, la solución es eliminar la fila que no hubo heridos e incluir en 'Boating la tabla dañada'.
  5. ¿Qué diferencia hay entre un ataque provocado o no provocado según los datos? -> esta distincción parece indicar que el ataque provocado hace referencia a casos en los que el animal ha sido pescado/intentado pescar o herido por alguna acción  humana mientras que los no provocados son aquellos en los que no ha intervenido ningún acto humano. 

De cara a nuestro análisis puede ser interesante diferenciar si los ataques han sido a personas o a barcos/vehículos marítimos (tablas de surf, barcas, remos...) por lo que crearemos una columna 'injured' que distinga entre ataque a human y . La **nueva columna en cuestión, se llamará 'injured'**.
  
### LIMPIEZA DE COLUMNA COUNTRY (limpieza conjunta de columnas area y location)

OJO: tanto en la columna area como location, seguimos teniendo Nan por lo que en primer lugar, rellenaremos esos valores con 'unknown' al tratarse de columnas categóricas.

En una primera exploración parece que la mayoría de países están correctamente, salvo ciertos casos en los que especifica que fue en dos países o entre dos lugares. Por ello, para poder dejar la información de la columna country lo más limpia y completa posible, nos apoyaremos en las columnas de area y location. 

Como son pocos casos, comprobaremos con las columnas de area y location (también google) y rellenaremos qué país es en cada caso.

En algunos casos sólo especifica océanos, realizaremos una exploración previa. Sin embargo como se puede obsevar abajo, las columnas de area y location no dan suficiente información para poder localizar el país por lo que eliminaremos estas filas.

En definitiva, eliminaremos tanto las que contengan OCEAN como los unknown.

### LIMPIEZA DE LA COLUMNA ACTIVITY

Primero reemplazaremos los Nan por: si type es provoked por 'risky_activity' (puede ser pesca, caza...) y si es unprovoked lo sustituiremos por 'el valor más repetido'(la moda, que en este caso es surfing.

Como se puede observar siguen quedando 30 Nan, Efectivamente, tras realizar una comprobación, vemos que no se han rellenado porque el valor equivalente de type era Boating. Por ello, los rellenaremos con la palabra 'Boating' para seguir teniendo identificado cuando un tiburón ha atacado a un humano o a un barco.


### LIMPIEZA DE LA COLUMNA FATAL Y/N

Se trata de una columna en la que si el valor Y el ataque ha sido mortal y si toma el valor N indica que no fue fatal.

Tras realizar una comprobación, parece que esta columna está relacionada con la columna injury, donde especifica la fatalidad del ataque, por lo que rellenaremos los valores Nan completar los datos en función de la descripción dada en la columna injury.

Como tenemos identificados los índices en los que estan los Nan, rellenaremos todos con el valor más repetido ('N') y luego reemplazaremos el único ataque fatal por su indice 6104 con 'Y'.

Para terminar la limpieza de esta columna tenemos que:
   1. Unificar ' N' con 'N' con función replace.
   2. Realizar el mismo proceso anterior para ver si podemos comprobar si el ataque fue fatal o no con los valores 'M','2017' y 'UNKNOWN'. Sin embargo, al mostrar la equivalencia con el valor 'UNKNOWN' todos los valores aparecen como 'no details' por lo que al no poder confirmar la fatalidad o no del ataque borraremos las 30 filas.
   
### LIMPIEZA DE LA COLUMNA SEX

Comprobamos la relación de los nan cuando la víctima es un barco con la columna type. Como podemos observar, hay casos en los que efectivamente la víctima era un barco y otros en los que eran personas (provocado o no).

Como para nuestro análisis nos interesa saber más si los tiburones atacan más a barcos o a personas, rellenaremos los nan con 'boat' cuando la columna type sea 'Boating' y 'unknown' en el resto de casos (que serán personas, pero nos nos afectará su género).

Una vez rellenados los Nan... El procedimiento de limpieza sería unificar los valores M (hay M y 'M ') y reemplazar la fila donde el valor es N por 'unknown'.

### LIMPIEZA DE LA COLUMNA AGE

Se trata de una columna que además de contener 1475 valores Nan, presenta edades representadas con integers, caracteres especiales o palabras y frases.

Procedimiento de limpieza, relleno de nulos:

Aunque la variable edad no sea significativa para nuestro análisis, para una mayor consistencia de los datos, rellenaremos los nulos con 'boat' cuando el ataque haya sido a un barco y con 'unknown' en el resto de casos.

Por otro lado, nos hemos encontrado con valores 'extraños' por lo que intentaremos unificarlos:

 - Todavía nos podemos encontrar casos en los que la edad no tenga sentido porque fue un ataque a un barco, por lo que reemplazaremos los datos por boat cuando se de el caso.
 - En algunas ocasiones aparecen dos edades de la siguiente forma '17 & 16'. En estos casos, entendemos que fueron atacadas más de una persona por las que las dejaremos así.
 -Hay casos que hay que tratar aisladamente, para lo que usaremos la función replace (teen, 20?) y los reemplazaremos por 'unknown.
 - En muchas ocasiones la edad va seguida de una s, eliminaremos la letra y obtendríamos la edad. 


### LIMPIEZA DE LA COLUMNA SPECIES

Comenzamos con la limpieza de valores nulos. Como es una columna categórica y no es relevante el tipo de tiburón para el análisis. Rellenaremos con 'unknown'.

En la mayoría de casos, antes de la palabra shark aparece o bien la raza o bien el tamaño, ambos nos sirven para dejar un poco más limpia la columna por lo que hemos creado una función para limpiar valores de species. Esta función dejaría como valor la palabra anterior a 'shark' y shark. Con ello, para unificar un poco más los valores, pasaremos todo a minúsculas y quitaremos algunos caracteres especiales.

### LIMPIEZA DE LA COLUMNA TIME

Es una columna que tiene NaN y que no nos interesa para el análisis por lo que rellenaremos los valores nulos con 'unknown'.

### LIMPIEZA DE LA COLUMNA ORIGINALORDER

Está compuesta por valores únicos que acaban en .0 (float) por lo que para su limpieza los convertiremos a integer tal y como hicimos con la columna de year.


## LIMPIEZA DE NULOS COMPLETADA + Resto de Columnas

Tras las limpiezas individuales que hemos ido realizando, nuestro df ya estaría completamente limpio de valores nulos.

En cuanto a las columnas: investigatororsource, pdf, hreffoemula, casenumber.1, casenumber.2 únicamente nos interesaba que no tuvieran valores nulos, ya que la información que contienen no aporta valor a nuestro análisis.


## DATAFRAME TRAS LA LIMPIEZA

 - El df está compuesto por **27 columnas y 3776 filas**.
 - El memory usage: **6.3 MB**
 
 


## Columnas constantes o de baja varianza

En las columnas numéricas no nos hemos encontrado ninguna columna constante, sin embargo, la columnna unnamed22 sí. Esto tiene sentido ya que completamos con 'unknown' todos los valores NaN (que eran casi el 100%) y parece ser que con la eliminación de filas ya no haya valores distintos.

No tiene sentido realizar el análisis de columnas de baja varianza ya que casi todas nuestras columnas son categóricas. 

## Outliers

La única forma de ver si tenemos outliers (por falta de columnas categóricas) sería analizando la columna year y ver la distribución temporal de los ataques de tiburón. 

Como se puede observar en las siguientes gráficas, los ataques de tiburón se concentran sobre todo a partir de 1950, bien por falta de datos de los años anteriores o porque con el paso del tiempo los humanos realizan más actividades en el mar.

![gráfica 1 out](https://github.com/BeaZatarain/Project-Shark-Attacks/blob/main/imagenes/grafica1_outliers.png)

![gráfica 2 out](https://github.com/BeaZatarain/Project-Shark-Attacks/blob/main/imagenes/grafica2_outliers.png)



## Colinealidad

También por ausencia de columnas numéricas no aporta ningún valor realizar un análisis de colinealidad. 

## ANÁLISIS COLUMNAS CATEGÓRICAS (Bonus)

El análisis que hemos realizado anteriormente, sólo nos aporta información de la distribución de los ataques de tiburones a lo largo del tiempo. Sin embargo, nosotros queremos analizar si: 
 
 1. Los tiburones atacan a más personas que a barcos 

![gráfica 1 conc](https://github.com/BeaZatarain/Project-Shark-Attacks/blob/main/imagenes/imagen_conclusiones.png)
 
 2. Las personas atacadas asumían un riesgo por la actividad que hacían en el momento del ataque o suele haber más ataques aleatorios.
 
![gráfica 2 conc](https://github.com/BeaZatarain/Project-Shark-Attacks/blob/main/imagenes/conclusiones2.png)
 
 3. Como se distribuye la fatalidad en función del ataque.
 
 ![gráfica 3 conc](https://github.com/BeaZatarain/Project-Shark-Attacks/blob/main/imagenes/conclu3.png)
 
 
