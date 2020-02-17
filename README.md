# SNL_discharge_prediction
predict where the patient will be discharged to before surgery 

# Problem Statement
After having a total joint surgery (either a total hip or total knee replacement), some patients must be discharged to a Skilled Nursing Facility (SNF) for recovery. These SNFs are very expensive and can be quite disruptive to patient’s lives. The goal of this project is to develop a model that can predict, before the surgery, where the patient will be discharged to. This is a binary prediction task: will the patient go to a snf or not.

# Data Processing 
The data preprocessing includes 9 steps: 

1) Impute missing data

   - drop variables that contains more than 70% missing values: 'Dx Code 2','Dx Code 3'
   - drop variable 'RF17_Has_CLD', which the value only contains 0
   - drop small amount of missing values in the follwoing varialbes: "INDEX_DISCH_DISP_NM","height","Reg Fsc 1"
   - fill the null values in "Reg Fsc 2" as other for a new level 

2) Binning variable age_yrs

   - age ranges from 20-90+, which is a wide range
   - Interest in knowing that bins age into 8 buckets: [(20, 29], (29, 39], (39, 49], (49, 59], (59, 69], (69, 79], (79, 89], (89, 99]]
   - Create a new variable: age_bins

3) Recoding variable height to cm
   - height with feet and inch is dirty and with wide range
   - convert height into numerical variable for computation
   - Impupte the NA values by mean after pre-processing 

4) Recoding diagnosis codes 
   - Used a helper function to check the count of diagnosis code 
   - Address processing the diagnosis code with the count > 1
   - Group the diagnosis code by ICD9 ICD10 and mapping 
   - Grouped diseases into 6 levels 

5) Recoding outcome variable 
   - Recoded the outcome variable into a binary varialbe
   - 1: SNF 0: Other levels

6) Construct new variables from ProcName
   - Constructed three variables: 
     1) ProcName_side: The side of surgery-left, right, both or unknown
     2) ProcName_pos: The position of surgery-knee or hip 
     3) ProcName_anterior: if used anterior for surgery

7) One-hot Encoding 
   - One hot encoding for the categorical variables: ['Service Date Year', 'Gender', 'State', 'Pat_EthGrp', 'Pat_Race', 'Provider', 'Reg Fsc 1', 'Reg Fsc 2', 'diag_1', 'ProcName_side', 'ProcName_pos', 'ProcName_anterior', 'age_bins']

8) Standarlization

   - Rescale the variables with wide range and variance to unit: height, weight and Length of Stay

9) Feature Engineering

   - Selected 20 numerical variables with 190 categorical variables 
   - Total 210 variables 

# Exploratory_Data_Analysis

 1) Distribution of label

    - 0: 3117 1:1170
    - 73.1% : 26.9%

 2) Distribution of numerical variables 

 	- Patients discharged to SNF tends to have longer stay than other patients
 	- Patients discharged tends to SNF  to be older than other patients
 	- Patients discharged to SNF tends to be be slightly lighter than other patients
 	- Patients discharged to SNF tends to be be slightly shorter than other patients

 3) Distribution of categorical variables 

    - Patients discharged to SNF are descreasing by years from 2013-2016
    - More female patients discharged to SNF than male patients 
    - Patients are mostly discharged to NYS SNF
    - Most patients discharged to SNF are not hispanic 
    - Most patients discharged to SNF are white or Caucasian
    - Most patients discharged to SNF are diagnosed with knee, bone and joint related disease
    - Most patients discharged to SNF had knee surgery 
    - Most patients discharged to SNF had surgery on both sides
    - Most patients discharged to SNF used anterior

 4) Correlation

    - No strong correlation (r >0.6) was found between variables
    - Previously went to SNF is highly correlated with the label discharged to SNF

 5) Normality

    - Length of stay is skewed
    - Weight, height and age_yrs are normally distributed


Please write up a README that includes a description of how you preprocessed (and split) the data, and run time instructions for how to train and test the models you developed. Please also include accuracy, precision, and recall for each of your models, as calculated on the testing set.

# Train_test_sets_split

I wrote a function train_validate_test_split to randomized set creation from the data to split the whole datasets into train, val and test sets with 70-15-15 ratio. 
THen I continued to split the independent and dependent variable, X, y using Split function. After splitting the train, validate and test dataset, I checked the class distribution to avoid the class imbalance issue. The ratio of 1 to 0 is 2.63:1. 

# Modeling

-Logistic Regression 
-SVM
-Random Forest
-Neural Network 

After running the logistic regression, SVM and Random Forest model for the provided dataset, the baseline accuracy, precision, and recall are shown as below: 

model:  Logistic Regression 
validation prediction result: 
validation accuracy result: 88.25301204819277
[[149  13]
 [ 26 144]]
 
              precision    recall  f1-score   support

           0       0.85      0.92      0.88       162
           1       0.92      0.85      0.88       170
   micro avg       0.88      0.88      0.88       332
   macro avg       0.88      0.88      0.88       332
weighted avg       0.89      0.88      0.88       332

test prediction result: 
test accuracy result: 88.06818181818183

              precision    recall  f1-score   support

           0       0.86      0.92      0.89       181
           1       0.91      0.84      0.87       171
   micro avg       0.88      0.88      0.88       352
   macro avg       0.88      0.88      0.88       352
weighted avg       0.88      0.88      0.88       352

model:  SVM
validation prediction result: 
validation accuracy result: 79.81927710843374
[[135  27]
 [ 40 130]]
 
              precision    recall  f1-score   support

           0       0.77      0.83      0.80       162
           1       0.83      0.76      0.80       170
   micro avg       0.80      0.80      0.80       332
   macro avg       0.80      0.80      0.80       332
weighted avg       0.80      0.80      0.80       332

test prediction result: 
test accuracy result: 80.68181818181817

              precision    recall  f1-score   support

           0       0.78      0.87      0.82       181
           1       0.85      0.74      0.79       171
   micro avg       0.81      0.81      0.81       352
   macro avg       0.81      0.80      0.81       352
weighted avg       0.81      0.81      0.81       352

model:  Ramdom Forest 
validation prediction result: 
validation accuracy result: 84.03614457831326
[[143  19]
 [ 34 136]]
 
              precision    recall  f1-score   support

           0       0.81      0.88      0.84       162
           1       0.88      0.80      0.84       170

   micro avg       0.84      0.84      0.84       332
   macro avg       0.84      0.84      0.84       332
weighted avg       0.84      0.84      0.84       332
test prediction result: 
test accuracy result: 82.95454545454545

              precision    recall  f1-score   support

           0       0.81      0.87      0.84       181
           1       0.85      0.78      0.82       171
   micro avg       0.83      0.83      0.83       352
   macro avg       0.83      0.83      0.83       352
weighted avg       0.83      0.83      0.83       352

For the neural network model, I adopted the script provided by the class. This network architect is a 4-layer NN: 1 input layer (96 nodes), two hidden layers (100 nodes and 10 nodes), and 1 output layer (2 nodes). I set the baseline model with SGD with default momentum, a learning rate of 0.5, and a batch size of 1, for 200 epochs. The baseline result is shown as below: 

By averaging the scores from the iteration between 30 to 80, the valid accuracy is 0.5, average precision is 0.5 and the average recall is 0.26. On test dataset, the result is 


# Hyper-parameter Tuning 

I. Random Forest and SVM

I tuned the parameter for Random Forest and SVM using grid search methods. As the previous session has already established the baseline parameter for these two models. 
The baseline parameter for SVM is {C:1, 'gamma': auto, 'kernel':'rbf'} , the precision, accuracy, and recall scores are shown above. 
The Random Forest baseline parameter is {n_estimators: 10, criterion: "mini", max_depth: None, min_samples_split: 2, max_features: "auto", min_samples_leaf: 1}, the precision, accuracy, and recall scores are shown above. 

Then I used grid search methods that exhaustively search over specified parameter values for an estimator. 

SVM

I constructed the hyper-parameters to be tuned are C, gamma and kernel, with the following values: 

    Cs = [0.001, 0.01, 0.1, 1, 10, 100, 1000]
    gammas = [0.001, 0.01, 0.1, 1]
    kernels = ["rbf", "poly","linear"]
Using the grid search methods, I identified the best parameters and results are below. 

Best parameters set found on validation set:

{'C': 0.1, 'gamma': 0.001, 'kernel': 'linear'}

Detailed classification report:

              precision    recall  f1-score   support

           0       0.86      0.94      0.90       181
           1       0.93      0.84      0.88       171
   micro avg       0.89      0.89      0.89       352
   macro avg       0.89      0.89      0.89       352
weighted avg       0.89      0.89      0.89       352

The whole process takes 95.38233 seconds. The tuning process increased the accuracy from 79.81% to 88.92% . 
The precision for class 0 increased from 0.77 to 0.86, and for class 1 increased from 0.83 to 0.93. 
The recall for class 0 increased from 0.88 to 0.94, and for class 1 increased from 0.80 to 0.84. 
The f1-score for class 0 increased from 0.80 to 0.90, and for class 1 increased from 0.80 to 0.88. 


Random Forest 

I constructed the hyper-parameters to be tuned are C, gamma and kernel, with the following values: 

    n_estimators = [2,4,8,16,32]
    max_depth = [2,4,8,16,32]
    max_features = ['auto', 'sqrt']
    min_samples_split = [2, 4, 6, 8]
    min_samples_leaf = [1, 2, 4, 6]

Using the grid search methods, I identified the best parameters and results are below. 


Best parameters set found on validation set:

{'criterion': 'entropy', 'max_depth': 16, 'max_features': 'sqrt', 'min_samples_leaf': 1, 'min_samples_split': 4, 'n_estimators': 50}

Detailed classification report:

              precision    recall  f1-score   support

           0       0.85      0.87      0.86       181
           1       0.86      0.84      0.85       171
   micro avg       0.86      0.86      0.86       352
   macro avg       0.86      0.85      0.85       352
weighted avg       0.86      0.86      0.86       352



The whole process takes 1053 seconds. The tuning process increased the accuracy from 84.03% to 85.5% . 
The precision for class 0 increased from 0.81 to 0.85, and for class 1 decreased from 0.88 to 0.86. 
The recall for class 0 decreased from 0.88 to 0.87, and for class 1 increased from 0.80 to 0.84. 
The f1-score for class 0 increased from 0.84 to 0.86, and for class 1 increased from 0.84 to 0.85. 

## NN Hyper Parameter Tuning

The NN hyper parameter tuning involves two stages: 
1) Hyperparameter Tuning 
2) Network Tuning 

1) Hyperparameter Tuning 
After setting the baseline for the NN, I started the hyper parameter tuning process by using loops to tuning three parameters: 
I tuned three parameters using three loops: learning rate, number of epochs, and batch size. Below are the values I constructed by magnitude. 
learn_rates = [0.001,0.005,0.01,0.5] 
num_epochs = [50,100,150,200]
batch_sizes = [1,16,32,64]
Optimizer: SGD and Adam 

2) Network Tuning 
After exporting the results, I plotted all results to get the training and validation accuracy plots, and selected the best model among the models, by finding the model with the best average accuracy over 30-80 epoch. By using this approach, I identified the best model is the model using Adam optimizer, with learning rate of 0.001, epoch as 64 and batch size with 64. The best average accuracy from this model is 87.03%. 

2.1 Early Stopping
Using the same approach, I set this hyper-parameter in the overfitting script and started without using the dropout and regularization, but with early stopping methods, the early_stopping_thresh = -50 and the early_stopping_num_epochs = 5. The validation accuracy drops to 82%. 
2.2 Dropout
I then tested only using the drop out method. I set the drop out rate as 0.2, and the validation accuracy reaches 86.84%. I continued to test setting the dropout rate as 0.25, and the validation result increased to 86.98%, dropout rate as 0.30 reached to 87.13%. When drop out arched to 0.35, the accuracy increased to 87.3%, valid loss 1.63. 
The best model was achieved with the dropout rate at 0.4 and the validation accuracy is 87.52%. 

2.3 Regularization 
I tested L2 regularization, and set the lambda to different magnitude, including 1, 0.1, 0.01, 0.05, and the best result yield from 0.01 is the validation accuracy reaching to 82.84%. 

Using the regularization methods seem to over regularize the model, the best model with drop out rate 0.4 yield 0.5 accuracy, so I used my best model without regularization methods, which is Adam_b64_ep_64_lr0.001

The best model is 

Accuracy = 0.8703
Recall = 0.8654970760233918
precision = 0.8705882352941177
Loss = 1.7884093672037125
