# 🐧 Paso 1: Configuración del Entorno en Linux

Guía detallada para preparar el entorno de desarrollo del **Taller de Redes Neuronales Feedforward (MNIST desde Cero)** en distribuciones Linux (Ubuntu, Debian, Fedora, Arch, Linux Mint, etc.).

---

## 📋 Prerrequisitos

Antes de iniciar, necesitas tener instalado **Python 3.10 o superior**, el gestor de paquetes **pip** y el módulo de entornos virtuales **venv**.

### 1. Actualizar repositorios e instalar paquetes necesarios

Abre tu terminal (`Ctrl + Alt + T`) y ejecuta según tu distribución:

#### En Ubuntu / Debian / Linux Mint:
```bash
sudo apt update
sudo apt install -y python3 python3-pip python3-venv git
```

#### En Fedora / RHEL:
```bash
sudo dnf install -y python3 python3-pip git
```

#### En Arch Linux / Manjaro:
```bash
sudo pacman -Syu python python-pip git
```

### 2. Verificar la instalación

Ejecuta en tu terminal:

```bash
python3 --version
pip3 --version
```

Deberías ver una salida similar a:
```text
Python 3.10.x (o superior)
pip 22.x.x (o superior)
```

---

## 🚀 Guía Paso a Paso

### Paso 1: Ubicarte en la carpeta del taller

Navega a la carpeta donde clonaste o descargaste este proyecto:

```bash
cd ~/Documentos/IA/TallerSaid
```
*(Ajusta la ruta según la ubicación real en tu equipo).*

---

### Paso 2: Crear el entorno virtual

Crear un entorno virtual aísla las librerías del taller para evitar conflictos con los paquetes del sistema:

```bash
python3 -m venv venv
```

> **Nota:** Esto creará un directorio llamado `venv/` dentro de la carpeta del taller.

---

### Paso 3: Activar el entorno virtual

Activa el entorno con:

```bash
source venv/bin/activate
```

> **✅ Verificación:** Observarás que el prompt de tu terminal ahora comienza con `(venv)`, por ejemplo:  
> `(venv) usuario@equipo:~/Documentos/IA/TallerSaid$`

---

### Paso 4: Actualizar pip e instalar dependencias

Con el entorno `(venv)` activo, instala las librerías requeridas:

```bash
pip install --upgrade pip
pip install numpy matplotlib jupyter
```

#### ¿Qué se instala y para qué se usa?

| Paquete | Versión Recomendada | Propósito en el Taller |
| :--- | :--- | :--- |
| **`numpy`** | $\ge 2.0$ | Cálculos matriciales, álgebra lineal y construcción completa de la red neuronal desde cero. |
| **`matplotlib`** | $\ge 3.8$ | Graficar imágenes de los dígitos MNIST y curvas de error y precisión. |
| **`jupyter`** | Notebook 7+ | Ejecución interactiva del código bloque por bloque. |

---

### Paso 5: Comprobar que las librerías funcionan correctamente

Ejecuta esta prueba rápida en la terminal:

```bash
python3 -c "import numpy as np; import matplotlib.pyplot as plt; print('✅ NumPy versión:', np.__version__); print('✅ Matplotlib versión:', plt.matplotlib.__version__)"
```

Si no arroja ningún error y muestra las versiones instaladas, la instalación fue exitosa.

---

### Paso 6: Iniciar Jupyter Notebook

Inicia el entorno de notebooks:

```bash
jupyter notebook
```

Esto abrirá automáticamente tu navegador web predeterminado en `http://localhost:8888`.

#### Para comenzar a trabajar:
1. En la interfaz web de Jupyter, haz clic en el botón superior derecho: **"New"** → **"Python 3 (ipykernel)"**.
2. Se abrirá una nueva libreta en blanco.
3. Puedes ir copiando y ejecutando las celdas de código del archivo [Taller.md](../Taller.md).
4. Para ejecutar una celda, presiona **Shift + Enter**.

---

## 🔄 Comandos del Día a Día

Cada vez que vayas a continuar el taller:

```bash
# 1. Entrar a la carpeta
cd ~/Documentos/IA/TallerSaid

# 2. Activar el entorno
source venv/bin/activate

# 3. Lanzar Jupyter
jupyter notebook
```

Cuando termines de trabajar y quieras cerrar el entorno virtual:
```bash
deactivate
```

---

## 🛠️ Solución de Problemas Comunes en Linux

### ❌ Error: `No module named venv`
- **Causa:** En distribuciones basadas en Debian/Ubuntu el paquete de entornos virtuales viene separado.
- **Solución:** Ejecuta `sudo apt install python3-venv` e intenta crear el entorno nuevamente.

### ❌ Error: `externally-managed-environment` al usar pip
- **Causa:** Intentaste instalar paquetes directamente en el sistema global sin haber activado el entorno virtual.
- **Solución:** Asegúrate de activar primero el entorno con `source venv/bin/activate` antes de correr `pip install`.

### ❌ Error al descargar MNIST (`HTTP Error 404: Not Found`)
- **Causa:** El servidor original de Yann LeCun ya no permite descargas directas.
- **Solución:** En el código del taller se utiliza el mirror oficial de AWS:
  ```python
  base_url = "https://ossci-datasets.s3.amazonaws.com/mnist/"
  ```

---

## ➡️ Siguiente Paso

Una vez configurado tu entorno, continúa con la guía práctica:  
👉 **[Ir a Taller.md](../Taller.md)**
