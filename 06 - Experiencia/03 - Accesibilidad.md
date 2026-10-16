# Accesibilidad

## Descripción de la sección

La accesibilidad de *EchoScrap* buscará facilitar el uso de los controles y la comprensión clara de la información en pantalla, conservando el desafío táctico de supervivencia. Las siguientes opciones se plantean como requisitos de diseño que deberán comprobarse durante las pruebas.

---

## Opciones previstas

| Opción                                          | Aplicación en EchoScrap                                                                                              |
| :---------------------------------------------- | :------------------------------------------------------------------------------------------------------------------- |
| **Tamaño de texto**                             | Permitir ampliar textos de menús, fichas del Almacén y tarjetas de mejora sin cortar el contenido.                   |
| **Contraste de interfaz y telégrafos**          | Utilizar fondos oscuros y bordes definidos que distingan textos, barras y círculos de peligro del escenario 3D.      |
| **Información independiente del color**         | Acompañar las rarezas de entidades/cartas, peligros y estados con etiquetas de texto, formas o iconos propios.       |
| **Reasignación de controles**                   | Permitir cambiar las teclas/botones y conservar las preferencias entre sesiones, conforme al apartado de Controles.  |
| **Navegación completa por teclado y mando**     | Permitir completar todo el flujo de menús, Almacén y selección de cartas (`1, 2, 3` o cruceta) sin usar el ratón.    |
| **Reducción de movimiento de cámara**           | Permitir desactivar la sacudida de pantalla (*Screen Shake*) y animaciones decorativas de la interfaz.               |
| **Reducción de destellos**                      | Sustituir parpadeos intensos por contornos o señales estables, manteniendo reconocibles el daño y las amenazas.      |
| **Volumen independiente por canal**             | Ofrecer deslizadores separados para volumen Maestro, Música (BGM) y Efectos de Sonido (SFX).                         |
| **Indicadores visuales de eventos importantes** | Acompañar con avisos visuales en pantalla la llegada de Gólgora (minuto 8:00), la subida de nivel y la vida baja.    |
| **Lectura sin límite de tiempo**                | Mantener completamente pausada la partida durante la selección de las 3 cartas de mejora y en el Almacén.            |

---

## Consideraciones de diseño accesible

- **Autoapuntado y disparo automático:** La linterna apunta de forma autónoma al enemigo vivo más cercano dentro de su alcance (y sigue la dirección de avance cuando no hay enemigos cerca), emitiendo sus ráfagas de 4 pulsos automáticamente. Esto elimina la exigencia de puntería manual con ratón o pulsaciones repetidas de disparo, reduciendo la carga motriz a desplazarse y usar el *Dash*.
- **Claridad de amenazas en pantalla:** Los peligros deberán distinguirse nítidamente del suelo y de los efectos del protagonista. En particular, el círculo telegrafiado del pisotón de Gólgora, los charcos corrosivos del Mortero y la preparación de la explosión de la Polilla deben conservar bordes y señales reconocibles incluso al activar la reducción de destellos.
- **Descripciones directas en mejoras:** Las tarjetas de subida de nivel utilizarán explicaciones concisas acompañadas del valor numérico exacto de la mejora (por ejemplo, incremento de alcance, ángulo o daño).
- **Consulta pausada del lore:** Las fichas de las 9 entidades en el Almacén serán legibles y consultables sin límite de tiempo antes de cada incursión.

> **Navegación:** [[00 - Índice]] · ← [[02 - UI y Menús]] · [[04 - Guardado y Configuración]] →
