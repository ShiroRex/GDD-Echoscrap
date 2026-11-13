# Diálogos y narrativa

El juego comunica su situación mediante nombres, entorno, presentaciones diegéticas y mensajes directos en la interfaz, todos ellos compuestos con la tipografía **`Pixelify Sans`**.

## Portada, ranking y selección de ronda

En la portada integrada al pulsar **Nueva colección** (`¿NUEVA COLECCIÓN?`), el jugador dispone del campo **`NOMBRE DEL JUGADOR (RANKING)...`** (máximo 20 caracteres visibles) para registrar un alias único en el ranking del evento (`GamertagRegistry` / `EventLeaderboardSync`). Al acercarse al portal de salida en la bóveda, la ventana **`ELIGE LA RONDA`** (*"Elige una ronda para comenzar."*) permite escoger entre **`RONDA DEMO`** (`3:00`), **`RONDA COMPLETA`** (`8:00`) o **`SALIR`**. Su confirmación reproduce una transición con rugido antes del despliegue en la arena.

## Señalización, índice y mensajes en la bóveda

Dentro de la bóveda, la interfaz y el escenario guían cada paso de la preparación:

- **Rótulos 3D en el mundo:** **`EQUIPO 1`**, **`EQUIPO 2`** y **`EQUIPO 3`** sobre los tres pedestales del balcón superior (añadiendo `· RESERVA` cuando muestran una copia en reserva mientras están vacíos).
- **Indicadores direccionales superiores (`VaultDirections`):** dos tarjetas en el borde superior con flechas dinámicas (`▲`, `▼`, `◄`, `►`, `●`) que orientan hacia la **Máquina de Fusión** (*"MÁQUINA DE FUSIÓN · Combina 4 copias de varias rarezas"*, *"BAJA A FUSIÓN"* o *"E · MÁQUINA DE FUSIÓN"*) y hacia el **Balcón de Equipo** (*"SUBE AL BALCÓN · Puestos de equipo 1, 2 y 3"*, *"EQUIPAR ENTIDADES"* o *"PUESTO 1..3 · EQUIPAR/EDITAR"*).
- **Panel de consejos (`VaultTips`):** tarjeta inferior izquierda con la insignia **`CONSEJO`** que rota cada `8 s` entre tres mensajes:
  1. *"Equipa coleccionables antes de ir a la arena."*
  2. *"Abre el índice para consultar tus descubrimientos."*
  3. *"Combina cuatro coleccionables en la máquina de fusión."*
  Cuando el jugador posee coleccionables pero aún no ha equipado ninguno (`OwnedCount > 0 && !HasEquippedOnce`), prioriza el mensaje: *"Pulsa E frente a un puesto para equipar tu primer coleccionable."*
- **Interacciones de pedestal y liberación:** en el balcón se muestra **`E · EQUIPAR`**, **`E · LIBERAR`** (si muestra una copia de reserva en un puesto de equipo vacío) o **`E · EDITAR`** (abriendo `PUESTO DE EQUIPO 1/2/3 · ELIGE UNA ENTIDAD DESCUBIERTA PARA ESTE PUESTO` con la nota *"Señala un compañero para ver su poder. Los poderes se acumulan por copia equipada."* y los botones **`QUITAR DEL PUESTO`**, **`LIBERAR COPIA`** y **`CERRAR · E / ESC`**). En los puestos ocupados de la planta baja aparece **`E · LIBERAR`**, que abre el modal **`LIBERAR UNIDAD`**: *"¿Seguro que quieres liberar a `<Nombre>`? Esta unidad desaparecerá de tu colección."* (`CANCELAR` / `LIBERAR`).
- **Índice de Colección (`[I]`):** accesible desde el botón **`ÍNDICE · [I]`**, presenta el título **`ÍNDICE DE COLECCIÓN`**, el contador **`{descubiertos} / 15 DESCUBIERTOS · {en posesión} / 10 UNIDADES`** y las pestañas **`POR RAREZAS`** (donde las entidades aún no registradas aparecen como **`???`**, **`NO ENCONTRADO`** y **`SILUETA BLOQUEADA`**) y **`EN TU BÓVEDA`** (`EN EQUIPO ACTIVO` / `EN BÓVEDA`).
- **Máquina de fusión:** muestra **`E · FUSIONAR`** o **`E · VER FUSIÓN`**, etiqueta las copias equipadas como **`EN EQUIPO 1/2/3`**, presenta el resultado con el botón **`COLECCIONAR`** y, si la colección está llena (`10/10`), advierte **`INVENTARIO LLENO · LIBERA ESPACIO`** (*"Tu colección tiene 10 unidades. Libera una en la bóveda y vuelve: el resultado de esta fusión seguirá aquí."*).

## Mensajes en la arena y encuentro final

La entrada a la arena presenta al Coleccionista y el objetivo de la incursión. Las oleadas de fragmentos anuncian **«¡NUEVOS COLECCIONABLES!»** y, si el jugador intenta recoger uno teniendo `10/10` unidades en la colección, aparece el texto flotante **`INVENTARIO LLENO / LIBERA ESPACIO`**.

Gólgora se anuncia con título y rugido al abrirse la caldera. Su barra de salud, los avisos de suelo y los textos de estado (*«Resistido»*, *«Vulnerable»*, *«EJECUTADO»*) acompañan los cambios del combate. **«VICTORIA»** o **«DERROTA»** cierran la incursión y **«VOLVER A LA BÓVEDA»** conduce a la siguiente preparación.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
