# Ficha del proyecto

| Campo | Información |
| --- | --- |
| Nombre | EchoScrap |
| Versión del GDD | 0.7.1 |
| Estado | Versión jugable sincronizada en `develop`: menú inicial con **3 ranuras de guardado** y reanudación por snapshot, configuración compartida con selector de **pantalla completa/ventana (`COMPLETA` / `VENTANA`)** y FPS, preparación guiada en bóveda de **18 unidades**, Índice de Colección / Bestiario (`[I]`), expediciones, fusión de 30 s (misma rareza o rarezas mixtas), **62 mejoras en cinco rarezas** (incluyendo *All In* Mítica y proyección de auras/zonas sobre relieve), encuentro final, ranking en vivo en [`https://echoscrap.web.app`](https://echoscrap.web.app) con todos los intentos válidos, compilaciones **Windows x64** y **macOS Universal**, publicación web previa en [`https://naked-hiro.itch.io/echoscrap`](https://naked-hiro.itch.io/echoscrap) y tráiler promocional (`EchoScrap-Trailer-Final-1080p.mp4`, `1080p`, `46 s`). |
| Actualización | 16 de noviembre de 2026 |
| Género | Acción. |
| Subgénero | Supervivencia de hordas con progresión roguelite. |
| Público objetivo | Jugadores de PC que disfrutan esquivar hordas, combinar mejoras y coleccionar poderes entre expediciones cortas. |
| Plataforma | PC (**Windows x64** y **macOS Universal `x86_64` + `arm64`**), un jugador local con soporte completo de teclado/ratón y mando (con sincronización de ranking en vivo en Firebase y visor web en [`https://echoscrap.web.app`](https://echoscrap.web.app)). |
| Motor | Unity 6000.6.3f1 y Universal Render Pipeline. |
| Lenguaje | C# y shaders en HLSL. |
| Perspectiva | Mundo 3D con personajes 2D; cámara exterior a 35,5° y encuadre propio para bóveda y jefe. |
| Distribución ejecutable (itch.io) | https://naked-hiro.itch.io/echoscrap |
| Tráiler promocional final | [EchoScrap-Trailer-Final-1080p.mp4](../Media/Trailer/EchoScrap-Trailer-Final-1080p.mp4) (`1920 × 1080`, `46,2 s`, gameplay desarrollado y audio propio; aprobado en QA) |
| Repositorio del juego | https://github.com/ShiroRex/EchoScrap |
| Revisión consultada | Código y escena de `develop` @ `b543684` (integra `734e2a5`/`6973556`, las 8 mejoras contextuales `1239072`, selector de FPS `993d459`, 3 ranuras `1c97922`, snapshots `24db181`, ranking por mejor marca `15d273f`, bóveda de 18 cápsulas `8c0dfae`, mejoras de rendimiento/arte `578ba07..aefed1a`, expansión a 62 mejoras + rareza Mítica `ebf90f9`, y correcciones `7a237e5..b543684`: selector `Pantalla` `COMPLETA`/`VENTANA` con pantalla completa al iniciar, limpieza de `_EmissionColor` en enemigos y proyección sobre relieve de auras/zonas). |
| Repositorio del GDD | https://github.com/ShiroRex/GDD-Echoscrap |

## Equipo

- **Mora Alejandro Angel Gabriel:** líder y coordinación del proyecto.
- **Sierra Sosa David Alejandro:** desarrollo, diseño funcional y balance.
- **Cupul Dzul Mirian Leticia:** personajes y animación.
- **Obispo Navarro Samuel Alejandro:** entornos e interfaz.
- **Nuñez Chan Luis Fernando:** audio, narrativa, UX y calidad.

## El juego en una frase

El Coleccionista reúne entidades en un vertedero dimensional, prepara sus poderes en la bóveda y combina mejoras durante cada expedición para enfrentarse al MataColeccionista.

El contenido actual incluye una arena de 200 × 200 m, cinco enemigos de horda, dos élites, un jefe (Gólgora), **62 mejoras en cinco rarezas** (Común, Rara, Épica, Legendaria y Mítica) y una colección de quince entidades con capacidad máxima de **18 unidades simultáneas** en la bóveda (`3` puestos de equipo en el balcón superior con rótulos `EQUIPO 1..3` y `15` puestos de exposición en la planta baja, con la máquina de fusión en la pared trasera en `x = 189, z = 28,88`), acompañada de indicadores direccionales (`VaultDirections`), consejos rotatorios (`VaultTips`) e Índice de Colección (`[I]`).

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
