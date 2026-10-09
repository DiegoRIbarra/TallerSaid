# 🤖 Taller de Inteligencia Artificial: Red Neuronal Feedforward desde Cero

¡Bienvenido/a al taller de **Feed Forward Neural Network (FFNN)**! 

En este taller aprenderás cómo funciona el aprendizaje profundo (*Deep Learning*) desde sus cimientos matemáticos, construyendo una red neuronal completa con **Python y NumPy**, sin recurrir a frameworks de alto nivel como TensorFlow o PyTorch.

---

## 🎯 ¿Qué es el taller y en qué consiste?

El objetivo central de este taller es **crear y entrenar una Inteligencia Artificial capaz de identificar números escritos a mano (dígitos del 0 al 9)** utilizando el dataset estándar en la industria: **MNIST**.

### ¿Cómo lo lograremos?

1. **Recepción de la imagen:** Cada número manuscrito es una imagen pequeña en escala de grises de $28 \times 28$ píxeles. La imagen se aplana para formar un vector de **784 valores numéricos**, que entran directamente a la red.
2. **Procesamiento en la capa oculta:** Los 784 valores se multiplican por una matriz de pesos (`weights_1`) hacia **100 neuronas ocultas**, se activan mediante la función **ReLU** y se regularizan con la técnica de **Dropout** para evitar que la red simplemente memorice las imágenes.
3. **Predicción en la salida:** Las activaciones pasan a través de una segunda matriz de pesos (`weights_2`) hacia **10 neuronas de salida** (una para cada dígito: 0, 1, 2, ..., 9). La neurona con la puntuación más alta (`argmax`) representa el número que la red predice.
4. **Aprendizaje (Backpropagation):** Durante 100 épocas, la red compara su predicción con la etiqueta real, calcula el error cometido y ajusta sus pesos mediante el algoritmo de **Descenso de Gradiente**, logrando alcanzar aproximadamente un **80% de precisión** con tan solo 1,000 ejemplos de muestra.
5. **Inferencia interactiva:** Al finalizar el entrenamiento, podrás poner a prueba la IA clasificando imágenes al azar o incluso imágenes dibujadas por ti en Paint u otro editor.

---

## 📐 Arquitectura de la Red Neuronal

```text
       [ ENTRADA ]                [ CAPA OCULTA ]                [ SALIDA ]
Imagen 28x28 aplanada        100 neuronas intermedias        10 puntuaciones
   (784 píxeles)                                             (Dígitos 0 al 9)
        │                                                           │
        │───► Matriz W1 (784 x 100) ──► ReLU + Dropout (p=0.5) ───►│───► Matriz W2 (100 x 10) ──► argmax = DÍGITO
```

---

## 🧭 Guía de Pasos del Taller

Para realizar el taller de manera ordenada, sigue los siguientes pasos:

### 🔹 Paso 1: Configuración del Entorno (SETUP)
Selecciona la guía correspondiente a tu sistema operativo para instalar las herramientas y activar tu entorno virtual:

* 🐧 **[Paso 1: Setup en Linux](Paso_1_SETUP_Linux/README.md)** (Ubuntu, Debian, Fedora, Arch, etc.)
* 🪟 **[Paso 1: Setup en Windows](Paso_1_SETUP_Windows/README.md)** (PowerShell y CMD)
* 🍎 **[Paso 1: Setup en macOS](Paso_1_SETUP_Mac/README.md)** (Apple Silicon M1/M2/M3/M4 e Intel)

---

### 🔹 Paso 2: Desarrollo Práctico del Taller
Una vez configurado y activo tu entorno virtual con Jupyter Notebook, abre el documento con todo el código explicado y ejecútalo celda por celda:

* 🧠 **[Paso 2: Taller Práctico Paso a Paso](Taller.md)**

---

## 📝 Preguntas de Evaluación

Para comprobar que comprendiste los puntos clave del taller, responde estas **dos preguntas sencillas**:

### 1. ¿Qué aprende a hacer la red neuronal en este taller?
> **Respuesta:** Aprende a observar imágenes de números escritos a mano e identificar qué dígito es (del 0 al 9).

### 2. ¿Cómo decide la red cuál número es el que está en la imagen?
> **Respuesta:** La red calcula una puntuación para cada número posible (del 0 al 9) y elige como resultado final el que tenga la puntuación más alta.

---

## 🛠️ Tecnologías Requeridas

* **Python 3.10+**
* **NumPy:** Álgebra lineal y manipulación matricial de alta velocidad.
* **Matplotlib:** Visualización de dígitos y gráficas de aprendizaje.
* **Jupyter Notebook:** Entorno de ejecución interactivo.
