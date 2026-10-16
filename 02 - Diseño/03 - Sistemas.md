# Sistemas del Juego

## Propósito y Visión General

En esta sección se definen los **sistemas funcionales que componen la arquitectura de juego de EchoScrap**, detallando su propósito dentro del diseño, sus responsabilidades e interacciones mutuas. 

Como el proyecto se encuentra en etapa de diseño y preproducción previa al desarrollo formal, los sistemas aquí planteados establecen las bases lógicas y operativas con las que se construirá la experiencia jugable.

---

## Inventario de Sistemas

| Sistema | Función en el Diseño | Relaciones Principales |
|---|---|---|
| **Entradas y Controles** | Capturar las órdenes del jugador (teclado, ratón o mando) de forma desacoplada y enviarlas a los módulos de acción. | Movimiento, dash, interfaz de draft y menús. |
| **Locomoción y Navegación** | Gestionar la física de movimiento del Coleccionista en el plano XZ y la orientación de su modelo visual. | Controles, colisiones de la arena, cámara y modificadores de velocidad. |
| **Evasión (Dash)** | Proveer una maniobra de escape rápido con aceleración instantánea, ventana de invulnerabilidad (*i-frames*) y recarga. | Controles, locomoción, atributos del jugador y mitigación de daño. |
| **Linterna y Autoapuntado** | Rastrear de forma autónoma al enemigo más cercano dentro del cono frontal y emitir ráfagas de 4 pulsos con retroceso físico en el último pulso. | Enemigos en rango, atributos de alcance/daño, efectos de cartas y audio. |
| **Atributos y Modificadores** | Centralizar las estadísticas base del personaje, integrando los bonos permanentes de las entidades del Almacén y las cartas obtenidas en la run. | Salud, velocidad, daño de linterna, radio magnético y cadencia. |
| **Salud, Daño y Mitigación** | Procesar impactos enemigos, ventanas de invulnerabilidad post-daño, curaciones mediante botiquines y resolución de vida cero. | Enemigos, ataques, dash, botiquines y estados de partida. |
| **Inteligencia de la Horda** | Coordinar los comportamientos de los enemigos: persecución directa, separación física para evitar amontonamiento y flanqueo lateral. | Coleccionista, colisiones entre enemigos, daño por contacto y drops. |
| **Generador de Oleadas (Spawner)** | Modular el flujo, la cadencia de aparición y la composición de enemigos a través de las 5 fases temporales de la arena (0:00 a 8:00+). | Tiempo transcurrido, límite de población (150/250 enemigos) y aparición de Gólgora. |
| **Economía y Magnetismo** | Administrar la generación en suelo de gemas de XP y botiquines de reparación, así como su atracción acelerada al entrar en el radio magnético. | Muerte de enemigos, posición del jugador, radio de imán y subida de nivel. |
| **Experiencia y Niveles** | Contabilizar la XP recogida y evaluar el umbral requerido para detonar la subida de nivel (escalado $\times 1.5$). | Recolección de gemas, interfaz de subida de nivel y progreso de la partida. |
| **Draft y Selección de Cartas** | Pausar la simulación táctica, filtrar candidatos válidos del catálogo de 35 cartas y presentar 3 opciones aleatorias ponderadas por rareza. | Catálogo de cartas, requisitos de sinergia, sistema de re-rolls y atributos. |
| **Cámara Cenital 2.5D** | Mantener un encuadre fluido que sigue al Coleccionista con ligera anticipación, ofreciendo lectura clara de las amenazas y los límites de la arena. | Posición del jugador, presentación visual y accesibilidad. |
| **Interfaz y HUD** | Comunicar en tiempo real la salud actual, barra de experiencia, tiempo transcurrido, cartas activas y alertas de peligro. | Salud, XP, temporizador de fases y retroalimentación audiovisual. |
| **Gestión de Estados de Partida** | Coordinar el flujo global: Menú Principal, Arena Activa, Pausa Táctica, Invocación de Gólgora, Derrota y Victoria. | Salud del jugador, salud del jefe, temporizador y Ruleta de Extracción. |
| **Ruleta de Extracción y Colección** | Al derrotar a Gólgora, activar una ruleta que selecciona aleatoriamente una entidad dimensional para añadirla a la colección permanente (o sumarle +1 nivel si es duplicada). | Victoria, base de datos del Almacén y metajuego. |
| **Gestión del Almacén (Hub)** | Permitir al jugador inspeccionar su colección de entidades capturadas, leer su trasfondo y equipar hasta 3 entidades acompañantes para la siguiente expedición. | Menú principal, colección persistente y atributos base. |
| **Persistencia y Guardado** | Almacenar localmente en formato estructurado (JSON con respaldo) el catálogo de entidades desbloqueadas y sus niveles, ajustes de usuario y configuraciones. | Colección del Almacén, menú de opciones e integridad de datos. |

---

## Responsabilidades y Reglas de Integración

### 1. Atributos: Equipamiento Permanente vs Mejoras de Run
El diseño establece una clara separación entre dos capas de progresión:
* **Capa Permanente (El Almacén):** Antes de iniciar la expedición, el jugador equipa hasta **3 entidades** de su colección dimensional. Estas aportan habilidades pasivas o ajustes iniciales a los atributos base del Coleccionista, los cuales se potencian cuando la entidad sube de nivel al obtener duplicados.
* **Capa Volátil (*In-Run*):** Durante los 8 minutos de partida, cada subida de nivel permite incorporar cartas del catálogo de 35 mejoras. Estas cartas modifican la configuración de combate exclusivamente durante la partida en curso y se reinician al terminar.

### 2. Combate, Daño y Retroalimentación
* El Coleccionista ataca mediante la linterna con autoapuntado, emitiendo ráfagas de 4 pulsos. El 4.º impacto aplica retroceso físico a los enemigos básicos, actuando como herramienta de contención y control de masas.
* Los enemigos aplican daño por contacto (10 puntos base) con un intervalo de gracia de 0.5s entre golpes.
* Si la salud del jugador llega a cero, el sistema declara inmediatamente la **Derrota**. Si Gólgora es derrotado mientras el jugador sobrevive, se declara la **Victoria**. En caso de muerte simultánea en el mismo instante, la prioridad de resolución se otorga a la Derrota.

### 3. Presión Temporal y Curva de Dificultad
* La partida se estructura en **5 fases canónicas** que van desde el reconocimiento inicial (Fase 1: 0:00-1:00) hasta la aparición del jefe final (Fase 5: 8:00+).
* El sistema de generación de oleadas mantiene una densidad estándar de hasta **150 enemigos simultáneos**, con capacidad de soportar hasta **250 enemigos en momentos de máxima intensidad**, recurriendo a sistemas de reutilización (*pooling*) para preservar una tasa óptima de fotogramas.

### 4. Victoria, Ruleta y Almacén
* Al vencer al jefe Gólgora, se activa la **Ruleta de Extracción**, una ceremonia de recompensa que selecciona al azar una entidad dimensional superviviente de la dimensión colapsada. Si la entidad ya formaba parte de la colección, el duplicado le suma **+1 nivel**, potenciando su aporte pasivo.
* La entidad obtenida se almacena de forma permanente en el Almacén (el Hub del juego), donde el jugador puede leer su micro-historia, contemplar su sprite e incorporarla en una de las **3 ranuras de equipamiento** para futuras partidas.

---

## Flujos de Información Previstos

| Evento o Entrada | Sistema Responsable | Resultado de Diseño |
|---|---|---|
| **Selección en el Almacén** | Almacén y Atributos | Cargar las hasta 3 entidades equipadas y aplicar sus bonificadores (según su nivel) a las estadísticas iniciales del Coleccionista. |
| **Entrada de movimiento o dash** | Controles y Locomoción | Desplazar al personaje en el plano XZ o ejecutar el impulso de evasión con invulnerabilidad temporal. |
| **Contacto enemigo / Ataque** | Salud y Mitigación | Verificar si el jugador está en *i-frames*. Si no lo está, descontar salud, activar periodo de gracia y reflejar el daño en el HUD. |
| **Destrucción de un enemigo** | Spawner y Recursos | Instanciar una gema de chatarra XP (100% de probabilidad) y evaluar la probabilidad de soltar un botiquín (4% básico, 25% pesado). |
| **Gema absorbida / Barra llena** | Experiencia y Draft | Congelar la simulación de juego y abrir la pantalla de draft con 3 cartas elegibles del catálogo. |
| **Elección de carta en el draft** | Draft y Atributos | Actualizar las estadísticas de la run (las mejoras de linterna se aplican al iniciar la siguiente ráfaga completa) y reanudar el combate. |
| **Cronómetro alcanza 8:00** | Spawner y Estados | Detener la escalada de oleadas normales, despejar la arena e invocar al jefe Gólgora (quien invocará esbirros periódicamente durante el combate). |
| **Derrota de Gólgora** | Estados y Ruleta | Declarar la Victoria, abrir la Ruleta de Extracción para capturar una entidad (o subirla +1 nivel si es duplicada) y registrarla en el Almacén persistente. |
| **Salud del jugador llega a 0** | Estados y HUD | Declarar la Derrota, mostrar estadísticas de supervivencia y permitir el retorno al Almacén sin recompensa de extracción. |

---

> **Navegación:** [[00 - Índice]] · ← [[02 - Mecánicas]] · [[04 - Controles]] →
