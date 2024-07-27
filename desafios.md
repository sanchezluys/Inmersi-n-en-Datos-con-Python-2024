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
- Sexo                  -0.74  // faltó incluirla
- Plazos del credito    +0.61
- Creditos en el banco  +0.44
- Historial crediticio  -0.23
- Estatus de chequera   -0.35
- Rango de Edad         -0.25
- Tipo de alojamiento   +0.35
- Ahorros               -0.22

##### 3.- Crear una conclusión para cada uno de los gráficos del histograma. Mirar los datos y extraer conclusiones, porque es una habilidad esencial

- respondida con el desafio 2


#### Desafios CLASE 02

##### 1.- Evaluar la Matrix de Confusión

Concepto: Imagina que eres un detective de animales y tu trabajo es identificar si un animal es un perro o un gato. Tienes una caja mágica (que en realidad es un modelo de computadora) que te ayuda a hacer esto.
La matriz de confusión es como un reporte especial que muestra qué tan bien lo está haciendo tu caja mágica. Es como una tabla de resultados que tiene cuatro partes:

- Aciertos de perros: Cuántas veces la caja dijo "perro" y realmente era un perro.
- Aciertos de gatos: Cuántas veces la caja dijo "gato" y realmente era un gato.
- Errores de perro: Cuántas veces la caja dijo "perro" pero en realidad era un gato.
- Errores de gato: Cuántas veces la caja dijo "gato" pero en realidad era un perro.

Aplicando la matriz de confusion al caso del banco:

```python
    # prompt: como puedo aplicar una matriz de confusion a los results_df 

    from sklearn.metrics import confusion_matrix

    def crea_modelos_con_matriz_confusion():
    global df_banco, resultados
    y = df_banco['default']
    x = df_banco.drop(columns='default')
    train_x, test_x, train_y, test_y = train_test_split(x, y, test_size=0.30, random_state = 77)

    models = {
        'Regresión Logística': LogisticRegression(),
        'Árbol de Decisión': DecisionTreeClassifier(),
        'Random Forest': RandomForestClassifier(),
        'Naive Bayes': GaussianNB()
    }

    results = {'Model': [], 'Accuracy': [], 'Precision': [], 'Recall': [], 'F1-score': [], 'AUC-ROC': [], 'Confusion Matrix': []}

    for name, model in models.items():
        model.fit(train_x, train_y)
        predictions = model.predict(test_x)
        accuracy = accuracy_score(test_y, predictions)
        precision = precision_score(test_y, predictions)
        recall = recall_score(test_y, predictions)
        f1 = f1_score(test_y, predictions)
        if hasattr(model, "predict_proba"):
            proba = model.predict_proba(test_x)
            roc_auc = roc_auc_score(test_y, proba[:, 1])
        else:
            roc_auc = None
        
        # Calcular y almacenar la matriz de confusión
        cm = confusion_matrix(test_y, predictions)

        results['Model'].append(name)
        results['Accuracy'].append(accuracy)
        results['Precision'].append(precision)
        results['Recall'].append(recall)
        results['F1-score'].append(f1)
        results['AUC-ROC'].append(roc_auc)
        results['Confusion Matrix'].append(cm)

    resultados = results

    crea_modelos_con_matriz_confusion()

    # Imprimir resultados incluyendo la matriz de confusión
    for i in range(len(resultados['Model'])):
        print("Model:", resultados['Model'][i])
        print("Accuracy: {:.2f}".format(resultados['Accuracy'][i]))
        print("Precision: {:.2f}".format(resultados['Precision'][i]))
        print("Recall: {:.2f}".format(resultados['Recall'][i]))
        print("F1-score: {:.2f}".format(resultados['F1-score'][i]))
        if resultados['AUC-ROC'][i] is not None:
        print("AUC-ROC: {:.2f}".format(resultados['AUC-ROC'][i]))
        print("Confusion Matrix:")
        print(resultados['Confusion Matrix'][i])
        print("----")

    # Visualización de la matriz de confusión (opcional)
    import seaborn as sns
    import matplotlib.pyplot as plt

    for i in range(len(resultados['Model'])):
        plt.figure(figsize=(6, 4))
        sns.heatmap(resultados['Confusion Matrix'][i], annot=True, fmt='g', cmap='Blues')
        plt.xlabel('Predicted')
        plt.ylabel('Actual')
        plt.title(f'Confusion Matrix - {resultados["Model"][i]}')
        plt.show()
```
Resultados graficos:

![Matriz Confusion Regresion Logistica ](/clase02/imagenes/mc_regeresion_logistica.png)

![Matriz de confusion Arbol de Decision](/clase02/imagenes/mc_arbol_decision.png)

![Matriz de confusion Random Forest](/clase02/imagenes/mc_random_forest.png)

![Matriz de confusion Naive Bayes](/clase02/imagenes/mc_naives_bayes.png)

Se observa que el mas acertivo es **Random Forest** con 200/14, pero ninguno de los modelos es acertivo para predecir cuando el cliente no pagará, sino cuando pagará.

#### 2.- Balancear la variable target

Una vez balanceado el target:

![Resultados con Target Balanceado](/clase02/imagenes/resultados_target_balanceado.png)

- De nuevo el modelo que mas acertividad tiene es el **Random Forest**

#### 3.- Seleccionar sólo algunas variable y reevaluar

