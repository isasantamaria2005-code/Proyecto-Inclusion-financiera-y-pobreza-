# Inclusión financiera y pobreza: ¿el acceso al crédito formal está asociado con mejores condiciones de bienestar en los hogares rurales de Colombia?

Proyecto del curso **Inteligencia Artificial con Aplicaciones en Economía I**, Universidad Externado de Colombia, Facultad de Economía. Profesora: Lina María Castro. 2026.

**Integrantes:** Isabella Ramirez Santamaria, Kevin Stiven Albarracin González, Julián Eduardo Lizarazo Caceres, Juan Felipe Pacheco Gómez.

---

## 1. Problema económico

El acceso al crédito formal en Colombia no es homogéneo entre territorios. En 2024 el indicador de acceso al crédito del sector financiero tradicional fue de 41,8 % en ciudades y aglomeraciones, 25,3 % en municipios intermedios, 22,1 % en municipios rurales y 17,1 % en municipios rurales dispersos (Reporte de Inclusión Financiera 2024). Si el crédito permite relajar restricciones de liquidez, esta brecha puede limitar la capacidad de los hogares rurales para suavizar su consumo, invertir en actividades productivas, educarse y ahorrar.

## 2. Pregunta de investigación

¿Qué relación existe entre el acceso al crédito formal y el bienestar económico de los hogares rurales en Colombia?

Con los datos disponibles la pregunta se estudia sobre **adultos que viven en municipios rurales y rurales dispersos** (clasificación de ruralidad del municipio, la misma del Reporte de Inclusión Financiera), con características de su hogar, y se responde como una **asociación**, no como un efecto causal. El análisis se centra en 2022.

## 3. Hipótesis

El acceso al crédito formal está asociado con mejores condiciones de bienestar (ingreso, gasto, ahorro y bienestar financiero) porque relaja restricciones de liquidez. Esta asociación puede estar sobrestimada por selección: las personas con más ingreso, educación o estabilidad laboral tienen, al mismo tiempo, más probabilidad de obtener crédito y mejores resultados de bienestar.

Hipótesis específicas que guían el análisis exploratorio:

1. El acceso al crédito formal es menor en zona rural y rural dispersa, y el crédito informal es relativamente mayor.
2. Con el mismo nivel de ingreso, la población rural sigue teniendo menos acceso, lo que apuntaría a barreras de oferta.
3. Quienes tienen crédito formal muestran mejor bienestar, pero también más educación, activos y empleo.
4. En la población rural pesan más las barreras de garantías e ingresos, consistente con racionamiento de crédito por información asimétrica.
5. En la población rural el crédito se usa más para consumo que para inversión productiva.

La siguiente entrega (modelos) plantea:
- **Modelo 1 (acceso):** P(Crédito = 1 | X) = F(β₀ + β′X), con Logit/Probit.
- **Modelo 2 (bienestar):** Bienestar = α + δ·Crédito + γ′X + ε.

## 4. Datos y fuentes

| Base | Descripción | Unidad | Periodo | Enlace |
|---|---|---|---|---|
| Encuesta de Demanda de Inclusión Financiera 2022 (Banca de las Oportunidades y Superintendencia Financiera) | Acceso y uso de productos financieros, crédito, ahorro, bienestar financiero y características sociodemográficas. Aplicada por el Centro Nacional de Consultoría | Adulto (5.610 encuestas, 432 variables, 21 departamentos) | Abril y mayo de 2022 | [Microdatos](https://www.bancadelasoportunidades.gov.co/sites/default/files/2022-10/Encuesta_demanda_2022_microdatos.xlsx) · [Diccionario](https://www.bancadelasoportunidades.gov.co/sites/default/files/2022-08/Diccionario%20encuesta%20de%20demanda%202021_1.xlsx) · [Formulario](https://www.bancadelasoportunidades.gov.co/sites/default/files/2022-08/Formulario%20Encuesta%20de%20Demanda%202021_1.pdf) |
| DIVIPOLA (DANE) | Códigos oficiales de departamentos y municipios | Municipio | Vigente | [datos.gov.co](https://www.datos.gov.co/Mapas-Nacionales/DIVIPOLA-C-digos-municipios/gdxc-w37w) |
| Reporte de Inclusión Financiera 2024 | Contexto de la brecha territorial de acceso al crédito | Categoría de ruralidad | 2024 | [Banca de las Oportunidades](https://www.bancadelasoportunidades.gov.co/es/publicaciones/reportes-anuales) |

Página con todos los archivos de la encuesta: https://www.bancadelasoportunidades.gov.co/es/publicaciones/encuestas-de-demanda

**Sobre la ECV 2025 (DANE):** la Encuesta de Demanda es de personas y de 2022; la ECV es de hogares y de 2025, y no comparten identificadores. Unirlas fila a fila no es válido, así que la unión se hace por territorio (DIVIPOLA). La ECV queda como fuente complementaria para la etapa de modelos.

## 5. Metodología

1. **Importación:** descarga programática de las bases desde sus fuentes oficiales con `requests` (sin pasos manuales).
2. **Selección y estandarización** de columnas con base en el formulario oficial.
3. **Unión** de la encuesta con la DIVIPOLA por departamento, con `merge(how='left')` y verificación de filas antes y después.
4. **Auditoría de calidad:** tipos de dato, faltantes, valores distintos, rangos y códigos de "No sabe / No responde".
5. **Limpieza:** duplicados, códigos 9 y 99, escalas fuera de rango, decodificación de categorías, tipos de dato y edades imposibles.
6. **Variables nuevas:** crédito formal e informal, ahorro formal, digital e informal, ingreso y gasto en salarios mínimos, índice de bienestar financiero (0 a 100, con alfa de Cronbach), meses de resiliencia, índice de activos y región.
7. **Atípicos:** regla del rango intercuartílico, con decisión justificada.
8. **Análisis exploratorio:** estadísticas ponderadas con el factor de expansión, distribuciones, comparaciones por zona, ingreso y departamento, barreras al crédito, uso del crédito, pruebas t de diferencia de medias y matriz de correlación.

## 6. Estructura del repositorio

```
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   ├── raw/          # microdatos de la encuesta y DIVIPOLA (descargados por el notebook)
│   └── processed/    # base limpia para la etapa de modelos
├── notebooks/
│   └── Proyecto_Inclusion_Financiera_Entrega2.ipynb
├── outputs/
│   ├── figures/      # gráficas del análisis exploratorio
│   └── tables/       # tablas_eda.xlsx: descriptivas, rechazo, barreras, correlaciones, atípicos...
└── docs/
    ├── Documento_analisis_entrega2.docx / .pdf
    ├── Presentacion_entrega2.pptx / .pdf
    └── Guia_sustentacion.md
```

## 7. Instalación y ejecución

**En Google Colab (recomendado):** abrir `notebooks/Proyecto_Inclusion_Financiera_Entrega2.ipynb` y usar *Entorno de ejecución > Ejecutar todas*. El notebook descarga las bases, crea las carpetas y guarda todas las gráficas y tablas.

**En un equipo local:**

```bash
git clone <enlace-del-repositorio>
cd <carpeta-del-repositorio>
pip install -r requirements.txt
jupyter notebook notebooks/Proyecto_Inclusion_Financiera_Entrega2.ipynb
```

Si alguna página oficial bloquea la descarga, las mismas bases están en `data/raw/` de este repositorio.

## 8. Principales resultados

Cifras ponderadas con el factor de expansión `Fexp_Reg_Rur` (36,1 millones de adultos).

1. **La brecha rural en tenencia de crédito formal es pequeña en la encuesta.** Ciudades y aglomeraciones 25,7 %, intermedio 16,4 %, rural 24,7 %, rural disperso 20,0 % (chi-cuadrado p = 0,004). Rural agregado 22,9 % frente a 23,8 % en el resto. La escalera del RIF 2024 no se reproduce.
2. **A igual ingreso, lo rural tiene igual o más acceso.** Con menos de 0,5 salarios mínimos: 14,7 % rural frente a 8,4 % urbano; entre 1 y 2 salarios: 36,8 % frente a 24,5 %. La ruralidad opera a través del ingreso (0,86 salarios mínimos en municipios rurales frente a 1,82 en ciudades).
3. **La fricción está en el rechazo.** Entre solicitantes, el rechazo es 8,1 % en ciudades y 13,7 % a 15,8 % en el resto. Los solicitantes rurales acuden más a cooperativas (27,6 % frente a 16,2 %).
4. **Crédito y bienestar: asociación con selección.** En lo rural, el 71 % de quienes tienen crédito aguanta un mes o más sin ingresos, frente al 60 %. El índice de bienestar ponderado casi no cambia (49,5 frente a 49,0), y quienes tienen crédito ganan más, tienen más activos, más empleo y más educación.
5. **Barreras y uso.** Domina la aversión a endeudarse (68,7 % en lo rural); pesan más los ingresos bajos que las garantías. En lo rural el crédito se usa más para invertir (54,8 % frente a 38,3 %).
6. **La brecha más clara es de ahorro:** no ahorra el 49,3 % en ciudades frente a cerca del 68 % en municipios rurales.

El detalle está en `docs/Documento_analisis_entrega2.pdf` y en el notebook.

## 9. Limitaciones

- **Asociación, no causalidad:** los datos son de corte transversal y no hay variación exógena en el acceso al crédito. Las diferencias observadas pueden deberse a selección.
- **Unidad de análisis:** el crédito se pregunta a la persona entrevistada, no al hogar completo.
- **Muestra rural concentrada:** las categorías rural y rural disperso provienen de 20 municipios, así que las cifras por ruralidad y por departamento dependen de cuáles fueron seleccionados.
- **Índice de bienestar:** alfa de Cronbach de 0,63 (consistencia moderada); por eso se complementa con la resiliencia.
- **Número de registros:** la base tiene 5.610 encuestas; el comunicado oficial reporta 5.513. La documentación no explica la diferencia.
- **Ingreso y gasto en rangos:** se usa el punto medio de cada rango, lo que no distingue hogares dentro de un mismo rango.
- **Fuentes distintas:** el RIF cuenta productos registrados por las entidades (2024) y la encuesta pregunta a la persona (2022); se comparan en tendencia, no cifra contra cifra.
- **Tamaño de muestra rural:** algunos cruces por departamento tienen pocas observaciones; solo se reportan departamentos con 30 o más encuestas.

## Referencias

- Banca de las Oportunidades y Superintendencia Financiera de Colombia. (2022). *Encuesta de Demanda de Inclusión Financiera 2022*.
- Banca de las Oportunidades y Superintendencia Financiera de Colombia. (2025). *Reporte de Inclusión Financiera 2024*.
- Departamento Administrativo Nacional de Estadística (DANE). *División Político-Administrativa de Colombia (DIVIPOLA)*.
- Stiglitz, J. E. y Weiss, A. (1981). Credit rationing in markets with imperfect information. *American Economic Review*, 71(3), 393-410.
