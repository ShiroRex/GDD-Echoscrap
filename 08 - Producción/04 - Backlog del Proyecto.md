# Entregas y trabajo pendiente

| Área | Estado al 8 de noviembre (`develop` @ `9eed59f`) |
| --- | --- |
| Flujo y ranking | Menú con nombre de jugador (`EchoScrap.Event.PlayerName`), colección, preparación guiada en bóveda, elección de ronda, expedición, sincronización con Firebase (`EventLeaderboardSync`) y regreso desde resultados integrados. |
| Señalización de bóveda e Índice | Rótulos 3D `EQUIPO 1..3`, flechas direccionales superiores (`VaultDirections`), panel de consejos rotatorios cada `8 s` (`VaultTips`) e **Índice de Colección (`[I]`)** con pestañas `POR RAREZAS` y `EN TU BÓVEDA`. |
| Equipo y liberación | Tres puestos en el balcón (`E · EQUIPAR` / `E · EDITAR`, botones `QUITAR DEL PUESTO`, `LIBERAR COPIA`, `CERRAR · E / ESC` y `T` en el mundo) y siete puestos de exposición en planta baja (`E · LIBERAR` con confirmación `LIBERAR UNIDAD`). |
| Colección y fusión | Quince entidades descubribles, capacidad máxima estricta de **10 unidades** (`INVENTARIO LLENO`), fusión de 4 copias de la misma rareza (incluidas copias equipadas `EN EQUIPO 1/2/3`, excluida Yanikua), duración fija de **30 s (`00:30`)** y botón único `COLECCIONAR`. |
| Combate y mejoras | Cinco arquetipos de horda, dos élites, linterna automática (`FLASH_05` activa todas las mejoras de cuarto pulso en cada pulso) y 35 mejoras. |
| Gólgora | Caldera de `22 m`, seis patrones (`Bombardeo` con `0,6 s` de recuperación, `Cerco de magma` con `40` de daño físico al aterrizar y `18 s` fijos de enfriamiento, `Fisura de magma` con `1,1 s` fijos de preparación), dos fases y refuerzos anunciados. |
| Interfaz y audio | Tipografía unificada **`Pixelify Sans`**, menú de pausa en 3 pestañas (`General`, `Interfaz` con `Atributos de coleccionables SÍ/NO`, `Sonido` con `Volumen general` y `Música`) y consulta rápida del HUD manteniendo `Q` / `Select`. |

## Integraciones cerradas en esta revisión (`0.6.2`)

| ID | Tarea | Estado | Comprobación en código (`9eed59f`) |
| --- | --- | --- | --- |
| UX-01 | Guiar la preparación de entidades y navegación en la bóveda. | Integrado | Rótulos 3D `EQUIPO 1..3` (`CollectibleDisplay`), indicadores direccionales `VaultDirections` y consejos `VaultTips` cada `8 s` (`GameMenuView`). |
| VAULT-INDEX-01 | Implementar Índice de Colección (`[I]`) y límite de 10 unidades con liberación en pedestales. | Integrado | `VaultCollectionIndex.cs` (`POR RAREZAS` con siluetas `???` y `EN TU BÓVEDA`), `FusionProgress.Capacity = 10` y `VaultReleaseInteraction.cs` (`E · LIBERAR`). |
| FUSION-30S-01 | Unificar duración de fusión a `30 s`, permitir copias equipadas y simplificar reclamo. | Integrado | `FusionRecipe.FusionSeconds = 30`, etiquetas `EN EQUIPO 1/2/3` con desequipado automático al confirmar, porcentajes enteros en UI (`60%`, `40%`, `1%`) y botón único `COLECCIONAR`. |
| UI-PAUSE-01 | Reorganizar pausa en 3 pestañas, añadir consulta rápida `Q`/`Select` y unificar tipografía `Pixelify Sans`. | Integrado | `CombatHud.cs` (`General`, `Interfaz`, `Sonido` y `IsHudDetailHeld`) y `PixelifySans-Regular/SemiBold` en `Typography.asset`. |
| EVENT-RANK-01 | Sincronizar ranking de sesión en vivo con Firebase. | Integrado | `EventLeaderboardSync.cs` con respaldo local JSON y peticiones HTTPS `PATCH` a Firebase Realtime Database. |
| ART-ELITE-01 / 02 | Conectar sprites y ciclos de combate de Caballero Fantasma y Centinela de Cobre. | Integrado | Atlas de 224 y 288 fotogramas integrados con mapas de normales y emisión. |
| COMBAT-SOLAR-01 | Aplicar empuje y efectos de cuarto pulso en cada pulso de *Rayo solar*. | Integrado | `UpgradeCombatRuntime.cs:169` (`finalPulse = true` con `FLASH_05`, activando `FLASH_03`, `BIO_02` y `CRIT_04`). |

## Pendientes identificados

| ID | Tarea | Prioridad propuesta | Responsable según el reparto | Dependencia | Criterio de aceptación |
| --- | --- | --- | --- | --- | --- |
| INPUT-01 | Revisar asignaciones de mando para las interacciones de bóveda y recogida (`E`, `T`, `I`). | Media | David Alejandro Sierra Sosa y coordinación de producción | Decisión de alcance sobre botones de mando fuera del combate | QA verifica abrir, seleccionar, cancelar y confirmar en los dispositivos cubiertos. |
| TECH-01 | Validar perfiles de rendimiento en build Windows x64 candidata. | Alta | David Alejandro Sierra Sosa, con revisión de Luis Fernando Nuñez Chan | Equipos de prueba de perfil mínimo y recomendado | Resolución, FPS, p95 de fotograma, memoria y tamaño instalado registrados por perfil. |

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
