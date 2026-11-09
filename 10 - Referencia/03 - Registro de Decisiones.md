# Registro de decisiones vigentes

Esta edición refleja el estado verificado de `develop` en el commit **`9eed59f`** (**8 de noviembre de 2026**, versión del GDD **`0.6.2`**).

| Área | Regla o decisión vigente (`9eed59f`) |
| --- | --- |
| Flujo general | Menú con campo `NOMBRE DEL JUGADOR (RANKING)...`, colección, preparación guiada en bóveda, selección de ronda, expedición, sincronización de ranking (`EventLeaderboardSync`) y regreso a la bóveda. |
| Señalización de bóveda e Índice | Implementada con rótulos 3D `EQUIPO 1..3` en el balcón, tarjetas direccionales superiores (`VaultDirections`), consejos rotatorios cada `8 s` (`VaultTips`) e **Índice de Colección (`[I]`)** con pestañas `POR RAREZAS` (siluetas `???`) y `EN TU BÓVEDA`. |
| Capacidad de colección (`10/10`) | Máximo estricto de **10 unidades en total** (`FusionProgress.Capacity = 10`), correspondientes a los **10 pedestales físicos de la bóveda** (`displays[10]`: 3 de equipo + 7 de exposición). Bloquea la recogida en arena (`INVENTARIO LLENO / LIBERA ESPACIO`) y el reclamo en fusión (`INVENTARIO LLENO · LIBERA ESPACIO`) al estar llena. |
| Equipo y liberación | Puestos 1 a 3 (balcón): `E` (`E · EQUIPAR` / `E · EDITAR`) abre el selector (`QUITAR DEL PUESTO`, `LIBERAR COPIA`, `CERRAR · E / ESC`) y `T` en el mundo desequipa directamente. Puestos 4 a 10 (planta baja): `E` (`E · LIBERAR`) abre confirmación `LIBERAR UNIDAD`. Liberar descuenta `1` de `owned[id]` y conserva `discovered[id]`. |
| Fusión (`30 s`) | Duración fija de **30 segundos (`00:30`)** para todas las rarezas. Requiere 4 copias de la misma rareza (excluida Yanikua), permite seleccionar copias equipadas (`EN EQUIPO 1/2/3`, desequipándolas al confirmar), redondea porcentajes a enteros en UI (`60%`, `40%`, `1%` para `59,4%`, `39,6%`, `1%`) y presenta un único botón `COLECCIONAR` al reclamar. |
| Tipografía, pausa y HUD | Tipografía unificada **`Pixelify Sans`**. Menú de pausa en 3 pestañas (`General`, `Interfaz` con `Atributos de coleccionables SÍ/NO`, `Sonido` con `Volumen general` y `Música`). Mantener `Q` o `Select` en combate despliega temporalmente estadísticas completas y poderes de acompañantes. |
| Rayo solar (`FLASH_05`) | Elimina el descanso y hace que todos los pulsos cuenten como cuarto pulso (`finalPulse = true`), aplicando empuje y activando `FLASH_03`, `BIO_02` y `CRIT_04` en cada pulso. |
| Cataclismo (`DASH_04`) | Fija la recarga del dash en `0,5 s` (anulando el `-10%/nivel` de recarga de `DASH_01`), pero **conserva el `+8%/nivel` de velocidad de dash de `DASH_01`**. |
| Demolición y Chatarra | `DEMO_03`: `0,25 s` de armado, `1,25 m` de activación, máx. 5 minas. `DEMO_04`: `75%` de daño a los `0,4 s` y atracción base del `45%` del radio. `SCRAP_03`: `180°/s` a `2,5 m`, daño por paso de simulación en solape. `SCRAP_04`: máx. 2 torretas, ráfagas de 3 tiros cada `0,6 s` separados por `0,08 s`. |
| Ojo del verdugo (`CRIT_05`) | Daño crítico ya calculado `×3` si el objetivo estaba estrictamente bajo el `35%` de vida antes del golpe, más aturdimiento de `1 s` en `3 m`. |
| Polilla Reactor y Gólgora | Polilla: espoleta por proximidad a `1,6 m` (`0,9 s` / `1,2 s` al morir), `35` de daño al jugador y `85%` (`29,75`) a otros enemigos normales. Gólgora: `15 000 HP` base (`3 600` demo / `9 000` completa), *Bombardeo* con `0,6 s` de recuperación, *Cerco de magma* con `40` de daño físico de aterrizaje y `18 s` fijos de enfriamiento en ambas fases, y *Fisura de magma* con `1,1 s` fijos de preparación en ambas fases. |
| Guardado y ranking | `SaveData` v4 en `EchoScrap.Collection.Production.v1`, volúmenes en `EchoScrap.Audio.MasterVolume` y `MusicVolume`, y ranking en `EchoScrap_EventRanking_LastSession.json` + Firebase Realtime Database (`EventLeaderboardSync`). |

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
