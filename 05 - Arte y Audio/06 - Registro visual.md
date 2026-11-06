# Registro visual

## Procedencia de los medios

El registro reúne previsualizaciones de recursos y capturas de ejecución, con su procedencia y fecha:

- **Previsualizaciones de atlas, 4 de noviembre.** Copias de los PNG activos y GIFs compuestos con sus rectángulos, etiquetas y tiempos JSON. Las acciones de personaje muestran cuatro direcciones a escala 2×, sin interpolación. No incluyen la iluminación ni la simulación del juego.
- **Capturas de Unity, 4 de noviembre.** CombatArena ejecutada desde una copia externa del proyecto, a 960 × 540 y doce imágenes por segundo. Se usaron cámara de detalle, jugador protegido y oleadas detenidas. Los patrones del jefe se invocaron individualmente con sus componentes y ajustes; para la lluvia se activó la segunda fase. Las pantallas de preparación utilizan un inventario de muestra, sin guardar esa colección.

- **Capturas de Unity, 5 de noviembre.** Veintiuna secuencias nuevas de linterna, mejoras, daño, fusión y portal del menú. Se ejecutaron desde una copia externa del montaje local basado en b9e3136, con cámara de detalle a 960 × 540 (1280 × 720 en números de daño) y doce imágenes por segundo, salvo el Micro-ensamblador y los números de daño, registrados a 24; los GIFs se exportan a 768 × 432. Las oleadas están detenidas, los objetivos tienen locomoción desactivada y se activan eventos y estados reales del código para aislar cada efecto. Algunos disparadores se repiten para facilitar su revisión. El jugador está protegido salvo el daño controlado de Nanobots; la fusión utiliza ingredientes de muestra y el guardado permanece desactivado.

Los registros del 4 de noviembre corresponden al montaje local basado en 30b88c6 y conservan sus dimensiones: PNG de 960 × 540 y GIF de Unity de 640 × 360. La cámara de detalle permite leer animaciones y efectos; no representa el encuadre de toda una partida ni una medición de rendimiento.

## Enemigos y personajes

[[02 - Personajes y Animación]] reúne el atlas y los GIFs del Coleccionista, los cinco spritesheets completos de horda con todas sus acciones y las capturas de los seis patrones de Gólgora. Caballero Fantasma y Centinela de Cobre (gólem) tienen su integración de sprite pendiente. Las capturas geométricas de esos élites se conservan como referencia fechada en [[03 - Enemigos y NPC]], junto a sus reglas de combate.

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

## Linterna

Las dos capturas usan los mismos objetivos inmóviles y la misma cámara. La presentación cambia con las mejoras de la linterna: alcance y apertura, cadencia, intensidad y color. Estas variantes reutilizan el haz y los impactos del arma.

### Linterna base

La ráfaga muestra el haz, los pulsos y las reacciones de los enemigos alcanzados.

![Linterna base: captura de Unity del 5 de noviembre](../Media/Avance_2026-11-05/VFX/Linterna.gif)

### Rayo solar

La mejora legendaria cambia la luz a dorado y mantiene los pulsos sin el descanso normal de la ráfaga. La secuencia permite comparar su continuidad con la linterna base.

![Rayo solar: captura de Unity del 5 de noviembre](../Media/Avance_2026-11-05/VFX/RayoSolar.gif)

## Efectos de mejoras

Estas capturas del 5 de noviembre sustituyen las muestras de mejoras del 26 de octubre. Cada secuencia identifica la acción que se está mostrando; los valores y condiciones de adquisición están en [[05 - Progresión]].

### Prisma voltaico

Tras un impacto del cuarto pulso, la descarga enlaza objetivos sucesivos y produce destellos eléctricos en los alcanzados. Los impactos iniciales se repiten durante la captura para mostrar la cadena.

![Prisma voltaico: descarga y enlaces entre objetivos](../Media/Avance_2026-11-05/VFX/PrismaVoltaico.gif)

### Frío y congelación

El primer impacto aplica la ralentización. Los pulsos siguientes alcanzan el umbral de congelación y añaden escarcha, partículas y niebla fría al objetivo.

![Freón residual y Cero absoluto: ralentización y congelación](../Media/Avance_2026-11-05/VFX/FrioCongelacion.gif)

### Ventisca

La mejora emite una onda de frío desde el jugador y congela a los objetivos alcanzados. Para aislar la emisión se adelanta su temporizador mediante el código de ejecución; el GIF no representa el intervalo completo entre activaciones.

![Ventisca: onda de frío y objetivos congelados](../Media/Avance_2026-11-05/VFX/Ventisca.gif)

### Quiebre de hielo

La muerte de un enemigo congelado libera seis esquirlas radiales. La captura activa el estado de congelación y resuelve el golpe letal para mostrar el disparador y la salida de los proyectiles.

![Quiebre de hielo: esquirlas tras una muerte congelada](../Media/Avance_2026-11-05/VFX/QuiebreHielo.gif)

### Minas de chatarra

La mina se coloca en el suelo y explota al detectar un enemigo cercano. La captura adelanta el temporizador de colocación; el radio visible pertenece a la explosión.

![Minas de chatarra: colocación y explosión](../Media/Avance_2026-11-05/VFX/Minas.gif)

### Termobárica

Después de la explosión inicial aparece una implosión que se contrae y atrae enemigos. La explosión de Cataclismo se utiliza como fuente para demostrar esta combinación.

![Termobárica: explosión inicial e implosión secundaria](../Media/Avance_2026-11-05/VFX/Termobarica.gif)

### Nova de plasma

La curación que excede la vida máxima acumula la condición de activación y emite una nova alrededor del jugador. La captura suministra curación sobrante para mostrar varias activaciones.

![Nova de plasma: expansión por curación sobrante](../Media/Avance_2026-11-05/VFX/NovaPlasma.gif)

### Curación y reparación

El efecto de reparación acompaña la recuperación efectiva de vida. Más vida, Sifón, Botiquín mejor y la regeneración de Nanobots comparten esta presentación cuando producen una curación.

![Curación: efecto compartido de reparación](../Media/Avance_2026-11-05/VFX/Curacion.gif)

### Nanobots: protección de emergencia

Un daño controlado deja al jugador por debajo del umbral y activa el escudo temporal. La captura muestra su aparición y desaparición; la regeneración utiliza el efecto de curación anterior.

![Nanobots: activación del escudo de emergencia](../Media/Avance_2026-11-05/VFX/Nanobots.gif)

### Micro-ensamblador

La recogida del grupo de gemas requerido dispara un proyectil guiado hacia un enemigo. Se observan el vuelo, la estela y el impacto; las gemas se suministran en la situación de captura.

![Micro-ensamblador: proyectil guiado, estela e impacto](../Media/Avance_2026-11-05/VFX/MicroEnsamblador.gif)

### Sierras orbitales

Las sierras giran alrededor del Coleccionista. Sus arcos y chispas acompañan el contacto con los enemigos colocados cerca del recorrido.

![Sierras orbitales: giro y efectos de contacto](../Media/Avance_2026-11-05/VFX/Sierras.gif)

### Pulso colector

El remolino de recogida acompaña el vuelo de gemas hacia el jugador. La animación mantiene el valor real de las recogidas y usa un conjunto de gemas de muestra.

![Pulso colector: remolino y recogida de gemas](../Media/Avance_2026-11-05/VFX/PulsoColector.gif)

### Onda sísmica

El evento de llegada del dash emite una expansión radial de daño y aturdimiento. La captura aísla la llegada y repite el disparador para revisar la onda.

![Onda sísmica: expansión al terminar el dash](../Media/Avance_2026-11-05/VFX/OndaSismica.gif)

### Holo-señuelo

El inicio del dash deja una copia del Coleccionista. El jugador se aparta para hacerla visible y, al agotarse su duración, la copia desaparece con una implosión. Los enemigos de captura permanecen inmóviles; este registro muestra el efecto visual, no una prueba de su persecución.

![Holo-señuelo: copia temporal e implosión](../Media/Avance_2026-11-05/VFX/HoloSenuelo.gif)

### Cataclismo

La explosión nace en el punto de salida y la cuchilla se lanza desde la llegada. La posición del jugador se desplaza de forma controlada entre ambos eventos para mostrar los dos componentes.

![Cataclismo: explosión de salida y cuchilla de llegada](../Media/Avance_2026-11-05/VFX/Cataclismo.gif)

## Números de daño

Los valores siguen arcos y distinguen daño físico y sobrenatural, con variantes de impacto normal y crítico, además del indicador `Resistido` en resistencias física y sobrenatural y el rótulo `EJECUTADO` al cumplirse el umbral de ejecución. La captura provoca impactos controlados para revisar los siete casos en pantalla.

![Números de daño: canales físicos y sobrenaturales, críticos, resistencias y ejecución](../Media/Avance_2026-11-05/VFX/NumerosDano.gif)

## Fusión y portal

### Aura de la máquina de fusión

Las partículas ascienden por los costados de la máquina y dejan libre la puerta. La secuencia muestra el efecto ambiental en la bóveda.

![Máquina de fusión: aura de partículas](../Media/Avance_2026-11-05/VFX/FusionAura.gif)

### Apertura y revelación de la fusión

Al abrir una fusión lista se eleva la puerta, aparece humo y se muestra la entidad resultante. La captura utiliza cuatro copias de muestra y adelanta la espera de producción; la apertura y su humo se ejecutan con sus componentes del juego.

![Fusión: apertura de la máquina y humo de revelación](../Media/Avance_2026-11-05/VFX/FusionApertura.gif)

### Portal del menú

El vórtice animado y las entidades en órbita forman el fondo del menú inicial. Este GIF registra la interfaz real con su tiempo de presentación recorrido de forma controlada, para observar el ciclo del vórtice y las órbitas que una captura estática no muestra.

![Menú inicial: portal animado y entidades en órbita](../Media/Avance_2026-11-05/VFX/PortalMenu.gif)

## Efectos compartidos y mejoras de estadísticas

Las 35 mejoras no necesitan 35 animaciones distintas. El catálogo de [[05 - Progresión]] conserva sus reglas; esta tabla identifica la presentación compartida y evita confundir una mejora sin VFX exclusivo con una captura ausente.

| Mejora o grupo | Presentación |
| --- | --- |
| Más alcance, Pulso rápido y Golpe potente | Reutilizan el haz, los impactos y la presentación de la linterna; cambian su apertura, alcance, cadencia, color o intensidad. |
| Dash ágil | Reutiliza la animación y las imágenes residuales del dash; modifica velocidad y recarga. Sus animaciones están en [[02 - Personajes y Animación]]. |
| Más vida, Sifón y Botiquín mejor | Reutilizan curación y reparación; el HUD expresa los cambios de vida. |
| Imán | Reutiliza el vuelo de recogida de gemas y botiquines. |
| Chatarrero | Cambia la XP recibida y su lectura en el HUD; no añade un efecto de combate propio. |
| Sintonizador | Se representa mediante reintentos y ofertas en la interfaz de cartas. |
| Óptica precisa y Calibración letal | Reutilizan los destellos y números críticos; cambian sus probabilidades o valores. |
| Ejecución | Muestra el rótulo `EJECUTADO` en los números de daño, resuelve la muerte del enemigo al cumplir su condición y reutiliza la presentación de muerte. |
| Ráfaga crítica y Ojo del verdugo | Reutilizan los avisos de onda, aturdimiento y daño crítico; no introducen una biblioteca de sprites separada. |
| Pólvora volátil y Reacción en cadena | Reutilizan explosiones; cambian su radio, daño o condición de aparición. |
| Blindaje | Se lee en las placas y valores del HUD; no añade una explosión o aura exclusiva. |

## Alcance de estas imágenes

La galería reúne 21 capturas nuevas del 5 de noviembre, los quince clips de la biblioteca de efectos de enemigos y los registros de torreta del 3 y 4 de noviembre. Los patrones de Gólgora están en [[02 - Personajes y Animación]]. Las muestras anteriores de octubre permanecen en el historial del repositorio.

Los GIFs permiten revisar aparición, trayectoria, impacto y desaparición visual. Las cifras de daño, probabilidades y condiciones de activación se contrastan con las reglas del juego. Las situaciones controladas y los temporizadores adelantados se identifican en cada caso. Las animaciones no contienen audio y no certifican un mínimo de FPS.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
