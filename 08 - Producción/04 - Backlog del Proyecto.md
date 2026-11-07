# Entregas y trabajo pendiente

| Área | Estado al 5 de noviembre |
| --- | --- |
| Flujo | Menú, colección, bóveda, elección de ronda, expedición y regreso desde resultados integrados. |
| Equipo | Tres puestos con poderes propios, independientes de cartas y sin acumulación por identidad duplicada. |
| Colección y fusión | Quince entidades, recogida manual, inventario por copias y fusión de cuatro ingredientes. |
| Combate | Cinco arquetipos de horda, dos élites, linterna y 35 mejoras. |
| Gólgora | Caldera, seis patrones, dos fases, dos valores de vida y refuerzos anunciados. |
| Presentación | Sprites de horda y ambos élites, VFX, minimapa, retratos, menús, audio e iluminación integrados. |
| Documentación | Reglas actualizadas, spritesheets y GIFs diferenciados por su procedencia. |

## Integraciones cerradas en esta revisión

| ID | Tarea | Estado | Comprobación |
| --- | --- | --- | --- |
| ART-ELITE-01 | Conectar Caballero Fantasma: marcha, preparación, ataque, recuperación, impacto y muerte. | Integrado | Atlas de 224 fotogramas, animación ligada al barrido y muerte independiente del reciclado. |
| ART-ELITE-02 | Conectar Centinela: apertura, preparación, disparo, vulnerabilidad y cierre. | Integrado | Atlas de 288 fotogramas, puertas abiertas antes del disparo y ventana de recuperación conservada. |
| COMBAT-SOLAR-01 | Aplicar empuje en todos los pulsos de Rayo solar. | Integrado | Ocho pulsos continuos; el modo base conserva empuje solo en el cuarto. |
| DOC-REG-01 | Asignar Registro visual a David Alejandro Sierra Sosa. | Documentado | Redactor y revisor asignados; total de 48 apartados. |

## Pendientes identificados

| ID | Tarea | Prioridad propuesta | Responsable según el reparto | Dependencia | Criterio de aceptación |
| --- | --- | --- | --- | --- | --- |
| UX-01 | Guiar la preparación de entidades. | Alta | Luis Fernando Nuñez Chan, con apoyo de David Alejandro Sierra Sosa | Flujo de primera colección y selector de puestos. | Antes de elegir ronda, una persona nueva identifica los tres puestos, entiende los poderes y sabe que puede salir con puestos vacíos. |
| INPUT-01 | Cerrar el alcance de mando en colección, equipo y fusión. | Media, según alcance | David Alejandro Sierra Sosa y coordinación de producción | Decisión de qué acciones cubre la entrega. | El GDD distingue las interacciones cubiertas. QA verifica abrir, seleccionar, cancelar y confirmar en el recorrido acordado. |
| TECH-01 | Validar requisitos de la build Windows x64. | Alta | David Alejandro Sierra Sosa, con revisión de Luis Fernando Nuñez Chan | Build candidata identificada, licencia de editor y equipos de prueba. | Resolución, FPS, p95 de fotograma, memoria y tamaño instalado registrados por perfil, con el commit y los ajustes empleados. |

**Cerrar el uso de las pistas de prueba.** La música de arena y jefe está integrada con nombres provisionales; su elección final corresponde al área de audio.

Estos puntos son necesidades de revisión, no funcionalidades que esta actualización del GDD haya añadido al juego.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
