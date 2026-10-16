# UX y Flujo del Jugador

## Descripción de la sección

La experiencia de usuario de *EchoScrap* busca que el jugador comprenda rápidamente cómo sobrevivir, identifique los peligros y reconozca el efecto de sus decisiones. La sensación principal será pasar de la vulnerabilidad al inicio a una mayor confianza al obtener mejoras, mientras las hordas aumentan la presión minuto a minuto.

El combate debe transmitir tensión y desafío; el Almacén y las pausas de selección de mejoras ofrecerán momentos de calma para revisar el progreso y preparar estrategias.

---

## Flujo principal

1. **Inicio y menú:** El jugador inicia una nueva colección o accede a su progreso guardado.
2. **Almacén del Coleccionista (Hub):** Consulta las fichas de las 9 entidades, revisa sus niveles y beneficios pasivos, y **equipa hasta 3 entidades** para preparar la siguiente expedición.
3. **Entrada a la arena:** Comienza la supervivencia en el Vertedero de Ecos (arena cerrada de 120 × 120 m delimitada por muros perimetrales).
4. **Combate y recolección:** Se mueve en el plano XZ mientras la linterna apunta automáticamente al enemigo más cercano en rango (emitiendo ráfagas de 4 pulsos con empuje en el cuarto impacto), utiliza el *Dash* para esquivar y recoge gemas de experiencia (Ecos) y botiquines.
5. **Desenlace:** La partida termina cuando la vida llega a cero (**Derrota**) o cuando el jugador sobrevive hasta el minuto 8:00 y logra abatir a *Gólgora (El Guardián)* (**Victoria**).
6. **Resultados y recompensa:** Se presenta el resumen de desempeño de la partida; si se logró la **Victoria**, se accede a la **Ruleta de Extracción** para capturar una entidad aleatoria (o sumarle **+1 nivel** si es duplicada).
7. **Regreso al Almacén:** El jugador revisa lo obtenido, lee nuevas fichas desbloqueadas y ajusta sus 3 ranuras para una nueva expedición.

El jugador podrá abrir el menú de pausa durante el combate en cualquier momento. Las partidas tendrán una duración prevista de entre 8 y 10 minutos; Gólgora aparecerá al cumplirse el minuto 8:00.

---

## Primeros minutos (Onboarding en partida)

### Antes de la primera expedición

Se propone una explicación breve y visual con los controles esenciales: movimiento con `WASD` o flechas (o stick izquierdo), `Dash` con `Espacio` (o botón Sur) y pausa con `Escape`. También se explicará que la linterna **apunta y ataca automáticamente al enemigo más cercano dentro de su alcance** (y se alinea con el movimiento cuando no hay enemigos cerca), y que absorber los Ecos permite subir de nivel. Estas indicaciones podrán consultarse nuevamente desde el menú de pausa.

### Minuto 0:00 a 1:00 — Aprender a moverse, posicionarse y atacar

El jugador descubrirá que el cono visible proyectado en el suelo representa su zona de ataque y que la linterna rastrea automáticamente a la amenaza más próxima dentro de sus 7 metros de alcance, empujándola en el cuarto pulso. La aparición inicial de enemigos individuales permitirá practicar el control de distancia y observar cómo los Ecos de experiencia vuelan hacia el personaje al entrar en su radio magnético (2.5 m).

### Minuto 1:00 a 2:00 — Comprender las mejoras

El aumento de enemigos en parejas y la aparición de los primeros Interceptores exigirán organizar rutas de movimiento. Al subir de nivel, la partida se pausará y el jugador elegirá entre 3 cartas aleatorias (como aumentar el daño, ampliar el cono de la linterna o mejorar el radio de recolección). La pausa permitirá leer cada opción sin recibir ataques.

### Minuto 2:00 a 3:00 — Utilizar el dash

La presión por los costados en grupos de tres enseñará la importancia de usar el *Dash* (con sus 0.15 s de invulnerabilidad) para atravesar cercos enemigos y reposicionarse. Debe quedar claro que tiene un tiempo de recarga (1.0 s base) y que los muros perimetrales de la arena impiden salir del mapa.

### Desde el minuto 3:00 hasta el 8:00+ — Combinar lo aprendido y enfrentar a Gólgora

El jugador deberá equilibrar posicionamiento, recolección y evasión frente a combinaciones de Colosos, Morteros y Polillas hasta la llegada de Gólgora en el minuto 8:00, quien detendrá las oleadas normales e invocará esbirros durante el combate final.

---

## Retroalimentación

Las respuestas visuales y sonoras se coordinan con el apartado de Arte y Audio:

| Entrada o evento                      | Respuesta visual                                                                         | Respuesta sonora propuesta       | Resultado                                                |
| :------------------------------------ | :--------------------------------------------------------------------------------------- | :------------------------------- | :------------------------------------------------------- |
| **Moverse**                           | El personaje se desplaza con fluidez; si no hay enemigos en rango, el cono mira al frente.| Pasos discretos.                 | El jugador controla su posición espacial en la arena.    |
| **Alcanzar a un enemigo con linterna**| El cono apunta al blanco más cercano; el enemigo parpadea en blanco y retrocede al 4.º pulso.| Chasquido de pulso y empuje al 4.º.| Se reconoce el ritmo de los 4 pulsos y el control de masas.|
| **Activar el dash**                   | Impulso rápido con estela de polvo e indicador de recarga en el HUD.                     | Efecto corto de impulso.         | Se comunica la evasión con invulnerabilidad y su recarga.|
| **Recibir daño**                      | Disminuye la barra de vida y el personaje parpadea durante sus 0.40 s de gracia.         | Sonido seco de daño (prioritario).| El jugador identifica al instante que fue alcanzado.     |
| **Recoger Ecos de XP o botiquines**   | Los objetos vuelan hacia el jugador a 12 m/s y sube la barra de XP o de salud.           | Sonido breve de recolección.     | Se percibe el avance hacia el siguiente nivel o curación.|
| **Subir de nivel**                    | Aparecen tres cartas de mejora y el combate se congela por completo.                     | Tono metálico y atenuación musical.| El jugador toma una decisión táctica sin presión temporal.|
| **Llegada de Gólgora (Min 8:00)**     | Alerta visible, despeje de horda previa y aparición de la barra de vida del jefe.        | Estruendo sísmico y música de jefe.| Se comunica el inicio del enfrentamiento final.          |

---

## Fricción intencional

La dificultad estará en administrar el espacio dentro de la arena de 120 × 120 m, mantener la distancia óptima para que la linterna castigue a las amenazas prioritarias, esquivar telégrafos de área y usar el *Dash* en el momento adecuado. Elegir entre las 3 cartas de mejora también implicará decidir qué necesita la *build* de esa partida.

No deben formar parte de la dificultad los textos ilegibles, los controles confusos, los ataques ocultos por exceso de efectos visuales ni menús que permitan recibir daño mientras se leen.

---

## Descubrimiento

- **Qué hacer:** Una indicación inicial clara explicará que se debe sobrevivir 8 minutos, recolectar Ecos para mejorar estadísticas/habilidades y derrotar al Guardián (*Gólgora*).
- **Dónde ir:** El jugador reconocerá el espacio abierto de la arena, los muros perimetrales que delimitan el mapa y los Ecos/botiquines en el suelo para trazar su ruta.
- **Con qué puede interactuar:** Los elementos seleccionables del Almacén, las 3 ranuras de equipamiento y las cartas de mejora tendrán un borde destacado e indicación de tecla/botón.
- **Qué ocurrió:** Los cambios de salud, experiencia, recarga de dash y mejoras tendrán una respuesta visual y sonora inmediata.
- **Por qué ganó o perdió:** La pantalla de resultados indicará claramente si cayó en combate (regresando al Almacén) o si abatió a Gólgora (pasando a la Ruleta de Extracción).

> **Navegación:** [[00 - Índice]] · ← [[05 - Audio y Música]] · [[02 - UI y Menús]] →
