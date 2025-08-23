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

1. 'car_age': 2025 - 'model_year' of the car
2. 'fuel_type': 'fuel_type' is an existing column with missing values and junk characters. I replaced such missing/incorrect values with the fuel type extracted from the column 'engine' using regular expression. I filled up missing values further with (1) the mode from the group of 'brand_model' and then (2) the global mode value.
3. 'engine_hp', 'engine_liter' and 'engine_cylinder': I used regular expression to extract engine horsepower, engine liter and number of cylinders in engine from the column 'engine'.
   Also, filled up the missing values of each of these fields using (1) median from 'brand_model' and 'fuel_type' group (2) median from 'brand_model' group and (3) global median in that order.
4. 'transmission_type': I picked up transmission types- 'automatic', 'manual' or 'CVT' using regular expression from the column 'transmission'. Missing values were filled up using (1) mode from 'brand_model' group and finally (2) from global mode.
5. 'ext_col' and 'int_col': Missing values in these columns were filled up using (1) mode value from grouped by 'brand_model' and finally (2) from the global mode.


### Visual exploration of correlation of independent variables with 'price'

1. The scatter plot of 'price' vs. 'milage' and 'price' vs 'car_age' reveal a faint -ve correlation, which is somewhat expected.

<img width="1121" height="221" alt="image" src="https://github.com/user-attachments/assets/c8232862-02fe-42e1-b633-7b4a6454be31" />

2. The box plot of 'price' vs. other cardinal variables show prominent correlation in the price range 2,000 to 1,00,000. But the correlation is vague for the cars in very high price rance above 1,00,000.

<img width="1454" height="812" alt="image" src="https://github.com/user-attachments/assets/b103dc65-1fe0-4453-ba6d-7f957c341c17" />

<img width="1454" height="814" alt="image" src="https://github.com/user-attachments/assets/88fd60bb-5bba-4024-b364-644a3ec9f597" />


### Statistical exploration of correlation of independent variables with 'price'

1. I used Pearson Correlation to find linear relationship between 'price' and the numeric independent variables like 'milage','car_age','engine_hp','engine_liter' and 'engine_cylinder'. But none showed any significant linear relationship. 'car_age' and 'milage' has little higher -ve correlation coefficient with 'price' in price range up to 1,00,000.

<img width="1121" height="220" alt="image" src="https://github.com/user-attachments/assets/2bfbba54-3394-4f69-96b4-7de64e43b1ff" />

2. I used Spearman's Rank Correlation Coefficient to find if any non-linear relationship exists between 'price' and the independent numeric variables. But in the price range up to 1,00,000 the 'car_age' and 'milage' has a coefficient around -0.5. In the high price range above 1,00,000 none are actually significant.

<img width="1030" height="131" alt="image" src="https://github.com/user-attachments/assets/ed0b8271-6c5a-4809-a6fe-74c3232a7da2" />

3. I used f_oneway from scipy.stats to run ANOVA test between continuous target variable 'price' and independent categorical variables to find out whether any correlation exists.
   The outcome reveals something interesting- below price 1,00,000 the brand-model does not have much significance but above 1,00,000, brand_model comes out as the most significant feature in deciding price.

<img width="1061" height="122" alt="image" src="https://github.com/user-attachments/assets/b868b513-62bc-443d-b127-b82b0582cad3" />

4. I also used Mutual Info Regression for numeric and low cardinality variable to find how much important those are in deciding the price.
   In the low price category, price below 25,000 'milage' revealed the maximum score of 0.4. In the price range 25,000 to 1,00,000, again 'milage' has highest score of 0.3. But in the high price range category, above 1,00,000, none of the variables score more than 0.2.

<img width="1039" height="160" alt="image" src="https://github.com/user-attachments/assets/7bcaf618-9ece-4702-a334-b54210122b2e" />


### Encoding very high cardinality variables

The independent variables 'brand-model', 'ext_col' and 'int_col' have very high cardinality. And 'fuel_type' and 'transmission_type' have high variance in terms of frequency of occurance.
Hence, I am replacing these independent variables with the median of log of price after treating it with a smoothing factor.
I am taking the smoothing factor as, smoothing_factor=1/(1 + np.exp((50 - group_count)/100)), where group count is the size of each set of records the train dataset grouped by the column values.
And the encodede value is defined as, encoded_val = smoothing_factor*(median of log_price grouped by column values) + (1 - smoothing_factor)*(global median of log_price)

<img width="632" height="274" alt="image" src="https://github.com/user-attachments/assets/c46bc775-89f9-4c34-9760-640c53b64e21" />

I generated the mutual info scores between 'price' and these encoded values of these high cardinality variables, but the highest value shows around 0.26 for 'brand_model_score' for price range between 25,000 and 1,00,000.

<img width="1177" height="137" alt="image" src="https://github.com/user-attachments/assets/b1b03c91-2127-48b2-8c91-0bd727b288f9" />

So, as we see, none of the features show much strong correlation with 'price', yet all have certain contributions in predicting 'price'. Hence, I am keeping all the features for developing regression models.


### Segmenting the data based on 'price'

The target variable 'price' has huge outliars on the right side, i.e. the distribution of price is right skwed with a very long tail.

<img width="1159" height="314" alt="image" src="https://github.com/user-attachments/assets/154f88e9-cbfa-4803-8a1a-34994273ec0d" />

Such skeweness is supposed to cause huge errors in the regression outcome. 
Hence I am segmenting the data based on prices- 'low' price range with prices up to 25,000, 'mid' price range with prices between 25,000 and 1,00,000 and 'high' price range with prices above 1,00,000.

<img width="1086" height="146" alt="image" src="https://github.com/user-attachments/assets/689b62bd-22e9-42a6-ab70-594b7285e1e3" />

<img width="1091" height="150" alt="image" src="https://github.com/user-attachments/assets/b00dc1f0-a620-4b3e-beef-4a490c6b03b5" />

<img width="1091" height="212" alt="image" src="https://github.com/user-attachments/assets/03f6686b-0ee5-424b-a7f2-b699ed7da95f" />





