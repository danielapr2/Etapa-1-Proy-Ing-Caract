# Proyecto Final: Ing. Características

### Nivel de educación y pobreza: ¿nexos de la violencia en México?
### Ana Daniela Pérez Romero

El objetivo de este proyecto es examinar la influencia de la educación y la pobreza sobre la delincuencia en México. Actualmente, el país está atravesando por una enorme crisis de violencia social; probablemente la más grande de su historia, por ello, esta situación ha incitado que la población quiera encontrar explicaciones, incluyéndome. La más socorrida es la que señala a la falta de oportunidades educativas y a la pobreza como los orígenes de estos conflictos; ¿una sociedad pobre pero más educada tendría una tasa delictiva más baja?


El principal vínculo teórico entre el aumento de la educación y el comportamiento criminal es bastante directo: la educación aumenta las oportunidades de trabajos legítimos y sus salarios, lo que reduce el atractivo financiero de las actividades delictivas. Es conocido que los criminales son racionales y buscan maximizar su bienestar, es decir, miden en términos monetarios los incentivos de realizar actividades legales versus ilegales. 


Si bien desde el punto de vista teórico podemos identificar varios canales a través de los cuales la educación afecta al crimen, abordar empíricamente esta relación resulta más difícil. La causalidad que usualmente se predice va desde la educación al comportamiento criminal, es decir, una persona con poca educación es más propensa a cometer delitos en comparación con una persona con mayor educación. Sin embargo, también debemos considerar que una persona con una propensión mayor a cometer delitos es menos probable que permanezca en la escuela en comparación con una persona que no es propensa a cometerlos.


Para esta primera etapa de investigación, se hará la descarga de datos que nos ayuden a revisar las relaciones entre educación, pobreza y delincuencia. Analizaremos específicamente la situación del estado de Sonora para entender el comportamiento de nuestro estado y sus municipios.

----------------------------------------------------------------------------------------------------------------------------------------------

El proyecto se divide en diferentes etapas: 
-	Descarga de datos desde diferentes fuentes
-	Limpieza de datos para su transformación en formato tidy
-	Análisis y exploración de datos
-	Análisis de datos por medio de un tablero

**El proyecto fue realizado con programación R y Software Tableau.**

----------------------------------------------------------------------------------------------------------------------------------------------

## Etapa 1:Obtención de datos de diferentes fuentes de datos:

#### Información Educativa en México - Data Mexico:
https://api.datamexico.org/tesseract/data.jsonrecordsState=26&cube=inegi_enoe&drilldowns=Municipality%2CYear%2CInstruction+Level&locale=es&measures=Number+of+Records%2CMonthly+Wage

#### Información Delictiva en México - Data Mexico:
https://api.datamexico.org/tesseract/data.jsonrecords?State=26&cube=sesnsp_crimes&drilldowns=Municipality%2CYear%2CCrime+Type&locale=es&measures=Value

#### Pobreza en México 2015 - CONEVAL:
https://www.coneval.org.mx/Informes/Pobreza/Datos_abiertos/pobreza_municipal_2010-2020/indicadores%20de%20pobreza%20municipal_2015.csv

#### Pobreza en México 2020 - CONEVAL:
https://www.coneval.org.mx/Informes/Pobreza/Datos_abiertos/pobreza_municipal_2010-2020/indicadores%20de%20pobreza%20municipal_2020.csv


## Etapa 2: Creación de tablas tidy y análisis exploratorio de datos
Para la creación de archivos tidy se hicieron las siguientes modificaciones a los datos de entrada:

#### Información Educativa en México:
Se agruparon los niveles educativos registrados por municipio:
- Educación Básica: preescolar, primaria y sencundaria
- Educación Media Superior: preparatoria y normal
- Educación Superior: carrera técnica, profesional y maestría
- Sin Educación

Archivo Final: dfeducacion.csv

#### Información Delictiva en México:
Se agrupan todos los diferentes tipos de crímenes para hacer una sumatoria total por municipio.
                            
Archivo Final: dfcrimenes.csv

#### Pobreza en México:
Archivo Final: dfpobreza.csv

Archivo del proyecto: dfproyecto.csv


## Etapa 3: Análisis de datos por medio de herramientas de visualización
### Link al Tablero en Tableau:
https://public.tableau.com/views/NiveldeeducacinypobrezanexosdelaviolenciaenMxico/Educacin?:language=en-US&:display_count=n&:origin=viz_share_link
