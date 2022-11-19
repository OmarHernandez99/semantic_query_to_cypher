
\item Realizar Experimentos para evaluar la viabilidad de la arquitectura propuesta usando los datos generados por el Módulo de Aumentación de Datos.


\section{Marco Experimental}
Se utiliza el servicio de computaci\'on en la nube que ofrece la p\'agina de kaggle[] para entrenar el modelo de aprendisaje de m\'aquina.\\

Con una configuraci\'on de especificaciones que provee Kaggle gratis de 12 GB de Ram, y de video una NVIDIA Tesla P100 PCI de 16GB GPUS.\\

\section{Resultados}
Con el objetivo de probar la viabilidad de la arquitectura propuesta en \nameref{obj-gen} se realizan experimentos tomando como base el prototipo implementado.

En el marco de esta investigaci\'on, se experimentar\'a con el comportamiento de la calidad de la respuesta dada por el modelo al probarlo en un dataset sint\'etico. Ya que la principal fuente de variabilidad en los resultados es precisamente este, porque el resto de la arquitectura es determinista.\\

La experimentaci\'on consiste en 

- consisten en coger el output del modelo y pasarlo por un dataset sintetico y ver en que porciento se parece al resultado deseado

- se medira en cuanto a las veces que detecta correctos la entidad, los atributos, los filtros, agregaciones, y los limites sobre los que no.

- se entrenara el modelo con cantidades variables de data, 

- por x generaciones, epocas

- se mediran los distintos resultados

- se quiere que los errores den menos del x porciento

- dio estos porcientos

Discusiones
- se cometieron estos errores ...
- se valido la hipotesis de que era posible 
