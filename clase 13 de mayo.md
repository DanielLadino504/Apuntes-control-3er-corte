# Identificacion de sistemas por curva de reaccion
Para poder sintonizar cualquier controlador se debe tener en cuenta los siguientes metodos:

-Modelo matematico

-Curvas de reaccion en lazo abierto

-Identificacion de parametros en lazo abierto

-Analisis de frecuencia

-Tecnicas de inteligencia computacional

## Procedimiento de sintonizacion 
Para diseñar cualquier controlador se requiere conocer el comportamiento de la planta, esto para escoger los parametros del sistema y para esto hay metodologias que permiten obtener un modelo aproximado.
## Metodologias de tipo empirico
Estas metodologias permiten obtener modelos de comportamiento a partir de pruebas realizadas sobre la planta involucrando manejo de datos, estadistica y señales.
## Metodologias de identificacion de lazo abierto
Este metodo permite obtener un modelo de primer orden y adicionalmente el tiempo muerto. Su formula caracteristica es:

$$ G(s)=\frac{ke^{-st_{0}}}{\tau s+1}$$

Donde $\tau$ es contante de tiempo del sistema, k es ganancia estatica del sistema y t_{0} es tiempo muerto.

## Aplicacion curvas de reaccion en lazo abierto
Para utilizar este metodo se debe tener en cuenta las siguientes condiciones.
El sistema debe ser estable en lazo abierto, Tambien debe tener un comportamiento sobre o criticamente amortiguado.
Este procedimiento consiste en la aplicacion de una señal de tipo escalon y capturar los datos obtenidos.
## Metodo de Ziegler y Nichols
