# Readmission Risk in Diabetic Patients
This Python code is part of a larger project for our 2025 Spring Semester Group Project, which involves ultimately creating a comprehensive presentation that showcases our team's full data science pipeline. This repo showcases number 2, 3, 4, and 5 of the instructions below (Data Retrieval & Cleaning, Model Development & Selection, and Predictive Analysis, respectively) as they were my primary roles. 

## Goal
Identify which data elements are most strongly associated with the probability of readmission within 30 days.

## Dataset
[Diabetes 130-US Hospitals for Years 1999-2008](https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008) from the UC Irvine Machine Learning Repository
  * NOTE: All patients have type 2 diabetes in this dataset

## Full Project Instruction
In a group of 4 or 5, complete the following:
1. Technologies & Environment:
    * Identify and describe the tools and technologies used in your data science environment (e.g., Python, Jupyter Notebooks, cloud platforms, AI).
    * Network diagram
    * Data flow diagram
2. Data Retrieval & Cleaning:
    * Explain your data retrieval process, including the source and format of the data.
    * Detail your data cleaning steps: handling missing values, filtering, transformations, and feature engineering.
3. Model Development & Selection:
    * Discuss the models you evaluated (e.g., logistic regression, decision trees, random forests, neural networks).
    * Justify your choice of the final model based on performance metrics.
4. Predictive Analysis:
    * Identify the data elements and features that were most influential in predicting patient remediation within 30 days.
    * Include relevant statistical analyses and significance testing.
5. Visualizing Results:
    * Create clear and impactful visualizations (graphs, charts, heatmaps) to communicate your findings.
    * Summarize insights gained from the analysis and discuss potential real-world implications.


## Target Metric
* Primary: **AUC**
* Secondary: **recall**

## Code Overview
* Target column: **readmitted**
* Data Cleaning
    * Dropped features based on the following criteria
        * Not as relevant or contains protected health information (e.g. encounter_id, number_diagnoses)
        * Missing significant percentage of data
        * Dropping them boosted AUC performance
    * Removed duplicates
    * Replacing missing values with "Unknown"
    * Unified null-like values. We standardized categorical codes representing unknown or missing data (like 5 = NULL, 6 = Missing) into a single "Unknown" category for consistency.
    * Created two new features—**insulin_only** and **triple_therapy**—as they led to higher AUC
* Broke data up into training, validation, and test using a 70-15-15 split
* Treated class imbalance w/ **over-sampling**
* Tested 9 models for best AUC
* Conducted hyperparametering tuning on the best model to improve AUC

### Models Tested
<table style="font-size: 20px;">
  <tr>
    <td>Logistic Regression</td>
    <td>Decision Tree</td>
    <td>Random Forest</td>
  </tr>
  <tr>
    <td>KNN</td>
    <td>Linear SVC</td>
    <td>Gradient Boosting</td>
  </tr>
  <tr>
    <td>Catboost</td>
    <td>Stochastic Gradient Descent</td>
    <td>XGB</td>
  </tr>
</table>
    

## Findings
* Best Model: **Random Forest**
* Top 20 Most Influential Features
![top 20 features](imgs/top_20_features.png)


# Insights from Visualization
![diabetes readmission dashboard](imgs/dashboard_diabetes_readmission.png)

There are a lot of emergencies for inpatient visits and lots of patients readmitted within 30 days (primarily those who take diabetes medicine). For patients admitted for emergencies, thorough evaluation should be conducted and careful post-discharge planning should be done for patients before they are sent back home. Since a lot of patients taking diabetes medicine (and patients in general) are being readmitted, there are possibly issues with the quality of care. 

The average time in the hospital for most patients is higher than the average length of stay (4.5 to 5.5 days) in the US. We found it concerning that most patients stayed above the average, which may indicate underlying issues such as delays in receiving timely care, staff shortages, inefficiencies in hospital processes, priorities being placed in the wrong places, or broader concerns around the quality and coordination of care being provided.

The dataset contains patients who all have type 2 diabetes, and metformin is widely regarded as the first-line treatment for this condition. Given the high number of emergency admissions amongst type 2 diabetic patients who are not taking metformin, this raises concerns about whether these patients are being managed according to standard clinical guidelines. It may reflect issues such as poor medication adherence, health conditions that prevent patients from taking metformin, gaps in the continuity of care, or issues with insurance—all of which warrant further investigation.