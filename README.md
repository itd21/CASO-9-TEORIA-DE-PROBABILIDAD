---
title: "CASO 9 TEORIA DE PROBABILIDAD"
author: "ANGELICA ITZEL LOPEZ DELGADO"
date: "`08/03/2022`"
output: html_document
---
code_folding: hide
    toc: true
    toc_float: true
    toc_depth: 6
    number_sections: yes
bibliography: references.bib
---

# Objetivo

Desarrollar ejercicios para encontrar la probabilidad de eventos de un espacio muestral.

# Descripción

Construir ejercicios de probabilidad conforme a partir de datos conforme la teoría de probabilidad.

A partir de un conjunto de datos generados estimar y determinar las probabilidades.

# Marco teórico

Para cuando los espacios muestrales tienen un espacio finito o un número de elementos finito, la probabilidad de ocurrencia de un evento que resulta de tal experimento estadístico se evalúa utilizando un conjunto de números reales denominados pesos o probabilidades, que van de 0 a 1.[@walpole2012].

Para todo punto en el espacio muestral se asigna una probabilidad tal que la suma de todas las probabilidades es 1. [@walpole2012].

Si se tiene certeza para creer que al llevar a cabo el experimento es bastante probable que ocurra cierto punto muestral, le tendríamos que asignar a éste una probabilidad cercana a uno. Por el contrario, si se cree que no hay probabilidades de que ocurra cierto punto muestral, se tendría que asignar a éste una probabilidad cercana a cero.

En un espacio muestral en donde todos los puntos muestrales tienen la misma oportunidad de ocurrencia, por lo tanto, se les asignan probabilidades iguales.

A los puntos fuera del espacio muestral, es decir, a los eventos simples que no tienen posibilidades de ocurrir, se les asigna una probabilidad de cero.

Entonces: La probabilidad de un evento A debe estar entre cero y uno

$$
0 \le P(A) \le 1
$$

La probabilidad de todo el espacio muestral S debe ser uno $$
P(S) = 1
$$

La probabilidad de que no ocurra un evento es cero

$$
p(\phi) = 0
$$

Ejemplo: lanzar un dado. La probabilidad de que caiga un 1, un 2, un 3 un 4 un 5 un 6 es la misma para cada elemento. Siendo S el espacio muestral, cual es la probabilidad de que al lanzar un dado a una mesa, el valor del mismo cara arriba sea un 5?, y ¿cuál es la probabilidad de que sea un 7?

![](images/dado.jfif){width="300"}

¿Cuántas veces está el 5 en el espacio muestral S?. Una sola vez.

¿Cuántas veces está el 7 en el espacio muestral S?. Ninguna

Entonces dividir el número de ocurrencias del 5 entre el número total de elementos N.

$$
prob = \frac{n}{N}
$$

En términos porcentuales sería:

$$
prob = \frac{n}{N} \times 100
$$

```{r}
dado <- c(1,2,3,4,5,6)
N <- length(dado)
# N

filtro <- subset(dado, dado == 5)
filtro
n <- length(filtro)
# n

paste("La probabilidad de que al lanzar el dado sea cinco es : ", n , " de entre", N , " elementos que existen en el espacio muestral. Representa: ", round(n/N * 100,2), "%")




```

# Desarrollo

## Cargar librerías

Se cargan librerías necesarias para distintos ejercicios

```{r message=FALSE, warning=FALSE}
library(gtools) # Comnaciones ypermutaciones
library(dplyr) # Procesar datos mutate, select ...
library(fdth) # Tablas de frecuencias

```

## Ejercicios

### Lanzar dos dados:

![](images/dos%20dados.jfif){width="300"}

¿Que probabilidad existe de que al lanzar los dos dados de que salga 10 la suma de los valores de los dos dados?.

#### Crear el espacio muestral de los dados

A partir de un vector dado del 1 al 6 que son los valores del dado generar permutaciones en donde se puedan repetir los valores del dado.

Poner nombre con la función *names()* nombres de columnas al conjunto de datos *lanzar_dados.*

Con la función *cbind()* se agrega una columna al conjunto de datos.

Con *apply()* se hace la suma de cada renglón del conjunto de datos *lanzar_dados*.

```{r}
dado <- c(1,2,3,4,5,6)

lanzar_dados <- data.frame(permutations(n=6, r = 2, v = dado, repeats.allowed = TRUE))

names(lanzar_dados) <- c("dado1", "dado2")

lanzar_dados <- cbind(lanzar_dados, suma = apply(X = lanzar_dados, MARGIN = 1, FUN = sum))

lanzar_dados

```

#### Probabilidad de dos dados

Encontrar en cuantas ocasiones la suma de los dos dados es diez, se hace con la función *subset()*

```{r}
sumados <- 10 # Puede ser cualquier valor
N <- nrow(lanzar_dados) # Cantidad de obervaciones
filtro <- subset(lanzar_dados, suma == sumados)
filtro

n <- nrow(filtro) # Cantidad de eventos que cumplen una condición
n


paste("Existen ", n, " alternativas de que la suma de lanzamiento de dos dados sea ", sumados, " de un total de ",N, " lo que representa ", round(n/N * 100,2), "%", "probable ")
```

### Juego de Black Jack

Se reparten dos barajas de tipo inglesa y el jugador debe sumar los valores numéricos de las dos barajas.

La pregunta es: ¿qué probabilidad existe de que al recibir dos cartas de una baraja de 52 cartas modalidad inglesa la suma de las dos cartas sea 20?

-   El As vale 1 punto

-   Los valores numérico valen lo que indica la carta

-   Los monos (J, Q y K ) valen 10 puntos

![](images/barajas.jpg){width="350"}

#### Simular las cartas ...

Reutilizar código que existe en "<https://raw.githubusercontent.com/rpizarrog/Probabilidad-y-EstadIstica-VIRTUAL-DISTANCIA/main/funciones/misfunciones.R>"

```{r message=FALSE, warning=FALSE}
# source("funciones/mis.funciones.r")
source ("https://raw.githubusercontent.com/rpizarrog/probabilidad-y-estad-stica/master/Enero%20Junio%202022/funciones/misfunciones.R")

```

#### Espacio muestral sin sumas

El espacio muestral de todas las cartas almacenada en una variable llamada S.casos.

```{r}
S.casos <- data.frame(permutations(13,2,baraja, repeats.allowed = TRUE))
names(S.casos) <- c("C1", "C2")
S.casos



```

Total de casos del espacio muestral:

```{r}
N <- nrow(S.casos) # El número de opciones
N   

```

#### Espacio muestral con sumas

Determinar columna para suma de las dos cartas

```{r message=FALSE, warning=FALSE}
S.casos <- f.sumar.cartas(S.casos)
S.casos

```

Nuevamente la pregunta es: ¿qué probabilidad existe de que al recibir dos cartas de una baraja de 52 cartas modalidad inglesa la suma de las dos cartas sea 20?

```{r}
sumados <- 20
filtro <- subset(S.casos, suma == sumados)
n <- nrow(filtro)
filtro
```

```{r}
paste("De las ", N, "alternativas, ", " existe ", n, " posibilidades de que la suma de las dos cartas repartidas sea", sumados, " ,que representa el ", round(n/N * 100, 2), "%")
```

### Ruleta

La ruleta tiene 39 números en colores negro y rojo ¿que probabilidad existe de que al dar vuelta se detenga en un valor en específico?

![](images/ruleta.jfif){width="300"}

```{r}
numeros <- 1:36
colores <- c("Negro", "Rojo")

S.ruleta <- c(paste(as.character(1:36), "Rojo"), 
              paste(as.character(1:36), "Negro"))

S.ruleta
```

¿Cuál es la la probabilidad de que al darle vuelta la ruleta se detenga en un valor específico es por ejemplo en la casilla "20 Negro".

```{r}
N <- length(S.ruleta)
n <- 1

paste ("La probabilidad de que caiga un valor en la ruleta de ", N , " alternativas es: ", round(n/N * 100, 2), "%")
```

### Dominó

El juego de dominó consiste en que de una cantidad de 28 fichas se reparten siete de ellas a cada jugador.

Uno de los variantes del dominó es contar los puntos de cada ficha, siendo los puntos la cantidad de puntos negros que tiene cada ficha.

Para este ejercicio se pide:

¿Cual es la probabilidad de que la suma de puntos de las siete fichas repartidas sea manor a 15 puntos?

¿Cuál es la probabilidad de que la suma de los puntos de las siete fichas sea mayor a 60 puntos?

¿Cual es la probabilidad de que al repartir siete fichas de dominó la suma total esté 30 y 40 puntos?. Siendo los puntos los puntos negros de cada ficha?.

¿Cual será el rango o intervalo de clase conforme a la suma de puntos existe mayor probabilidad de obtener esos puntos?

#### Espacio muestral del dominó

Primero se construye el espacio muestral a partir de funciones ya preparadas que se encuentran en la dirección <https://github.com/rpizarrog/Probabilidad-y-EstadIstica-VIRTUAL-DISTANCIA/blob/main/funciones/funciones.domino.r>

```{r}
#source("funciones/funciones.domino.r")

source("https://raw.githubusercontent.com/rpizarrog/probabilidad-y-estad-stica/master/Enero%20Junio%202022/funciones/funciones.domino.r")



```

Se muestra sólo las primeras 20 observaciones y las últimas 20 de todas las posibles combinaciones de siete fichas en siete fichas.

El campo suma es la cantidad de puntos de las siete fichas.

```{r}
head(fichas, 20)
tail(fichas, 20)


```

Se determina la cantidad de combinaciones posibles en grupos de siete fichas de dominó

#### Cantidad de combinaciones

```{r}
N <- nrow(fichas)
N

# Se puede usar fórmua de combinaciones
f.n.combinaciones(28,7)

```

#### Repartir fichas

Se pueden repartir siete fichas a partir de una simulación.

![](images/siete%20fichas.jfif){width="300"}

```{r}
mis.fichas <- f.repartir.fichas.domino(fichas)
mis.fichas

```

Para describir 1184040 de registros lo mejor es representarlo con un histograma utilizado la variable de interés suma de las fichas.

#### Histograma de suma de puntos

```{r}
hist(fichas$suma, main="Puntos en fichas de dominó", xlab = "Suma")

```

#### Tabla de frecuencias

Y se puede construir clases por medio de la función *fdt()* para determinar tablas de frecuencia

```{r}
tabla <- fdt(x = fichas$suma, start = 15, end =75, h = 5)
tabla

```

#### Probabilidades de puntos en el dominó

¿Cual es la probabilidad de que la suma de puntos de las siete fichas repartidas sea menor o igual a 15 puntos?

```{r}
filtro <- filter(fichas, suma <=15)
filtro
n<-nrow(filtro)

paste("Existe ", n, "eventos en donde los puntos sumados de fichas de dominó es menor o igual a 15, de un total de ", N , " alternativas. Lo que representa una probabilidad del ", round(n/N*100,4),"%")

```

¿Cuál es la probabilidad de que la suma de los puntos de las siete fichas sea mayor a 60 puntos?

```{r}
filtro <- filter(fichas, suma > 60)
head(filtro)
n<-nrow(filtro)

paste("Existe ", n, "eventos en donde los puntos sumados de fichas de dominó es mayor a 60, de un total de ", N , " alternativas. Lo que representa una probabilidad del ", round(n/N*100,4),"%")
```

¿Cual es la probabilidad de que al repartir siete fichas de dominó la suma total esté 30 y 40 puntos?. Siendo los puntos los puntos negros de cada ficha?.

```{r}
filtro <- filter(fichas, suma >= 30 & suma <= 40)
head(filtro)
n<-nrow(filtro)

paste("Existe ", n, "eventos en donde los puntos sumados de fichas de dominó son mayor o igual a 30 y menor o igual a 40, de un total de ", N , " alternativas. Lo que representa una probabilidad del ", round(n/N*100,4),"%")


```

# Interpretación

¿Cual será el rango o intervalo de clase conforme a la suma de puntos de las siete fichas repartidas de dominó en donde existe mayor probabilidad de obtener esos puntos?

```{r}
print ("Describir habiendo analizado la tabla de frecuencia de la suma de puntos de las siete fichas de dominó")

```

¿Cómo se determina probabilidad de eventos de un espacio muestral, y que valores puede tener una probabilidad?
La problabilidad de eventos se determina dividiendo la cantidad de eventos entre los resultados posibles, y los valores que puede tener una probabilidad van del 0 al 1 donde 0 es una probabilidad nula.

¿Para que sirve estimar probabilidades?
Para tener alguna idea de la posibilidad de que algo ocurra, para así tratar de tomar una decision de alguna manera más acertada.

¿Podrá haber probabilidades negativas?, justifique SI o NO ?
 No, ya que desde el momento que hablamos de una existencia el valor será positivo si no no existiría.

Describa y justifique su respuesta sobre que es más probable de estas tres cuestiones:

-   ¿Que salga águila al lanzar una moneda
50% ya que existe 1 evento de águila y los resultados posibles son 2, esto nos da .5 de probabilidad que representa en 50%.

-   ¿Que la suma de los puntos de dos cartas repartidas de baraja esté entre 8 y 12.
"De las  169 alternativas,   existe  55  posibilidades de que la suma de las dos cartas repartidas esté entre 8 y 12 que representa el  32.54 %"

-   ¿Que la suma de los puntos de las siete fichas de dominó repartidas esté entre 30 y 50 puntos?
"Existe  1004092 eventos en donde los puntos sumados de fichas de dominó son mayor o igual a 30 y menor o igual a 40, de un total de  1184040  alternativas. Lo que representa una probabilidad del  84.8022 %"

-   Que es mas probable que salga un 10 rojo en la ruleta o que la suma de dos cartas sea 10?
"De las  72 alternativas,   existe 1 posibilidad de que salga un 10 rojo en la ruleta  1.39 %"
"De las  169 alternativas,   existe  9  posibilidades de que la suma de las dos cartas repartidas sea 10 que representa el  5.33 %"
Es más probable que la suma de dos cartas sea 10 ya que esto representa 5.33% de probabilidad mientras que la probabilidad de que salga un 10 rojo en la ruleta es 1.93%

-   Que es más probable que la suma de dos cartas sea menor a 15 o que la suma de siete fichas sea menor a 50 puntos?
"De las  169 alternativas,   existe  103  posibilidades de que la suma de las dos cartas repartidas sea menor a 15 que representa el  60.95 %"
"Existe  1011499 eventos en donde los puntos sumados de fichas de dominó son menor a 50 puntos, de un total de  1184040  alternativas. Lo que representa una probabilidad del  85.4278 %"
Es más probable que la suma de siete fichas sea menor a 50 puntos ya que según los cálculos tiene una probabilidad de 85.4278% y la suma de dos cartas sea menor a 15 tiene un 60.95% de probabilidad.


-   Qué es más probable: que salga en la suma un valor entre 8 y 12 en dos barajas o que salga un valor de suma entre 20 y 30 puntos en siete fichas de dominó?
"De las  169 alternativas,   existe  55  posibilidades de que la suma de las dos cartas repartidas esté entre 8 y 12 que representa el  32.54 %"
"Existe  59561 eventos en donde los puntos sumados de fichas de dominó sea entre 20 y 30 puntos, de un total de  1184040  alternativas. Lo que representa una probabilidad del  5.0303 %"
Es más probable que salga en la suma de dos cartas valores entre 8 y 12 ya que tiene 32.54% de probabilidad, mientras que la suma de puntos de fichas entre 20 y 30 solo tiene un 5.0303% de probabilidades.

¿A qué conclusiones llegan respecto a los cuatro ejercicios vistos en este caso?.
Aunque tener una probabilidad alta de que ocurra algún evento no nos da la certeza de que ocurrirá primero que algún otro evento con menor probabilidad, simpre es bueno saber que acción puede ocurrir en más ocasiones, no sabemos cuando pero si que puede ocurrir más veces que otras cosas.

# Bibliografía
