# Datos y persistencia

Los valores del jugador, los arquetipos enemigos y las oleadas se configuran mediante `ScriptableObjects`. `ExpeditionEntrySettings` enlaza las rondas demo (`3:00`, `3 600 HP`) y completa (`8:00`, `9 000 HP`), mientras que `GolgoraCombatSettings` define los parámetros base del jefe (`15 000 HP` base, tiempos, daños, áreas y multiplicadores de Fase 2).

El catálogo de combate consta de `35` mejoras (`UpgradeId`). El catálogo de entidades consta de `15` identificadores (`0` a `14`), distribuidos en seis rarezas y dos fuentes de obtención (nueve en la arena y seis por fusión). `CompanionSystem` comunica los tres puestos de equipo a `CompanionPowerRuntime` y a `PlayerRuntimeStats`.

## Sesión en memoria

El reloj de incursión, la salud actual, la XP, el nivel, las cartas adquiridas, los enemigos activos y los objetos del suelo pertenecen a la sesión en memoria. Al regresar a la bóveda tras una victoria, una derrota o desde el menú de pausa se limpia el estado de la arena y se reaplican los poderes de las entidades equipadas. Una visita temporal a la bóveda por el portal norte suspende el combate sin destruir la sesión activa.

## Persistencia de la colección y fusión (`SaveData` v4)

`FusionProgress` serializa la colección, el equipo, la exposición y la máquina de fusión en un objeto JSON (`SaveData`, `version = 4`) almacenado en `PlayerPrefs` bajo la clave principal **`EchoScrap.Collection.Production.v1`** (con migración automática desde `EchoScrap.Collection.v1` y marca de inicialización en **`EchoScrap.CollectionStarted.v1`**).

La capacidad máxima de la colección es de **10 unidades en total** (`FusionProgress.Capacity = 10`, `OwnedCount <= 10`), que se corresponden exactamente con los **10 pedestales físicos de la bóveda** (`SlotCount = 10`): **3 puestos de equipo activo** en el balcón superior (`slot 0..2`) y **7 puestos de exposición/reserva** en la planta inferior (`slot 3..9`).

| Campo del JSON (`SaveData`) | Tipo | Contenido y reglas de validación (`FusionProgress.cs`) |
| --- | --- | --- |
| `version` | `int` | Versión del esquema de guardado. El valor vigente es **`4`**. |
| `hasEquippedCompanion` | `bool` | Registra si el jugador ya ha equipado al menos un acompañante alguna vez (`HasEquippedOnce`, usado por `VaultTips`). |
| `equipped` | `int[3]` | IDs de las entidades asignadas a los **3 puestos de equipo activo** del balcón (puestos 1 a 3) o `-1` si el puesto está vacío. |
| `owned` | `int[15]` | Copias en posesión de cada una de las 15 entidades (`ID 0..14`). Los valores negativos se ajustan a `0` y la **suma total está limitada a un máximo de `10` unidades** (`Capacity = 10`). |
| `discovered` | `bool[15]` | Registro permanente de descubrimiento de las 15 entidades para el **Índice de Colección (`[I]`, `VaultCollectionIndex`)** y los selectores de puesto. Toda entidad con `owned[i] > 0` marca `discovered[i] = true`, y liberar una copia conserva `discovered[i] = true`. |
| `displays` | `int[10]` | Array de longitud **`10`** (`SlotCount = 10`) que representa el total de los **10 puestos físicos de la bóveda**: los **3 puestos de equipo** (`displays[0..2]`, sincronizados con `equipped[0..2]`) más los **7 puestos de exposición/reserva** de la planta baja (`displays[3..9]`, ordenados por rareza descendente). `-1` indica pedestal vacío. |
| `phase` | `int` | Estado de la máquina de fusión: `0 = Idle`, `1 = Running`, `2 = AwaitingDecision`. |
| `result` | `int` | ID de la entidad resultante sorteada al iniciar la fusión (`-1` cuando está libre). |
| `remaining` | `float` | Segundos restantes de la cuenta atrás de fusión (`Time.unscaledDeltaTime`). |
| `duration` | `float` | Duración total de la fusión activa: **fija en `30 s` (`00:30`, `FusionRecipe.FusionSeconds = 30`)** para todas las rarezas. |
| `opening` | `bool` | Indica si la animación de apertura de la persiana estaba en curso; si la aplicación se cierra durante la apertura, al recargar se restablece a `AwaitingDecision` (`opening = false`) para no perder el resultado. |

### Reglas de carga, validación y escritura
- **Validación al cargar (`Validate`):** Sanea IDs fuera de rango, migra arrays de 13 a 15 posiciones dentro de la versión 4, garantiza que `discovered[id]` sea `true` para toda entidad con copias en `owned`, recorta cualquier exceso para que el total de unidades no supere `10` (`Capacity = 10`) y reconstruye `displays[10]` sincronizando los puestos `0..2` con `equipped[3]` y los puestos `3..9` con las copias libres ordenadas por rareza descendente.
- **Inicio y resolución de fusión (`FusionProgress.Begin`):** Al confirmar una fusión, si las 4 unidades seleccionadas incluyen copias equipadas (`EN EQUIPO 1/2/3`), el sistema las desequipa automáticamente de `equipped`, descuenta las 4 unidades de `owned`, sortea el resultado y fija `duration = 30` y `remaining = 30`. Durante la fase `Running`, el reloj avanza con `Time.unscaledDeltaTime` y persiste el progreso en `PlayerPrefs` cada `5 s`, además de hacerlo al pausar o cerrar la aplicación. Si al reclamar (`COLECCIONAR`) el jugador ya tiene `10/10` unidades, `FusionStation.Resolve` conserva el estado `AwaitingDecision` hasta que se libere una unidad en los pedestales de la bóveda.

## Otras claves de `PlayerPrefs` y sincronización de Ranking (`EventLeaderboardSync`, `GamertagRegistry`)

| Clave o ruta | Tipo / Valor por defecto | Descripción |
| --- | --- | --- |
| `EchoScrap.Collection.Production.v1` | `string` (JSON) | Estado completo de la colección, pedestales y fusión (`SaveData` v4). |
| `EchoScrap.CollectionStarted.v1` | `int` (`0` / `1`) | Indica si existe una colección iniciada para habilitar *Continuar* y pedir confirmación en *Nueva colección*. |
| `EchoScrap.Audio.MasterVolume` | `float` (`0.85`) | Volumen general (`AudioListener.volume`, deslizador en pestaña `Sonido`). |
| `EchoScrap.Audio.MusicVolume` | `float` (`0.85`) | Volumen de música (`ArenaMusicController.SetUserVolume` / `CombatMusicPlayer.SetMusicVolume`, deslizador en pestaña `Sonido`). |
| `EchoScrap.Rank.v2` | `string` (JSON `RankingLedger`) | Estado del ranking por colección (`usedNames`, `pending`, `cancelledSessionId`, `playerName`, `sessionId`, `eligible`). |
| `EchoScrap.GamertagReceipts.v1` | `string` (JSON) | Recibos locales de reserva de nombre en curso (`GamertagRegistry`). |
| `EchoScrap.Event.PlayerName` | `string` (máx. 20 caract.) | Nombre del jugador para el ranking del evento (`GameMenuView`, `EventLeaderboardSync`). |
| `EchoScrap.Event.SessionId` | `string` (12 hex) | Identificador único de la colección elegible para el ranking de Firebase. |
| `EchoScrap_EventRanking_LastSession.json` | Archivo JSON en `Application.persistentDataPath` | Respaldo local de las estadísticas de la expedición puntuable. |
| `https://echoscrap-default-rtdb.firebaseio.com` y [`https://echoscrap.web.app`](https://echoscrap.web.app) | Nodos `/gamertags/{sha256}` y `/players/{sessionId}` (`Tools/WebRanking/index.html`) | Reserva atómica de gamertag (`if-match: null_etag`) y sincronización en vivo cada `2 s` (`EN PARTIDA` con `heartbeat`, bloqueo definitivo en primera `VICTORIA`/`DERROTA` y `DELETE` al cancelar). Puntuación `rulesVersion = 2`: `RawPoints = 100 × kills + 300 × units + 150 × discoveries + 250 × (highest + 1) + 2 × seconds` (`100 %` en modo `3 MIN`, `÷ 2` hacia abajo en modo `8 MIN`). |

En Windows, `PlayerPrefs` almacena estas claves en el registro del usuario bajo `HKEY_CURRENT_USER\Software\DefaultCompany\EchoScrap`.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
