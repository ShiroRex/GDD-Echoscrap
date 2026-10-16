# Mecánicas

## Descripción de la sección

Una mecánica es una regla del juego que define **qué puede hacer el jugador, cómo responde el juego y qué resultados produce esa acción**.

Este documento está pensado como la **guía práctica de referencia para todo el equipo** (programadores, diseñadores de niveles y artistas). Explica con claridad cómo funcionarán los controles, las estadísticas base del personaje, el arma principal y el comportamiento de los enemigos en *EchoScrap*, usando un lenguaje directo, accesible y con ejemplos fáciles de entender.

---

## Estadísticas Base de El Coleccionista

Antes de aplicar las bonificaciones de las **3 entidades equipadas en el Almacén** o las cartas obtenidas durante la partida, el Coleccionista inicia con los siguientes **18 atributos fundamentales** y parámetros base de acción:

| # | Estadística / Atributo | Valor Base Inicial | Límite / Tope (*Cap*) | Función en el Juego |
|:---:|---|:---:|:---:|---|
| **1** | **Vida Máxima (HP)** | `100 HP` | Sin tope fijo | Salud total del personaje (cuenta con `0.40 s` de gracia invulnerable tras recibir un golpe). |
| **2** | **Daño Físico Adicional** | `0` | Sin tope fijo | Bonificador para impactos físicos, ondas de choque, minas y sierras orbitales. |
| **3** | **Daño Sobrenatural Adicional** | `0` | Sin tope fijo | Bonificador para el haz de luz de la linterna, chispas de plasma y pulsos elementales. |
| **4** | **Armadura** | `0%` | `60%` máx. | Porcentaje de reducción directa sobre el daño recibido. |
| **5** | **Regeneración de Vida** | `0 HP/s` | Sin tope fijo | Puntos de salud recuperados de forma pasiva cada segundo. |
| **6** | **Reducción de Enfriamiento** | `0%` | `50%` máx. | Reduce el tiempo de espera entre ráfagas de armas y habilidades automáticas. |
| **7** | **Recuperación de Dash** | `0%` | `100%` | Acelera la recarga del impulso evasivo (sobre su espera base de `1.0 s`). |
| **8** | **Probabilidad de Esquiva** | `0%` | `35%` máx. | Probabilidad de ignorar por completo el daño de un impacto enemigo. |
| **9** | **Probabilidad de Bloqueo** | `0%` | `50%` máx. | Probabilidad de mitigar un impacto recibido reduciendo su daño a la mitad (`50%`). |
| **10** | **Suerte** | `0%` | Sin tope fijo | Bonificador proporcional para caídas de botiquines, eventos aleatorios y rareza de opciones. |
| **11** | **Rango (Alcance)** | `+0%` (Base: `7.0 m`) | Sin tope fijo | Incrementa el alcance efectivo de la linterna y las habilidades con radio. |
| **12** | **Robo de Vida** | `0%` | `2% HP máx./s` | Fracción del daño elegible convertida en salud, con tope de seguridad por segundo. |
| **13** | **Probabilidad Crítica** | `0%` | `100%` máx. | Probabilidad de que un pulso o ataque inflija daño crítico. |
| **14** | **Multiplicador de Daño Crítico** | `×1.75` (`175%`) | Sin tope fijo | Factor por el cual se multiplica el daño al conseguir un impacto crítico. |
| **15** | **Velocidad de Movimiento** | `8.0 m/s` | Mínimo funcional | Rapidez de desplazamiento en la arena (aceleración de `80 m/s²`, frenado de `70 m/s²`). |
| **16** | **Velocidad de Ataque (Cadencia)** | `×1.00` | `×2.00` máx. | Multiplicador que acelera la velocidad de emisión de los 4 pulsos de la linterna. |
| **17** | **Radio de Imán** | `2.5 m` | Sin tope fijo | Distancia a la que las gemas de XP y botiquines vuelan hacia el jugador (a `12 m/s`). |
| **18** | **Área / Cobertura** | `+0%` (Base: `60°`) | `20°` a `160°` | Incrementa la amplitud angular del cono de la linterna y el radio de explosiones. |

---

## Mecánicas principales

### Mecánica Principal 01: Movimiento en Plano 2.5D (Caminar en XZ)

**Propósito:**  
Permitir que el Coleccionista se mueva con fluidez y rapidez por el escenario en perspectiva isométrica, esquivando a la horda y manteniendo el control espacial mientras la linterna frontal adquiere automáticamente a las amenazas más cercanas.

**Cómo se activa:**  
Moviendo la palanca izquierda del control o usando las teclas **WASD** (o flechas de dirección).

**Cómo funcionará en el juego:**
* **Plano de movimiento:** El personaje camina sobre el suelo en dos direcciones: horizontal (X) y profundidad (Z). La altura (Y) se mantiene fija para evitar saltos o desniveles no deseados.
* **Velocidad al caminar:** Se moverá a **8 metros por segundo** como velocidad base. Este valor se conectará con el sistema de atributos del Coleccionista, por lo que al equipar entidades en el Almacén o elegir cartas de velocidad durante la run, el personaje responderá de forma inmediata.
* **Sensación de control (Sin resbalar):** El personaje arranca de inmediato al pulsar una tecla y frena casi en seco al soltarla. Esto evita la molesta sensación de "patinar sobre hielo" y permite maniobras precisas cuando hay muchos enemigos cerca.
* **Giro del personaje:** El cuerpo del personaje rota hacia la dirección en la que camina. Si dejas de moverte, conserva su última orientación de avance.
* **Control diagonal equilibrado:** Si presionas dos teclas a la vez (por ejemplo, `W` y `D` para caminar en diagonal), el personaje se desplaza a la misma velocidad que en línea recta. No corre más rápido en diagonal como suele ocurrir en prototipos novatos.

**Entradas de control:**  
Teclado (WASD / Flechas) o Mando (Stick analógico izquierdo / Cruceta).

**Beneficio para el jugador:**  
Libertad total para abrirse paso entre los enemigos y maniobrar en 360° mientras el foco de luz castiga a la amenaza más próxima.

**Riesgo o penalización:**  
Quedarse quieto o ser acorralado significa recibir daño continuo por contacto con los enemigos.

**Conexiones con otras mecánicas:**
* **Cono de la Linterna (Auto-Aim):** Adquiere al enemigo vivo más cercano dentro del alcance efectivo del arma (7.0 metros de base, modificado por sus mejoras). Si no hay enemigos en rango, el haz se alinea suavemente con la marcha del personaje.
* **Dash (Esquiva):** Si usas el dash mientras caminas, te impulsarás en esa misma dirección. Si estás quieto, te impulsarás hacia donde esté mirando el personaje.

---

### Mecánica Principal 02: Impulso Evasivo / Dash con Invulnerabilidad

**Propósito:**  
Darle al jugador un botón de escape de emergencia para salir de emboscadas, atravesar grupos de enemigos densos y colocarse en una posición ventajosa sin recibir daño.

**Cómo se activa:**  
Pulsando la barra espaciadora en teclado o el botón de acción sur (**A** en Xbox, **Cruz** en PlayStation).

**Cómo funcionará en el juego:**
* **El Impulso:** El personaje sale disparado hacia adelante a gran velocidad (**22 metros por segundo**) durante una fracción de segundo (**0.15 segundos**). En la práctica, esto representa un desplazamiento instantáneo de unos **3.3 metros**.
* **Invulnerabilidad total (*i-frames*):** Durante esos 0.15 segundos del dash, el jugador es **completamente inmune al daño**. No puede ser herido por enemigos ni por charcos dañinos en el suelo.
* **Atravesar enemigos (*Ghosting*):** Durante el dash, el personaje puede pasar a través de los cuerpos de los enemigos básicos y colosos sin atascarse con ellos.
* **Tiempo de recarga (*Cooldown*):** Tarda exactamente **1.0 segundo** en recargarse antes de poder volver a usarse.
* **Ataque simultáneo:** La linterna no se apaga durante el dash; sigue iluminando y quemando a los enemigos que atraviesas en el camino.

**Beneficio para el jugador:**  
Romper el cerco de la horda cuando te rodean y esquivar golpes mortales en el último segundo.

**Riesgo o penalización:**  
Si gastas el dash en una mala dirección (por ejemplo, hacia un muro perimetral o hacia un grupo aún más grande de enemigos), quedarás expuesto durante 1 segundo entero sin defensa de escape.

---

### Mecánica Principal 03: Linterna Cónica con Autoapuntado

**Propósito:**  
Atacar y controlar a las hordas enemigas de forma automática para permitir que el jugador se concentre plenamente en la navegación espacial, la esquiva mediante el dash y la recolección de chatarra en la arena.

**Activación y Comportamiento:**  
* **Autoapuntado inteligente:** El haz de luz detecta y rastrea de forma autónoma al enemigo vivo más cercano dentro de su rango de alcance, sin importar la dirección en la que camine el Coleccionista.
* **Retorno fluido:** Si no hay amenazas en rango, la linterna se orienta suavemente hacia la dirección de avance del personaje a una velocidad de giro de 720°/s.

**Funcionamiento del Arma Inicial:**  
* **Geometría básica:** Cono de luz frontal con una apertura de 60° y un alcance de 7.0 metros.
* **Ráfaga rítmica de 4 pulsos:** La linterna no dispara de forma aislada, sino en ráfagas compuestas por 4 pulsos de luz emitidos a lo largo de 0.8 segundos (un pulso cada 0.2s), seguidos por un breve descanso de enfriamiento de 0.4 segundos (ciclo completo de 1.2 segundos).
* **Daño por ráfaga:** 15 puntos de daño total por ráfaga base (3.75 puntos por pulso).
* **El 4.º impacto (Control y Empuje):** Los tres primeros pulsos dañan a los enemigos dentro del cono; el 4.º pulso desata un retroceso físico (*knockback* base de 10 m/s) que frena a los enemigos básicos y abre espacio vital. Los enemigos pesados (Colosos) y el jefe Gólgora son inmunes a este empuje.

**Mejoras y Sinergias del Arma:**  
A través de las cartas de subida de nivel, la linterna puede transformarse profundamente:
* **`FLASH_01` · Lente de Cuarzo:** Amplía el cono (+1.0m de alcance y +10° de apertura por nivel).
* **`FLASH_02` · Balasto de Alta Frecuencia:** Acelera la cadencia reduciendo el tiempo entre pulsos y el descanso de enfriamiento (+15% de velocidad de ataque por nivel).
* **`FLASH_04` · Foco Sobrecargado:** Estrecha el cono (-20°) pero dispara el daño (+50%/+80%/+120%) y la fuerza de empuje.
* **`FLASH_03` · Prisma de Refracción:** Hace que el 4.º impacto desate chispas en cadena hacia enemigos cercanos.
* **`FLASH_05` · Hiper-Rayo Solar (Modo Solar):** Transformación legendaria (SSR) que duplica el alcance a 14 metros y elimina por completo el descanso de 0.4s entre ráfagas, proyectando un haz solar continuo de daño masivo (×3.75 DPS).

**Pausa Táctica y Aplicación Limpia:**  
Al subir de nivel o pausar, la simulación se congela por completo. Al reanudar, las nuevas mejoras adquiridas entran en vigor a partir del siguiente ciclo de ráfaga de la linterna, garantizando un cálculo numérico limpio y sin cortes visuales bruscos.

---

## Mecánicas secundarias

### Mecánica Secundaria 01: Comportamiento de la Horda (Separación y Flanqueo)

**Propósito:**  
Hacer que el enjambre de enemigos se sienta vivo, agresivo y amenazante, evitando que se amontonen de forma fea en un solo punto o que caminen en una fila recta aburrida.

**Cómo funcionará en el juego:**
* **Persecución activa:** Todos los enemigos se orientan y caminan hacia la posición del jugador.
* **Separación entre ellos (No encimarse):** Los enemigos respetan un pequeño espacio personal entre sí. Si dos enemigos se tocan, se empujan suavemente hacia afuera para no fusionarse en un solo modelo.
* **Flanqueo (Evitar la fila india):** Si un enemigo intenta avanzar hacia el jugador pero tiene a otro compañero bloqueándole el paso justo enfrente, en lugar de frenarse y hacer cola, decide abrirse hacia la izquierda o hacia la derecha para rodear al jugador por los lados.
* **Variación individual de velocidad:** Para evitar un avance uniforme y rígido en bloque, cada enemigo recibe al aparecer una fluctuación aleatoria de $\pm15\%$ respecto a su velocidad base. Esto aporta fluidez orgánica al enjambre, permitiendo que algunas unidades adelanten a otras de forma natural.
* **Daño por contacto:** Cada enemigo inflige **10 puntos de daño** al jugador al tocarlo, con una pausa de medio segundo (0.5s) entre golpe y golpe para darle oportunidad al jugador de reaccionar.

---

### Mecánica Secundaria 02: Gemas de Chatarra y Recolección Magnética (XP)

**Propósito:**  
Recompensar al jugador por eliminar enemigos y motivarlo a avanzar sobre el terreno que acaba de limpiar para recoger sus recompensas.

**Cómo funcionará en el juego:**
* **Aparición de gemas:** Cada enemigo eliminado suelta una gema de chatarra/XP en el suelo en el lugar exacto donde fue destruido. Las gemas básicas otorgan **10 puntos de experiencia**.
* **Imán de recolección:** El personaje tiene un radio magnético base de **2.5 metros**. Mientras estés lejos, la gema permanece quieta en el piso.
* **Atracción activa:** En cuanto el jugador entra en ese radio de 2.5 metros, la gema se activa y "vuela" directamente hacia el jugador a gran velocidad (12 m/s).
* **Consumo:** Al tocar al personaje, la gema desaparece con un efecto visual y suma su valor a la barra de experiencia del jugador.
* **Mejora de Imán:** La ficha prevista de "Núcleo Supermagnético" (`GREED_01`) amplía la atracción en **+2.0 metros por nivel**. Sus valores y máximos se mantienen en [[05 - Progresión]], como referencia común.

---

### Mecánica Secundaria 03: Subida de Nivel y Selección de Mejoras (Draft de Cartas)

**Propósito:**  
Permitir que el jugador construya y personalice su *build* de combate en cada partida, adaptándose a la presión creciente de la arena mediante elecciones tácticas individuales.

**Cómo funcionará en el juego:**
* **Pausa táctica:** Al completar la barra de experiencia, la simulación se congela por completo (se pausan enemigos, proyectiles y el reloj de la partida), eliminando cualquier estrés de tiempo para que el jugador pueda reflexionar sobre su elección.
* **Terna de cartas (Draft):** Se presenta una ventana con hasta 3 cartas aleatorias seleccionadas del catálogo de 35 mejoras del juego, ponderadas por sus probabilidades de rareza (Normal, Rara, Súper Rara y Legendaria/SSR).
* **Filtrado inteligente de opciones:**
  * Se excluyen automáticamente aquellas cartas que ya hayan alcanzado su nivel máximo (Tier máximo).
  * Se filtran cartas condicionales si el jugador no posee el requisito base (por ejemplo, *Pólvora Volátil* solo se ofrece si el jugador ya equipó al menos una fuente de daño explosivo).
* **Mecánica de Re-roll:** Si el jugador cuenta con la carta `GREED_04` (Sintonizador de Frecuencia), puede pulsar el botón de re-roll para descartar las cartas actuales y generar una nueva selección aleatoria.
* **Escalado de experiencia:** Para mantener un ritmo equilibrado de progresión a lo largo de los 8 minutos, cada nivel requiere un 50% más de experiencia que el anterior (escalado continuo de $\times 1.5$, comenzando desde 100 XP para el nivel 2).
* **Reanudación del combate:** Una vez confirmada la carta seleccionada, se actualizan los atributos y la partida se reanuda al instante.

---

### Mecánica Secundaria 04: Botiquines de Chatarra Médica (Recuperación en Combate)

**Propósito:**  
Garantizar la viabilidad y supervivencia del Coleccionista durante los 8 a 10 minutos de expedición, recompensando el combate activo y mitigando errores de posicionamiento sin tener que pausar el ritmo de acción.

**Cómo funcionará en el juego:**
* **Probabilidad de caída (Drops de Enemigos):** Al ser destruido cualquier enemigo en la arena, además de su gema de experiencia garantizada, existe una probabilidad de que suelte un Botiquín de Reparación:
  * **Enemigos Básicos y Enjambre (Polillas, Interceptores):** **4%** de probabilidad de soltar botiquín.
  * **Enemigos Pesados y Élite (Colosos, Morteros):** **25%** de probabilidad garantizada de soltar botiquín.
* **Efecto de Curación Directa:** Al entrar en contacto con el Coleccionista, el botiquín restaura de forma inmediata **15 puntos de vida** a su salud actual, con tope en la salud máxima base (100 HP), salvo si se posee la carta de Nova de Plasma (`BIO_04`), que permite acumular sobrecuración.
* **Recolección Magnética Automática:** Los botiquines comparten el mismo sistema de succión que las gemas de XP:
  * Si el jugador se acerca a menos de **2.5 metros** (radio de imán base), el botiquín se activa y vuela hacia el jugador a gran velocidad (12 m/s).
  * La carta "Núcleo Supermagnético" (`GREED_01`, +2.0m por nivel) amplía también este radio de atracción para los botiquines.
* **Consumo y Retroalimentación:** Al recogerse, el botiquín desaparece con un destello visual verde y un sonido de recarga de blindaje, actualizando al instante la barra de salud en la interfaz principal.

---

> **Navegación:** [[00 - Índice]] · ← [[01 - Jugabilidad]] · [[03 - Sistemas]] →
