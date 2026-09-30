# 🌎 Proyecto de Visualización de Información - Grupo 27
## Comparación de la Transición Energética en América: Pandemia (2020-2022) vs Post-pandemia (2023-2024)

Este repositorio contiene el análisis de datos y el desarrollo de visualizaciones interactivas enfocadas en entender cómo cambió la matriz de generación eléctrica en el continente americano tras el impacto de la pandemia de COVID-19.

El objetivo principal es responder a la pregunta: **¿La recuperación de la demanda energética post-pandemia impulsó una transición hacia energías limpias o profundizó la dependencia de los combustibles fósiles?**

📊 Sobre el Dataset
Los datos utilizados provienen del Ember Global Electricity Review (release_generation_yearly_global_3.csv).
Este dataset ofrece un desglose anual de la generación de electricidad por país y fuente de origen (TWh).

🧹 Limpieza y Auditoría de Datos
Para garantizar la integridad estadística y evitar sesgos en las comparaciones, se aplicaron los siguientes criterios:
Filtro Geográfico: Análisis restringido a Norteamérica y Sudamérica (Continent == "North America" | "South America"), excluyendo agrupaciones regionales.
Auditoría de Completitud: Se identificó que gran parte de los países no contaban con datos consolidados para 2025. Para evitar distorsiones, el periodo "Post-pandemia" se ajustó al rango 2023-2024.
Filtro de Cobertura: Se excluyeron los países que no presentaban datos continuos entre 2020 y 2024, resultando en un análisis robusto sobre 40 países.

🛠️ Metodología y Lógica de Análisis
El proyecto no solo evalúa el crecimiento bruto del consumo eléctrico, sino la composición de esa recuperación. Se agruparon las fuentes de energía en dos grandes categorías:
Energía Limpia (Clean): Hidroeléctrica, Solar, Eólica, Bioenergía, Geotérmica y Nuclear.
Energía Fósil (Fossil): Carbón, Gas natural y Otros combustibles fósiles.

🧮 Clasificación de la Recuperación
Se desarrolló un algoritmo propio (Paso 10 del Notebook) para categorizar el comportamiento de cada país al comparar la diferencia absoluta ($\Delta$ TWh) entre ambos periodos:
🟢 Transición Verde: El aumento en la demanda fue cubierto principalmente (o totalmente) por un incremento en la generación de energías limpias.
🔴 Dependencia Fósil: La recuperación energética fue sostenida predominantemente por un aumento en la quema de combustibles fósiles.
🟠 Estable: Ambas fuentes crecieron exactamente en la misma proporción, o no hubo cambios significativos.
⚪ Caída de Demanda: El país redujo su generación tanto en fuentes limpias como fósiles en comparación a la pandemia, indicando una contracción energética o económica.

📈 Visualizaciones Implementadas
El notebook genera dos mapas de coropletas interactivos desarrollados con plotly.express:
Mapa de Cambio Porcentual Continuo (fig): Utiliza una escala divergente (Rojo-Amarillo-Verde) para visualizar el cambio en puntos porcentuales de la cuota de energía limpia ($\Delta\%$ Clean). Permite identificar rápidamente qué países "enverdecieron" su red eléctrica y cuáles retrocedieron.
Mapa de Clasificación Categórica (fig2): Muestra de forma discreta la clasificación del país según el motor de su recuperación energética (Verde, Fósil, Estable o Caída).
Interactividad: Ambos mapas cuentan con tooltips enriquecidos que despliegan al usuario:
 Porcentaje de energía limpia en Pandemia y Post-pandemia.
 Variación absoluta en TWh tanto para fuentes limpias como fósiles.
 La fuente de energía principal (individual) que dominó la matriz del país en la post-pandemia.

🚀 Cómo Ejecutar el Proyecto
Requisitos Previos
Python 3.8+Librerías: pandas, plotlyInstrucciones
Clona este repositorio o descarga los archivos.
Asegúrate de que el archivo release_generation_yearly_global_3.csv se encuentre en el mismo directorio que el notebook.
Instala las dependencias necesarias
Abre y ejecuta el archivo proyecto_3.ipynb en Jupyter Notebook, JupyterLab o VS Code.Los mapas interactivos se renderizarán al final de las secciones correspondientes en el notebook.