# Economía y Recursos

## Descripción de la sección

Documenta los recursos cuantificables del juego, sus fuentes de obtención, mecanismos de gasto o pérdida, y las reglas de balance que rigen la economía en partida y a largo plazo.

---

## Recursos en partida (*In-Run*)

| Recurso | Obtención (Fuente) | Uso (Sumidero) | Límite / Tope | Pérdida |
|---|---|---|---|---|
| **Salud del Coleccionista (`Health`)** | Valor inicial fijado por `PlayerStats` (100 pts base). | Mitiga impactos de contacto directo con enemigos (10 pts por impacto). | 100 pts (ampliable mediante stats). | Si llega a 0, termina la partida (Derrota). |
| **Gemas de Chatarra / Ecos (`ExperienceGem`)** | Soltadas por cada enemigo básico eliminado (1 gema por enemigo). | Se absorben por magnetismo para llenar la barra de progreso de nivel. | Sin límite en arena (desaparecen al ser absorbidas). | Se pierden si no se recogen antes de que termine la partida. |
| **Puntos de Experiencia (`XP`)** | 10.0 pts por cada gema absorbida. | Se acumulan continuamente para alcanzar el umbral de subida de nivel. | Reinicia contador relativo al subir de nivel (escalando a x1.5). | No se pierde XP acumulada durante la partida activa. |
| **Niveles de Jugador (`Level`)** | Alcanzar el valor de `xpToNextLevel`. | Concede una tirada de draft (3 cartas de mejora de atributos). | Techo dinámico determinado por el tiempo de la partida. | Permanente durante la run; se reinicia a Nivel 1 en la siguiente. |
| **Tiempo de Partida (`TimeElapsed`)** | Cronómetro interno continuo en segundos. | Determina el avance a través de las 4 fases de dificultad del spawner. | Meta de 8 a 10 minutos (llegada de Gólgora). | Se reinicia al morir o comenzar nueva run. |

---

## Fuentes y sumideros

### Fuentes (Generación de Recursos)
* **Destrucción de Enemigos:** Cada enemigo básico derrotado instancia una gema de valor 10 XP (`Gem_XP.prefab`). A mayor tasa de eliminación (DPS), mayor flujo de gemas en el suelo.
* **Atracción Magnética:** La habilidad del jugador de atraer recursos a distancia (2.5m base, ampliable con mejoras) acelera la absorción sin interrumpir la navegación.

### Sumideros (Consumo y Gasto)
* **Subida de Nivel:** La experiencia acumulada se consume inmediatamente para comprar la selección de mejoras.
* **Daño por Contacto:** Los enemigos absorben la salud del jugador si logran sobrepasar la barrera del cono de luz y alcanzan al Coleccionista fuera del tiempo de *i-frames*.

---

## Reglas de economía y retención

1. **Persistencia de Gemas en Suelo:** Las gemas permanecen en la posición de muerte del enemigo hasta que el jugador las recolecta. No tienen tiempo de caducidad inmediato, permitiendo rodear una zona para cosecharlas más tarde.
2. **Prioridad de Magnetismo:** Una vez que una gema entra en el radio magnético, bloquea su estado a `isAttracting = true` y acelera a 12 m/s hacia el jugador, garantizando que el movimiento del jugador no rompa la recolección si este sigue avanzando.
3. **Pérdida por Muerte:** Si el Coleccionista es derrotado, la chatarra en suelo no recogida se pierde en el vertedero. Solo los Ecos consolidados regresan al Almacén.

---

## Balance y Filosofía de Diseño

* **Comportamiento que se busca incentivar:**  
  * **Agresividad y avance continuo:** El jugador solo obtiene chatarra si destruye enemigos y avanza sobre su posición previa. La huida pasiva hacia zonas vacías deja todas las gemas atrás, estancando la progresión de nivel y provocando que la curva del spawner supere el poder del jugador.
* **Comportamiento que se busca castigar / evitar:**  
  * **Campeo en esquinas (*Corner Camping*):** Permanecer quieto en una esquina del mapa provoca que los racimos de enemigos flanqueen rápidamente por ambos lados y atrapen al jugador contra el muro, donde el knockback pierde efectividad.

---

> **Navegación:** [[00 - Índice]] · ← [[05 - Progresión]] · [[07 - Reglas y Estados del Juego]] →
