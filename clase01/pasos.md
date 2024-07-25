### CLASE 01

Requisitos Previos

- Se debe contar con una cuenta GMAIL

- Descargar los archivos y colocarlos en el drive
  
  - 1.- funciones.md : contiene las funciones python que se usaran
  - 2.- diccionario.md : contiene la descripcion de los datos del archivo german_credit.csv
  - 3.- german_credit.csv : los datos del banco
  
- Iniciar COLAB, https://colab.research.google.com/notebooks/intro.ipynb
- Si pide las autorizaciones darlas
- Se crea el cuaderno nuevo de inmersion
- a mano derecha dar clic al boton de conectar 
- Existe la inserccion de texto y de código en el cuaderno
- Se le puede dar el titulo a la seccion con insertar texto

![Agregar texto](/clase01/imagenes/agregar_texto_seccion.png)

![Boton Conectar al Server de Colab](/clase01/imagenes/conectar_server_colab.png)

- Se importan los modulos para el cuaderno, son dependencias que se necesitan en el proyecto
  - Pandas
  - Matplotlib
  - Seaborn
  - Driver de google colab
  - Warnings

```python
  # prompt: importa los siguientes módulos con sus respectivos alias: pandas, matplotlib, seaborn, drive de google colab, warnings

  import pandas as pd
  import matplotlib.pyplot as plt
  import seaborn as sns
  from google.colab import drive
  import warnings

  drive.mount("/content/Drive")
  warnings.filterwarnings("ignore") # para ignorar las advertencias
```

- Al insertar codigo se puede usar '#' para documentarlo.

  ![Vista de las carpetas de Drive conectadas a Colab](https://github.com/user-attachments/assets/93d78816-5e51-4ad2-9bda-abc1ab6e45dc)

- Es necesario hacer unas configuraciones para un alcance global de las variables y la biblioteca Pnadas

```python
  pd.set_option('display.max_columns',None)
  global df_banco, resultados
```

![Configuraciones globales](/clase01/imagenes/configuraciones_globales.png)

- Es necesario importar el archivo (german_credit.csv) desde el drive para que colab lo tome, para eso es necesario dar clic derecho al arcivo a la izquierda y copiar la ruta primero.

![Copiar ruta](/clase01/imagenes/copiar_ruta.png)

- Ejecutar el codigo python para importar el archivo usando la variable global df_banco

```python
  df_banco = pd.read_csv('/content/drive/MyDrive/Inmersion_datos_2024/german_credit.csv')
  df_banco.head()
```

![Conectando archivo csv](/clase01/imagenes/conectando_archivo_csv.png)

- para saber cuantas columnas y filas tiene el objeto que contiene la tabla se pueden usar metodos y atributos ya que python es un lenguaje orientado a objetos (tambien es multiparadigma) **df_banco.shape**

![Filas y columnas del objeto df](/clase01/imagenes/filas_columnas_df.png)

- Para ver el nombre de las columnas: **df_banco.columns**
- Para ver la informacion detallada de data frame df se puede usar: **df_banco.info()** que muestra el detalle y tipos de dato de cada columna, asi como la cantidad de datos nulos. Object es string.

![Info del df](/clase01/imagenes/info_del_df.png)

- se observan datos numericos y no numericos (categoricas), para usar modelado se trabaja solo con numeros, entonces si los datos categoricos son importantes se hace necesario convertirlas de alguna manera en datos numericos. A esto ultimo de le llama **preprocesamiento de datos** 
- Es necesario saber cuales son las columnas que sus datos son tipo string / Object, para esto ejecutamos:

```python
  columnas = list(df_banco.select_dtypes(include=['object']).columns)
  columnas
```
- para cada columna python puede mostrarme que datos tiene y cuantas veces se repite en la columna y asi tener una idea de los datos y su frecuencia. se usa por ejemplo el siguiente codigo:

```python
  # solo la lista
  df_banco.account_check_status.value_counts().index
  # con este veo la frecuencia de repeticion
  df_banco.account_check_status.value_counts()
```
- se puede mostrar todas las columnas y sus categorias con el siguiente codigo:

```python
  for columna in columnas:
    print(f'El nombre de la columna: {columna}')
    print(list(df_banco[f'{columna}'].value_counts().index))
    print('\n')
```
- al tener clara las categorias de cada columna es necesario convertir esas categorias en numeros, por ejemplo yes en 1, no en 0 etc, pero se deben tener numero, para eso se crea la siguiente funcion en python para que haga esos ajustes con diccionarios. la funcion recibe el diccionario de cada categoria de las columnas y les asigna un numero entero del diccionario definido para cada caso

```python
  def procesar_datos():
    global df_banco
    df_banco = df_banco.drop_duplicates() if df_banco.duplicated().any() else df_banco
    df_banco = df_banco.dropna() if df_banco.isnull().values.any() else df_banco

    a = {'no checking account': 4,
        '>= 200 DM / salary assignments for at least 1 year': 3,
        '0 <= ... < 200 DM': 2,
        '< 0 DM': 1
    }
    df_banco['account_check_status'] = df_banco['account_check_status'].map(a)

    a = { 'no credits taken/ all credits paid back duly' : 1,
        'all credits at this bank paid back duly' : 2,
        'existing credits paid back duly till now' : 3,
        'delay in paying off in the past' : 4,
        'critical account/ other credits existing (not at this bank)' : 5
    }
    df_banco['credit_history'] = df_banco['credit_history'].map(a)

    a = {'car (new)' : 1,
        'car (used)' : 2,
        'furniture/equipment' : 3,
        'radio/television' : 4,
        'domestic appliances' : 5,
        'repairs' : 6,
        'education' : 7,
        '(vacation - does not exist?)' : 8,
        'retraining' : 9,
        'business' : 10,
        'others' : 11
    }
    df_banco['purpose'] = df_banco['purpose'].map(a)

    a = {'unknown/ no savings account' : 1,
        '.. >= 1000 DM ' : 2,
        '500 <= ... < 1000 DM ' : 3,
        '100 <= ... < 500 DM' : 4,
        '... < 100 DM' : 5
    }
    df_banco['savings'] = df_banco['savings'].map(a)

    a = {'.. >= 7 years' : 1,
        '4 <= ... < 7 years' : 2,
        '1 <= ... < 4 years' : 3,
        '... < 1 year ' : 4,
        'unemployed' : 5
    }
    df_banco['present_emp_since'] = df_banco['present_emp_since'].map(a)

    a = {'male : divorced/separated' : 1,
        'female : divorced/separated/married' : 2,
        'male : single' : 3,
        'male : married/widowed' : 4,
        'female : single' : 5
    }
    df_banco['personal_status_sex'] = df_banco['personal_status_sex'].map(a)

    a = {'none' : 1,
        'co-applicant' : 2,
        'guarantor' : 3
    }
    df_banco['other_debtors'] = df_banco['other_debtors'].map(a)

    a = {'real estate' : 1,
        'if not A121 : building society savings agreement/ life insurance' : 2,
        'if not A121/A122 : car or other, not in attribute 6' : 3,
        'unknown / no property' : 4
    }
    df_banco['property'] = df_banco['property'].map(a)

    a = {'bank' : 1,
        'stores' : 2,
        'none' : 3
    }
    df_banco['other_installment_plans'] = df_banco['other_installment_plans'].map(a)

    a = {'rent' : 1,
        'own' : 2,
        'for free' : 3
    }
    df_banco['housing'] = df_banco['housing'].map(a)

    a = {'unemployed/ unskilled - non-resident' : 1,
        'unskilled - resident' : 2,
        'skilled employee / official' : 3,
        'management/ self-employed/ highly qualified employee/ officer' : 4
    }
    df_banco['job'] = df_banco['job'].map(a)

    a = {'yes, registered under the customers name ' : 1,
        'none' : 0
    }
    df_banco['telephone'] = df_banco['telephone'].map(a)

    a = {'yes' : 1,
        'no' : 0
    }
    df_banco['foreign_worker'] = df_banco['foreign_worker'].map(a)
```
- luego que el df tiene solo numeros es necesario hacer un analisis exploratorio, donde la idea es verificar los datos numericos y observar su calidad
- entonces se revisan las variables discretas, por ejemplo los plazos del credito son datos discretos y no continuos.

```python
  variables_discretas = ['personal_status_sex','age',
                        'duration_in_month','credit_amount','default']
  df_banco[variables_discretas].tail(3)
```
- luego se crea una nueva columna llamada sexo en el df

```python
  dic_sexo = {2:1,5:1,1:0,3:0,4:0}
  df_banco['sexo'] = df_banco['personal_status_sex'].map(dic_sexo)
```
- se crean esas nuevas columnas con una funcion:

  
```python
  def feature_engineering():
      global df_banco
      dic_sexo = {2:1, 5:1, 1:0, 3:0, 4:0}
      dic_est_civil = {3:1, 5:1, 1:0, 2:0, 4:0}
      df_banco['sexo'] = df_banco['personal_status_sex'].map(dic_sexo)
      df_banco['estado_civil'] = df_banco['personal_status_sex'].map(dic_est_civil)
      df_banco['rango_edad'] = pd.cut(x = df_banco['age'],
                                      bins=[18, 30, 40, 50, 60, 70, 80],
                                      labels = [1, 2, 3, 4, 5, 6]).astype(int)
      df_banco['rango_plazos_credito']=pd.cut(x = df_banco['duration_in_month'],
                                                bins=[1, 12, 24, 36, 48, 60, 72],
                                                labels = [1, 2, 3, 4, 5, 6]).astype(int)
      df_banco['rango_valor_credito']=pd.cut(x = df_banco['credit_amount'],
                                               bins=[1, 1000, 2000, 3000, 4000,
                                                     5000, 6000, 7000, 8000, 9000,
                                                     10000, 11000, 12000, 13000,
                                                     14000, 15000, 16000, 17000,
                                                     18000, 19000, 20000],
                                               labels = [1, 2, 3, 4, 5, 6, 7, 8, 9,
                                                         10, 11, 12, 13, 14, 15, 16,
                                                         17, 18, 19, 20]).astype(int)
      df_banco = df_banco.drop(columns=['personal_status_sex','age',
                                        'duration_in_month','credit_amount'])
         
  
  feature_engineering()
  df_banco.head(2)
```

- se usa .cut() que permite definir rangos de valores tambien llamdas porciones que dependen del rangos preestablecidos
- la funcion tambien elimina las columnas que fueron categorizadas como age, credit_amount etc. esto se hace ya que el modelo machine learning comprende mejor los datos que no son tan dispersos, por eso es recomendable **agrupar por rangos** de los datos cuando sea posible
- usamos el metodo descripcion df_banco.describe(), que nos muestra el conteo, la media, desviacion estandar, minimo, maximo etc, para hacer un analisis estadistico
- ahora necesitamos un grafico histograma que es de distribucion, por ejemplo de sexo, de monto etc, no apoyamos con chatgpt.
- **prompt** genera un codigo en python que permita graficar un histograma con la variable 'sexo' utilizando matplotlib, seaborn y que tome los datos a partir de un dataframe creado por pandas llamado 'df_banco'

```python
  import matplotlib.pyplot as plt
  import seaborn as sns

  # Crear el histograma con Matplotlib
  plt.hist(df_banco['sexo'])
  plt.xlabel('Sexo')
  plt.ylabel('Frecuencia')
  plt.title('Histograma de Sexo')
  plt.show()
  
  # Crear el histograma con Seaborn
  sns.histplot(data=df_banco, x='sexo')
  plt.xlabel('Sexo')
  plt.ylabel('Frecuencia')
  plt.title('Histograma de Sexo')
  plt.show()
```
otro ejemplo:

```python
  # Configurar el estilo de Seaborn (opcional)
  sns.set(style="whitegrid")
  
  # Crear el histograma utilizando Seaborn
  plt.figure(figsize=(5, 4))
  sns.countplot(data=df_banco, x='sexo')
  plt.title('Histograma de Sexo')
  plt.xlabel('Sexo')
  plt.ylabel('Frecuencia')
  
  # Mostrar el histograma
  plt.show()
```
- se crea una funcion para crear los histogramas de todas las variables

```python
  def analisis_exploratorio():
    global df_banco
    histogramas = ['sexo','estado_civil','rango_plazos_credito','rango_edad','default']
    lista_histogramas = list(enumerate(histogramas))
    plt.figure(figsize = (30,20))
    plt.title('Histogramas')
    for i in lista_histogramas:
      plt.subplot(3, 2, i[0]+1)
      sns.countplot(x = i[1], data = df_banco)
      plt.xlabel(i[1], fontsize=20)
      plt.ylabel('Total', fontsize=20)
```

### Mentoria Clase 01

https://www.youtube.com/watch?v=4c5EvzqOsaU


### GOOGLE COLAB

- Es sencillo y facil de usar
- solo necesita cuenta gmail
- no se necesita instalar nada en el computador
- tiene una capa gratuita muy potente
- 


### RECURSOS EXTRAS:

- https://www.aluracursos.com/blog/google-colab-que-es-y-como-usarlo
- https://scikit-learn.org/stable/supervised_learning.html
- https://scikit-learn.org/stable/modules/model_evaluation.html
- https://pandas.pydata.org/Pandas_Cheat_Sheet.pdf
- https://www.aluracursos.com/blog/que-hace-un-cientista-de-datos

- ### Redes Sociales
- Etiqueta Alura Latam (@aluralatam) y los instructores Álvaro (ahcamachod), Alejandro Gamarra (elprofealejo.info) y a Christian Velasco (christian_pva).
-¡Nos encantaría ver tus proyectos y seguir tu evolución! Recuerda usar el hashtag #InmersionDatosAlura para que tu proyecto tenga más alcance.
