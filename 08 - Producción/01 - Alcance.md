# Alcance de la versión jugable

El juego integra menú inicial con registro de nombre para el ranking, preparación guiada en la bóveda y expediciones en ronda demo (`3:00`) o completa (`8:00`). En la bóveda, señalizada mediante rótulos 3D (`EQUIPO 1..3`), indicadores direccionales (`VaultDirections`) y consejos rotatorios (`VaultTips`), el jugador consulta el **Índice de Colección (`[I]`)**, equipa hasta tres entidades en el balcón superior, libera copias en los pedestales para gestionar el espacio y utiliza la máquina de fusión (`30 s` fijos).

El combate reúne cinco arquetipos de horda, dos élites, 35 mejoras y Gólgora. El jefe cuenta con seis patrones de ataque, segunda fase al `50%` de vida y una caldera propia de `22 m` de radio. Victoria, derrota y abandono sincronizan las métricas de sesión con el ranking de evento (`EventLeaderboardSync`) y devuelven al jugador a la bóveda conservando su colección.

La arena de `200 × 200 m` incorpora relieve, obstáculos y acceso norte. Las zonas de lava fija están desactivadas. La bóveda cuenta con **10 pedestales físicos** que corresponden a la capacidad máxima estricta de **10 unidades** de la colección: **3 puestos de equipo activo** en el balcón superior y **7 puestos de exposición/reserva** en la planta inferior, junto a la máquina de fusión central que consume cuatro copias de la misma rareza.

El catálogo comprende **15 entidades** en seis rarezas (nueve obtenibles en la arena y seis mediante fusión). El inventario (`owned[15]`, máx. `10`), el registro permanente de descubrimientos (`discovered[15]`), los pedestales (`displays[10]`) y el proceso de fusión tienen guardado local en `PlayerPrefs` (`SaveData` v4).

La presentación incluye tipografía unificada **`Pixelify Sans`**, menú de pausa en tres pestañas (`General`, `Interfaz`, `Sonido`), consulta rápida de detalles del HUD en combate (`Q` / `Select`), sprites animados de los cinco enemigos de horda y ambos élites, modelo 3D de Gólgora, efectos de enemigos y mejoras, minimapa con exploración, retratos, iluminación y audio contextual. El laboratorio y los sandboxes permanecen como herramientas de prueba.

## Criterio de cierre

La entrega comprende el flujo completo en Windows, desde una colección nueva hasta la preparación guiada en bóveda, la fusión de `30 s`, el Índice de Colección (`[I]`), las expediciones demo y completa, la sincronización del ranking y el retorno a la bóveda con persistencia completa. El contenido jugable se contrasta con [[04 - Criterios de Aceptación]].

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
