# Guardado y configuración

## Colección persistente (`EchoScrap.Collection.Production.v1`)

El sistema de colección y fusión (`FusionProgress.cs`, `FusionStation.cs`) serializa su estado en formato JSON (`SaveData`, `version = 4`) dentro de `PlayerPrefs` bajo la clave **`EchoScrap.Collection.Production.v1`** (con migración automática desde `EchoScrap.Collection.v1` y marca de inicio en **`EchoScrap.CollectionStarted.v1`**):

- **`version` (`int`):** versión del esquema (`4`).
- **`hasEquippedCompanion` (`bool`):** registra si el jugador ya ha equipado al menos un acompañante alguna vez (utilizado por `VaultTips` para priorizar el consejo inicial cuando vale `false`).
- **`equipped` (`int[3]`):** IDs de las entidades equipadas en los puestos 1 a 3 del balcón superior (`-1` si el puesto está vacío).
- **`owned` (`int[15]`):** copias en posesión de cada una de las 15 entidades. La suma total tiene un **límite máximo estricto de 10 unidades** (`FusionProgress.Capacity = 10`, `OwnedCount <= 10`).
- **`discovered` (`bool[15]`):** registro permanente de las entidades descubiertas para el **Índice de Colección (`[I]`)** y los selectores de puesto. Liberar una copia descuenta `1` de `owned[id]`, pero conserva siempre `discovered[id] = true`.
- **`displays` (`int[10]`):** IDs mostrados en los **10 pedestales físicos de la bóveda** (`SlotCount = 10`: los **3 puestos de equipo** en `slot 0..2` y los **7 puestos de exposición/reserva** en `slot 3..9`).
- **`phase` (`int`), `result` (`int`), `remaining` (`float`), `duration` (`float`), `opening` (`bool`):** estado de la máquina de fusión (`0 = Idle`, `1 = Running`, `2 = AwaitingDecision`), entidad resultante ya sorteada, tiempo restante, duración fija de **`30 s` (`00:30`)** e indicador de animación de apertura.

El guardado se ejecuta al recoger un fragmento en la arena, equipar o desequipar en el balcón, liberar una copia, iniciar o reclamar una fusión, cada `5 s` en tiempo real mientras corre la cuenta atrás de `30 s` y al pausar o cerrar la aplicación. El tiempo con la aplicación cerrada no adelanta la cuenta atrás de la fusión. Si la aplicación se cierra mientras la puerta de la máquina se estaba abriendo, al cargar vuelve al estado `AwaitingDecision` para que el jugador pueda reclamar el resultado con **`COLECCIONAR`**.

## Audio, preferencias y sincronización de Ranking (`EventLeaderboardSync`, `GamertagRegistry`)

Las claves adicionales persistidas en `PlayerPrefs` y disco son:

| Clave o archivo | Tipo / Valor por defecto | Función |
| --- | --- | --- |
| `EchoScrap.Audio.MasterVolume` | `float` (`0.85`) | Volumen general (`0%`–`100%` en la pestaña `Sonido` de pausa). |
| `EchoScrap.Audio.MusicVolume` | `float` (`0.85`) | Volumen de música (`0%`–`100%` en la pestaña `Sonido` de pausa, aplicado por `ArenaMusicController` / `CombatMusicPlayer`). |
| `EchoScrap.Rank.v2` | `string` (JSON `RankingLedger`) | Estado del ranking por colección: nombres usados (`usedNames`), resultado definitivo pendiente de confirmar (`pending`), cancelación pendiente (`cancelledSessionId`), nombre activo (`playerName`), `sessionId` y elegibilidad de única oportunidad (`eligible`). |
| `EchoScrap.GamertagReceipts.v1` | `string` (JSON) | Recibos locales de reserva de gamertag en curso (`GamertagRegistry`). |
| `EchoScrap.Event.PlayerName` | `string` (máx. 20 caracteres) | Nombre introducido en `NOMBRE DEL JUGADOR (RANKING)...` al crear una nueva colección. |
| `EchoScrap.Event.SessionId` | `string` (12 hex) | Identificador único de la colección elegible para el ranking. |
| `EchoScrap_EventRanking_LastSession.json` | Archivo JSON en `Application.persistentDataPath` | Respaldo local de las métricas de la expedición puntuable escrito por `EventLeaderboardSync`. |

Al iniciar una **Nueva colección** con un nombre nuevo, **`GamertagRegistry`** reserva de forma inmutable su hash SHA-256 en `/gamertags/{sha256}` (`if-match: null_etag`) y habilita **una única oportunidad puntuable** (`eligible = true`). Durante esa expedición (`EN PARTIDA`), **`EventLeaderboardSync`** autentica la sesión mediante Firebase Anonymous Auth y actualiza cada `2 s` el nodo `/players/{sessionId}` en **Firebase Realtime Database** (`https://echoscrap-default-rtdb.firebaseio.com`), alimentando en tiempo real la página web del ranking ([`https://echoscrap.web.app`](https://echoscrap.web.app), `Tools/WebRanking/index.html`).

La puntuación se calcula con `rulesVersion = 2` (`RawPoints = 100 × kills + 300 × units + 150 × discoveries + 250 × (highest + 1) + 2 × seconds`, conservando el `100 %` en la ronda de `3 min` y dividiendo entre `2` hacia abajo en la ronda de `8 min`). La primera **`VICTORIA`** o **`DERROTA`** bloquea el resultado definitivo (persistido en `ledger.pending` hasta recibir confirmación HTTP de Firebase), mientras que abandonar o cerrar el juego antes de terminar envía `DELETE` a `/players/{sessionId}` y conserva la oportunidad para el siguiente intento.

![Página web del Ranking en vivo (`echoscrap.web.app`)](../Media/Interfaz/RankingWeb_EchoScrap.png)

Los ajustes de la sesión en curso (`Movimiento de cámara`, `Efectos decorativos`, `Estadísticas generales` y `Atributos de coleccionables SÍ/NO`) se conservan entre transiciones de escena mientras la aplicación permanece abierta.

## Estado de la expedición

La salud, XP, nivel y cartas pertenecen al intento activo. Volver desde resultados o desde el menú de pausa limpia el combate y devuelve a la preparación en la bóveda, reaplicando los poderes de los tres puestos de equipo.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
