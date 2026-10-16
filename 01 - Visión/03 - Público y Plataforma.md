# Público y Plataforma

## Descripción de la sección

Aquí se define para quién se diseña el videojuego y dónde se jugará. Las decisiones de diseño, interfaz, controles y rendimiento dependen de estas variables.

---

## Público objetivo

* **Edad aproximada:** +14 años (adolescentes y adultos jóvenes).
* **Intereses relevantes:**
  * Aficionados a los subgéneros *Horde Survival* y *Action Roguelite*.
  * Jugadores atraídos por la estrategia mediante creación de *builds* y sinergias.
  * Fanáticos de la estética retro y *pixel art* 2D integrado en entornos 3D.
* **Conocimientos previos necesarios:**
  * Comprensión del movimiento direccional (WASD / Flechas o Stick izquierdo).
  * Concepto de evasión rápida (*Dash*) con ventana de invulnerabilidad.

---

## Plataforma

| Elemento                    | Decisión                                  | Justificación / Contexto                                                                                 |
| :-------------------------- | :---------------------------------------- | :------------------------------------------------------------------------------------------------------- |
| **Plataforma principal**    | **PC (Windows)**                          | Entorno principal de desarrollo y evaluación.                                                            |
| **Plataformas secundarias** | Steam Deck / Itch.io (Futuro)             | Portabilidad viable a futuro debido al reducido número de botones requeridos.                            |
| **Método de entrada**       | Teclado y Mando (Gamepad)                 | Movimiento con WASD/Flechas o Stick, Dash con Espacio/Botón Sur, y navegación completa sin usar ratón.   |
| **Resolución objetivo**     | **1920 × 1080 (Full HD)**                 | Estándar actual de monitores y laptops.                                                                  |
| **Relación de aspecto**     | **16:9 panorámico**                       | Garantiza un campo visual amplio con cámara a 60° para anticipar hordas desde los laterales.             |
| **Rendimiento objetivo**    | **60 FPS estables**                       | Requisito clave manteniendo al menos 150 enemigos simultáneos en pantalla (y hasta 250 en picos).       |
| **Conexión requerida**      | **100% Offline / Local**                  | Garantía de funcionamiento autónomo sin depender de conexión a la red.                                   |

---

## Restricciones de plataforma

1. **Hardware:** El juego debe proyectarse para correr de manera fluida en laptops y equipos de gama media en adelante sin caídas severas de rendimiento.
2. **Cero dependencia obligatoria de mouse:** Todo el flujo del juego (menús, Almacén, combate y selección de cartas con teclas `1, 2, 3` o cruceta) debe ser completamente navegable con teclado o control.
3. **Almacenamiento:** Guardado local en disco en formato JSON estructurado (con copia de respaldo `.bak`) para evitar pérdidas de datos.

---

## Sesión de juego

* **Duración esperada de partidas:**  
  Diseñado para sesiones rápidas e intensas con duración de 8 a 10 minutos, incluyendo el enfrentamiento contra el jefe final (*Gólgora / El Guardián*).
* **Duración total estimada:**  
  De 4 a 8 horas para un jugador promedio que busque desbloquear los 9 coleccionables del Almacén, subirlos de nivel mediante duplicados, armar combinaciones de hasta 3 entidades equipadas y descubrir la historia completa en sus registros.

---

## Modelo de distribución

* **Categoría:** **Prototipo / Demo académica.**
* **Distribución:** Ejecutable independiente para Windows mediante archivo `.zip` y publicación en *Itch.io* para pruebas y retroalimentación.

---

## Consideraciones de Diseño

- **Controles adaptados a la plataforma:** Esquema minimalista de movimiento, esquiva y selección rápida.
- **Interfaz legible:** Tipografía de alto contraste visible con claridad durante el combate.
- **Rendimiento:** Desempeño fluido a 60 FPS mediante sprites 2D integrados sobre un escenario 3D optimizado.
- **Adaptación de pantalla:** Escalado nativo a 1080p con soporte automático para 720p y 1440p (16:9).
- **Guardado:** Sistema de persistencia local que registre las entidades obtenidas, sus niveles y las 3 ranuras equipadas.

---

**Navegación:** `[[00 - Índice]]` • ⬅️ `[[02 - Visión del Juego]]` • `[[04 - Pilares de Diseño]]` ➡️
