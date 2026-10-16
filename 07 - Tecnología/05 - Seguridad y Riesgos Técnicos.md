# Seguridad y Riesgos Técnicos

## Descripción de la sección

En este apartado se identifican los principales riesgos técnicos que podrían afectar el rendimiento y la estabilidad de *EchoScrap*, así como las soluciones preventivas que el equipo planea implementar durante el desarrollo. Como proyecto académico, nuestro propósito es anticipar problemas de rendimiento, pérdida de datos o bloqueos antes de iniciar la programación oficial, garantizando que el juego funcione sin fallos durante las evaluaciones docentes.

---

## Matriz de Riesgos Técnicos y Mitigaciones

| Riesgo Técnico Identificado | Probabilidad | Impacto | Medida de Mitigación Planificada |
|---|:---:|:---:|---|
| **Caída de cuadros (FPS) por acumulación de enemigos** | Media | Alto | Limitar la horda a un máximo estricto de **150 enemigos activos** en la arena de 120 × 120 m. Implementar un sistema de reutilización de entidades (*Object Pooling*) para evitar la creación y destrucción continua de objetos en memoria. |
| **Saturación por exceso de gemas y botiquines en el suelo** | Media | Medio | Limitar el número máximo de recolectables sueltos en escena. Si se acumulan demasiadas gemas, se planea fusionar automáticamente las gemas pequeñas cercanas en una gema de mayor valor para reducir la carga de cálculos de física. |
| **Corrupción del archivo de guardado por cierre abrupto** | Baja | Alto | Implementar un guardado en dos pasos: escribir primero en un archivo temporal (`save_data.tmp`) y renombrarlo únicamente al comprobar que la escritura fue exitosa, manteniendo además un respaldo preventivo (`save_data.bak`). |
| **Fugas de memoria al cambiar entre la arena y el Almacén** | Baja | Medio | Diseñar la arquitectura técnica de forma modular, asegurando que todos los eventos de oleadas y enemigos se cancelen y limpien al descargar la escena de combate, permitiendo que el sistema libere la memoria RAM de forma limpia. |
| **Bloqueo o desconfiguración de controles** | Baja | Medio | Utilizar el sistema de control basado en acciones desacopladas e incluir en el menú de opciones un botón visible para restablecer todas las teclas a sus valores originales seguros. |

---

## Datos Sensibles y Privacidad

* **Tratamiento de información:** *EchoScrap* **no recopila, procesa ni almacena ningún tipo de dato personal**, identificador de usuario, correo electrónico ni información de hardware.
* **Privacidad local:** Toda la información que genera el juego se almacena de forma 100% local en la computadora del usuario y se limita estrictamente a contadores de progreso del juego (chatarra acumulada, personajes rescatados) y ajustes de configuración técnica (volumen y resolución).

---

## Multijugador y Conectividad en Red

* **Estado de la función:** **No aplica para el alcance de este proyecto.**
* **Justificación de diseño:** *EchoScrap* está concebido desde sus pilares como una **experiencia monojugador local (*single-player offline*)**. 
* Al no incluir juego en línea, servidores dedicados ni tablas de clasificación en la nube:
  * No se requieren sistemas de autenticación ni gestión de cuentas.
  * No existen problemas de latencia, desincronización de paquetes ni consumo de datos de internet.
  * No es necesario implementar capas complejas de seguridad contra trampas (*anti-cheat*) o validación de servidor, lo que permite que el equipo concentre el 100% del tiempo de desarrollo en la fluidez y el balance del combate.

---

## Recuperación ante Fallos y Cierres Inesperados

1. **Cierre abrupto durante una expedición (Alt+F4 o corte de energía):**
   * Dado que la partida en la arena de 120 × 120 m es volátil y no se guarda a mitad de oleada, si el juego se cierra inesperadamente, al volver a abrirlo el jugador aparecerá de forma segura en el Almacén con los recursos y personajes que ya tenía consolidados antes de iniciar esa run. No se producirán bloqueos ni partidas corruptas a mitad de partida.
2. **Fallo de lectura del archivo de guardado:**
   * Si el archivo `save_data.json` sufre algún daño o contiene datos ilegibles, el sistema intentará restaurar automáticamente la copia de respaldo `save_data.bak`.
   * Si ambos archivos presentan problemas, el juego generará un archivo nuevo con el progreso inicial por defecto y mostrará un mensaje informativo en pantalla, evitando que la aplicación se cierre de forma imprevista ante el evaluador.

> **Navegación:** [[00 - Índice]] · ← [[04 - Datos y Persistencia]] · [[01 - Alcance]] →
