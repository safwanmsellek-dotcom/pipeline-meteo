# 🌦️ Pipeline Météo Automatisé — 5 villes françaises

Pipeline de données complet (ETL) qui collecte, transforme et stocke les données météo en temps réel via l'API **Open-Meteo** (gratuite, sans clé).

---

## 🏗️ Architecture du pipeline

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐    ┌──────────────────┐
│   EXTRACT       │ →  │   TRANSFORM      │ →  │   LOAD          │ →  │   VISUALISE      │
│                 │    │                  │    │                 │    │                  │
│ API Open-Meteo  │    │ Enrichissement   │    │ SQLite          │    │ Graphiques       │
│ 5 villes        │    │ Agrégations      │    │ 2 tables        │    │ Rapport JSON     │
│ Données horaires│    │ Catégorisation   │    │ 1 vue SQL       │    │                  │
└─────────────────┘    └──────────────────┘    └─────────────────┘    └──────────────────┘
```

---

## 📊 Résultats (Mai 2024 — 30 jours)

| Ville | Temp. moyenne | Temp. max | Précipitations |
|-------|--------------|-----------|----------------|
| Marseille | 22.1°C | 32.4°C | 158 mm |
| Bordeaux | 20.1°C | 29.6°C | 208 mm |
| Lyon | 19.1°C | 28.4°C | 161 mm |
| Paris | 17.1°C | 28.1°C | 163 mm |
| Lille | 15.2°C | 24.8°C | 154 mm |

---

## 🛠️ Stack technique

- **Python** — requests, pandas, sqlite3, matplotlib
- **API** — Open-Meteo (REST, gratuite, sans clé)
- **Stockage** — SQLite (base de données locale)
- **Orchestration** — script autonome, prêt pour CRON / Airflow

---

## 📁 Structure du projet

```
projet-pipeline-meteo/
├── README.md
├── requirements.txt
├── src/
│   └── pipeline.py       ← script ETL principal
├── data/
│   └── meteo.db          ← base SQLite générée automatiquement
└── outputs/
    ├── pipeline_meteo.png ← visualisations
    └── rapport.json       ← rapport de synthèse
```

---

## 🚀 Lancer le pipeline

```bash
git clone https://github.com/TON_USERNAME/pipeline-meteo.git
cd pipeline-meteo
pip install -r requirements.txt
python src/pipeline.py
```

Le pipeline s'exécute et génère automatiquement la base SQLite, les graphiques et le rapport JSON.

**Pour automatiser l'exécution (CRON) :**
```bash
# Exécution toutes les heures
0 * * * * /usr/bin/python3 /chemin/vers/pipeline.py >> logs/pipeline.log 2>&1
```

---

## 🔍 Interroger la base SQLite

```python
import sqlite3, pandas as pd

conn = sqlite3.connect("data/meteo.db")

# Températures moyennes par ville
df = pd.read_sql("SELECT * FROM v_stats_ville", conn)

# Données horaires Paris
paris = pd.read_sql(
    "SELECT * FROM meteo_horaire WHERE ville='Paris' LIMIT 24",
    conn
)
```

---

## 📈 Visualisations

![Pipeline météo 5 villes françaises](outputs/pipeline_meteo.png)

---

## 🔄 Prochaines étapes

- [ ] Déploiement sur cloud (AWS Lambda ou GCP Cloud Functions)
- [ ] Orchestration avec Apache Airflow
- [ ] Alertes automatiques (pluie > 10mm, vent > 50 km/h)
- [ ] Dashboard Power BI connecté à la base SQLite

---

## 👤 Auteur

Projet réalisé dans le cadre d'un portfolio freelance Data Engineer.

**Contact :** [LinkedIn](https://linkedin.com/in/TON_PROFIL) · [GitHub](https://github.com/TON_USERNAME)
