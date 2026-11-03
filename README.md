<div align="center">

# DO YOU EVEN LOAF? 🍞
### Does Seoul Bulk or Cut?

**CDS 303 - Scientific Data Mining | Final Team Project Session 1**

*Exploring fitness and bakery commercial activity across Seoul*
</div>

---

## Project Overview

### Research Question

> Can Seoul's commercial districts be grouped based on fitness and bakery transaction activity, and what characteristics distinguish these groups?
**Team Members:** Santiago Rodriguez Gomez, Saebom Park

**Dataset:** Seoul Commercial Trade Area Analytics

**Period:** 2025 Q1-Q4

**Goal:** Identify commercial district profiles through explainable cluster analysis and explore their relationship with regional obesity rates.

---

## Research Methodology
Our project follows a 4-step explainable cluster analysis framework.
1. **Data Preprocessing** - Prepare and organize the dataset.
2. **Dimensionality Reduction** - Reduce unnecessary complexity.
3. **Clustering** - Identify groups of commercial districts.
4. **Clasification** - Explain the characteristics that distinguish the clusters.

We also plan to compare the resulting clusters with supplementary obesity rate data.


---

## Paper Summary: Explainable Cluster Analysis
Method Article : A Comprehensive Framework for Explainable Cluster Analysis (2024)
Application Article : Uncovering student profiles. An explainable cluster analysis
approach to PISA 2022 (2024)

### 1. Purpose of the Four Step Framework

Step 1: Data Preprocessing
The purpose of data preprocessing is to ensure data quality and obtain reliable results. Since real world data is often incomplete, noisy, or inconsistent, this step involves handling missing values, detecting outliers, scaling data, and handling mixed data types.

Step 2: Dimensionality Reduction
Dimensionality reduction reduces the number of variables to a manageable level for human comprehension. It improves interpretability and helps avoid the curse of dimensionality, where the number of instance increases exponentially to achieve the same explanation ability when the number of variables increases.

Step 3: Clustering
Clustering is an unsupervised learning method that groups similar observations together. Its purpose is to discover meaningful patterns within the dataset without using predefined class labels.

Step 4: Classification for Explainability
Classification uses the cluster labels generated in the precious step as target variables. This transforms the clustering results into a classification task, helping us understand which features distinguish the clusters. 

### 2. Why Do Clustering Results Need Interpretation?
Clustering is useful for extracting knowledge from large datasets because it can identify patterns without predefined labels. Interpretation is important because the results of machine learning models should be understandable to humans. By interpreting clusters, we can extract meaningful knowledge and improve decision-making processes. This helps different stakeholders translate clustering results into actionable insights.

### 3. How Can Classification Help Characterize Clusters?
After clustering, the generated cluster labels can be used as target variables in a classification model. Decision trees are commonly used because they can produce decision rules that help explain why observations belong to certain clusters.

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

#### Supplementary Data Integration
We plan to incorporate an additional dataset containing obesity rates in Seoul. We will examine whether this dataset can be connected to our commercial district
