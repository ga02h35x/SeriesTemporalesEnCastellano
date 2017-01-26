# Series Temporales



Table of Contents
=================

   * [Series Temporales](#series-temporales)
      * [Introducción](#introducción)
      * [Análisis de series temporales univariantes](#análisis-de-series-temporales-univariantes)
      * [Enfoques](#enfoques)
      	* [Enfoque clásico (descomposición)](#enfoque-clásico)
	* [Enfoque del Alisado Exponencial](#enfoque-del-alisado-exponencial)
	* [Enfoque Box-Jenkins](#enfoque-box-jenkins)
      * [Series temporales con R](#series-temporales-con-r)
      	 * [Paquetes R para el análisis y tratamiento de Series Temporales:](#paquetes-r-para-el-análisis-y-tratamiento-de-series-temporales)
      * [Ejemplos](#ejemplos)

   * [Bibliografía](#bibliografía)





## Introducción

Una serie temporal es una colección de observaciones de una variable tomadas de forma secuencial y ordenada en el tiempo (instantes de tiempo equiespaciados). Las series pueden tener una periodicidad anual, semestral, trimestral, mensual, etc., según los periodos de tiempo en los que están recogidos los datos que la componen. Las series temporales se pueden definir como un caso particular de los procesos estocásticos, ya que un proceso estocástico es una secuencia de variables aleatorias, ordenadas y equidistantes cronológicamente referidas a una característica observable en diferentes momentos.

Algunos ejemplos de series temporales vienen de campos como la economía (producto interior bruto anual, tasa de inflación, tasa de desempleo, ...),  la demografía (nacimientos anuales, tasa de dependencia, ...), la meteorología (temperaturas máximas, medias o mínimas, precipitaciones diarias, ...), etc.

![STexample](http://www.otexts.org/sites/default/files/fpp/images/a10.png)

El objetivo de una serie temporal reside en estudiar los cambios en esa variable con respeto al tiempo (descripción), y en predecir sus valores futuros (predicción). Por lo tanto, el análisis de series temporales presenta un conjunto de técnicas estadísticas que permiten extraer las regularidades que se observan en el comportamiento pasado de la variable, para tratar de predecir el comportamiento futuro.

Una serie temporal se representa mediante un gráfico temporal, con el valor de la serie en el eje de ordenadas y los tiempos en el eje de abscisas. Esta es la forma más sencilla de comenzar el análisis de una serie temporal y permite detectar las características y componentes más importantes de una serie. 

Los componentes que forman una serie temporal son los siguientes:

- Tendencia: Se puede definir como un cambio a largo plazo que se produce en relación al nivel medio, o el cambio a largo plazo de la media. La tendencia se identifica con un movimiento suave de la serie a largo plazo.
- Estacionalidad: Se puede definir como cierta periodicidad de corto plazo, es decir, cuando se observa en la serie un patrón sistemático que se repite periódicamente (cada año, cada mes, etc., dependiendo de las unidades de tiempo en que vengan recogidos los datos). Por ejemplo, el paro laboral aumenta en general en invierno y disminuye en verano. 
- Ciclo: Similar a la estacionalidad, ya que se puede definir como una fluctuación alrededor de la tendencia, pero de una duración irregular (no estrictamente periódica).
- Irregular: Son factores que aparecen de forma aleatoria y que no responden a un comportamiento sistemático o regular y por tanto no pueden ser predecidos. No se corresponden a la tendencia ni a la estacionalidad ni a los ciclos.

Además, las series temporales se pueden dividir en:

- Estacionarias: es aquella en la que las propiedades estadísticas de la serie son estables, no varían con el tiempo, más en concreto su media y varianza se mantienen constantes a lo lardo del tiempo. Si una serie temporal tiene una media constante a lo largo del tiempo, decimos que es estacionaria con respecto a la media. Si tiene varianza constante con respecto al tiempo, decimos que es estacionaria en varianza.
- No estacionarias: son aquellas en las que las propiedades estadísticas de la serie sí varían con el tiempo. Estas series pueden mostrar cambio de varianza, tendencia o efectos estacionales a lo largo del tiempo. Una serie es no estacionaria en media cuando muestra una tendencia, y una serie es no estacionaria en varianza cuando la variabilidad de los datos es diferente a lo largo de la serie.

La importancia de esta división reside en que la estacionaridad (en media y en varianza) es un requisito que debe cumplirse para poder aplicar modelos paramétricos de análisis y predicción de series de datos. Ya que con series estacionarias podemos obtener predicciones fácilmente, debido a que como la media es constante se puede estimar con todos los datos y utilizar este valor para predecir una nueva observación. Y también permite obtener intervalos de confianza para las predicciones. Un tipo especial de serie estacionaria es la serie denominada ruido blanco. Un ruido blanco es una serie estacionaria tal que ninguna observación influye sobre las siguientes, es decir, donde los valores son independientes e idénticamente distribuidos a lo largo del tiempo con media y covarianza cero e igual varianza.

Las series temporales también se pueden dividir según cuántas variables se observan o según su variabilidad:

- Univariante: la serie temporal es un conjunto de observaciones sobre una única caracteristica o variable.
- Multivariante: (o vectorial): la serie temporal es un conjunto de observaciones de varias variables. <br>
<br>
- Homocedástica: una serie es homocedástica si su variabilidad se mantiene constante a lo largo de la serie.
- Heterocedástica: una serie es heterocedástica cuando la variabilidad de la serie aumenta o disminuye a lo largo del tiempo.

Por otro lado, la variable que se observa en una serie temporal puede ser de tipo:

- Flujo: variable cuya cantidad es acumulada a lo largo del tiempo, por ejemplo: inversión, ahorro, etc.
- Stock: variable cuya cantidad se mide en un determinado momento del tiempo, por ejemplo: población, nº parados, etc.

________________________________________________________________________

## Análisis de series temporales univariantes

La tarea de predicción y análisis de series temporales por lo general consta de cinco pasos básicos (sólo como referencia):

- Definición del problema. A menudo, esta es la parte más difícil de la predicción. 
En la definición del problema se necesita que se comprenda la forma en que se utilizarán las predicciones. Hay que dedicar tiempo para hablar con todas las entidades que van a participar en la recogida de datos, el mantenimiento de bases de datos, y el uso de las predicciones.

- Recopilación de información.
Hay al menos dos tipos de información necesaria:
   1. los datos estadísticos
   2. la experiencia acumulada de las personas (expertos) en los datos y que hacen las predicciones. 
Será difícil obtener suficientes datos históricos para ser capaz de encajar un buen modelo estadístico. Sin embargo, datos historicos antiguos serán menos útiles para la predicción.

- Análisis Preliminar (exploratorio)
Primero, es necesario hacer una representación gráfica de los datos. Y hacerse las siguientes preguntas:
   - ¿Existen patrones? ¿Hay una/alguna tendencia que destaque? ¿Es importante la estacionalidad? ¿Hay presencia los ciclos? ¿Hay algunas anomalías en los datos que puedan ser explicados por expertos (conocimiento experto)? ¿Qué relación tienen las variables que se estudian? 

- Elección y ajuste de modelos.
El mejor modelo a utilizar depende (en la mayoría de los casos) de la disponibilidad de datos históricos, la fuerza de las relaciones entre la variable de predicción y las variables explicativas. Cada modelo es una construcción artificial que se basa en un conjunto de supuestos (explícitos e implícitos) y por lo general consiste en uno o más parámetros que deben ser "equipados" utilizando los datos históricos conocidos. Pra ello: modelos de regresión , los métodos de suavización exponencial, los modelos de Box-Jenkins ARIMA, los modelos dinámicos de regresión, redes neuronales, y vectores autorregresivos, deeplearning, ...

- Uso y evaluación de un modelo de predicción.
Una vez que el modelo ha sido seleccionado y sus parámetros estimados, el modelo se utiliza para hacer predicciones. El rendimiento del modelo sólo puede ser evaluado correctamente después de que los datos para el período de predicción estén disponibles.

## Enfoques

### Enfoque clásico

El análisis más clásico de las series temporales se basa en la idea de que los valores que toma la variable de observación son la consecuencia de las componentes anteriores (tendencia, estacionalidad, ciclo y aleatoriedad), aunque no siempre aparecen todas. Luego este enfoque descriptivo determinista consiste en encontrar componentes que se correspondan a una tendencia a largo plazo, un comportamiento estacional y una parte aleatoria. 

El primer paso a seguir a la hora de realizar un análisis es determinar cómo se combinan los componentes de la serie. Para ello se consideran dos modelos habituales para relacionar los valores de la serie con los componentes anteriores:

- Modelo aditivo: donde cada componente contribuye al comportamiento de la variable de interés en forma aditiva (unidades).
- Modelo multiplicativo: donde cada componente contribuye al comportamiento de la variable de interés en forma multiplicativa (porcentaje).

Si a modelo multiplicativo se le aplica un logaritmo para estabilizarlo, entonces es equivalente a un modelo aditivo. Así pues, una serie temporal se puede descomponer y denotar en su manera aditiva como:

X<sub>t</sub> = T<sub>t</sub> + E<sub>t</sub> + I<sub>t</sub>

ó en su forma multiplicativa como:

X<sub>t</sub> = T<sub>t</sub> x E<sub>t</sub> x I<sub>t</sub>

donde T<sub>t</sub> es la tendencia, E<sub>t</sub> es la componente estacional, que constituyen la señal o parte
determinística, e I<sub>t</sub> es el ruido o parte aleatoria. 

Para conocer qué tipo se adapta mejor a la serie, se pueden seguir los siguientes procedimientos:

- De manera visual:
   - La tendencia y la estacionalidad se mantienen relativamente constantes -> modelo aditivo
   - La tendencia y la estacionalidad varian creciendo o decreciento -> modelo multiplicativo
- De forma matemática:
   - Calcular las series diferencia y cociente, D y C.
   - Calcular el coeficiente de variación para cada serie, CVD y CVC.
   - Comparar sendos coeficientes.
      - Si CVC < CVD -> modelo multiplicativo
      - Si CVC > CVD -> modelo aditivo

Una vez que se conoce la forma en que se relacionan los componentes, se descompone la serie estimando los componentes de tendencia y estacionalidad:

- Componente de tendencia: la forma de la tendencia se puede modelar mediante los siguientes métodos (es importante reseñar que como la finalidad del análisis no es solo describir la serie sino predecir, hay que tener en cuenta cómo continúa la tendencia estimada para valores futuros):
   - Modelos lineales
   - Modelos polinómicos
   - Filtrado (Médias móviles)(no se recomienda ya que no predice, solo describe)
   - Diferenciación (diferencias regulares)
- Componente de estacionalidad: para estimar el efecto estacional se pueden obtener los índices o coeficientes de estacionalidad que representan el valor promedio para cada elemento de la estación (si es anual para cada mes, si es trimestral cada trimestre, esto es, el periodo de la serie)(la suma de los índices estacionales debe de ser igual al número de periodos). Además, la serie original se puede desestacionalizar también mediante una diferenciación estacional de la serie.
   - Médias móviles centradas
   - Diferenciación (diferencias estacionales)

*Se puede diferenciar la parte regular (lag=1) para la tendencia y se puede diferenciar la parte estacional para la estacionalidad (lag=periodo), o ambas. Se seleccionaría aquella diferenciación que minimice la varianza, ya que en una serie sobrediferenciada la varianza aumenta.*

Y por lo tanto, la tendencia y la estacionalidad una vez modelados se pueden eliminar para obtener la componente aleatoria:

I<sub>t</sub> = X<sub>t</sub> − T<sub>t</sub> − E<sub>t</sub>

ó

I<sub>t</sub> = X<sub>t</sub> / (T<sub>t</sub> * E<sub>t</sub>)

En este punto se tiene una descomposición de la serie en componentes que separan tendencia, estacionalidad y ruido. Estos componentes obtenidos de la serie la describen, pero no la predicen. Las predicciones de valores futuros se consiguen usando las componentes T<sub>t</sub> y E<sub>t</sub> con valores de tiempo t+1, t+2, etc. Para ello se realiza un pronóstico futuro de la tendencia, y se le añade la predicción de la estacionalidad (índice de estacionalidad) correspondiente a cada periodo (la componente irregular o aleatorio no es predecible y por lo tanto no se considera).

Los métodos clásicos de análisis de series temporales tienen la ventaja de no ser excesivamente complejos, ya que explican la evolución pasada de una serie en función de pautas simples pero tienen problemas y limitaciones. Aunque son útiles para describir las pautas que sigue una serie temporal, las predicciones que proporcionan suelen ser muy malas (es decir, con un gran error asociado). La razón de esto es que en una serie temporal la observación más reciente depende, en general, de sus valores pasados, pero esta dependencia suele ser más fuerte con los datos más recientes y más débil con los más alejados. 

_VARIANZA PARA VER EL MEJOR AJUSTE?_

### Enfoque del Alisado Exponencial

_METODOS DE SUAVIZADO?_
_SERIES SIN TENDENCIA NI ESTACIONALIDAD?_ _naibe?_
_SERIES CON TENDENCIA SIN ESTACIONALIDAD?_ _holt?_
_SERIES SIN TENDENCIA CON ESTACIONALIDAD?_ _holtwinters?_

### Enfoque Box-Jenkins

Hasta ahora se han analizado las series temporales desde un punto de vista determinista o clásico, sin tener en cuenta cuál es el mecanismo que genera la serie. Pero ahora con este nuevo enfoque se analizan desde un punto de vista estocástico, por lo que el punto de partida para elaborar un modelo a partir de una serie temporal consiste en considerar que dicha serie está generada y es una realización particular de un proceso estocástico.

Pero para poder efectuar inferencias sobre los parámetros de un proceso estocástico, es preciso imponer restricciones a este último. Las restricciones que se imponen habitualmente son que sea estacionario y ergódico. Ya se ha visto cuándo un proceso es estacionario, pero ¿cúando es ergódico?. Antes de explicar un proceso ergódico necesitamos definir la autocorrelación. Se denomina autocorrelación de orden k, a la correlación (dependencia) de cualesquiera dos variables aleatorias del proceso estocástico (serie temporal), distanciadas k instantes de tiempo. La autocorrelación se puede medir mediante:

- Función de autocorrelación (ACF): esta función mide la correlación entre dos variables separadas por k periodos.
- Función de autocorrelación parcial (PACF): esta función mide la correlación entre dos variables separadas por k periodos cuando no se considera la dependencia creada por los retardos intermedios existentes entre ambas.

Las funciones de autocorrelación simple y parcial constituyen uno de los instrumentos clave para ajustar el modelo que genera una serie temporal. Así pues, un proceso es ergódico cuando conforme k se hace más grande, a autocorrelación se hace más pequeña. Es decir, que la dependencia entre variables tiene menos importancia pasado más tiempo.

Box y Jenkins desarrollaron modelos estadísticos para series temporales que tienen en cuenta la dependencia existente entre los datos, esto es, cada observación en un momento dado es modelada en función de los valores anteriores. Los modelos se conocen con el nombre genérico de ARIMA (AutoRegresive Integrated Moving Average)(ARIMA para procesos estocásticos no estacionarios y ARMA para procesos estocásticos estacionarios), que deriva de sus tres componentes Autoregresivo, Integrado y Medias Móviles:

- _Procesos Autoregresivos AR(p)_<br> Los modelos autoregresivos se basan en la idea de que el valor actual de la serie, X<sub>t</sub>, puede explicarse o predecirse en función de _p_ valores pasados X<sub>t-1</sub>, ... X<sub>t-p</sub> más un término de error, donde _p_ determina el número de valores pasados necesarios para pronosticar un valor actual. El orden del modelo expresa el número de observaciones retrasadas de la serie temporal que intervienen en la ecuación, así pues, un modelo autoregresivo de orden 1 AR(1), la variable X<sub>t</sub> está determinada únicamente por un valor pasado, esto es X<sub>t-1</sub> y así sucesivamente. Todos los procesos autorregresivos son procesos invertibles. 

- _Procesos de Medias Móviles MA(q)_<br> Un modelo de medias móviles es aquel que explica el valor de una determinada variable en un período _t_ en función de un término independiente y una sucesión de errores correspondientes a períodos precedentes, ponderados convenientemente. Estos modelos se denotan normalmente con las siglas MA, seguidos del orden entre paréntesis. Todos los procesos de medias móviles son procesos estacionarios. No todos los procesos de medias móviles son invertibles.

- _Proceso Autoregresivo de Medias Móviles ARMA(p,q)_ <br> Una extensión natural de los modelos AR(p) y MA(q) es un tipo de modelos que incluyen tanto términos autorregresivos como de medias móviles. Los modelos autorregresivos de medias móviles, ARMA(p,q), son la suma de un proceso autorregresivo de orden _p_ y uno de medias móviles de orden _q_. Es muy probable que una serie de tiempo tenga características de AR y de MA a la vez y, por consiguiente, sea ARMA. Un proceso ARMA(p, q) es estacionario si lo es su componente autorregresiva, y es invertible si lo es su componente de medias móviles.

- _Proceso Integrado I(d)_ <br> No todas las series temporales son estacionarias, algunas presentan cambios de nivel en el tiempo o la varianza no es constante, por lo que la serie se diferencia _d_ veces para hacerla estacionaria. A este tipo de procesos se les considera procesos integrados, y se puede aplicar a esta serie diferenciada un modelo ARMA(p,q) para dar lugar a un modelo ARIMA(p,d,q).

Luego un Proceso Autoregresivo Integrado y de Media Móvil, ARIMA (p,d,q), es una serie de tiempo autoregresiva integrada de media móvil. Donde denota _p_ el número de términos autoregresivos, _d_ el número de veces que la serie debe ser diferenciada para hacerla estacionaria y _q_ el número de términos de la media móvil invertible. La metodología Box-Jenkins para la construcción de los modelos ARIMA(p,d,q) se realiza de manera iterativa mediante un proceso en el que se puede distinguir cuatro etapas:

- Identificación: esta primera fase consiste en identificar el posible modelo ARIMA(p,d,q) que sigue la serie (es posible identificar más de un modelo candidato que pueda describir la serie), lo que requiere:

      - Generalmente en primer lugar se debe de representar gráficamente la serie para identificar la tendencia y la estacionalidad (si es que las hay)(la estacionalidad de puede ver con las gráficas de las subsecuencias estacionarias). Posteriormente se prosigue modelando y eliminando tanto la tendencia como la estacionalidad para conseguir la estacionariedad (sobre todo si son muy pronunciadas y se ve claramente que existen)(ya se ha explicado anteriormente cómo se estiman y modelan ambas)(también pueden no eliminarse directamente e incluirse las diferenciaciones necesarias para hacerlo en el modelo ARIMA). Aunque en el análisis y modelado de series temporales no hay un patrón fijo a seguir, lo recomendable es llegar al análisis de tendencia y estacionalidad con la serie temporal lo mas limpia posible, eliminando todo tipo de comportamientos no deseados, luego si al graficar la serie vemos claramente que es heterocedástica (no es constante en varianza)(es frecuente observar que la varianza aumenta con el nivel de la serie) es recomendable una transformación logarítmica en la serie original (o una transformación de Box-Cox).       
      - Una vez esto hecho se comprueba la estacionariedad. La condición de estacionaridad es un requisito que debe cumplirse para poder aplicar modelos paramétricos de análisis y predicción de series de datos. Pero ¿cómo saber si una serie es estacionaria?
		- Gráficamente: Representando y observando las gráficas de autocorrelación (ACF) y autocorrelación parcial (PACF). Como norma general si el ACF tiende “rápidamente” a 0 entonces es estacionaria, cuando la serie no es estacionaria, el ACF decrece lentamente a 0.	
		- Test estadísticos: Dickey-Fuller Ampliado (Test ADF). Si el valor resultante, p-value, es menor de 0.05 (ya que el p-value representa la probabilidad y se toma como hipótesis que la serie es no estacionaria) indica que la serie es estacionaria con un nivel de confianza del 95% (en caso contrario no habría evidencia para rechazar la hipótesis de no estacionariedad) 
      
      Si encontramos que la serie no es estacionaria ni habiendo eliminado la tendencia y la estacionalidad, se debe de diferenciar hasta conseguir que lo sea. La parte integrada suele ser necesaria para corregir la estacionaridad en la varianza. Normalmente los ordenes de diferenciación para que desaparezcan la tendencia o estacionalidad que quede no suelen ser elevados.      
      - Por último, una vez que la serie sea estacionaria, se determina un modelo ARIMA (o ARMA en caso de que no haga falta diferenciar) para la serie estacionaria, es decir, los órdenes p, d y q de su estructura autorregresiva, integrado y de media móvil. El modelo se puede deducir leyendo los correlogramas de la serie, es decir, las funciones de autocorrelación y autocorelación parcial (de la serie transformada, no de la original), ya que por norma general:

		- Los modelos AR tienen un ACF que decrece rápidamente a 0 (con diferentes posibles formas: regulares, sinusoidales, anternando +/-). El número del orden “p” (AR(p)) es tantos valores “distintos de 0 como haya en el PACF”.
		- Los modelos MA tiene un PACF que decrece rápidamente a 0 (con diferentes posibles formas: regulares, sinusoidales, anternando +/-)(aunque generalmente no ayuda a establecer el orden). El número del orden “q” (MA(q)) es tantos “valores distintos de 0” como haya en el ACF.
		
		Considerando un valor “distinto de cero” si no está en el rango (-2/sqrt(N), 2/sqrt(N)), con N=longitud de la serie (banda de significancia, suelen ser del 95%). Luego la identificación de los órdenes del modelo consiste en comparar la ACF y PACF estimada de la serie estacionaria con la ACF y PACF de los modelos teóricos. Hay situaciones que no son muy claras, en estos casos puede tratarse de un modelo con las dos partes, la AR y la MA. Estos son modelos ARMA(p,q) o ARIMA(p,0,q). Sus funciones FAS y FAP son combinaciones de las de ambas partes, por lo que son más difíciles de identificar a simple vista. Debido a que en la práctica es difícil identificar con exactitud el orden p y q del modelo ARMA, se pueden plantear dos o más modelos plausibles, que luego de ser estimados son útiles para la elección del más apropiado. 

- Estimación: una vez seleccionado provisionalmente un modelo para la serie estacionaria, se pasa a la segunda etapa, donde se estiman los coeficientes/parámetros de los términos autorregresivos (AR) y de media móvil (MA) del modelo (siempre el modelo estacionario, no el original). Los parámetros se suelen obtener por máxima verosimilitud o por mínimos cuadrados lineales, pero en otras ocasiones se recurre a la estimación no lineal de los parámetros, el objetivo en ambos casos es encontrar los valores de los parametros que minimizen el sumatorio de los errores al cuadrado (o dicho de otra forma, minimizar la suma de los cuadrados de los residuos).  

- Validación: en esta etapa se busca evaluar si el modelo (o los modelos) estimado(s) se ajusta(n) razonablemente bien a los datos (y si se estimaron varios modelos para conocer cuál es mejor) antes de hacer uso del modelo para la predicción. Así pues la validación o verificación incluye 3 tipos de análisis:

  	- El análisis de los coeficientes o parámetros del modelo: se desea que el modelo estimado cumpla con las condiciones de estacionariedad e invertibilidad y que exista significancia estadística en los rezagos incorporados.
	- La evaluación de la bondad del ajuste: ya que los modelos han sido elegidos mediante identificación, es importante determinar cuál de los modelos presenta una mejor bondad de ajuste.
		- AIC (Akaike Information Criterion): es una medida de la calidad relativa de un modelo estadístico que proporciona un medio para la selección de un modelo entre un conjunto de modelos. El AIC maneja un trade-off entre la bondad de ajuste del modelo y la complejidad del modelo, representando un valor más pequeño del AIC un mejor ajuste. 
		- BIC (Bayesian Information Criterion)
	- El análisis de los residuos: debe verificarse el supuesto de que los errores del modelo son un proceso puramente aleatorio (media cero, varianza constante y no correlación serial), es decir, no tienen estructura de dependencia (los residuos no deben estar correlacionados el pasado, deben ser independientes los unos de los otros) y siguen un proceso de ruido blanco.
		- Gráfico de los residuos. Se pueden pintar 4 gráficas sobre los residuos cuyo propósito es testear la validez de la asumpción de aleatoriedad de los residuos.
			- Diagrama de secuencia de los residuos: el gráfico debe de mostrar unos residuos planos y sin tendencia (media constante) y una dispersión vertical aproximadamente igual a lo largo del eje horizontal (varianza constante).
			- Lag de los residuos: los datos aleatorios no deben exhibir ninguna estructura en este ploteo. Una estructura en este ploteo indica que los residuos no son aleatorios.
			- Histograma de los residuos: el histograma realiza una sumarización gráfica de la distribución de los residuos, debe tener una forma acampanada.
			- Probabilidad de distribución normal de los residuos: chequea si los residuos están aproximadamente distribuidos normalmente, debe tener una forma aproximadamente lineal.				
			![STexample](http://www.itl.nist.gov/div898/handbook/pmc/section6/negiz4/gif/res4plot.gif)				
		- Correlograma de los residuos. Se evalúa con el correlograma que los errores del modelo son ruido blanco (la ACF y la PACF de un ruido blanco (serie de datos independientes entre sí) tienen todos los coeficientes nulos en teoría, o no significativos en las gráficas). 	
		- Los estadísticos Ljung–Box (y también Box-Pierce) (test de aleatoriedad). Si el p-value obtenido es pequeño indica que no son aleatorios, en cambio, cuanto mayor es su p-valor más evidencia hay de que los residuos son ruido blanco. El nivel mínimo es 0,05, es decir, debe suceder que el p-valor sea mayor que 0,05.
		- El histograma de frecuencias para probar que los errores del modelo siguen una distribución normal. La prueba de normalidad se efectúa con los estadísticos Jarque Bera y Shapiro-Wilk (test de normalidad)(p-value pequeño = no son de distribución normal).
		
		Para que un modelo sea válido, los diferentes análisis sobre los residuos obtenidos en la estimación serán "ruido blanco". Un ruido blanco es una serie estacionaria en la que ninguna observación depende de las otras y, por tanto, todos los valores de la ACF y la PACF son nulos. El correlograma y el correlograma parcial deben ser muy similares y los valores no deben ser significativamente distintos de cero. Si ésto no es así y los residuos muestran estructura, habrá que realizar una nueva estimación incorporando la estructura más parecida al modelo teórico que podamos intuir y se repiten las etapas anteriores hasta obtener un modelo adecuado.

- Predicción: una vez seleccionado el mejor modelo, éste se puede utilizar para conseguir la mejor predicción de los valores a futuro de la serie a partir de su propia historia. ¿Pero cuál es el mejor predictor que se puede aplicar a los datos? El mejor predictor posible será "el que menos se equivoca" o, en términos estadísticos, aquel que minimiza el error cuadrático medio respecto a otro potencial predictor alternativo. 

![STexample](./images/diagramaflujo.png)

En principio se tiene que los modelos ARIMA(p,d,q) vistos anteriormente son modelos no estacionarios (aunque también se pueden ajustar a modelos estacionarios eliminando la estacionalidad previamente como se ha visto), por ello, también existen los modelos estacionales autorregresivos integrados de média móvil ARIMA(p,d,q)(P,D,Q)<sub>S</sub> (o SARIMA(p,d,q)(P,D,Q)). Aunque la metodología para constuir estos procesos es igual, son útiles sobre todo cuando la serie de tiempo observada tiene intervalos de observación menores a un año ya que entonces es frecuente que estas tengan variaciones ó patrones sistemáticos cada cierto periodo. Esta estacionalidad debe ser captadas e incluida dentro de la estructura del modelo a construir. Este tipo de procesos tiene las siguientes características:

- Contiene una componente ARIMA(p,d,q) que modela la dependencia regular, que es la dependencia asociada a observaciones consecutivas.
- Contiene una componente ARIMA(P,D,Q) que modela la dependencia estacional, que está asociada a observaciones separadas por periodos.

En principio la parte estacional se puede modelizar de la misma forma que la parte regular. La identificación se realiza estudiando la ACF y la PACF en los retardos estacionales. Cuando una serie tiene parte regular y parte estacional, la FAS presenta una interrelación entre ellas. Básicamente la ACF y PACF de los modelos estacionales presentan las siguientes características:

- ACF: en los primeros rezagos se observa únicamente la parte regular, mientras que la parte estacional se observa en los retardos del orden del periodo (12, 24, etc.). También se puede observar la interacción entre ambos, mediante una componente regular en los retardos cercanos a los estacionales.
- PACF: nuevamente en los primeros rezagos se observa únicamente la parte regular, mientras que la parte estacional se observa en los retardos del orden del periodo (12, 24, etc.). Nuevamente se puede observar la interacción entre ambos, mediante una componente regular en los retardos cercanos a los estacionales (positiva o negativa a la derecha y positiva a la izquierda).




_¿PERIODOGRAMA?_
_¿SERIES MULTIVARIANTES?_

________________________________________________________________________

## Series temporales con R

Para que R trate a un conjunto de datos como serie temporal:

	ts(data, start, end, frequency)

		- data: vector o matrix con las observaciones
		- start: tiempo de la primera observación
		- end: tiempo de la última observación
		- frequency: número de observaciones por unidad de tiempo

Conocer el tiempo de comienzo de la serie:

	start(serie temporal)

Conocer el tiempo de fin de la serie:

	end(serie temporal)

Conocer la frecuencia de la serie:

	frequency(serie temporal)

Representación gráfica de la serie:

	plot(serie temporal) *llama internamente a plot.ts(serie temporal)*

Representación numérica de la serie:

	print(serie temporal)
   
Unidad de tiempo a la que pertenece cada observación de la serie:

	cycle(serie temporal)

Obtener un subconjunto de la serie teporal (puede ser interesante por ejemplo para pintar un año y ver si presenta componente estacional):

	window(serie temporal, start, end)

		- start: tiempo de la primera observación
		- end: tiempo de la última observación

Extraer las subseries estacionales de la serie y pintarlas todas juntas (es interesante para ver la componente estacional)(lo ideal es usarlas sin la componente tendecia):

	monthplot(serie temporal, phase)
	
		- phase: indica la estación a la que corresponde cada muestra

	boxplot(formula, serie temporal)
	
		- formula: fórmula que indica la serie temporal y su división en ciclos (es decir, a que estación o ciclo corresponde cada muestra)

Agregación de los datos de la serie temporal (puede ser interesante por ejemplo para cambiar la frecuencia de anual a trimestral)(siempre que los datos sean agregables):

	aggregate(serie temporal, FUN, nfrequency)
	
		-FUN: función con la que se computa la agregación (mean)
		-nfrequency: nuevo número de observaciones por unidad de tiempo (12->mensual, 4->trimestral, 1->anual...)

Descomposición de una serie temporal en Tendencia + Efecto estacional + Residuos:

	decompose(serie temporal, type) *mediante medias móviles*

		- type: tipo de serie, aditiva o multiplicativa
		
	stl(serie temporal, s.windows) *mediante loess*
                
        	- s.windows: establecido con "period" o con el periodo para la extracción de la estacionalidad
		
Cómputo de las estimaciones de las funciones de autocorrelación y autocorrelación parcial:

	acf(serie temporal, lag.max)
	Acf(serie temporal, lag.max)
	
	pacf(serie temporal, lag.max)
	Pacf(serie temporal, lag.max)	
	
		- lag.max: número máximo de retardos calculados

Cálculo del test de Dickey-Fuller Ampliado (Test ADF):

	adf.test(serie temporal)

Cálculo de la tendencia mediante modelos lineales y polinómicos:

    lm(formula)

        - formula: 
            - Para modelos lineales: serie temporal ~ tiempo  
            - Para modelos polinómicos: serie temporal ~ tiempo + I(tiempo^2) + ... 

Cálculo de la tendencia mediante filtrado (medias móviles):

    filter(serie temporal, filter, method, sides)
    
    	- filter: vector de coeficientes del filtro
		- method: "convolution" para usar medias móviles
		- sides: "2" para centrados
	
    decompose(sertie temporal)$trend
    
    ma(serie temporal, order, centre)
    
    	- order: orden la media móvil
	- centre: si se desea centrar la media móvil sea cual sea el orden

Eliminación de la tendencia mediante diferenciación (lag=1):

    diff(serie temporal)

Eliminación de la estacionalidad mediante diferenciación del orden del periodo:

    diff(serie temporal, lag)
    
       - lag: retardo a emplear
       
La diferenciación se puede deshacer con:

    diffinv(serie temporal, lag)

       - lag: retardo empleado
       
Cálculo del número de diferenciaciones necesarias para hacer la serie estacionaria:

    ndiffs(serie temporal, test)

		- test: tipo de test con el que se realiza el cálculo; "adf" (Augmented Dickey-Fuller), "kpss", "pp" (Phillips-Perron)

Cálculo (identificación) automático del mejor modelo ARIMA al que se ajustan los datos (se puede usar por ejemplo simplemente como una primera aproximación): 

	auto.arima(serie temporal)
	
Ajuste del modelo ARIMA sobre una serie temporal:

	arima(serie temporal, order)
	Arima(serie temporal, order)  *permite una componente de tendencia*

		- order: especificación de los tres componentes del modelo en forma de vector
		
Ajuste de un modelo autorregresivo AR sobre una serie temporal:

	ar(serie temporal)

Evaluación de la bondad del ajuste de varios modelos mediante el criterio AIC (Akaike Information Criterion) o BIC (Bayesian Information Criterion):

	AIC(object, object, ...)
	BIC(object, object, ...)

		- object: modelos de serie temporal que se quieren comparar

Test de Ljung-Box para la hipótesis de independencia: 

	Box.test(residuos modelo)

Test de normalidad con los estadísticos Jarque Bera y Shapiro-Wilk:

	jarque.bera.test(residuos modelo)
	shapiro.test(residuos modelo)

Predecir una serie temporal:

	forecast(object, h)

		- object: modelo de serie temporal
		- h: número de periodos a predecir

	predict(object, n.ahead)

		- object: modelo de serie temporal
		- n.ahead: número de periodos a predecir


_arima.sim?_
_arima estacional?_

### Paquetes R para el análisis y tratamiento de Series Temporales:

- stats: incorporado en R
- base: incorporado en R
- tseries: https://cran.r-project.org/web/packages/tseries/index.html
- forecast: https://cran.r-project.org/web/packages/forecast/index.html

## Ejemplos de análisis de Series temporales

- Ejemplo simple de análisis y modelado de series temporales sin predicción: http://www.itl.nist.gov/div898/handbook/pmc/section6/pmc62.htm
- Ejemplo simple de análisis, modelado y predicción de series temporales: http://www.itl.nist.gov/div898/handbook/pmc/section4/pmc44a.htm

- Caso ejemplo Análisis y modelado Series temporales simple 02: [Ver](./estudio_02_simple/)
- Caso ejemplo Análisis y modelado Series temporales completo 03: [Ver](./estudio_03_completo/)
- Caso ejemplo Análisis y modelado Series temporales completo 04: [Ver](./estudio_04_completo/)






________________________________________________________________________

# Bibliografía

- Introduction to time series analysis http://www.itl.nist.gov/div898/handbook/pmc/section4/pmc4.htm <br>
- Series temporales http://halweb.uc3m.es/esp/Personal/personas/jmmarin/esp/EDescrip/tema7.pdf <br>
- Series temporales http://humanidades.cchs.csic.es/cchs/web_UAE/tutoriales/PDF/SeriesTemporales.pdf <br>
- Introducción al análisis de series temporales http://halweb.uc3m.es/esp/Personal/personas/amalonso/esp/seriestemporales.pdf <br>
- Introducción al análisis de series temporales https://www.ucm.es/data/cont/docs/518-2013-11-11-JAM-IAST-Libro.pdf <br>
- Introducción a series de tiempo http://www.estadisticas.gobierno.pr/iepr/LinkClick.aspx?fileticket=4_BxecUaZmg%3D <br>
- Modelización con datos de series temporales https://www.ucm.es/data/cont/docs/518-2013-10-25-Tema_6_EctrGrado.pdf <br> 
- Series temporales: Modelo ARIMA http://www.estadistica.net/ECONOMETRIA/SERIES-TEMPORALES/modelo-arima.pdf <br>
- Modelo ARIMA https://www.uam.es/personal_pdi/economicas/anadelsur/pdf/Box-Jenkins.PDF <br>
- Análisis descriptivo de series temporales con R https://www.uam.es/personal_pdi/ciencias/acuevas/docencia/doct/Series-temporales-con-R.pdf <br>
- Análisis básico de series temporales con R https://rpubs.com/joser/SeriesTemporalesBasicas <br>
- Quick-R: Time Series http://www.statmethods.net/advstats/timeseries.html <br>
- Series temporales en R https://dl.orangedox.com/9fzOYs2ZoimR4izLVE/7-Series%20temporales%20en%20R.pdf <br>
- A Complete Tutorial on Time Series Modeling in R https://www.analyticsvidhya.com/blog/2015/12/complete-tutorial-time-series-modeling/<br>

- Análisis de series temporales https://www.youtube.com/watch?v=NDOPKRAT3-E <br>
- Análisis clásico de series temporales https://www.youtube.com/watch?v=cQxFPPIj7gc <br>
- Series temporales https://www.youtube.com/watch?v=NRtgyq3MjAs <br>
- Prácticas series temporales https://www.youtube.com/watch?v=XXu2Mbg5-Lg <br>
- Predicción con series temporales https://www.youtube.com/watch?v=XXu2Mbg5-Lg <br>
- Lectura de correlogramas https://www.youtube.com/watch?v=zpFyhbcNWIU <br>
- Modelo autorregresivo en R https://www.youtube.com/watch?v=a5QQp9peaZ4 <br>
- Introduction to ARIMA modeling in R https://www.youtube.com/watch?v=zFo7QixEKvg <br>
