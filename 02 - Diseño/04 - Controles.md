# Controles

## Cómo se juega y cómo avanzar

1. **Inicio y bóveda (preparación):** en el menú inicial elige **Continuar** o **Nueva colección** (opcionalmente registra tu alias para el ranking en línea). Ya dentro de la bóveda, usa los rótulos `EQUIPO 1..3`, el **Índice de Colección (`I` / `Select`)** y la **Máquina de Fusión (`E` / botón oeste)** para equipar hasta **3 coleccionables** cuyos poderes pasivos se suman y acumulan por copia equipada, o combinar **4 copias** durante `30 s` para obtener entidades de mayor rareza (límite máximo de `10` unidades).
2. **Entrada a la arena:** acércate al **portal norte** en la bóveda para abrir `ELIGE LA RONDA` y seleccionar **Ronda Demo** (`3:00` hasta el jefe) o **Ronda Completa** (`8:00` hasta el jefe).
3. **Combate y progresión:** muévete (`WASD`/flechas o stick/cruceta) y esquiva con **Dash (`Espacio` / `A` / `Cruz`)** mientras tu linterna ataca automáticamente al enemigo más cercano. Recoge **gemas de XP** para subir de nivel y elegir mejoras (`1/2/3`, clic o mando), recoge **botiquines** para curarte y pulsa **`E`** (o botón oeste **`X` / `Cuadrado`**) cerca de los fragmentos caídos para capturar nuevas entidades coleccionables.
4. **Objetivo y victoria:** sobrevive hasta el final del reloj para invocar al jefe final **Gólgora (MataColeccionista)** en la caldera central (`22 m`) y derrótalo para ganar la incursión, registrar tu puntaje y regresar a la bóveda con tus capturas.

| Acción | Teclado y ratón | Mando |
| --- | --- | --- |
| Moverse | `WASD` o flechas | Stick izquierdo o cruceta |
| Dash | `Espacio` | Botón sur (`A` / `Cruz`) |
| Pausar / abrir ajustes (`General`, `Interfaz`, `Sonido`) | `Esc` | `Start` |
| Consultar estadísticas completas y poderes de acompañantes en HUD | Mantener `Q` | Mantener `Select` |
| Elegir una carta | `1`, `2` o `3`; clic | Navegación y confirmación |
| Cambiar carta enfocada | `A` / `D`, izquierda / derecha o puntero | Stick izquierdo o cruceta |
| Confirmar carta | `Enter` o clic | Botón sur (`A` / `Cruz`) |
| Reintentar oferta con *Sintonizador* | `R` o botón de interfaz | Botón norte (`Y` / `Triángulo`) |
| Recoger una entidad cercana en la arena | `E` (si hay hueco en `10/10`) | Botón oeste (`X` / `Cuadrado`) |
| Abrir / cerrar el Índice de Colección en la bóveda | `I` o clic en `ÍNDICE · [I]` (cierra también con `Esc`) | Botón `Select` (`VaultCollectionIndex`, cierra también con `B` / `Círculo`) |
| Equipar o editar un puesto de equipo (puestos 1 a 3, balcón) | `E` (`E · EQUIPAR` / `E · EDITAR`) | Botón oeste (`X` / `Cuadrado`) |
| Desequipar directamente un puesto de equipo ocupado en el mundo (puestos 1 a 3) | `T` | Botón norte (`Y` / `Triángulo`) |
| Liberar una unidad expuesta (planta baja) | `E` (`E · LIBERAR`); confirmar en modal con `E`, `Enter` o `Espacio` (`Esc` cancela) | Botón oeste (`X` / `Cuadrado`); botón este (`B` / `Círculo`) cancela |
| Abrir la máquina de fusión en la bóveda | `E` (`E · FUSIONAR` / `E · VER FUSIÓN`) | Botón oeste (`X` / `Cuadrado`) |
| Cerrar selector de equipo (`CompanionView`) | `E`, `Esc` o botón `CERRAR · E / ESC` | Botón este (`B` / `Círculo`) |
| Cerrar ventana de fusión (`FusionStation`) | `E`, `Esc` o botón `X` superior derecho | Botón este (`B` / `Círculo`) |
| Navegar entre consejos de la bóveda (`VaultTips`) | Clic en `<` / `>` | — |
| Cancelar selección de ronda o confirmación general | `Esc` o botón de interfaz | Botón este (`B` / `Círculo`) |

La linterna ataca automáticamente al objetivo válido más cercano. Los portales se activan por contacto; el de la bóveda abre la selección de ronda (`ELIGE LA RONDA`) durante la preparación.

`Espacio` se reserva al dash durante el combate y la selección de cartas para evitar confirmaciones accidentales; en el modal de confirmación de liberación de la bóveda (`LIBERAR UNIDAD`), tanto `E` como `Enter` o `Espacio` confirman la liberación y `Esc` la cancela.

Mantener pulsada la tecla **`Q`** en teclado o el botón **`Select`** en mando durante el combate (`CombatHud.IsHudDetailHeld`) despliega temporalmente el panel de estadísticas en modo completo (`COMPLETAS`) y muestra las líneas descriptivas de los poderes de los tres acompañantes equipados aunque estén ocultos o reducidos en la pestaña `Interfaz` del menú de pausa.

El laboratorio de pruebas (`EnemySandboxController`) usa `Tab` para mostrar u ocultar el panel y `R` para reiniciar la escena de prueba.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
