# Niveles

## Descripción de la sección

En este apartado se detalla el diseño del nivel principal contemplado para *EchoScrap*. Como estamos en la fase de documentación y planificación previa a la producción, aquí definimos las dimensiones, la distribución del espacio, el ritmo de juego y las reglas de entrada y salida que guiarán la construcción de la arena.

---

## Plantilla de nivel: Vertedero de Ecos (Arena Principal)

### Propósito y contexto

El nivel representa una cuenca industrial abandonada repleta de restos mecánicos y maquinaria oxidada. Es el lugar donde el Coleccionista se adentra para rescatar tecnología y componentes antes de que la corrosión o las hordas autómatas lo destruyan.

* **Objetivo principal:** Sobrevivir durante 8 minutos a la creciente oleada de enemigos y derrotar al titán Gólgora en el combate final.
* **Objetivos secundarios:** No se planean objetivos secundarios para este nivel; el diseño busca que el jugador se concentre al 100% en el posicionamiento, la recolección de experiencia y la supervivencia.
* **Duración estimada:** Entre 8 y 10 minutos por partida completa (8 minutos de oleadas progresivas más el tiempo necesario para resolver la batalla contra el jefe).
* **Dificultad:** Progresiva y escalonada. Arranca con una intensidad baja para que el jugador se adapte al movimiento y al auto-apuntado, y va sumando densidad y nuevos tipos de enemigos hasta llegar a la saturación máxima previa al jefe.

---

### Dimensiones y distribución espacial

Para que el combate funcione de forma fluida, se plantea un área amplia pero contenida:

* **Dimensiones de la arena:** Se proyecta un tamaño de **120 × 120 metros** sobre un plano horizontal continuo.
* **Justificación del tamaño:** Este espacio de 120 × 120 m ofrece el margen necesario para correr, girar en círculos (*kitear*) y esquivar ataques de área grandes (como los pisotones o los disparos de mortero) sin sentirse encajonado a los pocos segundos, pero sin ser tan extenso como para diluir la sensación de asedio o hacer que los enemigos tarden demasiado en llegar.
* **Límites del nivel:** El perímetro exterior del área de 120 × 120 m estará cerrado físicamente por **muros perimetrales de chatarra compactada** que impiden salir del mapa, diseñados con la altura justa para bloquear el paso sin ocluir la cámara cenital.
* **Geometría interior:** El centro del mapa se mantendrá despejado de obstáculos sólidos con colisión pesada, permitiendo que la luz de la linterna y los proyectiles viajen de forma limpia. Se podrán incluir elementos decorativos en el suelo (manchas de óxido, placas metálicas bajas) para dar ambientación sin entorpecer el movimiento.

---

### Mecánicas en juego

* **Mecánicas introducidas en este nivel:**
  * Movimiento libre en 8 direcciones y dash con breve invulnerabilidad.
  * Disparo automático en cono mediante ráfagas de luz.
  * Recolección por proximidad (gemas de experiencia y botiquines médicos).
  * Pantalla de subida de nivel para escoger cartas de mejora.
* **Mecánicas utilizadas activamente:**
  * Lectura de telégrafos en el suelo (círculos rojos de advertencia para ataques pesados).
  * Gestión de distancias frente a enemigos cuerpo a cuerpo y a distancia.
  * Aprovechamiento del retroceso de la linterna para ganar separación.

---

### Contenido presente en el nivel

* **Enemigos:** Se planea la aparición secuencial de los 5 arquetipos de horda documentados en [[03 - Enemigos y NPC]]:
  1. *Chatarrero Básico:* Presión frontal continua.
  2. *Interceptor:* Movimiento rápido para cerrar rutas de escape.
  3. *Coloso:* Resistencia alta y ataque de choque en área.
  4. *Mortero:* Proyectiles parabólicos que obligan a no quedarse quieto.
  5. *Polilla:* Ataque ágil por el aire ignorando aglomeraciones terrestres.
* **Jefe:** *Gólgora (Titán de Magma)*, cuya llegada se programa de forma fija al cumplirse el minuto 8:00 en el centro del escenario.
* **Reglas de generación (Spawn):** Los enemigos aparecerán en un anillo perimetral alrededor del jugador (aproximadamente a 14-16 metros de distancia), justo fuera del rango visible inmediato para mantener una sensación de asedio constante sin que broten de forma injusta encima del personaje.
* **Recompensas en el nivel:**
  * Gemas de experiencia sueltas al destruir enemigos para subir de nivel dentro de la run.
  * Botiquines de reparación médica con probabilidad de caída controlada.
  * Recompensa final: Al derrotar a Gólgora se consigue la extracción exitosa y la captura de una entidad para el Almacén (o +1 nivel si es duplicada).

---

### Condiciones de la partida

* **Condición de entrada:** Seleccionar la expedición desde el menú del Almacén (habiendo equipado hasta 3 entidades de la colección). El jugador inicia en el centro de la arena.
* **Puntos de guardado:** No aplican durante la partida. La expedición sigue una estructura roguelite donde cada intento se juega de principio a fin de forma ininterrumpida.
* **Condición de victoria:** Vencer a Gólgora conservando al menos 1 punto de vida.
* **Condición de derrota:** Llegar a 0 puntos de salud en cualquier momento de la partida (frente a las hordas o frente al jefe).
* **Eventos narrativos:**
  * Comienzo: Breve señal de radio confirmando el inicio del descenso.
  * Minuto 8: Alarma visual y cambio en la música que anuncia la llegada de Gólgora.
  * Conclusión: Pantalla de resumen con estadísticas de la expedición y retorno al Almacén.

---

### Esquema espacial de la arena (120 × 120 m)

```text
+-----------------------------------------------------------+
|       MUROS PERIMETRALES DE CHATARRA (LÍMITE FÍSICO)      |
|   +---------------------------------------------------+   |
|   |         ÁREA DE COMBATE ACTIVA (120 x 120 m)      |   |
|   |                                                   |   |
|   |                 Anillo de Spawn                   |   |
|   |              (14-16 m del jugador)                |   |
|   |                      ......                       |   |
|   |                   .          .                    |   |
|   |                  .    [P]     .   <-- Coleccionista|  |
|   |                   .  (Centro) .                   |   |
|   |                      ......                       |   |
|   |                                                   |   |
|   |            [Spawn de Gólgora al min 8:00]         |   |
|   |                                                   |   |
|   +---------------------------------------------------+   |
|       MUROS PERIMETRALES DE CHATARRA (LÍMITE FÍSICO)      |
+-----------------------------------------------------------+
```

---

## Fases del Nivel y Progresión Temporal

La progresión de la partida en la arena de 120 × 120 m está calibrada en un ciclo de supervivencia de 8 minutos estructurado en **5 fases canónicas de generación de horda**, seguido por el combate final de jefe:

### 1. Tabla de las 5 Fases Temporales del Spawner

| Fase Temporal | Duración | Frecuencia de Aparición | Tamaño del Grupo (*Racimo*) | Enemigos en Pantalla | Qué experimenta el Jugador |
|---|:---:|:---:|:---:|:---:|---|
| **Fase 1: Calentamiento** | 0:00 a 1:00 | 1.2 segundos | 1 enemigo individual | 10 a 20 enemigos | Aprender movimiento, autoapuntado, recolección y primeras mejoras. |
| **Fase 2: Presión Media** | 1:00 a 2:00 | 0.8 segundos | Parejas (2 enemigos juntos) | 30 a 50 enemigos | Empiezan a aparecer los primeros enemigos rápidos (interceptores); subes a niveles 2 y 3. |
| **Fase 3: Cerco en Racimo** | 2:00 a 3:00 | 0.6 segundos | Tríos (3 enemigos simultáneos) | 60 a 90 enemigos | La horda intenta rodearte; es momento de usar el Dash para abrirse paso. |
| **Fase 4: Escalada Intensa** | 3:00 a 8:00 | Disminuye de 0.40 a 0.20 s | Grupos de 4 (composición variada) | 150 enemigos continuos (picos de 250 en estrés) | Adaptarse a presión creciente: sostener una configuración al minuto 3 no garantiza sostenerla hasta el 8. |
| **Fase 5: El Juicio** | Minuto 8:00+ | Cesa escalada normal; Gólgora invoca esbirros cada 4.0 s | Aparece **Gólgora** | 1 jefe + esbirros invocados | Combatir hasta victoria o derrota; sólo la victoria concede la Ruleta de Extracción de entidad. |

---

### 2. Progresión Minuto a Minuto de la Escalada (0:00 a 8:00+)

La Fase 4 (de 3:00 a 8:00) funciona como una curva de interpolación dinámica continua entre intervalos de tiempo, aumentando la complejidad táctica cada minuto:

| Tiempo de Juego | Etapa / Sub-Fase | Intervalo entre Grupos | Experiencia Buscada |
|---|---|:---:|---|
| **0:00 – 1:00** | Calentamiento | 1.20 s | Comprender movimiento, autoapuntado, recolección y primeras mejoras. |
| **1:00 – 2:00** | Presión media | 0.80 s | Afrontar amenazas más rápidas y elegir rutas de recolección. |
| **2:00 – 3:00** | Cerco | 0.60 s | Usar dash y retroceso para conservar espacio frente a grupos. |
| **3:00 – 4:00** | Presión combinada | 0.40 s | Resolver grupos con funciones distintas y seguir recogiendo recursos mientras aumenta el ritmo de aparición. |
| **4:00 – 5:00** | Rutas comprometidas | 0.36 s | Afrontar más presencia de enemigos resistentes (colosos) y ataques que dificultan conservar una ruta segura. |
| **5:00 – 6:00** | Control de zona | 0.32 s | Combinar evasión de proyectiles y áreas dañinas (morteros) con la presión del combate cercano. |
| **6:00 – 7:00** | Amenazas simultáneas | 0.28 s | Gestionar combinaciones más frecuentes de artillería, tanques y enemigos explosivos (polillas). |
| **7:00 – 8:00** | Máxima presión de horda | 0.24 s a 0.20 s | Resistir la mayor frecuencia de refuerzo y las combinaciones más exigentes antes del jefe. |
| **Desde 8:00** | Juicio de Gólgora | Esbirros invocados (cada 4.0 s) | Esquivar los pisotones y proyectiles de Gólgora mientras se gestionan los esbirros que el jefe invoca durante el combate. |

---

## Flujo del nivel

```text
[Minuto 0:00] Entrada a la arena de 120 × 120 m
      ↓
[Minutos 0:00 - 1:00] Fase 1: Calentamiento (enemigo individual, 10-20 en pantalla)
      ↓
[Minutos 1:00 - 2:00] Fase 2: Presión Media (parejas, 30-50 en pantalla)
      ↓
[Minutos 2:00 - 3:00] Fase 3: Cerco en Racimo (tríos, 60-90 en pantalla)
      ↓
[Minutos 3:00 - 8:00] Fase 4: Escalada Intensa (curva minuto a minuto, grupos de 4, hasta 150-250 en pantalla)
      ↓
[Minuto 8:00] Fase 5: El Juicio (Aparición de Gólgora en el centro de la arena + invocación de esbirros)
      ↓
┌───────────────────────────────────────┴───────────────────────────────────────┐
↓                                                                               ↓
[Caída del Coleccionista]                                           [Derrota de Gólgora]
Muerte antes de abatir al jefe                                      Victoria de la expedición
      ↓                                                                         ↓
Pantalla de Derrota                                                 Ruleta de Extracción
Retorno al Almacén sin captura de entidad                           Captura de entidad (+1 nivel si es duplicada) y regreso al Almacén
```

---

## Criterios de calidad para el diseño del nivel

* **Claridad visual:** El espacio de 120 × 120 m debe permitir que el jugador distinga claramente las siluetas de los enemigos y los círculos de ataque en el suelo.
* **Legibilidad de límites:** Los muros perimetrales de chatarra deben ser visualmente evidentes para que el jugador identifique de un vistazo los bordes físicos de la arena y evite quedar acorralado en las esquinas.
* **Equilibrio de densidad:** Las oleadas deben mantener un ritmo dinámico donde siempre haya objetivos a los que apuntar, pero sin saturar la pantalla de manera que sea imposible maniobrar con el dash.
* **Transparencia en el resultado:** Si el jugador es derrotado, debe quedar claro qué impacto o proyectil provocó su caída; si gana, la recompensa debe sentirse proporcional al reto superado.

> **Navegación:** [[00 - Índice]] · ← [[01 - Estructura del Contenido]] · [[03 - Enemigos y NPC]] →
