# Glosario

| Término | Uso actual (`734e2a5` / `6973556`) |
| --- | --- |
| Coleccionista | Personaje controlado por el jugador, equipado con linterna automática y dash. |
| Vertedero | Arena exterior de expedición de `200 × 200 m`. |
| Bóveda o almacén | Sala modular en dos niveles conectada por portal, con **10 pedestales físicos** (`DisplaySlots[10]`, capacidad máxima de 10 unidades: 3 puestos de equipo en el balcón y 7 puestos de exposición en planta baja) y máquina de fusión central. |
| Entidad o coleccionable | Cada una de las **15 entradas** del catálogo (`ID 0..14`), distribuidas en seis rarezas (*Común, Raro/Poco común, Especial/Rara, Épico, Legendario, Mítico*). |
| Copia o unidad | Instancia en posesión de una entidad (`owned[15]`). La colección admite un máximo estricto de **10 unidades totales** (`FusionProgress.Capacity = 10`). |
| Entidad equipada | Unidad asignada a uno de los **3 puestos de equipo activo** del balcón (`EQUIPO 1..3`, `E` / botón oeste para editar o `T` / botón norte para desequipar). Activa un poder propio de `CompanionPowerRuntime` **acumulable por copia equipada** (`Copies(id)`, hasta `×3`) y puede seleccionarse como ingrediente de fusión (`EN EQUIPO 1/2/3`). |
| Exposición | Los **7 pedestales de la planta inferior** (`slot 3..9`) donde se exhiben las unidades en reserva ordenadas por rareza y desde donde pueden liberarse con `E · LIBERAR`; cuando se superan las 7 copias libres, cualquier puesto de equipo vacío del balcón (`slot 0..2`) exhibe visualmente la reserva marcada como `RESERVA`. |
| Índice de Colección (`[I]`) | Panel de la bóveda (`VaultCollectionIndex`, tecla `I` / `Select`) con contador `{descubiertos} / 15 DESCUBIERTOS · {en posesión} / 10 UNIDADES`, pestañas `POR RAREZAS` (`TODAS` y 6 rarezas, con siluetas negras `???` para las no descubiertas) y `EN TU BÓVEDA`, e inspector derecho de poder. |
| `VaultDirections` y `VaultTips` | Tarjetas direccionales superiores con flechas en vivo hacia Fusión y Balcón de Equipo, y panel inferior izquierdo `CONSEJO` que rota cada `8 s` en la bóveda. |
| Ronda demo / completa | Modalidades elegidas antes de salir: jefe a los `3:00` con `3 600 HP` (`Golgora_Demo.asset`) o a los `8:00` con `9 000 HP` (`Golgora_Complete.asset`). |
| Eco o gema de XP | Experiencia del intento actual, atraída con el imán y fusionable en el suelo a menos de `0,75 m`. |
| Draft | Elección pausada de hasta tres cartas válidas entre las 35 mejoras al subir de nivel. |
| Linterna | Arma sobrenatural de cuatro pulsos con autoapuntado y empuje en el cuarto pulso (o en todos los pulsos con `FLASH_05` *Rayo solar*). |
| Dash | Esquiva rápida con invulnerabilidad, limitada por obstáculos sólidos. |
| Élites | Caballero Fantasma y Centinela de Cobre, con resistencias y ataques propios. |
| Gólgora / MataColeccionista | Jefe final de la caldera de `22 m`, con seis patrones de ataque y dos fases. |
| Cerco de magma | Salto invulnerable al centro con impacto físico de aterrizaje (`40` en `3 m`), anillos de fuego con brecha segura de `60°` (1 onda en Fase 1 y 3 en Fase 2) y enfriamiento fijo de `18 s`. |
| Fusión | Proceso de **30 segundos (`00:30`)** fijos en la máquina central que consume 4 copias de la misma rareza o de rarezas mixtas (excluida Yanikua) y entrega una entidad reclamable con el botón único `COLECCIONAR`. |
| Ranking de evento (`EventLeaderboardSync`) | Sistema que reserva el gamertag mediante hash SHA-256 (`GamertagRegistry`), registra la elegibilidad en `PlayerPrefs` (`EchoScrap.Rank.v2`) y sincroniza métricas mediante HTTPS `PUT` / `DELETE` con Firebase Realtime Database (`https://echoscrap.web.app`). |
| Pool | Reserva de instancias reutilizables en memoria (`Object Pool`). |

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
