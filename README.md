# Pronóstico de Consumo Eléctrico con Deep Learning

Proyecto académico de series de tiempo multivariadas usando PyTorch.
Comparación de cuatro arquitecturas de Deep Learning para predecir
el consumo eléctrico horario de un hogar.

---

## Problema

Un hogar en Francia registra su consumo eléctrico minuto a minuto
entre 2006 y 2010. El objetivo es anticipar el consumo 1 hora hacia
el futuro usando las últimas 24 horas de mediciones.

**Variable objetivo:** `Global_active_power` (kW)  
**Historia:** 24 pasos de 1 hora  
**Horizonte:** 1 hora hacia el futuro  

---

## Dataset

**Individual Household Electric Power Consumption — UCI ML Repository**

| Característica | Valor |
|---|---|
| Fuente | [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/Individual+household+electric+power+consumption) |
| Período | Diciembre 2006 — Noviembre 2010 |
| Resolución original | 1 minuto |
| Resolución usada | 1 hora (submuestreo por media) |
| Filas tras limpieza | ~34,500 registros horarios |
| Variables | 7 numéricas + 1 categórica derivada |

### Variables

| Variable | Unidad | Rol |
|---|---|---|
| `Global_active_power` | kW | Target |
| `Global_reactive_power` | kVAR | Feature |
| `Voltage` | V | Feature |
| `Global_intensity` | A | Eliminada (correlación 0.99 con target) |
| `Sub_metering_1` | Wh | Feature — cocina |
| `Sub_metering_2` | Wh | Feature — lavandería |
| `Sub_metering_3` | Wh | Feature — climatización |
| `day_type` | — | Feature categórica — laborable / sábado / domingo |

---

## Estructura del proyecto
ProyectoSeriesTiempo/
│
├── KATALYNA_DAVID_TF_serie_tiempo.ipynb   # Notebook principal
├── KATALYNA_DAVID_Analisis_Serie.ipynb    # Análisis exploratorio
├── AGENTS.MD                              # Notas del agente
└── README.md                              # Este archivo


---

## Resultados

### Fase 1 — Benchmarking

| Modelo | MAE (kW) | RMSE (kW) | sMAPE% | R² | Params | T.Train | Lat(ms/smp) |
|---|---|---|---|---|---|---|---|
| Baseline | 0.4155 | 0.6289 | 76.57% | 0.3219 | 0 | 0.0s | 0.0153 |
| Conv1D | 0.3583 | 0.5173 | 73.80% | 0.5412 | 30,657 | 95.9s | 0.0610 |
| LSTM | 0.3513 | 0.5096 | 73.49% | 0.5548 | 54,593 | 207.7s | 0.1606 |
| Transformer | 0.3760 | 0.5460 | 79.43% | 0.4888 | 69,697 | 311.4s | 0.2440 |

### Fase 2 — Variantes LSTM

| Variante | MAE (kW) | RMSE (kW) | sMAPE% | R² | Params | T.Train | Lat(ms/smp) |
|---|---|---|---|---|---|---|---|
| LSTM-Base | 0.3458 | 0.5087 | 72.90% | 0.5564 | 54,593 | 167.6s | 0.1610 |
| LSTM-Deep | 0.3493 | 0.5110 | 72.74% | 0.5523 | 87,873 | 346.6s | 0.3003 |
| LSTM-Dropout | **0.3450** | **0.5022** | **72.90%** | **0.5677** | 54,593 | 249.8s | 0.1600 |
| LSTM-Wide | 0.3483 | 0.5034 | 73.79% | 0.5655 | 207,425 | 898.9s | 0.3849 |

### Modelo final: LSTM-Dropout

> MAE = 0.3450 kW | R² = 0.5677 | Latencia = 0.1600 ms/muestra

---

## Instalación

```bash
pip install numpy pandas matplotlib seaborn scikit-learn torch statsmodels requests
```

> **Nota:** En Google Colab puede aparecer un conflicto entre PyTorch y sympy.
> Ejecutar antes de entrenar:
> ```python
> import subprocess
> subprocess.run(["pip", "install", "-q", "--upgrade", "sympy"])
> ```
> Luego reiniciar la sesión.

---

## Uso

1. Clonar el repositorio
```bash
git clone https://github.com/Deivs117/ProyectoSeriesTiempo.git
cd ProyectoSeriesTiempo
```

2. Abrir el notebook principal
```bash
jupyter notebook KATALYNA_DAVID_TF_serie_tiempo.ipynb
```

3. Ejecutar todas las celdas en orden  
   **Runtime → Run all**

El dataset se descarga automáticamente desde UCI en la celda de carga.

---

## Arquitectura del sistema en producción (Cloud)

<img width="712" height="1024" alt="image" src="https://github.com/user-attachments/assets/79ef6667-d90b-4370-9d49-ffc3de5883af" />

---

## Decisiones clave

| Decisión | Justificación |
|---|---|
| Eliminar `Global_intensity` | Correlación 0.99 con target — redundante (P = V × I) |
| Submuestreo a 1h | Cubre ciclo circadiano completo en 24 pasos |
| `day_type` one-hot | Patrones horarios distintos por tipo de día |
| StandardScaler solo en train | Evita data leakage del futuro al pasado |
| No diferenciación | Trend-stationary — StandardScaler maneja la tendencia |
| Dropout=0.4 en modelo final | Mejor regularización sin costo extra en parámetros |
| Despliegue Cloud | Dashboard remoto, reentrenamiento automático, latencia aceptable |

---

## Autores

**Katalyna & David**  
Proyecto final — Series de Tiempo con Deep Learning
