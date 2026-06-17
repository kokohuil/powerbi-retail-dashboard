# 📊 Power BI Retail Dashboard

Dashboard de pilotage commercial connecté à BigQuery via DirectQuery.
Construit sur les marts dbt du repo [`dbt-retail-analytics-pipeline`](https://github.com/<votre-username>/dbt-retail-analytics-pipeline).

---

## 🎯 Objectif métier

Fournir aux équipes commerciales et marketing un outil de **self-service analytics** pour piloter la performance retail en autonomie — sans dépendance à la DSI.

---

## 📐 Pages du dashboard

| Page | KPIs clés | Visuels |
|------|-----------|---------|
| **Performance Ventes** | CA, Panier Moyen, Croissance MoM, Nb Commandes | Line chart, KPI cards, Bar chart |
| **Connaissance Client** | Segments RFM, Récence, Satisfaction | Scatter plot, Donut, Table |
| **Analyse Produit** | Top catégories, Classification ABC, Part de CA | Treemap, Pareto, Waterfall |
| **Qualité Livraison** | Taux retard, NPS proxy, Délai moyen | Gauge, Heatmap, Map |

---

## 📁 Structure

```
powerbi-retail-dashboard/
├── queries/
│   ├── kpi_ventes_mensuel.sql      # Page Performance Ventes
│   ├── segmentation_client.sql     # Page Connaissance Client
│   └── top_categories.sql          # Page Analyse Produit
├── DAX_measures.md                  # Toutes les mesures DAX documentées
├── screenshots/                     # Captures du dashboard
└── README.md
```

---

## 🔗 Connexion BigQuery → Power BI

1. Ouvrir **Power BI Desktop**
2. **Obtenir les données → Google BigQuery**
3. Renseigner le **Project ID GCP**
4. Sélectionner les marts : `mart_sales_performance`, `mart_customer_rfm`, `mart_product_analytics`
5. Choisir le mode **DirectQuery** pour des données toujours fraîches

---

## 📐 Mesures DAX principales

Voir [`DAX_measures.md`](./DAX_measures.md) pour toutes les mesures documentées.

Exemples :
```dax
// Croissance CA mois sur mois
Croissance CA MoM =
VAR ca_actuel   = SUM(mart_sales_performance[ca_ttc])
VAR ca_precedent = CALCULATE(SUM(mart_sales_performance[ca_ttc]),
                    DATEADD(mart_sales_performance[order_month_label], -1, MONTH))
RETURN DIVIDE(ca_actuel - ca_precedent, ca_precedent, 0)

// Part du CA générée par les clients Champions
CA Champions =
CALCULATE(SUM(mart_customer_rfm[total_revenue]),
          mart_customer_rfm[rfm_segment] = "CHAMPION")
```

---

## 🚀 Reproduire ce projet

```bash
# 1. Télécharger le dataset Olist (Kaggle)
#    https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce

# 2. Charger dans BigQuery (voir bigquery-data-pipeline)

# 3. Exécuter le pipeline dbt (voir dbt-retail-analytics-pipeline)
dbt run && dbt test

# 4. Connecter Power BI Desktop aux marts BigQuery
```

---

## 👤 Auteur

**William KOUKOUI** — Data Analyst / Analytics Engineer
[LinkedIn](https://linkedin.com/in/william-koukoui) · [Email](mailto:william.koukoui.ai@gmail.com)
