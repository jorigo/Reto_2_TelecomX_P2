# Reto_2_TelecomX_P2
# README

# Análisis de Predicción de Churn de Clientes en Telecomunicaciones

## Propósito del Análisis

El objetivo principal de este proyecto es construir modelos predictivos capaces de identificar clientes con alta probabilidad de **cancelar (churn)** sus servicios de telecomunicaciones. Al predecir el churn, la empresa puede implementar estrategias de retención proactivas y personalizadas para minimizar la pérdida de clientes y, consecuentemente, optimizar sus ingresos.

## Estructura del Proyecto

El proyecto está organizado de la siguiente manera:

*   `TelecomX_Churn_Prediction.ipynb`: Cuaderno principal de Jupyter/Colab que contiene todo el código para la preparación de datos, modelado, evaluación y análisis de importancia de variables.
*   `/content/drive/MyDrive/Colab Notebooks/Archivos_Pandas_DataBase/TelecomX_Datos_Procesados.json`: Archivo JSON que contiene los datos procesados iniciales utilizados en el cuaderno.
*   `/visualizaciones`: (En caso de generarse y guardarse) Carpeta para almacenar gráficos y otras visualizaciones generadas durante el Análisis Exploratorio de Datos (EDA) o la evaluación de modelos.

## Descripción del Proceso de Preparación de Datos

El proceso de preparación de datos fue crucial para asegurar la calidad y el formato adecuado para el modelado:

1.  **Carga de Datos**: Los datos se cargaron desde el archivo JSON (`TelecomX_Datos_Procesados.json`).
2.  **Eliminación de Columnas No Relevantes**: La columna `ID_Cliente` fue eliminada ya que es un identificador único y no aporta información predictiva para el modelo.
3.  **Clasificación y Codificación de Variables Categóricas**: Las variables categóricas como `Servicio_Internet`, `Contrato` y `Metodo_Pago` fueron identificadas y convertidas en variables numéricas mediante el método One-Hot Encoding (`pd.get_dummies`). Se utilizó `drop_first=True` para evitar la multicolinealidad.
4.  **Verificación y Balanceo de la Proporción de Churn**: Se detectó un **desbalance significativo** en la variable objetivo `Abandono` (aproximadamente 74% de clientes 'no abandono' vs. 26% 'abandono'). Para mitigar este problema y evitar que el modelo se incline a predecir la clase mayoritaria, se aplicó la técnica de **sobremuestreo SMOTE (Synthetic Minority Over-sampling Technique)**, lo que resultó en una distribución balanceada (50%-50%) de la variable objetivo.
5.  **Estandarización de Variables Numéricas**: Las columnas numéricas `Antiguedad_Meses`, `Cargos_Mensuales`, `Cargos_Totales` y `Cargos_Diarios` fueron escaladas utilizando `StandardScaler`. Esto es fundamental, especialmente para modelos como la Regresión Logística, ya que asegura que ninguna característica domine desproporcionadamente la función de costo debido a su escala original.
6.  **Separación en Conjuntos de Entrenamiento y Prueba**: Los datos balanceados fueron divididos en un conjunto de entrenamiento (80%) y un conjunto de prueba (20%) utilizando `train_test_split`. El parámetro `stratify=y` se utilizó para asegurar que la proporción de la variable objetivo se mantenga igual en ambos conjuntos, garantizando una evaluación imparcial del modelo.

## Justificaciones para las Decisiones de Modelización

Se eligieron dos modelos de clasificación principales, Regresión Logística y Random Forest, por las siguientes razones:

*   **Regresión Logística**: Es un modelo lineal simple y altamente interpretable. Se eligió para obtener una comprensión clara de la relación lineal entre las características y la probabilidad de churn a través de sus coeficientes. La estandarización de los datos fue una decisión clave para optimizar su rendimiento.
*   **Random Forest Classifier**: Este es un algoritmo de ensemble basado en árboles de decisión. Se seleccionó por su robustez, su capacidad para manejar relaciones no lineales y la baja sensibilidad a la escala de las características (no requiriendo estandarización, aunque se aplicó para consistencia). Además, Random Forest proporciona una medida de la importancia de las variables, lo cual es fundamental para este análisis.

## Ejemplos de Gráficos e Insights Obtenidos (EDA)

Durante la fase de EDA y análisis de correlación, se generaron gráficos como:

*   **Mapas de Calor de Correlación**: Revelaron las relaciones entre todas las variables, destacando la correlación de ciertas características con el churn (ej. `Cargos_Mensuales`, `Antiguedad_Meses`, `Servicio_Internet_Fiber_optic`).
*   **Boxplots de Distribución de Cargos/Antigüedad por Abandono**: Por ejemplo, los `boxplots` mostraron claramente que los clientes que abandonan suelen tener `Antigüedad_Meses` significativamente menor y `Cargos_Totales` más bajos en comparación con los que no abandonan. Esto sugiere que los clientes nuevos y con menos gasto total (que puede estar correlacionado con la antigüedad) son más propensos al churn.

## Instrucciones para Ejecutar el Cuaderno

Para ejecutar este cuaderno en Google Colab o un entorno Jupyter, siga los siguientes pasos:

1.  **Montar Google Drive (si usa Colab)**: En el cuaderno, ejecute el siguiente comando para acceder a los archivos en su Google Drive:
    ```python
    from google.colab import drive
    drive.mount('/content/drive')
    ```
2.  **Instalar Bibliotecas Necesarias**: Asegúrese de tener instaladas las siguientes bibliotecas. Si no las tiene, puede instalarlas ejecutando:
    ```bash
    !pip install pandas scikit-learn matplotlib seaborn imbalanced-learn
    ```
3.  **Cargar los Datos Tratados**: Asegúrese de que el archivo `TelecomX_Datos_Procesados.json` esté ubicado en la ruta especificada en la variable `file_path` (`/content/drive/MyDrive/Colab Notebooks/Archivos_Pandas_DataBase/TelecomX_Datos_Procesados.json`). El cuaderno cargará automáticamente los datos desde esa ubicación.
4.  **Ejecutar las Celdas**: Ejecute todas las celdas del cuaderno en orden para replicar el análisis completo, desde la preparación de datos hasta la evaluación del modelo y el análisis de importancia de variables.

Al seguir estos pasos, podrá reproducir el análisis y explorar los insights clave sobre la predicción de churn de clientes.
