# Guardado y configuración

## Colección persistente

La estación guarda JSON en PlayerPrefs bajo EchoScrap.Collection.Production.v1. El formato interno actual es la versión 4 e incluye las cantidades de quince entidades, los tres puestos equipados, los datos de exposición, la fase de fusión, el resultado sorteado y sus temporizadores.

Se guarda al recoger una entidad, cambiar el equipo, liberar copias, iniciar o resolver una fusión, durante el proceso y al cerrar. El resultado se decide una vez al comenzar y las cuatro copias se consumen en ese momento.

El tiempo fuera de la aplicación no adelanta la fusión. Al terminar queda lista para una apertura manual. Si se cierra durante la apertura, la carga la devuelve al estado listo para volver a interactuar.

La validación ajusta inventarios, identificadores, puestos y tiempos. Las colecciones de formato 4 que tenían trece entidades amplían su array a quince y conservan sus copias. Los formatos internos anteriores a 4 se restablecen. La antigua clave EchoScrap.FusionPrototype.v1 no se consulta desde el cargador de producción.

## Perfil y opciones

EchoScrap.CollectionStarted.v1 identifica que se ha iniciado una colección. Nueva colección confirma antes de restablecer inventario, equipo y fusión cuando existe progreso.

El volumen general se guarda bajo EchoScrap.Audio.MasterVolume. Los ajustes de decoración, efectos reducidos, sacudida y modo de estadísticas se mantienen entre los reinicios y transiciones de la aplicación en curso; no tienen en este montaje un guardado permanente equivalente al del volumen.

## Estado de la expedición

La salud, XP, nivel y cartas pertenecen al intento activo. Volver desde resultados o abandonar limpia el combate y devuelve a la preparación en bóveda. Las entidades equipadas vuelven a aplicar sus poderes al empezar otro intento.

La visita a la bóveda durante una expedición conserva esa sesión en memoria y suspende el combate. Cerrar la aplicación no guarda una partida de combate para retomarla en el mismo punto.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
