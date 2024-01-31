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

+ Es importante tener en cuenta que la base de datos que hemos creado está vacía. Solo hemos aprendido a construir su estructura. Para poner datos en su interior basta con ir rellenando a mano las tablas. Otra opción más elaborada es usar formularios de carga de datos. No tenemos tiempo en esta asignatura de aprender cómo se construyen formularios, pero te resultará fácil de aprenderlo una vez que has entendido bien cómo se construye una base de datos. 

+ Al finalizar esta sesión dispondremos del conocimiento necesario para intepretar correctamente la información contenida en el inventario forestal SinfoNevada.

