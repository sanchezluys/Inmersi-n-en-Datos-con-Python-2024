### CLASE 02

#### Objetivos

En esta aula avanzaremos con nuestro proyecto y comenzaremos a experimentar con algunos algoritmos de machine learning como Regresión Logística, Árboles de Decisión, Random Forest, Naive Bayes, entre otros. Luego de crearlos, evaluaremos los modelos utilizando métricas como precisión, recall, área bajo la curva ROC, y F1-score para finalmente, seleccionar el modelo con el mejor rendimiento para la predicción de la solvencia crediticia.

- creando el mapa de calor
  - Nos permite ver con colores la correlación entre las variables del df
  - Mide relacion, positiva entonces mas rojo, si es negativa entonces azul.
  - van de -1 a +1
  - **MULTICOLINEALIDAD** una variable puede estar explicada por otra variable, implica variables llevando la misma informacion al modelo
  - por ejemplo sexo y estado civil tienen una correlación igual de -0.74 entonces debe analizarse si se llevan ambas variables al modelo o solo una, y ambas variables nacieron de la misma columna de la data original
  - para los modelos se necesitan librerias, lo modelos de machine learning usan metodos diferentes no son iguales
    - train_test_split:  sirve para entrenar y hacer pruebas, y evaluar el pronostico
    - logisticRegression : modelo de machine learning de clasificacion
    - decisiontreeclassifier : modelo de machine learning de clasificacion
    - randomForestClassifier : modelo de machine learning de clasificacion
    - gaussianN8: modelo de machine learning de clasificacion
    - sklearn.metrics : son las metricas de evaluacion mas usadas para la machine learning

- existen diferentes modelos de machine learning: de clasificacion, de regresion y de clusterizacion y se aplican dependiendo del problema del negocio. ".. construir un modelo de machine learning preciso y confiable que sea capaz de evaluar con mayor precisión la probabilidad de incumplimiento crediticio de sus clientes.. "
  - Clasificacion: clasifica basandose en los datos para evaluar el nuevo dato y clasificarlo si cumple o no, aqui se le enseña a la maquina con los datos disponibles, es decir es supervisado
  - Regresion: predecir valores, estimar un numero
  - Clusterizacion: en este caso no se le enseña a la maquina, de manera no supervisada debe ser capaz de crear grupos de datos para poder predecir

- si por ejemplo el problema fuese cuanto se le puede aprobar? entonces los modelos a usar deberian ser de progresion
  
- **MODELADO MACHINE LEARNING**
    - Se toman 2 grupos de datos los de pruebas y los de entrenamiento.
    - en y se guarda el dato a pronosticar, se llama variable **target: variable dependiente / explicada** en este caso el default, si el cliente pagará o no el credito.
    - en x se guardan los demás datos menos la columna definida en y, **Features / Atributos: variables independientes / explicativas** 
    - luego x explica a y
    - el modelo selecciona de manera aleatoria los datos para el entrenamiento y pruebas.

![Primeros Resultados del Modelo ML](/clase02/imagenes/primeros_resultados_ml.png)

mejorando el formato del resultado:
- AUC-ROC: area bajo la curva

![Primeros Resultados Version 2](/clase02/imagenes/primeros_resultados_v2.png)

graficando los resultados:

![Grafica de los resultados](/clase02/imagenes/grafica_resultados.png)

**se pueden ejecutar todos los pasos de un cuaderno con Ctrl + F9, para no tener que ejecutar uno a uno cuando se inicia el cuaderno**

#### Metricas:

- Exactitud (Accuracy) : en comparación con todas las clases, la métrica «Exactitud» muestra cuántas se predijeron correctamente.

- Precisión : la métrica «Precisión» expresa cuántos de los resultados positivos que se han predicho son verdaderamente positivos.

- Medida F1 score : la métrica «F1 score» se revela muy útil para comparar un modelo de alta sensibilidad (recall) con otro modelo de baja precisión.

- Sensibilidad (Recall) : esta métrica indica con exactitud cuántas clases positivas se predijeron correctamente