# Requisitos técnicos

La plataforma del proyecto es Windows PC (`x64`). El montaje utiliza Unity `6000.6.3f1`, URP `17.6.0`, Input System `1.20.0` y Cinemachine `6.6.0` sobre `main` @ `734e2a5` / `develop` @ `6973556` (13 de noviembre de 2026). La compilación principal habilita las escenas `MainMenu` y `CombatArena`. El icono de aplicación de escritorio (`EchoScrap-AppIcon.png`) está enlazado en `ProjectSettings/ProjectSettings.asset`.

El director de oleadas limita la población ordinaria a `150` enemigos simultáneos (`250` en modo de estrés), con un tope de `6` Morteros activos, `2` élites simultáneos y `8` secuaces efectivos dentro de la caldera de Gólgora. La colección en la bóveda gestiona un máximo de `10` pedestales físicos (`3` de equipo + `7` de exposición).

## Perfiles de referencia y cierre de validación técnica

| Parámetro | Perfil mínimo objetivo | Perfil recomendado de referencia |
| --- | --- | --- |
| Sistema | Windows 10 de 64 bits | Windows 11 de 64 bits |
| CPU y GPU | Hardware de entrada de 64 bits compatible con DirectX 11 / URP | Equipo de referencia Ryzen 7 5700X y Radeon RX 6650 XT |
| Memoria de prueba | 8 GB de RAM y 4 GB de VRAM | 16 GB de RAM y 8 GB de VRAM |
| Resolución y objetivo | 1280 × 720, 16:9, 30 FPS | 1920 × 1080, 16:9, 60 FPS |
| Presupuesto de fotograma | p95 de 33,3 ms o menos | p95 de 16,7 ms o menos |
| Espacio libre previsto | 2 GB para instalación y margen | 2 GB para instalación y margen |

**Cierre de validación técnica (`TECH-01` — Cerrado para entrega v1.0):**

- **Compilación y distribución Release:** el perfil `Assets/Settings/Build Profiles/Windows.asset` configura la build final de distribución en modo **Release** (`m_Development: 0`, `m_ConnectProfiler: 0`, `m_AllowDebugging: 0`) con las escenas `MainMenu` y `CombatArena`. La compilación Windows x64 cerró con **`0` errores** y un tamaño medido de **`1 010 519 179` bytes (~`0,94 GiB`)**, dentro del margen de `2 GB`, quedando publicada como ejecutable independiente en [`https://naked-hiro.itch.io/echoscrap`](https://naked-hiro.itch.io/echoscrap).
- **Rendimiento y estabilidad en oleadas:** el registro del Profiler sobre `2 000` fotogramas arrojó un **`p95` de CPU de `6,61 ms`** (ampliamente por debajo del presupuesto de `16,7 ms` para `60 FPS`). Asimismo, el código integra la suite de medición `CombatBenchmarkRunner.cs` / `SpawnerRunValidation.cs`, pooling de enemigos, proyectiles y charcos, control de población (`150` ordinarios) y las correcciones de las ramas `feature/perfil-memoria-baja` y `bugfix/tirones-horda-windows` (con deslizadores adicionales de `Efectos decorativos` y `Efectos reducidos` en `MenuSettingsView` para hardware modesto).
- **Alcance acotado documentado:** no se registraron métricas cuantitativas dedicadas de FPS/`p95` sobre un equipo físico de gama mínima (`8 GB RAM`), por lo que la validación sobre ese hardware específico queda registrada como limitación conocida de la entrega en [[03 - Bugs y Seguimiento]].

## Conectividad y controles

- **Sincronización de ranking (`EventLeaderboardSync`):** utiliza peticiones HTTPS asíncronas (`UnityWebRequest` `PUT` y `DELETE`) hacia Firebase Realtime Database (`https://echoscrap-default-rtdb.firebaseio.com`) con tiempo de espera acotado (`8 s` para envío de ranking y `5 s` para autenticación) y cola local en `PlayerPrefs` (`EchoScrap.Rank.v2`). Si el equipo no dispone de conexión a internet, la partida y el guardado local continúan sin interrupciones y el envío pendiente se reintenta al reconectar.
- **Dispositivos de entrada:** soporte completo para teclado/ratón y mando en combate (movimiento y dash `Espacio` / `A` / `Cruz`), recogida de fragmentos e interacciones de bóveda (`E` / botón oeste `X` / `Cuadrado`), desequipar rápido en balcón (`T` / botón norte `Y` / `Triángulo`), Índice de Colección (`I` / `Select`), consulta rápida de detalles del HUD (`Q` / `Select`) y menús (`Esc` / `Start` / botón este `B` / `Círculo`).

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
