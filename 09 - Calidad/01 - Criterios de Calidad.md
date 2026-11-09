# Criterios de calidad

El diseño se revisa junto con el control, la interfaz y la persistencia. Las opciones deben reflejar su efecto inmediato, los avisos de peligro deben preceder al daño y la colección debe respetar su capacidad máxima de 10 unidades y conservar permanentemente los descubrimientos en el índice.

| Aspecto | Comprobación (`9eed59f`) |
| --- | --- |
| Preparación y guía en bóveda | El jugador localiza el balcón de equipo, la máquina de fusión y los puestos de exposición gracias a los rótulos 3D (`EQUIPO 1..3`), las flechas direccionales superiores (`VaultDirections`), el panel de consejos (`VaultTips`) y el **Índice de Colección (`[I]`)**. |
| Control y consulta rápida | Movimiento y dash responden y respetan obstáculos; mantener `Q` o `Select` en combate despliega estadísticas completas y poderes de acompañantes; `E`, `T` e `I` operan colección, equipo, liberación, fusión e índice. |
| Lectura y pausa | Ataques anunciados, resistencias élite y cambios de fase legibles tanto al `100%` como al reducir el deslizador de `Efectos decorativos` en la pestaña `General` del menú de pausa; tipografía unificada `Pixelify Sans`. |
| Progresión y combate | Cartas válidas y niveles propios; `FLASH_05` activa `FLASH_03`, `BIO_02` y `CRIT_04` en cada pulso; `DASH_04` fija `0,5 s` de recarga conservando el `+8%/nivel` de velocidad de `DASH_01`; mitigación del daño entrante en 7 pasos exactos (`PlayerHealth`). |
| Colección y capacidad | Capacidad máxima estricta de **10 unidades** (`3` de equipo + `7` de exposición, `displays[10]`), bloqueo en `10/10` en arena (`INVENTARIO LLENO / LIBERA ESPACIO`) y en fusión (`INVENTARIO LLENO · LIBERA ESPACIO`), y liberación con confirmación conservando `discovered[15]`. |
| Fusión | Duración fija de **30 segundos (`00:30`)** en todas las recetas, selección permitida de copias equipadas (`EN EQUIPO 1/2/3`) con desequipado automático al confirmar, exclusión de Yanikua como ingrediente, probabilidades enteras en UI (`60%`, `40%`, `1%`) y botón único `COLECCIONAR`. |
| Sesión y ranking | Pausa por máscaras independientes, visitas a bóveda, traslado a caldera y cierre de partida coherentes; sincronización no bloqueante con Firebase (`EventLeaderboardSync`) y respaldo JSON local. |
| Persistencia | `SaveData` v4 en `EchoScrap.Collection.Production.v1`, volúmenes general y de música conservados en `PlayerPrefs` y nombre/sesión de evento persistidos. |

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
