# Datos y persistencia

Los valores del jugador, los arquetipos y las oleadas se configuran mediante assets. ExpeditionEntrySettings enlaza las rondas demo y completa; sus assets seleccionan el tiempo del jefe y las variantes de 3600 o 9000 HP. GolgoraCombatSettings reúne tiempos, daños, áreas y modificadores de fase.

El catálogo de combate tiene 35 identificadores. El catálogo de entidades tiene quince, seis rarezas y dos fuentes separadas: nueve entradas de mapa y seis resultados de fusión. CompanionSystem comunica el equipo a CompanionPowerRuntime, que activa poderes propios y mantiene sus recargas y salvación por expedición. PlayerRuntimeStats combina los bonos de entidades con estadísticas de cartas sin convertir el equipo en niveles de mejoras.

## Sesión en memoria

El reloj, salud, XP, cartas, enemigos, peligros y objetos pertenecen a la expedición. Las transiciones entre preparación y combate restablecen ese estado. Una visita temporal a la bóveda conserva la sesión en memoria.

## Datos locales

FusionProgress serializa el inventario, equipo, exposición y proceso en JSON de formato 4. FusionStation lo guarda bajo EchoScrap.Collection.Production.v1. La validación amplía los inventarios de trece entradas del mismo formato, protege copias equipadas y restablece formatos anteriores a 4. La clave de prototipo no se carga desde este sistema.

| Campo del JSON | Contenido |
| --- | --- |
| `version` | Versión del esquema. El guardado actual escribe 4. |
| `owned` | Cantidades por ID de entidad, de 0 a 14. Los valores negativos se ajustan a cero. |
| `equipped` / `displays` | IDs de tres puestos de equipo y diez de exposición. El valor −1 indica un puesto vacío. Se retiran IDs inválidos y equipamientos que exceden las copias disponibles. |
| `phase` / `result` | Estado de la fusión e ID del resultado ya sorteado. Una fase inválida o un resultado fuera de catálogo en una fusión activa cancela ese estado. |
| `remaining` / `duration` / `opening` | Tiempo restante, duración y progreso de apertura. Una apertura interrumpida vuelve a estado listo para reabrirla. Una decisión de resultado válida permanece disponible. |

### Carga, recuperación y reinicio

- Si falta la clave, si la deserialización devuelve vacío o si el JSON lanza un error al cargar, se inicializa una colección vacía. Si el JSON se puede leer, la validación corrige campos fuera de rango. Un formato anterior a 4 o un inventario ausente restablece inventario, equipo y fusión. Un formato posterior pasa por el validador de versión 4 y se reescribe con ese número.
- El guardado escribe al cambiar inventario o equipo, iniciar o resolver fusión y pasar de estado. Mientras hay una fusión activa se escribe cada cinco segundos sin escalar, además de guardar al salir o al entrar en pausa de aplicación. El tiempo con el juego cerrado no adelanta la fusión.
- En la build Windows actual, PlayerPrefs usa el perfil del usuario. La clave del proyecto está bajo `HKEY_CURRENT_USER\Software\DefaultCompany\EchoScrap`. El mecanismo de recuperación disponible sanea campos o empieza una colección vacía según el fallo.
- **Nueva colección** en el menú, con confirmación cuando hay perfil, reemplaza inventario, equipo, exposición y fusión por un estado vacío y conserva el volumen general. Es el recorrido de reinicio disponible para el jugador.

El perfil usa EchoScrap.CollectionStarted.v1 y el volumen EchoScrap.Audio.MasterVolume. Las preferencias visuales y el modo de estadísticas se pasan entre escenas durante la aplicación en curso.

Los JSON de los atlas registran rectángulos, etiquetas y tiempos. Los GIFs documentales de animación se componen a partir de esos datos y las copias PNG se verifican contra los atlas activos.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
