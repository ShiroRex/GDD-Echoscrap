# Registro visual

## Procedencia de los medios

Esta edición reúne dos tipos de animación:

- **Previsualizaciones de atlas, 4 de noviembre.** Copias de los PNG activos y GIFs compuestos con sus rectángulos, etiquetas y tiempos JSON. Las acciones de personaje muestran cuatro direcciones a escala 2×, sin interpolación. No incluyen la iluminación ni la simulación del juego.
- **Capturas de Unity, 4 de noviembre.** CombatArena ejecutada desde una copia externa del proyecto, a 960 × 540 y doce imágenes por segundo. Se usaron cámara de detalle, jugador protegido y oleadas detenidas. Los patrones del jefe se invocaron individualmente con sus componentes y ajustes; para la lluvia se activó la segunda fase. Las pantallas de preparación utilizan un inventario de muestra, sin guardar esa colección.

Las capturas corresponden al montaje local basado en 30b88c6. Los PNG mantienen 960 × 540 y los GIFs de Unity se exportan a 640 × 360. La vista de detalle permite leer las animaciones y los efectos; no representa el encuadre de toda una partida ni una medición de rendimiento.

## Enemigos y personajes

[[03 - Enemigos y NPC]] contiene los cinco spritesheets completos y los GIFs por acción de Residual, Interceptor, Coloso, Mortero y Polilla. También muestra el barrido del Caballero, el disparo y vulnerabilidad del Centinela y los seis patrones de Gólgora en Unity. [[02 - Personajes y Animación]] añade el atlas y los cuatro grupos de animaciones del Coleccionista.

## Efectos de enemigos

La biblioteca activa contiene quince clips y 120 fotogramas. La siguiente hoja pertenece a esos efectos, no a los cuerpos de los élites o del jefe.

![Atlas activo de efectos de enemigos](../Media/Avance_2026-11-04/VFX/EnemyEffects_Atlas.png)

Los GIFs de esta tabla son previsualizaciones del atlas. La iluminación de los misiles, las trayectorias y los avisos de daño se observan en las capturas de los enemigos.

| Efecto | Animación del recurso |
| --- | --- |
| Golpe del Residual | ![Golpe del Residual](../Media/Avance_2026-11-04/VFX/Enemigos/ResidualSlash.gif) |
| Fragmentos de hueso | ![Fragmentos de hueso](../Media/Avance_2026-11-04/VFX/Enemigos/BoneChips.gif) |
| Golpe del Interceptor | ![Golpe del Interceptor](../Media/Avance_2026-11-04/VFX/Enemigos/InterceptorSlash.gif) |
| Estela del Interceptor | ![Estela del Interceptor](../Media/Avance_2026-11-04/VFX/Enemigos/InterceptorWake.gif) |
| Polvo del Coloso | ![Polvo del Coloso](../Media/Avance_2026-11-04/VFX/Enemigos/ColossusDust.gif) |
| Impacto del Coloso | ![Impacto del Coloso](../Media/Avance_2026-11-04/VFX/Enemigos/ColossusImpact.gif) |
| Barrido espectral | ![Barrido espectral](../Media/Avance_2026-11-04/VFX/Enemigos/SpectralSweep.gif) |
| Misil de cobre | ![Misil de cobre](../Media/Avance_2026-11-04/VFX/Enemigos/CopperMissile.gif) |
| Salida del disparo de cobre | ![Salida del disparo de cobre](../Media/Avance_2026-11-04/VFX/Enemigos/CopperMuzzle.gif) |
| Impacto de cobre | ![Impacto de cobre](../Media/Avance_2026-11-04/VFX/Enemigos/CopperImpact.gif) |
| Humo de la Polilla | ![Humo de la Polilla](../Media/Avance_2026-11-04/VFX/Enemigos/MothSmoke.gif) |
| Meteoro de magma | ![Meteoro de magma](../Media/Avance_2026-11-04/VFX/Enemigos/MagmaMeteor.gif) |
| Impacto de magma | ![Impacto de magma](../Media/Avance_2026-11-04/VFX/Enemigos/MagmaImpact.gif) |
| Zona de magma | ![Zona de magma](../Media/Avance_2026-11-04/VFX/Enemigos/MagmaPuddle.gif) |
| Barrido de magma | ![Barrido de magma](../Media/Avance_2026-11-04/VFX/Enemigos/MagmaSweep.gif) |

## Torreta centinela

La torreta actual usa trípode y cañón separados, orientación del cañón, boca de salida y retroceso. La secuencia de direcciones es una previsualización técnica del recurso preparada el 3 de noviembre; el disparo siguiente se capturó en Unity el 4.

![Torreta: previsualización técnica de sus direcciones](../Media/Avance_2026-11-04/VFX/Torreta_Direcciones.gif)

![Torreta disparando en la arena de Unity](../Media/Avance_2026-11-04/VFX/Torreta_Combate.gif)

## Efectos de mejoras

Los siguientes GIFs son capturas de Unity del 26 de octubre, conservadas para mostrar efectos de mejoras. Proceden de escenas controladas y recortes de cámara de 1280 × 720. Su fecha y encuadre se mantienen como contexto; no muestran la iluminación exterior renovada de noviembre.

### Prisma voltaico

Los enlaces siguen el cuarto pulso y recorren objetivos sucesivos.

![Prisma voltaico: captura de Unity del 26 de octubre](../Media/Avance_2026-10-27/VFX/Electricity.gif)

### Frío y congelación

El tinte, cristales y niebla acompañan ralentización y congelación.

![Frío y congelación: captura de Unity del 26 de octubre](../Media/Avance_2026-10-27/VFX/Ice.gif)

### Minas y explosiones

Expansión, humo y fragmentos acompañan la detonación; el daño lo resuelve la simulación.

![Minas y explosiones: captura de Unity del 26 de octubre](../Media/Avance_2026-10-27/VFX/Explosion.gif)

### Nova de plasma

La curación sobrante puede desencadenar la expansión alrededor del jugador.

![Nova de plasma: captura de Unity del 26 de octubre](../Media/Avance_2026-10-27/VFX/Plasma.gif)

### Nanobots

La superficie de energía y sus ondas acompañan las reglas de regeneración y protección.

![Nanobots: captura de Unity del 26 de octubre](../Media/Avance_2026-10-27/VFX/Shield.gif)

### Sierras orbitales

Las cuchillas, estelas y chispas permiten seguir el giro y sus contactos.

![Sierras orbitales: captura de Unity del 26 de octubre](../Media/Avance_2026-10-27/VFX/Saws.gif)

### Pulso colector

Los objetos conservan su valor durante el vuelo y conceden XP o curación al llegar.

![Pulso colector: captura de Unity del 26 de octubre](../Media/Avance_2026-10-27/VFX/Collection.gif)

### Onda sísmica

La expansión señala el efecto de daño y control al terminar el dash.

![Onda sísmica: captura de Unity del 26 de octubre](../Media/Avance_2026-10-27/VFX/Shockwave.gif)

### Números de daño

Los números siguen arcos; el físico usa amarillo y naranja crítico y el sobrenatural rosa violeta y morado crítico.

![Números de daño: captura de Unity del 26 de octubre](../Media/Avance_2026-10-27/VFX/NumerosDeDano.gif)

## Alcance de estas imágenes

Los GIFs permiten revisar fotogramas, anticipación, trayectoria e impacto visual. Las cifras de daño, la cadencia, probabilidades y condiciones de activación se contrastan con las reglas del juego. Las animaciones no contienen audio y no certifican un mínimo de FPS.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
