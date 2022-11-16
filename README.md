# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Project 2: Ames Housing Price Prediction

## Problem Statement

IowaGuru is a property agent company in United States based in Ames, Iowa. We are tasked to predict the prices of future listings so that we may give potential customers a quick and reliable estimate of their property value before they engage us to find a buyer. In addition, IowaGuru would also like to find out the key information to collect for future house listings. The model will be built using Ames Housing Dataset from 2006 to 2010.

During the data cleaning process & EDA, missing values are imputed, and graphs are plotted to identify outliers in the datasets, and addressed accordingly. During Pre-processing, ordinal features and nominal features are encoded. Split train-test and data transformations were done and Baseline Model were developed using Linear Regression, Lasso, Ridge, and ElasticNet.

When developing the Baseline Model, we found that Overall Quality, External Quality, and Ground Living Area exhibits a strong relationship with Sale Prices. Lasso Regression gave us the best results, with Ridge Regression as a close second in our Baseline Model. The Baseline Model's Lasso RMSE score is 24167.

During data processing and feature engineering, a new feature 'House Quality' were created by combining Kitchen Quality, Basement Quality, and Overall Quality features. The final regression model also incorporates hyperparameter tuning to achieve the best results. The final Lasso RMSE score is 21991.

From the analysis, the top 5 features that corresponds positively with SalePrices are: Ground Living Area, Overall Quality, Kitchen Quality, Basement Quality and External Quality. The top 5 features that corresponds negatively with SalePrices are: House Age, N.A. Mas Vnr Type, Detatched Garage Type, CBlock Foundation, and Lot Shape.

## Problem Statement

Ames is a city in Story County, Iowa, United States, located approximately 30 miles north of Des Moines in central Iowa. It is best known as the home of Iowa State University, with leading agriculture, design, engineering, and veterinary medicine colleges. Ames had a population of 66,427, making it the state's ninth largest city. ([*source*](https://en.wikipedia.org/wiki/Ames,_Iowa)).

IowaGuru is a property agent company in United States based in New York, and would like to expand their base in Ames, Iowa. As data scientist working in IowaGuru, We are tasked to predict the prices of future listings so that they may give potential customers a quick and reliable estimate of their property value before they engage them to find a buyer. In addition, IowaGuru would also tasked us to find out the key information to collect for future house listings.

The model will be built using [*Ames Housing Dataset*](https://www.kaggle.com/competitions/dsi-us-11-project-2-regression-challenge/data), and [*Data Dictionary*](http://jse.amstat.org/v19n3/decock/DataDocumentation.txt). In this analysis, we will be using Linear Regression, Lasso, Ridge, and ElasticNet will be developed. Model with highest Train/Test score, highest CrossVal score, as well as lowest RMSE score will then be selected to predict the housing price.

## Datasets

There are 2 datasets used provided: Training Set & Testing Set.

| No | Datasets | No. of Features | No. of Data | Sale Price Provided? |
| --- | --- | --- | --- | --- |
| 1 | Training Set | 80 | 2051 | Yes |
| 2 | Testing Set | 80 | 878 | No |

Please refer to the housing dataset: [*Ames Housing Dataset*](https://www.kaggle.com/competitions/dsi-us-11-project-2-regression-challenge/data) <br>
Please refer to data dictionary: [*Data Dictionary*](http://jse.amstat.org/v19n3/decock/DataDocumentation.txt) <br>

## Data Cleaning
During the data cleaning process & EDA, a lot of missing values were discovered. However, upon further investigations, they are not genuine missing, but instead should be N.A.. These features are: Alley, Bsmt Qual, Bsmt Cond, BsmtFin Type 1, BsmtFin Type 2, Fireplace Qu, Garage Finish, Garage Qual, Garage Cond, Pool QC, Fence, Misc Feature. There are 330 datas missing for 'Lot Frontage'. Based on the data dictionary, Lot Frontage are linear feet of street connected to property. Hence this simply means that there is no street connected to the property, hence value the missing value should be 0. 

## Feature Categorization
Features are first categorized into 3 categories: Nominal Feature, Numeric Feature, and Ordinal Feature. Example of Nominal Feature are such as Neighborhood, House Style, Sale Type, etc. These variables are to be encoded using One Hot Encoding [0, 1]. Numeric Features are such as Year Built, Bathrooms, Floor SF, etc. These variables are ready to be used in analysis. Ordinal features are such as: External Qualities, Basement Conditions, etc. These variables are to be encoded as per ordinal manner stated in data dictionary by integer mapping.

## EDA
After the missing values are imputed, graphs are plotted to identify outliers in the datasets, and addressed accordingly. Based on the initial findings, the top 5 features affecting SalePrice are: Overall Qual, Exter Qual, Gr Liv Area, Kitchen Qual, and Garage Area. These features are plotted against Sale Price to further inspect the data. There are 2 datas that are identified in outliers, seen in Gr Liv Area vs SalePrice, as well as Garage Area vs SalePrice. These houses are over 5000 square feet but priced at under $200,000. Hence, these 2 datas are removed from the analysis.

## Data Pre-Processing
In this stage, datas are split for validation / training purposes, with 75% of datas for Training, and 25% for Testing. The data are then scaled using sklearn StandardScaler.

## Baseline Model
Baseline Model is developed using Linear Regression, Lasso, Ridge, and ElasticNet Regression. The summary are as follows:

|No |Test Type | Train Score | Test Score | CrossVal R2 Score | RMSE Score | Remarks |
| --- | --- | --- | --- | --- | --- | --- |
|1. |Linear Regression | 0.936 | -9.8e+23 | -6.6e+22 | 7.7e+16 | Poor Performance |
|2. |Lasso | 0.929 | 0.903 | 0.908 | 24167 | To use for model tuning |
|3. |Ridge | 0.931 | 0.898 | 0.906 | 24718 | To use for model tuning |
|4. |ElasticNet | 0.929 | 0.903 | 0.908 | 24163 | Similar to Lasso (l1 ratio = 1) |

Features that has a strong relationship with Sale Prices are: Overall Quality, External Quality, and Ground Living Area.

## Feature Selection
- Feature 'HouseQual' is developed by multiplying 'Overall Quality', 'Kitchen Quality', and 'Basement Quality'
- Feature 'HouseArea*HouseQual' are created by multiplying 'HouseArea' with 'HouseQual'. This feature have 0.91 correlation value with SalePrice.
- Feature 'Bathroom' is developed by summarizing the total bathrooms in the house (including basement)
- Feature 'House Age' is developed by deducting Current Year with Year Built

## Final Model
The final model is developed with hyperparameter tuning using Lasso & Ridge Regression only, as they performed the best in Baseline Model.

|No | Model | Test Type | Train Score | Test Score | CrossVal R2 Score | RMSE Score | Remarks |
| --- | --- | --- | --- | --- | --- | --- | --- |
|1. | Tuned Model | Lasso | 0.942 | 0.920 | 0.925 | 21991 | Selected For Kaggle Submission |
|2. | Tuned Model | Ridge | 0.946 | 0.913 | 0.921 | 22854 | - |

Tuned Model has ~2% increase in Train/Test score, and CrossVal R2 Score. Although Ridge Model has slightly higher Train score, but it has higher RMSE score. Hence, Tuned Lasso Model is selected for Kaggle Submission as it has the lowest RMSE score. It should also be noted that the final model is slightly overfit (~2.2%).

From the final model, features that exhibits strong relationship with SalePrice are:
- Positive Correlations: House Area, House Quality (Overall Quality, Kitchen Quality, Basement Quality) and External Quality
- Negative Correlations: House Age, N.A. Mas Vnr Type, Detatched Garage Type, CBlock Foundation, and Lot Shape

## Conclusion
Ames Housing Price is best predicted using Lasso Regression with Polynomial Feature, with final RMSE score of 21991, and the top 5 features that positively drives the SalePrices are: Ground Living Area, Overall Quality, Kitchen Quality, Basement Quality and External Quality. The top 5 features that negatively drives the SalePrices are: House Age, N.A. Mas Vnr Type, Detatched Garage Type, CBlock Foundation, and Lot Shape.

## Recommendation & Limitation
- Model are not able to accurately predict housing above 400,000. Hence it is recommended to fine tune the model and expand the dataset to incorporate more expensive houses in the datasets.
- It should also be noted that the model is slightly overfit, hence some tweaking shall be done to improve model accuracy.
- There are still a lot of features that needs to be collected from clients in order to built the model, it is recommended to streamline some of the features in the future
- Some of the features are subjective, what one perceive as 'Excellent' might be percieved as 'Average' by others
- This model is only built on Ames housing dataset from 2006-2010 only. More recent datas should be included to better predict the current housing prices.
- Sales price can change drastically due external factors, such as: Interst Rates, Covid19, Recession, Wars, etc.