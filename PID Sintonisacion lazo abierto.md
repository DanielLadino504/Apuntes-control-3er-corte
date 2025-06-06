# Sintonizacion lazo abierto
## controladores PID
Sus origenes datan desde 1922 donde Nicolas Minorski realizo el analisis de este tipo de controladores y describe el funcionamiento de estos
Tambien en 1936 la idea de un controlador de tres terminos de proposito general con una accion de control  variable no fue introducido hasta finales de la decada de 1930.
## Acciones de control
Para poder entender estos controladores debemos tener en cuenta las siguientes caracteristicas:
-La accion proporcioanl es util en los problemas en las que unicamnete se quiere modificar uno de los parametros del sistema.
-Si se requiere manejar mas de un objetivo es adecuado utilizar varias acciones de control.
-Un control normalmente no tiene accion proporcional, en vez de esto tiene una funcion de transferencia que contiene las acciones de control.
Las acciones de control que existen son:
## Proporcional
Esta seccion multiplica el error por una constante proporcional. Esto para que el sistema tenga una tasa de crecimiento alta.
La formula de esta accion es:

$$u(t)=k_{p}\cdot e(t)$$

$$U(s)=k_{p}\cdot E(s)$$

## Integral
Esta seccion se encarga de calcular el error acumulado. Esto para que cuando el error tienda a cero el incremento en la accion de control se vaya haciendo mas pequeña hasta acercarse a cero.
Su formula es:

$$u(t)=k_{i}\int e(t)dt$$

$$U(s)=k_{i}\cdot \frac{E(s)}{s}$$

## Derivativa
Esta seccion se opone a las variaciones que pueda llegar a presentar el error.
Su formula es:

$$u(t)=k_{d}\cdot \frac{de(t)}{dt}$$

$$u(t)=k_{d}\cdot sE(s)$$

## Arquitecturas PID
Se refiere a las diferentes formas de convinar estas tres acciones. Aqui se presentaran las tres mas usadas:

* Arquitectura Paralela:



Esta estructura se realizan los siguientes calculos::

$$u(t)=k_{p}\cdot e(t)+k_{i}\int e(t)dt+k_{d}\cdot \frac{de(t)}{dt}$$

$$U(s)=k_{p}\cdot E(s)+k_{i}\cdot \frac{E(s)}{s}+k_{d}\cdot sE(s)$$

* Arquitectura ideal:



Esta arquitectura se calcula de la siguiente manera:

$$u(t)=k_{p}(e(t)+\frac{1}{t_{i}}\int e(t)dt+T_{d}\cdot \frac{de(t)}{dt})$$

$$U(s)=k_{p}(E(s)+\frac{1}{t_{i}}\cdot \frac{E(s)}{s}+T_{d}\cdot sE(s))$$

* Arquitectura Serie:



Para esta estructura se realizan los siguientes calculos:

$$u(t)=\frac{1}{T_{i}}\int ((e(t)+T_{d}\frac{\mathrm{d} e(t)}{\mathrm{d} t})k_{p})dt$$

$$U(s)=((E(s)(1+T_{d}s))k_{p})(1+\frac{1}{T_{i}s})$$

## Sintonizacion por prueba y error
Esta metodologia es de las mas usadas y se basa en seguir los siguientes pasos:
1. ajustar a 0 las ganancias integral y derivativa
2. Aumentar la ganancia proporcional hasta conseguir el tiempo de establecimiento deseado.
3. Se aumenta la ganancia integral hasta conseguir el sobre impulso deseado.
4. Aumenta la ganancia derivativa hasta reducir las oscilaciones hasta tener el comportamiento deseado.
5. Se ajusta la ganancia proporcional una vez mas
6. se hace el ajuste fino de las otras ganancias
# Criterios de desempeño para diseño de controladores PID
## Funciones de costo mas utilizados
IE=Integral de error=$\int e(t)dt$

ISE=Integral de error al cuadrado=$\int e(t)^{2}dt$

IAE=Integral del error al absoluto=$\int \left\|e(t) \right\|dt$

ITAE=Integral del error al absoluto por el tiempo=$\int \left\|e(t)\cdot t \right\|dt$

# Metodos de sintonizacion de lazo abierto
## Ziegler y Nichols



| Tipo de controlador | \( $K_{p}$ \)                       | \( $T_{i}$ \)     | \( $T_{d}$ \)     |
|---------------------|----------------------------------|---------------|---------------|
| P                   | \( $\frac{\tau}{t_{o} K}$ \)        | ————————       | ————————       |
| PI                  | \( $\frac{0.9 \tau}{t_{o} K}$ \)    | \( 3.3 $t_{o}$ \) | ————————       |
| PID                 | \( $\frac{1.2 \tau}{t_{o} K}$ \)    | \( 2 $t_{o}$ \)   | \( 0.5 $t_{o}$ \) |

## Metodo Cohen-Coon
Otro de los metodos que existen es el metodo cohen-coon el cual contiene las siguientes formulas:

| Tipo de controlador | \( $K_{p}$ \)                                                                 | \( $T_{i}$ \)                                                               | \($T_{d}$ \)     |
|---------------------|---------------------------------------------------------------------------|-------------------------------------------------------------------------|---------------|
| P                   | \( $\frac{1}{K} \cdot \frac{\tau}{t_{o}} \cdot \left( \frac{3\tau + t_{o}}{3\tau} \right)$ \) | ———————————————————                                                        | ———————————————————  |
| PI                  | \( $\frac{1}{K} \cdot \frac{\tau}{t_{o}} \cdot \left( \frac{10.8\tau + t_{o}}{12\tau} \right) $\) | \( $\frac{30 + 3\left(\frac{t_{o}}{\tau}\right)}{9 + 20\left(\frac{t_{o}}{\tau}\right)} \cdot t_{o} $\) | ———————————————————  |
| PID                 | \( $\frac{1}{K} \cdot \frac{\tau}{t_{o}} \cdot \left( \frac{16\tau + 3t_{o}}{12\tau} \right)$ \) | \( $2t_{o}$ \)                                                              | \( $0.5t_{o}$ \)  |
Para este metodo sew tienen los siguientes criterios de desempeño:
1. Decaimiento de 1/4
2. Minimo sobre impulso posible
3. minima area bajo la curva
# Metodo por coeficiente de ajustabilidad
Este metodo tiene las siguientes formulas para hayar sus parametros:

| \( $\gamma$ \)        | \( $K_{p}$ \)                                             | \( $T_{i}$ \)            | \( $T_{d}$ \)                                      |
|---------------------|--------------------------------------------------------|----------------------|------------------------------------------------|
| 0 a 0.1             | \( $\frac{5}{K}$ \)                                      | \( $\tau$ \)           | ———————————————————                                 |
| 0.1 a 0.2           | \( $\frac{0.5}{K \gamma}$ \)                             | \( $\tau$ \)           | ———————————————————                                 |
| 0.2 a 0.5           | \( $\frac{0.5(1 + 0.5 \gamma)}{K \gamma}$ \)             | \( $\tau(1 + 0.5 \gamma)$ \) | \($\tau \cdot \frac{0.5 \gamma}{0.5 \gamma + 1}$ \) |

Para este metodo hay los siguientes criterios de desempeño:
1. minimo ITAE
2. Minimo error con respecto aal factor de amortiguamiento.
