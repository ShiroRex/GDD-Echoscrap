# Objetos y coleccionables

## Gemas y botiquines

Las gemas conceden la XP del enemigo derrotado y permanecen en el escenario hasta recogerse (fusionándose entre sí cuando caen a menos de `0,75 m`). Los botiquines curan `15 HP` de base (`17 / 19 / 21 HP` con `GREED_03`) y caducan tras `22 s`. Ambos responden al imán del jugador y a *Pulso colector* (`GREED_05`).

## Catálogo de 15 entidades de la colección

La colección registra permanentemente hasta **15 entidades descubiertas** (`discovered[15]`) y almacena un **máximo estricto de 10 unidades en posesión** (`FusionProgress.Capacity = 10`, `owned[15]` cuya suma nunca supera `10`). Nueve entidades aparecen como fragmentos en la arena y seis se obtienen mediante la máquina de fusión.

| ID | Entidad | Rareza en catálogo | Fuente | Poder de acompañante (`CompanionPowerRuntime`, **acumulable por copia equipada**) |
| ---: | --- | --- | --- | --- |
| 0 | Lobito | Común | Arena · 25% | `+10 HP` máximos (`+10 × copias`). |
| 1 | Mago | Raro (Poco común) | Arena · 10% | Lanza un proyectil mágico perforante de `18` de daño base cada `10 s` (un disparo por copia equipada). |
| 2 | Demonio | Raro (Poco común) | Arena · 10% | `+15%` de daño por copia; `30%` de probabilidad por copia de aplicar quemadura de `4` de daño base/s durante `3 s`. |
| 3 | Shireen | Especial (Rara) | Arena · 5% | Cura `2 HP` por copia cada `5 s`. |
| 4 | Yanikua | Mítico (Legendaria) | Fusión (exclusiva) | Multiplicador de salud máxima `1 + copias` (`×2 / ×3 / ×4`); `+20%` de daño y `+15%` de velocidad de movimiento por copia; **1 salvación ante golpe letal por copia en cada expedición** (restaura al `30%` de HP máx. y otorga `3 s` de invulnerabilidad). No puede usarse como ingrediente de fusión. |
| 5 | Basket | Común | Arena · 25% | `+10%` de velocidad de movimiento por copia. |
| 6 | Hada | Épico | Arena · 7% | Bloquea por completo `1` golpe cada `20 s` (cada copia equipada gestiona un enfriamiento independiente de `20 s`). |
| 7 | Lisa | Legendario | Arena · 3% | `+20%` de cadencia de ataque por copia; cada `12 s` congela `2 s` por copia en un radio de `8 m` (a Gólgora lo ralentiza hasta su tope del `25%`). |
| 8 | Reaper | Común | Fusión | `+8%` de daño por copia. |
| 9 | Chico Emo | Raro (Poco común) | Fusión | Suma `+100%` (`×2 / ×3 / ×4`) a la velocidad de movimiento durante `2 s` al terminar el dash. |
| 10 | Conde Alucard | Especial (Rara) | Fusión | Cura `1 HP` por copia cada `5` bajas enemigas. |
| 11 | Murciélago Cristal | Épico | Fusión | Cada `8 s` emite una onda de `15` de daño base por copia en `8 m` que ralentiza un `40%` durante `3 s`. |
| 12 | Sirena | Legendario | Fusión | Cada `15 s` emite una onda en `8 m` que empuja `4 m` y cura el `5%` de la salud máxima por copia. |
| 13 | Soldier | Especial (Rara) | Arena · 5% | `+15 HP` máximos, `-15%` de daño recibido y `+10%` de armadura por copia equipada (`Soldier × N` en `PlayerRuntimeStats` y `PlayerHealth`). |
| 14 | Joel | Raro (Poco común) | Arena · 10% | `+15%` de cadencia de ataque por copia. |

Cada `30 s` de combate activo se intenta generar entre `1` y `3` fragmentos en posiciones válidas del mapa, con un máximo de `12` activos simultáneamente y `10 s` de duración por aparición. El jugador debe acercarse a menos de `2,5 m` con línea de visión despejada y pulsar **`E`** (o el botón oeste en mando).

## Capacidad máxima de 10 unidades y bloqueo por inventario lleno

La colección admite un máximo estricto de **10 unidades en total** (`FusionProgress.Capacity = 10`), que se corresponden uno a uno con los **10 pedestales físicos de la bóveda** (`DisplaySlots[10]`): **3 puestos de equipo activo** en el balcón superior (`slot 0..2`) y **7 puestos de exposición/reserva** en la planta inferior (`slot 3..9`). Cuando algún puesto del balcón está vacío, exhibe una copia en reserva etiquetada como `EQUIPO N · RESERVA`, permitiendo mostrar hasta las 10 unidades simultáneamente en la escena.

- **Bloqueo de recogida en la arena (`CollectibleShard.cs`, `CollectiblePickupFeedback.cs`):** si el jugador ya posee `10/10` unidades (`!FusionProgress.HasCollectionSpace`), no puede recoger más fragmentos en combate y aparece el aviso flotante **`INVENTARIO LLENO / LIBERA ESPACIO`**.
- **Bloqueo de reclamo en la máquina de fusión (`FusionStationUI.cs`):** si el jugador intenta reclamar el resultado de una fusión teniendo `10/10` unidades, la máquina muestra **`INVENTARIO LLENO · LIBERA ESPACIO`** (*"Tu colección tiene 10 unidades. Libera una en la bóveda y vuelve: el resultado de esta fusión seguirá aquí."*) y mantiene el resultado guardado en estado `AwaitingDecision` hasta que libere espacio en algún pedestal de la bóveda.

## Interacción con los puestos de la bóveda y señalización

La bóveda cuenta con señalización completa en el mundo y en el HUD (`GameMenuView.cs`, `CollectibleDisplay.cs`):

1. **Rótulos 3D sobre los pedestales del balcón:** **`EQUIPO 1`**, **`EQUIPO 2`** y **`EQUIPO 3`** (añadiendo `· RESERVA` cuando exhiben una unidad sin equipar).
2. **Indicadores direccionales superiores (`VaultDirections`):** dos tarjetas dinámicas en la parte superior con flechas (`▲`, `▼`, `◄`, `►`, `●`) que apuntan en tiempo real hacia la **Máquina de Fusión** (*"MÁQUINA DE FUSIÓN · Combina 4 copias de varias rarezas"*, *"BAJA A FUSIÓN"* o *"E · MÁQUINA DE FUSIÓN"*) y hacia los **Puestos de Equipo** (*"SUBE AL BALCÓN · Puestos de equipo 1, 2 y 3"*, *"EQUIPAR ENTIDADES"* o *"PUESTO 1..3 · EQUIPAR/EDITAR"*).
3. **Panel rotatorio de consejos (`VaultTips`):** tarjeta en la esquina inferior izquierda con la etiqueta **`CONSEJO`** que rota cada `8 s` entre *"Equipa coleccionables antes de ir a la arena."*, *"Abre el índice para consultar tus descubrimientos."* y *"Combina cuatro coleccionables en la máquina de fusión."*, o prioriza *"Pulsa E frente a un puesto para equipar tu primer coleccionable."* cuando `OwnedCount > 0 && !HasEquippedOnce`.

Las interacciones de cada nivel de la bóveda son:

- **Puestos 1 a 3 (balcón superior, equipo activo — `CompanionSystem.cs`, `CompanionView.cs`, `VaultReleaseInteraction.cs`):**
  - El aviso contextual al acercarse es **`E · EQUIPAR`**, **`E · LIBERAR`** (si muestra una copia de reserva) o **`E · EDITAR`** (si tiene una entidad equipada).
  - Pulsar **`E`** (o botón oeste en mando) abre el selector de acompañantes (`CompanionView`, encabezado `PUESTO DE EQUIPO 1/2/3`, subtítulo `ELIGE UNA ENTIDAD DESCUBIERTA PARA ESTE PUESTO` y la nota *"Señala un compañero para ver su poder. Los poderes se acumulan por copia equipada."*). Su barra inferior ofrece tres botones: **`QUITAR DEL PUESTO`** (desequipa la unidad y la devuelve a reserva), **`LIBERAR COPIA`** (abre el modal de confirmación para eliminar la copia de la colección) y **`CERRAR · E / ESC`**.
  - Además, al estar frente a un puesto de equipo ocupado en el mundo, pulsar **`T`** (o el botón norte `Y` / `Triángulo` en mando) retira directamente la unidad del puesto de equipo sin abrir el menú (`CompanionSystem.ClearSlot`).
- **Puestos 4 a 10 (planta inferior, exposición/reserva — `VaultReleaseInteraction.cs`):**
  - Al acercarse a un puesto ocupado, el aviso contextual es **`E · LIBERAR`**.
  - Pulsar **`E`** (o botón oeste en mando) abre el cuadro de confirmación **`LIBERAR UNIDAD`**: *"¿Seguro que quieres liberar a `<Nombre>`? Esta unidad desaparecerá de tu colección."* con los botones **`CANCELAR`** (`Esc` / botón este) y **`LIBERAR`** (`E`, `Enter` o `Espacio`).
  - Al confirmar (`FusionProgress.ReleaseUnit`), se descuenta `1` de `owned[id]`, se conserva permanentemente `discovered[id]` en el Índice de Colección, se reordenan los puestos de exposición por rareza y se guarda en `PlayerPrefs`.

![Selector de entidades del puesto](../Media/Interfaz/Equipar_coleccionable.png)
*Selector de entidades del puesto en la versión final de Unity (`b381ef4`), con la nota de acumulación por copia equipada y los tres botones inferiores integrados: `QUITAR DEL PUESTO`, `LIBERAR COPIA` y `CERRAR · E / ESC`.*

## Índice de Colección (Bestiario de entidades — `VaultCollectionIndex.cs`, tecla `[I]`)

El **Índice de Colección** funciona como el **bestiario y libro de registro** donde el jugador consulta qué criaturas ha descubierto, cuáles le faltan para completar las **15 entidades** del catálogo y qué unidades conserva actualmente en su bóveda.

- **Control de apertura y cierre (`[I]`):** dentro de la bóveda se abre pulsando la tecla **`I`**, el botón **`Select`** en mando o haciendo clic en el botón superior izquierdo **`ÍNDICE · [I]`** (`VaultIndexHint`), lo que pausa la simulación (`PauseReason.FusionMenu`). Se cierra con **`I`**, **`Select`**, **`Esc`**, botón este (`B` / `Círculo`) o el botón **`CERRAR · [I]`** *(nota: la tecla `Tab` está reservada exclusivamente al laboratorio de pruebas `EnemyLaboratory`, no al bestiario)*.
- **Encabezado y contadores:** muestra el título **`ÍNDICE DE COLECCIÓN`** y el contador en vivo **`{descubiertos} / 15 DESCUBIERTOS · {en posesión} / 10 UNIDADES`**.
- **Qué acción cuenta como descubrimiento (`FusionProgress.CollectFromArena` y `FusionProgress.Resolve`):** en la versión jugable, la ficha de una criatura (`discovered[id] = true`) se desbloquea **en el instante en que el jugador la obtiene por primera vez**, ya sea **recogiendo su fragmento en la arena con `E` / botón oeste** o **reclamándola en la máquina de fusión con el botón `COLECCIONAR`** (ver aparecer un fragmento en el suelo sin recogerlo no cuenta como descubrimiento). Al descubrirla por primera vez, su silueta negra cambia de forma permanente a su retrato a color, se incrementa el contador `{descubiertos} / 15` (y suma `+150` puntos base si ocurre durante la expedición puntuable) y el desbloqueo se mantiene para siempre aunque después se liberen sus copias en la bóveda (`ReleaseUnit`).
- **Pestaña 1 — `POR RAREZAS` (15 entidades en 6 categorías):** incluye el filtro **`TODAS`** además de las **6 categorías de rareza** (`COM`, `RAR`, `ESP`, `ÉPI`, `LEG`, `MÍT`):
  - **Común (`3`):** *Lobito, Basket y Reaper*.
  - **Raro / Poco común (`4`):** *Mago, Demonio, Chico Emo y Joel*.
  - **Especial / Rara (`3`):** *Shireen, Conde Alucard y Soldier*.
  - **Épico (`2`):** *Hada y Murciélago Cristal*.
  - **Legendario (`2`):** *Lisa y Sirena*.
  - **Mítico (`1`):** *Yanikua*.
  Las criaturas que aún faltan por descubrir aparecen como **siluetas negras** (`Color(0.03, 0.03, 0.04, 0.96)`) con un signo de interrogación (**`???`**) y el aviso **`NO ENCONTRADO`** / **`SILUETA BLOQUEADA`** (*"Aún no has registrado esta entidad. Encuéntrala en la arena o mediante la máquina de fusión para revelar su retrato y su efecto de compañero."*).
- **Pestaña 2 — `EN TU BÓVEDA`:** lista únicamente las unidades actualmente en posesión (`hasta 10 unidades`), indicando en cada tarjeta si se encuentra **`EN EQUIPO ACTIVO`** (puestos 1–3) o **`EN BÓVEDA`**.
- **Panel derecho de inspección (`PanelDetalleIndice`):** al seleccionar cualquier entidad descubierta, muestra su insignia de rareza, retrato a color ampliado, fuente de obtención (*Arena* o *Máquina de fusión*), copias actuales en posesión (`xN`) y la descripción completa de su **poder de compañero**.

![Índice de Colección (Bestiario en versión final)](../Media/Interfaz/Bestiario.png)
*Captura de la versión final del bestiario (`Índice de Colección`), mostrando el contador en vivo, la navegación por rarezas con Pixelify Sans, las siluetas bloqueadas (`???`) y la inspección completa de poder y origen.*

## Máquina de fusión (`FusionProgress.cs`, `FusionStation.cs`, `FusionStationUI.cs`, `CollectibleUnitGrid.cs`)

La estación central de la bóveda se abre con **`E`** (o botón oeste en mando: **`E · FUSIONAR`** cuando está libre o en cuenta atrás, o **`E · VER FUSIÓN`** cuando un resultado aguarda en `AwaitingDecision`).

### Reglas de ingredientes y duración fija de 30 segundos

1. **Duración fija de 30 segundos (`FusionRecipe.FusionSeconds = 30`):** **todas las fusiones duran exactamente `30 s` (`00:30`)**, sin importar la rareza de los ingredientes. El temporizador avanza en tiempo real (`Time.unscaledDeltaTime`) y persiste entre escenas.
2. **Cuatro unidades de la misma rareza o de rarezas mixtas (excluida Yanikua):** acepta **cualquier combinación de 4 copias válidas** (`FusionRecipe.TryPreview` / `TryCreate`), permitiendo tanto recetas homogéneas como mezclas de distintas rarezas. El resultado interpola linealmente los pesos internos en función de la calidad media (`qualityMean = total / 4f`). **Yanikua** (`id = 4`) está explícitamente excluida como ingrediente (`id != 4`).
3. **Uso permitido de copias equipadas (`EN EQUIPO 1/2/3`):** en el selector de unidades de la máquina (`CollectibleUnitGrid.cs`), las copias equipadas en los puestos 1 a 3 del balcón **sí se pueden seleccionar como ingredientes** y aparecen con la etiqueta **`EN EQUIPO 1/2/3`** (mientras que las ya asignadas a otra ranura de la máquina muestran `RANURA 1..4`). Al pulsar **`CONFIRMAR`** (`FusionProgress.Begin`), si las copias seleccionadas superan las copias en reserva, el sistema las desequipa automáticamente de sus puestos de equipo y consume las 4 unidades.

### Probabilidades de fusión y redondeo en interfaz

La tabla muestra los puntos de referencia cuando los 4 ingredientes pertenecen a la misma rareza (en combinaciones de rarezas mixtas, los porcentajes cambian de forma continua mediante interpolación lineal entre bandas adyacentes):

| Receta de entrada (4 ingredientes homogéneos) | Duración | Pesos internos por entidad (`FusionProgress.cs`) | Porcentajes mostrados en UI (`FusionRecipe.DisplayPercent`) |
| --- | ---: | --- | --- |
| **4 Comunes** (`media 0,0`) | `30 s` (`00:30`) | Reaper `60%` · Chico Emo `40%` | **`60%` Reaper · `40%` Chico Emo** |
| **4 Raros / Poco comunes** (`media 1,0`) | `30 s` (`00:30`) | Chico Emo `60%` · Conde Alucard `40%` | **`60%` Chico Emo · `40%` Conde Alucard** |
| **4 Especiales / Raros** (`media 2,0`) | `30 s` (`00:30`) | Conde Alucard `60%` · Murciélago Cristal `40%` | **`60%` Conde Alucard · `40%` Murciélago Cristal** |
| **4 Épicos** (`media 3,0`) | `30 s` (`00:30`) | **Murciélago Cristal `59,4%` · Sirena `39,6%` · Yanikua `1%`** | **`60%` Murciélago Cristal · `40%` Sirena · `1%` Yanikua** |
| **4 Legendarios** (`media 4,0`) | `30 s` (`00:30`) | Sirena `100%` *(ver nota superior Mítica)* | **`100%` Sirena** |

*(Nota sobre el techo de la tabla en `FusionProgress.cs:62-75`: en la versión final `table` termina en `{0,0,0,0,100}` para `Sirena`, mientras que la fila de `Yanikua` sigue la curva `{30,15,5,1,0}` sobre el índice `0..4`, por lo que `4 Épicas` conserva `59,4%` Murciélago Cristal / `39,6%` Sirena / `1%` Yanikua y la UI aplica el ajuste explícito `60% / 40% / 1%`).*

![Máquina de fusión con cuatro espacios de ingredientes y posibles resultados](../Media/Interfaz/Maquina_Fusion.png)
*Ventana definitiva de la Máquina de Fusión (`b381ef4`) con las 4 ranuras horizontales, temporizador `TIEMPO DE FUSIÓN 00:30`, panel `POSIBLES RESULTADOS` y botones `VACIAR`, `CONFIRMAR` y `X`.*

### Flujo de ventanas de la estación

- **Ventana principal:** presenta las **4 ranuras en fila horizontal** (`Espacio_0` a `Espacio_3`), el contador fijo **`TIEMPO DE FUSIÓN  00:30`**, el panel **`POSIBLES RESULTADOS`** actualizado en vivo para recetas homogéneas o mixtas, los botones **`VACIAR`** y **`CONFIRMAR`**, y el botón superior derecho **`X`** para cerrar.
- **Selector de ranura:** muestra en cuadrícula las unidades disponibles (incluidas `EN EQUIPO 1/2/3`) junto a los botones **`QUITAR INGREDIENTE`** y **`VOLVER`**.
- **Pantalla de resultado (`AwaitingDecision`):** al completarse los `30 s` y abrir la estación con `E · VER FUSIÓN`, se reproduce la animación de apertura y se muestra la unidad obtenida con el botón único **`COLECCIONAR`** (más la `X` de cierre). **No existe botón "Liberar" en la pantalla de resultado de la máquina**; la liberación se realiza en los pedestales de la bóveda (`Media/Interfaz/Resultado_Fusion.png`).

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
