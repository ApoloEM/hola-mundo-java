# 🧱 Breakout: x86 Assembly & SDL3 Hybrid Engine

![Language](https://img.shields.io/badge/Language-C%20%2F%20ASM%20(x86)-blue?style=for-the-badge&logo=c)
![Library](https://img.shields.io/badge/Library-SDL3-EA312F?style=for-the-badge&logo=sdl)
![IDE](https://img.shields.io/badge/IDE-Visual%20Studio%202022-5C2D91?style=for-the-badge&logo=visual-studio)
![Academic](https://img.shields.io/badge/Course-Org.%20y%20Arq.%20de%20Computadoras-green?style=for-the-badge)

> **Un clon avanzado de Breakout optimizado a bajo nivel.**
> Este proyecto demuestra la integración eficiente entre **C (para gestión de medios)** y **Ensamblador x86 (para lógica crítica)**, logrando un control total sobre la física y el rendimiento.

---

## 📖 Descripción del Proyecto

Desarrollado como proyecto final para la materia de **Organización y Arquitectura de Computadoras** en la **UABC**, este juego no es solo una recreación visual; es una implementación técnica que delega las tareas computacionales intensivas directamente al procesador mediante instrucciones nativas.

A diferencia de implementaciones estándar, aquí **no se utilizan librerías de física**. Toda la detección de colisiones, cálculo de trayectorias y ordenamiento de datos ha sido escrita a mano en bloques de ensamblador (`__asm`), utilizando la FPU (Floating Point Unit) para una precisión matemática superior.

## 📸 Galería

| **Menú Principal con Selector** | **Gameplay (Nivel 1)** |
|:---:|:---:|
| ![Menú](./menu.png) | ![Gameplay](./gameplay.png) |
| *Selector de nivel inicial y modos* | *Motor de física híbrido en acción* |

| **Tabla de Récords** | **Créditos** |
|:---:|:---:|
| ![Scores](./scores.png) | ![Créditos](./PF_OyAC_13.12.jpg) |
| *Bubble Sort implementado en ASM* | *Reconocimiento al equipo* |

---

## 🚀 Características Técnicas (El Motor Híbrido)

El núcleo del juego utiliza una arquitectura mixta para maximizar el rendimiento:

### 🧠 Lógica en Ensamblador (x86 Inline ASM)
* **Física Vectorial (FPU):** Cálculo de rebotes y trayectorias usando la pila de registros de punto flotante (`fld`, `fstp`, `fmul`).
* **Colisiones AABB:** Algoritmo de detección de impacto caja-caja optimizado para reducir ciclos de CPU.
* **Máquina de Estados:** Gestión del flujo (Menú $\to$ Juego $\to$ Win/Lose) mediante manipulación directa de registros de bandera y saltos condicionales (`cmp`, `je`, `jmp`).
* **Bubble Sort Nativo:** El sistema de *High Scores* ordena la tabla de puntuaciones manipulando directamente la memoria en tiempo real al finalizar la partida.

### 🎮 Mecánicas de Juego Actualizadas
* **10 Niveles Progresivos:** Diseños definidos por matrices con dificultad incremental.
* **Selector de Nivel:** Permite iniciar la partida desde cualquier nivel desbloqueado o para prácticas (visible en el menú principal).
* **Dificultad Dinámica:**
    * Aumento de velocidad (+15%) tras completar cada nivel.
    * **Resistencia:** Ladrillos de colores avanzados requieren múltiples impactos.
* **Física "Factor Caos":** Algoritmo que introduce micro-perturbaciones aleatorias en los rebotes para evitar bucles infinitos y aumentar el realismo.
* **Persistencia:** Sistema de guardado en archivo binario (`scores.dat`).

---

## 🕹️ Controles

| Contexto | Tecla | Acción |
| :--- | :---: | :--- |
| **Menú** | `Enter` | Iniciar Juego |
| | `←` / `→` | **Seleccionar Nivel Inicial** |
| | `Tab` | Ver Mejores Puntuaciones |
| | `C` | Ver Créditos |
| | `Esc` | Salir |
| **En Juego** | `←` / `→` | Mover la Paleta |
| | `Enter` | Pausar / Reanudar |
| **Final** | `Enter` | Guardar Récord y Continuar |

---

## 🛠️ Instalación y Compilación

⚠️ **Importante:** Este proyecto requiere compilarse en modo **x86 (32-bits)** debido a que el compilador MSVC de Visual Studio no admite `__asm` bloques en arquitectura x64.

1.  **Clonar el repositorio:**
    ```bash
    git clone [https://github.com/TU_USUARIO/oyac-breakout-proyectofinal.git](https://github.com/TU_USUARIO/oyac-breakout-proyectofinal.git)
    ```

2.  **Requisitos:**
    * Visual Studio 2022 (Workload: Desarrollo de escritorio con C++).
    * Librerías **SDL3** y **SDL3_ttf**.

3.  **Configuración en Visual Studio:**
    * Abrir `BreakoutGame.sln`.
    * Seleccionar la configuración **Debug** o **Release** y la plataforma **x86**.
    * Verificar rutas de inclusión (Include Directories):
        * `$(SolutionDir)SDL3\include` (Ajustar según tu estructura).
    * Verificar rutas de librería (Library Directories):
        * `$(SolutionDir)SDL3\lib\x86`.

4.  **Ejecución:**
    * Compilar la solución (`Ctrl + Shift + B`).
    * **Paso Crítico:** Copiar los siguientes archivos a la carpeta donde se generó el `.exe` (usualmente `x86/Debug`):
        * `SDL3.dll`
        * `SDL3_ttf.dll`
        * `RETRO.TTF` (Fuente tipográfica)
        * `scores.dat` (Si ya existe)

---

## 🔮 Roadmap y Mejoras Futuras

Basado en el análisis de rendimiento del proyecto final, se proponen las siguientes optimizaciones para futuras versiones:
* [ ] **Separación de ASM:** Mover la lógica a archivos externos (`.asm`) compilados con MASM para permitir soporte de 64 bits.
* [ ] **Instrucciones SIMD:** Implementar instrucciones SSE/AVX para procesar colisiones de múltiples ladrillos en paralelo.
* [ ] **Partículas:** Sistema de partículas en ASM para efectos visuales al romper ladrillos.

---

## 👥 Autores

**Universidad Autónoma de Baja California**
*Facultad de Ingeniería - Mexicali*

* **💻 Código y Lógica:** Erick Anselmo Moya Monreal
* **🎨 Diseño y Documentación:** Astrid Yamilet Jiménez Barrera

---
*Hecho con ❤️, C y mucho código ensamblador.*
