# Objetos y coleccionables

## Gemas y botiquines

Las gemas conceden la XP del enemigo que las dejó y permanecen hasta recogerse. Los botiquines curan 15 HP de base y caducan tras 22 s. Ambos responden al imán y al Pulso colector; se consumen al llegar al personaje y conservan su valor durante el vuelo.

## Entidades de la colección

Las entidades son copias de un inventario persistente. La colección empieza vacía. Nueve personajes aparecen en la arena y seis son resultados de fusión. Las probabilidades de la tabla corresponden a cada aparición de mapa; suman el 100% entre sus nueve entradas.

| Entidad | Rareza | Fuente | Poder al equiparla |
| --- | --- | --- | --- |
| Lobito | Común | Arena · 25% | +10 HP máximos. |
| Mago | Raro | Arena · 10% | Proyectil mágico perforante de 18 de daño base cada 10 s. |
| Demonio | Raro | Arena · 10% | +15% de daño; 30% de aplicar quemadura de 4 de daño base por segundo durante 3 s. |
| Shireen | Especial | Arena · 5% | Cura 2 HP cada 5 s. |
| Yanikua | Mítico | Fusión | Duplica la vida máxima; +20% de daño y +15% de movimiento; una salvación por expedición. |
| Basket | Común | Arena · 25% | +10% de velocidad de movimiento. |
| Hada | Épico | Arena · 7% | Bloquea un golpe; se recarga en 20 s. |
| Lisa | Legendario | Arena · 3% | +20% de velocidad de ataque; cada 12 s congela 2 s en un radio de 8 m; al jefe lo ralentiza. |
| Reaper | Común | Fusión | +8% de daño. |
| Chico Emo | Raro | Fusión | Duplica la velocidad de movimiento durante 2 s al terminar el dash. |
| Conde Alucard | Especial | Fusión | Cura 1 HP por cada cinco bajas. |
| Murciélago Cristal | Épico | Fusión | Cada 8 s emite una onda de 15 de daño base en 8 m y ralentiza 40% durante 3 s. |
| Sirena | Legendario | Fusión | Cada 15 s emite una onda de 8 m que empuja 4 m y cura 5% de la vida máxima. |
| Soldier | Especial | Arena · 5% | +15 HP máximos y 15% de reducción de daño recibido. |
| Joel | Raro | Arena · 10% | +15% de velocidad de ataque. |

Las seis rarezas de entidades son Común, Raro, Especial, Épico, Legendario y Mítico. El catálogo de cartas de combate mantiene sus cuatro rarezas propias.

Cada 30 s de combate activo se intenta generar entre una y tres entidades en posiciones válidas del mapa. Hay un máximo de doce presentes y cada aparición dura diez segundos. El jugador debe acercarse y pulsar E a menos de unos 2,5 m, con la interacción despejada. La recogida incorpora una copia y la guarda en ese momento. Yanikua solo puede obtenerse mediante fusión.

## Selección de entidades equipadas

La bóveda tiene tres puestos de equipo. Se abre el selector con E junto a un puesto vacío. En un puesto ocupado, T desequipa la entidad; entonces E permite elegir otra. E sobre un puesto ocupado abre la confirmación para liberar esa copia. El inventario muestra cada copia, sus retratos y su disponibilidad, con paginación cuando hace falta. Se puede cambiar la entidad o dejar el puesto vacío.

Las entidades equipadas activan poderes propios de CompanionPowerRuntime, independientes del catálogo de cartas. Una misma entidad activa su poder una sola vez, aunque se equipen varias copias; esas copias adicionales siguen reservadas y no aumentan el efecto. Las cartas conservan sus niveles y requisitos propios. Lobito aporta vida, no niveles de Dash ágil; Sirena tiene una onda de empuje y curación, sin dependencia de Freón o Cero absoluto.

Los bonos de daño de Demonio, Reaper y Yanikua se suman entre sí antes de multiplicar el daño del ataque. El daño base de la tabla también recibe los modificadores que corresponden: la quemadura de Demonio causa 4,6 por segundo con su propio bono de +15%, antes de resistencias y otras bonificaciones. La duplicación de vida de Yanikua se aplica después de sumar la vida base, las cartas y los bonos planos de entidades.

Yanikua evita una muerte una vez por expedición, deja al jugador con 30% de su vida máxima y concede 3 s de protección. Lisa aplica ralentización al jefe, limitada por el máximo del 25% de Gólgora. El aumento de Chico Emo afecta al movimiento durante 2 s; conserva la velocidad, duración y recarga propias del dash.

Los temporizadores avanzan durante combate activo. Cambiar equipo durante una visita a bóveda aplica los poderes nuevos sin reiniciar las recargas ni recuperar la salvación usada. Al iniciar otra expedición se restablecen esas recargas y la salvación. Una copia equipada permanece reservada; el selector de fusión permite quitarla explícitamente del equipo antes de usarla como ingrediente.

![Selector de entidades del puesto](../Media/Avance_2026-11-05/Interfaz/SeleccionEntidades_Actual.png)

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

Liberar una copia del inventario muestra una confirmación con su nombre. En la bóveda, E sobre un puesto ocupado también permite liberar su entidad tras confirmar; si era un puesto de equipo, primero se retira esa asignación. T permite desequiparla sin descartarla. Coleccionar el resultado de fusión suma una copia; Liberar descarta ese resultado.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
