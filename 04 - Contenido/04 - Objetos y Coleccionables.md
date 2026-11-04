# Objetos y coleccionables

## Gemas y botiquines

Las gemas conceden la XP del enemigo que las dejó y permanecen hasta recogerse. Los botiquines curan 15 HP de base y caducan tras 22 s. Ambos responden al imán y al Pulso colector; se consumen al llegar al personaje y conservan su valor durante el vuelo.

## Entidades de la colección

Las entidades son copias de un inventario persistente. La colección empieza vacía. Nueve personajes aparecen en la arena y seis son resultados de fusión. Las probabilidades de la tabla corresponden a cada aparición de mapa; suman el 100% entre sus nueve entradas.

| Entidad | Rareza | Fuente | Poder al equiparla |
| --- | --- | --- | --- |
| Lobito | Común | Arena · 25% | Dash ágil |
| Mago | Raro | Arena · 10% | Freón residual |
| Demonio | Raro | Arena · 10% | Reacción en cadena |
| Shireen | Especial | Arena · 5% | Sifón |
| Yanikua | Mítico | Fusión | Rayo solar |
| Basket | Común | Arena · 25% | Pulso rápido |
| Hada | Épico | Arena · 7% | Nanobots |
| Lisa | Legendario | Arena · 3% | Ventisca |
| Reaper | Común | Fusión | Óptica precisa |
| Chico Emo | Raro | Fusión | Onda sísmica |
| Conde Alucard | Especial | Fusión | Sifón |
| Murciélago Cristal | Épico | Fusión | Prisma voltaico |
| Sirena | Legendario | Fusión | Cero absoluto |
| Soldier | Especial | Arena · 5% | Reacción en cadena |
| Joel | Raro | Arena · 10% | Óptica precisa |

Las seis rarezas de entidades son Común, Raro, Especial, Épico, Legendario y Mítico. El catálogo de cartas de combate mantiene sus cuatro rarezas propias.

Cada 30 s de combate activo se intenta generar entre una y tres entidades en posiciones válidas del mapa. Hay un máximo de doce presentes y cada aparición dura diez segundos. El jugador debe acercarse y pulsar E a menos de unos 2,5 m, con la interacción despejada. La recogida incorpora una copia y la guarda en ese momento. Yanikua solo puede obtenerse mediante fusión.

## Selección de entidades equipadas

La bóveda tiene tres puestos de equipo. Se abre el selector con E junto al puesto que se quiere cambiar. El inventario muestra cada copia, sus retratos y su disponibilidad, con paginación cuando hace falta. Se puede cambiar la entidad o dejar el puesto vacío.

Cada copia aporta un nivel de su poder. Varias entidades con el mismo poder acumulan niveles hasta el máximo de la mejora. Las cartas de la expedición se calculan por separado: el nivel efectivo usa el mayor entre cartas y equipo. Una copia equipada no está disponible para consumirse en la máquina o liberarse del inventario.

![Selector de entidades del puesto](../Media/Avance_2026-11-04/Interfaz/SeleccionEntidades.png)

Elegir equipo debe formar parte de la preparación antes de escoger ronda. El sistema está implementado, pero falta señalizarlo de forma que el jugador comprenda sus tres puestos y sus poderes sin tener que encontrarlos por su cuenta.

## Fusión de cuatro copias

La máquina está dentro de la bóveda y se abre con E. Se eligen cuatro copias disponibles; al comenzar se consumen y se sortea una sola vez el resultado. Las equipadas y Yanikua están protegidas como ingredientes.

El sorteo usa la calidad media de los ingredientes. Cada banda ofrece resultados de su rareza y la siguiente, y las mezclas interpolan entre bandas. Los seis resultados posibles son Reaper, Chico Emo, Conde Alucard, Murciélago Cristal, Sirena y Yanikua.

| Cuatro ingredientes de la misma rareza | Tiempo | Resultados |
| --- | ---: | --- |
| Comunes | 60 s | Reaper 60%; Chico Emo 40%. |
| Raros | 120 s | Chico Emo 60%; Conde Alucard 40%. |
| Especiales | 180 s | Conde Alucard 60%; Murciélago Cristal 40%. |
| Épicos | 180 s | Murciélago Cristal 59,4%; Sirena 39,6%; Yanikua 1%. |
| Legendarios | 180 s | Sirena 70%; Yanikua 30%. |

En mezclas, Yanikua tiene 30% con cuatro legendarios, 15% con tres y 5% con dos. Con un legendario o algún épico su probabilidad es del 1%; sin épicos ni legendarios es cero. Esa cuota se reserva antes de repartir el resto de resultados. El tiempo es 60 s cuando todos son comunes, 120 s si la calidad media está por debajo de Especial y 180 s desde esa calidad.

![Máquina con cuatro espacios de ingredientes](../Media/Avance_2026-11-04/Interfaz/FusionCuatro.png)

La selección pausa el mundo. Al iniciar se cierra el menú y el proceso avanza durante el juego activo, también en la bóveda. Las pausas y el tiempo fuera de la aplicación no lo adelantan. Al llegar a cero queda listo: hay que volver a interactuar para abrir la persiana. El resultado espera Coleccionar o Liberar antes de comenzar otra fusión.

## Exposición y duplicados

Los siete puestos de exposición muestran copias disponibles ordenadas por rareza. Las cantidades del inventario pueden superar esos puestos. Los duplicados siguen siendo copias utilizables; la selección de equipo y las recetas respetan cuántas quedan libres.

Liberar una copia del inventario muestra una confirmación con su nombre. Coleccionar el resultado de fusión suma una copia; Liberar descarta ese resultado.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
