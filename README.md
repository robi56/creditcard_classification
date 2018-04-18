# creditcard_classification
Develop a classifier for predicating Credit  Card Approval 


```
#libraries  
library(C50)

## data reading 
```
data = read.csv('data/creditcard.csv')
print(head(data))
```

## data processing [The dataset contains categorical varibles, need categorical to numerical conversion] 

###Categorial Variables to Numeric 
```
must_convert<-sapply(data,is.factor)       # logical vector telling if a variable needs to be displayed as numeric
M2<-sapply(data[,must_convert],unclass)    # data.frame of all categorical variables now displayed as numeric
credit_card<-cbind(data[,!must_convert],M2) 
```
##Partitioning the data into traning and test by shuffing  
###https://stackoverflow.com/questions/17200114/how-to-split-data-into-training-testing-sets-using-sample-function
```
set.seed(101) # Set Seed so that same sample can be reproduced in future also
sample <- sample.int(n = nrow(credit_card), size = floor(.75*nrow(credit_card)), replace = F)
train <- credit_card[sample, ]
test  <- credit_card[-sample, ]

column_names = c("X0","X1.25","X01","X0.1","b","X30.83", "u", "g","w","v","t","t.1","f","g.1","X00202")
```
##Classification using c5.0 algorithm 
```
tree_mod <- C5.0(x =train[colum_names], y = as.factor(train$X.))
summary(tree_mod)
plot(tree_mod)
```

##Prediction 
```
predicted=predict(tree_mod, newdata = test[, colum_names])
```
###print(output_labels)
```
cm = as.matrix(table(Actual = test$X., Predicted = predicted)) # create the confusion matrix
print(cm)
```

##Evaluting the model performace
```
n = sum(cm) # number of instances
nc = nrow(cm) # number of classes
diag = diag(cm) # number of correctly classified instances per class 
rowsums = apply(cm, 1, sum) # number of instances per class
colsums = apply(cm, 2, sum) # number of predictions per class
p = rowsums / n # distribution of instances over the actual classes
q = colsums / n # distribution of instances over the predicted classes
```
###Accuracy 
```
accuracy = sum(diag) / n 
print("Accuracy: ")
print(accuracy)
```
### Per-class Precision, Recall, and F-1
```
precision = diag / colsums 
recall = diag / rowsums 
f1 = 2 * precision * recall / (precision + recall) 
evalution_stat=data.frame(precision, recall, f1)
print("Evalution Statistics")
print(evalution_stat)
```

### Macro-averaged Metrics
```
macroPrecision = mean(precision)
macroRecall = mean(recall)
macroF1 = mean(f1)
evalution_stat=data.frame(macroPrecision, macroRecall, macroF1)
print(macroPrecision)
print("Macro Evalution Statistics")
print(evalution_stat)
```


