# Mecánicas

## Movimiento y dash

El movimiento parte de 8 m/s, con aceleración de 80 m/s² y frenado de 70 m/s². La diagonal mantiene la misma velocidad máxima. La dirección de entrada se interpreta respecto a la cámara y el personaje se apoya sobre el relieve y las escaleras del escenario.

El dash avanza a 22 m/s durante 0,15 s, aproximadamente 3,3 m si el recorrido está libre. Concede invulnerabilidad durante ese intervalo y atraviesa cuerpos enemigos. Los muros y obstáculos sólidos limitan el recorrido. La recarga base es de un segundo desde el final.

## Linterna

| Parámetro base | Valor |
| --- | ---: |
| Alcance | 7 m |
| Apertura del cono | 60° |
| Daño de cada pulso | 3,75 sobrenatural |
| Pulsos por ráfaga | 4 |
| Separación entre pulsos | 0,2 s |
| Descanso entre ráfagas | 0,4 s |
| Impulso del cuarto pulso | 10 m/s, modulado por la susceptibilidad del enemigo |

La ráfaga base emite en 0,2, 0,4, 0,6 y 0,8 s y vuelve a empezar tras el descanso. La linterna busca objetivos en 360° dentro de su alcance y aplica los impactos a su cono frontal. Sin objetivos, vuelve hacia la orientación de movimiento. Rayo solar elimina el descanso, aumenta el alcance base a 14 m, multiplica el daño por 2,5 y aplica empuje en cada pulso del láser. Conserva la cadencia de pulsos y la susceptibilidad al empuje de cada enemigo; los cuerpos inmunes siguen sin desplazarse.

El alcance y el ángulo se evalúan en el plano horizontal X/Z y tienen en cuenta el radio del objetivo. Tanto el autoapuntado como cada impacto trazan una línea 3D desde la luz hasta el centro del enemigo. Los obstáculos sólidos de la capa Default pueden bloquearla aunque el enemigo esté dentro del cono. El trazado ignora los triggers. Cada enemigo válido recibe como máximo un impacto por pulso.

Al comenzar la ráfaga se fijan alcance, apertura, daño base por pulso, intervalo, descanso, empuje, probabilidad y multiplicador críticos y modo solar. Una mejora que cambie esos parámetros durante la secuencia se refleja al comenzar la siguiente. La dirección de apuntado y la visibilidad de cada objetivo se vuelven a evaluar durante la ráfaga.

## Salud y daño

El Coleccionista empieza con 100 HP y 0,4 s de protección después de recibir daño. El daño puede ser físico o sobrenatural según su fuente; los números conservan esa categoría. La armadura reduce el daño recibido hasta un límite del 60%. La probabilidad crítica empieza en 0% y su multiplicador base es ×1,75.

El daño entrante verifica invulnerabilidad y la protección de entidades. Después sortea evasión, aplica reducción de daño por entidades, sortea bloqueo y aplica armadura. Evasión y bloqueo parten de 0%, con topes técnicos de 35% y 50%. Un bloqueo reduce el daño un 50% antes de la armadura. Blindaje concede hasta 20 puntos porcentuales de armadura con cinco cartas, dentro del tope del sistema de 60%.

| Estado del enemigo | Aplicación y expiración                                                                                                                                                                                                                                                                  |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Ralentización      | Freón se aplica antes de contar el mismo pulso para Cero absoluto. Cada aplicación conserva la intensidad y el tiempo mayores. Al expirar se pierden los impactos de frío acumulados.                                                                                                    |
| Congelación        | Cero absoluto solo cuenta si hay ralentización activa y el objetivo sigue vivo y sin congelar. Congelar reinicia ese contador y conserva la duración mayor. Ventisca y Quiebre de hielo pueden congelar directamente. El objetivo congelado recibe ×1,30 de daño de ataques del jugador. |
| Aturdimiento       | Conserva el tiempo restante mayor. Igual que la congelación, bloquea el control del enemigo mientras el temporizador esté activo.                                                                                                                                                        |
| Empuje             | Se resuelve según susceptibilidad y frenado de cada arquetipo. Las excepciones y ventanas de inmunidad de Gólgora se especifican en [[03 - Enemigos y NPC]].                                                                                                                             |

Los números muestran la salud realmente perdida. Los impactos normales de una ráfaga pueden agruparse, mientras que los críticos se distinguen con tamaño, color y un signo de exclamación. Los impactos mitigados por resistencia muestran «Resistido» bajo el valor y una ejecución causada por la mejora correspondiente muestra «EJECUTADO».

## Recogida

El imán base alcanza 2,5 m y atrae los objetos a 12 m/s. Las gemas conceden experiencia; el botiquín base recupera 15 HP. El ácido y las áreas de ataque utilizan las mismas reglas de salud e invulnerabilidad que el resto del combate. Las zonas de lava fija del mapa están desactivadas; el magma de Gólgora sigue formando parte del encuentro.

Una gema nueva puede sumarse a una gema activa a un máximo de 0,75 m de distancia horizontal. Se acumulan la XP y el número de gemas originales. Micro-ensamblador cuenta las gemas originales del objeto recogido. La XP otorgada es la parte entera de `XP acumulada del objeto × multiplicador de XP + resto anterior`. El resto se conserva para la próxima recogida de la expedición. Chatarrero añade 15% de XP por nivel.

Las mejoras modifican estos parámetros o añaden acciones automáticas. Sus niveles y efectos se describen en [[05 - Progresión]].

## Entidades y poderes de inicio

Hasta tres copias equipadas activan poderes propios, independientes de las 35 cartas: bonos de estadísticas, ataques, curación, protección o salvación. Repetir una entidad en varios puestos no acumula su efecto. Las cartas mantienen sus niveles y requisitos y no reciben niveles iniciales por equipo. El catálogo, las combinaciones y los temporizadores están en [[04 - Objetos y Coleccionables]].

Los coleccionables que aparecen en arena requieren E dentro de unos 2,5 m y una línea de interacción despejada. No se recogen con el imán de las gemas y botiquines.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
