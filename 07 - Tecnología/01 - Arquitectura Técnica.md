# Arquitectura Técnica

## Propósito y estado

Esta sección presenta la **arquitectura propuesta para desarrollar EchoScrap en Unity**, una vez aprobado el GDD. Se encuentra en fase de diseño; no constituye un reporte de componentes implementados ni de rendimiento verificado.

El inventario funcional se documenta en [[03 - Sistemas]]. Aquí se propone cómo separar sus responsabilidades y datos. Los nombres finales de clases, métodos, archivos y soluciones concretas requieren definición técnica.

## Capas propuestas

| Capa | Responsabilidad prevista | Datos o relaciones |
|---|---|---|
| **Entradas** | Traducir teclado y mando a acciones del jugador. | Entregar acciones a movimiento, dash y navegación de interfaces. |
| **Control de la partida** | Coordinar tiempo de juego, pausa, jefe y resultados. | Aplicar las transiciones descritas en Reglas y Estados. |
| **Datos de diseño** | Mantener definiciones de personajes, enemigos, ataques, cartas y parámetros. | Proporcionar las reglas y valores del catálogo. |
| **Estado de la run** | Mantener vida actual, XP, mejoras temporales, efectos y estado de enemigos. | Cambiar durante la expedición y restablecer lo que corresponda al comenzar otra. |
| **Simulación** | Resolver movimiento, autoapuntado, ataques, defensas, IA, aparición y recolección. | Usar datos de diseño y estado; comunicar los cambios relevantes. |
| **Progresión permanente** | Gestionar colección y equipamiento. | Conceder captura sólo tras victoria y preparar los efectos de la siguiente run. |
| **Presentación** | Mostrar interfaces, cámara y respuestas visuales y sonoras. | Representar el estado y los eventos de juego para el jugador. |
| **Persistencia** | Conservar los datos permanentes y la configuración definida. | Guardar y recuperar según Guardado y Configuración. |

Esta separación es una propuesta técnica para revisar dentro del GDD. No impone nombres de scripts ni un número definitivo de componentes.

## Relaciones previstas

- Las entradas comunican acciones a control y simulación.
- La simulación consulta definiciones y actualiza el estado de la run.
- Los cambios de vida, XP y resultados se comunican a presentación.
- La colección y el equipamiento preparan los efectos del Coleccionista antes de la run.
- Una victoria habilita la captura y actualización de la colección.
- Persistencia conserva los datos que determine el diseño de guardado.

Las interfaces deben mostrar el resultado de las reglas de juego. Los cálculos de daño, experiencia o recompensa no deben depender de la existencia de una pantalla concreta.

## Datos y estado

Se propone separar tres grupos:

1. **Definiciones:** balance base y catálogo de cartas, ataques, enemigos y personajes.
2. **Estado temporal:** vida, XP, efectos y mejoras de la run en curso.
3. **Datos permanentes:** personajes adquiridos, equipamiento y configuración que deban conservarse.

Las instancias deben mantener su estado particular; modificar una instancia no debe alterar de forma involuntaria el catálogo compartido o a otro enemigo. El formato, los límites y las reglas de persistencia se definirán en [[04 - Datos y Persistencia]].

## Criterios técnicos propuestos

- Separar reglas, estado y presentación para facilitar revisión y mantenimiento.
- Centralizar el balance para evitar valores distintos en dos sistemas.
- Coordinar cambios de estado sin que las pantallas tengan que calcular las reglas.
- Diseñar la simulación de hordas y efectos con un presupuesto de rendimiento medible.
- Evaluar reutilización de objetos y buffers donde corresponda al desarrollar, en lugar de presentar una técnica como ya implementada.
- Validar pausa, reinicio y persistencia como parte del plan de pruebas.

El objetivo de rendimiento y sus condiciones de medición se especificarán en [[03 - Requisitos Técnicos]] y [[02 - Pruebas]]. No se declara una garantía de 60 FPS ni cero asignaciones antes de desarrollar y verificar esas condiciones.

## Definiciones pendientes

- Responsabilidades exactas de los componentes y su comunicación.
- Formato de definiciones, estado temporal y guardado.
- Orden de resolución de daño y resultados simultáneos.
- Presupuesto de simulación, renderizado, memoria y efectos.
- Técnicas concretas de reutilización y optimización.
- Procedimientos de prueba y criterios de cumplimiento.

Estas decisiones deben mantenerse coherentes con el diseño funcional y se concretarán en el trabajo técnico del equipo.

> **Navegación:** [[00 - Índice]] · ← [[04 - Guardado y Configuración]] · [[02 - Motor y Herramientas]] →
