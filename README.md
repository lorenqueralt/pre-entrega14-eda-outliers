# Pre-entrega · Análisis de Distribuciones Categóricas y Detección de Outliers

**Curso:** Data Science II — Coderhouse

Análisis exploratorio centrado en la **forma de las distribuciones** y la **detección de
valores extremos (outliers)**, usando gráficos de barras, boxplots, violin plots y la
técnica del Rango Intercuartílico (IQR).

## Origen de los datos

Dataset **`tips`** de la librería Seaborn
(https://github.com/mwaskom/seaborn-data). Contiene **244 registros** de consumos en un
restaurante, con las siguientes variables:

| Variable | Tipo | Descripción |
|----------|------|-------------|
| `total_bill` | numérica | Monto total de la cuenta |
| `tip` | numérica | Propina |
| `size` | numérica | Cantidad de comensales |
| `sex` | categórica | Sexo de quien paga |
| `smoker` | categórica | Fumador / no fumador |
| `day` | categórica | Día de la semana |
| `time` | categórica | Almuerzo / cena |

El CSV se incluye en `data/tips.csv` para que el notebook sea reproducible.

## Estructura

```
preentrega15-eda-outliers/
├── analisis_distribuciones.ipynb   # notebook con el análisis (ejecutado)
├── data/
│   └── tips.csv                     # dataset
├── requirements.txt
├── .gitignore
└── README.md
```

## Cómo ejecutar

```bash
python -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook analisis_distribuciones.ipynb
```

## Contenido del análisis

1. **Variables categóricas** — gráficos de barras (countplots) de `sex`, `smoker`, `day` y `time`.
2. **Asimetría y curtosis** — cálculo e interpretación de skewness y curtosis para las
   variables numéricas.
3. **Outliers** — boxplots y violin plots de `total_bill` y `tip`, más una función que
   aplica el criterio IQR para identificarlos y segmentarlos.

## Resultados principales

- `total_bill` (skewness ≈ 1.13) y `tip` (skewness ≈ 1.47) tienen **asimetría positiva**:
  la cola se extiende hacia la derecha. La mayoría de las cuentas y propinas son moderadas,
  con algunos valores altos que estiran la distribución.
- El criterio IQR marca **9 registros** como outliers en `total_bill` (≈ 3.7 % del total),
  todos del lado de los valores altos.

## ¿Qué hacer con los outliers?

Según el análisis visual, los outliers detectados **no parecen errores de carga**: son
cuentas y propinas altas pero plausibles (mesas grandes o cenas caras). Por eso la decisión
no es automática:

- **No eliminarlos por defecto.** Al ser valores reales, borrarlos descartaría información
  legítima sobre el comportamiento de los clientes de alto gasto.
- **Preferir una transformación** (por ejemplo, logarítmica) para reducir el efecto de la
  asimetría positiva sin perder registros. Esto comprime la cola derecha y suele mejorar el
  desempeño de modelos sensibles a la escala.
- **Eliminar solo si fueran errores.** Si un valor fuera imposible (una propina negativa o
  un monto absurdo), ahí sí correspondería quitarlo; no es el caso de este dataset.

En resumen: para este dataset se recomienda **conservar los outliers y trabajar la
asimetría con una transformación**, en lugar de eliminarlos.
