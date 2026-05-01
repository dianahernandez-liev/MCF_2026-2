# MCF_2026-2
Proyecto de Métodos Cuantitativos en Finanzas

Se analizan series históricas de precios de acciones obtenidas desde Yahoo Finance, transformadas en rendimientos diarios, con el objetivo de estudiar sus propiedades estadísticas y evaluar distintas metodologías de medición de riesgo.

El enfoque del proyecto está basado en herramientas de gestión cuantitativa del riesgo, donde se modela la distribución de pérdidas y se calculan medidas como Value at Risk (VaR) y Expected Shortfall 

Los datos utilizados corresponden a precios de cierre ajustados de las siguientes acciones:
- AAPL (Apple)
- MSFT (Microsoft)
- GOOGL (Alphabet)
- TSLA (Tesla)
- AMZN (Amazon)

Los datos son obtenidos mediante la librería `yfinance`, con un rango temporal desde 2010 hasta la fecha actual.

A partir de los precios, se calculan rendimientos simples diarios ; Este tipo de rendimiento es adecuado para análisis descriptivo y coincide con la implementación del código.

Segun lo que vimos en todo el semestre hasta ahora, com los rendimientos debiamos de ser muy cuidadosos debido a sus distribuciones pues los rendimiento tienen kas siguienres propiedades:
- No siguen una distribución normal
- Presentan colas pesadas
- Pueden ser asimétricos

- Nuestro codigo lo que hace es verificar todo esto con:
- ### Media
Representa el rendimiento promedio diario. Sin embargo, en gestión de riesgo tiene menor relevancia que las medidas de dispersión.

### Sesgo 

El sesgo mide la asimetría de la distribución:

\[

\beta = \frac{E[(X - E[X])^3]}{(E[(X - E[X])^2])^{3/2}}

\]
- Sesgo positivo: cola derecha más pesada (ganancias extremas)
- Sesgo negativo: cola izquierda más pesada (pérdidas extremas)

### Curtosis (Kurtosis)
La curtosis mide la concentración y el peso de las colas:
\[

\kappa = \frac{E[(X - E[X])^4]}{(E[(X - E[X])^2])^2}

\]
- Normal: κ = 3
- κ > 3: colas pesadas
En mercados financieros segun lo vimos con el profesor sucede que :

> Cuando la curtosis es alta, lo que implica mayor probabilidad de eventos extremos.

## Análisis de distribución

Se utilizan histogramas de las acciones a elegir para aproximar la distribución empírica de los rendimientos.
Estos permiten observar que:
- La distribución es más picuda que la normal
- Existen colas más gruesas lo cual no es bueno para la distribucion normal
- Hay presencia de valores extremos

Esto confirma que asumir normalidad es una simplificación fuerte.

## Definición de riesgo

El riesgo lo interpretamos como

> La posibilidad de obtener resultados negativos o pérdidas.

En este proyecto se estudia principalmente el **riesgo de mercado**, asociado a fluctuaciones en los precios de los activos

## Modelación del riesgo

El proyecto implementa tres enfoques principales para estimar la distribución de pérdidas:

### 1. Método paramétrico

Se asume una distribución teórica (normal o t-Student) y se estiman sus parámetros.

-  La ventaja mas importante que tiene es la Simplicidad computacional que tiene 
- La desventaja de este metodo es que genera supuestos fuertes sobre la distribución

### 2. Método histórico

Utiliza directamente la distribución empírica de los datos.
- La ventaja es que no requiere supuestos
- La desventaja es que No captura eventos no observados

### 3. Simulación Monte Carlo

Se generan escenarios simulados a partir de una distribución ajustada, en nuestro codigo lo hicimos para un numero ni tan grande ni tan pequeño, para obtener mejores resultados

- La ventaja con la simulación de Monte carlo es la Flexibilidad que presenta 

- La desventaja de utilizar esta simulacion es que genera una Dependencia del modelo asumido


## Medidas de riesgo que utilizamos

### Varianza

\[

Var(L) = E[(L - E[L])^2]

\]

- El problema con la varianza es que es simétrica (no distingue pérdidas de ganancias) esto es una cita de las notas del profesor

### Value at Risk (VaR)

El VaR es el cuantil de la distribución de pérdidas segun como lo vimos en clase

\[

VaR_\alpha(L) = q_\alpha(L)

\]

> Es la pérdida máxima esperada con nivel de confianza α.

- VaR 95% = -2% significa que el 95% de las pérdidas no superan 2% este es un ejemplo claro de como funciona el VaR


### Expected Shortfall 

\[

ES_\alpha(L) = E[L \mid L \geq VaR_\alpha]

\]

Es la pérdida promedio en los peores escenarios.

Propiedad importante:

\[

ES_\alpha \geq VaR_\alpha

\]


### Distribución Normal

- Subestima el riesgo extremo

- No captura colas pesadas
Segun lo que vimos en clase y con el profesor y lo podemos ver en los graficos, la distribucion normal no es la adecuada para realizar este tipo de analisis.

---
Distribución t-Student

- Captura colas pesadas

- Más adecuada para datos financieros

Como lo vimos en clase esta era la distribucion mas adecuada para realizar debido a que no subestimabamos los eventos aleatorios extremos donde se podrian presnert o bien ganancias extremas o tambien perdidas extremas

## Rolling Window

Se implementa un enfoque dinámico usando ventanas móviles de 252 días.

Esto permite:

- Capturar cambios en volatilidad

- Adaptar el riesgo en el tiempo


## Backtesting (Violaciones)

Se evalúa la calidad del modelo mediante:

\[

\text{Violación} = \{R_t < VaR_t\}

\]

Para un nivel de confianza α:

- Esperado ≈ (1 - α)

Ejemplo:

- 95% → 5% violaciones

Criterio utilizado:

Un modelo es adecuado si presenta menos del 2.5% de violaciones, pero como lo vimos en la reunion con Jonathan esto no necesariamente era cierto o significa que nuestro modelo estaba mal.

---

## Resultados e interpretación

Los resultados muestran que:

- Los rendimientos no son normales

- Existe curtosis elevada (riesgo extremo)

- El VaR paramétrico tiende a subestimar el riesgo

- El ES es una medida más robusta

- La distribución t-Student es más realista

- El riesgo es dinámico (no constante)

---

## Conclusiones

- El supuesto de normalidad no es adecuado para rendimientos diarios

- Es necesario modelar colas pesadas

- El VaR es útil pero incompleto

- El ES es una mejor medida de riesgo

- El uso de rolling windows mejora la estimación
- El backtesting es fundamental para validar modelos

---
