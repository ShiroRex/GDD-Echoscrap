# Ficha del proyecto

| Campo | Información |
| --- | --- |
| Nombre | EchoScrap |
| Versión del GDD | 0.6.2 |
| Estado | Versión jugable integrada: preparación guiada en bóveda, Índice de Colección (`[I]`), expediciones, colección de hasta 10 unidades, fusión de 30 s, encuentro final y ranking en vivo. |
| Actualización | 8 de noviembre de 2026 |
| Género | Acción. |
| Subgénero | Supervivencia de hordas con progresión roguelite. |
| Público objetivo | Jugadores de PC que disfrutan esquivar hordas, combinar mejoras y coleccionar poderes entre expediciones cortas. |
| Plataforma | PC, Windows, un jugador local (con sincronización opcional de ranking de evento en Firebase). |
| Motor | Unity 6000.6.3f1 y Universal Render Pipeline. |
| Lenguaje | C# y shaders en HLSL. |
| Perspectiva | Mundo 3D con personajes 2D; cámara exterior a 35,5° y encuadre propio para bóveda y jefe. |
| Repositorio del juego | https://github.com/ShiroRex/EchoScrap |
| Revisión consultada | Código y escena de `develop` en `9eed59f` (integra `bugfix/pulido-pausa-y-liberacion` `687e909` y `origin/develop` `b4d5ada` sobre `75bb79c`). Se conservan los registros de pruebas y capturas históricas identificadas. |
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
