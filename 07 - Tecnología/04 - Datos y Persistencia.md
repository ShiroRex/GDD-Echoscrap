# Datos y persistencia

Los valores del jugador, los arquetipos y las oleadas se configuran mediante assets. ExpeditionEntrySettings enlaza las rondas demo y completa; sus assets seleccionan el tiempo del jefe y las variantes de 3600 o 9000 HP. GolgoraCombatSettings reúne tiempos, daños, áreas y modificadores de fase.

El catálogo de combate tiene 35 identificadores. El catálogo de entidades tiene quince, seis rarezas y dos fuentes separadas: nueve entradas de mapa y seis resultados de fusión. CompanionSystem comunica el equipo a CompanionPowerRuntime, que activa poderes propios y mantiene sus recargas y salvación por expedición. PlayerRuntimeStats combina los bonos de entidades con estadísticas de cartas sin convertir el equipo en niveles de mejoras.

## Sesión en memoria

El reloj, salud, XP, cartas, enemigos, peligros y objetos pertenecen a la expedición. Las transiciones entre preparación y combate restablecen ese estado. Una visita temporal a la bóveda conserva la sesión en memoria.

## Datos locales

FusionProgress serializa el inventario, equipo, exposición y proceso en JSON de formato 4. FusionStation lo guarda bajo EchoScrap.Collection.Production.v1. La validación amplía los inventarios de trece entradas del mismo formato, protege copias equipadas y restablece formatos anteriores a 4. La clave de prototipo no se carga desde este sistema.

El perfil usa EchoScrap.CollectionStarted.v1 y el volumen EchoScrap.Audio.MasterVolume. Las preferencias visuales y el modo de estadísticas se pasan entre escenas durante la aplicación en curso.

Los JSON de los atlas registran rectángulos, etiquetas y tiempos. Los GIFs documentales de animación se componen a partir de esos datos y las copias PNG se verifican contra los atlas activos.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
