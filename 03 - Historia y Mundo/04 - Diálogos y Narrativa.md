# Diálogos y narrativa

El juego comunica su situación mediante nombres, entorno, presentaciones diegéticas y mensajes directos en la interfaz, todos ellos compuestos con la tipografía **`Pixelify Sans`**.

## Portada, ranking y selección de ronda

En la portada integrada y en el selector de ronda (`ELIGE LA RONDA`), el jugador dispone del campo **`NOMBRE DEL JUGADOR (RANKING)...`** (máximo 20 caracteres) para identificar sus marcas en el ranking del evento (`EventLeaderboardSync`), además de las opciones para continuar o iniciar una nueva colección y elegir entre **Ronda demo** y **Ronda completa**.

## Señalización, índice y mensajes en la bóveda

Dentro de la bóveda, la interfaz y el escenario guían cada paso de la preparación:

- **Rótulos 3D en el mundo:** **`EQUIPO 1`**, **`EQUIPO 2`** y **`EQUIPO 3`** sobre los tres pedestales del balcón superior.
- **Indicadores direccionales superiores (`VaultDirections`):** dos tarjetas en el borde superior con flechas dinámicas (`▲`, `▼`, `◄`, `►`, `●`) que orientan hacia la **Máquina de Fusión** (*"MÁQUINA DE FUSIÓN · Combina 4 copias de la misma rareza"*, *"BAJA A FUSIÓN"* o *"E · MÁQUINA DE FUSIÓN"*) y hacia el **Balcón de Equipo** (*"SUBE AL BALCÓN · Puestos de equipo 1, 2 y 3"*, *"EQUIPAR ENTIDADES"* o *"PUESTO 1..3 · EQUIPAR/EDITAR"*).
- **Panel de consejos (`VaultTips`):** tarjeta inferior izquierda con la insignia **`CONSEJO`** que rota cada `8 s` entre tres mensajes:
  1. *"Sube por las rampas al balcón para equipar hasta 3 entidades en los puestos 1, 2 y 3."*
  2. *"Acércate a la máquina central y pulsa E para fusionar 4 copias de la misma rareza."*
  3. *"En los puestos ocupados de la planta baja puedes pulsar E para liberar espacio."*
  Cuando el jugador posee coleccionables pero aún no ha equipado ninguno (`OwnedCount > 0 && !HasEquippedOnce`), prioriza el mensaje: *"Pulsa E frente a un puesto para equipar tu primer coleccionable."*
- **Interacciones de pedestal y liberación:** en el balcón se muestra **`E · EQUIPAR`** o **`E · EDITAR`** (abriendo `PUESTO DE EQUIPO 1/2/3 · ELIGE UNA ENTIDAD DESCUBIERTA PARA ESTE PUESTO`, con botones **`QUITAR DEL PUESTO`**, **`LIBERAR COPIA`** y **`CERRAR · E / ESC`**). En los puestos 4 a 10 de la planta baja aparece **`E · LIBERAR`**, que abre el modal **`LIBERAR UNIDAD`**: *"¿Seguro que quieres liberar a `<Nombre>`? Esta unidad desaparecerá de tu colección."* (`CANCELAR` / `LIBERAR`).
- **Índice de Colección (`[I]`):** accesible desde el botón **`ÍNDICE · [I]`**, presenta el título **`ÍNDICE DE COLECCIÓN`**, el contador **`{descubiertos} / 15 DESCUBIERTOS · {en posesión} / 10 UNIDADES`** y las pestañas **`POR RAREZAS`** (donde las entidades aún no registradas aparecen como **`???`**, **`NO ENCONTRADO`** y **`SILUETA BLOQUEADA`**) y **`EN TU BÓVEDA`** (`EN EQUIPO ACTIVO` / `EN BÓVEDA`).
- **Máquina de fusión:** muestra **`E · FUSIONAR`** o **`E · VER FUSIÓN`**, etiqueta las copias equipadas como **`EN EQUIPO 1/2/3`**, presenta el resultado con el botón **`COLECCIONAR`** y, si la colección está llena (`10/10`), advierte **`INVENTARIO LLENO · LIBERA ESPACIO`** (*"Tu colección tiene 10 unidades. Libera una en la bóveda y vuelve: el resultado de esta fusión seguirá aquí."*).

## Mensajes en la arena y encuentro final

La entrada a la arena presenta al Coleccionista y el objetivo de la incursión. Las oleadas de fragmentos anuncian **«¡NUEVOS COLECCIONABLES!»** y, si el jugador intenta recoger uno teniendo `10/10` unidades en la colección, aparece el texto flotante **`INVENTARIO LLENO / LIBERA ESPACIO`**.

Gólgora se anuncia con título y rugido al abrirse la caldera. Su barra de salud, los avisos de suelo y los textos de estado (*«Resistido»*, *«Vulnerable»*, *«EJECUTADO»*) acompañan los cambios del combate. **«VICTORIA»** o **«DERROTA»** cierran la incursión y **«VOLVER A LA BÓVEDA»** conduce a la siguiente preparación.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
