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
2. The fields 'brand', 'model', 'ext_col' and 'int_col' have very high cardinality.
3. There are some missing values in columns 'fuel_type', 'engine', 'transmission', 'ext_col', 'int_col', 'accident' and 'clean_title'.
4. The 'price' of the used cars have a very very wide range, strating from as low as 2,000 to around 3,00,000. The distribution of 'price' is also right skewed with a very long tail.

<img width="1104" height="429" alt="image" src="https://github.com/user-attachments/assets/b337dfab-bc15-4a34-b1ae-c187b2f2b962" />
