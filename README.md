## Data Pipeline

1. Training data has a row containing multiple subjects, separated by commas so that a single row is converted to multiple rows, this increased the explainability of the data set for the model and also makes the dataset a bit larger.
2. Groupby is done on year, month, and subject for getting the count of the number of papers published in that specific [month, year]
3. Data is split then into train and val dataset
    
    a. 1 type of dataset is created using one-hot encoded
    
    b. 1 type of dataset is created using only label encoding

This completes the data pipeline

## Machine learning pipeline

The dataset created is trained on the model selected, which is explained below as to which model and used and why

Rmse is calculated for the models when the training is completed.


### LSTM:

#### Why:

1. ``` Long Short-Term Memory (LSTM) ``` is designed to handle sequential data [time series data], where the previous data points are considered for making predictions about the future. It is capable of learning and retaining long-term dependencies in time series data, LSTM has memory cells that can store information about past inputs and outputs, which makes it ideal for processing sequential data and detecting patterns over time.

2. I have used 2 lstm versions in the code, 
    
    a.1st uses one hot encoding on the subject column and creates a data matrix with a one-hot encoding of the label encoded column, then this dataset is used to train the lstm model. 
    
    b. 2nd architecture firstly takes the subjects column into a label-encoded column which is further passed inside an embedding layer before going to the lstm layer, this embedding layer helps the model to learn an embedding space for all the subjects [total unique subjects; 109], as in the previous approach of one-hot encoding, the data matrix adds 109 columns [too many columns], so using this embedding layer approach we limit the dimensionality of our input data. This helps in better error optimization.


### XG-Boost
#### Why:

1. ```Ability to handle complex relationships```: XGBoost is capable of modeling complex relationships between the input features and the target variable. This is particularly useful for time series forecasting, where the target variable is typically influenced by many other variables.

2. ```Support for missing data```: XGBoost can handle missing data by assigning each missing value to a direction in the tree based on the values of the other features. This can be useful in time series forecasting, where missing values are common.

3. ```Speed and scalability```: XGBoost is a highly optimized algorithm that can be parallelized and distributed across multiple CPUs and GPUs. This makes it a good choice for large time series datasets or when multiple models need to be trained and evaluated. Additionally, XGBoost has a relatively fast training time compared to other machine learning algorithms.

### Performance of the algorithms
#### LSTM:
1. ```During Training```: training loss and val loss are taken into consideration, and monitoring those 2 helps us identify, how the model is performing. Both the metrics follow a similar pattern of reducing as the model training progresses, though the loss was still not able to converge to a small number.
2. ```During Testing:``` As the model is trained on RMSE, the score is taken into account while making the predictions on the held-out set.

#### XGBoost

I chose RMSE, which is used to measure the average difference between the predicted and actual values, squared and then square-rooted. It is a good metric to use when we want to penalize larger errors more heavily, as the squared term in the calculation magnifies larger errors. Additionally, RMSE is relatively easy to interpret, as it is expressed in the same units as the target variable.

