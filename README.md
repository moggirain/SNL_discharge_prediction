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

 ***************Exploratory_Data_Analysis*****************

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



*****************Train_test_sets_split*********************

I wrote a function train_validate_test_split to randomized set creation from the data to split the whole datasets into train, val and test sets with 70-15-15 ratio. 
THen I continued to split the independent and dependent variable, X, y using Split function. After splitting the train, validate and test dataset, I checked the class distribution to avoid the class imbalance issue. The ratio of 1 to 0 is 2.63:1. 

**********Modeling**************

-Logistic Regression 
-SVM
-Random Forest
-Neural Network 

**********Hyperparameter Tuning**************
-Grid Search 
