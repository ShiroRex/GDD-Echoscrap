# Público y Plataforma

## Descripción de la sección

Define para quién se diseña el videojuego y dónde se jugará. Las decisiones de diseño, interfaz, controles y rendimiento dependen de estas variables.

---

## Público objetivo

* **Edad aproximada:** +14 años (adolescentes y adultos jóvenes).
* **Experiencia esperada:**  
  **Mid-Core (Accesible en controles, desafiante en juego tardío).**  
  *Nota de diseño:* Aunque el esquema de control es simple (moverse y esquivar con una sola mano), la exigencia de reflejos, el posicionamiento constante frente a 250 enemigos y la optimización de sinergias numéricas no corresponden a un jugador casual puro. Es la curva de *Brotato* y *Vampire Survivors*: entrada instantánea, maestría exigente.
* **Intereses relevantes:**
  * Aficionados a los subgéneros *Bullet Heaven*, *Horde Survival* y *Action Roguelites*.
  * Jugadores atraídos por el *theorycrafting* rápido (construcción de builds mediante sinergias de objetos/personajes).
  * Entusiastas de la estética retro moderna, *Pixel Art* refinado e iluminación estilo *HD-2D*.
* **Conocimientos previos necesarios:**
  * Comprensión intuitiva del movimiento en plano bidimensional / 2.5D (WASD / Flechas).
  * Concepto de evasión con tiempo de invulnerabilidad (*Dash / i-frames*). No se requiere experiencia previa en puntería manual gracias al auto-disparo direccional.

---

## Plataforma

| Elemento | Decisión | Justificación / Contexto |
| :--- | :--- | :--- |
| **Plataforma principal** | **PC (Windows)** | Entorno nativo para la evaluación y defensa del comité sinodal universitario. |
| **Plataformas secundarias** | Steam Deck / Itch.io (Futuro) | Portabilidad viable a futuro debido al bajo número de botones requeridos. |
| **Método de entrada** | **Teclado (Primario)** / Gamepad (Secundario) | Movimiento con WASD o Flechas, Dash con Barra Espaciadora. Compatible con Stick izquierdo y Botón Sur (A/Cruz). |
| **Resolución objetivo** | **1920 × 1080 (Full HD)** | Estándar de la industria y resolución nativa de proyectores/monitores de evaluación. |
| **Relación de aspecto** | **16:9 panorámico** | Garantiza un campo visual periférico amplio para anticipar hordas desde los laterales. |
| **Rendimiento objetivo** | **60 FPS estables** | Requisito innegociable con al menos 250 enemigos simultáneos en pantalla (*Object Pooling*). |
| **Conexión requerida** | **No (100% Offline / Local)** | Garantía de ejecución sin fallos ni dependencias de red durante presentaciones en vivo. |

---

## Restricciones de plataforma

1. **Hardware Heterogéneo de Evaluación:** El juego debe correr de manera óptima en laptops de gama media (gráficos integrados modernos o GPUs discretas básicas) sin caídas de cuadros.
2. **Cero Dependencia de Ratón:** Todo el bucle de juego (in-game y menús de selección de 3 cartas) debe ser completamente navegable con teclado o control.
3. **Persistencia Local Segura:** Guardado en disco local (`JSON` encriptado o binario en `Application.persistentDataPath`) para evitar pérdidas de datos o permisos de administrador en Windows.

---

## Sesión de juego

* **Duración esperada de una sesión (Run):**  
  **8 a 10 minutos**. Diseñado para sesiones intensas, con ritmo comprimido y clímax garantizado al minuto 8:00 con *Gólgora*.
* **Duración total estimada del juego (Metaprogresión):**  
  **4 a 8 horas** para un jugador intermedio que busque desbloquear las 14 Entidades del Almacén, optimizar sus vitrinas y vencer consistentemente al jefe final.

---

## Modelo de distribución

* **Categoría:** **Prototipo Académico de Titulación / Demo Indie Gratuita**.
* **Distribución:** Ejecutable independiente empaquetado (.zip / instalador) y subida a plataforma *Itch.io* para testeo externo y retroalimentación de jugadores.

---

## Consideraciones de Diseño

- [x] **Controles adaptados a la plataforma:** Esquema minimalista centrado en movimiento y esquiva.
- [x] **Interfaz legible:** Tipografía pixel/sans de alto contraste visible a distancia de monitor o proyector.
- [x] **Rendimiento considerado:** Presupuesto de draw calls reducido y sprites 2D sobre geometría 3D optimizada.
- [x] **Resoluciones consideradas:** Escalado nativo a 1080p con soporte automático para 720p y 1440p (16:9).
- [x] **Guardado compatible:** Serialización ligera de progreso de vitrinas y chatarra acumulada.

---

**Navegación:** `[[00 - Índice]]` • ⬅️ `[[02 - Visión del Juego]]` • `[[04 - Pilares de Diseño]]` ➡️
