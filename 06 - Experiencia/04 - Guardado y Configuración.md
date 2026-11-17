# Guardado y configuración

## Guardado por ranuras, colección persistente y reanudación (`SaveSlotService`, `FusionProgress`, `ExpeditionSnapshotService`)

La persistencia principal de partidas abandona la clave única en favor de **3 ranuras independientes (`SaveSlotService.SlotCount = 3`)** almacenadas de forma atómica en disco bajo `Application.persistentDataPath/Saves/` (o `Saves_BatchTest/` en ejecución por lotes / pruebas):

- **`index.json` (`SaveIndexData`):** apunta a `lastPlayedSlot`, secuencia monotónica y estado de migración única (`legacyMigrated`) desde claves previas de `PlayerPrefs` (`EchoScrap.Collection.Production.v1`, `EchoScrap.Collection.v1`, `EchoScrap.Rank.v2` y `EchoScrap.Event.PlayerName`).
- **`slot_0.json`..`slot_2.json` (`SlotProfileData`):** cada ranura conserva `profileId`, `playerName`, `totalPlaySeconds`, `hasActiveExpedition`, `activeAttemptId`, `bestLocalScore`, `completedRankedRuns` y el estado íntegro de colección y fusión (`FusionProgress`, versión **`5`**):
  - **`hasEquippedCompanion` (`bool`):** registra si el jugador ya ha equipado al menos un acompañante alguna vez (utilizado por `VaultTips` para priorizar el consejo inicial cuando vale `false`).
  - **`equipped` (`int[3]`):** IDs de las entidades equipadas en los puestos 1 a 3 del balcón superior (`-1` si el puesto está vacío).
  - **`owned` (`int[15]`):** copias en posesión de cada una de las 15 entidades. La suma total tiene un **límite máximo estricto de 18 unidades** (`FusionProgress.Capacity = 18`, `OwnedCount <= 18`).
  - **`discovered` (`bool[15]`):** registro permanente de las entidades descubiertas para el **Índice de Colección (`[I]`)** y los selectores de puesto. Liberar una copia descuenta `1` de `owned[id]`, pero conserva siempre `discovered[id] = true`.
  - **`displays` (`int[18]`):** IDs mostrados en los **18 pedestales físicos de la bóveda** (`SlotCount = 18`: los **3 puestos de equipo** en `slot 0..2` y los **15 puestos de exposición/reserva** en `slot 3..17`).
  - **`phase` (`int` / `FusionPhase`), `result` (`int`), `remaining` (`float`), `duration` (`float`), `opening` (`float`):** estado de la máquina de fusión en **5 valores** (`0 = Idle`, `1 = Fusing`, `2 = Opening`, `3 = AwaitingDecision`, `4 = Ready`), entidad resultante ya sorteada, tiempo restante, duración fija de **`30 s` (`00:30`)** y progreso normalizado `0..1` de la apertura de puerta (`1,2 s`).
- **`expedition_0.json`..`expedition_2.json` (`ExpeditionSnapshotData`, `ExpeditionSnapshotService`):** durante una expedición activa se captura una instantánea completa de la simulación cada **`3 s`** (`AutosaveIntervalSeconds = 3f`) y ante hitos clave para reanudar exactamente desde `CONTINUAR` tras cerrar o suspender el juego (con recuperación segura a bóveda si el archivo estuviera dañado).
- **`ranking_outbox.json` (`RankingOutboxData`):** cola persistente por `ownerProfileId` para puntuaciones pendientes de envío a Firebase.

El guardado de ranura se ejecuta al recoger un fragmento en la arena, equipar o desequipar en el balcón, liberar una copia, iniciar o reclamar una fusión, periódicamente en la bóveda y en fusión cada `5 s` en tiempo real, y al pausar, salir al menú o cerrar la aplicación. El tiempo con la aplicación cerrada no adelanta la cuenta atrás de la fusión. Si la aplicación se cierra durante la animación de apertura (`Opening`), `RestoreAfterLoading()` pasa el estado a `AwaitingDecision` (`opening = 1f`); si estaba en `Ready`, conserva `Ready` para no perder el resultado.

## Audio, preferencias y sincronización de Ranking (`EventLeaderboardSync`, `GamertagRegistry`)

Las claves adicionales persistidas en `PlayerPrefs` son:

| Clave | Tipo / Valor por defecto | Función |
| --- | --- | --- |
| `EchoScrap.Audio.MasterVolume` | `float` (`0.85`) | Volumen general (`0%`–`100%` en `MenuSettingsView`). |
| `EchoScrap.Audio.MusicVolume` | `float` (`0.85`) | Volumen de música (`0%`–`100%` en `MenuSettingsView`, aplicado por `CombatMusicPlayer` y `MenuMusic`). |
| `EchoScrap.Graphics.FrameRateLimit` | `int` (`60`) | Tope de fotogramas seleccionado en pausa (`30`, `60` o `120` FPS; desactiva VSync mediante `QualitySettings.vSyncCount = 0`). |
| `EchoScrap.PendingGamertag.{sha256}` | `string` (`32 hex`) | Recibo local por alias para confirmar reservas cuyo primer `PUT` pudo haberse completado antes de perder conexión (`GamertagRegistry`). |
| `EchoScrap.Rank.v2` / `EchoScrap.Collection.*` | Legado migrado | Claves heredadas importadas automáticamente al directorio `Saves/` durante `SaveSlotService.EnsureMigrated()` (y usadas solo como respaldo sin perfil cargado). |

Al iniciar en **`NUEVA PARTIDA · ARCHIVO N`** con un alias válido, **`GamertagRegistry`** normaliza espacios (`FormKC`), pasa a mayúsculas conservando tildes (`CanonicalName`), calcula su hash SHA-256 y reserva `/gamertags/{sha256}.json` (`PUT` con `if-match: null_etag`, `timeout = 8 s`), permitiendo reutilizar el alias en el mismo dispositivo/perfil cuando ya posee recibo local o coincide con la reserva. Durante cada expedición (`EN PARTIDA`), **`EventLeaderboardSync`** autentica mediante Firebase Anonymous Auth (`timeout = 5 s`) y sincroniza cada `2 s` el nodo `/players/{sessionId}.json` (`PUT` con `timeout = 8 s`, usando `attemptId` de 32 caracteres hexadecimales), alimentando el visor web [`https://echoscrap.web.app`](https://echoscrap.web.app).

La puntuación se calcula con `rulesVersion = 2` (`RawPoints = 100 × kills + 300 × units + 150 × discoveries + 250 × (highest + 1) + 2 × seconds`, conservando el `100 %` en la ronda de `3 min` y dividiendo entre `2` hacia abajo en la ronda de `8 min`). Cada **`VICTORIA`** o **`DERROTA`** válida encola el resultado en `Saves/ranking_outbox.json` hasta recibir confirmación HTTP de Firebase, incrementa `completedRankedRuns` y actualiza `bestLocalScore` en la ranura; el ranking web conserva la mejor puntuación por gamertag. Regresar voluntariamente a la bóveda cancela el intento (`DELETE` en `/players/{sessionId}`), mientras que cerrar el juego en mitad de la arena preserva el snapshot sin cancelar la partida hasta su reanudación.

![Página web del Ranking en vivo (`echoscrap.web.app`)](../Media/Interfaz/RankingWeb_EchoScrap.png)

Los ajustes de la sesión en curso (`Movimiento de cámara`, `Efectos decorativos`, `Estadísticas generales` y `Atributos de coleccionables SÍ/NO`) se conservan entre transiciones de escena mientras la aplicación permanece abierta.

## Estado de la expedición

La salud, XP, nivel y cartas pertenecen al intento activo. Volver desde resultados o desde el menú de pausa limpia el combate y devuelve a la preparación en la bóveda, reaplicando los poderes de los tres puestos de equipo.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
