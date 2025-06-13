# Controladores PID Lazo Cerrado

Este informe se centra en el diseño de controladores Proporcional-Integral-Derivativo (PID) en lazo cerrado, una técnica fundamental en sistemas de control. Se abordarán los métodos de sintonización, prestando especial atención al método de Ziegler-Nichols y su mejora, el método del Relé. Además, se explorará el fenómeno del "wind-up" en la acción integral y diversas estrategias "anti-wind-up" para mitigar sus efectos negativos, asegurando un control óptimo y estable en aplicaciones industriales.

## 1. Introducción

El diseño de controladores es un pilar fundamental en la ingeniería de control, permitiendo la automatización de procesos y garantizando su operación eficiente y correcta. Dentro de este campo, los controladores Proporcional-Integral-Derivativo (PID) son ampliamente adoptados por su simplicidad y efectividad en la mejora del comportamiento de los sistemas. Los sistemas de control pueden operar de dos maneras principales: con retroalimentación (lazo cerrado) y sin retroalimentación (lazo abierto).

Este informe se centra en el diseño de controladores PID en lazo cerrado, donde la acción del controlador se ajusta continuamente basándose en la señal de error, que es la diferencia entre el valor deseado (referencia) y el valor medido de la salida del sistema. La sintonización de PID en lazo cerrado implica considerar la dinámica completa del sistema, lo que a menudo requiere pruebas en tiempo real o el uso de modelos precisos para determinar los parámetros óptimos del controlador. En la clase, se explicaron los fundamentos del PID, sus componentes individuales y cómo cada uno afecta el rendimiento del sistema en términos de tiempo de respuesta, oscilaciones, estabilidad y error en estado estacionario. También se abordaron las ventajas y desventajas de los métodos de sintonización en lazo cerrado. A continuación, se detallarán los temas trabajados en la clase, incluyendo los métodos de sintonización de lazo cerrado, el fenómeno del "wind-up" en la acción integral y las estrategias para mitigar sus efectos.

## 2. Definiciones

### 2.1. Lazo Cerrado

> 🔑 *Lazo cerrado:* La idea principal de los métodos de lazo cerrado es identificar los parámetros característicos de la dinámica del sistema realizando pruebas en lazo cerrado. A partir de estas pruebas, se procede al diseño del controlador PID.

## 3. Subsecciones

### 3.1. Método de Ziegler & Nichols

El método de Ziegler & Nichols es una técnica de sintonización de controladores PID que se basa en la respuesta del sistema en lazo cerrado. Este método es ampliamente utilizado en la industria a pesar de su antigüedad.

#### 3.1.1. Método Ziegler & Nichols (Ciclo Último)

La metodología del ciclo último del método de Ziegler & Nichols implica los siguientes pasos:
1.  Llevar las ganancias PID a cero (0).
2.  Variar (aumentar) la ganancia proporcional ($K_p$) hasta que la salida del sistema se comporte marginalmente estable.
3.  Medir el período de la oscilación obtenida, al cual se le llamará período último ($P_u$).
4.  La ganancia a la cual se logró el estado marginalmente estable se denomina ganancia última ($K_u$).

💡**Ejemplo 1:** Solución analítica para un sistema dado por la función de transferencia $G=\frac{1}{s^{3}+6s^{2}+11s+6}$.

La prueba se realiza en lazo cerrado, por lo tanto, la función de transferencia de lazo cerrado es:

$$G_{o}=\frac{K_{p}*\frac{1}{s^{3}+6s^{2}+11s+6}}{1+k_{p}*\frac{1}{s^{3}+6s^{2}+11s+6}}=\frac{K_{p}}{s^{3}+6s^{2}+11s+6+K_{p}}$$

Para obtener las características de la respuesta marginalmente estable, se busca cuando la parte real e imaginaria de la función de transferencia son cero (0). Se sustituye $s=j\omega$:

$$G_{o}=\frac{K_{p}}{(j\omega)^{3}+6(j\omega)^{2}+11(j\omega)+6+K_{p}}$$

Separando la parte real e imaginaria en el denominador:

$$(j\omega)^{3}+6(j\omega)^{2}+11(j\omega)+6+K_{p}$$ 
$$j\omega((j\omega)^{2}+11)+(6(j\omega)^{2}+6+K_{p})$$

Para hallar la frecuencia de oscilación, se iguala a cero la parte imaginaria:

$$j\omega(-(\omega)^{2}+11)+(-6(\omega)^{2}+6+K_{p})$$ 
$$-(\omega)^{2}+11=0$$ 
$$\omega=\sqrt{11}$$

El período último ($P_u$) se calcula como:

$$Pu=\frac{2\pi}{\sqrt{11}}=1,894~segundos$$

Para obtener la ganancia que produce el estado marginalmente estable ($K_u$), se iguala a cero la parte real:

$$-6\omega^{2}+6+K_{p}=0$$ 
$$K_{p}=6\omega^{2}-6$$ 
$$K_{p}=6(11)-6=60=K_{u}$$

A continuación, se presenta una tabla con las ecuaciones de diseño para diferentes tipos de controladores PID, utilizando los valores de $K_u$ y $P_u$ obtenidos:

| Tipo | $K_p$ | $T_i$ | $T_d$ |
|---|---|---|---|
| P | $0.5 K_u$ | - | - |
| PI | $0.45 K_u$ | $\frac{P_u}{1.2}$ | - |
| PID | $0.6 K_u$ | $\frac{P_u}{2}$ | $\frac{P_u}{8}$ |

Tabla 1. Aplicación de las ecuaciones de diseño de Ziegler & Nichols.

### 3.2. Método del Relé

El método del Relé es una mejora al método de lazo cerrado de Ziegler & Nichols. Este método ofrece varias ventajas:
* Evita tener que subir excesivamente la señal de control para lograr la oscilación sostenida.
* Permite manipular la respuesta obtenida durante la prueba, a diferencia del método de Ziegler & Nichols.

La metodología del método del Relé incluye los siguientes pasos:
* Medir el tiempo necesario para obtener el estado estacionario en el sistema en lazo abierto.
* Cerrar el lazo utilizando un Relé con histéresis como controlador.
* Capturar la salida obtenida (oscilación sostenida).
* Medir el período último ($P_u$) y la amplitud del ciclo último ($A_u$).
* Determinar la ganancia crítica ($K_c$).
* Determinar la ganancia estática.

La ganancia crítica $K_c$ se puede determinar utilizando la siguiente ecuación, donde $d$ es la amplitud de la señal de salida del relé y $A_u$ es la amplitud de la oscilación obtenida:

$$K_c = \frac{4d}{\pi \sqrt{A_u^2 - \epsilon^2}}$$

Es importante que exista un desfase de $180^\circ$.

La sintonización PI utilizando el método del Relé a menudo emplea ecuaciones basadas en las de Ziegler & Nichols para un máximo sobreimpulso menor al 10%. Las ecuaciones para el controlador PI son:

| Controlador | $K_p$ | $T_i$ |
|---|---|---|
| PI | $\frac{5K_c}{6}$ | $\frac{P_u}{5} \frac{1+12K_c}{1+14K_c}$ |

Tabla 2. Sintonización PI con el método del Relé.

### 3.3. Fenómeno Wind-up

El fenómeno "wind-up" es un problema común en los controladores PID, particularmente con la acción integral. Ocurre cuando la señal de control integral se acumula más allá de los límites físicos del actuador.

Cuando la acción integral sigue acumulándose, los tiempos de respuesta del sistema se ven afectados negativamente. Aunque esto protege al actuador, la acumulación integral puede causar un gran sobreimpulso cuando la señal de control vuelve a estar dentro de los límites del actuador.

#### 3.3.1. Estrategias Anti Wind-up

Para mitigar el fenómeno del "wind-up", se han desarrollado diversas estrategias "anti wind-up".

##### 3.3.1.1. Anti Wind-up por Saturación de la Acción Integral

Esta estrategia implica limitar directamente la acumulación de la acción integral. Aunque protege al actuador, la acción integral puede seguir acumulando valores fuera de rango, afectando los tiempos de respuesta del sistema.

##### 3.3.1.2. Anti Wind-up por Integración Condicional

En esta estrategia, la integración de la acción integral se suspende cuando el actuador alcanza sus límites de saturación. Esta técnica ayuda a solucionar el problema del tiempo de respuesta del sistema, pero puede producir algunos pulsos no deseados en la respuesta del sistema debido a la suspensión abrupta de la acción integral.

##### 3.3.1.3. Anti Wind-up por Recálculo y Seguimiento

Esta estrategia hace una estimación del modelo del actuador para suspender la acción integral de manera suave cuando se acerca a los niveles de saturación del actuador. El cálculo de la ganancia $T_t$ definirá la velocidad de respuesta del "anti wind-up", donde $T_t = T_i T_d$. Este método busca evitar los pulsos no deseados de la integración condicional, proporcionando una transición más suave.

## 4. Ejercicios

📚**Ejercicio 1:** Dado un sistema con función de transferencia $G=\frac{1}{s^{3}+10s^{2}+27s+18}$, determine la ganancia última $K_u$ y el período último $P_u$ utilizando el método de Ziegler & Nichols (ciclo último).

**Solución:**

La función de transferencia en lazo cerrado es:
$$G_{o}=\frac{K_{p}}{s^{3}+10s^{2}+27s+18+K_{p}}$$

Sustituimos $s=j\omega$:
$$G_{o}=\frac{K_{p}}{(j\omega)^{3}+10(j\omega)^{2}+27(j\omega)+18+K_{p}}$$Separamos la parte real e imaginaria del denominador:$$j\omega((j\omega)^{2}+27)+(10(j\omega)^{2}+18+K_{p})$$
$$j\omega(-\omega^{2}+27)+(-10\omega^{2}+18+K_{p})$$

Igualamos la parte imaginaria a cero para hallar $\omega$:
$$-\omega^{2}+27=0$$$$\omega^{2}=27$$$$\omega=\sqrt{27} \approx 5.196 \text{ rad/s}$$

Calculamos el período último $P_u$:
$$P_u = \frac{2\pi}{\omega} = \frac{2\pi}{\sqrt{27}} \approx \frac{6.283}{5.196} \approx 1.209 \text{ segundos}$$

Igualamos la parte real a cero para obtener $K_u$:
$$-10\omega^{2}+18+K_{p}=0$$$$K_{p}=10\omega^{2}-18$$$$K_{u}=10(27)-18 = 270-18 = 252$$

Por lo tanto, $K_u = 252$ y $P_u \approx 1.209$ segundos.

📚**Ejercicio 2:** Utilizando los valores de $K_u=252$ y $P_u=1.209$ segundos obtenidos en el Ejercicio 1, calcule los parámetros $K_p$, $T_i$ y $T_d$ para un controlador PID según las ecuaciones de diseño de Ziegler & Nichols.

**Solución:**

Para un controlador PID, las ecuaciones de diseño de Ziegler & Nichols son:

$K_p = 0.6 K_u$
$T_i = \frac{P_u}{2}$
$T_d = \frac{P_u}{8}$

Sustituyendo los valores:

$K_p = 0.6 \times 252 = 151.2$
$T_i = \frac{1.209}{2} = 0.6045 \text{ segundos}$
$T_d = \frac{1.209}{8} = 0.151125 \text{ segundos}$

Por lo tanto, los parámetros para el controlador PID son $K_p = 151.2$, $T_i = 0.6045$ segundos y $T_d = 0.151125$ segundos.

## 5. Conclusiones

Las pruebas en lazo cerrado ofrecen un enfoque valioso y aplicable en la industria para el diseño de controladores. A pesar de su antigüedad, el método de Ziegler & Nichols de lazo cerrado sigue siendo ampliamente utilizado. Además, muchos autores han propuesto métodos de sintonización que se basan en los parámetros obtenidos del método del Relé. Es crucial tener en cuenta el fenómeno del "wind-up" en las implementaciones reales e incorporar alguna estrategia "anti wind-up" para asegurar un control efectivo y estable del sistema.

## 6. Referencias

