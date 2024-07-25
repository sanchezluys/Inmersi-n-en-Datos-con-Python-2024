### DESAFIOS

### Clase 1

#### Objetivos
- Importar el conjunto de datos.
- Conocer el tamaño y el tipo de datos que hay para cada variable.
- Realizar un preprocesamiento a los datos para facilitar su análisis.
- Generar histogramas para entender cómo están distribuidos los datos.

#### Desafíos CLASE 01
1. - Analizar los datos de las distribuciones e identificar si hay algún valor o registros que no se deben considerar para el modelo.
2. - Investigar qué es y como crear un mapa de calor para analizar la correlación de las variables.
3. - Crear una conclusión para cada uno de los gráficos del histograma. Mirar los datos y extraer conclusiones, porque es una habilidad esencial

##### 1.- Analizar los datos de las distribuciones e identificar si hay algún valor o registros que no se deben considerar para el modelo

![Distribucion Sexo](/clase01/imagenes/distribuicion_sexo.png)

- Los datos estan distruibuidos mas hombres que mujeres
- Se considera que ambos datos son importantes para el modelo

![Distribucion Estado Civil](/clase01/imagenes/distribucion_edo_civil.png)

- Los datos estan distruibuidos muy equitativos
- Se considera que ambos datos son importantes para el modelo

![Distribucion Plazos Credito](/clase01/imagenes/distribucion_plazos_credito.png)

- Los valores predominantes son 1,2,3 y 4 luego 5 y 6 tienen muy pocos datos
- Dado que 6 tienen datos insignificantes con respecto a 1,2,3 entonces se recomienda eliminar
- Para el caso 5 se recomienda mantener y evaluar el modelo con y sin esa categoria para ver el impacto de la IA

![Distribucion Rango Edad](/clase01/imagenes/distribucion_rango_edad.png)

- Los valores predominantes son 1,2,3,4 luego 5 y 6  tienen muy pocos datos
- Dado que 6 tiene muy pocos datos con respecto a las demas categorias se recomienda eliminar
- Para el caso 5 se recomienda mantener y evaluar el modelo con y sin esa categoria

![Distribucion Default](/clase01/imagenes/distribucion_default.png)

- Existe una mayor cantidad de casos con default 0 y un 50% menos aproximadamente de casos con default en 1
- Ambos categorias son importantes ya que son la razon de ser del estudio

##### 2.- Investigar qué es y como crear un mapa de calor para analizar la correlación de las variables.

Correlación, positiva, negativa, 0-1, 0-1, con este grafico se puede analizar que variables se pueden tomar en cuenta para el modelo, por ejemplo una correlacion menor a 0.1 se puede despreciar, una mayor a 0.3 es levememnte importante y una mayor a 0.6 es importante.

![Mapa de Calor](/clase01/imagenes/mapa_calor.png)

Variables que se consideran importantes para el modelo:
- Estado civil          -0.74
- Creditos en el banco  +0.44
- Plazos del credito    +0.61
- Historial crediticio  -0.23
- Estatus de chequera   -0.35
- Rango de Edad         -0.25
- Tipo de alojamiento   +0.35
- Ahorros               -0.22

##### 3.- Crear una conclusión para cada uno de los gráficos del histograma. Mirar los datos y extraer conclusiones, porque es una habilidad esencial

- respondida con el desafio 2