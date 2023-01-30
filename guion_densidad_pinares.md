# Caracterización de la distribución espacial de la densidad de los pinares de repoblación en Sierra Nevada


> + **_Versión_**: 2022-2023
> + **_Asignatura (titulación)_**: Ciclo de gestión del dato: ecoinformática (máster conservación, gestión y restauración de la biodiversidad. UGR). 
> + **_Autor_**:  Curro Bonet-García (fjbonet@uco.es)
> + **_Duración_**: Seis horas (tres sesiones de dos horas)




## Objetivos del acto docente

Este acto docente tiene los siguientes objetivos:

+ Disciplinares: objetivos relacionados con la ecología:
  + Conocer cómo cambia en el dominio del espacio la distribución de la densidad de los bosques de Sierra Nevada.
  + Generar un mapa de densidad de los pinares de repoblación en Sierra Nevada.
+ Instrumentales: Objetivos relacionados con la adquisición de competencias en el manejo de herramientas. 
  + Aprender a usar bases de datos relacionales.
  + Aprender a asignar valores de una variable en un mapa en sitios donde no se han tomado datos de campo. Para ello usaremos la interpolación e imputación espacial. 




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

![image](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/imagenes/hilo_argumental.png)



## Secuencia de sesiones
La secuencia de acciones que se muestra a continuación pretende simular el proceso por el que abordamos un problema nuevo como el que nos ocupa. Es un proceso un poco detectivesco porque casi nunca tenemos toda la información sobre la mesa (y bien documentada) desde el principio. Iremos poco a poco construyendo un camino y recorriéndolo a la vez.

### Día 1 (17/01/2023). 

+ Iniciamos la metodología de captura de datos que se usó para generar la información que necesitamos: inventarios forestales. [Esta](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/presentaciones/inventarios_forestales.pptx) presentación muestra los principales conceptos que resultan importantes para nuestros objetivos. Y en [este](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/biblio/descripcion_sinfonevada.pdf) artículo tienes una descripción más detallada de la estructura de datos del inventario. 
+ Al analizar la estructura del inventario forestal tomamos conciencia que sus datos se almacenan en una base de datos relacional. Así que, el primer paso es aprender cómo funcionan estos instrumentos. La siguiente presentación (descargable versión dinámica [aquí](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/presentaciones/mac.zip) para mac y [aquí](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/presentaciones/windows.zip) para Windows. [Aquí](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/presentaciones/presentacion_bbdd.pdf) la tienes en pdf) muestra los elementos básicos de las bases de datos relacionales. 

<p><iframe src="https://prezi.com/view/bHpJ0HBvXNXvC96XS5pe/embed" width="1200" height="600"> </iframe></p>

### Día 2 (19/01/2023).


+ Después de conocer cómo se organizan los datos en una base de datos, procedemos a construir una que recoja las principales características de nuestro inventario forestal. Para ello nos basamos en el siguiente texto que hará las veces de "descripción literal de la base de datos": 

>*La metodología consiste en visitar una serie de parcelas que se delimitan con puntos. Cada parcela se caracteriza por su nombre, ubicación, altitud, pendiente media, orientación media, temperatura y precipitación media anual. En cada parcela caracterizamos los “pies de monte alto”, que son las plantas consideradas adultas o con porte arborescente. Para ello contamos el número de árboles mayores (más de 2 m de altura), el número de árboles menores, así como la densidad total de árboles y la regeneración (densidad del regenerado). También calculamos el área basimétrica de la parcela. Además, en cada visita anotamos: los nombres de las personas que realizan la visita, la hora de inicio-fin y el tipo de suelo de la parcela.*


+ Este proceso da lugar a un diagrama entidad-relación que se muestra a continuación en tres sucesivas versiones.

![esquema_v1](https://raw.githubusercontent.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/main/imagenes/esquema_v1.jpg)

*Esta primera versión muestra las entidades **PARCELAS**, **VISITAS** y **PIES_MONTE_ALTO**. También contiene un primer intento de modelar la forma en la que asignamos a cada visita un número indefinido de muestreadores*



![esquema_v3](https://raw.githubusercontent.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/main/imagenes/esquema_v3.jpg)

*La última versión de esta sesión incluye también un ejemplo de relaciones de uno a muchos realizado para modelar la asignación de tipos de uso del suelo a cada parcela. Creando la tabla **DICC_TIPOS_SUELO**, mejoramos la eficiencia porque nos aseguramos de que en la clave ajena de esa tabla en **PARCELAS**, entran solo valores existentes en el mencionado diccionario*


+ A partir del diagrama entidad-relación anterior, construimos una base de datos relacional propiamente dicha. Para ello usamos el programa *Access* de la suite de ofimática *Microsoft Office*. Es nuestra primera base de datos. [Aquí](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/geoinfo/miprimerabasededatos.accdb) puedes descargar la que hice yo durante la sesión. Construimos las tablas con sus campos y finalmente las relaciones. En este ejemplo solo hay un par de tablas y una relación. Cuando construyas las tablas, recuerda que se pueden visualizar en modo "edición" y en modo "vista". El primero permite añadir campos, modificarlos, etc. El segundo permite añadir datos. 
+ Al finalizar esta sesión disponemos del conocimiento necesario para intepretar correctamente la información contenida en el inventario forestal Sinfonevada.
+ Es importante tener en cuenta que la base de datos que hemos creado está vacía. Solo hemos aprendido a construir su estructura. Para poner datos en su interior basta con ir rellenando a mano las tablas. Otra opción más elaborada es usar formularios de carga de datos. No tenemos tiempo en esta asignatura de aprender cómo se construyen formularios. Pero te resultará fácil de aprenderlo por tí mismo/a una vez que has entendido bien cómo se construye una base de datos. 



### Día 3 (23/01/2023).

+ Empezamos la sesión abriendo la versión de la base de datos que contiene la información que necesitamos. [Aquí](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/geoinfo/input/bbdd_sinfonevada_densidad.mdb) puedes descargarte la base de datos. 
+ Abrimos la base de datos y tratamos de interpretar su contenido. Vemos que tiene una estructura muy parecida a la que hemos hecho los días anteriores. Identificamos varios campos que podrían ser de utilidad para calcular la densidad. En concreto, vemos que hay dos campos de densidad en las tablas: **PARCELA_DATOS_CEPAS_M_BAJO** y **PARCELA_DATOS_PIES_M_ALTO**. Sabiamente deducimos que con estos dos campos podemos obtener una capa que asigne un valor de densidad a cada parcela. Decidimos que los dos valores de los campos anteriores son sumables.
+ También observamos que hay una tabla que no conocíamos: **explota_PARCELAS_POR_ESTRATO20**. Esta tabla asigna a cada parcela el código del estrato en el que se encuentra. Recordemos que los estratos son polígonos que tienen la misma orientación, vegetación, piso bioclimático y tipo de suelo. Es una técnica que usamos para estratificar el muestreo antes de hacer el inventario. 
+ Un aspecto muy importante de las bases de datos son las consultas. Gracias a esta herramienta podemos extraer la información que queramos de la base de datos con el nivel de agregación que necesitemos. Para hacer consultas se usa el lenguaje [SQL](https://es.wikipedia.org/wiki/SQL). Durante la clase probamos a hacer una consulta muy sencilla. Si quieres profundizar en esto puedes echarle un vistazo a [estas](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/biblio/taller_SQL.ppt) diapos y a [este](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/biblio/manual_sql.pdf) manual de SQL.
+ Para entender mejor esto descargamos la capa SIG que contiene la distribución de los estratos. [Aquí](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/geoinfo/input/estratos.zip) puedes descargar el shapefile con los estratos. Y aprovechamos esta necesidad para aprender un poco el manejo de QGIS. Concretamente hacemos lo siguiente:
  + Cargar una capa vectorial.
  + Cargar un servicio WMS con una ortofoto.
  + Cambiar los colores de los polígonos.
  + Guardar un proyecto de QGIS.
+ La visualización de las parcelas en el SIG nos permite pensar dos formas distintas de obtener un mapa que muestre la distribución de la densidad en nuestra zona de estudio. Las dos imágenes siguientes muestran esta idea.


![image](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/imagenes/interpolacion.jpg)

![image](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/imagenes/imputacion.jpg)



+ Ahora estamos en disposición de construir un flujo de trabajo detallado con lo que queremos hacer. Hemos identificado una fuente de datos importante. La hemos analizado y hemos encontrado dos posibles formas de hacer lo que necesitamos. Hasta aquí hemos realizado una prospección general de los datos y hemos esbozado posibles análisis. Ahora que sabemos lo que podemos hacer, es momento de organizar bien todos los procesamientos que necesitamos. Es ahora cuando construimos un flujo de trabajo. Puedes verlo abajo y descargarlo [aquí](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/presentaciones/workflow_densidad.drawio.zip). 

<iframe frameborder="0" style="width:100%;height:1227px;" src="https://viewer.diagrams.net/?tags=%7B%7D&highlight=0000ff&edit=_blank&layers=1&nav=1&title=workflow_densidad.drawio#R%3Cmxfile%20pages%3D%222%22%3E%3Cdiagram%20id%3D%22C5RBs43oDa-KdzZeNtuy%22%20name%3D%22Page-1%22%3E7V1bd5pKFP41rnXOAyyY4foYo6a59DSJbdP0xUUQlQgMAYyaX39muCiXETFRwbRd57Qy3Ofbl2%2Fv2TO04Lm9uPA0d%2FIVDQ2rBbjhogU7LQB4nhfwP6RlGbUAkVOilrFnDuOj1g19882IG7m4dWYODT9zYICQFZhutlFHjmPoQaZN8zw0zx42Qlb2rq42NgoNfV2ziq0P5jCYxK8BobTe8cUwx5Pk1pIYv7KtJUfHr%2BJPtCGap5pgtwXPPYSC6Je9ODcs0n1JxzxcLh%2Bsm6l0cXXnv2g%2F2tff%2F%2FvJRBfr7XLK6h08wwnefenOuXfdQ7ezhdXRBoL9xZ%2BLFgOjS79q1izusPhdg2XSgx6aOUODXIRvwfZ8YgZG39V0sneOhQa3TQLbindrnh7LAC%2FjTQ8FWmAiB28zKocb%2FMBDU%2BMcWcjDbQ5y8KHtkWlZSVMLwF6vd9ZVcHvF146759XwAmORQj3uhgsD2UbgLfEh8V4GcJCFYnRaLNgML0FWitvma0nhsaywQI3aJylBkSBkhURQYyEdr261xgH%2FiKGgwzLjncVA%2Bem0pzfqUjd%2FKRfoB8OrteKCQWh3ZI4je0bICVLto%2FAPDbQuJ4ADgqayEp9BTJRZUMRLlWVWocAFZJEVD4WWUjdaQ%2BlJEqUiKhgqoOsHVaUUDDEwKs%2FKFGCUFQAZXIDCcvDjwLSV%2Bxu7N%2FrR9ri56J27D%2BaLzSTWP4WDMcTuId5EXjBBY%2BRoVnfd2l4jRbp6fcwNQm6Mz7MRBMsYIW0WoCx6xsIMfpHT8ftGW4%2BpPZ1FfOVwY5lsOPh9UyeRzcf0vvVp4VZy3m64%2Bmjm6XFH3N39%2BiX%2B%2FqV%2Bu5Ff9duRrXQeIZMYukDzxkZ8Qe5emLrX5jWYPIvTl9%2BcNAcTBsZXJN1ZKieeYWHZfc164x2A%2FuEb3renZ0INAGdpT4YVndq%2FuyG0xHB8c6gNBy0gWfh5208e%2FjUmvxYD3Cs6vnt0y0ROSl47K1hp3Z0g%2B2nmb9dbYitjsZAPqHSCxLGJiVm5L5VjufQfqaCBAsUsJm07III3U6CUgpTteqogpZgUrevHWBvdD%2FbjilxqT8lludL%2Bhdgm5WwaI3BcpQ7F5nB3a0bp0oKBo%2Fde8s5pzxNpApHFTF9KLzOU7GD8UErP8AGyu1jvS5RH0%2FXhU3Il%2FHTRxaJ9RZNqWZjNGxXcmu9GFH9kLoiBpShIiYzs5Jb4LFeAQoJKCr7kshl3xH3cFdHfARwGqb7p4MOMV22IDR3X6f7Xv%2BycdapCh0Mal%2FwcaoHmB8irAOLBMFOL%2BkUBSBBZ5UBsga%2BHLbzT87%2BHZbyfLdAxVqqyBbBvthCfeovMUHdiEQJqPqgTcvocvVN82lpezjxPW6YOc8kBfsmNODV%2FIz59ve0nyGpOXqNHWEvvqlf2zIoMHDxoWAgruGblpFgRrwjZLsaxO47oa6JEFQOUYqDYYJOzZ%2FORxBBbzYcgH8d8YF6dk6Cc9YietGA93q%2Bve6EAK3VPhTwl7r9U52G6Z4s6n%2FAF0w7TrmmhIopq6pp1ZpljkqkIiDCuWm%2FIO98i34zzGE8oCJCNDwg7A%2FPN6TgUa1qmKbzZWcIdORqRjJ%2BnMwkCklc%2BI9CAnj50IGti8j8yscJ4rI7vCHqE6%2BB%2FSLtPNpHtkt%2FM2Fq6E2ZkWgYzQp6tBQxBGIgS6UXSqvuvyZ5g6RoMj8ME1xkfyp4JkliI8rYHeZBi0eBhLBpdV2uxaO%2B3TGdS955ZLC%2B6zzejnjSegHHPihPpFQwTt2%2FDtGMWpIpGl%2Bc2agmwQc7QKjKryimx5gtirQKKo%2BZZDqbOOmLcDWOPtO9ojvUnbmPC7sRsVDdZctZcATpARwjBqVm%2B1fusQfuquSRsjjmxX%2BTLpvOKO0nzTFTo%2F8QX4n7ULMuw0NjTiEdzDc%2FED2x4%2BX236x07UuZIajBd4zkor%2F%2FDenMQNNN6RwEI7gEgegCu%2FsFsWKjsdMSanc4Px%2FTImyPytxZ45tOMqjv%2FdPvf7weA%2Bze6s2U60%2BgCWZY2RLrPvoxNn0UeJlM9yOI4rmc48S78zww%2FyMDWnJmGRag3R97UdMaDuRlMBq%2F46chz9KIfA9dDLqGchs%2BGEAP4jNm6z6zbK7hMoTwn3bzAF2bMrqyyavpP0ew2ciRAgM0jKrwE2GxKiReL4wA0cqLubiTfz7mFP4CNCHADqM1nI6VmvMhGkng%2BsbF5u5rJ4J0uIdkB0LoICYB%2FMiGRKxISEdRMSC5trCf4McnV8P93F5f9Kv5GPikvTx30Z3koKZICZRm7Kl7ioMHwx3T3BTXyoHT%2FtvxyJatv%2BuTpiZe%2B3mtJOqWsPKrROaJSqd%2BuHnWXypwjJ0zDJvoR%2BgmS0WztlZfjH9qYEHOSYB3E%2FQd6umdogTGwtKXhrUh52IiPZRxjzkS7mJGHbEZzGGNh%2BuG%2BsL2CIkdInLIi5xi7UqsKUz1h4mJPRWWpL0GJsKnH1R1g9w3L0HUTd5UXvT4Z0%2FHJEzlk28Tt%2BTh28%2FuelF5kq3YAzEWyR1SLaoEsXVdKu72WOFYQODabJIC8yspyWd%2FWl3Kn9uoni3E3S85JhrhldpQW4WbD3Miure0clu7E0BVAaG6U%2B0FIa8u6S3W49lWQu9p4zEa8xyl4T0ZFmjPUWzEzIdcKWr2ZiSSi2krkmoUZrEXRPsTetlJqsSISR5svUm3aY5FMdNbeKJoDS%2Fc%2B2IgHOV8f1zvpuENDH5MvhLLN4TCC0sAOJWZsBMy4yBRfV2y3xA65FhlIiyd%2BJdDF9062UwVSnW67I6pZiMHu6ladbaz80mpShJitRxIo8ycP5cuomSbadIhTClvp5o6SiG2WuXO7by%2FftO7Xx%2BeHH5I%2FGSH3tUvJ%2Bl3a7izQdLN1DlttSCie4WMuZpL00VY92zpzOG6qro80MpjFPqdwvV5blLkNiplM2ISHVEA%2Bp4BALo5G8jxF4%2FJ18HtDuahxl6TfXWSdGtBpALdY3l7vrMO1D5kLyVtaKtBHHa8UaVn8Ew7A9wOUyPMsSKeoctUCCijAtifUNiWoKg4iUyfHlrDtjfms5sbje1JFqLJSFlQxl5aUCxjvifTst8BGFEuBrGeqbc6jAQmwqlLUp0bkJem9Kn0uu1giOieZmSyl1JTUpOmHFY1p8rIacYkSl57hoqcMvTmZFOVHwd2%2FiSu3Z%2Fsam3Y9pBu%2BT0anNWtM9pOTySyghKySeUnJ6DTZZw7nmX1Z%2B0rPcTTQvgqYn2S9F8QWVpBSg83FOUSqTFup5bDjarSoIc9ZLjsPpygW5fxpf8OwJUK5E%2BFR2KLTrb3%2BoLhQGdYlLvzFFav0GhdVHhSxxFrGGi6w6UnPABTpKW1hhYONAsJistXF7CNUA9I5CYp8ssLKn4djQpKyyK0WX0whJwLqIlqHwk6oZZjpsEMWtGnn9ANj%2B7Y1n5rEMhKfK7IAvMTK6WhG2jM%2FKle88gLXhhSiCMK2Ip%2BGFaJA2mp%2F%2Bwj4AnPUnEoUeQPijQv4tuTBPMImz3ifOAHA2YjMnXCGKDw2myZb8bpwJaWwJCVA4ZGhr8I7PTNAq5mg5TJSPum6uQHiR4XhKDmwiqty%2Fl38caNjpHaYUPSL1OOqusW9DzOWPXXKGt9rRNvNt5IMTdNqX3klV%2FnKFd2gSCF%2B4h6IH7VXxb%2B6s9tkKUohTJm4NmTFEOojNpA2hvNwuSKdSHJayaL0jWCN1E79ZKRxs%2BCcMmeMPAeBMqxUCcnfmiGWZh49rKvsGKGxZcTrTEUzrHr4xB6vLNrM97l623%2FWnrm7bzNxoIPHW3glzC5vpt%2FUKVnAwDTmLdib%2BcRWYoLomcnSUqVydapE84MC1CCeCWuZJJzr%2FU0FKqfhUqn9Cihlb3QAwL59anxqbnlAyGVHUoR8UdW7lwesJmigOGh5glSXsrg2lDhW4UoH%2FUVIy3sejP4mK9ueRnl9RVuwZ8pLK0stldsmc17YxGXoc2sGC5LKymLpwj4NI71Jqv1Ts96EazSf9lYEDRRAo%2BZS%2F7HjTR0TyH9byaw%2BSv1MuCMzbfl0KeoOaB9hQp8mn9mX2vfr37%2FP5GfRnE77RpuphY82hDRCShKT2klH4ozYgOUWEBP547JGyvD7rYeGM52sIMX9o2uWPrNwqOutJubaWNv9fxtPJHmFW31nauUhi1XyR2aOsJbM6cEZYNlAwPYA7VhJz4oQNWEe5pEgSpx5czCqRtKpNruBHL24QKRE%2BVBUfbSc2o%2BfjJVvlpVTIOU71S9EC2PkKxdCr22RJXhTpeq2MTQ1JyC7zPy8yx0z2eRzCWPEzsYeSy6dyWwPpuO3G8t%2BdS76j29zecK%2FBIH%2B0%2F5yZ57fG2OSws5mtsPLD%2BbGUwV9B6X63tzA4YMCWU9um24pTmzZL%2BpLVA0JaqtrKHvqT0fbBVFmlUJIxMqUgoeDMXdqh9dC3N8v6qUlq1tlvTZGTn2aelcgWiUlDr0CUWWGXitkHyDoDawcWQ3SnQ49%2F2SlIpsl5S89j%2Bg5Zerbjgw9zcivZt7b9FGePWj%2B18nVN%2B%2Bn%2BtqedK3RoCd3%2B8KVUanWZDNB%2Bix8vGG1Ju9e23zzB8w2ItU0Spj%2FgAkjY2BFAchQlCUyLwwYDGUu1BHnJFI7uZb68vezj9Iv86bZR9kn8Q4%2FXqLk8mlMxU%2F4VriUcrCRF22oOz%2FFK%2B3%2B%2Br8HiwHM8vqL9TmXva9PYHamq%2Fv9tOrqqyvv%2FrRqtXTW30%2BrHv7TqrySraxhKNmHI35I9ZxDs8nsoQf7d4ZtX2vzyzvInFjmjf4FcYqxKP2S%2FJ7dy66fdYcwJxcwJneVP%2BueO6G143fdq4lGLX7kPZXERxAn6qrOYO%2Bp3OOIk5BLjJaLU%2FF0jsudLgosSE%2BbEStRqe1yijc9hIL04Th4mHxFZO1c2P0f%3C%2Fdiagram%3E%3Cdiagram%20id%3D%22Kc0jzny5qIj9sLP9HgPE%22%20name%3D%22Page-2%22%3EldG9DsIgEADgp2E0qUWNs%2FUvJi5q4s9iSDkLSnuVYlp9emugVuKiC4GP4%2BAOQqO0mmmWiyVyUCQMeEXomIThcDCsxxfcLfQotZBoyS11W1jLBzgMnN4kh8ILNIjKyNzHGLMMYuMZ0xpLP%2ByEyr81Zwl8wTpm6lu3khvhyuoHrc9BJqK5uRu4nZQ1wQ4KwTiWH0QnhEYa0dhZWkWgXr1r%2BrLf3LaH83R0XV12CziWQhfnjk02%2FefIuwQNmfk1dT1pn1YvvP%2Blkyc%3D%3C%2Fdiagram%3E%3C%2Fmxfile%3E"></iframe>

  

+ Para ejecutar el flujo de trabajo anterior necesitaremos la siguiente información además de la base de datos que ya debemos de tener descargada:
  + [Consultas](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/geoinfo/input/consultas.zip) en lenguaje SQL que usaremos para agregar la información de la manera necesaria para obtener la densidad (tanto en el caso de la densidad por parcela como en la densidad por estrato).
  + [Distribución de los pinares de Sierra Nevada](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/geoinfo/input/distribucion_pinares_sierra_nevada.zip). Se trata de un fichero de formas (shapefile) que contiene los polígonos del mapa de vegetación que están ocupados por pinares de repoblación. 
+ Empezamos con el cálculo del mapa de densidad mediante imputación. Para ello seguimos los pasos descritos en el flujo de trabajo. Por si no consigues tus propios resultados, ahí van los que he generado yo:
  + Tabla *[densidad_x_estrato.txt](https://raw.githubusercontent.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/main/geoinfo/output/densidad_x_estrato.txt)* generada a partir de la base de datos.
  + Fichero de formas mostrando la densidad de cada polígono de Sierra Nevada: *[densidad_x_estrato.zip](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/geoinfo/output/densidad_x_estrato.zip)*
  + El mismo mapa anterior pero en formato raster: *[densidad_x_estrato.tif](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/geoinfo/output/densidad_x_estrato.tif)*
  + Mapa de los pinares de repoblación en formato raster: *[mascara_pinares.tif](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/geoinfo/output/mascara_pinares.tif)*
  + Densidad de los pinares en formato raster: *[densidad_pinar_x_estratos.tif](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/raw/main/geoinfo/output/densidad_pinar_x_estratos.tif)*


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
+ Por si te resulta útil, [aquí](https://raw.githubusercontent.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/main/geoinfo/input/enp_sierra_nevada.zip) puedes descargar el mapa con los límites del espacio protegido de Sierra Nevada.



## Vídeo resumen

El siguiente vídeo describe la ejecución de todo el flujo de trabajo. No es una transcripción de una clase, sino una narración del proceso paso a paso.

<iframe width="560" height="315" src="https://www.youtube.com/embed/AYFUjLEC7h4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>



