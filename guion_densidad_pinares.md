# Caracterización de la distribución espacial de la densidad de los pinares de repoblación en Sierra Nevada


> + **_Versión_**: 2023-2024
> + **_Asignatura (titulación)_**: Ciclo de gestión del dato: ecoinformática (máster conservación, gestión y restauración de la biodiversidad. UGR) 
> + **_Autor_**:  María Suárez Muñoz (bv2sumum@uco.es) y Curro Bonet-García (fjbonet@uco.es)
> + **_Duración_**: Seis horas (tres sesiones de dos horas)


## Objetivos del acto docente

Este acto docente tiene los siguientes objetivos:

+ Disciplinares: objetivos relacionados con la ecología:
  + Conocer cómo cambia en el dominio del espacio la distribución de la densidad de los pinares de repoblación de Sierra Nevada.
  + Generar un mapa de densidad de los pinares de repoblación en Sierra Nevada.
+ Instrumentales: Objetivos relacionados con la adquisición de competencias en el manejo de herramientas.
  + Aprender cómo se puede recoger información forestal de forma sistemática. El concepto de inventario forestal.
  + Conocer y aprender a usar bases de datos relacionales.
  + Explorar la asignación de valores de una variable en un mapa en puntos donde no se han tomado datos de campo. Para ello usaremos la interpolación e imputación espacial. 


## Hilo argumental

El hilo argumental seguido en este acto docente es una cascada de razonamientos. Pretende mostrar el proceso por el que abordamos un problema nuevo como el que nos ocupa. Es un proceso un poco detectivesco porque casi nunca tenemos toda la información sobre la mesa (y bien documentada) desde el principio. Iremos poco a poco construyendo un camino y recorriéndolo a la vez.

+ La densidad de los pinares es una variable clave que explica la regeneración de la encina bajo su dosel.

+ Por tanto necesitamos generar un mapa que muestre la distribución de la densidad de los pinares en Sierra Nevada.

+ Pero en ecología resulta muy difícil recopilar información para cada punto del paisaje... ¿o no?

+ Normalmente tenemos información sólo para algunos puntos del paisaje, aquéllos donde hemos muestreado. En el caso de los bosques esta información se suele recoger en inventarios forestales. Esta presentación describe qué son los inventarios forestales:

<p><iframe src="https://prezi.com/view/O1mx63lp7jWX4fqekRZ7/embed" width="900" height="600"> </iframe></p>

+ Para obtener el mapa que necesitamos contamos con un inventario forestal realizado en Sierra Nevada en 2005 (SinfoNevada). [Este](https://doi.org/10.3897/phytokeys.35.6363) datapaper describe la estructura de SinfoNevada. [Aquí](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/blob/main/geoinfo/input/bbdd_sinfonevada_densidad.mdb) podéis descargar una versión simplificada de este inventario.

+ ¡Oh, sorpresa! Al analizar la estructura del inventario forestal tomamos conciencia que sus datos se almacenan en una base de datos relacional.

+ Entonces para acceder a dichos datos necesitaremos aprender cómo funcionan estos instrumentos... Esta presentación explica los conceptos más importantes para comprender qué es y cómo se hace una Base de Datos:

<p><iframe src="https://prezi.com/view/8Nl5PECsoSUnymoqyFvk/embed" width="900" height="600"> </iframe></p>

+ Después de conocer cómo se organizan los datos en una base de datos (BD), vamos a construir una base de datos sencilla que recoja información sobre un inventario forestal. La **descripción literal** de esta base de datos sería:

>*La metodología consiste en visitar una serie de parcelas que se delimitan con puntos. Cada parcela se caracteriza por su nombre, ubicación, altitud, pendiente media, orientación media, temperatura y precipitación media anual. En cada parcela caracterizamos los “pies de monte alto”, que son las plantas consideradas adultas o con porte arborescente. Para ello contamos el número de árboles mayores (más de 2 m de altura), el número de árboles menores, así como la densidad total de árboles y la regeneración (densidad del regenerado). También calculamos el área basimétrica de la parcela. Además, en cada visita anotamos: los nombres de las personas que realizan la visita, la hora de inicio-fin y el tipo de suelo de la parcela.*

+ A partir de ella construiremos un **diagrama entidad-relación**

+ Una vez creada la estructura conceptual de nuestra BD podremos crear el objeto digital que la contiene. Para ello usaremos el programa *Access* de la suite de ofimática *Microsoft Office*.

+ En este vídeo se muestra el proceso completo:

<iframe width="560" height="315" src="https://youtu.be/QqiNn2S8Qss" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

+ Es importante tener en cuenta que la base de datos que hemos creado está vacía. Solo hemos aprendido a construir su estructura. Para poner datos en su interior basta con ir rellenando a mano las tablas. Otra opción más elaborada es usar formularios de carga de datos. No tenemos tiempo en esta asignatura de aprender cómo se construyen formularios, pero te resultará fácil de aprenderlo una vez que has entendido bien cómo se construye una base de datos. 

+ Ya disponemos del conocimiento necesario para intepretar correctamente la información contenida en el inventario forestal SinfoNevada.

+ Ahora vamos a pensar qué pasos debemos seguir para extraer de la base de datos la información que necesitamos para generar el mapa de densidad del pinar. Recordamos que SinfoNevada recoge datos de parcelas siguiendo un muestreo estratificado por estratos.

+ Para entender mejor esto descargamos la capa SIG que contiene la distribución de los estratos. [Aquí](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/blob/main/geoinfo/input/estratos.zip) puedes descargar el shapefile con los estratos. Y aprovechamos esta necesidad para aprender un poco el manejo de qGIS. Concretamente hacemos lo siguiente:
  + Cargar una capa vectorial.
  + Cargar un servicio WMS con una ortofoto.
  + Cambiar los colores de los polígonos.
  + Guardar un proyecto de qGIS.

+ En la base de datos identificamos varios campos que podrían ser de utilidad para calcular la densidad de los pinares. En concreto, vemos que hay dos campos de densidad en las tablas: **PARCELA_DATOS_CEPAS_M_BAJO** y **PARCELA_DATOS_PIES_M_ALTO**. Sabiamente deducimos que con estos dos campos podemos obtener una capa que asigne un valor de densidad a cada parcela. Decidimos que los dos valores de los campos anteriores son sumables.

+ También observamos que hay una tabla **explota_PARCELAS_POR_ESTRATO20**. Esta tabla asigna a cada parcela el código del estrato en el que se encuentra. Recordemos que los estratos son polígonos que tienen la misma orientación, vegetación, piso bioclimático y tipo de suelo. Es una técnica que usamos para estratificar el muestreo antes de hacer el inventario.

+ La visualización de las parcelas en el SIG nos permite pensar dos formas distintas de obtener un mapa que muestre la distribución de la densidad en nuestra zona de estudio. Como necesitamos datos de densidad para todo el territorio de Sierra Nevada y SinfoNevada sólo tiene información para una serie de puntos, usaremos técnicas de análisis que nos permitan "extender" los datos de densidad desde los puntos de inventario a lugares en los que no se han tomado datos de campo.

+ Para ello usaremos dos técnicas muy diferentes, la interpolación y la imputación espacial, lo que dará lugar a dos resultados distintos.

![image](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/blob/main/imagenes/imputacion.jpg)

![image](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/blob/main/imagenes/interpolacion.jpg)

+ Ahora estamos en disposición de construir un flujo de trabajo detallado con lo que queremos hacer. Hemos identificado una fuente de datos importante. La hemos analizado y hemos encontrado dos posibles formas de hacer lo que necesitamos. Hasta aquí hemos realizado una prospección general de los datos y hemos esbozado posibles análisis. Ahora que sabemos lo que podemos hacer, es momento de organizar bien todos los procesamientos que necesitamos. Es ahora cuando construimos un flujo de trabajo. Puedes verlo abajo o descargarlo [aquí](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/blob/main/presentaciones/workflow_densidad.drawio) y abrirlo con https://app.diagrams.net/. 

<iframe frameborder="0" style="width:100%;height:1227px;" src="https://viewer.diagrams.net/?tags=%7B%7D&highlight=0000ff&edit=_blank&layers=1&nav=1&title=workflow_densidad.drawio#R%3Cmxfile%20pages%3D%222%22%3E%3Cdiagram%20id%3D%22C5RBs43oDa-KdzZeNtuy%22%20name%3D%22Page-1%22%3E7V1bd5pKFP41rnXOAyyY4foYo6a59DSJbdP0xUUQlQgMAYyaX39muCiXETFRwbRd57Qy3Ofbl2%2Fv2TO04Lm9uPA0d%2FIVDQ2rBbjhogU7LQB4nhfwP6RlGbUAkVOilrFnDuOj1g19882IG7m4dWYODT9zYICQFZhutlFHjmPoQaZN8zw0zx42Qlb2rq42NgoNfV2ziq0P5jCYxK8BobTe8cUwx5Pk1pIYv7KtJUfHr%2BJPtCGap5pgtwXPPYSC6Je9ODcs0n1JxzxcLh%2Bsm6l0cXXnv2g%2F2tff%2F%2FvJRBfr7XLK6h08wwnefenOuXfdQ7ezhdXRBoL9xZ%2BLFgOjS79q1izusPhdg2XSgx6aOUODXIRvwfZ8YgZG39V0sneOhQa3TQLbindrnh7LAC%2FjTQ8FWmAiB28zKocb%2FMBDU%2BMcWcjDbQ5y8KHtkWlZSVMLwF6vd9ZVcHvF146759XwAmORQj3uhgsD2UbgLfEh8V4GcJCFYnRaLNgML0FWitvma0nhsaywQI3aJylBkSBkhURQYyEdr261xgH%2FiKGgwzLjncVA%2Bem0pzfqUjd%2FKRfoB8OrteKCQWh3ZI4je0bICVLto%2FAPDbQuJ4ADgqayEp9BTJRZUMRLlWVWocAFZJEVD4WWUjdaQ%2BlJEqUiKhgqoOsHVaUUDDEwKs%2FKFGCUFQAZXIDCcvDjwLSV%2Bxu7N%2FrR9ri56J27D%2BaLzSTWP4WDMcTuId5EXjBBY%2BRoVnfd2l4jRbp6fcwNQm6Mz7MRBMsYIW0WoCx6xsIMfpHT8ftGW4%2BpPZ1FfOVwY5lsOPh9UyeRzcf0vvVp4VZy3m64%2Bmjm6XFH3N39%2BiX%2B%2FqV%2Bu5Ff9duRrXQeIZMYukDzxkZ8Qe5emLrX5jWYPIvTl9%2BcNAcTBsZXJN1ZKieeYWHZfc164x2A%2FuEb3renZ0INAGdpT4YVndq%2FuyG0xHB8c6gNBy0gWfh5208e%2FjUmvxYD3Cs6vnt0y0ROSl47K1hp3Z0g%2B2nmb9dbYitjsZAPqHSCxLGJiVm5L5VjufQfqaCBAsUsJm07III3U6CUgpTteqogpZgUrevHWBvdD%2FbjilxqT8lludL%2Bhdgm5WwaI3BcpQ7F5nB3a0bp0oKBo%2Fde8s5pzxNpApHFTF9KLzOU7GD8UErP8AGyu1jvS5RH0%2FXhU3Il%2FHTRxaJ9RZNqWZjNGxXcmu9GFH9kLoiBpShIiYzs5Jb4LFeAQoJKCr7kshl3xH3cFdHfARwGqb7p4MOMV22IDR3X6f7Xv%2BycdapCh0Mal%2FwcaoHmB8irAOLBMFOL%2BkUBSBBZ5UBsga%2BHLbzT87%2BHZbyfLdAxVqqyBbBvthCfeovMUHdiEQJqPqgTcvocvVN82lpezjxPW6YOc8kBfsmNODV%2FIz59ve0nyGpOXqNHWEvvqlf2zIoMHDxoWAgruGblpFgRrwjZLsaxO47oa6JEFQOUYqDYYJOzZ%2FORxBBbzYcgH8d8YF6dk6Cc9YietGA93q%2Bve6EAK3VPhTwl7r9U52G6Z4s6n%2FAF0w7TrmmhIopq6pp1ZpljkqkIiDCuWm%2FIO98i34zzGE8oCJCNDwg7A%2FPN6TgUa1qmKbzZWcIdORqRjJ%2BnMwkCklc%2BI9CAnj50IGti8j8yscJ4rI7vCHqE6%2BB%2FSLtPNpHtkt%2FM2Fq6E2ZkWgYzQp6tBQxBGIgS6UXSqvuvyZ5g6RoMj8ME1xkfyp4JkliI8rYHeZBi0eBhLBpdV2uxaO%2B3TGdS955ZLC%2B6zzejnjSegHHPihPpFQwTt2%2FDtGMWpIpGl%2Bc2agmwQc7QKjKryimx5gtirQKKo%2BZZDqbOOmLcDWOPtO9ojvUnbmPC7sRsVDdZctZcATpARwjBqVm%2B1fusQfuquSRsjjmxX%2BTLpvOKO0nzTFTo%2F8QX4n7ULMuw0NjTiEdzDc%2FED2x4%2BX236x07UuZIajBd4zkor%2F%2FDenMQNNN6RwEI7gEgegCu%2FsFsWKjsdMSanc4Px%2FTImyPytxZ45tOMqjv%2FdPvf7weA%2Bze6s2U60%2BgCWZY2RLrPvoxNn0UeJlM9yOI4rmc48S78zww%2FyMDWnJmGRag3R97UdMaDuRlMBq%2F46chz9KIfA9dDLqGchs%2BGEAP4jNm6z6zbK7hMoTwn3bzAF2bMrqyyavpP0ew2ciRAgM0jKrwE2GxKiReL4wA0cqLubiTfz7mFP4CNCHADqM1nI6VmvMhGkng%2BsbF5u5rJ4J0uIdkB0LoICYB%2FMiGRKxISEdRMSC5trCf4McnV8P93F5f9Kv5GPikvTx30Z3koKZICZRm7Kl7ioMHwx3T3BTXyoHT%2FtvxyJatv%2BuTpiZe%2B3mtJOqWsPKrROaJSqd%2BuHnWXypwjJ0zDJvoR%2BgmS0WztlZfjH9qYEHOSYB3E%2FQd6umdogTGwtKXhrUh52IiPZRxjzkS7mJGHbEZzGGNh%2BuG%2BsL2CIkdInLIi5xi7UqsKUz1h4mJPRWWpL0GJsKnH1R1g9w3L0HUTd5UXvT4Z0%2FHJEzlk28Tt%2BTh28%2FuelF5kq3YAzEWyR1SLaoEsXVdKu72WOFYQODabJIC8yspyWd%2FWl3Kn9uoni3E3S85JhrhldpQW4WbD3Miure0clu7E0BVAaG6U%2B0FIa8u6S3W49lWQu9p4zEa8xyl4T0ZFmjPUWzEzIdcKWr2ZiSSi2krkmoUZrEXRPsTetlJqsSISR5svUm3aY5FMdNbeKJoDS%2Fc%2B2IgHOV8f1zvpuENDH5MvhLLN4TCC0sAOJWZsBMy4yBRfV2y3xA65FhlIiyd%2BJdDF9062UwVSnW67I6pZiMHu6ladbaz80mpShJitRxIo8ycP5cuomSbadIhTClvp5o6SiG2WuXO7by%2FftO7Xx%2BeHH5I%2FGSH3tUvJ%2Bl3a7izQdLN1DlttSCie4WMuZpL00VY92zpzOG6qro80MpjFPqdwvV5blLkNiplM2ISHVEA%2Bp4BALo5G8jxF4%2FJ18HtDuahxl6TfXWSdGtBpALdY3l7vrMO1D5kLyVtaKtBHHa8UaVn8Ew7A9wOUyPMsSKeoctUCCijAtifUNiWoKg4iUyfHlrDtjfms5sbje1JFqLJSFlQxl5aUCxjvifTst8BGFEuBrGeqbc6jAQmwqlLUp0bkJem9Kn0uu1giOieZmSyl1JTUpOmHFY1p8rIacYkSl57hoqcMvTmZFOVHwd2%2FiSu3Z%2Fsam3Y9pBu%2BT0anNWtM9pOTySyghKySeUnJ6DTZZw7nmX1Z%2B0rPcTTQvgqYn2S9F8QWVpBSg83FOUSqTFup5bDjarSoIc9ZLjsPpygW5fxpf8OwJUK5E%2BFR2KLTrb3%2BoLhQGdYlLvzFFav0GhdVHhSxxFrGGi6w6UnPABTpKW1hhYONAsJistXF7CNUA9I5CYp8ssLKn4djQpKyyK0WX0whJwLqIlqHwk6oZZjpsEMWtGnn9ANj%2B7Y1n5rEMhKfK7IAvMTK6WhG2jM%2FKle88gLXhhSiCMK2Ip%2BGFaJA2mp%2F%2Bwj4AnPUnEoUeQPijQv4tuTBPMImz3ifOAHA2YjMnXCGKDw2myZb8bpwJaWwJCVA4ZGhr8I7PTNAq5mg5TJSPum6uQHiR4XhKDmwiqty%2Fl38caNjpHaYUPSL1OOqusW9DzOWPXXKGt9rRNvNt5IMTdNqX3klV%2FnKFd2gSCF%2B4h6IH7VXxb%2B6s9tkKUohTJm4NmTFEOojNpA2hvNwuSKdSHJayaL0jWCN1E79ZKRxs%2BCcMmeMPAeBMqxUCcnfmiGWZh49rKvsGKGxZcTrTEUzrHr4xB6vLNrM97l623%2FWnrm7bzNxoIPHW3glzC5vpt%2FUKVnAwDTmLdib%2BcRWYoLomcnSUqVydapE84MC1CCeCWuZJJzr%2FU0FKqfhUqn9Cihlb3QAwL59anxqbnlAyGVHUoR8UdW7lwesJmigOGh5glSXsrg2lDhW4UoH%2FUVIy3sejP4mK9ueRnl9RVuwZ8pLK0stldsmc17YxGXoc2sGC5LKymLpwj4NI71Jqv1Ts96EazSf9lYEDRRAo%2BZS%2F7HjTR0TyH9byaw%2BSv1MuCMzbfl0KeoOaB9hQp8mn9mX2vfr37%2FP5GfRnE77RpuphY82hDRCShKT2klH4ozYgOUWEBP547JGyvD7rYeGM52sIMX9o2uWPrNwqOutJubaWNv9fxtPJHmFW31nauUhi1XyR2aOsJbM6cEZYNlAwPYA7VhJz4oQNWEe5pEgSpx5czCqRtKpNruBHL24QKRE%2BVBUfbSc2o%2BfjJVvlpVTIOU71S9EC2PkKxdCr22RJXhTpeq2MTQ1JyC7zPy8yx0z2eRzCWPEzsYeSy6dyWwPpuO3G8t%2BdS76j29zecK%2FBIH%2B0%2F5yZ57fG2OSws5mtsPLD%2BbGUwV9B6X63tzA4YMCWU9um24pTmzZL%2BpLVA0JaqtrKHvqT0fbBVFmlUJIxMqUgoeDMXdqh9dC3N8v6qUlq1tlvTZGTn2aelcgWiUlDr0CUWWGXitkHyDoDawcWQ3SnQ49%2F2SlIpsl5S89j%2Bg5Zerbjgw9zcivZt7b9FGePWj%2B18nVN%2B%2Bn%2BtqedK3RoCd3%2B8KVUanWZDNB%2Bix8vGG1Ju9e23zzB8w2ItU0Spj%2FgAkjY2BFAchQlCUyLwwYDGUu1BHnJFI7uZb68vezj9Iv86bZR9kn8Q4%2FXqLk8mlMxU%2F4VriUcrCRF22oOz%2FFK%2B3%2B%2Br8HiwHM8vqL9TmXva9PYHamq%2Fv9tOrqqyvv%2FrRqtXTW30%2BrHv7TqrySraxhKNmHI35I9ZxDs8nsoQf7d4ZtX2vzyzvInFjmjf4FcYqxKP2S%2FJ7dy66fdYcwJxcwJneVP%2BueO6G143fdq4lGLX7kPZXERxAn6qrOYO%2Bp3OOIk5BLjJaLU%2FF0jsudLgosSE%2BbEStRqe1yijc9hIL04Th4mHxFZO1c2P0f%3C%2Fdiagram%3E%3Cdiagram%20id%3D%22Kc0jzny5qIj9sLP9HgPE%22%20name%3D%22Page-2%22%3EldG9DsIgEADgp2E0qUWNs%2FUvJi5q4s9iSDkLSnuVYlp9emugVuKiC4GP4%2BAOQqO0mmmWiyVyUCQMeEXomIThcDCsxxfcLfQotZBoyS11W1jLBzgMnN4kh8ILNIjKyNzHGLMMYuMZ0xpLP%2ByEyr81Zwl8wTpm6lu3khvhyuoHrc9BJqK5uRu4nZQ1wQ4KwTiWH0QnhEYa0dhZWkWgXr1r%2BrLf3LaH83R0XV12CziWQhfnjk02%2FefIuwQNmfk1dT1pn1YvvP%2Blkyc%3D%3C%2Fdiagram%3E%3C%2Fmxfile%3E"></iframe>

+ Para ejecutar el flujo de trabajo anterior necesitaremos extraer de la base de datos información agregada de acuerdo a nuestras necesidades. Para ello usamos las consultas. Gracias a esta herramienta podemos extraer la información que queramos de la base de datos con el nivel de agregación que necesitemos. Para hacer consultas se usa el lenguaje [SQL](https://es.wikipedia.org/wiki/SQL). Si quieres profundizar en esto puedes echarle un vistazo a [estas](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/blob/main/biblio/taller_SQL.ppt) diapos y a [este](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/blob/main/biblio/manual_sql.pdf) manual de SQL. Haremos una consulta para extraer la densidad por estrato y otra para extraer la densidad por parcela.

+ [Estas consultas](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/blob/main/geoinfo/input/consultas.zip) en lenguaje SQL nos permiten agregar la información de la manera necesaria para obtener la densidad (tanto en el caso de la densidad por parcela como en la densidad por estrato).

+ Puede resultar útil visualizar los [límites](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/blob/main/geoinfo/input/enp_sierra_nevada.zip) del espacio protegido de Sierra Nevada. 

+ También será necesario considerar la [distribución de los pinares en Sierra Nevada](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/blob/main/geoinfo/input/distribucion_pinares_sierra_nevada.zip). Se trata de un fichero vectorial que contiene los polígonos del mapa de vegetación que están ocupados por pinares de repoblación. 

+ Empezamos entonces con el cálculo del mapa de densidad mediante imputación. Para ello seguimos los pasos descritos en el flujo de trabajo. Por si no consigues tus propios resultados, ahí van los archivos intermedios:
  + Tabla *[densidad_x_estrato.txt](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/blob/main/geoinfo/output/densidad_x_estrato.txt)* generada a partir de la base de datos.
  + Fichero de formas mostrando la densidad de cada polígono de Sierra Nevada: *[densidad_x_estrato.zip](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/blob/main/geoinfo/output/densidad_x_estrato.zip)*
  + El mismo mapa anterior pero en formato raster: *[densidad_x_estrato.tif](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/blob/main/geoinfo/output/densidad_x_estrato.tif)*
  + Mapa de los pinares de repoblación en formato raster: *[mascara_pinares.tif](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/blob/main/geoinfo/output/mascara_pinares.tif)*
  + Densidad de los pinares en formato raster: *[densidad_pinar_x_estratos.tif](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/blob/main/geoinfo/output/densidad_pinar_x_estratos.tif)*

+ Una vez ejecutada esta primera "hebra" del flujo de trabajo, la imputación, abordaremos la obtención de un mapa de densidad mediante interpolación. Esta técnica es algo más compleja de entender, así que aquí tenéis algunas referencias por si queréis profundizar en el tema:
  + [Spatial interpolation: a brief introduction](http://www.bisolutions.us/A-Brief-Introduction-to-Spatial-Interpolation.php): Muy buen resumen de la teoría de la interpolación y de los dos grandes grupos de técnicas que utilizaremos.
  + [Introduction to spatial analysis](http://planet.botany.uwc.ac.za/nisl/GIS/spatial/index.htm): Texto bastante completo en el que se detallan varias técnicas de interpolación. Hace mucho hincapié en la interpolación de modelos digitales de elevaciones.
  + [Spatial interpolation with IDW method explained](https://www.geodose.com/2019/03/spatial-interpolation-inverse-distance-weighting-idw.html): Guía muy detallada y completa sobre cómo aplicar el método IDW en QGIS.

+ Ahora podemos comparar los mapas obtenidos. Además, veremos otro mapa de densidad obtenido mediante teledetección. Reflexionaremos sobre los resultados obtenidos en los tres casos.

+ Al igual que antes, abajo puedes ver los resultados intermedios que obtendríamos en cada paso:
  + Tabla *[densidad_x_parcela.txt](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/blob/main/geoinfo/output/densidad_x_parcela.txt)* generada a partir de la base de datos del inventario forestal.
  + Fichero de formas mostrando la densidad de cada parcela de Sierra Nevada: *[densidad_x_parcela.zip](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/blob/main/geoinfo/output/densidad_x_parcela.zip)*
  + Fichero de formas mostrando la densidad de cada parcela de pinar: *[densidad_pinar_x_parcela.zip](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/blob/main/geoinfo/output/densidad_pinar_x_parcela.zip)*
  + Capa raster mostrando la densidad de toda Sierra Nevada interpolando la densidad de las parcelas con pinar: *[densidad_interpolada.tif](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/blob/main/geoinfo/output/densidad_interpolada.tif)*. Ojo, esta capa está en un sistema de referencia diferente al resto (WGS84)
  + Capa raster mostrando la densidad de los pinares de Sierra Nevada: *[densidad_pinar_interpolada.tif](https://github.com/aprendiendo-cosas/TP_densidad_pinar_ecoinf_UGR/blob/main/geoinfo/output/densidad_pinar_interpolada.tif)*

## Vídeo resumen

El siguiente vídeo describe la ejecución del flujo de trabajo seguido para generar los mapas de densidad por imputación e interpolación espacial. No es una transcripción de una clase, sino una narración del proceso paso a paso.

<iframe width="560" height="315" src="https://www.youtube.com/embed/AYFUjLEC7h4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

