# Arquitectura técnica

El proyecto activo se encuentra en `EchoScrap/` (`develop` @ `a8a23c4`). La compilación principal empieza en `MainMenu` y carga `CombatArena` después de elegir colección, conservando en escena tanto la arena exterior como la bóveda modular para alternar preparación y expediciones sin recargar actores.

| Responsabilidad | Componentes actuales (`Assets/_Project/Scripts`) |
| --- | --- |
| Menú, señalización de bóveda y escenas | `MainMenuEntry`, `MainMenuController`, `MenuSettingsView`, `GameMenuView` (incluye `VaultDirections`, `VaultTips` y `VaultIndexHint`), `SceneTransitionState`, `LoadingScreen` |
| Preparación y expedición | `GameFlowController`, `ExpeditionEntrySettings`, `ExpeditionIntro` |
| Composición del mundo | `CombatGameBootstrap` |
| Tiempo, pausas y resultado | `CombatSession`, `GameplayClock`, `CombatOutcomeResolver` |
| Movimiento, salud y estadísticas | `PlayerInputReader`, `PlayerMotor`, `TerrainPlayerCollision`, `PlayerHealth` (mitigación en 7 pasos), `PlayerRuntimeStats` |
| Linterna | `FlashlightWeapon`, `FlashlightPresentation`, `FlashlightAudio` |
| Horda y élites | `EnemyActor`, `EnemyPool`, `EnemyObstacleNavigator`, `EliteEnemyController`, `EliteProjectilePool`, `MothExplosionPool` |
| Jefe | `CombatWaveDirector`, `BossEncounter`, `BossArena`, `GolgoraActor`, `GolgoraCombatSettings` |
| Progresión y mejoras | `CombatProgression`, `UpgradeDraftController`, `UpgradeCatalog`, `UpgradeCombatRuntime`, `UpgradeTuning` |
| Bóveda, colección, índice y fusión | `WarehouseTravel`, `WarehouseCameraDirector`, `WarehouseCameraFraming`, `FusionStation`, `FusionStationUI`, `FusionProgress` (`Capacity = 10`, `FusionSeconds = 30`), `VaultCollectionIndex` (`[I]`), `VaultReleaseInteraction`, `CollectibleDisplay` |
| Equipo y obtención | `CompanionSystem`, `CompanionPowerRuntime`, `CompanionPixelEffects`, `CompanionView`, `CollectibleUnitGrid`, `CollectibleShard`, `CollectiblePickupFeedback` |
| Arte y efectos | `EnemySpriteView`, `MortarSpriteView`, `MothSpriteView`, `EnemyEffectPool`, `UpgradeEffectPool` |
| Interfaz, pausa y audio | `CombatHud` (menú de pausa en 3 pestañas y consulta rápida `Q` / `Select`), `CombatMinimap`, `PlayerStatsReadout`, `DamageNumberPresenter`, `CombatMusicPlayer`, `ArenaMusicController` |
| Ranking en vivo y visor web | `EventLeaderboardSync` (`rulesVersion = 2`, sincronización HTTPS con Firebase Realtime Database), `GamertagRegistry` (reserva única SHA-256) y visor web en vivo `Tools/WebRanking/index.html` ([`https://echoscrap.web.app`](https://echoscrap.web.app)) |
| Iluminación | `ArenaLightingController`, `ArenaLightingProfile` y renderer de atmósfera |

La simulación comunica daño, muerte, recogida y resultados mediante eventos C#. Las reservas (`Object Pool`) reutilizan enemigos, proyectiles, números flotantes y efectos visuales entre expediciones, manteniendo separadas las reglas de simulación y su presentación.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
