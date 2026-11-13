# Jugabilidad

## Preparación y salida

Menú inicial (con campo `NOMBRE DEL JUGADOR (RANKING)...`) → continuar o crear colección → bóveda → consultar el Índice de Colección (`[I]`), fusionar copias (`E`) y seleccionar entidades equipadas en el balcón → elegir ronda demo o completa → expedición → regreso a la bóveda.

Continuar recupera la colección guardada. Nueva colección reinicia el inventario, el equipo y la fusión en curso, con confirmación cuando ya existe una colección.

La bóveda aloja una colección con capacidad máxima estricta de **10 unidades en total** (`FusionProgress.Capacity = 10`, `displays[10]`). En la escena (`FusionStation.DisplaySlots`), el recinto dispone de **10 pedestales físicos** (**3 puestos de equipo activo** en el balcón superior, `slot 0..2`, y **7 pedestales de exposición/reserva** en la planta inferior, `slot 3..9`; cuando algún puesto de equipo está vacío, muestra temporalmente una unidad en reserva con la etiqueta `EQUIPO N · RESERVA`, exhibiendo las hasta 10 unidades simultáneamente en el mundo mientras también se consultan íntegras en la pestaña `EN TU BÓVEDA` del Índice `[I]`). La navegación dentro de la bóveda está guiada en tiempo real por tres elementos:

- **Rótulos 3D en el mundo:** `EQUIPO 1`, `EQUIPO 2` y `EQUIPO 3` sobre los tres pedestales del balcón superior.
- **Indicadores direccionales superiores (`VaultDirections`):** dos tarjetas dinámicas en la parte alta del HUD con flechas (`▲`, `▼`, `◄`, `►`, `●`) que apuntan hacia la **Máquina de Fusión** (*"MÁQUINA DE FUSIÓN · Combina 4 copias de varias rarezas"*, *"BAJA A FUSIÓN"*, *"EN FUSIÓN"* o *"E · MÁQUINA DE FUSIÓN"*) y hacia los **Puestos de Equipo** (*"SUBE AL BALCÓN · Puestos de equipo 1, 2 y 3"*, *"EQUIPAR ENTIDADES"* o *"PUESTO 1..3 · EQUIPAR/EDITAR"*).
- **Panel de consejos (`VaultTips`) e Índice (`[I]`):** en la esquina inferior izquierda, la tarjeta `CONSEJO` rota cada `8 s` entre tres indicaciones (*"Equipa coleccionables antes de ir a la arena."*, *"Abre el índice para consultar tus descubrimientos."* y *"Combina cuatro coleccionables en la máquina de fusión."*), o prioriza *"Pulsa E frente a un puesto para equipar tu primer coleccionable."* si el jugador tiene copias pero aún no ha equipado ninguna. En la esquina superior izquierda, el botón **`ÍNDICE · [I]`** (o la tecla `I`) abre el **Índice de Colección** con las 15 entidades (`POR RAREZAS` y `EN TU BÓVEDA`).

En los **puestos 1 a 3 del balcón**, el aviso contextual es **`E · EQUIPAR`** (si el puesto está vacío) o **`E · EDITAR`** (si está ocupado). Pulsar **`E`** abre siempre el selector de acompañantes (`PUESTO DE EQUIPO 1/2/3`), donde equipar varias copias de la misma entidad **acumula sus poderes proporcionalmente** (`×2`, `×3`), e incluye en su barra inferior los botones **`QUITAR DEL PUESTO`**, **`LIBERAR COPIA`** y **`CERRAR · E / ESC`**. Además, pulsar **`T`** frente a un puesto de equipo ocupado en el mundo retira directamente la entidad sin abrir el menú. En los **pedestales de exposición de la planta inferior**, acercarse a un pedestal ocupado muestra **`E · LIBERAR`** y pulsar **`E`** abre el diálogo de confirmación de liberación.

El portal o el botón **`INICIAR EXPEDICIÓN`** abre la elección de ronda (`ELIGE LA RONDA`). La ronda demo lleva a Gólgora a los `3:00`; la completa, a los `8:00`. Tras la selección se presenta la entrada del Coleccionista en la arena.

## Bucle de combate

1. Mantener una ruta de escape con movimiento y dash.
2. Dañar a la horda con la linterna y los poderes de la configuración.
3. Recoger XP y elegir una mejora al subir de nivel.
4. Acercarse a las entidades que aparecen y recogerlas con `E` antes de que desaparezcan (siempre que la colección tenga menos de `10/10` unidades; si está llena, aparece el aviso flotante `INVENTARIO LLENO / LIBERA ESPACIO`).
5. Consultar en cualquier momento las estadísticas completas y las descripciones de los poderes de acompañantes manteniendo pulsada la tecla **`Q`**.
6. Responder a las composiciones de horda y a los ataques de los dos élites.
7. Combatir contra Gólgora en la caldera central.
8. Consultar el resultado y volver a la bóveda para revisar equipo, liberar espacio o fusionar copias.

Las elecciones de cartas pausan la simulación del combate mediante la máscara `PauseReason.LevelUpChoice`. Si una recogida acumula varias subidas de nivel consecutivas, las elecciones se encadenan una tras otra sin reanudar la arena entre selecciones.

## Cierre y continuidad

La vida del jugador a cero produce derrota. La muerte de Gólgora con el jugador vivo produce victoria. Si ambos mueren en el mismo paso de simulación, prevalece la derrota. El resumen muestra tiempo, nivel, bajas y XP, sincroniza la puntuación con el ranking del evento (`EventLeaderboardSync`) y ofrece volver a la bóveda.

Las cartas, el nivel y la salud de la expedición se restablecen. Las copias ya recogidas, las entidades equipadas y el progreso de fusión pertenecen a la colección y se conservan. Abandonar desde el menú de pausa (`Esc` / `Start`) también devuelve a la bóveda.

## Bóveda y fusión

La máquina central de la bóveda se abre con **`E`** (`E · FUSIONAR` cuando está libre o en cuenta atrás, o `E · VER FUSIÓN` cuando tiene un resultado listo) y acepta **4 unidades cualesquiera, ya sean de la misma rareza o de rarezas mixtas** (excluida *Yanikua*), interpolando linealmente las probabilidades según la rareza media de los 4 ingredientes (`qualityMean = total / 4f`). Pueden seleccionarse tanto copias en reserva como copias actualmente equipadas en el balcón (identificadas con **`EN EQUIPO 1/2/3`**); al pulsar **`CONFIRMAR`**, el sistema desequipa automáticamente las copias necesarias y consume las 4 unidades.

Todas las fusiones tienen una **duración fija de 30 segundos (`00:30`)** en tiempo real (`Time.unscaledDeltaTime`), que continúa avanzando entre escenas. Al completarse la cuenta atrás, la pantalla de resultado revela la entidad obtenida y ofrece un único botón de acción: **`COLECCIONAR`** (además del botón `X` para cerrar la ventana sin reclamar aún). Si la colección está en `10/10` unidades al intentar reclamar, la interfaz muestra **`INVENTARIO LLENO · LIBERA ESPACIO`** y conserva el resultado en espera (`AwaitingDecision`) hasta que el jugador libere una unidad en los pedestales de la bóveda.

Durante una expedición sigue siendo posible visitar la bóveda por el acceso norte. Esa visita conserva la sesión en memoria y suspende su combate; al regresar se retoma. Volver desde un resultado inicia la preparación de una nueva expedición.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
