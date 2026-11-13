# Bugs y seguimiento

## Señalización y guía de la bóveda (`UX-01` — Resuelto en `9eed59f`)

**Estado: resuelto e integrado (`develop` @ `9eed59f`).** En revisiones anteriores, los tres puestos de equipo y la gestión de la colección carecían de indicadores globales dentro de la bóveda. En el código actual se ha resuelto mediante cuatro elementos integrados:

1. **Rótulos 3D sobre los pedestales del balcón superior:** `EQUIPO 1`, `EQUIPO 2` y `EQUIPO 3` (`CollectibleDisplay.MarkEquipmentSlot`).
2. **Indicadores direccionales superiores (`VaultDirections` en `GameMenuView.cs`):** dos tarjetas dinámicas con flechas (`▲`, `▼`, `◄`, `►`, `●`) que guían en tiempo real hacia la **Máquina de Fusión** y hacia el **Balcón de Equipo** (puestos 1, 2 y 3).
3. **Panel rotatorio de consejos (`VaultTips` en `GameMenuView.cs`):** tarjeta inferior izquierda con etiqueta `CONSEJO` que rota cada `8 s` entre tres indicaciones sobre equipo, fusión y liberación (y prioriza *"Pulsa E frente a un puesto para equipar tu primer coleccionable"* cuando `OwnedCount > 0 && !HasEquippedOnce`).
4. **Índice de Colección (`VaultCollectionIndex.cs`, tecla `[I]`):** accesible desde el botón superior izquierdo `ÍNDICE · [I]`, con contador `{descubiertos} / 15 DESCUBIERTOS · {en posesión} / 10 UNIDADES`, pestañas `POR RAREZAS` y `EN TU BÓVEDA`, e inspector detallado de poderes.

## Acceso con mando en interacciones de colección (`INPUT-01` — Resuelto en `b381ef4`)

**Estado: resuelto e integrado (`main` @ `b381ef4` / `develop` @ `977bac5`).** Además del combate (`A` / `Cruz` para dash), pausa (`Start`), consulta rápida (`Select`) y elección de cartas, la recogida manual de fragmentos en la arena (`CollectibleShard`), la estación de fusión (`FusionStation`), los selectores de puesto (`CompanionSystem`) y la liberación (`VaultReleaseInteraction`) responden al botón oeste del mando (`X` / `Cuadrado`, equivalente a `E`), el desequipado rápido responde al botón norte (`Y` / `Triángulo`, equivalente a `T`), el cierre/cancelación usa el botón este (`B` / `Círculo`) y el Índice de Colección (`VaultCollectionIndex`) se alterna con `Select`.

## Comportamiento de límites de población, oleadas y alcance técnico

- **Población fuera de cámara:** el límite de `150` enemigos ordinarios contabiliza también a los activos fuera del encuadre; una retirada sostenida en línea recta puede concentrar parte de la población detrás del jugador hasta que se reciclen o sean eliminados.
- **Recorte de la ronda demo:** la ronda demo (`3:00`) emplea los tres primeros tramos de la tabla de oleadas (`0:00–3:00`) antes de invocar a Gólgora (`3 600 HP`), mientras que la ronda completa (`8:00`) recorre los cinco tramos completos hasta el encuentro de `9 000 HP` (`15 000 HP` base en código).
- **Cierre de validación técnica y limitación en hardware mínimo (`TECH-01`):** la compilación Windows x64 (`Windows.asset` Release, `0` errores, `~0,94 GiB`), las optimizaciones integradas de memoria/horda (`feature/perfil-memoria-baja` y `bugfix/tirones-horda-windows`) y el perfilado de `2 000` fotogramas (`p95` de CPU = `6,61 ms`) cumplen el presupuesto de `60 FPS` en equipo de desarrollo/referencia. Queda acotada como limitación conocida la ausencia de telemetría instrumentada en un equipo físico de gama mínima (`8 GB RAM` / `4 GB VRAM`), mitigada en el juego mediante los deslizadores de `Efectos decorativos` y `Efectos reducidos` en `MenuSettingsView`.
- **Arte conceptual de Gólgora:** el diseño del jefe final se concretó directamente mediante modelado 3D y capturas de animación en Unity ([`02 - Personajes y Animación`](../05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#g%C3%B3lgora-matacoleccionista)), sin hoja de boceto conceptual a lápiz previa.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
