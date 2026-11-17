# EchoScrap — Documento de diseño de videojuego

**GDD 0.7.2 · 16 de noviembre de 2026 (`develop` @ `b543684`)**

Equipo EchoScrap: Mora Alejandro Angel Gabriel, Sierra Sosa David Alejandro, Cupul Dzul Mirian Leticia, Obispo Navarro Samuel Alejandro y Nuñez Chan Luis Fernando.

Plantilla original: Ing. Luis Fernando Villafaña Rejón.

Esta edición documenta el código y las escenas actuales (`develop` @ `b543684`): menú inicial con selector de **3 ranuras de guardado** (`SaveSlotService`, migración automática desde `PlayerPrefs`) y **snapshots de expedición cada `3 s`** (`ExpeditionSnapshotService`), bóveda ampliada a **18 unidades** (`DisplaySlots[18]`: `3` de equipo + `15` de exposición) con máquina de fusión de `30 s` en la pared trasera `(189, 0, 28,88)`, paredes de basalto pixel art y relieve de roca, **Índice de Colección (`[I]`)**, poderes acumulables por copia equipada (`Copies(id)` y `DescribeHudPower`), catálogo ampliado a **62 mejoras en cinco rarezas** (`8` cartas contextuales `1239072`, `19` cartas nuevas sin GC, aura y zonas en suelo proyectadas sobre el relieve en `UpgradeEffectPool.ProjectGroundRing`, limpieza de `_EmissionColor` en enemigos y la carta **Mítica *All In*** `MYTH_01` con marco lit `Frame_Mythic`, además de la reelaboración de `DASH_05` *Paso de ariete* a distancia de dash), HUD con cuadrícula `9 × 34 px` y `22` estadísticas, selectores **`Límite de FPS` (`30 / 60 / 120`)** y **`Pantalla` (`COMPLETA` / `VENTANA`)** con arranque automático en pantalla completa sin bordes (`EchoScrap.Graphics.FullScreen`), ranking en vivo en [`https://echoscrap.web.app`](https://echoscrap.web.app) publicando todos los intentos válidos y conservando la mejor puntuación por gamertag, optimizaciones de navegación, GPU instancing e iluminación interior (`WarehouseLightScope`), pruebas automatizadas verificadas (`EditMode 63/63` con gráficos habilitados; `PlayMode 172/172` focalizado con `0` fallos en las suites de ajustes, sprites, mejoras, bóveda, guardado y jefe), y paquetes Standalone verificados para **Windows x64** (`1920 × 1080` en prueba de humo, `0` errores) y **macOS Universal (`x86_64` + `arm64`)**, junto con los recursos de identidad, arte conceptual, audio y tráiler (`Media/Trailer/EchoScrap-Trailer-Final-1080p.mp4`). Los GIFs se reproducen en lectores compatibles; una exportación a PDF conserva imágenes estáticas.

La sección Pruebas distingue las comprobaciones aprobadas de los fallos pendientes de la suite completa. La biblioteca general de clasificación (sección 11) permanece como referencia separada de esta compilación del diseño de EchoScrap.

---

# EchoScrap: avance jugable

**GDD 0.7.2 · 16 de noviembre de 2026 (`develop` @ `b543684`).**

El Coleccionista prepara su equipo en la bóveda y sale al Vertedero para reunir experiencia y entidades. Durante la expedición combate con una linterna automática, escoge mejoras y se enfrenta a Gólgora. Al terminar vuelve a la bóveda con su colección, donde puede consultar el Índice de Colección (`[I]`), editar el equipo activo, liberar copias para hacer hueco o usar la máquina de fusión.

## Recorrido de una sesión

Menú inicial (`CONTINUAR` / `NUEVA PARTIDA` sobre **3 ranuras de guardado** con registro de alias para ranking) → cargar ranura (retorno a bóveda o reanudación automática de expedición activa por snapshot) → consultar el Índice (`[I]` / `Select`), fusionar copias (`E` / botón oeste) en la pared trasera y seleccionar entidades equipadas en el balcón → elegir ronda demo o completa → expedición → regreso a la bóveda.

La preparación en la bóveda está guiada mediante rótulos 3D sobre los pedestales superiores (`EQUIPO 1`, `EQUIPO 2`, `EQUIPO 3`), dos indicadores direccionales en la parte superior de la pantalla (`VaultDirections`) que apuntan hacia la máquina de fusión y el balcón de equipo, y un panel inferior izquierdo de consejos (`VaultTips`) que rota cada `8 s`. En los puestos 1 a 3 del balcón, pulsar `E` o botón oeste (`E · EQUIPAR` / `E · EDITAR`) abre el selector de acompañantes (con botones `QUITAR DEL PUESTO`, `LIBERAR COPIA` y `CERRAR · E / ESC`), mientras que `T` o botón norte retira directamente la entidad equipada al mundo; en los pedestales de exposición de la planta baja, pulsar `E` o botón oeste (`E · LIBERAR`) abre el diálogo de confirmación para liberar una copia de la colección.

![Bóveda y preparación](Media/Interfaz/Boveda_almacen.png)
*Captura de referencia de la Bóveda (`734e2a5`) mostrando las tarjetas direccionales superiores (`VaultDirections`), el botón `ÍNDICE · [I]`, los rótulos 3D `EQUIPO 1..3` y el panel `CONSEJO` (`VaultTips`); en la versión `0.7.0`, la sala se amplía a `18` cápsulas y la máquina de fusión se ubica en la pared trasera `(189, 0, 28,88)`.*

| Área | Contenido de esta versión |
| --- | --- |
| Partida | Menú inicial con **3 ranuras de guardado independientes** (`SaveSlotService`), reanudación automática de expedición activa (`ExpeditionSnapshotService`), selector de gamertag para ranking en vivo (`EventLeaderboardSync`, manteniendo el mejor puntaje por gamertag entre todos los intentos válidos), preparación guiada en bóveda, selección de ronda (`ELIGE LA RONDA`), resultados con retorno a la bóveda, paquetes Release para **Windows x64** y **macOS Universal (`x86_64` + `arm64`)**, publicación web en [`https://naked-hiro.itch.io/echoscrap`](https://naked-hiro.itch.io/echoscrap) y [tráiler promocional final (`1080p`, `46 s`)](Media/Trailer/EchoScrap-Trailer-Final-1080p.mp4). |
| Control | Teclado/ratón y mando (`WASD`/flechas o stick/cruceta, dash `Espacio` / `A` / `Cruz` con `0,22 s` de invulnerabilidad, interacción `E` / botón oeste `X` / `Cuadrado`, desequipado rápido en balcón `T` / botón norte `Y` / `Triángulo`, Índice de Colección `I` / `Select` y consulta rápida de detalles del HUD manteniendo `Q` / `Select`). |
| Combate | Linterna de cuatro pulsos con autoapuntado y **62 mejoras en cinco rarezas** (`Común`, `Rara`, `Épica`, `Legendaria` y la carta **`Mítica` *All In***, incluyendo zonas persistentes, ataques orbitales/barridos y la reelaboración de *Paso de ariete* a distancia de dash). |
| Enemigos | Cinco arquetipos de horda, dos élites y Gólgora. |
| Rondas | Demo: jefe a los 3:00 y 3600 HP (`Golgora_Demo.asset`, mismo valor base en `EnemyArchetypeSettings`). Completa: jefe a los 8:00 y 9000 HP (`Golgora_Complete.asset`). La victoria exige derrotarlo. |
| Mundo | Arena de 200 × 200 m, acceso norte, bóveda modular en dos niveles y caldera central del jefe. Las zonas de lava fija del mapa están desactivadas. |
| Colección | Quince entidades en seis rarezas, capacidad máxima estricta de **18 unidades** en posesión por ranura (`FusionProgress.Capacity = 18`, con `DisplaySlots[18]`: 3 puestos de equipo en el balcón y 15 pedestales de exposición/reserva en planta baja; los poderes **se acumulan por copia equipada** hasta `×3`) e **Índice de Colección (`[I]`)** con pestañas `POR RAREZAS` (`TODAS` y 6 rarezas) y `EN TU BÓVEDA`. |
| Fusión | Cuatro copias cualesquiera (de la misma rareza o de rarezas mixtas, incluidas copias equipadas en los puestos 1–3 y excluida Yanikua), estación ubicada en la pared trasera (`(189, 0, 28,88)`), duración fija de **30 segundos (`00:30`)** en todas las recetas y botón único `COLECCIONAR` al reclamar el resultado (requiere espacio libre en las 18 unidades). |
| Presentación | Tipografía unificada `Pixelify Sans`, panel unificado de configuración y pausa en 3 pestañas (`General`, `Interfaz`, `Sonido` en `MenuSettingsView.cs`), sprites animados, modelo 3D del jefe, minimapa con exploración, retratos, VFX y audio contextual. |

## Dónde consultar cada parte

- [[01 - Jugabilidad]] y [[01 - UX y Flujo del Jugador]] describen la preparación en la bóveda y la expedición.
- [[04 - Objetos y Coleccionables]] reúne las quince entidades, sus poderes, el Índice de Colección (`[I]`), la capacidad de 18 unidades y las reglas de fusión.
- [[03 - Personajes]] identifica individualmente a los enemigos y su papel.
- [[03 - Enemigos y NPC]] contiene los valores, la respuesta al empuje y las reglas de los ataques.
- [[02 - Personajes y Animación]] reúne las fichas visuales, fuentes editables, spritesheets y GIFs de personajes y enemigos.
- [[06 - Registro visual]] reúne los GIFs de VFX y las condiciones de las capturas.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Ficha del proyecto

| Campo | Información |
| --- | --- |
| Nombre | EchoScrap |
| Versión del GDD | 0.7.1 |
| Estado | Versión jugable sincronizada en `develop`: menú inicial con **3 ranuras de guardado** y reanudación por snapshot, configuración compartida con selector de **pantalla completa/ventana (`COMPLETA` / `VENTANA`)** y FPS, preparación guiada en bóveda de **18 unidades**, Índice de Colección / Bestiario (`[I]`), expediciones, fusión de 30 s (misma rareza o rarezas mixtas), **62 mejoras en cinco rarezas** (incluyendo *All In* Mítica y proyección de auras/zonas sobre relieve), encuentro final, ranking en vivo en [`https://echoscrap.web.app`](https://echoscrap.web.app) con todos los intentos válidos, compilaciones **Windows x64** y **macOS Universal**, publicación web previa en [`https://naked-hiro.itch.io/echoscrap`](https://naked-hiro.itch.io/echoscrap) y tráiler promocional (`EchoScrap-Trailer-Final-1080p.mp4`, `1080p`, `46 s`). |
| Actualización | 16 de noviembre de 2026 |
| Género | Acción. |
| Subgénero | Supervivencia de hordas con progresión roguelite. |
| Público objetivo | Jugadores de PC que disfrutan esquivar hordas, combinar mejoras y coleccionar poderes entre expediciones cortas. |
| Plataforma | PC (**Windows x64** y **macOS Universal `x86_64` + `arm64`**), un jugador local con soporte completo de teclado/ratón y mando (con sincronización de ranking en vivo en Firebase y visor web en [`https://echoscrap.web.app`](https://echoscrap.web.app)). |
| Motor | Unity 6000.6.3f1 y Universal Render Pipeline. |
| Lenguaje | C# y shaders en HLSL. |
| Perspectiva | Mundo 3D con personajes 2D; cámara exterior a 35,5° y encuadre propio para bóveda y jefe. |
| Distribución ejecutable (itch.io) | https://naked-hiro.itch.io/echoscrap |
| Tráiler promocional final | [EchoScrap-Trailer-Final-1080p.mp4](Media/Trailer/EchoScrap-Trailer-Final-1080p.mp4) (`1920 × 1080`, `46,2 s`, gameplay desarrollado y audio propio; aprobado en QA) |
| Repositorio del juego | https://github.com/ShiroRex/EchoScrap |
| Revisión consultada | Código y escena de `develop` @ `b543684` (integra `734e2a5`/`6973556`, las 8 mejoras contextuales `1239072`, selector de FPS `993d459`, 3 ranuras `1c97922`, snapshots `24db181`, ranking por mejor marca `15d273f`, bóveda de 18 cápsulas `8c0dfae`, mejoras de rendimiento/arte `578ba07..aefed1a`, expansión a 62 mejoras + rareza Mítica `ebf90f9`, y correcciones `7a237e5..b543684`: selector `Pantalla` `COMPLETA`/`VENTANA` con pantalla completa al iniciar, limpieza de `_EmissionColor` en enemigos y proyección sobre relieve de auras/zonas). |
| Repositorio del GDD | https://github.com/ShiroRex/GDD-Echoscrap |

## Equipo

- **Mora Alejandro Angel Gabriel:** líder y coordinación del proyecto.
- **Sierra Sosa David Alejandro:** desarrollo, diseño funcional y balance.
- **Cupul Dzul Mirian Leticia:** personajes y animación.
- **Obispo Navarro Samuel Alejandro:** entornos e interfaz.
- **Nuñez Chan Luis Fernando:** audio, narrativa, UX y calidad.

## El juego en una frase

El Coleccionista reúne entidades en un vertedero dimensional, prepara sus poderes en la bóveda y combina mejoras durante cada expedición para enfrentarse al MataColeccionista.

El contenido actual incluye una arena de 200 × 200 m, cinco enemigos de horda, dos élites, un jefe (Gólgora), **62 mejoras en cinco rarezas** (Común, Rara, Épica, Legendaria y Mítica) y una colección de quince entidades con capacidad máxima de **18 unidades simultáneas** en la bóveda (`3` puestos de equipo en el balcón superior con rótulos `EQUIPO 1..3` y `15` puestos de exposición en la planta baja, con la máquina de fusión en la pared trasera en `x = 189, z = 28,88`), acompañada de indicadores direccionales (`VaultDirections`), consejos rotatorios (`VaultTips`) e Índice de Colección (`[I]`).

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Visión del juego

EchoScrap combina supervivencia frente a hordas con la preparación de una colección de hasta dieciocho unidades simultáneas. En el balcón superior de la bóveda, el jugador puede elegir hasta tres entidades para los puestos de equipo (`EQUIPO 1`, `EQUIPO 2`, `EQUIPO 3`). Cada identidad equipada activa un poder propio de combate, independiente de las cartas y **acumulable por copia equipada** hasta `×3` (`Copies(id)`). Después decide entre la ronda demo y la completa.

En la arena controla el movimiento y el dash del Coleccionista. La linterna apunta automáticamente al enemigo más cercano dentro de su alcance, lo que permite atender a las rutas de escape, los ataques anunciados y la recogida. La XP abre elecciones entre hasta tres cartas; las **62 mejoras en cinco rarezas** (incluyendo armas orbitales, auras de estática, proyectiles rebote/búmeran, zonas químicas/criogénicas y la carta Mítica *All In*) permiten construir una configuración distinta durante cada intento, y mantener pulsado `Q` (o `Select` en mando) despliega al instante las estadísticas completas (`22` filas) y las descripciones de los poderes de acompañantes.

También aparecen fragmentos de entidades que se recogen manualmente con `E` (o botón oeste en mando) mientras haya hueco entre las **18 unidades** de la colección (si está llena, aparece el aviso `INVENTARIO LLENO / LIBERA ESPACIO`). Esas copias pasan a la ranura guardada y sirven para equipar poderes en el balcón, exhibirse en los **18 pedestales físicos de la bóveda** (`3` puestos de equipo en el balcón y `15` pedestales de exposición/reserva en planta baja) o alimentar la máquina de fusión ubicada en la pared trasera (`(189, 0, 28,88)`) con cuatro ingredientes de la misma rareza o de rarezas mixtas (`30 s` de duración fija). La bóveda conecta esa actividad entre expediciones mediante indicadores direccionales (`VaultDirections`), consejos contextuales (`VaultTips`) y el **Índice de Colección (`[I]`)**.

Gólgora cierra la expedición en una caldera central. Sus ataques cubren distancias y formas diferentes, y su segunda fase acelera el encuentro. La muerte del jugador produce derrota; la del jefe con el jugador vivo produce victoria. El resultado sincroniza la puntuación con el ranking del evento (`EventLeaderboardSync`) y devuelve al jugador a la bóveda para preparar el siguiente intento.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Público y plataforma

EchoScrap es un juego de acción y supervivencia de hordas con progresión roguelite para PC (**Windows x64** y **macOS Universal**) y un solo jugador local, con **3 ranuras de guardado**, reanudación automática de expediciones y sincronización automática de puntuaciones hacia Firebase (`EventLeaderboardSync`). Está dirigido a quienes disfrutan esquivar grupos numerosos, aprender patrones de ataque y combinar mejoras en expediciones cortas. Las cartas se reinician en cada intento y la colección persistente permite preparar poderes distintos para el siguiente.

El autoapuntado facilita la entrada a jugadores que quieren concentrarse en moverse y elegir mejoras. La ronda demo permite aprender el ciclo de expedición. La completa da más tiempo para construir una configuración y aumenta la vida de Gólgora. El combate admite teclado y ratón o mando, con movimiento, dash directo y consulta rápida de estadísticas y poderes de acompañantes manteniendo `Q` (teclado) o `Select` (mando).

Las cartas introducen pausas de decisión. Antes de salir, la bóveda guía al jugador mediante flechas direccionales superiores (`VaultDirections`), consejos en la esquina inferior izquierda (`VaultTips`), rótulos 3D (`EQUIPO 1..3`) y el Índice de Colección (`[I]`). En el montaje actual, recoger entidades en la arena y operar los pedestales o la máquina de fusión utiliza la tecla `E` (además de `T` para desequipar rápido en el balcón e `I` para abrir el índice).

La ronda demo lleva al jefe a los tres minutos y la completa a los ocho. Estos tiempos corresponden al reloj de combate, que se detiene con las pausas y elecciones. La duración total incluye la preparación, los menús y el enfrentamiento final.

Los ataques se comunican con avisos de suelo, animaciones y texto. El menú de configuración y pausa (`Esc` / `Start`), organizado en las pestañas `General`, `Interfaz` y `Sonido`, permite ajustar el movimiento de cámara, los efectos decorativos, el **límite de FPS (`30 / 60 / 120`)**, el **modo de pantalla (`COMPLETA` / `VENTANA`)**, el nivel de detalle de estadísticas, la visibilidad de los atributos de coleccionables y los volúmenes general y de música.

La plataforma, perspectiva y modo se centralizan en [[01 - Ficha del Proyecto]]. Las comprobaciones de lectura y formato de pantalla se registran en [[03 - Accesibilidad]].

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Pilares de diseño

## Movimiento que abre oportunidades

El desplazamiento ágil (`8 m/s` base), el dash (`22 m/s`) y el empuje del cuarto pulso ayudan a atravesar huecos y cambiar la posición de la horda. La linterna apunta por su cuenta, por lo que el Coleccionista puede retirarse mientras ataca en otra dirección.

## Preparar una colección y construir una configuración

Las entidades equipadas en los tres puestos del balcón aportan poderes propios, independientes de los niveles de cartas. Las **62 mejoras en cinco rarezas** de la expedición amplían las herramientas disponibles mediante estadísticas, dispositivos orbitales, auras de daño continuo, ataques automáticos, zonas elementales, apuesta aleatoria Mítica (*All In*) y control de masas. Los requisitos relacionan las decisiones: *Cero absoluto* parte de *Freón residual*, *Rayo solar* convierte todos sus pulsos en cierres de ráfaga capaces de activar *Prisma voltaico*, *Sifón* y *Ráfaga crítica*, las mejoras de explosión o granadas necesitan una fuente compatible, y los ataques orbitales o de estática escalan su radio con `AreaMultiplier`.

## Ataques que se pueden leer

La carga del Coloso, el ácido del Mortero, la espoleta por proximidad de la Polilla Reactor (`2 m`), los ataques élite y los patrones de Gólgora muestran señales claras antes de resolver el daño. El ajuste de efectos decorativos en pausa conserva siempre los avisos esenciales de peligro.

## Una bóveda que da continuidad a las expediciones

Las entidades recogidas pasan al inventario de la ranura activa hasta un máximo de **18 unidades simultáneas** (`DisplaySlots[18]`: `3` puestos de equipo en el balcón + `15` pedestales de exposición/reserva en planta baja), mientras que su registro permanente queda guardado en el **Índice de Colección (`[I]`)** de 15 entidades. En la bóveda, señalizada mediante rótulos `EQUIPO 1..3`, indicadores direccionales (`VaultDirections`) y consejos (`VaultTips`), el jugador equipa hasta tres acompañantes (cuyos poderes se acumulan por copia equipada), libera copias para gestionar el espacio o consume cuatro ingredientes (de la misma rareza o de rarezas mixtas) en una fusión de `30 s` ubicada en la pared trasera.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Jugabilidad

## Preparación y salida

Menú inicial (`CONTINUAR` / `NUEVA PARTIDA` sobre **3 ranuras de guardado** y registro de alias para ranking) → continuar ranura (en bóveda o reanudando expedición activa mediante snapshot) o iniciar partida nueva → bóveda → consultar el Índice de Colección (`[I]`), fusionar copias (`E`) y seleccionar entidades equipadas en el balcón → elegir ronda demo o completa → expedición → regreso a la bóveda.

Continuar recupera la ranura elegida (incluyendo reanudación exacta de expedición activa si existe `expedition_{N}.json` válido). Nueva partida permite ocupar una ranura vacía o sobrescribir una existente tras confirmación.

La bóveda aloja una colección con capacidad máxima estricta de **18 unidades en total** (`FusionProgress.Capacity = 18`, `displays[18]`). En la escena (`FusionStation.DisplaySlots`), el recinto dispone de **18 cápsulas o pedestales físicos** (**3 puestos de equipo activo** en el balcón superior, `slot 0..2`, y **15 pedestales de exposición/reserva** en la planta inferior, `slot 3..17`; cuando algún puesto de equipo está vacío, muestra temporalmente una unidad en reserva con la etiqueta `EQUIPO N · RESERVA`, exhibiendo las hasta 18 unidades simultáneamente en el mundo mientras también se consultan íntegras en la pestaña `EN TU BÓVEDA` del Índice `[I]`). La navegación dentro de la bóveda está guiada en tiempo real por tres elementos:

- **Rótulos 3D en el mundo:** `EQUIPO 1`, `EQUIPO 2` y `EQUIPO 3` sobre los tres pedestales del balcón superior.
- **Indicadores direccionales superiores (`VaultDirections`):** dos tarjetas dinámicas en la parte alta del HUD con flechas (`▲`, `▼`, `◄`, `►`, `●`) que apuntan hacia la **Máquina de Fusión** (*"MÁQUINA DE FUSIÓN · Combina 4 copias de varias rarezas"*, *"BAJA A FUSIÓN"*, *"EN FUSIÓN"* o *"E · MÁQUINA DE FUSIÓN"*) y hacia los **Puestos de Equipo** (*"SUBE AL BALCÓN · Puestos de equipo 1, 2 y 3"*, *"EQUIPAR ENTIDADES"* o *"PUESTO 1..3 · EQUIPAR/EDITAR"*).
- **Panel de consejos (`VaultTips`) e Índice (`[I]`):** en la esquina inferior izquierda, la tarjeta `CONSEJO` rota cada `8 s` entre tres indicaciones (*"Equipa coleccionables antes de ir a la arena."*, *"Abre el índice para consultar tus descubrimientos."* y *"Combina cuatro coleccionables en la máquina de fusión."*), o prioriza *"Pulsa E frente a un puesto para equipar tu primer coleccionable."* si el jugador tiene copias pero aún no ha equipado ninguna. En la esquina superior izquierda, el botón **`ÍNDICE · [I]`** (o la tecla `I` / `Select`) abre el **Índice de Colección** con las 15 entidades (`POR RAREZAS` y `EN TU BÓVEDA`).

En los **puestos 1 a 3 del balcón**, el aviso contextual es **`E · EQUIPAR`** (si el puesto está vacío, incluso cuando exhibe visualmente una reserva) o **`E · EDITAR`** (si está ocupado). Pulsar **`E`** (o botón oeste en mando) abre siempre el selector de acompañantes (`PUESTO DE EQUIPO 1/2/3`), donde equipar varias copias de la misma entidad **acumula sus poderes proporcionalmente** (`×2`, `×3`), e incluye en su barra inferior los botones **`QUITAR DEL PUESTO`**, **`LIBERAR COPIA`** y **`CERRAR · E / ESC`**. Además, pulsar **`T`** (o botón norte en mando) frente a un puesto de equipo ocupado en el mundo retira directamente la entidad sin abrir el menú. En los **pedestales de exposición de la planta inferior** (`slot >= 3`), acercarse a un pedestal ocupado muestra **`E · LIBERAR`** y pulsar **`E`** abre el diálogo de confirmación de liberación (`LIBERAR UNIDAD`).

El portal o el botón **`INICIAR EXPEDICIÓN`** abre la elección de ronda (`ELIGE LA RONDA`). La ronda demo lleva a Gólgora a los `3:00`; la completa, a los `8:00`. Tras la selección se presenta la entrada del Coleccionista en la arena.

## Bucle de combate

1. Mantener una ruta de escape con movimiento y dash.
2. Dañar a la horda con la linterna y los poderes de la configuración.
3. Recoger XP y elegir una mejora al subir de nivel.
4. Acercarse a las entidades que aparecen y recogerlas con `E` antes de que desaparezcan (siempre que la colección tenga menos de `18/18` unidades; si está llena, aparece el aviso flotante `INVENTARIO LLENO / LIBERA ESPACIO`).
5. Consultar en cualquier momento las estadísticas completas y las descripciones de los poderes de acompañantes manteniendo pulsada la tecla **`Q`** (o **`Select`** en mando).
6. Responder a las composiciones de horda y a los ataques de los dos élites.
7. Combatir contra Gólgora en la caldera central.
8. Consultar el resultado y volver a la bóveda para revisar equipo, liberar espacio o fusionar copias.

Las elecciones de cartas pausan la simulación del combate mediante la máscara `PauseReason.TacticalDraft`. Si una recogida acumula varias subidas de nivel consecutivas, las elecciones se encadenan una tras otra sin reanudar la arena entre selecciones.

## Cierre y continuidad

La vida del jugador a cero produce derrota. La muerte de Gólgora con el jugador vivo produce victoria. Si ambos mueren en el mismo paso de simulación, prevalece la derrota. El resumen muestra tiempo, nivel, bajas y XP, sincroniza la puntuación con el ranking del evento (`EventLeaderboardSync`) y ofrece volver a la bóveda.

Las cartas, el nivel y la salud de la expedición se restablecen. Las copias ya recogidas, las entidades equipadas y el progreso de fusión pertenecen a la colección y se conservan. Abandonar desde el menú de pausa (`Esc` / `Start`) también devuelve a la bóveda.

## Bóveda y fusión

La máquina de fusión situada en la pared trasera de la bóveda (`189, 0, 28,88`) se abre con **`E`** (o botón oeste en mando; `E · FUSIONAR` cuando está libre o en cuenta atrás, o `E · VER FUSIÓN` cuando tiene un resultado listo) y acepta **4 unidades cualesquiera, ya sean de la misma rareza o de rarezas mixtas** (excluida *Yanikua*), interpolando linealmente las probabilidades según la rareza media de los 4 ingredientes (`qualityMean = total / 4f`). Pueden seleccionarse tanto copias en reserva como copias actualmente equipadas en el balcón (identificadas con **`EN EQUIPO 1/2/3`**); al pulsar **`CONFIRMAR`**, el sistema desequipa automáticamente las copias necesarias y consume las 4 unidades.

Todas las fusiones tienen una **duración fija de 30 segundos (`00:30`)** en tiempo real (`Time.unscaledDeltaTime`), que continúa avanzando entre escenas. Al completarse la cuenta atrás, la pantalla de resultado revela la entidad obtenida y ofrece un único botón de acción: **`COLECCIONAR`** (además del botón `X` para cerrar la ventana sin reclamar aún). Si la colección está en `18/18` unidades al intentar reclamar, la interfaz muestra **`INVENTARIO LLENO · LIBERA ESPACIO`** y conserva el resultado en espera (`AwaitingDecision`) hasta que el jugador libere una unidad en los pedestales de la bóveda.

Durante una expedición sigue siendo posible visitar la bóveda por el acceso norte. Esa visita conserva la sesión en memoria y suspende su combate; al regresar se retoma. Volver desde un resultado inicia la preparación de una nueva expedición.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Mecánicas

## Movimiento y dash

La velocidad de desplazamiento se calcula multiplicando la velocidad base de locomoción (`8 m/s` en `PlayerLocomotion.asset`, con aceleración de `80 m/s²` y frenado de `70 m/s²`) por el multiplicador de `PlayerRuntimeStats`:

**Velocidad de movimiento = `8 m/s × (1 + 0,04 × FLASH_02) × (1 + 0,10 × [Basket×N] + 0,15 × [Yanikua×N]) × (1 + [Chico Emo×N tras dash])`** *(multiplicada además por `1 + 0,20` mientras dura `BIO_05` Botiquín de combate)*.

*(Exactamente en `CompanionPowerRuntime.cs`: `(1 + 0,10 × Copies(Basket) + 0,15 × Copies(Yanikua)) × (1 + Copies(Chico Emo) si el impulso post-dash está activo)`.)* La diagonal mantiene la misma velocidad máxima. La dirección de entrada se interpreta respecto a la cámara y el personaje se apoya sobre el relieve, las rocas bajas (deslizamiento lateral integrado en `f965aa7`) y las escaleras del escenario.

El dash impulsa a `22 m/s` durante una duración base de `0,15 s` (`3,30 m`), extendida por `DASH_05` (*Paso de ariete*, `+25% / +45% / +70%` hasta `4,125 / 4,785 / 5,61 m`), concede `0,22 s` de invulnerabilidad activa (`dashInvulnerability = 0.22`) y atraviesa cuerpos enemigos. Se activa con `Espacio` en teclado o el botón sur (`A` / `Cruz`) en mando. Los muros y obstáculos sólidos limitan el recorrido. La recarga base es de `1,0 s`. La mejora `DASH_01` (*Dash ágil*) añade `+8%` de velocidad de dash y `-10%` de recarga por nivel; `DASH_10` (*Batería de rescate*) recupera `40% / 70% / 100%` de la recarga restante cada `20` gemas recogidas; y al adquirir `DASH_04` (*Cataclismo*), la recarga del dash queda fijada en **`0,5 s`** (anulando la reducción porcentual de recarga de `DASH_01`), conservando íntegramente el bono de `+8%` por nivel de velocidad de `DASH_01` y el bono de distancia de `DASH_05`.

## Linterna

| Parámetro base | Valor |
| --- | ---: |
| Alcance | 7 m |
| Apertura del cono | 60° |
| Daño de cada pulso | 3,75 sobrenatural |
| Pulsos por ráfaga | 4 |
| Separación entre pulsos | 0,20 s |
| Descanso entre ráfagas | 0,40 s |
| Impulso del cuarto pulso | 10 m/s, modulado por la susceptibilidad del enemigo |

El **multiplicador de cadencia de ataque** en `PlayerRuntimeStats` responde a la fórmula:

**Multiplicador de cadencia = `1 + 0,15 × FLASH_02 + 0,15 × [Joel] + 0,20 × [Lisa]`**

Este multiplicador divide tanto el intervalo entre pulsos (`0,20 s / mult`) como la pausa posterior al cuarto pulso (`0,40 s / mult`, o `0 s` con `FLASH_05`). *Luz fija* (`FLASH_08`), *Botiquín de combate* (`BIO_05`) y *Sobrecarga de pulso* (`FLASH_12`, `-12/20/28%` por baja en la ráfaga hasta `-75%`) acortan adicionalmente esa pausa. La ráfaga base emite en `0,2`, `0,4`, `0,6` y `0,8 s` y vuelve a empezar tras el descanso. La linterna busca objetivos en `360°` dentro de su alcance y aplica los impactos a su cono frontal; sin objetivos, vuelve hacia la orientación de movimiento.

Cuando el jugador adquiere **`FLASH_05` (*Rayo solar*)**, se elimina el descanso entre ráfagas (`0 s`), el alcance base pasa a `14 m`, el daño de luz se multiplica por `2,5` y en el código (`FlashlightWeapon.cs`, `bool finisher = pulseNumber == PulsesPerBurst;`) **todos los pulsos frontales del rayo continuo aplican empuje**. En cambio, las mejoras vinculadas al cuarto pulso —**`FLASH_03` (*Prisma voltaico*)**, **`BIO_02` (*Sifón*)** y **`CRIT_04` (*Ráfaga crítica*)**— **siguen evaluándose exclusivamente cuando `PulseIndex == 4`**, incluso con *Rayo solar* activo. Adicionalmente, **`FLASH_13` (*Condensador solar*)** sobrecarga la ráfaga posterior a un dash (`×2,0` daño y `×1,5` alcance) y **`FLASH_14` (*Faro de vacío*)** atrae levemente en pulsos sin empuje aplicando `+25%` de vulnerabilidad al daño (`2,5 s`).

El alcance y el ángulo se evalúan en el plano horizontal X/Z y tienen en cuenta el radio del objetivo. Tanto el autoapuntado como cada impacto trazan una línea 3D desde la luz hasta el centro del enemigo. Los obstáculos sólidos de la capa `Default` pueden bloquearla aunque el enemigo esté dentro del cono. El trazado ignora los triggers. Cada enemigo válido recibe como máximo un impacto por pulso.

Al comenzar la ráfaga se fijan alcance, apertura, daño base por pulso, intervalo, descanso, empuje, probabilidad y multiplicador críticos y modo solar. Una mejora que cambie esos parámetros durante la secuencia se refleja al comenzar la siguiente. La dirección de apuntado y la visibilidad de cada objetivo se vuelven a evaluar durante la ráfaga.

## Salud y daño

La **salud máxima** del Coleccionista se calcula en `PlayerRuntimeStats.cs` mediante la fórmula:

**Salud máxima = `(100 + 20 × BIO_01 + 10 × SCRAP_01 + 10 × [Lobito×N] + 15 × [Soldier×N]) × (1 + [Yanikua×N])`**

El daño puede ser físico o sobrenatural según su fuente; los números flotantes conservan esa categoría. La probabilidad crítica empieza en `0%` y su multiplicador base es `×1,75`.

Cada vez que el jugador recibe un impacto, `PlayerHealth.ReceiveDamage` evalúa la mitigación en este **orden exacto**:

1. **Invulnerabilidad activa:** comprueba si el jugador está en dash (`0,22 s`), en ventana de protección post-golpe (`damageProtectionDuration = 0,40 s` en `PlayerCombat_Default.asset`), bajo el escudo de *Nanobots* (`2 s`) o bajo la protección de *Yanikua* (`3 s`). Si cualquiera está activo, el golpe se ignora por completo.
2. **Bloqueo periódico del acompañante Hada:** si está disponible alguno de sus enfriamientos independientes (`20 s` por cada copia equipada en `CompanionPowerRuntime`), anula `1` golpe completo.
3. **Tirada de Evasión:** hasta un tope del `35%` (`bonusEvasion + DASH_09`, `+6%/+12%/+18%`). Si la tirada tiene éxito, anula el daño sin activar la invulnerabilidad post-golpe.
4. **Reducción del acompañante Soldier:** multiplica el daño entrante por `1 - 0,15 × Copies(13)` (`-15%` por copia equipada, acumulable).
5. **Tirada de Bloqueo:** hasta un tope técnico del `50%` (`bonusBlockChance + SCRAP_08`, `+10%/+18%/+26%`). Si bloquea, reduce el daño a la mitad (`× 0,5`) y con `SCRAP_08` (*Escudo deflector*) libera una onda de choque física (`18/30/45` en `2,5 m`).
6. **Armadura:** suma `10% × SCRAP_01 + 10% × Copies(13) [Soldier]` en `PlayerRuntimeStats`, hasta un tope máximo del **`60%`**, y multiplica el daño restante por `1 - armadura`.
7. **Salvación ante daño letal, umbral crítico y contraataque:** si el daño resultante sería letal (`HP <= 0`), primero intenta activar la salvación de **Yanikua** (`1` uso por copia equipada en cada expedición: restaura al `30%` de la salud máxima y otorga `3 s` de invulnerabilidad). Si el jugador sobrevive al golpe (`RemainingHealth > 0`), *Chatarra reactiva* (`DEMO_07`) detona una explosión física alrededor cada `2 s`, y si queda por debajo del `20%` de vida, **Nanobots** (`BIO_03`, `60 s` de enfriamiento) activa **`2 s` de protección temporal** (`GrantUpgradeProtection(2f)`), sin evitar golpes letales ni curar automáticamente hasta el `20%`.

| Estado del enemigo | Aplicación y expiración |
| --- | --- |
| Ralentización | *Freón residual*, *Cable eléctrico* e *Imán criogénico* aplican reducción de velocidad conservando intensidad y tiempo mayores. |
| Congelación | *Cero absoluto* cuenta pulsos sobre objetivo ralentizado y vivo; *Ventisca* y *Quiebre de hielo* congelan directamente. Al congelar con *Imán criogénico* (`CRYO_05`), atrae gemas y enfría alrededor. El objetivo congelado recibe `×1,30` de daño. |
| Vulnerabilidad | *Faro de vacío* (`FLASH_14`) marca al objetivo durante `2,5 s`, aumentando en `+25%` (`×1,25`) todo daño recibido. |
| Aturdimiento | Conserva el tiempo restante mayor y bloquea el control del enemigo. |
| Empuje (*knockback*) | Se resuelve según la susceptibilidad y la desaceleración de cada arquetipo (tabla inferior). |

### Respuesta al empuje (*knockback*) por arquetipo

| Arquetipo | Susceptibilidad al empuje | Desaceleración de frenado (`EnemyArchetypeSettings` / `.asset`) |
| --- | ---: | ---: |
| Chatarra Residual | `1,0` | `28 m/s²` |
| Interceptor | `1,0` | `18 m/s²` |
| Mortero Campana | `0,7` | `24 m/s²` |
| Polilla Reactor | `1,0` | `22 m/s²` |
| Caballero Fantasma (élite) | `0,35` | `28 m/s²` |
| Coloso | `0` (inmune) | `30 m/s²` (sin impulso) |
| Centinela de Cobre (élite) | `0` (inmune) | `24 m/s²` (sin impulso) |
| Gólgora (jefe) | `0` (inmune) | `30 m/s²` (sin impulso) |

Los números flotantes muestran la salud realmente perdida. Los impactos normales de una ráfaga pueden agruparse, mientras que los críticos se distinguen con tamaño, color y un signo de exclamación. Los impactos mitigados por resistencia muestran «Resistido» bajo el valor y una ejecución causada por `CRIT_03` muestra «EJECUTADO».

## Recogida

El imán base alcanza `2,5 m` y atrae los objetos a `12 m/s`. Las gemas conceden experiencia; el botiquín base recupera `15 HP`. El ácido y las áreas de ataque utilizan las mismas reglas de salud e invulnerabilidad que el resto del combate. Las zonas de lava fija del mapa están desactivadas; el magma de Gólgora sigue formando parte del encuentro.

Una gema nueva puede sumarse a una gema activa a un máximo de `0,75 m` de distancia horizontal. Se acumulan la XP y el número de gemas originales. *Micro-ensamblador* cuenta las gemas originales del objeto recogido. La XP otorgada es la parte entera de `XP acumulada del objeto × multiplicador de XP + resto anterior`. El resto se conserva para la próxima recogida de la expedición. *Chatarrero* añade `15%` de XP por nivel.

## Entidades y poderes de inicio

Hasta tres copias equipadas en los puestos del balcón (`EQUIPO 1`, `EQUIPO 2`, `EQUIPO 3`) activan poderes propios, independientes de las 62 cartas: bonos de estadísticas, ataques automáticos, curación, protección o salvación. **Los poderes sí se acumulan por copia equipada (`Copies(id)`, hasta `×3`)**, de modo que equipar copias repetidas multiplica sus bonos o cargas independientes. Las cartas mantienen sus niveles y requisitos y no reciben niveles iniciales por equipo. El catálogo completo, las recetas de fusión de `30 s` y el Índice de Colección (`[I]`) están en [[04 - Objetos y Coleccionables]].

Los coleccionables que aparecen en la arena requieren pulsar **`E`** (o el botón oeste en mando) dentro de unos `2,5 m` con línea de interacción despejada, y no se recogen con el imán de gemas y botiquines. Si el jugador ya ha alcanzado el límite máximo de **18 unidades** en su colección (`18/18`), el fragmento no puede recogerse y muestra en pantalla el aviso flotante **`INVENTARIO LLENO / LIBERA ESPACIO`**.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Sistemas

| Sistema | Funcionamiento actual |
| --- | --- |
| Flujo general | Menú inicial con `CONTINUAR` / `NUEVA PARTIDA` en **3 ranuras de guardado** (`SaveSlotService`), alias único para ranking, gestión de colección, bóveda de **18 cápsulas**, selección de ronda, expedición (con reanudación automática por snapshot), y retorno desde resultados. |
| Entradas y locomoción | Teclado, ratón y mando completos (`WASD` / stick, `Espacio` / `A` para dash con deslizamiento lateral en rocas bajas, `E` / botón oeste para recoger/equipar/liberar/fusionar, `T` / botón norte para desequipar rápido en balcón, `I` / `Select` para Índice de Colección y `Q` / `Select` mantenido para detalles del HUD en combate). |
| Sesión | Reloj, razones de pausa (`Manual`, `FocusLost`, `TacticalDraft`, `FusionMenu`, `CompanionMenu`, `FrontEnd`, `SessionEnded`), captura periódica y por eventos de snapshots (`ExpeditionSnapshotService`), entrada del jefe, resolución de resultados y limpieza entre expediciones. |
| Salud y daño | Salud máxima escalable, orden de mitigación en `PlayerHealth`, armadura hasta `60%` (`SCRAP_01` + `Soldier`), evasión hasta `35%` (`DASH_09`), bloqueo hasta `50%` (`SCRAP_08` con onda deflectora), vulnerabilidad (`FLASH_14`), críticos, protección temporal (`0,40 s`), curación y salvación/protección (*Yanikua* ante daño letal y *Nanobots* al sobrevivir bajo `20%` de vida). |
| Linterna | Autoapuntado en 360°, ráfagas de cuatro pulsos con empuje final y modo continuo *Rayo solar* (`FLASH_05`), además de sobrecarga tras dash (`FLASH_13`), reducción de descanso por baja (`FLASH_12`) y faro de vacío (`FLASH_14`). |
| Enemigos | Cinco arquetipos de horda, dos élites y Gólgora con navegación presupuesta por frame (`EnemyObstacleNavigator`), ataques, estados alterados y tabla unificada de susceptibilidad y frenado al empuje. |
| Oleadas | Grupos por fase, apariciones fuera de cámara, límites de población (150 normales, máx. 6 Morteros) y aparición alterna de élites desde los 90 s. |
| Encuentro del jefe | Traslado seguro a la caldera de 22 m, cámara compartida, refuerzos anunciados (límite de 8 secuaces) y dos fases con ataques de Bombardeo, Cerco de magma y Fisura de magma. |
| Progresión | XP, subidas de nivel y cola de elecciones entre **62 mejoras distribuidas en cinco rarezas** (Común, Rara, Épica, Legendaria y **Mítica**, con `MYTH_01` *All In*). |
| Entidades y capacidad | Apariciones temporales en arena, recogida manual con `E` (o botón oeste), capacidad estricta de **18 unidades** (`DisplaySlots[18]`: 3 de equipo en balcón + **15** de exposición en planta baja) y bloqueo al llenarse (`Bóveda llena (18/18)` / `INVENTARIO LLENO`). Los poderes se acumulan por copia equipada hasta `×3`. |
| Equipo y liberación | Tres puestos en el balcón superior (`EQUIPO 1..3`, `E` / botón oeste para editar y `LIBERAR COPIA` dentro del selector, y `T` / botón norte para desequipar) y **quince** puestos de exposición en planta baja (`E · LIBERAR` con confirmación modal). |
| Índice de Colección | Panel consultable en la bóveda con `I` / `Select` (`VaultCollectionIndex`): contador `{descubiertos} / 15 DESCUBIERTOS · {en posesión} / 18 UNIDADES`, pestañas `POR RAREZAS` (`TODAS` + 6 rarezas con siluetas negras `???`) y `EN TU BÓVEDA`, e inspector lateral de poderes. |
| Fusión | Estación ubicada en la pared trasera de la bóveda (`x = 189, z = 28,88`) que combina 4 copias de la misma rareza o de rarezas mixtas (incluidas copias equipadas `EN EQUIPO 1/2/3`, excluida Yanikua), duración fija de **30 segundos (`00:30`)** y botón único `COLECCIONAR` al reclamar. |
| Señalización de bóveda | Rótulos 3D `EQUIPO 1..3`, indicadores direccionales dinámicos en el borde superior (`VaultDirections`) y panel rotatorio de consejos cada `8 s` (`VaultTips`). |
| Interfaz y ajustes | Tipografía unificada `Pixelify Sans`, panel compartido `MenuSettingsView` en 3 pestañas (`General` con **`Límite de FPS` `30 / 60 / 120`** en pausa, `Efectos reducidos` en título y **`Pantalla` `COMPLETA` / `VENTANA`** en ambos, `Interfaz` con `Atributos de coleccionables SÍ/NO`, `Sonido`), HUD con grid de mejoras `9 × 34 px` (hasta 62 cartas) y consulta rápida (`Q` / `Select`), minimapa con niebla de exploración y resultados. |
| Persistencia y Ranking | Almacenamiento atómico en **3 ranuras** (`Saves/slot_0..2.json`, `expedition_0..2.json`, `index.json`, `ranking_outbox.json`), reserva de gamertag por SHA-256 (`GamertagRegistry`) y publicación de **todos los intentos válidos** ordenando la mejor puntuación por gamertag en [`https://echoscrap.web.app`](https://echoscrap.web.app) (`EventLeaderboardSync`). |
| Audio | Música adaptativa de arena, jefe y bóveda con control de volumen general y de música; sonidos de linterna, recogida, entrada, fusión y ataques del jefe. |

El reloj distingue pausa manual, pérdida de foco, cartas, fusión/índice (`PauseReason.FusionMenu`), selector de equipo y menús generales. Resolver una de esas razones no reanuda una sesión que sigue detenida por otra. La interfaz se anima con `Time.unscaledDeltaTime` mientras el combate está pausado.

La simulación resuelve daño y resultados. La presentación representa esos eventos con animación, sonido y VFX. Las reservas de actores y efectos se reutilizan entre expediciones. `CompanionPowerRuntime` mantiene los bonos, ataques, curaciones, protección y salvación de las entidades equipadas; sus recargas no se reinician al cambiar equipo durante una expedición.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Controles

## Cómo se juega y cómo avanzar

1. **Inicio y bóveda (preparación):** en el menú inicial elige **Continuar** o **Nueva partida** sobre cualquiera de las **3 ranuras de guardado** (con opción de reanudar una expedición activa desde su último snapshot y de registrar alias para el ranking en línea). Ya dentro de la bóveda, usa los rótulos `EQUIPO 1..3`, el **Índice de Colección (`I` / `Select`)** y la **Máquina de Fusión (`E` / botón oeste)** en la pared trasera para equipar hasta **3 coleccionables** cuyos poderes pasivos se suman y acumulan por copia equipada, o combinar **4 copias** durante `30 s` para obtener entidades de mayor rareza (límite máximo de `18` unidades).
2. **Entrada a la arena:** acércate al **portal norte** en la bóveda para abrir `ELIGE LA RONDA` y seleccionar **Ronda Demo** (`3:00` hasta el jefe) o **Ronda Completa** (`8:00` hasta el jefe).
3. **Combate y progresión:** muévete (`WASD`/flechas o stick/cruceta) y esquiva con **Dash (`Espacio` / `A` / `Cruz`)** mientras tu linterna ataca automáticamente al enemigo más cercano. Recoge **gemas de XP** para subir de nivel y elegir mejoras (`1/2/3`, clic o mando), recoge **botiquines** para curarte y pulsa **`E`** (o botón oeste **`X` / `Cuadrado`**) cerca de los fragmentos caídos para capturar nuevas entidades coleccionables.
4. **Objetivo y victoria:** sobrevive hasta el final del reloj para invocar al jefe final **Gólgora (MataColeccionista)** en la caldera central (`22 m`) y derrótalo para ganar la incursión, registrar tu puntaje y regresar a la bóveda con tus capturas.

| Acción | Teclado y ratón | Mando |
| --- | --- | --- |
| Moverse | `WASD` o flechas | Stick izquierdo o cruceta |
| Dash | `Espacio` | Botón sur (`A` / `Cruz`) |
| Pausar / abrir ajustes (`General`, `Interfaz`, `Sonido`) | `Esc` | `Start` |
| Consultar estadísticas completas y poderes de acompañantes en HUD | Mantener `Q` | Mantener `Select` |
| Elegir una carta | `1`, `2` o `3`; clic | Navegación y confirmación |
| Cambiar carta enfocada | `A` / `D`, izquierda / derecha o puntero | Stick izquierdo o cruceta |
| Confirmar carta | `Enter` o clic | Botón sur (`A` / `Cruz`) |
| Reintentar oferta con *Sintonizador* | `R` o botón de interfaz | Botón norte (`Y` / `Triángulo`) |
| Recoger una entidad cercana en la arena | `E` (si hay hueco en `18/18`) | Botón oeste (`X` / `Cuadrado`) |
| Abrir / cerrar el Índice de Colección en la bóveda | `I` o clic en `ÍNDICE · [I]` (cierra también con `Esc`) | Botón `Select` (`VaultCollectionIndex`, cierra también con `B` / `Círculo`) |
| Equipar o editar un puesto de equipo (puestos 1 a 3, balcón) | `E` (`E · EQUIPAR` / `E · EDITAR`) | Botón oeste (`X` / `Cuadrado`) |
| Desequipar directamente un puesto de equipo ocupado en el mundo (puestos 1 a 3) | `T` | Botón norte (`Y` / `Triángulo`) |
| Liberar una unidad expuesta (planta baja) | `E` (`E · LIBERAR`); confirmar en modal con `E`, `Enter` o `Espacio` (`Esc` cancela) | Botón oeste (`X` / `Cuadrado`); en modal, `A` / `Cruz` confirma y `B` / `Círculo` cancela |
| Abrir la máquina de fusión en la bóveda | `E` (`E · FUSIONAR` / `E · VER FUSIÓN`) | Botón oeste (`X` / `Cuadrado`) |
| Cerrar selector de equipo (`CompanionView`) | `E`, `Esc` o botón `CERRAR · E / ESC` | Botón oeste (`X` / `Cuadrado`) o botón este (`B` / `Círculo`) |
| Cerrar ventana de fusión (`FusionStation`) | `E`, `Esc` o botón `X` superior derecho | Botón oeste (`X` / `Cuadrado`) o botón este (`B` / `Círculo`) |
| Navegar entre consejos de la bóveda (`VaultTips`) | Clic en `<` / `>` | — |
| Cancelar selección de ronda o confirmación general | `Esc` o botón de interfaz | Botón este (`B` / `Círculo`) |

La linterna ataca automáticamente al objetivo válido más cercano. Los portales se activan por contacto; el de la bóveda abre la selección de ronda (`ELIGE LA RONDA`) durante la preparación.

`Espacio` se reserva al dash durante el combate y la selección de cartas para evitar confirmaciones accidentales; en el modal de confirmación de liberación de la bóveda (`LIBERAR UNIDAD`), tanto `E` como `Enter` o `Espacio` confirman la liberación y `Esc` la cancela.

Mantener pulsada la tecla **`Q`** en teclado o el botón **`Select`** en mando durante el combate (`CombatHud.IsHudDetailHeld`) despliega temporalmente el panel de estadísticas en modo completo (`COMPLETAS`) y muestra las líneas descriptivas de los poderes de los tres acompañantes equipados aunque estén ocultos o reducidos en la pestaña `Interfaz` del menú de pausa.

El laboratorio de pruebas (`EnemyLaboratoryPanel`) usa `Tab` para mostrar u ocultar el panel y `R` para reiniciar la escena de prueba.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Progresión

## Experiencia y elecciones

El Coleccionista empieza en nivel 1. La primera subida necesita 100 XP; el siguiente umbral es el anterior multiplicado por 1,5 y redondeado a entero. La experiencia que sobra se conserva para el siguiente nivel. Una misma recogida puede acumular varias elecciones.

Cada elección ofrece hasta tres cartas distintas. La rareza base se sortea con pesos Común 60, Rara 25, Épica 12, Legendaria 3 y **Mítica 1**, considerando únicamente las rarezas que todavía tienen cartas válidas. Después se elige una carta de esa rareza. Las cartas al máximo se retiran de la oferta.

*Sintonizador* permite gastar reintentos durante la sesión. También mueve peso hacia raras y épicas: cinco y tres puntos por nivel, respectivamente, desde el peso común (mínimo `1`). Los pesos legendario (`3`) y mítico (`1`) no cambian.

## Catálogo de 62 mejoras

Las series separadas por barras corresponden a sus niveles. Los valores describen la mejora por sí sola; otras cartas y acompañantes pueden modificar daño, radios, proyectiles, críticos y estadísticas finales.

| ID | Mejora | Rareza | Nivel máximo | Efecto actual | Tipo de daño |
| --- | --- | --- | ---: | --- | --- |
| FLASH_01 | Más alcance | Común | 5 | +1 m de alcance y +10° de apertura por nivel. | Sobrenatural |
| FLASH_02 | Pulso rápido | Común | 5 | +15% de cadencia de ataque por nivel (y +4% de velocidad de movimiento en `PlayerRuntimeStats`); reduce intervalo y descanso. | Sobrenatural |
| FLASH_03 | Prisma voltaico | Rara | 3 | El cuarto pulso (`PulseIndex == 4`) enlaza 2/3/4 enemigos (`+ExtraProjectileCount`) a 5 m, con 40/60/80% del daño del impacto inicial. | Sobrenatural |
| FLASH_04 | Golpe potente | Épica | 3 | Daño ×1,5/1,8/2,2 y +8/14/20 m/s de empuje; reduce 20° la apertura al adquirirla. | Sobrenatural |
| FLASH_05 | Rayo solar | Legendaria | 1 | Suprime el descanso (`0 s`), establece 14 m de alcance base, multiplica el daño de luz por 2,5 y aplica empuje en **todos los pulsos** (`capturedSolarMode` en `FlashlightWeapon.cs`); las mejoras de cuarto pulso (`FLASH_03`, `BIO_02` y `CRIT_04`) siguen evaluando únicamente `PulseIndex == 4`. | Sobrenatural |
| FLASH_06 | Choque pesado | Rara | 3 | El cuarto pulso marca al enemigo (`0,35 s`); si choca contra un obstáculo inflige `18/30/45` de daño físico en `2,2 m` escalados y aturde `0,5 s` (`0,35 s` de enfriamiento global). | Físico |
| FLASH_07 | Doble foco | Épica | 3 | Añade un haz trasero al `75%` del alcance con `45/60/75%` del daño y empuje proporcional en el cuarto pulso. | Sobrenatural |
| FLASH_08 | Luz fija | Rara | 3 | Tras `0,35 s` sin moverse ni hacer dash: `+15/25/35%` daño por pulso, `+2 m` alcance y `-50%` pausa de ráfaga. | Sobrenatural |
| FLASH_09 | Descarga orbital | Épica | 3 | Cada `3,5/2,8/2 s` invoca rayos verticales sobre `2/3/4` enemigos (`+ExtraProjectileCount`) en `11 m`, dañando `32/52/78` en `1,8 m` escalados. | Sobrenatural |
| FLASH_10 | Barrido de chatarra | Común | 3 | Cada `1,6 s` ejecuta un látigo frontal (`120°`, `4,5 m` escalados) de `20/26/32` de daño (`14 + 6×nivel`) y empuje `6 m/s`. | Físico |
| FLASH_11 | Amplificador de área | Épica | 3 | `+12% / +24% / +36%` a `AreaMultiplier` (amplía auras, ondas, charcos, estelas, sierras, rayos orbitales, barridos y explosiones). | Según ataque |
| FLASH_12 | Sobrecarga de pulso | Rara | 3 | Cada baja con la linterna reduce la pausa de esa misma ráfaga `-12/20/28%` por baja (hasta un máximo de `-75%`). | Sobrenatural |
| FLASH_13 | Condensador solar | Épica | 1 | Realizar un dash sobrecarga la próxima ráfaga (`1,5 s`), duplicando su daño (`×2,0`) y ampliando su alcance `+50%` (`×1,5`). | Sobrenatural |
| FLASH_14 | Faro de vacío | Legendaria | 1 | El haz atrae levemente a los enemigos golpeados (`3,5 m/s` inversos en pulsos sin empuje) y aplica `+25%` de daño recibido durante `2,5 s`. | Sobrenatural |
| DASH_01 | Dash ágil | Común | 5 | +8% de velocidad de dash y -10% de recarga por nivel (el bono de +8%/nivel de velocidad se conserva con `DASH_04`). | Sin daño directo |
| DASH_02 | Onda sísmica | Rara | 3 | Al terminar el dash: 25/45/70 de daño en 3,5/4,5/5,5 m (escalados por `AreaMultiplier`) y aturdimiento de 0,6/0,9/1,2 s. | Físico |
| DASH_03 | Holo-señuelo | Épica | 3 | Deja una copia que atrae enemigos en 12 m durante 2/2,5/3 s; su implosión en 4 m escalados causa 50/85/120 de daño. | Sobrenatural |
| DASH_04 | Cataclismo | Legendaria | 1 | Fija la recarga del dash en **0,5 s** (anulando la reducción de recarga de `DASH_01`, pero **manteniendo su +8%/nivel de velocidad de dash**), con explosión de salida de 120 (`4 m` escalados) y cuchilla de llegada de 80. | Físico |
| DASH_05 | Paso de ariete | Rara | 3 | Aumenta `+25% / +45% / +70%` la duración efectiva y distancia del dash (`4,125 / 4,785 / 5,61 m` desde `3,30 m`, sin daño ni arrastre). | Sin daño directo |
| DASH_06 | Paso atrás | Épica | 3 | Deja una marca durante `2,5 s`; volver a pulsar dash regresa al punto de partida (`0,25 s` invuln.) dañando `35/55/80` en `0,65 m` de ancho. | Sobrenatural |
| DASH_07 | Cable eléctrico | Rara | 3 | Al terminar el dash tiende un cable de `2/2,5/3 s` (máx. `2`, ancho `0,65 m`) que daña `7/12/18` cada `0,4 s` y ralentiza `35%`. | Sobrenatural |
| DASH_08 | Estela incendiaria | Rara | 3 | El dash deja focos ardientes cada `0,85 m` (`2,2 s` de duración, radio `0,9 m` escalado) que infligen `8/13/20` cada `0,45 s`. | Sobrenatural |
| DASH_09 | Reflejos de chatarra | Común | 3 | Otorga `+6% / +12% / +18%` de probabilidad de evasión acumulable hasta el tope global de `35%`. | Sin daño directo |
| DASH_10 | Batería de rescate | Común | 3 | Cada `20` gemas de XP recogidas recupera el `40% / 70% / 100%` del tiempo de recarga restante del dash. | Sin daño directo |
| BIO_01 | Más vida | Común | 5 | +20 HP máximos y curación de 15 HP en cada adquisición. | Sin daño directo |
| BIO_02 | Sifón | Rara | 3 | El cuarto pulso (`PulseIndex == 4`) tiene 12/20/30% de curar 2/3/4 HP por enemigo alcanzado. | Sin daño directo |
| BIO_03 | Nanobots | Épica | 3 | Regenera 1/2/3 HP cada 2,5 s; si el jugador sobrevive (`RemainingHealth > 0`) a un golpe quedando bajo el 20% de vida, otorga 2 s de protección temporal (`60 s` de enfriamiento; no evita golpes letales ni cura hasta el 20%). | Sin daño directo |
| BIO_04 | Nova de plasma | Legendaria | 1 | Cada 15 HP de curación sobrante generan una nova de 80 de daño y 6 m de radio (escalados por `AreaMultiplier`). | Sobrenatural |
| BIO_05 | Botiquín de combate | Épica | 3 | Recoge botiquines a vida llena (`+3%` aparición) y concede `+20%` velocidad y `+25%` cadencia (`-50%` descanso) durante `3/4/5 s`. | Sin daño directo |
| GREED_01 | Imán | Común | 5 | +2 m al radio de recogida por nivel. | Sin daño directo |
| GREED_02 | Chatarrero | Común | 5 | +15% de experiencia recibida por nivel. | Sin daño directo |
| GREED_03 | Botiquín mejor | Rara | 3 | Botiquines de 17/19/21 HP; aumentan su probabilidad de aparición (4/5/6% normal y 10/12/15% en pesados/élites). | Sin daño directo |
| GREED_04 | Sintonizador | Épica | 3 | Concede 1/2/3 reintentos totales y desplaza peso común hacia ofertas raras (+5/nivel) y épicas (+3/nivel). | Sin daño directo |
| GREED_05 | Pulso colector | Legendaria | 1 | Atrae todas las gemas y botiquines activos del mapa al adquirirla y al subir de nivel. | Sin daño directo |
| GREED_06 | Gemas afiladas | Rara | 3 | `+1,5 m` al imán de gemas; al volar hacia el jugador infligen `6/10/15` de daño físico (`0,18 m`; gemas fusionadas hasta `×3`). | Físico |
| GREED_07 | Imán bumerán | Rara | 3 | Cada `2,8 s` lanza `1/1/2` bumeranes (`+ExtraProjectileCount`) de `22/36/52` de daño a `18 m/s` (`0,55 m` escalados) que regresan tras `0,48 s` golpeando ida y vuelta. | Físico |
| CRIT_01 | Óptica precisa | Común | 5 | +8 puntos porcentuales de probabilidad crítica por nivel. | Según ataque |
| CRIT_02 | Calibración letal | Común | 5 | +0,2 al multiplicador crítico por nivel, desde ×1,75. | Según ataque |
| CRIT_03 | Ejecución | Rara | 3 | Remata tras un golpe a enemigos bajo 6/8/10% de vida; excluye a los jefes. | Según ataque |
| CRIT_04 | Ráfaga crítica | Épica | 3 | Un crítico en el cuarto pulso (`PulseIndex == 4`) garantiza 2/3/4 críticos al inicio de la siguiente ráfaga. | Según ataque |
| CRIT_05 | Ojo del verdugo | Legendaria | 1 | +20 puntos porcentuales de crítico; bajo 35% de vida antes del golpe, triplica el daño crítico ya calculado y emite aturdimiento de 1 s en 3 m escalados. | Según ataque |
| DEMO_01 | Pólvora volátil | Común | 5 | +12% de daño y +15% de radio de explosión por nivel (`ExplosionRadiusMultiplier` compuesto con `AreaMultiplier`). | Según ataque |
| DEMO_02 | Reacción en cadena | Rara | 3 | 25/40/55% de provocar una explosión de 35/60/90 (radio 3 m) al morir un enemigo. | Físico |
| DEMO_03 | Minas de chatarra | Épica | 3 | Al iniciar dash deja 1 mina (`45/70/100` de daño físico en `3 m`), con armado en `0,25 s`, radio de activación de `1,25 m`, empuje de `5 m` y límite de **5 minas activas simultáneas** (reemplaza la más antigua al colocar la sexta). | Físico |
| DEMO_04 | Termobárica | Legendaria | 1 | A los **0,4 s**, toda explosión repite el **75%** de su daño como sobrenatural en el mismo radio y atrae a los enemigos hacia el centro mediante desplazamiento directo (`PushDistance`) de hasta el **45% del radio de la explosión**. | Sobrenatural |
| DEMO_05 | Charcos de plasma | Rara | 3 | Cada `4/3,2/2,5 s` lanza un charco hacia amenazas en `10 m` (`3,5 s` de duración, radio `2,2 m` escalado) que daña `10/16/24` cada `0,4 s`. | Sobrenatural |
| DEMO_06 | Chatarra parabólica | Épica | 3 | Cada `3,0 s` arroja `2/3/4` proyectiles en arco (`+ExtraProjectileCount`) de `28/45/68` de daño a `14 m/s` (`0,5 m` escalados) atravesando muros. | Físico |
| DEMO_07 | Chatarra reactiva | Rara | 3 | Al recibir daño no letal (`2,0 s` enfriamiento) libera una explosión física alrededor (`3 m`) de `30/40/50` de daño (`20 + 10×nivel`). | Físico |
| SCRAP_01 | Blindaje | Común | 5 | +10 HP máximos y **`+10%` de armadura por nivel** (`armorPerTier = 0.10f` unificado en `UpgradeTuning.cs`, `UpgradeTuning.asset` y `PlayerRuntimeStats`, hasta un tope total del `60%`). | Sin daño directo |
| SCRAP_02 | Micro-ensamblador | Rara | 3 | Cada 15 gemas recogidas lanza `1` proyectil guiado (`+ExtraProjectileCount`) de 40/70/105 a 24 m/s. | Físico |
| SCRAP_03 | Sierras orbitales | Épica | 3 | `1/2/3` hojas (`+ExtraProjectileCount`, hasta `4`) giran a **180°/s** a **2,5 m** escalados (`0,65 m` de radio escalado), infligiendo `18/28/40` de daño físico y `4 m/s` de empuje mediante `StepSaws()` en solape. | Físico |
| SCRAP_04 | Torreta centinela | Legendaria | 1 | Cada **`10 s`** despliega 1 torreta de **`12 s`** de duración (hasta **2 torretas activas simultáneas**). Cada **0,6 s** disparan una ráfaga de **3 proyectiles** separados por **0,08 s** a **28 m/s**, con **`10`** de daño y **`12 m`** de alcance (`UpgradeTuning.cs` y `.asset`). | Físico |
| SCRAP_05 | Aura de estática | Común | 3 | Emite un campo eléctrico continuo (`2,5/3,2/4 m` escalados) que cada `0,35 s` inflige `6/10/15` sobrenatural y empuja levemente (`2,5 m/s`). | Sobrenatural |
| SCRAP_06 | Disco rebotante | Rara | 3 | Cada `3,5 s` lanza `1/2/3` discos (`+ExtraProjectileCount`) de `20/34/52` de daño a `16 m/s` (`3,5 s` vida, `0,45 m` escalados) que rebotan en muros y entre enemigos a `14 m` (`0,45 s` enfriamiento por enemigo). | Físico |
| SCRAP_07 | Duplicador de matriz | Legendaria | 1 | Suma `+1` proyectil, sierra orbital o enlace adicional (`ExtraProjectileCount = 1`) a armas automáticas, sierras orbitales y cadena voltaica. | Según ataque |
| SCRAP_08 | Escudo deflector | Rara | 3 | Añade `+10% / +18% / +26%` de bloqueo (tope `50%`); al bloquear emite una onda física de `18/30/45` en `2,5 m` escalados con empuje de `6 m`. | Físico |
| CRYO_01 | Freón residual | Común | 5 | La luz ralentiza 12/20/28/36/44% durante 1,5 s. | Sobrenatural |
| CRYO_02 | Cero absoluto | Rara | 3 | Congela tras 4/3/3 pulsos sobre objetivos ralentizados durante 1,2/1,8/2,4 s; el enemigo congelado recibe ×1,30 de daño. | Sobrenatural |
| CRYO_03 | Ventisca | Épica | 3 | Cada 6/5/4 s: onda de 30/50/75 en 5,5 m escalados que congela 1,5 s. | Sobrenatural |
| CRYO_04 | Quiebre de hielo | Legendaria | 1 | Una muerte congelada emite seis esquirlas perforantes de 50 (14 m/s, 1,2 s) que congelan 1,5 s. | Sobrenatural |
| CRYO_05 | Imán criogénico | Rara | 3 | Al congelar un enemigo atrae gemas en `6/9/12 m` escalados y ralentiza `25%` (`1,5 s`) a enemigos en `2,5 m` escalados. | Sin daño directo |
| MYTH_01 | All In | **Mítica** | 1 | Requiere otra mejora activa. Transforma al azar todas tus mejoras activas en otras cartas distintas y las sube `+1` nivel cada una (hasta el máximo de cada carta). | Sin daño directo |

## Interacciones clave entre mejoras

- **`FLASH_05` (*Rayo solar*) y empuje vs. mejoras de cuarto pulso (`FlashlightWeapon.cs`, `UpgradeCombatRuntime.cs`):** la condición `finisher || (capturedSolarMode && !rear)` hace que cada pulso frontal del rayo continuo aplique empuje. Sin embargo, **`FLASH_03` (*Prisma voltaico*), `BIO_02` (*Sifón*) y `CRIT_04` (*Ráfaga crítica*) mantienen su comprobación exclusiva `PulseIndex == 4`**, por lo que solo se disparan en el cuarto pulso de cada ciclo.
- **`DASH_04` (*Cataclismo*) con `DASH_01` (*Dash ágil*) (`PlayerRuntimeStats.cs`):** `DASH_04` fija la recarga del dash en `0,5 s` e ignora la reducción porcentual de recarga de `DASH_01` (`-10%` por nivel), pero mantiene activo el multiplicador de velocidad de desplazamiento del dash de `DASH_01` (`+8%` por nivel) y el multiplicador de duración/distancia de `DASH_05` (*Paso de ariete*).
- **`FLASH_11` (*Amplificador de área*) y `SCRAP_07` (*Duplicador de matriz*):** `AreaMultiplier` (`1 + 0,12 × FLASH_11`) escala áreas y ondas (`UpgradeCombatRuntime.QueueBlast`, auras, charcos, estelas, barridos, sierras orbitales y radio criogénico) y compone multiplicativamente `ExplosionRadiusMultiplier` con `DEMO_01`; `ExtraProjectileCount` (`1` con `SCRAP_07`) suma `+1` a `FLASH_03` (hasta `6` enlaces), `SCRAP_02`, `SCRAP_03` (hasta `4` sierras), `SCRAP_06`, `GREED_07`, `FLASH_09` y `DEMO_06`.
- **`MYTH_01` (*All In*):** al elegirse en `UpgradeDraftController.TryChoose`, sustituye cada carta poseída distinta de `MYTH_01` por otra mejora no repetida de `UpgradeCatalog` priorizando cartas ajenas a la build previa y con `MaxTier >= nivelAnterior + 1`, sube `+1` nivel cada una (limitado a `MaxTier`) y respeta los topes de salud en `PlayerHealth`. Si se agotan las cartas ajenas a la selección previa, permite reutilizar identidades anteriores; evita repetir resultados y prioriza cambiar la identidad de cada carta, pero no garantiza una identidad nueva cuando el catálogo disponible se agota.
- **`SCRAP_03` (*Sierras orbitales*):** `UpgradeTuning.cs:44-46` no declara `bladeHitCooldown`; el método `StepSaws()` (`UpgradeCombatRuntime.cs`) aplica `18 / 28 / 40` de daño (`sawDamage`) y `4 m/s` de empuje (`sawPush`) en cada paso de simulación en que el radio de la hoja (`0,65 m` a `2,5 m` del jugador, ambos escalados por `AreaMultiplier`, girando a `180°/s`) solape al enemigo.

## Fórmula de Ojo del verdugo y Vulnerabilidad

*Ojo del verdugo* añade 20 puntos porcentuales a la probabilidad crítica, limitada al 100%. Su multiplicador adicional solo se activa si el ataque resulta crítico y el objetivo está estrictamente por debajo del 35% de su vida máxima **antes** de recibir ese golpe. Un impacto que cruza desde 35% o más hasta ese umbral no recibe el bono todavía.

Para un ataque del jugador, antes de la ejecución y del límite de salud realmente perdida:

**Daño resuelto = daño de la fuente × multiplicador crítico × bono de daño de entidades × bono por congelación × bono de vulnerabilidad × multiplicador de Ojo × resistencia del objetivo.**

El multiplicador crítico vale `1` en un impacto normal y parte de `1,75` en un crítico; *Calibración letal* añade `0,2` por nivel. El multiplicador de *Ojo del verdugo* vale `3` cuando se cumplen sus condiciones y `1` en los demás casos. El bono por congelación es `1,30` mientras el objetivo esté congelado y *Faro de vacío* (`FLASH_14`) suma `+25%` de vulnerabilidad (`×1,25`) durante `2,5 s`.

Ejemplo sin otras bonificaciones ni resistencias: un ataque de `10` causa `17,5` con un crítico base; si *Ojo del verdugo* está activo y el objetivo ya estaba bajo `35%`, causa **`10 × 1,75 × 3 = 52,5`**.

Un crítico que cumple la condición emite un área de aturdimiento de `3 m` (escalada por `AreaMultiplier`) alrededor del impacto durante `1 s`. Gólgora aplica sus límites de control: duración reducida al `35%`, máximo de `0,6 s` y ventana de inmunidad de `1,5 s`. *Ejecución* (`CRIT_03`) se comprueba después del daño y sus resistencias; los números muestran la salud realmente perdida.

## Requisitos y combinaciones

- **Pólvora volátil (`DEMO_01`) y Termobárica (`DEMO_04`):** requieren *Onda sísmica*, *Holo-señuelo*, *Cataclismo*, *Reacción en cadena*, *Minas de chatarra*, *Chatarra reactiva* o *Nova de plasma*.
- **Cero absoluto (`CRYO_02`):** requiere *Freón residual* (`CRYO_01`).
- **Quiebre de hielo (`CRYO_04`):** necesita *Cero absoluto* (`CRYO_02`) o *Ventisca* (`CRYO_03`).
- **Ráfaga crítica (`CRIT_04`):** requiere una probabilidad crítica mayor que cero.
- **All In (`MYTH_01`):** requiere poseer al menos otra mejora activa antes de poder aparecer en el draft.

*Prisma voltaico* se activa en el cuarto pulso de luz (o en `PulseIndex == 4` con *Rayo solar*) y su daño derivado no inicia otra cadena. La repetición de *Termobárica* tampoco vuelve a repetirse a sí misma.

## Parámetros complementarios de balance

| Mejora | Parámetros de aplicación |
| --- | --- |
| Prisma voltaico (`FLASH_03`) | Intervalo de `0,045 s` entre saltos; radio de salto `5 m`; hasta `6` enlaces con `SCRAP_07`. |
| Golpe potente (`FLASH_04`) | Añade `8 / 14 / 20 m/s` al empuje base de `10 m/s`, para un total de `18 / 24 / 30 m/s` antes de la susceptibilidad. |
| Choque pesado (`FLASH_06`) | Ventana de choque `0,35 s`, radio `2,2 m`, aturdimiento `0,5 s` y enfriamiento global `0,35 s`. |
| Doble foco (`FLASH_07`) | Haz trasero al `75%` del alcance y `45 / 60 / 75%` del daño frontal; también empuja en el cuarto pulso. |
| Luz fija (`FLASH_08`) | Se activa a los `0,35 s` sin moverse; `+2 m` alcance, `+15/25/35%` daño y pausa entre ráfagas al `50%`. |
| Descarga orbital (`FLASH_09`) | Cada `3,5 / 2,8 / 2,0 s` impacta `2 / 3 / 4` blancos (`+ExtraProjectileCount`) en `11 m`, radio `1,8 m`. |
| Barrido de chatarra (`FLASH_10`) | Cada `1,6 s`, apertura `120°`, alcance `4,5 m`, daño `14 + 6×nivel` y empuje `6 m/s`. |
| Amplificador de área (`FLASH_11`) | `+12%` por nivel a `AreaMultiplier`; compone con `DEMO_01` en `ExplosionRadiusMultiplier`. |
| Sobrecarga de pulso (`FLASH_12`) | `-12 / -20 / -28%` de pausa por baja en la ráfaga actual, con tope del `75%`. |
| Condensador solar (`FLASH_13`) | Tras iniciar dash, la próxima ráfaga dentro de `1,5 s` aplica `×2,0` daño y `×1,5` alcance. |
| Faro de vacío (`FLASH_14`) | Impulso de atracción `3,5 m/s` inversos (en pulsos sin empuje) y `+25%` daño recibido durante `2,5 s`. |
| Onda sísmica (`DASH_02`) | Radio de `3,5 / 4,5 / 5,5 m` y empuje nominal de `8 m`, sujeto al enemigo y al terreno. |
| Holo-señuelo (`DASH_03`) | Radio de atracción de `12 m` y radio de implosión de `4 m`. |
| Cataclismo (`DASH_04`) | Recarga fija de `0,5 s` (conservando `+8%/nivel` de velocidad de `DASH_01`). Radio inicial de explosión de `4 m`. La cuchilla viaja a `18 m/s`, tiene radio de impacto de `0,45 m` y dura `0,6 s`. |
| Paso de ariete (`DASH_05`) | `+25% / +45% / +70%` a `DashDuration` y `DashDistance` (`4,125 / 4,785 / 5,61 m`), sin daño ni empuje. |
| Paso atrás (`DASH_06`) | Marca activa `2,5 s`, `0,25 s` de invulnerabilidad al volver, ancho de segmento `0,65 m`. |
| Cable eléctrico (`DASH_07`) | Máx. `2` cables simultáneos, ancho `0,65 m`, tick cada `0,4 s` y ralentización `35%`. |
| Estela incendiaria (`DASH_08`) | Paso entre focos `0,85 m`, duración `2,2 s`, radio `0,9 m` y daño cada `0,45 s`. |
| Reflejos de chatarra (`DASH_09`) | `+6% / +12% / +18%` de evasión hasta el límite global de `35%`. |
| Batería de rescate (`DASH_10`) | Cada `20` gemas de XP recupera `40 / 70 / 100%` de recarga restante del dash. |
| Botiquín mejor (`GREED_03`) | Probabilidad base de aparición de `3%` y de `4 / 5 / 6%` con la carta. Para Coloso, Mortero y ambos élites, `8%` base y `10 / 12 / 15%` con la carta. |
| Gemas afiladas (`GREED_06`) | `+1,5 m` al radio de imán para gemas; impacto en vuelo con radio `0,18 m`, multiplicado por `min(3, GemCount)`. |
| Imán bumerán (`GREED_07`) | Cada `2,8 s` a `18 m/s`, `0,48 s` de ida y retorno `×1,15`, radio `0,55 m`. |
| Reacción en cadena (`DEMO_02`) | Radio base de `3 m`. |
| Minas de chatarra (`DEMO_03`) | Al iniciar dash coloca 1 mina: retardo de armado de `0,25 s`, activación a `1,25 m`, radio de explosión de `3 m` (`45 / 70 / 100` de daño), empuje de `5 m` y máximo de **5 minas simultáneas**. |
| Termobárica (`DEMO_04`) | Repite el `75%` del daño como sobrenatural a los `0,4 s` en el mismo radio y atrae mediante desplazamiento directo (`PushDistance`) hasta el **45% del radio de explosión**. |
| Charcos de plasma (`DEMO_05`) | Lanzamiento cada `4 / 3,2 / 2,5 s` hasta `10 m`; radio `2,2 m`, duración `3,5 s`, daño cada `0,4 s`. |
| Chatarra parabólica (`DEMO_06`) | Cada `3,0 s` en arco sobre obstáculos (`14 m/s`, máx. `1,35 s`, radio `0,5 m`). |
| Chatarra reactiva (`DEMO_07`) | Daño `20 + 10×nivel` en `3 m`, enfriamiento de `2,0 s` tras daño no letal. |
| Micro-ensamblador (`SCRAP_02`) | El proyectil guiado viaja a `24 m/s` y dura hasta `10 s`. Cada umbral de `15` gemas consume ese contador aunque falte un objetivo válido. |
| Sierras orbitales (`SCRAP_03`) | `1 / 2 / 3` hojas (`+1` con `SCRAP_07`) giran a `180°/s` a `2,5 m` (`0,65 m` de hoja), aplicando `18 / 28 / 40` de daño y `4 m/s` de empuje por paso de simulación en solape. |
| Torreta centinela (`SCRAP_04`) | Despliega 1 torreta cada `10 s` con `12 s` de vida (máx. **2 torretas activas**). Con blanco en alcance (`12 m`), dispara cada `0,6 s` una ráfaga de **3 proyectiles** (`10` de daño) separados por `0,08 s` a `28 m/s`. |
| Aura de estática (`SCRAP_05`) | Radio `2,5 / 3,2 / 4,0 m`, intervalo `0,35 s` y empuje de `2,5 m/s`. |
| Disco rebotante (`SCRAP_06`) | Cada `3,5 s` a `16 m/s`, vida `3,5 s`, radio `0,45 m`, alcance de rebote `14 m` y enfriamiento por blanco `0,45 s`. |
| Duplicador de matriz (`SCRAP_07`) | Concede `+1` a proyectiles, sierras orbitales y saltos de cadena voltaica. |
| Escudo deflector (`SCRAP_08`) | `+10 / +18 / +26%` de bloqueo; onda física en `2,5 m` con `6 m` de empuje al bloquear. |
| Quiebre de hielo (`CRYO_04`) | Las seis esquirlas viajan a `14 m/s` y duran hasta `1,2 s`. |
| Imán criogénico (`CRYO_05`) | Radio de imán `6 / 9 / 12 m`; aura fría de `2,5 m` que ralentiza `25%` durante `1,5 s`. |
| All In (`MYTH_01`) | Peso base `1` (rareza Mítica); sustituye al azar todas las cartas poseídas distintas de sí misma por otras cartas únicas y las sube `+1` nivel hasta su `MaxTier`. |

## Oleadas

| Tiempo | Grupo | Intervalo | Residual | Interceptor | Coloso | Mortero | Polilla |
| --- | ---: | --- | ---: | ---: | ---: | ---: | ---: |
| 0:00–1:00 | 1 | 1,2 s | 100% | 0% | 0% | 0% | 0% |
| 1:00–2:00 | 2 | 0,8 s | 50% | 25% | 0% | 15% | 10% |
| 2:00–3:00 | 3 | 0,6 s | 20% | 35% | 5% | 20% | 20% |
| 3:00–5:00 | 4 | 0,4 → 0,32 s | 20% | 35 → 30% | 5 → 10% | 20% | 20% |
| 5:00–8:00 | 4 | 0,32 → 0,2 s | 20% | 30 → 25% | 10% | 20 → 25% | 20% |

Los valores con flecha se interpolan linealmente durante el tramo. Cada puesto ordinario se sortea por separado. La ronda demo usa los tres primeros tramos. Cuando hay seis Morteros activos o reservados para el grupo, las tiradas adicionales de Mortero se convierten en Residual. Desde los `90 s`, el primer puesto puede corresponder a un élite. Una aparición efectiva inicia `25 s` de espera y alterna Caballero Fantasma y Centinela de Cobre, con dos élites vivos como máximo.

El director recorta el tamaño a las plazas disponibles de su límite normal de 150 enemigos vivos. Prueba hasta 16 anclajes alrededor del jugador, a una distancia inicial de `14–16 m` que se amplía para situar al grupo fuera de cámara. Los grupos de dos forman una pareja, los de tres un triángulo y los de cuatro un cuadrado. Todos sus miembros necesitan suelo válido, espacio entre cuerpos y separación de obstáculos y del borde. Si no hay sitio se descarta el grupo en ese intervalo. Si falla la activación de un miembro, el sistema devuelve al pool los ya activados. El siguiente intervalo vuelve a intentar una aparición.

## Preparación con entidades

Antes de elegir ronda, el jugador puede ocupar hasta tres puestos de equipo en el balcón superior de la bóveda (`EQUIPO 1`, `EQUIPO 2`, `EQUIPO 3`), guiado por las flechas direccionales superiores (`VaultDirections`), el panel de consejos (`VaultTips`) y el Índice de Colección (`[I]`). Las entidades activan poderes propios y no conceden niveles de las cartas. **Sus efectos se acumulan por copia equipada (`Copies(id)`, hasta `×3`)**, de modo que equipar duplicados suma bonos o cargas independientes. Las 62 cartas se ofrecen y progresan con sus propios niveles y requisitos, independientemente de ese equipo.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Economía y recursos

## Recursos de la expedición

La XP y las cartas pertenecen al intento actual. Las gemas de los cinco enemigos de horda conceden 10, 15, 25 o 50 XP según el arquetipo; los élites entregan 65 y 80 XP. *Chatarrero* (`GREED_02`) multiplica la experiencia recogida (`+15%` por nivel). Gólgora añade 500 XP al resumen de victoria sin abrir nuevas elecciones.

El botiquín base recupera 15 HP. La probabilidad parte del `3%` en enemigos normales y del `8%` en los de recompensa fuerte (Coloso, Mortero y ambos élites). Hay seis segundos como mínimo entre apariciones, hasta cinco botiquines simultáneos en el suelo y `22 s` de duración. *Botiquín mejor* (`GREED_03`) eleva la curación a `17 / 19 / 21 HP` y las probabilidades a `4 / 5 / 6%` (normales) y `10 / 12 / 15%` (pesados y élites).

## Colección, límite de 18 unidades y liberación

La colección registra el descubrimiento permanente de las **15 entidades** (`discovered[15]`) y almacena un **máximo estricto de 18 unidades en posesión simultánea** (`FusionProgress.Capacity = 18`, `OwnedCount <= 18`). Esas 18 unidades se corresponden exactamente con los **18 pedestales físicos de la bóveda** (`displays[18]`):

- **3 puestos de equipo activo** en el balcón superior (puestos 1 a 3, `slot 0..2`, rotulados `EQUIPO 1`, `EQUIPO 2`, `EQUIPO 3`).
- **15 puestos de exposición y reserva** en la planta inferior (puestos 4 a 18, `slot 3..17`), ordenados automáticamente por rareza descendente.

El límite de 18 unidades gobierna tanto la recogida en combate como el reclamo en la máquina de fusión:

- **En la arena (`CompanionSystem.cs`, `CollectibleDisplay.cs`, `CollectiblePickupFeedback.cs`):** las copias recogidas con `E` (`TryCollectNearest` → `FusionStation.CollectFromArena`) se guardan de inmediato sin depender de derrotar a Gólgora. Si el jugador ya posee `18/18` unidades (`!FusionProgress.HasSpace`), no puede recoger más fragmentos y aparece el aviso flotante **`INVENTARIO LLENO / LIBERA ESPACIO`**.
- **En la máquina de fusión (`FusionStationUI.cs`):** cada receta consume **4 copias de cualquier rareza o combinación de rarezas** al confirmar (`30 s` de duración fija), interpolando los posibles resultados según la calidad media de los ingredientes (`qualityMean = total / 4f`). Las copias equipadas en los puestos 1 a 3 **sí están disponibles como ingredientes** (marcadas como **`EN EQUIPO 1/2/3`**) y se desequipan automáticamente al iniciar la fusión si son necesarias para cubrir las 4 unidades (*Yanikua* está excluida como ingrediente). Al terminar los `30 s`, la pantalla de resultado muestra únicamente el botón **`COLECCIONAR`** (más la `X` de cierre). Si en ese momento la colección tiene `18/18` unidades, aparece el aviso **`INVENTARIO LLENO · LIBERA ESPACIO`** (*"Tu colección tiene 18 unidades. Libera una en la bóveda y vuelve: el resultado de esta fusión seguirá aquí."*) y el resultado permanece guardado en estado `AwaitingDecision` hasta que el jugador libere hueco.
- **Liberación en la bóveda (`VaultReleaseInteraction.cs`, `CompanionView.cs`):** para hacer espacio, el jugador puede acercarse a cualquier pedestal ocupado de la planta baja (`slot >= 3`) y pulsar `E` (o botón oeste; **`E · LIBERAR`**), o bien abrir un puesto de equipo ocupado en el balcón (`E · EDITAR` en `slot 0..2`) y pulsar **`LIBERAR COPIA`**. Tras confirmar en el modal **`LIBERAR UNIDAD`**, `FusionStation.ReleaseCollectible` descuenta `1` de `owned[id]`, reordena los pedestales de exposición y conserva intacto el registro de descubrimiento (`discovered[id]`) en el **Índice de Colección (`[I]`)**.

Las probabilidades de aparición, las recetas de fusión y los poderes de cada entidad se detallan en [[04 - Objetos y Coleccionables]].

## Cómo se puntúa una expedición y Ranking en vivo (`echoscrap.web.app`)

El sistema de clasificación (`EventLeaderboardSync.cs`, `GamertagRegistry.cs` y el visor web en vivo [`https://echoscrap.web.app`](https://echoscrap.web.app)) evalúa el desempeño del jugador combinando combate, supervivencia y coleccionismo bajo la versión de reglas `rulesVersion = 2`.

![Página web del Ranking en vivo de EchoScrap (`echoscrap.web.app`)](Media/Interfaz/RankingWeb_EchoScrap.png)

### 1. Reserva de Gamertag, 3 ranuras de guardado y todos los intentos válidos

- Al elegir **`NUEVA PARTIDA`** en el menú inicial y seleccionar una de las **3 ranuras de guardado** (`ARCHIVO 1..3`), el cuadro de confirmación (`NUEVA PARTIDA · ARCHIVO N`) solicita opcionalmente un `NOMBRE DEL JUGADOR (RANKING)...` (máximo `20` caracteres visibles; si se sobrescribe una ranura existente, pide confirmar el reemplazo manteniendo intacto el archivo anterior hasta validar la nueva partida).
- Si el jugador introduce un nombre nuevo, `GamertagRegistry.Reserve` normaliza Unicode (`FormKC`), recorta y colapsa espacios múltiples (`DisplayName`), convierte a mayúsculas invariantes conservando tildes (`CanonicalName`) y reserva su hash SHA-256 en Firebase (`/gamertags/{sha256}`) con escritura condicional (`if-match: null_etag`, `timeout = 8 s`), impidiendo nombres duplicados en otra instalación (*"Ese gamertag ya está en uso. Elige otro."*). Cuando el dispositivo ya conserva el recibo local de esa reserva o el mismo gamertag sigue asociado al equipo (`SaveSlotService`), permite reutilizarlo sin bloqueo. Si se deja el campo vacío, la ranura queda en **modo práctica** sin enviar puntuaciones al ranking.
- **Todos los intentos válidos de un perfil con gamertag registrado puntúan:** al entrar a la arena (`BeginRankedExpedition`), se genera un `attemptId` de 32 caracteres hexadecimales y la partida transmite su progreso en vivo cada `2 s` a `/players/{sessionId}` (`PUT`, `timeout = 8 s`). Al concluir la expedición (`VICTORIA` o `DERROTA`), `EventLeaderboardSync` encola el resultado definitivo en `Saves/ranking_outbox.json` y actualiza en la ranura el contador `completedRankedRuns` y su mejor marca local (`bestLocalScore`). En el servidor y visor web ([`https://echoscrap.web.app`](https://echoscrap.web.app)) se conserva la **mejor puntuación por gamertag**.
- **Abandono o suspensión (`CancelAttempt` / `ExpeditionSnapshotService`):** si el jugador regresa voluntariamente a la bóveda o reinicia desde la pausa, el intento activo se marca `CANCELADA` (`DELETE` en `/players/{sessionId}`) sin puntuar. En cambio, si cierra o suspende la aplicación mientras la ranura conserva una expedición activa (`ExpeditionSnapshotService`), el intento queda preservado para reanudarse exactamente donde se detuvo sin enviarse como cancelado al destruir la sesión (`EventLeaderboardSync.OnDestroy`).

### 2. Puntos base de la expedición (`RawPoints`)

Durante la expedición puntuable se suman cuatro fuentes continuas y una bonificación única por la mayor rareza obtenida en esa incursión:

| Concepto en la expedición puntuable | Puntos base |
| --- | ---: |
| Baja de un enemigo (`kills`) | **`100`** |
| Unidad recogida en la arena (`E`) o resultado de fusión conservado (`COLECCIONAR`) (`units`) | **`300`** |
| Primer descubrimiento de esa entidad en la colección (`discoveries`) | **`150`** |
| Segundo completo de combate activo (`seconds`, `CombatSession.GameplayTime`) | **`2`** |
| **Mayor rareza obtenida en esa expedición (bonificación única):** | |
| · Común | **`250`** |
| · Rara *(Poco común)* | **`500`** |
| · Especial *(Rara)* | **`750`** |
| · Épica | **`1 000`** |
| · Legendaria | **`1 250`** |
| · Mítica | **`1 500`** |

$$\text{Puntos base} = 100 \times \text{bajas} + 300 \times \text{unidades} + 150 \times \text{descubrimientos} + 250 \times (\text{rareza máx.} + 1) + 2 \times \text{segundos}$$

*(Si no se obtiene ninguna unidad durante la expedición, la bonificación de rareza máxima es `0`. El reloj de combate activo se detiene en pausa y mientras el jugador está en la bóveda).*

### 3. Comparación justa entre Ronda Demo (`3 min`) y Ronda Completa (`8 min`)

Para que ambos modos compitan en la misma tabla de clasificación (`CalculateScore`):

- **Ronda demo de `3` minutos (`mode = "3 MIN"`):** conserva el `100 %` de los puntos base (`score = rawScore`).
- **Ronda completa de `8` minutos (`mode = "8 MIN"`):** divide el total de puntos base entre **`2`** y redondea hacia abajo (`score = rawScore / 2`).
- **Ejemplo:** una expedición con `20` bajas (`2 000`), `2` unidades obtenidas (`600`), `1` descubrimiento nuevo (`150`), rareza máxima Rara (`500`) y `90 s` de combate activo (`180`) suma **`3 430` puntos base**: registra **`3 430` pts** en la ronda de `3` minutos y **`1 715` pts** en la ronda de `8` minutos.

### 4. Qué aporta una fusión al ranking

Si el jugador entra a la bóveda durante su expedición puntuable y reclama el resultado de una fusión con **`COLECCIONAR`** (`FusionStation.Resolve(true)` → `EventLeaderboardSync.NotifyCollectibleObtained`), ese resultado cuenta como **una unidad obtenida (`+300` puntos base)**, añade **`+150` puntos base** si es la primera vez que se descubre esa entidad en la colección y puede elevar la **bonificación única de rareza máxima** de la expedición. En cambio, las `4` copias consumidas como ingredientes, el hecho de iniciar la máquina o liberar unidades en los pedestales aportan **`0` puntos**, y las fusiones realizadas antes de iniciar o después de cerrar la expedición puntuable tampoco modifican el marcador.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Reglas y estados del juego

| Momento | Regla |
| --- | --- |
| Menú inicial | `CONTINUAR` (selecciona entre las **3 ranuras de guardado** para volver a la bóveda o reanudar automáticamente una expedición suspendida mediante snapshot), `NUEVA PARTIDA` (elige una ranura vacía o sobrescribe una ocupada tras confirmación, con campo opcional de gamertag validado mediante `GamertagRegistry`), `CONFIGURACIÓN` (`MenuSettingsView` en 3 pestañas: `General`, `Interfaz`, `Sonido`) y `SALIR`. El mundo se carga después de seleccionar la ranura. |
| Preparación en bóveda | Movimiento, guía activa (`EQUIPO 1..3`, `VaultDirections` y `VaultTips`), consulta del Índice de Colección / Bestiario (`[I]` / `Select`), gestión de las **18 unidades** en `DisplaySlots[18]` (`3` puestos de equipo en balcón + `15` de exposición en planta baja, poderes acumulables hasta `×3`) y máquina de fusión situada en la pared trasera (`x = 189, z = 28,88`, `30 s`, misma rareza o rarezas mixtas). El reloj de expedición no avanza. |
| Selección y liberación de entidades | En los puestos 1 a 3 (balcón), `E` / botón oeste (`E · EQUIPAR` / `E · EDITAR`) abre y pausa el selector (`QUITAR DEL PUESTO`, `LIBERAR COPIA`, `CERRAR · E / ESC`), y `T` / botón norte en el mundo desequipa directamente. En los pedestales de planta baja (`slot >= 3`), `E · LIBERAR` / botón oeste abre el modal `LIBERAR UNIDAD`. |
| Índice de Colección (Bestiario) | `I`, `Select` o clic en `ÍNDICE · [I]` pausa la simulación (`PauseReason.FusionMenu`) y muestra las pestañas `POR RAREZAS` (`TODAS` + 6 rarezas; siluetas negras `???` hasta recoger o reclamar por primera vez) y `EN TU BÓVEDA` (hasta 18 unidades). |
| Selección de ronda | En **`ELIGE LA RONDA`** (*"Elige una ronda para comenzar."*) se elige `RONDA DEMO` (`3:00`) o `RONDA COMPLETA` (`8:00`), o `SALIR`. Si el perfil activo tiene gamertag registrado, entrar inicia el envío en vivo (`EN PARTIDA`) a `echoscrap.web.app`. |
| Combate | Avanzan reloj, oleadas, ataques, recogida (bloqueada en `18/18` para entidades), guardado automático periódico de snapshot cada `3 s` (`ExpeditionSnapshotService`) y temporizadores activos. Mantener `Q` o `Select` despliega estadísticas completas y poderes de acompañantes en el HUD. El selector de nivel ofrece hasta **62 mejoras** en **5 rarezas**. |
| Cartas o pausa | El combate se detiene; el menú de pausa (`Esc` / `Start`) ofrece las pestañas `General` (con **`Límite de FPS` `30 / 60 / 120`**), `Interfaz` y `Sonido`, además de `CONTINUAR` y salida a menú o bóveda (volver a la bóveda o salir cancela el intento activo del ranking; si solo se cierra o suspende la aplicación durante una expedición con ranura activa, el snapshot conserva la partida para reanudarla). |
| Visita a la bóveda | Se conserva la expedición en memoria y se suspenden enemigos y peligros hasta volver por el portal; reclamar una fusión con `COLECCIONAR` durante la expedición puntuable suma como unidad obtenida (`+300` pts base). |
| Entrada de Gólgora | Se retira la horda, se limpian amenazas y se traslada al jugador a la caldera con protección. |
| Transición del jefe | Al llegar al `50%` de vida, Gólgora entra en su segunda fase con una transición invulnerable (`1,5 s` unificado en `GolgoraCombatSettings.cs` y `CombatSettings.asset`) y un rugido que empuja en `12 m`. |
| Victoria o derrota | Se muestra el resumen de resultados, se limpia la expedición activa de la ranura, todo intento válido finalizado (`VICTORIA` o `DERROTA`) de un perfil con gamertag publica su puntuación en Firebase (`EventLeaderboardSync`, manteniendo la mejor marca por gamertag en [`https://echoscrap.web.app`](https://echoscrap.web.app) y en `bestLocalScore`) y se vuelve a la bóveda. |

## Encuentro con Gólgora

En los arquetipos del jefe, **`Golgora_Demo.asset`** (y el valor por defecto en `EnemyArchetypeSettings.cs`) fija **3 600 HP** a los `180 s` (`3:00`) para la ronda demo, mientras **`Golgora_Complete.asset`** fija **9 000 HP** a los `480 s` (`8:00`) para la ronda completa (ambos con transición a Fase 2 al `50%`: `1 800 HP` / `4 500 HP`). Alcanzar ese tiempo inicia el encuentro; sobrevivir hasta el minuto de aparición no concede la victoria por sí mismo.

La caldera central tiene `22 m` de radio y cierra su perímetro. La entrada reposiciona al jugador sin daño y la cámara encuadra a ambos combatientes. Durante la presentación inicial el jefe no es un objetivo válido.

Los refuerzos se solicitan cada cuatro segundos, en grupos de hasta cuatro y con el orden Residual, Residual, Mortero y Polilla. En la caldera el límite efectivo es de ocho secuaces simultáneos, incluyendo las apariciones pendientes. Cada entrada se anuncia durante `0,9 s`. El *Cerco de magma* y la transición de fase suspenden los refuerzos y retiran los que quedaban activos.

Los ataques (*Bombardeo*, *Cerco de magma* con `40` de daño físico de aterrizaje y `18 s` fijos de enfriamiento, y *Fisura de magma* con `1,1 s` fijos de preparación), inmunidades y tiempos de las dos fases se detallan en [[03 - Enemigos y NPC]].

## Cierre y nueva preparación

La muerte del jugador tiene prioridad si coincide con la del jefe en el mismo paso de simulación. La victoria incluye la presentación final, absorción de gema y `500 XP` para el resumen, sin abrir nuevas elecciones de cartas, y dispara la sincronización de estadísticas de sesión mediante `EventLeaderboardSync`.

Volver a la bóveda limpia actores, peligros y efectos, restablece la configuración de combate y vuelve a aplicar los poderes del equipo guardado. La colección (`hasta 18 unidades` en posesión y `15` descubiertas en el índice) y la fusión en curso o lista para reclamar siguen conservadas en la ranura activa.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Premisa y trama

El Coleccionista era un humano obsesionado con acumular rarezas. Una entidad le ofreció acceso a un mundo donde podría construir la colección definitiva. La oferta lo llevó al Vertedero de Ecos: un nexo al que llegan restos de realidades destruidas y del que no encuentra una salida.

Al cruzar pierde sus recuerdos, pero conserva el impulso de recolectar. Entre metal, ruinas y criaturas residuales, su linterna se convierte en la herramienta con la que abre espacio para seguir moviéndose. La bóveda es su refugio y el lugar donde custodia, equipa y fusiona lo que reúne.

Gólgora, el MataColeccionista, representa al guardián de ese lugar. El enfrentamiento con él da forma al cierre de cada expedición. La derrota detiene el intento; la victoria resuelve el combate, pero el mundo continúa como escenario de nuevas incursiones.

El pacto, la pérdida de recuerdos y la entidad que lo engaña son el trasfondo de diseño. La premisa se transmite en partida mediante el personaje, el entorno, la colección de quince entidades y el ciclo de supervivencia. El recorrido jugable de la versión actual está descrito en [[01 - Jugabilidad]], y los mensajes visibles durante el juego se recogen en [[04 - Diálogos y Narrativa]].

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Mundo

El Vertedero de Ecos es un espacio atemporal donde se mezclan restos de épocas y realidades distintas. La chatarra, las ruinas y la energía residual dan continuidad a esa mezcla. La bóveda funciona como refugio y centro de preparación entre expediciones.

| Lugar | Presencia actual | Función |
| --- | --- | --- |
| Vertedero | Arena de 200 × 200 m con relieve, obstáculos y restos industriales. | Supervivencia frente a la horda, obtención de XP y recogida manual de entidades. |
| Caldera central | Área circular de 22 m de radio con cierre perimetral durante el encuentro. | Combate contra Gólgora y sus refuerzos. |
| Acceso norte | Descenso, muros, rellano y portal (`x = 0, z = 78`). | Conexión física con la bóveda. |
| Bóveda | Sala modular en dos niveles con **18 pedestales físicos** (`DisplaySlots[18]`, capacidad máxima de 18 unidades): **3 puestos de equipo activo** en el balcón superior (`EQUIPO 1..3`, que también exhiben visualmente reservas como `RESERVA` si están vacíos) y **15 puestos de exposición/reserva** en la planta baja (`slot >= 3`, con liberación directa `E · LIBERAR`), guiada por `VaultDirections`, `VaultTips` y el **Índice de Colección (`[I]`)**. | Equipar poderes acumulables, consultar el índice de las 15 entidades y liberar copias para gestionar el espacio. |
| Máquina de fusión | Cámara ubicada en la pared trasera de la bóveda (`x = 189, z = 28,88`) con persiana (`30 s` fijos de proceso). | Consumir cuatro copias de la misma rareza o de rarezas mixtas (excluida Yanikua) y reclamar una nueva entidad con `COLECCIONAR`. |

Las gemas de XP impulsan el crecimiento durante una incursión. Las entidades pasan a una colección persistente por ranura de hasta **18 unidades en posesión** y un registro de **15 entidades descubiertas**, cuyas reglas se detallan en [[04 - Objetos y Coleccionables]].

Las tres zonas de lava fija del mapa están desactivadas en el montaje actual; el magma se reserva para Gólgora y sus patrones de ataque. La narrativa de origen se conserva en [[01 - Premisa y Trama]].

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Personajes

El reparto incluye al Coleccionista, cinco enemigos de horda, dos élites, Gólgora y las quince entidades de la colección. Cada enemigo tiene aquí su ficha de identidad y función; sus recursos visuales se reúnen en [[02 - Personajes y Animación]] y sus reglas de combate en [[03 - Enemigos y NPC]].

[Ver la lámina de proporciones del reparto](05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#proporciones-en-reposo).

## El Coleccionista

Es el personaje controlado por el jugador. Lleva capucha clara, máscara con pico, ropa oscura y farol. Su luz apunta de forma independiente del movimiento. Cuenta con sprites de reposo, marcha, dash y muerte en cuatro direcciones.

[Ver atlas, fuente editable y animaciones del Coleccionista](05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#el-coleccionista).

## Residual: Esqueleto

Enemigo básico de la horda. Su apariencia es un esqueleto animado en cuatro direcciones que persigue al Coleccionista y ejerce presión por acumulación y contacto.

[Ver ficha visual y animaciones](05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#residual-esqueleto).

## Interceptor: galgo de chatarra

Perseguidor rápido representado por un galgo de chatarra. Anticipa el movimiento del Coleccionista y obliga a variar las rutas de escape.

[Ver ficha visual y animaciones](05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#interceptor-galgo-de-chatarra).

## Coloso: Wendigo

Enemigo pesado representado por un Wendigo. Es inmune al empuje, compromete una dirección de embestida tras un aviso en el suelo, atraviesa el espacio libre y requiere un intervalo de recuperación.

[Ver ficha visual y animaciones](05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#coloso-wendigo).

## Mortero: Campana

Enemigo de artillería con forma de campana flotante. Mantiene distancia y lanza proyectiles de ácido que dejan charcos persistentes en el suelo.

[Ver ficha visual y animaciones](05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#mortero-campana).

## Polilla: Reactor

Enemigo explosivo volador que se aproxima hasta situarse a `2 m` del jugador (`MothArmDistance`) para armar su espoleta de `0,9 s` (o `1,2 s` si muere por daño del jugador antes de detonar). Su explosión en `3 m` inflige `35` de daño físico al Coleccionista y a todos los objetivos enemigos cercanos (`IsEnemyTarget`, incluido Gólgora, que aplica su `35%` de reducción mientras está en `CercoActive`).

[Ver ficha visual y animaciones](05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#polilla-reactor).

## Caballero Fantasma

Élite de ataque sobrenatural y resistencia al daño físico (`75%` de mitigación). Amenaza al Coleccionista con un barrido frontal anunciado de `120°` y presenta resistencia parcial al empuje (`susceptibilidad = 0,35`).

[Ver estado de integración visual](05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#caballero-fantasma).

## Centinela de Cobre: gólem

Élite pesado de proyectiles físicos y resistencia al daño sobrenatural (`75%` de mitigación fuera de su ventana de vulnerabilidad). Abre sus compuertas para disparar un abanico de tres misiles y queda expuesto con el aviso «Vulnerable» durante su recuperación de `2 s`.

[Ver estado de integración visual](05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#centinela-de-cobre-g%C3%B3lem).

## Gólgora: el MataColeccionista

Jefe final de la expedición. Su modelo 3D animado combina cuerpo pesado, arma contundente y magma. Combate dentro de la caldera central de `22 m` de radio, alterna seis patrones de ataque y acelera sus tiempos y velocidad al entrar en su segunda fase al `50%` de vida.

[Ver ficha visual y animaciones](05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#g%C3%B3lgora-matacoleccionista).

## Entidades de la colección

El catálogo reúne quince entidades provenientes de distintos mundos y realidades que colapsaron hacia el Vertedero de Ecos: Lobito, Mago, Demonio, Shireen, Yanikua, Basket, Hada, Lisa, Reaper, Chico Emo, Conde Alucard, Murciélago Cristal, Sirena, Soldier y Joel (Demonio y Demonio2 son vistas de una misma entrada). Nueve de ellas se encuentran directamente en la arena y seis se obtienen en la máquina de fusión.

Cada entidad activa un poder propio de acompañante al ocupar uno de los tres puestos de equipo del balcón superior (`EQUIPO 1..3`) y queda registrada al descubrirse en el **Índice de Colección (`[I]`)**. Sus rarezas, poderes y recetas de fusión se detallan en [[04 - Objetos y Coleccionables]], y sus vistas y archivos editables en [Personajes y animación](05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#entidades-de-la-colecci%C3%B3n).

### Yanikua

Supervisora de mantenimiento en una planta de agua potable de su mundo de origen, reconocible por su uniforme operativo con cintas reflectantes, botas de seguridad y las rodillas manchadas de lodo por el trabajo de campo. Durante una guardia nocturna, una fractura dimensional comenzó a tragarse las instalaciones; en lugar de huir, permaneció en la sala de válvulas cerrando manualmente las compuertas principales para dar tiempo a que su cuadrilla evacuara, convirtiéndose en la última en caer al Vertedero. Su temple bajo presión y su resistencia física refuerzan al Coleccionista e incluso le permiten sobrevivir a un golpe letal por expedición.

### Lisa

Joven viajera proveniente de una ciudad invernal que quedó congelada en el tiempo antes de fracturarse. Viste un abrigo blanco de corte ligero y boina rosa sobre su cabellera celeste, manteniendo una expresión serena incluso en medio del caos de la fosa. Acostumbrada al frío extremo de su hogar, conserva un aura gélida capaz de congelar periódicamente a los enemigos cercanos y mantener un ritmo constante de ataque.

### Sirena

Habitante de un océano abisal cuyos mares se evaporaron cuando su mundo se resquebrajó. Su cola escamosa oscura con destellos dorados y las joyas que adornan su cabello reflejan su origen en las profundidades, donde utilizaba pulsos de presión y cantos resonantes para orientarse en la penumbra. En el Vertedero emplea esa misma onda expansiva para apartar a la horda y restaurar parte de la vitalidad del jugador.

### Hada

Pequeña artesana alada de tonos azules que lleva un delantal de trabajo, un carrete de hilo con alfileres sujeto al cabello y unas tijeras doradas de costura en la mano. En su reino original se encargaba de remendar tapices encantados; tras caer a la fosa, utiliza sus hilos invisibles para tejer una barrera protectora alrededor del Coleccionista que absorbe impactos directos antes de volver a hilvanarse.

### Murciélago Cristal

Criatura nocturna nacida en cavernas subterráneas de cuarzo, cuyas alas translúcidas, orejas minerales y gema caudal refractan cualquier destello de luz. Cuando las bóvedas de su mundo se desplomaron, quedó atrapado entre la chatarra del Vertedero, donde emite vibraciones acústicas a través de sus cristales para dañar y entorpecer el avance de las criaturas cercanas.

### Shireen

Danzante y curandera de un oasis desértico, ataviada con sedas color magenta y una diadema dorada con rubí en la frente. Cuando su santuario fue absorbido por la anomalía, conservó consigo las plegarias de su pueblo para aliviar el agotamiento de los viajeros perdidos, canalizando una recuperación constante de salud durante el combate.

### Soldier

Motociclista y combatiente urbano sobreviviente de una metrópoli arrasada por una infección zombi. Porta un casco rojo con visor oscuro, rifle a la espalda, maletín de primeros auxilios en mano y una chamarra mostaza remendada y marcada en el pecho por zarpazos que no lograron derribarlo cuando defendía el último puente de evacuación de su ciudad. Curtido en resistir emboscadas cuerpo a cuerpo, aporta mayor vitalidad y reduce el daño recibido.

### Conde Alucard

Un peculiar capibara vampiro de porte aristocrático que habitaba los humedales nocturnos de un mundo gótico. Ataviado con sombrero de copa, medallón dorado y una capa negra de cuello carmesí, solía pasear con calma junto a las lagunas antes de que su realidad desapareciera. Fiel a su naturaleza vampírica pero pacífica con su portador, drena pequeñas dosis de vitalidad de los enemigos derrotados para curar al Coleccionista.

### Mago

Hechicero veterano de barba rojiza, túnica azul y sombrero puntiagudo que cayó al Vertedero aferrado a su báculo de madera coronado por un cristal arcano. Aunque la destrucción de su torre le arrebató sus libros de conjuros complejos, todavía concentra esferas de energía pura en su mano libre para disparar proyectiles mágicos que atraviesan a las filas de la horda.

### Demonio

Pequeño diablillo carmesí de mirada traviesa cuyas orejas y punta de la cola arden con fuego vivo. Proveniente de un plano volcánico menor colapsado, disfruta del caos de la arena más que cualquier otro habitante del almacén, imbuyendo los ataques del Coleccionista con su temperamento agresivo para aumentar el daño e incendiar a los enemigos.

### Joel

Joven programador y estudiante de ingeniería que fue tragado por una grieta dimensional en plena madrugada mientras depuraba código. Llegó al Vertedero tal como estaba: con su laptop llena de calcomanías bajo un brazo y un vaso de café cargado en la otra mano. Su ritmo acelerado por la cafeína y su costumbre de trabajar contra reloj incrementan la velocidad de ataque del farol.

### Chico Emo

Joven reservado de lentes, chamarra oscura y playera con estampado de calavera que solía pasar las tardes aislado entre música y callejones de su ciudad antes del colapso. Acostumbrado a escabullirse sin llamar la atención y reaccionar por reflejo ante el peligro, le permite al Coleccionista duplicar su velocidad de carrera justo después de ejecutar un *dash*.

### Lobito

Lobo antropomórfico y disciplinado practicante de artes marciales que viste un *karategi* blanco con cinturón negro, guantes de combate y una bufanda roja al cuello. Entrenaba en un dojo de montaña cuando su mundo se fragmentó; pese a estar lejos de su hogar, mantiene su guardia firme y comparte su acondicionamiento físico para incrementar la salud máxima del jugador.

### Basket

Jugador de baloncesto callejero que porta el uniforme azul número 77, su balón bajo el brazo y una llamativa máscara de lucha libre para ocultar su identidad en las canchas de su barrio. La falla dimensional lo sorprendió en pleno partido, por lo que ahora utiliza su juego de pies y cambios de ritmo para otorgar mayor velocidad de movimiento entre los obstáculos de la arena.

### Reaper

Pequeño espectro encapuchado de cráneo pálido, ojos rojos encendidos y costillas expuestas que carga una guadaña casi tan grande como él. Era un guía menor de almas en un plano espiritual extinguido; al quedar sin difuntos que escoltar, acompaña al Coleccionista desde la Bóveda afilando la contundencia de cada ataque.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Diálogos y narrativa

El juego comunica su situación mediante nombres, entorno, presentaciones diegéticas y mensajes directos en la interfaz, todos ellos compuestos con la tipografía **`Pixelify Sans`**.

## Portada, ranking y selección de ronda

En la portada integrada al elegir **`NUEVA PARTIDA`** y una de las **3 ranuras de guardado** (`NUEVA PARTIDA · ARCHIVO 1..3`), el jugador dispone del campo **`NOMBRE DEL JUGADOR (RANKING)...`** (máximo 20 caracteres visibles) para registrar un alias en el ranking del evento (`GamertagRegistry` / `EventLeaderboardSync`). Al acercarse al portal de salida en la bóveda, la ventana **`ELIGE LA RONDA`** (*"Elige una ronda para comenzar."*) permite escoger entre **`RONDA DEMO`** (`3:00`), **`RONDA COMPLETA`** (`8:00`) o **`SALIR`**. Su confirmación reproduce una transición con rugido antes del despliegue en la arena.

## Señalización, índice y mensajes en la bóveda

Dentro de la bóveda, la interfaz y el escenario guían cada paso de la preparación:

- **Rótulos 3D en el mundo:** **`EQUIPO 1`**, **`EQUIPO 2`** y **`EQUIPO 3`** sobre los tres pedestales del balcón superior (añadiendo `· RESERVA` cuando muestran una copia en reserva mientras están vacíos).
- **Indicadores direccionales superiores (`VaultDirections`):** dos tarjetas en el borde superior con flechas dinámicas (`▲`, `▼`, `◄`, `►`, `●`) que orientan hacia la **Máquina de Fusión** (*"MÁQUINA DE FUSIÓN · Combina 4 copias de varias rarezas"*, *"BAJA A FUSIÓN"* o *"E · MÁQUINA DE FUSIÓN"*) y hacia el **Balcón de Equipo** (*"SUBE AL BALCÓN · Puestos de equipo 1, 2 y 3"*, *"EQUIPAR ENTIDADES"* o *"PUESTO 1..3 · EQUIPAR/EDITAR"*).
- **Panel de consejos (`VaultTips`):** tarjeta inferior izquierda con la insignia **`CONSEJO`** que rota cada `8 s` entre tres mensajes:
  1. *"Equipa coleccionables antes de ir a la arena."*
  2. *"Abre el índice para consultar tus descubrimientos."*
  3. *"Combina cuatro coleccionables en la máquina de fusión."*
  Cuando el jugador posee coleccionables pero aún no ha equipado ninguno (`OwnedCount > 0 && !hasEquippedCompanion`), prioriza el mensaje: *"Pulsa E frente a un puesto para equipar tu primer coleccionable."*
- **Interacciones de pedestal y liberación:** en el balcón (`slot 0..2`) se muestra **`E · EQUIPAR`** (si está vacío, aunque exhiba una copia de reserva) o **`E · EDITAR`** (si está ocupado, abriendo `PUESTO DE EQUIPO 1/2/3 · ELIGE UNA ENTIDAD DESCUBIERTA PARA ESTE PUESTO` con la nota *"Señala un compañero para ver su poder. Los poderes se acumulan por copia equipada."* y los botones **`QUITAR DEL PUESTO`**, **`LIBERAR COPIA`** y **`CERRAR · E / ESC`**). En los puestos ocupados de la planta baja (`slot >= 3`) aparece **`E · LIBERAR`**, que abre el modal **`LIBERAR UNIDAD`**: *"¿Seguro que quieres liberar a `<Nombre>`? Esta unidad desaparecerá de tu colección."* (`CANCELAR` / `LIBERAR`).
- **Índice de Colección (`[I]`):** accesible desde el botón **`ÍNDICE · [I]`**, presenta el título **`ÍNDICE DE COLECCIÓN`**, el contador **`{descubiertos} / 15 DESCUBIERTOS · {en posesión} / 18 UNIDADES`** y las pestañas **`POR RAREZAS`** (donde las entidades aún no registradas aparecen como **`???`**, **`NO ENCONTRADO`** y **`SILUETA BLOQUEADA`**) y **`EN TU BÓVEDA`** (`EN EQUIPO ACTIVO` / `EN BÓVEDA`).
- **Máquina de fusión:** muestra **`E · FUSIONAR`** o **`E · VER FUSIÓN`**, etiqueta las copias equipadas como **`EN EQUIPO 1/2/3`**, presenta el resultado con el botón **`COLECCIONAR`** y, si la colección está llena (`18/18`), advierte **`INVENTARIO LLENO · LIBERA ESPACIO`** (*"Tu colección tiene 18 unidades. Libera una en la bóveda y vuelve: el resultado de esta fusión seguirá aquí."*).

## Mensajes en la arena y encuentro final

La entrada a la arena presenta al Coleccionista y el objetivo de la incursión. Las oleadas de fragmentos anuncian **«¡NUEVOS COLECCIONABLES!»** y, si el jugador intenta recoger uno teniendo `18/18` unidades en la colección, aparece el texto flotante **`INVENTARIO LLENO / LIBERA ESPACIO`**.

Gólgora se anuncia con título y rugido al abrirse la caldera. Su barra de salud, los avisos de suelo y los textos de estado (*«Resistido»*, *«Vulnerable»*, *«EJECUTADO»*) acompañan los cambios del combate. **«VICTORIA»** o **«DERROTA»** cierran la incursión y **«VOLVER A LA BÓVEDA»** conduce a la siguiente preparación.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Estructura del contenido

| Contenido | Cantidad y uso actual |
| --- | --- |
| Protagonista | El Coleccionista. |
| Escenario de expedición | Vertedero de 200 × 200 m con relieve, obstáculos y acceso norte (`x = 0, z = 78`). |
| Caldera | Encuentro cerrado de 22 m de radio para Gólgora. |
| Bóveda | Dieciocho pedestales físicos (capacidad máxima de **18 unidades**): **3 puestos de equipo** en el balcón superior (`EQUIPO 1..3`), **15 puestos de exposición** en la planta baja, máquina de fusión en la pared trasera (`x = 189, z = 28,88`), portal e indicadores `VaultDirections` y `VaultTips`. |
| Índice de Colección | Panel accesible con **`I`** (`VaultCollectionIndex`) con registro de las **15 entidades** (`POR RAREZAS`) y de las **hasta 18 unidades** en posesión (`EN TU BÓVEDA`). |
| Horda | Residual, Interceptor, Coloso, Mortero y Polilla Reactor. |
| Élites | Caballero Fantasma y Centinela de Cobre. |
| Jefe | Gólgora / MataColeccionista, con seis ataques y dos fases. |
| Mejoras | 62 cartas distribuidas en cinco rarezas (`Común`, `Rara`, `Épica`, `Legendaria` y `Mítica`), con niveles, pesos aleatorios y requisitos de desbloqueo. |
| Objetos de combate | Gemas de experiencia y botiquines. |
| Entidades | Quince en seis rarezas: nueve se obtienen en la arena y seis como resultados de fusión (`30 s` fijos por fusión). |
| Equipo | Hasta tres entidades equipadas en el balcón con poderes propios de `CompanionPowerRuntime`; puede quedar incompleto. |
| Rondas | Demo (`3:00`) y completa (`8:00`), seleccionables desde la bóveda sobre cualquiera de las **3 ranuras de guardado** (`SaveSlotService`) con sincronización de ranking (`EventLeaderboardSync`). |
| Pruebas | Laboratorio de ocho arquetipos (`EnemyLaboratory`) y sandboxes de locomoción y combate. |

La compilación principal habilita `MainMenu` y `CombatArena`. Las escenas `CombatArena_3Min` y `CombatArena_8Min` permanecen en el proyecto como variantes de desarrollo y están deshabilitadas en la lista principal de compilación. La selección de ronda del jugador utiliza los assets de oleadas dentro de `CombatArena`.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Niveles

## El Vertedero

La arena mide `200 × 200 m`. El suelo tiene relieve y los grupos de chatarra, rocas, árboles y restos industriales forman rutas y obstáculos. Los límites físicos contienen al jugador.

El centro alberga una caldera de `22 m` de radio. Al aparecer Gólgora se cierra el perímetro, se traslada al jugador con protección y la cámara adopta un encuadre que contiene a ambos combatientes. Los refuerzos se anuncian junto al borde interior.

Las tres zonas de lava fija están desactivadas en `CombatArena`. El magma de los ataques del jefe continúa activo. La iluminación exterior utiliza tonos violetas, niebla, haces y partículas ambientales; el perfil aplica una cámara a `35,5°` con campo de visión de `34°`.

### Plano a escala y arte conceptual

![Boceto conceptual a mano del mapa de El Vertedero|520](Media/ConceptArt/Mapa_Arte.JPG)

![Plano de la arena y la conexión con la bóveda|381](Media/Planos/Vertedero.svg)

La planta usa coordenadas X/Z de Unity y una cuadrícula de `20 m`. La expedición y Gólgora empiezan en el centro de la arena, en momentos distintos. Al comenzar el encuentro, el jugador se coloca `12 m` al sur del centro. La línea discontinua del acceso norte representa el traslado por portal a la sala de la bóveda, fuera de los límites de la arena.

La horda busca una posición dinámica fuera de cámara alrededor del jugador y dentro del perímetro. El anillo central indica el recinto de refuerzos y combate del jefe. Las reglas de aparición se detallan en [[05 - Progresión]].

## Acceso norte y bóveda

El acceso al almacén está al norte de la arena, alrededor de `x = 0` y `z = 78`. La escalera baja hasta el portal. La bóveda ocupa otra zona de la misma escena; el portal traslada al mismo jugador sin descargar el mundo.

La sala modular se organiza en dos niveles conectados por rampas y contiene exactamente **dieciocho pedestales físicos** (`displays[18]`, equivalentes al límite máximo de **18 unidades** de la colección):

- **Balcón superior (puestos 1 a 3):** alberga los **3 puestos de equipo activo**, señalizados en el mundo con los rótulos 3D **`EQUIPO 1`**, **`EQUIPO 2`** y **`EQUIPO 3`** (`E · EQUIPAR` / `E · EDITAR` y `T` para desequipar rápido).
- **Planta inferior (puestos 4 a 18 y pared trasera):** alberga los **15 puestos de exposición y reserva** (`E · LIBERAR` para liberar copias con confirmación) y la **máquina de fusión** ubicada en la pared trasera (`(189, 0, 28,88)`, `E · FUSIONAR` / `E · VER FUSIÓN`, `30 s` fijos de duración), junto con texturizado de basalto pixel art, fracturas de roca con relieve, bloqueo bajo escalera/balcón e iluminación interior acotada (`WarehouseLightScope`).

El HUD de la bóveda acompaña el recorrido con dos tarjetas direccionales superiores (`VaultDirections`), el botón superior izquierdo **`ÍNDICE · [I]`** para abrir el Índice de Colección y el panel inferior izquierdo de consejos rotatorios (`VaultTips`). Su cámara utiliza una inclinación de `32°` y campo de visión de `65°`, con límites de seguimiento para mantener la sala encuadrada.

![Boceto conceptual a mano de la Bóveda|420](Media/ConceptArt/Almacen_Arte.jpeg)

![Bóveda actual](Media/Interfaz/Boveda_almacen.png)
*Captura de referencia de la Bóveda (`734e2a5`) mostrando `VaultDirections`, `ÍNDICE · [I]`, `EQUIPO 1..3` y `VaultTips`; en la versión `0.7.0`, la bóveda aloja `18` pedestales físicos (`DisplaySlots[18]`) y la máquina de fusión se sitúa en la pared trasera.*

Al preparar una expedición, el portal abre la selección de ronda (`ELIGE LA RONDA`). Durante una visita desde el combate se conserva la sesión y se suspenden sus enemigos y peligros hasta volver.

## Escenas y rondas

| Elemento | Uso | Entrada del jefe y salud |
| --- | --- | --- |
| `MainMenu` | Menú inicial independiente del mundo de combate. | No aplica |
| `CombatArena` · Ronda demo | Mundo principal con escalada rápida seleccionada en la bóveda. | `3:00` (`180 s`); `3 600 HP` (asset demo) |
| `CombatArena` · Ronda completa | Mismo mundo con escalada completa de ocho minutos (y exclusión de apariciones dentro de edificios cerrados mediante `OverlapsBlockedInterior`). | `8:00` (`480 s`); `9 000 HP` (`Golgora_Complete.asset`) |
| `CombatArena_3Min` / `CombatArena_8Min` | Variantes guardadas para desarrollo; fuera de la lista principal de compilación. | Según su asset |
| `EnemyLaboratory` | Generación e inspección manual de los ocho arquetipos. | Manual |
| `LocomotionSandbox` / `CombatSandbox` | Comprobaciones aisladas de locomoción y combate. | Sin flujo completo |

El tiempo indica cuándo empieza la entrada del jefe. Las cartas, pausas y visitas a la bóveda detienen el reloj de combate.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Enemigos y NPC

El combate reúne cinco arquetipos de horda, dos élites y Gólgora. Las fichas visuales, spritesheets, fuentes editables y GIFs por acción se documentan en [[02 - Personajes y Animación]]. Caballero Fantasma y Centinela de Cobre (gólem) usan sus sprites animados integrados, con mapas de normales, emisión y secuencias sincronizadas con los estados de combate.

La [[02 - Personajes y Animación#Proporciones en reposo|comparación a escala]] muestra a los ocho enemigos junto al Coleccionista.

| Enemigo | Vida | Velocidad base | Daño principal | Susceptibilidad al empuje | Frenado (`EnemyArchetypeSettings` / `.asset`) | XP |
| --- | ---: | ---: | --- | ---: | ---: | ---: |
| Residual / Esqueleto | 15 HP | 3,5 m/s ±15% | Contacto: 10 (físico); recarga 0,5 s. | `1,0` | `28 m/s²` | 10 |
| Interceptor | 9 HP | 6,2 m/s ±10% | Contacto: 8 (físico); recarga 0,5 s. | `1,0` | `18 m/s²` | 10 |
| Coloso / Wendigo | 80 HP | 1,8 m/s | Contacto o carga: 25 (físico). | `0` (inmune) | `30 m/s²` (sin impulso) | 50 |
| Mortero Campana | 25 HP | 2,2 m/s ±10% | Contacto: 5; ácido: 5 cada 0,5 s. | `0,7` | `24 m/s²` | 25 |
| Polilla Reactor | 12 HP | 4,8 m/s ±10% | Explosión en 3 m: 35 (físico) al jugador y objetivos enemigos. | `1,0` | `22 m/s²` | 15 |
| Caballero Fantasma | 110 HP | 2,8 m/s | Barrido sobrenatural: 22. | `0,35` | `28 m/s²` | 65 |
| Centinela de Cobre | 160 HP | 1,7 m/s | Abanico de tres proyectiles físicos: 16 por impacto. | `0` (inmune) | `24 m/s²` (sin impulso) | 80 |
| Gólgora / MataColeccionista | `3 600` demo (`EnemyArchetypeSettings` base) / `9 000` completa | 1,6 m/s en Fase 1 (`×1,3` en Fase 2) | Contacto: 30; seis patrones descritos más abajo. | `0` (inmune) | `30 m/s²` (sin impulso) | 500 |

## Residual: Esqueleto

Forma la masa básica de la horda. Persigue al jugador, se separa de otros cuerpos y busca paso entre obstáculos. Los cuatro pulsos base de la linterna suman sus `15 HP` cuando todos aciertan. Recibe el empuje completo (`susceptibilidad = 1,0`, desaceleración `28 m/s²`) y su ataque de contacto reproduce la animación de golpe.

[Ver spritesheet, fuente editable y GIFs de todas sus acciones](05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#residual-esqueleto).

## Interceptor: galgo de chatarra

Anticipa hasta `0,4 s` del movimiento del jugador, con una velocidad de referencia limitada a `8 m/s`. A corta distancia pasa a perseguir la posición actual. Con susceptibilidad `1,0` y menor desaceleración de frenado (`18 m/s²`), tras recibir empuje se desliza más lejos que el Residual.

[Ver spritesheet, fuente editable y GIFs de todas sus acciones](05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#interceptor-galgo-de-chatarra).

## Coloso: Wendigo

Es pesado, sólido e inmune al empuje (`susceptibilidad = 0`). Cuando dispone de carga y el jugador está a `8 m` o menos, fija la dirección y muestra un corredor de aviso durante `0,8 s`. Embiste a `12 m/s` durante `0,35 s` (unos `4,2 m` libres) y se recupera durante `0,6 s`. Las paredes detienen el recorrido y la dirección queda comprometida tras el aviso. El intervalo entre cargas se sortea entre `5` y `7 s`.

Preparación y recuperación no infligen daño de contacto. El dash del jugador permite atravesar su cuerpo durante la ventana de invulnerabilidad.

[Ver spritesheet, fuente editable y GIFs de todas sus acciones](05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#coloso-wendigo).

## Mortero: Campana

Mantiene una banda de distancia entre `5,5` y `6,5 m` y ataca hasta `12 m`. Tiene un intervalo de `5 s`, aviso de `0,45 s` y vuelo de `0,85 s`. El proyectil deja un charco ácido de radio `1,2 m` durante `3 s`, con `5` de daño cada `0,5 s`. Su susceptibilidad al empuje es reducida (`0,7`, desaceleración `24 m/s²`). El director de oleadas limita los Morteros simultáneos a seis.

[Ver spritesheet, fuente editable y GIFs de todas sus acciones](05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#mortero-campana).

## Polilla: Reactor

Se aproxima al Coleccionista y **arma su espoleta por proximidad al situarse a `2 m` del jugador** (`EnemyActor.MothArmDistance = 2f`, `EnemyActor.cs:25, 708`; el asset `ExplosiveMoth.asset` no serializa este radio). La cuenta atrás por proximidad dura **`0,9 s`**; si la Polilla muere por daño del jugador antes de detonar, entra en una espoleta de muerte de **`1,2 s`**.

Al detonar en un radio de **`3 m`**, inflige **`35` de daño físico completo (`BlastDamage = 35f`) tanto al jugador como a todos los objetivos enemigos (`IsEnemyTarget`, incluido Gólgora**; durante `CercoActive`, Gólgora aplica su `35%` de reducción, `MothExplosionPool.cs:11-13, 111-131, 148-149`). Durante la mecha conserva su susceptibilidad al empuje (`1,0`, desaceleración `22 m/s²`), por lo que el cuarto pulso de la linterna o el *Rayo solar* pueden empujarla hacia el interior de la horda antes de que estalle.

[Ver spritesheet, fuente editable y GIFs de todas sus acciones](05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#polilla-reactor).

## Caballero Fantasma

Es uno de los dos élites. Recibe solo el `25%` del daño físico y el `100%` del daño sobrenatural, con susceptibilidad reducida al empuje (`0,35`, desaceleración `28 m/s²`). Su barrido tiene `3,6 m` de alcance, arco de `120°`, daño sobrenatural de `22`, preparación de `0,8 s`, recuperación de `0,8 s` y recarga de `3,5 s`. No causa daño por contacto normal.

El cuerpo usa su sprite de 224 fotogramas: la preparación ocupa `0,8 s`, y el ataque de seis fotogramas junto con la recuperación de seis comparten los `0,8 s` posteriores al barrido real.

![Caballero Fantasma: sprite integrado en Unity, 5 de noviembre](Media/Avance_2026-11-05/Enemigos/CaballeroFantasma_Integrado.gif)

## Centinela de Cobre

Recibe el `25%` del daño sobrenatural salvo durante su recuperación, cuando se abre su ventana de vulnerabilidad y recibe el daño completo (`100%`). El daño físico se mantiene siempre completo. Prepara un abanico de tres proyectiles durante `1,0 s`, se recupera durante `2,0 s` y recarga en `3,5 s`. El alcance es de `11 m` y cada impacto causa `16` de daño físico. Es pesado, inmune al empuje (`susceptibilidad = 0`) y no causa daño por contacto normal.

La presentación anuncia los rayos del abanico y muestra «Vulnerable» en su recuperación. El cuerpo usa su sprite de 288 fotogramas: abre las compuertas durante `0,3 s` y prepara `0,7 s` antes del disparo real; después reproduce disparo de `0,4 s`, vulnerabilidad de `1,3 s` y cierre de `0,3 s` dentro de los `2,0 s` de recuperación.

![Centinela de Cobre: sprite integrado en Unity, 5 de noviembre](Media/Avance_2026-11-05/Enemigos/CentinelaCobre_Integrado.gif)

Los élites empiezan a aparecer desde los `90 s` y se alternan cada `25 s` cuando hay capacidad, con un límite conjunto de dos élites vivos. Sus efectos de barrido y proyectiles se documentan en [[06 - Registro visual]].

## Gólgora: MataColeccionista

En los perfiles de arquetipo del jefe, **`Golgora_Demo.asset`** (y el valor por defecto en `EnemyArchetypeSettings.cs`) asigna **3 600 HP** para la ronda demo (`3:00`), mientras que **`Golgora_Complete.asset`** asigna **9 000 HP** para la ronda completa (`8:00`), ambos con transición a Fase 2 al bajar al `50%` (`1 800 HP` / `4 500 HP`) y parámetros de ataque unificados entre `GolgoraCombatSettings.cs` y `CombatSettings.asset`. Su entrada retira la horda ordinaria sin otorgar recompensas y traslada al jugador con protección a la caldera central de `22 m` de radio, cuyo perímetro queda cerrado.

Gólgora usa un modelo 3D animado y efectos de magma propios. Sus seis patrones se detallan en la tabla siguiente con los parámetros unificados de `GolgoraCombatSettings.cs`, `CombatSettings.asset` y `GolgoraActor.cs`:

| Patrón | Parámetros en Fase 1 y escalado en Fase 2 (`GolgoraCombatSettings.cs` / `CombatSettings.asset`) |
| --- | --- |
| **Martillazo** | Círculo de `3 m` situado `3 m` por delante; `40` de daño físico, preparación de `1,2 s` (`×0,85` en Fase 2), recuperación de `1,0 s` (`×0,75` en Fase 2) y enfriamiento de `6 s` (`×0,75` en Fase 2). Puede dañar y empujar secuaces. |
| **Barrido** | Radio de `6 m` y arco frontal de `180°`; `30` de daño físico, preparación de `1,0 s` (`×0,85` en Fase 2), recuperación de `1,0 s` (`×0,75` en Fase 2) y enfriamiento de `7 s` (`×0,75` en Fase 2). |
| **Bombardeo** | Alcance de `18 m`, preparación de `0,7 s` y vuelo base de `0,8 s` (ambos `×0,85` en Fase 2), **recuperación de `0,6 s`** (`×0,75` en Fase 2) y enfriamiento de `8 s` (`×0,75` en Fase 2). **Fase 1:** `3` proyectiles (`25` de daño en `2,5 m` de radio, dejando charco de `2,5 m` durante `6 s` a `8` daño/s). **Fase 2:** `5` proyectiles (charco de `3 m` durante `7,5 s`). |
| **Cerco de magma** | Preparación de salto de `0,9 s` (`×0,85` en Fase 2), salto invulnerable de `0,65 s` hacia el centro, **impacto de aterrizaje de `40` de daño físico en `3 m` de radio**, y anillo de fuego que se expande hasta `18 m` a `6 m/s` con `1,3 m` de grosor, `30` de daño sobrenatural y una brecha segura de `60°`. Emite `1` onda en Fase 1 y `3` ondas en Fase 2 (separadas por `1,3 s` y con la brecha rotada `45°`). Su **enfriamiento de `18 s` es fijo en ambas fases** (`GolgoraActor.cs`, no se multiplica por `0,75` en Fase 2). |
| **Fisura de magma** | **Preparación de `1,1 s` fija en ambas fases** (`GolgoraActor.cs`), `5` impactos en línea hacia el jugador entre `3,2 m` y `22 m` con vuelos escalonados, `35` de daño en radio de `2,2 m` y charcos de `2,5 m` durante `5 s` a `8` daño/s. Recuperación de `0,8 s` y enfriamiento de `10 s` (ambos `×0,75` en Fase 2). |
| **Lluvia de escoria** | Exclusiva de **Fase 2**: `5` impactos alrededor de la posición prevista del jugador (`26` de daño en radio `1,8 m`, preparación base `1,35 s`, vuelo base `0,9 s`, recuperación `1,0 s`, enfriamiento `14 s` y charcos de `3,5 s`). |

### Selección de ataques

En persecución se evalúa primero la transición de fase y después el *Cerco de magma* (habilitado a partir de los `12 s` del reloj del jefe). Si ninguno se inicia, se intenta *Lluvia de escoria* (solo en Fase 2, desde los `9 s`) y luego *Fisura de magma* (desde los `6 s`, a partir de `8 m`). Ambas requieren espacio en el pool de proyectiles.

A `6,5 m` o menos se alterna la preferencia entre *Martillazo* y *Barrido* según el último ataque ejecutado y sus enfriamientos (habilitados a los `2 s`). A más de `6,5 m` se intenta *Bombardeo* (habilitado a los `3,5 s`). La transición a segunda fase concluye iniciando un *Cerco de magma*.

| Canal de daño | Fuentes de Gólgora |
| --- | --- |
| **Físico** | Contacto (`30`), *Martillazo* (`40`), *Barrido* (`30`) e **impacto de aterrizaje de *Cerco de magma* (`40` en `3 m`)**. |
| **Sobrenatural** | Anillos expansivos del *Cerco de magma* (`30`), impactos de *Bombardeo* (`25`), *Fisura de magma* (`35`), *Lluvia de escoria* (`26`) y todos los charcos de magma (`4` cada `0,5 s` = `8` daño/s). |

[Ver los seis patrones de Gólgora en GIFs capturados en Unity](05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#g%C3%B3lgora-matacoleccionista).

### Segunda fase y resistencia al control

Al bajar al `50%` de vida se inicia una transición invulnerable de **`1,5 s`** con un rugido que empuja al jugador en un radio de **`12 m`**. En Fase 2 la velocidad de movimiento pasa a `×1,3`; las preparaciones compatibles se multiplican por `0,85` (*Fisura de magma* mantiene sus `1,1 s` fijos); y las recuperaciones y enfriamientos compatibles se multiplican por `0,75` (*Cerco de magma* mantiene sus `18 s` fijos de enfriamiento).

Gólgora es inmune al empuje (`susceptibilidad = 0`). La ralentización tiene un máximo del `25%`. Congelaciones y aturdimientos se reducen al `35%` de su duración, con un máximo de `0,6 s` y una ventana de inmunidad posterior de `1,5 s`. Durante introducción, transición y salto es invulnerable, y durante la emisión activa del *Cerco de magma* aplica un `35%` de reducción de daño.

### Refuerzos y cierre

El director solicita grupos de hasta cuatro secuaces cada `4 s` (con límite de `8` secuaces activos o anunciados en la caldera y aviso previo de `0,9 s`), rotando el orden Residual, Residual, Mortero y Polilla. El *Cerco de magma* y la transición de fase suspenden los refuerzos y retiran los presentes.

La muerte de Gólgora con el jugador vivo otorga la victoria, reproduce la absorción de su gema, suma `500 XP` al resumen y sincroniza la partida con el ranking del evento (`EventLeaderboardSync`).

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Objetos y coleccionables

## Gemas y botiquines

Las gemas conceden la XP del enemigo derrotado y permanecen en el escenario hasta recogerse (fusionándose entre sí cuando caen a menos de `0,75 m`). Los botiquines curan `15 HP` de base (`17 / 19 / 21 HP` con `GREED_03`) y caducan tras `22 s`. Ambos responden al imán del jugador y a *Pulso colector* (`GREED_05`).

## Catálogo de 15 entidades de la colección

La colección registra permanentemente hasta **15 entidades descubiertas** (`discovered[15]`) y almacena un **máximo estricto de 18 unidades en posesión** (`FusionProgress.Capacity = 18`, `owned[15]` cuya suma nunca supera `18`). Nueve entidades aparecen como fragmentos en la arena y seis se obtienen mediante la máquina de fusión.

| ID | Entidad | Rareza en catálogo | Fuente | Poder de acompañante (`CompanionPowerRuntime`, **acumulable por copia equipada**) |
| ---: | --- | --- | --- | --- |
| 0 | Lobito | Común | Arena · 25% | `+10 HP` máximos (`+10 × copias`). |
| 1 | Mago | Raro (Poco común) | Arena · 10% | Lanza un proyectil mágico perforante de `18` de daño base cada `10 s` (un disparo por copia equipada). |
| 2 | Demonio | Raro (Poco común) | Arena · 10% | `+15%` de daño por copia; `30%` de probabilidad por copia de aplicar quemadura de `4` de daño base/s durante `3 s`. |
| 3 | Shireen | Especial (Rara) | Arena · 5% | Cura `2 HP` por copia cada `5 s`. |
| 4 | Yanikua | Mítico (Legendaria) | Fusión (exclusiva) | Multiplicador de salud máxima `1 + copias` (`×2 / ×3 / ×4`); `+20%` de daño y `+15%` de velocidad de movimiento por copia; **1 salvación ante golpe letal por copia en cada expedición** (restaura al `30%` de HP máx. y otorga `3 s` de invulnerabilidad). No puede usarse como ingrediente de fusión. |
| 5 | Basket | Común | Arena · 25% | `+10%` de velocidad de movimiento por copia. |
| 6 | Hada | Épico | Arena · 7% | Bloquea por completo `1` golpe cada `20 s` (cada copia equipada gestiona un enfriamiento independiente de `20 s`). |
| 7 | Lisa | Legendario | Arena · 3% | `+20%` de cadencia de ataque por copia; cada `12 s` congela `2 s` por copia en un radio de `8 m` (a Gólgora lo ralentiza hasta su tope del `25%`). |
| 8 | Reaper | Común | Fusión | `+8%` de daño por copia. |
| 9 | Chico Emo | Raro (Poco común) | Fusión | Suma `+100%` (`×2 / ×3 / ×4`) a la velocidad de movimiento durante `2 s` al terminar el dash. |
| 10 | Conde Alucard | Especial (Rara) | Fusión | Cura `1 HP` por copia cada `5` bajas enemigas. |
| 11 | Murciélago Cristal | Épico | Fusión | Cada `8 s` emite una onda de `15` de daño base por copia en `8 m` que ralentiza un `40%` durante `3 s`. |
| 12 | Sirena | Legendario | Fusión | Cada `15 s` emite una onda en `8 m` que empuja `4 m` y cura el `5%` de la salud máxima por copia. |
| 13 | Soldier | Especial (Rara) | Arena · 5% | `+15 HP` máximos, `-15%` de daño recibido y `+10%` de armadura por copia equipada (`Soldier × N` en `PlayerRuntimeStats` y `PlayerHealth`). |
| 14 | Joel | Raro (Poco común) | Arena · 10% | `+15%` de cadencia de ataque por copia. |

Cada `30 s` de combate activo se intenta generar entre `1` y `3` fragmentos en posiciones válidas del mapa, con un máximo de `12` activos simultáneamente y `10 s` de duración por aparición. El jugador debe acercarse a menos de `2,5 m` con línea de visión despejada y pulsar **`E`** (o el botón oeste en mando).

## Capacidad máxima de 18 unidades y bloqueo por inventario lleno

La colección admite un máximo estricto de **18 unidades en total** (`FusionProgress.Capacity = 18`), que se corresponden uno a uno con los **18 pedestales físicos de la bóveda** (`DisplaySlots[18]`): **3 puestos de equipo activo** en el balcón superior (`slot 0..2`) y **15 puestos de exposición/reserva** en la planta inferior (`slot 3..17`). Cuando algún puesto del balcón está vacío, exhibe una copia en reserva etiquetada como `EQUIPO N · RESERVA`, permitiendo mostrar hasta las 18 unidades simultáneamente en la escena.

- **Bloqueo de recogida en la arena (`CompanionSystem.cs`, `CollectibleDisplay.cs`, `CollectiblePickupFeedback.cs`):** si el jugador ya posee `18/18` unidades (`!FusionProgress.HasSpace`), no puede recoger más fragmentos en combate y aparece el aviso flotante **`INVENTARIO LLENO / LIBERA ESPACIO`**.
- **Bloqueo de reclamo en la máquina de fusión (`FusionStationUI.cs`):** si el jugador intenta reclamar el resultado de una fusión teniendo `18/18` unidades, la máquina muestra **`INVENTARIO LLENO · LIBERA ESPACIO`** (*"Tu colección tiene 18 unidades. Libera una en la bóveda y vuelve: el resultado de esta fusión seguirá aquí."*) y mantiene el resultado guardado en estado `AwaitingDecision` hasta que libere espacio en algún pedestal de la bóveda.

## Interacción con los puestos de la bóveda y señalización

La bóveda cuenta con señalización completa en el mundo y en el HUD (`GameMenuView.cs`, `CollectibleDisplay.cs`):

1. **Rótulos 3D sobre los pedestales del balcón:** **`EQUIPO 1`**, **`EQUIPO 2`** y **`EQUIPO 3`** (añadiendo `· RESERVA` cuando exhiben una unidad sin equipar).
2. **Indicadores direccionales superiores (`VaultDirections`):** dos tarjetas dinámicas en la parte superior con flechas (`▲`, `▼`, `◄`, `►`, `●`) que apuntan en tiempo real hacia la **Máquina de Fusión** (*"MÁQUINA DE FUSIÓN · Combina 4 copias de varias rarezas"*, *"BAJA A FUSIÓN"* o *"E · MÁQUINA DE FUSIÓN"*) y hacia los **Puestos de Equipo** (*"SUBE AL BALCÓN · Puestos de equipo 1, 2 y 3"*, *"EQUIPAR ENTIDADES"* o *"PUESTO 1..3 · EQUIPAR/EDITAR"*).
3. **Panel rotatorio de consejos (`VaultTips`):** tarjeta en la esquina inferior izquierda con la etiqueta **`CONSEJO`** que rota cada `8 s` entre *"Equipa coleccionables antes de ir a la arena."*, *"Abre el índice para consultar tus descubrimientos."* y *"Combina cuatro coleccionables en la máquina de fusión."*, o prioriza *"Pulsa E frente a un puesto para equipar tu primer coleccionable."* cuando `OwnedCount > 0 && !hasEquippedCompanion`.

Las interacciones de cada nivel de la bóveda son:

- **Puestos 1 a 3 (balcón superior, equipo activo — `CompanionSystem.cs`, `CompanionView.cs`, `VaultReleaseInteraction.cs`):**
  - El aviso contextual al acercarse es **`E · EQUIPAR`** (si está vacío, incluso cuando exhibe visualmente una reserva) o **`E · EDITAR`** (si tiene una entidad equipada).
  - Pulsar **`E`** (o botón oeste en mando) abre el selector de acompañantes (`CompanionView`, encabezado `PUESTO DE EQUIPO 1/2/3`, subtítulo `ELIGE UNA ENTIDAD DESCUBIERTA PARA ESTE PUESTO` y la nota *"Señala un compañero para ver su poder. Los poderes se acumulan por copia equipada."*). Su barra inferior ofrece tres botones: **`QUITAR DEL PUESTO`** (desequipa la unidad y la devuelve a reserva), **`LIBERAR COPIA`** (abre el modal de confirmación para eliminar la copia de la colección) y **`CERRAR · E / ESC`**.
  - Además, al estar frente a un puesto de equipo ocupado en el mundo, pulsar **`T`** (o el botón norte `Y` / `Triángulo` en mando) retira directamente la unidad del puesto de equipo sin abrir el menú (`VaultReleaseInteraction.TryUnequip`).
- **Puestos 4 a 18 (planta inferior, exposición/reserva — `VaultReleaseInteraction.cs`):**
  - Al acercarse a un puesto ocupado (`slot >= 3`), el aviso contextual es **`E · LIBERAR`** (`VaultReleaseInteraction.cs:67`).
  - Pulsar **`E`** (o botón oeste en mando) abre el cuadro de confirmación **`LIBERAR UNIDAD`**: *"¿Seguro que quieres liberar a `<Nombre>`? Esta unidad desaparecerá de tu colección."* con los botones **`CANCELAR`** (`Esc` / botón este) y **`LIBERAR`** (`E`, `Enter`, `Espacio` o `A`/`Cruz`).
  - Al confirmar (`FusionStation.ReleaseCollectible`), se descuenta `1` de `owned[id]`, se conserva permanentemente `discovered[id]` en el Índice de Colección, se reordenan los puestos de exposición por rareza y se guarda en la ranura activa (`SaveSlotService`, con respaldo en `Saves/slot_{N}.json`).

![Selector de entidades del puesto](Media/Interfaz/Equipar_coleccionable.png)
*Selector de entidades del puesto (captura de referencia `734e2a5`), con la nota de acumulación por copia equipada y los tres botones inferiores integrados: `QUITAR DEL PUESTO`, `LIBERAR COPIA` y `CERRAR · E / ESC`.*

## Índice de Colección (Bestiario de entidades — `VaultCollectionIndex.cs`, tecla `[I]`)

El **Índice de Colección** funciona como el **bestiario y libro de registro** donde el jugador consulta qué criaturas ha descubierto, cuáles le faltan para completar las **15 entidades** del catálogo y qué unidades conserva actualmente en su bóveda.

- **Control de apertura y cierre (`[I]`):** dentro de la bóveda se abre pulsando la tecla **`I`**, el botón **`Select`** en mando o haciendo clic en el botón superior izquierdo **`ÍNDICE · [I]`** (`BotonIndiceBoveda` en `VaultCollectionIndex.cs`), lo que pausa la simulación (`PauseReason.FusionMenu`). Se cierra con **`I`**, **`Select`**, **`Esc`**, botón este (`B` / `Círculo`) o el botón **`X   CERRAR · [I]`** *(nota: la tecla `Tab` está reservada exclusivamente al laboratorio de pruebas `EnemyLaboratoryPanel`, no al bestiario)*.
- **Encabezado y contadores:** muestra el título **`ÍNDICE DE COLECCIÓN`** y el contador en vivo **`{descubiertos} / 15 DESCUBIERTOS · {en posesión} / 18 UNIDADES`**.
- **Qué acción cuenta como descubrimiento (`FusionStation.CollectFromArena` y `FusionStation.Resolve`):** en la versión jugable, la ficha de una criatura (`discovered[id] = true`) se desbloquea **en el instante en que el jugador la obtiene por primera vez**, ya sea **recogiendo su fragmento en la arena con `E` / botón oeste** o **reclamándola en la máquina de fusión con el botón `COLECCIONAR`** (ver aparecer un fragmento en el suelo sin recogerlo no cuenta como descubrimiento). Al descubrirla por primera vez, su silueta negra cambia de forma permanente a su retrato a color, se incrementa el contador `{descubiertos} / 15` (y suma `+150` puntos base si ocurre durante la expedición puntuable) y el desbloqueo se mantiene para siempre aunque después se liberen sus copias en la bóveda (`FusionStation.ReleaseCollectible`).
- **Pestaña 1 — `POR RAREZAS` (15 entidades en 6 categorías):** incluye el filtro **`TODAS`** además de las **6 categorías de rareza** (`COM`, `RAR`, `ESP`, `ÉPI`, `LEG`, `MÍT`):
  - **Común (`3`):** *Lobito, Basket y Reaper*.
  - **Raro / Poco común (`4`):** *Mago, Demonio, Chico Emo y Joel*.
  - **Especial / Rara (`3`):** *Shireen, Conde Alucard y Soldier*.
  - **Épico (`2`):** *Hada y Murciélago Cristal*.
  - **Legendario (`2`):** *Lisa y Sirena*.
  - **Mítico (`1`):** *Yanikua*.
  Las criaturas que aún faltan por descubrir aparecen como **siluetas negras** (`Color(0.03, 0.03, 0.04, 0.96)`) con un signo de interrogación (**`???`**) y el aviso **`NO ENCONTRADO`** / **`SILUETA BLOQUEADA`** (*"Aún no has registrado esta entidad. Encuéntrala en la arena o mediante la máquina de fusión para revelar su retrato y su efecto de compañero."*).
- **Pestaña 2 — `EN TU BÓVEDA`:** lista únicamente las unidades actualmente en posesión (`hasta 18 unidades`), indicando en cada tarjeta si se encuentra **`EN EQUIPO ACTIVO`** (puestos 1–3) o **`EN BÓVEDA`**.
- **Panel derecho de inspección (`PanelDetalleIndice`):** al seleccionar cualquier entidad descubierta, muestra su insignia de rareza, retrato a color ampliado, fuente de obtención (*Arena* o *Máquina de fusión*), copias actuales en posesión (`xN`) y la descripción completa de su **poder de compañero**.

![Índice de Colección (Bestiario en versión final)](Media/Interfaz/Bestiario.png)
*Captura de la versión final del bestiario (`Índice de Colección`), mostrando el contador en vivo, la navegación por rarezas con Pixelify Sans, las siluetas bloqueadas (`???`) y la inspección completa de poder y origen.*

## Máquina de fusión (`FusionProgress.cs`, `FusionStation.cs`, `FusionStationUI.cs`, `CollectibleUnitGrid.cs`)

La estación ubicada en la pared trasera de la bóveda (`x = 189, z = 28,88`) se abre con **`E`** (o botón oeste en mando: **`E · FUSIONAR`** cuando está libre o en cuenta atrás, o **`E · VER FUSIÓN`** cuando un resultado aguarda en `AwaitingDecision`).

### Reglas de ingredientes y duración fija de 30 segundos

1. **Duración fija de 30 segundos (`FusionRecipe.FusionSeconds = 30`):** **todas las fusiones duran exactamente `30 s` (`00:30`)**, sin importar la rareza de los ingredientes. El temporizador avanza en tiempo real (`Time.unscaledDeltaTime`) y persiste entre escenas.
2. **Cuatro unidades de la misma rareza o de rarezas mixtas (excluida Yanikua):** acepta **cualquier combinación de 4 copias válidas** (`FusionRecipe.TryPreview` / `TryCreate`), permitiendo tanto recetas homogéneas como mezclas de distintas rarezas. El resultado interpola linealmente los pesos internos en función de la calidad media (`qualityMean = total / 4f`). **Yanikua** (`id = 4`) está explícitamente excluida como ingrediente (`id != 4`).
3. **Uso permitido de copias equipadas (`EN EQUIPO 1/2/3`):** en el selector de unidades de la máquina (`CollectibleUnitGrid.cs`), las copias equipadas en los puestos 1 a 3 del balcón **sí se pueden seleccionar como ingredientes** y aparecen con la etiqueta **`EN EQUIPO 1/2/3`** (mientras que las ya asignadas a otra ranura de la máquina muestran `RANURA 1..4`). Al pulsar **`CONFIRMAR`** (`FusionProgress.Begin`), si las copias seleccionadas superan las copias en reserva, el sistema las desequipa automáticamente de sus puestos de equipo y consume las 4 unidades.

### Probabilidades de fusión y redondeo en interfaz

La tabla muestra los puntos de referencia cuando los 4 ingredientes pertenecen a la misma rareza (en combinaciones de rarezas mixtas, los porcentajes cambian de forma continua mediante interpolación lineal entre bandas adyacentes):

| Receta de entrada (4 ingredientes homogéneos) | Duración | Pesos internos por entidad (`FusionProgress.cs`) | Porcentajes mostrados en UI (`FusionRecipe.DisplayPercent`) |
| --- | ---: | --- | --- |
| **4 Comunes** (`media 0,0`) | `30 s` (`00:30`) | Reaper `60%` · Chico Emo `40%` | **`60%` Reaper · `40%` Chico Emo** |
| **4 Raros / Poco comunes** (`media 1,0`) | `30 s` (`00:30`) | Chico Emo `60%` · Conde Alucard `40%` | **`60%` Chico Emo · `40%` Conde Alucard** |
| **4 Especiales / Raros** (`media 2,0`) | `30 s` (`00:30`) | Conde Alucard `60%` · Murciélago Cristal `40%` | **`60%` Conde Alucard · `40%` Murciélago Cristal** |
| **4 Épicos** (`media 3,0`) | `30 s` (`00:30`) | **Murciélago Cristal `59,4%` · Sirena `39,6%` · Yanikua `1%`** | **`60%` Murciélago Cristal · `40%` Sirena · `1%` Yanikua** |
| **4 Legendarios** (`media 4,0`) | `30 s` (`00:30`) | **Sirena `70%` · Yanikua `30%`** | **`70%` Sirena · `30%` Yanikua** |

*(Nota sobre `FusionProgress.cs:48-65`: la reserva mítica `mythic` asigna `30%` con `4` Legendarios, `15%` con `3`, `5%` con `2` y `1%` cuando la mejor rareza es `≥ Épica`, repartiendo el resto `100% - mythic%` mediante `table`; así, `4 Legendarios` arroja `70%` Sirena / `30%` Yanikua, mientras que `4 Épicas` arroja `59,4%` Murciélago Cristal / `39,6%` Sirena / `1%` Yanikua y `DisplayPercent` redondea `60% / 40% / 1%` en la UI).*

![Máquina de fusión con cuatro espacios de ingredientes y posibles resultados](Media/Interfaz/Maquina_Fusion.png)
*Ventana de la Máquina de Fusión (captura de referencia `734e2a5`) con las 4 ranuras horizontales, temporizador `TIEMPO DE FUSIÓN 00:30`, panel `POSIBLES RESULTADOS` y botones `VACIAR`, `CONFIRMAR` y `X`; en `0.7.0`, la estación se sitúa en la pared trasera `(189, 0, 28,88)`.*

### Flujo de ventanas de la estación

- **Ventana principal:** presenta las **4 ranuras en fila horizontal** (`Espacio_0` a `Espacio_3`), el contador fijo **`TIEMPO DE FUSIÓN  00:30`**, el panel **`POSIBLES RESULTADOS`** actualizado en vivo para recetas homogéneas o mixtas, los botones **`VACIAR`** y **`CONFIRMAR`**, y el botón superior derecho **`X`** para cerrar.
- **Selector de ranura:** muestra en cuadrícula las unidades disponibles (incluidas `EN EQUIPO 1/2/3`) junto a los botones **`QUITAR INGREDIENTE`** y **`VOLVER`**.
- **Pantalla de resultado (`AwaitingDecision`):** al completarse los `30 s` y abrir la estación con `E · VER FUSIÓN`, se reproduce la animación de apertura y se muestra la unidad obtenida con el botón único **`COLECCIONAR`** (más la `X` de cierre). **No existe botón "Liberar" en la pantalla de resultado de la máquina**; la liberación se realiza en los pedestales de la bóveda (`Media/Interfaz/Resultado_Fusion.png`).

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Misiones y desafíos

La expedición tiene una condición principal: sobrevivir a la horda y derrotar a Gólgora con el Coleccionista vivo. La ronda demo activa la entrada del jefe a los tres minutos (`3:00`) y la completa a los ocho (`8:00`); alcanzar ese tiempo abre la caldera central y el combate continúa hasta que uno de los dos cae.

Antes de salir al Vertedero, la preparación en la bóveda invita a consultar el **Índice de Colección (`[I]`)**, fusionar grupos de cuatro copias de la misma rareza o de rarezas mixtas (`30 s`) y equipar hasta tres entidades en el balcón superior (`EQUIPO 1`, `EQUIPO 2`, `EQUIPO 3`), con el apoyo constante de las flechas direccionales (`VaultDirections`) y el panel de consejos (`VaultTips`). También es posible iniciar una incursión con puestos vacíos.

Los desafíos del combate surgen de mantener rutas de escape entre enemigos y obstáculos (deslizándose suavemente junto a rocas bajas), recoger gemas de XP, hacer detonar a las Polillas Reactor (`2 m` de activación) para que su explosión inflija `35` de daño físico a los enemigos circundantes, esquivar las embestidas del Coloso y aprovechar la ventana «Vulnerable» del Centinela de Cobre o el daño sobrenatural frente al Caballero Fantasma. Los fragmentos de coleccionables que aparecen durante `10 s` añaden una decisión de riesgo y gestión de inventario: acercarse a recogerlos manualmente con `E` siempre que quede espacio dentro del límite de **18 unidades** de la colección.

El cierre traslada la acción a la caldera de `22 m`. Gólgora alterna *Martillazo*, *Barrido*, *Bombardeo*, *Cerco de magma* (`40` de daño físico al aterrizar y ondas con brecha de `60°`, enfriamiento fijo de `18 s`) y *Fisura de magma* (`1,1 s` fijos de preparación), sumando *Lluvia de escoria* y mayor velocidad al bajar al `50%` de vida.

Al concluir el intento, el resumen presenta el tiempo sobrevivido, el nivel alcanzado, las bajas y la XP obtenida, y **`EventLeaderboardSync`** sincroniza la puntuación de la sesión con el ranking en vivo de Firebase antes de devolver al jugador a la bóveda.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Dirección de arte

El juego combina un escenario 3D de formas angulares con sprites de pixel art, un jefe con modelo 3D animado y efectos de luz y partículas. Metal, roca, madera y chatarra ocupan el fondo oscuro; las siluetas, los avisos de suelo y las fuentes luminosas separan con nitidez lo que requiere atención inmediata.

El perfil exterior coloca la cámara a `35,5°` con campo de visión de `34°`. La bóveda usa una perspectiva propia (`32°` y `65°` de FOV) y el encuentro de Gólgora adapta la distancia para encuadrar a ambos combatientes dentro de la caldera. El Coleccionista se representa en un plano vertical con proporciones fijas dentro del mundo.

| Uso | Tratamiento presente |
| --- | --- |
| Exterior | Azules y violetas oscuros, niebla, haces y polvo ambiental. |
| Materiales | Acero apagado, cobre, óxido, piedra y madera. |
| Jugador | Capucha clara, farol y haz automático; brillo pasivo en la bóveda. |
| Ácido | Verde brillante sobre el terreno oscuro. |
| Peligro anunciado | Ámbar y naranja, contornos definidos y progreso visible. |
| Hielo | Tinte celeste, cristales y niebla fría. |
| Jefe | Armadura oscura, núcleo incandescente y efectos de magma. |
| Colección e interfaz | Tipografía unificada **`Pixelify Sans`**, retratos enmarcados, rótulos 3D `EQUIPO 1..3`, colores por rareza y siluetas negras (`???`) para entidades aún no descubiertas en el Índice de Colección (`[I]`). |

Apollo permanece como referencia de paleta. Los sprites activos del Coleccionista, Mortero y Polilla usan celdas de `64 × 64`; Esqueleto, Interceptor, Wendigo, Caballero Fantasma y Centinela de Cobre usan `96 × 96`. Sus atlas y animaciones están en [[02 - Personajes y Animación]].

El deslizador de **Efectos decorativos** (`0%` a `100%` en la pestaña `General` de pausa) reduce decoración y movimiento accesorio por debajo del `99,9%`, conservando intactos todos los avisos de peligro necesarios para jugar.

## Identidad visual, logotipos y portadas

Los recursos de identidad gráfica de *EchoScrap* y sus archivos fuente editables se reúnen en `Media/Identidad/` (además de los editables de HUD, botones y marcos de cartas en `ArtSource/UI/` dentro del proyecto de Unity):

| Recurso | Archivo | Uso / Descripción |
| --- | --- | --- |
| **Isotipo (Logo)** | [EchoScrap-logo.svg](Media/Identidad/EchoScrap-logo.svg) | Emblema vectorial del Coleccionista para icono y material gráfico del proyecto. |
| **Logotipo (Título)** | [EchoScrap-titulo.svg](Media/Identidad/EchoScrap-titulo.svg) · [Editable `.aseprite`](Media/Identidad/EchoScrap-titulo.aseprite) | Tipografía gráfica oficial *ECHOSCRAP* en formato vectorial y fuente editable pixel art. |
| **Portada horizontal** | [Portada_EchoScrap.png](Media/Identidad/Portada_EchoScrap.png) | Ilustración principal de portada con el Coleccionista frente al umbral rúnico de la bóveda. |
| **Tarjeta vertical y arte fuente** | [Tarjeta_EchoScrap.png](Media/Identidad/Tarjeta_EchoScrap.png) · [Vectorial `.svg`](Media/Identidad/EchoScrap-arte.svg) | Tarjeta de presentación (`810 × 1080`) para difusión del proyecto y página de distribución. |

![Isotipo de EchoScrap|140](Media/Identidad/EchoScrap-logo.svg) ![Logotipo de EchoScrap|220](Media/Identidad/EchoScrap-titulo.svg) ![Portada oficial de EchoScrap|360](Media/Identidad/Portada_EchoScrap.png)

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Personajes y animación

Esta página reúne las fichas visuales del Coleccionista, los cinco enemigos de horda, los dos élites, Gólgora y las quince entidades de la colección. Cada enemigo de horda incluye su spritesheet, la fuente editable y los GIFs de todas sus acciones.

Las reglas y cifras de combate se consultan en [[03 - Enemigos y NPC]] y [[04 - Objetos y Coleccionables]]. La identidad, historia de origen y papel general de cada personaje se recogen en [[03 - Personajes]].

## Proporciones en reposo

![Comparación a escala del Coleccionista y los ocho enemigos](Media/Avance_2026-11-07/Enemigos/Proporciones.png)

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

![Boceto a mano del Coleccionista|260](Media/ConceptArt/Coleccionista.jpeg)

![Atlas activo del Coleccionista](Media/Avance_2026-11-04/Spritesheets/Coleccionista.png)

[Fuente editable del Coleccionista](Media/Avance_2026-11-04/Spritesheets/Coleccionista.aseprite).

| Animación | Cuatro direcciones |
| --- | --- |
| Reposo | ![Coleccionista: Reposo](Media/Avance_2026-11-04/Animaciones/Coleccionista/Idle.gif) |
| Marcha | ![Coleccionista: Marcha](Media/Avance_2026-11-04/Animaciones/Coleccionista/Walk.gif) |
| Dash | ![Coleccionista: Dash](Media/Avance_2026-11-04/Animaciones/Coleccionista/Dash.gif) |
| Muerte | ![Coleccionista: Muerte](Media/Avance_2026-11-04/Animaciones/Coleccionista/Death.gif) |

## Residual: Esqueleto

Es la masa básica de la horda. Su representación es un esqueleto animado en cuatro direcciones con atlas de 144 fotogramas de `96 × 96`: reposo, marcha y ataque de ocho, impacto de cuatro y muerte de ocho por dirección.

![Boceto a mano del Residual / Esqueleto|220](Media/ConceptArt/Esqueleto-enemigo.JPG)

![Spritesheet activo del Residual / Esqueleto](Media/Avance_2026-11-04/Spritesheets/Esqueleto.png)

[Fuente editable](Media/Avance_2026-11-04/Spritesheets/Esqueleto.aseprite).

| Animación | Cuatro direcciones |
| --- | --- |
| Reposo | ![Esqueleto: Reposo](Media/Avance_2026-11-04/Animaciones/Esqueleto/Idle.gif) |
| Marcha | ![Esqueleto: Marcha](Media/Avance_2026-11-04/Animaciones/Esqueleto/Walk.gif) |
| Ataque | ![Esqueleto: Ataque](Media/Avance_2026-11-04/Animaciones/Esqueleto/Attack.gif) |
| Impacto recibido | ![Esqueleto: Impacto recibido](Media/Avance_2026-11-04/Animaciones/Esqueleto/Hit.gif) |
| Muerte | ![Esqueleto: Muerte](Media/Avance_2026-11-04/Animaciones/Esqueleto/Death.gif) |

## Interceptor: galgo de chatarra

Perseguidor rápido de la horda. Su silueta baja y la marcha veloz distinguen su persecución. El atlas tiene 152 fotogramas de `96 × 96`: reposo de seis, marcha de doce, ataque de seis, impacto de cuatro y muerte de diez por dirección.

![Boceto a mano del Galgo / Interceptor|240](Media/ConceptArt/Galgo_enemigo.JPG)

![Spritesheet activo del Interceptor](Media/Avance_2026-11-04/Spritesheets/Interceptor.png)

[Fuente editable](Media/Avance_2026-11-04/Spritesheets/Interceptor.aseprite).

| Animación | Cuatro direcciones |
| --- | --- |
| Reposo | ![Interceptor: Reposo](Media/Avance_2026-11-04/Animaciones/Interceptor/Idle.gif) |
| Marcha | ![Interceptor: Marcha](Media/Avance_2026-11-04/Animaciones/Interceptor/Walk.gif) |
| Ataque | ![Interceptor: Ataque](Media/Avance_2026-11-04/Animaciones/Interceptor/Attack.gif) |
| Impacto recibido | ![Interceptor: Impacto recibido](Media/Avance_2026-11-04/Animaciones/Interceptor/Hit.gif) |
| Muerte | ![Interceptor: Muerte](Media/Avance_2026-11-04/Animaciones/Interceptor/Death.gif) |

## Coloso: Wendigo

Enemigo pesado que abre corredores con su embestida. Su diseño parte de un boceto conceptual a lápiz con cráneo astado de ciervo, costillar expuesto, manto de pelaje y garras largas. El atlas de Wendigo tiene 200 fotogramas de `96 × 96`: reposo, marcha y preparación de ocho; embestida y recuperación de seis; impacto de cuatro y muerte de diez por dirección.

![Boceto a mano del Coloso / Wendigo|240](Media/ConceptArt/Wendigo.jpeg)

![Spritesheet activo del Coloso / Wendigo](Media/Avance_2026-11-04/Spritesheets/Wendigo.png)

[Fuente editable](Media/Avance_2026-11-04/Spritesheets/Wendigo.aseprite).

| Animación | Cuatro direcciones |
| --- | --- |
| Reposo | ![Wendigo: Reposo](Media/Avance_2026-11-04/Animaciones/Wendigo/Idle.gif) |
| Marcha | ![Wendigo: Marcha](Media/Avance_2026-11-04/Animaciones/Wendigo/Walk.gif) |
| Preparación de carga | ![Wendigo: Preparación de carga](Media/Avance_2026-11-04/Animaciones/Wendigo/Windup.gif) |
| Embestida | ![Wendigo: Embestida](Media/Avance_2026-11-04/Animaciones/Wendigo/Charge.gif) |
| Recuperación | ![Wendigo: Recuperación](Media/Avance_2026-11-04/Animaciones/Wendigo/Recovery.gif) |
| Impacto recibido | ![Wendigo: Impacto recibido](Media/Avance_2026-11-04/Animaciones/Wendigo/Hit.gif) |
| Muerte | ![Wendigo: Muerte](Media/Avance_2026-11-04/Animaciones/Wendigo/Death.gif) |

## Mortero: Campana

Artillería de la horda. Su diseño conceptual parte de una campana flotante con aletas emisoras de brillo verde en la corona, jaula interior y una manguera lateral por la que escurre el ácido. En el juego encadena flotación, carga del emisor, disparo y recuperación en un atlas de 112 fotogramas de `64 × 64` (flotación y carga de seis, disparo y recuperación de cuatro, y muerte de ocho por dirección).

![Boceto a mano del Mortero Campana|280](Media/ConceptArt/Campana_Mortero.JPG)

![Spritesheet activo del Mortero Campana](Media/Avance_2026-11-04/Spritesheets/Mortero.png)

[Fuente editable](Media/Avance_2026-11-04/Spritesheets/Mortero.aseprite).

| Animación | Cuatro direcciones |
| --- | --- |
| Flotación | ![Mortero: Flotación](Media/Avance_2026-11-04/Animaciones/Mortero/Float.gif) |
| Carga del emisor | ![Mortero: Carga del emisor](Media/Avance_2026-11-04/Animaciones/Mortero/Charge.gif) |
| Disparo | ![Mortero: Disparo](Media/Avance_2026-11-04/Animaciones/Mortero/Fire.gif) |
| Recuperación | ![Mortero: Recuperación](Media/Avance_2026-11-04/Animaciones/Mortero/Recover.gif) |
| Muerte | ![Mortero: Muerte](Media/Avance_2026-11-04/Animaciones/Mortero/Death.gif) |

## Polilla: Reactor

Enemigo explosivo de la horda que arma su espoleta a `2 m` del jugador. Su boceto conceptual combina un cuerpo esférico de horno o reactor con ranuras frontales, ojos compuestos y alas mecánicas. Su atlas tiene 62 fotogramas de `64 × 64`: vuelo de ocho y cebado de seis por dirección, más seis de estallido compartido.

![Boceto a mano de la Polilla Reactor|280](Media/ConceptArt/Polilla.JPG)

![Spritesheet activo de la Polilla Reactor](Media/Avance_2026-11-04/Spritesheets/PolillaReactor.png)

[Fuente editable](Media/Avance_2026-11-04/Spritesheets/PolillaReactor.aseprite).

| Animación | Cuatro direcciones |
| --- | --- |
| Vuelo | ![PolillaReactor: Vuelo](Media/Avance_2026-11-04/Animaciones/PolillaReactor/Fly.gif) |
| Cebado | ![PolillaReactor: Cebado](Media/Avance_2026-11-04/Animaciones/PolillaReactor/Prime.gif) |
| Estallido compartido | ![PolillaReactor: Estallido compartido](Media/Avance_2026-11-04/Animaciones/PolillaReactor/Burst.gif) |

## Caballero Fantasma

Su boceto conceptual define una armadura pesada con hombreras de púas, cabeza espectral envuelta en llamas oscuras y espadón de barrido. En Unity cuenta con sprite integrado en cuatro direcciones con normal map y emisión (224 fotogramas de `96 × 96`). La preparación sigue los `0,8 s` del barrido, y el ataque junto con la recuperación se enlazan durante los `0,8 s` posteriores.

![Boceto a mano del Caballero Fantasma|280](Media/ConceptArt/Caballero_Fantasma.JPG)

![Atlas de Caballero Fantasma](Media/Avance_2026-11-05/Spritesheets/CaballeroFantasma.png)

[Fuente editable](Media/Avance_2026-11-05/Spritesheets/CaballeroFantasma.aseprite).

| Animación | Cuatro direcciones |
| --- | --- |
| Reposo | ![Caballero Fantasma: Reposo](Media/Avance_2026-11-05/Animaciones/CaballeroFantasma/Idle.gif) |
| Marcha | ![Caballero Fantasma: Marcha](Media/Avance_2026-11-05/Animaciones/CaballeroFantasma/Walk.gif) |
| Preparación | ![Caballero Fantasma: Preparación](Media/Avance_2026-11-05/Animaciones/CaballeroFantasma/Windup.gif) |
| Ataque | ![Caballero Fantasma: Ataque](Media/Avance_2026-11-05/Animaciones/CaballeroFantasma/Attack.gif) |
| Recuperación | ![Caballero Fantasma: Recuperación](Media/Avance_2026-11-05/Animaciones/CaballeroFantasma/Recovery.gif) |
| Impacto | ![Caballero Fantasma: Impacto](Media/Avance_2026-11-05/Animaciones/CaballeroFantasma/Hit.gif) |
| Muerte | ![Caballero Fantasma: Muerte](Media/Avance_2026-11-05/Animaciones/CaballeroFantasma/Death.gif) |

![Ciclo de combate integrado, captura de Unity del 5 de noviembre](Media/Avance_2026-11-05/Enemigos/CaballeroFantasma_Integrado.gif)

## Centinela de Cobre: gólem

Su boceto conceptual define un autómata pesado con bobinas eléctricas en los hombros y compuertas pectorales que se abren para disparar. En Unity cuenta con sprite integrado en cuatro direcciones con normal map y emisión (288 fotogramas de `96 × 96`). Apertura de `0,3 s` y preparación de `0,7 s` preceden al disparo; disparo de `0,4 s`, vulnerabilidad de `1,3 s` y cierre de `0,3 s` ocupan los `2,0 s` de recuperación.

![Boceto a mano del Centinela de Cobre|280](Media/ConceptArt/Golem_Cobre.JPG)

![Atlas de Centinela de Cobre: gólem](Media/Avance_2026-11-05/Spritesheets/CentinelaCobre.png)

[Fuente editable](Media/Avance_2026-11-05/Spritesheets/CentinelaCobre.aseprite).

| Animación | Cuatro direcciones |
| --- | --- |
| Reposo | ![Centinela de Cobre: gólem: Reposo](Media/Avance_2026-11-05/Animaciones/CentinelaCobre/Idle.gif) |
| Marcha | ![Centinela de Cobre: gólem: Marcha](Media/Avance_2026-11-05/Animaciones/CentinelaCobre/Walk.gif) |
| Apertura | ![Centinela de Cobre: gólem: Apertura](Media/Avance_2026-11-05/Animaciones/CentinelaCobre/Open.gif) |
| Preparación | ![Centinela de Cobre: gólem: Preparación](Media/Avance_2026-11-05/Animaciones/CentinelaCobre/Windup.gif) |
| Disparo | ![Centinela de Cobre: gólem: Disparo](Media/Avance_2026-11-05/Animaciones/CentinelaCobre/Fire.gif) |
| Vulnerabilidad | ![Centinela de Cobre: gólem: Vulnerabilidad](Media/Avance_2026-11-05/Animaciones/CentinelaCobre/Vulnerable.gif) |
| Cierre | ![Centinela de Cobre: gólem: Cierre](Media/Avance_2026-11-05/Animaciones/CentinelaCobre/Close.gif) |
| Impacto | ![Centinela de Cobre: gólem: Impacto](Media/Avance_2026-11-05/Animaciones/CentinelaCobre/Hit.gif) |
| Muerte | ![Centinela de Cobre: gólem: Muerte](Media/Avance_2026-11-05/Animaciones/CentinelaCobre/Death.gif) |

![Ciclo de combate integrado, captura de Unity del 5 de noviembre](Media/Avance_2026-11-05/Enemigos/CentinelaCobre_Integrado.gif)

## Gólgora: MataColeccionista

Jefe final de la expedición. Su diseño parte de un boceto conceptual a lápiz con cráneo cornudo, corona en llamas, hombreras y antebrazos flameantes y cadenas alrededor del torso y el faldón, trasladado al juego mediante un modelo 3D animado (`MataColeccionista.fbx`, `Martillo.fbx`, texturas `0_base.png` y `0_emission.png`) con efectos de magma propios.

![Boceto conceptual a mano de Gólgora / MataColeccionista|280](Media/ConceptArt/Golgora_guardian.jpeg)

### Ataques dentro de la arena

| Ataque en Unity | Registro |
| --- | --- |
| Martillazo | ![Gólgora: martillazo](Media/Avance_2026-11-04/Enemigos/Golgora_Martillo.gif) |
| Barrido | ![Gólgora: barrido](Media/Avance_2026-11-04/Enemigos/Golgora_Barrido.gif) |
| Bombardeo | ![Gólgora: bombardeo](Media/Avance_2026-11-04/Enemigos/Golgora_Bombardeo.gif) |
| Cerco y salto central | ![Gólgora: salto y cerco de magma](Media/Avance_2026-11-04/Enemigos/Golgora_Cerco.gif) |
| Fisura | ![Gólgora: fisura de magma](Media/Avance_2026-11-04/Enemigos/Golgora_Fisura.gif) |
| Lluvia de segunda fase | ![Gólgora: lluvia de escoria en segunda fase](Media/Avance_2026-11-04/Enemigos/Golgora_Lluvia.gif) |

Los daños, tiempos, fases, refuerzos y reglas de cierre están en [[03 - Enemigos y NPC]]. Los efectos de ataques y mejoras se reúnen en [[06 - Registro visual]].

## Entidades de la colección

Las quince entidades coleccionables se presentan en el mundo y en las vitrinas de la Bóveda mediante sprites 2D en pixel art exportados a doble escala sin interpolación. Sus historias de origen se documentan en [[03 - Personajes]] y sus rarezas, fuentes y poderes en [[04 - Objetos y Coleccionables]].

| Entidad            | Rareza     | Previsualización del sprite                                                                                                                          | Fuente editable                                                                                     |
| ------------------ | ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| Yanikua            | Mítico     | ![Sprite de Yanikua\|140](Media/Coleccionables/Yanikua.png)                                                                                       | [Yanikua.aseprite](Sprites/Yanikua.aseprite)                                                     |
| Lisa               | Legendario | ![Sprite de Lisa\|150](Media/Coleccionables/Lisa.png)                                                                                             | [Lisa.aseprite](Sprites/Lisa.aseprite)                                                           |
| Sirena             | Legendario | ![Sprite de Sirena](Media/Coleccionables/Sirena_adelante_atras.png)                                                                               | [Sirena_adelante_atras.aseprite](Sprites/Sirena_adelante_atras.aseprite)                         |
| Hada               | Épico      | ![Sprite de Hada](Media/Coleccionables/Hada_atras_adelante.png)                                                                                   | [Hada_atras_adelante.aseprite](Sprites/Hada_atras_adelante.aseprite)                             |
| Murciélago Cristal | Épico      | ![Sprite de Murciélago Cristal](Media/Coleccionables/Murcielagocristal_atras_adelante.png)                                                        | [Murcielagocristal_atras_adelante.aseprite](Sprites/Murcielagocristal_atras_adelante.aseprite)   |
| Shireen            | Especial   | ![Sprite de Shireen\|220](Media/Coleccionables/Shireen.png)                                                                                       | [Shireen.aseprite](Sprites/Shireen.aseprite)                                                     |
| Soldier            | Especial   | ![Sprite de Soldier](Media/Coleccionables/Soldier_zombie-Sheet.png)                                                                               | [Soldier_zombie-Sheet.aseprite](Sprites/Soldier_zombie-Sheet.aseprite)                           |
| Conde Alucard      | Especial   | ![Sprite de Conde Alucard](Media/Coleccionables/Conde_Alucard.png)                                                                                | [Conde_Alucard.aseprite](Sprites/Conde_Alucard.aseprite)                                         |
| Mago               | Raro       | ![Sprite de Mago](Media/Coleccionables/Mago.png)                                                                                                  | [Mago.aseprite](Sprites/Mago.aseprite)                                                           |
| Demonio            | Raro       | ![Sprite de Demonio (frente)\|240](Media/Coleccionables/Demonio.png)<br>![Sprite de Demonio (espalda)\|240](Media/Coleccionables/Demonio2.png) | [Demonio.aseprite](Sprites/Demonio.aseprite) · [Demonio2.aseprite](Sprites/Demonio2.aseprite) |
| Joel               | Raro       | ![Sprite de Joel](Media/Coleccionables/Jov4.png)                                                                                                  | [Jov4.aseprite](Sprites/Jov4.aseprite)                                                           |
| Chico Emo          | Raro       | ![Sprite de Chico Emo\|130](Media/Coleccionables/Chico_Emo.png)                                                                                   | [Chico_Emo.aseprite](Sprites/Chico_Emo.aseprite)                                                 |
| Lobito             | Común      | ![Sprite de Lobito\|220](Media/Coleccionables/Lobito.png)                                                                                         | [Lobito.aseprite](Sprites/Lobito.aseprite)                                                       |
| Basket             | Común      | ![Sprite de Basket](Media/Coleccionables/basket.png)                                                                                              | [basket.aseprite](Sprites/basket.aseprite)                                                       |
| Reaper             | Común      | ![Sprite de Reaper](Media/Coleccionables/Reaper.png)                                                                                              | [Reaper.aseprite](Sprites/Reaper.aseprite)                                                       |

### Bocetos conceptuales a mano

Dibujos a lápiz realizados durante el diseño visual de las entidades antes de su paso a pixel art, resguardados en `Media/ConceptArt/`.

| Entidad | Boceto a mano |
| --- | --- |
| Yanikua | ![Boceto de Yanikua\|140](Media/ConceptArt/Yanikua.jpeg) |
| Lisa | ![Boceto de Lisa\|140](Media/ConceptArt/Lisa.jpeg) |
| Sirena | ![Boceto de Sirena\|140](Media/ConceptArt/Sirena.JPG) |
| Hada | ![Boceto de Hada\|140](Media/ConceptArt/Hada_Costura.jpeg) |
| Murciélago Cristal | ![Boceto de Murciélago Cristal\|140](Media/ConceptArt/Murcielago_Cristal.jpeg) |
| Shireen | ![Boceto de Shireen\|140](Media/ConceptArt/Shireen.jpeg) |
| Soldier | ![Boceto de Soldier\|140](Media/ConceptArt/Soldier.JPG) |
| Conde Alucard | ![Boceto de Conde Alucard\|140](Media/ConceptArt/Conde_Alucard.jpeg) |
| Mago | ![Boceto de Mago\|140](Media/ConceptArt/Mago.JPG) |
| Demonio | ![Boceto de Demonio\|140](Media/ConceptArt/Demonio.JPG) |
| Joel | ![Boceto de Joel\|140](Media/ConceptArt/Joel_jov4.JPG) |
| Chico Emo | ![Boceto de Chico Emo\|140](Media/ConceptArt/Chico_Emo.JPG) |
| Lobito | ![Boceto de Lobito\|140](Media/ConceptArt/Lobito.jpeg) |
| Basket | ![Boceto de Basket\|140](Media/ConceptArt/Basket.JPG) |
| Reaper | ![Boceto de Reaper\|140](Media/ConceptArt/Reaper.JPG) |

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Entornos

El Vertedero ocupa `200 × 200 m` de relieve, restos industriales, rocas y árboles. Las colisiones participan en las rutas del jugador y de los enemigos. El acceso a la bóveda se encuentra al norte (`x = 0, z = 78`) y desciende hasta el portal.

El centro contiene la caldera de Gólgora (`22 m` de radio), con perímetro que se cierra durante el encuentro, avisos para refuerzos y materiales que responden a su activación. Los ataques aportan ondas, impactos, meteoros y zonas de magma. Las tres zonas de lava fija del mapa están desactivadas en el montaje actual.

La iluminación exterior utiliza un perfil compartido de luz, atmósfera y enfoque. La niebla y los haces acompañan los puntos luminosos sin sustituir los contornos que anuncian daño.

![Arte conceptual a mano de El Vertedero|520](Media/ConceptArt/Mapa_Arte.JPG)

*Boceto conceptual del exterior: el perímetro reúne fragmentos de distintos mundos colapsados (murallas de piedra, raíces gigantes, cúmulos de armas y escudos, grietas en el terreno y el vórtice del portal al norte).*

## La Bóveda (Almacén del Coleccionista)

La bóveda modular ocupa una sala separada dentro de la misma escena, articulada en dos niveles mediante rampas laterales para alojar sus **18 pedestales físicos** (equivalentes al límite de **18 unidades** de la colección):

- **Balcón superior (puestos 1 a 3):** contiene los tres pedestales de equipo activo con rótulos 3D flotantes **`EQUIPO 1`**, **`EQUIPO 2`** y **`EQUIPO 3`**.
- **Planta inferior (puestos 4 a 18 y pared trasera):** contiene los **15 pedestales de exposición/reserva** (`slot 3..17`), la **máquina de fusión** desplazada a la pared trasera (`(189, 0, 28,88)`) y el portal de salida hacia la arena, junto a paredes de basalto pixel art, fracturas de roca con relieve e iluminación acotada mediante `WarehouseLightScope`.

La cámara interior emplea una inclinación de `32°` y campo de visión de `65°`, con límites de seguimiento ajustados al recinto, mientras el HUD superpone las tarjetas direccionales (`VaultDirections`), el acceso al Índice de Colección (`ÍNDICE · [I]`) y el panel de consejos (`VaultTips`).

![Arte conceptual a mano de la Bóveda|420](Media/ConceptArt/Almacen_Arte.jpeg)

*Boceto conceptual del almacén: cápsulas cilíndricas de contención para las entidades, consola y cámara central de la máquina de fusión, escaleras de acceso y el portal circular en el suelo.*

![Interior y preparación](Media/Interfaz/Boveda_almacen.png)
*Captura de referencia de la Bóveda (`734e2a5`) mostrando `VaultDirections`, `ÍNDICE · [I]`, `EQUIPO 1..3` y `VaultTips`; en `0.7.0`, integra `18` cápsulas, máquina en pared trasera `(189, 0, 28,88)` y acabado de basalto pixel art y relieve de roca.*

La máquina de fusión combina cámara de resultado, persiana móvil, aura de partículas y efectos de humo al abrirse tras completar su cuenta atrás de `30 s` (`00:30`).

## Galería de bocetos conceptuales de elementos del juego

Diseños conceptuales dibujados a mano durante la preproducción de los elementos de entorno, mecánicas e interfaz de *EchoScrap*, resguardados en `Media/ConceptArt/Elementos/`.

| Elemento | Boceto conceptual a mano |
| --- | --- |
| **El Vertedero (Arena exterior)** | ![Boceto conceptual del Vertedero\|280](Media/ConceptArt/Elementos/Mapa_Arte.JPG) |
| **La Bóveda (Almacén)** | ![Boceto conceptual de la Bóveda\|280](Media/ConceptArt/Elementos/Almacen_Arte.jpeg) |
| **Portal Dimensional** | ![Boceto conceptual del Portal\|260](Media/ConceptArt/Elementos/Portal_dibujo.JPG) |
| **Máquina de Fusión** | ![Boceto conceptual de la Máquina de Fusión\|260](Media/ConceptArt/Elementos/maquina_fusion_dibujo.JPG) |
| **Libro del Bestiario (Índice de Colección)** | ![Boceto conceptual del Bestiario\|260](Media/ConceptArt/Elementos/Bestiario_dibujo.JPG) |
| **Menú de Pausa y Ajustes** | ![Boceto conceptual del Menú de Pausa\|260](Media/ConceptArt/Elementos/Pausa_dibujo.JPG) |
| **Elementos del entorno e interfaz (Lámina 1)** | ![Bocetos de elementos 1\|260](Media/ConceptArt/Elementos/Elementos_dibujos.JPG) |
| **Elementos del entorno e interfaz (Lámina 2)** | ![Bocetos de elementos 2\|260](Media/ConceptArt/Elementos/Elementos_dibujos2.JPG) |
| **Elementos del entorno e interfaz (Lámina 3)** | ![Bocetos de elementos 3\|260](Media/ConceptArt/Elementos/Elementos_dibujos3.JPG) |
| **Elementos del entorno e interfaz (Lámina 4)** | ![Bocetos de elementos 4\|260](Media/ConceptArt/Elementos/Elementos_dibujos4.JPG) |

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Interfaz visual

## Tipografía unificada (`Pixelify Sans`)

Toda la interfaz del juego utiliza de manera unificada la familia tipográfica **`Pixelify Sans`** (`PixelifySans-Regular.ttf` y `PixelifySans-SemiBold.ttf`, configurada en `Typography.asset`, `UpgradeCardSkin.asset` y los generadores de interfaz en código), abarcando menús, HUD de combate, cartas de mejora, paneles de la bóveda, Índice de Colección y números flotantes de daño.

## Menú inicial y selección de ronda

El menú inicial utiliza un portal animado, logotipos integrados (`Presentation/Logo` y `Presentation/Title`), entidades de la colección en órbita y botones para **`CONTINUAR`**, **`NUEVA PARTIDA`**, **`CONFIGURACIÓN`** y **`SALIR`**. Al pulsar **`CONTINUAR`** o **`NUEVA PARTIDA`** se abre el selector visual de **3 ranuras de guardado** (`ARCHIVO 1..3`) con el alias, contador `?/18`, tiempo total acumulado y estado (`Vacío`, `En la bóveda`, `Expedición en curso`, `Expedición dañada` o `Archivo dañado`). Al iniciar en **`NUEVA PARTIDA · ARCHIVO N`**, el modal incluye el campo **`NOMBRE DEL JUGADOR (RANKING)...`** (máximo `20` caracteres visibles, reservado en línea mediante `GamertagRegistry` y sincronizado con `EventLeaderboardSync`, o vacío para práctica). La transición al mundo presenta una pantalla de carga con animación del Coleccionista.

![Menú inicial (captura de avance)](Media/Avance_2026-11-04/Interfaz/MenuInicial.png)

Al acercarse al portal de la bóveda se abre la selección de ronda (**`ELIGE LA RONDA`** — *"Elige una ronda para comenzar."*) con las opciones **`RONDA DEMO`** (`3:00`), **`RONDA COMPLETA`** (`8:00`) o **`SALIR`** (sin campo de nombre, pues el alias de ranking se define al crear la colección). Su confirmación reproduce una animación de entrada y un rugido antes del despliegue en la arena.

![Selección de ronda (captura histórica de avance)](Media/Avance_2026-11-04/Interfaz/SeleccionRonda.png)

## Interfaz de la bóveda, Índice de Colección y fusión

Dentro de la bóveda, el HUD y el escenario proporcionan guía visual constante:

- **Indicadores direccionales superiores (`VaultDirections`):** dos tarjetas en la franja superior con flechas dinámicas (`▲`, `▼`, `◄`, `►`, `●`) que apuntan hacia la **Máquina de Fusión** (*"MÁQUINA DE FUSIÓN · Combina 4 copias de varias rarezas"*, *"BAJA A FUSIÓN"* o *"E · MÁQUINA DE FUSIÓN"*) y hacia los **Puestos de Equipo** (*"SUBE AL BALCÓN · Puestos de equipo 1, 2 y 3"*, *"EQUIPAR ENTIDADES"* o *"PUESTO 1..3 · EQUIPAR/EDITAR"*).
- **Rótulos 3D y avisos de pedestal:** los tres puestos del balcón (`slot 0..2`) muestran en el mundo **`EQUIPO 1`**, **`EQUIPO 2`** y **`EQUIPO 3`** (o `· RESERVA` si están vacíos y exhiben visualmente una copia de reserva), con aviso **`E · EQUIPAR`** (si está vacío) o **`E · EDITAR`** (si está ocupado; **`T`** o botón norte en mando para desequipar rápido), mientras que los puestos ocupados de la planta baja (`slot 3..17`) muestran **`E · LIBERAR`** (que abre el modal **`LIBERAR UNIDAD`** con botones `CANCELAR` y `LIBERAR`).
- **Panel de consejos (`VaultTips`):** tarjeta en la esquina inferior izquierda con la etiqueta **`CONSEJO`** que rota automáticamente cada `8 s` entre *"Equipa coleccionables antes de ir a la arena."*, *"Abre el índice para consultar tus descubrimientos."* y *"Combina cuatro coleccionables en la máquina de fusión."* (o prioriza *"Pulsa E frente a un puesto para equipar tu primer coleccionable."* cuando el jugador tiene copias pero aún no ha equipado ninguna).
- **Índice de Colección (`VaultCollectionIndex.cs`, tecla `[I]` / `Select`):** accesible mediante `I`, `Select` o el botón superior izquierdo **`ÍNDICE · [I]`**. Presenta el encabezado **`ÍNDICE DE COLECCIÓN`** con el contador **`{descubiertos} / 15 DESCUBIERTOS · {en posesión} / 18 UNIDADES`**, dos pestañas (**`POR RAREZAS`**, con el filtro **`TODAS`** más las 6 categorías de rareza y siluetas negras **`???`** / `NO ENCONTRADO` / `SILUETA BLOQUEADA` para las no descubiertas, y **`EN TU BÓVEDA`**, que distingue `EN EQUIPO ACTIVO` y `EN BÓVEDA`), un inspector lateral derecho con el poder de compañero y el botón **`CERRAR · [I]`**.
- **Selector de acompañantes (`CompanionView`):** al pulsar `E` (o botón oeste en mando) en los puestos 1 a 3 del balcón se abre `PUESTO DE EQUIPO 1/2/3 · ELIGE UNA ENTIDAD DESCUBIERTA PARA ESTE PUESTO`, con retratos enmarcados por rareza, la nota de poderes acumulables (*"Los poderes se acumulan por copia equipada."*) y barra inferior con tres botones: **`QUITAR DEL PUESTO`**, **`LIBERAR COPIA`** y **`CERRAR · E / ESC`**.
- **Estación de fusión (`FusionStationUI`):** ventana principal con **4 ranuras en fila horizontal** (`Espacio_0` a `Espacio_3`), temporizador fijo **`TIEMPO DE FUSIÓN  00:30`**, botón superior derecho **`X`**, panel **`POSIBLES RESULTADOS`** actualizado en vivo tanto para misma rareza como para rarezas mixtas (`60%`, `40%`, `1%` en 4 Épicas), botones **`VACIAR`** y **`CONFIRMAR`**, selector de copias (`EN EQUIPO 1/2/3`, `QUITAR INGREDIENTE`, `VOLVER`) y pantalla de resultado con el botón único **`COLECCIONAR`** (o aviso **`INVENTARIO LLENO · LIBERA ESPACIO`** si ya se tienen `18/18` unidades).

![Preparación de entidades y selector de equipo](Media/Interfaz/Equipar_coleccionable.png)
*Selector de acompañantes de la versión final en Unity, con la barra inferior integrada: `QUITAR DEL PUESTO`, `LIBERAR COPIA` y `CERRAR · E / ESC`.*

![Modal de liberación de unidad](Media/Interfaz/Liberar_Coleccionable.png)
*Modal `LIBERAR UNIDAD` en la Bóveda con opciones `CANCELAR` (`Esc`) y `LIBERAR` (`E` / `Enter` / `Espacio`).*

![Estación de fusión](Media/Interfaz/Maquina_Fusion.png)
*Ventana de la Máquina de Fusión con sus cuatro ranuras horizontales de ingredientes y desglose de posibles resultados.*

![Resultado de fusión](Media/Interfaz/Resultado_Fusion.png)
*Pantalla de resultado tras la cuenta atrás de 30 segundos, mostrando la entidad obtenida y el botón único `COLECCIONAR`.*

## HUD de combate, consulta rápida (`Q` / `Select`) y cartas

El HUD de combate muestra barra de salud, barra de XP y nivel, reloj de incursión, contador de bajas, estado del dash, cuadrícula de mejoras adquiridas (`9` columnas de `34 px` hasta `62` cartas, que se eleva `+100 px` si el ancho de lienzo es menor de `1530 px` para no invadir la barra de XP), panel de **22 filas de estadísticas** (`PlayerStatsReadout`: incluye `Distancia dash` y `Área efectos`), los tres retratos de acompañantes equipados (`Retrato_0..2`, sin insignia numérica superpuesta; cuando `Atributos de coleccionables` está en `SÍ` o se mantiene `Q` / `Select`, se muestran debajo las líneas de poder con multiplicador `×N` y valores totales acumulados calculados por `CompanionPowerRuntime.DescribeHudPower`) y un minimapa circular con niebla de exploración. Gólgora dispone de su propia barra superior de salud e indicador direccional fuera de pantalla.

![HUD activo durante el combate en la arena](Media/Interfaz/Arena_Hud.png)

Mantener pulsada la tecla **`Q`** en teclado o el botón **`Select`** en mando (`CombatHud.IsHudDetailHeld`) despliega temporalmente las **estadísticas completas** (`COMPLETAS`) y las **descripciones de los poderes de los acompañantes equipados**, aunque estén reducidas u ocultas en la configuración.

![Consulta rápida de detalles del HUD en combate manteniendo Q](Media/Interfaz/Q_estadisticas.png)
*Despliegue dinámico al mantener `Q` / `Select` en combate: panel lateral de estadísticas completas y textos de poderes de los 3 acompañantes equipados bajo los retratos.*

Las cartas de subida de nivel presentan cinco marcos lit 9-sliced según rareza (`Común`, `Rara`, `Épica`, `Legendaria` y **`Mítica`** mediante `Frame_Mythic.png` / `Frame_Mythic.mat`), `10` iconos en escala de grises compartidos, nombre, nivel actual/máximo, tipo de daño y progresión de valores, además del botón de reintento cuando se dispone de *Sintonizador*.

![Cartas: captura de Unity del 27 de octubre](Media/Avance_2026-10-27/Interfaz/Cartas.gif)

## Configuración, Menú de Pausa en 3 pestañas y resultados

Tanto **`CONFIGURACIÓN`** en el menú inicial como **`PAUSA`** (`Esc` / `Start` durante la partida o en la bóveda) comparten el panel unificado **`MenuSettingsView.cs`**, estructurado en **3 pestañas** más los botones inferiores de acción:

1. **`General`:** deslizadores **`Movimiento de cámara`** (`0%` a `100%`, `ShakeIntensity`) y **`Efectos decorativos`** (`0%` a `100%`, `DecorativeIntensity`), la casilla **`Efectos reducidos`** (`ReducedEffectsMode`) en el menú principal o el selector **`Límite de FPS`** (**`30`**, **`60`**, **`120`**, persistido en `EchoScrap.Graphics.FrameRateLimit`) en el menú de pausa, y el selector **`Pantalla`** (**`COMPLETA`** / **`VENTANA`**, persistido en `EchoScrap.Graphics.FullScreen`, iniciando en pantalla completa sin bordes por defecto).
2. **`Interfaz`:** selector **`Estadísticas generales`** (**`COMPLETAS`**, **`REDUCIDAS`**, **`OCULTAS`**) y selector **`Atributos de coleccionables`** (**`SÍ`** / **`NO`**, `ShowCompanionPowers`).
3. **`Sonido`:** deslizadores **`Volumen general`** (`0%` a `100%`, guardado en `EchoScrap.Audio.MasterVolume`) y **`Música`** (`0%` a `100%`, guardado en `EchoScrap.Audio.MusicVolume`).

En pausa, los botones inferiores permiten **`CONTINUAR`** y **`VOLVER AL MENÚ`** (desde la bóveda) o **`VOLVER A LA BÓVEDA`** (durante una incursión en la arena).

![Menú de Pausa en tres pestañas](Media/Interfaz/Menu_Pausa.png)
*Menú de Pausa (`Esc` / `Start`) en la versión final: pestañas `General`, `Interfaz` y `Sonido` con deslizadores y botones de acción.*

La pantalla de resultados muestra victoria o derrota, tiempo sobrevivido, nivel, bajas y XP, sincroniza todos los intentos válidos con el ranking de Firebase (`EventLeaderboardSync`, manteniendo la mejor puntuación por gamertag) y ofrece el botón **`VOLVER A LA BÓVEDA`**.

![Pantalla de fin de partida: Victoria](Media/Interfaz/Victoria.png)

![Pantalla de fin de partida: Derrota](Media/Interfaz/Derrota.png)

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Audio y música

El juego ya tiene música y efectos integrados. El área sonora acompaña el lugar, los ataques y las acciones del Coleccionista.

| Contexto | Audio presente |
| --- | --- |
| Menú principal | Pista ambiental introductoria con sintetizadores y una dinámica moderada y constante ([Menu.mp3](Media/Audio/Menu.mp3)). |
| Bóveda | Pista atmosférica propia, orientada a la calma con un matiz de suspenso previo a la incursión; se detiene al salir hacia el combate o al menú ([Boveda_Almacen.mp3](Media/Audio/Boveda_Almacen.mp3)). |
| Arena | Pista de combate en bucle ([Partida.mp3](Media/Audio/Partida.mp3)). En el proyecto de Unity el recurso conserva el nombre de prueba `Arena_combat_Test`. |
| Gólgora | Pista de jefe ([Jefe.mp3](Media/Audio/Jefe.mp3)) con transición desde la arena y reducción de mezcla (*ducking*) durante el rugido. El recurso sigue identificado como prueba en el motor. |
| Linterna | Inicio de ráfaga, tres variantes de pulso, cuarto pulso, descanso, impacto normal y crítico, activación y bucle solar. |
| Experiencia | Sonido de recogida con variación de tono. |
| Transiciones | Entrada del Coleccionista, llegada por portal y rugido al elegir ronda. |
| Fusión | Sonido de apertura de la cámara al revelar el resultado. |
| Jefe | Rugido, pasos, ataques e impactos mediante sus componentes de audio. |

La música de arena y jefe continúa durante la selección de cartas y la pausa manual. La pérdida de foco de la ventana pausa su reproducción. La bóveda tiene su propia mezcla y las transiciones evitan que siga sonando fuera de su contexto.

La pestaña **`Sonido`** del panel compartido `MenuSettingsView` (disponible tanto en **`CONFIGURACIÓN`** del menú inicial como en **`PAUSA`**) incorpora dos controles independientes persistidos en `PlayerPrefs`: **Volumen general** (`EchoScrap.Audio.MasterVolume`, `0.85` por defecto), que actúa sobre `AudioListener.volume`, y **Música** (`EchoScrap.Audio.MusicVolume`, `0.85` por defecto), aplicado sobre las pistas musicales por **`CombatMusicPlayer`** (`SetMusicVolume`) y **`MenuMusic`** (`RefreshVolume`).

## Música durante el juego

### Menú principal

- **Composición y timbre:** tema sintético con arreglos lentos en tonalidad menor y *pads* oscuros que aportan una atmósfera sombría, incorporando un sintetizador con sutil desafinación (*pitch drift*) para emular el comportamiento orgánico de instrumentos analógicos.
- **Estructura y loop:** bucle continuo de baja actividad rítmica diseñado para evitar fatiga auditiva durante estancias prolongadas en los menús.
- **Género, carácter y métrica:** electrónica de carácter melancólico a `75 BPM` en compás de `4/4`, manteniendo un pulso constante y moderado.
- **Muestra de audio:** [Escuchar pista de Menú principal (Menu.mp3)](Media/Audio/Menu.mp3)

![Pista de Menú principal](Media/Audio/Menu.mp3)

### Bóveda (Almacén)

- **Composición y timbre:** arreglos pausados en tonalidad menor con reverberación convolutiva amplia y fría que emula la acústica de un hangar o contenedor subterráneo, acompañados de ecos incidentales, *pads* oscuros, bombo, sintetizadores y violín para acentuar el misterio del recinto.
- **Estructura y loop:** bucle continuo con base rítmica marcada que acompaña la preparación y anticipa la entrada hacia la arena.
- **Género, carácter y métrica:** electrónica con sintetizador y cuerdas a `120 BPM` en compás de `4/4`, con un pulso constante que equilibra calma e intriga antes de la expedición.
- **Muestra de audio:** [Escuchar pista de la Bóveda (Boveda_Almacen.mp3)](Media/Audio/Boveda_Almacen.mp3)

![Pista de la Bóveda](Media/Audio/Boveda_Almacen.mp3)

### Arena (Partida)

- **Composición y timbre:** tema en tonalidad mayor que inicia con arreglos contenidos y gana densidad rítmica conforme avanza la incursión para sostener la adrenalina del combate, empleando sintetizadores, bombo para marcar el pulso, bajo, platillos, tarola y piano.
- **Estructura y loop:** bucle continuo de alta actividad rítmica con pulso constante y enérgico.
- **Género, carácter y métrica:** electrónica con sintetizador de carácter enérgico a `120 BPM` en compás de `4/4`.
- **Muestra de audio:** [Escuchar pista de Arena (Partida.mp3)](Media/Audio/Partida.mp3)

![Pista de Arena](Media/Audio/Partida.mp3)

### Jefe final (Gólgora)

- **Composición y timbre:** arreglos de alta intensidad desde el primer compás con un enfoque amenazador que sostiene la tensión durante todo el enfrentamiento en la caldera, utilizando dos capas de sintetizadores, tarola y bombo para marcar el pulso.
- **Estructura y loop:** bucle continuo de alta actividad rítmica y carácter adrenalínico.
- **Género, carácter y métrica:** electrónica pura con sintetizador de timbre brillante, digital y agresivo a `120 BPM` en compás de `4/4`.
- **Muestra de audio:** [Escuchar pista del Jefe final (Jefe.mp3)](Media/Audio/Jefe.mp3)

![Pista del Jefe final](Media/Audio/Jefe.mp3)

## Efectos especiales y diseño sonoro (SFX)

Los efectos de sonido puntuales refuerzan la lectura de ataques enemigos, acciones del Coleccionista, recolección de recursos y estados de interfaz. En `Media/Audio/SFX/` se conservan las quince muestras exportadas del estudio (`WAV` estéreo, `24 bits`, `48 kHz`), y en `Media/Audio/Unity/` ([`Flashlight/`](Media/Audio/Unity/Flashlight/Sources.txt), `MataColeccionista/` y `Sistema/`) se respaldan íntegramente los clips integrados en Unity (`Resources/Audio/` y `MataColeccionista/Audio/`) junto con la licencia original `Sources.txt` (CC0), conectados en código mediante `PlayerDashPresentation`, `PlayerHitAudio`, `LevelUpAudio`, `UpgradeChoiceAudio`, `SessionOutcomeAudio`, `GolgoraAudio`, `FlashlightAudio`, `MenuMusic` y `CombatMusicPlayer`:

| Efecto de sonido | Propósito en el videojuego | Descripción acústica | Muestra reproducible |
| --- | --- | --- | --- |
| **Dash del Coleccionista** | Se reproduce cuando el jugador activa el impulso evasivo (`Espacio` / `A` / `Cruz`) a `22 m/s`. | Barrido de aire corto y envolvente rápida (`0,51 s`) que transmite aceleración e invulnerabilidad momentánea. | [Dash.wav](Media/Audio/SFX/Dash.wav)<br>![SFX Dash](Media/Audio/SFX/Dash.wav) |
| **Golpe recibido** | Se reproduce cuando un ataque enemigo impacta al Coleccionista y descuenta salud. | Transiente seco y contundente (`0,51 s`) con refuerzo en frecuencias medias-bajas para alertar de inmediato la pérdida de vida. | [Golpe_Recibido.wav](Media/Audio/SFX/Golpe_Recibido.wav)<br>![SFX Golpe recibido](Media/Audio/SFX/Golpe_Recibido.wav) |
| **Recogida de coleccionable** | Se reproduce al capturar una entidad coleccionable en la arena. | Timbre brillante y ascendente (`0,71 s`) que confirma la obtención de una nueva copia para la bóveda. | [Coleccionable.wav](Media/Audio/SFX/Coleccionable.wav)<br>![SFX Coleccionable](Media/Audio/SFX/Coleccionable.wav) |
| **Curación con botiquín** | Se reproduce al recoger un botiquín soltado por los enemigos derrotados. | Pulso cálido y cristalino (`0,51 s`) que comunica restauración inmediata de vitalidad. | [Curacion_Botiquin.wav](Media/Audio/SFX/Curacion_Botiquin.wav)<br>![SFX Curación](Media/Audio/SFX/Curacion_Botiquin.wav) |
| **Subida de nivel** | Se reproduce cuando la barra de experiencia se llena durante la expedición. | Acorde sintético llamativo y expansivo (`0,72 s`) que anuncia la apertura del selector de mejoras. | [Subida_Nivel.wav](Media/Audio/SFX/Subida_Nivel.wav)<br>![SFX Subida de nivel](Media/Audio/SFX/Subida_Nivel.wav) |
| **Selección de carta** | Se reproduce al confirmar una carta de mejora en el selector (`UpgradeDraftController`). | Clic tonal breve y definido (`0,36 s`) que valida la elección del jugador antes de reanudar el combate. | [Seleccion_Carta.wav](Media/Audio/SFX/Seleccion_Carta.wav)<br>![SFX Selección de carta](Media/Audio/SFX/Seleccion_Carta.wav) |
| **Interceptor (Galgo de chatarra)** | Se reproduce cuando el galgo interceptor ejecuta su mordisco o embestida rápida a corta distancia. | Gruñido metálico y corte agresivo (`0,51 s`) que advierte la cercanía del perseguidor veloz de la horda. | [Galgo.wav](Media/Audio/SFX/Galgo.wav)<br>![SFX Galgo](Media/Audio/SFX/Galgo.wav) |
| **Mortero Campana** | Se reproduce cuando el Mortero Campana culmina su carga y dispara el proyectil parabólico de ácido. | Disparo neumático resonante (`0,86 s`) con timbre hueco de campana metálica que marca el lanzamiento a distancia. | [Mortero.wav](Media/Audio/SFX/Mortero.wav)<br>![SFX Mortero](Media/Audio/SFX/Mortero.wav) |
| **Polilla Reactor (aviso y explosión)** | Se reproduce cuando la Polilla Reactor entra en el radio de `2 m`, arma su espoleta y detona tras `0,9 s` (o `1,2 s` si muere antes). | Secuencia en dos fases (`1,99 s`): pitido acelerado de cebado que da margen para esquivar con *dash*, seguido del estallido térmico. | [Polilla_Aviso_Explosion.wav](Media/Audio/SFX/Polilla_Aviso_Explosion.wav)<br>![SFX Polilla](Media/Audio/SFX/Polilla_Aviso_Explosion.wav) |
| **Caballero Fantasma (barrido)** | Se reproduce cuando el élite Caballero Fantasma prepara (`0,8 s`) y usa el barrido de su espada. | Carga espectral seguida de un corte amplio y pesado (`1,99 s`) que delimita la ventana de peligro del ataque cuerpo a cuerpo. | [Barrido_Caballero.wav](Media/Audio/SFX/Barrido_Caballero.wav)<br>![SFX Barrido Caballero](Media/Audio/SFX/Barrido_Caballero.wav) |
| **Centinela de Cobre (apertura y disparo)** | Se reproduce cuando el gólem élite abre sus compuertas (`0,3 s`), prepara (`0,7 s`) y lanza su ataque de misiles. | Mecanismo metálico de apertura articulado con la descarga de artillería (`1,51 s`), señalizando además el inicio de su ventana vulnerable. | [Centinela_Apertura_Disparo.wav](Media/Audio/SFX/Centinela_Apertura_Disparo.wav)<br>![SFX Centinela](Media/Audio/SFX/Centinela_Apertura_Disparo.wav) |
| **Impacto de Gólgora** | Se reproduce cuando el jefe final Gólgora descarga sus golpes pesados contra el suelo de la caldera. | Impacto grave y contundente (`0,61 s`) con resonancia mineral que subraya la escala colosal del jefe. | [Impacto_Golgora.wav](Media/Audio/SFX/Impacto_Golgora.wav)<br>![SFX Impacto Gólgora](Media/Audio/SFX/Impacto_Golgora.wav) |
| **Señal de derrota** | Se reproduce cuando la salud del Coleccionista llega a cero (`PlayerHealth.IsDead`) y concluye la partida. | Secuencia tonal descendente en modo menor (`3,37 s`) con textura melancólica que rubrica la caída en combate antes de volver a la bóveda. | [Senal_Derrota.wav](Media/Audio/SFX/Senal_Derrota.wav)<br>![SFX Señal de derrota](Media/Audio/SFX/Senal_Derrota.wav) |
| **Señal de victoria** | Se reproduce al derrotar a Gólgora en la caldera central (`GolgoraActor.IsDead`), consolidando la victoria de la expedición. | Fanfarria en modo mayor con timbre brillante y festivo (`3,12 s`) que celebra la culminación exitosa de la ronda. | [Senal_Victoria.wav](Media/Audio/SFX/Senal_Victoria.wav)<br>![SFX Señal de victoria](Media/Audio/SFX/Senal_Victoria.wav) |
| **Fusión y resultado** | Se reproduce en la máquina de fusión al completarse la cuenta atrás de `30 s` (`00:30`) y desplegar la entidad resultante en pantalla. | Atmósfera musical envolvente (`3,28 s`) con acordes de campana y sintetizador que acompaña la recompensa y el balance final. | [Fusion_Resultado.wav](Media/Audio/SFX/Fusion_Resultado.wav)<br>![SFX Fusión y resultado](Media/Audio/SFX/Fusion_Resultado.wav) |

## Elaboración e instrumentación digital

Las pistas musicales y temas de ambientación fueron producidos en estación de trabajo de audio digital (DAW) combinando secuenciación MIDI, síntesis sustractiva/*hypersaw*, percusión acústica e instrumentación orquestal virtual.

### Instrumentación utilizada

- **Cuerdas y orquesta:** Chelo, Violín y Corno francés (mediante librerías orquestales virtuales como *Spitfire Audio – BBC Symphony Orchestra*).
- **Base rítmica y percusión:** Batería completa, bombo acústico/electrónico, tarola, platillos y campanas metálicas afinadas.
- **Armonía y bajos:** Teclado MIDI con sintetizadores virtuales (`JUNO-106`, `UJAM Usynth DETUNICORN`), bajo eléctrico/sintético y piano.

### Tonalidades y notas de referencia

La paleta armónica se estructura sobre centros tonales menores para transmitir misterio y peligro, alternando con modos mayores para momentos de recompensa y combate directo:

- **Tonalidades y acordes principales:** Do menor (`Cm`), Sol menor (`Gm`), Re menor (`Dm`) y La menor (`Am`), complementados por progresiones hacia Fa mayor (`F`) y Si bemol mayor (`Bb`).

![Sesión multipista de la banda sonora en el DAW|520](Media/Audio/Produccion_Multipista_Reaper.png)

*Estructura multipista y mezcla de canales de la banda sonora.*

![Edición MIDI e instrumentos digitales (JUNO-106)|520](Media/Audio/Edicion_MIDI_Juno106.png)

*Secuenciación melódica en piano roll con sintetizador virtual `JUNO-106`.*

![Configuración de sintetizador virtual UJAM Usynth DETUNICORN|520](Media/Audio/Sintetizador_UJAM_Detunicorn.png)

*Diseño sonoro y ajuste de desafinación (`Detune`) y frecuencias con el sintetizador `UJAM Usynth DETUNICORN`.*

![Instrumento virtual orquestal Spitfire Audio – BBC Symphony Orchestra|520](Media/Audio/Orquesta_Spitfire_BBC.png)

*Instrumento virtual `Spitfire Audio – BBC Symphony Orchestra` utilizado para incorporar las cuerdas y violines en la ambientación de la Bóveda.*

## Procedencia y autoría de recursos sonoros

Para dar cumplimiento estricto al criterio de evaluación de audio y licencias, la siguiente tabla distingue los recursos producidos directamente por el equipo de aquellos obtenidos o derivados de bibliotecas externas dentro del proyecto de Unity (`Assets/_Project/Resources/Audio/` y `Assets/_Project/Resources/MataColeccionista/Audio/`):

| Recurso / Contexto | Tipo | Origen y autoría | Licencia / Condiciones | Tratamiento técnico |
| --- | --- | --- | --- | --- |
| **Menú principal** (`Menu.mp3` / `MenuPrincipal.ogg`) | Música | **Composición original del equipo EchoScrap** (secuenciado y mezclado en DAW). | Obra propia (propiedad del equipo). | Estéreo, 320 kbps, 48 kHz. Sintetizadores virtuales y *pitch drift*. |
| **Bóveda (Almacén)** (`Boveda_Almacen.mp3` / `Vault.ogg`) | Música | **Composición original del equipo EchoScrap**. | Obra propia (propiedad del equipo). | Estéreo, 320 kbps, 48 kHz. Instrumentos orquestales Spitfire BBC y sintetizadores. |
| **Arena** (`Partida.mp3` / `Arena_combat_Test.ogg`) | Música | **Composición original del equipo EchoScrap**. | Obra propia (propiedad del equipo). | Estéreo, 320 kbps, 48 kHz. Bucle continuo de percusión y sintetizadores. |
| **Gólgora** (`Jefe.mp3` / `Bossfight_Prueba_From2m.ogg`) | Música | **Composición original del equipo EchoScrap**. | Obra propia (propiedad del equipo). | Estéreo, 320 kbps, 48 kHz. Síntesis pura y percusión de combate. |
| **Linterna: base, ráfagas y pulsos** (`Media/Audio/Unity/Flashlight/`: `BurstStart.wav`, `Pulse1..3.wav`, `FourthPulse.wav`, `Rest.wav`) | SFX | Muestras externas de microondas (`pinto0lucas`, Freesound `#422817`) y contador Geiger (`cookies+policy`, Freesound `#555923`). | Licencia **CC0 1.0 (Dominio Público)** registrada en [`Sources.txt`](Media/Audio/Unity/Flashlight/Sources.txt). | Conversión a mono, 44,1 kHz, 16 bits; selección de clics, capas, envolventes y tonos sintetizados por el equipo. |
| **Linterna: impactos y Rayo solar** (`Media/Audio/Unity/Flashlight/`: `Hit.wav`, `CriticalHit.wav`, `SolarStart.wav`, `SolarLoop.wav`) | SFX | Diseño sonoro propio del equipo mediante capas y transientes sintetizados. | Obra propia. | Mono, 44,1 kHz, 16 bits. Modulación y envolventes reactivas al impacto. |
| **Combate, enemigos e interfaz principal** (`Media/Audio/SFX/*.wav` y equivalentes en `Resources/Audio/`: `Caballero_Barrido.wav`, `Centinela_Disparo.wav`, `Galgo_Ataque.wav`, `Golgora_Impacto.wav`, además de `Resources/Audio/Coleccionable`) | SFX | **Diseño sonoro y procesamiento propio del equipo EchoScrap** (Dash, Golpe recibido, Coleccionable en `"Audio/Coleccionable"`, Botiquín, Subida de nivel, Selección de carta, Galgo, Mortero, Polilla, Caballero Fantasma, Centinela de Cobre, Impacto de Gólgora en `"Audio/Golgora_Impacto"`, Señal de derrota, Señal de victoria y Fusión/resultado). | Obra propia / procesamiento sobre muestras CC0 y sintetizadas. | WAV estéreo, 24 bits, 48 kHz. Chelo, bajo, campana, sintetizadores, batería, corno francés, violín y teclado MIDI. |
| **Experiencia y avisos de recolección** (`Media/Audio/Unity/Sistema/`: `XP_sound.mp3`, `InventoryRejected.mp3` y recurso de archivo `CollectiblePickup.wav`) | SFX | Síntesis y edición propia del equipo para retroalimentación inmediata en arena y bóveda (`XP_sound` e `InventoryRejected` activos en runtime; `CollectiblePickup.wav` se conserva respaldado en `Resources/Audio/` sin uso directo, mientras `CollectiblePickupFeedback.cs:51` carga `"Audio/Coleccionable"`). | Obra propia / procesamiento sobre muestras CC0. | Mono, modulación de tono progresiva según racha de XP y aviso tonal de inventario lleno. |
| **Transiciones de portal, selección de ronda y apertura de fusión** (`Media/Audio/Unity/Sistema/`: `PortalArrival.mp3`, `ArenaSelectionRoar.wav`, `FusionOpen.mp3` y recurso de archivo `CollectorPortalEntry.wav`) | SFX | Edición y procesamiento del equipo sobre muestras de portales, mecanismos neumáticos y rugido ambiental (`CollectorPortalEntry.wav` se conserva respaldado en `Resources/Audio/` sin referencia activa en C#). | Mezcla propia sobre librerías CC0 / Dominio público. | Capas de despresurización, reverberación convolutiva y atenuación sincronizada con animación. |
| **Gólgora (`Media/Audio/Unity/MataColeccionista/*.wav`: `Spawn`, `Footstep`, `HammerCharge`, `HammerImpact`, `SweepCharge`, `SweepImpact`, `Throw`, `Cerco`, `Overload`, `Death`, `FoundryFoundation`, `FoundryPressure`)** | SFX | Diseño y montaje sonoro en Unity mediante capas metálicas, percusión grave y texturas de fundición (`GolgoraAudio.cs:31-35` carga los clips locales excepto el impacto de martillo, que en combate usa `"Audio/Golgora_Impacto"`, mientras `HammerImpact.wav` se valida en `GolgoraPresentationTests`). | Mezcla propia sobre librerías CC0 / Dominio público. | Atenuación espacial 3D, variación de tono/volumen y mezcla dinámica (*ducking*) sobre la música del jefe. |

La integración de una pista no implica que sea una composición final. Los nombres de prueba de arena y jefe se mantienen identificados como tales. La autoría y los recursos de cada incorporación deben conservarse en su registro de fuentes.

Los GIFs de este GDD no contienen audio y sirven para revisar animación y VFX. Como excepción documental, esta sección incluye las cuatro pistas musicales del juego (`Menu.mp3`, `Boveda_Almacen.mp3`, `Partida.mp3` y `Jefe.mp3`) y las quince muestras de efectos de sonido y avisos (`Media/Audio/SFX/*.wav`); su mezcla dinámica y espacialización se comprueban directamente en el juego.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

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

![Atlas activo de efectos de enemigos](Media/Avance_2026-11-04/VFX/EnemyEffects_Atlas.png)

| Efecto | Animación del recurso |
| --- | --- |
| Golpe del Residual | ![Golpe del Residual](Media/Avance_2026-11-04/VFX/Enemigos/ResidualSlash.gif) |
| Fragmentos de hueso | ![Fragmentos de hueso](Media/Avance_2026-11-04/VFX/Enemigos/BoneChips.gif) |
| Golpe del Interceptor | ![Golpe del Interceptor](Media/Avance_2026-11-04/VFX/Enemigos/InterceptorSlash.gif) |
| Estela del Interceptor | ![Estela del Interceptor](Media/Avance_2026-11-04/VFX/Enemigos/InterceptorWake.gif) |
| Polvo del Coloso | ![Polvo del Coloso](Media/Avance_2026-11-04/VFX/Enemigos/ColossusDust.gif) |
| Impacto del Coloso | ![Impacto del Coloso](Media/Avance_2026-11-04/VFX/Enemigos/ColossusImpact.gif) |
| Barrido espectral | ![Barrido espectral](Media/Avance_2026-11-04/VFX/Enemigos/SpectralSweep.gif) |
| Misil de cobre | ![Misil de cobre](Media/Avance_2026-11-04/VFX/Enemigos/CopperMissile.gif) |
| Salida del disparo de cobre | ![Salida del disparo de cobre](Media/Avance_2026-11-04/VFX/Enemigos/CopperMuzzle.gif) |
| Impacto de cobre | ![Impacto de cobre](Media/Avance_2026-11-04/VFX/Enemigos/CopperImpact.gif) |
| Humo de la Polilla | ![Humo de la Polilla](Media/Avance_2026-11-04/VFX/Enemigos/MothSmoke.gif) |
| Meteoro de magma | ![Meteoro de magma](Media/Avance_2026-11-04/VFX/Enemigos/MagmaMeteor.gif) |
| Impacto de magma | ![Impacto de magma](Media/Avance_2026-11-04/VFX/Enemigos/MagmaImpact.gif) |
| Zona de magma | ![Zona de magma](Media/Avance_2026-11-04/VFX/Enemigos/MagmaPuddle.gif) |
| Barrido de magma | ![Barrido de magma](Media/Avance_2026-11-04/VFX/Enemigos/MagmaSweep.gif) |

## Torreta centinela

La torreta usa trípode y cañón separados, orientación del cañón, boca de salida y retroceso (hasta 2 torretas activas simultáneas, disparando ráfagas de 3 proyectiles cada `0,6 s` separados por `0,08 s`).

![Torreta: previsualización técnica de sus direcciones](Media/Avance_2026-11-04/VFX/Torreta_Direcciones.gif)

![Torreta disparando en la arena de Unity](Media/Avance_2026-11-04/VFX/Torreta_Combate.gif)

## Linterna

Las dos capturas usan los mismos objetivos inmóviles y la misma cámara. La presentación cambia con las mejoras de la linterna: alcance y apertura, cadencia, intensidad y color.

### Linterna base

La ráfaga muestra el haz, los cuatro pulsos y el empuje en el cuarto pulso sobre los enemigos alcanzados.

![Linterna base: captura de Unity del 5 de noviembre](Media/Avance_2026-11-05/VFX/Linterna.gif)

### Rayo solar

La mejora legendaria (`FLASH_05`) cambia la luz a dorado, amplía el alcance base a `14 m`, multiplica el daño por `2,5` y elimina el descanso entre ráfagas. En el código vigente (`FlashlightWeapon.cs:402`), todos sus pulsos aplican empuje (`capturedSolarMode`), mientras que `FLASH_03` (*Prisma voltaico*), `BIO_02` (*Sifón*) y `CRIT_04` (*Ráfaga crítica*) siguen disparándose en el cuarto pulso (`PulseIndex == 4` en `UpgradeCombatRuntime.cs:232-236, 341`). *(El GIF del 5 de noviembre se capturó antes de activar el empuje por pulso y muestra el tratamiento luminoso continuo).*

![Rayo solar: captura de Unity del 5 de noviembre](Media/Avance_2026-11-05/VFX/RayoSolar.gif)

## Efectos de mejoras

Cada secuencia muestra el comportamiento visual de la mejora en el motor; sus valores y reglas exactas se detallan en [[05 - Progresión]].

### Prisma voltaico

Tras un impacto de cuarto pulso (o de cualquier pulso con *Rayo solar*), la descarga enlaza `2 / 3 / 4` objetivos sucesivos en `5 m` y produce destellos eléctricos en los alcanzados.

![Prisma voltaico: descarga y enlaces entre objetivos](Media/Avance_2026-11-05/VFX/PrismaVoltaico.gif)

### Frío y congelación

El primer impacto aplica la ralentización de *Freón residual*. Los pulsos siguientes sobre el objetivo ralentizado alcanzan el umbral de *Cero absoluto* y añaden escarcha, partículas y niebla fría.

![Freón residual y Cero absoluto: ralentización y congelación](Media/Avance_2026-11-05/VFX/FrioCongelacion.gif)

### Ventisca

Emite una onda de frío en `5,5 m` desde el jugador y congela `1,5 s` a los objetivos alcanzados.

![Ventisca: onda de frío y objetivos congelados](Media/Avance_2026-11-05/VFX/Ventisca.gif)

### Quiebre de hielo

La muerte de un enemigo congelado libera seis esquirlas radiales perforantes (`14 m/s`) que congelan `1,5 s`.

![Quiebre de hielo: esquirlas tras una muerte congelada](Media/Avance_2026-11-05/VFX/QuiebreHielo.gif)

### Minas de chatarra

La mina se arma a los `0,25 s` (hasta 5 minas activas simultáneas) y detona al detectar un enemigo a `1,25 m`.

![Minas de chatarra: colocación y explosión](Media/Avance_2026-11-05/VFX/Minas.gif)

### Termobárica

A los `0,4 s` de la explosión inicial aparece una implosión que repite el `75%` del daño y atrae a los enemigos hacia el centro una distancia base equivalente al `45%` del radio de explosión.

![Termobárica: explosión inicial e implosión secundaria](Media/Avance_2026-11-05/VFX/Termobarica.gif)

### Nova de plasma

Cada `15 HP` de curación sobrante emiten una nova de `80` de daño sobrenatural en `6 m` de radio alrededor del jugador.

![Nova de plasma: expansión por curación sobrante](Media/Avance_2026-11-05/VFX/NovaPlasma.gif)

### Curación y reparación

Efecto compartido de recuperación efectiva de salud (*Más vida*, *Sifón*, *Botiquín mejor*, regeneración de *Nanobots* y poderes de acompañantes curativos).

![Curación: efecto compartido de reparación](Media/Avance_2026-11-05/VFX/Curacion.gif)

### Nanobots: protección de emergencia

Cuando un golpe deja al jugador con vida (`RemainingHealth > 0`) por debajo del `20%` (`emergencyThreshold`), activa el escudo temporal de `2 s` (`60 s` de enfriamiento; no evita golpes letales ni cura hasta el `20%`).

![Nanobots: activación del escudo de emergencia](Media/Avance_2026-11-05/VFX/Nanobots.gif)

### Micro-ensamblador

Cada `15` gemas originales recogidas dispara un proyectil guiado a `24 m/s` hacia un enemigo.

![Micro-ensamblador: proyectil guiado, estela e impacto](Media/Avance_2026-11-05/VFX/MicroEnsamblador.gif)

### Sierras orbitales

Las hojas giran a `180°/s` a `2,5 m` del Coleccionista, infligiendo daño y empuje en cada paso de simulación en solape.

![Sierras orbitales: giro y efectos de contacto](Media/Avance_2026-11-05/VFX/Sierras.gif)

### Pulso colector

El remolino de recogida acompaña el vuelo de todas las gemas y botiquines activos del mapa hacia el jugador.

![Pulso colector: remolino y recogida de gemas](Media/Avance_2026-11-05/VFX/PulsoColector.gif)

### Onda sísmica

El evento de llegada del dash emite una expansión radial de daño físico y aturdimiento.

![Onda sísmica: expansión al terminar el dash](Media/Avance_2026-11-05/VFX/OndaSismica.gif)

### Holo-señuelo

El inicio del dash deja una copia del Coleccionista que atrae enemigos en `12 m` y, al agotarse su duración, implosiona en `4 m`.

![Holo-señuelo: copia temporal e implosión](Media/Avance_2026-11-05/VFX/HoloSenuelo.gif)

### Cataclismo

La explosión nace en el punto de salida (`120` de daño en `4 m`) y la cuchilla (`80` de daño) se lanza desde el punto de llegada, fijando la recarga del dash en `0,5 s`.

![Cataclismo: explosión de salida y cuchilla de llegada](Media/Avance_2026-11-05/VFX/Cataclismo.gif)

## Números de daño

Los valores siguen arcos con la tipografía `Pixelify Sans` y distinguen daño físico y sobrenatural, con variantes de impacto normal y crítico (`!`), además del indicador `Resistido` y el rótulo `EJECUTADO`.

![Números de daño: canales físicos y sobrenaturales, críticos, resistencias y ejecución](Media/Avance_2026-11-05/VFX/NumerosDano.gif)

## Fusión y portal

### Aura de la máquina de fusión

Las partículas ascienden por los costados de la máquina y dejan libre la persiana central.

![Máquina de fusión: aura de partículas](Media/Avance_2026-11-05/VFX/FusionAura.gif)

### Apertura y revelación de la fusión

Al abrir una fusión lista (`E · VER FUSIÓN` tras los `30 s` de cuenta atrás) se eleva la puerta, aparece humo y se revela la entidad obtenida junto al botón `COLECCIONAR`.

![Fusión: apertura de la máquina y humo de revelación](Media/Avance_2026-11-05/VFX/FusionApertura.gif)

### Portal del menú

El vórtice animado y las entidades en órbita forman el fondo del menú inicial.

![Menú inicial: portal animado y entidades en órbita](Media/Avance_2026-11-05/VFX/PortalMenu.gif)

## Efectos compartidos y mejoras de estadísticas

| Rareza / grupo | Presentación y shaders / efectos |
| --- | --- |
| Cinco marcos iluminados de carta (`UpgradeCardAuthoring.cs`, `UpgradeCardSkin.asset`) | Marcos 9-sliced con mapas normales y material lit (`UpgradeCardFrameLit.shader` + `UpgradeCardSheen.shader`): `Frame_Common`, `Frame_Rare`, `Frame_Epic`, `Frame_Legendary` y **`Frame_Mythic`** (`Frame_Mythic.png`, `Frame_Mythic_Normal.png`, `Frame_Mythic.mat`) reservado para la carta Mítica *All In* (`MYTH_01`), compartiendo la familia de `10` iconos en escala de grises y la tipografía `Pixelify Sans`. |
| Más alcance, Pulso rápido y Golpe potente | Reutilizan el haz, los impactos y la presentación de la linterna; cambian su apertura, alcance, cadencia, color o intensidad. |
| Dash ágil y Paso de ariete (`DASH_01`, `DASH_05`) | Reutilizan la animación y las imágenes residuales del dash; `DASH_01` modifica velocidad y recarga, mientras `DASH_05` (*Paso de ariete*, reelaborado) extiende duración y distancia (`+25% / +45% / +70%`, hasta `5,61 m`) sin daño de contacto. |
| Más vida, Sifón y Botiquín mejor | Reutilizan el efecto compartido de curación y reparación; el HUD expresa los cambios de vida. |
| Imán | Reutiliza el vuelo de recogida de gemas y botiquines. |
| Chatarrero | Cambia la XP recibida y su lectura en el HUD. |
| Sintonizador | Se representa mediante reintentos y ofertas en la interfaz de cartas. |
| Óptica precisa y Calibración letal | Reutilizan los destellos y números críticos; cambian sus probabilidades o valores. |
| Ejecución | Muestra el rótulo `EJECUTADO` en los números de daño y resuelve la muerte del enemigo. |
| Ráfaga crítica y Ojo del verdugo | Reutilizan los avisos de onda, aturdimiento y daño crítico. |
| Pólvora volátil, Reacción en cadena y Granadas | Reutilizan explosiones, zonas y proyectiles parabólicos; cambian radio, daño o condición de aparición. |
| Nuevos sistemas de combate (`UpgradeCombatRuntime.cs`) | Disponen de piscinas preasignadas sin GC en runtime para zonas persistentes (`GroundZone[64]`: *Charco cáustico*, *Escarcha ártica*), ataque radial (*Aura estática*), descargas (*Tormenta orbital*, *Sobrecarga*), barridos (*Barrido voltaico*), discos/búmeran (*Sierra rebotante*, *Búmeran reflector*) y reflejo (*Escudo reflector*), escalados con `AreaMultiplier` (`FLASH_13`). |
| Blindaje | Se lee en las placas y valores del HUD. |

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# UX y flujo del jugador

Menú inicial (`CONTINUAR` / `NUEVA PARTIDA` sobre **3 ranuras de guardado**, con campo opcional `NOMBRE DEL JUGADOR (RANKING)...` al crear perfil y panel `CONFIGURACIÓN` de 3 pestañas) → cargar ranura (retorno a bóveda o reanudación directa de expedición suspendida mediante snapshot) → bóveda (guiada por `EQUIPO 1..3`, `VaultDirections`, `VaultTips` e **Índice de Colección `[I]`**) → seleccionar entidades equipadas (poderes acumulables hasta `×3`) y/o fusionar copias en la pared trasera (`30 s`, misma rareza o rarezas mixtas) → elegir ronda demo o completa (`ELIGE LA RONDA`) → expedición con autoguardado de snapshot cada `3 s` → sincronización de ranking (`EventLeaderboardSync`) y regreso a la bóveda.

## Preparar el equipo y gestionar la colección en la bóveda

**`CONTINUAR`** abre el selector de **3 ranuras de guardado** (`CONTINUAR PARTIDA`, `ARCHIVO 1..3`), recupera la colección de esa ranura (`hasta 18 unidades` en posesión y `15` descubiertas en el índice) y, si existe una expedición en curso válida (`expedition_{N}.json`), la reanuda exactamente donde se encontraba (o permite recuperar la bóveda si el snapshot estuviera dañado mediante `RECUPERAR BÓVEDA`). **`NUEVA PARTIDA`** permite ocupar una ranura vacía o confirmar el reemplazo de una existente (conservando el archivo anterior hasta validar la nueva partida) y registrar opcionalmente el alias del ranking.

Al entrar en la bóveda, la interfaz orienta al jugador en todo momento mediante tres sistemas complementarios:

1. **Rótulos 3D sobre los pedestales del balcón superior:** **`EQUIPO 1`**, **`EQUIPO 2`** y **`EQUIPO 3`** (`CollectibleDisplay.cs`, añadiendo `· RESERVA` cuando muestran una copia en reserva mientras el puesto está vacío).
2. **Indicadores direccionales superiores (`VaultDirections`):** dos tarjetas dinámicas en la parte superior de la pantalla con flechas (`▲`, `▼`, `◄`, `►`, `●`) que apuntan en tiempo real hacia la **Máquina de Fusión** (*"MÁQUINA DE FUSIÓN · Combina 4 copias de varias rarezas"*, cambiando a *"BAJA A FUSIÓN"* en el balcón o *"E · MÁQUINA DE FUSIÓN"* al estar al lado) y hacia los **Puestos de Equipo** (*"SUBE AL BALCÓN · Puestos de equipo 1, 2 y 3"*, *"EQUIPAR ENTIDADES"* o *"PUESTO 1..3 · EQUIPAR/EDITAR"*).
3. **Panel de consejos (`VaultTips`):** tarjeta en la esquina inferior izquierda con la etiqueta **`CONSEJO`** que rota automáticamente cada `8 s` (o manualmente con `<` y `>`) entre tres mensajes:
   - *"Equipa coleccionables antes de ir a la arena."*
   - *"Abre el índice para consultar tus descubrimientos."*
   - *"Combina cuatro coleccionables en la máquina de fusión."*
   Si el jugador posee coleccionables pero aún no ha equipado ninguno (`OwnedCount > 0 && !hasEquippedCompanion`), el panel prioriza el aviso: *"Pulsa E frente a un puesto para equipar tu primer coleccionable."*

Además, pulsar **`I`**, **`Select`** en mando o hacer clic en **`ÍNDICE · [I]`** arriba a la izquierda abre el **Índice de Colección** (`VaultCollectionIndex.cs`), donde el jugador consulta el contador `{descubiertos} / 15 DESCUBIERTOS · {en posesión} / 18 UNIDADES`, filtra las 15 entidades en la pestaña **`POR RAREZAS`** (`TODAS` y las 6 categorías, con siluetas negras `???` para las no descubiertas), revisa sus unidades actuales en **`EN TU BÓVEDA`** (`EN EQUIPO ACTIVO` o `EN BÓVEDA`) e inspecciona en el panel derecho el poder de cada compañero.

En los **puestos 1 a 3 del balcón** (`slot 0..2`), el aviso es **`E · EQUIPAR`** (si está vacío, incluso cuando exhibe una copia de reserva) o **`E · EDITAR`** (ocupado). Pulsar **`E`** (o botón oeste en mando) abre el selector (`PUESTO DE EQUIPO 1/2/3`, cuyos poderes se acumulan por copia equipada) con los botones inferiores **`QUITAR DEL PUESTO`**, **`LIBERAR COPIA`** y **`CERRAR · E / ESC`**; pulsar **`T`** (o botón norte en mando) frente a un puesto ocupado desequipa directamente la unidad sin abrir el menú. En los **puestos 4 a 18 de la planta inferior** (`slot >= 3`), acercarse a un puesto ocupado muestra **`E · LIBERAR`** (`VaultReleaseInteraction.cs:67`) y pulsar **`E`** (o botón oeste) abre la confirmación **`LIBERAR UNIDAD`** (`CANCELAR` / `LIBERAR`).

![Selector de un puesto de equipo](Media/Interfaz/Equipar_coleccionable.png)
*Selector de un puesto de equipo (`CompanionView`) en la versión definitiva de Unity con los tres botones inferiores integrados: `QUITAR DEL PUESTO`, `LIBERAR COPIA` y `CERRAR · E / ESC`.*

El portal conduce a la selección de ronda (**`ELIGE LA RONDA`** — *"Elige una ronda para comenzar."*). Tras confirmar Demo (`3:00`) o Completa (`8:00`) se presenta la entrada del Coleccionista en la arena.

## Durante la expedición

La linterna ataca automáticamente y el HUD comunica salud, experiencia, nivel, bajas, dash, mejoras y retratos de los tres acompañantes equipados. Mantener pulsada la tecla **`Q`** (o **`Select`** en mando) despliega temporalmente las **estadísticas completas** y las **descripciones de los poderes de acompañantes** en tiempo real.

El minimapa descubre terreno al recorrerlo. Los fragmentos de entidades muestran un aviso y se recogen con **`E`** (o botón oeste en mando) antes de que desaparezcan a los `10 s`. Si el jugador ya ha alcanzado el tope de **18 unidades** en su colección (`18/18`), el fragmento no puede recogerse y muestra el aviso flotante **`INVENTARIO LLENO / LIBERA ESPACIO`**.

Las subidas de nivel pausan el combate y presentan hasta tres cartas. La confirmación aplica la mejora y continúa, o abre la siguiente elección pendiente.

Gólgora anuncia su entrada a los `3:00` (demo) u `8:00` (completa), traslada al jugador con protección a la caldera central de `22 m` y activa su cámara compartida. Al bajar al `50%` de vida, un rugido invulnerable (`1,5 s`) que empuja en `12 m` marca el inicio de la segunda fase.

## Volver a preparar y fusionar

Victoria y derrota muestran el resumen de la incursión, sincronizan las estadísticas y la puntuación con Firebase (`EventLeaderboardSync`) y ofrecen **`VOLVER A LA BÓVEDA`**. La colección obtenida se conserva y las cartas de combate se restablecen para el siguiente intento.

En la máquina de fusión situada en la pared trasera (`E · FUSIONAR` / `E · VER FUSIÓN`, `E` o botón oeste), el jugador combina **4 copias de la misma rareza o de rarezas mixtas** (excluida Yanikua; incluidas copias equipadas en el balcón etiquetadas como **`EN EQUIPO 1/2/3`**, que se desequipan automáticamente al confirmar). El proceso dura **30 segundos (`00:30`)** fijos para cualquier combinación y al terminar ofrece el botón único **`COLECCIONAR`** (si la colección está en `18/18`, advierte **`INVENTARIO LLENO · LIBERA ESPACIO`** y conserva el resultado hasta que el jugador libere una unidad en la bóveda).

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# UI y menús

| Interfaz | Información y acciones (`8047d90` / `b543684`) |
| --- | --- |
| Menú inicial y Ajustes (`GameMenuView`, `MenuSettingsView`) | `CONTINUAR`, `NUEVA PARTIDA` (selector visual de **3 ranuras** `ARCHIVO 1..3` con alias, contador `?/18`, tiempo jugado y estado), `CONFIGURACIÓN` (panel unificado en 3 pestañas: `General` con movimiento de cámara, efectos decorativos, `Efectos reducidos` en título / `Límite de FPS` `30 / 60 / 120` en pausa y selector **`Pantalla`** `COMPLETA` / `VENTANA`; `Interfaz` y `Sonido`) y `SALIR`; en `NUEVA PARTIDA · ARCHIVO N` incluye el campo `NOMBRE DEL JUGADOR (RANKING)...` (máx. 20 caracteres, validado contra Firebase mediante `GamertagRegistry.Reserve`, o vacío para práctica). |
| HUD de bóveda (`GameMenuView`, `VaultCollectionIndex`) | Dos tarjetas direccionales superiores (`VaultDirections`) hacia Fusión y Balcón de Equipo, botón superior izquierdo `ÍNDICE · [I]` (`BotonIndiceBoveda`) y tarjeta inferior izquierda `CONSEJO` (`VaultTips`) que rota cada `8 s`. |
| Índice de Colección / Bestiario (`VaultCollectionIndex`, `I` / `Select`) | Contador `{descubiertos} / 15 DESCUBIERTOS · {en posesión} / 18 UNIDADES`, pestañas `POR RAREZAS` (`TODAS` + 6 rarezas; siluetas negras `???` que pasan a retrato a color al recoger o reclamar por primera vez) y `EN TU BÓVEDA` (`EN EQUIPO ACTIVO` / `EN BÓVEDA`), inspector derecho de poder y botón `CERRAR · [I]`. |
| Selector de puesto de equipo (`CompanionView`, puestos 1 a 3) | Se abre con `E` / botón oeste (`E · EQUIPAR` / `E · EDITAR`). Encabezado `PUESTO DE EQUIPO 1/2/3`, subtítulo `ELIGE UNA ENTIDAD DESCUBIERTA PARA ESTE PUESTO`, nota de poderes acumulables por copia equipada y botones inferiores `QUITAR DEL PUESTO`, `LIBERAR COPIA` y `CERRAR · E / ESC`. (`T` / botón norte en el mundo desequipa directamente). |
| Modal de liberación (`VaultReleaseInteraction`) | Se abre con `E` / botón oeste (`E · LIBERAR`) en los puestos ocupados de planta baja (`slot >= 3`), o con `LIBERAR COPIA` en el selector. Cuadro `LIBERAR UNIDAD` con botones `CANCELAR` (`Esc` / botón este) y `LIBERAR` (`E`, `Enter`, `Espacio` o `A`/`Cruz`). |
| Estación de fusión (`FusionStationUI`) | Ventana con 4 ranuras horizontales (`Espacio_0..3`), `TIEMPO DE FUSIÓN 00:30`, botón `X`, panel `POSIBLES RESULTADOS` (misma rareza o rarezas mixtas; `60%`, `40%`, `1%` en 4 Épicas), botones `VACIAR` y `CONFIRMAR`, selector (`EN EQUIPO 1/2/3`, `QUITAR INGREDIENTE`, `VOLVER`), y pantalla de resultado con botón único `COLECCIONAR` (o `INVENTARIO LLENO · LIBERA ESPACIO` si hay `18/18` unidades). |
| Selección de ronda (`ELIGE LA RONDA`) | *"Elige una ronda para comenzar."* con opciones `RONDA DEMO` (`3:00`), `RONDA COMPLETA` (`8:00`) o `SALIR` (`Esc` / botón este). |
| HUD de combate (`CombatHud`, `CompanionView`, `PlayerStatsReadout`) | Salud, XP, nivel, tiempo, bajas, dash, cuadrícula de mejoras (`9 × 34 px` hasta `62` cartas; `+100 px` si ancho `< 1530 px`), panel de `22` filas de estadísticas (incluye `Distancia dash` y `Área efectos`), tres retratos equipados (`Retrato_0..2`, sin insignia numérica superpuesta; líneas inferiores con multiplicador `×N` y totales acumulados via `DescribeHudPower` si `Atributos de coleccionables = SÍ` o manteniendo `Q`/`Select`), minimapa circular con exploración y consulta rápida. |
| Cartas (`UpgradeDraftController`) | Hasta tres opciones en **5 rarezas** (`Común`, `Rara`, `Épica`, `Legendaria` y `Mítica` para *All In*); rareza, nivel, efecto, confirmación y reintento con *Sintonizador* (`R` o botón norte). |
| Menú de Pausa (`CombatHud` + `MenuSettingsView`, `Esc` / `Start`) | Comparte las **3 pestañas** de `MenuSettingsView` (`General`: `Movimiento de cámara`, `Efectos decorativos`, `Límite de FPS` `30 / 60 / 120` y **`Pantalla`** `COMPLETA` / `VENTANA`; `Interfaz`: `Estadísticas generales` `COMPLETAS`/`REDUCIDAS`/`OCULTAS` y `Atributos de coleccionables` `SÍ`/`NO`; `Sonido`: `Volumen general` y `Música`) más los botones `CONTINUAR` y `VOLVER AL MENÚ` (en bóveda) o `VOLVER A LA BÓVEDA` (en arena). |
| Resultados y ranking (`EventLeaderboardSync`) | Victoria o derrota, tiempo, nivel, bajas, XP, sincronización automática de todos los intentos válidos con Firebase Realtime Database (`rulesVersion = 2`, conservando la mejor puntuación por gamertag en [`https://echoscrap.web.app`](https://echoscrap.web.app)) y botón `VOLVER A LA BÓVEDA`. |
| Laboratorio (`EnemyLaboratory`) | Ocho arquetipos, generación, inspección, daño de prueba, congelación, pausa y avance (`Tab` y `R`). |

## Detalle del panel de estadísticas y consulta rápida (`Q` / `Select`)

Toda la interfaz emplea la fuente **`Pixelify Sans`**. El panel de estadísticas se adapta dinámicamente al número de filas que difieren de su valor base:

- **`COMPLETAS`:** muestra todas las estadísticas modificadas respecto a su base (por cartas o por poderes de acompañantes).
- **`REDUCIDAS`:** filtra los seis valores principales (daño por pulso, velocidad de ataque, alcance, probabilidad crítica, multiplicador crítico y velocidad de movimiento).
- **`OCULTAS`:** oculta el panel durante el combate normal.

En cualquier momento del combate, **mantener pulsada la tecla `Q` (teclado) o el botón `Select` (mando)** activa `CombatHud.IsHudDetailHeld`, desplegando temporalmente las **estadísticas completas** y las **descripciones de los poderes de los tres acompañantes equipados**, independientemente del modo configurado en la pestaña `Interfaz`.

## Vistas del Índice de Colección (Bestiario) y Ranking Web

![Índice de Colección (Bestiario en versión final)](Media/Interfaz/Bestiario.png)
*Vista de la versión definitiva del bestiario (`Índice de Colección`, tecla `[I]`) con la tipografía unificada Pixelify Sans, contador en vivo, filtros por rareza, siluetas e inspector de compañero.*

![Página web del Ranking en vivo (`echoscrap.web.app`)](Media/Interfaz/RankingWeb_EchoScrap.png)
*Visor web en vivo ([`https://echoscrap.web.app`](https://echoscrap.web.app)) conectado a Firebase Realtime Database con la clasificación general por puesto, jugador, bajas, tiempo y puntos.*

![Modal de Nueva Colección con Gamertag](Media/Interfaz/Nueva_Coleccion.png)
*Captura de referencia de la confirmación con gamertag (`734e2a5`); en `0.7.0`, este campo se integra tras elegir ranura en `NUEVA PARTIDA · ARCHIVO N`.*

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Accesibilidad

El panel unificado de ajustes (`MenuSettingsView.cs`), compartido tanto en **`CONFIGURACIÓN`** del menú inicial como en **`PAUSA`** (`Esc` / `Start`), organiza las opciones de confort visual, lectura y audio en tres pestañas dedicadas:

- **Pestaña `General`:** deslizadores independientes para **`Movimiento de cámara`** (`0%` a `100%`, `ShakeIntensity`) y **`Efectos decorativos`** (`0%` a `100%`, `DecorativeIntensity`), la casilla **`Efectos reducidos`** (`ReducedEffectsMode`) en el menú inicial o el selector **`Límite de FPS`** (`30 / 60 / 120`, `EchoScrap.Graphics.FrameRateLimit`) en el menú de pausa, y el selector **`Pantalla`** (**`COMPLETA`** / **`VENTANA`**, `EchoScrap.Graphics.FullScreen`, conservando íntegros los avisos de ataque y peligro).
- **Pestaña `Interfaz`:** selector **`Estadísticas generales`** (**`COMPLETAS`**, **`REDUCIDAS`**, **`OCULTAS`**) y selector **`Atributos de coleccionables`** (**`SÍ`** / **`NO`**, `ShowCompanionPowers`, para mostrar u ocultar las descripciones de los poderes bajo los 3 retratos del HUD). Además, mantener pulsado **`Q`** (teclado) o **`Select`** (mando) en combate despliega bajo demanda tanto las estadísticas completas como los atributos de coleccionables.
- **Pestaña `Sonido`:** deslizadores independientes para **`Volumen general`** (`0%` a `100%`, `EchoScrap.Audio.MasterVolume`) y **`Música`** (`0%` a `100%`, `EchoScrap.Audio.MusicVolume`).

La pausa y la pérdida de foco de ventana bloquean el control de combate y exigen soltar las entradas activas al regresar para impedir confirmaciones o dashes accidentales.

## Lectura, orientación espacial y formatos comprobados

| Comprobación | Evidencia |
| --- | --- |
| `1920 × 1080`, `16:9` | Verificado en runtime con la tipografía unificada **`Pixelify Sans`**. Incluye HUD de arena y jefe, cartas, panel unificado de ajustes en 3 pestañas, señalización de bóveda, Índice de Colección (`[I]`) y pantallas de resultado. |
| Escala de la interfaz | HUD y menús usan un lienzo de referencia de `1920 × 1080`. Los números flotantes de daño usan `1280 × 720` como referencia de escala. |
| Señalización en la bóveda | Implementada mediante rótulos 3D (`EQUIPO 1..3`), tarjetas direccionales superiores con flechas en vivo (`VaultDirections`), panel inferior de consejos rotatorios cada `8 s` (`VaultTips`) y botón `ÍNDICE · [I]`. |
| Señales redundantes | El daño crítico combina tamaño, color y signo de exclamación (`!`). Las resistencias muestran `Resistido` y las ejecuciones `EJECUTADO`. En el Índice de Colección (`[I]`), las entidades no descubiertas combinan silueta negra, nombre `???` y etiqueta `NO ENCONTRADO` / `SILUETA BLOQUEADA`. |
| Otros tamaños | `1280 × 720`, `2560 × 1440` y relaciones de aspecto distintas de `16:9` ajustan dinámicamente el HUD (`CombatHud` eleva `+100 px` la cuadrícula de mejoras por debajo de `1530 px` de ancho y `PlayerStatsReadout` se adapta al alto disponible). |

![Panel de Configuración](Media/Interfaz/Configuracion.png)
*Panel unificado de Configuración (`CONFIGURACIÓN`) en el menú principal (`MenuSettingsView.cs`), con las tres pestañas `General`, `Interfaz` y `Sonido` compartidas con el menú de pausa.*

![Menú de Pausa en combate y bóveda](Media/Interfaz/Menu_Pausa.png)
*Menú de Pausa durante la partida (`CombatHud.cs` + `MenuSettingsView.cs`, `Esc` / `Start`), organizado en las tres pestañas `General`, `Interfaz` y `Sonido`.*

## Cobertura de dispositivos de entrada (`8047d90` / `b543684`)

El sistema de entrada (`Input System 1.20.0`) cubre teclado/ratón y mando en combate (movimiento, dash `A` / `Cruz`), pausa (`Start`), cartas (`1..3` / navegación + `A` / `Y` para Sintonizador), consulta rápida del HUD (`Q` / `Select`), recogida manual de fragmentos (`E` / botón oeste `X` / `Cuadrado`), interacciones de bóveda en pedestales y máquina de fusión (`E` / botón oeste y `T` / botón norte para desequipar rápido en balcón) e Índice de Colección (`I` / `Select`).

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Guardado y configuración

## Guardado por ranuras, colección persistente y reanudación (`SaveSlotService`, `FusionProgress`, `ExpeditionSnapshotService`)

La persistencia principal de partidas abandona la clave única en favor de **3 ranuras independientes (`SaveSlotService.SlotCount = 3`)** almacenadas de forma atómica en disco bajo `Application.persistentDataPath/Saves/` (o `Saves_BatchTest/` en ejecución por lotes / pruebas):

- **`index.json` (`SaveIndexData`):** apunta a `lastPlayedSlot`, secuencia monotónica y estado de migración única (`legacyMigrated`) desde claves previas de `PlayerPrefs` (`EchoScrap.Collection.Production.v1`, `EchoScrap.Collection.v1`, `EchoScrap.Rank.v2` y `EchoScrap.Event.PlayerName`).
- **`slot_0.json`..`slot_2.json` (`SlotProfileData`):** cada ranura conserva `profileId`, `playerName`, `totalPlaySeconds`, `hasActiveExpedition`, `activeAttemptId`, `bestLocalScore`, `completedRankedRuns` y el estado íntegro de colección y fusión (`FusionProgress`, versión **`5`**):
  - **`hasEquippedCompanion` (`bool`):** registra si el jugador ya ha equipado al menos un acompañante alguna vez (utilizado por `VaultTips` para priorizar el consejo inicial cuando vale `false`).
  - **`equipped` (`int[3]`):** IDs de las entidades equipadas en los puestos 1 a 3 del balcón superior (`-1` si el puesto está vacío).
  - **`owned` (`int[15]`):** copias en posesión de cada una de las 15 entidades. La suma total tiene un **límite máximo estricto de 18 unidades** (`FusionProgress.Capacity = 18`, `OwnedCount <= 18`).
  - **`discovered` (`bool[15]`):** registro permanente de las entidades descubiertas para el **Índice de Colección (`[I]`)** y los selectores de puesto. Liberar una copia descuenta `1` de `owned[id]`, pero conserva siempre `discovered[id] = true`.
  - **`displays` (`int[18]`):** IDs mostrados en los **18 pedestales físicos de la bóveda** (`SlotCount = 18`: los **3 puestos de equipo** en `slot 0..2` y los **15 puestos de exposición/reserva** en `slot 3..17`).
  - **`phase` (`int` / `FusionPhase`), `result` (`int`), `remaining` (`float`), `duration` (`float`), `opening` (`float`):** estado de la máquina de fusión en **5 valores** (`0 = Idle`, `1 = Fusing`, `2 = Opening`, `3 = AwaitingDecision`, `4 = Ready`), entidad resultante ya sorteada, tiempo restante, duración fija de **`30 s` (`00:30`)** y progreso normalizado `0..1` de la apertura de puerta (`1,2 s`).
- **`expedition_0.json`..`expedition_2.json` (`ExpeditionSnapshotData`, `ExpeditionSnapshotService`):** durante una expedición activa se captura una instantánea completa de la simulación cada **`3 s`** (`AutosaveIntervalSeconds = 3f`) y ante hitos clave para reanudar exactamente desde `CONTINUAR` tras cerrar o suspender el juego (con recuperación segura a bóveda si el archivo estuviera dañado).
- **`ranking_outbox.json` (`RankingOutboxData`):** cola persistente por `ownerProfileId` para puntuaciones pendientes de envío a Firebase.

El guardado de ranura se ejecuta al recoger un fragmento en la arena, equipar o desequipar en el balcón, liberar una copia, iniciar o reclamar una fusión, periódicamente en la bóveda y en fusión cada `5 s` en tiempo real, y al pausar, salir al menú o cerrar la aplicación. El tiempo con la aplicación cerrada no adelanta la cuenta atrás de la fusión. Si la aplicación se cierra durante la animación de apertura (`Opening`), `RestoreAfterLoading()` pasa el estado a `AwaitingDecision` (`opening = 1f`); si estaba en `Ready`, conserva `Ready` para no perder el resultado.

## Audio, preferencias y sincronización de Ranking (`EventLeaderboardSync`, `GamertagRegistry`)

Las claves adicionales persistidas en `PlayerPrefs` son:

| Clave | Tipo / Valor por defecto | Función |
| --- | --- | --- |
| `EchoScrap.Audio.MasterVolume` | `float` (`0.85`) | Volumen general (`0%`–`100%` en `MenuSettingsView`). |
| `EchoScrap.Audio.MusicVolume` | `float` (`0.85`) | Volumen de música (`0%`–`100%` en `MenuSettingsView`, aplicado por `CombatMusicPlayer` y `MenuMusic`). |
| `EchoScrap.Graphics.FrameRateLimit` | `int` (`60`) | Tope de fotogramas seleccionado en pausa (`30`, `60` o `120` FPS; desactiva VSync mediante `QualitySettings.vSyncCount = 0`). |
| `EchoScrap.Graphics.FullScreen` | `int` (`1`) | Modo de pantalla elegido en la pestaña `General` (`1 = COMPLETA` sin bordes a resolución nativa; `0 = VENTANA`), aplicado automáticamente al iniciar el juego en Standalone. |
| `EchoScrap.PendingGamertag.{sha256}` | `string` (`32 hex`) | Recibo local por alias para confirmar reservas cuyo primer `PUT` pudo haberse completado antes de perder conexión (`GamertagRegistry`). |
| `EchoScrap.Rank.v2` / `EchoScrap.Collection.*` | Legado migrado | Claves heredadas importadas automáticamente al directorio `Saves/` durante `SaveSlotService.EnsureMigrated()` (y usadas solo como respaldo sin perfil cargado). |

Al iniciar en **`NUEVA PARTIDA · ARCHIVO N`** con un alias válido, **`GamertagRegistry`** normaliza espacios (`FormKC`), pasa a mayúsculas conservando tildes (`CanonicalName`), calcula su hash SHA-256 y reserva `/gamertags/{sha256}.json` (`PUT` con `if-match: null_etag`, `timeout = 8 s`), permitiendo reutilizar el alias en el mismo dispositivo/perfil cuando ya posee recibo local o coincide con la reserva. Durante cada expedición (`EN PARTIDA`), **`EventLeaderboardSync`** autentica mediante Firebase Anonymous Auth (`timeout = 5 s`) y sincroniza cada `2 s` el nodo `/players/{sessionId}.json` (`PUT` con `timeout = 8 s`, usando `attemptId` de 32 caracteres hexadecimales), alimentando el visor web [`https://echoscrap.web.app`](https://echoscrap.web.app).

La puntuación se calcula con `rulesVersion = 2` (`RawPoints = 100 × kills + 300 × units + 150 × discoveries + 250 × (highest + 1) + 2 × seconds`, conservando el `100 %` en la ronda de `3 min` y dividiendo entre `2` hacia abajo en la ronda de `8 min`). Cada **`VICTORIA`** o **`DERROTA`** válida encola el resultado en `Saves/ranking_outbox.json` hasta recibir confirmación HTTP de Firebase, incrementa `completedRankedRuns` y actualiza `bestLocalScore` en la ranura; el ranking web conserva la mejor puntuación por gamertag. Regresar voluntariamente a la bóveda cancela el intento (`DELETE` en `/players/{sessionId}`), mientras que cerrar el juego en mitad de la arena preserva el snapshot sin cancelar la partida hasta su reanudación.

![Página web del Ranking en vivo (`echoscrap.web.app`)](Media/Interfaz/RankingWeb_EchoScrap.png)

Los ajustes de la sesión en curso (`Movimiento de cámara`, `Efectos decorativos`, `Estadísticas generales` y `Atributos de coleccionables SÍ/NO`) se conservan entre transiciones de escena mientras la aplicación permanece abierta.

## Estado de la expedición

La salud, XP, nivel y cartas pertenecen al intento activo. Volver desde resultados o desde el menú de pausa limpia el combate y devuelve a la preparación en la bóveda, reaplicando los poderes de los tres puestos de equipo.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Arquitectura técnica

El proyecto activo se encuentra en `EchoScrap/` (`develop` @ `b543684`). La compilación principal empieza en `MainMenu` y carga `CombatArena` después de elegir ranura en **`CONTINUAR`** o **`NUEVA PARTIDA`**, conservando en escena tanto la arena exterior como la bóveda modular para alternar preparación y expediciones sin recargar actores.

| Responsabilidad | Componentes actuales (`Assets/_Project/Scripts`) |
| --- | --- |
| Menú, ajustes unificados y señalización | `MainMenuEntry`, `MainMenuController`, `MenuSettingsView` (3 pestañas compartidas entre `MainMenu` y `PAUSA`), `GameMenuView` (`VaultDirections`, `VaultTips`), `VaultCollectionIndex` (`BotonIndiceBoveda`), `SceneTransitionState`, `LoadingScreen` |
| Preparación y expedición | `GameFlowController`, `ExpeditionEntrySettings`, `ExpeditionIntro` |
| Composición del mundo | `CombatGameBootstrap` |
| Tiempo, pausas y resultado | `CombatSession`, `GameplayClock`, `CombatOutcomeResolver` |
| Movimiento, salud y estadísticas | `PlayerInputReader`, `PlayerMotor`, `TerrainPlayerCollision`, `PlayerHealth` (mitigación en `ReceiveDamage`), `PlayerRuntimeStats` |
| Linterna | `FlashlightWeapon`, `FlashlightPresentation`, `FlashlightAudio` |
| Horda y élites | `EnemyActor`, `EnemyPool`, `EnemySpriteView`, `MothSpriteView`, `MortarSpriteView`, `EnemyObstacleNavigator`, `EliteEnemyController`, `EliteProjectilePool`, `MothExplosionPool` |
| Jefe | `CombatWaveDirector`, `BossEncounter`, `BossArena`, `GolgoraActor`, `GolgoraCombatSettings` |
| Progresión y mejoras | `CombatProgression`, `UpgradeDraftController`, `UpgradeCatalog`, `UpgradeCombatRuntime`, `UpgradeEffectPool`, `UpgradeTuning` |
| Bóveda, colección, índice y fusión | `WarehouseTravel`, `WarehouseCameraDirector`, `WarehouseCameraFraming`, `FusionStation`, `FusionStationUI`, `FusionProgress` (`Capacity = 18`, `FusionSeconds = 30`), `VaultCollectionIndex` (`[I]`), `VaultReleaseInteraction`, `CollectibleDisplay` |
| Equipo y obtención | `CompanionSystem`, `CompanionPowerRuntime`, `CompanionPixelEffects`, `CompanionView`, `CollectibleUnitGrid`, `CollectibleDisplay`, `CollectiblePickupFeedback` |
| Arte y efectos | `EnemySpriteView`, `MortarSpriteView`, `MothSpriteView`, `EnemyEffectPool`, `UpgradeEffectPool`, `GolgoraMagmaRingPool` |
| Interfaz, pausa y audio | `CombatHud` (menú de pausa en 3 pestañas y consulta rápida `Q` / `Select`), `CombatMinimap`, `PlayerStatsReadout`, `DamageNumberPresenter`, `CombatMusicPlayer`, `MenuMusic` |
| Guardado y snapshots | `SaveSlotService` (3 ranuras JSON atómicas, migración de `PlayerPrefs` legado y cola de ranking), `ExpeditionSnapshotService` (captura/restauración cada `3 s`) y `FusionProgress` v5 |
| Ranking en vivo y visor web | `EventLeaderboardSync` (`rulesVersion = 2`, sincronización HTTPS de todos los intentos válidos conservando mejor marca por gamertag en Firebase Realtime Database), `GamertagRegistry` (reserva SHA-256) y visor web en vivo `Tools/WebRanking/index.html` ([`https://echoscrap.web.app`](https://echoscrap.web.app)) |
| Iluminación | `ArenaLightingController`, `ArenaLightingProfile` y renderer de atmósfera |

La simulación comunica daño, muerte, recogida y resultados mediante eventos C#. Las reservas (`Object Pool`) reutilizan enemigos, proyectiles, números flotantes y efectos visuales entre expediciones, manteniendo separadas las reglas de simulación y su presentación.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Motor y herramientas

| Tecnología | Uso en el proyecto actual (`8047d90` / `b543684`) |
| --- | --- |
| Unity 6000.6.3f1 | Escenas, física, integración, Play Mode y compilación Standalone (**Windows x64** y **macOS Universal**). |
| URP 17.6.0 | Iluminación, materiales, shaders, instanciación GPU e iluminación interior acotada (`WarehouseLightScope`). |
| Input System 1.20.0 | Lectura completa de teclado, ratón y mando (`PlayerInputReader`, `CombatHud`, `VaultCollectionIndex`, `CompanionSystem`, `FusionStation`, `VaultReleaseInteraction`). |
| Cinemachine 6.6.0 | Seguimiento de cámara en arena, caldera del jefe y bóveda. |
| UnityWebRequest / JSON | Reserva atómica de gamertag (`GamertagRegistry`), sincronización HTTPS `PUT` / `DELETE` del ranking con Firebase (`EventLeaderboardSync`) y almacenamiento atómico en JSON (`SaveSlotService`, `ExpeditionSnapshotData`, `FusionProgress` v5). |
| Unity Test Framework 1.8.0 | Pruebas automatizadas de reglas e integración. |
| C# | Sistemas de simulación, interfaz dinámica y herramientas del Editor. |
| HLSL | Shaders de luz, magma, portal, materiales de interfaz y efectos. |
| Aseprite | Fuentes editables, animaciones, atlas e interfaz de pixel art. |
| Blender | Preparación y edición del modelo 3D de Gólgora y piezas de escenario. |
| Pixelify Sans (`Regular` / `SemiBold`) | Familia tipográfica unificada en `Typography.asset`, `UpgradeCardSkin.asset` y generadores de UI. |
| Obsidian / Markdown | Redacción del GDD y presentación de imágenes, planos SVG y GIFs. |
| Git y GitHub | Versionado separado del repositorio del juego y del documento. |

## Organización del repositorio

El juego organiza el código en `Assets/_Project/Scripts`, las escenas en `Scenes`, los perfiles en `Settings` y los recursos visuales y sonoros en `Art`, `Audio` y `Resources`. Las fuentes editables (`.aseprite`) se conservan en `ArtSource`.

Los medios documentales enlazados en este GDD se ubican en las carpetas `Media/Planos`, `Media/Avance_2026-10-27`, `Media/Avance_2026-11-04`, `Media/Avance_2026-11-05`, `Media/Avance_2026-11-06` y `Media/Avance_2026-11-07`.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Requisitos técnicos

Las plataformas del proyecto son **PC Windows (`x64`)** y **macOS Universal (`x86_64` + `arm64`)**. El montaje utiliza Unity `6000.6.3f1`, URP `17.6.0`, Input System `1.20.0` y Cinemachine `6.6.0` sobre `develop` @ `b543684` (16 de noviembre de 2026). La compilación principal habilita las escenas `MainMenu` y `CombatArena`. El icono de aplicación de escritorio (`EchoScrap-AppIcon.png`) está enlazado en `ProjectSettings/ProjectSettings.asset`.

El director de oleadas limita la población ordinaria a `150` enemigos simultáneos (`250` en modo de estrés), con un tope de `6` Morteros activos, `2` élites simultáneos y `8` secuaces efectivos dentro de la caldera de Gólgora. La colección en la bóveda gestiona un máximo de `18` pedestales físicos (`3` de equipo + `15` de exposición).

## Perfiles de referencia y cierre de validación técnica

| Parámetro | Perfil mínimo objetivo | Perfil recomendado de referencia |
| --- | --- | --- |
| Sistema | Windows 10 de 64 bits | Windows 11 de 64 bits |
| CPU y GPU | Hardware de entrada de 64 bits compatible con DirectX 11 / URP | Equipo de referencia Ryzen 7 5700X y Radeon RX 6650 XT |
| Memoria de prueba | 8 GB de RAM y 4 GB de VRAM | 16 GB de RAM y 8 GB de VRAM |
| Resolución y objetivo | 1280 × 720, 16:9, 30 FPS | 1920 × 1080, 16:9, 60 FPS |
| Presupuesto de fotograma | p95 de 33,3 ms o menos | p95 de 16,7 ms o menos |
| Espacio libre previsto | 2 GB para instalación y margen | 2 GB para instalación y margen |

**Cierre de validación técnica (`TECH-01` — Cerrado para entrega v1.0):**

- **Compilación y distribución Release:** el perfil `Assets/Settings/Build Profiles/Windows.asset` configura la build final de distribución en modo **Release** (`m_Development: 0`, `m_ConnectProfiler: 0`, `m_AllowDebugging: 0`) con las escenas `MainMenu` y `CombatArena`. La compilación Windows x64 cerró con **`0` errores** y un tamaño medido de **`1 010 519 179` bytes (~`0,94 GiB`)**, dentro del margen de `2 GB`, quedando publicada como ejecutable independiente en [`https://naked-hiro.itch.io/echoscrap`](https://naked-hiro.itch.io/echoscrap).
- **Rendimiento, tope de FPS y estabilidad en oleadas:** sobre la base previa (`p95` de CPU de `6,61 ms` en `2 000` fotogramas), las entregas recientes incorporan suspensión del renderizado de arena al mostrar resultados (`cf81e7d`), presupuesto de consultas por frame en `EnemyObstacleNavigator` (`9a8c141`), instanciación GPU completa en shaders de personaje y sombra (`972b642`, limpiando `_EmissionColor` al concluir destellos y reciclar sprites en `MothSpriteView`, `MortarSpriteView` y `EnemySpriteView`), optimización de texturas y mallas/LODs (`cb037bb`, `66c9ed5`), luces interiores acotadas (`WarehouseLightScope`), pooling sin asignaciones GC en tiempo de ejecución para todas las nuevas habilidades de ataque y proyección de anillos sobre relieve en `UpgradeEffectPool.ProjectGroundRing`, selector **`Límite de FPS` (`30 / 60 / 120`)** en el menú de pausa (`EchoScrap.Graphics.FrameRateLimit`) y selector **`Pantalla` (`COMPLETA` / `VENTANA`)** (`EchoScrap.Graphics.FullScreen`, iniciando automáticamente en `FullScreenWindow` tras cargar escena en Standalone), junto con los controles de `Efectos decorativos` y `Efectos reducidos`.
- **Alcance acotado documentado:** no se registraron métricas cuantitativas dedicadas de FPS/`p95` sobre un equipo físico de gama mínima (`8 GB RAM`), por lo que la validación sobre ese hardware específico queda registrada como limitación conocida de la entrega en [[03 - Bugs y Seguimiento]].

## Conectividad y controles

- **Sincronización de ranking (`EventLeaderboardSync`):** utiliza peticiones HTTPS asíncronas (`UnityWebRequest` `PUT` y `DELETE`) hacia Firebase Realtime Database (`https://echoscrap-default-rtdb.firebaseio.com`) con tiempo de espera acotado (`8 s` para envío de ranking y `5 s` para autenticación) y cola local persistente en `Saves/ranking_outbox.json` (y respaldo heredado `EchoScrap.Rank.v2`). Si el equipo no dispone de conexión a internet, la partida y el guardado local continúan sin interrupciones y el envío pendiente se reintenta al reconectar.
- **Dispositivos de entrada:** soporte completo para teclado/ratón y mando en combate (movimiento y dash `Espacio` / `A` / `Cruz`), recogida de fragmentos e interacciones de bóveda (`E` / botón oeste `X` / `Cuadrado`), desequipar rápido en balcón (`T` / botón norte `Y` / `Triángulo`), Índice de Colección (`I` / `Select`), consulta rápida de detalles del HUD (`Q` / `Select`) y menús (`Esc` / `Start` / botón este `B` / `Círculo`).

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Datos y persistencia

Los valores del jugador, los arquetipos enemigos y las oleadas se configuran mediante `ScriptableObjects`. `ExpeditionEntrySettings` enlaza las rondas demo (`3:00`, `Golgora_Demo.asset` con `3 600 HP`, mismo valor base en `EnemyArchetypeSettings`) y completa (`8:00`, `Golgora_Complete.asset` con `9 000 HP`), mientras que `GolgoraCombatSettings` y `CombatSettings.asset` definen de forma unificada los parámetros de combate del jefe (`TransitionDuration = 1,5 s`, daños, tiempos, áreas y multiplicadores de Fase 2).

El catálogo de combate consta de **`62` mejoras en cinco rarezas** (`UpgradeCatalog.Cards`, `UpgradeRarity.N..M`). El catálogo de entidades consta de `15` identificadores (`0` a `14`), distribuidos en seis rarezas y dos fuentes de obtención (nueve en la arena y seis por fusión). `CompanionSystem` comunica los tres puestos de equipo a `CompanionPowerRuntime` (con poderes **acumulables por copia equipada**, hasta `×3`) y a `PlayerRuntimeStats`.

## Sesión en memoria

El reloj de incursión, la salud actual, la XP, el nivel, las cartas adquiridas, los enemigos activos y los objetos del suelo pertenecen a la sesión en memoria. Al regresar a la bóveda tras una victoria, una derrota o desde el menú de pausa se limpia el estado de la arena y se reaplican los poderes de las entidades equipadas. Una visita temporal a la bóveda por el portal norte suspende el combate sin destruir la sesión activa.

## Persistencia en 3 ranuras, snapshots y fusión (`SaveSlotService`, `ExpeditionSnapshotService`, `FusionProgress` v5)

`SaveSlotService` gestiona hasta **`3` ranuras independientes (`SlotCount = 3`)** en disco mediante escritura atómica y respaldo bajo `Application.persistentDataPath/Saves/` (o `Saves_BatchTest/` en batch mode): `index.json`, `slot_0..2.json` (`SlotProfileData`, que incluye `FusionProgress` versión **`5`**, `totalPlaySeconds`, `hasActiveExpedition`, `activeAttemptId`, `bestLocalScore` y `completedRankedRuns`), `expedition_0..2.json` (`ExpeditionSnapshotData`) y `ranking_outbox.json` (`RankingOutboxData`). En la primera carga, `SaveSlotService.EnsureMigrated()` importa automáticamente cualquier progreso previo de `PlayerPrefs` (`EchoScrap.Collection.Production.v1`, `EchoScrap.Collection.v1`, `EchoScrap.Rank.v2` y `EchoScrap.Event.PlayerName`).

La capacidad máxima de la colección es de **18 unidades en total** (`FusionProgress.Capacity = 18`; las nuevas recogidas y reclamaciones requieren `OwnedCount < 18`), que se corresponden exactamente con los **18 pedestales físicos de la bóveda** (`DisplaySlots[18]`): **3 puestos de equipo activo** en el balcón superior (`slot 0..2`) y **15 puestos de exposición/reserva** en la planta inferior (`slot 3..17`).

| Campo del JSON (`FusionProgress`) | Tipo | Contenido y reglas de validación (`FusionProgress.cs`) |
| --- | --- | --- |
| `version` | `int` | Versión del esquema de guardado. El valor vigente es **`5`**. |
| `hasEquippedCompanion` | `bool` | Registra si el jugador ya ha equipado al menos un acompañante alguna vez (`VaultTips` y `FusionStation.cs`). |
| `equipped` | `int[3]` | IDs de las entidades asignadas a los **3 puestos de equipo activo** del balcón (puestos 1 a 3) o `-1` si el puesto está vacío. |
| `owned` | `int[15]` | Copias en posesión de cada una de las 15 entidades (`ID 0..14`). Los valores negativos se ajustan a `0`. Las nuevas recogidas y reclamaciones se bloquean al alcanzar `18` unidades (`Capacity = 18`); la carga conserva un posible exceso heredado, sin eliminar copias. |
| `discovered` | `bool[15]` | Registro permanente de descubrimiento de las 15 entidades para el **Índice de Colección (`[I]`, `VaultCollectionIndex`)** y los selectores de puesto. Toda entidad con `owned[i] > 0` marca `discovered[i] = true`, y liberar una copia conserva `discovered[i] = true`. |
| `displays` | `int[18]` | Array de longitud **`18`** (`Capacity = 18`) que representa el total de los **18 puestos físicos de la bóveda** (`displays[0..2]` para el balcón y `displays[3..17]` para la planta baja). `-1` indica pedestal vacío. |
| `phase` | `int` (`FusionPhase`) | Estado de la máquina de fusión en **5 estados**: `0 = Idle`, `1 = Fusing`, `2 = Opening`, `3 = AwaitingDecision`, `4 = Ready`. |
| `result` | `int` | ID de la entidad resultante sorteada al iniciar la fusión (`-1` cuando está libre). |
| `remaining` | `float` | Segundos restantes de la cuenta atrás de fusión (`Time.unscaledDeltaTime`). |
| `duration` | `float` | Duración total de la fusión activa: **fija en `30 s` (`00:30`, `FusionRecipe.FusionSeconds = 30`)** para recetas homogéneas o de rarezas mixtas. |
| `opening` | `float` | Progreso normalizado (`0..1`) de la apertura de persiana (`1,2 s`). Al cargar con `RestoreAfterLoading()`, si estaba en `Opening`, pasa a `AwaitingDecision` (`opening = 1f`); si estaba en `Ready`, conserva `Ready`. |

### Reglas de carga, snapshots y escritura
- **Validación al cargar (`Validate`):** Descarta guardados `version < 4` (o `owned == null`), migra esquemas `v4` (`displays` de `10`) al formato **`v5` (`displays[18]`)**, ajusta arreglos de 13 a 15 entidades, limita `owned[i] >= 0`, sanea IDs de `displays` y `equipped`, y garantiza que `discovered[id]` sea `true` para toda entidad con copias en `owned` o equipada, **pero no recorta automáticamente el exceso de `owned` sobre `18` al cargar** (`HasSpace` aplica el tope al recoger o reclamar). En escena (`FusionStation.RefreshDisplays`), los puestos `3..17` exhiben hasta `15` copias en reserva y cualquier puesto de equipo vacío `0..2` exhibe las copias restantes marcadas como `RESERVA`.
- **Snapshots de expedición (`ExpeditionSnapshotService`):** durante la incursión guarda cada `3 s` (`AutosaveIntervalSeconds = 3f`) y en hitos de combate la instantánea `expedition_{N}.json` (`profileId`, `attemptId`, `revision`, reloj, vida, nivel, XP, cartas adquiridas, oleadas y progreso de ranking), restaurándola silenciosamente al elegir la ranura desde `CONTINUAR` (o ofreciendo recuperar bóveda si estuviera dañada).
- **Inicio y resolución de fusión (`FusionProgress.Begin`):** Al confirmar una fusión (con 4 copias de la misma rareza o de rarezas mixtas, excluida Yanikua), si las 4 unidades seleccionadas incluyen copias equipadas (`EN EQUIPO 1/2/3`), el sistema las desequipa automáticamente de `equipped`, descuenta las 4 unidades de `owned`, sortea el resultado según la calidad media (`qualityMean = total / 4f`) y fija `duration = 30` y `remaining = 30`. Durante la fase `Fusing`, el reloj avanza con `Time.unscaledDeltaTime` y persiste la ranura cada `5 s`, además de hacerlo al pausar o cerrar la aplicación. Si al reclamar (`COLECCIONAR`) el jugador ya tiene `18/18` unidades, `FusionStation.Resolve` conserva el estado hasta que se libere una unidad en los pedestales de la bóveda.

## Otras claves de `PlayerPrefs`, archivos en `Saves/` y sincronización de Ranking (`EventLeaderboardSync`, `GamertagRegistry`)

| Clave o ruta | Tipo / Valor por defecto | Descripción |
| --- | --- | --- |
| `Saves/index.json`, `slot_0..2.json`, `expedition_0..2.json`, `ranking_outbox.json` | Archivos JSON atómicos en `Application.persistentDataPath` | Estado completo de las 3 ranuras, perfil (`bestLocalScore`, `completedRankedRuns`), colección (`FusionProgress` v5), snapshot de expedición y cola de envíos al ranking (`Saves_BatchTest/` en ejecución por lotes). |
| `EchoScrap.Graphics.FrameRateLimit` | `int` (`60`) | Tope de FPS elegido en la pestaña `General` de pausa (`30`, `60` o `120`; fuerza `QualitySettings.vSyncCount = 0`). |
| `EchoScrap.Graphics.FullScreen` | `int` (`1`) | Modo de pantalla elegido en la pestaña `General` de `MenuSettingsView` (`1 = COMPLETA` `FullScreenWindow` a resolución nativa del monitor; `0 = VENTANA` `Windowed`), reaplicado automáticamente tras el arranque en Standalone. |
| `EchoScrap.Audio.MasterVolume` | `float` (`0.85`) | Volumen general (`AudioListener.volume`, deslizador en pestaña `Sonido` de `MenuSettingsView`). |
| `EchoScrap.Audio.MusicVolume` | `float` (`0.85`) | Volumen de música (`CombatMusicPlayer.SetMusicVolume` y `MenuMusic.RefreshVolume`, deslizador en pestaña `Sonido` de `MenuSettingsView`). |
| `EchoScrap.PendingGamertag.{sha256}` | `string` (`32 hex`) | Recibo local temporal de reserva de nombre en curso (`GamertagRegistry`). |
| `EchoScrap.Collection.*` / `EchoScrap.Rank.v2` / `EchoScrap.Event.PlayerName` | Claves `PlayerPrefs` heredadas | Claves de versiones previas migradas automáticamente hacia `Saves/` durante `SaveSlotService.EnsureMigrated()` (mantenidas como respaldo si no hay ranura cargada). |
| `https://echoscrap-default-rtdb.firebaseio.com` y [`https://echoscrap.web.app`](https://echoscrap.web.app) | Nodos `/gamertags/{sha256}` y `/players/{sessionId}` (`Tools/WebRanking/index.html`) | Reserva atómica de gamertag (`PUT` con `if-match: null_etag`, `timeout = 8 s`, mayúsculas conservando tildes) y sincronización en vivo cada `2 s` (`PUT` con `heartbeat` y `timeout = 8 s`, `attemptId` de `32` caracteres hexadecimales). **Todos los intentos válidos** finalizados (`VICTORIA`/`DERROTA`) se publican y el ranking conserva la mejor puntuación por gamertag (`DELETE` al volver voluntariamente a bóveda o reiniciar). Puntuación `rulesVersion = 2`: `RawPoints = 100 × kills + 300 × units + 150 × discoveries + 250 × (highest + 1) + 2 × seconds` (`100 %` en modo `3 MIN`, `÷ 2` hacia abajo en modo `8 MIN`). |

En Windows, `PlayerPrefs` almacena estas claves de configuración en el registro del usuario bajo `HKEY_CURRENT_USER\Software\DefaultCompany\EchoScrap` y los archivos de guardado en `%USERPROFILE%\AppData\LocalLow\DefaultCompany\EchoScrap\Saves\`. En macOS, se ubican bajo `~/Library/Application Support/DefaultCompany/EchoScrap/Saves/`.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Seguridad y riesgos técnicos

El inventario de entidades (`hasta 18 unidades` en posesión y `15` descubiertas), los dieciocho pedestales de la bóveda (`displays[18]`: 3 de equipo + 15 de exposición), los snapshots de expedición (`expedition_{0..2}.json`) y el estado de la máquina de fusión se validan atómicamente al cargar los JSON de `SaveSlotService` antes de reanudar cualquier proceso.

## Comportamientos verificados y salvaguardas del código (`8047d90` / `b543684`)

- **Límite estricto de capacidad (`Capacity = 18`):** tanto la recogida de fragmentos en la arena (`CompanionSystem.TryCollectNearest` → `FusionStation.CollectFromArena`) como la reclamación del resultado en la máquina de fusión (`FusionStation.Resolve(true)` desde el botón `COLECCIONAR`) comprueban `FusionProgress.HasSpace` (`OwnedCount < 18`). Si la colección está en `18/18`, se bloquea la adición y se conserva el resultado de la fusión hasta que el jugador libere una unidad en la bóveda.
- **Protección contra corrupción de ranuras y snapshots (`SaveSlotService` / `ExpeditionSnapshotService`):** las escrituras usan reemplazo atómico con copia de respaldo (`TryReadVerifiedPayload` + `.bak`), aíslan los tests en `Saves_BatchTest/` (`Application.isBatchMode`), conservan la ranura destino intacta hasta validar una nueva partida y ofrecen **`RECUPERAR BÓVEDA`** (`SlotHealth.ExpeditionDamagedRecoverable`) si el snapshot de una expedición en curso resultara dañado.
- **Consumo de copias equipadas en fusión (`FusionProgress.Begin`):** cuando las 4 unidades seleccionadas para una fusión (misma rareza o rarezas mixtas) incluyen copias equipadas en el balcón (`EN EQUIPO 1/2/3`), el sistema calcula si las copias restantes tras descontar las 4 unidades son inferiores a las equipadas y desequipa automáticamente los puestos necesarios antes de guardar el estado `Fusing` (`30 s`).
- **Preservación de descubrimientos al liberar (`FusionStation.ReleaseCollectible`):** al liberar una copia desde un pedestal de exposición de planta baja (`E · LIBERAR`, `slot >= 3`) o desde el selector de equipo (`LIBERAR COPIA`), se descuenta `1` de `owned[id]` y se reordenan los puestos de exposición, manteniendo `discovered[id] = true` para no perder nunca el progreso en el Índice de Colección (`[I]`).
- **Reciclado limpio y asignación cero en combate (`Object Pool` / `UpgradeCombatRuntime`):** todo enemigo, proyectil o efecto reciclado limpia sus temporizadores, propiedades de shader (`_EmissionColor` en `MothSpriteView`, `MortarSpriteView` y `EnemySpriteView`), estados alterados y objetivos antes de volver a activarse; además, las nuevas zonas persistentes (`GroundZone[64]`), cadenas voltaicas (`VoltaicChain.Targets[6]`), sierras (`sawViews[5]`), relámpagos orbitales, aura estática proyectada sobre el relieve (`UpgradeEffectPool.ProjectGroundRing`, malla `12 × 12`) y proyectiles reutilizan arreglos preasignados con `0` bytes de basura en GC verificados bajo estrés de `150` enemigos.
- **Aleatorización segura de *All In* (`MYTH_01` / `UpgradeDraftController`):** al aplicar la carta Mítica *All In*, cada mejora poseída se sustituye aleatoriamente por otra distinta elegible y sube `+1` nivel respetando su `MaxTier` y sin permitir autorreferencias ni reinicializar *All In* recursivamente.
- **Razones independientes de pausa (`CombatSession`):** la pausa manual (`Esc`), la pérdida de foco, la elección de cartas, la máquina de fusión / Índice de Colección (`PauseReason.FusionMenu`), el selector de acompañantes (`PauseReason.CompanionMenu`) y el menú general usan máscaras independientes para que cerrar una ventana no reanude por error una sesión detenida por otra causa.
- **Sincronización de ranking no bloqueante (`EventLeaderboardSync` / `GamertagRegistry`):** las peticiones HTTPS (`PUT` y `DELETE`) a Firebase Realtime Database se ejecutan en corrutinas con `timeout = 8 s` (`5 s` para autenticación y `GET` de comprobación) y cola persistente en `Saves/ranking_outbox.json` (y recibos en `EchoScrap.PendingGamertag.{sha256}`), conservando partidas activas suspendidas sin enviar `CANCELADA` al cerrar la aplicación.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Alcance de la versión jugable

El juego integra menú inicial con selector de **3 ranuras de guardado**, reanudación automática de expedición activa por snapshot, registro opcional de alias para ranking, preparación guiada en la bóveda y expediciones en ronda demo (`3:00`) o completa (`8:00`). En la bóveda, señalizada mediante rótulos 3D (`EQUIPO 1..3`), indicadores direccionales (`VaultDirections`) y consejos rotatorios (`VaultTips`), el jugador consulta el **Índice de Colección (`[I]`)**, equipa hasta tres entidades en el balcón superior, libera copias en los pedestales para gestionar el espacio y utiliza la máquina de fusión ubicada en la pared trasera (`(189, 0, 28,88)`, `30 s` fijos).

El combate reúne cinco arquetipos de horda, dos élites, **62 mejoras en cinco rarezas** (Común, Rara, Épica, Legendaria y **Mítica** con *All In*, además de mejoras de área, auras, orbitales, proyectiles y la reelaboración de *Paso de ariete*) y Gólgora. El jefe cuenta con seis patrones de ataque, segunda fase al `50%` de vida y una caldera propia de `22 m` de radio. Victoria, derrota y abandono sincronizan las métricas de sesión con el ranking de evento (`EventLeaderboardSync`, manteniendo la mejor puntuación por gamertag entre todos los intentos válidos) y devuelven al jugador a la bóveda conservando su colección.

La arena de `200 × 200 m` incorpora relieve, obstáculos con deslizamiento suave y acceso norte. Las zonas de lava fija están desactivadas. La bóveda cuenta con **18 pedestales físicos** (`DisplaySlots[18]`) que corresponden a la capacidad máxima estricta de **18 unidades** de la colección: **3 puestos de equipo activo** en el balcón superior (que también exhiben reservas si están vacíos) y **15 puestos de exposición/reserva** en la planta inferior, junto a la máquina de fusión en la pared trasera que consume cuatro copias de la misma rareza o de rarezas mixtas (excluida Yanikua).

El catálogo comprende **15 entidades** en seis rarezas (nueve obtenibles en la arena y seis mediante fusión), con **poderes acumulables por copia equipada** (`Copies(id)`, hasta `×3`). El inventario (`owned[15]`, máx. `18`), el registro permanente de descubrimientos (`discovered[15]`), los pedestales (`displays[18]`), el proceso de fusión (`FusionPhase` de 5 estados) y las instantáneas de expedición tienen guardado atómico por ranura en JSON (`SaveSlotService`, `FusionProgress` v5).

La presentación incluye tipografía unificada **`Pixelify Sans`**, menú de pausa en tres pestañas (`General`, `Interfaz`, `Sonido`), consulta rápida de detalles del HUD en combate (`Q` / `Select`), sprites animados de los cinco enemigos de horda y ambos élites, modelo 3D de Gólgora, efectos de enemigos y mejoras, minimapa con exploración, retratos, iluminación y audio contextual. El laboratorio y los sandboxes permanecen como herramientas de prueba.

## Criterio de cierre

La entrega comprende el flujo completo multiplataforma (**Windows x64** y **macOS Universal**), desde las 3 ranuras de guardado y preparación guiada en bóveda hasta la fusión de `30 s`, el Índice de Colección (`[I]`), las expediciones demo y completa con snapshots cada `3 s`, el selector de FPS (`30 / 60 / 120`), las 62 mejoras en cinco rarezas, la sincronización del ranking y el retorno a la bóveda con persistencia completa. El contenido jugable se contrasta con [[04 - Criterios de Aceptación]].

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Roles y Responsabilidades

## Objetivo de este reparto

El reparto organiza el mantenimiento del GDD y el trabajo del equipo sobre la versión jugable. La documentación combina reglas actuales, recursos integrados y registros visuales para que cada área pueda revisar el mismo estado del juego.

El equipo consta de cinco integrantes con roles definidos. Sierra Sosa David Alejandro es responsable del balance, las mecánicas y el diseño funcional y técnico del juego; Mora Alejandro Angel Gabriel coordina la visión global y la producción; Cupul Dzul Mirian Leticia (Artista 1) lleva dirección de arte, personajes y animación; Obispo Navarro Samuel Alejandro (Artista 2) lleva mundo, entornos e interfaz visual y colabora en audio; y Nuñez Chan Luis Fernando lleva la narrativa presentada al jugador, audio, experiencia de usuario (UX/UI), calidad (QA) y referencias.

## Equipo y responsabilidad principal

| Integrante / Rol | Responsabilidad en el GDD | Apartados como redactor |
|---|---|---:|
| **Mora Alejandro Angel Gabriel** (Líder) | Visión, premisa, alcance, producción, acuerdos y cierre de entrega. | 12 |
| **Sierra Sosa David Alejandro** (Desarrollo y diseño de juego) | Mecánicas, balance, contenido jugable, tecnología, guardado y registro visual. | 19 |
| **Cupul Dzul Mirian Leticia** (Artista 1) | Dirección visual, fichas de personajes y animaciones. | 3 |
| **Obispo Navarro Samuel Alejandro** (Artista 2 — apoya audio) | Mundo, entornos e interfaz visual; apoyo al responsable de audio. | 3 |
| **Nuñez Chan Luis Fernando** (Audio, UX y apoyo general) | Narrativa presentada al jugador, sonido, UX, QA y referencias. | 11 |

La cantidad de archivos no mide la carga por sí sola: una ficha de animaciones o un inventario de interfaz requiere un nivel alto de detalle. Se aprovechan las reglas documentadas por David y se distribuyen revisión, narrativa y calidad para que cada área pueda cerrarse con coherencia.

## Cómo se entrega cada apartado

- **Redactor:** escribe y deja el apartado completo, coherente y enlazado. Hay exactamente uno por archivo.
- **Revisor:** lee el borrador, verifica su coherencia y devuelve observaciones; no edita en paralelo sin coordinarse.
- **Apoyos:** aportan datos o recursos concretos cuando el apartado los necesita. No sustituyen al redactor.
- **Se validan reglas, valores y viabilidad técnica.** Los artistas deciden su presentación visual; el responsable de audio decide su presentación sonora.
- **El líder coordina decisiones globales de alcance y la entrega.** Los cambios que afecten a varias áreas se acuerdan con el equipo.
- **El profesor aprueba el GDD.** La revisión interna del equipo no equivale a aprobación docente.

Un apartado está listo cuando contiene decisiones propias de *EchoScrap*, sus límites y casos relevantes, y coincide con los documentos relacionados. Cuando algo no aplica, debe explicarse; no se añaden funciones sólo por aparecer como ejemplo en la plantilla.

## Asignación por apartado

| Apartado                                            | Redactor                                    | Revisor                                     |
| --------------------------------------------------- | ------------------------------------------- | ------------------------------------------- |
| 01 - Visión · 01 - Ficha del Proyecto               | Mora Alejandro Angel Gabriel (Líder)        | Sierra Sosa David Alejandro                 |
| 01 - Visión · 02 - Visión del Juego                 | Mora Alejandro Angel Gabriel (Líder)        | Sierra Sosa David Alejandro                 |
| 01 - Visión · 03 - Público y Plataforma             | Mora Alejandro Angel Gabriel (Líder)        | Sierra Sosa David Alejandro                 |
| 01 - Visión · 04 - Pilares de Diseño                | Mora Alejandro Angel Gabriel (Líder)        | Sierra Sosa David Alejandro                 |
| 02 - Diseño · 01 - Jugabilidad                      | Sierra Sosa David Alejandro                 | Nuñez Chan Luis Fernando                    |
| 02 - Diseño · 02 - Mecánicas                        | Sierra Sosa David Alejandro                 | Nuñez Chan Luis Fernando                    |
| 02 - Diseño · 03 - Sistemas                         | Sierra Sosa David Alejandro                 | Nuñez Chan Luis Fernando                    |
| 02 - Diseño · 04 - Controles                        | Sierra Sosa David Alejandro                 | Nuñez Chan Luis Fernando                    |
| 02 - Diseño · 05 - Progresión                       | Sierra Sosa David Alejandro                 | Nuñez Chan Luis Fernando                    |
| 02 - Diseño · 06 - Economía y Recursos              | Sierra Sosa David Alejandro                 | Mora Alejandro Angel Gabriel (Líder)        |
| 02 - Diseño · 07 - Reglas y Estados del Juego       | Sierra Sosa David Alejandro                 | Nuñez Chan Luis Fernando                    |
| 03 - Historia y Mundo · 01 - Premisa y Trama        | Mora Alejandro Angel Gabriel (Líder)        | Nuñez Chan Luis Fernando                    |
| 03 - Historia y Mundo · 02 - Mundo                  | Obispo Navarro Samuel Alejandro (Artista 2) | Mora Alejandro Angel Gabriel (Líder)        |
| 03 - Historia y Mundo · 03 - Personajes             | Cupul Dzul Mirian Leticia (Artista 1)       | Mora Alejandro Angel Gabriel (Líder)        |
| 03 - Historia y Mundo · 04 - Diálogos y Narrativa   | Nuñez Chan Luis Fernando                    | Mora Alejandro Angel Gabriel (Líder)        |
| 04 - Contenido · 01 - Estructura del Contenido      | Sierra Sosa David Alejandro                 | Mora Alejandro Angel Gabriel (Líder)        |
| 04 - Contenido · 02 - Niveles                       | Sierra Sosa David Alejandro                 | Obispo Navarro Samuel Alejandro (Artista 2) |
| 04 - Contenido · 03 - Enemigos y NPC                | Sierra Sosa David Alejandro                 | Nuñez Chan Luis Fernando                    |
| 04 - Contenido · 04 - Objetos y Coleccionables      | Sierra Sosa David Alejandro                 | Nuñez Chan Luis Fernando                    |
| 04 - Contenido · 05 - Misiones y Desafíos           | Sierra Sosa David Alejandro                 | Mora Alejandro Angel Gabriel (Líder)        |
| 05 - Arte y Audio · 01 - Dirección de Arte          | Cupul Dzul Mirian Leticia (Artista 1)       | Obispo Navarro Samuel Alejandro (Artista 2) |
| 05 - Arte y Audio · 02 - Personajes y Animación     | Cupul Dzul Mirian Leticia (Artista 1)       | Obispo Navarro Samuel Alejandro (Artista 2) |
| 05 - Arte y Audio · 03 - Entornos                   | Obispo Navarro Samuel Alejandro (Artista 2) | Cupul Dzul Mirian Leticia (Artista 1)       |
| 05 - Arte y Audio · 04 - Interfaz Visual            | Obispo Navarro Samuel Alejandro (Artista 2) | Mora Alejandro Angel Gabriel (Líder)        |
| 05 - Arte y Audio · 05 - Audio y Música             | Nuñez Chan Luis Fernando                    | Obispo Navarro Samuel Alejandro (Artista 2) |
| 05 - Arte y Audio · 06 - Registro visual            | Sierra Sosa David Alejandro                 | Sierra Sosa David Alejandro                 |
| 06 - Experiencia · 01 - UX y Flujo del Jugador      | Nuñez Chan Luis Fernando                    | Sierra Sosa David Alejandro                 |
| 06 - Experiencia · 02 - UI y Menús                  | Nuñez Chan Luis Fernando                    | Sierra Sosa David Alejandro                 |
| 06 - Experiencia · 03 - Accesibilidad               | Nuñez Chan Luis Fernando                    | Sierra Sosa David Alejandro                 |
| 06 - Experiencia · 04 - Guardado y Configuración    | Sierra Sosa David Alejandro                 | Nuñez Chan Luis Fernando                    |
| 07 - Tecnología · 01 - Arquitectura Técnica         | Sierra Sosa David Alejandro                 | Nuñez Chan Luis Fernando                    |
| 07 - Tecnología · 02 - Motor y Herramientas         | Sierra Sosa David Alejandro                 | Mora Alejandro Angel Gabriel (Líder)        |
| 07 - Tecnología · 03 - Requisitos Técnicos          | Sierra Sosa David Alejandro                 | Nuñez Chan Luis Fernando                    |
| 07 - Tecnología · 04 - Datos y Persistencia         | Sierra Sosa David Alejandro                 | Nuñez Chan Luis Fernando                    |
| 07 - Tecnología · 05 - Seguridad y Riesgos Técnicos | Sierra Sosa David Alejandro                 | Mora Alejandro Angel Gabriel (Líder)        |
| 08 - Producción · 01 - Alcance                      | Mora Alejandro Angel Gabriel (Líder)        | Sierra Sosa David Alejandro                 |
| 08 - Producción · 02 - Roles y Responsabilidades    | Mora Alejandro Angel Gabriel (Líder)        | Sierra Sosa David Alejandro                 |
| 08 - Producción · 03 - Roadmap                      | Mora Alejandro Angel Gabriel (Líder)        | Sierra Sosa David Alejandro                 |
| 08 - Producción · 04 - Backlog del Proyecto         | Mora Alejandro Angel Gabriel (Líder)        | Sierra Sosa David Alejandro                 |
| 08 - Producción · 05 - Gestión de Cambios           | Mora Alejandro Angel Gabriel (Líder)        | Sierra Sosa David Alejandro                 |
| 09 - Calidad · 01 - Criterios de Calidad            | Nuñez Chan Luis Fernando                    | Sierra Sosa David Alejandro                 |
| 09 - Calidad · 02 - Pruebas                         | Nuñez Chan Luis Fernando                    | Sierra Sosa David Alejandro                 |
| 09 - Calidad · 03 - Bugs y Seguimiento              | Nuñez Chan Luis Fernando                    | Sierra Sosa David Alejandro                 |
| 09 - Calidad · 04 - Criterios de Aceptación         | Nuñez Chan Luis Fernando                    | Sierra Sosa David Alejandro                 |
| 10 - Referencia · 01 - Glosario                     | Nuñez Chan Luis Fernando                    | Sierra Sosa David Alejandro                 |
| 10 - Referencia · 02 - Referencias e Inspiración    | Nuñez Chan Luis Fernando                    | Mora Alejandro Angel Gabriel (Líder)        |
| 10 - Referencia · 03 - Registro de Decisiones       | Mora Alejandro Angel Gabriel (Líder)        | Sierra Sosa David Alejandro                 |
| 10 - Referencia · 04 - Historial de Versiones       | Mora Alejandro Angel Gabriel (Líder)        | Nuñez Chan Luis Fernando                    |

El reparto cubre los 48 apartados de contenido de las secciones 01 a 10. Por asignación expresa de David Alejandro Sierra Sosa, Registro visual queda a su cargo como redactor y revisor.

Los apoyos y el entregable específico de cada archivo se detallan en la matriz de reparto. Cada área proporciona sus referencias al responsable que las consolida en `10 - Referencia/02 - Referencias e Inspiración.md`.

## Dependencias para avanzar

1. El líder cierra con el equipo qué incluye la entrega, el MVP y el juego final.
2. David entrega las reglas y el catálogo de contenido con valores canónicos.
3. Los artistas y audio documentan presentación y recursos necesarios usando esas reglas.
4. Luis Fernando (Audio y apoyo general) redacta UX y QA; David verifica las reglas y los resultados esperados.
5. El líder integra observaciones, actualiza la versión y prepara la entrega al profesor.

El trabajo puede avanzar en paralelo en las partes que no dependan de una decisión pendiente. Una modificación de balance o alcance debe registrarse y comunicarse para que no queden valores distintos en dos páginas.

## Índice y biblioteca

El líder mantiene `00 - Índice.md`, la navegación y la autoría de la guía. La sección 11 es biblioteca de consulta; se conserva como referencia y no se reparte como siete tareas nuevas de diseño del juego.

## Créditos del proyecto y licencias de terceros

### Equipo de desarrollo (*EchoScrap*)

- **Mora Alejandro Angel Gabriel:** liderazgo, coordinación general del proyecto, producción, alcance, control de entrega y documentación.
- **Sierra Sosa David Alejandro:** programación principal en Unity (C# / URP), arquitectura técnica, diseño de sistemas, mecánicas, balance, guardado, ranking y registro visual.
- **Cupul Dzul Mirian Leticia:** dirección de arte, diseño conceptual, sprites y animaciones de personajes, enemigos y coleccionables.
- **Obispo Navarro Samuel Alejandro:** diseño de entornos 3D, arte de escenarios, interfaz visual y apoyo en producción sonora.
- **Nuñez Chan Luis Fernando:** composición musical y diseño de audio, narrativa integrada, experiencia de usuario (UX/UI) y verificación de calidad (QA).

### Motor, servicios y recursos con licencia externa

- **Motor y paquetes:** Unity `6000.6.3f1`, Universal Render Pipeline (`URP 17.6.0`), `Input System 1.20.0`, `Cinemachine 6.6.0` y Firebase Realtime Database (ranking en vivo en `https://echoscrap.web.app`).
- **Tipografías:** *Pixelify Sans* (Copyright 2021 The Pixelify Sans Project Authors) y *Chakra Petch*, bajo licencia abierta **SIL Open Font License v1.1** (`Assets/StreamingAssets/Licenses/`).
- **Muestras de audio base (`CC0 1.0 Universal`):** grabaciones de microondas (`pinto0lucas`, Freesound `#422817`) y contador Geiger (`cookies+policy`, Freesound `#555923`) documentadas en `Assets/_Project/Resources/Audio/Flashlight/Sources.txt`, además de capas libres procesadas por el equipo según la tabla de [Audio y música](05%20-%20Arte%20y%20Audio/05%20-%20Audio%20y%20M%C3%BAsica.md).

> **Navegación:** [[00 - Índice]] · ← [[01 - Alcance]] · [[03 - Roadmap]] →

---

# Hitos del desarrollo documentado

| Periodo                               | Resultado incorporado al juego                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 21–22 de octubre                      | Proyecto Unity, locomoción, cámara, salud, dash y linterna.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| 22 de octubre                         | Cinco enemigos de horda, XP, botiquines, cartas, oleadas, Gólgora, HUD y cierre de partida.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| 23–24 de octubre                      | Ajustes de aparición y balance, sprites animados, iluminación, cartas y presentación de daños.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| 24–25 de octubre                      | Mundo 3D, MataColeccionista, acceso a la bóveda, colisiones y laboratorio de enemigos.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| 25–26 de octubre                      | Catálogo de 35 mejoras, variantes de tres y ocho minutos, interfaz y estadísticas del jugador.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| 26 de octubre                         | Fusión por rareza, colección expuesta, barra de XP, magma, portal y renovación de VFX.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| 27 de octubre                         | Limpieza de material auxiliar del repositorio y actualización del GDD con registros del juego.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| 4–5 de noviembre                      | Menú independiente y carga, quince entidades con poderes propios (`CompanionPowerRuntime`), equipo de tres puestos, fusión de cuatro copias, dos élites con sprites integrados, sprites de Esqueleto, Interceptor y Wendigo, minimapa y audio contextual.                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| 6–8 de noviembre                      | Señalización completa de la bóveda (`EQUIPO 1..3`, `VaultDirections`, `VaultTips`), **Índice de Colección (`[I]`)**, capacidad máxima estricta de **10 unidades**, liberación directa en pedestales de planta baja (`E · LIBERAR`), fusión fija de **30 s (`00:30`)**, menú de pausa en 3 pestañas (`General`, `Interfaz`, `Sonido`), consulta rápida `Q`, tipografía unificada `Pixelify Sans` y ranking con Firebase (`EventLeaderboardSync`).                                                                                                                                                                                                                                                                                      |
| 10–13 de noviembre                    | Entrega de cierre `v0.6.12` (`main` @ `734e2a5` / `develop` @ `6973556`): ranking `rulesVersion = 2`, acumulación `Copies(id)`, fusión homogénea/mixta, panel unificado `MenuSettingsView`, mando completo, audio, publicación en **itch.io**, tráiler `1080p` y cierre técnico `TECH-01`. |
| **14–16 de noviembre**                | **Sincronización `v0.7.1` (`develop` @ `8047d90` + `feature/mejoras-y-allin-mitica` @ `b543684`):** **3 ranuras de guardado** con migración automática desde `PlayerPrefs` (`SaveSlotService`), **snapshots de expedición cada `3 s`** con reanudación automática (`ExpeditionSnapshotService`), ranking ampliado a **todos los intentos válidos** conservando el mejor puntaje por gamertag, bóveda ampliada a **18 cápsulas** (`3` equipo + `15` exposición) con la máquina de fusión en la pared trasera `(189, 0, 28,88)` y texturizado/colisiones de roca, **selector `Límite de FPS` (`30 / 60 / 120`)** y **selector `Pantalla` (`COMPLETA` / `VENTANA`)** con pantalla completa sin bordes por defecto, limpieza de `_EmissionColor` en enemigos y proyección sobre relieve de auras/zonas sin GC, deslizamiento suave junto a rocas bajas, optimizaciones de navegación (`EnemyObstacleNavigator`), GPU instancing, LODs/texturas e iluminación de bóveda (`WarehouseLightScope`), expansión del catálogo de mejoras con **`8` cartas contextuales** (`1239072`) + **`19` cartas nuevas** + **rareza `Mítica` (*All In*, `MYTH_01` con marco lit `Frame_Mythic`)** + reelaboración de **`DASH_05` (*Paso de ariete*)** a distancia de dash = **62 mejoras en cinco rarezas**, y compilaciones verificadas **Windows x64** y **macOS Universal (`x86_64` + `arm64`)**. |

## Estado final de hitos (cierre v1.0)

| Hito | Responsable según el reparto | Evidencia de cierre | Estado |
| --- | --- | --- | --- |
| Validación técnica y cierre de entrega (`TECH-01`) | David Alejandro Sierra Sosa y Luis Fernando Nuñez Chan | Build Windows x64 Release (`Windows.asset`, `0` errores, `~0,94 GiB`), `p95` de CPU de `6,61 ms` (`2 000` fotogramas), optimizaciones de horda/memoria en `main` @ `734e2a5` y publicación ejecutable en [`https://naked-hiro.itch.io/echoscrap`](https://naked-hiro.itch.io/echoscrap). Limitación de hardware mínimo sin telemetría dedicada documentada en [[03 - Bugs y Seguimiento]]. | **Cerrado (v1.0)** |

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Entregas y trabajo pendiente

| Área | Estado actual (`develop` @ `b543684`) |
| --- | --- |
| Flujo, guardado y ranking | Menú con selector de **3 ranuras de guardado** (`CONTINUAR` / `NUEVA PARTIDA` en `SaveSlotService`), migración automática desde `PlayerPrefs`, snapshots cada `3 s` (`ExpeditionSnapshotService`), campo `NOMBRE DEL JUGADOR (RANKING)...` en `NUEVA PARTIDA · ARCHIVO N` (`GamertagRegistry`), preparación en bóveda, elección de ronda (`ELIGE LA RONDA`), expedición y ranking `rulesVersion = 2` publicando **todos los intentos válidos** conservando la mejor puntuación por gamertag en `https://echoscrap.web.app`. |
| Señalización de bóveda e Índice | Rótulos 3D `EQUIPO 1..3`, flechas direccionales superiores (`VaultDirections`), panel de consejos (`VaultTips`), paredes con basalto pixel art, fracturas de roca con relieve, bloqueo bajo escalera, iluminación `WarehouseLightScope` e **Índice de Colección (`[I]` / `Select`)** con pestañas `POR RAREZAS` (`TODAS` + 6 categorías) y `EN TU BÓVEDA`. |
| Equipo y liberación | Tres puestos en el balcón (`E` / botón oeste para editar, botones `QUITAR DEL PUESTO`, `LIBERAR COPIA`, `CERRAR · E / ESC`, `T` / botón norte para desequipar rápido) y **15 puestos de exposición** en planta baja (`E · LIBERAR`, `slot >= 3`), con **poderes acumulables por copia equipada** y totales dinámicos en HUD (`DescribeHudPower`). |
| Colección y fusión | Quince entidades descubribles, capacidad máxima estricta de **18 unidades** (`DisplaySlots[18]`, aviso `INVENTARIO LLENO`), máquina reubicada en la pared trasera (`189, 0, 28,88`), fusión de 4 copias de la misma rareza o de **rarezas mixtas** (incluidas `EN EQUIPO 1/2/3`, excluida Yanikua), duración fija de **30 s (`00:30`)** y botón único `COLECCIONAR`. |
| Combate y mejoras | Cinco arquetipos de horda (exclusión de interiores cerrados via `OverlapsBlockedInterior`, presupuesto por frame en `EnemyObstacleNavigator`, deslizamiento en rocas bajas e instanciación GPU), dos élites, linterna automática (`FLASH_05` aplica empuje en cada pulso; `FLASH_03`, `BIO_02` y `CRIT_04` en el cuarto pulso), `0,22 s` de invulnerabilidad en dash y **62 mejoras en 5 rarezas** (incluyendo *All In* Mítica, `DASH_05` reelaborada, 8 cartas contextuales y 19 cartas con pooling sin GC). |
| Gólgora | Caldera de `22 m`, seis patrones unificados en `GolgoraCombatSettings.cs` y `CombatSettings.asset`, dos fases y refuerzos anunciados. |
| Interfaz, audio y distribución | Tipografía **`Pixelify Sans`**, panel unificado `MenuSettingsView` en 3 pestañas (`General` con `Límite de FPS` `30 / 60 / 120` en pausa y **`Pantalla` `COMPLETA` / `VENTANA`** en ambos menús, `Interfaz` y `Sonido`), cuadrícula HUD `9 × 34 px` + `22` estadísticas, consulta rápida `Q` / `Select`, audio integrado, icono de escritorio `EchoScrap-AppIcon.png`, limpieza segura en `GolgoraMagmaRingPool`, build publicada en [`https://naked-hiro.itch.io/echoscrap`](https://naked-hiro.itch.io/echoscrap) y compilaciones verificadas **Windows x64** y **macOS Universal (`x86_64` + `arm64`)**. |

## Integraciones cerradas (`0.6.2` – `0.7.1`)

| ID | Tarea | Estado | Comprobación en código (`8047d90` / `b543684`) |
| --- | --- | --- | --- |
| UX-01 | Guiar la preparación de entidades y navegación en la bóveda. | Integrado | Rótulos 3D `EQUIPO 1..3` (`CollectibleDisplay`), indicadores direccionales `VaultDirections` y consejos `VaultTips` cada `8 s` (`GameMenuView`). |
| VAULT-INDEX-01 | Implementar Índice de Colección (`[I]`) y ampliar bóveda a 18 unidades en pared trasera. | Integrado | `VaultCollectionIndex.cs` (`TODAS` + 6 rarezas y `EN TU BÓVEDA`), `FusionProgress.Capacity = 18`, `FusionStation` en `(189, 0, 28,88)` y `VaultReleaseInteraction.cs`. |
| SAVE-SLOTS-01 | Implementar 3 ranuras atómicas, migración de `PlayerPrefs` y snapshots cada `3 s`. | Integrado | `SaveSlotService.cs` (`Saves/slot_0..2.json`, `index.json`, `ranking_outbox.json`) y `ExpeditionSnapshotService.cs`. |
| SETTINGS-FPS-01 | Añadir selector de límite de FPS (`30 / 60 / 120`) en pausa y optimizaciones GPU/nav/bóveda. | Integrado | `MenuSettingsView.cs` (`EchoScrap.Graphics.FrameRateLimit`), `EnemyObstacleNavigator.cs`, `WarehouseLightScope.cs` y `CombatBenchmarkRunner.cs`. |
| FUSION-30S-01 | Unificar duración de fusión a `30 s`, permitir copias equipadas y rarezas mixtas. | Integrado | `FusionRecipe.FusionSeconds = 30`, soporte de rarezas mixtas (`qualityMean`), `EN EQUIPO 1/2/3`, porcentajes en UI (`60%`, `40%`, `1%`) y `COLECCIONAR`. |
| UI-PAUSE-01 | Compartir ajustes en 3 pestañas, consulta rápida `Q`/`Select` y tipografía `Pixelify Sans`. | Integrado | `MenuSettingsView.cs` en Menú Inicial y Pausa (`General`, `Interfaz`, `Sonido`) y `CombatHud.IsHudDetailHeld`. |
| EVENT-RANK-01 | Sincronizar todos los intentos válidos en Firebase conservando la mejor puntuación por gamertag (`rulesVersion = 2`). | Integrado | `GamertagRegistry.cs` (`if-match: null_etag`) y `EventLeaderboardSync.cs` (`Saves/ranking_outbox.json`, `PUT`/`DELETE`, `8 s` timeout). |
| UPGRADES-62-01 | Añadir 8 cartas contextuales + 19 mejoras nuevas, rareza Mítica (*All In*) y reelaborar `DASH_05`. | Integrado | `UpgradeCatalog.cs` (`62` cartas, `UpgradeRarity.M`), `UpgradeCombatRuntime.cs` (`0` GC bytes bajo estrés) y `Frame_Mythic.png`/`Frame_Mythic.mat`. |
| INPUT-01 | Cubrir con mando las interacciones de bóveda, recogida y bestiario (`E`, `T`, `I`). | Integrado | `CompanionSystem`, `FusionStation`, `VaultReleaseInteraction`, `CollectibleDisplay` (`buttonWest`), `VaultReleaseInteraction` (`buttonNorth`) y `VaultCollectionIndex` (`selectButton`). |
| ART-ELITE-01 / 02 | Conectar sprites y ciclos de combate de Caballero Fantasma y Centinela de Cobre. | Integrado | Atlas de 224 y 288 fotogramas integrados con mapas de normales y emisión. |
| COMBAT-SOLAR-01 | Aplicar empuje en cada pulso de *Rayo solar* (`FLASH_05`) manteniendo `FLASH_03`, `BIO_02` y `CRIT_04` en el cuarto pulso. | Integrado | `FlashlightWeapon.cs:402` (`capturedSolarMode`) y `UpgradeCombatRuntime.cs:232-236, 341` (`PulseIndex == 4`). |
| MEDIA-TRAILER-01 | Grabar, montar y validar el tráiler promocional final de *EchoScrap*. | Integrado y aprobado en QA | [`Media/Trailer/EchoScrap-Trailer-Final-1080p.mp4`](Media/Trailer/EchoScrap-Trailer-Final-1080p.mp4) (`1920 × 1080`, `46,2 s`, `70,9 MB`), con gameplay desarrollado y audio propio. |
| TECH-01 / PLAT-MAC-01 | Cerrar validación técnica de compilación y rendimiento **Windows x64** y **macOS Universal**. | Compilación verificada y ejecución manual en macOS confirmada por el equipo | Paquetes Release verificados `EchoScrap-Windows-x64-62mejoras.zip` (`0x8664`, `0` errores en humo, arranque fullscreen `1920 × 1080`) y `EchoScrap-macOS-Universal-62mejoras.zip` (`x86_64` + `arm64`, `-rwxr-xr-x`). |
| SETTINGS-DISPLAY-01 | Añadir selector **`Pantalla` (`COMPLETA` / `VENTANA`)**, limpiar `_EmissionColor` en enemigos y proyectar anillos de área sobre terreno. | Integrado | `MenuSettingsView.cs` (`EchoScrap.Graphics.FullScreen`), `MothSpriteView.cs`, `MortarSpriteView.cs`, `EnemySpriteView.cs`, `EnemyActor.cs` y `UpgradeEffectPool.ProjectGroundRing` (`0` GC bytes). |

## Estado de cierre del backlog

El contenido previsto está integrado, pero el cierre de calidad sigue pendiente: la suite completa conserva fallos que requieren revisión de fixtures, expectativas y comportamiento de terreno/colisiones. Los resultados y el alcance de las pruebas figuran en [[02 - Pruebas]] y [[03 - Bugs y Seguimiento]]. También falta telemetría en un equipo físico de perfil mínimo. La ejecución nativa en macOS sí fue realizada por un integrante del equipo, según confirmación del equipo del 16 de noviembre; se registra como validación manual, complementaria a la verificación del paquete Universal.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Gestión de Cambios

## Descripción de la sección

El diseño del videojuego evoluciona con cada integración en `develop`. Esta sección establece el protocolo para evaluar y registrar modificaciones manteniendo la coherencia total entre los apartados del GDD.

## Flujo para aprobar un cambio

1. **Propuesta:** cualquier integrante identifica una necesidad de ajuste (de balance, alcance, arte o interfaz) y describe el motivo y los archivos afectados.
2. **Validación técnica y de reglas:** Sierra Sosa David Alejandro verifica cómo impacta el cambio en las estadísticas, las 62 mejoras, la colección, el rendimiento o la persistencia.
3. **Coordinación y cierre:** Mora Alejandro Angel Gabriel (Líder) confirma que el cambio respeta el alcance de entrega y coordina la actualización simultánea en todos los documentos relacionados.

## Registro de cambios aplicados al diseño

| Fecha | Cambio | Motivo | Impacto en otros sistemas | Responsable | Estado |
|---|---|---|---|---|---|
| **15/10/2026** | Unificación de historia de origen del Coleccionista (humano atrapado por un pacto engañoso). | Alinear `01 - Visión` y `03 - Historia y Mundo` bajo una sola premisa narrativa. | Fichas de lore y descripción general del protagonista. | Mora Alejandro Angel Gabriel / Nuñez Chan Luis Fernando | Histórico |
| **15/10/2026** | Eliminación de divisas residuales y fijación de hasta 3 entidades equipables. | Simplificar la meta-progresión entre incursiones. | `02 - Diseño`, `04 - Contenido`, `05 - Interfaz Visual`, `06 - Experiencia` y `07 - Datos y Persistencia`. | Sierra Sosa David Alejandro / Mora Alejandro Angel Gabriel | Histórico |
| **16/10/2026** | Invocación periódica de esbirros por Gólgora cada `4,0 s`. | Mantener activas las mejoras de área, curación y recolección de XP durante el jefe final. | `02 - Sistemas`, `04 - Niveles` y `04 - Enemigos y NPC`. | Sierra Sosa David Alejandro | Histórico |
| **04–05/11/2026** | Catálogo de 15 entidades con poderes propios (`CompanionPowerRuntime`), fusión de 4 copias, élites con sprites integrados y cámara exterior a `35,5°`. | Consolidar el bucle de colección y combate de la versión `0.6.0`. | Todas las secciones de diseño, contenido, arte y tecnología. | Sierra Sosa David Alejandro / Equipo EchoScrap | Integrado |
| **08/11/2026** | Señalización completa de bóveda (`EQUIPO 1..3`, `VaultDirections`, `VaultTips`), Índice de Colección (`[I]`), capacidad máxima de `10` unidades (`3` equipo + `7` exposición), liberación en pedestales (`E · LIBERAR`), fusión fija de `30 s` con copias equipadas y botón único `COLECCIONAR`, pausa en 3 pestañas, consulta rápida `Q`, tipografía `Pixelify Sans` y ranking con Firebase (`EventLeaderboardSync`). | Sincronizar el GDD `0.6.2` con el código fuente verificado de `develop` (`9eed59f`). | Sincronización transversal de las secciones `00` a `10`. | Equipo EchoScrap | Integrado (`0.6.2`) |
| **10/11/2026** | Reglas definitivas de puntuación del Ranking (`rulesVersion = 2`, una oportunidad por nueva colección, reserva de gamertag por SHA-256 en `GamertagRegistry`, comparación `3 min` vs `8 min ÷ 2`) y visor web en vivo [`https://echoscrap.web.app`](https://echoscrap.web.app); documentación del Bestiario (`Índice de Colección [I]`) con captura de su fase beta; e integración de los 22 bocetos a mano (`Media/ConceptArt/`), 4 pistas musicales y 15 efectos de sonido con tabla de autoría. | Sincronizar el GDD `0.6.4` con `develop` (`a8a23c4`) y completar todos los criterios de arte, audio y ranking de la entrega final. | `02 - Economía y Recursos`, `04 - Objetos y Coleccionables`, `05 - Personajes y Animación`, `05 - Audio y Música`, `06 - UI y Guardado` y `07 - Tecnología`. | Equipo EchoScrap | Integrado (`0.6.4`) |
| **13/11/2026** | Publicación de la build en **itch.io** (`https://naked-hiro.itch.io/echoscrap`); sustitución de capturas por las definitivas de Unity (`Media/Interfaz/`); incorporación de bocetos de Esqueleto, Galgo y Joel más galería de elementos (`Media/ConceptArt/Elementos/`); y actualización de paridad final con `main` @ `b381ef4` / `develop` @ `977bac5`: poderes de acompañantes acumulables por copia equipada (`Copies(id)`), fusión de 4 copias de la misma rareza o de rarezas mixtas, panel unificado de ajustes en 3 pestañas (`MenuSettingsView.cs`) compartido entre menú principal y pausa, controles completos de mando en bóveda/Índice (`buttonWest`, `buttonNorth`, `selectButton`), persistencia en `EchoScrap.Rank.v2` (`PUT`/`DELETE`, `8 s`, ID de 32 hex, preservación de oportunidad puntuable al continuar hasta la primera ronda finalizada, gamertag en mayúsculas conservando tildes), calibración unificada de enemigos/Gólgora y `0,22 s` de invulnerabilidad de dash. | Culminar la documentación final, auditoría de paridad y criterios de entrega. | Transversal a todas las secciones (`00` a `10`) y documento compilado para PDF. | Mora Alejandro Angel Gabriel / Equipo EchoScrap | Integrado (`0.6.6`) |
| **13/11/2026** | Integración y aprobación QA del tráiler promocional final (`Media/Trailer/EchoScrap-Trailer-Final-1080p.mp4`, `1920 × 1080`, `46,2 s`); ampliación de procedencia sonora con todos los clips de `Resources/Audio/` y `MataColeccionista/Audio/`; resumen rápido de juego y avance en `02 - Diseño/04 - Controles.md`; registro de créditos de desarrollo y licencias de terceros (`Pixelify Sans` OFL, `Chakra Petch` OFL, `Flashlight/Sources.txt` CC0); y restablecimiento transversal del soporte completo de **teclado/ratón y mando**. | Cerrar el hito `MEDIA-TRAILER-01`, dejar listos los apartados de entrega y confirmar compatibilidad total con teclado/ratón y mando. | `01 - Ficha`, `02 - Controles`, `05 - Audio`, `08 - Producción`, `09 - Pruebas` y `10 - Referencia` (transversal). | Mora Alejandro Angel Gabriel / Equipo EchoScrap | Integrado (`0.6.9`) |
| **13/11/2026** | Integración de identidad visual (`Media/Identidad/`: logo SVG, título SVG/`.aseprite`, portada y tarjeta) en `05 - Arte y Audio/01 - Dirección de Arte.md`; respaldo íntegro de los clips de audio del motor (`Media/Audio/Unity/Flashlight/`, `MataColeccionista/` y `Sistema/` + `Sources.txt` CC0); y **cierre técnico `TECH-01`** (`Windows.asset` Release, build `~0,94 GiB` con `0` errores, `p95` de CPU `6,61 ms`, optimizaciones de memoria/tirones en Windows y registro transparente de limitaciones conocidas para gama mínima y boceto 3D directo de Gólgora). | Completar los entregables de arte digital, audio y control de calidad y cerrar todos los hitos técnicos de entrega. | `05 - Arte`, `07 - Tecnología`, `08 - Producción`, `09 - Calidad` y `10 - Referencia`. | Mora Alejandro Angel Gabriel / Sierra Sosa David Alejandro / Nuñez Chan Luis Fernando | Integrado (`0.6.10`) |
| **13/11/2026** | Integración del boceto conceptual a mano del jefe final **Gólgora** ([`Media/ConceptArt/Golgora_guardian.jpeg`](Media/ConceptArt/Golgora_guardian.jpeg)) en `05 - Arte y Audio/02 - Personajes y Animación.md` junto con la referencia de sus archivos 3D (`MataColeccionista.fbx`, `Martillo.fbx`, texturas), completando el 100% de bocetos de personajes, enemigos y jefe. | Cubrir por completo el criterio de bosquejos y diseño conceptual de la entrega final. | `05 - Arte y Audio` y `09 - Bugs y Seguimiento`. | Mora Alejandro Angel Gabriel / Equipo EchoScrap | Integrado (`0.6.11`) |
| **13/11/2026** | Auditoría total de paridad contra `EchoScrap` (`main` @ `734e2a5` / `develop` @ `6973556`, commits `84a72c0`..`6973556`). | Garantizar cero contradicciones entre el GDD `0.6.12` y el código fuente antes de cerrar en `main`. | Transversal a la raíz, secciones `01` a `10` y `00 - GDD Completo`. | Equipo EchoScrap | Histórico (`0.6.12`) |
| **16/11/2026** | Sincronización total contra `develop` @ `8047d90` + `feature/mejoras-y-allin-mitica` @ `ebf90f9`: **3 ranuras de guardado** (`SaveSlotService`), **snapshots de expedición cada `3 s`** (`ExpeditionSnapshotService`), ranking de **todos los intentos válidos** por mejor puntuación, **18 cápsulas** en bóveda (`3` + `15`) con máquina de fusión en pared trasera (`189, 0, 28,88`) y mejoras visuales/colisiones, selector **`Límite de FPS` (`30 / 60 / 120`)**, deslizamiento en rocas bajas, optimizaciones de navegación/GPU/texturas/bóveda, **62 mejoras en 5 rarezas** (`8` cartas contextuales + `19` nuevas + **rareza Mítica `MYTH_01` *All In*** con `Frame_Mythic` + reelaboración de `DASH_05`), panel de `22` estadísticas, cuadrícula HUD `9 × 34 px` y builds **Windows x64** y **macOS Universal**. | Mantener juego y GDD 100 % sincronizados tras la integración de `develop` y las nuevas mejoras. | Transversal a la raíz (`README`, `00 - Índice`, `00 - Avance jugable`), secciones `01` a `10` y `00 - GDD Completo (Para Exportar PDF).md`. | Equipo EchoScrap | Histórico (`0.7.0`) |
| **16/11/2026** | Parche de paridad `0.7.1` (`feature/mejoras-y-allin-mitica` @ `b543684`): selector **`Pantalla` (`COMPLETA` / `VENTANA`)** en `MenuSettingsView` (`EchoScrap.Graphics.FullScreen`) con reaplicación tras carga de escena en Standalone (`1920 × 1080` sin bordes por defecto), limpieza de `_EmissionColor` en `MothSpriteView` / `MortarSpriteView` / `EnemySpriteView` y exclusión de destello blanco en ticks continuos (`StaticAura`, `PlasmaPuddle`, `FlameTrail`), proyección sobre relieve (`12 × 12` sin GC en `UpgradeEffectPool.ProjectGroundRing`) para *Aura estática* y zonas en suelo, y reconstrucción de los builds **Windows x64** y **macOS Universal**. | Resolver ventana forzada por registro heredado, tinte blanquecino persistente de enemigos y recorte de anillos de área en laderas. | `01 - Visión`, `02 - Diseño`, `05 - Interfaz Visual`, `06 - Experiencia`, `07 - Tecnología`, `08 - Producción`, `09 - Calidad`, `10 - Referencia` y compilado PDF. | Equipo EchoScrap | Histórico (`0.7.1`) |

## Revisión 0.6.2 del 8 de noviembre (`develop` @ `9eed59f`)

La versión `0.6.2` cierra la brecha de señalización en la bóveda (`UX-01`) documentando los sistemas ya implementados en código: rótulos 3D `EQUIPO 1..3`, tarjetas direccionales superiores (`VaultDirections`), consejos rotatorios cada `8 s` (`VaultTips`) e **Índice de Colección (`[I]`)**. Asimismo, actualiza las reglas exactas de capacidad (`10` unidades totales correspondientes a `displays[10]`), la duración fija de `30 s` en todas las recetas de la máquina de fusión, el uso de copias equipadas como ingredientes (`EN EQUIPO 1/2/3`), el menú de pausa en tres pestañas (`General`, `Interfaz`, `Sonido`), la consulta rápida del HUD con `Q`, la tipografía unificada `Pixelify Sans`, la sincronización del ranking con Firebase (`EventLeaderboardSync`) y las interacciones de combate (`FLASH_05`, `DASH_04`, `DEMO_03/04`, `SCRAP_03/04`, orden de mitigación en `PlayerHealth`, Polilla Reactor y Gólgora).

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Criterios de calidad

El diseño se revisa junto con el control, la interfaz y la persistencia. Las opciones deben reflejar su efecto inmediato, los avisos de peligro deben preceder al daño y la colección debe respetar su capacidad máxima de 18 unidades por ranura y conservar permanentemente los descubrimientos en el índice.

| Aspecto | Comprobación (`8047d90` / `b543684`) |
| --- | --- |
| Preparación y guía en bóveda | El jugador localiza el balcón de equipo, la máquina de fusión y los puestos de exposición gracias a los rótulos 3D (`EQUIPO 1..3`), las flechas direccionales superiores (`VaultDirections`), el panel de consejos (`VaultTips`) y el **Índice de Colección (`[I]`)**. |
| Control y consulta rápida | Movimiento, dash (`Espacio` / `A`), consulta rápida (`Q` / `Select`) y controles de bóveda/arena/bestiario (`E` / botón oeste, `T` / botón norte, `I` / `Select`) operan tanto en teclado/ratón como en mando. |
| Lectura y pausa | Ataques anunciados, resistencias élite y cambios de fase legibles tanto al `100%` como con `Efectos decorativos` / `Efectos reducidos` en el panel compartido `MenuSettingsView` (3 pestañas); tipografía unificada `Pixelify Sans`. |
| Progresión y combate | Cartas válidas y niveles propios; `FLASH_05` aplica empuje en cada pulso (`capturedSolarMode`) y `FLASH_03`, `BIO_02` y `CRIT_04` mantienen el disparo en el cuarto pulso (`PulseIndex == 4`); `DASH_04` fija `0,5 s` de recarga conservando el `+8%/nivel` de velocidad de `DASH_01`; `DASH_05` (*Paso de ariete*) amplía distancia sin daño; `MYTH_01` (*All In*) aleatoriza cartas poseídas con `+1` nivel dentro de su tope sin recursión; mitigación del daño entrante en `PlayerHealth`; poderes de acompañantes **acumulables por copia equipada** y mostrados con su total en HUD (`DescribeHudPower`). |
| Colección y capacidad | Capacidad máxima estricta de **18 unidades** (`DisplaySlots[18]`: `3` de equipo + `15` de exposición), bloqueo en `18/18` en arena (`INVENTARIO LLENO / LIBERA ESPACIO`) y en fusión (`INVENTARIO LLENO · LIBERA ESPACIO`), y liberación con confirmación conservando `discovered[15]`. |
| Fusión | Duración fija de **30 segundos (`00:30`)** en recetas homogéneas o de **rarezas mixtas**, selección permitida de copias equipadas (`EN EQUIPO 1/2/3`) con desequipado automático al confirmar, exclusión de Yanikua como ingrediente, probabilidades enteras en UI (`60%`, `40%`, `1%`) y botón único `COLECCIONAR`. |
| Sesión y ranking | Pausa por máscaras independientes, selector **`Límite de FPS` (`30 / 60 / 120`)**, selector **`Pantalla` (`COMPLETA` / `VENTANA`)** con arranque automático en pantalla completa sin bordes, visitas a bóveda, reanudación automática por snapshot cada `3 s`, traslado a caldera y cierre de partida coherentes; reserva de gamertag (`GamertagRegistry`) y sincronización no bloqueante (`PUT`/`DELETE`, `8 s`, `Saves/ranking_outbox.json`) con Firebase (`EventLeaderboardSync`) para todos los intentos válidos. |
| Persistencia | **3 ranuras atómicas** (`SaveSlotService`, `Saves/slot_0..2.json`, `FusionProgress` v5 con migración desde `v4`/`PlayerPrefs`), `FusionPhase` de 5 estados, `expedition_0..2.json` con recuperación segura, volúmenes, tope de FPS y modo de pantalla en `PlayerPrefs` (`MenuSettingsView`) y cola/nombre de ranking persistidos. |

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Pruebas

El proyecto dispone de pruebas automatizadas de locomoción, dash, daño, navegación, oleadas, élites, jefe, cartas, recogida, pausas, menús, bóveda, colección y fusión, además del laboratorio interactivo (`EnemyLaboratory`) para generar e inspeccionar los ocho arquetipos.

## Verificación actual de paridad, pruebas automatizadas y builds (`develop` @ `b543684`, 16 de noviembre de 2026)

Esta revisión valida el código fuente, las escenas (`CombatArena.unity`, `MainMenu.unity`), los assets regenerados (`UpgradeCardSkin.asset`, `Frame_Mythic.png`, `Frame_Mythic.mat`), las pruebas automatizadas sobre el commit final `b543684` y las compilaciones Standalone finales en **Windows x64** y **macOS Universal**:

- **EditMode (ejecución con gráficos habilitados sobre `b543684`): `63 / 63` pruebas aprobadas (`RESULT=Passed`, `TOTAL=63`, `PASSED=63`, `FAILED=0`, `SKIPPED=0`).** *(En ejecución sin dispositivo gráfico `-nographics`, `3` pruebas de `RenderTexture` en `ArenaLightingAuthoringTests` / `EditorPlayerStartupTests` requieren GPU o renderizador activo; con gráficos habilitados la suite completa aprueba al `100%`).*
- **PlayMode (batería focalizada post-`b543684` sobre ajustes de pantalla, sprites/instanciación, mejoras y flujo): `172 / 172` pruebas aprobadas (`0` fallos, `0` omitidas):**
  - `CombatUpgradeDraftTests (31/31)`, `CompleteUpgradeTests (52/52; incluye StaticAuraProjectsOntoTerrainSlopesWithZeroAllocations y prueba de estrés de 180 fotogramas con 150 enemigos en 0,683 ms/paso y 0 bytes asignados en GC)`, `CombatArenaRunFlowTests (16/16; incluye PauseDisplayModeButtonsPersistSelectionAndChainGeneralNavigation)`, `SceneSeparationTests (8/8; valida MenuSettingsView y DisplayMode en Menú Inicial)`, `EnemyMothTests (22/22)`, `MortarSpriteTests (10/10)`, `CharacterInstancingTests (4/4)`, `AcidPresentationTests (4/4)`, `PlayerStatsReadoutTests (9/9)`, `ExpeditionResumeTests (6/6)`, `UpgradeArtDirectionTests (5/5)` y `UpgradeVisualIntegrationTests (5/5)`.
  - En la ejecución previa de la suite completa aislada sobre `ebf90f9` (`712` totales · `667` aprobadas · `45` fallidas · `0` omitidas, frente a `684` / `633` / `51` en `develop`), los `45` fallos correspondieron íntegramente a pruebas preexistentes de sandbox/terreno/autoría ajenas a estas entregas: `CombatHealthSessionTests` (`8`), `CombatSandboxSetupTests` (`1`), `EnemyLaboratoryTests` (`9`), `EnemyMortarTests` (`1`), `EnemyResidualTests` (`1`), `FusionIntegrationTests.VaultMachineInCentralAisleAndIndexByRarity` (`1`, esperaba la coordenada anterior `188.3466f` frente a `189f` introducida por `8c0dfae`), `LocomotionTests` (`16`), `PlayerReliefScaleTests` (`3`), `PlayerTreeTraversalTests` (`1`), `WarehousePresentationTests` (`1`) y `WorldArtIntegrationTests` (`3`).
- **Compilaciones Release y pruebas de humo sobre `b543684`:**
  - **Windows x64 (`EchoScrap-Windows-x64-62mejoras.zip`, `145 267 061` bytes):** cabecera PE x64 (`0x8664`), ejecución real verificada en modo **`FullScreenWindow` (`1920 × 1080`, `Screenmanager Fullscreen mode = 1`)** sobrescribiendo valores heredados en ventana del registro y cerrando con `0` errores en `SmokeTest_v3.log`.
  - **macOS Universal (`EchoScrap-macOS-Universal-62mejoras.zip`, `153 000 566` bytes):** binario Mach-O Universal (`0xCAFEBABE`, `x86_64` + `arm64`) en `Contents/MacOS/EchoScrap` y `Contents/Frameworks/UnityPlayer.dylib`, con permisos POSIX `-rwxr-xr-x` verificados mediante `Set-ZipMacModes.cjs`. Además, el equipo confirmó el 16 de noviembre de 2026 que un integrante ejecutó el juego en su Mac. Se registra como validación manual reportada por el equipo, complementaria a la comprobación técnica del paquete.

## Auditoría previa de paridad (`main` @ `734e2a5` / `develop` @ `6973556`, 13 de noviembre de 2026)

Se conserva la auditoría histórica de `0.6.12` (previa a la ampliación a `3` ranuras, `18` unidades en bóveda y `62` mejoras en `5` rarezas):

- **Bóveda, colección e Índice (`FusionProgress.cs`, `FusionStation.cs`, `CompanionSystem.cs`, `CompanionView.cs`, `CompanionPowerRuntime.cs`, `VaultReleaseInteraction.cs`, `VaultCollectionIndex.cs`, `GameMenuView.cs`):** verificada la capacidad máxima de `10` unidades (`Capacity = 10`, `DisplaySlots[10]` en `CombatArena`: 3 puestos de equipo en el balcón + 7 puestos de exposición/reserva en planta baja, exhibiendo copias en reserva con `RESERVA` si los puestos de equipo están vacíos), la acumulación real de poderes por copia equipada (`Copies(id)` hasta `×3`), la señalización (`EQUIPO 1..3`, `VaultDirections` con *"Combina 4 copias de varias rarezas"*, los tres `VaultTips` vigentes), el Índice de Colección (`I` / `Select`, filtro `TODAS` + 6 rarezas y `EN TU BÓVEDA`) y el soporte completo de mando (`buttonWest`, `buttonNorth`, `selectButton`).
- **Máquina de fusión (`FusionProgress.cs`, `FusionStation.cs`, `FusionStationUI.cs`):** verificada la duración fija de `30 s` (`FusionSeconds = 30`), la aceptación de **4 copias de la misma rareza o de rarezas mixtas** (`qualityMean = total / 4f`, excluyendo `Yanikua` `id != 4`), las 4 ranuras horizontales (`POSIBLES RESULTADOS`, `VACIAR`, `CONFIRMAR`, `X`), los `5` estados de `FusionPhase` (`Idle`, `Fusing`, `Opening`, `AwaitingDecision`, `Ready`) con `opening` normalizado `float` y recuperación tras reinicio (`RestoreAfterLoading`), la migración desde `EchoScrap.Collection.v1` cuando `owned != null` y el botón único `COLECCIONAR`.
- **Interfaz, menú compartido, tipografía y ranking (`CombatHud.cs`, `GameMenuView.cs`, `MenuSettingsView.cs`, `EventLeaderboardSync.cs`, `GamertagRegistry.cs`):** verificada la unificación de `MenuSettingsView` en 3 pestañas (`General`, `Interfaz`, `Sonido`) compartido entre `CONFIGURACIÓN` del menú inicial y `PAUSA`, el encabezado `ELIGE LA RONDA` sin campo de nombre, la reserva única de alias por SHA-256 (`FormKC` + mayúsculas conservando tildes, `if-match: null_etag`), la preservación de elegibilidad pendiente (`eligible`) al pulsar Continuar hasta cerrar la primera `VICTORIA`/`DERROTA`, y la sincronización HTTPS `PUT` / `DELETE` (`timeout = 8 s`, `attemptId` de 32 hex) respaldada en `EchoScrap.Rank.v2`.
- **Combate, mitigación, locomoción y enemigos (`PlayerLocomotion.asset`, `PlayerCombat_Default.asset`, `PlayerRuntimeStats.cs`, `PlayerHealth.cs`, `UpgradeCombatRuntime.cs`, `UpgradeTuning.asset`, `EnemyArchetypeSettings`, `GolgoraCombatSettings.cs`, `CombatSettings.asset`):** verificada la locomoción base de `8 m/s` (`80 m/s²` / `70 m/s²`), el dash de `22 m/s` (`0,15 s`, `0,22 s` invulnerabilidad, `Espacio` / `A` / `Cruz`), la protección post-golpe de `0,40 s`, `SCRAP_01` (`+10` HP y `+10%` armadura), `SCRAP_03` (`1/2/3` sierras, `18/28/40`), `SCRAP_04` (torreta cada `10 s`, duración `12 s`, máx. `2`, daño `10`, alcance `12 m`, `28 m/s`), `DEMO_03` (`45/70/100` en dash, `3 m`), exclusión de interiores cerrados (`OverlapsBlockedInterior`), optimización de navegación en horda (`EnemyObstacleNavigator`), limpieza segura en `GolgoraMagmaRingPool`, la calibración unificada entre definiciones C# y `.asset` de empuje de enemigos y parámetros de Gólgora (`TransitionDuration = 1,5 s`), y la integración de pistas y clips de audio en Unity.

## Verificaciones previas de pruebas automatizadas y capturas (4 y 5 de noviembre)

- **Edit Mode (`FusionRulesTests`):** 20 pruebas aprobadas sobre reglas de colección y fusión.
- **Play Mode (`EnemySpriteIntegrationTests`, `EliteEnemyTests`, `FlashlightBurstFeedbackTests`, `CompanionPowerTests`, `CompanionIntegrationTests`, `CompleteUpgradeTests`, `CombatArenaRunFlowTests`):** 169 casos aprobados cubriendo integración de sprites de élites, fases de apertura y vulnerabilidad, empuje continuo de *Rayo solar*, poderes de acompañantes, fórmula de *Ojo del verdugo*, catálogo de 35 mejoras y flujo de rondas y reinicios en `CombatArena`.
- **Condiciones de los medios documentales:** los GIFs de atlas se exportaron a escala `2×` desde los JSON de fotogramas, y las capturas en motor de Unity se registraron en sesiones aisladas con cámara de detalle, jugador protegido y oleadas detenidas, documentadas en [[06 - Registro visual]]. Las capturas de interfaz actualizadas en `Media/Interfaz/` pertenecen al juego compilado final (13 de noviembre).

## Cobertura de entrada, validación técnica y control de calidad de medios (`8047d90` / `b543684`)

Con la incorporación de las asignaciones de mando para `E` (`buttonWest`), `T` (`buttonNorth`) e `I` (`selectButton`), todas las acciones principales de arena, bóveda, bestiario y menús cuentan con soporte completo en **teclado/ratón y mando**. Asimismo, el **tráiler promocional final (`MEDIA-TRAILER-01`)** quedó integrado en [`Media/Trailer/EchoScrap-Trailer-Final-1080p.mp4`](Media/Trailer/EchoScrap-Trailer-Final-1080p.mp4) (`1920 × 1080`, `46,2 s`, `70,9 MB`) y **superó el control de calidad** comprobando gameplay desarrollado y mezcla de audio acorde con la versión definitiva.

En el apartado técnico (`TECH-01`), el proyecto mantiene perfil de compilación Windows x64 Release (`Assets/Settings/Build Profiles/Windows.asset`, `0` errores y `1 010 519 179` bytes / `~0,94 GiB`), perfilado de `2 000` fotogramas con `p95` de CPU de `6,61 ms`, instrumentación `CombatBenchmarkRunner.cs` / `SpawnerRunValidation.cs` y las correcciones de `feature/perfil-memoria-baja` / `bugfix/tirones-horda-windows`, con el alcance acotado de gama mínima registrado en [[03 - Bugs y Seguimiento]].

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Bugs y seguimiento

## Suite completa pendiente de cierre (`QA-FULL-01`)

La ejecución completa anterior registró 45 fallos; las 172 pruebas focalizadas aprobadas sobre `b543684` no sustituyen esa validación. Los grupos afectados incluyen carga de escenas sandbox/laboratorio que están deshabilitadas en el perfil de entrega, expectativas antiguas de balance y posición de la máquina de fusión, capturas con `WaitForEndOfFrame` incompatibles con batch mode y comprobaciones de terreno, árboles y paredes. Estos últimos fallos deben investigarse antes de concluir que son únicamente pruebas obsoletas. Los resultados actuales se registran en [[02 - Pruebas]].

La integración en `develop` y la apertura de PR a `main` acreditan la sincronización del repositorio, no la ausencia de defectos ni la aprobación de toda la suite.

## Señalización y guía de la bóveda (`UX-01` — Resuelto en `9eed59f`)

**Estado: resuelto e integrado (`develop` @ `9eed59f`).** En revisiones anteriores, los tres puestos de equipo y la gestión de la colección carecían de indicadores globales dentro de la bóveda. En el código actual se ha resuelto mediante cuatro elementos integrados:

1. **Rótulos 3D sobre los pedestales del balcón superior:** `EQUIPO 1`, `EQUIPO 2` y `EQUIPO 3` (`CollectibleDisplay.MarkEquipmentSlot`).
2. **Indicadores direccionales superiores (`VaultDirections` en `GameMenuView.cs`):** dos tarjetas dinámicas con flechas (`▲`, `▼`, `◄`, `►`, `●`) que guían en tiempo real hacia la **Máquina de Fusión** y hacia el **Balcón de Equipo** (puestos 1, 2 y 3).
3. **Panel rotatorio de consejos (`VaultTips` en `GameMenuView.cs`):** tarjeta inferior izquierda con etiqueta `CONSEJO` que rota cada `8 s` entre tres indicaciones sobre equipo, fusión y liberación (y prioriza *"Pulsa E frente a un puesto para equipar tu primer coleccionable"* cuando `OwnedCount > 0 && !hasEquippedCompanion`).
4. **Índice de Colección (`VaultCollectionIndex.cs`, tecla `[I]`):** accesible desde el botón superior izquierdo `ÍNDICE · [I]`, con contador `{descubiertos} / 15 DESCUBIERTOS · {en posesión} / 18 UNIDADES`, pestañas `POR RAREZAS` y `EN TU BÓVEDA`, e inspector detallado de poderes.

## Acceso con mando en interacciones de colección (`INPUT-01` — Resuelto en `b381ef4` / `734e2a5`)

**Estado: resuelto e integrado (`8047d90` / `b543684`).** Además del combate (`A` / `Cruz` para dash), pausa (`Start`), consulta rápida (`Select`) y elección de cartas, la recogida manual de fragmentos en la arena (`CompanionSystem.TryCollectNearest` / `CollectibleDisplay`), la estación de fusión (`FusionStation`), los selectores de puesto (`CompanionSystem`) y la liberación (`VaultReleaseInteraction`) responden al botón oeste del mando (`X` / `Cuadrado`, equivalente a `E`), el desequipado rápido responde al botón norte (`Y` / `Triángulo`, equivalente a `T`), el cierre/cancelación usa el botón este (`B` / `Círculo`) y el Índice de Colección (`VaultCollectionIndex`) se alterna con `Select`.

## Incidencias corregidas en `0.7.1` (`7a237e5..b543684`)

- **Ventana forzada por registro heredado en Windows:** solucionado añadiendo el selector **`Pantalla` (`COMPLETA` / `VENTANA`)** en `MenuSettingsView` (`EchoScrap.Graphics.FullScreen`, valor por defecto `1`) y reaplicando `FullScreenWindow` a resolución nativa tras cargar escena en Standalone (`AfterSceneLoad` + segundo fotograma).
- **Enemigos blanquecinos por fuga de `_EmissionColor` o saturación por DoT continuo:** `MothSpriteView`, `MortarSpriteView` y `EnemySpriteView` limpian `_EmissionColor` y el `MaterialPropertyBlock` al terminar el destello y al reciclar desde el pool, y `EnemyActor.ReceiveDamage` excluye `StaticAura`, `PlasmaPuddle` y `FlameTrail` del destello blanco de impacto directo.
- **Recorte de *Aura estática* y zonas de suelo en pendientes:** `UpgradeEffectPool.ProjectGroundRing` proyecta los anillos de suelo sobre `ArenaSurface.Height(...) + 0,06 m` con malla preasignada `12 × 12` (`0` bytes de basura en GC).

## Comportamiento de límites de población, oleadas y alcance técnico

- **Población fuera de cámara:** el límite de `150` enemigos ordinarios contabiliza también a los activos fuera del encuadre; una retirada sostenida en línea recta puede concentrar parte de la población detrás del jugador hasta que se reciclen o sean eliminados.
- **Recorte de la ronda demo:** la ronda demo (`3:00`) emplea los tres primeros tramos de la tabla de oleadas (`0:00–3:00`) antes de invocar a Gólgora (`Golgora_Demo.asset`, `3 600 HP`, mismo valor base en `EnemyArchetypeSettings`), mientras que la ronda completa (`8:00`) recorre los cinco tramos completos hasta el encuentro de `9 000 HP` (`Golgora_Complete.asset`).
- **Validación técnica multiplataforma y limitación en hardware mínimo (`TECH-01` / `0.7.1`):** sobre las compilaciones Release vigentes (**Windows x64** y **macOS Universal**), las optimizaciones de GPU instancing, presupuesto de navegación en `EnemyObstacleNavigator`, `WarehouseLightScope`, LODs/texturas, deslizamiento en rocas bajas, pooling sin GC de habilidades y los selectores **`Límite de FPS` (`30 / 60 / 120`)** y **`Pantalla` (`COMPLETA` / `VENTANA`)** en `MenuSettingsView` amplían la estabilidad y compatibilidad de pantalla. Sigue acotada como limitación conocida la ausencia de telemetría instrumentada en un equipo físico de gama mínima (`8 GB RAM` / `4 GB VRAM`), mitigada mediante `Efectos decorativos`, `Efectos reducidos` y el tope de FPS.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Criterios de aceptación

| Parte | Comportamiento esperado (`develop` @ `b543684`) |
| --- | --- |
| Inicio y ranking | Menú con selector de **3 ranuras (`CONTINUAR` / `NUEVA PARTIDA`)**, `CONFIGURACIÓN` compartida en 3 pestañas (`MenuSettingsView`), campo `NOMBRE DEL JUGADOR (RANKING)...` (máx. 20 caracteres, validado por `GamertagRegistry`), reanudación directa si existe snapshot válido (`expedition_{N}.json`), y confirmación segura al sobrescribir una ranura ocupada. |
| Preparación y guía en bóveda | Rótulos 3D `EQUIPO 1..3` en el balcón, indicadores direccionales superiores (`VaultDirections`), panel inferior de consejos rotatorios cada `8 s` (`VaultTips`), máquina de fusión en pared trasera (`189, 0, 28,88`) e **Índice de Colección (`[I]`)** (`I` / `Select`) con contador `{descubiertos} / 15 DESCUBIERTOS · {en posesión} / 18 UNIDADES` y pestañas `POR RAREZAS` (`TODAS` + 6 rarezas, siluetas `???`) y `EN TU BÓVEDA`. |
| Equipo y liberación | En los puestos 1 a 3 (balcón, `slot 0..2`), `E` / botón oeste (`E · EQUIPAR` / `E · EDITAR`) abre el selector con botones `QUITAR DEL PUESTO`, `LIBERAR COPIA` y `CERRAR · E / ESC`, y `T` / botón norte en el mundo desequipa directamente. En los puestos ocupados de planta baja (`slot >= 3`), `E · LIBERAR` / botón oeste abre `LIBERAR UNIDAD` (`CANCELAR` / `LIBERAR`). **Los poderes se acumulan por copia equipada (`Copies(id)` hasta `×3`) y se muestran con su total dinámico mediante `DescribeHudPower`.** |
| Rondas | Selección en **`ELIGE LA RONDA`** de ronda demo (`180 s`, `3 600 HP` en `Golgora_Demo.asset`) o completa (`480 s`, `9 000 HP` en `Golgora_Complete.asset`). |
| Movimiento y dash | Locomoción base de `8 m/s` (`80 m/s²` / `70 m/s²`), diagonal normalizada, apoyo sobre relieve y dash de `22 m/s` (`0,15 s`, `0,22 s` de invulnerabilidad, `Espacio` / `A`) detenido por obstáculos. `DASH_04` fija la recarga en `0,5 s` manteniendo el `+8%/nivel` de velocidad de `DASH_01`. |
| Linterna y cartas | Autoapuntado y ráfaga de cuatro pulsos; con `FLASH_05` (*Rayo solar*) todos los pulsos aplican empuje (`capturedSolarMode`), mientras `FLASH_03`, `BIO_02` y `CRIT_04` se evalúan en el cuarto pulso (`PulseIndex == 4`); `CRIT_05` triplica el daño crítico bajo el `35%` de vida previa; `DASH_05` (*Paso de ariete*) amplía distancia sin daño; `MYTH_01` (*All In*) aleatoriza cartas poseídas a otras distintas con `+1` nivel (hasta `MaxTier`); ofertas válidas entre las **62 mejoras en 5 rarezas**. |
| Salud y mitigación | Salud máxima según fórmula de `PlayerRuntimeStats` (`+10` HP y `+10%` armadura por `SCRAP_01`) y resolución del daño entrante en `PlayerHealth.ReceiveDamage` (`0,40 s` de protección post-golpe → Hada por copia → evasión → Soldier por copia `-15%` → bloqueo → armadura máx. `60%` → Yanikua por copia ante daño letal / Nanobots `2 s` de escudo si sobrevive bajo `< 20%`). |
| Enemigos | Cinco arquetipos de horda (sin aparecer dentro de casas/almacenes cerrados via `OverlapsBlockedInterior`) y dos élites con parámetros unificados entre `EnemyArchetypeSettings` y `.asset`. Polilla Reactor arma su espoleta a `2 m` (`0,9 s` / `1,2 s` al morir) e inflige `35` de daño físico completo tanto al jugador como a los objetivos enemigos (`IsEnemyTarget`, incluido Gólgora). |
| Jefe (Gólgora) | Traslado protegido a la caldera de `22 m`, seis patrones unificados en `GolgoraCombatSettings.cs` y `CombatSettings.asset` (*Bombardeo* `0,6 s` de recuperación, *Cerco de magma* `40` de daño físico al aterrizar y `18 s` fijos de enfriamiento, *Fisura de magma* `1,1 s` fijos de preparación), transición a Fase 2 al `50%` de vida (`1,5 s` + empuje `12 m`) y límite efectivo de 8 secuaces. |
| Colección y capacidad (`18/18`) | Apariciones temporales en arena y recogida manual con `E` / botón oeste hasta el máximo estricto de **18 unidades** (`DisplaySlots[18]`: `3` de equipo + `15` de exposición). Con `18/18` unidades, bloquea la recogida en arena (`INVENTARIO LLENO / LIBERA ESPACIO`) y el reclamo en fusión (`INVENTARIO LLENO · LIBERA ESPACIO`). |
| Fusión (`30 s`) | Acepta **4 copias de la misma rareza o de rarezas mixtas** (excluida Yanikua; permite copias equipadas `EN EQUIPO 1/2/3`, desequipándolas automáticamente al confirmar), dura **30 segundos (`00:30`)** fijos, muestra probabilidades en UI (`60%`, `40%`, `1%` en 4 Épicas) y ofrece un único botón `COLECCIONAR` al reclamar. |
| Interfaz y ajustes | Tipografía unificada **`Pixelify Sans`**, panel unificado `MenuSettingsView` en 3 pestañas (`General` con **`Límite de FPS` `30 / 60 / 120`** en pausa, `Efectos reducidos` en título y **`Pantalla`** `COMPLETA` / `VENTANA` en ambos menús, `Interfaz` con `Atributos de coleccionables SÍ/NO`, `Sonido` con `Volumen general` y `Música`) en Menú Inicial y Pausa, cuadrícula de mejoras `9 × 34 px`, `22` estadísticas y consulta rápida manteniendo `Q` o `Select` en combate. |
| Resultado, retorno y persistencia | Prioridad de derrota en empate simultáneo, sincronización `PUT` / `DELETE` (`timeout = 8 s`) con Firebase (`EventLeaderboardSync` + `Saves/ranking_outbox.json`) publicando todos los intentos válidos y conservando la mejor puntuación por gamertag, retorno limpio a la bóveda, limpieza segura de efectos en `GolgoraMagmaRingPool` y guardado atómico por ranura (`SaveSlotService`, `FusionProgress` v5 con `displays[18]`, `discovered[15]`, `hasEquippedCompanion`, `FusionPhase` de 5 estados y `expedition_0..2.json`). |

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Glosario

| Término | Uso actual (`8047d90` / `b543684`) |
| --- | --- |
| Coleccionista | Personaje controlado por el jugador, equipado con linterna automática y dash. |
| Vertedero | Arena exterior de expedición de `200 × 200 m`. |
| Bóveda o almacén | Sala modular en dos niveles conectada por portal, con **18 pedestales físicos** (`DisplaySlots[18]`, capacidad máxima de 18 unidades: 3 puestos de equipo en el balcón y 15 puestos de exposición en planta baja) y máquina de fusión en la pared trasera (`189, 0, 28,88`). |
| Entidad o coleccionable | Cada una de las **15 entradas** del catálogo (`ID 0..14`), distribuidas en seis rarezas (*Común, Raro/Poco común, Especial/Rara, Épico, Legendario, Mítico*). |
| Copia o unidad | Instancia en posesión de una entidad (`owned[15]`). La colección de cada ranura admite un máximo estricto de **18 unidades totales** (`FusionProgress.Capacity = 18`). |
| Entidad equipada | Unidad asignada a uno de los **3 puestos de equipo activo** del balcón (`EQUIPO 1..3`, `E` / botón oeste para editar o `T` / botón norte para desequipar). Activa un poder propio de `CompanionPowerRuntime` **acumulable por copia equipada** (`Copies(id)`, hasta `×3`) y puede seleccionarse como ingrediente de fusión (`EN EQUIPO 1/2/3`). |
| Exposición | Los **15 pedestales de la planta inferior** (`slot 3..17`) donde se exhiben las unidades en reserva ordenadas por rareza y desde donde pueden liberarse con `E · LIBERAR`; cuando se superan las 15 copias libres, cualquier puesto de equipo vacío del balcón (`slot 0..2`) exhibe visualmente la reserva marcada como `RESERVA`. |
| Índice de Colección (`[I]`) | Panel de la bóveda (`VaultCollectionIndex`, tecla `I` / `Select`) con contador `{descubiertos} / 15 DESCUBIERTOS · {en posesión} / 18 UNIDADES`, pestañas `POR RAREZAS` (`TODAS` y 6 rarezas, con siluetas negras `???` para las no descubiertas) y `EN TU BÓVEDA`, e inspector derecho de poder. |
| `VaultDirections` y `VaultTips` | Tarjetas direccionales superiores con flechas en vivo hacia Fusión y Balcón de Equipo, y panel inferior izquierdo `CONSEJO` que rota cada `8 s` en la bóveda. |
| Ronda demo / completa | Modalidades elegidas antes de salir: jefe a los `3:00` con `3 600 HP` (`Golgora_Demo.asset`) o a los `8:00` con `9 000 HP` (`Golgora_Complete.asset`). |
| Eco o gema de XP | Experiencia del intento actual, atraída con el imán y fusionable en el suelo a menos de `0,75 m`. |
| Draft | Elección pausada de hasta tres cartas válidas entre las **62 mejoras en cinco rarezas** (incluyendo la carta Mítica *All In*) al subir de nivel. |
| Linterna | Arma sobrenatural de cuatro pulsos con autoapuntado y empuje en el cuarto pulso (o en todos los pulsos con `FLASH_05` *Rayo solar*). |
| Dash | Esquiva rápida con invulnerabilidad, limitada por obstáculos sólidos. |
| Élites | Caballero Fantasma y Centinela de Cobre, con resistencias y ataques propios. |
| Gólgora / MataColeccionista | Jefe final de la caldera de `22 m`, con seis patrones de ataque y dos fases. |
| Cerco de magma | Salto invulnerable al centro con impacto físico de aterrizaje (`40` en `3 m`), anillos de fuego con brecha segura de `60°` (1 onda en Fase 1 y 3 en Fase 2) y enfriamiento fijo de `18 s`. |
| Fusión | Proceso de **30 segundos (`00:30`)** fijos en la máquina de fusión de la pared trasera (`189, 0, 28,88`) que consume 4 copias de la misma rareza o de rarezas mixtas (excluida Yanikua) y entrega una entidad reclamable con el botón único `COLECCIONAR`. |
| Ranking de evento (`EventLeaderboardSync`) | Sistema que reserva el gamertag mediante hash SHA-256 (`GamertagRegistry`), encola intentos en `Saves/ranking_outbox.json` (con respaldo legado `EchoScrap.Rank.v2`) y sincroniza cada partida válida mediante HTTPS `PUT` / `DELETE` con Firebase Realtime Database, conservando la mejor puntuación por gamertag en [`https://echoscrap.web.app`](https://echoscrap.web.app). |
| Pool | Reserva de instancias reutilizables en memoria (`Object Pool`). |

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Referencias e Inspiración

## Descripción de la sección

Registra los videojuegos y obras utilizadas como referencia de diseño, arte, narrativa y ritmo para *EchoScrap*.

## Referencias principales

| Nombre | Aspecto de referencia | Qué nos interesa tomar | Qué NO buscamos copiar |
|---|---|---|---|
| **Vampire Survivors** | Bucle de supervivencia, escalado de hordas y selección pausada de mejoras al subir de nivel. | La claridad del bucle de absorción magnética de gemas de XP, la satisfacción de limpiar oleadas masivas y la pausa táctica de 3 opciones al subir de nivel. | La ausencia de esquive activo (*Dash*) y las partidas de 30 minutos; *EchoScrap* concentra la partida en 3 u 8 minutos más el encuentro contra el jefe. |
| **Brotato / 20 Minutes Till Dawn** | Construcción de *builds* compactas y sinergias entre mejoras pasivas y activas en arenas cerradas. | La legibilidad de las estadísticas del personaje, los topes defensivos claros y la tensión dentro de un perímetro delimitado. | El sistema de tienda con moneda entre oleadas; en *EchoScrap* la progresión en partida ocurre por nivel de XP y la preparación entre incursiones mediante la colección de hasta 18 unidades y la fusión de 4 copias. |
| **Risk of Rain Returns / Hyper Light Drifter** | Dirección de arte *pixel art* de alto contraste, paleta cromática y atmósfera melancólica de mundos en ruinas. | El uso de sombras profundas (paleta Apollo) contrastadas con cianos y ámbares brillantes para mantener legibles los ataques y peligros. | La exploración de mapas laberínticos abiertos o plataformas de salto. |
| **Enter the Gungeon** | Respuesta kinestésica del *Dash* con invulnerabilidad (*i-frames*) y telegrafiado visual de ataques enemigos. | La precisión del esquive para atravesar cercos o proyectiles y los indicadores claros antes de una explosión o pisotón. | El apuntado manual de doble *stick* con recarga de munición tradicional. |
| **Resident Evil (Archivos y vitrinas)** | Consulta e inspección de una colección en un espacio seguro de preparación. | La exposición física en los 10 pedestales de la bóveda (3 de equipo + 7 de reserva) y la consulta detallada en el **Índice de Colección (`[I]`)** con retratos, siluetas bloqueadas (`???`) e inspector de poderes. | La gestión de inventario de supervivencia clásica para munición y llaves. |

> [!warning] Propiedad intelectual
> Todas las referencias sirven únicamente como guía de análisis mecánico, visual y de ritmo; la procedencia de cada recurso de EchoScrap se conserva en su registro correspondiente.

> **Navegación:** [[00 - Índice]] · ← [[01 - Glosario]] · [[03 - Registro de Decisiones]] →

---

# Registro de decisiones vigentes

Esta edición refleja el estado verificado del código en **`develop` @ `b543684`** (**16 de noviembre de 2026**, versión del GDD **`0.7.1`**).

| Área | Regla o decisión vigente (`8047d90` / `b543684`) |
| --- | --- |
| Flujo general | Menú con selector de **3 ranuras de guardado** (`CONTINUAR` / `NUEVA PARTIDA` en `SaveSlotService`), reanudación automática por snapshot (`ExpeditionSnapshotService`), `CONFIGURACIÓN` unificada (`MenuSettingsView.cs` en 3 pestañas), campo `NOMBRE DEL JUGADOR (RANKING)...` en `NUEVA PARTIDA · ARCHIVO N`, bóveda con señales, selección `ELIGE LA RONDA`, expedición, sincronización de ranking (`EventLeaderboardSync`) y regreso a la bóveda. |
| Señalización de bóveda e Índice | Implementada con rótulos 3D `EQUIPO 1..3` en el balcón, tarjetas direccionales superiores (`VaultDirections`), consejos rotatorios cada `8 s` (`VaultTips`) e **Índice de Colección (`[I]` / `Select`)** con pestañas `POR RAREZAS` (`TODAS` + 6 rarezas y siluetas `???`) y `EN TU BÓVEDA`. |
| Capacidad de colección (`18/18`) | Máximo estricto de **18 unidades en total** (`FusionProgress.Capacity = 18`), correspondientes a los **18 pedestales físicos de la bóveda** (`DisplaySlots[18]`: `3` puestos de equipo + `15` pedestales de exposición/reserva), con máquina de fusión en la pared trasera `(189, 0, 28,88)`. Bloquea la recogida en arena (`INVENTARIO LLENO / LIBERA ESPACIO`) y el reclamo en fusión (`INVENTARIO LLENO · LIBERA ESPACIO`) al estar llena. |
| Equipo, acumulación y liberación | Puestos 1 a 3 (balcón, `slot 0..2`): `E` / botón oeste abre el selector (`QUITAR DEL PUESTO`, `LIBERAR COPIA`, `CERRAR · E / ESC`) y `T` / botón norte desequipa directamente. **Los poderes de acompañantes se acumulan por copia equipada** (`Copies(id)`, hasta `×3`, y se muestran con su total dinámico mediante `DescribeHudPower`). Pedestales inferiores ocupados (`slot >= 3`): `E · LIBERAR` / botón oeste abre `LIBERAR UNIDAD` (`E`, `Enter`, `Espacio` o `A`/`Cruz` confirman). Liberar descuenta `1` de `owned[id]` y conserva `discovered[id]`. |
| Fusión (`30 s`) | Duración fija de **30 segundos (`00:30`)** para todas las combinaciones. Acepta **4 ingredientes de la misma rareza o de rarezas mixtas** (excluida Yanikua), interpolando por la rareza media (`qualityMean = total / 4f`), permite seleccionar copias equipadas (`EN EQUIPO 1/2/3`, desequipándolas al confirmar), redondea porcentajes en UI (`60%`, `40%`, `1%` para 4 Épicas) y presenta un único botón `COLECCIONAR`. |
| Tipografía, ajustes y HUD | Tipografía unificada **`Pixelify Sans`**. `MenuSettingsView` compartido en 3 pestañas (`General` con **`Límite de FPS` `30 / 60 / 120`** en pausa, `Efectos reducidos` en título y **`Pantalla` `COMPLETA` / `VENTANA`** en ambos, `Interfaz` con `Atributos de coleccionables SÍ/NO`, `Sonido` con `Volumen general` y `Música`). HUD de combate con cuadrícula `9 × 34 px` (hasta `62` cartas, subida `+100 px` si ancho `< 1530 px`) y `22` filas de estadísticas; mantener `Q` o `Select` en combate despliega detalles completos; soporte completo de teclado/ratón y mando. |
| Rayo solar (`FLASH_05`) y Paso de ariete (`DASH_05`) | `FLASH_05` elimina el descanso (`0 s`), eleva el alcance base a `14 m`, multiplica el daño de luz por `2,5` y aplica empuje en todos los pulsos (`capturedSolarMode`), mientras `FLASH_03`, `BIO_02` y `CRIT_04` mantienen la comprobación de cuarto pulso (`PulseIndex == 4`). `DASH_05` pasa a ampliar un `+25% / +45% / +70%` la duración y distancia del dash (`4,125 / 4,785 / 5,61 m`) sin daño de contacto ni arrastre. |
| Catálogo ampliado (`62` cartas, `5` rarezas) y *All In* (`MYTH_01`) | Pesos aleatorios `60 / 25 / 12 / 3 / 1` (`Común`, `Rara`, `Épica`, `Legendaria`, `Mítica`). Incluye `8` cartas contextuales (`1239072`), `19` cartas nuevas de combate/área/proyectiles sin GC y **`MYTH_01` (*All In*)** con marco lit `Frame_Mythic`, que aleatoriza todas las mejoras poseídas en otras distintas y las sube `+1` nivel (hasta su `MaxTier`). Los anillos en suelo (`StaticAura`, `PlasmaPuddle`, `FlameTrail`) se proyectan sobre `ArenaSurface` (`12 × 12` sin GC), y los ticks continuos de zona no disparan destello blanco completo (`MothSpriteView` / `MortarSpriteView` / `EnemySpriteView` limpian `_EmissionColor` tras cada destello). |
| Cataclismo (`DASH_04`) | Fija la recarga del dash en `0,5 s` (anulando el `-10%/nivel` de recarga de `DASH_01`), pero **conserva el `+8%/nivel` de velocidad de dash de `DASH_01`** y el bono de distancia de `DASH_05`. |
| Demolición y Chatarra | `DEMO_03`: `0,25 s` de armado, `1,25 m` de activación, máx. 5 minas (`45/70/100` de daño, `3 m` de radio). `DEMO_04`: `75%` de daño a los `0,4 s` y atracción base del `45%` del radio. `SCRAP_03`: `1 / 2 / 3` sierras (`18 / 28 / 40`), `180°/s` a `2,5 m`. `SCRAP_04`: cada `10 s`, duración `12 s`, máx. `2` torretas, ráfagas de `3` tiros (`10` daño, `12 m`, `28 m/s`) cada `0,6 s` separados por `0,08 s`. |
| Ojo del verdugo (`CRIT_05`) | Daño crítico ya calculado `×3` si el objetivo estaba estrictamente bajo el `35%` de vida antes del golpe, más aturdimiento de `1 s` en `3 m`. |
| Polilla Reactor y Gólgora | Polilla: espoleta por proximidad a `2 m` (`0,9 s` / `1,2 s` al morir), `35` de daño físico completo al jugador y a los objetivos enemigos (`IsEnemyTarget`, incluido Gólgora). Gólgora: `3 600 HP` en `Golgora_Demo.asset` (mismo valor base en `EnemyArchetypeSettings`) y `9 000 HP` en `Golgora_Complete.asset`; transición invulnerable de `1,5 s`, *Bombardeo* con `0,6 s` de recuperación, *Cerco de magma* con `40` de daño físico de aterrizaje y `18 s` fijos de enfriamiento en ambas fases, y *Fisura de magma* con `1,1 s` fijos de preparación (`GolgoraCombatSettings` = `CombatSettings.asset`). |
| Guardado, snapshots y ranking | `SaveSlotService` en `Saves/` (`3` ranuras JSON atómicas, `FusionProgress` v5 y migración automática desde `PlayerPrefs` legado), `ExpeditionSnapshotService` cada `3 s`, tope de FPS en `EchoScrap.Graphics.FrameRateLimit`, modo de pantalla en `EchoScrap.Graphics.FullScreen` (por defecto `1`, reaplicado tras carga de escena en Standalone), volúmenes en `EchoScrap.Audio.*`, reserva atómica de gamertag en `/gamertags/{sha256}` y cola `Saves/ranking_outbox.json` enviando **todos los intentos válidos** conservando la mejor puntuación por gamertag (`PUT`/`DELETE`, `timeout = 8 s`). |

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]

---

# Historial de Versiones

## Descripción de la sección

Registra los cambios importantes realizados al GDD. Esto permite conocer cómo evolucionó el diseño y cuándo se modificó una decisión.

## Formato

| Versión | Fecha | Cambios principales | Responsable |
|---|---|---|---|
| **0.1.0** | 25/09/2026 | Creación inicial de la estructura documental del GDD. | Mora Alejandro Angel Gabriel |
| **0.1.1** | 25/09/2026 | Redacción de la Sección 01 - Visión (Ficha, Concepto, Público y Pilares). | Mora Alejandro Angel Gabriel |
| **0.2.0** | 04/10/2026 | Documentación de la **Sección 02 - Diseño** (Jugabilidad, Mecánicas, Sistemas, Controles, Progresión, Economía y Estados), los 5 arquetipos de enemigos de horda, Gólgora y la arquitectura técnica. | Sierra Sosa David Alejandro / Mora Alejandro Angel Gabriel |
| **0.3.0** | 16/10/2026 | Consolidación del documento de diseño, narrativa, mecánicas, catálogo de mejoras y organización del equipo. | Equipo EchoScrap |
| **0.4.0** | 27/10/2026 | Actualización al proyecto jugable EchoScrap: reglas y valores actuales, seis enemigos, 35 mejoras, mundo de 200 m, almacén y fusión; atlas activos y GIFs de enemigos, cartas y VFX capturados en Unity. | Sierra Sosa David Alejandro |
| **0.5.0** | 04/11/2026 | Flujo de preparación con selección de entidades; quince entidades, equipo de tres puestos y fusión de cuatro copias; élites, sprites y animaciones actuales, caldera y patrones de Gólgora, minimapa, cámara, audio y guardado de producción. | Sierra Sosa David Alejandro |
| **0.5.1** | 05/11/2026 | Fichas individuales de enemigos en Personajes; spritesheets, fuentes editables y GIFs actuales reunidos en Personajes y Animación; navegación y documento completo sincronizados. | Equipo EchoScrap |
| **0.5.2** | 05/11/2026 | Registro visual completado con 21 capturas actuales de VFX de linterna, mejoras, daño, fusión y portal; muestras de octubre sustituidas, procedencia y efectos compartidos documentados. | Equipo EchoScrap |
| **0.6.0** | 05/11/2026 | Poderes propios de las quince entidades traídos de `develop` y controles de puestos actualizados; sprites integrados de Caballero y Centinela con ciclos de combate; *Rayo solar* con empuje en todos sus pulsos; fórmula y condiciones de *Ojo del verdugo*; Registro visual asignado a David y reparto de 48 apartados. | Sierra Sosa David Alejandro |
| **0.6.1** | 07/11/2026 | Especificación de balance, composición de oleadas, prioridad y daño de Gólgora, plano del Vertedero, público y accesibilidad, guardado y objetivos de validación técnica. Backlog con responsables, dependencias y criterios. | Equipo EchoScrap |
| **0.6.2** | 08/11/2026 | Sincronización íntegra con `develop` @ `9eed59f`: capacidad máxima estricta de **10 unidades** (`3` puestos de equipo en el balcón + `7` de exposición en planta baja, `displays[10]`), bloqueo por inventario lleno (`10/10`), interacción exacta de pedestales (`E · EQUIPAR`/`EDITAR`, `T` y `E · LIBERAR`), señalización e indicadores de bóveda implementados (`EQUIPO 1..3`, `VaultDirections`, `VaultTips`), **Índice de Colección (`[I]`)**, fusión fija de **30 s (`00:30`)** con copias equipadas (`EN EQUIPO 1/2/3`), porcentajes redondeados a enteros (`60%`, `40%`, `1%`) y botón único `COLECCIONAR`, tipografía unificada **`Pixelify Sans`**, menú de pausa en 3 pestañas (`General`, `Interfaz`, `Sonido`), consulta rápida del HUD (`Q`), ranking en vivo con Firebase (`EventLeaderboardSync`) y reglas exactas de combate (`FLASH_05`, `DASH_04`, `DEMO_03/04`, `SCRAP_03/04`, fórmulas y orden de mitigación en `PlayerHealth`, Polilla Reactor, Gólgora y tabla de empuje). | Sierra Sosa David Alejandro |
| **0.6.3** | 09/11/2026 | Historias de origen de las quince entidades coleccionables en Personajes; tabla de sprites (`.png` a escala `2×` y fuentes `.aseprite`) en Personajes y Animación; integración de 20 bocetos y arte conceptual a mano (El Vertedero, la Bóveda, Coloso/Wendigo, Mortero Campana, Polilla Reactor, Caballero Fantasma, Centinela de Cobre y trece entidades coleccionables) en `Media/ConceptArt/`, Niveles y Entornos; desglose de composición musical, capturas de producción en DAW, instrumentación (chelo, bajo, campana, sintetizadores, batería, corno francés, violín y teclado MIDI), tonalidades armónicas (Cm, Gm, Dm, Am, F, Bb), cuatro pistas de banda sonora (`Media/Audio/*.mp3`), quince muestras de efectos de sonido y avisos (`Media/Audio/SFX/*.wav`) y tabla de procedencia/autoría sonora en Audio y Música; sincronización del GDD completo. | Mora Alejandro Angel Gabriel / Equipo EchoScrap |
| **0.6.4** | 10/11/2026 | Integración de los bocetos a mano del **Coleccionista** y **Murciélago Cristal** (`22` bocetos en `Media/ConceptArt/`); documentación detallada del **Índice de Colección / Bestiario (`VaultCollectionIndex.cs`, tecla `[I]`)** con su acción exacta de desbloqueo (`CollectFromArena` / `Resolve`), distribución de las 15 criaturas en 6 rarezas y captura de su fase beta (`Media/Interfaz/IndiceColeccion_Beta.jpg`); y documentación íntegra de la **mecánica de Ranking en vivo (`EventLeaderboardSync.cs`, `GamertagRegistry.cs`, `rulesVersion = 2`)** y su página web en tiempo real ([`https://echoscrap.web.app`](https://echoscrap.web.app), `Media/Interfaz/RankingWeb_EchoScrap.png`) alineada con `develop` @ `a8a23c4`. | Mora Alejandro Angel Gabriel / Equipo EchoScrap |
| **0.6.5** | 13/11/2026 | Incorporación del enlace a la versión ejecutable en **itch.io** (`https://naked-hiro.itch.io/echoscrap`); renovación total de las capturas históricas por capturas de la versión final en Unity (`Media/Interfaz/`: Bóveda con HUD y señalización en vivo, Bestiario final, Equipar acompañante con barra unificada, Liberar unidad, Máquina de fusión, Resultado de fusión con `COLECCIONAR`, HUD de combate en arena, Consulta rápida `Q`, Menú de pausa en 3 pestañas, Victoria, Derrota y Nueva colección); integración de bocetos a mano pendientes de personajes (**Residual / Esqueleto**, **Interceptor / Galgo** y **Joel**) y creación de la galería de bocetos de elementos (`Media/ConceptArt/Elementos/`: mapa, almacén, portal, máquina, bestiario, pausa y láminas de elementos). | Mora Alejandro Angel Gabriel / Equipo EchoScrap |
| **0.6.6** | 13/11/2026 | Auditoría de paridad contra `main` @ `b381ef4` / `develop` @ `977bac5` y criterios de entrega final: acumulación real de poderes de acompañantes por copia equipada (`Copies(id)` hasta `×3`); fusión de 4 copias de la misma rareza o de **rarezas mixtas** (`qualityMean`); panel compartido `MenuSettingsView` de 3 pestañas entre `CONFIGURACIÓN` y `PAUSA`; soporte completo de mando (`buttonWest`, `buttonNorth`, `selectButton`); calibración de locomoción (`8 m/s`, dash `22 m/s` con `0,22 s` invuln., `0,40 s` post-golpe) y exactitud de persistencia (`FusionPhase` de 5 estados, `opening` normalizado `float`, `EchoScrap.Rank.v2`, `PUT`/`DELETE` con `8 s` y conservación de oportunidad pendiente al pulsar Continuar). *(Las imprecisiones residuales en `FLASH_05`, `BIO_03`, Polilla Reactor, Gólgora, `4 Legendarios` y nombres C# quedaron corregidas en `0.6.12`).* | Equipo EchoScrap |
| **0.6.7** | 13/11/2026 | Delimitación temporal de controles en teclado/ratón con soporte de mando proyectado a futuro *(revertida y superada en la versión `0.6.9`)*. | Equipo EchoScrap |
| **0.6.8** | 13/11/2026 | Integración y aprobación QA del tráiler promocional final (`Media/Trailer/EchoScrap-Trailer-Final-1080p.mp4`, `1080p`, `46,2 s`); ampliación de la tabla de procedencia sonora con todos los efectos reales en Unity (`Flashlight/`, `MataColeccionista/Audio/`, portales y XP); resumen de juego y avance en Controles; y apartado de créditos y licencias de terceros en Roles y Responsabilidades. | Mora Alejandro Angel Gabriel / Equipo EchoScrap |
| **0.6.9** | 13/11/2026 | Restablecimiento transversal del soporte completo y oficial de **mando** junto con teclado y ratón en todos los apartados del GDD (arena, bóveda, Índice de Colección, fusión, selección de cartas y menús). | Equipo EchoScrap |
| **0.6.10** | 13/11/2026 | Integración de identidad visual (`Media/Identidad/`: logo SVG, título SVG/`.aseprite`, portada y tarjeta), respaldo completo de clips de audio de Unity (`Media/Audio/Unity/`) y **cierre técnico `TECH-01`** (perfil Release `Windows.asset`, `0` errores, `~0,94 GiB`, `p95` CPU `6,61 ms` y registro transparente de limitaciones conocidas para la entrega `v1.0`). | Mora Alejandro Angel Gabriel / Sierra Sosa David Alejandro / Nuñez Chan Luis Fernando |
| **0.6.11** | 13/11/2026 | Incorporación del boceto conceptual a mano de **Gólgora** (`Media/ConceptArt/Golgora_guardian.jpeg`) y referencia de su modelo 3D (`MataColeccionista.fbx`), completando todos los bosquejos de personajes, enemigos, entidades y jefe. | Mora Alejandro Angel Gabriel / Equipo EchoScrap |
| **0.6.12** | 13/11/2026 | Auditoría integral de paridad contra **`origin/main` @ `734e2a5` / `develop` @ `6973556`** y los commits `84a72c0`..`6973556`: integración de retirada de cápsula residual (`84a72c0`), bloqueo de apariciones en interiores via `OverlapsBlockedInterior` (`3e59a4b`), totales dinámicos en HUD via `DescribeHudPower` (`f2e5470`), icono de escritorio `EchoScrap-AppIcon.png` (`d4e81df`), optimización física de `EnemyObstacleNavigator` (`cd6e4f2`) y limpieza segura de `GolgoraMagmaRingPool` (`acc5bf2`); corrección de fusión **4 Legendarios (`70%` Sirena · `30%` Yanikua)**, **Polilla Reactor (`2 m`, `35` de daño físico a enemigos incluido Gólgora)**, **`FLASH_05`** (empuje en cada pulso manteniendo `FLASH_03`/`BIO_02`/`CRIT_04` en `PulseIndex == 4`), **`BIO_03`** (`2 s` de protección al sobrevivir bajo `20%`), **Gólgora `3 600` / `9 000 HP` y `1,5 s`**, **`DEMO_03/04`**, **`SCRAP_01/03/04`**, frenado élite (`30` / `24 m/s²`), `E · LIBERAR` exclusivo de `slot >= 3`, retratos `Retrato_0..2` sin insignia numérica superpuesta, validación de `FusionProgress`, sustitución de símbolos C# inexistentes y rutas reales de audio en runtime. | Equipo EchoScrap |
| **0.7.0** | 16/11/2026 | Sincronización total contra **`develop` @ `8047d90` + `feature/mejoras-y-allin-mitica` @ `ebf90f9`**: integración de **3 ranuras de guardado atómicas** (`SaveSlotService` con migración desde `PlayerPrefs`), **snapshots de expedición cada `3 s`** (`ExpeditionSnapshotService`) con reanudación automática, ranking publicando **todos los intentos válidos** conservando mejor puntuación por gamertag, ampliación de bóveda a **18 unidades** (`DisplaySlots[18]`: `3` equipo + `15` exposición) con máquina en pared trasera (`189, 0, 28,88`), paredes de basalto pixel art y relieve de roca, selector **`Límite de FPS` (`30 / 60 / 120`)** en pausa, deslizamiento junto a rocas bajas, optimizaciones de navegación/GPU/texturas/bóveda, expansión a **62 mejoras en cinco rarezas** (`8` cartas contextuales `1239072` + `19` cartas nuevas con pooling sin GC + **rareza `Mítica` con `MYTH_01` *All In*** y `Frame_Mythic` lit + reelaboración de **`DASH_05` *Paso de ariete*** a distancia de dash), HUD de `22` estadísticas y cuadrícula `9 × 34 px`, y compilaciones verificadas **Windows x64** y **macOS Universal (`x86_64` + `arm64`)**. | Equipo EchoScrap |
| **0.7.1** | 16/11/2026 | Paridad final contra **`feature/mejoras-y-allin-mitica` @ `b543684`**: añadido selector **`Pantalla` (`COMPLETA` / `VENTANA`)** en la pestaña `General` de `MenuSettingsView` (`EchoScrap.Graphics.FullScreen`, arrancando automáticamente en pantalla completa sin bordes tras cargar escena en Standalone aunque el registro previo estuviera en ventana), limpieza explícita de `_EmissionColor` en `MothSpriteView`, `MortarSpriteView` y `EnemySpriteView` junto con exclusión de flash blanco completo en ticks continuos (`StaticAura`, `PlasmaPuddle`, `FlameTrail`), proyección sobre relieve mediante malla `12 × 12` sin asignaciones GC para *Aura estática* y zonas en suelo (`UpgradeEffectPool.ProjectGroundRing`), y actualización de los paquetes Release **Windows x64** y **macOS Universal**. | Equipo EchoScrap |
| **0.7.2** | 16/11/2026 | Sincronización del juego y del GDD en `develop` con PR a `main`; corrección de capacidad a 18 unidades en visión y arquitectura, precisión sobre guardados heredados, reparación de enlaces del documento completo y separación explícita entre pruebas focalizadas y suite completa. | Equipo EchoScrap |

## Convención sugerida

- **MAJOR:** cambios estructurales importantes.
- **MINOR:** nuevas secciones o características relevantes.
- **PATCH:** correcciones, aclaraciones y pequeños ajustes.

> [!example] Ejemplo
> `1.2.3` → versión mayor 1, versión menor 2, corrección 3.

> **Navegación:** [[00 - Índice]] · ← [[03 - Registro de Decisiones]] · [[00 - Índice]] →
