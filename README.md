# Comercial Andina | Retail Sales Analytics with Python

**Proyecto 04 · Data Analytics | Python | Regresión lineal | Pruebas de hipótesis**

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=for-the-badge&logo=scipy&logoColor=white)
![Plotly](https://img.shields.io/badge/Plotly-3F4F75?style=for-the-badge&logo=plotly&logoColor=white)

**Autor:** Emilio Caldas  
**Formación:** Data Analyst Expert — Ciencia de Datos Ecuador  
**Caso:** Comercial Andina, empresa de retail del ejercicio académico

---

## Objetivo de negocio

Explorar las ventas de Comercial Andina, construir un modelo de regresión lineal y contrastar estadísticamente las diferencias de ventas entre transacciones con y sin promociones para orientar decisiones comerciales. La consigna original solicitaba **predecir ventas semanales**; el notebook entregado **analiza ventas semanales de manera descriptiva, pero entrena la regresión a nivel de transacción**, no a nivel semanal. Esta diferencia forma parte de las limitaciones del proyecto.

## Dataset

El notebook se conecta a una tabla de Databricks/Spark llamada `workspace.comercial_andina.comercial_andina_2_m` (los datos fuente no se incluyen en este repositorio).

| Característica | Resultado registrado en el notebook |
| --- | ---: |
| Transacciones | 2.000.000 |
| Periodo | 1 enero – 31 diciembre de 2025 |
| Sucursales | 6 |
| Categorías de producto | 5 |
| Campos originales | 8 |
| Valores nulos detectados en `descuento_pct` | 199.239 (9,96 %) |

Los valores nulos de `descuento_pct` se sustituyeron por 0. Se construyeron variables de mes, semana ISO, día de la semana y fin de semana. **Nota:** el notebook presupone que los nulos de descuento corresponden a ausencia de descuento; esta relación debe validarse contra la fuente si se reutiliza el análisis.

## Metodología

1. **Preparación de datos:** lectura desde Spark, exploración con Pandas, tratamiento de nulos y creación de variables temporales.
2. **Exploración:** distribuciones por categoría y sucursal, ventas semanales agregadas por sucursal y comparación entre transacciones con/sin promoción.
3. **Regresión lineal:** variables de precio, promoción, descuento, tiempo, sucursal y categoría; codificación one-hot; partición aleatoria 80/20 y entrenamiento con `LinearRegression` de scikit-learn.
4. **Pruebas estadísticas:** prueba t de Welch para comparar medias de ventas por promoción y prueba chi-cuadrado para evaluar asociación entre categoría y presencia de promoción.
5. **Recomendaciones:** revisar el diseño de promociones, incorporar variables de demanda y evaluar modelos de pronóstico semanal específicos.

## Resultados registrados

### Regresión lineal

| Métrica | Resultado |
| --- | ---: |
| Observaciones de entrenamiento | 1.600.000 |
| Observaciones de prueba | 400.000 |
| R² en prueba | ≈ 0,0000 |
| RMSE en prueba | 44,74 |
| MAE en prueba | 38,76 |

**Interpretación:** el modelo no presenta capacidad predictiva útil con las variables disponibles. Este es un resultado negativo que se reporta explícitamente; no se presenta como una predicción precisa. La validación fue una separación aleatoria de transacciones, no una validación temporal apropiada para pronósticos futuros.

### Promociones y ventas

| Indicador | Resultado |
| --- | ---: |
| Transacciones con promoción | 599.916 |
| Transacciones sin promoción | 1.400.084 |
| Venta media con promoción | 82,50 |
| Venta media sin promoción | 82,46 |
| Diferencia de medias | 0,04 |
| Prueba t de Welch, p-valor | 0,5448 |
| IC del 95 % para diferencia | [-0,09; 0,18] |
| Chi-cuadrado categoría × promoción, p-valor | 0,1965 |

**Interpretación:** con α = 0,05 no se encontró evidencia estadística suficiente para detectar una diferencia entre las ventas medias por transacción de ambos grupos. **Esto no demuestra que las promociones no tengan efecto causal:** el análisis es observacional y no controla los factores que determinan la asignación de promociones. Tampoco se detectó asociación estadísticamente significativa entre categoría y presencia de promoción en la prueba realizada.

## Recomendaciones

- Evaluar promociones mediante experimentos A/B controlados, antes de afirmar causalidad o ajustar campañas a gran escala.
- Añadir unidades por venta, inventarios, características de clientes, feriados y factores de estacionalidad.
- Si se busca **pronosticar ventas semanales**, agregar la variable objetivo por semana y sucursal, realizar divisiones temporales y comparar con una línea base estacional o ingenua.
- Explorar modelos no lineales solo después de definir correctamente la métrica, la granularidad y el esquema de validación.

## Tecnologías

- **Python:** Pandas, NumPy, SciPy, Plotly, Matplotlib (no utilizado explícitamente en el notebook), scikit-learn.
- **Datos:** Spark/Databricks para la lectura de la fuente; conversión a Pandas para el procesamiento local.
- **Técnicas:** limpieza, ingeniería de variables, codificación one-hot, regresión lineal, R², RMSE, MAE, t de Welch y chi-cuadrado.

## Archivos

```text
comercial-andina-python-github/
├── README.md
├── notebooks/
│   └── comercial-andina-analisis-predictivo.ipynb
├── requirements.txt
└── docs/
    └── NOTAS_DE_REPRODUCCION.md
```

**Abrir notebook:** [`notebooks/comercial-andina-analisis-predictivo.ipynb`](notebooks/comercial-andina-analisis-predictivo.ipynb)

## Cómo reproducirlo

1. Abrir el notebook en un entorno Databricks con acceso a `workspace.comercial_andina.comercial_andina_2_m`.
2. Instalar las dependencias indicadas en `requirements.txt`, en versiones compatibles con el entorno.
3. Ejecutar las celdas en orden, verificando que la tabla y los nombres de columnas coincidan.

**Limitación:** el repositorio no incluye los 2 millones de registros de origen ni un cargador independiente, por lo que no es ejecutable en un Colab/Jupyter vacío sin adaptar la conexión a los datos. Los resultados presentados se extrajeron de la ejecución guardada en el notebook; no se recalcularon en esta preparación del repositorio.

## Alcance y autoría

Ejercicio académico con datos del caso Comercial Andina. Las conclusiones corresponden al análisis realizado en el notebook y no constituyen una evaluación comercial de una empresa real verificable. Código y análisis del proyecto: **Emilio Caldas**.
