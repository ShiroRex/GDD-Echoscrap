# Economía y recursos

## Recursos de la expedición

La XP y las cartas pertenecen al intento actual. Las gemas de los cinco enemigos de horda conceden 10, 15, 25 o 50 XP según el arquetipo; los élites entregan 65 y 80 XP. *Chatarrero* (`GREED_02`) multiplica la experiencia recogida (`+15%` por nivel). Gólgora añade 500 XP al resumen de victoria sin abrir nuevas elecciones.

El botiquín base recupera 15 HP. La probabilidad parte del `3%` en enemigos normales y del `8%` en los de recompensa fuerte (Coloso, Mortero y ambos élites). Hay seis segundos como mínimo entre apariciones, hasta cinco botiquines simultáneos en el suelo y `22 s` de duración. *Botiquín mejor* (`GREED_03`) eleva la curación a `17 / 19 / 21 HP` y las probabilidades a `4 / 5 / 6%` (normales) y `10 / 12 / 15%` (pesados y élites).

## Colección, límite de 10 unidades y liberación

La colección registra el descubrimiento permanente de las **15 entidades** (`discovered[15]`) y almacena un **máximo estricto de 10 unidades en posesión simultánea** (`FusionProgress.Capacity = 10`, `OwnedCount <= 10`). Esas 10 unidades se corresponden exactamente con los **10 pedestales físicos de la bóveda** (`displays[10]`):

- **3 puestos de equipo activo** en el balcón superior (puestos 1 a 3, `slot 0..2`, rotulados `EQUIPO 1`, `EQUIPO 2`, `EQUIPO 3`).
- **7 puestos de exposición y reserva** en la planta inferior (puestos 4 a 10, `slot 3..9`), ordenados automáticamente por rareza descendente.

El límite de 10 unidades gobierna tanto la recogida en combate como el reclamo en la máquina de fusión:

- **En la arena (`CollectibleShard.cs`, `CollectiblePickupFeedback.cs`):** las copias recogidas con `E` se guardan de inmediato sin depender de derrotar a Gólgora. Si el jugador ya posee `10/10` unidades (`!FusionProgress.HasCollectionSpace`), no puede recoger más fragmentos y aparece el aviso flotante **`INVENTARIO LLENO / LIBERA ESPACIO`**.
- **En la máquina de fusión (`FusionStationUI.cs`):** cada receta consume **4 copias de cualquier rareza o combinación de rarezas** al confirmar (`30 s` de duración fija), interpolando los posibles resultados según la calidad media de los ingredientes (`qualityMean = total / 4f`). Las copias equipadas en los puestos 1 a 3 **sí están disponibles como ingredientes** (marcadas como **`EN EQUIPO 1/2/3`**) y se desequipan automáticamente al iniciar la fusión si son necesarias para cubrir las 4 unidades (*Yanikua* está excluida como ingrediente). Al terminar los `30 s`, la pantalla de resultado muestra únicamente el botón **`COLECCIONAR`** (más la `X` de cierre). Si en ese momento la colección tiene `10/10` unidades, aparece el aviso **`INVENTARIO LLENO · LIBERA ESPACIO`** (*"Tu colección tiene 10 unidades. Libera una en la bóveda y vuelve: el resultado de esta fusión seguirá aquí."*) y el resultado permanece guardado en estado `AwaitingDecision` hasta que el jugador libere hueco.
- **Liberación en la bóveda (`VaultReleaseInteraction.cs`, `CompanionView.cs`):** para hacer espacio, el jugador puede acercarse a cualquier puesto ocupado en la bóveda y pulsar `E` (**`E · LIBERAR`** en planta baja o en un puesto de equipo en modo reserva), o bien abrir un puesto de equipo ocupado en el balcón (`E · EDITAR`) y pulsar **`LIBERAR COPIA`**. Tras confirmar en el modal **`LIBERAR UNIDAD`**, se descuenta `1` de `owned[id]`, se reordenan los pedestales de exposición y se conserva intacto el registro de descubrimiento (`discovered[id]`) en el **Índice de Colección (`[I]`)**.

Las probabilidades de aparición, las recetas de fusión y los poderes de cada entidad se detallan en [[04 - Objetos y Coleccionables]].

## Cómo se puntúa una expedición y Ranking en vivo (`echoscrap.web.app`)

El sistema de clasificación (`EventLeaderboardSync.cs`, `GamertagRegistry.cs` y el visor web en vivo [`https://echoscrap.web.app`](https://echoscrap.web.app)) evalúa el desempeño del jugador combinando combate, supervivencia y coleccionismo bajo la versión de reglas `rulesVersion = 2`.

![Página web del Ranking en vivo de EchoScrap (`echoscrap.web.app`)](../Media/Interfaz/RankingWeb_EchoScrap.png)

### 1. Una oportunidad por nueva colección y reserva única de Gamertag

- Al elegir **Nueva colección** en el menú inicial, el cuadro `¿NUEVA COLECCIÓN?` solicita un `NOMBRE DEL JUGADOR (RANKING)...` (máximo `20` caracteres visibles).
- Si el jugador introduce un nombre nuevo, `GamertagRegistry.Reserve` normaliza Unicode (`FormKC`), recorta y colapsa espacios múltiples (`DisplayName`), convierte a mayúsculas invariantes conservando tildes (`CanonicalName`) y reserva su hash SHA-256 en Firebase (`/gamertags/{sha256}`) con escritura condicional (`if-match: null_etag`, `timeout = 8 s`), impidiendo nombres duplicados en cualquier instalación (*"Ese gamertag ya está en uso. Elige otro."*). Si deja el campo vacío, juega en **modo práctica** sin enviar puntuaciones al ranking. Al pulsar **Continuar** sobre una colección ya iniciada, se **conserva la oportunidad pendiente (`eligible`)** si aún no se ha cerrado la primera `VICTORIA` o `DERROTA`.
- **Una nueva colección con nombre válido concede exactamente una oportunidad de clasificación (`eligible = true` en `EchoScrap.Rank.v2`).** Al entrar a la arena (`BeginRankedExpedition`), `RankingLedger.Claim()` consume el estado `eligible` en la primera `VICTORIA` o `DERROTA` y la partida transmite su progreso en vivo cada `2 s` a `/players/{sessionId}` (`PUT`, `timeout = 8 s`, con `attemptId` de 32 caracteres hexadecimales).
- **La primera `VICTORIA` o `DERROTA` bloquea la puntuación definitiva** de esa colección. Las expediciones posteriores dentro de esa misma colección funcionan como práctica y no sobrescriben el registro del ranking.
- **Abandono o cierre imprevisto (`CancelAttempt` / `QueueCancellation`):** si el jugador vuelve a la bóveda desde el menú de pausa o cierra el juego antes de terminar la expedición puntuable, el intento se cancela (`DELETE` en `/players/{sessionId}`) y la colección **restaura y conserva intacta su oportunidad** (`eligible = true`) para el siguiente ingreso a la arena.

### 2. Puntos base de la expedición (`RawPoints`)

Durante la expedición puntuable se suman cuatro fuentes continuas y una bonificación única por la mayor rareza obtenida en esa incursión:

| Concepto en la expedición puntuable | Puntos base |
| --- | ---: |
| Baja de un enemigo (`kills`) | **`100`** |
| Unidad recogida en la arena (`E`) o resultado de fusión conservado (`COLECCIONAR`) (`units`) | **`300`** |
| Primer descubrimiento de esa entidad en la colección (`discoveries`) | **`150`** |
| Segundo completo de combate activo (`seconds`, `CombatSession.GameplayTime`) | **`2`** |
| **Mayor rareza obtenida en esa expedición (bonificación única):** | |
| · Común | **`250`** |
| · Rara *(Poco común)* | **`500`** |
| · Especial *(Rara)* | **`750`** |
| · Épica | **`1 000`** |
| · Legendaria | **`1 250`** |
| · Mítica | **`1 500`** |

$$\text{Puntos base} = 100 \times \text{bajas} + 300 \times \text{unidades} + 150 \times \text{descubrimientos} + 250 \times (\text{rareza máx.} + 1) + 2 \times \text{segundos}$$

*(Si no se obtiene ninguna unidad durante la expedición, la bonificación de rareza máxima es `0`. El reloj de combate activo se detiene en pausa y mientras el jugador está en la bóveda).*

### 3. Comparación justa entre Ronda Demo (`3 min`) y Ronda Completa (`8 min`)

Para que ambos modos compitan en la misma tabla de clasificación (`CalculateScore`):

- **Ronda demo de `3` minutos (`mode = "3 MIN"`):** conserva el `100 %` de los puntos base (`score = rawScore`).
- **Ronda completa de `8` minutos (`mode = "8 MIN"`):** divide el total de puntos base entre **`2`** y redondea hacia abajo (`score = rawScore / 2`).
- **Ejemplo:** una expedición con `20` bajas (`2 000`), `2` unidades obtenidas (`600`), `1` descubrimiento nuevo (`150`), rareza máxima Rara (`500`) y `90 s` de combate activo (`180`) suma **`3 430` puntos base**: registra **`3 430` pts** en la ronda de `3` minutos y **`1 715` pts** en la ronda de `8` minutos.

### 4. Qué aporta una fusión al ranking

Si el jugador entra a la bóveda durante su expedición puntuable y reclama el resultado de una fusión con **`COLECCIONAR`** (`RecordFusionOutput`), ese resultado cuenta como **una unidad obtenida (`+300` puntos base)**, añade **`+150` puntos base** si es la primera vez que se descubre esa entidad en la colección y puede elevar la **bonificación única de rareza máxima** de la expedición. En cambio, las `4` copias consumidas como ingredientes, el hecho de iniciar la máquina o liberar unidades en los pedestales aportan **`0` puntos**, y las fusiones realizadas antes de iniciar o después de cerrar la expedición puntuable tampoco modifican el marcador.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
