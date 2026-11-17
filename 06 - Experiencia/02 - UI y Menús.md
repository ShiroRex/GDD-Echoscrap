# UI y menús

| Interfaz | Información y acciones (`8047d90` / `b543684`) |
| --- | --- |
| Menú inicial y Ajustes (`GameMenuView`, `MenuSettingsView`) | `CONTINUAR`, `NUEVA PARTIDA` (selector visual de **3 ranuras** `ARCHIVO 1..3` con alias, contador `?/18`, tiempo jugado y estado), `CONFIGURACIÓN` (panel unificado en 3 pestañas: `General` con movimiento de cámara, efectos decorativos, `Efectos reducidos` en título / `Límite de FPS` `30 / 60 / 120` en pausa y selector **`Pantalla`** `COMPLETA` / `VENTANA`; `Interfaz` y `Sonido`) y `SALIR`; en `NUEVA PARTIDA · ARCHIVO N` incluye el campo `NOMBRE DEL JUGADOR (RANKING)...` (máx. 20 caracteres, validado contra Firebase mediante `GamertagRegistry.Reserve`, o vacío para práctica). |
| HUD de bóveda (`GameMenuView`, `VaultCollectionIndex`) | Dos tarjetas direccionales superiores (`VaultDirections`) hacia Fusión y Balcón de Equipo, botón superior izquierdo `ÍNDICE · [I]` (`BotonIndiceBoveda`) y tarjeta inferior izquierda `CONSEJO` (`VaultTips`) que rota cada `8 s`. |
| Índice de Colección / Bestiario (`VaultCollectionIndex`, `I` / `Select`) | Contador `{descubiertos} / 15 DESCUBIERTOS · {en posesión} / 18 UNIDADES`, pestañas `POR RAREZAS` (`TODAS` + 6 rarezas; siluetas negras `???` que pasan a retrato a color al recoger o reclamar por primera vez) y `EN TU BÓVEDA` (`EN EQUIPO ACTIVO` / `EN BÓVEDA`), inspector derecho de poder y botón `CERRAR · [I]`. |
| Selector de puesto de equipo (`CompanionView`, puestos 1 a 3) | Se abre con `E` / botón oeste (`E · EQUIPAR` / `E · EDITAR`). Encabezado `PUESTO DE EQUIPO 1/2/3`, subtítulo `ELIGE UNA ENTIDAD DESCUBIERTA PARA ESTE PUESTO`, nota de poderes acumulables por copia equipada y botones inferiores `QUITAR DEL PUESTO`, `LIBERAR COPIA` y `CERRAR · E / ESC`. (`T` / botón norte en el mundo desequipa directamente). |
| Modal de liberación (`VaultReleaseInteraction`) | Se abre con `E` / botón oeste (`E · LIBERAR`) en los puestos ocupados de planta baja (`slot >= 3`), o con `LIBERAR COPIA` en el selector. Cuadro `LIBERAR UNIDAD` con botones `CANCELAR` (`Esc` / botón este) y `LIBERAR` (`E`, `Enter`, `Espacio` o `A`/`Cruz`). |
| Estación de fusión (`FusionStationUI`) | Ventana con 4 ranuras horizontales (`Espacio_0..3`), `TIEMPO DE FUSIÓN 00:30`, botón `X`, panel `POSIBLES RESULTADOS` (misma rareza o rarezas mixtas; `60%`, `40%`, `1%` en 4 Épicas), botones `VACIAR` y `CONFIRMAR`, selector (`EN EQUIPO 1/2/3`, `QUITAR INGREDIENTE`, `VOLVER`), y pantalla de resultado con botón único `COLECCIONAR` (o `INVENTARIO LLENO · LIBERA ESPACIO` si hay `18/18` unidades). |
| Selección de ronda (`ELIGE LA RONDA`) | *"Elige una ronda para comenzar."* con opciones `RONDA DEMO` (`3:00`), `RONDA COMPLETA` (`8:00`) o `SALIR` (`Esc` / botón este). |
| HUD de combate (`CombatHud`, `CompanionView`, `PlayerStatsReadout`) | Salud, XP, nivel, tiempo, bajas, dash, cuadrícula de mejoras (`9 × 34 px` hasta `62` cartas; `+100 px` si ancho `< 1530 px`), panel de `22` filas de estadísticas (incluye `Distancia dash` y `Área efectos`), tres retratos equipados (`Retrato_0..2`, sin insignia numérica superpuesta; líneas inferiores con multiplicador `×N` y totales acumulados via `DescribeHudPower` si `Atributos de coleccionables = SÍ` o manteniendo `Q`/`Select`), minimapa circular con exploración y consulta rápida. |
| Cartas (`UpgradeDraftController`) | Hasta tres opciones en **5 rarezas** (`Común`, `Rara`, `Épica`, `Legendaria` y `Mítica` para *All In*); rareza, nivel, efecto, confirmación y reintento con *Sintonizador* (`R` o botón norte). |
| Menú de Pausa (`CombatHud` + `MenuSettingsView`, `Esc` / `Start`) | Comparte las **3 pestañas** de `MenuSettingsView` (`General`: `Movimiento de cámara`, `Efectos decorativos`, `Límite de FPS` `30 / 60 / 120` y **`Pantalla`** `COMPLETA` / `VENTANA`; `Interfaz`: `Estadísticas generales` `COMPLETAS`/`REDUCIDAS`/`OCULTAS` y `Atributos de coleccionables` `SÍ`/`NO`; `Sonido`: `Volumen general` y `Música`) más los botones `CONTINUAR` y `VOLVER AL MENÚ` (en bóveda) o `VOLVER A LA BÓVEDA` (en arena). |
| Resultados y ranking (`EventLeaderboardSync`) | Victoria o derrota, tiempo, nivel, bajas, XP, sincronización automática de todos los intentos válidos con Firebase Realtime Database (`rulesVersion = 2`, conservando la mejor puntuación por gamertag en [`https://echoscrap.web.app`](https://echoscrap.web.app)) y botón `VOLVER A LA BÓVEDA`. |
| Laboratorio (`EnemyLaboratory`) | Ocho arquetipos, generación, inspección, daño de prueba, congelación, pausa y avance (`Tab` y `R`). |

## Detalle del panel de estadísticas y consulta rápida (`Q` / `Select`)

Toda la interfaz emplea la fuente **`Pixelify Sans`**. El panel de estadísticas se adapta dinámicamente al número de filas que difieren de su valor base:

- **`COMPLETAS`:** muestra todas las estadísticas modificadas respecto a su base (por cartas o por poderes de acompañantes).
- **`REDUCIDAS`:** filtra los seis valores principales (daño por pulso, velocidad de ataque, alcance, probabilidad crítica, multiplicador crítico y velocidad de movimiento).
- **`OCULTAS`:** oculta el panel durante el combate normal.

En cualquier momento del combate, **mantener pulsada la tecla `Q` (teclado) o el botón `Select` (mando)** activa `CombatHud.IsHudDetailHeld`, desplegando temporalmente las **estadísticas completas** y las **descripciones de los poderes de los tres acompañantes equipados**, independientemente del modo configurado en la pestaña `Interfaz`.

## Vistas del Índice de Colección (Bestiario) y Ranking Web

![Índice de Colección (Bestiario en versión final)](../Media/Interfaz/Bestiario.png)
*Vista de la versión definitiva del bestiario (`Índice de Colección`, tecla `[I]`) con la tipografía unificada Pixelify Sans, contador en vivo, filtros por rareza, siluetas e inspector de compañero.*

![Página web del Ranking en vivo (`echoscrap.web.app`)](../Media/Interfaz/RankingWeb_EchoScrap.png)
*Visor web en vivo ([`https://echoscrap.web.app`](https://echoscrap.web.app)) conectado a Firebase Realtime Database con la clasificación general por puesto, jugador, bajas, tiempo y puntos.*

![Modal de Nueva Colección con Gamertag](../Media/Interfaz/Nueva_Coleccion.png)
*Captura de referencia de la confirmación con gamertag (`734e2a5`); en `0.7.0`, este campo se integra tras elegir ranura en `NUEVA PARTIDA · ARCHIVO N`.*

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
