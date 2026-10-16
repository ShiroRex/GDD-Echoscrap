# Objetos y Coleccionables

## Descripción de la sección

Documenta todos los elementos interactivos que el Coleccionista puede recoger en la arena durante la partida (recolectables en tiempo real) y los recursos extraídos para la meta-progresión en el Almacén.

---

## 1. Recolectables en Tiempo Real (En Combate)

Los objetos en combate son expulsados por los enemigos al ser destruidos. Permanecen sobre el plano de la arena hasta ser atraídos por el campo magnético del personaje o recogidos por contacto directo.

### 1.1 Gemas de Chatarra y Núcleos de Eco (Experiencia / XP)

**Tipo:** Recolectable pasivo de progresión durante la partida.  
**Propósito:** Alimentar la barra de experiencia del Coleccionista para activar la selección de hasta tres cartas de mejora, según [[05 - Progresión]], y adaptar su configuración frente a la horda.

* **Cómo se obtiene:**
  * Cada enemigo eliminado suelta una gema de chatarra en el punto exacto de su destrucción (100% de probabilidad).
* **Valores según el enemigo eliminado:**
  * **Gema Residual Básica (Color Cian/Azul):** **10 XP** (Enemigo Básico Residual e Interceptor).
  * **Gema Ámbar Volátil (Color Naranja):** **15 XP** (Polilla).
  * **Gema de Alta Presión (Color Púrpura):** **25 XP** (Mortero).
  * **Núcleo de Alta Densidad (Color Dorado brillante):** **50 XP** (Coloso).
* **Comportamiento y Atracción Magnética:**
  * **Radio de detección base:** **2.5 metros**. Mientras el jugador esté a mayor distancia, la gema permanece en el suelo.
  * **Velocidad de atracción:** Al cruzar el umbral del imán, la gema se orienta y viaja a **12 m/s** directamente hacia el personaje.
  * **Escalado:** Cartas como "Núcleo Supermagnético" (`GREED_01`) amplían este radio de atracción según el catálogo de [[05 - Progresión]].
* **Efecto al consumir:** Suma su valor a la experiencia acumulada. Al alcanzar el umbral de nivel, pausa la partida y despliega la selección de cartas de draft.

---

### 1.2 Botiquín de Chatarra Médica

**Tipo:** Recolectable reactivo de supervivencia y curación en combate.  
**Propósito:** Garantizar la viabilidad de supervivencia durante los 8 a 10 minutos de partida, mitigando el desgaste por contacto con la horda y recompensando la eliminación agresiva de enemigos pesados.

* **Función:** Restaura de forma inmediata **15 puntos de salud** al Coleccionista.
* **Restricción de Salud Máxima y Sobrecuración:**
  * Por regla general, la curación no sobrepasa el límite máximo del jugador (base de 100 HP) ni otorga escudos. Si el jugador ya se encuentra al 100% de vida, recoger el botiquín lo consume sin incrementar la salud.
  * **Excepción de diseño:** Cuando el jugador tiene activa la carta de mejora `BIO_04` ([[05 - Progresión]]), el exceso de salud no se descarta; se acumula en un contador especial que desata una *Nova de Plasma* al reunir 15 HP de sobrecuración.
* **Probabilidades de Caída (Drops calibrados):**
  * **Enemigos Básicos y Ligeros (Básico, Polilla, Interceptor):** **4%** de probabilidad de soltar botiquín al morir.
  * **Enemigos Pesados y Élite (Coloso, Mortero):** **25%** de probabilidad garantizada de soltar botiquín.
* **Comportamiento Físico y Magnético:**
  * Comparte el mismo radio magnético base de **2.5 metros** y velocidad de succión de **12 m/s** que las gemas de XP.
* **Retroalimentación Audiovisual:** Emite un destello visual verde/cian de reparación y actualiza de inmediato el indicador de vida en la interfaz del jugador.

---

## 2. Coleccionables Permanentes (Meta-Progresión en el Almacén)

### 2.1 Entidades Interdimensionales Capturadas (Bestiario del Almacén)

**Tipo:** Coleccionable principal, permanente y equipable de meta-progresión.  
**Propósito:** Dar sentido al rol de *El Coleccionista*, permitiendo reunir supervivientes de otras dimensiones en el Almacén (Hub), conocer su trasfondo y personalizar las estadísticas o efectos iniciales antes de cada expedición.

* **Cómo se obtiene (Ruleta de Extracción):**
  * Exclusivamente al lograr la **Victoria** en una expedición (sobrevivir los 8 minutos de asedio y abatir al jefe Gólgora). Al caer Gólgora, se activa la **Ruleta de Extracción**, que selecciona aleatoriamente 1 entidad dimensional para incorporarla de forma permanente a la vitrina del Almacén. En caso de derrota, no se realiza ninguna captura.
* **Equipamiento en el Almacén (3 ranuras activas):**
  * Desde el Almacén, el jugador puede inspeccionar las entidades capturadas y equipar hasta **3 entidades por partida**. Cada entidad equipada otorga un modificador pasivo o rasgo funcional al Coleccionista desde el segundo 0:00 de la incursión.
* **Sistema de Duplicados (Subida de Nivel):**
  * Si la Ruleta de Extracción otorga una entidad que el jugador ya poseía en su colección, el duplicado **suma +1 nivel a dicha entidad**, potenciando escalonadamente el beneficio pasivo que aporta al ser equipada.
* **Estructura de Rarezas y Catálogo:**
  * La colección se organiza en 5 niveles de rareza: **Común, Raro, Épico, Legendario y Mítico** (con una visión de 14 entidades y un alcance prioritario de entrega de 8 a 10 entidades). La lista definitiva de identidades, nombres y trasfondo de cada entidad se documenta en coordinación con el responsable de personajes en [[03 - Personajes]].

---

## 3. Resumen Rápido de Objetos y Coleccionables

| Objeto / Coleccionable | Tipo / Origen | Frecuencia de Obtención | Efecto Funcional | Persistencia |
|---|---|---|---|---|
| **Gema de Chatarra XP** | Botín de combate en suelo | 100% en todos los enemigos | +10 a +50 XP (según tipo de enemigo; 500 XP en Gólgora) | Volátil (*In-Run*; radio magnético 2.5 m, 12 m/s) |
| **Botiquín de Reparación** | Botín de combate en suelo | 4% (Básicos) / 25% (Pesados) | +15 Salud inmediata (acumula sobrecuración con `BIO_04`) | Volátil (*In-Run*; radio magnético 2.5 m, 12 m/s) |
| **Entidad Interdimensional** | Ruleta de Extracción tras vencer a Gólgora | 1 entidad por Victoria | Equipable en el Almacén (máx. 3 por partida); duplicados suman +1 nivel a la entidad | Permanente (Guardado en el Almacén) |

---

> **Navegación:** [[00 - Índice]] · ← [[03 - Enemigos y NPC]] · [[05 - Misiones y Desafíos]] →
