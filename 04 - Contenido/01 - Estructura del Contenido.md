# Estructura del Contenido

## Descripción de la sección

Este apartado organiza todo el contenido jugable que se tiene planificado para *EchoScrap*. Como el proyecto se encuentra en su fase de diseño y validación académica previa al desarrollo oficial, aquí establecemos el alcance exacto de lo que se va a construir, asegurando que el equipo tenga una guía clara de los elementos que conformarán la entrega.

## Estructura planificada del juego

El flujo de contenido se organiza alrededor de una expedición central de supervivencia tipo roguelite, conectada con el espacio del Almacén para la gestión posterior a la partida:

```text
EchoScrap
├── Hub Central: El Almacén
│   ├── Bestiario y Vitrina de Entidades (Colección de supervivientes interdimensionales)
│   ├── Configuración de Equipamiento (Hasta 3 entidades activas para la siguiente partida)
│   └── Terminal de Despliegue a la Expedición
└── Expedición de Supervivencia: Vertedero de Ecos (Arena 120 × 120 m)
    ├── Oleadas de Supervivencia (Minutos 0:00 a 8:00)
    │   ├── Fase 1: Calentamiento (0:00 a 1:00) — Enemigos individuales, 10-20 en pantalla
    │   ├── Fase 2: Presión Media (1:00 a 2:00) — Parejas de enemigos, 30-50 en pantalla
    │   ├── Fase 3: Cerco en Racimo (2:00 a 3:00) — Tríos de choque, 60-90 en pantalla
    │   └── Fase 4: Escalada Intensa (3:00 a 8:00) — Progresión minuto a minuto, hasta 150-250 enemigos
    └── Fase 5: El Juicio (Minuto 8:00 en adelante)
        ├── Aparición de Gólgora (Titán de Magma) + esbirros invocados por el jefe
        ├── Caída del Coleccionista → Derrota y retorno al Almacén sin captura de entidad
        └── Derrota de Gólgora → Victoria y Ruleta de Extracción (Captura de entidad o +1 nivel si es duplicada)
```

## Cantidad estimada para el alcance

La siguiente tabla resume las cantidades previstas para la entrega del proyecto:

| Tipo de Contenido | Cantidad Planificada | Notas de Alcance y Responsabilidad |
|---|:---:|---|
| **Mundos / Biomas** | 1 | Vertedero de Ecos (zona industrial en ruinas y chatarra). |
| **Niveles / Arenas** | 1 | Arena plana de 120 × 120 metros, delimitada físicamente por muros perimetrales de chatarra. |
| **Hub / Base Central** | 1 | El Almacén (espacio donde se gestiona la colección y se equipan hasta 3 entidades por partida). |
| **Personajes Jugables** | 1 | El Coleccionista, equipado con la linterna de pulsos y dash direccional. |
| **Bestiario de Entidades** | 14 proyectadas (8 a 10 en entrega) | Entidades interdimensionales capturadas mediante la ruleta al vencer a Gólgora (3 equipables por partida; los duplicados suman +1 nivel a la entidad). Visión completa de 14 entidades (1 Mítico, 2 Legendarios, 2 Épicos, 2 Raros y 7 Comunes), con recorte viable para la entrega a un núcleo de 8 a 10 entidades cuya lista definitiva define el área de personajes. |
| **Enemigos de Horda** | 5 | Diseñados en [[03 - Enemigos y NPC]]: Básico, Interceptor, Coloso, Mortero y Polilla. |
| **Jefes** | 1 | Gólgora (Titán de Magma), quien aparece al cumplirse el minuto 8 e invoca enemigos como esbirros. |
| **Armas Secundarias** | Integradas en mejoras | Efectos y proyectiles auxiliares automáticos (como sierras orbitales o novas) activados mediante cartas. |
| **NPCs** | 0 | No aplica. La experiencia en arena es en solitario y el Almacén se gestiona directamente por menús de interfaz sin personajes interactivos. |
| **Misiones Principales** | 1 | Sobrevivir los 8 minutos de asedio y derrotar al jefe final. |
| **Misiones Secundarias** | 0 | No aplican para el alcance de este proyecto; se prioriza pulir el bucle central. |
| **Cartas de Mejora** | 35 | Catálogo de 35 mejoras individuales organizadas en 4 niveles de rareza ([[05 - Progresión]]). |
| **Objetos en Arena** | 2 tipos base | Gemas de experiencia y botiquines médicos (definidos en [[04 - Objetos y Coleccionables]]). |

> **Mecánica de recompensa climática:** Al derrotar a Gólgora se activa la **Ruleta de Extracción**, el sorteo donde el Coleccionista captura una entidad aleatoria para sumarla a la vitrina de su Almacén (o subirla +1 nivel si ya la poseía) y equiparla en una de sus 3 ranuras para futuras runs.

## Dependencias de desarrollo

Para poder construir este contenido de manera ordenada una vez aprobada la fase de diseño, se contempla el siguiente orden de implementación:

1. **Base de movimiento y nivel:** Se debe montar primero la arena de 120 × 120 m con sus límites perimetrales para asegurar que el jugador se mueva y use el dash correctamente.
2. **Entidades básicas:** Se implementará el enemigo básico para validar el radio de ataque de la linterna y la recolección de gemas.
3. **Sistema de oleadas:** Con el enemigo básico probado, se estructurará el generador por tiempo para activar progresivamente las 5 fases hasta el minuto 8.
4. **Variedad de enemigos:** Se irán integrando los 4 arquetipos restantes (interceptores, morteros, colosos y polillas) para sumar variedad de comportamiento.
5. **Jefe final (Gólgora):** Se integrará el encuentro del minuto 8 con sus patrones de ataque (pisotón y bombardeo).
6. **Pool de cartas:** En paralelo a las oleadas, se implementará el sistema de subida de nivel para poder elegir entre las cartas planificadas.

> **Navegación:** [[00 - Índice]] · ← [[04 - Diálogos y Narrativa]] · [[02 - Niveles]] →
