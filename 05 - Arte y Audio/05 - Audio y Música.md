# Audio y música

El juego ya tiene música y efectos integrados. El área sonora acompaña el lugar, los ataques y las acciones del Coleccionista.

| Contexto | Audio presente |
| --- | --- |
| Arena | Pista de combate en bucle. El recurso conserva el nombre de prueba Arena_combat_Test. |
| Gólgora | Pista de jefe con transición desde la arena y reducción de mezcla durante el rugido. El recurso sigue identificado como prueba. |
| Bóveda | Pista propia que se detiene al salir hacia combate o menú. |
| Linterna | Inicio de ráfaga, tres variantes de pulso, cuarto pulso, descanso, impacto normal y crítico, activación y bucle solar. |
| Experiencia | Sonido de recogida con variación de tono. |
| Transiciones | Entrada del Coleccionista, llegada por portal y rugido al elegir ronda. |
| Fusión | Sonido de apertura de la cámara al revelar el resultado. |
| Jefe | Rugido, pasos, ataques e impactos mediante sus componentes de audio. |

La música de arena y jefe continúa durante cartas y pausa manual. La pérdida de foco pausa su reproducción. La bóveda tiene su propia mezcla y las transiciones evitan que siga sonando fuera de su contexto.

El menú de pausa incorpora volumen general, guardado localmente bajo EchoScrap.Audio.MasterVolume. El control actúa sobre el volumen global de escucha.

## Procedencia documentada

La mezcla de la linterna utiliza grabaciones de microondas de pinto0lucas y contador Geiger de cookies+policy. El archivo Sources.txt del proyecto registra sus enlaces, la licencia CC0 indicada para ambas y las transformaciones realizadas: conversión a mono, selección de clics, capas, envolventes y tonos sintetizados. Los WAV entregados son mono, 44,1 kHz y 16 bits.

La integración de una pista no implica que sea una composición final. Los nombres de prueba de arena y jefe se mantienen identificados como tales. La autoría y los recursos de cada incorporación deben conservarse en su registro de fuentes.

Los GIFs de este GDD no contienen audio. Sirven para revisar animación y VFX; la escucha se comprueba en el juego.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
