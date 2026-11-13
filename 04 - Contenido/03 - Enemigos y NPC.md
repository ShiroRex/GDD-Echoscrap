# Enemigos y NPC

El combate reúne cinco arquetipos de horda, dos élites y Gólgora. Las fichas visuales, spritesheets, fuentes editables y GIFs por acción se documentan en [[02 - Personajes y Animación]]. Caballero Fantasma y Centinela de Cobre (gólem) usan sus sprites animados integrados, con mapas de normales, emisión y secuencias sincronizadas con los estados de combate.

La [[02 - Personajes y Animación#Proporciones en reposo|comparación a escala]] muestra a los ocho enemigos junto al Coleccionista.

| Enemigo | Vida | Velocidad base | Daño principal | Susceptibilidad al empuje | Frenado (`EnemyDefinition` / `.asset`) | XP |
| --- | ---: | ---: | --- | ---: | ---: | ---: |
| Residual / Esqueleto | 15 HP | 3,5 m/s ±15% | Contacto: 10 (físico); recarga 0,5 s. | `1,0` | `28 m/s²` | 10 |
| Interceptor | 9 HP | 6,2 m/s ±10% | Contacto: 8 (físico); recarga 0,5 s. | `1,0` | `18 m/s²` | 10 |
| Coloso / Wendigo | 80 HP | 1,8 m/s | Contacto o carga: 25 (físico). | `0` (inmune) | — | 50 |
| Mortero Campana | 25 HP | 2,2 m/s ±10% | Contacto: 5; ácido: 5 cada 0,5 s. | `0,7` | `24 m/s²` | 25 |
| Polilla Reactor | 12 HP | 4,8 m/s ±10% | Explosión en 3 m: 35 al jugador / 85% (`29,75`) a enemigos normales. | `1,0` | `22 m/s²` | 15 |
| Caballero Fantasma | 110 HP | 2,8 m/s | Barrido sobrenatural: 22. | `0,35` | `28 m/s²` | 65 |
| Centinela de Cobre | 160 HP | 1,7 m/s | Abanico de tres proyectiles físicos: 16 por impacto. | `0` (inmune) | — | 80 |
| Gólgora / MataColeccionista | 15 000 HP base (`3 600` demo; `9 000` completa) | 1,6 m/s en Fase 1 (`×1,3` en Fase 2) | Contacto: 30; seis patrones descritos más abajo. | `0` (inmune) | — | 500 |

## Residual: Esqueleto

Forma la masa básica de la horda. Persigue al jugador, se separa de otros cuerpos y busca paso entre obstáculos. Los cuatro pulsos base de la linterna suman sus `15 HP` cuando todos aciertan. Recibe el empuje completo (`susceptibilidad = 1,0`, desaceleración `28 m/s²`) y su ataque de contacto reproduce la animación de golpe.

[Ver spritesheet, fuente editable y GIFs de todas sus acciones](../05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#residual-esqueleto).

## Interceptor: galgo de chatarra

Anticipa hasta `0,4 s` del movimiento del jugador, con una velocidad de referencia limitada a `8 m/s`. A corta distancia pasa a perseguir la posición actual. Con susceptibilidad `1,0` y menor desaceleración de frenado (`18 m/s²`), tras recibir empuje se desliza más lejos que el Residual.

[Ver spritesheet, fuente editable y GIFs de todas sus acciones](../05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#interceptor-galgo-de-chatarra).

## Coloso: Wendigo

Es pesado, sólido e inmune al empuje (`susceptibilidad = 0`). Cuando dispone de carga y el jugador está a `8 m` o menos, fija la dirección y muestra un corredor de aviso durante `0,8 s`. Embiste a `12 m/s` durante `0,35 s` (unos `4,2 m` libres) y se recupera durante `0,6 s`. Las paredes detienen el recorrido y la dirección queda comprometida tras el aviso. El intervalo entre cargas se sortea entre `5` y `7 s`.

Preparación y recuperación no infligen daño de contacto. El dash del jugador permite atravesar su cuerpo durante la ventana de invulnerabilidad.

[Ver spritesheet, fuente editable y GIFs de todas sus acciones](../05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#coloso-wendigo).

## Mortero: Campana

Mantiene una banda de distancia entre `5,5` y `6,5 m` y ataca hasta `12 m`. Tiene un intervalo de `5 s`, aviso de `0,45 s` y vuelo de `0,85 s`. El proyectil deja un charco ácido de radio `1,2 m` durante `3 s`, con `5` de daño cada `0,5 s`. Su susceptibilidad al empuje es reducida (`0,7`, desaceleración `24 m/s²`). El director de oleadas limita los Morteros simultáneos a seis.

[Ver spritesheet, fuente editable y GIFs de todas sus acciones](../05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#mortero-campana).

## Polilla: Reactor

Se aproxima al Coleccionista y **arma su espoleta por proximidad al situarse a `1,6 m` del jugador** (`ExplosiveMoth.asset`, `EnemyActor.cs:433`). La cuenta atrás por proximidad dura **`0,9 s`**; si la Polilla muere por daño del jugador antes de detonar, entra en una espoleta de muerte de **`1,2 s`**.

Al detonar en un radio de **`3 m`**, inflige **`35` de daño físico al jugador** y un **`85 %` de ese daño (`29,75`) a otros enemigos normales** dentro del radio (`EnemyActor.cs:470, 479`, sin afectar a Gólgora). Durante la mecha conserva su susceptibilidad al empuje (`1,0`, desaceleración `22 m/s²`), por lo que el cuarto pulso de la linterna o el *Rayo solar* pueden empujarla hacia el interior de la horda antes de que estalle.

[Ver spritesheet, fuente editable y GIFs de todas sus acciones](../05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#polilla-reactor).

## Caballero Fantasma

Es uno de los dos élites. Recibe solo el `25%` del daño físico y el `100%` del daño sobrenatural, con susceptibilidad reducida al empuje (`0,35`, desaceleración `28 m/s²`). Su barrido tiene `3,6 m` de alcance, arco de `120°`, daño sobrenatural de `22`, preparación de `0,8 s`, recuperación de `0,8 s` y recarga de `3,5 s`. No causa daño por contacto normal.

El cuerpo usa su sprite de 224 fotogramas: la preparación ocupa `0,8 s`, y el ataque de seis fotogramas junto con la recuperación de seis comparten los `0,8 s` posteriores al barrido real.

![Caballero Fantasma: sprite integrado en Unity, 5 de noviembre](../Media/Avance_2026-11-05/Enemigos/CaballeroFantasma_Integrado.gif)

## Centinela de Cobre

Recibe el `25%` del daño sobrenatural salvo durante su recuperación, cuando se abre su ventana de vulnerabilidad y recibe el daño completo (`100%`). El daño físico se mantiene siempre completo. Prepara un abanico de tres proyectiles durante `1,0 s`, se recupera durante `2,0 s` y recarga en `3,5 s`. El alcance es de `11 m` y cada impacto causa `16` de daño físico. Es pesado, inmune al empuje (`susceptibilidad = 0`) y no causa daño por contacto normal.

La presentación anuncia los rayos del abanico y muestra «Vulnerable» en su recuperación. El cuerpo usa su sprite de 288 fotogramas: abre las compuertas durante `0,3 s` y prepara `0,7 s` antes del disparo real; después reproduce disparo de `0,4 s`, vulnerabilidad de `1,3 s` y cierre de `0,3 s` dentro de los `2,0 s` de recuperación.

![Centinela de Cobre: sprite integrado en Unity, 5 de noviembre](../Media/Avance_2026-11-05/Enemigos/CentinelaCobre_Integrado.gif)

Los élites empiezan a aparecer desde los `90 s` y se alternan cada `25 s` cuando hay capacidad, con un límite conjunto de dos élites vivos. Sus efectos de barrido y proyectiles se documentan en [[06 - Registro visual]].

## Gólgora: MataColeccionista

En código (`GolgoraCombatSettings.cs`) y su perfil `CombatSettings.asset` ya unificado, la salud máxima por defecto de Gólgora es de **15 000 HP** (transición a Fase 2 al bajar al `50%` = `7 500 HP`), mientras que los perfiles de ronda asignan **3 600 HP** en la ronda demo (`3:00`) y **9 000 HP** en la ronda completa (`8:00`). Su entrada retira la horda ordinaria sin otorgar recompensas y traslada al jugador con protección a la caldera central de `22 m` de radio, cuyo perímetro queda cerrado.

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

[Ver los seis patrones de Gólgora en GIFs capturados en Unity](../05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#g%C3%B3lgora-matacoleccionista).

### Segunda fase y resistencia al control

Al bajar al `50%` de vida se inicia una transición invulnerable de **`1,5 s`** con un rugido que empuja al jugador en un radio de **`12 m`**. En Fase 2 la velocidad de movimiento pasa a `×1,3`; las preparaciones compatibles se multiplican por `0,85` (*Fisura de magma* mantiene sus `1,1 s` fijos); y las recuperaciones y enfriamientos compatibles se multiplican por `0,75` (*Cerco de magma* mantiene sus `18 s` fijos de enfriamiento).

Gólgora es inmune al empuje (`susceptibilidad = 0`). La ralentización tiene un máximo del `25%`. Congelaciones y aturdimientos se reducen al `35%` de su duración, con un máximo de `0,6 s` y una ventana de inmunidad posterior de `1,5 s`. Durante introducción, transición y salto es invulnerable, y durante la emisión activa del *Cerco de magma* aplica un `35%` de reducción de daño.

### Refuerzos y cierre

El director solicita grupos de hasta cuatro secuaces cada `4 s` (con límite de `8` secuaces activos o anunciados en la caldera y aviso previo de `0,9 s`), rotando el orden Residual, Residual, Mortero y Polilla. El *Cerco de magma* y la transición de fase suspenden los refuerzos y retiran los presentes.

La muerte de Gólgora con el jugador vivo otorga la victoria, reproduce la absorción de su gema, suma `500 XP` al resumen y sincroniza la partida con el ranking del evento (`EventLeaderboardSync`).

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
