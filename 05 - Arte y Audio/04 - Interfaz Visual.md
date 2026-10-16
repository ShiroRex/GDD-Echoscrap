# Interfaz Visual

## Descripción de la sección

Documenta la información que el juego muestra al jugador en pantalla y las reglas visuales de la interfaz, manteniendo coherencia con la estética *pixel art* y la paleta de colores Apollo.

---

## HUD (Interfaz en Combate)

Elementos presentes durante la partida en la arena:

- **Barra de Vida (Salud):** Muestra la salud actual y máxima del Coleccionista (100 HP base).
- **Indicador de Dash:** Muestra si el impulso evasivo está disponible o en su segundo de recarga (*cooldown*).
- **Barra de Experiencia (Ecos) y Nivel actual:** Ubicada en el borde superior, indica el progreso hacia la siguiente subida de nivel.
- **Cronómetro de Supervivencia:** Reloj visible (de `0:00` a `8:00+`) que marca el avance hacia la llegada de Gólgora.
- **Iconos de Mejoras y Entidades Activas:** Resumen discreto de las hasta 3 entidades equipadas del Almacén y las cartas obtenidas en la partida.
- **Barra de Salud del Jefe (Fase 5 — Minuto 8:00+):** Aparece de forma destacada cuando *Gólgora (El Guardián)* entra en la arena.

---

## Jerarquía visual

Para no saturar la vista cuando hay hasta 150–250 enemigos en pantalla, la lectura se ordena en tres niveles de prioridad:

1. **Primer nivel (Prioridad inmediata en el centro):** El Coleccionista, el cono proyectado de la linterna, los telégrafos de ataque en el suelo (círculos de peligro) y la disponibilidad del Dash.
2. **Segundo nivel (Bordes principales del HUD):** La barra de vida del jugador, el cronómetro de supervivencia y, a partir del minuto 8:00, la barra de vida de Gólgora.
3. **Tercer nivel (Progreso secundario):** La barra de experiencia (XP), el indicador de nivel actual y los iconos de mejoras acumuladas.

---

## Estados visuales de la interfaz

- **Estado normal:** Elementos del HUD estables en los bordes de la pantalla, dejando libre el área central de juego.
- **Advertencia / Peligro (Vida baja o llegada del Jefe):** Destello sutil en el marco de la barra de salud cuando la vida cae a un umbral crítico, y aviso visual en pantalla al cumplirse el minuto 8:00 con la llegada de Gólgora.
- **Daño recibido:** Parpadeo breve en la barra de salud sincronizado con el destello del personaje y sus *i-frames* (0.40 s).
- **Éxito / Subida de nivel:** Destello limpio en la barra de XP al llenarse, dando paso inmediato al modal pausado con las 3 cartas de mejora.
- **Bloqueo / Enfriamiento:** Atenuación visual del icono del Dash mientras transcurre su recarga, y siluetas oscurecidas para las entidades aún no desbloqueadas en las vitrinas del Almacén.

---

## Convenciones visuales

- **Tipografía:** Fuente *pixel art* / monoespaciada de alto contraste, sin serifas decorativas que dificulten la lectura rápida.
- **Iconografía:** Iconos en *pixel art* (32×32 y 64×64 px) diferenciados por forma y borde según su nivel de rareza (Común, Raro, Épico, Legendario y Mítico en entidades; Normal, Rara, Súper Rara y Legendaria en cartas).
- **Tamaños y legibilidad:** Textos claros y escalables a 1920×1080 (con opción de texto grande en Accesibilidad) sin recortar descripciones.
- **Contraste:** Fondos oscuros semitransparentes detrás de los textos y tarjetas de mejora para separar la información del escenario 3D, siguiendo la paleta Apollo.
- **Animaciones de UI:** Transiciones breves e instantáneas en menús y resaltado claro del botón o carta seleccionada para permitir navegación fluida con teclado (`WASD` / `1, 2, 3`) o mando sin requerir ratón.

---

## Pantallas

1. **Pantalla de carga:**  
   Carga inicial de *assets* y texturas, con el arte del juego y una barra de progreso.
2. **Menú principal:**  
   Pantalla de título con opciones directas: *Nueva colección / Continuar (Almacén)*, *Configuración* y *Salir*.
3. **Lobby / El Almacén:**  
   Zona segura entre partidas. Desde aquí el jugador consulta las fichas de los 9 coleccionables, revisa sus niveles, **equipa hasta 3 coleccionables** para la incursión y da inicio a la partida.
4. **Tutorial / Controles:**  
   Pantalla rápida que enseña los controles esenciales (Movimiento, Dash, Pausa y Selección de cartas) y qué hace cada uno.
5. **Arena (HUD en partida):**  
   La interfaz durante el combate: barra de vida, indicador de dash, tiempo de supervivencia, nivel actual, barra de XP y barra de Gólgora en la fase final.
6. **Subida de nivel (Selección de mejoras):**  
   Pausa táctica al recolectar suficiente XP. Congela el combate y despliega las **3 opciones de mejora aleatorias** (del catálogo de 35 cartas) para que el jugador elija una sin prisa.
7. **Menú de pausa:**  
   Menú emergente en partida con opciones para *Reanudar*, consultar *Controles*, abrir *Configuración* o *Abandonar expedición*.
8. **Ruleta de Extracción:**  
   Se activa **al lograr la victoria derrotando a Gólgora**. Otorga al jugador una tirada para capturar uno de los 9 coleccionables de diferentes rarezas (si sale repetido, le suma **+1 nivel**).
9. **Pantalla final de resultados:**  
   Muestra el desenlace (Victoria o Derrota), tiempo total sobrevivido, enemigos eliminados, nivel alcanzado y, en caso de victoria, el coleccionable obtenido en la ruleta antes de volver al Almacén.
10. **Ajustes / Configuración:**  
    Ventana de configuración para ajustar audio, video, accesibilidad y reasignación de controles.

> **Navegación:** [[00 - Índice]] · ← [[03 - Entornos]] · [[05 - Audio y Música]] →
