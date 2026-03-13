# XZeus v 1.0
<br>
<image src="logo.png" width="500" height="300">

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python&logoColor=white)
![C](https://img.shields.io/badge/C-GCC-blue?logo=c&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
<br>
## Descripción

**XZeus v 1.0** Es una aplicación de escritorio desarrollada y diseñada para proteger, empaquetar y ofuscar ejecutables de Windows (`.exe`). La herramienta actúa como un "Packer/Protector", envolviendo el ejecutable original dentro de otro ejecutable que contiene capas de seguridad y desencriptación.

La aplicación cuenta con una interfaz gráfica moderna, oscura y personalizada, con acentos en color neón, diseñada para ser intuitiva pero potente.

## Capturas de Pantalla

<image src="screen.png" width="400" height="400">
*(Nota: La interfaz utiliza un tema oscuro con acentos en verde neón y botones de estilo macOS).*


## Características Principales

La aplicación permite aplicar múltiples capas de protección de forma simultánea:

1.  **Encriptación XOR Dinámica:**
    *   Soporte para clave personalizada o generación de clave aleatoria de 16 bytes.
    *   Utiliza un cifrado XOR con encadenamiento (Chaining), donde el byte anterior afecta al siguiente, dificultando el análisis de patrones.

2.  **Anti-Debugging (Anti-Depuración):**
    *   Implementa comprobaciones nativas utilizando la API de Windows (`IsDebuggerPresent`).
    *   Verifica el PEB (`Process Environment Block`) para detectar depuradores activos.

3.  **Verificación de Integridad (CRC Check):**
    *   Calcula el hash **CRC32** del ejecutable original.
    *   Muestra el código CRC en el log de proceso.
    *   El ejecutable protegido verifica su integridad antes de ejecutarse. Si el archivo ha sido modificado, se cierra inmediatamente.

4.  **Ejecución Dinámica (RunPE):**
    *   El ejecutable resultante extrae el payload en memoria o en un archivo temporal y lo ejecuta sin dejar rastros en disco permanentes (técnica de dropper).

5.  **Compresión UPX Scramble Xmodificada:**
    *   Integración con UPX para comprimir el ejecutable final y reducir su tamaño.
    *   **Fake UPX Version:** Permite "parchear" la cabecera de UPX con un número de versión falso (ej. v99) para confundir a las herramientas de análisis automático y desempaquetadores.

6.  **Compilación Automática:**
    *   Genera código C "al vuelo", lo compila con GCC y lo optimiza (`-Os`, `-s`).

## Requisitos Previos

Para que XZeus v 1.0 funcione correctamente, el sistema debe cumplir con los siguientes requisitos:

### 1. Python y Librerías
*   **Python 3.x** instalado.
*   Librería **Pillow** (para el manejo de imágenes del logo):
    ```bash
    pip install Pillow
    ```

### 2. Compilador GCC (MinGW-w64)
Es imprescindible tener `gcc` instalado y añadido a las variables de entorno del sistema (PATH), ya que la aplicación genera código C y lo compila en tiempo real.

### 3. UPX (Opcional)
Si deseas utilizar la función de compresión, necesitas el ejecutable `upx.exe`.
*   Descárgalo desde [upx.github.io](https://upx.github.io/).
*   Coloca `upx.exe` en la misma carpeta que el script de Python o en una ruta accesible por el sistema.

## Instalación y Uso

1.  **Descarga:** Guarda el script principal (por ejemplo, `Xprotector.py`) en una carpeta.
2.  **Logo (Opcional):** Si deseas ver el logo en la interfaz, coloca una imagen llamada `logo.png` en la misma carpeta.
3.  **Ejecución:**
    ```bash
    python XZeus.py
    ```

### Guía Rápida de Uso

1.  **Archivo Original:** Haz clic en el botón `...` para seleccionar el ejecutable `.exe` que deseas proteger.
2.  **Guardar Como:** Define el nombre del archivo de salida (por defecto añade `_protected` al nombre).
3.  **Clave XOR:** (Opcional) Escribe una clave o déjalo vacío para usar una clave aleatoria segura.
4.  **Opciones:** Marca las casillas de las protecciones que deseas aplicar:
    *   `Anti-Debug`
    *   `Encriptar XOR`
    *   `CRC Check` (Mostrará el hash en la ventana de log).
    *   `Comprimir` (Requiere UPX).
    *   `Fake UPX Ver` (Si comprimes, puedes poner un número falso como 99).
5.  **Proteger:** Haz clic en **PROTEGER Y COMPILAR**. El log mostrará el progreso: verificación de GCC, cálculo de CRC, generación de código, compilación y compresión.

## Detalles Técnicos

El script funciona realizando las siguientes operaciones en segundo plano:

1.  **Lectura:** Lee el binario del archivo original.
2.  **Cálculo:** Si CRC está activo, calcula el CRC32 y lo inyecta en el código C.
3.  **Encriptación:** Aplica XOR encadenado a los bytes del archivo.
4.  **Generación de Código:** Crea un archivo temporal `temp_build.c` con un array de bytes del payload encriptado y la lógica de desencriptado.
5.  **Compilación:** Ejecuta `gcc` para compilar el stub C en un nuevo `.exe`.
6.  **Parcheo:** Si está activo, usa Python para abrir el archivo compilado y cambiar el byte de versión de la firma `UPX!`.

## Disclaimer (Aviso Legal)

Esta herramienta está diseñada con fines educativos y de investigación en seguridad informática (Ofuscación y Protección de Software). El uso de técnicas como RunPE y Anti-Debugging puede ser detectado por antivirus como comportamiento sospechoso.

**No utilices esta herramienta para distribuir malware o software malicioso.** El autor no se hace responsable del mal uso de este software. Úsalo bajo tu propia responsabilidad y siempre en entornos controlados o con software del que seas propietario.

## Créditos

*   **Desarrollador:** Rodolfo Hernandez Baz
*   **GUI:** Tkinter / ttk
*   **Compilación:** GCC (MinGW)
*   **Compresión:** UPX
```
