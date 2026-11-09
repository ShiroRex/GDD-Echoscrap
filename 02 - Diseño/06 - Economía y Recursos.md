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
- **En la máquina de fusión (`FusionStationUI.cs`):** cada receta consume **4 copias de la misma rareza** al confirmar (`30 s` de duración fija). Las copias equipadas en los puestos 1 a 3 **sí están disponibles como ingredientes** (marcadas como **`EN EQUIPO 1/2/3`**) y se desequipan automáticamente al iniciar la fusión si son necesarias para cubrir las 4 unidades (*Yanikua* está excluida como ingrediente). Al terminar los `30 s`, la pantalla de resultado muestra únicamente el botón **`COLECCIONAR`** (más la `X` de cierre). Si en ese momento la colección tiene `10/10` unidades, aparece el aviso **`INVENTARIO LLENO · LIBERA ESPACIO`** (*"Tu colección tiene 10 unidades. Libera una en la bóveda y vuelve: el resultado de esta fusión seguirá aquí."*) y el resultado permanece guardado en estado `AwaitingDecision` hasta que el jugador libere hueco.
- **Liberación en la bóveda (`VaultReleaseInteraction.cs`, `CompanionView.cs`):** para hacer espacio, el jugador puede acercarse a cualquier puesto ocupado de la planta inferior (puestos 4 a 10) y pulsar `E` (**`E · LIBERAR`**), o bien abrir un puesto de equipo ocupado en el balcón (`E · EDITAR`) y pulsar **`LIBERAR COPIA`**. Tras confirmar en el modal **`LIBERAR UNIDAD`**, se descuenta `1` de `owned[id]`, se reordenan los pedestales de exposición y se conserva intacto el registro de descubrimiento (`discovered[id]`) en el **Índice de Colección (`[I]`)**.

Las probabilidades de aparición, las recetas de fusión y los poderes de cada entidad se detallan en [[04 - Objetos y Coleccionables]].

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
