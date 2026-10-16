# Entornos

## Descripción de la sección

*EchoScrap* utiliza un estilo **2.5D con cámara cenital a 60°**: los entornos y límites están construidos mediante modelos y geometría 3D, mientras que el protagonista, los enemigos y los coleccionables son *sprites* 2D en *pixel art*. Para mantener la cohesión visual, tanto el diseño del entorno como el de los personajes se basan en la **paleta de colores Apollo**, conservando el contraste necesario para leer el combate con claridad.

---

## Zonas

| Zona                   | Estilo                                                                                      | Elementos                                                                                                                                             | Función                                                                                                                                                                                                |
| :--------------------- | :------------------------------------------------------------------------------------------ | :---------------------------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Lobby / Almacén** | Subterráneo industrial en penumbra.                                                         | • Vitrinas y cápsulas de contención.<br>• Terminales y pantallas de control.                                                                          | Zona segura (Hub) fuera de combate para consultar las fichas de los 9 coleccionables, ver su nivel y equipar hasta 3 entidades antes de iniciar la partida.                                           |
| **2. Arena / Mapa**    | Vertedero dimensional e industrial en ruinas (restos de épocas colapsadas bajo paleta Apollo). | • Suelo agrietado de asfalto y tierra.<br>• Luminarias de poste y fuegos/antorchas.<br>• Muros perimetrales de contenedores oxidados, vallas y chatarra.<br>• Detalles bajos de escombros en suelo. | Espacio principal de combate (120 × 120 m). Área amplia cerrada por muros perimetrales físicos, pensada para moverse, usar el dash y combatir oleadas de 150 a 250 enemigos sin obstruir la cámara a 60°. |

---

## Modularidad

¿Qué elementos pueden reutilizarse?

- **Suelos:** Módulos cuadrados de losa de asfalto agrietado y tierra/chatarra que se conectan entre sí para conformar la superficie de 120 × 120 m.
- **Muros perimetrales (Límites de la arena):** Contenedores oxidados, vallas metálicas y montículos altos de chatarra colocados en el borde exterior para impedir físicamente salir del mapa sin bloquear la cámara cenital.
- **Detalles ambientales de chatarra y escombros:** Piezas y placas metálicas bajas distribuidas para dar variedad visual al suelo sin atascar el paso de la horda ni bloquear el cono de luz.
- **Luminarias y antorchas:** Lámparas de poste y fuentes de fuego repartidas en puntos de referencia para aportar iluminación fija y guiar la orientación espacial del jugador.
- **Equipo del Almacén:** Piezas fijas que arman la estación de control y la zona segura del lobby, como los monitores, vitrinas y pantallas de interacción para consultar y equipar coleccionables.

---

## Iluminación

La luz cumple dos metas: crear un ambiente sombrío y melancólico, pero sin quitarle claridad a la pantalla para que siempre identifiques por dónde vas y qué te está atacando:

- **Luz ambiental general:** Es tenue y en tonos fríos/oscuros de la paleta Apollo. El mapa nunca queda a oscuras por completo, garantizando la lectura de las siluetas enemigas.
- **Linterna del jugador:** Es la luz del arma principal del juego. Se proyecta directamente sobre el suelo con un cono bien delimitado (7 m base / 14 m en modo solar) para que el jugador reconozca con exactitud qué área está iluminando y atacando.
- **Puntos de luz en el mapa:** Las luminarias de poste y las antorchas/fuegos dispersos sirven como referencias visuales para no perder el sentido de orientación dentro de la arena de 120 × 120 m.
- **Destellos de peligro y telégrafos:** Los ataques de área (como el círculo de advertencia del pisotón de Gólgora, los charcos del Mortero y el parpadeo de la Polilla) y los impactos destacan con alto contraste sobre el suelo para advertir el peligro de un vistazo.

---

## Efectos visuales

Los efectos visuales son moderados y limpios para no saturar la pantalla ni afectar los 60 FPS cuando se reúnan entre 150 y 250 enemigos en combate:

- **Ambiente y clima:**
  - *Polvo:* Partículas sutiles flotando cerca del suelo para que la arena no se sienta estática ni vacía.
  - *Bruma estética a ras de suelo:* Una capa visual muy ligera que reacciona al paso de la linterna y aporta profundidad sin ocultar enemigos ni hacer daño.
  - *Humo:* Saliendo de escombros perimetrales, tuberías o zonas con fuego/antorchas.
- **Combate e impactos:**
  - *Flash de impacto:* Parpadeo blanco breve en los enemigos al recibir daño de los pulsos de la linterna para confirmar el golpe.
  - *Dash:* Estela de polvo a ras de piso para remarcar la dirección y duración de la evasión.
  - *Explosiones:* Breves y con onda expansiva bien delimitada al detonar minas, Polillas o el pisotón de Gólgora.

---

## Recursos requeridos

Lista de los *assets* necesarios para los dos escenarios (Almacén y Arena):

- **Modelos 3D:**
  - **Pisos:** Módulo de losa de asfalto agrietado y losa de tierra/chatarra para armar el suelo de 120 × 120 m.
  - **Límites perimetrales:** Muros de chatarra, vallas y contenedores oxidados para cerrar los bordes del mapa.
  - **Elementos de ambientación:** Montículos de chatarra y pilas pequeñas de escombros.
  - **Luces del mapa:** Bases de alumbrado para postes de luz y antorchas/fuego.
  - **Almacén:** Vitrinas/cápsulas de contención, terminal de pantallas y monitores de control.
- **Texturas y Materiales:**
  - Texturas ajustadas a la paleta de colores **Apollo**.
  - Material modular para el suelo.
  - Textura/material aditivo para el cono de luz proyectado de la linterna y los círculos de telégrafo en el piso.
- **VFX y Partículas:**
  - Polvo ambiental flotando a nivel del suelo.
  - Efecto de humo y fuego para las luminarias/antorchas.
  - Estela de polvo para el *Dash* del personaje.

> **Navegación:** [[00 - Índice]] · ← [[02 - Personajes y Animación]] · [[04 - Interfaz Visual]] →
