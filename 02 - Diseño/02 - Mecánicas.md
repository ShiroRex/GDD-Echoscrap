# Mecánicas

## Movimiento y dash

La velocidad de desplazamiento se calcula en `PlayerRuntimeStats` con la fórmula:

**Velocidad de movimiento = `6 m/s × (1 + 0,04 × FLASH_02) × (1,15 si Orco está equipado) × (1,25 si Frenesí BIO_04 está activo)`**

*(En el ajuste de escena `PlayerCombatSettings.asset`, la velocidad base de locomoción se configura en `8 m/s`, con aceleración de `80 m/s²` y frenado de `70 m/s²`.)* La diagonal mantiene la misma velocidad máxima. La dirección de entrada se interpreta respecto a la cámara y el personaje se apoya sobre el relieve y las escaleras del escenario.

El dash parte de una velocidad base de `18 m/s` en `PlayerRuntimeStats` (`22 m/s` durante `0,15 s`, unos `3,3 m` en `PlayerCombatSettings`), concede invulnerabilidad activa (`0,22 s` en `PlayerHealth`) y atraviesa cuerpos enemigos. Los muros y obstáculos sólidos limitan el recorrido. La recarga base es de `1,0 s`. La mejora `DASH_01` (*Dash ágil*) añade `+8%` de velocidad de dash y `-10%` de recarga por nivel; al adquirir `DASH_04` (*Cataclismo*), la recarga del dash queda fijada en **`0,5 s`** (anulando la reducción porcentual de recarga de `DASH_01`), pero **se conserva íntegramente el bono de `+8%` por nivel a la velocidad del dash de `DASH_01`**.

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

Este multiplicador divide tanto el intervalo entre pulsos (`0,20 s / mult`) como la pausa posterior al cuarto pulso (`0,40 s / mult`, o `0 s` con `FLASH_05`). La ráfaga base emite en `0,2`, `0,4`, `0,6` y `0,8 s` y vuelve a empezar tras el descanso. La linterna busca objetivos en `360°` dentro de su alcance y aplica los impactos a su cono frontal; sin objetivos, vuelve hacia la orientación de movimiento.

Cuando el jugador adquiere **`FLASH_05` (*Rayo solar*)**, se elimina el descanso entre ráfagas (`0 s`), el alcance base pasa a `14 m`, el daño de luz se multiplica por `2,5` y en el código (`UpgradeCombatRuntime.cs:169`, `bool finalPulse = hit.PulseNumber >= 4 || attackTiers[(int)UpgradeId.FLASH_05] > 0;`) **todos los pulsos del rayo continuo cuentan como cuarto pulso (`finalPulse = true`)**. En consecuencia, cada pulso de *Rayo solar* aplica empuje y además activa todas las mejoras vinculadas al cuarto pulso: **`FLASH_03` (*Prisma voltaico*)**, **`BIO_02` (*Sifón*)** y **`CRIT_04` (*Ráfaga crítica*)**.

El alcance y el ángulo se evalúan en el plano horizontal X/Z y tienen en cuenta el radio del objetivo. Tanto el autoapuntado como cada impacto trazan una línea 3D desde la luz hasta el centro del enemigo. Los obstáculos sólidos de la capa `Default` pueden bloquearla aunque el enemigo esté dentro del cono. El trazado ignora los triggers. Cada enemigo válido recibe como máximo un impacto por pulso.

Al comenzar la ráfaga se fijan alcance, apertura, daño base por pulso, intervalo, descanso, empuje, probabilidad y multiplicador críticos y modo solar. Una mejora que cambie esos parámetros durante la secuencia se refleja al comenzar la siguiente. La dirección de apuntado y la visibilidad de cada objetivo se vuelven a evaluar durante la ráfaga.

## Salud y daño

La **salud máxima** del Coleccionista se calcula mediante la fórmula:

**Salud máxima = `(100 + 20 × BIO_01 + 10 × SCRAP_01 + 10 × [Lobito] + 15 × [Soldier]) × (2 si Yanikua está equipada)`**

El daño puede ser físico o sobrenatural según su fuente; los números flotantes conservan esa categoría. La probabilidad crítica empieza en `0%` y su multiplicador base es `×1,75`.

Cada vez que el jugador recibe un impacto, `PlayerHealth.TryApplyIncomingDamage` evalúa la mitigación en este **orden exacto de 7 pasos**:

1. **Invulnerabilidad activa:** comprueba si el jugador está en dash (`0,22 s`), en ventana de protección post-golpe (`0,75 s` en `PlayerHealth`; `0,40 s` en el ajuste de escena), bajo el escudo de *Nanobots* (`2 s`) o bajo la protección de *Yanikua* (`3 s`). Si cualquiera está activo, el golpe se ignora por completo.
2. **Bloqueo periódico del acompañante Hada:** si está disponible su recarga, anula `1` golpe cada `20 s`.
3. **Tirada de Evasión:** hasta un tope del `35%` (`8% × nivel` en `PlayerRuntimeStats`). Si la tirada tiene éxito, anula el daño sin activar los `0,75 s` de invulnerabilidad post-golpe.
4. **Reducción del acompañante Soldier:** multiplica el daño entrante por `0,85` (`-15%` de daño recibido).
5. **Tirada de Bloqueo:** `15%` con el acompañante **Caballero** (tope técnico del `50%`). Si bloquea, reduce el daño a la mitad (`× 0,5`).
6. **Armadura:** suma `10% × SCRAP_01 + 10% [Golem] + 10% [Soldier]` en `PlayerRuntimeStats` (`+4 puntos porcentuales` por nivel de `SCRAP_01` en `UpgradeTuning`), hasta un tope máximo del **`60%`**, y multiplica el daño restante por `1 - armadura`.
7. **Salvación ante daño letal o umbral crítico:** si el daño resultante sería letal, primero intenta activar la salvación única por expedición de **Yanikua** (restaura al `30%` de la salud máxima y otorga `3 s` de invulnerabilidad); si no está disponible, evalúa **Nanobots** (cuando el golpe deja la vida por debajo del `20%` o resulta letal y su enfriamiento de `60 s` está libre, frena la caída en el `20%` de la salud máxima y concede `2 s` de escudo).

| Estado del enemigo | Aplicación y expiración |
| --- | --- |
| Ralentización | *Freón residual* se aplica antes de contar el mismo pulso para *Cero absoluto*. Cada aplicación conserva la intensidad y el tiempo mayores. Al expirar se pierden los impactos de frío acumulados. |
| Congelación | *Cero absoluto* solo cuenta si hay ralentización activa y el objetivo sigue vivo y sin congelar. Congelar reinicia ese contador y conserva la duración mayor. *Ventisca* y *Quiebre de hielo* pueden congelar directamente. El objetivo congelado recibe `×1,30` de daño de ataques del jugador. |
| Aturdimiento | Conserva el tiempo restante mayor. Igual que la congelación, bloquea el control del enemigo mientras el temporizador esté activo. |
| Empuje (*knockback*) | Se resuelve según la susceptibilidad y la desaceleración de cada arquetipo (tabla inferior). |

### Respuesta al empuje (*knockback*) por arquetipo

| Arquetipo | Susceptibilidad al empuje | Desaceleración de frenado (`EnemyDefinition` / asset) |
| --- | ---: | ---: |
| Chatarra Residual | `1,0` | `28 m/s²` (`18 m/s²` en asset) |
| Interceptor | `1,0` | `18 m/s²` (`12 m/s²` en asset) |
| Mortero Campana | `0,7` (`0,8` en asset) | `24 m/s²` (`18 m/s²` en asset) |
| Polilla Reactor | `1,0` | `22 m/s²` (`18 m/s²` en asset) |
| Caballero Fantasma (élite) | `0,35` | `28 m/s²` (`24 m/s²` en asset) |
| Coloso | `0` (inmune) | — |
| Centinela de Cobre (élite) | `0` (inmune) | — |
| Gólgora (jefe) | `0` (inmune) | — |

Los números flotantes muestran la salud realmente perdida. Los impactos normales de una ráfaga pueden agruparse, mientras que los críticos se distinguen con tamaño, color y un signo de exclamación. Los impactos mitigados por resistencia muestran «Resistido» bajo el valor y una ejecución causada por `CRIT_03` muestra «EJECUTADO».

## Recogida

El imán base alcanza `2,5 m` y atrae los objetos a `12 m/s`. Las gemas conceden experiencia; el botiquín base recupera `15 HP`. El ácido y las áreas de ataque utilizan las mismas reglas de salud e invulnerabilidad que el resto del combate. Las zonas de lava fija del mapa están desactivadas; el magma de Gólgora sigue formando parte del encuentro.

Una gema nueva puede sumarse a una gema activa a un máximo de `0,75 m` de distancia horizontal. Se acumulan la XP y el número de gemas originales. *Micro-ensamblador* cuenta las gemas originales del objeto recogido. La XP otorgada es la parte entera de `XP acumulada del objeto × multiplicador de XP + resto anterior`. El resto se conserva para la próxima recogida de la expedición. *Chatarrero* añade `15%` de XP por nivel.

## Entidades y poderes de inicio

Hasta tres copias equipadas en los puestos del balcón (`EQUIPO 1`, `EQUIPO 2`, `EQUIPO 3`) activan poderes propios, independientes de las 35 cartas: bonos de estadísticas, ataques automáticos, curación, protección o salvación. Repetir una misma entidad en varios puestos no acumula su efecto. Las cartas mantienen sus niveles y requisitos y no reciben niveles iniciales por equipo. El catálogo completo, las recetas de fusión de `30 s` y el Índice de Colección (`[I]`) están en [[04 - Objetos y Coleccionables]].

Los coleccionables que aparecen en la arena requieren pulsar **`E`** dentro de unos `2,5 m` con línea de interacción despejada, y no se recogen con el imán de gemas y botiquines. Si el jugador ya ha alcanzado el límite máximo de **10 unidades** en su colección (`10/10`), el fragmento no puede recogerse y muestra en pantalla el aviso flotante **`INVENTARIO LLENO / LIBERA ESPACIO`**.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
