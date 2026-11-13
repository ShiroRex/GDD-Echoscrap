# Accesibilidad

El menú de pausa (`Esc` / `Start`) organiza las opciones de confort visual, lectura y audio en tres pestañas dedicadas (`CombatHud.cs:631-756`):

- **Pestaña `General`:** deslizadores independientes para **`Movimiento de cámara`** (`0%` a `100%`, `ScreenShakeScale`) y **`Efectos decorativos`** (`0%` a `100%`, `VfxDensityScale`, que activa automáticamente el modo de efectos reducidos por debajo de `99,9%` conservando íntegros los avisos de ataque y peligro).
- **Pestaña `Interfaz`:** selector **`Estadísticas generales`** (**`COMPLETAS`**, **`REDUCIDAS`**, **`OCULTAS`**) y selector **`Atributos de coleccionables`** (**`SÍ`** / **`NO`**, `ShowCompanionPowers`, para mostrar u ocultar las descripciones de los poderes bajo los 3 retratos del HUD). Además, mantener pulsado **`Q`** (teclado) o **`Select`** (mando) en combate despliega bajo demanda tanto las estadísticas completas como los atributos de coleccionables.
- **Pestaña `Sonido`:** deslizadores independientes para **`Volumen general`** (`0%` a `100%`, `EchoScrap.Audio.MasterVolume`) y **`Música`** (`0%` a `100%`, `EchoScrap.Audio.MusicVolume`).

La pausa y la pérdida de foco de ventana bloquean el control de combate y exigen soltar las entradas activas al regresar para impedir confirmaciones o dashes accidentales.

## Lectura, orientación espacial y formatos comprobados

| Comprobación | Evidencia |
| --- | --- |
| `1920 × 1080`, `16:9` | Verificado en runtime con la tipografía unificada **`Pixelify Sans`**. Incluye HUD de arena y jefe, cartas, menú de pausa en 3 pestañas, señalización de bóveda, Índice de Colección (`[I]`) y pantallas de resultado. |
| Escala de la interfaz | HUD y menús usan un lienzo de referencia de `1920 × 1080`. Los números flotantes de daño usan `1280 × 720` como referencia de escala. |
| Señalización en la bóveda | Implementada mediante rótulos 3D (`EQUIPO 1..3`), tarjetas direccionales superiores con flechas en vivo (`VaultDirections`), panel inferior de consejos rotatorios cada `8 s` (`VaultTips`) y botón `ÍNDICE · [I]`. |
| Señales redundantes | El daño crítico combina tamaño, color y signo de exclamación (`!`). Las resistencias muestran `Resistido` y las ejecuciones `EJECUTADO`. En el Índice de Colección (`[I]`), las entidades no descubiertas combinan silueta negra, nombre `???` y etiqueta `NO ENCONTRADO` / `SILUETA BLOQUEADA`. |
| Otros tamaños | `1280 × 720`, `2560 × 1440` y relaciones de aspecto distintas de `16:9` se mantienen como comprobaciones adicionales de escalado. |

![Pantalla de configuración y pausa en tres pestañas](../Media/Interfaz/Menu_Pausa.png)
*Menú de Pausa (`Esc` / `Start`) en la versión definitiva de Unity, organizado en las tres pestañas `General`, `Interfaz` y `Sonido` con sus deslizadores accesibles.*

## Aspectos pendientes de dispositivo de entrada

El combate, el menú de pausa, la selección de cartas y la consulta rápida del HUD (`Select`) cuentan con soporte completo de mando. En la versión actual (`9eed59f`), la recogida manual de fragmentos en la arena y las interacciones directas con los pedestales, la máquina de fusión (`E`, `T`) y el Índice de Colección (`I`) se operan mediante teclado y ratón.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
