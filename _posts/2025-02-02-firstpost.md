# First Kaggle Competition
After graduating from Purdue University with a degree in Data Science, I realized that my portfolio lacked projects beyond the classroom. To address this, I decided to embark on a solo project to put my knowledge into practice. I aimed to apply what I had learned in a practical setting, focusing on model creation rather than a data engineering-focused task.

## Project Overview
For this project, I chose a dataset from Kaggle's Playground Series ([Competition Link](https://www.kaggle.com/competitions/playground-series-s5e2/overview)). This competition involves predicting the price of backpacks based on various attributes. The dataset includes multiple features describing different characteristics of the backpacks, and our goal is to develop a model that accurately estimates their prices.

## Data Exploration
For the data exploration phase of this project, I decided to start with a smaller subset of the data that was provided in a separate file. This subset consisted of 300,000 rows, representing approximately 7.5% of the entire training dataset. I believed this smaller sample was a good starting point for conducting a preliminary analysis and gaining a comprehensive understanding of the features and target variable.

The first step involved loading the data into a pandas DataFrame. Once the data was loaded, I visualized the distributions of all the columns. This involved plotting histograms for numerical variables and bar charts for categorical variables. The visualizations revealed that most columns were evenly distributed across categories for categorical variables and was mostly normally distributed for numeric variables (aside from the Weight Capacity which is multimodal).

<img src="/images/brand_bar.png" alt="Brand Bar Graph" width="300" height="200"> <img src="/images/color_bar.png" alt="Color Bar Graph" width="300" height="200">

<img src="/images/compartments_hist.png" alt="Compartments Histogram" width="300" height="200"> <img src="/images/laptop_compartment_bar.png" alt="Laptop Compartment Bar Graph" width="300" height="200">

<img src="/images/material_bar.png" alt="Material Bar Graph" width="300" height="200"> <img src="/images/sizes_bar.png" alt="Size Bar Graph" width="300" height="200">

<img src="/images/style_bar.png" alt="Style Bar Graph" width="300" height="200"> <img src="/images/waterproof_bar.png" alt="Waterproof Bar Graph" width="300" height="200">

<img src="/images/weight_hist.png" alt="Weight Capacity Histogram" width="300" height="200"> <img src="/images/price_hist.png" alt="Price Histogram" width="300" height="200">

Following the initial exploration, I examined a correlation matrix for the numerical variables. This matrix helped identify the relationships between the features and the target variable. However, the correlation matrix indicated that there were no particularly strong correlations worth noting between the numeric features and the target variable.

<img src="/images/correlation_matrix.png" alt="Correlation Matrix" width="400" height="300">

## Model Selection
The competition focuses on minimizing the Root Mean Squared Error (RMSE) for our model's performance. I began by training the XGBoost model with default parameters, except for n_jobs=-1 and enable_categorical=True. Unfortunately, the initial model's performance was subpar, with an RMSE of 38.88. A scatter plot of the predicted versus actual values revealed that the model's predictions had no significant correlation with the actual labels, tending instead to cluster around the mean.

Insert image here of performance

Recognizing signs of underfitting, I adjusted the hyperparameters to enhance model fitting and uncover meaningful signals in the data. I selected `'n_estimators': [50, 100, 200]`, `'max_depth': [3, 6, 9]`, and `'learning_rate': [0.1, 0.2]` for my parameter grid with `cv=5`. The grid search identified the optimal hyperparameters as `'learning_rate': 0.2`, `'max_depth': 3`, and `'n_estimators': 200`. However, upon evaluating the tuned model, the RMSE slightly increased to 38.9. To gain further insights, I plotted overlapping histograms comparing the model's predictions to the actual labels.

Insert histogram here

The histograms indicated that the model's predictions were still centered around the mean, displaying a normal distribution, whereas the actual labels were more uniformly distributed. Additionally, the increased histogram bin count highlighted a significant cluster of backpacks priced at the maximum value, which the model had failed to account for.

To compare the pre-tuned model with the tuned one, I examined feature importance. The reduced `max_depth` in the tuned model resulted in fewer features being used. The final model emphasized material, color, weight capacity (kg), and compartments.

Insert bar graph here

In contrast, the original model utilized all available features, with a similar focus on material, color, and compartments, but also included weight capacity (kg).

## Conclusion
