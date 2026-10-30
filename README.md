# do-you-even-loaf
Final Project 1 for Data Mining CDS303. In this project we explore a dataset with business information...
| Variable | Description and unit | Data type | Proposed role | Rationale / preparation needed |
| --- | --- | --- | --- | --- |
| Standard_Year_Quarter_Code | Year and quarter of the recorded sales data (e.g., 20251 = 2025 Q1) | Numerical (int64) | Contextual variable | |
| Commercial_District_Name | Name of the commercial district in Seoul | Categorical (object) | Contextual variable / Exclude from clustering |  |
| Service_Business_Code | Unique code identifying each business category | int64 | int64 |  |
| Service_Business_Name | Type of business name | object | Row 1, Cell 3 |  |
| Monthly_Sales_Count | Monthly sales count to see the demand for gym and bakery | int64 |  |  |
