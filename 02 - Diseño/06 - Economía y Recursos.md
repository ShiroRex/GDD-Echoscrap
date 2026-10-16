# Economía y Recursos

## Descripción de la sección

Documenta los recursos cuantificables del juego, sus fuentes de obtención, mecanismos de gasto o pérdida, y las reglas de balance que rigen la economía en partida y a largo plazo.

---

## Recursos en partida (*In-Run*)

| Recurso                                         | Obtención (Fuente)                                                                     | Uso (Sumidero)                                                                                                      | Límite / Tope                                                 | Pérdida                                                           |
| ----------------------------------------------- | -------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------- | ----------------------------------------------------------------- |
| **Salud del Coleccionista**                     | Valor base inicial de 100 HP (modificable por entidades y cartas).                     | Mitiga impactos de contacto o proyectiles enemigos (10 pts base por impacto).                                       | 100 pts base (ampliable mediante mejoras).                    | Si llega a 0, termina la partida (Derrota).                       |
| **Botiquines de Reparación**                    | Botín de combate (4% en básicos / 25% en pesados).                                     | Recupera de inmediato +15 HP (y carga sobrecuración con `BIO_04`).                                                  | Tope de vida máxima (salvo con `BIO_04`).                     | Se pierden si no se recogen en la run.                            |
| **Gemas de Chatarra XP**                        | Soltadas por cada enemigo eliminado (10 a 50 XP).                                      | Se absorben por magnetismo para llenar la barra de progreso de nivel.                                               | Sin límite en arena (desaparecen al ser absorbidas).          | Se pierden si no se recogen antes de terminar.                    |
| **Puntos de Experiencia (`XP`)**                | Absorción de gemas de chatarra en combate.                                             | Se acumulan continuamente para alcanzar el umbral de subida de nivel.                                               | Reinicia contador relativo al subir de nivel (escalado ×1.5). | No se pierde XP acumulada durante la partida activa.              |
| **Niveles de Jugador**                          | Alcanzar el umbral de experiencia requerida.                                           | Concede una tirada de draft (3 cartas de mejora de atributos).                                                      | Techo dinámico determinado por el tiempo de la partida.       | Permanente durante la run; se reinicia a Nivel 1 en la siguiente. |
| **Tiempo de Partida**                           | Cronómetro interno continuo en segundos.                                               | Determina el avance a través de las 5 fases de dificultad del spawner.                                              | Minuto 8:00 (Aparición de Gólgora).                           | Se reinicia al morir o comenzar nueva run.                        |
| **Entidades Interdimensionales (Meta-recurso)** | Captura de 1 entidad aleatoria mediante la Ruleta de Extracción al derrotar a Gólgora. | Se equipan en el Almacén (hasta **3 entidades por partida**); obtener un duplicado suma **+1 nivel** a esa entidad. | Catálogo del Bestiario (3 ranuras activas por expedición).    | Permanente; nunca se pierden al morir en una partida Posterior.   |

---

## Fuentes y sumideros

### Fuentes (Generación de Recursos)
* **Destrucción de Enemigos:** Cada enemigo básico derrotado genera una gema de valor 10 XP. A mayor tasa de eliminación (DPS), mayor flujo de gemas en el suelo.
* **Atracción Magnética:** La habilidad del jugador de atraer recursos a distancia (2.5m base, ampliable con mejoras) acelera la absorción sin interrumpir la navegación.
* **Victoria sobre Gólgora (Extracción):** Abatir al jefe final al minuto 8:00+ habilita la única fuente de progresión permanente (la Ruleta de Extracción de entidades).

### Sumideros (Consumo y Gasto)
* **Subida de Nivel:** La experiencia acumulada se consume inmediatamente para desbloquear una selección de mejoras de la partida.
* **Daño por Contacto:** Los enemigos absorben la salud del jugador si logran sobrepasar la barrera del cono de luz y alcanzan al Coleccionista fuera del tiempo de *i-frames*.
* **Fusión de Duplicados:** Cualquier entidad repetida obtenida en la Ruleta de Extracción se consume automáticamente para incrementar en **+1 el nivel** de dicha entidad en el Almacén.

---

## Reglas de economía y retención

1. **Persistencia de Gemas en Suelo:** Las gemas permanecen en la posición de muerte del enemigo hasta que el jugador las recolecta. No tienen tiempo de caducidad inmediato, permitiendo rodear una zona para cosecharlas más tarde.
2. **Prioridad de Magnetismo:** Una vez que una gema entra en el radio magnético, pasa al estado de atracción activa y acelera a 12 m/s hacia el jugador, garantizando que el movimiento del jugador no rompa la recolección si este sigue avanzando.
3. **Pérdida por Muerte:** Si el Coleccionista es derrotado, todas las gemas, botiquines y mejoras *in-run* acumuladas en esa expedición se pierden y no se activa la Ruleta de Extracción. Las entidades previamente almacenadas en el Almacén se conservan intactas.

---

## Balance y Filosofía de Diseño

* **Comportamiento que se busca incentivar:**  
  * **Agresividad y avance continuo:** El jugador solo obtiene experiencia si destruye enemigos y avanza sobre su posición previa. La huida pasiva hacia zonas vacías deja todas las gemas atrás, estancando la progresión de nivel y provocando que la curva del spawner supere el poder del jugador.
* **Comportamiento que se busca castigar / evitar:**  
  * **Campeo en esquinas (*Corner Camping*):** Permanecer quieto en una esquina del mapa provoca que los racimos de enemigos flanqueen rápidamente por ambos lados y acorralen al jugador contra los muros perimetrales de la arena, cerrando sus rutas de evasión.

---

> **Navegación:** [[00 - Índice]] · ← [[05 - Progresión]] · [[07 - Reglas y Estados del Juego]] →
