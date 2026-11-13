# Crédito formal, ingreso y resiliencia en los hogares rurales de Colombia: evidencia de la Encuesta de Demanda de Inclusión Financiera 2022 con modelos de clasificación

Curso Inteligencia Artificial con Aplicaciones en Economía I, Universidad Externado de Colombia, profesora Lina María Castro, 2026.

Autores:

- Isabella Ramirez Santamaria
- Kevin Stiven Albarracín González
- Julián Eduardo Lizarazo Cáceres
- Juan Felipe Pacheco Gómez

## Descripción Corta y Relevancia Económica

Según el Reporte de Inclusión Financiera 2024, en las ciudades el 41,8 % de los adultos tiene crédito formal, mientras que en los municipios rurales dispersos solo el 17,1 %. En este proyecto queremos saber si esa brecha se debe a vivir en el campo o a las condiciones de los hogares, y si tener crédito formal se asocia con un mejor bienestar en los hogares rurales. Esto es importante para saber si la política de inclusión financiera rural se debe enfocar en dar más crédito o en mejorar la estabilidad del ingreso de los hogares.

Usamos los microdatos de la Encuesta de Demanda de Inclusión Financiera 2022 y entrenamos dos modelos de clasificación. Comparamos regresión logística, árbol de decisión, Random Forest y XGBoost con GridSearchCV y validación cruzada, y los interpretamos con odds ratios y SHAP. El principal hallazgo es que, con el mismo ingreso, empleo y activos, vivir en un municipio rural aumenta las probabilidades de tener crédito formal, o sea que la desventaja rural pasa por el menor ingreso. Además, la relación entre el crédito y la resiliencia ante choques se reduce a la mitad cuando se controla por las condiciones de partida del hogar.

## Estructura del Repositorio

```
├── data/
│   ├── raw/
│   │   └── microdatos_demanda_2022.xlsx            # Microdatos originales de la encuesta
│   └── processed/
│       └── base_inclusion_financiera_2022.csv      # Base limpia, unida con la DIVIPOLA, que usa el notebook
├── notebooks/
│   └── codigo_final_IA.ipynb                       # Modelo 1 de acceso al crédito y Modelo 2 de resiliencia rural
├── outputs/
│   ├── figures/                                    # Gráficas que genera el notebook
│   └── tables/                                     # Tablas de resultados de los modelos
├── docs/
│   ├── Documento_analisis_final.pdf                # Documento de análisis
│   └── Presentacion_final_Inclusion_Financiera.pdf # Presentación de la sustentación
├── README.md                                       # Este archivo
└── requirements.txt                                # Librerías y versiones necesarias
```

## Instrucciones de Reproducibilidad

Se necesita Python 3.10 a 3.13.

1. Clonar el repositorio

```bash
git clone https://github.com/Lizaraz0004/PROYECTO-FINAL-IA-1.git
cd PROYECTO-FINAL-IA-1
```

2. Crear y activar el entorno virtual

```bash
python -m venv env
```

En Windows:

```bash
env\Scripts\activate
```

En macOS o Linux:

```bash
source env/bin/activate
```

3. Instalar dependencias

```bash
pip install -r requirements.txt
pip install notebook
```

La segunda línea solo hace falta si se va a abrir el notebook con Jupyter. Con VS Code no es necesaria.

4. Orden de ejecución

Se corre completo `notebooks/codigo_final_IA.ipynb`. El notebook lee la base `data/processed/base_inclusion_financiera_2022.csv`, entrena y compara los modelos, y guarda las figuras en `outputs/figures`, las tablas en `outputs/tables/tablas_modelos.xlsx` y el `requirements.txt`.

El notebook se puede abrir desde la carpeta `notebooks` o desde la raíz, porque al inicio detecta la carpeta y trabaja desde la raíz del proyecto. Todos los procesos aleatorios usan la semilla 42, así que los resultados salen iguales cada vez que se corre.

Para correrlo en Google Colab, en una celda nueva al inicio se corre `!git clone https://github.com/Lizaraz0004/PROYECTO-FINAL-IA-1.git` y luego `%cd PROYECTO-FINAL-IA-1`. Después se ejecutan todas las celdas del notebook.

## Datos y Fuentes

- Encuesta de Demanda de Inclusión Financiera 2022, microdatos: 5.610 filas y 432 columnas. Cada fila es un adulto de 18 años o más, con información de su hogar, en 21 departamentos, encuestado en abril y mayo de 2022. Fuente: Banca de las Oportunidades y Superintendencia Financiera, https://www.bancadelasoportunidades.gov.co/es/publicaciones/encuestas-de-demanda
- DIVIPOLA: 1.122 filas y 7 columnas. Cada fila es un municipio y se usó para saber si cada municipio es ciudad, intermedio, rural o rural disperso. Fuente: DANE, por la API de datos abiertos, https://www.datos.gov.co/Mapas-Nacionales/DIVIPOLA-C-digos-municipios/gdxc-w37w
- Base procesada: 5.532 filas y 59 columnas. Cada fila es un adulto con respuesta válida sobre crédito. Es la encuesta limpia y unida con la DIVIPOLA, y es la base que usa el notebook.

## Resultados Clave

Modelo 1, quién tiene crédito formal. Los modelos tienen un desempeño parecido. En validación cruzada Random Forest llega a un AUC de 0,708, la regresión logística a 0,707, XGBoost a 0,705 y el árbol de decisión a 0,677. En el conjunto de prueba los tres primeros llegan a cerca de 0,73. Con las mismas características, vivir en un municipio rural multiplica los odds de tener crédito formal por 1,64 y en uno rural disperso por 1,57 frente a las ciudades. Lo que sí reduce el acceso es la menor capacidad de pago: estar ocupado, el ingreso, los activos y la educación lo aumentan, y los ingresos irregulares lo reducen en 21 %.

![Odds ratios del Modelo 1](outputs/figures/12_odds_ratios_modelo1.png)

Modelo 2, si el crédito se asocia con más resiliencia rural. Sin controles, tener crédito formal multiplica los odds de poder vivir un mes o más sin ingresos por 1,67. Al controlar por ingreso, activos, educación y empleo baja a 1,34. Cerca de la mitad de la ventaja se explica por quiénes obtienen el crédito, y lo que más explica la resiliencia es el ingreso del hogar, estar ocupado y la educación.

![Asociación entre crédito y resiliencia](outputs/figures/16_seleccion_credito_resiliencia.png)

Como recomendación, proponemos medir la inclusión financiera rural por la estabilidad del ingreso de los hogares y no solo por el número de créditos, desarrollar modelos de riesgo que reconozcan ingresos estacionales y promover el ahorro formal, que es donde está la brecha más grande entre el campo y la ciudad.
