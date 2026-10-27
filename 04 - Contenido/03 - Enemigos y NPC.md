# Enemigos y NPC

La horda reúne cinco arquetipos y el cierre de partida utiliza un jefe. Cada uno se diferencia por su velocidad, su forma de acercarse y el espacio que obliga a defender. Los valores siguientes corresponden a los ajustes actuales de la arena.

| Enemigo | Vida | Velocidad | Daño principal | XP al morir | Representación actual |
| --- | ---: | ---: | --- | ---: | --- |
| Residual | 15 HP | 3,5 m/s ±15% | Contacto: 10 cada 0,5 s | 10 | Geometría 3D y animación de presentación. |
| Interceptor | 9 HP | 6,2 m/s ±10% | Contacto: 8 cada 0,5 s | 10 | Geometría 3D de perfil bajo y estela. |
| Coloso | 80 HP | 1,8 m/s | Contacto o carga: 25 | 50 | Geometría 3D pesada con preparación y embestida. |
| Mortero | 25 HP | 2,2 m/s ±10% | Contacto: 5; ácido: 5 cada 0,5 s | 25 | Sprite animado Campana, de cuatro direcciones. |
| Polilla | 12 HP | 4,8 m/s ±10% | Explosión: 35 | 15 | Sprite animado Reactor, de cuatro direcciones. |
| Gólgora / MataColeccionista | 1500 HP | 1,6 m/s | Contacto 30; pisotón 40; impacto de bombardeo 25 | 500 de cierre en el resumen | Modelo 3D animado y VFX de magma. |

## Residual

Persigue al jugador, se separa de los demás enemigos y busca paso alrededor de obstáculos. Su función es formar la masa de la horda y dejar gemas accesibles. Una ráfaga base de cuatro pulsos suma sus 15 HP cuando todos los impactos alcanzan al objetivo. El cuarto pulso puede desplazarlo.

![Residual en la arena](../Media/Avance_2026-10-27/Enemigos/Residual.gif)

## Interceptor

Anticipa hasta 0,4 s del desplazamiento del jugador, limitado a una velocidad de referencia de 8 m/s. A corta distancia pasa a perseguir su posición actual. Su frenado tras el empuje es menor que el del Residual, por lo que puede deslizarse más lejos.

![Interceptor en la arena](../Media/Avance_2026-10-27/Enemigos/Interceptor.gif)

## Coloso

Es un cuerpo sólido y pesado, inmune al empuje. Cuando tiene la carga disponible y el jugador está a 8 m o menos, se detiene, fija la dirección y muestra un corredor de aviso durante 0,8 s. Embiste a 12 m/s durante 0,35 s, hasta 4,2 m si el camino está libre. Las paredes detienen el recorrido y la dirección no sigue corrigiéndose tras el aviso.

Después se recupera durante 0,6 s. La preparación y la recuperación no infligen contacto; el dash permite atravesar su cuerpo durante la ventana de invulnerabilidad. El intervalo entre cargas parte de un sorteo de 5–7 s.

![Preparación, carga y recuperación del Coloso](../Media/Avance_2026-10-27/Enemigos/Coloso.gif)

## Mortero: Campana

Mantiene una banda de distancia entre 5,5 y 6,5 m y dispara hasta 12 m. El intervalo es de cinco segundos, con 0,45 s de aviso y 0,85 s de vuelo. El proyectil deja un charco de radio 1,2 m durante tres segundos; el ácido causa 5 de daño cada 0,5 s. La arena limita a seis los Morteros simultáneos.

Su hoja activa contiene 112 fotogramas de 64 × 64: flotación, carga, disparo, recuperación y muerte en cuatro direcciones.

![Hoja de sprites activa del Mortero](../Media/Avance_2026-10-27/Spritesheets/Mortero.png)

[Fuente editable del Mortero](../Media/Avance_2026-10-27/Spritesheets/Mortero.aseprite).

![Mortero, aviso y disparo dentro de Unity](../Media/Avance_2026-10-27/Enemigos/Mortero.gif)

## Polilla: Reactor

Se acerca hasta cebar su explosión. La mecha por proximidad dura 0,9 s; si el cebado procede de daño letal dura 1,2 s. El radio de explosión es de 3 m y afecta al jugador y a otros enemigos. Durante la mecha sigue aceptando empuje, lo que permite desplazarla hacia la horda.

La hoja activa contiene 62 fotogramas de 64 × 64: vuelo y cebado en cuatro direcciones, más una secuencia de estallido de seis fotogramas.

![Hoja de sprites activa de la Polilla](../Media/Avance_2026-10-27/Spritesheets/PolillaReactor.png)

[Fuente editable de la Polilla](../Media/Avance_2026-10-27/Spritesheets/PolillaReactor.aseprite).

![Vuelo, cebado y detonación de la Polilla dentro de Unity](../Media/Avance_2026-10-27/Enemigos/Polilla.gif)

## Gólgora: MataColeccionista

Es inmune al empuje y alterna ataques sin solaparlos. A corta distancia prepara un pisotón de radio 5 m durante 1,2 s. El pisotón causa 40 de daño al jugador y puede alcanzar a los secuaces. Su bombardeo prepara el lanzamiento durante 0,4 s, fija tres posiciones y envía proyectiles con 0,65 s de vuelo. El impacto causa 25 de daño en 2,5 m de radio y deja zonas de cuatro segundos que causan 5 cada 0,5 s. Tras el ataque se recupera durante 0,8 s. Los enfriamientos del pisotón y del bombardeo son de seis y ocho segundos desde sus respectivas preparaciones. Al bajar de la mitad de vida cambia la presentación del núcleo, conservando las estadísticas del encuentro.

El modelo reproduce surgimiento, marcha pesada, martillazo o barrido y lanzamiento de proyectiles. Su muerte desencadena la presentación final y la resolución de victoria cuando el jugador sigue vivo.

![Animación y ataque de Gólgora dentro de Unity](../Media/Avance_2026-10-27/Enemigos/Golgora.gif)

Las hojas de sprites corresponden al Mortero y a la Polilla. Los otros tres enemigos de horda usan la geometría que se ve en los GIFs; Gólgora utiliza un modelo animado. Todas las secuencias de esta página proceden de la arena ejecutándose en Unity, con cámara de detalle y jugador protegido para observar los patrones.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
