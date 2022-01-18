# Caracterización de la distribución espacial de la densidad de los pinares de repoblación en Sierra Nevada


> + **_Versión_**: 2021-2022
> + **_Asignatura (titulación)_**: Ciclo de gestión del dato: ecoinformática (máster conservación, gestión y restauración de la biodiversidad. UGR). 
> + **_Autor_**:  Curro Bonet-García (fjbonet@uco.es)
> + **_Duración_**: Unas seis horas (tres sesiones de dos horas)
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









## **Hilo argumental**
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







**interplación*

Lista de enlaces de interés para aprender sobre interpolación espacial


Spatial interpolation: a brief introduction: Muy buen resumen de la teoría de la interpolación y de los dos grandes grupos de técnicas que hemos visto en clase. 
Introduction to spatial analysis: Texto bastante completo en el que se detallan varias técnicas de interpolación. Hace mucho hincapié en la interpolación de modelos digitales de elevaciones.
Spatial interpolation with IDW method explained: Guía muy detallada y completa sobre cómo aplicar el método IDW en QGIS.


- - Objetivos

- - Ecológicos

  - - Conocer cómo cambia la densidad de los pinares de repoblación

  - Ecoinformáticos

  - - Aprender a usar bases de datos relacionales
    - Aprender a interpolar variables ambientales
    - Mejorar nuestro manejo de los SIG

- Metodología

- - Estudiar contenido base de datos Sinfonevada

  - Identificar tablas con datos de densidad

  - Generar tabla con densidad de pies mayores

  - Exportar

  - Interpolar densidad

  - - QGIS
    - R

- Datos necesarios

- - Base de datos sinfonevada
  - Mapas de cantones de sinfonevada

- Resultados esperados

- - Mapa con la distribución de la densidad de los pinares

- Ejercicio a realizar

- Secuencia de sesiones

- - Sesión 1

  - - Generalidades de inventarios forestales. 10’. Presentación.
    - Teoría: bases de datos relacionales (prezi bbdd. primera pregunta): 60'.

  - Sesión 2

  - - Teoría: bases de datos relacionales (60’). Segunda pregunta. lo contamos y hacemos como ejemplo la construcción de la base de datos del inventario forestal. Para ello usamos el siguiente texto descriptivo: *La metodología consiste en visitar una serie de parcelas que se delimitan con puntos. Cada parcela se caracteriza por su nombre, ubicación, altitud, pendiente media, orientación media, temperatura y precipitación media anual. En cada parcela caracterizamos los “pies de monte alto”, que son las plantas consideradas adultas o con porte arborescente. Para ello contamos el número de árboles mayores (más de 2 m de altura), el número de árboles menores, así como la densidad total de árboles y la regeneración (densidad del regenerado). También calculamos el área basimétrica de la parcela. Además, en cada visita anotamos: los nombres de las personas que realizan la visita, la hora de inicio-fin y el tipo de suelo de la parcela.*

  - Sesión 3:

  - - Terminamos de construir la base de datos. 

    - - Relaciones de muchos a muchos. Lo hacemos con el tipo de uso del suelo. Y les pregunto en relación a los censadores.
      - ¿tiene en realidad sentido que pongamos los datos de monte bajo en una tabla distinta a la visita?. 

    - Abrimos la base de datos en LO y en office

    - - Ver las tablas que contiene.

      - - modo vista y modo diseño.

      - Ver las relaciones

      - - cardinalidad e integridad referencial

      - vemos la información geográfica existente en la base de datos: parcelas

    - Vemos el shapefile con los estratos.

    - Definimos lo que queremos hacer:

    - Consulta “densidad_x_estrato”:

    - - SELECT `explota_PARCELAS_POR_ESTRATO20`.`COD_ESTRATOS20`, AVG( `PARCELA_DATOS_CEPAS_M_BAJO`.`DENSIDAD_TOTAL` ) AS `dens_mbajo_me`, AVG( `PARCELA_DATOS_PIES_M_ALTO`.`DENSIDAD_TOTAL` ) AS `dens_malto_me` FROM `DICC_PARCELAS`, `explota_PARCELAS_POR_ESTRATO20`, `PARCELA_DATOS_CEPAS_M_BAJO`, `VISITAS_PARCELAS`, `PARCELA_DATOS_PIES_M_ALTO` WHERE `DICC_PARCELAS`.`ID_PARCELA` = `explota_PARCELAS_POR_ESTRATO20`.`ID_PARCELA` AND `PARCELA_DATOS_CEPAS_M_BAJO`.`COD_VISITA_PARCELA` = `VISITAS_PARCELAS`.`COD_VISITA_PARCELA` AND `PARCELA_DATOS_PIES_M_ALTO`.`COD_VISITA_PARCELA` = `VISITAS_PARCELAS`.`COD_VISITA_PARCELA` AND `VISITAS_PARCELAS`.`ID_PARCELA` = `DICC_PARCELAS`.`ID_PARCELA` GROUP BY `explota_PARCELAS_POR_ESTRATO20`.`COD_ESTRATOS20` HAVING ( ( `explota_PARCELAS_POR_ESTRATO20`.`COD_ESTRATOS20` IS NOT NULL ) )

    - Consulta densidad_x_parcela

    - - SELECT `DICC_PARCELAS`.`ID_PARCELA`, `DICC_PARCELAS`.`UTM_X_GPS`, `DICC_PARCELAS`.`UTM_Y_GPS`, `PARCELA_DATOS_PIES_M_ALTO`.`DENSIDAD_PIES_MAY`, `PARCELA_DATOS_PIES_M_ALTO`.`DENSIDAD_PIES_MEN`, `PARCELA_DATOS_PIES_M_ALTO`.`DENSIDAD_TOTAL` FROM `VISITAS_PARCELAS`, `DICC_PARCELAS`, `PARCELA_DATOS_PIES_M_ALTO` WHERE `VISITAS_PARCELAS`.`ID_PARCELA` = `DICC_PARCELAS`.`ID_PARCELA` AND `PARCELA_DATOS_PIES_M_ALTO`.`COD_VISITA_PARCELA` = `VISITAS_PARCELAS`.`COD_VISITA_PARCELA`

  - Sesión 4

  - - Breve introducción teórica a la interpolación: 30'

    - Vemos la capa de parcelas en qgis. ¿hemos tenido en cuenta de alguna manera la especie cuya densidad se está mostrando?

    - No… Así que tenemos que ceñirnos de alguna manera a pinares de repoblación, que es nuestro objeto de estudio:

    - - Cargamos la capa con las formaciones vegetales: usos_sierra_nevada_23030.shp
      - Seleccionamos los pinares de repoblación y luego aquellas parcelas que estén sobre pinares. "COMENTARIO" ILIKE 'Pinar%'
      - Creamos una nueva capa con las parcelas sobre pinares. Será esta capa sobre la que trabajemos. parcelas_pinar.shp

    - IDW:

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

- Sesión 5:

- - Los alumnos intentan ejecutar el script de R para hacer kriging: 30’
  - Resolvemos dudas de todo lo visto hasta ahora: 30’
  - Vemos un poco del constructor de modelos de QGIS: 60’





Vídeo primera sesión: 

<iframe width="560" height="315" src="https://www.youtube.com/embed/KO0qtpAIkPw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>