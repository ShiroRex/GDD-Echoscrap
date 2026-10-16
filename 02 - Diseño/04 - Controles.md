# Controles

## Descripción de la sección

Define la relación entre las acciones del jugador y las entradas físicas del dispositivo (teclado, ratón y gamepad), así como la arquitectura técnica de captura y reasignación.

---

## Mapa de controles

| Acción | Teclado | Ratón | Gamepad (Xbox / Genérico) | Gamepad (PlayStation) |
|---|---|---|---|---|
| **Moverse / Posicionarse** | `W, A, S, D` o Flechas | N/A | Stick Izquierdo / D-Pad | Stick Izquierdo / D-Pad |
| **Dash / Evasión** | `Espacio` | N/A | Botón Sur (`A`) / Triggers (`RT` o `LT`) | Botón Sur (`Cross`) / Triggers (`R2` o `L2`) |
| **Ataque con Linterna** | *Automático (Auto-Aim)* | N/A | *Automático (Auto-Aim)* | *Automático (Auto-Aim)* |
| **Seleccionar Mejora (Draft UI)** | `1, 2, 3` / Enter | Clic Izquierdo | Botones de Acción / Navegación D-Pad | Botones de Acción / Navegación D-Pad |
| **Re-roll de Cartas (con `GREED_04`)** | `R` | Clic en botón UI | Botón Norte (`Y`) | Botón Norte (`Triangle`) |
| **Pausa / Menú** | `Escape` | N/A | Botón `Start` / `Menu` | Botón `Options` |

---

## Arquitectura Planteada para la Gestión de Entradas

Se planea implementar la gestión de controles mediante el nuevo sistema de entradas de Unity (**New Input System**), siguiendo un patrón modular y desacoplado:

1. **Desacoplamiento Funcional:** Los módulos de locomoción y evasión no consultan el hardware físico directamente; en su lugar, se suscriben a eventos y canales emitidos por un gestor de entradas centralizado. Esto facilita la compatibilidad simultánea con teclado, ratón y gamepad.
2. **Normalización Vectorial:** La lectura del movimiento direccional compuesto se normaliza en magnitud ($\le 1.0$) para evitar que desplazarse en diagonal otorgue una mayor velocidad física de desplazamiento.
3. **Soporte de Reasignación en Caliente:** El diseño contempla permitir la reconfiguración personalizada de teclas y botones desde el menú de opciones.
4. **Persistencia de Preferencias:** Cualquier modificación en la asignación de controles realizada por el usuario se guardará en los datos de configuración local, aplicándose automáticamente al iniciar la aplicación.

---

## Principios de control y retroalimentación

1. **Inmediatez y Responsividad (Baja Latencia):** La respuesta del movimiento tiene una aceleración alta (80 m/s²), transmitiendo una sensación instantánea de control sin inercias pesadas.
2. **Accesibilidad con una Mano (Estilo Survivors):** Gracias al autoapuntado que adquiere al enemigo más cercano dentro del alcance efectivo de la linterna (7.0 metros de base, modificado por sus mejoras), o sigue la marcha si no hay amenazas, el jugador puede jugar una partida completa utilizando exclusivamente una mano en el teclado (WASD + Espacio) o en el mando (Stick analógico + Botón A).
3. **Retroalimentación Visual Inmediata:**
   * **Al Moverse y Apuntar:** El cuerpo del personaje rota hacia la dirección de avance, mientras que el haz de luz de la linterna pivota de forma autónoma (720°/s) hacia el enemigo más próximo dentro del alcance efectivo del arma, mostrando su cobertura resultante.
   * **Al Dashear:** El personaje acelera instantáneamente a 22 m/s, acompañado de una ráfaga de movimiento y activación de invulnerabilidad.
   * **Al Recibir Daño:** Si el jugador es alcanzado fuera de los *i-frames*, el material parpadea en rojo/blanco durante 0.15s y se activa un periodo de gracia de 0.40s.

---

> **Navegación:** [[00 - Índice]] · ← [[03 - Sistemas]] · [[05 - Progresión]] →
