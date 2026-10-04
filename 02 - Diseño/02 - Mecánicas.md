# Mecánicas

## Descripción de la sección

Una mecánica es una regla o interacción mediante la cual el jugador puede actuar dentro del sistema del juego.

En esta sección se documentan las mecánicas actualmente implementadas y operativas en el motor del juego (*EchoScrap*), detallando sus reglas cuantitativas, entradas y estados para garantizar coherencia en el equipo.

---

## Mecánicas principales

### Mecánica Principal 01: Locomoción en Plano 2.5D (XZ)

**Propósito:**  
Proporcionar un control responsivo y fluido del Coleccionista en el plano horizontal (XZ), permitiendo esquivar enemigos y orientar el ataque frontal hacia el enjambre.

**Activación:**  
Mover el stick analógico izquierdo o pulsar las teclas de dirección (WASD).

**Reglas:**
* El personaje se desplaza exclusivamente en los ejes X y Z. La altura Y permanece fija a nivel del suelo.
* Velocidad máxima base: **8.0 m/s** (modificable dinámicamente mediante `PlayerStats`).
* Aceleración progresiva: **80 m/s²** para respuesta rápida sin arranque instantáneo rígido.
* Desaceleración: **70 m/s²** al soltar las entradas, evitando patinaje excesivo.
* Rotación direccional: **720°/s** de rotación suave mirando hacia el vector de desplazamiento actual.
* Si el jugador se detiene, conserva su última dirección de orientación (`lastMoveDirection`).

**Entrada:**  
Vector2 compuesto mediante `InputAction` (`Move`: Teclado WASD / Flechas, Gamepad Stick Izquierdo o D-Pad).

**Estado inicial:**  
Personaje inmóvil 

**Estado final:**  
Desplazamiento a la velocidad resultante de la entrada y orientación alineada al vector de avance.

**Recursos utilizados:**  
Ninguno. Movimiento libre sin barra de estamina.

**Recompensa:**  
Libertad táctica para abrir espacio, bordear enjambres y dirigir el ataque.

**Penalización:**  
Detenerse o quedar atrapado entre enemigos resulta en daño por contacto continuo.

**Interacciones con otras mecánicas:**
* **Cono de Linterna:** El foco de luz y su malla de daño están montados en el frente del personaje y rotan automáticamente con el vector de locomoción.
* **Dash:** El dash toma como vector preferente la dirección del movimiento actual. Si el jugador está detenido, utiliza `lastMoveDirection`.

**Casos límite:**  
* Entrada diagonal en teclado: Normalizada matemáticamente para evitar que el avance en 45° otorgue un 41% extra de velocidad.
* Colisión contra bordes o enemigos: La velocidad se desliza a lo largo del plano de colisión (`Rigidbody.linearVelocity` administrado con físicas cinemáticas).

---

### Mecánica Principal 02: Impulso Evasivo / Dash con I-Frames

**Propósito:**  
Ofrecer una herramienta reactiva de emergencia para romper cercos, atravesar filas de enemigos densas y reposicionarse agresivamente sin sufrir penalización de daño.

**Activación:**  
Pulsar el botón o tecla de esquiva asignada.

**Reglas:**
* Desplaza al jugador a una velocidad fija de **22.0 m/s** durante una ventana breve de **0.15 segundos**.
* Durante los 0.15s completos del dash, el jugador adquiere **invulnerabilidad total (*i-frames*)** frente a daño por contacto de enemigos (`IsInvulnerable = true`).
* Enfriamiento (*Cooldown*): **1.0 segundo** a partir del momento de activación.
* Si el jugador se está moviendo, el impulso se ejecuta en la dirección del movimiento actual. Si está quieto, se impulsa hacia donde apunta el personaje.
* Dispara eventos de inicio y fin (`OnDashStarted`, `OnDashEnded`) para sincronizar efectos visuales (*ghosting/afterimages*) y audio.

**Entrada:**  
Botón digital (`Dash`: Teclado `Espacio`, Gamepad Botón Sur / `A` en Xbox, `Cross` en PlayStation, o Gatillos).

**Estado inicial:**  
Dash disponible (`canDash == true` e `isDashing == false`).

**Estado final:**  
Jugador reposicionado ~3.3 metros más adelante; habilidad en enfriamiento durante 1.0s.

**Recursos utilizados:**  
Temporizador de enfriamiento interno (1.0s).

**Recompensa:**  
Ruptura inmediata de bloqueos de horda y evasión garantizada de daño.

**Penalización:**  
Quedar vulnerable durante el segundo de recarga si se usa el dash en una dirección desfavorable o contra una pared.

**Interacciones con otras mecánicas:**
* **Daño por Contacto:** Los enemigos que tocan al jugador durante el dash no infligen daño ni interrumpen la trayectoria.
* **Linterna Frontal:** La linterna continúa activa durante el dash, barriendo a los enemigos a lo largo de la trayectoria de avance.

**Casos límite:**  
* Intentar dashear durante un dash previo: Bloqueado por bandera booleana.
* Dashear contra obstáculos estáticos: Las físicas del Rigidbody impiden atravesar geometría sólida de la arena.

---

### Mecánica Principal 03: Linterna Frontal Cónica (Arma Primaria)

**Propósito:**  
Constituye el método de ataque principal del Coleccionista. Aplica daño continuo en área y empuja a los enemigos en un arco frontal obligatorio hacia la dirección de avance, premiando el encarar la horda.

**Activación:**  
Pasiva y automática continua mientras el jugador se desplaza y orienta en la arena.

**Reglas:**
* Proyecta un sector cónico de **60° de apertura angular** y un alcance longitudinal de **6.0 metros**.
* Ritmo de daño (*Tick Rate*): Emite pulsos de daño cada **0.20 segundos** (5 impactos por segundo).
* Daño base por pulso: **5.0 puntos de daño** por impacto (DPS base de 25.0).
* Detección geométrica: Identifica enemigos dentro del cono sin generar basura en memoria (*Zero GC*).
* Control de masas (*Knockback*): Cada tick que impacta a un enemigo le transfiere un impulso de retroceso físico alejándolo de la posición del jugador.
* Representación visual dual:
  1. *Luz Spot URP:* Fuente de luz direccional (`LightType.Spot`) inclinada hacia el piso (16° de caída) sin sombras para maximizar el rendimiento.
  2. *Malla Procedural de Suelo:* Generación de un cono plano de 24 segmentos con material translúcido aditivo y contorno nítido (`LineRenderer`), indicando exactamente el área de impacto.

**Entrada:**  
No requiere botón de disparo; se orienta mediante el vector de avance del jugador.

**Estado inicial:**  
Foco activo barriendo continuamente el plano frontal.

**Estado final:**  
Enemigos alcanzados reciben daño, parpadean en blanco/rojo y son empujados hacia atrás; los eliminados sueltan gemas.

**Recursos utilizados:**  
Temporizador interno de tick (0.20s).

**Recompensa:**  
Apertura de pasillos seguros en el enjambre y generación continua de Chatarra/XP.

**Penalización:**  
Ninguna directa; sin embargo, no cubre los flancos ni la espalda del personaje.

**Interacciones con otras mecánicas:**
* **Subida de Nivel:** Sus parámetros se potencian directamente a través del modal de mejoras:
  * *Mejora de Daño:* +3 de daño por pulso.
  * *Mejora de Cobertura:* +15° de ángulo de cono y +1.0m de alcance adicional.
* **Knockback de Enemigos:** Se acopla a la desaceleración por fricción del `EnemyBasic`.

**Casos límite:**  
* Enemigos a espaldas del jugador: No reciben daño ni knockback; obligan al jugador a girar o usar el dash.
* Múltiples enemigos apilados en el cono: Todos los enemigos dentro del volumen reciben daño simultáneamente (ataque perforante total).

---

## Mecánicas secundarias

### Mecánica Secundaria 01: IA de Horda y Flanqueo Dinámico (Enjambre)

**Propósito:**  
Garantizar que la horda enemiga ejerza una presión envolvente orgánica y amenazante, evitando que se formen filas indias rígidas y fáciles de esquivar.

**Reglas:**
* **Persecución directa:** Cada enemigo calcula el vector hacia la posición del jugador en el plano XZ.
* **Variación individual de velocidad:** Velocidad base de 3.5 m/s con una fluctuación aleatoria individual de **±15%** asignada al nacer, rompiendo la sincronía de movimiento.
* **Separación de vecinos (*Flocking Separation*):** Radio de separación de 1.4m con fuerza de 5.5. Los enemigos se repelen mutuamente mediante un búfer estático preasignado (`neighborBuffer[16]`) libre de recolección de basura (*Zero GC*).
* **Fuerza de flanqueo lateral:** Si un enemigo se encuentra bloqueado de frente por otro, aplica una fuerza tangencial (fuerza 4.0) para abrirse hacia los costados y rodear al jugador.
* **Micro-ondulación errática:** Incorpora una semilla sinusoidal (`wanderIntensity = 0.35`) que produce pequeños desvíos orgánicos en la marcha.
* **Daño por contacto:** Inflige **10.0 puntos de daño** al jugador con un intervalo de enfriamiento de **0.50 segundos** por enemigo.

---

### Mecánica Secundaria 02: Magnetismo y Recolección de Chatarra (Gemas de XP)

**Propósito:**  
Recompensar al jugador por avanzar sobre los terrenos recién despejados y consolidar el ciclo de progreso continuo de la partida.

**Reglas:**
* Cada enemigo básico derrotado instancia una Gema de XP (`Gem_XP.prefab`) orientada hacia la cámara 2.5D (rotación fija `55°, 0°, 45°`).
* **Valor de recurso:** Cada gema otorga **10.0 puntos de XP** al ser recogida.
* **Radio de detección base:** **2.5 metros**. Mientras el jugador se encuentre fuera de este radio, la gema permanece estática en el suelo.
* **Atracción activa:** Al entrar en el radio magnético, la gema adquiere estado de atracción y vuela directamente hacia la coordenada del jugador a una velocidad de **12.0 m/s**.
* **Consumo:** Al situarse a menos de **0.35 metros**, la gema es absorbida, añade su valor a `PlayerExperience` y se destruye inmediatamente.
* **Ampliación de radio:** La mejora de imán en el modal de subida de nivel incrementa el radio magnético en **+1.5 metros** de forma permanente durante la partida.

---

### Mecánica Secundaria 03: Subida de Nivel y Selección de Mejoras (Draft Modal)

**Propósito:**  
Introducir toma de decisiones estratégicas y variedad de *builds* en tiempo real, ofreciendo una recompensa inmediata al esfuerzo de recolección.

**Reglas:**
* La subida de nivel se activa automáticamente cuando `currentXP >= xpToNextLevel`.
* **Curva de requerimiento:** El umbral inicial es de **100 XP** (10 gemas) y escala multiplicándose por **1.5x** en cada nivel subsiguiente (`Mathf.Round(xpToNextLevel * 1.5f)`).
* **Congelación del tiempo:** Al subir de nivel se ejecuta `Time.timeScale = 0f`. La simulación de físicas, enemigos y armas se pausa por completo; el cursor se desbloquea.
* **Draft de 3 opciones:** El sistema despliega un panel interactivo con 3 cartas de mejora disponibles:
  1. *Opción A (Potencia de Fuego):* +3 daño por impacto en la linterna.
  2. *Opción B (Cobertura Visual):* +15° de ángulo y +1.0m de alcance en el cono.
  3. *Opción C (Imán de Chatarra):* +1.5m al radio de atracción de gemas de XP.
* **Gestión de subidas acumuladas:** Si una gran absorción de gemas otorga múltiples niveles consecutivamente, el sistema procesa una cola interna (`pendingLevelUps`), presentando un nuevo draft por cada nivel ganado antes de reanudar el juego (`Time.timeScale = 1f`).

---

> **Navegación:** [[00 - Índice]] · ← [[01 - Jugabilidad]] · [[03 - Sistemas]] →
