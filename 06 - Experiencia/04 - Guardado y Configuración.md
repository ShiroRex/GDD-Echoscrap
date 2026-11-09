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

## Audio, preferencias y sincronización de Ranking (`EventLeaderboardSync`)

Las claves adicionales persistidas en `PlayerPrefs` y disco son:

| Clave o archivo | Tipo / Valor por defecto | Función |
| --- | --- | --- |
| `EchoScrap.Audio.MasterVolume` | `float` (`0.85`) | Volumen general (`0%`–`100%` en la pestaña `Sonido` de pausa). |
| `EchoScrap.Audio.MusicVolume` | `float` (`0.85`) | Volumen de música (`0%`–`100%` en la pestaña `Sonido` de pausa, aplicado por `ArenaMusicController` / `CombatMusicPlayer`). |
| `EchoScrap.Event.PlayerName` | `string` (máx. 20 caracteres) | Nombre introducido en `NOMBRE DEL JUGADOR (RANKING)...` en portada o selección de ronda. |
| `EchoScrap.Event.SessionId` | `string` (GUID) | Identificador único de sesión para el ranking del evento. |
| `EchoScrap_EventRanking_LastSession.json` | Archivo JSON en `Application.persistentDataPath` | Respaldo local de las métricas de sesión escrito por `EventLeaderboardSync`. |

Al finalizar una incursión (o al recoger fragmentos y descubrir entidades), **`EventLeaderboardSync`** guarda el respaldo JSON local y sincroniza mediante peticiones HTTPS `PATCH` con **Firebase Realtime Database** (`https://echoscrap-default-rtdb.firebaseio.com`, nodos `event_leaderboard/current/players/{sessionId}` y `runs/{runId}`) el nombre del jugador, bajas totales (`kills`), coleccionables obtenidos (`collectibles`), entidades descubiertas (`discoveredCount / 15`), mejor tiempo de supervivencia (`bestSurvivalSeconds`), rondas jugadas/victorias y la puntuación calculada (`kills * 10 + collectibles * 250 + discoveredCount * 500 + round(bestSurvivalSeconds * 2)`).

Los ajustes de la sesión en curso (`Movimiento de cámara`, `Efectos decorativos`, `Estadísticas generales` y `Atributos de coleccionables SÍ/NO`) se conservan entre transiciones de escena mientras la aplicación permanece abierta.

## Estado de la expedición

La salud, XP, nivel y cartas pertenecen al intento activo. Volver desde resultados o desde el menú de pausa limpia el combate y devuelve a la preparación en la bóveda, reaplicando los poderes de los tres puestos de equipo.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
