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
+ Al analizar la estructura del inventario forestal tomamos conciencia que sus datos se almacenan en una base de datos relacional. Así que, el primer paso es aprender cómo funcionan estos instrumentos. La siguiente presentación (descargable versión dinámica [aquí](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/presentaciones/mac.zip) para mac y [aquí](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/presentaciones/windows.zip) para Windows. [Aquí](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/presentaciones/presentacion_bbdd.pdf) la tienes en pdf) muestra los elementos básicos de las bases de datos relacionales. 

<p><iframe src="https://prezi.com/view/bHpJ0HBvXNXvC96XS5pe/embed" width="1200" height="600"> </iframe></p>

+ Después de conocer cómo se organizan los datos en una base de datos, procedemos a construir una que recoja las principales características de nuestro inventario forestal. Para ello nos basamos en el siguiente texto que hará las veces de "descripción literal de la base de datos": 

>*La metodología consiste en visitar una serie de parcelas que se delimitan con puntos. Cada parcela se caracteriza por su nombre, ubicación, altitud, pendiente media, orientación media, temperatura y precipitación media anual. En cada parcela caracterizamos los “pies de monte alto”, que son las plantas consideradas adultas o con porte arborescente. Para ello contamos el número de árboles mayores (más de 2 m de altura), el número de árboles menores, así como la densidad total de árboles y la regeneración (densidad del regenerado). También calculamos el área basimétrica de la parcela. Además, en cada visita anotamos: los nombres de las personas que realizan la visita, la hora de inicio-fin y el tipo de suelo de la parcela.*



+ Este proceso da lugar a un diagrama entidad-relación que consta de los elementos que se pueden ver en la siguiente figura (puedes descargar el diagrama [aquí](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/presentaciones/2021_2022_bbdd_inventario_forestal.drawio.zip))

<iframe frameborder="0" style="width:100%;height:579px;" src="https://viewer.diagrams.net/?tags=%7B%7D&highlight=0000ff&edit=_blank&layers=1&nav=1&title=2021_2022_bbdd_inventario_forestal.drawio#R5ZtNe5s4EIB%2FDcfmAcTn0R%2FZ5rDb5mm62%2BYog2JrF5BXiMTur1%2FJSDYgu6EbG9nOpUUjQcRoXkYzGltgkq8%2BUrhc%2FEFSlFmuna4sMLVcN%2FJt%2Fq8QrGtB4ErBnOK0Fjk7wQP%2BgaRQDatwisrWQEZIxvCyLUxIUaCEtWSQUvLSHvZEsvZfXcI50gQPCcx06TecskUtdQEIdh13CM8X6k8Hvlf35FCNlq9SLmBKXhoicGuBCSWE1Vf5aoIyoTylmPq%2B3w70bmdGUcH63BB9eUwfRvNyWsWLRfLt89e71eyDWz%2FlGWaVfON7SBOUQTlntlaaoKQqUiSeZVtg%2FLLADD0sYSJ6X%2FjSc9mC5RlvOfxSn5uc7jOiDK0aIjnXj4jkiNE1HyJ7w0DqTVqOE8v2y24dHCBli8YSKJODcunn20fvlMMvpH5%2BRVdAUwpKubXIJqFsQeakgNntTjpuq2035ndCllJZfyPG1tL0YcVIW5Vcg3T9Xd6%2FaTyKxo2vmtNVs3O6lq2DS1CSiiboJ%2B%2FpKNAgnSP2uvEIJfx0RSm3J4af20gdfXnUtBu2%2FInkM4q0VeMWyNpKLhkl%2F6AJyQjlkoIUYumecJZ1RDDD84I3E65UxOVjYc%2BYfy5GsiPHabpZ932AtG3hCIxsv6aKEVdnJNiDCDgZIp4JRN5g6kc3YXnrPcF8Lgc%2FZlunoB5Rsybv6izCdhpvYEP%2Fzv8543abYGsCrDEo3hEjoWlG%2FHNyI%2BEJ%2FQi4TD8CNFZGfMPJKpZeMSWg84GyTVMSmKVkt796bPYdHxLvMiHxNEjuUZFiYc0i%2BEEp1kOI66XFN01LaJqW0GvxcmPb8SvMbFr3iGKuArGmbwXJv0yQfA2kz1Rg1NybXT1Pne0xMB7HRIZ5CoDb4ckDA%2FMU9ORJpcJOHkM5nY9uOHAMFWikfkX5ElHIKnrFcAK%2FrXcvMA1nbBROxxoqDxcaI%2FBtLi3U94YUJXiJ9zo1GxYVzK6XHy%2Fs8OMY5kc92Jxzc1oMmXBuUU%2B0vDNDK9LQ%2BmRNXGs8EorAqOT%2FpSIAE5SNHDoj2UaWwzWh4upaIYuiNmSB6fwFcDRln3cmPL5QIuKeRMAdDKi4bhhirwOD6R0bcE17HDto79oGT0%2BoiPbS%2BFLzbvA1RUWJU5iKtyHsmrduXZCMhz7AbAlCO%2FQ5WeCzrfQZnpUDqYfO7kJb4BOnHraWu4PwC5qjAtH3cIIbd7LtvvG9nZEqh4Y7i2PQIvHGG9yduRfqzlyNJMFP5FAkUhEzWOJ8EzvFjHKLv2KoOq7NN32EBYwcYf1%2FAkDf86Yzy84B%2FcBJlcnVgZKo%2FbSXiJZckfzq3wptNnq5aKSQkT6BU39j54EY%2FgFnm0eJNVwKH7p5Z39s%2BVPxLL5oZb1%2Bv8LbERhxOm5%2FXwnqtpKrCUm3uutoi%2BeZPpfyvNY57wceSTkDux7Q92DKPcnmcEQpXDcGSIM9uHf04rYRAd9vmsGr41W92qHxkcoJ7h%2FPL%2BoZH3VDCvSzsL9widkmnb%2F9hpxJJbnbOcACQMd42Epy5XuNYWx38yGBEwxMcd9847m5Tz3feEcoVM4TFzjB5P14yNBvobXvbCse1EGazjSCsFu4MTRZXt%2FsyZmR5elJjiZZT7hPiuM6sHI7pS7enuAsHBQrwzXrjTL105bjepdZjuvp5bgML4lip6xQ9n6cUtgpuACRTk80KD3Gq3MHgsdcyeDb1kePZqZ1osN%2BEifFDNYHx2TGUIHTTQcqVJjzLAMfw1GO3%2FEZ%2Fp7fOR0ryuHN3e%2BW65hy9%2BtvcPsf"></iframe>



A continuación puedes ver el vídeo con la grabación de la clase:

<iframe width="560" height="315" src="https://www.youtube.com/embed/KO0qtpAIkPw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>



### Día 2 (18/01/2022).

+ A partir del diagrama entidad-relación anterior, construimos una base de datos relacional propiamente dicha. Para ello usamos el programa *base* de la suite de ofimática *LibreOffice*. Es nuestra primera base de datos. [Aquí](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/geoinfo/miprimerabasededatos.odb) puedes descargar la que hice yo durante la sesión. Construimos las tablas con sus campos y finalmente las relaciones.
+ Las primeras relaciones son sencillas (ojo, esto solo sirve para bases de datos no para relaciones entre personas): de uno a muchos. En algunas ocasiones hay relaciones de muchos a muchos, como en el caso de que queramos asignar más de un muestreador a cada visita.
+ Al finalizar esta sesión disponemos del conocimiento necesario para intepretar correctamente la información contenida en el inventario forestal Sinfonevada.



El siguiente vídeo muestra la grabación de la clase:


<iframe width="560" height="315" src="https://www.youtube.com/embed/Pczm48Q8Q9k" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>



### Día 3 (19/01/2022).

+ Empezamos la sesión abriendo la versión de la base de datos que contiene la información que necesitamos. [Aquí](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/geoinfo/input/bbdd_sinfonevada.zip) puedes descargarte la base de datos. En el archivo zip hay una versión para Libreoffice y otra para Microsoft Access.
+ Abrimos la base de datos y tratamos de interpretar su contenido. Vemos que tiene una estructura muy parecida a la que hemos hecho los días anteriores. Identificamos varios campos que podrían ser de utilidad para calcular la densidad. En concreto, vemos que hay dos campos de densidad en las tablas: **PARCELA_DATOS_CEPAS_M_BAJO** y **PARCELA_DATOS_PIES_M_ALTO**. Sabiamente deducimos que con estos dos campos podemos obtener una capa que asigne un valor de densidad a cada parcela. Decidimos que los dos valores de los campos anteriores son sumables.
+ También observamos que hay una tabla que no conocíamos: **explota_PARCELAS_POR_ESTRATO20**. Esta tabla asigna a cada parcela el código del estrato en el que se encuentra. Recordemos que los estratos son polígonos que tienen la misma orientación, vegetación, piso bioclimático y tipo de suelo. Es una técnica que usamos para estratificar el muestreo antes de hacer el inventario. 
+ Para entender mejor esto descargamos la capa SIG que contiene la distribución de los estratos. [Aquí](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/geoinfo/input/estratos.zip) puedes descargar el shapefile con los estratos. Y aprovechamos esta necesidad para aprender un poco el manejo de QGIS. Concretamente hacemos lo siguiente:
  + Cargar una capa vectorial.
  + Cargar un servicio WMS con una ortofoto.
  + Cambiar los colores de los polígonos.
  + Guardar un proyecto de QGIS.

+ La existencia de la capa de estratos abre la posibilidad de obtener un mapa de densidad de otra manera diferente a la esbozada anteriormente. En este caso nos planteamos la posibilidad de obtener un mapa de densidad asignando a todos los polígonos de cada estrato la densidad promedio de los puntos de inventario que hay en los mismos. Discutimos brevemente las ventajas e inconvenientes de esta decisión. Nos quedamos con una idea preliminar de un concepto muy común al trabajar con SIG: la imputación espacial. 

Por una razón que desconozco en esta sesión no se grabó la pantalla. Así que solo tenemos el audio:

<iframe src="https://www.ivoox.com/player_ej_81121377_6_1.html?c1=8daa4d" width="100%" height="200" frameborder="0" allowfullscreen="" scrolling="no" loading="lazy"></iframe>




### Día 4 (20/01/2022)

+ Empezamos la sesión reptiendo el concepto de imputación y comparándolo con el de interpolación, que también pondremos en práctica en esta sesión. Las dos figuras mostradas más abajo resumen bien las dos metodologías que usaremos a continuación:







+ Ahora estamos en disposición de construir un flujo de trabajo detallado con lo que queremos hacer. Hemos identificado una fuente de datos importante. La hemos analizado y hemos encontrado dos posibles formas de hacer lo que necesitamos. Flujo de trabajo.
  + Mapa de densidad por imputación: Descripción del paso a paso.



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
