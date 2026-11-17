# EchoScrap — GDD 0.7.2

Documento actualizado al **16 de noviembre de 2026** (`develop` @ `b543684` + `chore/version-gdd-0.7.2` @ `2f919f5`, versión del juego `0.7.2`). Esta revisión documenta el estado actual del código fuente y las compilaciones **Windows x64** y **macOS Universal (`x86_64` + `arm64`)**: el ejecutable previo publicado en **itch.io** ([`https://naked-hiro.itch.io/echoscrap`](https://naked-hiro.itch.io/echoscrap)), el **tráiler promocional final (`1080p`, `46,2 s`, gameplay real y audio propio; aprobado en QA)** ([Media/Trailer/EchoScrap-Trailer-Final-1080p.mp4](Media/Trailer/EchoScrap-Trailer-Final-1080p.mp4)), el catálogo de **62 mejoras en cinco rarezas** (`Común`, `Rara`, `Épica`, `Legendaria` y la carta **`Mítica` *All In*** con marco propio lit `Frame_Mythic`, junto con la reelaboración de *Paso de ariete* a distancia de dash, las 8 mejoras contextuales balanceadas y los nuevos sistemas de auras proyectadas sobre relieve, relámpagos orbitales, búmeran, barrido y zonas en suelo sin GC en runtime), la ampliación de la bóveda a **18 unidades** (`DisplaySlots[18]`: 3 puestos de equipo en el balcón y 15 puestos de exposición/reserva en la planta baja, con la máquina de fusión en la pared trasera en `x = 189, z = 28,88`, relieve en fracturas y basalto pixel art), **3 ranuras de guardado independientes** (`SaveSlotService`) con reanudación exacta de expediciones por **snapshots periódicos (`ExpeditionSnapshotService`)**, **selector de límite de FPS (`30 / 60 / 120`)** y **selector de pantalla (`COMPLETA` / `VENTANA`)** en `MenuSettingsView.cs` (pantalla completa sin bordes por defecto), ranking en vivo en [`https://echoscrap.web.app`](https://echoscrap.web.app) que publica **todos los intentos válidos** conservando la mejor puntuación por gamertag (`EventLeaderboardSync.cs`, `GamertagRegistry.cs`), las optimizaciones de rendimiento/CPU/GPU/navegación/texturas en escena (`develop` `8047d90`) y el catálogo visual y sonoro completo de la entrega.

- [Índice](00%20-%20%C3%8Dndice.md).
- [Avance y flujo de preparación](00%20-%20Avance%20jugable.md).
- [Tráiler promocional final (`1080p`, `46 s`)](Media/Trailer/EchoScrap-Trailer-Final-1080p.mp4).
- [GDD completo con imágenes y GIFs](00%20-%20GDD%20Completo%20%28Para%20Exportar%20PDF%29.md).
- [Personajes y enemigos: spritesheets, fuentes y GIFs](05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md).
- [Valores y reglas de combate de los enemigos](04%20-%20Contenido/03%20-%20Enemigos%20y%20NPC.md).
- [Registro de VFX y capturas](05%20-%20Arte%20y%20Audio/06%20-%20Registro%20visual.md).

La edición documenta el flujo completo de preparación antes de elegir ronda: quince entidades con sus historias de origen y poderes acumulables por copia equipada, colección de hasta dieciocho unidades simultáneas, equipo activo de tres puestos con rótulos 3D (`EQUIPO 1..3`), indicadores direccionales y consejos contextuales en la bóveda, Índice de Colección (`[I]`), fusión de cuatro copias de la misma rareza o de rarezas mixtas (`30 s`), cinco enemigos de horda, dos élites y el encuentro completo contra Gólgora.

Personajes y Animación reúne las fichas individuales, el atlas y los GIFs de todas las acciones de los cinco enemigos de horda, el Coleccionista, las capturas del modelo 3D de Gólgora, la tabla de sprites y fuentes `.aseprite` de las quince entidades de la colección y los bocetos conceptuales a lápiz (`Media/ConceptArt/`), los cuales también acompañan al Vertedero y la Bóveda en Niveles y Entornos. Caballero Fantasma y Centinela de Cobre (gólem) usan sus sprites integrados, con ciclos de ataque, apertura y recuperación conectados al combate.

El registro visual incorpora linterna, Rayo solar, Holo-señuelo, Cataclismo, Termobárica, Micro-ensamblador, frío, esquirlas, curación, daño, fusión y portal, junto a las muestras renovadas de mejoras.

Puede abrirse como bóveda de Obsidian o con un lector compatible con Markdown y GIFs. **La referencia vigente es el Markdown 0.7.2.** Las exportaciones PDF muestran fotogramas estáticos y no conservan las animaciones.

## Repositorios y publicación

La fuente vigente del juego es [EchoScrap, rama develop](https://github.com/ShiroRex/EchoScrap/tree/develop); la del documento es [GDD-Echoscrap, rama develop](https://github.com/ShiroRex/GDD-Echoscrap/tree/develop). Los cambios se integran en `develop` y se proponen a `main` mediante PR. Las carpetas locales de prototipos y las copias antiguas del GDD son referencias históricas.

Los resultados de validación y sus limitaciones se detallan en [Pruebas](09%20-%20Calidad/02%20-%20Pruebas.md). Una batería focalizada aprobada no equivale a que toda la suite esté aprobada. El PDF exportado y los plugins personales de Obsidian son archivos locales derivados; no forman parte de la fuente versionada.

Plantilla original: Ing. Luis Fernando Villafaña Rejón. Proyecto y contenidos: equipo EchoScrap.
