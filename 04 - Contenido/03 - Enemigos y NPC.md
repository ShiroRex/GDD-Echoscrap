# Enemigos y NPC

El combate tiene cinco arquetipos de horda, dos élites y Gólgora. Las cifras de esta página corresponden a sus assets actuales. Las hojas de personaje son copias de los atlas usados por el juego; sus GIFs de acciones respetan los tiempos del JSON exportado y muestran las cuatro vistas cuando existen.

| Enemigo | Vida | Velocidad base | Daño principal | XP |
| --- | ---: | ---: | --- | ---: |
| Residual / Esqueleto | 15 HP | 3,5 m/s ±15% | Contacto: 10; recarga de contacto 0,5 s. | 10 |
| Interceptor | 9 HP | 6,2 m/s ±10% | Contacto: 8; recarga de contacto 0,5 s. | 10 |
| Coloso / Wendigo | 80 HP | 1,8 m/s | Contacto o carga: 25. | 50 |
| Mortero Campana | 25 HP | 2,2 m/s ±10% | Contacto: 5; ácido: 5 cada 0,5 s. | 25 |
| Polilla Reactor | 12 HP | 4,8 m/s ±10% | Explosión: 35. | 15 |
| Caballero Fantasma | 110 HP | 2,8 m/s | Barrido sobrenatural: 22. | 65 |
| Centinela de Cobre | 160 HP | 1,7 m/s | Abanico de tres proyectiles físicos: 16 por impacto. | 80 |
| Gólgora / MataColeccionista | 3600 HP demo; 9000 completa | 1,6 m/s en primera fase | Contacto: 30; seis patrones descritos más abajo. | 500 de cierre |

## Residual: Esqueleto

Forma la masa básica de la horda. Persigue al jugador, se separa de otros cuerpos y busca paso entre obstáculos. Los cuatro pulsos base suman sus 15 HP cuando todos aciertan. Acepta empuje y su ataque de contacto se acompaña de la animación de golpe.

Su atlas tiene 144 fotogramas de 96 × 96: reposo, marcha y ataque de ocho, impacto de cuatro y muerte de ocho por dirección.

![Spritesheet activo del Residual / Esqueleto](../Media/Avance_2026-11-04/Spritesheets/Esqueleto.png)

[Fuente editable](../Media/Avance_2026-11-04/Spritesheets/Esqueleto.aseprite).

| Animación | Cuatro direcciones |
| --- | --- |
| Reposo | ![Esqueleto: Reposo](../Media/Avance_2026-11-04/Animaciones/Esqueleto/Idle.gif) |
| Marcha | ![Esqueleto: Marcha](../Media/Avance_2026-11-04/Animaciones/Esqueleto/Walk.gif) |
| Ataque | ![Esqueleto: Ataque](../Media/Avance_2026-11-04/Animaciones/Esqueleto/Attack.gif) |
| Impacto recibido | ![Esqueleto: Impacto recibido](../Media/Avance_2026-11-04/Animaciones/Esqueleto/Hit.gif) |
| Muerte | ![Esqueleto: Muerte](../Media/Avance_2026-11-04/Animaciones/Esqueleto/Death.gif) |

## Interceptor: galgo de chatarra

Anticipa hasta 0,4 s del movimiento del jugador, con una velocidad de referencia limitada a 8 m/s. A corta distancia pasa a perseguir la posición actual. Su menor frenado tras el empuje le permite deslizarse más lejos que el Residual.

El atlas tiene 152 fotogramas de 96 × 96: reposo de seis, marcha de doce, ataque de seis, impacto de cuatro y muerte de diez por dirección. El sprite de galgo sustituye su representación geométrica anterior.

![Spritesheet activo del Interceptor](../Media/Avance_2026-11-04/Spritesheets/Interceptor.png)

[Fuente editable](../Media/Avance_2026-11-04/Spritesheets/Interceptor.aseprite).

| Animación | Cuatro direcciones |
| --- | --- |
| Reposo | ![Interceptor: Reposo](../Media/Avance_2026-11-04/Animaciones/Interceptor/Idle.gif) |
| Marcha | ![Interceptor: Marcha](../Media/Avance_2026-11-04/Animaciones/Interceptor/Walk.gif) |
| Ataque | ![Interceptor: Ataque](../Media/Avance_2026-11-04/Animaciones/Interceptor/Attack.gif) |
| Impacto recibido | ![Interceptor: Impacto recibido](../Media/Avance_2026-11-04/Animaciones/Interceptor/Hit.gif) |
| Muerte | ![Interceptor: Muerte](../Media/Avance_2026-11-04/Animaciones/Interceptor/Death.gif) |

## Coloso: Wendigo

Es pesado, sólido e inmune al empuje. Cuando dispone de carga y el jugador está a 8 m o menos, fija la dirección y muestra un corredor durante 0,8 s. Embiste a 12 m/s durante 0,35 s, unos 4,2 m libres, y se recupera durante 0,6 s. Las paredes detienen el recorrido; la dirección queda comprometida tras el aviso. El intervalo entre cargas se sortea entre cinco y siete segundos.

Preparación y recuperación no infligen contacto. El dash permite atravesar su cuerpo durante la protección.

El atlas de Wendigo tiene 200 fotogramas de 96 × 96: reposo, marcha y preparación de ocho; embestida y recuperación de seis; impacto de cuatro y muerte de diez por dirección.

![Spritesheet activo del Coloso / Wendigo](../Media/Avance_2026-11-04/Spritesheets/Wendigo.png)

[Fuente editable](../Media/Avance_2026-11-04/Spritesheets/Wendigo.aseprite).

| Animación | Cuatro direcciones |
| --- | --- |
| Reposo | ![Wendigo: Reposo](../Media/Avance_2026-11-04/Animaciones/Wendigo/Idle.gif) |
| Marcha | ![Wendigo: Marcha](../Media/Avance_2026-11-04/Animaciones/Wendigo/Walk.gif) |
| Preparación de carga | ![Wendigo: Preparación de carga](../Media/Avance_2026-11-04/Animaciones/Wendigo/Windup.gif) |
| Embestida | ![Wendigo: Embestida](../Media/Avance_2026-11-04/Animaciones/Wendigo/Charge.gif) |
| Recuperación | ![Wendigo: Recuperación](../Media/Avance_2026-11-04/Animaciones/Wendigo/Recovery.gif) |
| Impacto recibido | ![Wendigo: Impacto recibido](../Media/Avance_2026-11-04/Animaciones/Wendigo/Hit.gif) |
| Muerte | ![Wendigo: Muerte](../Media/Avance_2026-11-04/Animaciones/Wendigo/Death.gif) |

## Mortero: Campana

Mantiene una banda de distancia entre 5,5 y 6,5 m y ataca hasta 12 m. Tiene un intervalo de cinco segundos, aviso de 0,45 s y vuelo de 0,85 s. El proyectil deja un charco de radio 1,2 m durante tres segundos, con 5 de daño cada 0,5 s. El director limita los Morteros simultáneos a seis.

Su atlas contiene 112 fotogramas de 64 × 64: flotación y carga de seis, disparo y recuperación de cuatro, y muerte de ocho por dirección.

![Spritesheet activo del Mortero Campana](../Media/Avance_2026-11-04/Spritesheets/Mortero.png)

[Fuente editable](../Media/Avance_2026-11-04/Spritesheets/Mortero.aseprite).

| Animación | Cuatro direcciones |
| --- | --- |
| Flotación | ![Mortero: Flotación](../Media/Avance_2026-11-04/Animaciones/Mortero/Float.gif) |
| Carga del emisor | ![Mortero: Carga del emisor](../Media/Avance_2026-11-04/Animaciones/Mortero/Charge.gif) |
| Disparo | ![Mortero: Disparo](../Media/Avance_2026-11-04/Animaciones/Mortero/Fire.gif) |
| Recuperación | ![Mortero: Recuperación](../Media/Avance_2026-11-04/Animaciones/Mortero/Recover.gif) |
| Muerte | ![Mortero: Muerte](../Media/Avance_2026-11-04/Animaciones/Mortero/Death.gif) |

## Polilla: Reactor

Se aproxima hasta cebar la explosión. La mecha por proximidad dura 0,9 s; si procede de daño letal, 1,2 s. Explota en un radio de 3 m y afecta al jugador y a otros enemigos. Durante la mecha acepta empuje, por lo que puede desplazarse hacia la horda.

Su atlas tiene 62 fotogramas de 64 × 64: vuelo de ocho y cebado de seis por dirección, más seis de estallido compartido.

![Spritesheet activo de la Polilla Reactor](../Media/Avance_2026-11-04/Spritesheets/PolillaReactor.png)

[Fuente editable](../Media/Avance_2026-11-04/Spritesheets/PolillaReactor.aseprite).

| Animación | Cuatro direcciones |
| --- | --- |
| Vuelo | ![PolillaReactor: Vuelo](../Media/Avance_2026-11-04/Animaciones/PolillaReactor/Fly.gif) |
| Cebado | ![PolillaReactor: Cebado](../Media/Avance_2026-11-04/Animaciones/PolillaReactor/Prime.gif) |
| Estallido compartido | ![PolillaReactor: Estallido compartido](../Media/Avance_2026-11-04/Animaciones/PolillaReactor/Burst.gif) |

## Caballero Fantasma

Es uno de los dos élites. Recibe el 25% del daño físico y el daño sobrenatural completo. Su barrido tiene 3,6 m de alcance, arco de 120°, daño sobrenatural de 22, preparación de 0,8 s, recuperación de 0,8 s y recarga de 3,5 s. No causa daño por contacto normal.

Su cuerpo actual usa geometría de presentación y el barrido emplea un efecto animado. No tiene un spritesheet de personaje equivalente a los cinco de horda.

![Caballero Fantasma: captura controlada de Unity](../Media/Avance_2026-11-04/Enemigos/CaballeroFantasma.gif)

## Centinela de Cobre

Recibe el 25% del daño sobrenatural salvo durante la recuperación, cuando se abre su ventana de vulnerabilidad y recibe el daño completo. El daño físico se mantiene completo. Prepara un abanico de tres proyectiles durante un segundo, se recupera dos y recarga en 3,5 s. El alcance es de 11 m y cada impacto causa 16 de daño físico. Es pesado, inmune al empuje y no causa contacto normal.

La presentación anuncia los rayos del abanico y muestra «Vulnerable» en su recuperación. Los misiles tienen animación, iluminación y un límite de luces simultáneas. Su cuerpo actual no tiene spritesheet de personaje.

![Centinela de Cobre: aviso, disparo y recuperación en Unity](../Media/Avance_2026-11-04/Enemigos/CentinelaCobre.gif)

Los élites empiezan a aparecer desde los 90 s y se alternan cada 25 s cuando hay capacidad, con un límite conjunto de dos vivos. Sus efectos de barrido y proyectiles están en [[06 - Registro visual]].

## Gólgora: MataColeccionista

La ronda demo usa 3600 HP y la completa 9000 HP. La entrada ocurre a los tres u ocho minutos y retira la horda ordinaria sin recompensas. El jugador se traslada con protección a una caldera de 22 m de radio cuyo perímetro se cierra. Una presentación con título y rugido precede al combate; la cámara encuadra al jugador y al jefe.

Gólgora usa un modelo 3D animado. Sus efectos de magma tienen recursos propios, pero no existe un spritesheet de personaje del modelo. Los siguientes GIFs proceden de Unity con los patrones invocados de forma aislada, jugador protegido y oleadas detenidas.

| Patrón | Regla de primera fase, salvo donde se indica |
| --- | --- |
| Martillazo | Círculo de 3 m, situado 3 m por delante; 40 de daño, preparación de 1,2 s, recuperación de 1 s y recarga de 6 s. Puede dañar y empujar secuaces. |
| Barrido | Radio de 6 m y arco de 180°; 30 de daño, preparación y recuperación de 1 s, recarga de 7 s. |
| Bombardeo | Tres posiciones previstas; 25 de daño en radio 2,5 m, preparación de 0,7 s, vuelo base de 0,8 s y recarga de 8 s. Deja magma durante 4 s. |
| Cerco de magma | Salto al centro, impacto de llegada y ondas expansivas con un hueco de 60°. Las ondas avanzan a 6 m/s y causan 30 de daño. Una onda en primera fase y tres en segunda, separadas 1,3 s y con el hueco rotado 45°. |
| Fisura de magma | Cinco puntos a lo largo de una línea hacia el jugador, hasta 22 m; 24 de daño en radio 1,4 m. Preparación de 1,1 s, recuperación de 0,8 s, recarga de 10 s y zonas de 3 s. |
| Lluvia de escoria | Segunda fase: cinco impactos alrededor de la posición prevista, 26 de daño en radio 1,8 m. Preparación base de 1,35 s, vuelo base de 0,9 s, recuperación de 1 s y recarga de 14 s antes de los modificadores de fase. Zonas de 3,5 s. |

Las zonas de magma de estos proyectiles causan 5 de daño cada 0,5 s. El cerco empieza a estar disponible a los doce segundos de combate del jefe y después se programa con una recarga base de 18 s al completar la secuencia.

| Ataque en Unity | Registro |
| --- | --- |
| Martillazo | ![Gólgora: martillazo](../Media/Avance_2026-11-04/Enemigos/Golgora_Martillo.gif) |
| Barrido | ![Gólgora: barrido](../Media/Avance_2026-11-04/Enemigos/Golgora_Barrido.gif) |
| Bombardeo | ![Gólgora: bombardeo](../Media/Avance_2026-11-04/Enemigos/Golgora_Bombardeo.gif) |
| Cerco y salto central | ![Gólgora: salto y cerco de magma](../Media/Avance_2026-11-04/Enemigos/Golgora_Cerco.gif) |
| Fisura | ![Gólgora: fisura de magma](../Media/Avance_2026-11-04/Enemigos/Golgora_Fisura.gif) |
| Lluvia de segunda fase | ![Gólgora: lluvia de escoria en segunda fase](../Media/Avance_2026-11-04/Enemigos/Golgora_Lluvia.gif) |

### Segunda fase y resistencia al control

Al llegar al 50% de vida se inicia una transición protegida con rugido. La velocidad pasa a ×1,3; las preparaciones de los ataques que usan ese modificador, a ×0,85; las recuperaciones y recargas, a ×0,75. El cerco emite tres ondas y se incorpora la lluvia de escoria.

Gólgora es inmune al empuje. La ralentización tiene un máximo del 25%. Congelaciones y aturdimientos se reducen al 35% de su duración, con un máximo de 0,6 s y una ventana de inmunidad posterior de 1,5 s. Durante introducción, transición y salto no admite el control normal y no recibe daño. El estado activo del cerco aplica su reducción de daño configurada del 35%.

### Refuerzos y cierre

El director solicita grupos de hasta cuatro cada cuatro segundos. La caldera limita el conjunto a ocho secuaces y apariciones pendientes, con 0,9 s de aviso. La composición rota entre Residual, Residual, Mortero y Polilla. Cerco y transición de fase suspenden esos refuerzos y retiran los presentes.

La muerte de Gólgora cierra el encuentro si el jugador vive. La presentación final incluye la absorción de una gema y 500 XP de resumen, sin nuevas cartas. Si el jugador muere en el mismo paso, prevalece la derrota.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
