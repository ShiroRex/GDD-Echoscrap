# Bugs y seguimiento

## Suite completa pendiente de cierre (`QA-FULL-01`)

La ejecución completa anterior registró 45 fallos; las 172 pruebas focalizadas aprobadas sobre `b543684` no sustituyen esa validación. Los grupos afectados incluyen carga de escenas sandbox/laboratorio que están deshabilitadas en el perfil de entrega, expectativas antiguas de balance y posición de la máquina de fusión, capturas con `WaitForEndOfFrame` incompatibles con batch mode y comprobaciones de terreno, árboles y paredes. Estos últimos fallos deben investigarse antes de concluir que son únicamente pruebas obsoletas. Los resultados actuales se registran en [[02 - Pruebas]].

La integración en `develop` y la apertura de PR a `main` acreditan la sincronización del repositorio, no la ausencia de defectos ni la aprobación de toda la suite.

## Señalización y guía de la bóveda (`UX-01` — Resuelto en `9eed59f`)

**Estado: resuelto e integrado (`develop` @ `9eed59f`).** En revisiones anteriores, los tres puestos de equipo y la gestión de la colección carecían de indicadores globales dentro de la bóveda. En el código actual se ha resuelto mediante cuatro elementos integrados:

1. **Rótulos 3D sobre los pedestales del balcón superior:** `EQUIPO 1`, `EQUIPO 2` y `EQUIPO 3` (`CollectibleDisplay.MarkEquipmentSlot`).
2. **Indicadores direccionales superiores (`VaultDirections` en `GameMenuView.cs`):** dos tarjetas dinámicas con flechas (`▲`, `▼`, `◄`, `►`, `●`) que guían en tiempo real hacia la **Máquina de Fusión** y hacia el **Balcón de Equipo** (puestos 1, 2 y 3).
3. **Panel rotatorio de consejos (`VaultTips` en `GameMenuView.cs`):** tarjeta inferior izquierda con etiqueta `CONSEJO` que rota cada `8 s` entre tres indicaciones sobre equipo, fusión y liberación (y prioriza *"Pulsa E frente a un puesto para equipar tu primer coleccionable"* cuando `OwnedCount > 0 && !hasEquippedCompanion`).
4. **Índice de Colección (`VaultCollectionIndex.cs`, tecla `[I]`):** accesible desde el botón superior izquierdo `ÍNDICE · [I]`, con contador `{descubiertos} / 15 DESCUBIERTOS · {en posesión} / 18 UNIDADES`, pestañas `POR RAREZAS` y `EN TU BÓVEDA`, e inspector detallado de poderes.

## Acceso con mando en interacciones de colección (`INPUT-01` — Resuelto en `b381ef4` / `734e2a5`)

**Estado: resuelto e integrado (`8047d90` / `b543684`).** Además del combate (`A` / `Cruz` para dash), pausa (`Start`), consulta rápida (`Select`) y elección de cartas, la recogida manual de fragmentos en la arena (`CompanionSystem.TryCollectNearest` / `CollectibleDisplay`), la estación de fusión (`FusionStation`), los selectores de puesto (`CompanionSystem`) y la liberación (`VaultReleaseInteraction`) responden al botón oeste del mando (`X` / `Cuadrado`, equivalente a `E`), el desequipado rápido responde al botón norte (`Y` / `Triángulo`, equivalente a `T`), el cierre/cancelación usa el botón este (`B` / `Círculo`) y el Índice de Colección (`VaultCollectionIndex`) se alterna con `Select`.

## Incidencias corregidas en `0.7.1` (`7a237e5..b543684`)

- **Ventana forzada por registro heredado en Windows:** solucionado añadiendo el selector **`Pantalla` (`COMPLETA` / `VENTANA`)** en `MenuSettingsView` (`EchoScrap.Graphics.FullScreen`, valor por defecto `1`) y reaplicando `FullScreenWindow` a resolución nativa tras cargar escena en Standalone (`AfterSceneLoad` + segundo fotograma).
- **Enemigos blanquecinos por fuga de `_EmissionColor` o saturación por DoT continuo:** `MothSpriteView`, `MortarSpriteView` y `EnemySpriteView` limpian `_EmissionColor` y el `MaterialPropertyBlock` al terminar el destello y al reciclar desde el pool, y `EnemyActor.ReceiveDamage` excluye `StaticAura`, `PlasmaPuddle` y `FlameTrail` del destello blanco de impacto directo.
- **Recorte de *Aura estática* y zonas de suelo en pendientes:** `UpgradeEffectPool.ProjectGroundRing` proyecta los anillos de suelo sobre `ArenaSurface.Height(...) + 0,06 m` con malla preasignada `12 × 12` (`0` bytes de basura en GC).

## Comportamiento de límites de población, oleadas y alcance técnico

- **Población fuera de cámara:** el límite de `150` enemigos ordinarios contabiliza también a los activos fuera del encuadre; una retirada sostenida en línea recta puede concentrar parte de la población detrás del jugador hasta que se reciclen o sean eliminados.
- **Recorte de la ronda demo:** la ronda demo (`3:00`) emplea los tres primeros tramos de la tabla de oleadas (`0:00–3:00`) antes de invocar a Gólgora (`Golgora_Demo.asset`, `3 600 HP`, mismo valor base en `EnemyArchetypeSettings`), mientras que la ronda completa (`8:00`) recorre los cinco tramos completos hasta el encuentro de `9 000 HP` (`Golgora_Complete.asset`).
- **Validación técnica multiplataforma y limitación en hardware mínimo (`TECH-01` / `0.7.1`):** sobre las compilaciones Release vigentes (**Windows x64** y **macOS Universal**), las optimizaciones de GPU instancing, presupuesto de navegación en `EnemyObstacleNavigator`, `WarehouseLightScope`, LODs/texturas, deslizamiento en rocas bajas, pooling sin GC de habilidades y los selectores **`Límite de FPS` (`30 / 60 / 120`)** y **`Pantalla` (`COMPLETA` / `VENTANA`)** en `MenuSettingsView` amplían la estabilidad y compatibilidad de pantalla. Sigue acotada como limitación conocida la ausencia de telemetría instrumentada en un equipo físico de gama mínima (`8 GB RAM` / `4 GB VRAM`), mitigada mediante `Efectos decorativos`, `Efectos reducidos` y el tope de FPS.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
