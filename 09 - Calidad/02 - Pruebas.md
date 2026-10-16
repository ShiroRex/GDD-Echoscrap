# Pruebas

## Descripción de la sección

Define el plan de pruebas y las comprobaciones que se ejecutarán durante el desarrollo de *EchoScrap* para verificar que las mecánicas, sinergias de cartas, flujo de partida y rendimiento operen conforme al GDD.

## Tipos de prueba

### 1. Pruebas funcionales

Verifican que cada mecánica individual cumpla sus reglas matemáticas:
- Ciclo de la linterna de 4 pulsos ($0.2 / 0.4 / 0.6 / 0.8\text{ s}$) y descanso hasta $1.2\text{ s}$, con retroceso únicamente en el 4.º impacto.
- Aplicación de mejoras adquiridas durante la pausa al inicio del siguiente ciclo completo (sin reiniciar el contador ni restaurar críticos gastados).
- Topes defensivos (Armadura $60\%$, Esquiva $35\%$, Bloqueo $50\%$) y ventana de gracia (*i-frames* de $0.40\text{ s}$ tras daño y $0.15\text{ s}$ en *Dash*).

### 2. Pruebas de integración

Verifican la interacción entre sistemas:
- Elegibilidad condicional de cartas (por ejemplo, Pólvora `EXP_02` pasa de no elegible a elegible tras adquirir Minas de Proximidad o Cataclismo).
- Transición del minuto 8:00: corte del escalado normal, onda de choque inicial que despeja la arena sin soltar XP, aparición de Gólgora e invocación periódica de esbirros cada $4.0\text{ s}$.
- Flujo post-victoria: derrota de Gólgora $\rightarrow$ Ruleta de Extracción $\rightarrow$ desbloqueo de Entidad o incremento de $+1$ nivel por duplicado $\rightarrow$ guardado en JSON y reflejo en el Almacén.

### 3. Pruebas de usabilidad y accesibilidad

Comprueban que el jugador comprenda las señales visuales y sonoras:
- Navegación completa de menús, selección de hasta 3 entidades en el Almacén y elección de las 3 cartas al subir de nivel usando únicamente teclado (`WASD` / `1, 2, 3`) o mando.
- Claridad del círculo de advertencia de Gólgora ($5\text{ m}$) y del parpadeo ámbar de la Polilla ($0.5\text{ s}$) incluso con la reducción de destellos activada.

### 4. Pruebas de rendimiento

Comprueban la estabilidad en condiciones de carga máxima:
- Mantener $60\text{ FPS}$ estables a $1920 \times 1080$ durante las Fases 4 y 5 con $150$ a $250$ enemigos simultáneos y efectos derivados (como chispas de Prisma o explosiones en cadena).

## Casos de prueba principales

| ID | Caso de prueba | Precondiciones | Pasos | Resultado esperado |
|---|---|---|---|---|
| **CP-01** | Sinergia Lente 5 + Foco 3 (independencia de orden) | Partida activa en arena. | Adquirir `FLASH_01` (nivel 5) y `FLASH_03` (nivel 3) en cualquier orden. | Alcance normal de $12\text{ m}$ ($19\text{ m}$ en modo solar), apertura final de $90^\circ$, retroceso final de $30$ y multiplicador de daño de Foco $\times 2.20$. |
| **CP-02** | Pausa tras el 3.er pulso y selección de mejora | Linterna en el pulso 3 de 4 al subir de nivel. | Elegir Prisma o Hiper-Rayo Solar en el modal de subida de nivel y reanudar. | No se aplica daño durante la pausa; el 4.º pulso termina con las reglas del ciclo previo y los cambios entran en el siguiente ciclo completo. |
| **CP-03** | Derrota vs. Victoria y Ruleta de Extracción | Dos partidas: una cayendo a $0\text{ HP}$ y otra derrotando a Gólgora. | Completar el cierre de ambas partidas. | Al morir se muestra Derrota y se vuelve al Almacén sin ruleta; al vencer a Gólgora se activa la Ruleta de Extracción y se guarda la entidad (o $+1$ nivel si es duplicada). |
| **CP-04** | Fuego amigo de La Polilla | Polilla dentro de $1.5\text{ m}$ rodeada de enemigos básicos. | Activar su fusible de $0.5\text{ s}$ y esquivar fuera del radio de $3.0\text{ m}$ con *Dash*. | La explosión de $35.0$ de daño impacta a los enemigos cercanos sin dañar al Coleccionista. |

> **Navegación:** [[00 - Índice]] · ← [[01 - Criterios de Calidad]] · [[03 - Bugs y Seguimiento]] →
