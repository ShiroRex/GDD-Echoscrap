# Guardado y configuración

## Colección persistente (`EchoScrap.Collection.Production.v1`)

El sistema de colección y fusión (`FusionProgress.cs`, `FusionStation.cs`) serializa su estado en formato JSON (`FusionProgress`, `version = 4`) dentro de `PlayerPrefs` bajo la clave **`EchoScrap.Collection.Production.v1`** (migrando desde `EchoScrap.Collection.v1` cuando contiene `owned != null` y descartando guardados prototipo `version < 4` sin `owned`; la marca de inicio se guarda en **`EchoScrap.CollectionStarted.v1`**):

- **`version` (`int`):** versión del esquema (`4`).
- **`hasEquippedCompanion` (`bool`):** registra si el jugador ya ha equipado al menos un acompañante alguna vez (utilizado por `VaultTips` para priorizar el consejo inicial cuando vale `false`).
- **`equipped` (`int[3]`):** IDs de las entidades equipadas en los puestos 1 a 3 del balcón superior (`-1` si el puesto está vacío).
- **`owned` (`int[15]`):** copias en posesión de cada una de las 15 entidades. La suma total tiene un **límite máximo estricto de 10 unidades** (`FusionProgress.Capacity = 10`, `OwnedCount <= 10`).
- **`discovered` (`bool[15]`):** registro permanente de las entidades descubiertas para el **Índice de Colección (`[I]`)** y los selectores de puesto. Liberar una copia descuenta `1` de `owned[id]`, pero conserva siempre `discovered[id] = true`.
- **`displays` (`int[10]`):** IDs mostrados en los **10 pedestales físicos de la bóveda** (`SlotCount = 10`: los **3 puestos de equipo** en `slot 0..2` y los **7 puestos de exposición/reserva** en `slot 3..9`).
- **`phase` (`int` / `FusionPhase`), `result` (`int`), `remaining` (`float`), `duration` (`float`), `opening` (`float`):** estado de la máquina de fusión en **5 valores** (`0 = Idle`, `1 = Fusing`, `2 = Opening`, `3 = AwaitingDecision`, `4 = Ready`), entidad resultante ya sorteada, tiempo restante, duración fija de **`30 s` (`00:30`)** y progreso normalizado `0..1` de la apertura de puerta (`1,2 s`).

El guardado se ejecuta al recoger un fragmento en la arena, equipar o desequipar en el balcón, liberar una copia, iniciar o reclamar una fusión, cada `5 s` en tiempo real mientras corre la cuenta atrás de `30 s` y al pausar o cerrar la aplicación. El tiempo con la aplicación cerrada no adelanta la cuenta atrás de la fusión. Si la aplicación se cierra durante la animación de apertura (`Opening`), `RestoreAfterLoading()` pasa el estado a `AwaitingDecision` (`opening = 1f`); si estaba en `Ready`, conserva `Ready` para no perder el resultado.

## Audio, preferencias y sincronización de Ranking (`EventLeaderboardSync`, `GamertagRegistry`)

Las claves adicionales persistidas en `PlayerPrefs` son:

| Clave | Tipo / Valor por defecto | Función |
| --- | --- | --- |
| `EchoScrap.Audio.MasterVolume` | `float` (`0.85`) | Volumen general (`0%`–`100%` en `MenuSettingsView`). |
| `EchoScrap.Audio.MusicVolume` | `float` (`0.85`) | Volumen de música (`0%`–`100%` en `MenuSettingsView`, aplicado por `CombatMusicPlayer` y `MenuMusic`). |
| `EchoScrap.Rank.v2` *(o `.Test.v2` en batchMode)* | `string` (JSON `RankingLedger`) | Estado del ranking por colección: lista de nombres locales (`names`), resultados pendientes de envío (`pending`), nombre activo (`name`), intento interrumpido (`interrupted`) y bandera de elegibilidad (`eligible`). |
| `EchoScrap.PendingGamertag.{sha256}` | `string` (`32 hex`) | Recibo local por alias para confirmar reservas cuyo primer `PUT` pudo haberse completado antes de perder conexión (`GamertagRegistry`). |
| `EchoScrap.Event.PlayerName` | `string` (máx. `20` caract.) | Nombre introducido en `NOMBRE DEL JUGADOR (RANKING)...` al crear una nueva colección. |

Al iniciar una **Nueva colección** con un nombre nuevo, **`GamertagRegistry`** normaliza espacios (`FormKC`), pasa a mayúsculas conservando tildes (`CanonicalName`), calcula su hash SHA-256 y reserva `/gamertags/{sha256}.json` (`PUT` con `if-match: null_etag`, `timeout = 8 s`), habilitando **una única oportunidad puntuable** (`eligible = true`, que **se conserva al pulsar Continuar** hasta cerrar la primera `VICTORIA` o `DERROTA`). Durante esa expedición (`EN PARTIDA`), **`EventLeaderboardSync`** autentica mediante Firebase Anonymous Auth (`timeout = 5 s`) y sincroniza cada `2 s` el nodo `/players/{sessionId}.json` (`PUT` con `timeout = 8 s`, usando `attemptId` de 32 caracteres hexadecimales), alimentando el visor web [`https://echoscrap.web.app`](https://echoscrap.web.app).

La puntuación se calcula con `rulesVersion = 2` (`RawPoints = 100 × kills + 300 × units + 150 × discoveries + 250 × (highest + 1) + 2 × seconds`, conservando el `100 %` en la ronda de `3 min` y dividiendo entre `2` hacia abajo en la ronda de `8 min`). La primera **`VICTORIA`** o **`DERROTA`** consume `eligible` y encola el resultado en `ledger.pending` hasta recibir confirmación HTTP de Firebase, mientras que abandonar o cerrar el juego antes de terminar envía `DELETE` a `/players/{sessionId}` y restaura `eligible = true` para el siguiente intento.

![Página web del Ranking en vivo (`echoscrap.web.app`)](../Media/Interfaz/RankingWeb_EchoScrap.png)

Los ajustes de la sesión en curso (`Movimiento de cámara`, `Efectos decorativos`, `Estadísticas generales` y `Atributos de coleccionables SÍ/NO`) se conservan entre transiciones de escena mientras la aplicación permanece abierta.

## Estado de la expedición

La salud, XP, nivel y cartas pertenecen al intento activo. Volver desde resultados o desde el menú de pausa limpia el combate y devuelve a la preparación en la bóveda, reaplicando los poderes de los tres puestos de equipo.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
