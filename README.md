# Etapa-1-Proy-Ing-Caract
## Descargando datos de la web

Mandamos llamar nuestras librerías:
```{r}
library(httr)
library(jsonlite)
library (magrittr)
library (dplyr)
library(tidyr)
```

Descargamos la base de datos de nivel educativo por año y municipio de Sonora:
```{r}
#Mandamos llamar el URL que se saca desde la pagina de Data Mexico:

educacion.url = GET("https://api.datamexico.org/tesseract/data.jsonrecords?State=26&cube=inegi_enoe&drilldowns=Municipality%2CYear%2CInstruction+Level&locale=es&measures=Number+of+Records%2CMonthly+Wage")

#Convertimos nuestros datos en “string”:

rawToChar(educacion.url$content)

#Nuestros datos vienen en lenguaje JSON, por lo que debemos convertirlos para que sean mas manejables:

educacion = fromJSON(rawToChar(educacion.url$content))
names(educacion)

#Pedimos que solo nos de la data de la variable "Datos" que declaramos anteriormente:

educacion<-educacion$data

educacion <- educacion[, -7]

```

Siguiendo la misma metodología, descargamos la base de datos de tipos de crímenes por año y municipo de Sonora:

```{r}

crimenes.url = GET("https://api.datamexico.org/tesseract/data.jsonrecords?State=26&cube=sesnsp_crimes&drilldowns=Municipality%2CYear%2CCrime+Type&locale=es&measures=Value")

rawToChar(crimenes.url$content)

crimenes = fromJSON(rawToChar(crimenes.url$content))
names(crimenes)

crimenes<-crimenes$data

```

Ya que el fin del proyecto va mas enfocado a dar énfasis al nivel educativo que al tipo de crimen, agrupamos todos los tipos de crímenes por municipio y año:
```{r}
crimenesxaño <- crimenes %>% group_by(Municipality, Year)
```

Una vez agrupados lo que queremos visualizar es la sumatoria del número de crímenes:
```{r}
dfcrimenes <- crimenesxaño %>% summarise(
  NumCrimenes = sum(Value) 
  )
```

En la base de datos de educación hay registros de personas que desconocen su nivel educativo, por lo que filtramos los niveles para eliminarlos: 
```{r}
colnames(educacion)[4] = "LevelID"
educacion <- educacion[educacion$LevelID %in% c("0","1","2","3","4","5","6","7","8"),]

```

El sistema educativo de México está compuesto por tres tipos de enseñanza, básica (preescolar, primaria y secundaria), media superior (preparatoria y normal) y superior (carrera técnica, profesional y maestría), por ello se agrupan los datos bajo estos tres tipos de enseñanza, así como creando un grupo para las personas sin estudios:  
```{r}
educacion2 <- educacion %>% mutate(Nivel=
                     case_when(LevelID%in% "0" ~ "NumPersonasSinEstudios",
                               LevelID %in% c("1", "2", "3") ~ "NumPersonasEduBasico",
                               LevelID %in% c("4", "5") ~ "NumPersonasEduMediaSuperior",
                               LevelID %in% c("6", "7", "8") ~ "NumPersonasEduSuperior"

                               ))
```

Ya que el fin del proyecto va mas enfocado a dar énfasis al nivel educativo que al tipo de crimen, agrupamos todos los tipos de crímenes por municipio y año:
```{r}
educacionxaño <- educacion2 %>% group_by(Municipality, Year, Nivel)
```

De la misma manera que la base de datos de crímenes, agrupamos por municipio y año:
```{r}
dfeducacion <- educacionxaño %>% summarise(
  numpersonas = sum(`Number of Records`)
  )
```
