# Controles

## Cómo se juega y cómo avanzar

1. **Inicio y bóveda (preparación):** en el menú inicial elige **Continuar** o **Nueva colección** (opcionalmente registra tu alias para el ranking en línea). Ya dentro de la bóveda, usa los rótulos `EQUIPO 1..3`, el **Índice de Colección (`I`)** y la **Máquina de Fusión (`E`)** para equipar hasta **3 acompañantes** cuyos poderes pasivos se suman y acumulan por copia equipada, o combinar **4 copias** durante `30 s` para obtener entidades de mayor rareza (límite máximo de `10` unidades).
2. **Entrada a la arena:** acércate al **portal norte** en la bóveda para abrir `ELIGE LA RONDA` y seleccionar **Ronda Demo** (`3:00` hasta el jefe) o **Ronda Completa** (`8:00` hasta el jefe).
3. **Combate y progresión:** muévete (`WASD`/flechas) y esquiva con **Dash (`Espacio`)** mientras tu linterna ataca automáticamente al enemigo más cercano. Recoge **gemas de XP** para subir de nivel y elegir mejoras (`1/2/3` o clic), recoge **botiquines** para curarte y pulsa **`E`** cerca de los fragmentos caídos para capturar nuevas entidades coleccionables.
4. **Objetivo y victoria:** sobrevive hasta el final del reloj para invocar al jefe final **Gólgora (MataColeccionista)** en la caldera central (`22 m`) y derrótalo para ganar la incursión, registrar tu puntaje y regresar a la bóveda con tus capturas.

> [!NOTE]
> **Esquema oficial de la versión actual (`1.0`):** *EchoScrap* utiliza **teclado y ratón** como único esquema de control oficial, completo y validado para esta entrega en PC. Aunque el código base (`Input System 1.20.0`) incluye enlaces preliminares y experimentales para gamepad (`Gamepad.current`), **no se utiliza ni se garantiza soporte oficial de mando en esta versión** (no cubre la navegación completa de todos los menús ni ha sido certificado sobre hardware); su integración formal queda planteada como objetivo a futuro.

| Acción | Teclado y ratón *(oficial)* |
| --- | --- |
| Moverse | `WASD` o flechas |
| Dash | `Espacio` |
| Pausar / abrir ajustes (`General`, `Interfaz`, `Sonido`) | `Esc` |
| Consultar estadísticas completas y poderes de acompañantes en HUD | Mantener `Q` |
| Elegir una carta | `1`, `2` o `3`; clic |
| Cambiar carta enfocada | `A` / `D`, izquierda / derecha o puntero |
| Confirmar carta | `Enter` o clic |
| Reintentar oferta con *Sintonizador* | `R` o botón de interfaz |
| Recoger una entidad cercana en la arena | `E` (si hay hueco en `10/10`) |
| Abrir / cerrar el Índice de Colección en la bóveda | `I` o clic en `ÍNDICE · [I]` (cierra también con `Esc`) |
| Equipar o editar un puesto de equipo (puestos 1 a 3, balcón) | `E` (`E · EQUIPAR` / `E · EDITAR`) |
| Desequipar directamente un puesto de equipo ocupado en el mundo (puestos 1 a 3) | `T` |
| Liberar una unidad expuesta (planta baja) | `E` (`E · LIBERAR`); confirmar en modal con `E`, `Enter` o `Espacio` (`Esc` cancela) |
| Abrir la máquina de fusión en la bóveda | `E` (`E · FUSIONAR` / `E · VER FUSIÓN`) |
| Cerrar selector de equipo (`CompanionView`) | `E`, `Esc` o botón `CERRAR · E / ESC` |
| Cerrar ventana de fusión (`FusionStation`) | `E`, `Esc` o botón `X` superior derecho |
| Navegar entre consejos de la bóveda (`VaultTips`) | Clic en `<` / `>` |
| Cancelar selección de ronda o confirmación general | `Esc` o botón de interfaz |

## Enlaces experimentales de mando (pensado a futuro, sin soporte en v1.0)

El proyecto no contempla el uso de mando como requisito ni prestación garantizada en la entrega actual. En el código fuente (`PlayerInputReader`, `CombatHud`, `CompanionSystem`, `FusionStation`, `VaultReleaseInteraction`, `CollectibleShard` y `VaultCollectionIndex`) existen asignaciones experimentales de partida para lecturas puntuales (`Stick izquierdo`, `A`/`Cruz` para dash, `Q`/`Select` para detalles del HUD, `buttonWest` para `E`, `buttonNorth` para `T` y `selectButton` para `[I]`), pero quedan **fuera del alcance de soporte oficial, documentación de usuario y validación de QA** hasta una futura actualización de post-entrega que complete también la navegación de cursor/UI con controlador.

La linterna ataca automáticamente al objetivo válido más cercano. Los portales se activan por contacto; el de la bóveda abre la selección de ronda (`ELIGE LA RONDA`) durante la preparación.

`Espacio` se reserva al dash durante el combate y la selección de cartas para evitar confirmaciones accidentales; en el modal de confirmación de liberación de la bóveda (`LIBERAR UNIDAD`), tanto `E` como `Enter` o `Espacio` confirman la liberación y `Esc` la cancela.

Mantener pulsada la tecla **`Q`** durante el combate (`CombatHud.IsHudDetailHeld`) despliega temporalmente el panel de estadísticas en modo completo (`COMPLETAS`) y muestra las líneas descriptivas de los poderes de los tres acompañantes equipados aunque estén ocultos o reducidos en la pestaña `Interfaz` del menú de pausa.

El laboratorio de pruebas (`EnemySandboxController`) usa `Tab` para mostrar u ocultar el panel y `R` para reiniciar la escena de prueba.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
