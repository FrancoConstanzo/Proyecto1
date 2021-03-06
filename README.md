###Proyecto 1

```{r}
data = read.csv("sanguchez.csv" , sep = ";")
```


##Preparaci?n

1. Agregamos los packages
2. LLamamos a la librer?a

```{r}
library(dplyr)
library(Select)
library(stringr)
library(tidyr)
library(ggplot2)
```

3. Importamos la base de datos

Ya teniendo la base de datos importada, debemos comenzar a desarrollar el problema.

#Paso1
Filtramos la base con las filas que contienen la nota = 5

```{r}
nota5 <- filter(data, nota == 5)
```

#Paso2
Usamos la funci?n "separate" para poder separar las palabras con el separador de espacio.
Luego seguidamente usamos la funci?n "group_by" para agrupar, contando todas las palabras en minuscula. 
Finalmente con la funci?n "summarise" cuento la cantidad de veces que existe cada palabra.

```{r}
Paso2 <- nota5 %>% 
  separate_rows(Ingredientes, sep = " ") %>% 
  group_by(Ingredientes = tolower(Ingredientes)) %>% 
  summarise(Cantidad = n())
```


#Paso3
En este paso, lo que hacemos es filtrar el objeto del paso2 para una cantidad mayor a 4, esto se debe a que queremos filtrar los "ingredientes" que no son tan usados, o que se usan solo 1 vez.


```{r}
Paso3 <- Paso2 %>% filter(Cantidad>4)

```


#Paso4
En el siguiente paso, pedimos que solo se mantengan los ingredientes que tengan el numero de car?cteres mayores a 2, esto es para eliminar conceptos como "y", "de", "en", etc.

```{r}
Paso4 <- Paso3[nchar(Paso3$Ingredientes) > 2, ]
```


#?ltimo paso
Se grafica las palabras que mas se repiten
Por ?ltimo usamos la funcion de ggplot para graficar el resultado obtenido en el paso4, que nos muestra los ingredientes mas demandados para las recetas con hamburguesas con mejor feedback y nota. (usamos coord_flip para invertir el gr?fico de barras).

```{r}
ggplot(data = Paso4, aes(x = Ingredientes, y = Cantidad )) + geom_bar(stat = 'identity') + coord_flip()
```

Finalmente, considerando que tenemos distintos tipos de recetas en donde sus ingredientes varian en cantidad entre 4 y 9.

Los ingredientes para asegurar una receta de un s?ndwich con buena calificaci?n son los siguientes:
      - Hamburguesa
      - Queso Cheddar
      - Cebollas caramelizada
      - Tomate
      - Palta
      - Tocino
      - Champi?on
      - Mayonesa
      - Salsa
      
  PD: Ser?a una bomba, pero sin duda un sandwich con excelente calificaci?n. 
