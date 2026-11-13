# Mundo

El Vertedero de Ecos es un espacio atemporal donde se mezclan restos de épocas y realidades distintas. La chatarra, las ruinas y la energía residual dan continuidad a esa mezcla. La bóveda funciona como refugio y centro de preparación entre expediciones.

| Lugar | Presencia actual | Función |
| --- | --- | --- |
| Vertedero | Arena de 200 × 200 m con relieve, obstáculos y restos industriales. | Supervivencia frente a la horda, obtención de XP y recogida manual de entidades. |
| Caldera central | Área circular de 22 m de radio con cierre perimetral durante el encuentro. | Combate contra Gólgora y sus refuerzos. |
| Acceso norte | Descenso, muros, rellano y portal (`x = 0, z = 78`). | Conexión física con la bóveda. |
| Bóveda | Sala modular en dos niveles con **10 pedestales físicos** (`DisplaySlots[10]`, capacidad máxima de 10 unidades): **3 puestos de equipo activo** en el balcón superior (`EQUIPO 1..3`, que también exhiben visualmente reservas como `RESERVA` si están vacíos) y **7 puestos de exposición/reserva** en la planta baja (`slot >= 3`, con liberación directa `E · LIBERAR`), guiada por `VaultDirections`, `VaultTips` y el **Índice de Colección (`[I]`)**. | Equipar poderes acumulables, consultar el índice de las 15 entidades y liberar copias para gestionar el espacio. |
| Máquina de fusión | Cámara central con persiana dentro de la bóveda (`30 s` fijos de proceso). | Consumir cuatro copias de la misma rareza o de rarezas mixtas (excluida Yanikua) y reclamar una nueva entidad con `COLECCIONAR`. |

Las gemas de XP impulsan el crecimiento durante una incursión. Las entidades pasan a una colección persistente de hasta **10 unidades en posesión** y un registro de **15 entidades descubiertas**, cuyas reglas se detallan en [[04 - Objetos y Coleccionables]].

Las tres zonas de lava fija del mapa están desactivadas en el montaje actual; el magma se reserva para Gólgora y sus patrones de ataque. La narrativa de origen se conserva en [[01 - Premisa y Trama]].

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
