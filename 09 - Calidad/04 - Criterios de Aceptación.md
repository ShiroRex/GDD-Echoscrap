# Criterios de Aceptación

## Descripción de la sección

Los criterios de aceptación permiten verificar objetivamente si los sistemas principales de *EchoScrap* cumplen con el diseño acordado en el GDD.

## Escenarios de aceptación (*Dado / Cuando / Entonces*)

### 1. Preparación en el Almacén y equipamiento de Entidades
- **Dado que** el jugador se encuentra en la interfaz del Almacén y posee Entidades Interdimensionales desbloqueadas en su colección,
- **Cuando** selecciona sus vitrinas antes de iniciar una expedición,
- **Entonces** el sistema le permite equipar **hasta un máximo de 3 Entidades activas**, aplicando sus bonificaciones pasivas (escaladas según el nivel de cada entidad) al comenzar la incursión en la arena.

### 2. Combate con Linterna Táctica y ciclo de 4 pulsos
- **Dado que** el Coleccionista se desplaza por la arena de $120 \times 120\text{ m}$ frente a una horda de enemigos,
- **Cuando** existen enemigos vivos dentro del alcance de la linterna ($7.0\text{ m}$ base),
- **Entonces** el cono rota automáticamente en 360° hacia el enemigo más cercano y emite 4 pulsos de daño en $0.8\text{ s}$ seguidos de $0.4\text{ s}$ de descanso, aplicando retroceso en el 4.º impacto a todos los enemigos vulnerables sin detener el movimiento del jugador.

### 3. Subida de nivel y selección pausada de mejoras
- **Dado que** el jugador absorbe suficientes Ecos (Gemas de XP) para alcanzar el umbral de nivel,
- **Cuando** se llena la barra de experiencia,
- **Entonces** la simulación de combate se congela por completo, se conserva el excedente de XP y se despliegan hasta 3 cartas elegibles del catálogo de 35 mejoras; la mejora elegida entra en vigor en el siguiente ciclo completo de ataque.

### 4. Clímax en Fase 5 (Gólgora) e invocación de esbirros
- **Dado que** el reloj de la partida alcanza el minuto `08:00`,
- **Cuando** emerge Gólgora (*El Guardián*, $1500\text{ HP}$),
- **Entonces** se detiene el escalado normal de oleadas, una onda de choque limpia a los enemigos menores previos sin soltar XP, y Gólgora persigue al jugador ejecutando su pisotón de magma, bombardeo de lava e **invocando esbirros cada $4.0\text{ s}$** hasta que uno de los dos cae derrotado.

### 5. Victoria, Ruleta de Extracción y duplicados
- **Dado que** el jugador reduce a $0\text{ HP}$ la salud de Gólgora,
- **Cuando** se declara el estado de Victoria,
- **Entonces** se habilita una tirada en la **Ruleta de Extracción** que otorga una de las 9 Entidades Interdimensionales (o le suma **+1 nivel** si ya formaba parte de la colección) y persiste el resultado automáticamente en el archivo JSON antes de volver al Almacén.

## Criterios globales de entrega del proyecto

1. **Bucle principal completo:** Transición continua entre Almacén, Arena (8 minutos + Gólgora), Ruleta de Extracción (en Victoria) y pantalla de resultados.
2. **Condiciones de cierre diferenciadas:** La Ruleta de Extracción solo se activa en Victoria; la Derrota devuelve al jugador al Almacén sin tirada.
3. **Persistencia verificada:** Guardado y carga local en JSON de las 9 Entidades, sus niveles, las 3 ranuras equipadas y la configuración de usuario.
4. **Rendimiento estable:** Ejecución a $60\text{ FPS}$ en Windows PC ($1920 \times 1080$) con $150$ enemigos simultáneos y cero errores bloqueantes.

> **Navegación:** [[00 - Índice]] · ← [[03 - Bugs y Seguimiento]] · [[01 - Glosario]] →
