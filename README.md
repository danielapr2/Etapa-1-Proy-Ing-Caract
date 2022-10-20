# Etapa-1-Proy-Ing-Caract
## Descargando datos de la web

### Nivel de educación y pobreza: ¿nexos de la violencia en México?
### Ana Daniela Pérez Romero

El objetivo de este proyecto es examinar la influencia de la educación y la pobreza sobre la delincuencia en México. Actualmente, el país está atravesando por una enorme crisis de violencia social; probablemente la más grande de su historia, por ello, esta situación ha incitado que la población quiera encontrar explicaciones, incluyéndome. La más socorrida es la que señala a la falta de oportunidades educativas y a la pobreza como los orígenes de estos conflictos; ¿una sociedad pobre pero más educada tendría una tasa delictiva más baja?


El principal vínculo teórico entre el aumento de la educación y el comportamiento criminal es bastante directo: la educación aumenta las oportunidades de trabajos legítimos y sus salarios, lo que reduce el atractivo financiero de las actividades delictivas. Es conocido que los criminales son racionales y buscan maximizar su bienestar, es decir, miden en términos monetarios los incentivos de realizar actividades legales versus ilegales. 


Si bien desde el punto de vista teórico podemos identificar varios canales a través de los cuales la educación afecta al crimen, abordar empíricamente esta relación resulta más difícil. La causalidad que usualmente se predice va desde la educación al comportamiento criminal, es decir, una persona con poca educación es más propensa a cometer delitos en comparación con una persona con mayor educación. Sin embargo, también debemos considerar que una persona con una propensión mayor a cometer delitos es menos probable que permanezca en la escuela en comparación con una persona que no es propensa a cometerlos.


Para esta primera etapa de investigación, se hará la descarga de datos que nos ayuden a revisar las relaciones entre educación, pobreza y delincuencia. Analizaremos específicamente la situación del estado de Sonora para entender el comportamiento de nuestro estado y sus municipios.


----------------------------------------------------------------------------------------------------------------------------------------------

Mandamos llamar nuestras librerías:

```{r}
library(httr)
library(jsonlite)
library (magrittr)
library (dplyr)
library(tidyr)
library(vtable)
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

colnames(educacion)[1] = "MunicipalityID"

colnames(educacion)[4] = "LevelID"

educacion

```

**Definición de nuestras variables:**

**MunicipalityID** y **Municipality**: se relacionan directamente, la primera nos da el código y la segunda el nombre del municipio.

26001	Aconchi

26002	Agua Prieta

26003	Alamos

26004	Altar

26005	Arivechi

26006	Arizpe

26007	Atil

26008	Bacadéhuachi

26009	Bacanora

26010	Bacerac

26011	Bacoachi

26012	Bácum

26013	Banámichi

26014	Baviácora

26015	Bavispe

26016	Benjamín Hill

26017	Caborca

26018	Cajeme

26019	Cananea

26020	Carbó

26021	La Colorada

26022	Cucurpe

26023	Cumpas

26024	Divisaderos

26025	Empalme

26026	Etchojoa

26027	Fronteras

26028	Granados

26029	Guaymas

26030	Hermosillo

26031	Huachinera

26032	Huásabas

26033	Huatabampo

26034	Huépac

26035	Imuris

26036	Magdalena

26037	Mazatán

26038	Moctezuma

26039	Naco

26040	Nácori Chico

26041	Nacozari de García

26042	Navojoa

26043	Nogales

26044	Ónavas

26045	Opodepe

26046	Oquitoa

26047	Pitiquito

26048	Puerto Peñasco

26049	Quiriego

26050	Rayón

26051	Rosario

26052	Sahuaripa

26053	San Felipe de Jesús

26054	San Javier

26055	San Luis Río Colorado

26056	San Miguel de Horcasitas

26057	San Pedro de la Cueva

26058	Santa Ana

26059	Santa Cruz

26060	Sáric

26061	Soyopa

26062	Suaqui Grande

26063	Tepache

26064	Trincheras

26065	Tubutama

26066	Ures

26067	Villa Hidalgo

26068	Villa Pesqueira

26069	Yécora

26070	General Plutarco Elías Calles

26071	Benito Juárez

26072	San Ignacio Río Muerto

**Year**: es el años de registro.

**LevelID** e **Instruction Level**: se relacionan directamente, la primera nos da el código y la segunda el nombre del nivel educativo. Los niveles de ID van de menor a mayor.

0-Ningun nivel de educación

1-Preescolar

2-Primaria

3-Secundaria

4-Preparatoria

5-Normal

6-Carrera Técnica

7-Profesional

8-Maestría



Observamos la metadata de nuestros datos y confirmamos que no hay valores nulos:

```{r}
vtable(educacion)
countNA(educacion)
```

Siguiendo la misma metodología, descargamos la base de datos de tipos de crímenes por año y municipo de Sonora:

```{r}

crimenes.url = GET("https://api.datamexico.org/tesseract/data.jsonrecords?State=26&cube=sesnsp_crimes&drilldowns=Municipality%2CYear%2CCrime+Type&locale=es&measures=Value")

rawToChar(crimenes.url$content)

crimenes = fromJSON(rawToChar(crimenes.url$content))
names(crimenes)

crimenes<-crimenes$data

colnames(crimenes)[1] = "MunicipalityID"

crimenes

```


**Definición de nuestras variables:**

**MunicipalityID** y **Municipality**: se relacionan directamente, la primera nos da el código y la segunda el nombre del municipio.

**Year**: es el años de registro.

**Cryme Type ID** y **Cryme Type**: se relacionan directamente, la primera nos da el código y la segunda el nombre del tipo de crimen.

101- Abuso de Confianza

102- Daño a la Propiedad

103- Despojo

104- Extorsión

105- Fraude

106- Robo

107- Otros Delitos contra el Patrimonio

201- Incumplimiento de Obligaciones de Asistencia Familiar

202- Violencia de Genero en Todas sus Modalidades Distinta a la Violencia Familiar

203- Violencia Familiar

204- Otros delitos contra la familia

301- Abuso Sexual

302- Acoso Sexual

303- Hostigamiento Sexual

304- Incesto

305- Violación Equipada

306- Violación Simple

307- Otros Delitos que Atenta contra la Libertad y la Seguridad Sexual

401- Corrupción de Menores

402- Trata de Personas

403- Otros Delitos contra la Sociedad

501- Aborto

502- Feminicidio

503- Homicidio

504- Lesiones

505- Otros Delitos que atentan contra la Libertad Personal

601- Rapto

602- Secuestro

603- Tráfico de Menores

604- Otros delitos que Atentan contra la libertad personal

701- Allanamiento de Morada

702- Amenazas

703- Contra el Medio Ambiente

704- Delitos Cometidos por Servidores Públicos

705- Electorales

706- Evasión de Presos

707- Falsedad

708- Falsificación

709- Narcomenudeo

710- Otros Delitos del Fuero Común


Observamos la metadata de nuestros datos y confirmamos que no hay valores nulos:

```{r}
vtable(crimenes)
countNA(crimenes)
```

Anteriormente, observamos que la base de datos de educación iba del 2010 al 2022, sin embargo, los registros de la base de datos de crímenes comenzaban a partir del 2015; por ello, se filtra la información para que los años de ambas bases de datos coincidas:

```{r}
educacion <- educacion[educacion$Year %in% c("2015","2016","2017","2018","2019","20202","2021","2022"),]
vtable(educacion)
```

Ya que el fin del proyecto va mas enfocado a dar énfasis al nivel educativo que al tipo de crimen, agrupamos todos los tipos de crímenes por municipio y año:

```{r}
crimenesxaño <- crimenes %>% group_by(MunicipalityID, Year)
```

Una vez agrupados lo que queremos visualizar es la sumatoria del número de crímenes:

```{r}
dfcrimenes <- crimenesxaño %>% summarise(
  NumCrimenes = sum(Value) 
  )
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

Agrupamos nuestros datos por municipio, año y nivel de estudio:

```{r}
educacionxaño <- educacion2 %>% group_by(MunicipalityID, Year, Nivel)
```

Sumamos el numero de personas por cada uno de nuestros niveles educativos:

```{r}
dfeducacion <- educacionxaño %>% summarise(
  numpersonas = sum(`Number of Records`)
  )
```

Ya que debemos juntar las dos bases de datos, es mejor que se separen los niveles de educación por columna, en lugar de por renglón:

```{r}
dfeducacion <- pivot_wider(dfeducacion, names_from = Nivel, values_from = numpersonas)
```

Sabiendo que ambas bases de datos cuentan con registros de municipios y años, realizamos nuestra unión basada en estas dos columnas:

```{r}
dfproyecto <- merge(dfeducacion,dfcrimenes, by = c("Year", "MunicipalityID"))
dfproyecto[is.na(dfproyecto)] <- 0
```

---------------------------------------------------------------------------------------------------------------------------------------------------
Ya que estamos haciendo referencia a la tasa de pobreza en el estado, descargamos dos bases de datos de Pobreza a escala municipal, que se hace cada 5 años por la CONEVAL. Descargamos los datos del 2015 y 2020 ya que son años que se encuentran en nuestras bases de datos de crímenes y nivel educativo:

```{r}
pobreza2015 <- read.csv('https://www.coneval.org.mx/Informes/Pobreza/Datos_abiertos/pobreza_municipal_2010-2020/indicadores%20de%20pobreza%20municipal_2015.csv')
pobreza2020 <- read.csv('https://www.coneval.org.mx/Informes/Pobreza/Datos_abiertos/pobreza_municipal_2010-2020/indicadores%20de%20pobreza%20municipal_2020.csv')

pobreza2015 <- pobreza2015[pobreza2015$clave_entidad %in% c("26"),]
pobreza2020 <- pobreza2020[pobreza2020$clave_entidad %in% c("26"),]

```

Estos son estudios muy específicos, que nos muestran diferentes indicadores que clasifican a una persona en estado de pobreza; entre ellos, se toma en cuenta los accesos a servicios de salud, seguridad social, calidad y espacios de la vivienda, servicios básicos en la vivienda, alimentación y otros más. Para este análisis, me enfocare solamente en el porcentaje de pobreza por municipio, el cual, se calcula tomando en cuenta el numero total de habitantes entre el número de habitantes que entran a la clasificación de pobreza:

```{r}
pobreza2015 <- pobreza2015[, -c(1:2, 7:36)]
pobreza2020 <- pobreza2020[, -c(1:2, 7:36)]

pobreza2015 <- pobreza2015[, -c(3,5)]
pobreza2020 <- pobreza2020[, -c(3,5)]

colnames(pobreza2015)[1] = "Municipality ID"
colnames(pobreza2020)[1] = "Municipality ID"

colnames(pobreza2015)[3] = "PcgPobreza2015"
colnames(pobreza2020)[3] = "PcgPobreza2020"

```


Finalmente, juntamos nuestros dos data frames y calculamos en una nueva columna el promedio del porcentaje de pobreza para los estudios del 2015 y 2020:
```{r}
dfproyecto2 <- merge(pobreza2015,pobreza2020, by = "Municipality ID")

dfproyecto2$PcgPobreza2015 <- as.numeric(as.character(dfproyecto2$PcgPobreza2015))
dfproyecto2$PcgPobreza2020 <- as.numeric(as.character(dfproyecto2$PcgPobreza2020))
 

dfproyecto2$PcgPobrezaProm <- rowMeans(dfproyecto2[ , c(3,5)], na.rm=TRUE)
```

