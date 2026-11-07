# Requisitos técnicos

La plataforma del proyecto es Windows PC. El montaje usa Unity 6000.6.3f1, URP 17.6.0, Input System 1.20.0 y Cinemachine 6.6.0. La compilación principal habilita MainMenu y CombatArena.

El director limita normalmente la población a 150 enemigos; el modo de estrés usa 250. Los Morteros tienen un límite de seis, los élites uno conjunto de dos y los refuerzos de la caldera uno efectivo de ocho.

## Objetivos para la build de entrega

Estos perfiles fijan condiciones de prueba propuestas. La compatibilidad de cada perfil se publica después de medir la build candidata en el equipo correspondiente.

| Parámetro | Perfil mínimo a validar | Perfil recomendado a validar |
| --- | --- | --- |
| Sistema | Windows 10 de 64 bits | Windows 11 de 64 bits |
| CPU y GPU | Modelo mínimo por medir en un equipo de entrada | Equipo de referencia Ryzen 7 5700X y Radeon RX 6650 XT |
| Memoria de prueba | 8 GB de RAM y 4 GB de VRAM | 16 GB de RAM y 8 GB de VRAM |
| Resolución y objetivo | 1280 × 720, 16:9, 30 FPS | 1920 × 1080, 16:9, 60 FPS |
| Presupuesto de fotograma | p95 de 33,3 ms o menos | p95 de 16,7 ms o menos |
| Espacio libre previsto | 2 GB para instalación y margen | 2 GB para instalación y margen |

La build Windows x64 de desarrollo del 5 de noviembre compiló con 0 errores y 34 advertencias. El informe de Unity indica 1 010 519 179 bytes, unos 0,94 GiB. Este dato sirve para estimar el espacio de instalación. La galería del 6 de noviembre cubre la lectura de la interfaz a 1080p. El registro del Profiler del 3 de noviembre contiene 2 000 fotogramas del Editor, con p95 de CPU de 6,61 ms y sin tiempos de GPU válidos. Cada registro tiene condiciones distintas de la build de entrega.

La medición en Windows x64 debe registrar commit, sistema, CPU, GPU, RAM, VRAM, resolución y configuración. Medir tras el calentamiento en una oleada avanzada y en la caldera, registrando población, FPS medios, p95 de tiempo de fotograma, memoria máxima y tamaño instalado. Verificar también el flujo a la resolución del perfil y conservar los resultados por separado.

El 7 de noviembre se intentó compilar `75bb79c` en una copia externa. Unity terminó con código 198 por falta de licencia válida para el editor en modo por lotes. Los FPS, los requisitos de hardware y la compatibilidad con Windows 10 quedan pendientes de validar en la build candidata.

## Alcance de los registros

Los GIFs nuevos de animación proceden de los atlas y tiempos exportados del proyecto. Las capturas de Unity se ejecutan sobre una copia externa, con cámara de detalle, jugador protegido y oleadas detenidas. Sus condiciones se describen en [[06 - Registro visual]].

Las imágenes y GIFs comprueban recursos y presentación. No establecen una tasa mínima de FPS ni certifican requisitos mínimos de hardware. Las mediciones de builds anteriores deben conservar su fecha, equipo y configuración para poder compararse.

El combate admite teclado y mando. Las interacciones de entidades, equipo y fusión usan E y necesitan una revisión adicional para completar su recorrido con mando.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
