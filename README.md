# Ecobici CABA — Análisis de uso y saturación de estaciones

Proyecto de portfolio de análisis y engineering de datos, usando datos abiertos de Ecobici (Buenos Aires) enriquecidos con datos históricos de clima.

## Preguntas de negocio
1. ¿Cuáles son los patrones de uso (horarios, días, estaciones)?
2. ¿Qué estaciones sufren saturación (sin bicis o sin anclajes disponibles)?
3. ¿Cómo impacta el clima en el uso del sistema?
4. ¿Cómo varía el uso año a año?

## Stack
- **BigQuery** — data warehouse
- **Python (pandas)** — limpieza y enriquecimiento
- **Open-Meteo API** — datos climáticos históricos
- **Looker Studio & Power BI** — dashboards
- **Google Cloud Storage** — almacenamiento de datos crudos (desde Fase 2)

## Progreso
- [x] Fase 1: Carga de datos crudos a BigQuery
- [x] Fase 2: Enriquecimiento con datos climáticos
- [ ] Fase 3: Modelado SQL (esquema estrella)
- [ ] Fase 4: Análisis exploratorio (EDA)
- [ ] Fase 5: Dashboards (Looker Studio + Power BI)
- [ ] Fase 6: Documentación final

---

## Fase 1 — Carga de datos crudos a BigQuery

**Fuente de datos:** [Portal de datos abiertos de Buenos Aires](https://data.buenosaires.gob.ar/),
dataset "Recorridos realizados" de Ecobici, año 2024.

**Qué se hizo:**
- Se descargó el dataset completo de recorridos 2024 (`recorridos-realizados-2024.csv`).
- Como el archivo era demasiado grande para subir directo a BigQuery, se filtró
  a un solo mes usando un script de Python (pandas) — ver `notebooks/01_carga_bigquery.ipynb`.
- Se creó un proyecto en Google Cloud (`ecobici-portfolio`) y un dataset en
  BigQuery (`ecobici_raw`), usando el modo **Sandbox** (sin cuenta de
  facturación, sin costo).
- Se cargó el CSV filtrado como tabla en BigQuery y se validó con una consulta
  simple (`SELECT * ... LIMIT 10`).

**Decisiones y aprendizajes:**
- Se optó por cargar el CSV directo a BigQuery en vez de pasar por Google
  Cloud Storage, para simplificar los primeros pasos (GCS se suma en la Fase 2).
- El modo Sandbox de BigQuery permite practicar sin tarjeta de crédito ni
  riesgo de costos, ideal para portfolio.
- Trabajar con un mes de datos en vez del año completo reduce la carga
  cognitiva al empezar, sin perder representatividad para el análisis.

**Cómo reproducirlo:**
1. Descargar el dataset de la fuente mencionada arriba.
2. Correr `scripts/01_carga_bigquery.py` para filtrar el mes deseado.
3. Subir el CSV resultante a un dataset de BigQuery vía la consola.

## Fase 2: Enriquecimiento con datos climáticos ✅

- Se obtuvieron datos climáticos diarios de Buenos Aires para enero 2024 mediante la API pública de Open-Meteo (endpoint histórico `/v1/archive`).
- Variables utilizadas: temperatura mínima/máxima diaria, precipitación acumulada y horas de lluvia.
- Se activó Google Cloud Storage y se creó un bucket (`ecobici-portfolio-nahir`) para almacenamiento intermedio.
- El CSV resultante se subió a GCS y luego se cargó a BigQuery en un nuevo dataset (`ecobici_clima`), separado del dataset de datos crudos (`ecobici_raw`).
- Notebooks:
  - `Notebooks/02_API.ipynb` — consulta a la API de Open-Meteo
  - `Notebooks/03_google-cloud-storage.ipynb` — subida del CSV a Cloud Storage
  - `Notebooks/04_carga_clima_bigquery.ipynb` — carga de la tabla a BigQuery