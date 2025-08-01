# ARIMAX-Modelling-CO2

This project aims at modelling CO_2 emission by using an ARIMAX model using renewable energy data and other exogenuous features.

## Dataset
The renewable energy data is from the International Renewable Energy Agency (IRENA), and the CO_2 emission data along with the exogenuous variables are from Our World In Data. Although they both contain a large number of countries throughout many years, only the US data from 1965 and older are used because the renewable energy data is fairly incomplete before then. The features used are:
- year: Year of the observation
- population: Population of USA 
- gdp: $ in 2011 prices
- CO₂: Annual CO₂ emissions in million tons
- solar_generation: Solar energy generated in TWh
- wind_generation: Wind energy generated in TWh
- hydro_generation: Hydro energy generated in TWh
- other_generation: Other sources of clean energy in TWh

## Preprocessing
First, the two data sets are merged, only the relevant features are selected and it is split into 80-20 for train and test (where the 20% in test are the latest data). Noticing missing values in solar_generation and wind_generation in the training set, they are imputed with zero because the missing values represent they do not exist (i.e. zero). Then, as we do not care about the source of the clean energy, all of the clean energy generation features are combined into another feature: total_generation. Lastly, total_generation is discovered to have some outliers, in which case they are trimmed into into the third quartile. 
All of these preprocessing steps are also done onto the train set. However, the train set also has a year where gdp is missing. To prevent biased results, that observation is dropped instead of imputed.

## Modelling
Raw CO_2 emission is not stationary, as can be seen by the plot of CO_2 emission. After differencing once, CO_2 emission appears to be stationary, which is further confirmed with an ADF test. This means d = 1 in the ARIMAX model. Before determining p and q, multicollinearity in the exogenuous variables is checked for. Unfortunately all three features, population, gdp and total_generation, have high multicollinearity with each other, so only total_generation is kept. After, ACF and PACF plots of CO_2 emission accounting for the exogenuous feature total_generation are drawn and it is determined p = 1 and q = 0. With all the parameters and the cleaned data, the ARIMAX model is fitted with the training data.

## Evaluation of Results
Using MAE and MAPE to assess the model, the model performed very well. The MAE is 136.842 and MAPE is 4.16%, meaning the predictions are about 4% of the actual values. Here is a plot of the prediction against the actual values:
<img width="1177" height="562" alt="image" src="https://github.com/user-attachments/assets/bf797ea0-1068-42d4-914a-1393e67333fd" />
