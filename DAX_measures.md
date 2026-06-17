# 📐 Mesures DAX — Dashboard Retail

## Page Performance Ventes

```dax
// CA Total (période sélectionnée)
CA Total = SUM(mart_sales_performance[ca_ttc])

// Panier Moyen
Panier Moyen = AVERAGE(mart_sales_performance[panier_moyen])

// Croissance CA vs période précédente
Croissance CA MoM =
VAR ca_actuel = SUM(mart_sales_performance[ca_ttc])
VAR ca_precedent = CALCULATE(
    SUM(mart_sales_performance[ca_ttc]),
    DATEADD(mart_sales_performance[order_month_label], -1, MONTH)
)
RETURN DIVIDE(ca_actuel - ca_precedent, ca_precedent, 0)

// Taux de livraison à l'heure
Taux Livraison OK =
DIVIDE(
    SUMX(mart_sales_performance, mart_sales_performance[nb_orders] - mart_sales_performance[nb_late_deliveries]),
    SUM(mart_sales_performance[nb_orders]),
    0
)
```

## Page Connaissance Client

```dax
// Nb clients Champions
Clients Champions =
CALCULATE(
    COUNTROWS(mart_customer_rfm),
    mart_customer_rfm[rfm_segment] = "CHAMPION"
)

// CA généré par les Champions
CA Champions =
CALCULATE(
    SUM(mart_customer_rfm[total_revenue]),
    mart_customer_rfm[rfm_segment] = "CHAMPION"
)

// Score de satisfaction moyen
Satisfaction Moyenne = AVERAGE(mart_customer_rfm[avg_review_score])
```

## Page Analyse Produit

```dax
// Part des catégories TOP (classe A) dans le CA
Part CA Classe A =
DIVIDE(
    CALCULATE(SUM(mart_product_analytics[category_revenue]),
               mart_product_analytics[abc_class] = "A - TOP"),
    SUM(mart_product_analytics[category_revenue]),
    0
)
```
