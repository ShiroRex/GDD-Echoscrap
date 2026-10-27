# Arquitectura técnica

El proyecto activo es `EchoScrap/`. La arena guarda los objetos, referencias y ajustes necesarios para iniciar la partida desde Unity. El arranque enlaza al jugador con los sistemas de combate y reutiliza la sesión al reiniciar.

| Responsabilidad | Componentes actuales |
| --- | --- |
| Composición de la arena | CombatGameBootstrap |
| Tiempo, pausas y resultado | CombatSession, GameplayClock, CombatOutcomeResolver |
| Entradas y movimiento | PlayerInputReader, PlayerMotor, TerrainPlayerCollision |
| Salud y estadísticas | PlayerHealth, PlayerRuntimeStats, PlayerCombatSettings |
| Arma | FlashlightWeapon y su presentación |
| Enemigos | EnemyActor, EnemyPool, navegación y módulos de ataques |
| Horda y jefe | CombatWaveDirector, CombatWaveSettings, BossEncounter, GolgoraActor |
| Progresión y cartas | CombatProgression, UpgradeDraftController, UpgradeCatalog |
| Habilidades y presentación | UpgradeCombatRuntime, UpgradeEffectPool, VoltaicChainRenderer |
| Mundo y colección | WarehouseTravel, WarehouseCameraDirector, FusionStation, FusionProgress |
| Interfaz | CombatHud, UpgradeDraftPresentation, PlayerStatsReadout, DamageNumberPresenter |

El daño, las muertes, el dash y otros cambios se comunican mediante eventos de la sesión. La presentación reacciona a esos eventos sin resolver por su cuenta el resultado de los ataques. El cierre de victoria o derrota se decide después de los impactos del paso de simulación.

Los enemigos, objetos y efectos utilizan reservas reutilizables. La semilla de combate y la de presentación están separadas para que los cambios decorativos no cambien los sorteos jugables.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
