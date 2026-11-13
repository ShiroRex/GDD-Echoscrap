# Ficha del proyecto

| Campo | Información |
| --- | --- |
| Nombre | EchoScrap |
| Versión del GDD | 0.6.9 |
| Estado | Versión jugable final integrada: preparación guiada en bóveda, Índice de Colección / Bestiario (`[I]`), expediciones, colección de hasta 10 unidades con poderes acumulables por copia equipada, fusión de 30 s (misma rareza o rarezas mixtas), encuentro final, ranking en vivo en [`https://echoscrap.web.app`](https://echoscrap.web.app), ejecutable publicado en [`https://naked-hiro.itch.io/echoscrap`](https://naked-hiro.itch.io/echoscrap) y tráiler promocional final (`EchoScrap-Trailer-Final-1080p.mp4`, `1080p`, `46 s`) aprobado en QA. |
| Actualización | 13 de noviembre de 2026 |
| Género | Acción. |
| Subgénero | Supervivencia de hordas con progresión roguelite. |
| Público objetivo | Jugadores de PC que disfrutan esquivar hordas, combinar mejoras y coleccionar poderes entre expediciones cortas. |
| Plataforma | PC, Windows, un jugador local con soporte completo de teclado/ratón y mando (con sincronización de ranking en vivo en Firebase y visor web en [`https://echoscrap.web.app`](https://echoscrap.web.app)). |
| Motor | Unity 6000.6.3f1 y Universal Render Pipeline. |
| Lenguaje | C# y shaders en HLSL. |
| Perspectiva | Mundo 3D con personajes 2D; cámara exterior a 35,5° y encuadre propio para bóveda y jefe. |
| Distribución ejecutable (itch.io) | https://naked-hiro.itch.io/echoscrap |
| Tráiler promocional final | [EchoScrap-Trailer-Final-1080p.mp4](../Media/Trailer/EchoScrap-Trailer-Final-1080p.mp4) (`1920 × 1080`, `46,2 s`, gameplay desarrollado y audio propio; aprobado en QA) |
| Repositorio del juego | https://github.com/ShiroRex/EchoScrap |
| Revisión consultada | Código y escena de `main` @ `b381ef4` / `develop` @ `977bac5` (integra `a8a23c4`, `7ebe712`, `4b396f1`, `ca22b7a`, `adacb3d`, `9a89d8a`, `974c2d9`, `5926e63` y `2c9d8c2`). Se conservan los registros de pruebas y capturas históricas identificadas. |
| Repositorio del GDD | https://github.com/ShiroRex/GDD-Echoscrap |

## Equipo

- **Mora Alejandro Angel Gabriel:** líder y coordinación del proyecto.
- **Sierra Sosa David Alejandro:** desarrollo, diseño funcional y balance.
- **Cupul Dzul Mirian Leticia:** personajes y animación.
- **Obispo Navarro Samuel Alejandro:** entornos e interfaz.
- **Nuñez Chan Luis Fernando:** audio, narrativa, UX y calidad.

## El juego en una frase

El Coleccionista reúne entidades en un vertedero dimensional, prepara sus poderes en la bóveda y combina mejoras durante cada expedición para enfrentarse al MataColeccionista.

El contenido actual incluye una arena de 200 × 200 m, cinco enemigos de horda, dos élites, un jefe (Gólgora), 35 mejoras y una colección de quince entidades con capacidad máxima de 10 unidades simultáneas en la bóveda (3 puestos de equipo en el balcón superior con rótulos `EQUIPO 1..3` y 7 puestos de exposición en la planta baja), acompañada de indicadores direccionales (`VaultDirections`), consejos rotatorios (`VaultTips`) e Índice de Colección (`[I]`).

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
