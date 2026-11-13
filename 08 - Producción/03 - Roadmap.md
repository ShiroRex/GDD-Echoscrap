# Hitos del desarrollo documentado

| Periodo | Resultado incorporado al juego |
| --- | --- |
| 21–22 de octubre | Proyecto Unity, locomoción, cámara, salud, dash y linterna. |
| 22 de octubre | Cinco enemigos de horda, XP, botiquines, cartas, oleadas, Gólgora, HUD y cierre de partida. |
| 23–24 de octubre | Ajustes de aparición y balance, sprites animados, iluminación, cartas y presentación de daños. |
| 24–25 de octubre | Mundo 3D, MataColeccionista, acceso a la bóveda, colisiones y laboratorio de enemigos. |
| 25–26 de octubre | Catálogo de 35 mejoras, variantes de tres y ocho minutos, interfaz y estadísticas del jugador. |
| 26 de octubre | Fusión por rareza, colección expuesta, barra de XP, magma, portal y renovación de VFX. |
| 27 de octubre | Limpieza de material auxiliar del repositorio y actualización del GDD con registros del juego. |
| 4–5 de noviembre | Menú independiente y carga, quince entidades con poderes propios (`CompanionPowerRuntime`), equipo de tres puestos, fusión de cuatro copias, dos élites con sprites integrados, sprites de Esqueleto, Interceptor y Wendigo, minimapa y audio contextual. |
| 6–8 de noviembre (`develop` @ `9eed59f`) | Señalización completa de la bóveda (`EQUIPO 1..3`, `VaultDirections`, `VaultTips`), **Índice de Colección (`[I]`)**, capacidad máxima estricta de **10 unidades**, liberación directa en pedestales (`E · LIBERAR`), fusión fija de **30 s (`00:30`)**, menú de pausa en 3 pestañas (`General`, `Interfaz`, `Sonido`), consulta rápida `Q`, tipografía unificada `Pixelify Sans` y ranking con Firebase (`EventLeaderboardSync`). |
| 10–13 de noviembre (`main` @ `b381ef4` / `develop` @ `977bac5`) | Ranking `rulesVersion = 2` (`GamertagRegistry`, visor en vivo `echoscrap.web.app`, preservación de elegibilidad al continuar), poderes de acompañantes **acumulables por copia equipada (`Copies(id)`)**, fusión homogénea y de **rarezas mixtas**, panel unificado `MenuSettingsView` de 3 pestañas en menú inicial y pausa, esquema oficial consolidado en **teclado y ratón** (dejando los enlaces experimentales de mando como objetivo a futuro), calibración unificada de enemigos/Gólgora (`CombatSettings.asset`), audio integrado y publicación en **itch.io** (`https://naked-hiro.itch.io/echoscrap`). |
| Post-entrega (planificación a futuro) | Soporte oficial completo y navegación de UI/menús con **mando** (llevando los enlaces experimentales de `Input System` a cobertura integral y validación QA sobre hardware), junto con el montaje final del tráiler promocional en **itch.io**. |

## Hitos de cierre pendientes

| Hito | Responsable según el reparto | Depende de | Condición de cierre |
| --- | --- | --- | --- |
| Tráiler promocional final (`TRAILER-01`) | Equipo EchoScrap / Arte y Edición | Capturas de gameplay final y montaje de video | Tráiler publicado y enlazado en ficha de proyecto y página de itch.io. |
| Validación técnica en perfiles de hardware | David Alejandro Sierra Sosa y Luis Fernando Nuñez Chan | Build Windows x64 final y equipos de prueba | Mediciones de resolución, FPS y `p95` completadas en equipos mínimo y recomendado. |

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
