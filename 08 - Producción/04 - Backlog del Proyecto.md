# Entregas y trabajo pendiente

| Área | Estado final (`main` @ `b381ef4` / `develop` @ `977bac5`) |
| --- | --- |
| Flujo y ranking | Menú con campo `NOMBRE DEL JUGADOR (RANKING)...` en `¿NUEVA COLECCIÓN?` (`GamertagRegistry`), preservación de oportunidad pendiente al continuar, preparación en bóveda, elección de ronda (`ELIGE LA RONDA`), expedición y ranking `rulesVersion = 2` en `https://echoscrap.web.app`. |
| Señalización de bóveda e Índice | Rótulos 3D `EQUIPO 1..3`, flechas direccionales superiores (`VaultDirections`), panel de consejos (`VaultTips`) e **Índice de Colección (`[I]`)** (`I` / `Select`) con pestañas `POR RAREZAS` (`TODAS` + 6 categorías) y `EN TU BÓVEDA`. |
| Equipo y liberación | Tres puestos en el balcón (`E` / botón oeste para editar, botones `QUITAR DEL PUESTO`, `LIBERAR COPIA`, `CERRAR · E / ESC`, `T` / botón norte para desequipar rápido) y siete puestos de exposición en planta baja (`E · LIBERAR`), con **poderes acumulables por copia equipada**. |
| Colección y fusión | Quince entidades descubribles, capacidad máxima estricta de **10 unidades** (`DisplaySlots[10]`, aviso `INVENTARIO LLENO`), fusión de 4 copias de la misma rareza o de **rarezas mixtas** (incluidas `EN EQUIPO 1/2/3`, excluida Yanikua), duración fija de **30 s (`00:30`)** y botón único `COLECCIONAR`. |
| Combate y mejoras | Cinco arquetipos de horda, dos élites, linterna automática (`FLASH_05` activa todas las mejoras de cuarto pulso en cada pulso), `0,22 s` de invulnerabilidad en dash y 35 mejoras calibradas. |
| Gólgora | Caldera de `22 m`, seis patrones unificados en `GolgoraCombatSettings.cs` y `CombatSettings.asset`, dos fases y refuerzos anunciados. |
| Interfaz, audio y distribución | Tipografía **`Pixelify Sans`**, panel unificado `MenuSettingsView` en 3 pestañas (`General`, `Interfaz`, `Sonido`) en menú principal y pausa, consulta rápida `Q` / `Select`, audio integrado y build publicada en [`https://naked-hiro.itch.io/echoscrap`](https://naked-hiro.itch.io/echoscrap). |

## Integraciones cerradas (`0.6.2` – `0.6.6`)

| ID | Tarea | Estado | Comprobación en código (`b381ef4`) |
| --- | --- | --- | --- |
| UX-01 | Guiar la preparación de entidades y navegación en la bóveda. | Integrado | Rótulos 3D `EQUIPO 1..3` (`CollectibleDisplay`), indicadores direccionales `VaultDirections` y consejos `VaultTips` cada `8 s` (`GameMenuView`). |
| VAULT-INDEX-01 | Implementar Índice de Colección (`[I]`) y límite de 10 unidades con liberación en pedestales. | Integrado | `VaultCollectionIndex.cs` (`TODAS` + 6 rarezas y `EN TU BÓVEDA`), `FusionProgress.Capacity = 10` y `VaultReleaseInteraction.cs`. |
| FUSION-30S-01 | Unificar duración de fusión a `30 s`, permitir copias equipadas y rarezas mixtas. | Integrado | `FusionRecipe.FusionSeconds = 30`, soporte de rarezas mixtas (`qualityMean`), `EN EQUIPO 1/2/3`, porcentajes en UI (`60%`, `40%`, `1%`) y `COLECCIONAR`. |
| UI-PAUSE-01 | Compartir ajustes en 3 pestañas, consulta rápida `Q`/`Select` y tipografía `Pixelify Sans`. | Integrado | `MenuSettingsView.cs` en Menú Inicial y Pausa (`General`, `Interfaz`, `Sonido`) y `CombatHud.IsHudDetailHeld`. |
| EVENT-RANK-01 | Sincronizar ranking de sesión en vivo con Firebase (`rulesVersion = 2`). | Integrado | `GamertagRegistry.cs` (`if-match: null_etag`) y `EventLeaderboardSync.cs` (`PUT`/`DELETE`, `8 s` timeout, cola en `EchoScrap.Rank.v2`). |
| INPUT-01 | Cubrir con mando las interacciones de bóveda, recogida y bestiario (`E`, `T`, `I`). | Integrado | `CompanionSystem`, `FusionStation`, `VaultReleaseInteraction`, `CollectibleShard` (`buttonWest`), `VaultReleaseInteraction` (`buttonNorth`) y `VaultCollectionIndex` (`selectButton`). |
| ART-ELITE-01 / 02 | Conectar sprites y ciclos de combate de Caballero Fantasma y Centinela de Cobre. | Integrado | Atlas de 224 y 288 fotogramas integrados con mapas de normales y emisión. |
| COMBAT-SOLAR-01 | Aplicar empuje y efectos de cuarto pulso en cada pulso de *Rayo solar*. | Integrado | `UpgradeCombatRuntime.cs:169` (`finalPulse = true` con `FLASH_05`, activando `FLASH_03`, `BIO_02` y `CRIT_04`). |

## Pendientes identificados para entrega final

| ID | Tarea | Prioridad propuesta | Responsable según el reparto | Dependencia | Criterio de aceptación |
| --- | --- | --- | --- | --- | --- |
| MEDIA-TRAILER-01 | Grabar y publicar el tráiler de presentación de *EchoScrap*. | Alta | Equipo EchoScrap | Build final en `itch.io` y capturas finales | Video editado con gameplay real, música propia y enlace visible en `itch.io` / GDD. |
| TECH-01 | Validar perfiles de rendimiento en build Windows x64 final. | Media | David Alejandro Sierra Sosa, con revisión de Luis Fernando Nuñez Chan | Equipos de prueba de perfil mínimo y recomendado | Resolución, FPS, `p95` de fotograma, memoria y tamaño instalado registrados por perfil. |

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
