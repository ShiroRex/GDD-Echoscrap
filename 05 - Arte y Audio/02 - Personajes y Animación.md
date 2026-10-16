# Personajes y Animación

## Descripción de la sección

Esta sección define la identidad visual, el comportamiento y los requerimientos de animación para todos los elementos vivos y coleccionables dentro de _EchoScrap_.

Su propósito es establecer una guía clara para el área de arte y programación, asegurando que cada entidad (desde el protagonista hasta las entidades capturadas) mantenga coherencia visual, rendimiento óptimo en pantalla y una retroalimentación clara durante la experiencia de juego.

## Requisitos por personaje

| Personaje | Modelo / Sprite | Animaciones | VFX | Rol y Estado |
| :--- | :--- | :--- | :--- | :--- |
| **El Coleccionista (Protagonista)** | Sprite 2D en *pixel art* ($64 \times 64\text{ px}$) con vista en perspectiva 2.5D cenital a 60°. | • **Idle:** Reposar (8 frames)<br>• **Mover:** Caminar/Correr en 3 dir. (8 frames)<br>• **Apuntar:** Orientar linterna (autoapuntado 360°)<br>• **Daño:** Recibir impacto<br>• **Muerte:** Caída y desintegración (8 frames) | • Cono de luz de la linterna.<br>• Parpadeo en rojo/blanco (*Flash Shader*) al recibir daño.<br>• Destellos de energía al activar mejoras y habilidades. | • **Normal:** Movimiento libre ($8.0\text{ m/s}$) y *Dash* ($22.0\text{ m/s}$).<br>• **Invulnerable temporal:** $0.40\text{ s}$ de *i-frames* tras recibir daño.<br>• **Derrotado:** Control bloqueado al caer a $0\text{ HP}$. |
| **Gólgora (El Guardián — Jefe Final)** | Sprite 2D en *pixel art* (base $64 \times 64\text{ px}$ escalado a tamaño de jefe) con vista 2.5D a 60°. | • **Idle:** Reposar<br>• **Mover:** Avance pesado en 3 dir.<br>• **Entrada:** Emerger / onda de choque inicial (min 8:00)<br>• **Ataque:** Pisotón de magma ($5\text{m}$), bombardeo de lava e invocación de esbirros (cada $4.0\text{ s}$)<br>• **Derrota:** Desintegración | • Sacudida de cámara (*Screen Shake*).<br>• Círculos de advertencia y ondas de choque en el suelo.<br>• Proyectiles de lava y partículas de invocación. | • **Clímax (Min 8:00+):** Persecución implacable e inmunidad al retroceso.<br>• **Stats:** HP: $1500.0$ \| Daño: $30.0$ (contacto) / $40.0$ (pisotón) \| Vel: $3.2\text{ m/s}$.<br>• **Derrotado:** Concede la Victoria y habilita la Ruleta de Extracción. |
| **Enemigo Básico Residual** | Sprite 2D ($64 \times 64\text{ px}$) | • **Movimiento:** Avance continuo (4–8 frames)<br>• **Mover:** Caminar en 3 dir.<br>• **Daño:** Impacto<br>• **Muerte:** Disolución (soltar gema de 5 o 15 XP) | • Parpadeo blanco (*Flash Shader*) al recibir luz/daño.<br>• Partículas al morir.<br>• Impulso por retroceso en el 4.º pulso de la linterna. | • **Horda / Enjambre:** Persecución activa con separación (*Flocking*) y flanqueo.<br>• **Stats:** HP: $15.0$ \| Daño: $10.0$ \| Vel: $3.5\text{ m/s}$. |
| **El Interceptor** | Sprite 2D ($64 \times 64\text{ px}$) | • **Movimiento:** Carrera ágil (4–8 frames)<br>• **Mover:** Correr en 3 dir.<br>• **Daño:** Impacto con retroceso<br>• **Muerte:** Disolución (soltar gema de 5 o 15 XP) | • Parpadeo blanco (*Flash Shader*) al recibir daño.<br>• Estela ligera de velocidad.<br>• Empujable por el 4.º pulso de la linterna. | • **Corta-caminos / Rusher:** Predicción de trayectoria (*Lead Target*, $0.4\text{ s}$).<br>• **Stats:** HP: $7.0\text{–}9.0$ \| Daño: $8.0$ \| Vel: $6.2\text{ m/s}$. |
| **El Coloso** | Sprite 2D ($64 \times 64\text{ px}$) | • **Movimiento:** Pasos pesados<br>• **Daño:** Sin interrupción<br>• **Muerte:** Disolución (soltar gema de 50 XP) | • Partículas de polvo en pisadas.<br>• Inmunidad al retroceso ($0\text{ m/s}$). | • **Rompe-líneas / Tanque:** Avance implacable sin repulsión; bloquea el paso libre.<br>• **Stats:** HP: $80.0$ \| Daño: $25.0$ \| Vel: $1.8\text{ m/s}$. |
| **El Mortero** | Sprite 2D ($64 \times 64\text{ px}$) | • **Movimiento:** Flotar tenue<br>• **Ataque:** Escupir proyectil<br>• **Muerte:** Disolución (soltar gema de 15 o 50 XP) | • Proyectil parabólico.<br>• Instancia de charco corrosivo en suelo ($4.0\text{ s}$ activo). | • **Zoner / Control de Arena:** Mantiene distancia de $7.0\text{ m}$ y ataca a distancia.<br>• **Stats:** HP: $25.0$ \| Daño: $5.0$ por tick (charco) \| Vel: $2.2\text{ m/s}$. |
| **La Polilla** | Sprite 2D ($64 \times 64\text{ px}$) | • **Movimiento:** Vuelo kamikaze<br>• **Cebado:** Sobrecarga / Parpadeo<br>• **Muerte:** Detonación (soltar gema de 5 o 15 XP) | • Parpadeo ámbar a alta frecuencia.<br>• Explosión radial de $3.0\text{ m}$ (*Friendly Fire*). | • **Kamikaze / Bomba:** Fusible de $0.5\text{ s}$ al acercarse o morir; empujable con la linterna.<br>• **Stats:** HP: $12.0$ \| Daño: $35.0$ \| Vel: $4.8\text{ m/s}$. |
| **9 Entidades del Almacén** *(Coleccionables)* | Sprite 2D ($64 \times 64\text{ px}$) | • **Idle Vitrina:** Flotar / Respirar en loop (2–4 frames) | • Marcos diferenciados por rareza y destello especial en la entidad Mítica. | • **Meta-Progresión y Lore:** 8 entidades normales (distribuidas en Común, Raro, Épico y Legendario) y 1 entidad Mítica (14 en visión completa). Otorgan pasivas permanentes al equipar **hasta 3 entidades por partida** en el Almacén y suben **+1 nivel** por duplicado en la Ruleta de Extracción. |

> **Nota sobre NPCs:** No existen NPCs ni comerciantes dentro del Almacén ni en la arena; toda la gestión de vitrinas, lectura de fichas de lore y equipamiento de hasta 3 entidades se realiza directamente desde la interfaz del Almacén.

## Animaciones

### Movimiento

- **Idle (Reposo):**
  - **Estructura y Frames:** Ciclo corto de 8 *frames* con una velocidad de reproducción cadenciada.
  - **Comportamiento Visual:** Muestra una sutil respiración del torso del Coleccionista y un parpadeo leve en el haz de la linterna. Su propósito es evitar la sensación estática cuando el jugador detiene temporalmente la marcha para posicionarse.

- **Caminar / Correr:**
  - **Frecuencia:** Loop continuo (8 *frames*).
  - **Comportamiento:** Ciclo de pasos en perspectiva 2.5D cenital a 60° optimizado en 3 direcciones (con volteo horizontal para cubrir izquierda/derecha). En *EchoScrap*, la velocidad de movimiento determina la aceleración del ciclo de *frames*.

### Interacción

- **Absorción / Succión de Recursos en Arena (In-Game):**
  - **Estructura y Frames:** Animación de desplazamiento y partículas en el objeto recolectable.
  - **Comportamiento Visual:** Al entrar en el radio de recolección magnética ($2.5\text{ m}$ base) del Coleccionista, los **Ecos (Gemas de XP)** y los **Botiquines de salud** aceleran velozmente ($12.0\text{ m/s}$) hacia el centro del personaje siguiendo una curva de succión suave.

- **Navegación e Interacción de Menús (Almacén / UI):**
  - **Comportamiento Visual:** La interacción con el Almacén se realiza íntegramente a través de la interfaz de usuario. Los iconos y vitrinas reaccionan a los estados de enfoque/selección (resaltado de borde al navegar con teclado, mando o ratón) y confirmación (pequeño rebote de escala y destello del marco al equipar una de las 3 entidades activas).

### Combate

- **Proyección de Ataque del Jugador (Linterna y Habilidades):**
  - **Acción Autónoma:** Las habilidades secundarias obtenidas por cartas muestran destellos luminosos o micro-impulsos de 1 a 2 *frames*, dejando que los efectos visuales (VFX) de los proyectiles, ondas o sierras orbitales guíen la atención del jugador.
  - **Comportamiento Visual de la Linterna:** La linterna funciona como un cono de luz independiente que **apunta de forma autónoma en 360° hacia el enemigo vivo más cercano dentro de su alcance** ($7.0\text{ m}$ base), y solo se alinea con la dirección de avance cuando no hay enemigos dentro de ese radio.

- **Ataques y Alertas Enemigas:**
  - **Ataques por Contacto:** Los enemigos de presión directa (como el Básico, el Interceptor o el Coloso) se desplazan hacia el jugador; su propia animación de avance representa la amenaza de daño por contacto.
  - **Alertas de Ataques Especiales:** Los enemigos con mecánicas de área (como el Mortero, la Polilla o Gólgora) incluyen animaciones claras de anticipación. Por ejemplo, la Polilla entra en una vibración errática acelerada con un parpadeo ámbar brillante durante $0.5\text{ s}$ antes de estallar, dando una señal inequívoca para que el jugador esquive con *Dash* o la empuje con el cuarto pulso de la linterna.

### Daño

- **Daño Recibido por el Jugador:**
  - **Efecto Visual:** En lugar de una animación de aturdimiento que detenga la caminata, el *sprite* del Coleccionista parpadea velozmente en color rojo/blanco durante sus $0.40\text{ s}$ de invulnerabilidad temporal (*i-frames*).
  - **Respuesta Kinestésica:** El personaje conserva el control total de su desplazamiento mientras parpadea para poder escapar del cerco de la horda.

- **Impactos en Enemigos (Luz y Habilidades):**
  - **Efecto Visual:** Cada vez que un pulso del cono de luz o una habilidad daña a un enemigo, su *sprite* parpadea instantáneamente en blanco a la frecuencia del impacto.
  - **Físicas de Empuje (*Knockback*):** Al recibir el cuarto pulso de la linterna, los enemigos vulnerables al empuje son desplazados físicamente hacia atrás sobre el plano de la arena (mientras que el Coloso y Gólgora conservan su avance por ser inmunes al retroceso).

### Muerte / Derrota

- **Disolución y Derrota de Enemigos:**
  - **Secuencia Visual:** Cuando la vida de un enemigo llega a cero, su animación de movimiento se interrumpe de inmediato y el *sprite* ejecuta una breve secuencia de disolución en partículas.
  - **Transición a Recompensa:** Al desaparecer el enemigo, queda flotando en ese punto su gema de experiencia (Eco) o, según probabilidad, un botiquín de salud.

- **Colapso y Muerte del Coleccionista:**
  - **Secuencia Visual:** Al agotarse la vida del jugador ($0\text{ HP}$), se bloquea el control de movimiento y se ejecuta una animación de caída en seco (8 *frames*, sin bucle).
  - **Efectos de Cierre:** El traje del Coleccionista se desploma, la luz de la linterna parpadea hasta apagarse y el personaje se desvanece en partículas residuales. Tras esta animación, la partida concluye mostrando la **pantalla de resultados de Derrota** para regresar al Almacén con las manos vacías (mientras que la **Ruleta de Extracción** se reserva exclusivamente para el estado de **Victoria** al derrotar a Gólgora).

> **Navegación:** [[00 - Índice]] · ← [[01 - Dirección de Arte]] · [[03 - Entornos]] →
