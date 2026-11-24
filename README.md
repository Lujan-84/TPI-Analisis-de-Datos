# TPI - Análisis de Datos: Customer Churn

**Trabajo Práctico Integrador · Introducción al Análisis de Datos**
Tecnicatura Universitaria en Programación · UTN-FRA · 2026

Análisis exploratorio de datos e identificación de los factores asociados al abandono de clientes (*churn*) en una empresa de telecomunicaciones.

## Integrantes

- Jazmín Luque
- Thiago Martínez
- Luján Miguel
- Edelmar Ricaldez

**Docente:** Enzo Martín Zotti

## Problema de negocio

Una empresa de telecomunicaciones (telefonía, internet y servicios complementarios como seguridad online, respaldo, soporte técnico y streaming) pierde clientes todos los meses. Como retener a un cliente cuesta mucho menos que conseguir uno nuevo, el objetivo es **identificar qué características demográficas, contractuales y de facturación impulsan la cancelación** para diseñar estrategias de retención.

**Variable objetivo:** `Churn` (cualitativa nominal binaria): `Yes` si el cliente canceló el servicio en el último mes, `No` si continúa activo.

## Hipótesis

| # | Hipótesis | Resultado |
|---|-----------|-----------|
| H1 | Los adultos mayores (`SeniorCitizen = 1`) tienen una tasa de cancelación significativamente mayor que los más jóvenes. | ✅ Confirmada |
| H2 | Los contratos mensuales (`Month-to-month`) tienen una tasa de abandono notablemente superior a los contratos anuales o bianuales. | ✅ Confirmada |

## Dataset

**Telco Customer Churn** (IBM / Kaggle): 7.043 registros y 21 atributos. Cada fila es un cliente único y cada columna una característica (demográfica, servicios contratados, tipo de contrato, facturación y churn).

## Estructura del repositorio

```
TPI-Analisis-de-Datos/
├── TPI_Analisis_de_Datos.ipynb          # Notebook con todo el análisis
├── INFORME_EJECUTIVO.pdf                # Informe técnico-ejecutivo
├── WA_Fn-UseC_-Telco-Customer-Churn.csv # Dataset original
├── telco_churn_limpio.csv               # Dataset depurado
└── README.md
```

> Nota: el notebook exporta la base limpia a `data/processed/telco_churn_limpio.csv`. Si ordenás los archivos en carpetas, ajustá las rutas en el notebook.

## Metodología

1. **Relevamiento de variables:** clasificación teórica (nominal, ordinal, discreta, continua) contrastada con el `dtype` de Pandas.
2. **Manipulación con Pandas:** filtrado con operadores lógicos, agregaciones con `groupby` + `agg()`, columnas calculadas (`Cargo_Esperado`, `Cargo_Anual_Estimado`), renombrado de `Churn` a `Cancelacion` y eliminación de `customerID`.
3. **Limpieza y calidad de datos:**
   - 22 filas duplicadas en atributos, pero 0 `customerID` repetidos → se conservaron (son clientes distintos con el mismo perfil).
   - 11 registros con espacios en blanco en `TotalCharges` (clientes nuevos con `tenure = 0`) → convertidos con `pd.to_numeric(errors="coerce")` e imputados con `0.0`.
   - Sin outliers fuera de los límites teóricos (boxplots) → se conservó el 100 % de los registros.
4. **Análisis exploratorio:** medidas de tendencia central y dispersión, correlación de Pearson, histogramas con KDE, tasas de cancelación por contrato y método de pago, y tablas de contingencia.

## Principales resultados

- **Tasa global de cancelación: 26,54 %** (1.869 bajas sobre 7.043 clientes).
- **Tipo de contrato:** Month-to-month 42,71 % · One year 11,27 % · Two year 2,83 %.
- **Adultos mayores:** 41,68 % de churn frente a 23,61 % en clientes más jóvenes.
- **Método de pago:** Electronic check 45,29 % · Bank transfer (automatic) 16,71 % · Credit card (automatic) 15,24 %.
- **Antigüedad:** distribución bimodal; el mayor riesgo de fuga está en los primeros meses.
- **Correlaciones:** `tenure`–`TotalCharges` r = 0,83 · `MonthlyCharges`–`TotalCharges` r = 0,65 · `tenure`–`MonthlyCharges` r = 0,25.

## Recomendaciones estratégicas

1. Migrar clientes mensuales a contratos anuales con descuentos escalonados o beneficios por permanencia.
2. Programa de bienvenida y seguimiento durante los primeros 3 a 6 meses.
3. Incentivar el débito automático entre quienes pagan con cheque electrónico.
4. Atención personalizada para adultos mayores, especialmente con contratos mensuales.

## Limitaciones y próximos pasos

- Las asociaciones encontradas **no implican causalidad**.
- Es un corte transversal y no incluye variables externas (reclamos, calidad de servicio, competencia).
- Próximo paso: entrenar modelos de clasificación (Regresión Logística, Árboles de Decisión) para estimar la probabilidad individual de fuga.

## Reproducibilidad

### Requisitos

- Python 3.9 o superior
- Librerías: `pandas`, `numpy`, `matplotlib`, `seaborn`, `kagglehub`, `ipython`, `jupyter`

### Instalación y ejecución

```bash
git clone https://github.com/Lujan-84/TPI-Analisis-de-Datos.git
cd TPI-Analisis-de-Datos

pip install pandas numpy matplotlib seaborn kagglehub ipython jupyter

jupyter notebook TPI_Analisis_de_Datos.ipynb
```

El notebook descarga el dataset desde Kaggle con `kagglehub`. Si no tenés acceso, podés usar el CSV incluido en el repositorio (`WA_Fn-UseC_-Telco-Customer-Churn.csv`) cargándolo con `pd.read_csv()`.

## Tecnologías

Python · Pandas · NumPy · Matplotlib · Seaborn · Jupyter Notebook
