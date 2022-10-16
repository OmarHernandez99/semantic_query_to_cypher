# Introduccion

En el mundo en que vivimos, diariamente se produce mucha informacion relevante, en formato visual, de audio o texto, especialmente producto del trabajo creativo e investigativo de muchas personas. La mayor parte de esta información es de interés almacenerla, lo cual puede hacerse de forma estructurada o no.

Guardar la información de forma estructurada tiene varias ventajas, esto hace mucho mas fácil su consulta, ampliación y modificación. Especialistas de sus respectivos campos necesitan acceder a dicha informacion para toma de decisiones.

A estos corpus de información sobre determinado tema, les denominamos bases de conocimientos, y, principalmente por su ubicuidad, el recobro de informacion de estas mediante consultas se ha vuelto una tarea de primera necesidad.

La información guardada en bases de conocimiento de forma estructurada, como en base de datos relacionales, grafos de conocimiento etc., hace necesario el conocer un lenguaje de consulta especifico para interactuar con estas. Los especialistas de cada campo no estan familiarizados en su mayoría con los lenguajes de consulta necesarios para acceder correctamente a la información que necesitan. Es necesaria entonces la intervención de un experto en el ámbito, solamente para traducir la consulta.

Esto encarece y ralentiza el proceso de acceder a la informacion, por el hecho de necesitar un intermediario para mediar entre la base de conocimiento y la persona que quiere buscar información en ella. Si existiera alguna forma de que los expertos de cada campo pudieran hacer consultas sobre la información almacenada, en un lenguaje cercano al natural, esto trajera consigo la posibilidad de acceder a información crítica en tiempo real, sin retrasos y con un costo monetario menor. Disímiles campos entre los que se encuentran la medicina, demografía, deporte y la economía se vieran afectados de forma muy positiva con tal funcionalidad.

Este cometido trae consigo no pocos retos. Entre los principales se encuentra la gran ambiguedad que presenta el lenguaje natural, ya que en la elaboración de una consulta casi siempre intervienen el contexto y el sentido común; conocimientos que no son triviales de modelar ni de enseñar a los sistemas expertos.

Los grafos de conocimiento son estructuras que han venido ganando popularidad como formas de guardar información desde hace varios años. Son capaces de modelar una amplísima gama de escenarios reales manteniendo un nivel alto de simplicidad y lectura por humanos.

Por otro lado, el avance en la comprensión del lenguaje natural, con algunos modelos de aprendisaje de máquina como GPT-3 y BERT, ha permitido acortar la distancia entre lo que pueden entender los sistemas expertos y lo que expresa el hombre en lenguaje común. Estos modelos ayudan a traducir con cierto grado de certeza, las palabras del lenguaje natural, a estructuras llamadas embbedings, que incorporan el concepto de cercanía vectorial cuando dos palabras son semánticamente cercanas.

En lo que respecta a la comprensión del lenguaje natural y su uso en consultas a bases de conocimiento existen investigaciones que han logrado buenos resultados por varias vías distintas. Principalmente se hacen análisis sintácticos y semánticos sobre la consulta, muchas veces asistidos por diccionarios o mapas sobre la base de conocimiento en cuestión. Se usan modelos de paráphrasis como técnica de aumento de datos y principalmente Transformers para llevar de la consulta ya curada al lenguaje de consulta formal, en la mayoria de los casos, SQL.

Lo expuesto anteriormente provee un marco propicio para idear una solución que se apoye en lo investigado hasta ahora en los campos de teoría de grafos y procesamiento del lenguaje natural, sumándole además toda la teoría de lenguaje, para crear un framework que reciba una consulta en lenguaje natural, dirigida a una base de conocimiento, y devuelva con una elevada precisión los resultados buscados.


## Situacion Problemica
A pesar de los avances hechos en estos campos por la comunidad científica, no se encontró un sistema completo de público acceso que permita,dado una base de conocimiento con datos y una estructura arbitraria, hacer una consulta en lenguaje semi-natural, más cercano al hablado comúnmente, sobre la información almacenadda, y obtener los resultados con la posibilidad de incluir agregaciones y filtros. La mayor parte de las investigaciones analizadas que obtienen buenos resultados estan enfocadas solamente en bases de datos SQL, y no se pueden utilizar para la búsqueda en bases de datos con estructura de grafos, que es de la forma en que se encuentran gran parte de las bases de conocimiento a dia de hoy.

## Objetivos Generales
Comprobar la viabilidad de una arquitectura y un prototipo de un sistema que permita, dado una base de conocimiento con estructura arbitraria en forma de grafo,
introducir una consulta en lenguaje semi-natural sobre los datos almacenados, incluyendo agregaciones y filtros, y transformarla en una consulta en un lenguaje formal de consultas, entendible sin ambiguedades por lo motores de búsqueda para grafos, como lo es el lenguaje CYPHER. Lo anterior usando principalmente teoría de los lenguajes formales y procesamiento de lenguaje natural.

## Objetivos Especificos

- Investigar estado del arte sobre procesamiento de lenguaje natural, bases de conocimiento en forma de grafos y teoría de los lenguajes formales.
- Analizar las técnicas y resultados obtenidos por las investigaciones que resultaron más prometedoras en el punto anterior.
- Crear una Arquitectura divida en módulos:Parseo, Procesador Semántico, Creación de Árbol de Sintaxis Abstracta(AST), Módulo de Aumentación de los Datos, Generación de Consulta en Cypher.
- Implementar un prototipo de la arquitectura mencionada en el punto anterior en el lenguaje Python
- Realizar Experimentos para evaluar la viabilidad de la arquitectura propuesta usando los datos generados por el Módulo de Aumentación de Datos.

