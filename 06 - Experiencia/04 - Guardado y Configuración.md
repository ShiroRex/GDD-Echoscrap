# Guardado y configuración

## Datos de la estación de fusión

La colección usa un guardado local bajo la clave `EchoScrap.FusionPrototype.v1` de PlayerPrefs. El contenido serializado incluye cantidades de los cinco personajes, las ocho plazas de exposición, fase del proceso, resultado sorteado y tiempo restante.

Se guarda al iniciar una fusión, cambiar de fase, resolver el resultado, durante el proceso y al cerrar. El resultado se sortea una vez al comienzo y se conserva durante una recarga. El tiempo fuera de la aplicación no hace avanzar la fusión.

Al cargar se validan tamaños de inventario, identificadores y temporizadores. Si el contenido no puede leerse se recupera el estado inicial del sistema. Los cuatro ingredientes base conservan la disponibilidad ilimitada del montaje actual.

## Estado de la partida

La vida, XP, nivel y cartas elegidas pertenecen a la sesión. Reiniciar comienza una configuración de combate nueva. La visita al almacén conserva al mismo jugador y sus valores mientras suspende el combate.

Los ajustes de decoración, efectos reducidos, sacudida y presentación de estadísticas pertenecen a las opciones disponibles de la sesión y se conservan durante su reinicio. El guardado permanente documentado en esta página corresponde al inventario y al proceso de fusión.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
