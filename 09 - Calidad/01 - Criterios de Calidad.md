# Criterios de calidad

El diseño se revisa junto con el control, la interfaz y la persistencia. Las opciones deben reflejar su efecto inmediato, los avisos de peligro deben preceder al daño y la colección debe respetar su capacidad máxima de 10 unidades y conservar permanentemente los descubrimientos en el índice.

| Aspecto | Comprobación (`b381ef4`) |
| --- | --- |
| Preparación y guía en bóveda | El jugador localiza el balcón de equipo, la máquina de fusión y los puestos de exposición gracias a los rótulos 3D (`EQUIPO 1..3`), las flechas direccionales superiores (`VaultDirections`), el panel de consejos (`VaultTips`) y el **Índice de Colección (`[I]`)**. |
| Control y consulta rápida | Movimiento, dash (`Espacio` / `A`), consulta rápida (`Q` / `Select`) y controles de bóveda/arena/bestiario (`E` / botón oeste, `T` / botón norte, `I` / `Select`) operan tanto en teclado/ratón como en mando. |
| Lectura y pausa | Ataques anunciados, resistencias élite y cambios de fase legibles tanto al `100%` como con `Efectos decorativos` / `Efectos reducidos` en el panel compartido `MenuSettingsView` (3 pestañas); tipografía unificada `Pixelify Sans`. |
| Progresión y combate | Cartas válidas y niveles propios; `FLASH_05` activa `FLASH_03`, `BIO_02` y `CRIT_04` en cada pulso; `DASH_04` fija `0,5 s` de recarga conservando el `+8%/nivel` de velocidad de `DASH_01`; mitigación del daño entrante en `PlayerHealth`; poderes de acompañantes **acumulables por copia equipada**. |
| Colección y capacidad | Capacidad máxima estricta de **10 unidades** (`DisplaySlots[10]`: `3` de equipo + `7` de exposición), bloqueo en `10/10` en arena (`INVENTARIO LLENO / LIBERA ESPACIO`) y en fusión (`INVENTARIO LLENO · LIBERA ESPACIO`), y liberación con confirmación conservando `discovered[15]`. |
| Fusión | Duración fija de **30 segundos (`00:30`)** en recetas homogéneas o de **rarezas mixtas**, selección permitida de copias equipadas (`EN EQUIPO 1/2/3`) con desequipado automático al confirmar, exclusión de Yanikua como ingrediente, probabilidades enteras en UI (`60%`, `40%`, `1%`) y botón único `COLECCIONAR`. |
| Sesión y ranking | Pausa por máscaras independientes, visitas a bóveda, traslado a caldera y cierre de partida coherentes; reserva única de gamertag (`GamertagRegistry`) y sincronización no bloqueante (`PUT`/`DELETE`, `8 s`, `EchoScrap.Rank.v2`) con Firebase (`EventLeaderboardSync`), conservando la elegibilidad pendiente al pulsar Continuar hasta finalizar la primera ronda. |
| Persistencia | `SaveData` v4 en `EchoScrap.Collection.Production.v1` (migración si `owned != null`), `FusionPhase` de 5 estados, volúmenes en `PlayerPrefs` (`MenuSettingsView`) y cola/nombre de ranking persistidos. |

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
