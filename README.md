# Prediction-of-Used-Car-Prices

Hi friends! This is one of my very initial attempts in Machine Learning.

Given a set of train data about used cars, I tried to fit them into a regression model and predict the prices of used cars of the test dataset. 
(Data Source: Kaggle)

Description of the Train Dataset:

<img width="531" height="322" alt="image" src="https://github.com/user-attachments/assets/ee9d0abf-1c3a-418a-8bd3-cee004cb5d0f" />

Setting 'id' as index, here is a glimpse of the train dataset:

<img width="1105" height="218" alt="image" src="https://github.com/user-attachments/assets/22667d1d-146d-4d93-a61e-f13e07b8d990" />

### Findings from basic data exploration

The elementary exploration of data revealed the following:

1. The fields 'engine' and 'transmission' hold descriptive strings. But valuable numeric or categorical information can be derived from them.
2. The fields 'brand', 'model', 'ext_col' and 'int_col' have very high cardinality. Such high cardinality values require to be encoded to use them in the regression models.
3. There are some missing values in columns 'fuel_type', 'engine', 'transmission', 'ext_col', 'int_col', 'accident' and 'clean_title'. Since all of these features, except 'accident' and 'clean_title', practically depend on the brand and model of a car, the missing values can be filled up with mean, median or mode values grouped by 'brand' and 'model'.
4. The 'price' of the used cars have a very very wide range, strating from as low as 2,000 to around 3,00,000. The distribution of 'price' is also right skewed with a very long tail.

<img width="1104" height="429" alt="image" src="https://github.com/user-attachments/assets/b337dfab-bc15-4a34-b1ae-c187b2f2b962" />

### Extracting new features from existing features and filling up missing values

I derived the following features from existing columns:

1. 'car_age': 2025 - 'model_year' of the car
2. 'fuel_type': 'fuel_type' is an existing column with missing values and junk characters. I replaced such missing/incorrect values with the fuel type extracted from the string values in column 'engine' using regular expression. I filled up missing values further with 1> the mode from the group of 'brand_model' 2> the global mode value
3. 'engine_hp', 'engine_liter' and 'engine_cylinder': I used regular expression to extract engine horsepower, engine liter and number of cylinders in engine from the field 'engine'.
   Also, filled up the missing values of each of these fields using 1> median of 'brand_model' and 'fuel_type' group 2> median of 'brand_model' group and 3> global median in that order.
5. 
