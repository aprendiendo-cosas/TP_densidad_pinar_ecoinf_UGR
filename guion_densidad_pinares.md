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

+ Iniciamos la metodología de captura de datos que se usó para generar la información que necesitamos: inventarios forestales. [Esta](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/presentaciones/inventarios_forestales.pptx) presentación muestra los principales conceptos que resultan importantes para nuestros objetivos. Y en [este](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/biblio/descripcion_sinfonevada.pdf) artículo tienes una descripción más detallada de la estructura de datos del inventario. 
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
+ Un aspecto muy importante de las bases de datos son las consultas. Gracias a esta herramienta podemos extraer la información que queramos de la base de datos con el nivel de agregación que necesitemos. Para hacer consultas se usa el lenguaje [SQL](https://es.wikipedia.org/wiki/SQL). Durante la clase probamos a hacer una consulta muy sencilla. Si quieres profundizar en esto puedes echarle un vistazo a [estas](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/biblio/taller_SQL.ppt) diapos y a [este](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/biblio/manual_sql.pdf) manual de SQL.
+ Para entender mejor esto descargamos la capa SIG que contiene la distribución de los estratos. [Aquí](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/geoinfo/input/estratos.zip) puedes descargar el shapefile con los estratos. Y aprovechamos esta necesidad para aprender un poco el manejo de QGIS. Concretamente hacemos lo siguiente:
  + Cargar una capa vectorial.
  + Cargar un servicio WMS con una ortofoto.
  + Cambiar los colores de los polígonos.
  + Guardar un proyecto de QGIS.
+ La existencia de la capa de estratos abre la posibilidad de obtener un mapa de densidad de otra manera diferente a la esbozada anteriormente. En este caso nos planteamos la posibilidad de obtener un mapa de densidad asignando a todos los polígonos de cada estrato la densidad promedio de los puntos de inventario que hay en los mismos. Discutimos brevemente las ventajas e inconvenientes de esta decisión. Nos quedamos con una idea preliminar de un concepto muy común al trabajar con SIG: la imputación espacial. 

Por una razón que desconozco en esta sesión no se grabó la pantalla. Así que solo tenemos el audio:

<iframe src="https://www.ivoox.com/player_ej_81121377_6_1.html?c1=8daa4d" width="100%" height="200" frameborder="0" allowfullscreen="" scrolling="no" loading="lazy"></iframe>




### Día 4 (21/01/2022)

+ Empezamos la sesión reptiendo el concepto de imputación y comparándolo con el de interpolación, que también pondremos en práctica en esta sesión. Las dos figuras mostradas más abajo resumen bien las dos metodologías que usaremos a continuación:

![image](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/imagenes/imputacion.jpg)



![image](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/imagenes/interpolacion.jpg)

+ Ahora estamos en disposición de construir un flujo de trabajo detallado con lo que queremos hacer. Hemos identificado una fuente de datos importante. La hemos analizado y hemos encontrado dos posibles formas de hacer lo que necesitamos. Hasta aquí hemos realizado una prospección general de los datos y hemos esbozado posibles análisis. Ahora que sabemos lo que podemos hacer, es momento de organizar bien todos los procesamientos que necesitamos. Es ahora cuando construimos un flujo de trabajo. Puedes verlo abajo y descargarlo [aquí](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/presentaciones/workflow_densidad.drawio.zip). 

<iframe frameborder="0" style="width:100%;height:1227px;" src="https://viewer.diagrams.net/?tags=%7B%7D&highlight=0000ff&edit=_blank&layers=1&nav=1&title=workflow_densidad.drawio#R%3Cmxfile%20pages%3D%222%22%3E%3Cdiagram%20id%3D%22C5RBs43oDa-KdzZeNtuy%22%20name%3D%22Page-1%22%3E7V1bd5pKFP41rnXOAyyY4foYo6a59DSJbdP0xUUQlQgMAYyaX39muCiXETFRwbRd57Qy3Ofbl2%2Fv2TO04Lm9uPA0d%2FIVDQ2rBbjhogU7LQAUkcN%2Fk4Zl1ACALEQtY88cRm38uqFvvhlxY3zieGYODT9zYICQFZhutlFHjmPoQaZN8zw0zx42Qlb2rq42NgoNfV2ziq0P5jCYxK8BobTe8cUwx5Pk1pIYv6CtJUfHr%2BJPtCGap5pgtwXPPYSC6Je9ODcs0ntJxzxcLh%2Bsm6l0cXXnv2g%2F2tff%2F%2FvJRBfr7XLK6h08wwnefenOuXfdQ7ezhdXRBoL9xZ%2BLFgOjS79q1izusPhdg2XSgx6aOUODXIRvwfZ8YgZG39V0sneOZQa3TQLbindrnh7LAC%2FjTQ8FWmAiB28zKocb%2FMBDU%2BMcWcjDbQ5y8KHtkWlZSVMLwF6vd9ZVcHvF146759XwAmORQj3uhgsD2UbgLfEh8V4GcJCFYnRaLNgML0FWitvma0nhsaywQI3aJylBkSBkhURQYyEdr261xgH%2FiKGgwzLjncVA%2Bem0pzfqUjd%2FKRfoB8OrteKCQWh3ZI4je0bICVLto%2FAPDbQuJ4ADgqayEp9BTJRZUMRLlWVWocAFZJEVD4WWUjdaQ%2BlJEqUiKhgqoOsHVaUUDDEwKs%2FKFGCUFQAZXIDCcvDjwLSV%2Bxu7N%2FrR9ri56J27D%2BaLzSTWP4WDMcTuId5EXjBBY%2BRoVnfd2l4jRbp6fcwNQm6Mz7MRBMsYIW0WoCx6xsIMfpHT8ftGW4%2BpPZ1FfOVwY5lsOPh9UyeRzcf0vvVp4VZy3m64%2Bmjm6XFH3N39%2BiX%2B%2FqV%2Bu5Ff9duRrXQeIZMYukDzxkZ8Qe5emLrX5jWYPIvTl9%2BcNAcTBsZXJN1ZKieeYWHZfc164x2A%2FuEb3renZ0INAGdpT4YVndq%2FuyG0xHB8c6gNBy0gWfh5208e%2FjUmvxYD3Cs6vnt0y0ROSl47K1hp3Z0g%2B2nmb9dbYitjsZAPqHSCxLGJiVm5L5VjufQfqaCBAsUsJm07III3U6CUgpTteqogpZgUrevHWBvdD%2FbjilxqT8lludL%2Bhdgm5WwaI3BcpQ7F5nB3a0bp0oKBo%2Fde8s5pzxNpApHFTF9KLzOU7GD8UErP8AGyu1jvS5SHRcOn5EL44aJrRbuKFtWyMJk3Kng1340Y%2FshcEPtK0Y8SEamkIDFacAVCCq3kMhnvw33c89CfGRwGmL7p4MOMV22I7RrX6f7Xv%2BycdapChSMYl%2FwcaoHmB8irANr%2BMUr4XFGdKAAJIqsciBzw9ZCDdzr695CK95MDOsZKVXIA9k0O4lNvkRnqTixCQM3HcEJOn6N3ik9by8uZ52nL1GEuOcAvuRGn5m%2FEp6%2B3%2FQRZzclr9Ahr6V31yp5JkIFjBQ0LYQVPrJwUCeIVIdvFOFTHAXxNDKhiPFKMCxtscvZsPpKQYav5EOTjmA9Mo3MSlLMe0ZMWrMf79XUvFGCl7qkIp8T9l%2Bo8TPdsUecTvmDaYZY1LVREUU1ds84sc0wSEwERxlXrDXnnW%2BSbcdriCQUBsvEBYWe0NX06DsWallgKb3aWcEWORhzj5%2BlMgoCkkc8INKCnDx3Impjrj0ysMB6r4zuCHuE6%2BB%2FS7pNNZLvkNzO2lu6EGZmWwYyQZ2sBQxAGokR6kbTq%2FmuyJ1i6BsPjqMB1xoeyZ4IkFoK67TEdpFg0eBiLRtfVWiza%2By3TmdS9ZxbLi%2B7zzagnjSdg3LPivHkFw8Tt2zDtmPSootHlqYxa4mmQM7SKzKpySqz5glirgOKoeZaDqbOOGGbD2CPtPcz2J25jwuzEbFQ3WXLWXAE6QEcIwalJvdX7rEH7qrkkbI45sV%2Fky6bzijtJ80xU6P%2FEF%2BJ%2B1CzLsNDY04hHcw3PxA9sePl9t%2BsdO1LmSGowXeM5KK%2F%2Fw3pzEDTTekcBCO4BIHoArv7BbFio7HTEmp3OD8f0yJsj8rcWeObTjKo7%2F3T73%2B8HgPs3urNlOtPoAlmWNkS6z76MTZ9FHiZTPcjiOK5nOPEu%2FM8MP8jA1pyZhkWoN0fe1HTGg7kZTAav%2BOnIc%2FSiHwPXQy6hnIbPhhAD%2BIzZus%2Bs2yu4TKE8Bd28wBdmzK6ssmr6T9HsNjLxL8DmERVeAmw2pcSLxbQ%2FjZyouxvJ93Nu4Q9gIwLcAGrz2UipGS%2BykSSeT2xs3q5mMninS0h2ALQuQgLgn0xI5IqERAQ1E5JLG%2BsJfkxyNfz%2F3cVlv4q%2FkU%2FKy1PH%2BFkeSoqkQFnGroqXOGgw%2FDHdfUGNPCjdvy2%2FXMnqmz55euKlr%2Fdakk4pq4ZqdI6oVOq3q0fdlTHnyAnTsIl%2BhH6CZDRbe%2BXl%2BIc2JsScJFgHcf%2BBnu4ZWmAMLG1peCtSHjbiYxnHmDPRLmbkIZvRHMZYmH64L2yvoMgREqesyDnGrtSqwlRPmLjYU1FZ6ktQImzqcXUH2H3DMnTdxF3lRa9PxnR88kQO2TZxez6O3fy%2BJ6UX2YJeAHOR7BHVologS9eV0m6vJY4VBI7NJgkgr7KyXNa39aXcqb36yWLczZJzkiFumR2lRbjZMDeya2s7h6U7MXQFEJob5X4Q0tqy7lIdrn0V5K42HrMR73Hq25NRkeYM9VbMTMi1glZvZiKJqLYSuWZhBmtRtA%2Bxt62UWqyIxNGmh1Sb5VgkE521N4pmvNK9DzbiQc7Xx%2FVOOu7Q0MfkC6FscziMoDSwQ4kZGwEzLjLF1xXbLbFDrkUG0uJ5Xgl08b2T7VSBVKfb7ohqFmKwu7pVZxsrv7SaAyFm65EEynTJQ%2FkyaqaJNvvhlMJWurmjJGKbZe7c7tvLN6379fH54YfkT0bIfe1Ssn6XtjsLNN1sncNWGxKKZ%2FiYi5kkfbRVz7ZOFI6bqusjjQxmsc8pXK%2FXFmVug2Im8zPhIRWQzykgkIujkTxP0bh8HfzeUC5q3CXpdxdZpwZ0GsAtlrfXO%2Btw7UPmQvKWlgr0UccrRVoW%2F4QD8P0AJfI8C9Ipqly1gAIKsO0JtU0JqoqDyNS5sCVse2M%2Bq7nx%2BJ5UEaqslAVVzKUl5QLGeyI9%2By2wEcVSIOuZWZvzaEACrKoU9akReUl6r0qfyy6WiM5JZiZLKTUlNWn6YUVjmrysRlyixKVnuOgpQ29OJkX5UXD3b%2BLK7dm%2BxqZdD%2BmG75PRac0ak%2F3kZDILKCGrZF5SMjpN9pnDeWZf1r7ScxwNtK8C5idZ7wWxhRWk1GBzcQ6RKtMWZjnsuBotashzlsvOwymKRTl%2F2t8wbIlQ7kR4FLbodGuvPyiuS4Z1iQt%2FccUqvcZFlQdFLLGWsYYLbHrSMwBFekpbWOFgo4CwmGx1MfsI1YB0ToIinyyo8ufhmJCkLHI8LxSQEwF1zaxDYSfUMsx02CEL2rRz%2BoGxfduaT01iGYnPFVkAXmLldDQj7ZkflSteeYFrQwpRBGFbkU%2FDClEgbXG%2FfQR8gTlqTiWKvAHxxgV8W%2FJgHmGTZ7xPnADgbETmTjhDFB6bTZOteF24klJYkhKg8MjQV%2BGdnhmg1UzQchkpn3Td3ADxo8JwlBxYxUU4%2F671uNExUjtMKPpF6nFV3eLehxnLnjplje81ou3mW0mGpmm1r7ySq3zlim5QpBA%2FcQ%2FEj9qr4l%2Fd2W2yFKUQpkxcG7JiCPURG0gbw3m4XJFOJDktTmkQa6R26icjjZsF55Q5Y%2BQ5CJRhpUpI%2FtYMsTTz6GFdZccIjS0jXmcqmmHVwyf2eGXRZr7P1dv%2Bs%2FbM3X2biQMdPN7CK2F2eTP9pk7JAgamMW%2FB3swnthITRM9MlpYqlatTJZofFKAG8UxYyyThXO9vKlA5DZdK7VdAKXujAwD27VPjU3PLA0IuO5Ii5Iuq3r08YDVBA8VByxOkupS1tKHEsQpXOugvQlre82D0N1nZ9jTK6yvagj1TXlpZaqncNpnzwiauOp9bM1iQVFYWSxf2aRjpTVLtn5r1Jlyj%2BbS3ImigABo1l%2FqPHW%2FqmED%2B20pm9VHqZ8IdmWnLp0tRd0D7CBP6NPnMvtS%2BX%2F%2F%2BfSY%2Fi%2BZ02jfaTC18tCGkEVKSmNROOhJnxAYst4CYyB%2BXNVKG3289NJzpZAUp7h9ds%2FSZhUNdbzUx18ba7v%2FbeCLJK9zqs1IrD1mskj8yc4S1ZE4PzgDLBgK2B2jHSnpWhKgJ8zCPBFHizJuDUTWSTrXZDeToxQUiJcp3oeqj5dR%2B%2FGSsfLOsnAIp36l%2BIVoYI1%2B5EHptiyzBmypVt42hqTkB2WXm513umMkmn0sYI3Y29lhy6UxmezAdv91Y9qtz0X98m8sT%2FiUI9J%2F2lzvz%2FN4YkxR2NrMdXn4wN54q6Dso1ffmBg4fFMh6ctt0S3Fiy35RX6JqSFBbXUPZU3862i6IMqsUQiJWphQ8HIy5Uzu8FuL%2BflEvLVndKuu1MXLq09S7AtEqKXHoFYgqM%2FRaIfsAQW9g5chqkO506PknKxXZLCl%2F6XlEzylT33Zk6GlGfjXz3qaP8uxB879Orr55P9XX9qRrjQY9udsXroxKtSabCdJn4eMNqzV599rmmz9gthGpplHC%2FAdMGBkDKwpAhqIskXlhwGAoc6GOOCeR2sm11Je%2Fn32Ufpk3zT7KPol3%2BPESJZdPYyp%2BwrfCpZSDjbxoQ935KV5p99f%2FPVgMYJbXX6xGLXufw602CTomXd3vp1VXX11596dVq6Wz%2Fn5a9fCfVuWVbGUNQ8k%2BHPFDquccmk1mDz3YvzNs%2B1qbX95B5sQyb%2FQviFNsQ%2BmX5PfsXnb9rDuEObmAMbmr%2FFn33AmtHb%2FrXk00avEj76kkPoI4UVd1BntP5R5HnIRcYrRcnIqnc1zudFFgQXrajFiJSm2XU7zpIRSkD8fBw%2BQrImvnwu7%2F%3C%2Fdiagram%3E%3Cdiagram%20id%3D%22Kc0jzny5qIj9sLP9HgPE%22%20name%3D%22Page-2%22%3EldG9DsIgEADgp2E0qUWNs%2FUvJi5q4s9iSDkLSnuVYlp9emugVuKiC4GP4%2BAOQqO0mmmWiyVyUCQMeEXomIThcDCsxxfcLfQotZBoyS11W1jLBzgMnN4kh8ILNIjKyNzHGLMMYuMZ0xpLP%2ByEyr81Zwl8wTpm6lu3khvhyuoHrc9BJqK5uRu4nZQ1wQ4KwTiWH0QnhEYa0dhZWkWgXr1r%2BrLf3LaH83R0XV12CziWQhfnjk02%2FefIuwQNmfk1dT1pn1YvvP%2Blkyc%3D%3C%2Fdiagram%3E%3C%2Fmxfile%3E"></iframe>

  

+ Para ejecutar el flujo de trabajo anterior necesitaremos la siguiente información además de la base de datos que ya debemos de tener descargada:
  + [Consultas](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/geoinfo/input/consultas.zip) en lenguaje SQL que usaremos para agregar la información de la manera necesaria para obtener la densidad (tanto en el caso de la densidad por parcela como en la densidad por estrato).
  + [Distribución de los pinares de Sierra Nevada](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/geoinfo/input/distribucion_pinares_sierra_nevada.zip). Se trata de un fichero de formas (shapefile) que contiene los polígonos del mapa de vegetación que están ocupados por pinares de repoblación. 
+ Empezamos con el cálculo del mapa de densidad mediante imputación. Para ello seguimos los pasos descritos en el flujo de trabajo. Por si no consigues tus propios resultados, ahí van los que he generado yo:
  + Tabla *[densidad_x_estrato.txt](https://raw.githubusercontent.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/main/geoinfo/output/densidad_x_estrato.txt)* generada a partir de la base de datos.
  + Fichero de formas mostrando la densidad de cada polígono de Sierra Nevada: *[densidad_x_estrato.zip](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/geoinfo/output/densidad_x_estrato.zip)*
  + El mismo mapa anterior pero en formato raster: *[densidad_x_estrato.tif](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/geoinfo/output/densidad_x_estrato.tif)*
  + Mapa de los pinares de repoblación en formato raster: *[mascara_pinares.tif](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/geoinfo/output/mascara_pinares.tif)*
  + Densidad de los pinares en formato raster: *[densidad_pinar_x_estratos.tif](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/geoinfo/output/densidad_pinar_x_estratos.tif)*

A continuación puedes ver el vídeo de la sesión:
<iframe width="560" height="315" src="https://www.youtube.com/embed/Utj_rQSS0_Q" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Día 5 (24/01/2022)


+ Una vez ejecutada esta primera "hebra" del flujo de trabajo, abordamos la obtención de un mapa de densidad mediante interpolación. Esta técnica es algo más compleja de entender, así que os paso aquí las siguientes referencias por si queréis profundizar en el tema:
  + [Spatial interpolation: a brief introduction](http://www.bisolutions.us/A-Brief-Introduction-to-Spatial-Interpolation.php): Muy buen resumen de la teoría de la interpolación y de los dos grandes grupos de técnicas que utilizaremos.
  + [Introduction to spatial analysis](http://planet.botany.uwc.ac.za/nisl/GIS/spatial/index.htm): Texto bastante completo en el que se detallan varias técnicas de interpolación. Hace mucho hincapié en la interpolación de modelos digitales de elevaciones.
  + [Spatial interpolation with IDW method explained](https://www.geodose.com/2019/03/spatial-interpolation-inverse-distance-weighting-idw.html): Guía muy detallada y completa sobre cómo aplicar el método IDW en QGIS.
+ Al igual que antes, abajo puedes ver los resultados que obtendríamos:
  + Tabla *[densidad_x_parcela.txt](https://raw.githubusercontent.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/main/geoinfo/output/densidad_x_parcela.txt)* generada a partir de la base de datos del inventario forestal.
  + Fichero de formas mostrando la densidad de cada parcela de Sierra Nevada: *[densidad_x_parcela.zip](https://raw.githubusercontent.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/main/geoinfo/output/densidad_x_parcela.zip)*
  + Fichero de formas mostrando la densidad de cada parcela de pinar: *[densidad_pinar_x_parcela.zip](https://raw.githubusercontent.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/main/geoinfo/output/densidad_pinar_x_parcela.zip)*
  + Capa raster mostrando la densidad de toda Sierra Nevada interpolando la densidad de las parcelas con pinar: *[densidad_interpolada.tif](https://raw.githubusercontent.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/main/geoinfo/output/densidad_interpolada.tif)*. Ojo, esta capa está en un sistema de referencia diferente al resto (WGS84)
  + Capa raster mostrando la densidad de los pinares de Sierra Nevada: *[densidad_pinar_interpolada.tif](https://raw.githubusercontent.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/main/geoinfo/output/densidad_interpolada.tif)*




## Vídeo resumen
El siguiente vídeo describe la ejecución de todo el flujo de trabajo. No es una transcripción de una clase, sino una narración del proceso paso a paso.

<iframe width="560" height="315" src="https://www.youtube.com/embed/AYFUjLEC7h4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>



