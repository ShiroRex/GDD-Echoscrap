# Seguridad y riesgos técnicos

El inventario de fusión se conserva de forma local. Al cargarlo se validan sus arrays, identificadores y temporizadores antes de continuar el proceso. La colección y el estado del combate se gestionan por separado.

## Comportamientos que requieren atención al revisar cambios

- Un objeto reciclado debe limpiar sus estados, objetivos y efectos antes de volver a aparecer.
- La pausa debe detener temporizadores de combate y mantener la coherencia al recuperar el foco.
- Un ataque anunciado debe conservar su área, posición y tiempo de resolución.
- La saturación de una reserva de peligros debe posponer el ataque sin sustituir zonas ya anunciadas.
- La entrada del jefe necesita espacio libre para no activar colisiones encima del jugador.
- El guardado de una fusión debe conservar el resultado ya sorteado, incluidos los reinicios de la aplicación.

Los generadores históricos del escenario se conservan en el archivo externo. El montaje actual vive en sus escenas y assets, evitando que una herramienta de una fase anterior reemplace colocaciones posteriores.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
