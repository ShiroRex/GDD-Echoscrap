# 01 - Ficha del Proyecto

## Descripción de la sección
Esta ficha identifica el proyecto de forma rápida. Debe permitir que alguien que nunca haya visto el videojuego entienda qué proyecto es y cuál es su estado actual.

---

## Información general

| Campo                        | Información                               |
| :--------------------------- | :---------------------------------------- |
| **Nombre del proyecto**      | EchoScrap                                 |
| **Nombre clave**             | EchoScrap                                 |
| **Versión del GDD**          | 0.1.1                                     |
| **Estado del proyecto**      | Preproducción / Inicio de Producción      |
| **Equipo**                   | -Angel Mora (Lider)<br>-insertar personas |
| **Responsable del proyecto** | Angel Mora                                |
| **Fecha de inicio**          | 25 de septiembre de 2026                  |
| **Última actualización**     | 25 de septiembre de 2026                  |
| **Repositorio**              | [Enlace al repositorio de GitHub]         |

---

## Identidad del videojuego

* **Género principal:**  
  **Action Roguelite** (Roguelite de Acción).

* **Subgéneros:**  
  **Bullet Heaven** / **Horde Survival** (supervivencia contra hordas masivas por tiempo con arma principal direccional y armas secundarias de activación automática).

* **Plataforma objetivo:**  
  **PC (Windows)** (distribución vía ejecutable local / itch.io para demostración académica).

* **Modo de juego:**  
  **Un jugador (Single Player)** / Local.

* **Perspectiva y Cámara:**  
  **2.5D Isométrica**. Cámara cenital inclinada en perspectiva fija (~50° en el eje X) con seguimiento suave del jugador (*Cinemachine*).

* **Clasificaciones y etiquetas adicionales:**  
  *Scrap-punk*, Fantasía Oscura Residual, Pixel Art 2D, Low-Poly 3D, Progresión Permanente (Meta-progression), Control con Teclado.

* **Tecnología principal:**  
  * **Motor:** Unity Engine (Universal Render Pipeline - URP).
  * **Lenguaje:** C# (.NET).
  * **Arte 2D:** Aseprite (Sprites del protagonista, enemigos e iconos de interfaz).
  * **Arte 3D:** Blender (Geometría modular del escenario y props de chatarra).
  * **Interfaz y Mockups:** Figma.
  * **Audio:** n/a
  * **Control de versiones:** Git con Git LFS vía GitHub.

---

## Elevator Pitch

**¿Qué es?**  
*EchoScrap* es un *horde survival* en 2.5D donde encarnas al *Coleccionista*, el último superviviente de una orden de captores interdimensionales, que viaja al Vertedero de Ecos para sobrevivir oleadas de criaturas residuales. Con una linterna direccional como arma principal y armas secundarias automáticas desbloqueables, el jugador captura entidades de mundos extintos y acumula artefactos permanentes en su Almacén antes de que *Gólgora*, un coloso titánico, despierte para purificarlo por sus crímenes.

---

## Estado actual

- [x] Concepto definido
- [ ] Alcance inicial definido
- [ ] Mecánica principal definida
- [ ] Prototipo funcional *(En desarrollo - Semana 1)*
- [ ] Vertical slice
- [ ] Producción
- [ ] Pruebas
- [ ] Versión final

---

## Criterio de éxito del proyecto

Para considerar que el videojuego cumplió su propósito en la titulación universitaria, debe alcanzarse lo siguiente:

1. **Estabilidad Técnica:** La build de PC corre a 60 FPS estables sin caídas graves de rendimiento con al menos 250 enemigos simultáneos en pantalla (*Object Pooling* implementado).
2. **Bucle Completo sin Fricción:** El jugador puede iniciar en el Almacén, jugar una partida de supervivencia de 8 a 10 minutos, enfrentar al jefe *Gólgora*, girar la ruleta de extracción post-partida y ver reflejada su mejora permanente en el Almacén.
3. **Persistencia Funcional:** El progreso (artefactos y chatarra) se guarda y carga correctamente en disco local sin pérdida de datos.
4. **Cohesión Estética:** Integración limpia de sprites *Pixel Art* en el escenario 3D sin errores de orden de dibujado (*Z-fighting* ni parpadeos), acompañada de efectos sonoros y música en todas las transiciones.
5. **Cero Bugs Bloqueantes:** Ningún fallo crítico que interrumpa la ejecución durante la defensa en vivo ante el comité sinodal.

---

**Navegación:** `[[00 - Índice]]` · ← `[[00 - Índice]]` · `[[02 - Visión del Juego]]` →