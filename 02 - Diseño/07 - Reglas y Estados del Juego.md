# Reglas y Estados del Juego

## Descripción de la sección

Define las reglas globales de funcionamiento del juego y la máquina de estados por la que transita el ciclo de ejecución en Unity.

---

## Estados principales del juego

* **1. Menú Principal (Meta / Almacén):** Estado fuera de partida donde el jugador gestiona sus entidades, revisa estadísticas y prepara la siguiente expedición.
* **2. Jugando (In-Game / Arena Activa):** Simulación en tiempo real (`Time.timeScale = 1.0f`). El jugador se desplaza, la linterna emite pulsos de daño, el spawner invoca oleadas y los enemigos persiguen y flanquean. El cursor está bloqueado y oculto.
* **3. Subida de Nivel / Pausa Táctica (`LevelUpModal`):** El tiempo del juego se detiene por completo (`Time.timeScale = 0.0f`). El cursor se libera y se muestra el modal con las 3 cartas de mejora disponibles.
* **4. Derrota (Game Over):** La salud del jugador desciende a 0. Se detiene el combate, se presenta el resumen de tiempo y chatarra recolectada y se permite reiniciar la escena o regresar al menú.
* **5. Victoria (Extracción Exitosa):** El jugador sobrevive al tiempo total de la run (8 a 10 minutos) y derrota al jefe final *Gólgora*, permitiendo conservar los Ecos más valiosos.

---

## Matriz de transiciones de estados

| Estado Origen | Evento Desencadenante | Estado Destino | Condición Técnica |
|---|---|---|---|
| **Menú Principal** | Pulsar "Comenzar Partida" | **Jugando** | Carga de la escena de arena (`Test_PlayerGameplay.unity`). |
| **Jugando** | XP alcanza el umbral de nivel | **Subida de Nivel** | `currentXP >= xpToNextLevel` dispara `OnLevelUpEvent`. `Time.timeScale = 0f`. |
| **Subida de Nivel** | El jugador selecciona 1 mejora | **Jugando** (o siguiente nivel) | Se procesa `SelectOption()`. Si `pendingLevelUps == 0`, `Time.timeScale = 1f`. |
| **Jugando** | Salud del jugador llega a cero | **Derrota** | `playerHealth.CurrentHealth <= 0`. Se congela la partida o se recarga escena. |
| **Jugando** | Cronómetro alcanza meta y cae Gólgora | **Victoria** | Tiempo transcurrido >= duración de partida y jefe final derrotado. |

---

## Reglas globales de implementación

1. **Restricción Cinemática 2.5D:**  
   Todos los cálculos físicos de movimiento, colisiones, empuje y distancia se calculan estrictamente sobre el plano **XZ**. La componente **Y** se mantiene constante o se resetea a nivel del piso en el Rigidbody (`Vector3(v.x, 0, v.z)`) para evitar que enemigos o jugadores salgan despedidos hacia arriba tras impactos repetidos.
2. **Filosofía de Memoria Zero GC en Combate:**  
   Queda terminantemente prohibido utilizar llamadas que generen alocaciones dinámicas temporales en el *Heap* durante los bucles de `Update` o `FixedUpdate`:
   * Se prohíbe `Physics.OverlapSphere` estándar; se exige el uso obligatorio de variantes con búfer preasignado (`Physics.OverlapSphereNonAlloc` con arrays estáticos compartidos).
   * Se prohíbe el uso de cadenas de texto dinámicas con concatenación (`+`) en bucles rápidos; se utiliza `TMP_Text.SetText()` con sobrecargas numéricas directas para evitar recolección de basura.
3. **Tope de Seguridad de Horda (FPS Safeguard):**  
   El spawner respeta un límite estricto de **150 enemigos simultáneos activos**. Si se alcanza este tope, el generador suspende nuevos nacimientos hasta que la población baje, asegurando 60 FPS estables sin caídas de framerate en equipos de especificación media.
4. **Prioridad Absoluta de I-Frames:**  
   Cuando la bandera `IsInvulnerable` del jugador está activa (por activación del Dash o por periodo de gracia post-daño de 0.40s), cualquier colisión con enemigos o proyectiles es ignorada a nivel de daño.

---

## Casos especiales

* **Acumulación de Múltiples Subidas de Nivel:**  
  Si el jugador absorbe un cúmulo denso de gemas que le otorga suficiente XP para subir 2 o más niveles de golpe, el sistema almacena las subidas pendientes en un contador entero (`pendingLevelUps`). El modal no reanuda el juego al elegir la primera mejora; en su lugar, descuenta 1 punto al contador y refresca inmediatamente las opciones para el siguiente nivel, reanudando la partida (`Time.timeScale = 1.0f`) únicamente cuando la cola de niveles pendientes llega a cero.
* **Daño durante el estado de pausa:**  
  Dado que `Time.timeScale` se sitúa en 0.0f y los enemigos operan en base a `Time.deltaTime` o físicas continuas, ningún enemigo puede desplazarse, colisionar ni infligir daño mientras el modal de mejoras esté en pantalla.

---

> **Navegación:** [[00 - Índice]] · ← [[06 - Economía y Recursos]] · [[01 - Premisa y Trama]] →
