# Pilares de Diseño

## Descripción de la sección

Los pilares son los principios inmutables que deben mantenerse durante todo el desarrollo de *EchoScrap*. Cuando surja una duda, debate o propuesta de nueva mecánica, los pilares determinan si la idea pertenece al juego o si debe descartarse de inmediato.

---

## Pilar 1 — Agresividad Frontal (*Face the Swarm*)

**Descripción:**  
La mejor defensa en *EchoScrap* es abrirse paso hacia adelante. El juego rechaza la huida pasiva infinita: el jugador no puede ganar corriendo en círculos mientras armas automáticas a su espalda limpian la pantalla.

**¿Cómo se refleja en el juego?**
* **Disparo hacia el movimiento:** El arma principal (la linterna) dispara ráfagas estrictamente hacia la dirección en la que camina el personaje, obligando a encarar la horda para infligir daño.
* **Control de masas por luz:** La ráfaga ofensiva empuja (*knockback*) y frena a los enemigos, convirtiendo el ataque en una herramienta para abrir brechas y romper cercos.
* **El Dash como herramienta de reposicionamiento:** La evasión con invulnerabilidad (*i-frames*) no es para huir al rincón del mapa, sino para atravesar enjambres, ganar espacio y volver a encarar el peligro.

---

## Pilar 2 — Coleccionismo con Alma (*Every Scrap has an Identity*)

**Descripción:**  
El progreso permanente y el poder del jugador no se basan en números planos o tablas de estadísticas genéricas (+5% de daño); se construyen mediante **rehenes y entidades con nombre, diseño, rareza y lore propio**.

**¿Cómo se refleja en el juego?**
* **El Almacén como vitrina:** La base entre partidas es un refugio interactivo donde el jugador exhibe y equipa las 14 Entidades capturadas (de Común a Mítico).
* **Impacto tangible en la run:** Cada entidad equipada transforma los atributos base o añade comportamientos ofensivos activos, fomentando la experimentación de combinaciones (*builds*).
* **La Ruleta del Destino:** Al terminar una partida (por muerte o supervivencia), el jugador experimenta la satisfacción de un sorteo visual donde desbloquea nuevas entidades o materiales para subirlas de nivel.

---

## Pilar 3 — Claridad Táctica HD-2D (*Readability over Chaos*)

**Descripción:**  
Tener 250 enemigos y cientos de efectos simultáneos en pantalla jamás debe comprometer la lectura instantánea del peligro. El jugador siempre debe saber por qué recibió daño y cómo evitarlo.

**¿Cómo se refleja en el juego?**
* **Visibilidad total de la arena:** El escenario tiene iluminación ambiental nítida; no hay enemigos invisibles ni muertes injustas por caminar en sombras opacas.
* **Mallas de ataque exactas:** Los conos y áreas de efecto (como el cono de luz procedural en el suelo) reflejan exactamente los límites matemáticos del daño. Lo que ves iluminado es exactamente lo que recibe daño.
* **Estética HD-2D cohesiva:** Fusión limpia de sprites *Pixel Art* en el protagonista y enemigos con entornos *Low-Poly 3D* estilizados en URP, enriquecidos con post-procesamiento cinemático (iluminación dinámica, Bloom y Depth of Field sutil) que corre a 60 FPS estables.

---

## Pilar 4 — Tensión Comprimida (*8 Minutes to Judgement*)

**Descripción:**  
La experiencia está diseñada para ser intensa, compacta y sin tiempos muertos. No hay sesiones de relleno ni partidas lentas de 45 minutos.

**¿Cómo se refleja en el juego?**
* **Partidas de 8 a 10 minutos:** Ritmo de juego acelerado donde cada segundo cuenta y la acumulación de Chatarra/Ecos exige decisiones inmediatas.
* **Escalada constante de amenaza:** Cada minuto introduce mayor densidad de horda, nuevos patrones o enemigos más agresivos.
* **Clímax definitivo (Gólgora):** Al llegar al minuto final, *Gólgora*, un coloso titánico, aparece para juzgar los crímenes del Coleccionista. Toda la partida se reduce a una prueba de fuego: o tu build es letal, o eres purificado.

---

## Reglas de decisión para el Equipo

Antes de incorporar cualquier arma, enemigo o mecánica, debemos pasarla por este filtro:

1. **¿Favorece el avance y el posicionamiento táctico, o fomenta la huida pasiva?** *(Si fomenta la pasividad, se descarta).*
2. **¿Se puede representar como parte del universo de chatarra y entidades del Coleccionista?** *(Si es genérico, se rediseña).*
3. **¿Afecta la tasa de 60 FPS o ensucia la legibilidad de la pantalla?** *(Si causa lag o ruido visual excesivo, se optimiza o elimina).*
4. **¿Aporta al ciclo de 8 minutos o dilata innecesariamente la partida?** *(Si alarga la run sin agregar tensión, se corta).*

---

**Navegación:** `[[00 - Índice]]` • ⬅️ `[[03 - Público y Plataforma]]` • `[[01 - Jugabilidad]]` ➡️
