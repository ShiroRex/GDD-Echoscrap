# UI y Menús

## Descripción de la sección

La interfaz de *EchoScrap* permitirá consultar información y tomar decisiones sin perder de vista el combate. Todos los menús deberán poder utilizarse íntegramente con teclado (`WASD` / Flechas / `1, 2, 3`) y ofrecer navegación con mando (Gamepad), sin depender del ratón, de acuerdo con los controles definidos en el GDD.

---

## Mapa de navegación

| Pantalla de origen     | Opciones y destinos                                                                                             |
| :--------------------- | :-------------------------------------------------------------------------------------------------------------- |
| **Menú principal**     | Nueva colección, Continuar, Configuración y Salir.                                                              |
| **Almacén (Hub)**      | Consultar fichas de las 9 entidades, ver su nivel, equipar hasta 3 entidades en las ranuras e iniciar partida.  |
| **Arena**              | HUD de combate, modal de selección de mejoras (al subir de nivel) y menú de pausa.                              |
| **Pausa**              | Reanudar, consultar controles, Configuración y abandonar expedición.                                            |
| **Resultados**         | Consultar desempeño de la partida; regresar al Almacén (en derrota) o avanzar a la Ruleta (en victoria).        |
| **Ruleta de Extracción**| Exclusiva tras vencer a Gólgora: obtener y revisar la entidad capturada (+1 nivel si es duplicada) e ir al Almacén.|
| **Configuración**      | Audio, Video, Controles y Accesibilidad.                                                                        |

---

## Pantallas

### Menú principal

- **Propósito:** Permitir el acceso al juego y a sus opciones generales.
- **Elementos:** Título del juego y botones principales (*Continuar*, *Nueva colección*, *Configuración*, *Salir*).
- **Acciones disponibles:** Crear una colección desde cero, continuar el progreso guardado, abrir configuración o cerrar la aplicación.
- **Estados:** *“Continuar”* estará deshabilitado cuando no exista un archivo de guardado previo.
- **Transiciones:** *Nueva colección* y *Continuar* llevarán directamente al Almacén.

### Almacén del Coleccionista (Hub)

- **Propósito:** Mostrar el progreso permanente de la colección y preparar la configuración pasiva para la siguiente partida.
- **Elementos:** Vitrina de los **9 personajes coleccionables**, rareza, nivel actual de cada entidad, ficha de registro narrativo, descripción de su beneficio pasivo y las **3 ranuras de equipamiento activo**.
- **Acciones disponibles:** Consultar fichas de lore, asignar o cambiar las hasta **3 entidades equipadas** e iniciar la expedición hacia la arena.
- **Estados:** Personajes bloqueados (silueta), obtenidos (con su nivel actual) y equipados (asignados en una de las 3 ranuras).
- **Transiciones:** Acceso a la arena de combate, configuración y menú principal.

### HUD de combate

- **Propósito:** Mostrar en tiempo real la información indispensable para sobrevivir sin tapar la acción.
- **Elementos:** Salud actual y máxima, barra de experiencia (Ecos), nivel actual, cronómetro de partida (0:00 a 8:00+) e indicador de disponibilidad del *Dash*. Durante el enfrentamiento final (minuto 8:00+) se añade la barra de vida de *Gólgora (El Guardián)*.
- **Acciones disponibles:** Consultar el estado de un vistazo sin interrumpir el movimiento.
- **Estados:** Vida normal o crítica; dash listo o en recarga (1.0 s base).
- **Transiciones:** Menú de pausa, modal de selección de mejoras o pantalla de resultados.

La salud y las amenazas del terreno tendrán prioridad visual. Los indicadores del HUD se ubicarán en los bordes de la pantalla para mantener completamente despejada la zona central alrededor del Coleccionista.

### Selección de mejoras (Subida de nivel)

- **Propósito:** Elegir cómo fortalecer la *build* del personaje durante la partida.
- **Elementos:** Tres tarjetas de mejora elegidas de manera aleatoria ponderada del catálogo de 35 cartas, mostrando su nombre, rareza, nivel actual y efecto numérico, además del botón de *Re-roll* (si se cuenta con la mejora correspondiente).
- **Acciones disponibles:** Seleccionar 1 de las 3 mejoras mediante teclado (`1, 2, 3` o flechas + confirmar), mando o clic.
- **Estados:** Tarjeta enfocada y tarjeta seleccionada.
- **Transiciones:** Regreso inmediato al combate (aplicando los cambios de linterna al siguiente ciclo de 4 pulsos). Si existen varias subidas de nivel acumuladas en cola, se muestra la siguiente elección antes de reanudar el tiempo.

### Pausa

- **Propósito:** Permitir un descanso o ajustar opciones sin riesgo de recibir daño.
- **Elementos:** *Reanudar*, *Controles*, *Configuración* y *Abandonar expedición*.
- **Acciones disponibles:** Continuar la partida, revisar el esquema de controles, cambiar ajustes o retirarse al Almacén.
- **Estados:** Simulación de combate completamente congelada mientras el menú permanezca abierto.
- **Transiciones:** Regreso a la arena o retorno al Almacén tras confirmar el abandono.

### Resultados y Ruleta de Extracción

- **Propósito:** Mostrar el resumen de la incursión y, en caso de victoria, otorgar la recompensa de colección.
- **Elementos:** Estado final (*Victoria* o *Derrota*), tiempo sobrevivido, enemigos eliminados y nivel alcanzado. Si el jugador derrotó a Gólgora, se despliega la **Ruleta de Extracción**, mostrando el nombre, sprite, rareza y beneficio del coleccionable obtenido (o indicando que subió **+1 nivel** por ser duplicado).
- **Acciones disponibles:** Consultar las estadísticas, girar la Ruleta (en Victoria) y regresar al Almacén.
- **Estados:** Resumen en pantalla, giro de ruleta en curso, entidad revelada y progreso guardado automáticamente.
- **Transiciones:** Regreso al Almacén después de registrar el resultado en el guardado local.

### Configuración

- **Propósito:** Adaptar el sonido, la imagen y los controles a las preferencias del jugador.
- **Elementos:** Categorías de *Audio*, *Video*, *Controles* y *Accesibilidad* con sus valores actuales.
- **Acciones disponibles:** Modificar valores, aplicar cambios o restaurar los ajustes predeterminados.
- **Estados:** Ajustes vigentes y cambios pendientes de aplicar.
- **Transiciones:** Regreso al menú de origen. Si se abrió desde la pausa en la arena, el combate seguirá detenido hasta elegir *Reanudar*.

---

## Confirmaciones

Se solicitará una ventana de confirmación explícita únicamente en acciones destructivas: al **abandonar una expedición en curso** (advirtiendo que se perderán las mejoras temporales de esa run y no habrá captura) o al **borrar el progreso guardado / iniciar una nueva colección desde cero** (advirtiendo que se reiniciarán las entidades desbloqueadas y sus niveles).

Las acciones habituales —como abrir una ficha, cambiar las 3 entidades equipadas en el Almacén, elegir una carta o reanudar el combate— no requerirán confirmación adicional. El elemento enfocado siempre será evidente mediante borde resaltado y contraste.

> **Navegación:** [[00 - Índice]] · ← [[01 - UX y Flujo del Jugador]] · [[03 - Accesibilidad]] →
