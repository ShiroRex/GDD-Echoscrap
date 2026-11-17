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
- **`MYTH_01` (*All In*):** al elegirse en `UpgradeDraftController.TryChoose`, sustituye cada carta poseída distinta de `MYTH_01` por otra mejora no repetida de `UpgradeCatalog` priorizando cartas ajenas a la build previa y con `MaxTier >= nivelAnterior + 1`, sube `+1` nivel cada una (limitado a `MaxTier`) y respeta los topes de salud en `PlayerHealth`.
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
