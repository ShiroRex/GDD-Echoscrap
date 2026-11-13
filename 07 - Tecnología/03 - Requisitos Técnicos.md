# Requisitos técnicos

La plataforma del proyecto es Windows PC (`x64`). El montaje utiliza Unity `6000.6.3f1`, URP `17.6.0`, Input System `1.20.0` y Cinemachine `6.6.0` sobre `main` @ `b381ef4` / `develop` @ `977bac5` (13 de noviembre de 2026). La compilación principal habilita las escenas `MainMenu` y `CombatArena`.

El director de oleadas limita la población ordinaria a `150` enemigos simultáneos (`250` en modo de estrés), con un tope de `6` Morteros activos, `2` élites simultáneos y `8` secuaces efectivos dentro de la caldera de Gólgora. La colección en la bóveda gestiona un máximo de `10` pedestales físicos (`3` de equipo + `7` de exposición).

## Objetivos para la build de entrega

| Parámetro | Perfil mínimo a validar | Perfil recomendado a validar |
| --- | --- | --- |
| Sistema | Windows 10 de 64 bits | Windows 11 de 64 bits |
| CPU y GPU | Modelo mínimo por medir en equipo de entrada | Equipo de referencia Ryzen 7 5700X y Radeon RX 6650 XT |
| Memoria de prueba | 8 GB de RAM y 4 GB de VRAM | 16 GB de RAM y 8 GB de VRAM |
| Resolución y objetivo | 1280 × 720, 16:9, 30 FPS | 1920 × 1080, 16:9, 60 FPS |
| Presupuesto de fotograma | p95 de 33,3 ms o menos | p95 de 16,7 ms o menos |
| Espacio libre previsto | 2 GB para instalación y margen | 2 GB para instalación y margen |

La build Windows x64 de desarrollo compiló con `0` errores y un tamaño de salida de `1 010 519 179` bytes (aproximadamente `0,94 GiB`). El registro del Profiler en el Editor sobre `2 000` fotogramas arrojó un `p95` de CPU de `6,61 ms`.

## Conectividad y controles

- **Sincronización de ranking (`EventLeaderboardSync`):** utiliza peticiones HTTPS asíncronas (`UnityWebRequest` `PUT` y `DELETE`) hacia Firebase Realtime Database (`https://echoscrap-default-rtdb.firebaseio.com`) con tiempo de espera acotado (`8 s` para envío de ranking y `5 s` para autenticación) y cola local en `PlayerPrefs` (`EchoScrap.Rank.v2`). Si el equipo no dispone de conexión a internet, la partida y el guardado local continúan sin interrupciones y el envío pendiente se reintenta al reconectar.
- **Dispositivos de entrada:** **teclado y ratón** como único esquema de control oficial y validado en esta entrega: movimiento y dash (`WASD`/flechas y `Espacio`), recogida de fragmentos e interacciones de bóveda (`E`), desequipar rápido en balcón (`T`), Índice de Colección (`I`), consulta rápida de detalles del HUD (`Q`), cartas (`1..3`/`R`) y menús (`Esc`). *(Nota: aunque el proyecto incluye lecturas experimentales de `Gamepad.current` en `Input System`, no se usa ni se garantiza soporte oficial de mando en v1.0; su implementación integral queda proyectada a futuro).*

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
