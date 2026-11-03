<div align="center">

# DO YOU EVEN LOAF? 🍞
### Does Seoul Bulk or Cut?

**CDS 303 - Scientific Data Mining | Final Team Project Session 1**

*Exploring fitness and bakery commercial activity across Seoul*
</div>

---

## Project Overview

**Team Members:** 
Santiago Rodriguez Gomez, Saebom Park

### Research Question
> Can Seoul's commercial districts be grouped based on fitness and bakery transaction activity, and what characteristics distinguish these groups?

**Goal:** Identify commercial district profiles through explainable cluster analysis and explore their relationship with regional obesity rates.

### Current Dataset:
**Seoul Commercial Trade Area Analytics: Estimated Sales by Commercial District (서울시 상권분석서비스 추정매출-상권)**
- **Provider:** Seoul Metropolitan Government / Seoul Credit Guarantee Foundation, via Seoul Open Data Plaza
- **Source:** https://data.seoul.go.kr/dataList/OA-15572/S/1/datasetView.do
- **Period:** 2025, Q1 to Q4
- **Size:** 85,732 rows × 55 columns (translated from Korean to English by our team)

The dataset reports estimated card sales for 1,577 commercial districts across Seoul, broken down by 62 categories. For each district, category and quarter it gives the total amount and number of transactions, plus the same figures split by weekday vs weekend, day of the week, time of day, customer gender, and customer age group. Districts are classified into four types:
- Alley commercial areas
- Developed commercial areas
- Traditional markets
- Tourist special zones

---

# Session 1

## Research Methodology
Our project follows a 4-step explainable cluster analysis framework.
1. **Data Preprocessing** - Prepare and organize the dataset.
2. **Dimensionality Reduction** - Reduce unnecessary complexity.
3. **Clustering** - Identify groups of commercial districts.
4. **Classification** - Explain the characteristics that distinguish the clusters.

As part of our goal we plan to compare the resulting clusters with supplementary obesity rate data.


---

## Paper Summary: Explainable Cluster Analysis
Method Article : A Comprehensive Framework for Explainable Cluster Analysis (2024) 

Application Article : Uncovering student profiles. An explainable cluster analysis approach to PISA 2022 (2024)

### 1. Purpose of the Four Step Framework

Step 1: Data Preprocessing
The purpose of data preprocessing is to ensure data quality and obtain reliable results. Since real world data is often incomplete, noisy, or inconsistent, this step involves handling missing values, detecting outliers, scaling data, and handling mixed data types.

Step 2: Dimensionality Reduction
Dimensionality reduction reduces the number of variables to a smaller set that people can interpret. It also helps avoid the curse of dimensionality: as the number of variables grows, distances between observations become less meaningful, which weakens distance-based methods like k-means. The framework uses Sparse PCA (SPCA) for numerical variables, where each component depends on only a few original variables so it is easier to name, and Multiple Correspondence Analysis (MCA) for categorical variables.

Step 3: Clustering
Clustering is an unsupervised learning method that groups similar observations together. Its purpose is to discover meaningful patterns within the dataset without using predefined class labels.

Step 4: Classification for Explainability
Classification uses the cluster labels generated in the previous step as target variables. This transforms the clustering results into a classification task, helping us understand which features distinguish the clusters. 

### 2. Why Do Clustering Results Need Interpretation?
Clustering is useful for extracting knowledge from large datasets because it can identify patterns without predefined labels. Interpretation is important because the results of machine learning models should be understandable to humans. By interpreting clusters, we can extract meaningful knowledge and improve decision-making processes. This helps different stakeholders translate clustering results into actionable insights.

### 3. How Can Classification Help Characterize Clusters?
After clustering, the generated cluster labels can be used as target variables in a classification model. If the classifier can predict the cluster membership accurately, the patterns it learns show what separates the clusters. Earlier work often used decision trees for this because they produce readable rules. The framework in the paper uses a pipeline with a feature selection (random forest and recursive feature elimination) hyperparameter tuning with cross-validation, and an XGBoost classifier, then explains the model with SHAP values. The trained classifier can also assign new observations to existing clusters. 

The paper also describes several approaches to interpreting clustering results:

#### 1) Cluster Comparison: 
Compare the mean values of variables within each cluster against the overall dataset to identify distinctive characteristics.
#### 2) Classification Model Performance: 
Evaluate how well the classification model predicts the generated cluster labels.
#### 3) SHAP Analysis:
Use SHAP values to examine how individual features contribute to the classification model's predictions.

By combining cluster comparison with SHAP analysis, we can better  understand and label the identified clusters.



---

## Variable Selection
The following variables were selected from our original dataset.

| Variable | Description and unit | Data type | Proposed role | Rationale / preparation needed |
| --- | --- | --- | --- | --- |
| Standard_Year_Quarter_Code | Year and quarter of the recorded sales data (ex 20251 = 2025 Q1) | Numerical (int64) | Contextual variable | Used to identify the time period. Data may need to be aggregated across quarters or filtered to a specific quarter|
| Commercial_District_Name | Name of the commercial district in Seoul | Categorical (object) | Contextual variable | Used to identify & group observations by commercial district |
| Service_Business_Code | Unique code identifying each business category | Numerical(int64) | Contextual variable | Used to identify & filter fitness-related + bakery-related businesses |
| Service_Business_Name | Type of business cateory | Categorical(object) | Contextual variable | Used to select relevant business categories. Supplementary to Service_Business_Code |
| Monthly_Sales_Count | Mnothly # of sales transactions for each business category in a commercial district | Numerical(int64) | Clustering input | Used to compare fitness and bakery commercial activity. Sales counts should be aggregated by district and business category, then scled to make districts comparable. |

## Variable Selection and Rationale

### 1. Relationship Between Clustering Variables & Research Question

Our research question is "Does seoul bulk or cut". 

We plan to use bakery sales share and gym store share as our main clustering variables. We will use Monthly_Sales_count as our main clustering input to compare transaction activity between bakery and fitness business categories. Service_Business_Code and Service_Business_Name will help us identify the relevant businesses, while Commercial_District_Name will be used to group the observations by location.

By comparing the transaction patterns, we aim to identify different commercial district profiles, such as fitness-oriented, bakery-oriented, or mixed districts.

We also plan to use supplementary obesity rate data to examine whether the resulting clusters show differences in local obesity rates.

### 2. Candidate Predictors for Classification

After generating the clusters, we plan to use classification to understand the characteristics that distinguish them.

Our main candidate predictors will be fitness-related and bakery-related transaction counts derived from Monthly_Sales_Count, organized by business category. This will allow us to examine how transaction activity differs across clusters.

Service_Business_Code and Service_Business_Name will help us identify the business categories used to construct these predictors.

The target variable will be the cluster labels generated during clustering. 

Since classification will use the same transaction information as clustering, its purpose will be to characterize the clusters rather than independently validate them.

We will separately compare the resulting clusters with obesity rate data as an external validation step. Obesity rates will not be used to generate the original cluster labels.

### 3. Variables to Be Excluded as Identifiers

Several variables will be excluded from direct clustering inputs:
- Standard_Year_Quarter_quarter_Code: Used to identify the time period.
- Commercial_District_Name: Used to identify and group commercial districts
- Service_Business_Code: Used to identify business categories
- Service_Business_Name: Provides business category information that overlaps with Service_Business_Code

These variables will remain useful for organizing, interpreting and evaluating the results.

### 4. Data Preparation Needs

#### Missing Values
There were no missing values in the original dataset.

#### Scaling
Monthly_Sales_Count may differ across commercial districts and business categories due to differences in transaction activity. We plan to aggregate transaction counts by district and business category. Depending on the results, we might need to consider scaling to prevent variables with larger numerical ranges from dominating the clustering results.

#### Categorical Encoding
Commercial_District_Name and Service_Business_Name are categorical variables, while Service_Business_Code represents categorical information through codes. Since these variables will mainly be used for identification and filtering, categorical encoding is not currently necessary for clustering. If categorical variables are later used directly in classification, we will consider appropriate encoding methods.

#### Data Aggregation
The original dataset contains observations organized by commercial district, business category, and quarter. We plan to organize the data at the commercial district level to compare fitness-related and bakery-related transaction activity.

We will also determine how to handle quarterly observations to ensure consistent comparison across districts.
** the original dataset that we have rightnow is 2025 Q1,Q2,Q3,Q4

---


## Supplementary Datasets
Data integration is planned for a later session.

### 1. Seoul Commercial District Areas (서울시 상권분석서비스 영역-상권)
- **Provider:** Seoul Metropolitan Government / Seoul Credit Guarantee Foundation
- **Source:** https://data.seoul.go.kr/dataList/OA-15560/S/1/datasetView.do
- **Relevant variables:** Commercial district code, borough (자치구) code and name, location coordinates
- **Contribution:** Our sales data has no borough column. This file links each district to its borough, which is needed to connect borough-level data such as obesity rates.
- **Connection:** Shared identifier (Commercial_District_Code).

### 2. Seoul Obesity Statistics (서울시 비만도 통계)
- **Provider:** Seoul Metropolitan Government, using the KDCA Community Health Survey
- **Source:** https://data.seoul.go.kr/dataList/DT201004E0500052001/S/2/datasetView.do
- **Relevant variables:** Borough, year, obesity rate (share of adults with BMI of 25 or higher)
- **Contribution:** Answers the "bulk or cut" part of our question. After clustering, we will compare obesity rates across clusters to see whether gym-oriented districts are in boroughs with lower obesity.
- **Connection:** Borough name (through the area file) and time period (2025 survey year). Direct merging is limited because every district in a borough receives the same value, so this dataset provides context for interpreting clusters rather than a clustering feature.

### 3. Seoul Stores by Commercial District (서울시 상권분석서비스 점포-상권)
- **Provider:** Seoul Metropolitan Government / Seoul Credit Guarantee Foundation
- **Source:** https://data.seoul.go.kr/dataList/OA-15577/S/1/datasetView.do
- **Relevant variables:** Year-quarter code, district code, business category code and name, number of stores, opening and closure counts, franchise store count
- **Contribution:** Measures bakery and gym supply, not just transactions. This helps tell apart districts with many small bakeries from districts with a few high-traffic ones, and franchise counts allow a comparison of chain vs independent bakeries across clusters.
- **Connection:** Same district codes, business category codes, and quarter codes as our main dataset, so it merges directly.

### 4. Seoul Fitness Center Licenses (서울시 체력단련장업 인허가 정보)
- **Provider:** Seoul Metropolitan Government (local government licensing data)
- **Source:** https://data.seoul.go.kr/dataList/OA-16142/A/1/datasetView.do
- **Relevant variables:** Business name, license date, closure date, business status, road address, coordinates
- **Contribution:** Counts licensed gyms per borough, adding context on fitness supply beyond the Sports Club and Sports Instruction categories in our sales data.
- **Connection:** No shared ID. We can count open gyms per borough using the borough in each address, and filter to gyms operating during 2025 using license and closure dates.

---

# Session 2
*Coming soon.*

# Session 3
*Coming soon.*

