# Predicción de la respuesta patológica axilar en cáncer de mama con datos clínicos y de PET/CT

Código del Trabajo Fin de Máster *Aprendizaje automático aplicado a la predicción de la respuesta patológica axilar en cáncer de mama con datos clínicos y de PET-CT*
(Máster en Ingeniería del Software: Cloud, Datos y Gestión TI, Universidad de Sevilla, curso 2025/26).

**Autora:** Paloma Bravo del Cid
**Tutores:** Jorge García Gutiérrez y Rosa Mª Álvarez Pérez

## Qué hace este repositorio

Compara cuatro algoritmos (XGBoost, regresión logística con penalización elastic-net, random forest y TabPFN) para predecir la afectación ganglionar axilar tras la neoadyuvancia (ypN+ frente a ypN0) en 85 pacientes. La evaluación se hace solo con validación cruzada estratificada repetida (10×5, semilla 42), sin conjunto de test reservado, y se completa con validación cruzada anidada (selección de variables y optimización de hiperparámetros dentro de cada fold), contrastes estadísticos, robustez frente a la semilla (5 semillas) e interpretación con SHAP.

## Datos

Los datos clínicos y de imagen proceden del Hospital Universitario Virgen del Rocío (HUVR) y se obtuvieron a través de EUCAIM. **No se publican** por tratarse de datos de pacientes. Por ello:

- Los notebooks 01 y 02 documentan cómo se construye la matriz final a partir de los datos originales, pero no pueden ejecutarse sin ellos.
- Los notebooks 05 a 07 y las gráficas necesitan `results/X_full.csv` y `results/y_full.csv` (generados por el notebook 02), que tampoco se incluyen.
- Sí se incluye `results/` con los resultados agregados (métricas, AUC por fold, tests estadísticos, hiperparámetros), de modo que se pueden consultar y se puede regenerar el notebook `GRAFICAS` a partir de los CSV de resultados que lee.

## Notebooks y orden de ejecución

Ejecutar en este orden, con los notebooks en la raíz del repositorio (usan las rutas relativas `data/`, `results/`, `fig/` y `models/`):

| Orden | Notebook | Función | Salida principal |
|---|---|---|---|
| 1 | `01_agregacion_clinica.ipynb` | Agregación a una fila por paciente y definición de la cohorte | `tabla_clinica_agregada.csv` |
| 2 | `02_dataset_predictores.ipynb` | Selección de predictores, variable objetivo, análisis exploratorio y matriz final | `dataset_modelado.csv`, `X_full.csv`, `y_full.csv` |
| 3 | `05_modelado_baseline.ipynb` | Evaluación de referencia (CV repetida 10×5) | resumen de AUC y modelos entrenados |
| 4 | `06_evaluacion_final.ipynb` | Métricas por CV y predicciones fuera de muestra | `metricas_finales_cv_robusta.csv`, matrices de confusión |
| 5 | `06b_robustez_semillas.ipynb` | Robustez frente a la semilla de partición (5 semillas) | `robustez_semillas_detalle.csv` |
| 6 | `07_validacion_cruzada_anidada.ipynb` | Validación anidada, comparación con TabPFN, modelo de referencia y SHAP | tablas, figuras y `modelo_referencia.pkl` |
| 7 | `GRAFICAS.ipynb` | Figuras de resumen de los notebooks 06, 06b y 07 | figuras de comparación de modelos |

Tiempos aproximados: el notebook 07 tarda unos 30 minutos (50 folds externos con búsqueda de hiperparámetros y TabPFN). El resto es rápido, salvo TabPFN en los notebooks 05, 06 y 06b.

## Instalación

Se usó Python 3.13. Con un entorno virtual:

```
python -m venv .venv
source .venv/bin/activate        # en Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter lab
```

TabPFN descarga sus pesos la primera vez que se usa. Los resultados de TabPFN y random forest pueden variar ligeramente entre versiones de las librerías y equipos distintos.

## Carpetas

- `results/`: resultados agregados (CSV).
- `fig/`: figuras generadas.
- `models/`: modelos entrenados. No se publican.
- `data/`: datos originales. No se publican.

## Cómo citar

Bravo del Cid, P. (2026). *Aprendizaje automático aplicado a la predicción de la respuesta patológica axilar en cáncer de mama con datos clínicos y de PET-CT*. Trabajo Fin de Máster, Universidad de Sevilla.
