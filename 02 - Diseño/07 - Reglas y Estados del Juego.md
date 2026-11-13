# Reglas y estados del juego

| Momento | Regla |
| --- | --- |
| Menú inicial | Continuar (preserva la oportunidad puntuable pendiente si aún no se ha cerrado la primera ronda), Nueva colección (con campo `NOMBRE DEL JUGADOR (RANKING)...` validado mediante `GamertagRegistry` para habilitar una única oportunidad puntuable, o vacío para práctica), Configuración (`MenuSettingsView` en 3 pestañas: `General`, `Interfaz`, `Sonido`) y Salir. El mundo se carga después de elegir colección. |
| Preparación en bóveda | Movimiento, guía activa (`EQUIPO 1..3`, `VaultDirections` y `VaultTips`), consulta del Índice de Colección / Bestiario (`[I]` / `Select`), gestión de las 10 unidades en `DisplaySlots[10]` (3 puestos de equipo en balcón + 7 de exposición en planta baja, poderes acumulables hasta `×3`) y máquina de fusión (`30 s`, misma rareza o rarezas mixtas). El reloj de expedición no avanza. |
| Selección y liberación de entidades | En los puestos 1 a 3 (balcón), `E` / botón oeste (`E · EQUIPAR` / `E · EDITAR`) abre y pausa el selector (`QUITAR DEL PUESTO`, `LIBERAR COPIA`, `CERRAR · E / ESC`), y `T` / botón norte en el mundo desequipa directamente. En los pedestales de planta baja (`slot >= 3`), `E · LIBERAR` / botón oeste abre el modal `LIBERAR UNIDAD`. |
| Índice de Colección (Bestiario) | `I`, `Select` o clic en `ÍNDICE · [I]` pausa la simulación (`PauseReason.FusionMenu`) y muestra las pestañas `POR RAREZAS` (`TODAS` + 6 rarezas; siluetas negras `???` hasta recoger o reclamar por primera vez) y `EN TU BÓVEDA` (hasta 10 unidades). |
| Selección de ronda | En **`ELIGE LA RONDA`** (*"Elige una ronda para comenzar."*) se elige `RONDA DEMO` (`3:00`) o `RONDA COMPLETA` (`8:00`), o `SALIR`. Si la colección tiene su oportunidad puntuable activa, entrar inicia el envío en vivo (`EN PARTIDA`) a `echoscrap.web.app`. |
| Combate | Avanzan reloj, oleadas, ataques, recogida (bloqueada en `10/10` para entidades) y temporizadores activos. Mantener `Q` o `Select` despliega estadísticas completas y poderes de acompañantes en el HUD. |
| Cartas o pausa | El combate se detiene; el menú de pausa (`Esc` / `Start`) ofrece las pestañas `General`, `Interfaz` y `Sonido`, además de `CONTINUAR` y salida a menú o bóveda (abandonar una expedición puntuable cancela el intento y conserva la oportunidad para el siguiente ingreso). |
| Visita a la bóveda | Se conserva la expedición en memoria y se suspenden enemigos y peligros hasta volver por el portal; reclamar una fusión con `COLECCIONAR` durante la expedición puntuable suma como unidad obtenida (`+300` pts base). |
| Entrada de Gólgora | Se retira la horda, se limpian amenazas y se traslada al jugador a la caldera con protección. |
| Transición del jefe | Al llegar al `50%` de vida, Gólgora entra en su segunda fase con una transición invulnerable (`1,5 s` unificado en `GolgoraCombatSettings.cs` y `CombatSettings.asset`) y un rugido que empuja en `12 m`. |
| Victoria o derrota | Se muestra el resumen de resultados, la primera victoria o derrota de una colección elegible bloquea la puntuación definitiva en Firebase (`EventLeaderboardSync`, visible en [`https://echoscrap.web.app`](https://echoscrap.web.app)) y se vuelve a la bóveda. |

## Encuentro con Gólgora

En los arquetipos del jefe, **`Golgora_Demo.asset`** (y el valor por defecto en `EnemyArchetypeSettings.cs`) fija **3 600 HP** a los `180 s` (`3:00`) para la ronda demo, mientras **`Golgora_Complete.asset`** fija **9 000 HP** a los `480 s` (`8:00`) para la ronda completa (ambos con transición a Fase 2 al `50%`: `1 800 HP` / `4 500 HP`). Alcanzar ese tiempo inicia el encuentro; sobrevivir hasta el minuto de aparición no concede la victoria por sí mismo.

La caldera central tiene `22 m` de radio y cierra su perímetro. La entrada reposiciona al jugador sin daño y la cámara encuadra a ambos combatientes. Durante la presentación inicial el jefe no es un objetivo válido.

Los refuerzos se solicitan cada cuatro segundos, en grupos de hasta cuatro y con el orden Residual, Residual, Mortero y Polilla. En la caldera el límite efectivo es de ocho secuaces simultáneos, incluyendo las apariciones pendientes. Cada entrada se anuncia durante `0,9 s`. El *Cerco de magma* y la transición de fase suspenden los refuerzos y retiran los que quedaban activos.

Los ataques (*Bombardeo*, *Cerco de magma* con `40` de daño físico de aterrizaje y `18 s` fijos de enfriamiento, y *Fisura de magma* con `1,1 s` fijos de preparación), inmunidades y tiempos de las dos fases se detallan en [[03 - Enemigos y NPC]].

## Cierre y nueva preparación

La muerte del jugador tiene prioridad si coincide con la del jefe en el mismo paso de simulación. La victoria incluye la presentación final, absorción de gema y `500 XP` para el resumen, sin abrir nuevas elecciones de cartas, y dispara la sincronización de estadísticas de sesión mediante `EventLeaderboardSync`.

Volver a la bóveda limpia actores, peligros y efectos, restablece la configuración de combate y vuelve a aplicar los poderes del equipo guardado. La colección (`hasta 10 unidades` en posesión y `15` descubiertas en el índice) y la fusión en curso o lista para reclamar siguen conservadas.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
