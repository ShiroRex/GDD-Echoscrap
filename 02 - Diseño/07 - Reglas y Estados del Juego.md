# Reglas y estados del juego

| Momento | Regla |
| --- | --- |
| Menú inicial | Continuar, Nueva colección, Configuración y Salir. El mundo se carga después de elegir colección. |
| Preparación en bóveda | Movimiento, revisión de inventario, equipo de hasta tres entidades y fusión. El reloj de expedición no avanza. |
| Selección de entidades | Al acercarse a cada puesto y pulsar E se pausa la interacción del mundo y se escoge una copia disponible. Puede dejarse vacío. |
| Selección de ronda | Se elige demo o completa antes de entrar. |
| Combate | Avanzan reloj, oleadas, ataques, recogida y temporizadores activos. |
| Cartas o pausa | El combate se detiene; la interfaz mantiene sus animaciones y confirmaciones. |
| Visita a la bóveda | Se conserva la expedición en memoria y se suspenden enemigos y peligros hasta volver. |
| Entrada de Gólgora | Se retira la horda, se limpian amenazas y se traslada al jugador a la caldera con protección. |
| Transición del jefe | Al llegar al 50% de vida, Gólgora entra en su segunda fase con una presentación protegida. |
| Victoria o derrota | Se muestra el resultado y se puede volver a la bóveda. |

## Encuentro con Gólgora

La demo activa la entrada a los 180 s y usa 3600 HP; la completa, a los 480 s y con 9000 HP. El tiempo inicia el encuentro. No concede una victoria por sí mismo.

La caldera central tiene 22 m de radio y cierra su perímetro. La entrada reposiciona al jugador sin daño y la cámara encuadra a ambos combatientes. Durante la presentación inicial el jefe no es un objetivo válido.

Los refuerzos se solicitan cada cuatro segundos, en grupos de hasta cuatro y con el orden Residual, Residual, Mortero y Polilla. En la caldera el límite efectivo es de ocho secuaces, incluyendo las apariciones pendientes. Cada entrada se anuncia durante 0,9 s. El cerco de magma y la transición de fase suspenden los refuerzos y retiran los que quedaban activos.

Los ataques, inmunidades y tiempos de las dos fases están en [[03 - Enemigos y NPC]].

## Cierre y nueva preparación

La muerte del jugador tiene prioridad si coincide con la del jefe en el mismo paso. La victoria incluye la presentación final, absorción de gema y 500 XP para el resumen, sin nuevas cartas.

Volver a la bóveda limpia actores, peligros y efectos, restablece la configuración de combate y vuelve a aplicar los poderes del equipo guardado. La colección y la fusión siguen conservadas. Abandonar una expedición requiere confirmación; Nueva colección también confirma antes de borrar la colección existente.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
