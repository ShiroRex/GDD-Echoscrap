# Controles

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
| Recoger una entidad cercana en la arena | `E` (si hay hueco en `10/10`) | Sin asignación equivalente en el sistema actual |
| Abrir / cerrar el Índice de Colección en la bóveda | `I` o clic en `ÍNDICE · [I]` (cierra también con `Esc`) | Sin asignación equivalente |
| Equipar o editar un puesto de equipo (puestos 1 a 3, balcón) | `E` (`E · EQUIPAR` / `E · EDITAR`) | Sin asignación equivalente |
| Desequipar directamente un puesto de equipo ocupado en el mundo (puestos 1 a 3) | `T` | Sin asignación equivalente |
| Liberar una unidad expuesta (puestos 4 a 10, planta baja) | `E` (`E · LIBERAR`); confirmar en modal con `E`, `Enter` o `Espacio` (`Esc` cancela) | Sin asignación equivalente |
| Abrir la máquina de fusión en la bóveda | `E` (`E · FUSIONAR` / `E · VER FUSIÓN`) | Sin asignación equivalente |
| Cerrar selector de equipo (`CompanionView`) | `E`, `Esc` o botón `CERRAR · E / ESC` | — |
| Cerrar ventana de fusión | `E`, `Esc` o botón `X` superior derecho | — |
| Navegar entre consejos de la bóveda (`VaultTips`) | Clic en `<` / `>` | — |
| Cancelar selección de ronda o confirmación general | `Esc` o botón de interfaz | Botón este (`B` / `Círculo`) |

La linterna ataca automáticamente al objetivo válido más cercano. Los portales se activan por contacto; el de la bóveda abre la selección de ronda (`ELIGE LA RONDA`) durante la preparación.

`Espacio` se reserva al dash durante el combate y la selección de cartas para evitar confirmaciones accidentales; en el modal de confirmación de liberación de la bóveda (`LIBERAR UNIDAD`), tanto `E` como `Enter` o `Espacio` confirman la liberación y `Esc` la cancela.

Mantener pulsada la tecla **`Q`** en teclado o el botón **`Select`** en mando durante el combate (`CombatHud.IsHudDetailHeld`) despliega temporalmente el panel de estadísticas en modo completo (`COMPLETAS`) y muestra las líneas descriptivas de los poderes de los tres acompañantes equipados aunque estén ocultos o reducidos en la pestaña `Interfaz` del menú de pausa.

El laboratorio de pruebas (`EnemySandboxController`) usa `Tab` para mostrar u ocultar el panel y `R` para reiniciar la escena de prueba.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
