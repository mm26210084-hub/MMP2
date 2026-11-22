[![Open in MATLAB Online](https://www.mathworks.com/images/responsive/global/open-in-matlab-online.svg)](https://matlab.mathworks.com/open/github/v1?repo=mm26210084-hub/MMP2)

# Práctica 2 — Caos en sistemas biológicos: análisis de estabilidad y condiciones de eliminación

Repositorio académico de la asignatura **Modelado Matemático**, correspondiente a la **Maestría en Ciencias de la Ingeniería** del Tecnológico Nacional de México / Instituto Tecnológico de Tijuana. La práctica estudia un sistema de tres ecuaciones diferenciales ordinarias no lineales que representa la interacción entre poblaciones celulares y busca establecer condiciones para la eliminación de células patológicas.

**Docente:** Dr. Paul Antonio Valle Trujillo  
**Departamento:** Ingeniería Eléctrica y Electrónica  
**Institución:** Tecnológico Nacional de México / Instituto Tecnológico de Tijuana  
**Asignatura:** Modelado Matemático  
**Programa:** Maestría en Ciencias de la Ingeniería  
**Alumno:** Jesús René Correa Martínez  
**No. de control:** 26210084  
**Correo institucional:** mm26210084@tectijuana.edu.mx

---

<a id="contenido"></a>
## Contenido

1. [Resumen](#resumen)
2. [Objetivos](#objetivos)
3. [Modelo matemático](#modelo)
4. [Positividad del sistema](#positividad)
5. [Puntos de equilibrio](#equilibrios)
6. [Estabilidad asintótica local](#local)
7. [Conjuntos compactos invariantes](#invariantes)
8. [Estabilidad global y función de Lyapunov](#global)
9. [Resultados](#resultados)
10. [Interpretación de las condiciones](#interpretacion)
11. [Desarrollo analítico y comprobación](#comprobacion)
12. [Estructura del repositorio](#estructura)
13. [Requisitos](#requisitos)
14. [Reproducibilidad](#reproducibilidad)
15. [Referencias y alcance](#referencias)

---

<a id="resumen"></a>
## Resumen

Se analiza la dinámica de tres poblaciones celulares mediante un sistema autónomo de ecuaciones diferenciales no lineales. Primero se comprueba la invariancia del octante no negativo, necesaria para mantener la interpretación biológica de las variables. Después se calculan los equilibrios libres de células patológicas y se estudia su estabilidad local mediante la matriz Jacobiana y sus valores propios.

El análisis continúa con la localización de conjuntos invariantes que permiten acotar las variables de estado. Estas cotas se emplean en el método directo de Lyapunov para obtener una condición suficiente de eliminación de la población patológica, expresada en términos del parámetro de control $\rho$.

**Palabras clave:** sistemas dinámicos no lineales; poblaciones celulares; invariancia positiva; puntos de equilibrio; matriz Jacobiana; estabilidad local; Lyapunov; LaSalle.

[Volver al contenido](#contenido)

---

<a id="objetivos"></a>
## Objetivos

### Objetivo general

Analizar la estabilidad de un modelo matemático de interacción entre poblaciones celulares y determinar condiciones sobre el parámetro de control que favorezcan la eliminación de células patológicas.

### Objetivos específicos

- Verificar la positividad de las soluciones para condiciones iniciales no negativas.
- Obtener los puntos de equilibrio con población patológica nula, $x=0$.
- Evaluar la estabilidad asintótica local mediante la matriz Jacobiana.
- Determinar cotas de las variables mediante conjuntos invariantes.
- Analizar la función candidata de Lyapunov $V=x$ y establecer una condición suficiente para que la población patológica tienda a cero.
- Distinguir entre eliminación de la población patológica y estabilidad del equilibrio del sistema completo.

---

<a id="modelo"></a>
## Modelo matemático

El modelo está formado por tres ecuaciones diferenciales ordinarias no lineales:

$$
\begin{aligned}
\dot{x} &= r_1x(1-b_1x)-a_{12}xy-a_{13}xz,\\
\dot{y} &= r_2y(1-b_2y)-a_{21}xy,\\
\dot{z} &= (r_3-a_{31})xz-d_3z+\rho.
\end{aligned}
$$

Las variables $x(t)$, $y(t)$ y $z(t)$ representan las tres poblaciones celulares del modelo; $x(t)$ corresponde a las células patológicas. Los coeficientes del sistema son positivos y $\rho\geq 0$ es el parámetro de control. Se consideran condiciones iniciales no negativas.

### Significado de los términos

| Término | Interpretación en las ecuaciones |
|---|---|
| $r_1x(1-b_1x)$ | Crecimiento logístico de la población patológica en ausencia de interacciones con otras poblaciones. |
| $-a_{12}xy$ | Reducción de $x$ asociada con la interacción entre las poblaciones $x$ y $y$. |
| $-a_{13}xz$ | Reducción de $x$ asociada con la interacción entre $x$ y $z$. |
| $r_2y(1-b_2y)$ | Crecimiento logístico de $y$ sin la interacción con $x$. |
| $-a_{21}xy$ | Efecto inhibidor de $x$ sobre $y$. |
| $(r_3-a_{31})xz$ | Contribución neta de la interacción entre $x$ y $z$. |
| $-d_3z$ | Término de decaimiento de $z$. |
| $\rho$ | Aporte externo no negativo a la dinámica de $z$; parámetro de control del modelo. |

Si se anulan las interacciones entre $x$ e $y$ o $z$, los términos $1/b_1$ y $1/b_2$ representan las capacidades de carga de las poblaciones con crecimiento logístico. Los valores de los parámetros no se ajustan a datos experimentales en el informe: el objetivo de la práctica es **deducir condiciones analíticas**.

### Forma vectorial

Definiendo $\mathbf{u}=(x,y,z)^T$, el sistema puede escribirse como

$$
\dot{\mathbf{u}}=\mathbf f(\mathbf{u};\boldsymbol\theta),
$$

donde $\boldsymbol\theta$ reúne los parámetros positivos $r_i$, $b_i$, $a_{ij}$, $d_3$ y el parámetro de control $\rho$.

---

<a id="positividad"></a>
## Positividad del sistema

La evaluación del campo vectorial en las fronteras del octante no negativo permite verificar que las trayectorias no atraviesan dichas fronteras hacia valores negativos. En particular:

$$
\left.\dot{x}\right|_{x=0}=0,\qquad
\left.\dot{y}\right|_{y=0}=0,\qquad
\left.\dot{z}\right|_{z=0}=\rho\geq0.
$$

Así, el dominio $\mathbb{R}_{\geq0}^{3}$ es positivamente invariante.

### Verificación sobre cada frontera

En $x=0$, la ecuación de $x$ se anula porque todos sus términos contienen el factor $x$; una solución que alcanza esa frontera permanece en ella. Ocurre lo mismo para $y=0$. Para $z=0$, la derivada vale $\rho$, por lo que el campo vectorial apunta hacia el interior del octante si $\rho>0$ y es tangente si $\rho=0$.

Esta verificación permite utilizar el octante no negativo como dominio de análisis. No demuestra, por sí sola, que las soluciones sean acotadas: esa cuestión se examina después mediante las funciones localizadoras.

---

<a id="equilibrios"></a>
## Puntos de equilibrio libres de células patológicas

Al imponer $x=0$ y resolver las ecuaciones de equilibrio se obtienen dos estados:

$$
P_0=\left(0,0,\frac{\rho}{d_3}\right),\qquad
P_1=\left(0,\frac{1}{b_2},\frac{\rho}{d_3}\right).
$$

Ambos representan ausencia de células patológicas, aunque su estabilidad es diferente.

### Obtención de los equilibrios

La condición de equilibrio se expresa como

$$
\dot{x}=\dot{y}=\dot{z}=0.
$$

Al imponer $x=0$, las ecuaciones restantes se reducen a

$$
r_2y(1-b_2y)=0,\qquad -d_3z+\rho=0.
$$

La primera ecuación tiene dos soluciones, $y=0$ e $y=1/b_2$, mientras que la segunda determina $z=\rho/d_3$. Por eso aparecen exactamente los dos equilibrios anteriores **dentro de la restricción $x=0$**. Esto no excluye la existencia de equilibrios adicionales con $x>0$; esos no son el objeto de la sección del informe dedicada a estados libres de células patológicas.

---

<a id="local"></a>
## Estabilidad asintótica local

Para estudiar los equilibrios se calcula la matriz Jacobiana del sistema y se evalúan sus valores propios.

### Matriz Jacobiana general

Al derivar cada componente del campo vectorial con respecto a $x$, $y$ y $z$, se obtiene

$$
J(x,y,z)=
\begin{bmatrix}
r_1-2r_1b_1x-a_{12}y-a_{13}z & -a_{12}x & -a_{13}x\\
-a_{21}y & r_2-2r_2b_2y-a_{21}x & 0\\
(r_3-a_{31})z & 0 & (r_3-a_{31})x-d_3
\end{bmatrix}.
$$

La estabilidad local de un equilibrio hiperbólico se establece a partir del signo de las partes reales de sus valores propios: si todas son negativas, el equilibrio es localmente asintóticamente estable; si alguna es positiva, resulta inestable.

### Evaluación en el equilibrio $P_0$

Sustituyendo $P_0=(0,0,\rho/d_3)$:

$$
J(P_0)=
\begin{bmatrix}
r_1-\frac{a_{13}\rho}{d_3}&0&0\\
0&r_2&0\\
\frac{\rho}{d_3}(r_3-a_{31})&0&-d_3
\end{bmatrix}.
$$

La matriz es triangular inferior; sus valores propios son los elementos de la diagonal:

$$
\lambda_1=r_1-\frac{a_{13}\rho}{d_3},\qquad
\lambda_2=r_2,\qquad
\lambda_3=-d_3.
$$

La hipótesis $r_2>0$ implica que $\lambda_2>0$ para cualquier elección de $\rho$. Incluso si el parámetro de control hace negativo a $\lambda_1$, permanece la dirección inestable asociada con $y$.

### Evaluación en el equilibrio $P_1$

Al sustituir $P_1=(0,1/b_2,\rho/d_3)$:

$$
J(P_1)=
\begin{bmatrix}
r_1-\frac{a_{12}}{b_2}-\frac{a_{13}\rho}{d_3}&0&0\\
-\frac{a_{21}}{b_2}&-r_2&0\\
\frac{\rho}{d_3}(r_3-a_{31})&0&-d_3
\end{bmatrix}.
$$

El polinomio característico se obtiene de $\det(J(P_1)-\lambda I)=0$. Como la matriz también es triangular inferior, se factoriza directamente:

$$
\left(r_1-\frac{a_{12}}{b_2}-\frac{a_{13}\rho}{d_3}-\lambda\right)
(-r_2-\lambda)(-d_3-\lambda)=0.
$$

En $P_0$, uno de los valores propios es $\lambda_2=r_2>0$; por tanto, el equilibrio es **inestable**, independientemente del valor positivo del parámetro de control.

En $P_1$, los valores propios son

$$
\lambda_1=r_1-\frac{a_{12}}{b_2}-\frac{a_{13}\rho}{d_3},
\qquad \lambda_2=-r_2,\qquad \lambda_3=-d_3.
$$

Por consiguiente, $P_1$ es **localmente asintóticamente estable** si

$$
\boxed{\rho>\frac{d_3}{a_{13}}\left(r_1-\frac{a_{12}}{b_2}\right).}
$$

Esta condición se refiere a trayectorias que parten suficientemente cerca de $P_1$.

### Despeje de la condición sobre el control

Debido a que $r_2,d_3>0$, los dos últimos valores propios son negativos. Para el primero se exige

$$
r_1-\frac{a_{12}}{b_2}-\frac{a_{13}\rho}{d_3}<0.
$$

Despejando $\rho$ y recordando que $a_{13},d_3>0$, se obtiene la condición de estabilidad local presentada arriba. Cuando el umbral del lado derecho es negativo, cualquier $\rho\geq0$ satisface la desigualdad estricta; si el umbral es cero, se requiere $\rho>0$.

Si $\lambda_1>0$, el equilibrio $P_1$ es inestable. En la igualdad $\lambda_1=0$, la linealización es no hiperbólica y el criterio de los valores propios por sí solo no decide la estabilidad. Por esa razón la condición del documento se formula como una desigualdad estricta.

---

<a id="invariantes"></a>
## Conjuntos compactos invariantes

Se emplean funciones localizadoras para establecer cotas de las variables $x(t)$, $y(t)$ y $z(t)$. En el documento se obtienen regiones de interés para la dinámica en el octante no negativo, incluyendo la cota inferior

$$
z(t)\geq \frac{\rho}{d_3}
$$

en la región correspondiente. Estas restricciones permiten evaluar el signo de la derivada de la función candidata de Lyapunov en el dominio considerado.

### Función localizadora $h_1=x$

La derivada de Lie de $h_1$ coincide con la primera ecuación del sistema:

$$
L_fh_1=r_1x(1-b_1x)-a_{12}xy-a_{13}xz.
$$

La condición $L_fh_1=0$ puede escribirse como

$$
x\left(r_1-r_1b_1x-a_{12}y-a_{13}z\right)=0.
$$

De este producto se desprenden el plano invariante $x=0$ y la superficie

$$
x=\frac{1}{b_1}-\frac{a_{12}}{r_1b_1}y-\frac{a_{13}}{r_1b_1}z.
$$

Como las variables son no negativas, las interacciones disminuyen la tasa de crecimiento de $x$. También se tiene la comparación

$$
\dot{x}\leq r_1x(1-b_1x).
$$

Por el principio de comparación, para condiciones iniciales no negativas,

$$
0 \leq x(t) \leq \max\left(x(0),\frac{1}{b_1}\right).
$$

En particular, el intervalo $0\leq x\leq 1/b_1$ es positivamente invariante **si la condición inicial ya pertenece a ese intervalo**. El valor $1/b_1$ no debe interpretarse como una cota válida instantáneamente para una trayectoria que parte con $x(0)>1/b_1$.

### Función localizadora $h_2=y$

En este caso,

$$
L_fh_2=r_2y(1-b_2y)-a_{21}xy
=y\left(r_2-r_2b_2y-a_{21}x\right).
$$

La superficie donde la derivada se anula, además del plano $y=0$, está dada por

$$
y=\frac{1}{b_2}-\frac{a_{21}}{r_2b_2}x.
$$

Utilizando $x\geq0$:

$$
\dot{y}\leq r_2y(1-b_2y),
$$

de donde se obtiene

$$
0\leq y(t)\leq\max\left\{y(0),\frac{1}{b_2}\right\}.
$$

La región $0\leq y\leq1/b_2$ también resulta invariante para condiciones iniciales dentro de ella.

### Función localizadora $h_3=z$

Para la tercera variable,

$$
L_fh_3=\dot z=(r_3-a_{31})xz-d_3z+\rho.
$$

En la superficie $L_fh_3=0$, siempre que el denominador no se anule,

$$
z=\frac{\rho}{d_3-(r_3-a_{31})x}.
$$

El informe considera la condición $r_3-a_{31}>0$. Bajo esa hipótesis, como $x,z\geq0$, se cumple

$$
\dot z\geq -d_3z+\rho.
$$

La solución de la ecuación escalar de comparación $\dot w=-d_3w+\rho$ es

$$
w(t)=\frac{\rho}{d_3}+\left(z(0)-\frac{\rho}{d_3}\right)e^{-d_3t}.
$$

Así, $z(t)\geq w(t)$; en particular, el semiespacio $z\geq\rho/d_3$ es positivamente invariante si $z(0)\geq\rho/d_3$. Para $z(0)<\rho/d_3$, la comparación no justifica la desigualdad $z(t)\geq\rho/d_3$ en todo instante, aunque sí proporciona una cota inferior dependiente del tiempo.

### Alcance de las cotas

| Variable | Región indicada en el informe | Precisión para la invariancia |
|---|---|---|
| $x$ | $0\leq x\leq1/b_1$ | Requiere $x(0)\leq1/b_1$ para mantenerse en ese intervalo desde $t=0$. |
| $y$ | $0\leq y\leq1/b_2$ | Requiere $y(0)\leq1/b_2$ para mantenerse en ese intervalo desde $t=0$. |
| $z$ | $z\geq\rho/d_3$ | Requiere $r_3>a_{31}$ y $z(0)\geq\rho/d_3$ para esta justificación directa. |

La intersección de las tres regiones es invariante bajo esas hipótesis. No obstante, una cota inferior de $z$ no proporciona una cota superior; para afirmar que una región completa es **compacta**, se necesita además una cota superior de $z$. Por ejemplo, si se dispone de $x(t)\leq M_x$ y se cumple $(r_3-a_{31})M_x<d_3$, entonces

$$
\dot z\leq-\left[d_3-(r_3-a_{31})M_x\right]z+\rho,
$$

lo que permite deducir una cota superior mediante comparación. Se trata de una **condición adicional**, no de un resultado incondicional de la práctica.

---

<a id="global"></a>
## Estabilidad global y función de Lyapunov

Para estudiar la eliminación de células patológicas se considera

$$
V(x,y,z)=x.
$$

Su derivada temporal a lo largo de las soluciones es

$$
\dot V=x\left(r_1-r_1b_1x-a_{12}y-a_{13}z\right).
$$

Al utilizar las cotas del análisis de conjuntos invariantes, se obtiene como condición suficiente:

$$
\boxed{\rho>\frac{r_1d_3}{a_{13}}.}
$$

Bajo esta condición y las hipótesis de invariancia y acotación empleadas, $\dot V<0$ para $x>0$ en la región estudiada, lo que permite establecer la disminución de la población patológica hacia $x=0$. La función $V=x$ no es definida positiva ni radialmente desacotada respecto al estado tridimensional completo; por ello, la eliminación de $x$ debe distinguirse de una prueba de estabilidad asintótica global del equilibrio completo.

El documento también revisa la diferencia entre derivada negativa definida y semidefinida negativa y el papel del principio de invariancia de LaSalle.

### Positividad de la función candidata

En el octante no negativo, $V=x\geq0$ y $V=0$ cuando $x=0$. Sin embargo, $V$ no es definida positiva respecto al punto aislado $P_1$, porque se anula para todos los estados del plano $x=0$, independientemente de $y$ y $z$. Puede entenderse como una medida de distancia al **conjunto objetivo** de eliminación de células patológicas, pero no como una función de Lyapunov definida positiva para el sistema completo alrededor de $P_1$.

### Desacotación radial

Para un estado tridimensional, una función radialmente desacotada debe tender a infinito siempre que $\|(x,y,z)\|\to\infty$. La función $V=x$ no cumple ese requisito: por ejemplo, $V(1,y,0)=1$ aunque $y\to\infty$. Sí crece sin límite cuando únicamente crece $x\geq0$.

### Signo de la derivada temporal

En el dominio no negativo, una condición suficiente para que $\dot V\leq0$ es

$$
r_1b_1x+a_{12}y+a_{13}z\geq r_1.
$$

La estrategia del informe es reemplazar las variables por sus cotas inferiores. Al utilizar $x\geq0$, $y\geq0$ y $z\geq\rho/d_3$, se obtiene

$$
\begin{aligned}
\dot V&=x(r_1-r_1b_1x-a_{12}y-a_{13}z)\\
&\leq x\left(r_1-\frac{a_{13}\rho}{d_3}\right).
\end{aligned}
$$

Si $\rho>r_1d_3/a_{13}$, existe $\varepsilon=a_{13}\rho/d_3-r_1>0$ tal que

$$
\dot V\leq-\varepsilon x.
$$

En cualquier trayectoria que permanezca en el dominio donde es válida la cota inferior de $z$, resulta

$$
0\leq x(t)\leq x(0)e^{-\varepsilon t},
$$

lo cual prueba una disminución exponencial de $x(t)$ hacia cero. Esta estimación directa refuerza la conclusión de eliminación de esa población; no necesita asumir que $V$ sea definida positiva respecto a $P_1$.

### Derivada semidefinida negativa y LaSalle

La derivada de una función de Lyapunov es semidefinida negativa cuando $\dot V\leq0$; es definida negativa respecto al equilibrio cuando $\dot V<0$ fuera de él. En este modelo, incluso bajo la condición estricta sobre $\rho$, se tiene $\dot V=0$ sobre todo el plano $x=0$, por lo que no es negativa definida respecto al punto $P_1$.

El conjunto de ceros de la derivada, antes de imponer condiciones estrictas sobre los parámetros, es

$$
E=\{(x,y,z):x=0\}\cup
\{(x,y,z):r_1-r_1b_1x-a_{12}y-a_{13}z=0\}.
$$

El principio de invariancia de LaSalle permite estudiar el mayor conjunto invariante contenido en $E$, bajo las hipótesis necesarias de invariancia positiva, acotación y $\dot V\leq0$. En particular, que $x(t)\to0$ no es idéntico a probar que el estado completo converge a $P_1$: sobre $x=0$ aún evolucionan $y$ y $z$.

### Dinámica restringida al plano $x=0$

En ese plano, las otras dos ecuaciones se desacoplan:

$$
\dot y=r_2y(1-b_2y),\qquad \dot z=-d_3z+\rho.
$$

Si $y(0)>0$, la dinámica logística converge a $1/b_2$; si $y(0)=0$, permanece en cero. En ambos casos $z(t)\to\rho/d_3$. Esto explica por qué $P_0$ y $P_1$ son posibles estados libres de células patológicas y por qué la distinción entre condiciones iniciales estrictamente positivas y meramente no negativas importa al formular una afirmación de convergencia global.

---

<a id="resultados"></a>
## Resultados

| Análisis | Resultado |
|---|---|
| Positividad | El octante no negativo es positivamente invariante. |
| Equilibrio $P_0$ | Inestable por presentar un valor propio positivo. |
| Equilibrio $P_1$ | Localmente asintóticamente estable si $\rho>\frac{d_3}{a_{13}}(r_1-\frac{a_{12}}{b_2})$. |
| Eliminación de células patológicas | Condición suficiente $\rho>\frac{r_1d_3}{a_{13}}$, bajo las hipótesis del análisis global. |

La condición de eliminación obtenida mediante Lyapunov es más restrictiva que la condición local. El análisis establece un criterio para la convergencia de la población patológica a cero; la convergencia de todas las variables al equilibrio $P_1$ requiere examinar además la dinámica de $y$ y $z$.

### Comparación algebraica de umbrales

Denotando por $\rho_{\mathrm{local}}$ y $\rho_{\mathrm{elim}}$ los umbrales respectivos:

$$
\rho_{\mathrm{local}}=\frac{d_3}{a_{13}}\left(r_1-\frac{a_{12}}{b_2}\right),
\qquad
\rho_{\mathrm{elim}}=\frac{r_1d_3}{a_{13}}.
$$

La diferencia es

$$
\rho_{\mathrm{elim}}-\rho_{\mathrm{local}}
=\frac{a_{12}d_3}{a_{13}b_2}>0.
$$

Esta desigualdad, que se sigue únicamente de la positividad de los parámetros, confirma que el umbral de eliminación global propuesto es más exigente. La condición local aprovecha el valor positivo $y=1/b_2$ en el equilibrio, mientras que la estimación de la derivada de Lyapunov considera el peor caso $y\geq0$ y no utiliza una cota inferior estrictamente positiva para esa población.

### Resultados que no se afirman

- No se estimaron parámetros a partir de datos observados ni se reportaron errores de ajuste.
- No se incluyeron ejecuciones numéricas, mapas de bifurcación o cálculos de exponentes de Lyapunov. Aunque el título de la práctica menciona el caos, el desarrollo entregado se concentra en condiciones de estabilidad y eliminación.
- La invariancia del octante no negativo no implica automáticamente la existencia de un conjunto compacto absorbente.
- La condición $\rho>r_1d_3/a_{13}$ se obtuvo utilizando una cota específica de $z$; su aplicación directa debe respetar la región y las hipótesis en que dicha cota es válida.

---

<a id="interpretacion"></a>
## Interpretación de las condiciones

### ¿Por qué se distinguen estabilidad local y eliminación global?

La matriz Jacobiana describe la evolución de perturbaciones pequeñas alrededor de un equilibrio. Cuando todos los valores propios de $J(P_1)$ tienen parte real negativa, el sistema retorna al entorno de $P_1$ después de perturbaciones suficientemente pequeñas. El criterio no caracteriza, sin trabajo adicional, el comportamiento de trayectorias que comienzan lejos de ese punto.

El método directo de Lyapunov, por su parte, estudia el signo de $\dot V$ en una región del espacio de estados. Para $V=x$, el resultado se refiere directamente a la disminución de una población concreta. Es una afirmación distinta de estabilidad de todo el vector de estado $(x,y,z)$.

### ¿Qué significa que el parámetro de control supere un umbral?

En la primera ecuación, el término $-a_{13}xz$ se opone al crecimiento de $x$. En la región $z\geq\rho/d_3$, el aporte mínimo de esa interacción es proporcional al parámetro de control. Cuando

$$
\frac{a_{13}\rho}{d_3}>r_1,
$$

ese término basta, aun sin utilizar el efecto de $y$, para superar la tasa lineal de crecimiento $r_1$ de la población patológica. Ésa es la razón algebraica de la condición de eliminación obtenida en el informe.

### ¿Qué ocurre en el umbral exacto?

La igualdad $\rho=r_1d_3/a_{13}$ produce una estimación no estricta $\dot V\leq0$. La desigualdad por sí sola ya no proporciona la tasa exponencial positiva $\varepsilon$ utilizada antes. La demostración del caso límite requiere analizar los términos restantes o aplicar un argumento de invariancia apropiado; no está cubierto por la condición estricta del informe.

---

<a id="comprobacion"></a>
## Desarrollo analítico y comprobación

La práctica sigue un orden lógico que permite revisar cada resultado sin depender de una simulación:

1. **Definir las ecuaciones y el dominio.** Se consideran parámetros positivos y condiciones iniciales no negativas. El plano $x=0$ es invariante porque $\dot x$ contiene el factor $x$.
2. **Comprobar la positividad.** Se evalúan las tres derivadas en las respectivas caras del octante.
3. **Resolver los equilibrios libres de células patológicas.** Se impone $x=0$ y se resuelve el sistema reducido.
4. **Formar la matriz Jacobiana.** Se calculan las nueve derivadas parciales del campo vectorial.
5. **Evaluar cada equilibrio.** Se sustituyen las coordenadas de $P_0$ y $P_1$; la triangularidad de ambas matrices simplifica los valores propios.
6. **Aplicar el criterio local.** Se exige que los valores propios tengan parte real negativa y se despeja el parámetro $\rho$.
7. **Buscar regiones invariantes.** Se proponen $h_1=x$, $h_2=y$ y $h_3=z$, y se calculan sus derivadas de Lie.
8. **Estudiar la eliminación.** Con $V=x$, se obtiene $\dot V$ y se emplea la cota inferior de $z$ para establecer una condición suficiente.
9. **Revisar el alcance de la conclusión.** Se distingue convergencia de $x$ a cero, estabilidad de un equilibrio específico y validez de las cotas para las condiciones iniciales.

### Comprobaciones algebraicas recomendadas

| Comprobación | Expresión verificable |
|---|---|
| Frontera $z=0$ | $\dot z=\rho\geq0$ |
| Valores propios de $P_0$ | $r_1-a_{13}\rho/d_3$, $r_2$, $-d_3$ |
| Valores propios de $P_1$ | $r_1-a_{12}/b_2-a_{13}\rho/d_3$, $-r_2$, $-d_3$ |
| Condición local | $\rho>\frac{d_3}{a_{13}}(r_1-a_{12}/b_2)$ |
| Derivada de $V=x$ | $\dot V=x(r_1-r_1b_1x-a_{12}y-a_{13}z)$ |
| Condición de eliminación | $\rho>r_1d_3/a_{13}$ en la región de aplicación |

Los resultados son simbólicos. La sustitución de valores numéricos exige un conjunto de parámetros especificado previamente; no se presentan valores supuestos como si fueran mediciones o estimaciones del trabajo.

---

<a id="estructura"></a>
## Estructura del repositorio

El desarrollo completo —deducciones, matrices, valores propios, conjuntos invariantes y condiciones de estabilidad— se encuentra en el informe de la práctica. El archivo fuente fue preparado en **Scientific WorkPlace** con sintaxis LaTeX.

Una estructura simple para el repositorio de esta práctica es:

```text
Practica-2/
├── README.md
├── Correa26210084.tex
└── Correa26210084.pdf
```

Los nombres corresponden a los materiales trabajados en esta conversación. Si el PDF tiene otro nombre en tu repositorio, modifica la lista antes de publicarlo.

### Archivos

- `Correa26210084.tex`: fuente del informe.
- `Correa26210084.pdf`: versión del informe para consulta.
- `README.md`: descripción de la práctica y resultados principales.

---

<a id="requisitos"></a>
## Requisitos

- Un lector de archivos PDF para consultar el informe compilado.
- **Scientific WorkPlace** o un entorno LaTeX compatible para editar el archivo fuente.
- Si se utiliza un compilador LaTeX externo, es necesario revisar las dependencias del documento original.

**Nota de compilación:** el archivo `.tex` incluye `\input{tcilatex}`, propio del entorno Scientific WorkPlace. Si se compila fuera de ese software, puede ser necesario disponer de sus archivos de soporte o adaptar el preámbulo.

---

<a id="reproducibilidad"></a>
## Reproducibilidad

La práctica es principalmente analítica. Para reproducir sus resultados no se necesita una base de datos ni ejecutar código de MATLAB: basta con seguir las deducciones del informe y verificar los cálculos de derivadas parciales, sustitución de equilibrios, determinantes y desigualdades.

### Resultados mínimos esperados

1. Verificar que las derivadas en las fronteras cumplen las condiciones de positividad.
2. Obtener los equilibrios $P_0$ y $P_1$ al fijar $x=0$.
3. Reproducir la Jacobiana y sus evaluaciones en ambos equilibrios.
4. Identificar el valor propio positivo $r_2$ en $P_0$.
5. Deducir el umbral local para $P_1$.
6. Recuperar las funciones localizadoras y precisar sus regiones de aplicación.
7. Deducir el umbral suficiente de eliminación a partir de $\dot V$.

No se requiere comparar cifras experimentales: las verificaciones consisten en comprobar identidades y desigualdades matemáticas.

---

<a id="referencias"></a>
## Referencias y alcance

La fuente principal de esta descripción es el informe **Práctica 2: Caos en sistemas biológicos**, elaborado por Jesús René Correa Martínez para la asignatura Modelado Matemático. En el apartado de positividad se menciona el criterio de De Leenheer y Aeyels, y en el estudio global se emplean los conceptos del método directo de Lyapunov y el principio de invariancia de LaSalle. El documento fuente proporcionado no contiene los datos bibliográficos completos de esas obras; por tanto, no se incluyen citas editoriales inventadas en este README.

El texto del repositorio resume el desarrollo y añade precisiones sobre el dominio de validez de las cotas. El informe adjunto sigue siendo la referencia para el procedimiento y los resultados presentados en la práctica.

---

## Uso académico

Material elaborado para la asignatura **Modelado Matemático**, Maestría en Ciencias de la Ingeniería, Instituto Tecnológico de Tijuana.
