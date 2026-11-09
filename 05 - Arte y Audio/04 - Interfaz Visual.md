# Interfaz visual

## Tipografía unificada (`Pixelify Sans`)

Toda la interfaz del juego utiliza de manera unificada la familia tipográfica **`Pixelify Sans`** (`PixelifySans-Regular.ttf` y `PixelifySans-SemiBold.ttf`, configurada en `Typography.asset`, `UpgradeCardSkin.asset` y los generadores de interfaz en código), abarcando menús, HUD de combate, cartas de mejora, paneles de la bóveda, Índice de Colección y números flotantes de daño.

## Menú inicial y selección de ronda

El menú inicial utiliza un portal animado, entidades de la colección en órbita, campo de texto **`NOMBRE DEL JUGADOR (RANKING)...`** (máximo 20 caracteres, sincronizado con `EventLeaderboardSync`) y botones para Continuar, Nueva colección, Configuración y Salir. La transición al mundo presenta una pantalla de carga con animación del Coleccionista.

![Menú inicial](../Media/Avance_2026-11-04/Interfaz/MenuInicial.png)

La selección de ronda (`ELIGE LA RONDA`) permite editar también el nombre del jugador para el ranking y elegir entre **Ronda demo** (`3:00`), **Ronda completa** (`8:00`) o cancelar la salida. Su confirmación reproduce una animación de entrada y un rugido antes del despliegue en la arena.

![Selección de ronda](../Media/Avance_2026-11-04/Interfaz/SeleccionRonda.png)

## Interfaz de la bóveda, Índice de Colección y fusión

Dentro de la bóveda, el HUD y el escenario proporcionan guía visual constante:

- **Indicadores direccionales superiores (`VaultDirections`):** dos tarjetas en la franja superior con flechas dinámicas (`▲`, `▼`, `◄`, `►`, `●`) que apuntan hacia la **Máquina de Fusión** (*"MÁQUINA DE FUSIÓN · Combina 4 copias de la misma rareza"*, *"BAJA A FUSIÓN"* o *"E · MÁQUINA DE FUSIÓN"*) y hacia los **Puestos de Equipo** (*"SUBE AL BALCÓN · Puestos de equipo 1, 2 y 3"*, *"EQUIPAR ENTIDADES"* o *"PUESTO 1..3 · EQUIPAR/EDITAR"*).
- **Rótulos 3D y avisos de pedestal:** los tres puestos del balcón muestran en el mundo **`EQUIPO 1`**, **`EQUIPO 2`** y **`EQUIPO 3`** con aviso **`E · EQUIPAR`** o **`E · EDITAR`** (y desequipado directo con **`T`**), mientras que los puestos ocupados 4 a 10 de la planta baja muestran **`E · LIBERAR`** (que abre el modal **`LIBERAR UNIDAD`** con botones `CANCELAR` y `LIBERAR`).
- **Panel de consejos (`VaultTips`):** tarjeta en la esquina inferior izquierda con la etiqueta **`CONSEJO`** que rota automáticamente cada `8 s` entre tres indicaciones (o prioriza *"Pulsa E frente a un puesto para equipar tu primer coleccionable"* cuando el jugador tiene copias pero aún no ha equipado ninguna).
- **Índice de Colección (`VaultCollectionIndex.cs`, tecla `[I]`):** accesible mediante `I` o el botón superior izquierdo **`ÍNDICE · [I]`**. Presenta el encabezado **`ÍNDICE DE COLECCIÓN`** con el contador **`{descubiertos} / 15 DESCUBIERTOS · {en posesión} / 10 UNIDADES`**, dos pestañas (**`POR RAREZAS`**, con filtros por rareza y siluetas negras **`???`** / `NO ENCONTRADO` / `SILUETA BLOQUEADA` para las no descubiertas, y **`EN TU BÓVEDA`**, que distingue `EN EQUIPO ACTIVO` y `EN BÓVEDA`), un inspector lateral derecho con el poder de compañero y el botón **`CERRAR · [I]`**.
- **Selector de acompañantes (`CompanionView`):** al pulsar `E` en los puestos 1 a 3 del balcón se abre `PUESTO DE EQUIPO 1/2/3 · ELIGE UNA ENTIDAD DESCUBIERTA PARA ESTE PUESTO`, con retratos enmarcados por rareza y barra inferior con tres botones: **`QUITAR DEL PUESTO`**, **`LIBERAR COPIA`** y **`CERRAR · E / ESC`**.
- **Estación de fusión (`FusionStationUI`):** ventana principal (`Marco_Fusion`) con 4 ranuras en cruz (`Cuadro_Fusion` + `Cruz`), botón superior derecho **`X`**, probabilidades redondeadas a enteros (`60%`, `40%`, `1%`), botones **`VACIAR`** y **`CONFIRMAR`**, selector de copias (`MiGaraje`, donde las copias equipadas muestran **`EN EQUIPO 1/2/3`** y botones **`QUITAR INGREDIENTE`** y **`VOLVER`**), cuenta atrás fija de **`00:30`** y pantalla de resultado con un único botón de acción **`COLECCIONAR`** (o aviso **`INVENTARIO LLENO · LIBERA ESPACIO`** si ya se tienen `10/10` unidades).

![Preparación de entidades](../Media/Avance_2026-11-05/Interfaz/SeleccionEntidades_Actual.png)
*Nota sobre la captura histórica: la imagen muestra el selector de puesto en la iteración del 5 de noviembre; en `9eed59f` la barra inferior integra los tres botones `QUITAR DEL PUESTO`, `LIBERAR COPIA` y `CERRAR · E / ESC`.*

## HUD de combate, consulta rápida (`Q` / `Select`) y cartas

El HUD de combate muestra barra de salud, barra de XP y nivel, reloj de incursión, contador de bajas, estado del dash, iconos de mejoras adquiridas, los tres retratos de acompañantes equipados (con sus líneas descriptivas de poder activo si `Atributos de coleccionables` está en `SÍ`) y un minimapa circular con niebla de exploración. Gólgora dispone de su propia barra superior de salud e indicador direccional fuera de pantalla.

Mantener pulsada la tecla **`Q`** en teclado o el botón **`Select`** en mando (`CombatHud.IsHudDetailHeld`) despliega temporalmente las **estadísticas completas** (`COMPLETAS`) y las **descripciones de los poderes de los acompañantes equipados**, aunque estén reducidas u ocultas en la configuración.

Las cartas de subida de nivel presentan marcos por rareza, icono, nombre, nivel actual/máximo, tipo de daño y progresión de valores, además del botón de reintento cuando se dispone de *Sintonizador*.

![Cartas: captura de Unity del 27 de octubre](../Media/Avance_2026-10-27/Interfaz/Cartas.gif)

## Menú de Pausa en 3 pestañas y resultados

Al pulsar **`Esc`** (o `Start` en mando) durante la partida o en la bóveda se abre el menú **`PAUSA`** (`CombatHud.cs:631-756`), estructurado en **3 pestañas** más los botones inferiores de acción:

1. **`General`:** deslizadores **`Movimiento de cámara`** (`0%` a `100%`, `ScreenShakeScale`) y **`Efectos decorativos`** (`0%` a `100%`, `VfxDensityScale`, que activa `ReducedEffects` por debajo de `99,9%`).
2. **`Interfaz`:** selector **`Estadísticas generales`** (**`COMPLETAS`**, **`REDUCIDAS`**, **`OCULTAS`**) y selector **`Atributos de coleccionables`** (**`SÍ`** / **`NO`**, `ShowCompanionPowers`).
3. **`Sonido`:** deslizadores **`Volumen general`** (`0%` a `100%`, guardado en `EchoScrap.Audio.MasterVolume`) y **`Música`** (`0%` a `100%`, guardado en `EchoScrap.Audio.MusicVolume`).

Los botones inferiores permiten **`CONTINUAR`** y **`VOLVER AL MENÚ`** (desde la bóveda) o **`VOLVER A LA BÓVEDA`** (durante una incursión en la arena). *(En la escena independiente `MainMenu`, el panel `CONFIGURACIÓN` de `MenuSettingsView.cs` ofrece los deslizadores de volumen, la casilla de efectos reducidos y los botones `COMPLETA`, `REDUCIDA` y `SIN`.)*

La pantalla de resultados muestra victoria o derrota, tiempo sobrevivido, nivel, bajas y XP, sincroniza la sesión con el ranking de Firebase (`EventLeaderboardSync`) y ofrece el botón **`VOLVER A LA BÓVEDA`**.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
