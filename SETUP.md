# 🛠️ Guía de Configuración del Entorno

**Tutorial:** Deep Learning on MNIST — desde cero con NumPy  
**Autor de la guía:** Generado como apoyo para el taller  
**Fecha:** Septiembre 2026

---

## Prerrequisitos

Antes de empezar, asegúrate de tener instalado:

- **Python 3.10+** → [Descargar Python](https://www.python.org/downloads/)
- **pip** (viene incluido con Python)
- **Git** (opcional, para clonar el repositorio)

### Verificar la instalación

Abre una terminal (PowerShell en Windows) y ejecuta:

```powershell
python --version
pip --version
```

Deberías ver algo como:

```
Python 3.14.5
pip 26.1.1 from ... (python 3.14)
```

> **⚠️ Nota:** Si el comando `python` no es reconocido, asegúrate de que Python esté agregado al `PATH` del sistema. Durante la instalación de Python, marca la casilla **"Add Python to PATH"**.

---

## Paso 1: Clonar o descargar el repositorio

Si aún no tienes el repositorio del tutorial, clónalo con Git:

```powershell
cd C:\Users\TuUsuario\Desktop\Taller
git clone https://github.com/8bitmp3/NumPy-org-Tutorial-Deep-Learning-from-Scratch.git
cd NumPy-org-Tutorial-Deep-Learning-from-Scratch
```

O descárgalo como ZIP desde GitHub y descomprímelo en tu carpeta de trabajo.

---

## Paso 2: Crear un entorno virtual

Un entorno virtual aísla las dependencias del proyecto para no afectar tu instalación global de Python.

```powershell
cd C:\Users\TuUsuario\Desktop\Taller\NumPy-org-Tutorial-Deep-Learning-from-Scratch
python -m venv venv
```

Esto crea una carpeta `venv/` dentro del proyecto.

---

## Paso 3: Activar el entorno virtual

### Windows (PowerShell)

```powershell
.\venv\Scripts\Activate.ps1
```

### Windows (CMD)

```cmd
.\venv\Scripts\activate.bat
```

### macOS / Linux

```bash
source venv/bin/activate
```

> **✅ Confirmación:** Verás `(venv)` al inicio de tu línea de terminal, indicando que el entorno está activo.

> **⚠️ Problema con PowerShell?** Si recibes un error de permisos de ejecución de scripts, ejecuta primero:
> ```powershell
> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
> ```

---

## Paso 4: Instalar las dependencias

Con el entorno virtual activado, instala los paquetes necesarios:

```powershell
pip install numpy matplotlib jupyter
```

### ¿Qué se instala y para qué?

| Paquete | Versión instalada | Uso en el tutorial |
|---|---|---|
| **numpy** | 2.5.3 | Operaciones matemáticas, matrices, la red neuronal completa |
| **matplotlib** | 3.11.2 | Visualizar imágenes MNIST y gráficas de entrenamiento |
| **jupyter** | (Notebook 7.6.3) | Ejecutar el código celda por celda de forma interactiva |

### Verificar la instalación

```powershell
python -c "import numpy; print('NumPy:', numpy.__version__)"
python -c "import matplotlib; print('Matplotlib:', matplotlib.__version__)"
jupyter --version
```

---

## Paso 5: Lanzar Jupyter Notebook

Con el entorno virtual activo, ejecuta:

```powershell
jupyter notebook
```

Esto abrirá automáticamente tu navegador en `http://localhost:8888` con el explorador de archivos de Jupyter.

### Crear un nuevo notebook

1. En la interfaz de Jupyter, haz clic en **"New"** → **"Python 3"**
2. Se abrirá un notebook vacío donde puedes ir pegando y ejecutando cada bloque de código del tutorial (`tutorial-deep-learning-on-mnist.md`)
3. Ejecuta cada celda con **Shift + Enter**

---

## Paso 6: Módulos de Python estándar (no requieren instalación)

El tutorial también usa estos módulos que **ya vienen incluidos con Python**:

| Módulo | Uso |
|---|---|
| `urllib.request` | Descargar los archivos MNIST desde internet |
| `gzip` | Descomprimir los archivos `.gz` del dataset |
| `pickle` | Serializar/deserializar datos (formato pickle) |

No necesitas instalar nada extra para estos.

---

## Resumen de la estructura del proyecto

```
NumPy-org-Tutorial-Deep-Learning-from-Scratch/
├── venv/                          ← Entorno virtual (creado en el paso 2)
├── tutorial-deep-learning-on-mnist.md  ← El tutorial completo (lo que debes seguir)
├── tutorial-deep-learning-on-mnist/    ← Imágenes y recursos del tutorial
│   ├── output_19_0.svg
│   ├── output_20_0.svg
│   ├── output_70_0.svg
│   └── tutorial-deep-learning-on-mnist.png
├── README.md
└── SETUP.md                       ← Esta guía
```

---

## Referencia rápida: Comandos del día a día

Cada vez que quieras retomar el tutorial:

```powershell
# 1. Ir a la carpeta del proyecto
cd C:\Users\TuUsuario\Desktop\Taller\NumPy-org-Tutorial-Deep-Learning-from-Scratch

# 2. Activar el entorno virtual
.\venv\Scripts\Activate.ps1

# 3. Lanzar Jupyter
jupyter notebook
```

Para desactivar el entorno virtual cuando termines:

```powershell
deactivate
```

---

## Solución de problemas comunes

### ❌ `python` no se reconoce como comando
- Reinstala Python marcando **"Add Python to PATH"**
- O usa `python3` en lugar de `python`

### ❌ Error de permisos al activar el entorno virtual en PowerShell
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### ⚠️ Error al descargar MNIST (`HTTP Error 404: Not Found`)

La URL original del tutorial (`http://yann.lecun.com/exdb/mnist/`) **ya no funciona**.

Cuando llegues al paso de descarga del dataset, **cambia la URL base** en tu notebook:

```python
# ❌ URL original (ya no funciona):
# base_url = "http://yann.lecun.com/exdb/mnist/"

# ✅ Mirror alternativo (funciona):
base_url = "https://ossci-datasets.s3.amazonaws.com/mnist/"
```

Todo lo demás del código de descarga queda exactamente igual.

### ❌ `ModuleNotFoundError: No module named 'numpy'`
- Asegúrate de tener el entorno virtual **activado** (debes ver `(venv)` en la terminal)
- Si persiste: `pip install numpy`

---

> **💡 Tip:** Una vez configurado el entorno, sigue el tutorial paso a paso en el archivo [`tutorial-deep-learning-on-mnist.md`](tutorial-deep-learning-on-mnist.md).
