# 📖 AGENTS.MD: Directrices del Sistema y Reglas de Inyección para Copilot / AI Assistants
**Proyecto:** Procesamiento de Datos Secuenciales con Deep Learning (deivs117/ProyectoSeriesTiempo)  
**Notebooks Core:** `KTALYNA_DAVID_TF_serie_tiempo.ipynb` y `KTALYNA_DAVID_Analisis_Series_Tiempo_Métricas.ipynb`

---

## 🛡️ 1. REGLAS CRÍTICAS DE ARQUITECTURA Y NO-DUPLICACIÓN (ANTI-ALUCINACIÓN)

* **PROHIBIDO CREAR DUPLICADOS:** El notebook ya cuenta con una estructura cronológica definida. Está estrictamente prohibido generar nuevos encabezados o secciones independientes al final del documento si la temática ya existe (ej. no duplicar la sección de métricas, gráficos o preprocesamiento).
* **REFACTORIZACIÓN *IN SITU*:** Toda propuesta de código o texto explicativo debe especificar con precisión milimétrica la celda, función o sección existente que va a **reemplazar, fusionar o complementar**.
* **PRESERVACIÓN DE PIPELINES OPERATIVOS:** No destruyas ni alteres la lógica de carga de datos (`DataLoaders`), limpieza previa, tokenización, escalado numérico (`MinMaxScaler` / `StandardScaler`) ni los hiperparámetros core de los modelos que ya compilan sin errores. Las mejoras de código deben integrarse de forma modular y aditiva.
* **CONTEXTO DE AMBOS NOTEBOOKS:** El agente debe cruzar la información teórica y metodológica de `KTALYNA_DAVID_Analisis_Series_Tiempo_Métricas.ipynb` (conceptos matemáticos de errores, justificación de métricas, fundamentos de convoluciones) para robustecer las explicaciones y celdas de texto de `KTALYNA_DAVID_TF_serie_tiempo.ipynb`.
* **PROHIBICIÓN ESTRICTA DE EMOJIS:** Queda rotundamente prohibido el uso de emojis, emoticonos o caracteres pictográficos en cualquier celda de Markdown, explicaciones de texto o comentarios de código generados. Toda salida debe mantener una estética puramente académica, sobria y de nivel de ingeniería de software.

---

## 🧠 2. NIVEL TÉCNICO Y RIGOR DE INGENIERÍA (TONO Y VOCABULARIO)

* **PROHIBIDO EL CONTENIDO SUPERFICIAL:** Quedan vetadas las explicaciones genéricas o introductorias (ej. "LSTM es bueno para secuencias"). El lenguaje debe ser de nivel Senior/Postgrado en Machine Learning.
* **VOCABULARIO REQUERIDO:** Utiliza terminología avanzada del estado del arte de forma mandatoria:
    * *Optimización:* Desvanecimiento/explosión del gradiente, regularización estocástica, decaimiento de peso (*Weight Decay*), superficies de pérdida no convexas, dinámicas de aprendizaje adaptativas.
    * *Series de Tiempo:* Estacionariedad en sentido amplio (WSS), ergodicidad, autocovarianza, autocorrelación residual, *Data Drift*, *Concept Drift*, dependencias temporales de largo/corto alcance, fugas de datos (*Data Leakage*).
    * *Arquitecturas:* Mecanismos de compuertas (*gating mechanisms*), campos receptivos locales, causalidad convolucional, mapas de atención autoregresivos, codificación posicional (*Positional Encoding*).

---