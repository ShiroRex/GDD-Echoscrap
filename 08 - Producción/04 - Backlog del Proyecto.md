# Backlog del Proyecto

## Descripción de la sección

El *backlog* organiza las tareas necesarias para construir *EchoScrap* una vez aprobado el GDD, priorizando aquellas que validan el bucle principal y desbloquean el trabajo del resto del equipo.

## Tareas principales por área

| ID | Tarea | Área / Responsable | Prioridad | Dependencias | Criterio de aceptación |
|---|---|---|---|---|---|
| **BG-01** | Implementar locomoción 2.5D ($8.0\text{ m/s}$), cámara cenital fija a $60^\circ$ y *Dash* ($22.0\text{ m/s}$, $0.15\text{ s}$ *i-frames*, $1.0\text{ s}$ CD). | Desarrollo (David) | Alta | Aprobación del GDD | Movimiento fluido en 8 direcciones con colisión contra muros perimetrales. |
| **BG-02** | Programar la Linterna Táctica: autoapuntado 360° ($7.0\text{ m}$, $60^\circ$), ciclo de 4 pulsos ($0.8\text{ s}$ + $0.4\text{ s}$ descanso) y retroceso en el 4.º impacto. | Desarrollo (David) | Alta | BG-01 | Ciclo exacto de $1.2\text{ s}$ y aplicación de cambios al inicio del siguiente ciclo. |
| **BG-03** | Construir el generador de oleadas de 5 fases (8 min), los 5 arquetipos de enemigos y el sistema de *Object Pooling*. | Desarrollo (David) | Alta | BG-01, BG-02 | $60\text{ FPS}$ estables con $150\text{–}250$ enemigos activos en pantalla. |
| **BG-04** | Implementar al jefe Gólgora ($1500\text{ HP}$, pisotón de magma, bombardeo de lava e invocación de esbirros cada $4.0\text{ s}$). | Desarrollo (David) | Alta | BG-03 | Aparición en el minuto 8:00, corte de escalado normal y condición de Victoria al derrotarlo. |
| **BG-05** | Desarrollar el sistema de experiencia (Ecos de $5/15/50\text{ XP}$), pausa táctica de selección de 3 cartas y catálogo de 35 mejoras. | Desarrollo (David) | Alta | BG-02, BG-03 | Subida de nivel sin pérdida de excedente y aplicación de reglas de elegibilidad y topes. |
| **BG-06** | Implementar el Almacén (3 ranuras equipables), la Ruleta de Extracción post-victoria ($+1$ nivel por duplicado) y el guardado JSON. | Desarrollo (David) | Media | BG-04, BG-05 | Persistencia verificada de las 9 Entidades y sus niveles entre sesiones. |
| **BG-07** | Producción de *sprites* $64 \times 64\text{ px}$ (Coleccionista, 5 enemigos, Gólgora y 9 Entidades) bajo la paleta Apollo. | Arte 2D (Mirian) | Alta | Aprobación del GDD | *Spritesheets* con filtrado *Point* y sin *outlines* negros puros (`#000000`). |
| **BG-08** | Modelado 3D *low-poly* de la arena ($120 \times 120\text{ m}$), muros perimetrales de chatarra y diseño de interfaz visual (HUD y menús). | Arte 3D / UI (Samuel) | Alta | Aprobación del GDD | Escenario cerrado sin fugas de colisión y mockups de HUD/Almacén legibles. |
| **BG-09** | Producción de música dinámica, efectos de sonido (SFX), fichas narrativas de las 9 Entidades y ejecución de plan de pruebas QA. | Audio / UX / QA (Luis Fernando) | Media | BG-05, BG-06 | Mezcla con prioridad para daño/*Dash*, textos de 35–65 palabras y matriz de pruebas validada. |

## Criterio de priorización

Se construyen primero los sistemas del bucle de combate en arena (`BG-01` a `BG-05`) junto con los recursos visuales base (`BG-07`, `BG-08`), seguidos de la capa de meta-progresión en el Almacén y el pulido audiovisual (`BG-06`, `BG-09`).

> **Navegación:** [[00 - Índice]] · ← [[03 - Roadmap]] · [[05 - Gestión de Cambios]] →
