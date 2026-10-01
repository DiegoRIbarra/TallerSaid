# 🧠 Guía Completa de Deep Learning con NumPy (MNIST desde cero)

Esta guía documenta la construcción, entrenamiento y evaluación de una red neuronal artificial (*Feedforward Multi-Layer Perceptron*) con una capa oculta programada enteramente desde cero utilizando **Python y NumPy** para clasificar dígitos manuscritos del dataset **MNIST**.

Contiene **todos y cada uno de los bloques de código exactos del tutorial original** en orden secuencial, acompañados de explicaciones de cada concepto, análisis profundo de los hiperparámetros, interpretación de las gráficas resultantes y la solución a todos los errores encontrados durante la ejecución.

---

## 1. Arquitectura de la Red

```
[Entrada: 784 píxeles] 
        │
        ▼  Matriz W1 (784 x 100)
[Capa Oculta: 100 neuronas] ──► Activación ReLU ──► Regularización Dropout (p=0.5)
        │
        ▼  Matriz W2 (100 x 10)
[Capa Salida: 10 neuronas]  ──► 10 puntuaciones (una por cada dígito del 0 al 9)
```

- **Entrada ($layer_0$):** Imagen aplanada de $28 \times 28 = 784$ píxeles.
- **Capa oculta ($layer_1$):** 100 neuronas con activación no lineal **ReLU** y **Dropout** ($p=0.5$).
- **Capa de salida ($layer_2$):** 10 valores que se comparan contra las etiquetas codificadas en *one-hot*.
- **Pérdida:** Error Cuadrático (Suma de residuos al cuadrado - SSE/MSE).

---

## 2. Sección 1: Carga del Dataset MNIST

### Celda 1: Definir los nombres de archivos
```python
filename = [["training_images", "train-images-idx3-ubyte.gz"],   # 60,000 training images.
            ["test_images", "t10k-images-idx3-ubyte.gz"],        # 10,000 test images.
            ["training_labels", "train-labels-idx1-ubyte.gz"],   # 60,000 training labels.
            ["test_labels", "t10k-labels-idx1-ubyte.gz"]]        # 10,000 test labels.
```

### Celda 2: Descargar los 4 archivos

```python
from urllib import request

base_url = "https://ossci-datasets.s3.amazonaws.com/mnist/"

for name in filename:
    print("Downloading file: " + name[1])
    request.urlretrieve(base_url + name[1], name[1])
```

**Salida esperada:**
```
Downloading file: train-images-idx3-ubyte.gz
Downloading file: t10k-images-idx3-ubyte.gz
Downloading file: train-labels-idx1-ubyte.gz
Downloading file: t10k-labels-idx1-ubyte.gz
```

### Celda 3: Descomprimir y crear los arreglos NumPy
Las imágenes tienen un encabezado de 16 bytes y se redimensionan a vectores de 784 ($28 \times 28$). Las etiquetas tienen un encabezado de 8 bytes:

```python
import gzip
import numpy as np

mnist_dataset = {}

# Images
for name in filename[:2]:
    with gzip.open(name[1], 'rb') as mnist_file:
        mnist_dataset[name[0]] = np.frombuffer(mnist_file.read(), np.uint8, offset=16).reshape(-1, 28*28)
# Labels
for name in filename[-2:]:
    with gzip.open(name[1], 'rb') as mnist_file:
        mnist_dataset[name[0]] = np.frombuffer(mnist_file.read(), np.uint8, offset=8)
```

### Celda 4: Separar en conjuntos de Entrenamiento y Prueba
```python
x_train, y_train, x_test, y_test = (mnist_dataset["training_images"],
                                    mnist_dataset["training_labels"],
                                    mnist_dataset["test_images"],
                                    mnist_dataset["test_labels"])
```

### Celda 5: Confirmar dimensiones de los arreglos
```python
print('The shape of training images: {} and training labels: {}'.format(x_train.shape, y_train.shape))
print('The shape of test images: {} and test labels: {}'.format(x_test.shape, y_test.shape))
```

**Salida esperada:**
```
The shape of training images: (60000, 784) and training labels: (60000,)
The shape of test images: (10000, 784) and test labels: (10000,)
```

### Celda 6: Inspeccionar una imagen con Matplotlib
Tomamos la última imagen del set de entrenamiento (índice 59,999) y la remodelamos a $28 \times 28$:

```python
import matplotlib.pyplot as plt

# Take the 60,000th image (indexed at 59,999) from the training set,
# reshape from (784, ) to (28, 28) to have a valid shape for displaying purposes.
mnist_image = x_train[59999, :].reshape(28, 28)
# Set the color mapping to grayscale to have a black background.
plt.imshow(mnist_image, cmap='gray')
# Display the image.
plt.show()
```

### Celda 7: Mostrar 5 imágenes aleatorias del entrenamiento
```python
# Display 5 random images from the training set.
np.random.seed(0)
indices = list(np.random.randint(x_train.shape[0], size=9))
for i in range(5):
    plt.subplot(1, 5, i+1)
    plt.imshow(x_train[indices[i]].reshape(28, 28), cmap='gray')
    plt.tight_layout()
```

### Celda 8: Inspeccionar la etiqueta real de la imagen 59,999
```python
# Display the label of the 60,000th image (indexed at 59,999) from the training set.
y_train[59999]
```

**Salida esperada:**
```
8
```

---

## 3. Sección 2: Preprocesamiento de Datos

### Celda 9: Verificar el tipo de dato original de las imágenes
```python
print('The data type of training images: {}'.format(x_train.dtype))
print('The data type of test images: {}'.format(x_test.dtype))
```

**Salida esperada:**
```
The data type of training images: uint8
The data type of test images: uint8
```

### Celda 10: Normalizar imágenes a punto flotante $[0.0, 1.0]$
Dividir entre 255 promueve el tipo a `float64`. Usamos `sample = 1000` para entrenar rápidamente:

```python
sample = 1000
training_images = x_train[0:sample] / 255
test_images = x_test / 255
```

### Celda 11: Confirmar el nuevo tipo de dato de las imágenes
```python
print('The data type of training images: {}'.format(training_images.dtype))
print('The data type of test images: {}'.format(test_images.dtype))
```

**Salida esperada:**
```
The data type of training images: float64
The data type of test images: float64
```

### Celda 12: Verificar el tipo de dato original de las etiquetas
```python
print('The data type of training labels: {}'.format(y_train.dtype))
print('The data type of test labels: {}'.format(y_test.dtype))
```

**Salida esperada:**
```
The data type of training labels: uint8
The data type of test labels: uint8
```

### Celda 13: Definir la función de One-Hot Encoding
Convierte enteros de 0 a 9 en vectores de ceros con un 1.0 en la posición correspondiente:

```python
def one_hot_encoding(labels, dimension=10):
    # Define a one-hot variable for an all-zero vector 
    # with 10 dimensions (number labels from 0 to 9).
    one_hot_labels = (labels[..., None] == np.arange(dimension)[None])
    # Return one-hot encoded labels.
    return one_hot_labels.astype(np.float64)
```

### Celda 14: Codificar las etiquetas de entrenamiento y prueba
```python
training_labels = one_hot_encoding(y_train)
test_labels = one_hot_encoding(y_test)
```

### Celda 15: Comprobar el tipo de dato de las etiquetas codificadas
```python
print('The data type of training labels: {}'.format(training_labels.dtype))
print('The data type of test labels: {}'.format(test_labels.dtype))
```

**Salida esperada:**
```
The data type of training labels: float64
The data type of test labels: float64
```

### Celda 16: Examinar etiquetas codificadas
```python
print(training_labels[0])
print(training_labels[1])
print(training_labels[2])
```

**Salida esperada:**
```
[0. 0. 0. 0. 0. 1. 0. 0. 0. 0.]
[1. 0. 0. 0. 0. 0. 0. 0. 0. 0.]
[0. 0. 0. 0. 1. 0. 0. 0. 0. 0.]
```

### Celda 17: Comparar con las etiquetas originales
```python
print(y_train[0])
print(y_train[1])
print(y_train[2])
```

**Salida esperada:**
```
5
0
4
```

---

## 4. Sección 3: Construcción y Entrenamiento de la Red

### Celda 18: Fijar semilla para reproducibilidad
```python
np.random.seed(1)
```

### Celda 19: Funciones de activación (ReLU y su derivada)
```python
# Define ReLU that returns the input if it's positive and 0 otherwise.
def relu (x):
    return (x>=0) * x

# Set up a derivative of the ReLU function that returns 1 for a positive input 
# and 0 otherwise.
def relu2deriv(output):
    return output >= 0
```

### Celda 20: Definir los Hiperparámetros
```python
learning_rate = 0.005
epochs = 100  
hidden_size = 100
pixels_per_image = 784
num_labels = 10
```

---

### 🔬 Explicación Detallada de los Hiperparámetros

| Hiperparámetro | Valor Configurado | ¿Qué es y qué función cumple? | ¿Qué pasa si se modifica? |
|---|---|---|---|
| **`learning_rate` ($\alpha$)** | `0.005` | Tasa o paso de aprendizaje del Descenso de Gradiente. Escala los gradientes calculados antes de restarlos a las matrices de pesos. | • Si es **demasiado alto** (ej. 0.5): las actualizaciones son excesivas, el algoritmo diverge y la pérdida se dispara a infinito.<br>• Si es **muy bajo** (ej. 0.00001): la red requeriría miles de épocas para converger. |
| **`epochs`** | `100` | Cantidad total de iteraciones completas sobre todo el dataset de entrenamiento ($N=1000$). | • Pocas épocas (ej. 10): el modelo queda en *underfitting* (no termina de aprender los patrones).<br>• Demasiadas épocas sin regularización: el modelo memoriza el conjunto y pierde generalización (*overfitting*). por lo tanto el numero recomendado es 100 pero si el dispositivo no es tan potente se puede bajar el numero para no esperar tanto|
| **`hidden_size`** | `100` | Número de neuronas en la capa oculta intermedia. Define el espacio latente de abstracción del modelo. | • Menor (ej. 20): menor capacidad de aprendizaje; puede fallar al distinguir dígitos similares (como 4 y 9 o 3 y 8).<br>• Mayor (ej. 500): mayor capacidad representacional, pero el costo computacional por época crece significativamente. |
| **`pixels_per_image`** | `784` | Dimensión del vector de entrada. Corresponde al aplanado de la imagen cuadrada ($28 \times 28 = 784$). | Parámetro rígido determinado por la resolución del dataset MNIST. |
| **`num_labels`** | `10` | Dimensión de la capa de salida. Cada neurona produce una puntuación para cada clase ($0, 1, 2, ..., 9$). | Parámetro rígido correspondiente a los 10 dígitos decimales. |
| **`Dropout (p=0.5)`** | `np.random.randint(0, 2, ...)` | Técnica de regularización estocástica. Durante el entrenamiento apaga aleatoriamente el 50% de las activaciones de `layer_1` multiplicándolas por 0 y escalando las restantes por 2 (`layer_1 *= dropout_mask * 2`). | Fuerza a la red a desarrollar representaciones redundantes y robustas, impidiendo que grupos de neuronas se vuelvan codependientes. |

---

### Celda 21: Inicialización de Pesos
Se inicializan uniformemente en el intervalo $[-0.1, 0.1]$:

```python
weights_1 = 0.2 * np.random.random((pixels_per_image, hidden_size)) - 0.1
weights_2 = 0.2 * np.random.random((hidden_size, num_labels)) - 0.1
```

### Celda 22: El Bucle de Entrenamiento (Training Loop)
Ejecuta la propagación hacia adelante (*forward pass*), el cálculo de pérdida y aciertos, y la retropropagación de gradientes (*backpropagation*):

```python
# To store training and test set losses and accurate predictions
# for visualization.
store_training_loss = []
store_training_accurate_pred = []
store_test_loss = []
store_test_accurate_pred = []

# This is a training loop.
# Run the learning experiment for a defined number of epochs (iterations).
for j in range(epochs):
    # Set the initial loss/error and the number of accurate predictions to zero.
    training_loss = 0.0
    training_accurate_predictions = 0
    
    # For all images in the training set, perform a forward pass
    # and backpropagation and adjust the weights accordingly.
    for i in range(len(training_images)):
        # Forward propagation/forward pass:
        # 1. The input layer:
        #    Initialize the training image data as inputs.
        layer_0 = training_images[i]
        # 2. The hidden layer:
        #    Take in the training image data into the middle layer by 
        #    matrix-multiplying it by randomly initialized weights. 
        layer_1 = np.dot(layer_0, weights_1)
        # 3. Pass the hidden layer's output through the ReLU activation function.
        layer_1 = relu(layer_1)
        # 4. Define the dropout function for regularization.
        dropout_mask = np.random.randint(0, high=2, size=layer_1.shape)
        # 5. Apply dropout to the hidden layer's output.
        layer_1 *= dropout_mask * 2
        # 6. The output layer:
        #    Ingest the output of the middle layer into the the final layer
        #    by matrix-multiplying it by randomly initialized weights.
        #    Produce a 10-dimension vector with 10 scores.
        layer_2 = np.dot(layer_1, weights_2)

        # Backpropagation/backward pass:
        # 1. Measure the training error (loss function) between the actual
        #    image labels (the truth) and the prediction by the model.
        training_loss += np.sum((training_labels[i] - layer_2) ** 2)
        # 2. Increment the accurate prediction count.
        training_accurate_predictions += int(np.argmax(layer_2) == np.argmax(training_labels[i]))
        # 3. Differentiate the loss function/error.
        layer_2_delta = (training_labels[i] - layer_2)
        # 4. Propagate the gradients of the loss function back through the hidden layer.
        layer_1_delta = np.dot(weights_2, layer_2_delta) * relu2deriv(layer_1)
        # 5. Apply the dropout to the gradients.
        layer_1_delta *= dropout_mask
        # 6. Update the weights for the middle and input layers
        #    by multiplying them by the learning rate and the gradients.
        weights_1 += learning_rate * np.outer(layer_0, layer_1_delta)
        weights_2 += learning_rate * np.outer(layer_1, layer_2_delta)
    
    # Store training set losses and accurate predictions.
    store_training_loss.append(training_loss)
    store_training_accurate_pred.append(training_accurate_predictions)

    # Evaluate on the test set:
    # 1. Set the initial error and the number of accurate predictions to zero.
    test_loss = 0.0
    test_accurate_predictions = 0
    
    # 2. Start testing the model by evaluating on the test image dataset.
    for i in range(len(test_images)):
        # 1. Pass the test images through the input layer.
        layer_0 = test_images[i]
        # 2. Compute the weighted sum of the test image inputs in and
        #    pass the hidden layer's output through ReLU.
        layer_1 = relu(np.dot(layer_0, weights_1))
        # 3. Compute the weighted sum of the hidden layer's inputs.
        #    Produce a 10-dimensional vector with 10 scores.
        layer_2 = np.dot(layer_1, weights_2)

        # 4. Measure the error between the actual label (truth) and prediction values.
        test_loss += np.sum((test_labels[i] - layer_2) ** 2)
        # 5. Increment the accurate prediction count.
        test_accurate_predictions += int(np.argmax(layer_2) == np.argmax(test_labels[i]))

    # Store test set losses and accurate predictions.
    store_test_loss.append(test_loss)
    store_test_accurate_pred.append(test_accurate_predictions)

    # 3. Display the error and accuracy metrics in the output.
    print("\n" + \
          "Epoch: " + str(j) + \
          " Training set error:" + str(training_loss/ float(len(training_images)))[0:5] +\
          " Training set accuracy:" + str(training_accurate_predictions/ float(len(training_images))) +\
          " Test set error:" + str(test_loss/ float(len(test_images)))[0:5] +\
          " Test set accuracy:" + str(test_accurate_predictions/ float(len(test_images))))
```

**Muestra del progreso en consola:**
```
Epoch: 0 Training set error:0.891 Training set accuracy:0.413 Test set error:0.641 Test set accuracy:0.6333
Epoch: 1 Training set error:0.650 Training set accuracy:0.62 Test set error:0.578 Test set accuracy:0.6835
...
Epoch: 98 Training set error:0.363 Training set accuracy:0.859 Test set error:0.409 Test set accuracy:0.7974
Epoch: 99 Training set error:0.365 Training set accuracy:0.867 Test set error:0.407 Test set accuracy:0.7954
```

### Celda 23: Graficar Error y Precisión
```python
# The training set metrics.
y_training_error = [store_training_loss[i]/float(len(training_images)) for i in range(len(store_training_loss))]
x_training_error = range(1, len(store_training_loss)+1)
y_training_accuracy = [store_training_accurate_pred[i]/ float(len(training_images)) for i in range(len(store_training_accurate_pred))]
x_training_accuracy = range(1, len(store_training_accurate_pred)+1)

# The test set metrics.
y_test_error = [store_test_loss[i]/float(len(test_images)) for i in range(len(store_test_loss))]
x_test_error = range(1, len(store_test_loss)+1)
y_test_accuracy = [store_training_accurate_pred[i]/ float(len(training_images)) for i in range(len(store_training_accurate_pred))]
x_test_accuracy = range(1, len(store_test_accurate_pred)+1)

# Display the plots.
fig, axes = plt.subplots(nrows=1, ncols=2, figsize=(15, 5))
axes[0].set_title('Training set error, accuracy')
axes[0].plot(x_training_accuracy, y_training_accuracy, label = "Training set accuracy")
axes[0].plot(x_training_error, y_training_error, label = "Training set error")
axes[0].set_xlabel("Epochs")
axes[0].legend()
axes[1].set_title('Test set error, accuracy')
axes[1].plot(x_test_accuracy, y_test_accuracy, label = "Test set accuracy")
axes[1].plot(x_test_error, y_test_error, label = "Test set error")
axes[1].set_xlabel("Epochs")
axes[1].legend()
plt.show()
```

---

## 5. 📊 Análisis e Interpretación de las Gráficas

Al evaluar las curvas de las 100 épocas de entrenamiento se obtienen las siguientes conclusiones:

### 1. Evolución del Error (Loss Cuadrático)
- **Comportamiento:** Comienza en $\approx 0.89$ en la época 0 y experimenta una caída rápida en las primeras 20 épocas, estabilizándose entre $0.36$ y $0.40$ al llegar a la época 100.
- **Razón del estancamiento en ~0.40:** En problemas de clasificación multiclase, lo ideal es usar **Cross-Entropy Loss** combinada con **Softmax**. Aquí se utiliza una función de error cuadrático ordinario (MSE) sobre salidas lineales sin acotar. La red consigue separar las clases en términos de valor máximo (`np.argmax`), pero las distancias numéricas al vector binario ($[0, ..., 1]$) difícilmente convergerán a cero perfecto.

### 2. Evolución de la Precisión (Accuracy)
- **Comportamiento:** Arranca en $41.3\%$ (muy superior al azar puro que sería $10\%$) y sube de forma consistente hasta rondar el **$86.7\%$ en entrenamiento** y **$79.5\%\text{--}80\%$ en el conjunto de prueba**.
- **Generalización y Regularización:** La cercanía entre el rendimiento del conjunto de entrenamiento y el conjunto de prueba demuestra que la regularización por **Dropout** cumplió su función: previno que una red con 78,400 parámetros en `weights_1` sobreajustara de forma desmedida a una muestra de tan solo 1,000 imágenes.

---

## 6. 🚨 Registro de Errores Encontrados y Soluciones

A lo largo del taller se presentaron los siguientes fallos típicos:

### 1. `HTTP Error 404: Not Found` al descargar MNIST
- **Causa:** El servidor de Yann LeCun (`http://yann.lecun.com/exdb/mnist/`) dio de baja los enlaces HTTP directos para descargas sin navegador.
- **Solución:** Cambiar la variable `base_url` por el mirror de AWS S3 de PyTorch:
  ```python
  base_url = "https://ossci-datasets.s3.amazonaws.com/mnist/"
  ```

### 2. `jupyter : El término 'jupyter' no se reconoce...` en PowerShell
- **Causa:** Intentar invocar `jupyter notebook` sin activar previamente el entorno virtual `venv` donde fue instalado.
- **Solución:** Activar el entorno virtual antes de ejecutar comandos:
  ```powershell
  cd c:\Users\Diego\Desktop\Taller\NumPy-org-Tutorial-Deep-Learning-from-Scratch
  .\venv\Scripts\Activate.ps1
  jupyter notebook
  ```

### 3. `NameError: name 'train_images' is not defined`
- **Causa:** En la celda se escribió por error `train_images[0]` en vez de `training_images[0]`.

### 4. `NameError: name 'training_images' is not defined` en el bucle de entrenamiento
- **Causa:** Se intentó correr el bloque de entrenamiento de la Sección 3 sin haber ejecutado las celdas de la Sección 2 (donde se declaran `sample = 1000`, `training_images = x_train[0:sample] / 255`, y las etiquetas en one-hot).
- **Solución:** Ejecutar en estricto orden secuencial las celdas 10 a la 17 antes de iniciar el bucle de entrenamiento.

---

## 7. 🔮 Inferencia: ¿Cómo Probar y Clasificar Nuevas Imágenes?

Una vez terminado el entrenamiento, las matrices `weights_1` y `weights_2` contienen el "conocimiento" de la red. Para clasificar cualquier imagen realizamos una **pasada hacia adelante (*Forward Pass*) sin Dropout** (el Dropout se utiliza exclusivamente durante la fase de entrenamiento).

### Opción 1: Probar una imagen específica del conjunto de prueba (`test_images`)

```python
import matplotlib.pyplot as plt
import numpy as np

# 1. Seleccionar cualquier imagen del conjunto de prueba (del 0 al 9999)
indice_imagen = 42

imagen_vector = test_images[indice_imagen]  # Vector de 784 píxeles normalizados
etiqueta_real = np.argmax(test_labels[indice_imagen])

# 2. Inferencia (Forward Pass con los pesos entrenados)
# Paso por la capa oculta con activación ReLU (SIN DROPOUT)
layer_1 = relu(np.dot(imagen_vector, weights_1))

# Paso por la capa de salida
layer_2 = np.dot(layer_1, weights_2)

# 3. La predicción es el índice de la neurona con mayor puntuación
prediccion = np.argmax(layer_2)

# 4. Mostrar la imagen evaluada y el resultado obtenido
plt.imshow(imagen_vector.reshape(28, 28), cmap='gray')
plt.title(f"Predicción de la Red: {prediccion} | Etiqueta Real: {etiqueta_real}", fontsize=14)
plt.axis('off')
plt.show()

# Puntuaciones numéricas asignadas por la red a cada dígito:
print("Puntuaciones de salida por dígito (0 al 9):")
for digito, puntuacion in enumerate(layer_2):
    print(f"Dígito {digito}: {puntuacion:6.2f} {'◄-- PREDICCIÓN' if digito == prediccion else ''}")
```

---

### Opción 2: Función reutilizable para clasificar múltiples imágenes al azar

```python
def predecir_digito(vector_imagen_784):
    """
    Recibe un array de 784 píxeles normalizados (entre 0 y 1).
    Retorna el número predicho y el vector con las 10 puntuaciones.
    """
    l1 = relu(np.dot(vector_imagen_784, weights_1))
    l2 = np.dot(l1, weights_2)
    return np.argmax(l2), l2

# Probar con 5 imágenes elegidas aleatoriamente
indices_azar = np.random.randint(0, len(test_images), size=5)

fig, axes = plt.subplots(1, 5, figsize=(15, 3))
for i, idx in enumerate(indices_azar):
    pred, _ = predecir_digito(test_images[idx])
    real = np.argmax(test_labels[idx])
    
    axes[i].imshow(test_images[idx].reshape(28, 28), cmap='gray')
    axes[i].set_title(f"Pred: {pred} (Real: {real})", 
                      color="green" if pred == real else "red", 
                      fontweight="bold")
    axes[i].axis('off')
plt.show()
```

---

### Opción 3: Clasificar una imagen propia (dibujada en Paint o foto externa)

Para que la red pueda reconocer un dibujo propio (ej. `mi_numero.png`), este debe coincidir exactamente con el formato de entrada de MNIST:
1. Redimensionar a **$28 \times 28$ píxeles**.
2. Convertir a **escala de grises**.
3. Invertir colores si el fondo es blanco y el trazo negro (MNIST utiliza **fondo negro = 0 y trazo blanco = 255**).
4. Aplanar a un vector de 784 y normalizar dividiendo entre `255.0`.

```python
from PIL import Image, ImageOps
import numpy as np

# 1. Cargar la imagen externa
img = Image.open("mi_numero.png").convert("L")  # Convertir a escala de grises
img = img.resize((28, 28))                     # Redimensionar a 28x28 píxeles
img = ImageOps.invert(img)                     # Invertir colores (fondo negro, trazo blanco)

# 2. Convertir a array de NumPy aplanado y normalizado
mi_vector = np.array(img).reshape(784) / 255.0

# 3. Predecir con la función de la Opción 2
pred, puntuaciones = predecir_digito(mi_vector)
print(f"La red neuronal reconoce el número: {pred}")
```
