# Sistemas

| Sistema | Funcionamiento actual |
| --- | --- |
| Flujo general | Menú inicial con nombre de jugador para ranking, gestión de colección, preparación guiada en bóveda, selección de ronda, expedición y retorno desde resultados. |
| Entradas y locomoción | Teclado y mando para movimiento y dash; `E` para recoger, equipar/editar, liberar y fusionar; `T` para desequipar rápido en el balcón; `I` para el Índice de Colección; `Q` / `Select` mantenido para desplegar detalles del HUD en combate. |
| Sesión | Reloj, razones de pausa (`Manual`, `FocusLost`, `LevelUpChoice`, `FusionMenu`, `CompanionMenu`, `GameMenu`), entrada del jefe, resolución de resultados y limpieza entre expediciones. |
| Salud y daño | Salud máxima escalable, orden de mitigación en 7 pasos (`PlayerHealth`), armadura hasta 60%, evasión hasta 35%, bloqueo hasta 50%, críticos, protección temporal, curación y salvación. |
| Linterna | Autoapuntado en 360°, ráfagas de cuatro pulsos con empuje final y modo continuo *Rayo solar* (`FLASH_05`, donde cada pulso cuenta como cuarto pulso). |
| Enemigos | Cinco arquetipos de horda, dos élites y Gólgora con navegación, ataques, estados alterados y tabla propia de susceptibilidad al empuje. |
| Oleadas | Grupos por fase, apariciones fuera de cámara, límites de población (150 normales, máx. 6 Morteros) y aparición alterna de élites desde los 90 s. |
| Encuentro del jefe | Traslado seguro a la caldera de 22 m, cámara compartida, refuerzos anunciados (límite de 8 secuaces) y dos fases con ataques de Bombardeo, Cerco de magma y Fisura de magma. |
| Progresión | XP, subidas de nivel y cola de elecciones entre 35 mejoras distribuidas en cuatro rarezas. |
| Entidades y capacidad | Apariciones temporales en arena, recogida manual con `E` y capacidad estricta de **10 unidades** en posesión (`3` de equipo + `7` de exposición), con bloqueo de recogida al llenarse (`INVENTARIO LLENO / LIBERA ESPACIO`). |
| Equipo y liberación | Tres puestos en el balcón superior (`EQUIPO 1..3`, `E · EQUIPAR` / `E · EDITAR` y `T` para desequipar) y siete puestos de exposición en la planta baja (`E · LIBERAR` con confirmación modal). |
| Índice de Colección | Panel consultable en la bóveda con `I` (`VaultCollectionIndex`): contador `{descubiertos} / 15 DESCUBIERTOS · {en posesión} / 10 UNIDADES`, pestañas `POR RAREZAS` (con siluetas negras `???`) y `EN TU BÓVEDA`, e inspector lateral de poderes. |
| Fusión | Estación central que combina 4 copias de la misma rareza (incluidas copias equipadas `EN EQUIPO 1/2/3`, excluida Yanikua), duración fija de **30 segundos (`00:30`)** para todas las rarezas y botón único `COLECCIONAR` al reclamar. |
| Señalización de bóveda | Rótulos 3D `EQUIPO 1..3`, indicadores direccionales dinámicos en el borde superior (`VaultDirections`) y panel rotatorio de consejos cada `8 s` (`VaultTips`). |
| Interfaz y pausa | Tipografía unificada `Pixelify Sans`, menú de pausa en 3 pestañas (`General`, `Interfaz` con `Atributos de coleccionables SÍ/NO`, `Sonido`), HUD con consulta rápida (`Q` / `Select`), minimapa con niebla de exploración y resultados. |
| Ranking de evento | `EventLeaderboardSync`: registro de nombre de jugador (`EchoScrap.Event.PlayerName`), respaldo local en JSON y sincronización HTTPS `PATCH` con Firebase Realtime Database. |
| Audio | Música adaptativa de arena, jefe y bóveda con control de volumen general y de música; sonidos de linterna, recogida, entrada, fusión y ataques del jefe. |

El reloj distingue pausa manual, pérdida de foco, cartas, fusión/índice (`PauseReason.FusionMenu`), selector de equipo y menús generales. Resolver una de esas razones no reanuda una sesión que sigue detenida por otra. La interfaz se anima con `Time.unscaledDeltaTime` mientras el combate está pausado.

La simulación resuelve daño y resultados. La presentación representa esos eventos con animación, sonido y VFX. Las reservas de actores y efectos se reutilizan entre expediciones. `CompanionPowerRuntime` mantiene los bonos, ataques, curaciones, protección y salvación de las entidades equipadas; sus recargas no se reinician al cambiar equipo durante una expedición.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
