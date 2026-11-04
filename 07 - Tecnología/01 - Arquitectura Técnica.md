# Arquitectura técnica

El proyecto activo es EchoScrap/. La compilación principal empieza en MainMenu y carga CombatArena después de elegir colección. El mundo conserva sus sistemas para preparar y reiniciar expediciones sin recargar a todos los actores.

| Responsabilidad | Componentes actuales |
| --- | --- |
| Menú y escenas | MainMenuEntry, MainMenuController, GameMenuView, SceneTransitionState, LoadingScreen |
| Preparación y expedición | GameFlowController, ExpeditionEntrySettings, ExpeditionIntro |
| Composición del mundo | CombatGameBootstrap |
| Tiempo, pausas y resultado | CombatSession, GameplayClock, CombatOutcomeResolver |
| Movimiento y salud | PlayerInputReader, PlayerMotor, TerrainPlayerCollision, PlayerHealth, PlayerRuntimeStats |
| Linterna | FlashlightWeapon, FlashlightPresentation, FlashlightAudio |
| Horda y élites | EnemyActor, EnemyPool, EnemyObstacleNavigator, EliteEnemyController, EliteProjectilePool |
| Jefe | CombatWaveDirector, BossEncounter, BossArena, GolgoraActor, GolgoraCombatSettings |
| Progresión | CombatProgression, UpgradeDraftController, UpgradeCatalog, UpgradeCombatRuntime |
| Bóveda y colección | WarehouseTravel, WarehouseCameraDirector, WarehouseCameraFraming, FusionStation, FusionProgress |
| Equipo y obtención | CompanionSystem, CompanionView, CollectibleUnitGrid |
| Arte y efectos | EnemySpriteView, MortarSpriteView, MothSpriteView, EnemyEffectPool, UpgradeEffectPool |
| Interfaz y audio | CombatHud, CombatMinimap, PlayerStatsReadout, DamageNumberPresenter, CombatMusicPlayer |
| Iluminación | ArenaLightingController, ArenaLightingProfile y renderer de atmósfera |

La simulación comunica daño, muerte y resultados mediante eventos. Las reservas reutilizan actores, objetos y efectos. Las reglas del combate y su presentación mantienen responsabilidades separadas; las capturas controladas no cambian los assets originales del proyecto.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
