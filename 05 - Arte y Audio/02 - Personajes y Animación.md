# Personajes y animación

## Recursos de la versión actual

| Personaje | Recurso | Animaciones presentes |
| --- | --- | --- |
| Coleccionista | Atlas de 108 fotogramas, celdas de 64 × 64, cuatro direcciones. | Reposo: 8; marcha: 8; dash: 3; muerte: 8 por dirección. |
| Mortero Campana | Atlas de 112 fotogramas, celdas de 64 × 64, cuatro direcciones. | Flotación: 6; carga: 6; disparo: 4; recuperación: 4; muerte: 8 por dirección. |
| Polilla Reactor | Atlas de 62 fotogramas, celdas de 64 × 64. | Vuelo: 8 y cebado: 6 por dirección; estallido compartido: 6. |
| Residual e Interceptor | Geometría y materiales 3D. | Orientación, inclinación de locomoción, reacción al daño y muerte. |
| Coloso | Geometría y materiales 3D. | Marcha, compresión de preparación, embestida y recuperación. |
| Gólgora / MataColeccionista | Modelo 3D con animaciones. | Entrada, marcha pesada, golpes y lanzamiento; cierre con efectos de muerte. |

## El Coleccionista

![Atlas activo del Coleccionista](../Media/Avance_2026-10-27/Spritesheets/Coleccionista.png)

[Abrir fuente editable del Coleccionista](../Media/Avance_2026-10-27/Spritesheets/Coleccionista.aseprite).

Su sprite se apoya en el terreno y recibe iluminación del mundo. El farol acompaña al personaje y la presentación del haz sigue el apuntado del arma. El daño se expresa mediante flashes sin convertir cada impacto en una interrupción del movimiento.

### Animaciones dentro de la arena

Estas secuencias muestran al Coleccionista en CombatArena, con una cámara de detalle que permite observar el sprite sobre el terreno y su iluminación real.

**Reposo.** El personaje conserva su postura y el movimiento del ciclo de espera.

![Coleccionista en reposo dentro de la arena](../Media/Avance_2026-10-27/Jugador/Reposo.gif)

**Marcha en cuatro direcciones.** El cambio de vista y el ciclo de pasos acompañan el desplazamiento por el escenario.

![Coleccionista caminando en cuatro direcciones dentro de la arena](../Media/Avance_2026-10-27/Jugador/Caminar.gif)

**Dash.** La secuencia incluye dos desplazamientos y las imágenes residuales que deja el personaje.

![Dash del Coleccionista dentro de la arena](../Media/Avance_2026-10-27/Jugador/Dash.gif)

**Muerte.** El impacto da paso a la caída y a la desaparición del sprite y su sombra.

![Animación de muerte del Coleccionista dentro de la arena](../Media/Avance_2026-10-27/Jugador/Muerte.gif)

## El Mortero Campana

![Atlas activo del Mortero](../Media/Avance_2026-10-27/Spritesheets/Mortero.png)

El Mortero encadena flotación, preparación, disparo y recuperación con su ataque de ácido. El GIF muestra cómo se integra el sprite con el aviso del ataque y el proyectil dentro del escenario.

![Mortero animado y ataque de ácido dentro de la arena](../Media/Avance_2026-10-27/Enemigos/Mortero.gif)

## La Polilla Reactor

![Atlas activo de la Polilla](../Media/Avance_2026-10-27/Spritesheets/PolillaReactor.png)

La Polilla distingue vuelo, cebado y explosión. La secuencia permite ver la transición de aproximación a detonación junto al jugador.

![Vuelo, cebado y explosión de la Polilla dentro de la arena](../Media/Avance_2026-10-27/Enemigos/Polilla.gif)

## Enemigos con representación 3D

El Residual, el Interceptor y el Coloso utilizan geometría 3D. Sus movimientos y deformaciones se registran directamente en la arena; no disponen de una hoja de sprites de personaje.

**Residual.** Aproximación y movimiento de su cuerpo durante la persecución.

![Residual en movimiento dentro de la arena](../Media/Avance_2026-10-27/Enemigos/Residual.gif)

**Interceptor.** Desplazamiento y orientación del cuerpo al perseguir al Coleccionista.

![Interceptor en movimiento dentro de la arena](../Media/Avance_2026-10-27/Enemigos/Interceptor.gif)

**Coloso.** Preparación, embestida y recuperación del ataque.

![Embestida del Coloso dentro de la arena](../Media/Avance_2026-10-27/Enemigos/Coloso.gif)

**Gólgora / MataColeccionista.** El modelo animado acompaña su marcha pesada y sus ataques en la arena.

![Gólgora animado y atacando dentro de la arena](../Media/Avance_2026-10-27/Enemigos/Golgora.gif)

Las reglas de cada enemigo se describen en [[03 - Enemigos y NPC]].

Las normales, sombras, flashes, tintes de frío y partículas complementan los fotogramas originales. La elección de una cámara o una condición visual distinta no cambia las reglas de los ataques.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
