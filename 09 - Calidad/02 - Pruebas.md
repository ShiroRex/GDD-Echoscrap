# Pruebas

El proyecto dispone de pruebas automatizadas de locomoción, dash, daño, navegación, oleadas, élites, jefe, cartas, recogida, pausas, menús, bóveda, colección y fusión, además del laboratorio interactivo (`EnemyLaboratory`) para generar e inspeccionar los ocho arquetipos.

## Verificación de la revisión 0.6.2 (`develop` @ `9eed59f`, 8 de noviembre de 2026)

Esta revisión documental contrasta cada cifra, fórmula, tecla y etiqueta de interfaz directamente contra el código fuente y los assets de `EchoScrap` en el commit `9eed59f`:

- **Bóveda, colección e Índice (`FusionProgress.cs`, `CompanionSystem.cs`, `CompanionView.cs`, `VaultReleaseInteraction.cs`, `VaultCollectionIndex.cs`, `GameMenuView.cs`, `CollectibleDisplay.cs`):** verificada la capacidad máxima de `10` unidades (`Capacity = 10`, `displays[10]`), los avisos de inventario lleno en arena y fusión, la interacción en los puestos 1 a 3 (`E · EQUIPAR` / `E · EDITAR`, botones `QUITAR DEL PUESTO`, `LIBERAR COPIA`, `CERRAR · E / ESC` y tecla `T`) y en los puestos 4 a 10 (`E · LIBERAR`), la señalización (`EQUIPO 1..3`, `VaultDirections`, `VaultTips`) y el Índice de Colección (`[I]`, pestañas `POR RAREZAS` y `EN TU BÓVEDA`).
- **Máquina de fusión (`FusionProgress.cs`, `FusionStation.cs`, `FusionStationUI.cs`, `CollectibleUnitGrid.cs`):** verificada la duración fija de `30 s` (`FusionSeconds = 30`) para todas las recetas, la exclusión de Yanikua (`id != 4`), el uso permitido de copias equipadas (`EN EQUIPO 1/2/3`) con desequipado automático al confirmar, el redondeo a enteros en interfaz (`Mathf.RoundToInt`: `60%`, `40%`, `1%` para pesos `59,4%`, `39,6%`, `1%`) y el botón único `COLECCIONAR`.
- **Interfaz, pausa, tipografía y ranking (`CombatHud.cs`, `GameMenuView.cs`, `MenuSettingsView.cs`, `EventLeaderboardSync.cs`):** verificada la tipografía unificada `Pixelify Sans`, el menú de pausa en 3 pestañas (`General`, `Interfaz`, `Sonido`), la consulta rápida manteniendo `Q` o `Select` (`CombatHud.IsHudDetailHeld`) y la sincronización HTTPS `PATCH` con Firebase Realtime Database más respaldo JSON local.
- **Combate, mejoras y enemigos (`PlayerRuntimeStats.cs`, `PlayerHealth.cs`, `UpgradeCombatRuntime.cs`, `EnemyActor.cs`, `GolgoraActor.cs`, `GolgoraCombatSettings.cs`):** verificada la activación de `FLASH_03`, `BIO_02` y `CRIT_04` en cada pulso de `FLASH_05`, la conservación del bono de velocidad de `DASH_01` con `DASH_04` (`0,5 s`), los parámetros de `DEMO_03`, `DEMO_04`, `SCRAP_03` y `SCRAP_04`, las fórmulas y el orden de mitigación en 7 pasos del jugador, la Polilla Reactor (`1,6 m` de activación y `85%` de daño a otros enemigos), Gólgora (`0,6 s` de recuperación en *Bombardeo*, `40` de daño físico de aterrizaje y `18 s` fijos en *Cerco de magma*, `1,1 s` fijos en *Fisura de magma*) y la tabla de susceptibilidad al empuje.

## Verificaciones previas de pruebas automatizadas y capturas (4 y 5 de noviembre)

- **Edit Mode (`FusionRulesTests`):** 20 pruebas aprobadas sobre reglas de colección y fusión.
- **Play Mode (`EnemySpriteIntegrationTests`, `EliteEnemyTests`, `FlashlightBurstFeedbackTests`, `CompanionPowerTests`, `CompanionIntegrationTests`, `CompleteUpgradeTests`, `CombatArenaRunFlowTests`):** 169 casos aprobados cubriendo integración de sprites de élites, fases de apertura y vulnerabilidad, empuje continuo de *Rayo solar*, poderes de acompañantes, fórmula de *Ojo del verdugo*, catálogo de 35 mejoras y flujo de rondas y reinicios en `CombatArena`.
- **Condiciones de los medios documentales:** los GIFs de atlas se exportaron a escala `2×` desde los JSON de fotogramas, y las capturas en motor de Unity se registraron en sesiones aisladas con cámara de detalle, jugador protegido y oleadas detenidas, documentadas en [[06 - Registro visual]].

## Revisión de entrada pendiente

Con la señalización de la bóveda (`EQUIPO 1..3`, `VaultDirections`, `VaultTips`) y el Índice de Colección (`[I]`) ya integrados en `9eed59f`, la revisión pendiente de experiencia de entrada se centra en decidir y verificar las asignaciones equivalentes de mando para las interacciones que actualmente emplean las teclas `E`, `T` e `I`.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
