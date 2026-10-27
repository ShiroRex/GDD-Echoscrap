# Datos y persistencia

Los valores del jugador y de los enemigos se configuran mediante assets. Las oleadas cuentan con ajustes independientes para las escenas normal y rápida. El catálogo de mejoras contiene 35 identificadores y una tabla de ajustes de sus efectos.

## Datos de la sesión

El reloj, la semilla, vida, XP, nivel, cartas, enemigos, peligros y objetos pertenecen al combate activo. El reinicio restaura sus valores y limpia sus instancias y efectos. La visita al almacén conserva esa sesión en memoria.

## Datos que se conservan localmente

`FusionProgress` serializa inventario, plazas, fase, resultado y temporizadores en JSON dentro de PlayerPrefs. La versión del formato es 1 y su clave es `EchoScrap.FusionPrototype.v1`. El sistema valida los datos al cargar y conserva el resultado de una fusión comenzada.

Los atlas y las fuentes editables contienen los fotogramas y sus tiempos. Los materiales y recursos de efectos se referencian desde el proyecto para mantener disponibles sus texturas y shaders en la ejecución.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
