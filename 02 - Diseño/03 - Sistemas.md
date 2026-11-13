# Sistemas

| Sistema | Funcionamiento actual |
| --- | --- |
| Flujo general | Menú inicial con nombre de jugador para ranking, gestión de colección, preparación guiada en bóveda, selección de ronda, expedición y retorno desde resultados. |
| Entradas y locomoción | Teclado, ratón y mando completos (`WASD` / stick, `Espacio` / `A` para dash, `E` / botón oeste para recoger/equipar/liberar/fusionar, `T` / botón norte para desequipar rápido en balcón, `I` / `Select` para Índice de Colección y `Q` / `Select` mantenido para detalles del HUD en combate). |
| Sesión | Reloj, razones de pausa (`Manual`, `FocusLost`, `LevelUpChoice`, `FusionMenu`, `CompanionMenu`, `GameMenu`), entrada del jefe, resolución de resultados y limpieza entre expediciones. |
| Salud y daño | Salud máxima escalable, orden de mitigación en `PlayerHealth`, armadura hasta `60%` (`SCRAP_01` + `Soldier`), evasión hasta `35%`, bloqueo hasta `50%`, críticos, protección temporal (`0,40 s`), curación y salvación/protección (*Yanikua* ante daño letal y *Nanobots* al sobrevivir bajo `20%` de vida). |
| Linterna | Autoapuntado en 360°, ráfagas de cuatro pulsos con empuje final y modo continuo *Rayo solar* (`FLASH_05`, donde cada pulso aplica empuje; `FLASH_03`, `BIO_02` y `CRIT_04` mantienen el disparo en el cuarto pulso). |
| Enemigos | Cinco arquetipos de horda, dos élites y Gólgora con navegación, ataques, estados alterados y tabla unificada de susceptibilidad y frenado al empuje. |
| Oleadas | Grupos por fase, apariciones fuera de cámara, límites de población (150 normales, máx. 6 Morteros) y aparición alterna de élites desde los 90 s. |
| Encuentro del jefe | Traslado seguro a la caldera de 22 m, cámara compartida, refuerzos anunciados (límite de 8 secuaces) y dos fases con ataques de Bombardeo, Cerco de magma y Fisura de magma. |
| Progresión | XP, subidas de nivel y cola de elecciones entre 35 mejoras distribuidas en cuatro rarezas. |
| Entidades y capacidad | Apariciones temporales en arena, recogida manual con `E` (o botón oeste), capacidad estricta de **10 unidades** (`DisplaySlots[10]`: 3 de equipo + 7 de exposición) y bloqueo al llenarse (`INVENTARIO LLENO / LIBERA ESPACIO`). Los poderes se acumulan por copia equipada hasta `×3`. |
| Equipo y liberación | Tres puestos en el balcón superior (`EQUIPO 1..3`, `E` / botón oeste para editar y `LIBERAR COPIA` dentro del selector, y `T` / botón norte para desequipar) y siete puestos de exposición en planta baja (`E · LIBERAR` con confirmación modal). |
| Índice de Colección | Panel consultable en la bóveda con `I` / `Select` (`VaultCollectionIndex`): contador `{descubiertos} / 15 DESCUBIERTOS · {en posesión} / 10 UNIDADES`, pestañas `POR RAREZAS` (`TODAS` + 6 rarezas con siluetas negras `???`) y `EN TU BÓVEDA`, e inspector lateral de poderes. |
| Fusión | Estación central que combina 4 copias de la misma rareza o de rarezas mixtas (incluidas copias equipadas `EN EQUIPO 1/2/3`, excluida Yanikua), duración fija de **30 segundos (`00:30`)** y botón único `COLECCIONAR` al reclamar. |
| Señalización de bóveda | Rótulos 3D `EQUIPO 1..3`, indicadores direccionales dinámicos en el borde superior (`VaultDirections`) y panel rotatorio de consejos cada `8 s` (`VaultTips`). |
| Interfaz y ajustes | Tipografía unificada `Pixelify Sans`, panel compartido `MenuSettingsView` en 3 pestañas (`General`, `Interfaz` con `Atributos de coleccionables SÍ/NO`, `Sonido`) para Menú Inicial y Pausa, HUD con consulta rápida (`Q` / `Select`), minimapa con niebla de exploración y resultados. |
| Ranking de evento | `GamertagRegistry` + `EventLeaderboardSync`: reserva atómica de alias por SHA-256 (mayúsculas conservando tildes), cola en `PlayerPrefs` (`EchoScrap.Rank.v2`) y sincronización HTTPS `PUT` / `DELETE` (`8 s`) con Firebase Realtime Database. |
| Audio | Música adaptativa de arena, jefe y bóveda con control de volumen general y de música; sonidos de linterna, recogida, entrada, fusión y ataques del jefe. |

El reloj distingue pausa manual, pérdida de foco, cartas, fusión/índice (`PauseReason.FusionMenu`), selector de equipo y menús generales. Resolver una de esas razones no reanuda una sesión que sigue detenida por otra. La interfaz se anima con `Time.unscaledDeltaTime` mientras el combate está pausado.

La simulación resuelve daño y resultados. La presentación representa esos eventos con animación, sonido y VFX. Las reservas de actores y efectos se reutilizan entre expediciones. `CompanionPowerRuntime` mantiene los bonos, ataques, curaciones, protección y salvación de las entidades equipadas; sus recargas no se reinician al cambiar equipo durante una expedición.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
