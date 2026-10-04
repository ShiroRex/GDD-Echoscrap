# Controles

## Descripción de la sección

Define la relación entre las acciones del jugador y las entradas físicas del dispositivo (teclado, ratón y gamepad), así como la arquitectura técnica de captura y reasignación.

---

## Mapa de controles

| Acción | Teclado | Ratón | Gamepad (Xbox / Genérico) | Gamepad (PlayStation) |
|---|---|---|---|---|
| **Moverse / Orientar** | `W, A, S, D` o Flechas | N/A | Stick Izquierdo / D-Pad | Stick Izquierdo / D-Pad |
| **Dash / Evasión** | `Espacio` | N/A | Botón Sur (`A`) / Triggers (`RT` o `LT`) | Botón Sur (`Cross`) / Triggers (`R2` o `L2`) |
| **Ataque Frontal (Linterna)** | *Automático hacia el avance* | N/A | *Automático hacia el avance* | *Automático hacia el avance* |
| **Seleccionar Mejora (Draft UI)** | `1, 2, 3` / Enter | Clic Izquierdo | Botones de Acción / Navegación D-Pad | Botones de Acción / Navegación D-Pad |
| **Pausa / Menú** | `Escape` | N/A | Botón `Start` / `Menu` | Botón `Options` |

---

## Arquitectura técnica de entradas (`InputReader`)

La gestión de entradas en *EchoScrap* se implementa mediante el **Unity New Input System**, siguiendo un patrón completamente desacoplado:

1. **Desacoplamiento Total:** Los componentes de locomoción (`PlayerMovement`) y evasión (`PlayerDash`) no consultan directamente el hardware ni usan métodos legados como `Input.GetKey`. En su lugar, se suscriben a eventos y propiedades expuestas por el componente `InputReader`.
2. **Soporte de Fallback Autónomo:** Si el asset de configuración de inputs no está asignado en el Inspector, el componente genera dinámicamente un mapa de acciones en memoria (`Move` compuesto 2D y `Dash` con bindings para teclado y mando), garantizando que el juego sea siempre jugable en cualquier escena de prueba.
3. **Normalización Vectorial:** La entrada de movimiento compuesta se normaliza en magnitud para evitar que desplazarse en diagonal proporcione mayor velocidad física.

---

## Reasignación y Persistencia

* **¿El jugador puede cambiar los controles?**  
  **Sí.** El sistema está diseñado para admitir reasignación en caliente mediante la API del New Input System (`InputAction.PerformInteractiveRebinding`).
* **Persistencia Local:**  
  Cualquier reasignación realizada por el usuario se serializa y guarda en el almacenamiento local del sistema mediante `PlayerPrefs` bajo la clave estandarizada:
  ```text
  Key: "Echoscrap_Controls_Overrides"
  ```
  Al iniciar la partida o recargar la escena, `InputReader` carga automáticamente la cadena JSON de overrides y la aplica al mapa de acciones antes del primer frame.

---

## Principios de control y retroalimentación

1. **Inmediatez y Responsividad (Baja Latencia):** La respuesta del movimiento tiene una aceleración alta (80 m/s²), transmitiendo una sensación instantánea de control sin inercias pesadas.
2. **Accesibilidad con una Mano:** Gracias a que la linterna primaria apunta automáticamente hacia la dirección del movimiento, el jugador puede jugar una partida completa utilizando exclusivamente una mano en el teclado (WASD + Espacio) o en el mando (Stick analógico + Botón A).
3. **Retroalimentación Visual Inmediata:**
   * **Al Moverse:** La orientación del cuerpo del personaje y el haz de luz giran con una velocidad de rotación suave (720°/s), proyectando en el suelo la zona exacta de cobertura.
   * **Al Dashear:** El personaje acelera instantáneamente a 22 m/s, acompañado de una ráfaga de movimiento y activación de invulnerabilidad.
   * **Al Recibir Daño:** Si el jugador es alcanzado fuera de los *i-frames*, el material parpadea en rojo/blanco durante 0.15s y se activa un periodo de gracia de 0.40s.

---

> **Navegación:** [[00 - Índice]] · ← [[03 - Sistemas]] · [[05 - Progresión]] →
