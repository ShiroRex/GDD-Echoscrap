# Datos y persistencia

Los valores del jugador, los arquetipos enemigos y las oleadas se configuran mediante `ScriptableObjects`. `ExpeditionEntrySettings` enlaza las rondas demo (`3:00`, `Golgora_Demo.asset` con `3 600 HP`, mismo valor base en `EnemyArchetypeSettings`) y completa (`8:00`, `Golgora_Complete.asset` con `9 000 HP`), mientras que `GolgoraCombatSettings` y `CombatSettings.asset` definen de forma unificada los parámetros de combate del jefe (`TransitionDuration = 1,5 s`, daños, tiempos, áreas y multiplicadores de Fase 2).

El catálogo de combate consta de **`62` mejoras en cinco rarezas** (`UpgradeCatalog.Cards`, `UpgradeRarity.N..M`). El catálogo de entidades consta de `15` identificadores (`0` a `14`), distribuidos en seis rarezas y dos fuentes de obtención (nueve en la arena y seis por fusión). `CompanionSystem` comunica los tres puestos de equipo a `CompanionPowerRuntime` (con poderes **acumulables por copia equipada**, hasta `×3`) y a `PlayerRuntimeStats`.

## Sesión en memoria

El reloj de incursión, la salud actual, la XP, el nivel, las cartas adquiridas, los enemigos activos y los objetos del suelo pertenecen a la sesión en memoria. Al regresar a la bóveda tras una victoria, una derrota o desde el menú de pausa se limpia el estado de la arena y se reaplican los poderes de las entidades equipadas. Una visita temporal a la bóveda por el portal norte suspende el combate sin destruir la sesión activa.

## Persistencia en 3 ranuras, snapshots y fusión (`SaveSlotService`, `ExpeditionSnapshotService`, `FusionProgress` v5)

`SaveSlotService` gestiona hasta **`3` ranuras independientes (`SlotCount = 3`)** en disco mediante escritura atómica y respaldo bajo `Application.persistentDataPath/Saves/` (o `Saves_BatchTest/` en batch mode): `index.json`, `slot_0..2.json` (`SlotProfileData`, que incluye `FusionProgress` versión **`5`**, `totalPlaySeconds`, `hasActiveExpedition`, `activeAttemptId`, `bestLocalScore` y `completedRankedRuns`), `expedition_0..2.json` (`ExpeditionSnapshotData`) y `ranking_outbox.json` (`RankingOutboxData`). En la primera carga, `SaveSlotService.EnsureMigrated()` importa automáticamente cualquier progreso previo de `PlayerPrefs` (`EchoScrap.Collection.Production.v1`, `EchoScrap.Collection.v1`, `EchoScrap.Rank.v2` y `EchoScrap.Event.PlayerName`).

La capacidad máxima de la colección es de **18 unidades en total** (`FusionProgress.Capacity = 18`, `OwnedCount <= 18`), que se corresponden exactamente con los **18 pedestales físicos de la bóveda** (`DisplaySlots[18]`): **3 puestos de equipo activo** en el balcón superior (`slot 0..2`) y **15 puestos de exposición/reserva** en la planta inferior (`slot 3..17`).

| Campo del JSON (`FusionProgress`) | Tipo | Contenido y reglas de validación (`FusionProgress.cs`) |
| --- | --- | --- |
| `version` | `int` | Versión del esquema de guardado. El valor vigente es **`5`**. |
| `hasEquippedCompanion` | `bool` | Registra si el jugador ya ha equipado al menos un acompañante alguna vez (`VaultTips` y `FusionStation.cs`). |
| `equipped` | `int[3]` | IDs de las entidades asignadas a los **3 puestos de equipo activo** del balcón (puestos 1 a 3) o `-1` si el puesto está vacío. |
| `owned` | `int[15]` | Copias en posesión de cada una de las 15 entidades (`ID 0..14`). Los valores negativos se ajustan a `0` y la **suma total está limitada a un máximo de `18` unidades** (`Capacity = 18`). |
| `discovered` | `bool[15]` | Registro permanente de descubrimiento de las 15 entidades para el **Índice de Colección (`[I]`, `VaultCollectionIndex`)** y los selectores de puesto. Toda entidad con `owned[i] > 0` marca `discovered[i] = true`, y liberar una copia conserva `discovered[i] = true`. |
| `displays` | `int[18]` | Array de longitud **`18`** (`Capacity = 18`) que representa el total de los **18 puestos físicos de la bóveda** (`displays[0..2]` para el balcón y `displays[3..17]` para la planta baja). `-1` indica pedestal vacío. |
| `phase` | `int` (`FusionPhase`) | Estado de la máquina de fusión en **5 estados**: `0 = Idle`, `1 = Fusing`, `2 = Opening`, `3 = AwaitingDecision`, `4 = Ready`. |
| `result` | `int` | ID de la entidad resultante sorteada al iniciar la fusión (`-1` cuando está libre). |
| `remaining` | `float` | Segundos restantes de la cuenta atrás de fusión (`Time.unscaledDeltaTime`). |
| `duration` | `float` | Duración total de la fusión activa: **fija en `30 s` (`00:30`, `FusionRecipe.FusionSeconds = 30`)** para recetas homogéneas o de rarezas mixtas. |
| `opening` | `float` | Progreso normalizado (`0..1`) de la apertura de persiana (`1,2 s`). Al cargar con `RestoreAfterLoading()`, si estaba en `Opening`, pasa a `AwaitingDecision` (`opening = 1f`); si estaba en `Ready`, conserva `Ready`. |

### Reglas de carga, snapshots y escritura
- **Validación al cargar (`Validate`):** Descarta guardados `version < 4` (o `owned == null`), migra esquemas `v4` (`displays` de `10`) al formato **`v5` (`displays[18]`)**, ajusta arreglos de 13 a 15 entidades, limita `owned[i] >= 0`, sanea IDs de `displays` y `equipped`, y garantiza que `discovered[id]` sea `true` para toda entidad con copias en `owned` o equipada, **pero no recorta automáticamente el exceso de `owned` sobre `18` al cargar** (`HasSpace` aplica el tope al recoger o reclamar). En escena (`FusionStation.RefreshDisplays`), los puestos `3..17` exhiben hasta `15` copias en reserva y cualquier puesto de equipo vacío `0..2` exhibe las copias restantes marcadas como `RESERVA`.
- **Snapshots de expedición (`ExpeditionSnapshotService`):** durante la incursión guarda cada `3 s` (`AutosaveIntervalSeconds = 3f`) y en hitos de combate la instantánea `expedition_{N}.json` (`profileId`, `attemptId`, `revision`, reloj, vida, nivel, XP, cartas adquiridas, oleadas y progreso de ranking), restaurándola silenciosamente al elegir la ranura desde `CONTINUAR` (o ofreciendo recuperar bóveda si estuviera dañada).
- **Inicio y resolución de fusión (`FusionProgress.Begin`):** Al confirmar una fusión (con 4 copias de la misma rareza o de rarezas mixtas, excluida Yanikua), si las 4 unidades seleccionadas incluyen copias equipadas (`EN EQUIPO 1/2/3`), el sistema las desequipa automáticamente de `equipped`, descuenta las 4 unidades de `owned`, sortea el resultado según la calidad media (`qualityMean = total / 4f`) y fija `duration = 30` y `remaining = 30`. Durante la fase `Fusing`, el reloj avanza con `Time.unscaledDeltaTime` y persiste la ranura cada `5 s`, además de hacerlo al pausar o cerrar la aplicación. Si al reclamar (`COLECCIONAR`) el jugador ya tiene `18/18` unidades, `FusionStation.Resolve` conserva el estado hasta que se libere una unidad en los pedestales de la bóveda.

## Otras claves de `PlayerPrefs`, archivos en `Saves/` y sincronización de Ranking (`EventLeaderboardSync`, `GamertagRegistry`)

| Clave o ruta | Tipo / Valor por defecto | Descripción |
| --- | --- | --- |
| `Saves/index.json`, `slot_0..2.json`, `expedition_0..2.json`, `ranking_outbox.json` | Archivos JSON atómicos en `Application.persistentDataPath` | Estado completo de las 3 ranuras, perfil (`bestLocalScore`, `completedRankedRuns`), colección (`FusionProgress` v5), snapshot de expedición y cola de envíos al ranking (`Saves_BatchTest/` en ejecución por lotes). |
| `EchoScrap.Graphics.FrameRateLimit` | `int` (`60`) | Tope de FPS elegido en la pestaña `General` de pausa (`30`, `60` o `120`; fuerza `QualitySettings.vSyncCount = 0`). |
| `EchoScrap.Graphics.FullScreen` | `int` (`1`) | Modo de pantalla elegido en la pestaña `General` de `MenuSettingsView` (`1 = COMPLETA` `FullScreenWindow` a resolución nativa del monitor; `0 = VENTANA` `Windowed`), reaplicado automáticamente tras el arranque en Standalone. |
| `EchoScrap.Audio.MasterVolume` | `float` (`0.85`) | Volumen general (`AudioListener.volume`, deslizador en pestaña `Sonido` de `MenuSettingsView`). |
| `EchoScrap.Audio.MusicVolume` | `float` (`0.85`) | Volumen de música (`CombatMusicPlayer.SetMusicVolume` y `MenuMusic.RefreshVolume`, deslizador en pestaña `Sonido` de `MenuSettingsView`). |
| `EchoScrap.PendingGamertag.{sha256}` | `string` (`32 hex`) | Recibo local temporal de reserva de nombre en curso (`GamertagRegistry`). |
| `EchoScrap.Collection.*` / `EchoScrap.Rank.v2` / `EchoScrap.Event.PlayerName` | Claves `PlayerPrefs` heredadas | Claves de versiones previas migradas automáticamente hacia `Saves/` durante `SaveSlotService.EnsureMigrated()` (mantenidas como respaldo si no hay ranura cargada). |
| `https://echoscrap-default-rtdb.firebaseio.com` y [`https://echoscrap.web.app`](https://echoscrap.web.app) | Nodos `/gamertags/{sha256}` y `/players/{sessionId}` (`Tools/WebRanking/index.html`) | Reserva atómica de gamertag (`PUT` con `if-match: null_etag`, `timeout = 8 s`, mayúsculas conservando tildes) y sincronización en vivo cada `2 s` (`PUT` con `heartbeat` y `timeout = 8 s`, `attemptId` de `32` caracteres hexadecimales). **Todos los intentos válidos** finalizados (`VICTORIA`/`DERROTA`) se publican y el ranking conserva la mejor puntuación por gamertag (`DELETE` al volver voluntariamente a bóveda o reiniciar). Puntuación `rulesVersion = 2`: `RawPoints = 100 × kills + 300 × units + 150 × discoveries + 250 × (highest + 1) + 2 × seconds` (`100 %` en modo `3 MIN`, `÷ 2` hacia abajo en modo `8 MIN`). |

En Windows, `PlayerPrefs` almacena estas claves de configuración en el registro del usuario bajo `HKEY_CURRENT_USER\Software\DefaultCompany\EchoScrap` y los archivos de guardado en `%USERPROFILE%\AppData\LocalLow\DefaultCompany\EchoScrap\Saves\`. En macOS, se ubican bajo `~/Library/Application Support/DefaultCompany/EchoScrap/Saves/`.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]