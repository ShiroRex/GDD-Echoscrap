# Pruebas

El proyecto dispone de pruebas de locomoción, dash, daño, navegación, oleadas, élites, jefe, cartas, recogida, pausas, menús, bóveda, colección y fusión. El laboratorio permite generar ocho arquetipos y observar sus estados.

## Verificación de esta edición

La revisión documental contrasta los textos con los scripts, assets y montaje local de CombatArena. Se comprueban los 35 identificadores de mejora, las quince entidades, sus poderes y fuentes, las recetas de cuatro ingredientes y los parámetros vigentes del jefe y la cámara.

Los spritesheets se copian de los PNG activos. Sus previsualizaciones usan los rectángulos y tiempos de los JSON exportados; cada acción muestra sus cuatro vistas cuando existen. Son previsualizaciones del recurso, no capturas de una partida.

Las secuencias nuevas de Unity se obtienen desde una copia externa del proyecto, con jugador protegido, oleadas detenidas y cámara de detalle. Los patrones del jefe se invocan de forma aislada con los componentes del juego. La lluvia se registra en segunda fase. Las pantallas de preparación tienen un inventario de muestra y la persistencia de colección está desactivada en esa captura.

Los registros de VFX del 26 de octubre y cartas del 27 se mantienen identificados por su fecha. Las comprobaciones de medios y documentos no equivalen a una ejecución completa de todas las pruebas del juego ni a una certificación de rendimiento.

Las veinte pruebas de FusionRulesTests pasaron en Edit Mode, al igual que la prueba de captura en Play Mode. La revisión documental verificó las 35 mejoras, las quince entidades y sus poderes, los atlas y los enlaces locales. Esos resultados corresponden a estas comprobaciones concretas.

## Verificación de poderes y élites del 5 de noviembre

Se ejecutaron 120 casos de Play Mode de EnemySpriteIntegrationTests, EliteEnemyTests, FlashlightBurstFeedbackTests, CompanionPowerTests y CompanionIntegrationTests, todos aprobados. Incluyen los atlas y direcciones de los dos élites, apertura antes del disparo, recuperación, pausa, muerte y reciclado; el modo solar empuja durante ocho pulsos seguidos y el modo base conserva el cuarto pulso.

También se aprobaron 49 casos de CompleteUpgradeTests, CombatArenaRunFlowTests y la captura externa de élites. Cubren la fórmula de Ojo, las mejoras, las rondas, resultados y reinicios de la arena. La captura de interfaz actual se comprobó aparte. El conjunto verifica 169 casos de integración y reglas; no equivale a certificar toda la suite del proyecto ni una partida completa jugada manualmente.

Las nuevas capturas de los élites se ejecutan en CombatArena desde copia externa, con jugador protegido, oleadas detenidas y fases avanzadas mediante el controlador real. Cada secuencia tiene 72 imágenes a 12 por segundo y resolución de 960 × 540, exportada a GIF de 768 × 432. El selector de equipo usa inventario de muestra con el guardado de producción desactivado. Se generó además una build de desarrollo Windows x64 con MainMenu y CombatArena; la compilación terminó sin errores y con 34 advertencias en código y shaders existentes fuera de los cambios de esta integración. No sustituye una prueba manual completa de la build.

## Revisión de UX pendiente

Hay que observar si una persona que abre el juego por primera vez entiende que puede equipar entidades antes de salir. En el estado actual esa información no queda presentada de forma suficiente. También queda por revisar el recorrido de colección, equipo y fusión con mando.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
