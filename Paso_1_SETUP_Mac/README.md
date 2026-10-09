# 🍎 Paso 1: Configuración del Entorno en macOS

Guía detallada paso a paso para preparar el entorno de desarrollo del **Taller de Redes Neuronales Feedforward (MNIST desde Cero)** en macOS (compatible tanto con procesadores Apple Silicon M1/M2/M3/M4 como procesadores Intel).

---

## 📋 Prerrequisitos

macOS incluye herramientas de desarrollo a través de la Terminal (`zsh`).

### 1. Verificar o instalar Python 3

Abre la aplicación **Terminal** (presiona `Cmd + Espacio`, escribe `Terminal` y presiona Enter).

Verifica si tienes Python 3 instalado ejecutando:

```bash
python3 --version
```

- Si tienes **Python 3.10 o superior**, puedes continuar.
- Si macOS te pide instalar las **Herramientas de línea de comandos para desarrolladores (Command Line Tools)**, pulsa **Instalar** y espera a que termine. También puedes forzar la instalación ejecutando:
  ```bash
  xcode-select --install
  ```
- Si no tienes Python o tienes una versión muy antigua, instálalo fácilmente con cualquiera de estas opciones:
  - **Opción recomendada (Oficial):** Descarga el instalador de macOS desde [python.org/downloads](https://www.python.org/downloads/).
  - **Vía Homebrew:** Si tienes instalado [Homebrew](https://brew.sh), ejecuta:
    ```bash
    brew install python
    ```

---

## 🚀 Guía Paso a Paso

### Paso 1: Ubicarte en la carpeta del taller

En la Terminal de macOS, dirígete a la carpeta del proyecto:

```bash
cd ~/Documentos/IA/TallerSaid
```
*(O arrastra la carpeta del taller directamente a la ventana de la Terminal después de escribir `cd `).*

---

### Paso 2: Crear el entorno virtual

Genera un entorno virtual dedicado para las dependencias del taller:

```bash
python3 -m venv venv
```

Esto creará la carpeta aislada `venv/` dentro de tu proyecto.

---

### Paso 3: Activar el entorno virtual

Ejecuta el siguiente comando en la Terminal:

```bash
source venv/bin/activate
```

> **✅ Verificación:** Observarás `(venv)` al inicio de tu línea de comandos en la Terminal:  
> `(venv) usuario@MacBook-Pro TallerSaid %`

---

### Paso 4: Actualizar pip e instalar dependencias

Con el entorno `(venv)` activado, instala los paquetes requeridos:

```bash
pip install --upgrade pip
pip install numpy matplotlib jupyter
```

#### ¿Qué se instala y para qué se usa?

| Paquete | Versión Recomendada | Propósito en el Taller |
| :--- | :--- | :--- |
| **`numpy`** | $\ge 2.0$ | Cálculos matriciales de alta velocidad, optimizados de forma nativa para Apple Silicon / Intel. |
| **`matplotlib`** | $\ge 3.8$ | Visualización de las imágenes de dígitos manuscritos y gráficas de aprendizaje. |
| **`jupyter`** | Notebook 7+ | Entorno interactivo para ejecutar el taller celda por celda en el navegador. |

---

### Paso 5: Comprobar la instalación

Verifica que las librerías se importen sin problemas en tu Mac:

```bash
python3 -c "import numpy as np; import matplotlib.pyplot as plt; print('NumPy versión:', np.__version__); print('Matplotlib versión:', plt.matplotlib.__version__)"
```

Si muestra las versiones instaladas correctamente, el entorno está preparado.

---

### Paso 6: Lanzar Jupyter Notebook

Inicia el servidor local de Jupyter ejecutando:

```bash
jupyter notebook
```

Esto abrirá automáticamente tu navegador (Safari, Chrome o tu navegador predeterminado) en `http://localhost:8888`.

#### Cómo empezar a trabajar:
1. En la esquina superior derecha de la interfaz de Jupyter, haz clic en **"New"** → **"Python 3 (ipykernel)"**.
2. Se abrirá una libreta interactiva en blanco.
3. Copia y ejecuta secuencialmente los bloques de código que se detallan en el archivo [Taller.md](../Taller.md).
4. Para ejecutar una celda, presiona **Shift + Return**.

---

## 🔄 Comandos del Día a Día

Cada vez que vayas a retomar el taller en tu Mac:

```bash
# 1. Ir a la carpeta del proyecto
cd ~/Documentos/IA/TallerSaid

# 2. Activar el entorno virtual
source venv/bin/activate

# 3. Iniciar Jupyter
jupyter notebook
```

Para salir del entorno virtual cuando termines:
```bash
deactivate
```

---

## 🛠️ Solución de Problemas Comunes en macOS

### ❌ Error de certificados SSL al descargar MNIST (`SSL: CERTIFICATE_VERIFY_FAILED`)
- **Causa:** En algunas instalaciones de Python en macOS desde python.org, los certificados SSL del sistema no se instalan por defecto.
- **Solución:** Abre la carpeta de Python en Finder (`/Applications/Python 3.x/`) y haz doble clic sobre el archivo ejecutable **`Install Certificates.command`**.

### ❌ Error al descargar el dataset MNIST (`HTTP Error 404: Not Found`)
- **Causa:** El enlace clásico original de Yann LeCun ya no permite descargas directas.
- **Solución:** En el archivo [Taller.md](../Taller.md) se utiliza el enlace espejo funcional de AWS S3:
  ```python
  base_url = "https://ossci-datasets.s3.amazonaws.com/mnist/"
  ```

---

## ➡️ Siguiente Paso

Una vez listo el entorno, continúa con el desarrollo de la red neuronal:  
👉 **[Ir a Taller.md](../Taller.md)**
