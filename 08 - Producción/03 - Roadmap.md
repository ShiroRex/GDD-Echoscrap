# Roadmap

## Descripción de la sección

El *roadmap* organiza el desarrollo de *EchoScrap* en etapas secuenciales con objetivos y entregables claros a partir de la aprobación del GDD.

## Etapas

### 1. Concepto y Preproducción (Estado actual)

- **Objetivo:** Definir la visión, historia, reglas mecánicas, balance matemático, catálogo de 35 mejoras, 9 entidades coleccionables, dirección de arte y arquitectura técnica en el GDD.
- **Entregables:** Documento de Diseño de Videojuego (GDD v0.3.0) completo, coherente y aprobado por el profesor.

### 2. Prototipo Funcional (*Greybox*)

- **Objetivo:** Validar en el motor el movimiento 2.5D con cámara a 60°, el *Dash* con *i-frames*, el autoapuntado 360° de la linterna con ráfaga de 4 pulsos y el sistema de *pooling* de enemigos.
- **Entregables:** Escena base de arena ($120 \times 120\text{ m}$ con muros perimetrales), locomoción del Coleccionista y ciclo básico de combate a $60\text{ FPS}$.

### 3. Producción (*Vertical Slice* y Contenido Completo)

- **Objetivo:** Integrar los 5 arquetipos de enemigos de horda, el jefe Gólgora (minuto 8:00 con invocación de esbirros), el catálogo de 35 mejoras en 4 rarezas, la interfaz del Almacén (hasta 3 entidades equipables), la Ruleta de Extracción post-victoria y los recursos de arte 2D/3D y audio.
- **Entregables:** Bucle completo de partida (Almacén $\rightarrow$ Arena 8 min $\rightarrow$ Gólgora $\rightarrow$ Ruleta de Extracción / Resultados $\rightarrow$ Guardado JSON).

### 4. Pruebas y Pulido (QA)

- **Objetivo:** Ejecutar las comprobaciones de balance, casos límite de mejoras, pruebas de estrés ($150\text{–}250$ enemigos simultáneos), accesibilidad y corrección de errores.
- **Entregables:** Reporte de pruebas superadas, ajuste fino de curva de dificultad y estabilidad verificada.

### 5. Entrega Final

- **Objetivo:** Empaquetar la versión ejecutable de Windows PC junto con la documentación actualizada.
- **Entregables:** Ejecutable final `EchoScrap.exe` (x86_64) y repositorio oficial en GitHub.

## Hitos

| Hito | Entregable principal | Criterio de finalización | Estado |
|---|---|---|---|
| **H1 — Aprobación de GDD** | GDD v0.3.0 completo en Obsidian/GitHub. | Validación docente sin contradicciones entre áreas. | En revisión |
| **H2 — Núcleo Jugable** | Locomoción, *Dash*, Linterna 4 pulsos y *Spawner* base. | Ciclo de combate funcional y fluido a $60\text{ FPS}$. | Planificado |
| **H3 — Bucle de 8 Minutos y Jefe** | 5 arquetipos, 35 cartas de mejora y combate contra Gólgora. | Partida completa de principio a fin con Victoria y Derrota. | Planificado |
| **H4 — Meta-Progresión y Arte** | Almacén (3 ranuras), 9 Entidades, Ruleta, *sprites* Apollo y audio. | Persistencia JSON funcional e identidad visual/sonora integrada. | Planificado |
| **H5 — Cierre y Entrega** | Build final para Windows PC verificada por QA. | Cero errores bloqueantes y criterios de aceptación cumplidos. | Planificado |

> **Navegación:** [[00 - Índice]] · ← [[02 - Roles y Responsabilidades]] · [[04 - Backlog del Proyecto]] →
