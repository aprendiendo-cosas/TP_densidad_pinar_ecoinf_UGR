# Caracterización de la distribución espacial de la densidad de los pinares de repoblación en Sierra Nevada


> + **_Versión_**: 2021-2022
> + **_Asignatura (titulación)_**: Ciclo de gestión del dato: ecoinformática (máster conservación, gestión y restauración de la biodiversidad. UGR). 
> + **_Autor_**:  Curro Bonet-García (fjbonet@uco.es)
> + **_Duración_**: Ocho horas (cuatro sesiones de dos horas)
> + ***Versión:*** 1.1



## Objetivos del acto docente

Este acto docente tiene los siguientes objetivos:

+ Disciplinares: objetivos relacionados con la ecología:
  + Conocer cómo cambia en el dominio del espacio la distribución de la densidad de los bosques de Sierra Nevada.
  + Generar un mapa de densidad de los pinares de repoblación en Sierra Nevada.
+ Instrumentales: Objetivos relacionados con la adquisición de competencias en el manejo de herramientas. 
  + Aprender a usar bases de datos relacionales.
  + Aprender a interpolar variables ambientales usando kriging e imputación espacial. 
  + Mejorar el manejo de los SIG y de R.



## Hilo argumental
El hilo argumental seguido en este acto docente es una cascada de razonamientos:

+ La densidad de los pinares es una variable clave que explica la regeneración de la encina bajo su dosel. 
+ Por tanto necesitamos generar un mapa que muestre la distribución de la densidad de los pinares en Sierra Nevada.
+ Para obtener este mapa usaremos los datos generados por un inventario forestal realizado en Sierra Nevada en 2005 (SinfoNevada). Los datos de este inventario se almacenan en una base de datos relacional.
+ Por tanto, para acceder a dichos datos necesitamos aprender a usar bases de datos relacionales...
+ Cuando aprendamos lo anterior veremos que la información que suministra el inventario se refiere a una serie de puntos. Pero nosotros necesitamos datos de densidad para todo el territorio de Sierra Nevada.
+ Para resolver esto usaremos técnicas de análisis que nos permitirán "extender" los datos de densidad desde los puntos de inventario a lugares en los que no se han tomado datos de campo.
+ Haremos lo anterior usando dos técnicas muy diferentes. Esto dará lugar a dos resultados distintos.
+ Además, al final de todo veremos otro mapa de densidad obtenido mediante teledetección. Reflexionaremos sobre los resultados obtenidos en los tres casos.

Lo anterior se puede ver gráficamente en esta imagen:

![image](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/imagenes/hilo_argumental.jpg)



## Secuencia de sesiones
La secuencia de acciones que se muestra a continuación pretende simular el proceso por el que abordamos un problema nuevo como el que nos ocupa. Es un proceso un poco detectivesco porque casi nunca tenemos toda la información sobre la mesa (y bien documentada) desde el principio. Iremos poco a poco construyendo un camino y recorriéndolo a la vez.

### Día 1 (17/01/2022). 

+ Iniciamos la metodología de captura de datos que se usó para generar la información que necesitamos: inventarios forestales. [Esta](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/presentaciones/inventarios_forestales.pptx) presentación muestra los principales conceptos que resultan importantes para nuestros objetivos. 
+ Al analizar la estructura del inventario forestal tomamos conciencia que sus datos se almacenan en una base de datos relacional. Así que, el primer paso es aprender cómo funcionan estos instrumentos. La siguiente presentación (descargable versión dimánica aquí para mac y aquí para Windows. Aquí la tienes en pdf) muestra los elementos básicos de las bases de datos relacionales. 

<p><iframe src="https://prezi.com/view/bHpJ0HBvXNXvC96XS5pe/embed" width="1200" height="900"> </iframe></p>




+ Construcción del diagrama entidad-relación. Enlace a drawio e imagen.
+ construimos la base de datos a partir de este texto:





*La metodología consiste en visitar una serie de parcelas que se delimitan con puntos. Cada parcela se caracteriza por su nombre, ubicación, altitud, pendiente media, orientación media, temperatura y precipitación media anual. En cada parcela caracterizamos los “pies de monte alto”, que son las plantas consideradas adultas o con porte arborescente. Para ello contamos el número de árboles mayores (más de 2 m de altura), el número de árboles menores, así como la densidad total de árboles y la regeneración (densidad del regenerado). También calculamos el área basimétrica de la parcela. Además, en cada visita anotamos: los nombres de las personas que realizan la visita, la hora de inicio-fin y el tipo de suelo de la parcela.*

<iframe width="560" height="315" src="https://www.youtube.com/embed/KO0qtpAIkPw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Día 2 (18/01/2022).
+ Creación de base de datos a partir del diagrama entidad-relación anterior.


<iframe width="560" height="315" src="https://www.youtube.com/embed/Pczm48Q8Q9k" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


### Día 3 (19/01/2022).
+ Estudiar la base de datos que realmente contiene información.
+ Identificar las variables que usaremos para nuestro trabajo. Formular las preguntas que queremos hacer a la base de datos.
+ Identificamos los campos de densidad para asignar un valor único de esta variable a cada parcela. 
+ Estudiar la capa de estratos que se relaciona con la base de datos.
+ Observamos que disponemos de dos formas de generar un mapa de densidad: estratos y puntos.
+ Analizamos este hecho con detalle. Presentación.
+ Ahora estamos en disposición de construir un flujo de trabajo detallado con lo que queremos hacer. Hemos identificado una fuente de datos importante. La hemos analizado y hemos encontrado dos posibles formas de hacer lo que necesitamos. Flujo de trabajo.
  + Mapa de densidad por imputación: Descripción del paso a paso.

AUdio describiendo lo que hicimos:

<iframe src="https://www.ivoox.com/player_ej_81121377_6_1.html?c1=8daa4d" width="100%" height="200" frameborder="0" allowfullscreen="" scrolling="no" loading="lazy"></iframe>




### Día 4 (20/01/2022)

[Spatial interpolation: a brief introduction](http://www.bisolutions.us/A-Brief-Introduction-to-Spatial-Interpolation.php): Muy buen resumen de la teoría de la interpolación y de los dos grandes grupos de técnicas que hemos visto en clase. 
[Introduction to spatial analysis](http://planet.botany.uwc.ac.za/nisl/GIS/spatial/index.htm): Texto bastante completo en el que se detallan varias técnicas de interpolación. Hace mucho hincapié en la interpolación de modelos digitales de elevaciones.
[Spatial interpolation with IDW method explained](https://www.geodose.com/2019/03/spatial-interpolation-inverse-distance-weighting-idw.html): Guía muy detallada y completa sobre cómo aplicar el método IDW en QGIS.






------
Vídeo describiendo todo lo que hacemos (sin alumnos)

<iframe width="560" height="315" src="https://www.youtube.com/embed/AYFUjLEC7h4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

-----


- - - IDW:

    - - Hacemos la interpolación basada en la distancia de los valores de esa capa. 100 m de resolución.

    - Intersectamos el raster con la distribución de pinares. 

  - Sesión 4: Generamos varias capas con varios algoritmos y parámetros. Las analizamos de visu. Calculamos el promedio de todas ellas. El ejercicio para ellos esta semana es que ideen un mecanismo de validación de lo que hemos hecho y lo expresen como un flujo de trabajo. Eso o que busquen un método alternativo para calcular la densidad del bosque. También hacemos el kriging con y sin zona de inf.uencia

  - - Obtenemos capas interpolando la densidad con los siguientes parámetros y algoritmos:

    - - QGis:

      - - potencia 2: idw_QGIS_p2.tif
        - potencia 6: idw_QGIS_p6.tif

      - GDAL

      - - potencia 2, radio 500: idw_GDAL_p2_r500.tif
        - potencia 2, radio 1000: idw_GDAL_p2_r1000.tif
        - potencia 2, radio 5000: idw_GDAL_p2_r5000.tif
        - Potencia 6, radio 1000: idw_GDAL_p6_r1000.tif
        - Potencia 4, radio 2000: idw_GDAL_p4_r2000.tif

      - Grass:

      - - Potencia 2: idw_GRASS_p2.tif
        - Potencia 4: idw_GRASS_p4.tif






- - 

  - Sesión 4

  - - Breve introducción teórica a la interpolación: 30'

    - Vemos la capa de parcelas en qgis. ¿hemos tenido en cuenta de alguna manera la especie cuya densidad se está mostrando?

    - No… Así que tenemos que ceñirnos de alguna manera a pinares de repoblación, que es nuestro objeto de estudio:

    - - Cargamos la capa con las formaciones vegetales: usos_sierra_nevada_23030.shp
      - Seleccionamos los pinares de repoblación y luego aquellas parcelas que estén sobre pinares. "COMENTARIO" ILIKE 'Pinar%'
      - Creamos una nueva capa con las parcelas sobre pinares. Será esta capa sobre la que trabajemos. parcelas_pinar.shp

    - 



SELECT "DICC_PARCELAS"."ID_PARCELA", "DICC_PARCELAS"."NOMBRE", "DICC_PARCELAS"."UTM_X_GPS", "DICC_PARCELAS"."UTM_Y_GPS", "explota_PARCELAS_POR_ESTRATO20"."COD_ESTRATOS20", "DICC_PARCELAS"."MUNICIPIO" FROM "explota_PARCELAS_POR_ESTRATO20", "DICC_PARCELAS" WHERE "explota_PARCELAS_POR_ESTRATO20"."ID_PARCELA" = "DICC_PARCELAS"."ID_PARCELA" AND "DICC_PARCELAS"."MUNICIPIO" = 'ABLA'
