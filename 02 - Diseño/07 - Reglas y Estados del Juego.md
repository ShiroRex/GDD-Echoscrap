# Reglas y Estados del Juego

## Descripción de la sección

Define las reglas globales de funcionamiento del juego y la máquina de estados por la que transita el ciclo de ejecución en Unity.

---

## Estados principales del juego

* **1. Menú Principal (Meta / Almacén):** Estado fuera de partida donde el jugador gestiona sus entidades, revisa estadísticas y prepara la siguiente expedición.
* **2. Jugando (In-Game / Arena Activa):** Simulación en tiempo real. El jugador se desplaza, la linterna emite pulsos de daño, el spawner invoca oleadas y los enemigos persiguen y flanquean. El cursor está bloqueado y oculto.
* **3. Subida de Nivel / Pausa Táctica:** Se detiene el tiempo de juego y se muestran hasta tres cartas elegibles. Con menos candidatas se muestran menos; con ninguna, se informa y se permite continuar según [[05 - Progresión]].
* **4. Derrota (Game Over):** La salud del jugador desciende a 0. Se detiene el combate, se presenta el resumen de tiempo y chatarra recolectada y se permite reiniciar la partida o regresar al menú.
* **5. Victoria (Extracción Exitosa):** El jugador sobrevive a los 8 minutos de combate y derrota al jefe final *Gólgora*, activando la Ruleta de Extracción para capturar una entidad permanente.

---

## Matriz de transiciones de estados

| Estado Origen | Evento Desencadenante | Estado Destino | Condición de Diseño |
|---|---|---|---|
| **Menú Principal** | Pulsar "Comenzar Partida" | **Jugando** | Carga de la escena de la arena de combate. |
| **Jugando** | XP alcanza el umbral de nivel | **Subida de Nivel** | La experiencia acumulada alcanza el umbral requerido. Se pausa la simulación táctica. |
| **Subida de Nivel** | Elegir una carta o continuar una oferta vacía | **Jugando** (o siguiente elección) | Resolver la elección, actualizar configuración y preparar otra si queda pendiente; reanudar sólo al terminar todas. |
| **Jugando** | Salud del jugador llega a cero | **Derrota** | La vida del jugador llega a 0. Se detiene el combate y se pasa a la pantalla de fin de partida. |
| **Jugando** | Salud de Gólgora llega a cero | **Victoria** | Gólgora es derrotado mientras el jugador conserva vida (> 0 HP). Se activa la Ruleta de Extracción. |
| **Jugando** | Presionar Escape / Botón Menú | **Menú de Pausa** | Se detiene la simulación temporalmente para consultar opciones o reanudar. |
| **Menú de Pausa** | Reanudar partida | **Jugando** | Se restaura la simulación en tiempo real. |

---

## Reglas globales de diseño técnico

1. **Restricción Cinemática 2.5D:**  
   Todos los cálculos físicos de movimiento, colisiones, empuje y distancia se calculan estrictamente sobre el plano **XZ**. La componente **Y** se mantiene constante o nivelada al piso para evitar que enemigos o jugadores salgan despedidos hacia arriba tras impactos repetidos.
2. **Optimización de Memoria en Combate:**  
   Se diseñará para evitar alocaciones dinámicas continuas en memoria durante el combate activo, reutilizando estructuras y búferes preasignados para consultas de área y detección de amenazas.
3. **Población de Horda y Rendimiento:**  
   El spawner mantiene una densidad de combate continua de hasta **150 enemigos simultáneos activos**, con margen para escalar hasta **250 enemigos en momentos de máximo estrés u oleadas pico** (estándar del género *Horde Survival* / *Bullet Heaven*), recurriendo a reutilización de entidades (*pooling*) para garantizar una tasa fluida de 60 FPS.
4. **Prioridad Absoluta de I-Frames:**  
   Cuando la condición de invulnerabilidad del jugador está activa (por activación del Dash o por periodo de gracia post-daño), cualquier colisión con enemigos o proyectiles es ignorada a nivel de daño.

---

## Casos especiales

* **Resolución de Muerte Simultánea (Coleccionista vs Gólgora):**  
  Si la salud del Coleccionista y la salud de Gólgora descienden a 0 en el mismo instante o fotograma, la prioridad de resolución se otorga estrictamente a la **Derrota**. La justificación de diseño es que un recolector caído en combate no puede activar la Ruleta ni realizar la extracción de entidades al Almacén.
* **Acumulación de Múltiples Subidas de Nivel:**  
  Si se ganan varias elecciones de subida de nivel simultáneas, se resuelven una por una sin reanudar la acción entre ellas. Después de cada elección se recalcula la elegibilidad; si no quedan opciones disponibles, se informa y se permite continuar la partida.
* **Daño durante el estado de pausa:**  
  Durante la pausa táctica (subida de nivel o menú de pausa), la simulación se congela por completo; ningún enemigo puede desplazarse, atacar ni causar daño mientras el modal esté activo.

---

> **Navegación:** [[00 - Índice]] · ← [[06 - Economía y Recursos]] · [[01 - Premisa y Trama]] →
