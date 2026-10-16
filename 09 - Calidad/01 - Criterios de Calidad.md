# Criterios de Calidad

## Descripción de la sección

Define los estándares funcionales, de experiencia de usuario y técnicos que debe cumplir cada característica de *EchoScrap* para considerarse terminada.

## Calidad funcional

- **Fidelidad a las reglas del GDD:** El ciclo de la linterna ($4$ pulsos en $0.8\text{ s}$ + $0.4\text{ s}$ de descanso), el autoapuntado 360°, las 18 estadísticas base y los topes defensivos (Armadura $60\%$, Esquiva $35\%$, Bloqueo $50\%$) operan exactamente con los valores documentados.
- **Manejo de casos límite:** Las mejoras elegidas en pausa se aplican al inicio del siguiente ciclo completo; el excedente de XP se conserva al subir varios niveles seguidos; y cuando se agotan las cartas elegibles, el juego permite continuar sin bloquear la partida.
- **Condiciones de cierre claras:** La Ruleta de Extracción se activa única y exclusivamente al derrotar a Gólgora (Victoria), sumando $+1$ nivel si la entidad obtenida es un duplicado.

## Calidad de experiencia (UX / UI)

- **Claridad visual en combate:** Los ataques telegrafiados (círculo de $5\text{ m}$ del pisotón de Gólgora, charcos del Mortero y parpadeo ámbar de $0.5\text{ s}$ de la Polilla) se distinguen con nitidez sobre el suelo oscuro de la paleta Apollo.
- **Respuesta inmediata de control:** El *Dash* ($22\text{ m/s}$ durante $0.15\text{ s}$) y la ventana de gracia por daño ($0.40\text{ s}$) responden al instante con señales visuales y auditivas claras.
- **Navegación sin fricción:** Todo el flujo de menús, el equipamiento de hasta 3 entidades en el Almacén y la selección de las 3 cartas al subir de nivel pueden operarse íntegramente con teclado (`WASD` / `1, 2, 3`), mando o ratón.

## Calidad técnica

- **Rendimiento objetivo:** $60\text{ FPS}$ constantes a $1920 \times 1080$ con $150$ enemigos simultáneos en pantalla (soportando picos de $250$ mediante reutilización en memoria / *Object Pooling*).
- **Persistencia segura:** Escritura atómica del archivo de guardado JSON (con respaldo `.bak`) que preserva las entidades desbloqueadas, sus niveles y las 3 ranuras equipadas sin corrupción de datos.
- **Estabilidad:** Cero cierres inesperados (*crashes*), fugas de memoria o bloqueos de estado entre el Almacén y la Arena.

## Definición de terminado (*Definition of Done*)

Una característica se considera terminada cuando cumple las reglas numéricas del GDD, supera sus casos de prueba asociados en [[02 - Pruebas]], mantiene los $60\text{ FPS}$ objetivo y no introduce regresiones visuales ni de guardado.

> **Navegación:** [[00 - Índice]] · ← [[05 - Gestión de Cambios]] · [[02 - Pruebas]] →
