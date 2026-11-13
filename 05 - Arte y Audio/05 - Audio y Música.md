# Audio y música

El juego ya tiene música y efectos integrados. El área sonora acompaña el lugar, los ataques y las acciones del Coleccionista.

| Contexto | Audio presente |
| --- | --- |
| Menú principal | Pista ambiental introductoria con sintetizadores y una dinámica moderada y constante ([Menu.mp3](../Media/Audio/Menu.mp3)). |
| Bóveda | Pista atmosférica propia, orientada a la calma con un matiz de suspenso previo a la incursión; se detiene al salir hacia el combate o al menú ([Boveda_Almacen.mp3](../Media/Audio/Boveda_Almacen.mp3)). |
| Arena | Pista de combate en bucle ([Partida.mp3](../Media/Audio/Partida.mp3)). En el proyecto de Unity el recurso conserva el nombre de prueba `Arena_combat_Test`. |
| Gólgora | Pista de jefe ([Jefe.mp3](../Media/Audio/Jefe.mp3)) con transición desde la arena y reducción de mezcla (*ducking*) durante el rugido. El recurso sigue identificado como prueba en el motor. |
| Linterna | Inicio de ráfaga, tres variantes de pulso, cuarto pulso, descanso, impacto normal y crítico, activación y bucle solar. |
| Experiencia | Sonido de recogida con variación de tono. |
| Transiciones | Entrada del Coleccionista, llegada por portal y rugido al elegir ronda. |
| Fusión | Sonido de apertura de la cámara al revelar el resultado. |
| Jefe | Rugido, pasos, ataques e impactos mediante sus componentes de audio. |

La música de arena y jefe continúa durante la selección de cartas y la pausa manual. La pérdida de foco de la ventana pausa su reproducción. La bóveda tiene su propia mezcla y las transiciones evitan que siga sonando fuera de su contexto.

La pestaña **`Sonido`** del menú de pausa incorpora dos controles independientes persistidos en `PlayerPrefs`: **Volumen general** (`EchoScrap.Audio.MasterVolume`, `0.85` por defecto), que actúa sobre el volumen global de escucha, y **Música** (`EchoScrap.Audio.MusicVolume`, `0.85` por defecto), aplicado sobre las pistas musicales por `ArenaMusicController` y `CombatMusicPlayer`.

## Música durante el juego

### Menú principal

- **Composición y timbre:** tema sintético con arreglos lentos en tonalidad menor y *pads* oscuros que aportan una atmósfera sombría, incorporando un sintetizador con sutil desafinación (*pitch drift*) para emular el comportamiento orgánico de instrumentos analógicos.
- **Estructura y loop:** bucle continuo de baja actividad rítmica diseñado para evitar fatiga auditiva durante estancias prolongadas en los menús.
- **Género, carácter y métrica:** electrónica de carácter melancólico a `75 BPM` en compás de `4/4`, manteniendo un pulso constante y moderado.
- **Muestra de audio:** [Escuchar pista de Menú principal (Menu.mp3)](../Media/Audio/Menu.mp3)

![Pista de Menú principal](../Media/Audio/Menu.mp3)

### Bóveda (Almacén)

- **Composición y timbre:** arreglos pausados en tonalidad menor con reverberación convolutiva amplia y fría que emula la acústica de un hangar o contenedor subterráneo, acompañados de ecos incidentales, *pads* oscuros, bombo, sintetizadores y violín para acentuar el misterio del recinto.
- **Estructura y loop:** bucle continuo con base rítmica marcada que acompaña la preparación y anticipa la entrada hacia la arena.
- **Género, carácter y métrica:** electrónica con sintetizador y cuerdas a `120 BPM` en compás de `4/4`, con un pulso constante que equilibra calma e intriga antes de la expedición.
- **Muestra de audio:** [Escuchar pista de la Bóveda (Boveda_Almacen.mp3)](../Media/Audio/Boveda_Almacen.mp3)

![Pista de la Bóveda](../Media/Audio/Boveda_Almacen.mp3)

### Arena (Partida)

- **Composición y timbre:** tema en tonalidad mayor que inicia con arreglos contenidos y gana densidad rítmica conforme avanza la incursión para sostener la adrenalina del combate, empleando sintetizadores, bombo para marcar el pulso, bajo, platillos, tarola y piano.
- **Estructura y loop:** bucle continuo de alta actividad rítmica con pulso constante y enérgico.
- **Género, carácter y métrica:** electrónica con sintetizador de carácter enérgico a `120 BPM` en compás de `4/4`.
- **Muestra de audio:** [Escuchar pista de Arena (Partida.mp3)](../Media/Audio/Partida.mp3)

![Pista de Arena](../Media/Audio/Partida.mp3)

### Jefe final (Gólgora)

- **Composición y timbre:** arreglos de alta intensidad desde el primer compás con un enfoque amenazador que sostiene la tensión durante todo el enfrentamiento en la caldera, utilizando dos capas de sintetizadores, tarola y bombo para marcar el pulso.
- **Estructura y loop:** bucle continuo de alta actividad rítmica y carácter adrenalínico.
- **Género, carácter y métrica:** electrónica pura con sintetizador de timbre brillante, digital y agresivo a `120 BPM` en compás de `4/4`.
- **Muestra de audio:** [Escuchar pista del Jefe final (Jefe.mp3)](../Media/Audio/Jefe.mp3)

![Pista del Jefe final](../Media/Audio/Jefe.mp3)

## Efectos especiales y diseño sonoro (SFX)

Los efectos de sonido puntuales refuerzan la lectura de ataques enemigos, acciones del Coleccionista, recolección de recursos y estados de interfaz. En `Media/Audio/SFX/` se incluyen las quince muestras exportadas del estudio (`WAV` estéreo, `24 bits`, `48 kHz`):

| Efecto de sonido | Propósito en el videojuego | Descripción acústica | Muestra reproducible |
| --- | --- | --- | --- |
| **Dash del Coleccionista** | Se reproduce cuando el jugador activa el impulso evasivo (`Espacio` / `RT`) a `18 m/s`. | Barrido de aire corto y envolvente rápida (`0,51 s`) que transmite aceleración e invulnerabilidad momentánea. | [Dash.wav](../Media/Audio/SFX/Dash.wav)<br>![SFX Dash](../Media/Audio/SFX/Dash.wav) |
| **Golpe recibido** | Se reproduce cuando un ataque enemigo impacta al Coleccionista y descuenta salud. | Transiente seco y contundente (`0,51 s`) con refuerzo en frecuencias medias-bajas para alertar de inmediato la pérdida de vida. | [Golpe_Recibido.wav](../Media/Audio/SFX/Golpe_Recibido.wav)<br>![SFX Golpe recibido](../Media/Audio/SFX/Golpe_Recibido.wav) |
| **Recogida de coleccionable** | Se reproduce al capturar una entidad coleccionable en la arena. | Timbre brillante y ascendente (`0,71 s`) que confirma la obtención de una nueva copia para la bóveda. | [Coleccionable.wav](../Media/Audio/SFX/Coleccionable.wav)<br>![SFX Coleccionable](../Media/Audio/SFX/Coleccionable.wav) |
| **Curación con botiquín** | Se reproduce al recoger un botiquín soltado por los enemigos derrotados. | Pulso cálido y cristalino (`0,51 s`) que comunica restauración inmediata de vitalidad. | [Curacion_Botiquin.wav](../Media/Audio/SFX/Curacion_Botiquin.wav)<br>![SFX Curación](../Media/Audio/SFX/Curacion_Botiquin.wav) |
| **Subida de nivel** | Se reproduce cuando la barra de experiencia se llena durante la expedición. | Acorde sintético llamativo y expansivo (`0,72 s`) que anuncia la apertura del selector de mejoras. | [Subida_Nivel.wav](../Media/Audio/SFX/Subida_Nivel.wav)<br>![SFX Subida de nivel](../Media/Audio/SFX/Subida_Nivel.wav) |
| **Selección de carta** | Se reproduce al confirmar una carta de mejora en el selector (`UpgradeDraftController`). | Clic tonal breve y definido (`0,36 s`) que valida la elección del jugador antes de reanudar el combate. | [Seleccion_Carta.wav](../Media/Audio/SFX/Seleccion_Carta.wav)<br>![SFX Selección de carta](../Media/Audio/SFX/Seleccion_Carta.wav) |
| **Interceptor (Galgo de chatarra)** | Se reproduce cuando el galgo interceptor ejecuta su mordisco o embestida rápida a corta distancia. | Gruñido metálico y corte agresivo (`0,51 s`) que advierte la cercanía del perseguidor veloz de la horda. | [Galgo.wav](../Media/Audio/SFX/Galgo.wav)<br>![SFX Galgo](../Media/Audio/SFX/Galgo.wav) |
| **Mortero Campana** | Se reproduce cuando el Mortero Campana culmina su carga y dispara el proyectil parabólico de ácido. | Disparo neumático resonante (`0,86 s`) con timbre hueco de campana metálica que marca el lanzamiento a distancia. | [Mortero.wav](../Media/Audio/SFX/Mortero.wav)<br>![SFX Mortero](../Media/Audio/SFX/Mortero.wav) |
| **Polilla Reactor (aviso y explosión)** | Se reproduce cuando la Polilla Reactor entra en el radio de `1,6 m`, arma su espoleta y detona tras `0,75 s`. | Secuencia en dos fases (`1,99 s`): pitido acelerado de cebado que da margen para esquivar con *dash*, seguido del estallido térmico. | [Polilla_Aviso_Explosion.wav](../Media/Audio/SFX/Polilla_Aviso_Explosion.wav)<br>![SFX Polilla](../Media/Audio/SFX/Polilla_Aviso_Explosion.wav) |
| **Caballero Fantasma (barrido)** | Se reproduce cuando el élite Caballero Fantasma prepara (`0,8 s`) y usa el barrido de su espada. | Carga espectral seguida de un corte amplio y pesado (`1,99 s`) que delimita la ventana de peligro del ataque cuerpo a cuerpo. | [Barrido_Caballero.wav](../Media/Audio/SFX/Barrido_Caballero.wav)<br>![SFX Barrido Caballero](../Media/Audio/SFX/Barrido_Caballero.wav) |
| **Centinela de Cobre (apertura y disparo)** | Se reproduce cuando el gólem élite abre sus compuertas (`0,3 s`), prepara (`0,7 s`) y lanza su ataque de misiles. | Mecanismo metálico de apertura articulado con la descarga de artillería (`1,51 s`), señalizando además el inicio de su ventana vulnerable. | [Centinela_Apertura_Disparo.wav](../Media/Audio/SFX/Centinela_Apertura_Disparo.wav)<br>![SFX Centinela](../Media/Audio/SFX/Centinela_Apertura_Disparo.wav) |
| **Impacto de Gólgora** | Se reproduce cuando el jefe final Gólgora descarga sus golpes pesados contra el suelo de la caldera. | Impacto grave y contundente (`0,61 s`) con resonancia mineral que subraya la escala colosal del jefe. | [Impacto_Golgora.wav](../Media/Audio/SFX/Impacto_Golgora.wav)<br>![SFX Impacto Gólgora](../Media/Audio/SFX/Impacto_Golgora.wav) |
| **Señal de derrota** | Se reproduce cuando la salud del Coleccionista llega a cero (`PlayerHealth.IsDead`) y concluye la partida. | Secuencia tonal descendente en modo menor (`3,37 s`) con textura melancólica que rubrica la caída en combate antes de volver a la bóveda. | [Senal_Derrota.wav](../Media/Audio/SFX/Senal_Derrota.wav)<br>![SFX Señal de derrota](../Media/Audio/SFX/Senal_Derrota.wav) |
| **Señal de victoria** | Se reproduce al derrotar a Gólgora en la caldera central (`GolgoraActor.IsDead`), consolidando la victoria de la expedición. | Fanfarria en modo mayor con timbre brillante y festivo (`3,12 s`) que celebra la culminación exitosa de la ronda. | [Senal_Victoria.wav](../Media/Audio/SFX/Senal_Victoria.wav)<br>![SFX Señal de victoria](../Media/Audio/SFX/Senal_Victoria.wav) |
| **Fusión y resultado** | Se reproduce en la máquina de fusión al completarse la cuenta atrás de `30 s` (`00:30`) y desplegar la entidad resultante en pantalla. | Atmósfera musical envolvente (`3,28 s`) con acordes de campana y sintetizador que acompaña la recompensa y el balance final. | [Fusion_Resultado.wav](../Media/Audio/SFX/Fusion_Resultado.wav)<br>![SFX Fusión y resultado](../Media/Audio/SFX/Fusion_Resultado.wav) |

## Elaboración e instrumentación digital

Las pistas musicales y temas de ambientación fueron producidos en estación de trabajo de audio digital (DAW) combinando secuenciación MIDI, síntesis sustractiva/*hypersaw*, percusión acústica e instrumentación orquestal virtual.

### Instrumentación utilizada

- **Cuerdas y orquesta:** Chelo, Violín y Corno francés (mediante librerías orquestales virtuales como *Spitfire Audio – BBC Symphony Orchestra*).
- **Base rítmica y percusión:** Batería completa, bombo acústico/electrónico, tarola, platillos y campanas metálicas afinadas.
- **Armonía y bajos:** Teclado MIDI con sintetizadores virtuales (`JUNO-106`, `UJAM Usynth DETUNICORN`), bajo eléctrico/sintético y piano.

### Tonalidades y notas de referencia

La paleta armónica se estructura sobre centros tonales menores para transmitir misterio y peligro, alternando con modos mayores para momentos de recompensa y combate directo:

- **Tonalidades y acordes principales:** Do menor (`Cm`), Sol menor (`Gm`), Re menor (`Dm`) y La menor (`Am`), complementados por progresiones hacia Fa mayor (`F`) y Si bemol mayor (`Bb`).

![Sesión multipista de la banda sonora en el DAW|520](../Media/Audio/Produccion_Multipista_Reaper.png)

*Estructura multipista y mezcla de canales de la banda sonora.*

![Edición MIDI e instrumentos digitales (JUNO-106)|520](../Media/Audio/Edicion_MIDI_Juno106.png)

*Secuenciación melódica en piano roll con sintetizador virtual `JUNO-106`.*

![Configuración de sintetizador virtual UJAM Usynth DETUNICORN|520](../Media/Audio/Sintetizador_UJAM_Detunicorn.png)

*Diseño sonoro y ajuste de desafinación (`Detune`) y frecuencias con el sintetizador `UJAM Usynth DETUNICORN`.*

![Instrumento virtual orquestal Spitfire Audio – BBC Symphony Orchestra|520](../Media/Audio/Orquesta_Spitfire_BBC.png)

*Instrumento virtual `Spitfire Audio – BBC Symphony Orchestra` utilizado para incorporar las cuerdas y violines en la ambientación de la Bóveda.*

## Procedencia y autoría de recursos sonoros

Para dar cumplimiento estricto al criterio de evaluación de audio y licencias, la siguiente tabla distingue los recursos producidos directamente por el equipo de aquellos obtenidos o derivados de bibliotecas externas:

| Recurso / Contexto                                         | Tipo   | Origen y autoría                                                                                                                                                                                                                                                                                 | Licencia / Condiciones                                          | Tratamiento técnico                                                                                                |
| ---------------------------------------------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| **Menú principal** (`Menu.mp3`)                            | Música | **Composición original del equipo EchoScrap** (secuenciado y mezclado en DAW).                                                                                                                                                                                                                   | Obra propia (propiedad del equipo).                             | Estéreo, 320 kbps, 48 kHz. Sintetizadores virtuales y *pitch drift*.                                               |
| **Bóveda (Almacén)** (`Boveda_Almacen.mp3`)                | Música | **Composición original del equipo EchoScrap**.                                                                                                                                                                                                                                                   | Obra propia (propiedad del equipo).                             | Estéreo, 320 kbps, 48 kHz. Instrumentos orquestales Spitfire BBC y sintetizadores.                                 |
| **Arena** (`Partida.mp3` / `Arena_combat_Test`)            | Música | **Composición original del equipo EchoScrap**.                                                                                                                                                                                                                                                   | Obra propia (propiedad del equipo).                             | Estéreo, 320 kbps, 48 kHz. Bucle continuo de percusión y sintetizadores.                                           |
| **Gólgora** (`Jefe.mp3`)                                   | Música | **Composición original del equipo EchoScrap**.                                                                                                                                                                                                                                                   | Obra propia (propiedad del equipo).                             | Estéreo, 320 kbps, 48 kHz. Síntesis pura y percusión de combate.                                                   |
| **Linterna (base y ráfagas)**                              | SFX    | Muestras externas de microondas (`pinto0lucas`) y contador Geiger (`cookies+policy`).                                                                                                                                                                                                            | Licencia **CC0 (Dominio Público)** registrada en `Sources.txt`. | Conversión a mono, 44,1 kHz, 16 bits; selección de clics, capas, envolventes y tonos sintetizados.                 |
| **Linterna (impactos y Rayo solar)**                       | SFX    | Diseño sonoro propio del equipo mediante capas sintetizadas.                                                                                                                                                                                                                                     | Obra propia.                                                    | Mono, 44,1 kHz, 16 bits. Modulación y envolventes reactivas al impacto.                                            |
| **Combate, enemigos e interfaz (`Media/Audio/SFX/*.wav`)** | SFX    | **Diseño sonoro y procesamiento propio del equipo EchoScrap** (Dash, Golpe recibido, Coleccionable, Botiquín, Subida de nivel, Selección de carta, Galgo, Mortero, Polilla, Caballero Fantasma, Centinela de Cobre, Impacto de Gólgora, Señal de derrota, Señal de victoria y Fusión/resultado). | Obra propia / procesamiento sobre muestras CC0 y sintetizadas.  | WAV estéreo, 24 bits, 48 kHz. Chelo, bajo, campana, sintetizadores, batería, corno francés, violín y teclado MIDI. |
| **Experiencia y gemas**                                    | SFX    | Síntesis procedural/frecuencial del equipo.                                                                                                                                                                                                                                                      | Obra propia.                                                    | Mono, modulación de tono progresiva según racha de recogida.                                                       |
| **Fusión (apertura de cámara)**                            | SFX    | Muestra derivada de mecanismos metálicos / neumáticos.                                                                                                                                                                                                                                           | Licencia libre / CC0.                                           | Procesamiento de graves, capas de despresurización y reverberación.                                                |
| **Gólgora y enemigos (rugido, pasos, impactos en motor)**  | SFX    | Componentes de audio de Unity y capas de impacto procesadas.                                                                                                                                                                                                                                     | Mezcla propia sobre librerías CC0 / Dominio público.            | Atenuación espacial 3D, balance de volumen y mezcla dinámica (*ducking*).                                          |

La integración de una pista no implica que sea una composición final. Los nombres de prueba de arena y jefe se mantienen identificados como tales. La autoría y los recursos de cada incorporación deben conservarse en su registro de fuentes.

Los GIFs de este GDD no contienen audio y sirven para revisar animación y VFX. Como excepción documental, esta sección incluye las cuatro pistas musicales del juego (`Menu.mp3`, `Boveda_Almacen.mp3`, `Partida.mp3` y `Jefe.mp3`) y las quince muestras de efectos de sonido y avisos (`Media/Audio/SFX/*.wav`); su mezcla dinámica y espacialización se comprueban directamente en el juego.

> **Navegación:** [[00 - Índice]] · [[00 - Avance jugable]]
