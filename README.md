# 🧱 Breakout - Proyecto Final (OyAC)

![Language](https://img.shields.io/badge/Language-C%20%2F%20ASM%20(x86)-blue?style=for-the-badge&logo=c)
![Library](https://img.shields.io/badge/Library-SDL3-EA312F?style=for-the-badge&logo=sdl)
![IDE](https://img.shields.io/badge/IDE-Visual%20Studio%202022-5C2D91?style=for-the-badge&logo=visual-studio)
![Academic](https://img.shields.io/badge/Course-Org.%20y%20Arq.%20de%20Computadoras-green?style=for-the-badge)

Un clon avanzado del clásico juego **Breakout** desarrollado en **C** puro y **SDL3**, destacando por su motor híbrido: **toda la lógica crítica (física, colisiones, máquina de estados y algoritmos de ordenamiento) está implementada nativamente en Lenguaje Ensamblador (MASM x86).**

A diferencia de implementaciones estándar, aquí **no se utilizan librerías de física**. Toda la detección de colisiones, cálculo de trayectorias, ordenamiento de puntajes y lógica de juego ha sido escrita a mano en bloques de ensamblador en línea (`__asm`), utilizando la FPU (Floating Point Unit) para una precisión matemática superior.

## 📸 Capturas de Pantalla

| Menú Principal | Gameplay (Nivel 1) |
|:---:|:---:|
| ![Menú](https://github.com/user-attachments/assets/1f8b0d76-23e2-482d-9bdd-52c4cd767028) | ![Gameplay](https://github.com/user-attachments/assets/165bb9c3-bcfa-4c33-9c0c-3631cc51414d) |
| *Acceso a modos y créditos* | *Física de rebote dinámica* |

| Pantalla de Victoria | Créditos |
|:---:|:---:|
| ![Victoria](https://github.com/user-attachments/assets/e5199888-28ce-424d-add2-075768032872) | ![Créditos](https://github.com/user-attachments/assets/b3f93282-b019-43df-a18e-04f445b950a7) |
| *Mensaje al completar los 10 niveles* | *Reconocimiento a los autores* |

---

## 🚀 Características Técnicas

### 🧠 Motor Híbrido C/ASM
El núcleo del juego combina la facilidad de C para gráficos con la potencia de ASM para lógica:
* **Física de la Pelota:** Cálculos de trayectoria y velocidad utilizando registros de punto flotante (FPU `fld`, `fstp`, `fadd`).
* **Sistema de Colisiones:** Detección de impacto AABB optimizada en ensamblador con cálculo de rebotes.
* **Máquina de Estados:** Gestión del flujo del juego (Menú -> Juego -> Pausa -> Victoria) mediante saltos condicionales (`cmp`, `je`, `jmp`).
* **Ordenamiento (Bubble Sort):** Implementación manual de ordenamiento de burbuja en ASM para organizar la tabla de puntuaciones (`Hall of Fame`) en tiempo real.
* **Generación de Mapas:** Lógica de lectura de matrices y asignación de propiedades de ladrillos hecha en bajo nivel.

### 🎮 Mecánicas de Juego
* **10 Niveles Progresivos:** Diseños únicos definidos por matrices de patrones.
* **Dificultad Personalizable:** Menú de ajustes para modificar la velocidad de la pelota, velocidad de la paleta y nivel inicial.
* **Resistencia de Ladrillos:** Mecánica de "multigolpe" (indicada por colores) gestionada en memoria.
* **Física "Factor Caos":** Algoritmo de rebote que introduce micro-perturbaciones aleatorias en el ángulo de la pelota para evitar bucles infinitos.
* **Persistencia:** Sistema de guardado y lectura de récords en archivo binario (`scores.dat`).

### 🎨 Estética Retro
* Fuente tipográfica estilo Arcade (`RETRO.TTF`).
* Renderizado de corazones (vidas) mediante primitivas geométricas (Pixel Art procedural).
* Interfaz limpia utilizando SDL3_ttf para renderizado de texto de alta calidad.

## 🕹️ Controles

| Contexto | Tecla | Acción |
| :--- | :---: | :--- |
| **Menú Principal** | `Enter` | Iniciar Juego |
| | `Tab` | Ver Mejores Puntuaciones |
| | `A` | Ajustes (Dificultad/Nivel) |
| | `C` | Ver Créditos |
| | `Esc` | Salir |
| **En Juego** | `←` / `→` | Mover la Paleta |
| | `Enter` | Pausar / Reanudar |
| **Ajustes** | `↑` / `↓` | Seleccionar opción |
| | `←` / `→` | Cambiar valor |

---

## 🛠️ Guía de Instalación y Compilación

⚠️ **Requisito Crítico:** Este proyecto utiliza bloques de ensamblador en línea (`__asm`), los cuales **solo son soportados por el compilador MSVC en arquitectura x86 (32-bits)**. Si intentas compilar en x64, obtendrás errores de compilación.

### 1. Preparación de Librerías (SDL3)
El proyecto requiere **SDL3** y **SDL3_ttf**.
1.  Descargar **SDL3-devel-win32-vc.zip** desde [libsdl.org](https://github.com/libsdl-org/SDL/releases).
2.  Descargar **SDL3_ttf-devel-vc.zip** desde [el repo de SDL_ttf](https://github.com/libsdl-org/SDL_ttf/releases).
3.  Descomprimir ambas en una ruta conocida (ej. `C:\Librerias\SDL3` y `C:\Librerias\SDL3_ttf`).

### 2. Configuración en Visual Studio 2022
Abre `BreakoutGame.sln` y sigue estos pasos:

#### Paso A: Configurar Arquitectura
En la barra superior de Visual Studio, asegúrate de que el selector de arquitectura diga **x86** (o Win32). **No usar x64**.

#### Paso B: Rutas de Inclusión (Headers)
* Clic derecho en el proyecto -> **Propiedades**.
* Ve a **C/C++** -> **General** -> **Additional Include Directories**.
* Añade las carpetas `include` de tus descargas:
    * `C:\Librerias\SDL3\include`
    * `C:\Librerias\SDL3_ttf\include`

#### Paso C: Rutas de Librerías (Libs)
* Ve a **Linker** -> **General** -> **Additional Library Directories**.
* Añade las carpetas `lib\x86` de tus descargas:
    * `C:\Librerias\SDL3\lib\x86`
    * `C:\Librerias\SDL3_ttf\lib\x86`

#### Paso D: Dependencias (Input)
* Ve a **Linker** -> **Input** -> **Additional Dependencies**.
* Escribe manualmente:
    ```
    SDL3.lib;SDL3_ttf.lib
    ```

### 3. Archivos Runtime (DLLs y Assets)
Para que el juego funcione, el ejecutable necesita encontrar las librerías dinámicas y la fuente.

1.  Compila el proyecto (Ctrl + Shift + B).
2.  Ve a la carpeta donde se creó el `.exe` (usualmente `\Debug` o `\Release`).
3.  **Copia y pega los siguientes archivos junto al `.exe`**:
    * `SDL3.dll` (Desde `SDL3\lib\x86`)
    * `SDL3_ttf.dll` (Desde `SDL3_ttf\lib\x86`)
    * **`RETRO.TTF`** (Incluido en este repositorio)

> **Nota:** Si el juego no abre o se cierra inmediatamente, verifica que `RETRO.TTF` esté en la misma carpeta que el ejecutable. El código busca la fuente en la ruta relativa actual.

## 👥 Autores

Proyecto desarrollado con fines académicos para la materia de **Organización y Arquitectura de Computadoras**:

* **♥ Astrid Yamilet Jiménez Barrera ♥**
* **✨ Erick Anselmo Moya Monreal ✨**

---
*Hecho con ❤️, C y mucho código ensamblador.*
