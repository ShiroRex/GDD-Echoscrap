# Dirección de Arte

## Descripción de la sección

Define la identidad visual del videojuego y las reglas que deben seguir los recursos gráficos.

## Identidad visual

**Estilo:** Estilo híbrido 2.5D con inclinación al *HD-2D*: escenario en 3D *low-poly* de geometría limpia combinado con *sprites* 2D en *pixel art* proyectados bajo una cámara cenital a 60°.

**Referencias:** _Vampire Survivors_, _Risk of Rain 1 / Returns_, _Hyper Light Drifter_ y _Enter the Gungeon_.

**Sensación buscada:** Desolación, misterio, degradación y la atmósfera de mundos en ruinas y magia oscura provenientes de diferentes épocas.

**Nivel de realismo:** Estilizado / *Pixel Art* retro integrado sobre entorno 3D.

**Formas predominantes:** Formas angulares, ruinas fracturadas y siluetas orgánicas bien diferenciadas para transmitir que el entorno es hostil, peligroso e inhóspito sin perder legibilidad en combate.

**Iluminación:** Contraste alto. Iluminación oscura en los bordes de la pantalla con fuentes de luz direccionales clave (como el cono de la linterna del jugador, los destellos de las habilidades o las señales de ataque enemigas) para enfocar la atención en el centro de la acción.

**Materiales:** En el escenario predominan el metal oxidado, la chatarra abandonada, contenedores industriales y roca fracturada para dar la idea de un vertedero dimensional. Para las habilidades, proyectiles y la linterna se emplean efectos de energía y luz brillante que resaltan nítidamente sobre la penumbra del mapa.

## Paleta de color (Apollo Palette)

La paleta Apollo cuenta con 56 colores. Para mantener una lectura clara en pantalla y evitar que los elementos se confundan durante las oleadas, se dividen en 3 grupos según su función:

**Colores principales (Ambientación y fondo):** Se utilizarán los tonos más oscuros y desaturados —grises fríos, azules oscuros y morados profundos— para el suelo, los muros perimetrales y las ruinas del Vertedero de Ecos:

- **Noche / Sombras profundas:** `#090a14`, `#10141f`, `#172038`, `#1e1d39`
- **Estructuras / Terreno:** `#202e37`, `#394a50`, `#577277`, `#402751`

**Colores secundarios (Elementos de interés y entorno):** Colores tierra, verdes desaturados y magentas oscuros para los restos orgánicos y acumulaciones de chatarra:

- **Biomasa y corrosión:** `#25562e`, `#468232`, `#752438`, `#a53030`
- **Chatarra y ruinas:** `#4d2b32`, `#7a4841`, `#ad7757`, `#884b2b`

**Colores para información jugable:** Tonos de alto brillo y saturación que resaltan de inmediato sobre el fondo oscuro para indicar la cobertura de la linterna, ataques o recompensas:

- **Linterna del Jugador / Energía FX:** `#a4dddb`, `#73bed3`, `#4f8fba`, `#ebede9`
- **Ataques Enemigos / Peligro:** `#cf573c`, `#da863e`, `#e8c170`

## Reglas visuales

1. **Jerarquía por contraste y brillo:** El escenario 3D siempre debe usar los tonos más oscuros y desaturados de la paleta. Los *sprites* 2D del jugador, enemigos, orbes de experiencia y proyectiles deben usar los tonos medios y altos para que nunca se pierdan en pantalla.
2. **Sin *outlines* negros puros:** El delineado (*outline*) de los *sprites* no usará el negro absoluto `#000000`. En su lugar, se utilizará el tono más oscuro de la paleta Apollo (`#090a14` o `#172038`) para mantener la armonía cromática en todo el juego.
3. **Coherencia de escala y PPU (*Pixels Per Unit*):** Los *sprites* 2D de personajes y enemigos estándar se trabajarán sobre una base constante de $64 \times 64\text{ px}$ (escalando proporcionalmente en jefes como Gólgora) con filtrado *Point (no filter)* para evitar deformaciones o discrepancias en el tamaño del píxel.

> **Navegación:** [[00 - Índice]] · ← [[05 - Misiones y Desafíos]] · [[02 - Personajes y Animación]] →
