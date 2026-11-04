# Personajes y animación

## Recursos actuales

| Personaje o arquetipo | Atlas | Animaciones por dirección |
| --- | --- | --- |
| Coleccionista | 108 fotogramas de 64 × 64. | Reposo 8, marcha 8, dash 3 y muerte 8. |
| Residual / Esqueleto | 144 fotogramas de 96 × 96. | Reposo 8, marcha 8, ataque 8, impacto 4 y muerte 8. |
| Interceptor | 152 fotogramas de 96 × 96. | Reposo 6, marcha 12, ataque 6, impacto 4 y muerte 10. |
| Coloso / Wendigo | 200 fotogramas de 96 × 96. | Reposo 8, marcha 8, preparación 8, carga 6, recuperación 6, impacto 4 y muerte 10. |
| Mortero Campana | 112 fotogramas de 64 × 64. | Flotación 6, carga 6, disparo 4, recuperación 4 y muerte 8. |
| Polilla Reactor | 62 fotogramas de 64 × 64. | Vuelo 8 y cebado 6; estallido común de 6. |
| Caballero Fantasma y Centinela | Geometría de presentación; efectos animados en una biblioteca propia. | Estados de preparación, resolución y recuperación. No tienen atlas de personaje. |
| Gólgora | Modelo 3D animado y VFX de magma. | Entrada, locomoción, martillazo, barrido, lanzamiento, salto y presentación final. |

Las cinco hojas de enemigos y los GIFs de todas sus acciones se encuentran junto a sus reglas en [[03 - Enemigos y NPC]]. Esa página también incorpora las capturas actuales de los dos élites y de los seis patrones del jefe.

## El Coleccionista

El farol sigue al personaje y el haz acompaña al apuntado automático. El sprite mantiene un plano vertical con proporciones fijas en el mundo y se apoya sobre el terreno. La iluminación, sombras y reacciones al daño se añaden durante la ejecución.

![Atlas activo del Coleccionista](../Media/Avance_2026-11-04/Spritesheets/Coleccionista.png)

[Fuente editable del Coleccionista](../Media/Avance_2026-11-04/Spritesheets/Coleccionista.aseprite).

| Animación | Cuatro direcciones |
| --- | --- |
| Reposo | ![Coleccionista: Reposo](../Media/Avance_2026-11-04/Animaciones/Coleccionista/Idle.gif) |
| Marcha | ![Coleccionista: Marcha](../Media/Avance_2026-11-04/Animaciones/Coleccionista/Walk.gif) |
| Dash | ![Coleccionista: Dash](../Media/Avance_2026-11-04/Animaciones/Coleccionista/Dash.gif) |
| Muerte | ![Coleccionista: Muerte](../Media/Avance_2026-11-04/Animaciones/Coleccionista/Death.gif) |

## Cómo leer las previsualizaciones

Cada GIF de atlas coloca juntas las cuatro vistas y usa sus fotogramas y tiempos exportados. La escala es el doble del píxel original, sin interpolación. El bucle facilita revisar una acción; los golpes y las muertes no se repiten indefinidamente durante el juego.

Estas imágenes muestran el recurso de animación. Las luces, sombras, posición sobre el terreno, movimiento físico y VFX se revisan en las capturas de Unity. El registro visual diferencia ambas fuentes para que una previsualización de sprites no se confunda con una partida.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
