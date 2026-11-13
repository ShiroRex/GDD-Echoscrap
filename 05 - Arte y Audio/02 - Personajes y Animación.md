# Personajes y animación

Esta página reúne las fichas visuales del Coleccionista, los cinco enemigos de horda, los dos élites, Gólgora y las quince entidades de la colección. Cada enemigo de horda incluye su spritesheet, la fuente editable y los GIFs de todas sus acciones.

Las reglas y cifras de combate se consultan en [[03 - Enemigos y NPC]] y [[04 - Objetos y Coleccionables]]. La identidad, historia de origen y papel general de cada personaje se recogen en [[03 - Personajes]].

## Proporciones en reposo

![Comparación a escala del Coleccionista y los ocho enemigos](../Media/Avance_2026-11-07/Enemigos/Proporciones.png)

La lámina reúne capturas de Unity con cámara ortográfica inclinada a `35,5°`, igual distancia y escala, y una línea común de suelo. El Coleccionista equivale a `1,00×`. La tabla mide la altura visible de una pose de reposo o flotación, con el cuerpo de Gólgora aislado de su martillo (`432` píxeles frente a `173` del Coleccionista).

| Personaje | Altura visible frente al Coleccionista |
| --- | ---: |
| Coleccionista | 1,00× |
| Residual / Esqueleto | 1,02× |
| Interceptor / Galgo | 0,61× |
| Mortero Campana | 1,10× |
| Polilla Reactor | 0,76× |
| Coloso / Wendigo | 1,27× |
| Caballero Fantasma | 1,31× |
| Centinela de Cobre | 1,28× |
| Gólgora | 2,50× |

El Esqueleto toma al Coleccionista como referencia y los dos élites toman al Wendigo. El Galgo creció un `10%` respecto a su escala inicial. Gólgora tiene una altura visual objetivo de `2,50×` en reposo; la cámara de la caldera reserva espacio para el cuerpo ampliado y el modelo conserva su escala al caer.

## Recursos actuales y estado de integración

| Personaje o arquetipo | Recurso y estado | Animaciones por dirección |
| --- | --- | --- |
| Coleccionista | Sprite integrado; atlas de 108 fotogramas de 64 × 64. | Reposo 8, marcha 8, dash 3 y muerte 8. |
| Residual / Esqueleto | Sprite integrado; atlas de 144 fotogramas de 96 × 96. | Reposo 8, marcha 8, ataque 8, impacto 4 y muerte 8. |
| Interceptor / galgo de chatarra | Sprite integrado; atlas de 152 fotogramas de 96 × 96. | Reposo 6, marcha 12, ataque 6, impacto 4 y muerte 10. |
| Coloso / Wendigo | Sprite integrado; atlas de 200 fotogramas de 96 × 96. | Reposo 8, marcha 8, preparación 8, carga 6, recuperación 6, impacto 4 y muerte 10. |
| Mortero Campana | Sprite integrado; atlas de 112 fotogramas de 64 × 64. | Flotación 6, carga 6, disparo 4, recuperación 4 y muerte 8. |
| Polilla Reactor | Sprite integrado; atlas de 62 fotogramas de 64 × 64. | Vuelo 8 y cebado 6; estallido común de 6. |
| Caballero Fantasma | Sprite integrado; atlas de 224 fotogramas de 96 × 96, con normales y emisión. | Reposo 8, marcha 12, preparación 8, ataque 6, recuperación 6, impacto 4 y muerte 12. |
| Centinela de Cobre / gólem | Sprite integrado; atlas de 288 fotogramas de 96 × 96, con normales y emisión. | Reposo 8, marcha 12, apertura 6, preparación 8, disparo 8, vulnerabilidad 8, cierre 6, impacto 4 y muerte 12. |
| Gólgora / MataColeccionista | Modelo 3D animado integrado y VFX de magma. | Entrada, locomoción, martillazo, barrido, lanzamiento, salto y presentación final. |
| Entidades de la colección (15) | Sprites 2D en pixel art integrados en arena, vitrinas y selectores. | Vistas estáticas o ciclos cortos de reposo según entidad. |

## Cómo leer las previsualizaciones

Los GIFs de atlas colocan juntas las cuatro vistas cardinales y usan sus fotogramas y tiempos exportados a escala `2×` sin interpolación. Las condiciones técnicas de las capturas en motor se detallan en [[06 - Registro visual]].

## El Coleccionista

Su boceto conceptual a lápiz define la silueta de explorador con capucha y manto raído sujetos por un broche de cruz en el pecho, junto con una máscara de pico inspirada en los médicos de la peste. En el juego, el farol sigue al personaje y el haz acompaña al apuntado automático; el sprite mantiene un plano vertical con proporciones fijas en el mundo y se apoya sobre el terreno.

![Boceto a mano del Coleccionista|260](../Media/ConceptArt/Coleccionista.jpeg)

![Atlas activo del Coleccionista](../Media/Avance_2026-11-04/Spritesheets/Coleccionista.png)

[Fuente editable del Coleccionista](../Media/Avance_2026-11-04/Spritesheets/Coleccionista.aseprite).

| Animación | Cuatro direcciones |
| --- | --- |
| Reposo | ![Coleccionista: Reposo](../Media/Avance_2026-11-04/Animaciones/Coleccionista/Idle.gif) |
| Marcha | ![Coleccionista: Marcha](../Media/Avance_2026-11-04/Animaciones/Coleccionista/Walk.gif) |
| Dash | ![Coleccionista: Dash](../Media/Avance_2026-11-04/Animaciones/Coleccionista/Dash.gif) |
| Muerte | ![Coleccionista: Muerte](../Media/Avance_2026-11-04/Animaciones/Coleccionista/Death.gif) |

## Residual: Esqueleto

Es la masa básica de la horda. Su representación es un esqueleto animado en cuatro direcciones con atlas de 144 fotogramas de `96 × 96`: reposo, marcha y ataque de ocho, impacto de cuatro y muerte de ocho por dirección.

![Boceto a mano del Residual / Esqueleto|220](../Media/ConceptArt/Esqueleto-enemigo.JPG)

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

Perseguidor rápido de la horda. Su silueta baja y la marcha veloz distinguen su persecución. El atlas tiene 152 fotogramas de `96 × 96`: reposo de seis, marcha de doce, ataque de seis, impacto de cuatro y muerte de diez por dirección.

![Boceto a mano del Galgo / Interceptor|240](../Media/ConceptArt/Galgo_enemigo.JPG)

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

Enemigo pesado que abre corredores con su embestida. Su diseño parte de un boceto conceptual a lápiz con cráneo astado de ciervo, costillar expuesto, manto de pelaje y garras largas. El atlas de Wendigo tiene 200 fotogramas de `96 × 96`: reposo, marcha y preparación de ocho; embestida y recuperación de seis; impacto de cuatro y muerte de diez por dirección.

![Boceto a mano del Coloso / Wendigo|240](../Media/ConceptArt/Wendigo.jpeg)

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

Artillería de la horda. Su diseño conceptual parte de una campana flotante con aletas emisoras de brillo verde en la corona, jaula interior y una manguera lateral por la que escurre el ácido. En el juego encadena flotación, carga del emisor, disparo y recuperación en un atlas de 112 fotogramas de `64 × 64` (flotación y carga de seis, disparo y recuperación de cuatro, y muerte de ocho por dirección).

![Boceto a mano del Mortero Campana|280](../Media/ConceptArt/Campana_Mortero.JPG)

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

Enemigo explosivo de la horda que arma su espoleta a `1,6 m` del jugador. Su boceto conceptual combina un cuerpo esférico de horno o reactor con ranuras frontales, ojos compuestos y alas mecánicas. Su atlas tiene 62 fotogramas de `64 × 64`: vuelo de ocho y cebado de seis por dirección, más seis de estallido compartido.

![Boceto a mano de la Polilla Reactor|280](../Media/ConceptArt/Polilla.JPG)

![Spritesheet activo de la Polilla Reactor](../Media/Avance_2026-11-04/Spritesheets/PolillaReactor.png)

[Fuente editable](../Media/Avance_2026-11-04/Spritesheets/PolillaReactor.aseprite).

| Animación | Cuatro direcciones |
| --- | --- |
| Vuelo | ![PolillaReactor: Vuelo](../Media/Avance_2026-11-04/Animaciones/PolillaReactor/Fly.gif) |
| Cebado | ![PolillaReactor: Cebado](../Media/Avance_2026-11-04/Animaciones/PolillaReactor/Prime.gif) |
| Estallido compartido | ![PolillaReactor: Estallido compartido](../Media/Avance_2026-11-04/Animaciones/PolillaReactor/Burst.gif) |

## Caballero Fantasma

Su boceto conceptual define una armadura pesada con hombreras de púas, cabeza espectral envuelta en llamas oscuras y espadón de barrido. En Unity cuenta con sprite integrado en cuatro direcciones con normal map y emisión (224 fotogramas de `96 × 96`). La preparación sigue los `0,8 s` del barrido, y el ataque junto con la recuperación se enlazan durante los `0,8 s` posteriores.

![Boceto a mano del Caballero Fantasma|280](../Media/ConceptArt/Caballero_Fantasma.JPG)

![Atlas de Caballero Fantasma](../Media/Avance_2026-11-05/Spritesheets/CaballeroFantasma.png)

[Fuente editable](../Media/Avance_2026-11-05/Spritesheets/CaballeroFantasma.aseprite).

| Animación | Cuatro direcciones |
| --- | --- |
| Reposo | ![Caballero Fantasma: Reposo](../Media/Avance_2026-11-05/Animaciones/CaballeroFantasma/Idle.gif) |
| Marcha | ![Caballero Fantasma: Marcha](../Media/Avance_2026-11-05/Animaciones/CaballeroFantasma/Walk.gif) |
| Preparación | ![Caballero Fantasma: Preparación](../Media/Avance_2026-11-05/Animaciones/CaballeroFantasma/Windup.gif) |
| Ataque | ![Caballero Fantasma: Ataque](../Media/Avance_2026-11-05/Animaciones/CaballeroFantasma/Attack.gif) |
| Recuperación | ![Caballero Fantasma: Recuperación](../Media/Avance_2026-11-05/Animaciones/CaballeroFantasma/Recovery.gif) |
| Impacto | ![Caballero Fantasma: Impacto](../Media/Avance_2026-11-05/Animaciones/CaballeroFantasma/Hit.gif) |
| Muerte | ![Caballero Fantasma: Muerte](../Media/Avance_2026-11-05/Animaciones/CaballeroFantasma/Death.gif) |

![Ciclo de combate integrado, captura de Unity del 5 de noviembre](../Media/Avance_2026-11-05/Enemigos/CaballeroFantasma_Integrado.gif)

## Centinela de Cobre: gólem

Su boceto conceptual define un autómata pesado con bobinas eléctricas en los hombros y compuertas pectorales que se abren para disparar. En Unity cuenta con sprite integrado en cuatro direcciones con normal map y emisión (288 fotogramas de `96 × 96`). Apertura de `0,3 s` y preparación de `0,7 s` preceden al disparo; disparo de `0,4 s`, vulnerabilidad de `1,3 s` y cierre de `0,3 s` ocupan los `2,0 s` de recuperación.

![Boceto a mano del Centinela de Cobre|280](../Media/ConceptArt/Golem_Cobre.JPG)

![Atlas de Centinela de Cobre: gólem](../Media/Avance_2026-11-05/Spritesheets/CentinelaCobre.png)

[Fuente editable](../Media/Avance_2026-11-05/Spritesheets/CentinelaCobre.aseprite).

| Animación | Cuatro direcciones |
| --- | --- |
| Reposo | ![Centinela de Cobre: gólem: Reposo](../Media/Avance_2026-11-05/Animaciones/CentinelaCobre/Idle.gif) |
| Marcha | ![Centinela de Cobre: gólem: Marcha](../Media/Avance_2026-11-05/Animaciones/CentinelaCobre/Walk.gif) |
| Apertura | ![Centinela de Cobre: gólem: Apertura](../Media/Avance_2026-11-05/Animaciones/CentinelaCobre/Open.gif) |
| Preparación | ![Centinela de Cobre: gólem: Preparación](../Media/Avance_2026-11-05/Animaciones/CentinelaCobre/Windup.gif) |
| Disparo | ![Centinela de Cobre: gólem: Disparo](../Media/Avance_2026-11-05/Animaciones/CentinelaCobre/Fire.gif) |
| Vulnerabilidad | ![Centinela de Cobre: gólem: Vulnerabilidad](../Media/Avance_2026-11-05/Animaciones/CentinelaCobre/Vulnerable.gif) |
| Cierre | ![Centinela de Cobre: gólem: Cierre](../Media/Avance_2026-11-05/Animaciones/CentinelaCobre/Close.gif) |
| Impacto | ![Centinela de Cobre: gólem: Impacto](../Media/Avance_2026-11-05/Animaciones/CentinelaCobre/Hit.gif) |
| Muerte | ![Centinela de Cobre: gólem: Muerte](../Media/Avance_2026-11-05/Animaciones/CentinelaCobre/Death.gif) |

![Ciclo de combate integrado, captura de Unity del 5 de noviembre](../Media/Avance_2026-11-05/Enemigos/CentinelaCobre_Integrado.gif)

## Gólgora: MataColeccionista

Jefe final de la expedición, representado mediante un modelo 3D animado con efectos de magma propios.

### Ataques dentro de la arena

| Ataque en Unity | Registro |
| --- | --- |
| Martillazo | ![Gólgora: martillazo](../Media/Avance_2026-11-04/Enemigos/Golgora_Martillo.gif) |
| Barrido | ![Gólgora: barrido](../Media/Avance_2026-11-04/Enemigos/Golgora_Barrido.gif) |
| Bombardeo | ![Gólgora: bombardeo](../Media/Avance_2026-11-04/Enemigos/Golgora_Bombardeo.gif) |
| Cerco y salto central | ![Gólgora: salto y cerco de magma](../Media/Avance_2026-11-04/Enemigos/Golgora_Cerco.gif) |
| Fisura | ![Gólgora: fisura de magma](../Media/Avance_2026-11-04/Enemigos/Golgora_Fisura.gif) |
| Lluvia de segunda fase | ![Gólgora: lluvia de escoria en segunda fase](../Media/Avance_2026-11-04/Enemigos/Golgora_Lluvia.gif) |

Los daños, tiempos, fases, refuerzos y reglas de cierre están en [[03 - Enemigos y NPC]]. Los efectos de ataques y mejoras se reúnen en [[06 - Registro visual]].

## Entidades de la colección

Las quince entidades coleccionables se presentan en el mundo y en las vitrinas de la Bóveda mediante sprites 2D en pixel art exportados a doble escala sin interpolación. Sus historias de origen se documentan en [[03 - Personajes]] y sus rarezas, fuentes y poderes en [[04 - Objetos y Coleccionables]].

| Entidad            | Rareza     | Previsualización del sprite                                                                                                                          | Fuente editable                                                                                     |
| ------------------ | ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| Yanikua            | Mítico     | ![Sprite de Yanikua\|140](../Media/Coleccionables/Yanikua.png)                                                                                       | [Yanikua.aseprite](../Sprites/Yanikua.aseprite)                                                     |
| Lisa               | Legendario | ![Sprite de Lisa\|150](../Media/Coleccionables/Lisa.png)                                                                                             | [Lisa.aseprite](../Sprites/Lisa.aseprite)                                                           |
| Sirena             | Legendario | ![Sprite de Sirena](../Media/Coleccionables/Sirena_adelante_atras.png)                                                                               | [Sirena_adelante_atras.aseprite](../Sprites/Sirena_adelante_atras.aseprite)                         |
| Hada               | Épico      | ![Sprite de Hada](../Media/Coleccionables/Hada_atras_adelante.png)                                                                                   | [Hada_atras_adelante.aseprite](../Sprites/Hada_atras_adelante.aseprite)                             |
| Murciélago Cristal | Épico      | ![Sprite de Murciélago Cristal](../Media/Coleccionables/Murcielagocristal_atras_adelante.png)                                                        | [Murcielagocristal_atras_adelante.aseprite](../Sprites/Murcielagocristal_atras_adelante.aseprite)   |
| Shireen            | Especial   | ![Sprite de Shireen\|220](../Media/Coleccionables/Shireen.png)                                                                                       | [Shireen.aseprite](../Sprites/Shireen.aseprite)                                                     |
| Soldier            | Especial   | ![Sprite de Soldier](../Media/Coleccionables/Soldier_zombie-Sheet.png)                                                                               | [Soldier_zombie-Sheet.aseprite](../Sprites/Soldier_zombie-Sheet.aseprite)                           |
| Conde Alucard      | Especial   | ![Sprite de Conde Alucard](../Media/Coleccionables/Conde_Alucard.png)                                                                                | [Conde_Alucard.aseprite](../Sprites/Conde_Alucard.aseprite)                                         |
| Mago               | Raro       | ![Sprite de Mago](../Media/Coleccionables/Mago.png)                                                                                                  | [Mago.aseprite](../Sprites/Mago.aseprite)                                                           |
| Demonio            | Raro       | ![Sprite de Demonio (frente)\|240](../Media/Coleccionables/Demonio.png)<br>![Sprite de Demonio (espalda)\|240](../Media/Coleccionables/Demonio2.png) | [Demonio.aseprite](../Sprites/Demonio.aseprite) · [Demonio2.aseprite](../Sprites/Demonio2.aseprite) |
| Joel               | Raro       | ![Sprite de Joel](../Media/Coleccionables/Jov4.png)                                                                                                  | [Jov4.aseprite](../Sprites/Jov4.aseprite)                                                           |
| Chico Emo          | Raro       | ![Sprite de Chico Emo\|130](../Media/Coleccionables/Chico_Emo.png)                                                                                   | [Chico_Emo.aseprite](../Sprites/Chico_Emo.aseprite)                                                 |
| Lobito             | Común      | ![Sprite de Lobito\|220](../Media/Coleccionables/Lobito.png)                                                                                         | [Lobito.aseprite](../Sprites/Lobito.aseprite)                                                       |
| Basket             | Común      | ![Sprite de Basket](../Media/Coleccionables/basket.png)                                                                                              | [basket.aseprite](../Sprites/basket.aseprite)                                                       |
| Reaper             | Común      | ![Sprite de Reaper](../Media/Coleccionables/Reaper.png)                                                                                              | [Reaper.aseprite](../Sprites/Reaper.aseprite)                                                       |

### Bocetos conceptuales a mano

Dibujos a lápiz realizados durante el diseño visual de las entidades antes de su paso a pixel art, resguardados en `Media/ConceptArt/`.

| Entidad | Boceto a mano |
| --- | --- |
| Yanikua | ![Boceto de Yanikua\|140](../Media/ConceptArt/Yanikua.jpeg) |
| Lisa | ![Boceto de Lisa\|140](../Media/ConceptArt/Lisa.jpeg) |
| Sirena | ![Boceto de Sirena\|140](../Media/ConceptArt/Sirena.JPG) |
| Hada | ![Boceto de Hada\|140](../Media/ConceptArt/Hada_Costura.jpeg) |
| Murciélago Cristal | ![Boceto de Murciélago Cristal\|140](../Media/ConceptArt/Murcielago_Cristal.jpeg) |
| Shireen | ![Boceto de Shireen\|140](../Media/ConceptArt/Shireen.jpeg) |
| Soldier | ![Boceto de Soldier\|140](../Media/ConceptArt/Soldier.JPG) |
| Conde Alucard | ![Boceto de Conde Alucard\|140](../Media/ConceptArt/Conde_Alucard.jpeg) |
| Mago | ![Boceto de Mago\|140](../Media/ConceptArt/Mago.JPG) |
| Demonio | ![Boceto de Demonio\|140](../Media/ConceptArt/Demonio.JPG) |
| Joel | ![Boceto de Joel\|140](../Media/ConceptArt/Joel_jov4.JPG) |
| Chico Emo | ![Boceto de Chico Emo\|140](../Media/ConceptArt/Chico_Emo.JPG) |
| Lobito | ![Boceto de Lobito\|140](../Media/ConceptArt/Lobito.jpeg) |
| Basket | ![Boceto de Basket\|140](../Media/ConceptArt/Basket.JPG) |
| Reaper | ![Boceto de Reaper\|140](../Media/ConceptArt/Reaper.JPG) |

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
