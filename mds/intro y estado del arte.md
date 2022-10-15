
# Introduccion

En el mundo en que vivimos, diariamente se produce mucha informacion relevante, en formato visual, de audio o texto, especialmente producto del trabajo creativo e investigativo de muchas personas. La mayor parte de esta información es de interés almacenerla, lo cual puede hacerse de forma estructurada o no.

Guardar la información de forma estructurada tiene varias ventajas, esto hace mucho mas fácil su consulta, ampliación y modificación. Especialistas de sus respectivos campos necesitan acceder a dicha informacion para toma de decisiones.

A estos corpus de información sobre determinado tema, les denominamos bases de conocimientos, y, principalmente por su ubicuidad, el recobro de informacion de estas mediante consultas se ha vuelto una tarea de primera necesidad.

La información guardada en bases de conocimiento de forma estructurada, como en base de datos relacionales, grafos de conocimiento etc., hace necesario el conocer un lenguaje de consulta especifico para interactuar con estas. Los especialistas de cada campo no estan familiarizados en su mayoría con los lenguajes de consulta necesarios para acceder correctamente a la información que necesitan. Es necesaria entonces la intervención de un experto en el ámbito, solamente para traducir la consulta.

Esto encarece y ralentiza el proceso de acceder a la informacion, por el hecho de necesitar un intermediario para mediar entre la base de conocimiento y la persona que quiere buscar información en ella. Si existiera alguna forma de que los expertos de cada campo pudieran hacer consultas sobre la información almacenada, en un lenguaje cercano al natural, esto trajera consigo la posibilidad de acceder a información crítica en tiempo real, sin retrasos y con un costo monetario menor. Disímiles campos entre los que se encuentran la medicina, demografía, deporte y la economía se vieran afectados de forma muy positiva con tal funcionalidad.

Este cometido trae consigo no pocos retos. Entre los principales se encuentra la gran ambiguedad que presenta el lenguaje natural, ya que en la elaboración de una consulta casi siempre intervienen el contexto y el sentido común; conocimientos que no son triviales de modelar ni de enseñar a los sistemas expertos.

Los grafos de conocimiento son estructuras que han venido ganando popularidad como formas de guardar información desde hace varios años. Son capaces de modelar una amplísima gama de escenarios reales manteniendo un nivel alto de simplicidad y lectura por humanos.

Por otro lado, el avance en la comprensión del lenguaje natural, con algunos modelos de ML como GPT-3 y BERT, ha permitido acortar la distancia entre lo que pueden entender los sistemas expertos y lo que expresa el hombre en lenguaje común. Estos modelos ayudan a traducir con cierto grado de certeza, las palabras del lenguaje natural, a estructuras llamadas embbedings, que incorporan el concepto de cercanía vectorial cuando dos palabras son semánticamente cercanas.

Se podria entonces pensar en una solución que se ayude de lo investigado hasta ahora en estas dos áreas, sumándole toda la teoría de lenguaje, para crear un framework que reciba una consulta en lenguaje natural, dirigida a una base de conocimiento, y devuelva con una elevada precisión los resultados buscados.

//////////////////////////////////////////////
De aki abajo creo q necesito preguntarle mas cosas y decidirnos sobre otras
//////////////////////////////////////////////

## Pequenio State of the Art
En este tema existen investigaciones que han logrado buenos resultados por varias vías distintas. Principalmente se hacen análisis sintácticos y semánticos sobre la consulta, muchas veces asistidos por diccionarios o mapas sobre la base de conocimiento en cuestión. Se usan modelos de paráphrasis como técnica de aumento de datos y principalmente Transformers para llevar de la consulta ya curada al lenguaje de consulta formal, en la mayoria de los casos, SQL.

///////////////////////////////////////////////////////////


Con estas partes tengo duda, ya me ha dicho la estructura general que tienen, y eso es lo que está plasmado abajo, pero con respecto a la especificidad de mi problema es lo que tengo que poner y no se si nos tenemos que sentar para eso? espero respuesta por aki

## Situacion Problemica(por trabajar)
A pesar de los avances en nlp en estos momentos tod no existe un sistema que permita convertir de ln a un lenguaje de consulta que sea capaz de representar agregaciones, filtros ... 

## Objetivos Generales(por trabajar)
Hacer una estrategia para consultar bc en leng nat basada en el paradigma ..., que puede ser aplicada a bases de conocimiento arbitrarias, formadas por entidades y relaciones de dominio general ...

## Objetivos Especificos(por trabajar)
Vamos a identificar los modelos que hay pa esto en el estado del arte...? Parte de la propuesta de sol,
Hacer una implementacion computacional de [algo], vamos a diseniar un conjunto de casos de prueba pa experimentar y vamos a hacer una evaluacion cuantitativa y cualitativa bim bam


# Estado del Arte

Con respecto a la recuperación de información de una base de conocimiento, mediante consultas hechas en lenguaje natural se han hecho varias investigaciones científicas. En su gran mayoría siguen el siguiente patrón:

Data => NLQuery => Pre-Processing => Post-Processing => FormalQuery => Results

Se busca o crea un conjunto de datos(benchmark) en los que entrenar y probar el sistema. En el primer caso los más tenidos en cuenta son ( estos ..............). En el segundo, se suele usar las propias bases de conocimiento objeto de estudio para crear la data entrenante, una de las técnicas más empleadas aquí es *Random Walk*[], en la que se hace un recorrido aleatorio sobre un subconjunto de las entidades y relaciones de la base de conocimiento, y se elaboran consultas artificiales que respondan a dichas entidades y relaciones. De esta forma se pueden evaluar con precisión los resultados obtenidos porque se elaboran las respuestas de antemano.

Luego de tener la consulta de la que se quiere la respuesta, se suele hacer un Pre-Procesamiento. La mayor parte de los artículos realizan un análisis léxico y morfológico, en el que entran la tokenización, la lematización y el tagueo como parte de la oración(POS-tag), algunos además remueven *stop words* y ambiguedades. Otro grupo de investigaciones optan en esta etapa por usar paráfrasis, en las que llevan la consulta a una *representación canonica* o, al menos, a una con palabras e ideas más claras para el sistema. Usualmente se hace esta *traducción* a base de diccionarios especializados. Otros ejemplos de pre-procesamiento, aunque menos comunes, lo son el codificado de la consulta directo a embbeding y la transformación de la consulta a un grafo que la representa.

En el Post-Procesamiento hay variantes principales igualmente en las investigaciones realizadas. La mayor cantidad de investigadores hace un Parsing Semántico; en el que hacen mapeo por tipos, usan ontologías del dominio y bases de conocimiento externas con las que hacen mapeo, utilizan reglas semánticas y de concepto. Otro grupo de investigaciones emplea un Análisis semantico basado en modelos, se destacan el empleo de BART[], Decoder Autoregresivos[] y Agregaciones Conscientes de la Estructura(en los casos que usan representaciones de grafos). Con menos casos, también hay vertientes que parsean a un Lenguaje Intermedio, como es el caso de GraphQIR[].

Por último, en la etapa de Construcción de la consulta hay 3 principales vías por las que se decanta la bibliografía: La primera es en la que la construcción se realiza "Manual", se le da formato a las partes identificadas como *where*, *select*, *cláusulas*, se mapean los atributos que coincidan con tablas, etc. Otra vía de investigación es centrada en modelos nuevamente, se utilizan Decoders, en menor medida Redes Neuronales Recurrentes(RNN) y, en mayor, Transformers(RAT y Multiheaded Attention). Por último se usa también en la construccion de la consulta formalizada, un Compilador, especialmente en los casos en los que se usó con anterioridad un lenguaje intermedio entre el natural y el formal de consulta, esta última ha probado ser una de las más fructíferas vias de ataque en esta etapa junto a los Transformers.
