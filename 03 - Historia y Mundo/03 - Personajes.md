# Personajes

El reparto incluye al Coleccionista, cinco enemigos de horda, dos élites, Gólgora y las quince entidades de la colección. Cada enemigo tiene aquí su ficha de identidad y función; sus recursos visuales se reúnen en [[02 - Personajes y Animación]] y sus reglas de combate en [[03 - Enemigos y NPC]].

[Ver la lámina de proporciones del reparto](../05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#proporciones-en-reposo).

## El Coleccionista

Es el personaje controlado por el jugador. Lleva capucha clara, máscara con pico, ropa oscura y farol. Su luz apunta de forma independiente del movimiento. Cuenta con sprites de reposo, marcha, dash y muerte en cuatro direcciones.

[Ver atlas, fuente editable y animaciones del Coleccionista](../05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#el-coleccionista).

## Residual: Esqueleto

Enemigo básico de la horda. Su apariencia es un esqueleto animado en cuatro direcciones que persigue al Coleccionista y ejerce presión por acumulación y contacto.

[Ver ficha visual y animaciones](../05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#residual-esqueleto).

## Interceptor: galgo de chatarra

Perseguidor rápido representado por un galgo de chatarra. Anticipa el movimiento del Coleccionista y obliga a variar las rutas de escape.

[Ver ficha visual y animaciones](../05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#interceptor-galgo-de-chatarra).

## Coloso: Wendigo

Enemigo pesado representado por un Wendigo. Es inmune al empuje, compromete una dirección de embestida tras un aviso en el suelo, atraviesa el espacio libre y requiere un intervalo de recuperación.

[Ver ficha visual y animaciones](../05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#coloso-wendigo).

## Mortero: Campana

Enemigo de artillería con forma de campana flotante. Mantiene distancia y lanza proyectiles de ácido que dejan charcos persistentes en el suelo.

[Ver ficha visual y animaciones](../05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#mortero-campana).

## Polilla: Reactor

Enemigo explosivo volador que se aproxima hasta situarse a `2 m` del jugador (`MothArmDistance`) para armar su espoleta de `0,9 s` (o `1,2 s` si muere por daño del jugador antes de detonar). Su explosión en `3 m` inflige `35` de daño físico al Coleccionista y a todos los objetivos enemigos cercanos (`IsEnemyTarget`, incluido Gólgora, que aplica su `35%` de reducción mientras está en `CercoActive`).

[Ver ficha visual y animaciones](../05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#polilla-reactor).

## Caballero Fantasma

Élite de ataque sobrenatural y resistencia al daño físico (`75%` de mitigación). Amenaza al Coleccionista con un barrido frontal anunciado de `120°` y presenta resistencia parcial al empuje (`susceptibilidad = 0,35`).

[Ver estado de integración visual](../05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#caballero-fantasma).

## Centinela de Cobre: gólem

Élite pesado de proyectiles físicos y resistencia al daño sobrenatural (`75%` de mitigación fuera de su ventana de vulnerabilidad). Abre sus compuertas para disparar un abanico de tres misiles y queda expuesto con el aviso «Vulnerable» durante su recuperación de `2 s`.

[Ver estado de integración visual](../05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#centinela-de-cobre-g%C3%B3lem).

## Gólgora: el MataColeccionista

Jefe final de la expedición. Su modelo 3D animado combina cuerpo pesado, arma contundente y magma. Combate dentro de la caldera central de `22 m` de radio, alterna seis patrones de ataque y acelera sus tiempos y velocidad al entrar en su segunda fase al `50%` de vida.

[Ver ficha visual y animaciones](../05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#g%C3%B3lgora-matacoleccionista).

## Entidades de la colección

El catálogo reúne quince entidades provenientes de distintos mundos y realidades que colapsaron hacia el Vertedero de Ecos: Lobito, Mago, Demonio, Shireen, Yanikua, Basket, Hada, Lisa, Reaper, Chico Emo, Conde Alucard, Murciélago Cristal, Sirena, Soldier y Joel (Demonio y Demonio2 son vistas de una misma entrada). Nueve de ellas se encuentran directamente en la arena y seis se obtienen en la máquina de fusión.

Cada entidad activa un poder propio de acompañante al ocupar uno de los tres puestos de equipo del balcón superior (`EQUIPO 1..3`) y queda registrada al descubrirse en el **Índice de Colección (`[I]`)**. Sus rarezas, poderes y recetas de fusión se detallan en [[04 - Objetos y Coleccionables]], y sus vistas y archivos editables en [Personajes y animación](../05%20-%20Arte%20y%20Audio/02%20-%20Personajes%20y%20Animaci%C3%B3n.md#entidades-de-la-colecci%C3%B3n).

### Yanikua

Supervisora de mantenimiento en una planta de agua potable de su mundo de origen, reconocible por su uniforme operativo con cintas reflectantes, botas de seguridad y las rodillas manchadas de lodo por el trabajo de campo. Durante una guardia nocturna, una fractura dimensional comenzó a tragarse las instalaciones; en lugar de huir, permaneció en la sala de válvulas cerrando manualmente las compuertas principales para dar tiempo a que su cuadrilla evacuara, convirtiéndose en la última en caer al Vertedero. Su temple bajo presión y su resistencia física refuerzan al Coleccionista e incluso le permiten sobrevivir a un golpe letal por expedición.

### Lisa

Joven viajera proveniente de una ciudad invernal que quedó congelada en el tiempo antes de fracturarse. Viste un abrigo blanco de corte ligero y boina rosa sobre su cabellera celeste, manteniendo una expresión serena incluso en medio del caos de la fosa. Acostumbrada al frío extremo de su hogar, conserva un aura gélida capaz de congelar periódicamente a los enemigos cercanos y mantener un ritmo constante de ataque.

### Sirena

Habitante de un océano abisal cuyos mares se evaporaron cuando su mundo se resquebrajó. Su cola escamosa oscura con destellos dorados y las joyas que adornan su cabello reflejan su origen en las profundidades, donde utilizaba pulsos de presión y cantos resonantes para orientarse en la penumbra. En el Vertedero emplea esa misma onda expansiva para apartar a la horda y restaurar parte de la vitalidad del jugador.

### Hada

Pequeña artesana alada de tonos azules que lleva un delantal de trabajo, un carrete de hilo con alfileres sujeto al cabello y unas tijeras doradas de costura en la mano. En su reino original se encargaba de remendar tapices encantados; tras caer a la fosa, utiliza sus hilos invisibles para tejer una barrera protectora alrededor del Coleccionista que absorbe impactos directos antes de volver a hilvanarse.

### Murciélago Cristal

Criatura nocturna nacida en cavernas subterráneas de cuarzo, cuyas alas translúcidas, orejas minerales y gema caudal refractan cualquier destello de luz. Cuando las bóvedas de su mundo se desplomaron, quedó atrapado entre la chatarra del Vertedero, donde emite vibraciones acústicas a través de sus cristales para dañar y entorpecer el avance de las criaturas cercanas.

### Shireen

Danzante y curandera de un oasis desértico, ataviada con sedas color magenta y una diadema dorada con rubí en la frente. Cuando su santuario fue absorbido por la anomalía, conservó consigo las plegarias de su pueblo para aliviar el agotamiento de los viajeros perdidos, canalizando una recuperación constante de salud durante el combate.

### Soldier

Motociclista y combatiente urbano sobreviviente de una metrópoli arrasada por una infección zombi. Porta un casco rojo con visor oscuro, rifle a la espalda, maletín de primeros auxilios en mano y una chamarra mostaza remendada y marcada en el pecho por zarpazos que no lograron derribarlo cuando defendía el último puente de evacuación de su ciudad. Curtido en resistir emboscadas cuerpo a cuerpo, aporta mayor vitalidad y reduce el daño recibido.

### Conde Alucard

Un peculiar capibara vampiro de porte aristocrático que habitaba los humedales nocturnos de un mundo gótico. Ataviado con sombrero de copa, medallón dorado y una capa negra de cuello carmesí, solía pasear con calma junto a las lagunas antes de que su realidad desapareciera. Fiel a su naturaleza vampírica pero pacífica con su portador, drena pequeñas dosis de vitalidad de los enemigos derrotados para curar al Coleccionista.

### Mago

Hechicero veterano de barba rojiza, túnica azul y sombrero puntiagudo que cayó al Vertedero aferrado a su báculo de madera coronado por un cristal arcano. Aunque la destrucción de su torre le arrebató sus libros de conjuros complejos, todavía concentra esferas de energía pura en su mano libre para disparar proyectiles mágicos que atraviesan a las filas de la horda.

### Demonio

Pequeño diablillo carmesí de mirada traviesa cuyas orejas y punta de la cola arden con fuego vivo. Proveniente de un plano volcánico menor colapsado, disfruta del caos de la arena más que cualquier otro habitante del almacén, imbuyendo los ataques del Coleccionista con su temperamento agresivo para aumentar el daño e incendiar a los enemigos.

### Joel

Joven programador y estudiante de ingeniería que fue tragado por una grieta dimensional en plena madrugada mientras depuraba código. Llegó al Vertedero tal como estaba: con su laptop llena de calcomanías bajo un brazo y un vaso de café cargado en la otra mano. Su ritmo acelerado por la cafeína y su costumbre de trabajar contra reloj incrementan la velocidad de ataque del farol.

### Chico Emo

Joven reservado de lentes, chamarra oscura y playera con estampado de calavera que solía pasar las tardes aislado entre música y callejones de su ciudad antes del colapso. Acostumbrado a escabullirse sin llamar la atención y reaccionar por reflejo ante el peligro, le permite al Coleccionista duplicar su velocidad de carrera justo después de ejecutar un *dash*.

### Lobito

Lobo antropomórfico y disciplinado practicante de artes marciales que viste un *karategi* blanco con cinturón negro, guantes de combate y una bufanda roja al cuello. Entrenaba en un dojo de montaña cuando su mundo se fragmentó; pese a estar lejos de su hogar, mantiene su guardia firme y comparte su acondicionamiento físico para incrementar la salud máxima del jugador.

### Basket

Jugador de baloncesto callejero que porta el uniforme azul número 77, su balón bajo el brazo y una llamativa máscara de lucha libre para ocultar su identidad en las canchas de su barrio. La falla dimensional lo sorprendió en pleno partido, por lo que ahora utiliza su juego de pies y cambios de ritmo para otorgar mayor velocidad de movimiento entre los obstáculos de la arena.

### Reaper

Pequeño espectro encapuchado de cráneo pálido, ojos rojos encendidos y costillas expuestas que carga una guadaña casi tan grande como él. Era un guía menor de almas en un plano espiritual extinguido; al quedar sin difuntos que escoltar, acompaña al Coleccionista desde la Bóveda afilando la contundencia de cada ataque.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
