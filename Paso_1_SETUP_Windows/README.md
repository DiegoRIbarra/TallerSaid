# 🪟 Paso 1: Configuración del Entorno en Windows

Guía detallada paso a paso para preparar el entorno de desarrollo del **Taller de Redes Neuronales Feedforward (MNIST desde Cero)** en Windows 10 u 11 usando PowerShell o Símbolo del sistema (CMD).

---

## 📋 Prerrequisitos

Antes de comenzar, debes tener instalado **Python 3.10 o superior** en tu sistema.

### 1. Descargar e Instalar Python

1. Descarga el instalador oficial de Python desde [python.org/downloads](https://www.python.org/downloads/).
2. Ejecuta el archivo instalador `.exe`.
3. ⚠️ **MUY IMPORTANTE (Paso crítico):** En la primera pantalla del instalador, marca la casilla que dice:
   > ☑️ **"Add python.exe to PATH"** (o "Agregar Python al PATH").
   *(Si no marcas esta casilla, la terminal no reconocerá el comando `python`).*
4. Haz clic en **Install Now** y completa la instalación.

### 2. Verificar la instalación en PowerShell

Abre **PowerShell** (presiona la tecla Windows, escribe `powershell` y presiona Enter). Ejecuta:

```powershell
python --version
pip --version
```

Deberías ver algo similar a:
```text
Python 3.11.x (o superior)
pip 23.x.x (o superior)
```

---

## 🚀 Guía Paso a Paso

### Paso 1: Abrir la terminal y navegar a la carpeta del taller

En PowerShell, dirígete a la carpeta donde tienes los archivos del taller:

```powershell
cd C:\Users\TuUsuario\Desktop\TallerSaid
```
*(Reemplaza `C:\Users\TuUsuario\Desktop\TallerSaid` por la ruta real de tu carpeta).*

> **Tip:** También puedes abrir la carpeta en el Explorador de archivos de Windows, hacer clic en la barra de direcciones superior, escribir `powershell` y presionar Enter.

---

### Paso 2: Crear el entorno virtual

Crea un entorno virtual aislado para no interferir con otros programas:

```powershell
python -m venv venv
```

Esto generará una carpeta llamada `venv` dentro de tu proyecto.

---

### Paso 3: Activar el entorno virtual

#### Opción A: En PowerShell (Recomendado)

```powershell
.\venv\Scripts\Activate.ps1
```

> **⚠️ ¿Te apareció un error de color rojo sobre directivas de ejecución?**  
> Si PowerShell muestra: `...no se puede cargar el archivo Activate.ps1 porque la ejecución de scripts está deshabilitada en este sistema`, ejecuta este comando una sola vez para permitir scripts locales:
> ```powershell
> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
> ```
> Confirma presionando la tecla `S` (o `Y` en inglés) y Enter. Luego, vuelve a ejecutar `.\venv\Scripts\Activate.ps1`.

#### Opción B: En Símbolo del Sistema (CMD)

Si prefieres usar la consola clásica de CMD:
```cmd
.\venv\Scripts\activate.bat
```

> **✅ Verificación:** Verás que aparece el prefijo `(venv)` al inicio de la línea de comandos, confirmando que el entorno está activo:  
> `(venv) PS C:\Users\TuUsuario\Desktop\TallerSaid>`

---

### Paso 4: Actualizar pip e instalar las dependencias

Con el entorno `(venv)` activo, ejecuta:

```powershell
python -m pip install --upgrade pip
pip install numpy matplotlib jupyter
```

#### ¿Qué se instala y para qué se usa?

| Paquete | Versión Recomendada | Propósito en el Taller |
| :--- | :--- | :--- |
| **`numpy`** | $\ge 2.0$ | Operaciones matemáticas con matrices, álgebra lineal y algoritmo de la red neuronal. |
| **`matplotlib`** | $\ge 3.8$ | Graficar imágenes de los dígitos MNIST y curvas de rendimiento (error y precisión). |
| **`jupyter`** | Notebook 7+ | Entorno interactivo para ejecutar el código celda por celda. |

---

### Paso 5: Verificar que la instalación fue exitosa

Ejecuta el siguiente comando en PowerShell:

```powershell
python -c "import numpy as np; import matplotlib.pyplot as plt; print('NumPy versión:', np.__version__); print('Matplotlib versión:', plt.matplotlib.__version__)"
```

Si muestra las versiones sin errores, tu entorno está listo.

---

### Paso 6: Lanzar Jupyter Notebook

Con el entorno virtual activado, escribe:

```powershell
jupyter notebook
```

Se abrirá automáticamente una pestaña en tu navegador web en la dirección `http://localhost:8888`.

#### Cómo crear y usar tu libreta de trabajo:
1. En la parte superior derecha de la pantalla de Jupyter, haz clic en **"New"** → **"Python 3 (ipykernel)"**.
2. Se abrirá un notebook en blanco.
3. Copia y pega de forma secuencial los bloques de código que se encuentran en el archivo [Taller.md](../Taller.md).
4. Ejecuta cada celda pulsando **Shift + Enter**.

---

## 🔄 Comandos del Día a Día

Cada vez que quieras reanudar el taller en Windows:

```powershell
# 1. Navegar a la carpeta del proyecto
cd C:\Users\TuUsuario\Desktop\TallerSaid

# 2. Activar el entorno virtual
.\venv\Scripts\Activate.ps1

# 3. Iniciar Jupyter
jupyter notebook
```

Para salir del entorno virtual cuando termines:
```powershell
deactivate
```

---

## 🛠️ Solución de Problemas Comunes en Windows

### ❌ `'python' no se reconoce como un comando interno o externo`
- **Causa:** Python no se agregó a la variable de entorno `PATH` durante la instalación.
- **Solución:** Vuelve a ejecutar el instalador de Python descargado, selecciona **"Modify"** y asegúrate de activar la casilla **"Add Python to PATH"**. O busca en el menú inicio "Editar las variables de entorno del sistema" y agrega la ruta de instalación de Python al `Path`.

### ❌ Error al descargar el dataset MNIST (`HTTP Error 404: Not Found`)
- **Causa:** El servidor original de Yann LeCun ya no admite descargas automáticas por script.
- **Solución:** En el código del taller ya está configurado el mirror confiable de AWS S3:
  ```python
  base_url = "https://ossci-datasets.s3.amazonaws.com/mnist/"
  ```

---

## ➡️ Siguiente Paso

Una vez preparado tu entorno de Windows, ve al documento del taller:  
👉 **[Ir a Taller.md](../Taller.md)**
