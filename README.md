# 🌎 Proyecto de Visualización de Información - Grupo 27
## Comparación de la Transición Energética en América: Pandemia (2020-2022) vs Post-pandemia (2023-2024)

Este repositorio contiene el análisis de datos y el desarrollo de visualizaciones interactivas enfocadas en entender cómo cambió la matriz de generación eléctrica en el continente americano tras el impacto de la pandemia de COVID-19.

El objetivo principal es responder a la pregunta: **¿La recuperación de la demanda energética post-pandemia impulsó una transición hacia energías limpias o profundizó la dependencia de los combustibles fósiles?**

---

## 📊 Sobre el Dataset

Los datos utilizados provienen del **Ember Global Electricity Review** (`release_generation_yearly_global.csv`).
Este dataset ofrece un desglose anual de la generación de electricidad por país y fuente de origen (TWh).

---

## 🧹 Limpieza y Auditoría de Datos

Para garantizar la integridad estadística y evitar sesgos en las comparaciones, se aplicaron los siguientes criterios:

- **Filtro Geográfico:** Análisis restringido a Norteamérica y Sudamérica (`Continent == "North America" | "South America"`), excluyendo agrupaciones regionales.
- **Auditoría de Completitud:** Se identificó que gran parte de los países no contaban con datos consolidados para 2025. Para evitar distorsiones, el periodo "Post-pandemia" se ajustó al rango 2023-2024.
- **Filtro de Cobertura:** Se excluyeron los países que no presentaban datos continuos entre 2020 y 2024, resultando en un análisis robusto sobre 40 países.

---

## 🛠️ Metodología y Lógica de Análisis

El proyecto no solo evalúa el crecimiento bruto del consumo eléctrico, sino la composición de esa recuperación. Se agruparon las fuentes de energía en dos grandes categorías:

- **Energía Limpia (Clean):** Hidroeléctrica, Solar, Eólica, Bioenergía, Geotérmica y Nuclear.
- **Energía Fósil (Fossil):** Carbón, Gas natural y Otros combustibles fósiles.

---

## 🧮 Clasificación de la Recuperación

Se desarrolló un algoritmo propio (Paso 10 del Notebook) para categorizar el comportamiento de cada país al comparar la diferencia absoluta (Δ TWh) entre ambos periodos:

- **Transición Verde:** El aumento en la demanda fue cubierto principalmente (o totalmente) por un incremento en la generación de energías limpias.
- **Dependencia Fósil:** La recuperación energética fue sostenida predominantemente por un aumento en la quema de combustibles fósiles.
- **Estable:** Ambas fuentes crecieron exactamente en la misma proporción, o no hubo cambios significativos.
- **Caída de Demanda:** El país redujo su generación tanto en fuentes limpias como fósiles en comparación a la pandemia, indicando una contracción energética o económica.

---

## 📈 Visualizaciones Implementadas

El proyecto genera dos mapas de coropletas interactivos desarrollados con **Plotly**:

### Mapa 13 — Cambio en la Cuota de Energía Limpia (continuo)
Visualiza el cambio en puntos porcentuales (Δ% Clean) de la participación de la energía limpia por país.
Utiliza una **escala divergente accesible naranja ↔ blanco ↔ azul**, donde:
- Tonos **azules** indican mayor adopción de energía limpia.
- Tonos **naranjas** indican mayor dependencia de combustibles fósiles.

### Mapa 14 — Clasificación Categórica de la Recuperación
Muestra de forma discreta la categoría de recuperación energética de cada país.

Ambos mapas cuentan con **tooltips enriquecidos** que al hacer hover despliegan:
- Nombre del país y clasificación energética.
- Fuente de energía principal dominante.
- Cambio en puntos porcentuales de energía limpia.
- **Gráfico de barras dinámico** comparando el porcentaje de energía limpia vs fósil entre el periodo pandemia (2020-2022) y post-pandemia (2023-2024).

---

## 🎨 Paleta de Colores Accesible para Daltonismo

La paleta de colores fue diseñada para garantizar la correcta diferenciación visual en los tres tipos de daltonismo más comunes:

| Categoría | Color | Código HEX | Accesibilidad |
|---|---|---|---|
| Transición Verde | 🔵 Azul | `#0077cc` | Distinguible en protanopía y deuteranopía |
| Dependencia Fósil | 🟠 Naranja | `#d55e00` | Alto contraste de tono frente al azul en todos los tipos |
| Estable | 🟡 Dorado/Ámbar | `#f0a500` | Diferente luminancia respecto al naranja |
| Caída de Demanda | ⚪ Gris neutro | `#999999` | Neutro e inequívoco en cualquier tipo de daltonismo |

- **Protanopía** (ausencia de rojo): el par azul/naranja mantiene contraste por tono y luminancia.
- **Deuteranopía** (ausencia de verde): mismo principio, el eje azul/naranja es robusto.
- **Tritanopía** (ausencia de azul): el naranja, dorado y gris se distinguen por luminancia relativa.

> La escala divergente del Mapa 13 también fue reemplazada de rojo-verde (inaccesible) a **naranja-blanco-azul**, que es la combinación más robusta para todos los tipos de daltonismo.

---

## 🔊 Sonificación

Ambos mapas cuentan con un botón **"Activar sonido"** que habilita la sonificación completa: primero suena un **tono de datos** sintetizado con [Tone.js](https://tonejs.github.io/), y luego la voz narra el nombre del país y su clasificación.

### ¿Por qué dos canales auditivos?

Siguiendo la Cápsula 23 del curso (*"La vista tiene un límite — con la sonificación convertimos datos en sonido para percibir patrones, anomalías y cambios a través del oído"*), la sonificación de este proyecto va más allá de la narración de voz: el **dato numérico en sí mismo se convierte en sonido perceptible**, igual que el contador Geiger convierte radiación en clics, o el sensor de estacionamiento convierte distancia en pitidos.

### Mapa 13 — Sonificación continua (Tone.js FM Synth)

El cambio en puntos porcentuales (Δ%) de cada país se mapea a una **frecuencia musical**:

| Δ% energía limpia | Frecuencia | Percepción |
|---|---|---|
| −15.6 pp (máximo retroceso) | 130 Hz (Do2) | Tono grave, oscuro |
| 0 pp (sin cambio) | 440 Hz (La4) | Tono neutro |
| +15.6 pp (máxima mejora) | 880 Hz (La5) | Tono agudo, brillante |

- **Timbre:** Sintetizador FM (tipo "data display") — breve pitido científico con envolvente rápida.
- **Interpolación logarítmica** para que los semitonos se perciban equidistantes (como el oído los escucha naturalmente).

### Mapa 14 — Sonificación categórica (Tone.js Synth)

Cada categoría tiene un **timbre distinto** para ser identificable solo por el oído:

| Categoría | Tipo de onda | Frecuencia | Carácter sonoro |
|---|---|---|---|
| Transición Verde | Sinusoidal | 660 Hz | Limpio, suave, ascendente |
| Dependencia Fósil | Diente de sierra | 180 Hz | Grave, áspero, industrial |
| Estable | Triángulo | 330 Hz | Neutro, plano, simple |
| Caída de Demanda | Cuadrada | 220 Hz | Oscuro, percusivo, descendente |

### Narración de voz (Web Speech API)

Después del tono musical, el sistema anuncia en voz alta:

> **"[Nombre del País], [Clasificación]"**

- No menciona colores en ningún momento.
- Prioriza voz en español chileno (`es-CL`), con fallback a cualquier voz en español.
- Incluye debounce de 120 ms para evitar activaciones accidentales.

### Detalles técnicos

- **Tone.js 14.9.9**: biblioteca de síntesis de audio de alto nivel sobre Web Audio API.
- El contexto de audio se activa solo tras el primer clic del usuario (requisito de seguridad de todos los navegadores modernos).
- Los nodos de síntesis se eliminan automáticamente tras sonar (`synth.dispose()`) para evitar fugas de memoria.

---

## 🚀 Cómo Ejecutar el Proyecto

### Requisitos Previos
- Python 3.8+
- Librerías: `pandas`, `plotly`

### Instrucciones

1. Clona este repositorio o descarga los archivos.
2. Asegúrate de que el archivo `release_generation_yearly_global.csv` se encuentre en el mismo directorio que el notebook.
3. Instala las dependencias necesarias:
   ```bash
   pip install pandas plotly
   ```
4. Abre y ejecuta el archivo `proyecto_finalizado.ipynb` en Jupyter Notebook, JupyterLab o VS Code.
5. Para ver la visualización web completa con sonificación y tooltips dinámicos, abre directamente `index.html` en cualquier navegador moderno (Chrome, Edge, Firefox).

> **Nota:** La sonificación requiere un navegador que soporte la Web Speech API. Chrome y Edge ofrecen la mejor compatibilidad.

---

*Grupo 27 · Visualización de Información · 2026*

---

## 📝 Proceso de Diseño e Iteraciones (E1)

Siguiendo la metodología del curso de evaluar el proceso y las decisiones de diseño (y no solo el resultado final), a continuación se detallan las 4 versiones por las que pasó esta visualización:

### 1. Versión Inicial (Prototipo Básico)
- **Visualización:** Dos mapas de calor con las escalas de colores estándar de Plotly.
- **Interacción:** Solo el tooltip por defecto de Plotly con datos brutos.
- **Sonificación:** Ninguna.
- **Problema detectado:** El mapa divergente usaba la escala "RdYlGn" (Rojo-Amarillo-Verde), lo cual excluía a usuarios con daltonismo (Protanopía/Deuteranopía). El tooltip no daba suficiente contexto sobre qué significaban los cambios porcentuales.

### 2. Versión Accesible
- **Visualización:** Se reemplazó la paleta cromática por una escala divergente segura para daltónicos (Naranja - Blanco - Azul) basada en la luminancia y el contraste seguro para Protanopía, Deuteranopía y Tritanopía. Las 4 categorías del segundo mapa también se mapearon a Azul, Naranja, Dorado y Gris.
- **Justificación (Cápsula 11):** El color es un canal preatentivo, pero debe ser universal.

### 3. Versión con Sonificación Básica y Tooltips Dinámicos
- **Interacción:** Se agregó un tooltip flotante (Hover) que superpone un mini-gráfico de barras comparando la cuota de energía fósil vs. limpia entre los periodos analizados, sumando contexto.
- **Sonificación:** Se implementó la Web Speech API para leer en voz alta el país y su color.
- **Problema detectado:** La voz leía los colores (ej. "Chile, azul"), lo cual rompía el propósito de accesibilidad visual y no comunicaba el dato subyacente. Además, narrar no es sonificar (Cápsula 27).

### 4. Versión Multisensorial y Explicativa
- **Sonificación Real (Tone.js):** Se reemplazó la lectura de colores por síntesis FM. El valor porcentual exacto del mapa 1 ahora modula la frecuencia de un tono (Pitch). En el mapa 2, cada categoría activa un timbre distinto. La voz ahora acompaña diciendo la "clasificación" en lugar del color.
- **"Overview first, zoom and filter" (Cápsula 25):** Se agregaron botones interactivos a la leyenda del Mapa 14 para poder filtrar (ocultar/mostrar) grupos de países y limpiar el ruido visual.
- **Diseño Explicativo:** Se añadieron anotaciones directas en el Mapa 13 sobre los "outliers" (Chile y Panamá) para destacar el hallazgo principal sin obligar al usuario a buscarlo manualmente.

### 5. Versión Final Definitiva (Ajuste estricto por Daltonismo Tritanomalía)
- **Problema detectado:** Tras una segunda revisión, notamos que la paleta Azul-Naranja, si bien es perfecta para Protanopía y Deuteranopía, sigue comprometiendo el eje Azul-Amarillo, lo que puede causar confusión leve en casos de **Tritanopía** (ceguera al azul).
- **Solución Aplicada:** Se rediseñó todo el mapeo cromático utilizando estándares clínicos:
  - Para el Mapa 1 (divergente), se implementó la escala **ColorBrewer PiYG/Okabe-Ito Unificada** (Verde - Blanco - Naranja).
  - Para el Mapa 2 (categórico), se adoptó la estricta **Paleta Okabe-Ito**, diseñada por investigadores japoneses para ser 100% inequívoca ante cualquier deficiencia visual.
- **Justificación:** El color debe ser un canal preatentivo universal, sin excepciones (Cápsula 11).

---

## 🧪 Evaluación con Usuarios (Think-Aloud)

Para validar la iteración 3, se realizó una prueba rápida siguiendo el método "Think-Aloud" (Cápsula 22) con compañeros. 
**Hallazgos:**
1. Un usuario intentó hacer clic en la leyenda del Mapa 14 para ocultar las "Caídas de Demanda" y ver solo los países en transición. Al no funcionar, sugirió que la leyenda fuera interactiva (implementado en V4).
2. Otro usuario comentó que el pitido de Tone.js en el Mapa 1 le permitía "escanear" el mapa con el oído mucho más rápido que leyendo el tooltip, validando la hipótesis de la sonificación de datos.

---

## ⚠️ Limitaciones y Criterios de Clasificación (R1/R3)

Para la defensa del proyecto (R1/R3) es fundamental transparentar los siguientes límites de nuestro análisis y algoritmos:

1. **Uso de métricas relativas vs. absolutas (Discordancia aparente):**
   El Mapa 1 visualiza el cambio en **puntos porcentuales (pp)** de la cuota limpia (métrica relativa). Sin embargo, el Mapa 2 (Clasificación) incluye reglas basadas en **crecimiento absoluto (TWh)** de la demanda. 
   - *Ejemplo Uruguay:* Aumentó su cuota limpia en +6.17 pp, pero como su consumo *total* absoluto cayó en post-pandemia, el algoritmo lo clasifica en "Caída de Demanda", no en Transición Verde.
   - *Decisión de diseño:* Preferimos mantener esta dualidad para mostrar que el % no cuenta toda la historia si la demanda subyacente cae.
2. **Factores Climáticos vs. Políticas (El problema de la Hidrología):**
   Países como Panamá, Colombia, Ecuador y Guatemala mostraron caídas dramáticas en su cuota limpia (Dependencia Fósil). Sin embargo, esto no siempre refleja un cambio de política gubernamental, sino **sequías (Fenómeno de El Niño)** que mermaron la generación hidroeléctrica, obligando a despachar gas/carbón de emergencia.
3. **Ponderación por País:**
   El mapa coroplético le da el mismo peso visual a una pequeña isla del Caribe (con 0.01 TWh de demanda) que a Estados Unidos. Esto es un límite intrínseco de los mapas geográficos para la energía, que podría abordarse en futuros proyectos (Ej. cartogramas).
4. **Fórmula de Sonificación Logarítmica:**
   Usamos una interpolación logarítmica continua $f = f_{min} \times (f_{max}/f_{min})^t$ entre 130 Hz y 880 Hz porque el oído humano percibe el "tono musical" de forma exponencial, no lineal (cada octava duplica la frecuencia). Al mapear el intervalo [-15.6, +15.6], el punto neutro de 0 pp corresponde a $t=0.5$, lo que produce una frecuencia de ~338 Hz (y no 440 Hz como sería en un mapeo aritmético erróneo).