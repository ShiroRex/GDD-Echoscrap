# EchoScrap: avance jugable

**Versión del GDD: 0.4.0 · Actualización: 27 de octubre de 2026.**

EchoScrap cuenta con una partida de supervivencia completa: el Coleccionista se desplaza por el Vertedero, combate con una linterna automática, recoge experiencia, elige mejoras y se enfrenta a Gólgora. La victoria y la derrota cierran la sesión con un resumen y permiten reiniciar. Esta edición describe el comportamiento que existe en el proyecto Unity actual.

![Arena y HUD actuales](Media/Avance_2026-10-27/Interfaz/Arena_HUD.png)

## Qué se puede jugar

| Área | Contenido actual |
| --- | --- |
| Control | Movimiento con teclado o mando, dash direccional con invulnerabilidad, apoyo sobre relieve y escaleras. |
| Arma | Linterna de cuatro pulsos con autoapuntado; alcance, cadencia, cobertura y daño modificables durante la partida. |
| Enemigos | Residual, Interceptor, Coloso, Mortero, Polilla y el jefe Gólgora, representado por el MataColeccionista. |
| Progresión | Experiencia, subida de nivel y selección de tres opciones entre 35 mejoras implementadas. |
| Escenario | Arena de 200 × 200 m, desniveles, obstáculos, lava, acceso subterráneo y portal al almacén. |
| Variantes | Entrada del jefe a los tres o a los ocho minutos, según la escena cargada. El combate del jefe continúa hasta resolver la partida. |
| Colección | Cinco personajes en el sistema de fusión por rareza, ocho espacios de exposición y guardado local del inventario y la fusión. |
| Interfaz | Vida, experiencia, nivel, tiempo, bajas, dash, mejoras, pausa, resultados y estadísticas del jugador. |
| Presentación | Sprites animados, modelos 3D, avisos de ataques, números de daño, partículas, cadenas eléctricas, hielo, dispositivos y efectos de recogida. |

## Recorrer el avance

- [[01 - Jugabilidad]] explica el recorrido de una partida.
- [[03 - Enemigos y NPC]] reúne los seis enemigos, sus reglas, sus representaciones actuales y GIFs capturados en la arena.
- [[05 - Progresión]] contiene el catálogo de las 35 mejoras y sus requisitos.
- [[04 - Objetos y Coleccionables]] describe los objetos, la colección y las recetas de fusión que se pueden utilizar.
- [[02 - Personajes y Animación]] incluye las hojas de sprites activas, las fuentes editables y GIFs del Coleccionista y los enemigos dentro de la arena.
- [[06 - Registro visual]] reúne las capturas y los GIFs de interfaz, enemigos y VFX.

Los GIFs de enemigos se registraron en Play Mode el 27 de octubre sobre la arena real, con cámara de detalle, oleadas detenidas y el jugador protegido para observar cada ataque. Los enemigos conservan sus valores y sus animaciones. Los GIFs de mejoras son recortes de capturas de Unity de la revisión visual del 26 de octubre; los componentes y recursos que dibujan esos efectos coinciden con los del avance actual. Los atlas son los PNG utilizados por el juego. Las condiciones de las capturas se explican en [[06 - Registro visual]].

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
