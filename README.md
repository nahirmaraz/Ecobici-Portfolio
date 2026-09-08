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
- [ ] Fase 2: Enriquecimiento con datos climáticos
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