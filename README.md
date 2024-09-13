
# **Volatile Organic Compounds (VOCs) Data Dnalysis**

## Table of Contents

- [**Volatile Organic Compounds (VOCs) Data Dnalysis**](#--volatile-organic-compounds--vocs--data-dnalysis--)
  * [Table of Contents](#table-of-contents)
  * [Problem Statement](#problem-statement)
  * [Background](#background)
  * [Goal](#goal)
  * [Methods](#methods)
  * [Results](#results)
    + [A Clinical Breathomics Dataset:](#a-clinical-breathomics-dataset-)
    + [Breast cancer-related VOCs:](#breast-cancer-related-vocs-)
  * [Recommendation](#recommendation)
  * [Risks and Assumptions](#risks-and-assumptions)
  * [Data Availability](#data-availability)

In this repository we explore 3 datasets of VOCs in order to showcase analytical methods that can be used to gain insight into their use. These include:

 1. [Breath Biopsy® OMNI® – Example
    Dataset](https://www.owlstonemedical.com/downloads/breath-biopsy-omni-dataset/)
 2. [A Clinical Breathomics Dataset](https://www.nature.com/articles/s41597-024-03052-2#Sec9)
 3. [Breast cancer-related VOCs](https://data.mendeley.com/datasets/h7fvhycmmn/1)

## Problem Statement
Each dataset presents its own problem:
 1. Breath Biopsy® OMNI®: What insights can we gain from breath samples compared to "blank" samples?
 2. A Clinical Breathomics Dataset: Can we use breathomics to identify which respiratory disease a patient has, and which are the key VOCs?
 3. Breast cancer-related VOCs: Can we use breathomics to identify patients with malignant breast cancer compared to benign, and the key VOCs?

## Background

 VOCs are chemical markers that reflect metabolic changes in the body, and analysing them can provide early disease detection. With non-invasive diagnostics, we can deliver a non-invasive, painless, and rapid diagnostic method that can identify disease signatures much earlier than conventional tests, which often require blood tests, biopsies, or imaging techniques.

## Goal

Using advanced data modelling techniques, we want to make gain insights of VOCs and their diagnostics capabilities. Ultimately, we want to find ways we can use these predictions to help diagnose in a non-invasive manner.

## Methods

The datasets are fairly small, of 166 (Breath Biopsy® OMNI® – Example), 121 (A Clinical Breathomics Dataset) and 476 (Breast cancer-related VOCs) examples. For the second dataset we have "Disease" as a response variable, making it a multi-class problem of asthma/bronchiectasis/COPD, whereas the third dataset the response variable is "Label", a binary category of benign/malignant. While we could classify the examples of the first dataset between "breath" and "blank", the reference article indicated that a simple threshold of 3 standard deviations from the blank samples would suffice, thus we employed that. The second and third dataset have patient data in addition to the VOCs, and we kept them in order to evaluate if the former are stronger predictors than the latter. 

We replaced the missing data with the mean of each class where needed, RandomOversampled rows to match the number of examples to the majority class, and scale each variable via standardisation. Subsequently, we did a 5-Kfold split in order to train the ML models including: Decision Tree-based,  Random Forest,  Gradient Boosting, K-Nearest Neighbours and Support Vector Machines. We judged our model based on Accuracy, Precision, Recall and F1 score in the binary classification, and just on accuracy for the multi-class classification.

## Results 

### A Clinical Breathomics Dataset:

The Random forest achieved an average accuracy of 95.83% across 5 fold. By utilising SHAP values, we have identified the main 5 VOCs from individuals with asthma, bronchiectasis, and chronic obstructive pulmonary disease:

| m/z | Potential VOCs                                                                                                        | CAS number | Molecular weight | Molecular formula | feature importance |
| --- | --------------------------------------------------------------------------------------------------------------------- | ---------- | ---------------- | ----------------- | ------------------ |
| 138 | 2-pentylfuran                                                                                                         | 3777-69-3  | 138.2069         |  C9H14O           | 0.098637           |
| 519 | 2,2,4,4,6,6,8,8,10,10,12,12,14,14-tetradecamethyl-1,3,5,7,9,11,13-heptaoxa-2,4,6,8,10,12,14-heptasilacyclotetradecane | 107-50-6   | 519.08           | C14H42O7Si7       | 0.064379           |
| 226 | hexadecane                                                                                                            | 544-76-3   | 226.445          |  C16H34           | 0.038067           |
| 120 | 1-ethyl-4-methylbenzene                                                                                               | 622-96-8   |  120.1916        |  C9H12            | 0.033673           |
| 128 | azulene                                                                                                               |  275-51-4  |  128.1705        |  C10H8            | 0.021945           |


They proved to be better biomarkers than physical descriptors such as sex,	age,	FVC PP,	FEV10 PP,	BH (cm)	BW (kg), and	BMI.

### Breast cancer-related VOCs:
The Random forest achieved an accuracy of 94.11%, 93.55% Precision, 98.16% Recall and 95.74% F1-score across 5 fold. By utilising SHAP values, we have identified the main 5 VOCs from individuals with malignant breast cancer:

| m/z | Potential VOCs | CAS number | Molecular weight | Molecular formula | feature importance |
| --- | ---------------- | ---------- | ---------------- | ----------------- | ------------------ |
| 105 | Pentanethiol | 110-66-7 | 104.214 | C5H12S(+H) | 0.047455 |
| 59 | Acetone | 67-64-1 | 58.079 | C3H6O | 0.01863 |
| 31 | formaldehyde | 50-00-0 | 30.031 | CH₂O | 0.012311 |
| 116 | Isobutyl acetate | 110-19-0 | 116.158 | C6H12O2 | 0.011847 |
| 42 | Acetonitrile | 75-05-8 | 41.05 | C₂H₃N | 0.011263 |

These are more relevant than patient characteristics (cancer history, family cancer history, drinking history, comorbidities, smoking history, BMI, etc), however age is still a very strong predictor.

## Recommendation

The models could be improved by exploring more complex models such as ensembled models such as XGBoost. A different feature engineering, such as different scaling or ratios among the variables, could improve results as well. Expanding the datasets sizes with bigger population samples will make these models more robust and reliable to deploy in a live diagnostics server.

## Risks and Assumptions
The results of this analysis should be compared with larger population sizes, after comparing the VOCs recorded with actual laboratory results of real life scenarios. Additionally, as we have not recorded the data ourselves, there might be errors unbeknownst to us regarding data quality, thus we advice not to utilise the findings of our analysis as a source of truth.

## Data Availability
Each dataset is described and are accessible from their respective papers, and the files we utilised for each project were:
 1. Breath Biopsy® OMNI® – Example:
    -Dataset: "Breath Biopsy OMNI example dataset.csv"
 2. A Clinical Breathomics Dataset:
	-Asthma_peaktable_ver3.csv
	-Bronchi_peaktable_ver3.csv
	-COPD_peaktable_ver3.csv
	-CBD_metadata_ver3.xlsx
 3. Breast cancer-related VOCs:
	 -Basic_clinic_info.xlsx
	 -All patients breathomics recordings saved as .txt in the breath-omics/ folder

