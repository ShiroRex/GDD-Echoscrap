# Alcance

## Descripción de la sección

Define qué se va a construir y, sobre todo, qué queda fuera del proyecto.

## Alcance de la versión objetivo

### Incluido

- **Locomoción y Control 2.5D:** Control reactivo del personaje (WASD) con traslación desacoplada y mecánica de *Dash* con invulnerabilidad temporal (*i-frames*).
- **Combate con Auto-Aim Inteligente:** Linterna frontal como arma principal que rota automáticamente a 360° hacia la amenaza más cercana dentro de 7.0 metros, disparando ráfagas rítmicas de 4 pulsos con retroceso físico planar en el último impacto.
- **Curva de Supervivencia de 8 Minutos:** Spawner modular organizado en 5 fases de intensidad progresiva (Calentamiento, Presión Media, Cerco en Racimo, Muro Extremo y Clímax del Juicio).
- **Arquetipos de Enemigos:** 5 enemigos de horda (Básico Residual, Interceptor veloz, Coloso blindado, Mortero artillero con charcos corrosivos y Polilla kamikaze con detonación táctica).
- **Jefe Final de Clímax (Gólgora):** Titán con 1500 HP, inmunidad a knockback, ataque telegrafiado de Pisotón de magma (5m), bombardeo de lava parabólico, invocación periódica de esbirros y condición formal de Victoria al ser derrotado.
- **Sistema de Progresión y Subida de Nivel:** Recolección magnética de gemas de XP, barra de progreso con escalado exponencial ($\times 1.5$) y draft de 3 cartas de mejora aleatorias seleccionables mediante teclado (`1, 2, 3`), clic de ratón o gamepad.
- **Gestión del Ciclo de Vida de Partida y Almacén:** Flujo integral que coordina preparación en el Almacén (hasta 3 entidades equipadas por partida), combate, pausa táctica de subida de nivel, pantalla de derrota y victoria formal con Ruleta de Extracción de entidades (donde los duplicados suman +1 nivel).
- **Optimización y Rendimiento:** Arquitectura orientada a 60 FPS estables con hasta 150 enemigos continuos (y hasta 250 en picos de estrés) mediante técnicas de *pooling*.

### No incluido

- **Almacén 3D Interactivo Completo:** La navegación física por el refugio del Coleccionista se sustituye en el MVP por la interfaz 2D del Almacén para inspeccionar y equipar entidades.
- **Modo Multijugador Cooperativo / En Red:** La experiencia está enfocada exclusivamente en un solo jugador local (Single Player).
- **Armas Secundarias Múltiples Simultáneas:** El MVP prioriza la pulcritud de la linterna y su autoapuntado sobre un catálogo masivo de armas accesorias secundarias fijas (las secundarias se obtienen como mejoras de cartas automáticas, como sierras orbitales).
- **Generación Procedural de Escenarios:** La arena de combate utiliza un diseño fijo de 120 × 120 m delimitado físicamente por muros perimetrales de chatarra.
- **Soporte de Plataformas Móviles o Consolas:** El ejecutable objetivo es exclusivamente Windows PC (x86_64).

## MVP

El **Producto Mínimo Viable** debe contener únicamente lo necesario para demostrar que la experiencia principal funciona de extremo a extremo sin fricciones ni bugs bloqueantes.

**MVP del proyecto:**
1. Arena de combate principal (120×120m) completamente funcional.
2. Un bucle completo de supervivencia de 8 minutos: inicio $\rightarrow$ subida de niveles y cartas de draft $\rightarrow$ supervivencia frente a 5 arquetipos de enemigos $\rightarrow$ enfrentamiento y derrota del jefe Gólgora $\rightarrow$ pantalla de Victoria con Ruleta o Derrota con opción de reinicio.
3. Movimiento desacoplado y linterna auto-aim operativos sin errores visuales de renderizado ni tirones de framerate (60 FPS estables).
4. Pruebas automatizadas en verde que validen la adquisición de objetivos, cálculos de ráfaga y transición de fases.

## Vertical Slice

¿Qué parte del juego debe estar completamente terminada para demostrar la calidad y dirección del producto?

- **Los primeros 3 minutos de partida y la batalla contra Gólgora:** Deben reflejar el estándar visual y sonoro definitivo del juego (iluminación volumétrica URP, materiales estilizados HD-2D, retroalimentación de impacto en enemigos con *hit-flash*, círculos de advertencia de explosión telegrafiados y respuesta de control inmediata). Demuestra al jurado evaluador la profundidad táctica, la fluidez del control y la estabilidad técnica del motor.

## Criterios de recorte

Si el tiempo o los recursos del equipo son insuficientes antes de la entrega:

1. **Primer recorte:** Simplificar la pantalla de resultados post-partida a un panel limpio de estadísticas (tiempo sobrevivido, bajas y nivel alcanzado) omitiendo la animación interactiva de la ruleta de recompensas.
2. **Segundo recorte:** Desactivar los efectos de post-procesamiento pesados (Bloom multicapa y Depth of Field) en favor de iluminación plana en caso de requerir optimización para ordenadores de gama baja de la universidad.
3. **Elemento inmutable (No negociable):** El control fluido del personaje, el autoapuntado con retroceso táctico de la linterna y el enfrentamiento contra el jefe Gólgora constituyen el núcleo fundamental y bajo ninguna circunstancia pueden ser eliminados o recortados.


> **Navegación:** [[00 - Índice]] · ← [[05 - Seguridad y Riesgos Técnicos]] · [[02 - Roles y Responsabilidades]] →
