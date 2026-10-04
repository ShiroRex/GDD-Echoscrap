# Motor y Herramientas

## Descripción de la sección

Documenta las herramientas, tecnologías, versiones y convenciones empleadas en la producción de *EchoScrap*.

---

## Motor de juego

* **Motor:** Unity
* **Versión Oficial del Proyecto:** `6000.6.3f1 (45d8eee7de74)` (Unity 6)
* **Pipeline de Renderizado:** Universal Render Pipeline (URP)
* **Motor de Física:** Unity 3D Physics (PhysX) restringido al plano horizontal XZ con restricciones cinemáticas de rotación en X y Z.
* **Sistema de Entrada:** Unity New Input System (`com.unity.inputsystem`, versión 1.14.2+)

---

## Lenguajes de programación

| Lenguaje | Ámbito de Uso | Estándar / Versión |
|---|---|---|
| **C#** | Lógica de juego, IA de hordas, sistemas de atributos, control de interfaz y físicas. | .NET Standard 2.1 / C# 9.0+ |
| **HLSL / ShaderGraph** | Shaders de URP, materiales aditivos de cono visual y renderizado de gemas transparentes. | URP Lit / Unlit Core |

---

## Herramientas de desarrollo y creación de contenido

| Herramienta | Propósito | Versión / Formato |
|---|---|---|
| **Unity Editor** | Integración de escenas, lógica, físicas, iluminación y compilación del juego. | `6000.6.3f1` |
| **Aseprite** | Creación y animación de sprites, iconos de interfaz y entidades en Pixel Art. | `.aseprite` (32x32 y 64x64 px) |
| **Obsidian** | Redacción, vinculación y mantenimiento del Game Design Document (GDD). | Bóveda Markdown con enlaces wiki |
| **Visual Studio / VS Code** | Edición de código C#, análisis estático de tipos y depuración en tiempo de ejecución. | C# Dev Kit / OmniSharp |
| **Git + Git LFS** | Control de versiones distribuido con seguimiento LFS para assets pesados (`.png`, `.wav`, etc.). | Git 2.40+ / Git LFS 3.x |

---

## Paquetes y dependencias clave (Unity Package Manager)

* `com.unity.render-pipelines.universal` (URP)
* `com.unity.inputsystem` (New Input System)
* `com.unity.textmeshpro` (Renderizado tipográfico nítido SDF para HUD e interfaz)
* `com.unity.cinemachine` (Sistema cinemático de cámara suave 2.5D)

---

## Convenciones de estructura y nomenclatura

### Estructura de carpetas en Unity (`Assets/_Project/`)
Para aislar el contenido propio del proyecto de paquetes externos o plugins de la Asset Store:
* `_Project/Art/`: Texturas, sprites, iconos y materiales.
* `_Project/Audio/`: Efectos de sonido (SFX) y música instrumental.
* `_Project/Prefabs/`: Prefabs preconfigurados del jugador, enemigos (`Enemy_Basic.prefab`) y gemas (`Gem_XP.prefab`).
* `_Project/Scenes/`: Escenas de arena y pruebas (`Test_PlayerGameplay.unity`).
* `_Project/Scripts/`: Código fuente modular organizado por subdominios:
  * `Camera/`
  * `Enemies/`
  * `Player/` (`Input/`, `Movement/`, `Stats/`, `Weapons/`)
  * `Systems/` (`Stats/`)
  * `UI/`
* `_Project/Settings/`: Configuraciones de renderizado y presets de URP.

### Control de versiones
* Repositorio del Juego: `Echoscrap_Game` (rama de desarrollo activa: `develop`).
* Repositorio del GDD: `GDD_Videojuego_ES_2026-main` (rama de documentación: `main`).
* Carpeta de Sprites: `Sprites/` (almacenamiento de archivos `.aseprite` en bruto previo a exportación a Unity).

---

> **Navegación:** [[00 - Índice]] · ← [[01 - Arquitectura Técnica]] · [[03 - Requisitos Técnicos]] →
