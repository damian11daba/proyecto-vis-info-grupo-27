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

Ambos mapas cuentan con un botón **"Activar sonido"** que habilita la sonificación por voz al interactuar con cada país.

Al pasar el cursor sobre un país, el sistema anuncia en voz alta:

> **"[Nombre del País], [Clasificación]"**

**Ejemplos:**
- *"Chile, mejora destacada"*
- *"Argentina, Transición Verde"*
- *"México, Dependencia Fósil"*
- *"Panamá, retroceso destacado"*

### Detalles técnicos
- Utiliza la **Web Speech API** nativa del navegador (`SpeechSynthesis`), sin dependencias externas.
- Prioriza una voz en español chileno (`es-CL`), con fallback a cualquier voz en español disponible.
- **No menciona colores** en ningún momento: la sonificación está basada exclusivamente en el nombre del país y su clasificación energética, lo que la hace completamente accesible e independiente de la percepción visual del color.
- Incluye un debounce de 120ms para evitar lecturas repetidas al moverse rápidamente entre países.

### Clasificaciones anunciadas por mapa

**Mapa 13 (cambio continuo):**
`mejora destacada` · `mejora significativa` · `mejora moderada` · `cambio leve positivo` · `sin cambio relevante` · `leve retroceso` · `retroceso moderado` · `retroceso significativo` · `retroceso destacado`

**Mapa 14 (categórico):**
`Transición Verde` · `Dependencia Fósil` · `Estable` · `Caída de Demanda`

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