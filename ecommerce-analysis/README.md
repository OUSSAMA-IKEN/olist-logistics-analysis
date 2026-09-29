# Bilan Data Cleaning

## What we have now

| Element                      |                             Value |
| ---------------------------- | --------------------------------: |
| Final rows                   |                           110,181 |
| Columns                      |                                21 |
| Delay rate (`est_en_retard`) |                              6.8% |
| Median distance              |                            432 km |
| Multivariate anomalies       | 2.0% (feature kept, not excluded) |
| Physical errors removed      |                                 8 |

The Data Cleaning milestone is officially complete. We now have a clean analytical table, a clear target variable, and geographically and logistically consistent features.

# Bilan EDA

## What we have now

| Element                    |                                                                        Value |
| -------------------------- | ---------------------------------------------------------------------------: |
| Main focus                 |                                              Exploratory Data Analysis (EDA) |
| Data quality status        |                                                 Clean and ready for analysis |
| Key variables studied      | Order date, delivery delay, payment, product category, seller zone, distance |
| Notable patterns           | Strong concentration of late deliveries in certain cities and product groups |
| Customer behavior insights |           Orders are mostly concentrated in a few key regions and categories |
| Operational signals        |     Logistics and distance variables show meaningful influence on delay risk |
| Recommendation status      |               Insights are ready to support feature engineering and modeling |

The EDA milestone is complete. We have explored the structure, quality, and relationships in the dataset to better understand the business drivers behind delivery performance and customer behavior.

## What we have done in this milestone

- We reviewed the cleaned dataset to understand its overall structure, distributions, and missing-value patterns.
- We analyzed the target variable and identified the main drivers of delivery delay, including logistics and geographic factors.
- We explored customer, product, and seller behavior to detect repeat patterns and segmentation opportunities.
- We examined correlations and distributions for key variables such as distance, order value, category, and delivery timing.
- We visualized the main anomalies and outliers to verify whether they were meaningful business signals or data issues.
- We extracted actionable insights to guide feature engineering and the machine learning phase.
- We validated that the business story is coherent before moving to predictive modeling.

# Bilan Feature Engineering

## What we have now

| Element                       |                                                  Value |
| ----------------------------- | -----------------------------------------------------: |
| Main focus                    |                             Feature Engineering for ML |
| Dataset status                |                                  Clean and transformed |
| Target variable               |                          `est_en_retard` (binary flag) |
| Geographic variables created  |      `customer_region`, `seller_region`, `meme_region` |
| Operational variables created | `densite`, `mois_a_risque`, `trimestre`, `distance_km` |
| Encoding                      |                          One-Hot on regional variables |
| Final feature quality         |                               Ready for model training |
| Data leakage risk             |                                   Reduced / controlled |
| Modeling readiness            |                                                   High |

The Feature Engineering milestone is complete. We transformed the cleaned dataset into a predictive-ready structure by building meaningful variables that reflect delivery behavior, geography, and logistics constraints.

## What we have done in this milestone

- We created geographic aggregation variables to group states into operational regions.
- We derived `meme_region` to capture whether the customer and seller belong to the same region.
- We calculated a `densite` metric from weight and volume to separate compact from bulky products.
- We extracted temporal signals such as `mois_a_risque` and `trimestre` to capture seasonal delay risk.
- We encoded categorical geographic variables with One-Hot encoding for machine learning compatibility.
- We excluded non-predictive identifiers and leakage-prone fields to keep the model valid and interpretable.
- We verified missing values, stabilized scales, and kept only business-relevant features.

## Key business impact

- Geographic proximity influences delivery risk and helps structure regional patterns.
- Product density captures operational differences between compact and volumetric shipments.
- Seasonal and timing variables make delay patterns more explicit for prediction.
- The final feature set is better aligned with the business logic behind logistics performance.

## Final status

The feature engineering stage is now complete and the dataset is ready to be used in the modeling phase. The resulting variables are more informative, more stable, and more suitable for predictive analysis than the raw operational data.
