# El Sistema
El sistema contiene varios modulos:

### Modulos
1 - Graph-Contractor
2 - Pre-Processor
3 - Post-Processor
4 - Parser-to-AST
5 - Query-Maker
6 - Lenguaje-Intermedio
7 - Data-Generation

Primero se configura la base de conocimiento a la que se quiere consultar
    - Para poder Interactuar e Intercambiar información
        - Con link y password

El módulo de `Graph-Contractor` interactua con ella extrae la informacion relevante
    - ya que con metadatos de la bd se puede hacer más preciso el reconocimiento de la intención de la consulta del usuario
    - también sirve para entrenar el modelo de aprendisaje texto a texto usado en el `Post-Processor`.

        - se computan las entidades de la bd
        - se computan las propiedades de las entidades
        - se buscan algunos valores de las propiedades
        
            - lo anterior mediante la librería py2neo y consultas en Cypher
            - se guardan las entidades y sus propiedades internamente, en listas, y los valores en diccionarios propiedad: [valores]

Luego se muestra en la UI información relevante del grafo. Sus entidades y sus propiedades, asi como algunos valores.
    - esto con el objetivo de para facilitar la elaboración de consultas correctas para el usuario
        - Esta información es provista por un `Mediator` que pide información al `Graph-Contractor`.


Luego el usuario hace una consulta a la bd en lenguaje natural mediante la UI
    - La UI fue hecha en streamlit
    - se comunica con el `Mediator` que se encarga de inicializar el sistema y construir el `Asker`.

        - Para construir el `Asker` se le pasan instancias que implementen los modulos 1-5.

            - Que son, en codigo: `Py2NeoOracle`, `SentenceNormalizer`, `ModelInterprete`, `OpherParser` y `ASTInterpreter` respectivamente.


    - se recibe la consulta, se corrigen palabras mal escritas y se le añaden etiquetas a palabras clave.

        - Lo que permite que el modelo se pueda enfocar solamente en el vocabulario alrededor de las palabras clave para percibir la intención
        - Esto ayuda sobre todo a que el sistema pueda generalizar mejor y que no sea necesario entrenar el modelo de aprendisaje para cada grafo de conocimiento distinto.

            - De esto se encarga el `SentenceNormalizer`, su tarea es recibir la consulta  en lenguaje natural y hacerla más facil para su procesamiento del sistema.
            - Se hace primero una correción de palabras mal escritas

                -  usando distancia Levenshtein 1 y el `Py2NeoOracle` para consultar las palabras importantes del grafo

            - Se reemplazan las palabras por sus respectivas etiquetas usando el `Py2NeoOracle`

                - se usan las etiquetas: "LABEL" y "ATTRIBUTE" en sustitución de las entitades y atributos que se mencionan en la consulta
                - se introduce un sufijo `_i` con el número correspondiente a la etiqueta.


    - se transpila a un lenguaje intermedio con un modelo de aprendisaje texto a texto, y se le remueven las etiquetas (Post-Processor)

        - se creó un lenguaje intermedio llamado *Opher*, con varios objetivos:

            -  que no fuera ambiguo, y por tanto fuera parseable.
            -  que fuera más sencillo que Cypher de aprender por un modelo de aprendisaje de máquina.
            -  que en su sencillez, abarcara la mayor parte de las posibles consultas que un usuario puede querer hacerle a su base de conocimiento

                - El lenguaje tiene esta arquitectura ...

        - se utiliza un modelo texto a texto porque la entrada del problema, la consulta en lenguaje natural, es texto, y la salida debia ser, bien un AST o bien un texto parseable en un AST.

            - se opto por que fuera texto 

                - porque un modelo no puede devolver una instancia directamente, debería entonces devolver una codificación de un AST
                    para lo cual tiene que saber resolver el problema de parentesis balanceados(que no son buenos), 
                - y la otra via era la de tener muchos modelos, uno para cada decision que tomar en el AST, que no se escogio pq iba a resultar en 
                    varias instancias de  modelos por cada nivel del AST que representaba al lenguaje intermedio, que iba a resultar en mucha dependencia entre el lenguaje y los modelos de aprendisaje
                - además se cree que tiene mas versatilidad el sistema ante futuros cambios de esta manera

            - el modelo que se usó fue un T5, implementado con la librería `simpleT5`, en su version mediana, `t5-base`.

            - la data que se usó para entrenar el modelo se extrajo del módulo `Data-Generation`.

                - se vio que era posible atacar el problema de la data entrenante resolviendo el problema inverso

                    - es decir teniendo ejemplos en lenguaje intermedio Opher, generar consultas en lenguaje natural que reflejaran la misma intención.
                    - esto se hacia con un humano o creando un sistema especifico para la tarea, la via del humano era inviable. 

                - se crean 3 submódulos encargados de producir muchos ejemplos de pares de consultas sintéticas con su correspondiente representación en Opher.

                    - es necesario uno para que cree instancias aleatorias de AST, `AST-Generator`

                        - encargado de generar AST sintéticos basados en la instancia de base de conocimiento escogida en particular. 
                        - Para eso se apoya  en el GraphContractor para saber que nodos y atributos existen en el grafo de conocimiento.

                            - a nivel de implementacion, es un patron Visitor

                        - contiene un `Ast-Context` que lleva la traza de las variables que ha creado, su tipo y propiedades que posee. 
                            - esto para que las propiedades y valores que se generan en los filtros tengan concordancia con la etiqueta ya elegida.
                                - a nivel de implementación es un diccionario: nombre de variable: etiqueta

                    
                    - otro para que cree las consultas en texto equivalentes al AST, ya sea lenguaje natural o lenguaje intermedio

                        - el encargado de esto es el `Query-Generator`, y su objetivo es es capaz de generar representaciones en texto de las instancias de ast.
                            Esto permite generar montones de data entrenante.

                            - cambiando el glosario de términos de referencia provistos por el `Word-Module`, es capaz de generar tanto las consultas en lenguaje natural como en lenguaje intermedio
                            - A partir de una instancia de AST, genera variaciones que expresan la misma idea con diferentes palabras
                            - sería capaz de generar consultas en distintos idiomas con la misma facilidad.

                                - A nivel de implementacion es un Visitor tambien

                    - el 3er modulo carga con el conocimiento linguistico, sinónimos, y frases equivalentes de los nodos del AST de Opher

                        - este papel lo hace el `Word-Module`, y sirve de nexo entre los modulos creadores de consultas y el lenguaje en cual se genera esta
                        - lo que permite que cambiar de lenguaje sea solamente añadir un nuevo glosario de palabras y frases sin tener que modificar el resto del sistema

                            - a nivel de implementación es un grupo de diccionarios


                - luego se le reemplazan las palabras claves por etiquetas

                    - para que el entrenamiento sea lo mas semejante a la data que se le va a pasar, sin importar el dominio de la bd

                        - con regex

                - y se le introduce ruido en el sufijo de las etiquetas `_i` 

                    - ya que se observó durante los experimentos que el modelo estaba mapeando por precedencia de los numeros (Attribute_1 lo ponia primero que Attribute_2), y era algo que no se deseaba que aprendiera.

                        - se reemplaza por un # random en [0, 15]

        - se remueven las etiquetas y se recuperan las palabras originales por las que se sustituyeron

            -  para que el siguiente modulo construya, del lenguaje intermedio, el AST con valores en los nodos hojas iguales a los de la bd especifica

                - con ayuda del `Word-Module`


    - se parsea este lenguaje intermedio a una instancia de AST creado especificamente para este lenguaje(modulo Parser)

        - para tener la consulta de forma estructurada y poder hacerle transformaciones y chequeos de validacion

            - se hicieron 2 implementaciones de este modulo

                - una con una pila, que iba sacando de la pila y construyendo el nodo que tocara en el AST

                    - no era resiliente a errores de sintaxis y no soportaba cambios en la estructura del lenguaje con relativo cambio en su implementacion
                    - se desecho

                - se implemento una version recursiva que era mas facil de mantener y mas robusta a cambios en la estructura del lenguaje

                    - funciona semejante a un visitor, donde se busca en un rango especifico de la consulta por la palabra clave del nodo actual, y se manda a buscar los hijos de ese nodo en un rango mas restringido
                    - cada nodo tiene un metodo que se encarga de crearlo, por lo que es facil de extender si cambia la estructura del lenguaje

                - ademas se hace un chequeo de tipos para diferenciar las cadenas de texto de otros tipos de datos


    - a partir de la instancia de AST, se crea una consulta en lenguaje de consulta formal

        - esto para que la pueda procesar la bd, ya que lo que es capaz de entender es cypher, en este caso
        - ademas se pudiera cambiar a lenguaje SQL u otro con relativa facilidad ya que se parte de un AST que representa la intencion de la query

            - el papel de Query-Maker lo implementa la clase `ASTInterpreter`
            - es un Visitor que recorre el AST y va generando el pedazo de consulta correspondiente a cada nodo y se llama en los nodos hijos para que generen su parte
            - se le hace un chequeo de sintaxis para ver si la consulta es valida


    - se hace el pedido a la bd 
        - con el fin de obtener la respuesta a la consulta hecho por el usuario
            - la clase `Py2NeoOracle` es la encargada, a traves de la libreria py2neo.

    - la respuesta de cada modulo a lo largo del proceso queda logueada
        - para llevar record del proceso

    - se devuelve al usuario la respuesta dada.
        - con el fin de obtener los resultados
            - se pasa la informacion devuelta por la base de conocimiento directo por el `Mediator` hasta la UI


    - Y se devuelve tanto la respuesta dada por la base de conocimiento a la consulta, como los logs.

