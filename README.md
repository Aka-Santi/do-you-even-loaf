# do-you-even-loaf
Final Project 1 for Data Mining CDS303. In this project we explore a dataset with business information...
| Variable | Description and unit | Data type | Proposed role | Rationale / preparation needed |
| --- | --- | --- | --- | --- |
| Standard_Year_Quarter_Code | Year and quarter of the recorded sales data (ex 20251 = 2025 Q1) | Numerical (int64) | Contextual variable | Used to identify the time period. Data may need to be aggregated across quarters or filtered to a specific quarter|
| Commercial_District_Name | Name of the commercial district in Seoul | Categorical (object) | Contextual variable | Used to identify & group observations by commercial district |
| Service_Business_Code | Unique code identifying each business category | Numerical(int64) | Contextual variable | Used to identify & filter fitness-related + bakery-related businesses |
| Service_Business_Name | Type of business cateory | Categorical(object) | Contextual variable | Used to select relevant business categories. Supplementary to Service_Business_Code |
| Monthly_Sales_Count | Mnothly # of sales transactions for each business category in a commercial district | Numerical(int64) | Clustering input | Used to compare fitness and bakery commercial activity. Sales counts should be aggregated by district and business category, then scled to make districts comparable. |
