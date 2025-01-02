# Case9_Project
Forcast Revenue
# Rossmann Store Sales Prediction

**This project aims to predict daily sales for Rossmann stores based on various influencing factors such as promotions, holidays, competition, and seasonal trends.**

## Dataset Description

The dataset consists of two CSV files:

* **rossman_store.csv:** Contains store-level information:
    * **Store:** Unique store ID
    * **StoreType:** Store model (a, b, c, d)
    * **Assortment:** Classification level (a, b, c)
    * **CompetitionDistance:** Distance from the nearest competitor (meters)
    * **CompetitionOpenSinceMonth:** Month the nearest competitor opened
    * **CompetitionOpenSinceYear:** Year the nearest competitor opened
    * **Promo2:** Continuous promotion program (0: not participating, 1: participating)
    * **Promo2SinceWeek:** Week the participating promotion program 2 starts
    * **PromoInterval:** Continuous time period Promo2 starts

* **rossman_train.csv:** Contains daily sales data:
    * **Sales:** Daily sales of the store
    * **Customers:** Number of customers on a specific day
    * **Open:** 0 if closed, 1 if open
    * **StateHoliday:** Indicates holidays (a, b, c) or 0 for none
    * **SchoolHoliday:** Whether the store is affected by school closures (0: No, 1: Yes)

The two datasets are merged based on the `Store` feature to create a new dataset named `data`.

## Problem Statement

The primary objective is to predict daily sales for 1115 Rossmann stores for the next 6 weeks, considering factors like:

* **Promotions:** Impact of regular and seasonal promotions on sales
* **Holidays:** Influence of public and school holidays on store operations
* **Competition:** Effect of competitor proximity and opening dates on sales
* **Seasonality:** Seasonal trends and patterns in sales

## Data Issues and Solutions

* **Time Series Problem:** 
    * **Problem:** The `Date` column is in the format %D-%M-%Y, which is not suitable for machine learning models.
    * **Solution:** Split the `Date` column into separate features for day, month, and year.

* **Missing Values:**
    * **Problem:** 
        * `CompetitionDistance` has 2642 NaN values.
        * `CompetitionOpenSinceMonth` and `CompetitionOpenSinceYear` have 323348 NaN values.
        * `Promo2SinceWeek`, `Promo2SinceYear`, and `PromoInterval` have 508031 NaN values.
    * **Solution:**
        * **CompetitionDistance:** 
            * If `CompetitionDistance` is NaN, `CompetitionOpenSinceMonth` and `CompetitionOpenSinceYear` are also NaN.
            * Delete rows with NaN values in `CompetitionDistance`.
            * Fill NaN values in `CompetitionOpenSinceMonth` and `CompetitionOpenSinceYear` with the mean of their respective columns.
        * **Promo2SinceWeek, Promo2SinceYear, PromoInterval:** 
            * If `Promo2` is 0, fill NaN values in these columns with 0.

* **Data Encoding:**
    * **StoreType, Assortment, StateHoliday:** Perform Label Encoding.
    * **PromoInterval:** Fill NaN values as "Unknown" and then perform encoding.

* **Outliers:**
    * **Sales and Customers:** Outliers can reflect special events. Use Robust Scaling to normalize and avoid bias during model training.
    * **CompetitionDistance:** Outliers (very large distances) may be valid.

* **Data Balancing:** 
    * Balanced the `Sales` target variable (details in the analysis file).

## Exploratory Data Analysis (EDA)

* **Revenue Impact:**
    * **StateHoliday:** Revenue is significantly lower on holidays due to store closures (97.07%).
    * **CompetitionDistance:** No significant impact on revenue observed.
    * **Promotions:** Revenue nearly doubles during promotions.

* **Seasonal Trends:** 
    * Promotion programs in January, April, July, and October are most effective.

* **Store Type:** 
    * Store type "a" (basic) has the highest revenue.

* **Day of the Week:** 
    * Monday has the highest sales, while Sunday has the lowest.

* **Continuous Promotion (Promo2):** 
    * Negligible impact on revenue.

## Model Selection

* **Linear Regression:** Simple, fast, suitable for linear relationships.
* **Random Forest Regression:** Handles non-linearity, resistant to overfitting, good with outliers.
* **Gradient Boosting Machines (GBM):** High performance, handles non-linearity well.
* **XGBoost:** Optimized GBM, handles overfitting and outliers effectively.
* **CatBoostRegressor:** Handles outliers, fast, less dependent on data normalization.

## Model Evaluation and Results

* **Random Forest:** Best performance with the lowest MAE, MSE, and highest R².
* **Gradient Boosting:** Good R² but higher MAE and MSE than Random Forest.
* **Linear Regression:** Worst performance with high MAE, MSE, and low R².
* **XGBoost:** Strong performance with lower MAE and MSE than Gradient Boosting.

## Conclusion

* Random Forest is the most suitable model for predicting Rossmann store sales based on the evaluation results.
* The model can accurately predict sales and explain nearly 99% of the variance in the data.
* Promotions have a significant positive impact on sales.
* Further analysis and potential improvements can be explored.

