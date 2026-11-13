# Bugs y seguimiento

## Señalización y guía de la bóveda (`UX-01` — Resuelto en `9eed59f`)

**Estado: resuelto e integrado (`develop` @ `9eed59f`).** En revisiones anteriores, los tres puestos de equipo y la gestión de la colección carecían de indicadores globales dentro de la bóveda. En el código actual se ha resuelto mediante cuatro elementos integrados:

1. **Rótulos 3D sobre los pedestales del balcón superior:** `EQUIPO 1`, `EQUIPO 2` y `EQUIPO 3` (`CollectibleDisplay.MarkEquipmentSlot`).
2. **Indicadores direccionales superiores (`VaultDirections` en `GameMenuView.cs`):** dos tarjetas dinámicas con flechas (`▲`, `▼`, `◄`, `►`, `●`) que guían en tiempo real hacia la **Máquina de Fusión** y hacia el **Balcón de Equipo** (puestos 1, 2 y 3).
3. **Panel rotatorio de consejos (`VaultTips` en `GameMenuView.cs`):** tarjeta inferior izquierda con etiqueta `CONSEJO` que rota cada `8 s` entre tres indicaciones sobre equipo, fusión y liberación (y prioriza *"Pulsa E frente a un puesto para equipar tu primer coleccionable"* cuando `OwnedCount > 0 && !HasEquippedOnce`).
4. **Índice de Colección (`VaultCollectionIndex.cs`, tecla `[I]`):** accesible desde el botón superior izquierdo `ÍNDICE · [I]`, con contador `{descubiertos} / 15 DESCUBIERTOS · {en posesión} / 10 UNIDADES`, pestañas `POR RAREZAS` y `EN TU BÓVEDA`, e inspector detallado de poderes.

## Soporte integral de mando (`INPUT-01` — Pospuesto a futuro / fuera de alcance v1.0)

**Estado: experimental en código, no garantizado ni validado en v1.0 (`pensado a futuro`).** La versión actual (`1.0`) utiliza **teclado y ratón** como único esquema de control oficial y probado. Aunque `PlayerInputReader` y varios scripts de interacción (`CollectibleShard`, `FusionStation`, `CompanionSystem`, `VaultReleaseInteraction`, `VaultCollectionIndex` y `CombatHud`) incorporan enlaces preliminares de `Input System` (`buttonWest`, `buttonNorth`, `selectButton`, `A`/`Cruz`, `Start`), el soporte completo de controlador (incluida navegación integral de menús y certificación sobre hardware) no forma parte de los compromisos de esta entrega y queda planificado para una iteración posterior.

## Comportamiento de límites de población y curva de oleadas

- **Población fuera de cámara:** el límite de `150` enemigos ordinarios contabiliza también a los activos fuera del encuadre; una retirada sostenida en línea recta puede concentrar parte de la población detrás del jugador hasta que se reciclen o sean eliminados.
- **Recorte de la ronda demo:** la ronda demo (`3:00`) emplea los tres primeros tramos de la tabla de oleadas (`0:00–3:00`) antes de invocar a Gólgora (`3 600 HP`), mientras que la ronda completa (`8:00`) recorre los cinco tramos completos hasta el encuentro de `9 000 HP` (`15 000 HP` base en código).

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
