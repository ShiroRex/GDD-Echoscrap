# Requisitos Técnicos

## Descripción de la sección

En este apartado se especifican las características de hardware y software previstas para ejecutar y probar *EchoScrap*. Como se trata de un proyecto académico desarrollado para PC, nuestro objetivo es que el juego funcione de manera fluida y accesible en equipos de cómputo estándar (tanto de los evaluadores como de los propios estudiantes), estableciendo metas de rendimiento claras para cuando comience la etapa de producción.

---

## Requisitos del Sistema

### Requisitos mínimos (Orientados a 720p / 1080p con 30-60 FPS)

* **Sistema operativo:** Windows 10 de 64 bits.
* **Procesador (CPU):** Intel Core i3 de 8.ª generación / AMD Ryzen 3 1200 (o equivalente de 4 núcleos a 3.0 GHz).
* **Tarjeta gráfica (GPU):** Gráficos integrados Intel UHD Graphics 630 o tarjeta dedicada básica Nvidia GeForce GT 1030 / AMD Radeon RX 550 (con compatibilidad para DirectX 11).
* **Memoria RAM:** 8 GB.
* **Almacenamiento:** 1 GB de espacio disponible en disco duro.
* **Resolución mínima:** 1280 × 720 píxeles.
* **Conexión a internet:** No se requiere conexión; el juego se diseñará como una experiencia local para un solo jugador.

### Requisitos recomendados (Orientados a 1080p a 60 FPS estables)

* **Sistema operativo:** Windows 10 o Windows 11 de 64 bits.
* **Procesador (CPU):** Intel Core i5 de 10.ª generación / AMD Ryzen 5 3600 (o superior).
* **Tarjeta gráfica (GPU):** Nvidia GeForce GTX 1050 Ti / GTX 1650 o AMD Radeon RX 570.
* **Memoria RAM:** 16 GB.
* **Almacenamiento:** 1 GB en unidad de estado sólido (SSD) para agilizar tiempos de carga.
* **Resolución recomendada:** 1920 × 1080 píxeles.

---

## Objetivos de Rendimiento Técnico

Al iniciar la programación, el equipo trabajará para cumplir con las siguientes metas técnicas:

* **Tasa de cuadros por segundo (FPS):** Mantener **60 FPS estables** a resolución nativa de 1080p. El diseño contempla que el rendimiento no caiga por debajo de 50 FPS durante los momentos de mayor saturación de la horda en el minuto 7 u 8.
* **Tiempos de carga:** Se proyecta que la transición entre el menú principal/Almacén y la arena de combate tome **menos de 3 segundos**, y que el cambio entre pantallas de resultados sea prácticamente instantáneo.
* **Consumo de memoria RAM:** El juego no debería superar los **2 GB de memoria RAM en ejecución**, permitiendo que funcione sin problemas en computadoras con múltiples aplicaciones abiertas.

---

## Restricciones y Límites de Diseño Técnico

Para garantizar la estabilidad del proyecto y evitar retrasos en el desarrollo, se fijan las siguientes restricciones:

1. **Plataforma única:** La entrega se concentrará exclusivamente en computadoras con sistema operativo Windows. No se contemplan adaptaciones para Mac, Linux, consolas ni dispositivos móviles en esta fase.
2. **Tope de enemigos en pantalla:** La densidad de combate se limitará a un máximo de **150 enemigos activos simultáneos**. Si el generador alcanza este número, pausará nuevas apariciones hasta que el jugador elimine unidades, evitando caídas bruscas de cuadros por exceso de colisiones.
3. **Optimización visual:** Al emplear una estética en pixel art sobre un plano horizontal, se buscará empaquetar los sprites en atlas de texturas para mantener bajo el número de llamadas de dibujado (*draw calls*).
4. **Independencia de red:** Al no haber componentes multijugador ni marcadores en línea, no existirán riesgos asociados a latencia, caídas de servidor o consumo de ancho de banda.

> **Navegación:** [[00 - Índice]] · ← [[02 - Motor y Herramientas]] · [[04 - Datos y Persistencia]] →
