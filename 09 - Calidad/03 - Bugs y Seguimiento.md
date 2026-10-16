# Bugs y Seguimiento

## Descripción de la sección

Establece la clasificación de severidad y el formato que utilizará el equipo de *EchoScrap* para registrar, priorizar y dar seguimiento a los errores detectados durante las fases de prototipo y pruebas.

## Clasificación de severidad

| Severidad | Definición en *EchoScrap* | Tiempo de atención |
|---|---|---|
| **Crítica (Bloqueante)** | Cierre inesperado del juego (*crash*), corrupción del archivo de guardado JSON, imposibilidad de avanzar tras subir de nivel o fallo que impida activar la Victoria al derrotar a Gólgora. | Inmediata (bloquea cualquier entrega). |
| **Alta** | Desviación en las reglas de combate o meta-progresión (por ejemplo, que un enemigo atraviese los muros perimetrales, que la Ruleta de Extracción no sume $+1$ nivel en duplicados o caídas por debajo de $45\text{ FPS}$). | Prioritaria dentro de la misma iteración. |
| **Media** | Errores visuales o sonoros que no detienen la partida (desfase en una animación de *sprite*, icono de carta desalineado o sonido secundario que no se atenúa en pausa). | Antes del cierre de versión. |
| **Baja** | Detalles estéticos menores o ajustes de texto en las fichas del Almacén. | Durante la etapa de pulido final. |

## Plantilla de reporte de incidencia

- **ID y Título:** Identificador único y resumen breve del fallo.
- **Versión y Escena:** Versión de la build y lugar donde ocurre (Almacén, Arena Fase 1–4, Fase 5 Gólgora o Ruleta de Extracción).
- **Pasos para reproducir:** Secuencia numerada de acciones para replicar el comportamiento.
- **Resultado esperado vs. Resultado actual:** Comparación directa contra la regla documentada en el GDD.
- **Estado:** *Abierto* $\rightarrow$ *En progreso* $\rightarrow$ *En verificación QA* $\rightarrow$ *Cerrado*.

## Estado actual del registro

Actualmente el proyecto se encuentra en **fase de revisión y aprobación del GDD** previa al inicio de la construcción oficial; esta tabla se poblará a partir del Hito 2 (*Núcleo Jugable*).

| ID | Incidencia | Severidad | Responsable | Estado |
|---|---|---|---|---|
| — | *Sin incidencias activas (fase documental de GDD).* | — | Equipo EchoScrap | Al día |

> **Navegación:** [[00 - Índice]] · ← [[02 - Pruebas]] · [[04 - Criterios de Aceptación]] →
