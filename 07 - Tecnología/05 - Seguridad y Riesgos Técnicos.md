# Seguridad y riesgos técnicos

El inventario de entidades (`hasta 10 unidades` en posesión y `15` descubiertas), los diez pedestales de la bóveda (`displays[10]`: 3 de equipo + 7 de exposición) y el estado de la máquina de fusión se validan al cargar el JSON de `PlayerPrefs` antes de reanudar cualquier proceso.

## Comportamientos verificados y salvaguardas del código (`734e2a5`)

- **Límite estricto de capacidad (`Capacity = 10`):** tanto la recogida de fragmentos en la arena (`CompanionSystem.TryCollectNearest` → `FusionStation.CollectFromArena`) como la reclamación del resultado en la máquina de fusión (`FusionStation.Resolve(true)` desde el botón `COLECCIONAR`) comprueban `FusionProgress.HasSpace` (`OwnedCount < 10`). Si la colección está en `10/10`, se bloquea la adición y se conserva el resultado de la fusión hasta que el jugador libere una unidad en la bóveda.
- **Consumo de copias equipadas en fusión (`FusionProgress.Begin`):** cuando las 4 unidades seleccionadas para una fusión (misma rareza o rarezas mixtas) incluyen copias equipadas en el balcón (`EN EQUIPO 1/2/3`), el sistema calcula si las copias restantes tras descontar las 4 unidades son inferiores a las equipadas y desequipa automáticamente los puestos necesarios antes de guardar el estado `Fusing` (`30 s`).
- **Preservación de descubrimientos al liberar (`FusionStation.ReleaseCollectible`):** al liberar una copia desde un pedestal de exposición de planta baja (`E · LIBERAR`, `slot >= 3`) o desde el selector de equipo (`LIBERAR COPIA`), se descuenta `1` de `owned[id]` y se reordenan los puestos de exposición, manteniendo `discovered[id] = true` para no perder nunca el progreso en el Índice de Colección (`[I]`).
- **Reciclado limpio de pools (`Object Pool`):** todo enemigo, proyectil o efecto reciclado limpia sus temporizadores, estados alterados y objetivos antes de volver a activarse.
- **Razones independientes de pausa (`CombatSession`):** la pausa manual (`Esc`), la pérdida de foco, la elección de cartas, la máquina de fusión / Índice de Colección (`PauseReason.FusionMenu`), el selector de acompañantes (`PauseReason.CompanionMenu`) y el menú general usan máscaras independientes para que cerrar una ventana no reanude por error una sesión detenida por otra causa.
- **Sincronización de ranking no bloqueante (`EventLeaderboardSync` / `GamertagRegistry`):** las peticiones HTTPS (`PUT` y `DELETE`) a Firebase Realtime Database se ejecutan en corrutinas con `timeout = 8 s` (`5 s` para autenticación y `GET` de comprobación) y cola persistente en `PlayerPrefs` (`EchoScrap.Rank.v2` y `EchoScrap.PendingGamertag.{sha256}`), de modo que un corte de red nunca bloquea el juego ni pierde puntuaciones finales pendientes.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
