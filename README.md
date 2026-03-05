# NYC Taxi Trip Duration Prediction - PySpark Pipeline
Production-ready PySpark ML pipeline for predicting NYC taxi trip durations. Full ETL, feature engineering, GBT modeling, hyperparameter tuning, and evaluation – scalable for big data and cluster deployment
Full ETL, feature engineering, GBT modeling, hyperparameter tuning, and evaluation – scalable for big data and cluster deployment.

---

## 🚀 Project Overview

This project demonstrates a **PySpark pipeline** for predicting taxi trip durations using NYC Yellow Taxi data.  

Key highlights:

- **ETL** in PySpark (no Pandas)
- **Feature engineering**: time-based, cyclical, interaction features
- **Machine Learning**: Gradient Boosted Trees (GBT) using PySpark MLlib
- **Hyperparameter tuning**: TrainValidationSplit with 18 parameter combinations
- **Evaluation metrics**: RMSE, MAE, R²
- **Production-ready**: scalable to clusters, model saving/loading

---

## 🏗 Architecture

```
+-------------------+
|  Data Ingestion   |
| (Parquet / API)   |
+-------------------+
           |
           v
+-------------------+
|   ETL & FE        |
| (filtering,       |
| time & cyclical   |
| features,         |
| interactions)     |
+-------------------+
           |
           v
+-------------------+
|  Train/Test Split |
|   (80% / 20%)    |
+-------------------+
           |
           v
+-------------------+
| Hyperparameter    |
| Tuning (TVS)      |
| 18 param combos   |
+-------------------+
           |
           v
+-------------------+
| ML Pipeline (GBT) |
| StringIndexer,    |
| VectorAssembler,  |
| GBTRegressor      |
+-------------------+
           |
           v
+-------------------+
| Evaluation &      |
| Feature Importance|
| RMSE, MAE, R²     |
+-------------------+
```

---

## 📊 Evaluation Metrics

**On Test Set:**

| Metric | Value | Unit |
|--------|-------|------|
| ME     | 14.1  | sec  |
| MAE    | 219.2 | sec  |
| RMSE   | 387.8 | sec  |
| R²     | 0.695 | %    |

**Error by Pickup Hour:**

- Early morning (0–5h): model overestimates, up to 3–4 minutes
- Evening rush (16–17h): model underestimates ~1 min
- Systematic error indicates traffic peaks are not fully captured

**Relative Error (Prediction / Actual):**

- Mean: 1.114 → slight overestimation (~11%)
- Median: 1.05 → most trips predicted closely
- Max: 21.57 → outliers significantly affect long trips

---

## ⚡ Key Takeaways

- The GBT model captures **short-to-medium trips well**, but underperforms on rare long trips (heteroscedasticity)
- Observed **overfitting**:
  - Train R² = 0.836
  - Test R² = 0.695 → drop of 14 pp
  - Model likely too deep, too many iterations, or too small sample (1%)
- Highly **production-ready** for cluster deployment
- Advantages of full PySpark pipeline:
  - ✅ Scalable for Big Data
  - ✅ Distributed computing
  - ✅ Ready for Spark clusters
  - ✅ No Pandas overhead
- Limitations:
  - ⚠️ Fewer algorithms (GBT instead of CatBoost)
  - ⚠️ Performance on long trips can be improved

---


📂 Data Sources

NYC Yellow Taxi Trip Records (Parquet)

PDF data dictionary parsed via pdfplumber

📝 References

PySpark MLlib Documentation: https://spark.apache.org/docs/latest/ml-guide.html

NYC TLC Taxi Data: https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page
