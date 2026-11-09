# UI y menús

| Interfaz | Información y acciones (`9eed59f`) |
| --- | --- |
| Menú inicial (`GameMenuView` / `MainMenu`) | Campo `NOMBRE DEL JUGADOR (RANKING)...` (máx. 20 caracteres), Continuar, Nueva colección, Configuración y Salir; confirmación al sustituir una colección existente. |
| HUD de bóveda (`VaultDirections`, `VaultTips`, `VaultIndexHint`) | Dos tarjetas direccionales superiores con flechas hacia Fusión y Balcón de Equipo, botón superior izquierdo `ÍNDICE · [I]` y tarjeta inferior izquierda `CONSEJO` que rota cada `8 s`. |
| Índice de Colección (`VaultCollectionIndex`, tecla `I`) | Contador `{descubiertos} / 15 DESCUBIERTOS · {en posesión} / 10 UNIDADES`, pestañas `POR RAREZAS` (filtros por rareza y siluetas negras `???`) y `EN TU BÓVEDA` (`EN EQUIPO ACTIVO` / `EN BÓVEDA`), inspector derecho de poder y botón `CERRAR · [I]`. |
| Selector de puesto de equipo (`CompanionView`, puestos 1 a 3) | Se abre con `E` (`E · EQUIPAR` / `E · EDITAR`). Encabezado `PUESTO DE EQUIPO 1/2/3`, subtítulo `ELIGE UNA ENTIDAD DESCUBIERTA PARA ESTE PUESTO` y botones inferiores `QUITAR DEL PUESTO`, `LIBERAR COPIA` y `CERRAR · E / ESC`. (`T` en el mundo desequipa directamente). |
| Modal de liberación (`VaultReleaseInteraction`, puestos 4 a 10 y selector) | Se abre con `E` (`E · LIBERAR`) en planta baja o con `LIBERAR COPIA` en el selector. Cuadro `LIBERAR UNIDAD` con botones `CANCELAR` (`Esc`) y `LIBERAR` (`E`, `Enter` o `Espacio`). |
| Estación de fusión (`FusionStationUI`) | Ventana `Marco_Fusion` con 4 ranuras en cruz (`Cuadro_Fusion` + `Cruz`), botón `X`, probabilidades redondeadas a enteros (`60%`, `40%`, `1%`), botones `VACIAR` y `CONFIRMAR`, selector `MiGaraje` (`EN EQUIPO 1/2/3`, `QUITAR INGREDIENTE`, `VOLVER`), cuenta atrás fija `00:30` y pantalla de resultado con botón único `COLECCIONAR` (y aviso `INVENTARIO LLENO · LIBERA ESPACIO` si hay `10/10` unidades). |
| Selección de ronda (`ELIGE LA RONDA`) | Campo de nombre de jugador para el ranking, Ronda demo (`3:00`), Ronda completa (`8:00`) o cancelar. |
| HUD de combate (`CombatHud`) | Salud, XP, nivel, tiempo, bajas, dash, mejoras, tres retratos equipados (con poderes activos visibles si `Atributos de coleccionables = SÍ`), minimapa circular con exploración y consulta rápida manteniendo `Q` o `Select`. |
| Cartas (`UpgradeDraftController`) | Hasta tres opciones; rareza, nivel, efecto, confirmación y reintento con *Sintonizador* (`R` o botón norte). |
| Menú de Pausa (`CombatHud`, `Esc` / `Start`) | Organizado en **3 pestañas** (`General`: `Movimiento de cámara` y `Efectos decorativos`; `Interfaz`: `Estadísticas generales` `COMPLETAS`/`REDUCIDAS`/`OCULTAS` y `Atributos de coleccionables` `SÍ`/`NO`; `Sonido`: `Volumen general` y `Música`) más los botones `CONTINUAR` y `VOLVER AL MENÚ` (en bóveda) o `VOLVER A LA BÓVEDA` (en arena). |
| Resultados y ranking (`EventLeaderboardSync`) | Victoria o derrota, tiempo, nivel, bajas, XP, sincronización automática con Firebase Realtime Database y botón `VOLVER A LA BÓVEDA`. |
| Laboratorio (`EnemyLaboratory`) | Ocho arquetipos, generación, inspección, daño de prueba, congelación, pausa y avance (`Tab` y `R`). |

## Detalle del panel de estadísticas y consulta rápida (`Q` / `Select`)

Toda la interfaz emplea la fuente **`Pixelify Sans`**. El panel de estadísticas se adapta dinámicamente al número de filas que difieren de su valor base:

- **`COMPLETAS`:** muestra todas las estadísticas modificadas respecto a su base (por cartas o por poderes de acompañantes).
- **`REDUCIDAS`:** filtra los seis valores principales (daño por pulso, velocidad de ataque, alcance, probabilidad crítica, multiplicador crítico y velocidad de movimiento).
- **`OCULTAS`:** oculta el panel durante el combate normal.

En cualquier momento del combate, **mantener pulsada la tecla `Q` (teclado) o el botón `Select` (mando)** activa `CombatHud.IsHudDetailHeld`, desplegando temporalmente las **estadísticas completas** y las **descripciones de los poderes de los tres acompañantes equipados**, independientemente del modo configurado en la pestaña `Interfaz`.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
