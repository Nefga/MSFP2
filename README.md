# MSFP2
Practica 2: Modelado y control del sistema cardiovascular
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

## Objetivos

1. Calcular la función de transferencia.
2. Determinar el modelo de ecuaciones integro-diferenciales.
3. Analizar la estabilidad del sistema.
4. Determinar el error en estado estacionario.
5. Emular la respuesta del modelo Windkessel en Simulink/MATLAB para los casos hipotenso, normotenso e hipertenso.
6. Sintonizar las ganancias de un controlador PID en Simulink/MATLAB para el caso hipotenso y otro para el hipertenso, tomando la respuesta normotensa como referencia.
7. Simular la respuesta de los tres casos en lazo abierto y de los casos hipotenso e hipertenso en lazo cerrado en Simulink/MATLAB.
8. Obtener la respuesta en lazo abierto y en lazo cerrado con el controlador PID en VS Code/Python con la función de transferencia.

## Descripción detallada del sistema

En esta práctica se presenta el modelo Windkessel de cuatro elementos del sistema arterial, un circuito de segundo orden con impedancia característica Z, distensibilidad arterial C, resistencia periférica R e inertancia arterial L. Se comparan tres casos: normotenso (referencia), hipotenso e hipertenso, que difieren únicamente en la distensibilidad arterial (C = 3.5, 1.5 y 1.0 ml/mmHg, respectivamente) con Z = 0.010 mmHg·s/ml, L = 0.001 mmHg·s²/ml y R = 1.000 mmHg·s/ml constantes. Los tres sistemas son estables, con ganancia estática unitaria y un error en estado estacionario de 0 ante una entrada escalón; el caso hipotenso presenta una respuesta subamortiguada y los casos normotenso e hipertenso una respuesta sobreamortiguada, de modo que la diferencia entre ellos es transitoria y depende directamente de la distensibilidad arterial.

Los tres modelos se excitan con la misma señal aleatoria uniforme (mínimo −0.2, máximo 1, semilla 106, Ts = 0.1 s, t ∈ [0, 15] s) y, para los casos hipotenso e hipertenso, se diseña un controlador PID que reduce la diferencia entre la respuesta del caso y la del individuo normotenso. Las estrategias de control tienen propósitos exclusivamente académicos y de simulación.

Palabras clave: Modelo Windkessel; Sistema cardiovascular; Controlador PID; Modelo matemático; Simulaciones numéricas.

## Análisis matemático

### Modelo de ecuaciones integro-diferenciales

Por conservación del flujo en el nodo de $P_p(t)$:

$$F_a(t) = F_Z(t) + F_L(t) = F_C(t) + F_R(t)$$

con

$$F_Z(t) = \frac{P_a(t) - P_p(t)}{Z}, \quad F_L(t) = \frac{1}{L}\int \left[P_a(t) - P_p(t)\right]dt, \quad F_C(t) = C\,\frac{dP_p(t)}{dt}, \quad F_R(t) = \frac{P_p(t)}{R}$$

Sustituyendo y agrupando los términos en $P_p(t)$:

$$C\,\frac{dP_p(t)}{dt} + \left(\frac{1}{R} + \frac{1}{Z}\right)P_p(t) = \frac{P_a(t)}{Z} + \frac{1}{L}\int \left[P_a(t) - P_p(t)\right]dt$$

La única variable desconocida es la salida $P_p(t)$. Como aparece también dentro de una derivada y de una integral, no puede despejarse de forma algebraica explícita, por lo que el modelo se deja en esta forma.

### Función de transferencia

Aplicando la transformada de Laplace con condiciones iniciales nulas:

$$F_Z(s) + F_L(s) = F_C(s) + F_R(s)$$

$$F_Z(s) = \frac{P_a(s) - P_p(s)}{Z}, \quad F_L(s) = \frac{P_a(s) - P_p(s)}{Ls}, \quad F_C(s) = Cs\,P_p(s), \quad F_R(s) = \frac{P_p(s)}{R}$$

$$\frac{P_a(s) - P_p(s)}{Z} + \frac{P_a(s) - P_p(s)}{Ls} = Cs\,P_p(s) + \frac{P_p(s)}{R}$$

$$P_a(s)\left(\frac{1}{Z} + \frac{1}{Ls}\right) = P_p(s)\left(Cs + \frac{1}{R} + \frac{1}{Z} + \frac{1}{Ls}\right)$$

$$P_a(s)\,\frac{Ls + Z}{LZs} = P_p(s)\,\frac{CLRZ\,s^2 + L(Z+R)\,s + RZ}{RLZs}$$

$$G(s) = \frac{P_p(s)}{P_a(s)} = \frac{LR\,s + RZ}{CLRZ\,s^2 + L(Z+R)\,s + RZ}$$

Con $Z = 0.01$, $L = 0.001$ y $R = 1$, el numerador es $0.001\,s + 0.01$ y el coeficiente de $s$ en el denominador es $0.00101$ en los tres casos; solo cambia el coeficiente de $s^2$, que vale $CLRZ = C \times 10^{-5}$:

$$G_{hipo}(s) = \frac{0.001\,s + 0.01}{3.5\times10^{-5}\,s^2 + 0.00101\,s + 0.01}$$

$$G_{normo}(s) = \frac{0.001\,s + 0.01}{1.5\times10^{-5}\,s^2 + 0.00101\,s + 0.01}$$

$$G_{hiper}(s) = \frac{0.001\,s + 0.01}{1\times10^{-5}\,s^2 + 0.00101\,s + 0.01}$$

Las tres funciones son de segundo orden, lo que coincide con los dos elementos independientes de almacenamiento de energía ($C$ y $L$).

### Error en estado estacionario

Para una entrada escalón de amplitud $A_0$, por el teorema del valor final:

$$P_p(\infty) = \lim_{s\to 0} s\,G(s)\,\frac{A_0}{s} = G(0)\,A_0 = A_0$$

Como $G(0) = 1$ en los tres modelos, el error entre la entrada y la salida es $e_{ss} = 0$ V en lazo abierto para los casos hipotenso, normotenso e hipertenso.

### Estabilidad en lazo abierto

Con $a = CLRZ$, $b = L(Z+R)$ y $c = RZ$, los polos son las raíces del denominador:

$$\lambda_{1,2} = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}$$

| Modelo | $C$ (ml/mmHg) | $b^2 - 4ac$ | Polos | Respuesta |
|---|---|---|---|---|
| Hipotenso | 3.5 | $-3.799\times10^{-7}$ | $-14.43 \pm 8.81j$ | Estable y subamortiguada |
| Normotenso | 1.5 | $4.201\times10^{-7}$ | $-12.06,\ -55.27$ | Estable y sobreamortiguada |
| Hipertenso | 1.0 | $6.201\times10^{-7}$ | $-11.13,\ -89.87$ | Estable y sobreamortiguada |

Todos los polos tienen parte real negativa, por lo que los tres sistemas son estables. El amortiguamiento crítico ocurriría en $C \approx 2.55$ ml/mmHg; ningún caso lo alcanza.

## Lista de archivos incluidos en el repositorio

1. Cuaderno computacional de MATLAB \[.mlx] y \[.pdf].
2. Modelo de Simulink \[.slx].
3. Archivos de VSC \[.py].
4. Señal de entrada utilizada en las simulaciones \[signals.xlsx].
5. Imagen con los parámetros de los controladores.
6. Imágenes de las simulaciones \[.pdf y .png].
7. Diagrama biológico del sistema cardiovascular.
8. Análisis matemático: Función de transferencia, modelo de ecuaciones integro-diferenciales, estabilidad y error en estado estacionario.

## Referencias

\[1] P. A. Valle, Syllabus para Modelado de Sistemas Fisiológicos, Tecnológico Nacional de México / Instituto Tecnológico de Tijuana, Tijuana, B.C., México, 2025. Permalink: https://biomath.xyz/course/

\[2] M. C. Khoo, Physiological Control Systems Analysis Simulation, and Estimation, 2nd ed. Piscataway, New Jersey, USA: IEEE Press, 2018, Section 4, Page 93.

\[3] N. S. Nise, Control Systems Engineering, 8th ed. Hoboken, New Jersey, USA: John Wiley & Sons, 2020.
