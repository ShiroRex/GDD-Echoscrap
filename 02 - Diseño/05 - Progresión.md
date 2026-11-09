# Progresión

## Experiencia y elecciones

El Coleccionista empieza en nivel 1. La primera subida necesita 100 XP; el siguiente umbral es el anterior multiplicado por 1,5 y redondeado a entero. La experiencia que sobra se conserva para el siguiente nivel. Una misma recogida puede acumular varias elecciones.

Cada elección ofrece hasta tres cartas distintas. La rareza base se sortea con pesos Común 60, Rara 25, Épica 12 y Legendaria 3, considerando únicamente las rarezas que todavía tienen cartas válidas. Después se elige una carta de esa rareza. Las cartas al máximo se retiran de la oferta.

*Sintonizador* permite gastar reintentos durante la sesión. También mueve peso hacia raras y épicas: cinco y tres puntos por nivel, respectivamente, desde el peso común. El peso legendario permanece en tres.

## Catálogo de 35 mejoras

Las series separadas por barras corresponden a sus niveles. Los valores describen la mejora por sí sola; otras cartas y acompañantes pueden modificar daño, radios, críticos y estadísticas finales.

| ID | Mejora | Rareza | Nivel máximo | Efecto actual | Tipo de daño |
| --- | --- | --- | ---: | --- | --- |
| FLASH_01 | Más alcance | Común | 5 | +1 m de alcance y +10° de apertura por nivel. | Sobrenatural |
| FLASH_02 | Pulso rápido | Común | 5 | +15% de cadencia de ataque por nivel (y +4% de velocidad de movimiento en `PlayerRuntimeStats`); reduce intervalo y descanso. | Sobrenatural |
| FLASH_03 | Prisma voltaico | Rara | 3 | El cuarto pulso (y todos los pulsos con `FLASH_05`) enlaza 2/3/4 enemigos a 5 m, con 40/60/80% del daño del impacto inicial. | Sobrenatural |
| FLASH_04 | Golpe potente | Épica | 3 | Daño ×1,5/1,8/2,2 y +8/14/20 m/s de empuje; reduce 20° la apertura al adquirirla. | Sobrenatural |
| FLASH_05 | Rayo solar | Legendaria | 1 | Suprime el descanso (`0 s`), establece 14 m de alcance base, multiplica el daño de luz por 2,5 y convierte **todos los pulsos** en cuarto pulso (`finalPulse = true`), aplicando empuje y activando `FLASH_03`, `BIO_02` y `CRIT_04` en cada pulso. | Sobrenatural |
| DASH_01 | Dash ágil | Común | 5 | +8% de velocidad de dash y -10% de recarga por nivel (el bono de +8%/nivel de velocidad se conserva con `DASH_04`). | Sin daño directo |
| DASH_02 | Onda sísmica | Rara | 3 | Al terminar el dash: 25/45/70 de daño en 3,5/4,5/5,5 m y aturdimiento de 0,6/0,9/1,2 s. | Físico |
| DASH_03 | Holo-señuelo | Épica | 3 | Deja una copia que atrae enemigos en 12 m durante 2/2,5/3 s; su implosión en 4 m causa 50/85/120 de daño. | Sobrenatural |
| DASH_04 | Cataclismo | Legendaria | 1 | Fija la recarga del dash en **0,5 s** (anulando la reducción de recarga de `DASH_01`, pero **manteniendo su +8%/nivel de velocidad de dash**), con explosión de salida de 120 (4 m) y cuchilla de llegada de 80. | Físico |
| BIO_01 | Más vida | Común | 5 | +20 HP máximos y curación de 15 HP en cada adquisición. | Sin daño directo |
| BIO_02 | Sifón | Rara | 3 | El cuarto pulso (y cada pulso con `FLASH_05`) tiene 12/20/30% de curar 2/3/4 HP por enemigo alcanzado. | Sin daño directo |
| BIO_03 | Nanobots | Épica | 3 | Regenera 1/2/3 HP cada 2,5 s; al recibir un golpe que deje bajo 20% de vida o sea letal, frena la caída en 20% y protege 2 s (`60 s` de enfriamiento). | Sin daño directo |
| BIO_04 | Nova de plasma | Legendaria | 1 | Cada 15 HP de curación sobrante generan una nova de 80 de daño y 6 m de radio. | Sobrenatural |
| GREED_01 | Imán | Común | 5 | +2 m al radio de recogida por nivel. | Sin daño directo |
| GREED_02 | Chatarrero | Común | 5 | +15% de experiencia recibida por nivel. | Sin daño directo |
| GREED_03 | Botiquín mejor | Rara | 3 | Botiquines de 17/19/21 HP; aumentan su probabilidad de aparición (4/5/6% normal y 10/12/15% en pesados/élites). | Sin daño directo |
| GREED_04 | Sintonizador | Épica | 3 | Concede 1/2/3 reintentos totales y desplaza peso común hacia ofertas raras (+5/nivel) y épicas (+3/nivel). | Sin daño directo |
| GREED_05 | Pulso colector | Legendaria | 1 | Atrae todas las gemas y botiquines activos del mapa al adquirirla y al subir de nivel. | Sin daño directo |
| CRIT_01 | Óptica precisa | Común | 5 | +8 puntos porcentuales de probabilidad crítica por nivel. | Según ataque |
| CRIT_02 | Calibración letal | Común | 5 | +0,2 al multiplicador crítico por nivel, desde ×1,75. | Según ataque |
| CRIT_03 | Ejecución | Rara | 3 | Remata tras un golpe a enemigos bajo 6/8/10% de vida; excluye a los jefes. | Según ataque |
| CRIT_04 | Ráfaga crítica | Épica | 3 | Un crítico en el cuarto pulso (o en cualquier pulso con `FLASH_05`) garantiza 2/3/4 críticos al inicio de la siguiente ráfaga. | Según ataque |
| CRIT_05 | Ojo del verdugo | Legendaria | 1 | +20 puntos porcentuales de crítico; bajo 35% de vida antes del golpe, triplica el daño crítico ya calculado y emite aturdimiento de 1 s en 3 m. | Según ataque |
| DEMO_01 | Pólvora volátil | Común | 5 | +12% de daño y +15% de radio de explosión por nivel. | Según explosión |
| DEMO_02 | Reacción en cadena | Rara | 3 | 25/40/55% de provocar una explosión de 35/60/90 (radio 3 m) al morir un enemigo. | Físico |
| DEMO_03 | Minas de chatarra | Épica | 3 | Armado en `0,25 s`, radio de activación de `1,25 m`, empuje de `5 m` y límite de **5 minas activas simultáneas** (reemplaza la más antigua al colocar la sexta). En `UpgradeCombatRuntime` deja 1 mina al hacer dash (`45/70/100` en `3 m`); en catálogo/tuning periódico cada `4/3,2/2,5 s` (`70/110/160` en `4 m`). | Físico |
| DEMO_04 | Termobárica | Legendaria | 1 | A los **0,4 s**, toda explosión repite el **75%** de su daño en el mismo radio y atrae a los enemigos hacia el centro una distancia base del **45% del radio de la explosión** (impulso `12 m/s`, modulado por susceptibilidad al empuje). | Sobrenatural |
| SCRAP_01 | Blindaje | Común | 5 | +10 HP máximos y armadura por nivel (+4 puntos porcentuales en `UpgradeTuning` / +10% en `PlayerRuntimeStats`, tope total 60%). | Sin daño directo |
| SCRAP_02 | Micro-ensamblador | Rara | 3 | Cada 15 gemas recogidas lanza un proyectil guiado de 40/70/105 a 24 m/s. | Físico |
| SCRAP_03 | Sierras orbitales | Épica | 3 | `1/2/3` hojas en `UpgradeCombatRuntime` (`2/3/4` en catálogo) giran a **180°/s** a **2,5 m** de distancia (`0,65 m` de radio de hoja), infligiendo `18/28/40` de daño físico y `4 m/s` de empuje en cada paso de simulación en que la hoja solape al enemigo. | Físico |
| SCRAP_04 | Torreta centinela | Legendaria | 1 | Hasta **2 torretas activas simultáneas** (`12 s` de duración al recoger chatarra en `UpgradeCombatRuntime`; `8 s` cada `10 s` en catálogo). Cada **0,6 s** disparan una ráfaga de **3 proyectiles** separados por **0,08 s** a **28 m/s** (`10/16/24` de daño y `12 m` de alcance en runtime; `35` a `8 m` y `24 m/s` en catálogo). | Físico |
| CRYO_01 | Freón residual | Común | 5 | La luz ralentiza 12/20/28/36/44% durante 1,5 s. | Sobrenatural |
| CRYO_02 | Cero absoluto | Rara | 3 | Congela tras 4/3/3 pulsos sobre objetivos ralentizados durante 1,2/1,8/2,4 s; el enemigo congelado recibe ×1,30 de daño. | Sobrenatural |
| CRYO_03 | Ventisca | Épica | 3 | Cada 6/5/4 s: onda de 30/50/75 en 5,5 m que congela 1,5 s. | Sobrenatural |
| CRYO_04 | Quiebre de hielo | Legendaria | 1 | Una muerte congelada emite seis esquirlas perforantes de 50 (14 m/s, 1,2 s) que congelan 1,5 s. | Sobrenatural |

## Interacciones clave entre mejoras

- **`FLASH_05` (*Rayo solar*) con mejoras de cuarto pulso (`UpgradeCombatRuntime.cs:169`):** la condición interna `bool finalPulse = hit.PulseNumber >= 4 || attackTiers[(int)UpgradeId.FLASH_05] > 0;` hace que cada pulso del rayo continuo se evalúe como cuarto pulso. Por ello, **todos los pulsos de `FLASH_05` aplican empuje y activan `FLASH_03` (*Prisma voltaico*), `BIO_02` (*Sifón*) y `CRIT_04` (*Ráfaga crítica*)**.
- **`DASH_04` (*Cataclismo*) con `DASH_01` (*Dash ágil*) (`PlayerRuntimeStats.cs:75-77`):** `DASH_04` fija la recarga del dash en `0,5 s` e ignora la reducción porcentual de recarga de `DASH_01` (`-10%` por nivel), pero mantiene activo el multiplicador de velocidad de desplazamiento del dash de `DASH_01` (`+8%` por nivel).
- **`SCRAP_03` (*Sierras orbitales*):** aunque `UpgradeTuning.cs` declara el campo serializado `bladeHitCooldown = 0.30f`, el método `TickBlades` aplica el daño (`18 / 28 / 40`) y el empuje (`4 m/s`) en cada paso de simulación en que el radio de la hoja (`0,65 m` a `2,5 m` del jugador, girando a `180°/s`) solape al enemigo.

## Fórmula de Ojo del verdugo

*Ojo del verdugo* añade 20 puntos porcentuales a la probabilidad crítica, limitada al 100%. Su multiplicador adicional solo se activa si el ataque resulta crítico y el objetivo está estrictamente por debajo del 35% de su vida máxima **antes** de recibir ese golpe. Un impacto que cruza desde 35% o más hasta ese umbral no recibe el bono todavía.

Para un ataque del jugador, antes de la ejecución y del límite de salud realmente perdida:

**Daño resuelto = daño de la fuente × multiplicador crítico × bono de daño de entidades × bono por congelación × multiplicador de Ojo × resistencia del objetivo.**

El multiplicador crítico vale `1` en un impacto normal y parte de `1,75` en un crítico; *Calibración letal* añade `0,2` por nivel. El multiplicador de *Ojo del verdugo* vale `3` cuando se cumplen sus condiciones y `1` en los demás casos. El bono por congelación es `1,30` mientras el objetivo esté congelado.

Ejemplo sin otras bonificaciones ni resistencias: un ataque de `10` causa `17,5` con un crítico base; si *Ojo del verdugo* está activo y el objetivo ya estaba bajo `35%`, causa **`10 × 1,75 × 3 = 52,5`**.

Un crítico que cumple la condición emite un área de aturdimiento de `3 m` alrededor del impacto durante `1 s`. Gólgora aplica sus límites de control: duración reducida al `35%`, máximo de `0,6 s` y ventana de inmunidad de `1,5 s`. *Ejecución* (`CRIT_03`) se comprueba después del daño y sus resistencias; los números muestran la salud realmente perdida.

## Requisitos y combinaciones

- **Pólvora volátil (`DEMO_01`) y Termobárica (`DEMO_04`):** requieren *Onda sísmica*, *Holo-señuelo*, *Cataclismo*, *Reacción en cadena*, *Minas de chatarra* o *Nova de plasma*.
- **Cero absoluto (`CRYO_02`):** requiere *Freón residual* (`CRYO_01`).
- **Quiebre de hielo (`CRYO_04`):** necesita *Cero absoluto* (`CRYO_02`) o *Ventisca* (`CRYO_03`).
- **Ráfaga crítica (`CRIT_04`):** requiere una probabilidad crítica mayor que cero.

*Prisma voltaico* se activa en el cuarto pulso de luz (o en cada pulso con *Rayo solar*) y su daño derivado no inicia otra cadena. La repetición de *Termobárica* tampoco vuelve a repetirse a sí misma.

## Parámetros complementarios de balance

| Mejora | Parámetros de aplicación |
| --- | --- |
| Prisma voltaico (`FLASH_03`) | Intervalo de `0,045 s` entre saltos; radio de salto `5 m`. |
| Golpe potente (`FLASH_04`) | Añade `8 / 14 / 20 m/s` al empuje base de `10 m/s`, para un total de `18 / 24 / 30 m/s` antes de la susceptibilidad. |
| Onda sísmica (`DASH_02`) | Radio de `3,5 / 4,5 / 5,5 m` y empuje nominal de `8 m`, sujeto al enemigo y al terreno. |
| Holo-señuelo (`DASH_03`) | Radio de atracción de `12 m` y radio de implosión de `4 m`. |
| Cataclismo (`DASH_04`) | Recarga fija de `0,5 s` (conservando `+8%/nivel` de velocidad de `DASH_01`). Radio inicial de explosión de `4 m`. La cuchilla viaja a `18 m/s`, tiene radio de impacto de `0,45 m` y dura `0,6 s`. |
| Botiquín mejor (`GREED_03`) | Probabilidad base de aparición de `3%` y de `4 / 5 / 6%` con la carta. Para Coloso, Mortero y ambos élites, `8%` base y `10 / 12 / 15%` con la carta. |
| Reacción en cadena (`DEMO_02`) | Radio base de `3 m`. |
| Minas de chatarra (`DEMO_03`) | Retardo de armado de `0,25 s`, radio de activación de `1,25 m`, radio de explosión de `3 m` (`4 m` en catálogo), empuje de `5 m` y máximo de **5 minas simultáneas**. |
| Termobárica (`DEMO_04`) | Repite el `75%` del daño a los `0,4 s` en el mismo radio y atrae una distancia base del **45% del radio de explosión** (impulso `12 m/s`, modulado por susceptibilidad). |
| Micro-ensamblador (`SCRAP_02`) | El proyectil guiado viaja a `24 m/s` y dura hasta `10 s`. Cada umbral de `15` gemas consume ese contador aunque falte un objetivo válido. |
| Sierras orbitales (`SCRAP_03`) | Giran a `180°/s` a `2,5 m` de radio (`0,65 m` de hoja), aplicando `18 / 28 / 40` de daño y `4 m/s` de empuje por paso de simulación en solape. |
| Torreta centinela (`SCRAP_04`) | Hasta **2 torretas activas** (`12 s` de vida en runtime; `8 s` en catálogo). Con blanco en alcance (`12 m` / `8 m`), dispara cada `0,6 s` una ráfaga de **3 proyectiles** separados por `0,08 s` a `28 m/s` (`24 m/s` en catálogo). |
| Quiebre de hielo (`CRYO_04`) | Las seis esquirlas viajan a `14 m/s` y duran hasta `1,2 s`. |

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

Antes de elegir ronda, el jugador puede ocupar hasta tres puestos de equipo en el balcón superior de la bóveda (`EQUIPO 1`, `EQUIPO 2`, `EQUIPO 3`), guiado por las flechas direccionales superiores (`VaultDirections`), el panel de consejos (`VaultTips`) y el Índice de Colección (`[I]`). Las entidades activan poderes propios y no conceden niveles de las cartas. Sus efectos se aplican una sola vez por identidad equipada; repetir una entidad ocupa otra copia entre las 10 unidades de capacidad sin duplicar su poder. Las 35 cartas se ofrecen y progresan con sus propios niveles y requisitos, independientemente de ese equipo.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
