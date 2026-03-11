# ChallengeTelecomX2

 📁 Estructura del proyecto
La organización de archivos y carpetas es la siguiente:

text
📁 TelecomX_Churn_Analysis/
│
├── 📄 README.md                          # Este archivo
├── 📓 TelecomX_Churn_Analysis.ipynb      # Notebook principal (Google Colab / Jupyter)
│
├── 📁 data/
│   ├── telecom_churn_clean.csv           # Dataset final limpio y procesado
│   └── (otros archivos si se generan)
│
├── 📁 images/                             # Gráficos generados durante el análisis
│   ├── churn_global.png
│   ├── boxplots_churn.png
│   ├── correlation_matrix.png
│   ├── rf_importance.png
│   ├── lr_coefficients.png
│   ├── roc_curve.png
│   └── ...
│
└── 📁 informe/
    └── informe_final.md                   # Informe detallado con conclusiones y recomendaciones
Nota: Las carpetas data/, images/ e informe/ pueden crearse automáticamente al ejecutar el notebook o manualmente.

🔧 Descripción del proceso de preparación de los datos
1. Clasificación de variables
El dataset original contenía 21 columnas, de las cuales:

Variables numéricas:

customer_SeniorCitizen (binaria 0/1)

customer_tenure (meses como cliente)

account_Charges.Monthly (cargo mensual)

account_Charges.Total (cargo total acumulado)

Variables categóricas:

customer_gender, customer_Partner, customer_Dependents, phone_PhoneService, phone_MultipleLines, internet_InternetService, internet_OnlineSecurity, internet_OnlineBackup, internet_DeviceProtection, internet_TechSupport, internet_StreamingTV, internet_StreamingMovies, account_Contract, account_PaperlessBilling, account_PaymentMethod.

Variable objetivo:

Churn (Yes/No), convertida a numérica (1/0) para modelado.

2. Etapas de normalización y codificación
Valores nulos: Se identificaron 11 nulos en account_Charges.Total, que fueron imputados con la mediana del grupo según antigüedad y cargo mensual.

Codificación de categóricas: Se aplicó One-Hot Encoding a todas las variables categóricas, utilizando drop='first' para evitar multicolinealidad.

Normalización (escalado): Para la Regresión Logística (sensible a la escala), se aplicó StandardScaler a las variables numéricas. Para Random Forest (insensible a la escala), no se escalaron.

3. Separación en entrenamiento y prueba
Se dividió el dataset en 80% entrenamiento y 20% prueba utilizando train_test_split con estratificación (stratify=y) para mantener la misma proporción de clases en ambos conjuntos. Esto asegura una evaluación realista del rendimiento.

🧠 Justificación de decisiones durante la modelización
Selección de modelos:

Regresión Logística: Modelo lineal interpretable, útil para entender la dirección y magnitud del efecto de cada variable. Requiere escalado.

Random Forest: Modelo no lineal, captura interacciones complejas y no necesita escalado. Proporciona importancia de variables.

Manejo del desbalance de clases: La clase positiva (Churn = Yes) representa el 26.5% de los datos. Se optó por mantener el desbalance y evaluar con métricas adecuadas (precisión, recall, F1, AUC-ROC) en lugar de aplicar técnicas de balanceo, para reflejar la realidad del negocio.

Prevención de overfitting en Random Forest: Se detectó overfitting inicial (rendimiento perfecto en entrenamiento). Se ajustaron hiperparámetros (max_depth, min_samples_split, min_samples_leaf) mediante validación cruzada para mejorar la generalización.

Evaluación: Se priorizó el recall (sensibilidad) para la clase positiva, ya que el costo de no detectar un cliente en riesgo es alto. El AUC-ROC se usó como métrica global de discriminación.

📊 Ejemplos de gráficos e insights obtenidos
1. Distribución global de Churn
https://images/churn_global.png
Insight: La tasa de churn es del 26.5%, lo que justifica la necesidad de acciones de retención.

2. Boxplots comparativos
https://images/boxplots_churn.png
Insight: Los clientes que cancelan tienen menor antigüedad (mediana de 10 meses vs 38 meses) y cargos mensuales ligeramente superiores.

3. Matriz de correlación
https://images/correlation_matrix.png
Insight: La antigüedad tiene la correlación negativa más fuerte con churn (-0.35). Los cargos mensuales tienen correlación positiva débil (0.19).

4. Importancia de variables (Random Forest)
https://images/rf_importance.png
Insight: Las variables más importantes son tenure, Contract_Month-to-month y MonthlyCharges.

5. Coeficientes de Regresión Logística
https://images/lr_coefficients.png
Insight: El contrato mensual, el pago con cheque electrónico y la ausencia de seguridad online aumentan la probabilidad de churn.

6. Curva ROC comparativa
https://images/roc_curve.png
Insight: Ambos modelos tienen AUC superior a 0.83, superando ampliamente al azar. La regresión logística muestra un recall ligeramente superior.

🚀 Instrucciones para ejecutar el notebook
1. Abrir en Google Colab (recomendado)
Ve a colab.research.google.com.

Selecciona Archivo > Subir notebook y elige TelecomX_Churn_Analysis.ipynb.

2. Instalar bibliotecas necesarias
El notebook incluye las instalaciones necesarias al inicio. Si se ejecuta en un entorno local, asegúrate de tener instaladas:

bash
pip install pandas numpy matplotlib seaborn scikit-learn
3. Cargar los datos
Los datos se cargan directamente desde la URL pública:

python
url = "https://raw.githubusercontent.com/ingridcristh/challenge2-data-science-LATAM/refs/heads/main/TelecomX_Data.json"
df = pd.read_json(url)
Si prefieres usar el archivo CSV limpio (telecom_churn_clean.csv), súbelo a Colab o colócalo en la misma carpeta y cárgalo con:

python
df = pd.read_csv('telecom_churn_clean.csv')
4. Ejecutar el notebook paso a paso
El notebook está dividido en secciones:

Carga y limpieza de datos.

Análisis exploratorio con visualizaciones.

Preparación (codificación, escalado, división train/test).

Entrenamiento y evaluación de modelos.

Análisis de importancia de variables.

Generación de gráficos para el informe.

Ejecuta las celdas en orden. Los gráficos se guardarán automáticamente en la carpeta images/ (se crea si no existe).

5. Resultados esperados
Los modelos alcanzarán una exactitud cercana al 80% y un AUC-ROC ~0.84.

Se obtendrán los gráficos de importancia de variables y coeficientes.
