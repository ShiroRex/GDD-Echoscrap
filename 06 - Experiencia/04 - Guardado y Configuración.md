# Guardado y Configuración

## Descripción de la sección

En este apartado se define cómo se planea gestionar la persistencia de datos del jugador y qué opciones de configuración técnica y de accesibilidad estarán disponibles en *EchoScrap*. Como el juego sigue una estructura de supervivencia roguelite con partidas autocontenidas de 8 a 10 minutos, el sistema de guardado está pensado para ser ágil, automático y centrado en el metajuego del Almacén sin interrumpir el ritmo del combate.

---

## Sistema de Guardado

### Tipo de guardado
* **Tipo:** **Completamente Automático.** El jugador no tendrá que preocuparse por guardar manualmente en ningún momento.

### Momentos de guardado
1. **Al finalizar una expedición:** Tanto tras una victoria frente a Gólgora (registrando la entidad capturada o su subida de nivel en la Ruleta de Extracción) como tras una derrota por caída de salud (registrando las estadísticas de la partida).
2. **Al salir del Almacén o modificar las 3 ranuras de equipamiento:** Cualquier cambio en las entidades equipadas previo a una partida se consolida de inmediato.
3. **Al modificar la configuración:** Cada vez que el usuario cambie alguna opción en el menú de ajustes y presione "Aplicar".

> **Decisión de diseño para las partidas:** Durante la estancia en la arena de 120 × 120 m **no se contempla el guardado intermedio ni la reanudación a mitad de oleada**. Si el jugador sale abruptamente durante el combate, esa expedición se contará como retirada/derrota, regresando al Almacén. Esto preserva la tensión y la regla clásica del género roguelite.

### Datos que se planea conservar
* **Datos de metajuego y colección (El Almacén):**
  * Catálogo de entidades interdimensionales desbloqueadas junto con el **nivel actual de cada entidad** (incrementado al obtener duplicados).
  * Configuración activa de las **3 ranuras de entidades equipadas** para la siguiente partida.
* **Estadísticas globales de juego:**
  * Número total de expediciones intentadas y completadas con éxito.
  * Tiempo máximo de supervivencia registrado.
  * Cantidad acumulada de enemigos destruidos.
* **Ajustes de usuario:**
  * Niveles de volumen, resolución, modo de pantalla y personalización de controles.

### Ranuras y perfiles
* Se proyecta **1 ranura de guardado local principal**.
* Para el alcance de este proyecto no se contemplan múltiples perfiles simultáneos, pero sí se incluirá un botón en el menú de opciones que permita "Borrar progreso y reiniciar partida" en caso de que se desee comenzar desde cero para pruebas.

### Manejo de errores y fallos de guardado
* Si el sistema detecta que el archivo de guardado está dañado o no se puede leer, se planea que el juego cargue automáticamente un perfil limpio con valores por defecto seguros (o restaure la copia `.bak`) y muestre un aviso en el menú principal ("No se pudo leer el archivo de guardado; se han restaurado los valores de respaldo"), impidiendo que el programa se cierre de forma inesperada o sufra errores críticos.

---

## Menú de Configuración

Se contempla implementar un menú de opciones accesible tanto desde la pantalla de inicio como desde el menú de pausa dentro de la partida:

| Categoría | Opciones Planificadas | Valores / Opciones |
|---|---|---|
| **Audio** | Volumen Maestro<br>Volumen de Efectos (SFX)<br>Volumen de Música (BGM) | Deslizadores continuos de 0% a 100% (con incrementos de 5% o 10%). |
| **Video** | Modo de Pantalla<br>Resolución<br>Sincronización Vertical (V-Sync)<br>Límite de Cuadros | Pantalla Completa / Ventana sin bordes.<br>1920×1080 (nativa), 1600×900, 1280×720.<br>Activada / Desactivada.<br>60 FPS fijos / Ilimitado. |
| **Controles** | Esquema de Teclas<br>Reasignación básica | Visualización del mapeo WASD + Dash (Espacio) + Reroll (R) + Pausa (Esc).<br>Posibilidad de reasignar teclas direccionales o de acción en PC. |
| **Accesibilidad** | Sacudida de Pantalla (*Screen Shake*)<br>Contraste de Telégrafos<br>Tamaño de Textos | Interruptor Sí / No (para jugadores sensibles al movimiento de cámara).<br>Opción de realzar el brillo de los círculos rojos de peligro.<br>Normal / Grande en las tarjetas de subida de nivel. |
| **Jugabilidad** | Asistencia de Auto-apuntado<br>Confirmación de Abandono | Activado por defecto (apunta al enemigo más cercano dentro del cono).<br>Ventana de confirmación al presionar "Salir al menú" durante una run. |

---

## Restablecimiento de Ajustes

* **Opciones que pueden restablecerse:** Todos los apartados de Audio, Video, Controles y Accesibilidad contarán con un botón visible de **"Restablecer a valores por defecto"**.
* **Protección del progreso:** Esta acción de restablecimiento afectará única y exclusivamente a las preferencias técnicas; los datos de entidades desbloqueadas, sus niveles y las estadísticas de juego nunca se borrarán al presionar este botón, requiriendo un comando separado de confirmación para evitar pérdidas accidentales.

> **Navegación:** [[00 - Índice]] · ← [[03 - Accesibilidad]] · [[01 - Arquitectura Técnica]] →
