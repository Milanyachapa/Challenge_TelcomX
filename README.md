# Challenge_TelcomX
# TelecomX LATAM – Análisis Exploratorio de Bajas

**Análisis de fuga (churn) en clientes de telecomunicaciones de Latinoamérica** utilizando Python, Pandas y LightGBM.  
Este repositorio incluye un Jupyter Notebook con el flujo completo de trabajo de análisis exploratorio (EDA), segmentación y modelado, así como una aplicación interactiva en Streamlit para visualización ejecutiva.

---

## 📂 Contenido del repositorio

| Ruta/Archivo            | Descripción                                                                 |
|-------------------------|-----------------------------------------------------------------------------|
| `TelecomX_LATAM.ipynb`  | Notebook principal con preparación de datos, análisis exploratorio y modelo.|
| `Informe.md`            | Informe con hallazgos clave y recomendaciones para el cliente.              |
| `app_churn.py`          | App de Streamlit que visualiza métricas e identifica clientes en riesgo.   |
| `TelecomX_Data.json`    | Dataset original con estructuras anidadas en formato JSON.                  |
| `requirements.txt`      | Lista de dependencias mínimas de Python.                                   |

---

## 1. Descripción del problema

Las compañías de telecomunicaciones afrontan pérdidas significativas debido a la **fuga de clientes (churn)**.  
Este proyecto busca:

- Entender los factores que impulsan la baja de clientes.
- Construir un modelo de machine learning para anticipar clientes con alta probabilidad de cancelar el servicio.
- Visualizar y segmentar los resultados para facilitar la toma de decisiones.

---

## 2. Flujo de trabajo

### 🔹 Ingesta & Normalización
- Lectura del JSON con Pandas.
- Transformación de estructuras anidadas usando `explode` y `pd.json_normalize`.
- Obtención de una tabla plana (flat table) lista para análisis.

### 🔹 Limpieza & Enriquecimiento
- Conversión de vacíos a `NaN` y tipado correcto de columnas (`float`, `category`, etc.).
- Renombrado de columnas al español.
- Mapeo de variable `Churn` a binaria (`1 = Baja`, `0 = Activo`).

### 🔹 Análisis Exploratorio (EDA)
- Estadísticos descriptivos y distribución de KPIs clave: cargos, antigüedad, servicios contratados.
- Visualizaciones de churn según:
  - Género
  - Senioridad
  - Tipo de contrato
  - Método de pago
  - Servicio de internet

### 🔹 Segmentación Crítica
- Clientes con:
  - ≤ 12 meses de antigüedad
  - Contrato *month-to-month*
  - Servicio de fibra óptica
  - Pago por *electronic check*

### 🔹 Hallazgos clave
- **Tasa de bajas > 43 %** entre contratos *month-to-month*.
- El método de pago *electronic check* duplica el churn respecto a tarjetas o débito automático.
- Segmento crítico presenta alto ARPU y **3× más probabilidad** de baja.

### 🔹 Modelado Predictivo
- Entrenamiento con `LightGBMClassifier`.
- Variables incluidas: contractuales, de servicios y comportamiento de uso.
- **Métrica principal:** ROC-AUC ≈ **0.81**  
- Matriz de confusión con umbral configurable (`default = 0.40`).

### 🔹 Dashboard Ejecutivo
- Visualización en Streamlit:
  - KPIs en tiempo real
  - Gráficos interactivos con Plotly
  - Secciones: Introducción, Género/Senior, Fidelidad, Cruces, Conclusiones
- **Funcionalidad de descarga** de CSV con clientes de alto riesgo.

---

## 3. Ejecución local

```bash
# 1. Clonar el repositorio
git clone https://github.com/ingridcristh/challenge2-data-science-LATAM.git

# 2. Crear entorno virtual (opcional)
python -m venv .venv && source .venv/bin/activate

# 3. Instalar dependencias
pip install -r requirements.txt

# 4. Abrir el notebook
jupyter lab TelecomX_LATAM.ipynb

# 5. Ejecutar el dashboard
streamlit run app_churn.py --server.port 8501

# Créditos
Proyecto desarrollado por Milan Yachapa (Data Science).
linkidin : https://www.linkedin.com/in/milanyb/
