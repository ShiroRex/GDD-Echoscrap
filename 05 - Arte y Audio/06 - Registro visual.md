# Registro visual

## Procedencia de los medios

El registro reúne previsualizaciones de recursos y capturas de ejecución, con su procedencia y fecha:

- **Previsualizaciones de atlas, 4 de noviembre.** Copias de los PNG activos y GIFs compuestos con sus rectángulos, etiquetas y tiempos JSON. Las acciones de personaje muestran cuatro direcciones a escala `2×`, sin interpolación. No incluyen la iluminación ni la simulación del juego.
- **Capturas de Unity, 4 de noviembre.** `CombatArena` ejecutada desde una copia externa del proyecto (`30b88c6`), a `960 × 540` y doce imágenes por segundo (`640 × 360` en GIF). Se usaron cámara de detalle, jugador protegido y oleadas detenidas. Los patrones del jefe se invocaron individualmente con sus componentes y ajustes; para la lluvia se activó la segunda fase. Las pantallas de preparación utilizan un inventario de muestra, sin guardar esa colección.
- **Capturas de Unity, 5 de noviembre.** Veintiuna secuencias de linterna, mejoras, daño, fusión y portal del menú, más los ciclos integrados de Caballero Fantasma y Centinela de Cobre (`b9e3136`), con cámara de detalle a `960 × 540` (`1280 × 720` en números de daño) y doce imágenes por segundo (`24 fps` en *Micro-ensamblador* y números de daño; GIFs exportados a `768 × 432`). Las oleadas están detenidas, los objetivos tienen locomoción desactivada y se activan eventos y estados reales del código para aislar cada efecto.

## Enemigos y personajes

[[02 - Personajes y Animación]] reúne el atlas y los GIFs del Coleccionista, los cinco spritesheets completos de horda con todas sus acciones, los dos élites integrados y las capturas de los seis patrones de Gólgora.

## Efectos de enemigos

La biblioteca activa contiene quince clips y 120 fotogramas. La siguiente hoja pertenece a esos efectos de combate.

![Atlas activo de efectos de enemigos](../Media/Avance_2026-11-04/VFX/EnemyEffects_Atlas.png)

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

La torreta usa trípode y cañón separados, orientación del cañón, boca de salida y retroceso (hasta 2 torretas activas simultáneas, disparando ráfagas de 3 proyectiles cada `0,6 s` separados por `0,08 s`).

![Torreta: previsualización técnica de sus direcciones](../Media/Avance_2026-11-04/VFX/Torreta_Direcciones.gif)

![Torreta disparando en la arena de Unity](../Media/Avance_2026-11-04/VFX/Torreta_Combate.gif)

## Linterna

Las dos capturas usan los mismos objetivos inmóviles y la misma cámara. La presentación cambia con las mejoras de la linterna: alcance y apertura, cadencia, intensidad y color.

### Linterna base

La ráfaga muestra el haz, los cuatro pulsos y el empuje en el cuarto pulso sobre los enemigos alcanzados.

![Linterna base: captura de Unity del 5 de noviembre](../Media/Avance_2026-11-05/VFX/Linterna.gif)

### Rayo solar

La mejora legendaria (`FLASH_05`) cambia la luz a dorado, amplía el alcance base a `14 m`, multiplica el daño por `2,5` y elimina el descanso entre ráfagas. En el código vigente (`UpgradeCombatRuntime.cs:169`), todos sus pulsos cuentan como cuarto pulso (`finalPulse = true`), por lo que cada pulso aplica empuje y activa `FLASH_03` (*Prisma voltaico*), `BIO_02` (*Sifón*) y `CRIT_04` (*Ráfaga crítica*). *(El GIF del 5 de noviembre se capturó antes de activar el empuje por pulso y muestra el tratamiento luminoso continuo).*

![Rayo solar: captura de Unity del 5 de noviembre](../Media/Avance_2026-11-05/VFX/RayoSolar.gif)

## Efectos de mejoras

Cada secuencia muestra el comportamiento visual de la mejora en el motor; sus valores y reglas exactas se detallan en [[05 - Progresión]].

### Prisma voltaico

Tras un impacto de cuarto pulso (o de cualquier pulso con *Rayo solar*), la descarga enlaza `2 / 3 / 4` objetivos sucesivos en `5 m` y produce destellos eléctricos en los alcanzados.

![Prisma voltaico: descarga y enlaces entre objetivos](../Media/Avance_2026-11-05/VFX/PrismaVoltaico.gif)

### Frío y congelación

El primer impacto aplica la ralentización de *Freón residual*. Los pulsos siguientes sobre el objetivo ralentizado alcanzan el umbral de *Cero absoluto* y añaden escarcha, partículas y niebla fría.

![Freón residual y Cero absoluto: ralentización y congelación](../Media/Avance_2026-11-05/VFX/FrioCongelacion.gif)

### Ventisca

Emite una onda de frío en `5,5 m` desde el jugador y congela `1,5 s` a los objetivos alcanzados.

![Ventisca: onda de frío y objetivos congelados](../Media/Avance_2026-11-05/VFX/Ventisca.gif)

### Quiebre de hielo

La muerte de un enemigo congelado libera seis esquirlas radiales perforantes (`14 m/s`) que congelan `1,5 s`.

![Quiebre de hielo: esquirlas tras una muerte congelada](../Media/Avance_2026-11-05/VFX/QuiebreHielo.gif)

### Minas de chatarra

La mina se arma a los `0,25 s` (hasta 5 minas activas simultáneas) y detona al detectar un enemigo a `1,25 m`.

![Minas de chatarra: colocación y explosión](../Media/Avance_2026-11-05/VFX/Minas.gif)

### Termobárica

A los `0,4 s` de la explosión inicial aparece una implosión que repite el `75%` del daño y atrae a los enemigos hacia el centro una distancia base equivalente al `45%` del radio de explosión.

![Termobárica: explosión inicial e implosión secundaria](../Media/Avance_2026-11-05/VFX/Termobarica.gif)

### Nova de plasma

Cada `15 HP` de curación sobrante emiten una nova de `80` de daño sobrenatural en `6 m` de radio alrededor del jugador.

![Nova de plasma: expansión por curación sobrante](../Media/Avance_2026-11-05/VFX/NovaPlasma.gif)

### Curación y reparación

Efecto compartido de recuperación efectiva de salud (*Más vida*, *Sifón*, *Botiquín mejor*, regeneración de *Nanobots* y poderes de acompañantes curativos).

![Curación: efecto compartido de reparación](../Media/Avance_2026-11-05/VFX/Curacion.gif)

### Nanobots: protección de emergencia

Cuando un golpe deja al jugador por debajo del `20%` de vida (o resulta letal sin Yanikua disponible), frena la caída en `20%` y activa el escudo temporal de `2 s` (`60 s` de enfriamiento).

![Nanobots: activación del escudo de emergencia](../Media/Avance_2026-11-05/VFX/Nanobots.gif)

### Micro-ensamblador

Cada `15` gemas originales recogidas dispara un proyectil guiado a `24 m/s` hacia un enemigo.

![Micro-ensamblador: proyectil guiado, estela e impacto](../Media/Avance_2026-11-05/VFX/MicroEnsamblador.gif)

### Sierras orbitales

Las hojas giran a `180°/s` a `2,5 m` del Coleccionista, infligiendo daño y empuje en cada paso de simulación en solape.

![Sierras orbitales: giro y efectos de contacto](../Media/Avance_2026-11-05/VFX/Sierras.gif)

### Pulso colector

El remolino de recogida acompaña el vuelo de todas las gemas y botiquines activos del mapa hacia el jugador.

![Pulso colector: remolino y recogida de gemas](../Media/Avance_2026-11-05/VFX/PulsoColector.gif)

### Onda sísmica

El evento de llegada del dash emite una expansión radial de daño físico y aturdimiento.

![Onda sísmica: expansión al terminar el dash](../Media/Avance_2026-11-05/VFX/OndaSismica.gif)

### Holo-señuelo

El inicio del dash deja una copia del Coleccionista que atrae enemigos en `12 m` y, al agotarse su duración, implosiona en `4 m`.

![Holo-señuelo: copia temporal e implosión](../Media/Avance_2026-11-05/VFX/HoloSenuelo.gif)

### Cataclismo

La explosión nace en el punto de salida (`120` de daño en `4 m`) y la cuchilla (`80` de daño) se lanza desde el punto de llegada, fijando la recarga del dash en `0,5 s`.

![Cataclismo: explosión de salida y cuchilla de llegada](../Media/Avance_2026-11-05/VFX/Cataclismo.gif)

## Números de daño

Los valores siguen arcos con la tipografía `Pixelify Sans` y distinguen daño físico y sobrenatural, con variantes de impacto normal y crítico (`!`), además del indicador `Resistido` y el rótulo `EJECUTADO`.

![Números de daño: canales físicos y sobrenaturales, críticos, resistencias y ejecución](../Media/Avance_2026-11-05/VFX/NumerosDano.gif)

## Fusión y portal

### Aura de la máquina de fusión

Las partículas ascienden por los costados de la máquina y dejan libre la persiana central.

![Máquina de fusión: aura de partículas](../Media/Avance_2026-11-05/VFX/FusionAura.gif)

### Apertura y revelación de la fusión

Al abrir una fusión lista (`E · VER FUSIÓN` tras los `30 s` de cuenta atrás) se eleva la puerta, aparece humo y se revela la entidad obtenida junto al botón `COLECCIONAR`.

![Fusión: apertura de la máquina y humo de revelación](../Media/Avance_2026-11-05/VFX/FusionApertura.gif)

### Portal del menú

El vórtice animado y las entidades en órbita forman el fondo del menú inicial.

![Menú inicial: portal animado y entidades en órbita](../Media/Avance_2026-11-05/VFX/PortalMenu.gif)

## Efectos compartidos y mejoras de estadísticas

| Mejora o grupo | Presentación |
| --- | --- |
| Más alcance, Pulso rápido y Golpe potente | Reutilizan el haz, los impactos y la presentación de la linterna; cambian su apertura, alcance, cadencia, color o intensidad. |
| Dash ágil | Reutiliza la animación y las imágenes residuales del dash; modifica velocidad y recarga. |
| Más vida, Sifón y Botiquín mejor | Reutilizan el efecto compartido de curación y reparación; el HUD expresa los cambios de vida. |
| Imán | Reutiliza el vuelo de recogida de gemas y botiquines. |
| Chatarrero | Cambia la XP recibida y su lectura en el HUD. |
| Sintonizador | Se representa mediante reintentos y ofertas en la interfaz de cartas. |
| Óptica precisa y Calibración letal | Reutilizan los destellos y números críticos; cambian sus probabilidades o valores. |
| Ejecución | Muestra el rótulo `EJECUTADO` en los números de daño y resuelve la muerte del enemigo. |
| Ráfaga crítica y Ojo del verdugo | Reutilizan los avisos de onda, aturdimiento y daño crítico. |
| Pólvora volátil y Reacción en cadena | Reutilizan explosiones; cambian su radio, daño o condición de aparición. |
| Blindaje | Se lee en las placas y valores del HUD. |

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
