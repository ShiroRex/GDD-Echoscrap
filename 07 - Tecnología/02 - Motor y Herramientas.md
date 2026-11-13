# Motor y herramientas

| Tecnología | Uso en el proyecto actual (`734e2a5`) |
| --- | --- |
| Unity 6000.6.3f1 | Escenas, física, integración, Play Mode y ejecutable Windows x64. |
| URP 17.6.0 | Iluminación, materiales, shaders y renderizado. |
| Input System 1.20.0 | Lectura completa de teclado, ratón y mando (`PlayerInputReader`, `CombatHud`, `VaultCollectionIndex`, `CompanionSystem`, `FusionStation`, `VaultReleaseInteraction`). |
| Cinemachine 6.6.0 | Seguimiento de cámara en arena, caldera del jefe y bóveda. |
| UnityWebRequest / JSON | Reserva atómica de gamertag (`GamertagRegistry`) y sincronización HTTPS `PUT` / `DELETE` del ranking de evento con Firebase Realtime Database (`EventLeaderboardSync`) + serialización `JsonUtility` (`FusionProgress` v4). |
| Unity Test Framework 1.8.0 | Pruebas automatizadas de reglas e integración. |
| C# | Sistemas de simulación, interfaz dinámica y herramientas del Editor. |
| HLSL | Shaders de luz, magma, portal, materiales de interfaz y efectos. |
| Aseprite | Fuentes editables, animaciones, atlas e interfaz de pixel art. |
| Blender | Preparación y edición del modelo 3D de Gólgora y piezas de escenario. |
| Pixelify Sans (`Regular` / `SemiBold`) | Familia tipográfica unificada en `Typography.asset`, `UpgradeCardSkin.asset` y generadores de UI. |
| Obsidian / Markdown | Redacción del GDD y presentación de imágenes, planos SVG y GIFs. |
| Git y GitHub | Versionado separado del repositorio del juego y del documento. |

## Organización del repositorio

El juego organiza el código en `Assets/_Project/Scripts`, las escenas en `Scenes`, los perfiles en `Settings` y los recursos visuales y sonoros en `Art`, `Audio` y `Resources`. Las fuentes editables (`.aseprite`) se conservan en `ArtSource`.

Los medios documentales enlazados en este GDD se ubican en las carpetas `Media/Planos`, `Media/Avance_2026-10-27`, `Media/Avance_2026-11-04`, `Media/Avance_2026-11-05`, `Media/Avance_2026-11-06` y `Media/Avance_2026-11-07`.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
