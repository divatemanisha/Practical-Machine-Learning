# Practical-Machine-Learning
Coursera Project
Title: "PML"
Author: "Manisha Divate"
Date: "Saturday, September 26, 2015"
Output: html_document


###Our goal is predicting the class value for the givien data
The training data is  https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv

The testing data is https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv
### Step 1 : Load the training data using command 
 
##### data=read.csv('pml-training.csv',na.strings=c('','NA'))

>dim(data)
>[1] 19622   160

remove column with NA value. 
pml_clean_data=data[,!apply(data,2,function(x) any(is.na(x)) )]
dim(pml_clean_data)
[1] 19622    60
remove unwanted column remains only 53 variables
##### final_pml_data=pml_clean_data[,-c(1:7)]
dim(final_pml_data)
[1] 19622    53

Load the necessary packages
install.packages('randomForest')
library('randomForest')
install.packages('caret')
library('caret')
install.packages('e1071')
library('e1071')

### Step 2 Develop the Prediction Model
data_partition=createDataPartition(y=final_pml_data$classe, p=0.6, list=FALSE)
data_training =final_pml_data[data_partition,]
data_Testing=final_pml_data[-data_partition, ]

dim(data_training)
[1] 11776    53
dim(data_Testing)
[1] 7846   53

>model=randomForest(classe~., data=data_training, method='class')
>pred=predict(model,data_Testing, type='class')
>z=confusionMatrix(pred,data_Testing$classe)
>save(z,file='test.RData')

>load('test.RData')

z$table

To display accuracy
z$overall[1]
>1.  Accuracy 
>0.9924802 
Here accuracy of training data is 99.24% hence model build up is good and reliable model
##### Out of sample Error
>out of sample error is calculated as    100%  -   Accuracy  =100  -  99.248 = 0.751%
>missClass = function(values, predicted) {
  sum(predicted != values) / length(values)
}
>OOS_errRate = missClass(data_Testing$classe, pred)

##### OOS_errRate
>[1] 0.007519755


### Step 3 Testing the data

>test_data=read.csv('pml-testing.csv',na.strings=c('','NA'))
>final_test_data=test_data[,!apply(test_data,2,function(x) any(is.na(x)) )]
>test_data_pred=final_test_data[,-c(1:7)]

>predicted=predict(model,test_data,type='class')
>save(predicted,file='predicted.RData')

>The final prediction for the 20 ends up as:
> predicted
 1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 
 B  A  B  A  A  E  D  B  A  A  B  C  B  A  E  E  A  B  B  B 
Levels: A B C D E


