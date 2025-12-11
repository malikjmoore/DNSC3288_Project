# Predict Future Sales Model Card

### Basic Information

* **Person or organization developing model**: Lia B, liab@gmail.com. Malik M, malikm@gmail.com. Aedan B, aedanb@gmail.com
* **Model date**: December 10, 2025
* **Model version**: 1.0
* **License**: MIT
* **Model implementation code**: [Final Big Data Project Code](https://github.com/malikjmoore/DNSC3288_Project/blob/main/Final_Big_Data_Project_Code.ipynb)

### Intended Use
* **Primary intended uses**: This model predicts future retail sales using historical data for time-series forecasting purposes in the educational context of an academic competition.
* **Primary intended users**: Students and instructors in the GWU DNSC 3288 course along with Kaggle competition hosts.
* **Out-of-scope use cases**: Any real-world business use for operational decision-making would be out of our scope.

### Training Data

* **Source of training data**: Kaggle, Predict Future Sales (https://www.kaggle.com/competitions/competitive-data-science-predict-future-sales/data)
* **How training data was divided into training and validation data**:
  * Training: Months 3-32
  * Validation: Month 33 (last month)
* **Number of rows in training and validation data**:
  * Training rows: 9,552,837
  * Validation rows: 238,172

* Data dictionary: 

| Name | Modeling Role | Measurement Level| Description|
| ---- | ------------- | ---------------- | ---------- |
|**date_block_num**| time index | int | monthly index to represent time (e.g., 0 = January 2013) |
| **shop_id** | ID | int | unique identifier for each shop |
| **item_id** | ID | int | unique identifier for each item |
| **item_cnt_month** | target | float | total units sold for an item in a shop during the month |
| **item_category_id** | input | int | identifier for the item category |
| **subtype_code** | input | int | label-encoded subcategory formed from the second part of the item category name (what comes after the hyphen) |
| **item_cnt_month_lag_1** | input | float | number of units sold of this item in the previous month |
| **item_cnt_month_lag_2** | input | float | number of units sold of this item 2 months prior |
| **item_cnt_month_lag_3** | input | float | number of units sold of this item 3 months prior |
| **date_avg_item_cnt_lag_1** | input | float | average sales across all items in the previous month |
| **date_item_avg_item_cnt_lag_1**| input | float | average sales for a particular item last month |
| **date_item_avg_item_cnt_lag_2**| input | float | average sales for a particular item 2 months ago |
| **date_item_avg_item_cnt_lag_3**| input | float | average sales for a particular item 3 months ago |
| **date_shop_avg_item_cnt_lag_1**| input | float | average sales across all items in a particular shop last month |
| **date_shop_avg_item_cnt_lag_2**| input | float | average sales across all items in a particular shop 2 months ago |
| **date_cat_avg_item_cnt_lag_1**| input | float | average sales across all items in a particular category last month |
| **date_shop_cat_avg_item_cnt_lag_1**| input | float | average sales across all items in a particular category in a particular shop last month |
| **date_item_city_avg_item_cnt_lag_1**| input | float | average sales of an item in a particular city last month |
| **delta_price_lag**| input | float | relative change in item price compared to historical average, based on the most recent non-zero lag |
| **month**| input | int | month of the year (Jan-Dec), derived from date_block_num |
| **item_shop_last_sale**| input | int | number of months since the last sale of a particular item in a particular shop (-1 if never sold) |
| **item_last_sale**| input | int | number of months since the item was last sold anywhere (-1 if never sold) |
| **item_shop_first_sale**| input | int | number of months since the item was first sold in a particular shop |
| **item_first_sale**| input | int | number of months since the item was first sold anywhere |

### Test Data
* **Source of test data**: Kaggle, Predict Future Sales (https://www.kaggle.com/competitions/competitive-data-science-predict-future-sales/data)
* **Number of rows in test data**: 214,200
* **State any differences in columns between training and test data**: Test data only includes the ID column and the target variable, item_cnt_month. Training data includes all predictor variables along with the target variable, item_cnt_month.

### Model details
* **Columns used as inputs in the final model**: 'date_block_num', 'shop_id', 'item_id', 'city_code', 'item_category_id', 'type_code', 'subtype_code', 'item_cnt_month_lag_1', 'item_cnt_month_lag_2', 'item_cnt_month_lag_3', 'date_avg_item_cnt_lag_1', 'date_item_avg_item_cnt_lag_1', 'date_item_avg_item_cnt_lag_2', 'date_item_avg_item_cnt_lag_3', 'date_shop_avg_item_cnt_lag_1', 'date_shop_avg_item_cnt_lag_2', 'date_cat_avg_item_cnt_lag_1', 'date_shop_cat_avg_item_cnt_lag_1', 'date_item_city_avg_item_cnt_lag_1', 'delta_price_lag', 'month', 'item_shop_last_sale', 'item_last_sale', 'item_shop_first_sale', 'item_first_sale'
* **Column(s) used as target(s) in the final model**: ‘item_cnt_month’
* **Type of model**: eXtreme Gradient Boosting, XGBRegressor 
* **Software used to implement the model**: Python, xgboost
* **Version of the modeling software**: current xgboost version = 3.1.2, Python version = 3
* **Hyperparameters or other settings of your model**: 
```
model = XGBRegressor(
    max_depth=8,
    n_estimators=1000,
    min_child_weight=300,
    colsample_bytree=0.8,
    subsample=0.8,
    eta=0.3,
    seed=42,
    eval_metric="rmse",
    early_stopping_rounds = 10)
```
### Quantitative Analysis

* Metrics used to evaluate your final model: RMSE
* Final Values:
  * Training: 0.8423
  * Validation: 0.9194
  * Test: 0.9231
* Plots:

#### Feature Importance

![Feature Importance](Feature%20Importance%20Graph.png)

Figure 1. Feature Importance graph for inputs.

### Ethical Considerations
* Potential negative impacts of using the model:
  * Math or software problems:
      * Incorrectly filtering or splitting the data. This could be a software problem when using lag features, as future information can leak into training data. This would give greater performance than what the model should actually be able to predict.
      * The model relies on many mean-encoded and lag features, making it susceptible to less accuracy over time. Using features such as these rely on historical data, meaning this model will not account for changing conditions. You cannot change a pattern of behavior through training a machine learning model based on that same pattern of behavior. 
  * Real-world risks: 
      * Managers of retail stores and suppliers that rely on forecasts would be primarily affected by inaccurate predictions. An inaccurate model would also harm consumers through stock shortages or overstocking as a result of incorrect forecasts.
      * If the model produced an incorrect forecast, an underprediction could lead to a stock shortage while an overprediction could lead to higher costs and spoilage.
      * The model is likely to produce incorrect forecasts during periods of changing market conditions, such as holidays, promotions, or other changes in external factors.
      * The model uses historical data for predictions and will predict based on these trends, assuming future demand behaves the same as the historical trend. Disruptions and changing conditions would result in inaccurate predictions. 
* Potential uncertainties relating to the impacts of using the model:
  * Math or software problems
      * The model relies on specific hyperparameters and inputs. It would produce different predictions if any of them were to be changed, affecting RMSE values.
      * There is no way to verify that this is the optimal solution/model. Different configurations or predictors could produce better results. 
  * Real-world risks: 
      * The model is unable to capture behavioral changes or trends among consumers. The model has no awareness of any sort of external event, trend, or marketing strategy and would produce inaccurate predictions if they affected sales.
      * The model is an incomplete representation of the real retail environment, as it lacks all relevant factors that would influence the target. This could include successful marketing campaigns, competitor activity, macroeconomic changes/disruptions, and consumer preference shifts. 
* Unexpected results
  * One possible unexpected result that could arise from the use of this model is predictions of zero for new products, as it would not have the historical data necessary to produce values for the lag variables.
  * Another unexpected result could be the delayed reactions to change. Many of the lag variables are for the past 2 or 3 months, meaning it would take a longer period for demand spikes to be reflected in model predictions. 
