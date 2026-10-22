# MSFP2
Práctica 2: Modelado y control del sistema cardiovascular mediante el modelo Windkessel
[![Open in MATLAB Online](https://www.mathworks.com/images/responsive/global/open-in-matlab-online.svg)](https://matlab.mathworks.com/open/github/v1?repo=Nefga/MSFP2)

# Práctica 2: Modelado y control del sistema cardiovascular mediante el modelo Windkessel

## Información del estudiante

Neltzon Farid García Altamirano 23212194 l23212194@tijuana.tecnm.mx

Modelado de Sistemas Fisiológicos

Ingeniería Biomédica

## Docente

Dr. Paul Antonio Valle Trujillo; paul.valle@tectijuana.edu.mx

Departamento de Ingeniería Eléctrica y Electrónica, Tecnológico Nacional de México/IT Tijuana, Blvd. Alberto Limón Padilla s/n, Tijuana, C.P. 22454, B.C., México.

## Descripción de la asignatura

El modelizado de sistemas fisiológicos es una herramienta importante en Ingeniería Biomédica, permite comprender el funcionamiento del cuerpo humano, así como diseñar y evaluar terapias y dispositivos médicos; se define como el proceso de formular modelos matemáticos o computacionales que representan el comportamiento y la interacción de los sistemas biológicos y fisiológicos. Esta asignatura pretende aportar al perfil del Ingeniero Biomédico la capacidad de realizar investigación científica en el área de Biología de Sistemas con la finalidad de dirigir y participar en equipos de trabajo interdisciplinarios en contextos nacionales e internacionales, así como de proporcionar soluciones informáticas para resolver problemas en el campo de la Ingeniería Biomédica con ética profesional; lo anterior al proporcionar al estudiante bases sólidas para modelizar sistemas y diseñar controladores para la solución de problemas en las áreas de atención médica y del sector industrial médico. La construcción de analogías entre circuitos eléctricos y sistemas fisiológicos para la formulación de modelos matemáticos y el diseño de controladores mediante la experimentación in silico brindan herramientas de gran aplicación en el quehacer profesional del Ingeniero Biomédico.

La asignatura de Modelado de Sistemas Fisiológicos forma parte del plan de estudios de la carrera en Ingeniería Biomédica con la siguiente competencia general del curso: Utiliza las propiedades de los circuitos RLC para describir la dinámica de sistemas fisiológicos, obtener modelos matemáticos y aplicar el control clásico, esto con el objetivo de integrar los principios de la Ingeniería de Control, la Electrónica Analógica y las Ciencias de la Computación con la Anatomía y Fisiología del cuerpo humano para proporcionar descripciones cuantitativas y cualitativas de sistemas fisiológicos complejos con el objetivo de modelizar, analizar, controlar, ilustrar y predecir su dinámica tanto en el corto como en el largo plazo.

## Competencia a desarrollar

Modeliza sistemas fisiológicos mediante circuitos eléctricos equivalentes, ecuaciones diferenciales y funciones de transferencia para analizar su comportamiento dinámico y diseña estrategias de control mediante técnicas de control clásico. En particular, durante la práctica se analiza el modelo Windkessel de cuatro elementos, se compara la dinámica cardiovascular de un individuo normotenso (control) con dos casos simulados, hipotenso e hipertenso, y se diseñan estrategias de control *in silico* orientadas a reducir las diferencias entre las respuestas de los casos y la respuesta de referencia.

## Objetivos

1. Identificar las variables y los parámetros del modelo Windkessel y relacionarlos con propiedades hemodinámicas.
2. Determinar el modelo de ecuaciones diferenciales ordinarias y su representación en variables de estado.
3. Calcular las funciones de transferencia $G_{hipo}(s)$, $G_{normo}(s)$ y $G_{hiper}(s)$.
4. Determinar el error en estado estacionario de cada modelo ante una entrada escalón.
5. Analizar la estabilidad de los tres modelos a partir de la ubicación de sus polos.
6. Emular el modelo en MATLAB/Simulink mediante ecuaciones de estado, función de transferencia y circuito equivalente en Simscape Electrical.
7. Simular la respuesta en lazo abierto de los tres modelos ante una señal aleatoria uniforme con la misma realización.
8. Sintonizar con la herramienta Tune de Simulink un controlador PID para el caso hipotenso y otro para el hipertenso, utilizando la respuesta normotensa como referencia.
9. Simular la respuesta en lazo cerrado de los dos casos y comparar el seguimiento de la referencia.
10. Reproducir los resultados en VS Code/Python con las funciones de transferencia y verificar la consistencia entre plataformas.
11. Elaborar un diagrama biológico que relacione los elementos del modelo con las estructuras fisiológicas del sistema cardiovascular.

## Descripción detallada del sistema

Las características hemodinámicas del sistema arterial permiten obtener información relevante sobre su comportamiento dinámico. El modelo Windkessel representa estas propiedades mediante un circuito eléctrico. Su formulación original se atribuye a Otto Frank, quien propuso representar el sistema arterial con una sección distensible y una resistencia periférica concentrada (modelo de dos elementos, $R$ y $C$). Posteriormente se incorporó la impedancia característica $Z$ de la región proximal del lecho vascular y, finalmente, la inertancia $L$ asociada con la masa de la sangre en movimiento, dando lugar al modelo de cuatro elementos utilizado en esta práctica.

![Modelo Windkessel de cuatro elementos](Imagenes/windkessel_4_elementos.png)

*Modelo Windkessel de cuatro elementos para la representación del sistema cardiovascular.*

La analogía entre variables hemodinámicas y eléctricas es:

| Variable hemodinámica | Variable eléctrica |
|---|---|
| Presión | Voltaje |
| Flujo sanguíneo | Corriente |

Los elementos del modelo y su interpretación fisiológica son:

| Elemento | Propiedad hemodinámica | Analogía eléctrica |
|---|---|---|
| $Z$ | Impedancia característica de la aorta proximal | Resistencia |
| $C$ | Distensibilidad arterial total | Capacitancia |
| $R$ | Resistencia periférica total | Resistencia |
| $L$ | Inertancia arterial | Inductancia |

Variables del modelo: $P_a(t)$ presión arterial de entrada, $P_p(t)$ presión sobre la distensibilidad arterial (salida), $F_a(t)$ flujo dirigido hacia la aorta o arteria pulmonar y $F_L(t)$ flujo a través de la inertancia.

A pesar de tener cuatro elementos, el modelo contiene solamente dos elementos independientes de almacenamiento de energía ($C$ y $L$), por lo que corresponde a un sistema de **segundo orden** con dos variables de estado.

### Parámetros de los tres casos

El modelo normotenso se utiliza como referencia (control) y los modelos hipotenso e hipertenso como casos. Las diferencias se establecen modificando únicamente la distensibilidad arterial $C$.

| Parámetro | Hipotenso | Normotenso | Hipertenso | Unidades |
|---|---|---|---|---|
| $Z$ | 0.010 | 0.010 | 0.010 | mmHg·s·ml⁻¹ |
| $L$ | 0.001 | 0.001 | 0.001 | mmHg·s²·ml⁻¹ |
| $R$ | 1.000 | 1.000 | 1.000 | mmHg·s·ml⁻¹ |
| $C$ | 3.500 | 1.500 | 1.000 | ml·(mmHg)⁻¹ |

Un incremento de $C$ (caso hipotenso) corresponde a un sistema arterial con mayor capacidad para distenderse y almacenar volumen, lo que tiende a atenuar los incrementos de presión. Una disminución de $C$ (caso hipertenso) representa menor distensibilidad y mayor rigidez arterial, de modo que una variación de volumen produce cambios de presión más pronunciados.

### Señales de error

Para cada caso se define una señal de error independiente:

$$e_{hipo}(t) = P_{p,normo}(t) - P_{p,hipo}(t)$$

$$e_{hiper}(t) = P_{p,normo}(t) - P_{p,hiper}(t)$$

> **Aviso:** las estrategias de control desarrolladas en esta práctica tienen propósitos exclusivamente académicos y de simulación. No constituyen una estrategia terapéutica ni una recomendación para el tratamiento de alteraciones de la presión arterial.

Palabras clave: Modelo Windkessel; Sistema cardiovascular; Controlador PID; Función de transferencia; Simulaciones numéricas.

## Desarrollo matemático

### Modelo mediante ecuaciones diferenciales ordinarias

Se eligen como variables de estado $x_1(t) = F_L(t)$ y $x_2(t) = P_p(t)$, y como entrada $u(t) = P_a(t)$. Como $Z$ y $L$ están en paralelo entre los nodos de $P_a(t)$ y $P_p(t)$, ambos tienen la misma diferencia de presión.

Para la inertancia:

$$P_a(t) - P_p(t) = L\,\frac{dF_L(t)}{dt} \quad\Rightarrow\quad \frac{dF_L(t)}{dt} = -\frac{1}{L}P_p(t) + \frac{1}{L}P_a(t)$$

Flujo a través de la impedancia característica:

$$F_Z(t) = \frac{P_a(t) - P_p(t)}{Z}$$

Conservación del flujo en el nodo de $P_p(t)$:

$$F_Z(t) + F_L(t) = F_C(t) + F_R(t), \qquad F_C(t) = C\,\frac{dP_p(t)}{dt}, \qquad F_R(t) = \frac{P_p(t)}{R}$$

Sustituyendo:

$$\frac{dP_p(t)}{dt} = \frac{1}{C}F_L(t) - \left(\frac{1}{ZC} + \frac{1}{RC}\right)P_p(t) + \frac{1}{ZC}P_a(t)$$

### Representación en variables de estado

$$\dot{x}(t) = A\,x(t) + B\,u(t), \qquad y(t) = C_s\,x(t)$$

$$A = \begin{bmatrix} 0 & -\dfrac{1}{L} \\[2mm] \dfrac{1}{C} & -\left(\dfrac{1}{ZC} + \dfrac{1}{RC}\right) \end{bmatrix}, \quad B = \begin{bmatrix} \dfrac{1}{L} \\[2mm] \dfrac{1}{ZC} \end{bmatrix}, \quad C_s = \begin{bmatrix} 0 & 1 \end{bmatrix}$$

con $x(t) = [F_L(t),\ P_p(t)]^T$, $u(t) = P_a(t)$ y $y(t) = P_p(t)$.

Evaluando con los parámetros de cada caso:

| Caso | $A$ | $B$ |
|---|---|---|
| Hipotenso | $\begin{bmatrix} 0 & -1000 \\ 0.2857 & -28.857 \end{bmatrix}$ | $\begin{bmatrix} 1000 \\ 28.571 \end{bmatrix}$ |
| Normotenso | $\begin{bmatrix} 0 & -1000 \\ 0.6667 & -67.333 \end{bmatrix}$ | $\begin{bmatrix} 1000 \\ 66.667 \end{bmatrix}$ |
| Hipertenso | $\begin{bmatrix} 0 & -1000 \\ 1 & -101 \end{bmatrix}$ | $\begin{bmatrix} 1000 \\ 100 \end{bmatrix}$ |

### Modelo de ecuaciones integro-diferenciales

Partiendo de la conservación del flujo en el nodo de $P_p(t)$:

$$F_a(t) = F_Z(t) + F_L(t) = F_C(t) + F_R(t)$$

con

$$F_Z(t) = \frac{P_a(t) - P_p(t)}{Z}, \quad F_L(t) = \frac{1}{L}\int \left[P_a(t) - P_p(t)\right]dt, \quad F_C(t) = C\,\frac{dP_p(t)}{dt}, \quad F_R(t) = \frac{P_p(t)}{R}$$

Sustituyendo y agrupando los términos en $P_p(t)$ se obtiene el modelo integro-diferencial:

$$C\,\frac{dP_p(t)}{dt} + \left(\frac{1}{R} + \frac{1}{Z}\right)P_p(t) = \frac{P_a(t)}{Z} + \frac{1}{L}\int \left[P_a(t) - P_p(t)\right]dt$$

La única variable desconocida es la salida $P_p(t)$. Como aparece también dentro de una derivada y de una integral, no puede despejarse de forma algebraica explícita, por lo que el modelo se deja en esta forma.

### Función de transferencia

Aplicando la transformada de Laplace con condiciones iniciales nulas:

$$F_Z(s) + F_L(s) = F_C(s) + F_R(s)$$

$$F_Z(s) = \frac{P_a(s) - P_p(s)}{Z}, \quad F_L(s) = \frac{P_a(s) - P_p(s)}{Ls}, \quad F_C(s) = Cs\,P_p(s), \quad F_R(s) = \frac{P_p(s)}{R}$$

Sustituyendo:

$$\frac{P_a(s) - P_p(s)}{Z} + \frac{P_a(s) - P_p(s)}{Ls} = Cs\,P_p(s) + \frac{P_p(s)}{R}$$

Agrupando $P_a(s)$ y $P_p(s)$:

$$P_a(s)\left(\frac{1}{Z} + \frac{1}{Ls}\right) = P_p(s)\left(Cs + \frac{1}{R} + \frac{1}{Z} + \frac{1}{Ls}\right)$$

$$P_a(s)\,\frac{Ls + Z}{LZs} = P_p(s)\,\frac{CLRZ\,s^2 + L(Z+R)\,s + RZ}{RLZs}$$

Despejando:

$$G(s) = \frac{P_p(s)}{P_a(s)} = \frac{LR\,s + RZ}{CLRZ\,s^2 + L(Z+R)\,s + RZ}$$

Con $Z = 0.01$, $L = 0.001$ y $R = 1$ el numerador es $0.001\,s + 0.01$ y el coeficiente de $s$ en el denominador es $0.00101$ en los tres casos; solo cambia el coeficiente de $s^2$, que vale $CLRZ = C \times 10^{-5}$:

$$G_{hipo}(s) = \frac{0.001\,s + 0.01}{3.5\times10^{-5}\,s^2 + 0.00101\,s + 0.01} = \frac{0.1\,s + 1}{0.0035\,s^2 + 0.101\,s + 1}$$

$$G_{normo}(s) = \frac{0.001\,s + 0.01}{1.5\times10^{-5}\,s^2 + 0.00101\,s + 0.01} = \frac{0.1\,s + 1}{0.0015\,s^2 + 0.101\,s + 1}$$

$$G_{hiper}(s) = \frac{0.001\,s + 0.01}{1\times10^{-5}\,s^2 + 0.00101\,s + 0.01} = \frac{0.1\,s + 1}{0.001\,s^2 + 0.101\,s + 1}$$

(la segunda igualdad de cada línea resulta de dividir numerador y denominador entre $RZ = 0.01$).

Las tres funciones son de **segundo orden**, lo que coincide con los dos elementos independientes de almacenamiento de energía ($C$ y $L$). Todas tienen un cero en $s = -Z/L = -10\ \text{s}^{-1}$.

### Error en estado estacionario

Para una entrada escalón de amplitud $A_0$, por el teorema del valor final:

$$P_p(\infty) = \lim_{s\to 0} s\,G(s)\,\frac{A_0}{s} = G(0)\,A_0 = A_0$$

Como $G(0) = 1$ en los tres modelos, la presión final es igual a la de entrada y el error entre entrada y salida es cero en lazo abierto. Además, al ser idéntica la ganancia estática, el error en estado estacionario entre el caso y la referencia normotensa también es cero para entrada escalón:

| Modelo | $G(0)$ | $P_p(\infty)$ ante escalón $A_0$ | $e_{ss}$ respecto a la entrada |
|---|---|---|---|
| Hipotenso | 1 | $A_0$ | 0 |
| Normotenso | 1 | $A_0$ | 0 |
| Hipertenso | 1 | $A_0$ | 0 |

Las diferencias entre los casos son, por tanto, de carácter **transitorio**, no de valor final.

### Análisis de estabilidad

Con $a = CLRZ$, $b = L(Z+R)$ y $c = RZ$, los polos son las raíces del denominador:

$$\lambda_{1,2} = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}$$

El signo del discriminante $b^2 - 4ac$ determina el tipo de respuesta: negativo, polos complejos conjugados (subamortiguada); positivo, polos reales distintos (sobreamortiguada). Con los parámetros de la práctica $b^2 = 1.0201\times10^{-6}$ y $4ac = 4\times10^{-7}\,C$, por lo que el amortiguamiento crítico ocurriría en $C \approx 2.55$ ml/mmHg: el caso hipotenso ($C = 3.5$) queda por encima y los casos normotenso e hipertenso por debajo.

| Modelo | $b^2 - 4ac$ |
|---|---|
| Hipotenso | $-3.799\times10^{-7}$ |
| Normotenso | $4.201\times10^{-7}$ |
| Hipertenso | $6.201\times10^{-7}$ |

Polos obtenidos (coinciden con los valores propios de $A$):

| Modelo | $\omega_n$ (rad/s) | $\zeta$ | Polos | Tipo de respuesta |
|---|---|---|---|---|
| Hipotenso | 16.90 | 0.854 | $-14.43 \pm 8.81j$ | Subamortiguada |
| Normotenso | 25.82 | 1.304 | $-12.06,\ -55.27$ | Sobreamortiguada |
| Hipertenso | 31.62 | 1.597 | $-11.13,\ -89.87$ | Sobreamortiguada |

Los tres sistemas son **estables** (todos los polos en el semiplano izquierdo). Ningún caso es críticamente amortiguado: el hipertenso tiene dos polos reales distintos. Al disminuir $C$ aumenta $\omega_n$ y $\zeta$: el polo rápido se aleja del origen y la respuesta se vuelve más veloz. En el hipotenso los polos son complejos conjugados, por lo que aparece oscilación amortiguada.

![Mapa de polos y ceros](Imagenes/polos_ceros.png)

*Ubicación de polos y ceros de los tres modelos en el plano complejo.* `[COMPLETAR: generar y guardar la figura]`

### Respuesta teórica al escalón unitario

Calculada con `scipy.signal` a partir de las funciones de transferencia anteriores:

| Modelo | Sobreimpulso | Tiempo de pico (s) | Tiempo de asentamiento 2 % (s) | Valor final |
|---|---|---|---|---|
| Hipotenso | 16.1 % | 0.126 | 0.304 | 1 |
| Normotenso | 8.7 % | 0.072 | 0.214 | 1 |
| Hipertenso | 6.2 % | 0.054 | 0.167 | 1 |

El sobreimpulso en los casos sobreamortiguados se debe al cero en $s = -10$, que aporta un término derivativo a la respuesta.

## Metodología

### Señal de excitación

Se utilizó el bloque *Uniform Random Number* de Simulink con los parámetros:

| Parámetro | Valor |
|---|---|
| Mínimo | −0.2 |
| Máximo | 1 |
| Semilla (seed) | 106 |
| Tiempo de muestreo $T_s$ | 0.1 s |
| Intervalo de simulación | $t \in [0, 15]$ s |

Se emplea exactamente la misma realización de la señal para los tres modelos, de forma que las diferencias entre sus respuestas sean consecuencia exclusiva de sus parámetros. La señal se exportó a `signals.xlsx` para reproducirla en Python.

### Esquema de control

Para cada caso se construyó un sistema de retroalimentación independiente. La respuesta del modelo normotenso es la referencia; la diferencia entre esta y la salida del caso alimenta al controlador PID, cuya salida es la entrada del caso:

![Esquema de control](Imagenes/esquema_control.png)

*Esquema general de control para reducir la diferencia entre la respuesta de un caso simulado y la del individuo normotenso.*

Con un controlador $G_c(s) = k_P + \dfrac{k_I}{s} + k_D\,s$ y la planta del caso $G_{caso}(s)$, la función de transferencia de lazo cerrado respecto a la referencia es:

$$T(s) = \frac{P_{p,caso,PID}(s)}{P_{p,normo}(s)} = \frac{G_c(s)\,G_{caso}(s)}{1 + G_c(s)\,G_{caso}(s)}$$

Con acción integral, el error en estado estacionario ante una referencia escalón es cero.

### Sintonización

Los controladores se sintonizaron de forma independiente con la herramienta **Tune** de Simulink, buscando una respuesta estable y una reducción del error entre el caso y la respuesta normotensa.

| Controlador | $k_P$ | $k_I$ | $k_D$ | Coeficiente de filtro $N$ |
|---|---|---|---|---|
| PID hipotenso | `[COMPLETAR]` | `[COMPLETAR]` | `[COMPLETAR]` | `[COMPLETAR]` |
| PID hipertenso | `[COMPLETAR]` | `[COMPLETAR]` | `[COMPLETAR]` | `[COMPLETAR]` |

![Parámetros del controlador](Imagenes/parametros_controlador.png)

*Captura de la herramienta Tune con la configuración utilizada.* `[COMPLETAR]`

## Resultados

### Respuesta en lazo abierto

![Lazo abierto](Imagenes/lazo_abierto.png)

*Comparación de $P_{p,hipo}(t)$, $P_{p,normo}(t)$ y $P_{p,hiper}(t)$ ante la misma señal aleatoria.* `[COMPLETAR]`

`[COMPLETAR: descripción breve de lo observado, por ejemplo amplitud de las variaciones y rapidez de seguimiento de la entrada en cada caso]`

### Respuesta en lazo cerrado

**Caso hipotenso:** comparación de $P_{p,normo}(t)$, $P_{p,hipo}(t)$ y $P_{p,hipo,PID}(t)$.

![Lazo cerrado hipotenso](Imagenes/lazo_cerrado_hipotenso.png)

**Caso hipertenso:** comparación de $P_{p,normo}(t)$, $P_{p,hiper}(t)$ y $P_{p,hiper,PID}(t)$.

![Lazo cerrado hipertenso](Imagenes/lazo_cerrado_hipertenso.png)

`[COMPLETAR: efecto de cada controlador sobre el seguimiento de la referencia, el error y el comportamiento transitorio]`

### Verificación entre plataformas

Se compararon las respuestas obtenidas con cada implementación para los tres conjuntos de parámetros:

| Implementación | Herramienta | Consistente con las demás |
|---|---|---|
| Ecuaciones de estado | Simulink | `[COMPLETAR]` |
| Función de transferencia | Simulink | `[COMPLETAR]` |
| Circuito Windkessel | Simscape Electrical | `[COMPLETAR]` |
| Cuaderno computacional | MATLAB (.mlx) | `[COMPLETAR]` |
| Código en Python | VS Code (`numpy`, `control`, `scipy`, `matplotlib`, `pandas`) | `[COMPLETAR]` |

`[COMPLETAR: diferencias observadas y su posible origen (método numérico, discretización de la señal aleatoria, implementación de los modelos)]`

## Diagrama biológico

![Diagrama biológico](Imagenes/diagrama_biologico.png)

*Diagrama del sistema cardiovascular elaborado en BioRender.* `[COMPLETAR]`

| Estructura fisiológica | Elemento del modelo |
|---|---|
| Corazón | Fuente de presión/flujo de entrada ($P_a$, $F_a$) |
| Aorta proximal | Impedancia característica $Z$ |
| Grandes arterias | Distensibilidad arterial $C$ |
| Masa de sangre en movimiento | Inertancia $L$ |
| Vasos periféricos | Resistencia periférica $R$ |

## Discusión

- **Efecto de la distensibilidad.** Como solo cambia $C$, las diferencias entre los tres casos provienen de ese único parámetro. Su reducción desplaza el polo rápido hacia la izquierda y aumenta $\omega_n$ y $\zeta$, de modo que el modelo hipertenso responde más rápido que el normotenso y este más rápido que el hipotenso.
- **Estado estacionario.** Los tres modelos tienen ganancia estática unitaria, por lo que ante un escalón convergen al mismo valor; la diferencia entre casos es transitoria.
- **Controladores.** `[COMPLETAR: comentar cómo se comportó cada PID, si hubo sobreimpulso o ruido amplificado por la acción derivativa y por qué cada caso requirió ganancias distintas]`
- **Consistencia entre plataformas.** `[COMPLETAR: comentar coincidencia o diferencias entre MATLAB/Simulink y Python]`
- **Limitaciones.** El modelo es lineal, de parámetros concentrados y de cuatro elementos; la señal de entrada es aleatoria y no una forma de onda de presión fisiológica, y los controladores son un ejercicio de simulación sin interpretación terapéutica.

## Conclusiones

1. Se obtuvo el modelo Windkessel de cuatro elementos como sistema de segundo orden y sus funciones de transferencia para los casos hipotenso, normotenso e hipertenso.
2. Los tres modelos son estables y tienen ganancia estática unitaria; la distensibilidad arterial determina la velocidad y el amortiguamiento de la respuesta.
3. `[COMPLETAR: resultado del diseño de los controladores PID y su efecto sobre el seguimiento de la referencia normotensa]`
4. `[COMPLETAR: resultado de la verificación cruzada entre Simulink, Simscape, MATLAB y Python]`

## Lista de archivos incluidos en el repositorio

1. Desarrollo matemático del modelo Windkessel, ecuaciones de estado y funciones de transferencia (este README).
2. Tabla de parámetros de los modelos hipotenso, normotenso e hipertenso.
3. Cuaderno computacional de MATLAB \[.mlx].
4. Modelo de Simulink \[.slx] (ecuaciones de estado y funciones de transferencia).
5. Modelo de Simscape Electrical del circuito Windkessel.
6. Código fuente en Python \[.py].
7. Señal de entrada utilizada en las simulaciones (`signals.xlsx`).
8. Figuras generadas con MATLAB y Python \[.pdf y .png].
9. Resultados de los sistemas en lazo abierto y en lazo cerrado.
10. Imagen con las ganancias de los controladores PID.
11. Diagrama biológico.
12. Capturas de las simulaciones.
13. Archivos complementarios utilizados durante la práctica.
14. `CITATION.cff` con la información del alumno y del repositorio.

## Referencias

\[1] M. C. Khoo, Physiological Control Systems: Analysis, Simulation, and Estimation, 2nd ed. Piscataway, New Jersey, USA: IEEE Press, 2018.

\[2] F. C. Hoppensteadt and C. S. Peskin, Modeling and Simulation in Medicine and the Life Sciences. Springer.

\[3] J. E. Hall, Guyton and Hall Textbook of Medical Physiology, 14th ed. Elsevier, 2021.

\[4] K. Ogata, Ingeniería de Control Moderna, 5a ed. Pearson Educación, 2010.

\[5] N. S. Nise, Control Systems Engineering, 8th ed. Hoboken, New Jersey, USA: John Wiley & Sons, 2020.

\[6] P. A. Valle, Syllabus para Modelado de Sistemas Fisiológicos, Tecnológico Nacional de México / Instituto Tecnológico de Tijuana, Tijuana, B.C., México, 2026. Permalink: https://biomath.xyz/course/

\[7] Tecnológico Nacional de México, Programa de estudio de la asignatura Modelado de Sistemas Fisiológicos, IBF-1020.
