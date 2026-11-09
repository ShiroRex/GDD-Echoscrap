# Estructura del contenido

| Contenido | Cantidad y uso actual |
| --- | --- |
| Protagonista | El Coleccionista. |
| Escenario de expedición | Vertedero de 200 × 200 m con relieve, obstáculos y acceso norte (`x = 0, z = 78`). |
| Caldera | Encuentro cerrado de 22 m de radio para Gólgora. |
| Bóveda | Diez pedestales físicos (capacidad máxima de **10 unidades**): **3 puestos de equipo** en el balcón superior (`EQUIPO 1..3`) y **7 puestos de exposición** en la planta baja, máquina de fusión central, portal e indicadores `VaultDirections` y `VaultTips`. |
| Índice de Colección | Panel accesible con **`I`** (`VaultCollectionIndex`) con registro de las **15 entidades** (`POR RAREZAS`) y de las **hasta 10 unidades** en posesión (`EN TU BÓVEDA`). |
| Horda | Residual, Interceptor, Coloso, Mortero y Polilla Reactor. |
| Élites | Caballero Fantasma y Centinela de Cobre. |
| Jefe | Gólgora / MataColeccionista, con seis ataques y dos fases. |
| Mejoras | 35 cartas distribuidas en cuatro rarezas, con niveles y requisitos de desbloqueo. |
| Objetos de combate | Gemas de experiencia y botiquines. |
| Entidades | Quince en seis rarezas: nueve se obtienen en la arena y seis como resultados de fusión (`30 s` fijos por fusión). |
| Equipo | Hasta tres entidades equipadas en el balcón con poderes propios de `CompanionPowerRuntime`; puede quedar incompleto. |
| Rondas | Demo (`3:00`) y completa (`8:00`), seleccionables desde la bóveda junto al nombre de jugador para el ranking (`EventLeaderboardSync`). |
| Pruebas | Laboratorio de ocho arquetipos (`EnemyLaboratory`) y sandboxes de locomoción y combate. |

La compilación principal habilita `MainMenu` y `CombatArena`. Las escenas `CombatArena_3Min` y `CombatArena_8Min` permanecen en el proyecto como variantes de desarrollo y están deshabilitadas en la lista principal de compilación. La selección de ronda del jugador utiliza los assets de oleadas dentro de `CombatArena`.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
