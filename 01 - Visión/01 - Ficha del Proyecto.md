# 01 - Ficha del Proyecto

## Descripción de la sección
Esta sección permite conocer e identificar el proyecto de forma rápida. Debe permitir que alguien que nunca haya visto el videojuego entienda qué proyecto es y cuál es el estado actual.

---

## Información general

| Campo                        | Información                                                                                                                                                                                                                                              |
| :--------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Nombre del proyecto**      | EchoScrap                                                                                                                                                                                                                                                |
| **Versión del GDD**          | 0.3.0                                                                                                                                                                                                                                                    |
| **Estado del proyecto**      | Preproducción (Fase de Diseño y Aprobación de GDD)                                                                                                                                                                                                       |
| **Equipo**                   | - Mora Alejandro Angel Gabriel (Líder)<br>- Sierra Sosa David Alejandro (Desarrollo y Diseño de Juego)<br>- Cupul Dzul Mirian Leticia (Artista 1 — Personajes y Animación)<br>- Obispo Navarro Samuel Alejandro (Artista 2 — Entornos e Interfaz)<br>- Nuñez Chan Luis Fernando (Audio, Narrativa, UX y Calidad) |
| **Responsable del proyecto** | Mora Alejandro Angel Gabriel                                                                                                                                                                                                                             |
| **Fecha de inicio**          | 25 de septiembre de 2026                                                                                                                                                                                                                                 |
| **Última actualización**     | 16 de octubre de 2026                                                                                                                                                                                                                                    |
| **Repositorio**              | https://github.com/ShiroRex/GDD-Echoscrap                                                                                                                                                                                                                |

---

## Identidad del videojuego

* **Género principal:**  
  **Action Roguelite**.

* **Subgéneros:**  
  **Acción** / **Horde Survival**.

* **Plataforma:**  
  **PC (Windows)**.

* **Modo de juego:**  
  **Un jugador Local.**

* **Perspectiva y Cámara:**  
  El juego se ve en **2.5D con cámara cenital a una inclinación fija de 60°** desde arriba, siguiendo al personaje de forma fluida mientras se mueve por el mapa.

* **Clasificaciones y etiquetas adicionales:**  
  *Scrap-punk*, Fantasía Oscura, Pixel Art 2D, Low-Poly 3D, Progresión Permanente y Control con Teclado / Mando.

* **Tecnología principal:**  
  * **Motor:** Unity Engine.
  * **Lenguaje:** C# (.NET).
  * **Arte 2D:** Aseprite (Sprites del protagonista, antagonista, enemigos, entidades coleccionables e iconos de interfaz).
  * **Arte 3D:** Blender.
  * **Interfaz y Mockups:** Figma.
  * **Audio:** Reaper.
  * **Control de versiones:** Git con Git LFS vía GitHub.

---

## Elevator Pitch

**¿Qué es?**  
*EchoScrap* es un *horde survival* en 2.5D donde tomas el rol de *El Coleccionista*, un humano cuya obsesión por poseer lo inalcanzable lo llevó a aceptar el trato engañoso de una entidad misteriosa, despertando sin recuerdos atrapado en el Vertedero de Ecos: una fosa y prisión dimensional donde caen los restos de realidades destruidas. Con una linterna de autoapuntado como arma principal, habilidades secundarias automáticas desbloqueables en partida y un dash evasivo, el jugador sobrevive durante 8 minutos a oleadas de criaturas residuales y captura **Entidades** de mundos extintos (pudiendo equipar hasta 3 en su Almacén) al derrotar a *Gólgora* (*El Guardián*), un coloso titánico encargado de impedir que cualquier recluso escape de la arena.

---

## Estado actual

- **Concepto, alcance inicial y mecánicas principales:** Definidos y documentados en este GDD para revisión y aprobación docente.
- **Siguientes etapas tras aprobación del GDD:** Construcción del prototipo funcional, producción de recursos definitivos, pruebas integrales y cierre de versión final.

---

## Criterios de éxito del proyecto

Para considerar que el videojuego cumplió su propósito, debe alcanzarse lo siguiente:

1. **Estabilidad Técnica:** La build de PC deberá correr a 60 FPS estables sin caídas graves de rendimiento con al menos 150 enemigos simultáneos (soportando picos de hasta 250 en máximo estrés).
2. **Bucle Completo sin Fricción:** El jugador podrá iniciar en el Almacén (equipando hasta 3 entidades), jugar una partida de supervivencia de 8 a 10 minutos, enfrentar al jefe (*Gólgora / El Guardián*) y, al derrotarlo, girar la Ruleta de Extracción para obtener una entidad aleatoria (o sumarle +1 nivel si es duplicada) y verla reflejada en el Almacén.
3. **Persistencia Funcional:** La obtención de **Entidades coleccionables**, sus niveles y su equipamiento en el Almacén se guardará y cargará localmente en formato JSON estructurado sin pérdida de datos.
4. **Cohesión Estética:** Integración de sprites *Pixel Art* en el escenario 3D bajo la paleta de colores Apollo sin errores visuales, acompañado de efectos sonoros y música dinámica.
5. **Cero Bugs Críticos:** Ningún fallo bloqueante que perjudique la versión final del videojuego.

---

**Navegación:** `[[00 - Índice]]` · ← `[[00 - Índice]]` · `[[02 - Visión del Juego]]` →