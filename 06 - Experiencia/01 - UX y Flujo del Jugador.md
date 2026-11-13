# UX y flujo del jugador

Menú inicial (con campo `NOMBRE DEL JUGADOR (RANKING)...`) → continuar o crear colección → bóveda (guiada por `EQUIPO 1..3`, `VaultDirections`, `VaultTips` e **Índice de Colección `[I]`**) → seleccionar entidades equipadas y/o fusionar copias (`30 s`) → elegir ronda demo o completa → expedición → sincronización de ranking (`EventLeaderboardSync`) y regreso a la bóveda.

## Preparar el equipo y gestionar la colección en la bóveda

**Continuar** recupera la colección guardada (`hasta 10 unidades` en posesión y `15` descubiertas en el índice). **Nueva colección** empieza con el inventario vacío y restablece la fusión y el equipo, tras confirmar si ya existe progreso.

Al entrar en la bóveda, la interfaz orienta al jugador en todo momento mediante tres sistemas complementarios:

1. **Rótulos 3D sobre los pedestales del balcón superior:** **`EQUIPO 1`**, **`EQUIPO 2`** y **`EQUIPO 3`** (`CollectibleDisplay.MarkEquipmentSlot`).
2. **Indicadores direccionales superiores (`VaultDirections`):** dos tarjetas dinámicas en la parte superior de la pantalla con flechas (`▲`, `▼`, `◄`, `►`, `●`) que apuntan en tiempo real hacia la **Máquina de Fusión** (*"MÁQUINA DE FUSIÓN · Combina 4 copias de la misma rareza"*, cambiando a *"BAJA A FUSIÓN"* en el balcón o *"E · MÁQUINA DE FUSIÓN"* al estar al lado) y hacia los **Puestos de Equipo** (*"SUBE AL BALCÓN · Puestos de equipo 1, 2 y 3"*, *"EQUIPAR ENTIDADES"* o *"PUESTO 1..3 · EQUIPAR/EDITAR"*).
3. **Panel de consejos (`VaultTips`):** tarjeta en la esquina inferior izquierda con la etiqueta **`CONSEJO`** que rota automáticamente cada `8 s` (o manualmente con `<` y `>`) entre tres mensajes:
   - *"Sube por las rampas al balcón para equipar hasta 3 entidades en los puestos 1, 2 y 3."*
   - *"Acércate a la máquina central y pulsa E para fusionar 4 copias de la misma rareza."*
   - *"En los puestos ocupados de la planta baja puedes pulsar E para liberar espacio."*
   Si el jugador posee coleccionables pero aún no ha equipado ninguno (`OwnedCount > 0 && !HasEquippedOnce`), el panel prioriza el aviso: *"Pulsa E frente a un puesto para equipar tu primer coleccionable."*

Además, pulsar **`I`** (o hacer clic en **`ÍNDICE · [I]`** arriba a la izquierda) abre el **Índice de Colección** (`VaultCollectionIndex.cs`), donde el jugador consulta el contador `{descubiertos} / 15 DESCUBIERTOS · {en posesión} / 10 UNIDADES`, filtra las 15 entidades en la pestaña **`POR RAREZAS`** (con siluetas negras `???` para las no descubiertas), revisa sus unidades actuales en **`EN TU BÓVEDA`** (`EN EQUIPO ACTIVO` o `EN BÓVEDA`) e inspecciona en el panel derecho el poder de cada compañero.

En los **puestos 1 a 3 del balcón**, el aviso es **`E · EQUIPAR`** (vacío) o **`E · EDITAR`** (ocupado). Pulsar **`E`** abre el selector (`PUESTO DE EQUIPO 1/2/3`) con los botones inferiores **`QUITAR DEL PUESTO`**, **`LIBERAR COPIA`** y **`CERRAR · E / ESC`**; pulsar **`T`** en el mundo frente a un puesto ocupado desequipa directamente la unidad sin abrir el menú. En los **puestos 4 a 10 de la planta inferior**, acercarse a un puesto ocupado muestra **`E · LIBERAR`** y pulsar **`E`** abre la confirmación **`LIBERAR UNIDAD`** (`CANCELAR` / `LIBERAR`).

![Selector de un puesto de equipo](../Media/Interfaz/Equipar_coleccionable.png)
*Selector de un puesto de equipo (`CompanionView`) en la versión definitiva de Unity con los tres botones inferiores integrados: `QUITAR DEL PUESTO`, `LIBERAR COPIA` y `CERRAR · E / ESC`.*

El portal conduce a la selección de ronda (`ELIGE LA RONDA`). Tras confirmar Demo (`3:00`) o Completa (`8:00`) se presenta la entrada del Coleccionista en la arena.

## Durante la expedición

La linterna ataca automáticamente y el HUD comunica salud, experiencia, nivel, bajas, dash, mejoras y retratos de los tres acompañantes equipados. Mantener pulsada la tecla **`Q`** (o **`Select`** en mando) despliega temporalmente las **estadísticas completas** y las **descripciones de los poderes de acompañantes** en tiempo real.

El minimapa descubre terreno al recorrerlo. Los fragmentos de entidades muestran un aviso y se recogen con **`E`** antes de que desaparezcan a los `10 s`. Si el jugador ya ha alcanzado el tope de **10 unidades** en su colección (`10/10`), el fragmento no puede recogerse y muestra el aviso flotante **`INVENTARIO LLENO / LIBERA ESPACIO`**.

Las subidas de nivel pausan el combate y presentan hasta tres cartas. La confirmación aplica la mejora y continúa, o abre la siguiente elección pendiente.

Gólgora anuncia su entrada a los `3:00` (demo) u `8:00` (completa), traslada al jugador con protección a la caldera central de `22 m` y activa su cámara compartida. Al bajar al `50%` de vida, un rugido invulnerable (`1,5 s`) que empuja en `12 m` marca el inicio de la segunda fase.

## Volver a preparar y fusionar

Victoria y derrota muestran el resumen de la incursión, sincronizan las estadísticas y la puntuación con Firebase (`EventLeaderboardSync`) y ofrecen **`VOLVER A LA BÓVEDA`**. La colección obtenida se conserva y las cartas de combate se restablecen para el siguiente intento.

En la máquina de fusión central (`E · FUSIONAR` / `E · VER FUSIÓN`), el jugador combina **4 copias de la misma rareza** (incluidas copias equipadas en el balcón, etiquetadas como **`EN EQUIPO 1/2/3`**, que se desequipan automáticamente al confirmar). El proceso dura **30 segundos (`00:30`)** fijos para cualquier rareza y al terminar ofrece el botón único **`COLECCIONAR`** (si la colección está en `10/10`, advierte **`INVENTARIO LLENO · LIBERA ESPACIO`** y conserva el resultado hasta que el jugador libere una unidad en la bóveda).

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
