# Datos y Persistencia

## Descripción de la sección

En este apartado se describe cómo se planea estructurar, gestionar y almacenar la información en *EchoScrap*. Como estamos definiendo la arquitectura técnica previa al desarrollo, aquí establecemos qué datos son estáticos (parámetros de balance y diseño), cuáles son volátiles (el estado de una partida en curso) y cuáles deben conservarse de manera permanente en el disco del usuario.

---

## Categorías de Datos en el Juego

Para mantener un sistema ordenado y eficiente, la información se dividirá en tres niveles:

### 1. Datos estáticos de diseño (De solo lectura)
Son los valores fijos que define el equipo y que no cambian durante la partida:
* **Catálogo de cartas:** Identificador único, nombre, rareza, descripción, requisitos previos e índices de modificación de estadísticas (según lo planificado en [[05 - Progresión]]).
* **Fichas de enemigos:** Salud base, velocidad de desplazamiento, daño de contacto, cadencia de ataque y puntos de experiencia otorgados (según [[03 - Enemigos y NPC]]).
* **Parámetros de oleadas:** Tiempos de inicio y fin de cada una de las 5 fases, tipos de enemigos permitidos por fase e intervalos de aparición.

### 2. Datos volátiles de la partida (En memoria durante la run)
Información que solo existe mientras el jugador se encuentra dentro de la arena de 120 × 120 m:
* Salud actual del Coleccionista y temporizador de supervivencia (de 0:00 a 8:00+).
* Lista de cartas acumuladas en la partida actual y cálculo dinámico de estadísticas (daño aumentado, velocidad, reducción de enfriamiento).
* Posición y vida de los enemigos activos en la arena.
* Gemas de experiencia y botiquines en el suelo pendientes de recolección.
* *Nota:* Estos datos se descartan al terminar la expedición (por victoria o por derrota).

### 3. Datos persistentes (Guardados en disco)
Información que se conserva de forma permanente entre sesiones de juego:
* **Progreso del Almacén:** Catálogo de entidades interdimensionales capturadas, el **nivel actual de cada entidad** (incrementado mediante duplicados en la Ruleta de Extracción) y las **3 ranuras de entidades equipadas** para la siguiente partida.
* **Historial y estadísticas:** Conteo de expediciones jugadas, victorias conseguidas, récord de tiempo sobrevivido y total de enemigos eliminados.
* **Configuración del usuario:** Volúmenes de audio (Maestro, SFX, Música), resolución de pantalla, modo ventana/pantalla completa y mapeo de controles.

---

## Modelo Lógico de Datos para Guardado

Se contempla estructurar el archivo de guardado en un esquema claro y modular:

```text
Estructura del Guardado (SaveData)
├── version: 1 (Número de control de versión)
├── almacen:
│   ├── entidades_desbloqueadas: lista de { id_entidad: string, nivel: int }
│   └── ranuras_equipadas: [ slot_1: string, slot_2: string, slot_3: string ]
├── estadisticas:
│   ├── partidas_jugadas: int
│   ├── victorias: int
│   ├── tiempo_record_segundos: float
│   └── enemigos_destruidos: int
└── configuracion:
    ├── audio: { maestro: 1.0, sfx: 0.8, musica: 0.7 }
    ├── video: { resolucion_ancho: 1920, resolucion_alto: 1080, pantalla_completa: true, vsync: true }
    └── accesibilidad: { screen_shake: true, alto_contraste: false }
```

---

## Formato y Almacenamiento Físico

* **Formato de guardado:** Se planea utilizar archivos de texto en formato **JSON**. Elegimos este formato porque es un estándar ampliamente soportado, ligero, legible a simple vista por los integrantes del equipo para depuración y muy fácil de serializar y deserializar desde el motor de juego.
* **Ubicación del archivo:** Se almacenará en la carpeta local de datos de usuario de Windows (mediante la ruta estándar persistente del sistema, como `AppData/LocalLow/Proyecto/EchoScrap`), con los nombres `save_data.json` y `settings.json`.
* **Versionado de datos:** El archivo JSON incluirá siempre un campo numérico entero (`version: 1`). Esto permitirá que el juego reconozca si el archivo corresponde a la versión actual o a una entrega previa.

---

## Migraciones y Compatibilidad entre Versiones

Como el proyecto avanzará a lo largo del curso académico, es común que se agreguen nuevos parámetros a las cartas o al Almacén. Para evitar que el juego falle al cargar un archivo antiguo:

1. **Lectura con valores predeterminados:** Si al leer el archivo JSON falta algún campo recién añadido, el sistema de carga le asignará automáticamente un valor por defecto seguro en lugar de arrojar un error nulo.
2. **Respaldo preventivo:** Antes de sobreescribir el archivo de guardado con una nueva versión, se planea generar una copia temporal de respaldo (`save_data.bak`).
3. **Restauración en caso de corrupción:** Si el archivo se encuentra incompleto o no se puede interpretar, el juego informará al usuario y creará un archivo nuevo con el progreso inicial sin cerrarse de golpe.

> **Navegación:** [[00 - Índice]] · ← [[03 - Requisitos Técnicos]] · [[05 - Seguridad y Riesgos Técnicos]] →
