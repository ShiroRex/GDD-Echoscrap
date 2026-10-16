# Audio y Música

## Descripción de la sección

Aquí definimos cómo suena el juego. La idea es que la música marque el ritmo de la partida y que los efectos de sonido sirvan de guía clara durante el combate; a medida que avanza el cronómetro de 8 minutos la música evoluciona en intensidad, reaccionando también ante la poca salud y la aparición del jefe final (*Gólgora*).

---

## Música

- **Estilo e instrumentos:** Electrónica industrial, tensa y frenética, acorde al tono melancólico y hostil del Vertedero de Ecos.
- **Música dinámica:** Sí. En el Almacén suena un tema ambiental sombrío y pausado; en la arena la música empieza con una base rítmica constante y va sumando capas e intensidad conforme avanzan las fases de oleadas, al quedar con baja salud y al estallar el combate contra el jefe final en el minuto 8:00.

---

## Efectos de sonido (SFX)

Los efectos de sonido sirven para que el jugador sepa qué está pasando en la partida sin tener que desviar la vista hacia los bordes de la interfaz:

| Evento                             | Sonido                                    | Propósito                                                                                             |
| :--------------------------------- | :---------------------------------------- | :---------------------------------------------------------------------------------------------------- |
| **Pulso de linterna (1.º a 3.º)**  | Clic eléctrico corto o chasquido seco.    | Confirma rítmicamente que la linterna está aplicando daño dentro del cono.                            |
| **4.º pulso de linterna (Empuje)** | Descarga resonante más grave.             | Distingue auditivamente el pulso final de cada ráfaga que empuja hacia atrás a la horda.              |
| **Dash (Esquive)**                 | Impulso rápido de aire/fricción.          | Da sensación de velocidad y confirma al instante que se activó la evasión con invulnerabilidad.       |
| **Recibir daño**                   | Golpe seco y distorsionado.               | Alerta inmediata de impacto para que el jugador reaccione antes de que terminen los 0.40 s de gracia. |
| **Destruir enemigo**               | Crujido metálico / desintegración.        | Confirma que el enemigo cayó y soltó un Eco de experiencia.                                           |
| **Recoger Eco (XP) o Botiquín**    | Tono breve ascendente (cristalino/médico).| Retroalimenta la absorción magnética de recursos sin interrumpir el ritmo.                            |
| **Subida de nivel**                | Tono metálico resonante y limpio.         | Avisa que se llenó la barra de experiencia justo al pausarse el juego con el modal de 3 mejoras.      |
| **Alerta de Gólgora (Min 8:00)**   | Estruendo sísmico grave / sirena lejana.  | Anuncia el corte de oleadas normales y la llegada del Guardián a la arena.                            |
| **Botones de UI / Cartas / Ruleta**| Clic mecánico de selección y giro.        | Da respuesta táctil al navegar por el Almacén, elegir cartas o girar la Ruleta de Extracción.         |

---

## Audio espacial

¿Se utilizará audio posicional?

- No se contempla audio posicional 3D complejo para esta entrega; se prioriza una mezcla estéreo 2D limpia donde los sonidos críticos de combate se escuchen siempre con total claridad.

---

## Voz

- **Narración:** No existen diálogos hablados ni pistas de voz. Toda la historia y el trasfondo se consultan mediante los textos escritos en las fichas de los coleccionables dentro del Almacén (según [[04 - Diálogos y Narrativa]]).
- **Diálogos hablados:** No.
- **Idioma de textos en pantalla:** Español.

---

## Mezcla y Prioridad Sonora

Para que el caos de 150 a 250 enemigos en combate nunca se vuelva confuso:

1. **Prioridad máxima:** El sonido de recibir daño, las alertas de ataque de Gólgora/Polilla y el sonido del *Dash* siempre se reproducen por encima del resto de efectos. Si golpean al jugador, debe escucharse sin excepción.
2. **Pausa de subida de nivel (*Ducking*):** Al subir de nivel o abrir la pausa, el volumen de la música se atenúa a la mitad para que el jugador pueda leer y elegir su carta sin distracciones, manteniendo la continuidad musical de fondo.

> **Navegación:** [[00 - Índice]] · ← [[04 - Interfaz Visual]] · [[01 - UX y Flujo del Jugador]] →
