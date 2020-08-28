---
title: "Predicting exercise types"
author: "Amarins van de Voorde"
date: "2020-08-28"
output: 
  html_document: 
    keep_md: yes
---
### Synopsis
In this article, a dataset regarding personal activity is investigated. 6 participants have taken measurements while performing barbell lifts, with accelerometers on the belt, forearm, arm, and dumbell. The data of these 4 accelerometers has been recorded, as well as a statement whether the exercise was performed correctly or incorrectly and in the latter case, in what way. This exercise classe has been recorded in the 'classe' variable. The goal of this article is to analyse the dataset and set up a model that will correctly predict the exercise classe for a second dataset that doesn't contain the classe variable.
For predicting the classe types, 3 prediction methods have been tested: prediction with a tree, prediction with boosting and prediction based on random forests. The resulting predictions should have an accuracy of at least 95%.
Both the second and the third prediction method, based on boosting and random forest, respectively, have an accuracy of over 95%. Both models have been used to predict the classe type for the validation dataset. The predicted classe types have been compared to the real values, by submitting them in the course prediction quiz. Both the second and the third model have a score of 20/20 correct predictions. 


### Processing the data
#### Loading the dataset
The dataset consists of a training dataset and a testing dataset. The latter doesn't contain the _classe_ variable and has been used for validation. Hence, the training dataset has been split into a training and testing dataset. Code for loading the data is included in Appendix A01.









#### Processing the datasets
Some exploratory analysis was done on both the training and validation datasets, to investigate what kind of data has been included in the dataset. Full analyses have been included in appendix A02. 
The analyses show that both datasets contain almost the same columns. The only difference is that training contains column _classe_ and testing contains column _problem_id_ instead.
It appears that the _classe_ variable has been stored as a character. It will be converted to a factor variable, for easier processing.  
Further analysis shows that quite a number of columns don't seem to contain data. Looking at the full data frame confirms that these columns only contain data when field _new_window_ has value "yes". The headers of these columns seem to refer to statistical values, like average, sum or skewness, rather than to measured values. It appears as though the rows with _new_window_ value set to "yes" contain statistics. All these columns, as well as the "yes" rows have been excluded from the dataset, as the values are sparse and most likely dependent on the original measures.  
The first 7 columns of  the dataset seem to contain information about the measurement, rather than measured values, i.e. name of the participant, moment of exercise, etc. As these won't be relevant to the prediction, these columns have been removed from the datasets as well.
The resulting training, testing and validation dataset only contain measured values on the exercise and the _classe_ variable. 





### Fitting models
As the goal is to predict in which manner the participants did their exercise, the model will have to predict _classe_. Desired accuracy of the model is 95% or higher. Code for all 3 models has been included in appendix A04.

The first fitted model is a tree. The accuracy is only 49%, so a different model is required.

```
##  Accuracy 
## 0.4899021
```

![](AssignmentProject_files/figure-html/Model1-1.png)<!-- -->

The second fitted model uses boosting. Its accuracy is about 96%, which is a lot better. We will see if we can find a model with even higher accuracy.

```
##    user  system elapsed 
##   28.64    0.30  152.03
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 1347   29    0    1    1
##          B    8  879   16    3   12
##          C    3   23  813   20    7
##          D    5    0   10  753   10
##          E    2    1    3    8  849
## 
## Overall Statistics
##                                           
##                Accuracy : 0.9663          
##                  95% CI : (0.9608, 0.9712)
##     No Information Rate : 0.2842          
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.9573          
##                                           
##  Mcnemar's Test P-Value : 6.535e-05       
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.9868   0.9431   0.9656   0.9592   0.9659
## Specificity            0.9910   0.9899   0.9866   0.9938   0.9964
## Pos Pred Value         0.9775   0.9575   0.9388   0.9679   0.9838
## Neg Pred Value         0.9947   0.9864   0.9926   0.9920   0.9924
## Prevalence             0.2842   0.1940   0.1753   0.1634   0.1830
## Detection Rate         0.2804   0.1830   0.1693   0.1568   0.1768
## Detection Prevalence   0.2869   0.1911   0.1803   0.1620   0.1797
## Balanced Accuracy      0.9889   0.9665   0.9761   0.9765   0.9812
```

The third fitted model is based on random forests. Its accuracy is about 99.5% and should therefore do well in predicting the classe type of the validation set correctly. We have to be wary of such a high accuracy percentage, as it might indicate overfitting of the model.

```
##    user  system elapsed 
##   63.12    0.46  430.02
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 1365    4    0    0    0
##          B    0  926    2    0    1
##          C    0    2  836    8    0
##          D    0    0    4  777    0
##          E    0    0    0    0  878
## 
## Overall Statistics
##                                           
##                Accuracy : 0.9956          
##                  95% CI : (0.9933, 0.9973)
##     No Information Rate : 0.2842          
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.9945          
##                                           
##  Mcnemar's Test P-Value : NA              
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            1.0000   0.9936   0.9929   0.9898   0.9989
## Specificity            0.9988   0.9992   0.9975   0.9990   1.0000
## Pos Pred Value         0.9971   0.9968   0.9882   0.9949   1.0000
## Neg Pred Value         1.0000   0.9985   0.9985   0.9980   0.9997
## Prevalence             0.2842   0.1940   0.1753   0.1634   0.1830
## Detection Rate         0.2842   0.1928   0.1741   0.1618   0.1828
## Detection Prevalence   0.2850   0.1934   0.1761   0.1626   0.1828
## Balanced Accuracy      0.9994   0.9964   0.9952   0.9944   0.9994
```



### Results
Both the second model, based on boosting, and the third model, based on random trees, will be used to predict the classe type for the validation dataset. We will show the resulting predictions as well as the number of correct predictions for each set. For comparative purposes, the prediction results for model 1 have also been included.
Code for the predictions is included in appendix A05. The resulting predictions are:

```
## [1] "Score for model 1:  8"
```

```
## [1] "Score for model 2:  20"
```

```
## [1] "Score for model 3:  20"
```

### Source of the data
The data for this project is the _Weight Lifting Exercise Database_, gathered from http://web.archive.org/web/20161224072740/http:/groupware.les.inf.puc-rio.br/har. 


### Appendix
#### A01 Loading packages and data; creating partitions

```r
list.of.packages <- c("caret","rattle","randomForest","klaR","doParallel","parallel")
new.packages <- list.of.packages[!(list.of.packages %in% installed.packages()[,"Package"])]
if(length(new.packages)) install.packages(new.packages)
invisible(lapply(list.of.packages,require,character.only=TRUE))
```


```r
train_url="https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv"
test_url="https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv"
train<-read.csv(train_url)
validation<-read.csv(test_url)
```


```r
inTrain<-createDataPartition(y=train$classe,p=0.75,list=FALSE)
training<-train[inTrain,]
testing<-train[-inTrain,]
```

#### A02 Exploratory analysis

```r
head(training,n=3L)
```

```
##   X user_name raw_timestamp_part_1 raw_timestamp_part_2   cvtd_timestamp
## 1 1  carlitos           1323084231               788290 05/12/2011 11:23
## 3 3  carlitos           1323084231               820366 05/12/2011 11:23
## 4 4  carlitos           1323084232               120339 05/12/2011 11:23
##   new_window num_window roll_belt pitch_belt yaw_belt total_accel_belt
## 1         no         11      1.41       8.07    -94.4                3
## 3         no         11      1.42       8.07    -94.4                3
## 4         no         12      1.48       8.05    -94.4                3
##   kurtosis_roll_belt kurtosis_picth_belt kurtosis_yaw_belt skewness_roll_belt
## 1                                                                            
## 3                                                                            
## 4                                                                            
##   skewness_roll_belt.1 skewness_yaw_belt max_roll_belt max_picth_belt
## 1                                                   NA             NA
## 3                                                   NA             NA
## 4                                                   NA             NA
##   max_yaw_belt min_roll_belt min_pitch_belt min_yaw_belt amplitude_roll_belt
## 1                         NA             NA                               NA
## 3                         NA             NA                               NA
## 4                         NA             NA                               NA
##   amplitude_pitch_belt amplitude_yaw_belt var_total_accel_belt avg_roll_belt
## 1                   NA                                      NA            NA
## 3                   NA                                      NA            NA
## 4                   NA                                      NA            NA
##   stddev_roll_belt var_roll_belt avg_pitch_belt stddev_pitch_belt
## 1               NA            NA             NA                NA
## 3               NA            NA             NA                NA
## 4               NA            NA             NA                NA
##   var_pitch_belt avg_yaw_belt stddev_yaw_belt var_yaw_belt gyros_belt_x
## 1             NA           NA              NA           NA         0.00
## 3             NA           NA              NA           NA         0.00
## 4             NA           NA              NA           NA         0.02
##   gyros_belt_y gyros_belt_z accel_belt_x accel_belt_y accel_belt_z
## 1            0        -0.02          -21            4           22
## 3            0        -0.02          -20            5           23
## 4            0        -0.03          -22            3           21
##   magnet_belt_x magnet_belt_y magnet_belt_z roll_arm pitch_arm yaw_arm
## 1            -3           599          -313     -128      22.5    -161
## 3            -2           600          -305     -128      22.5    -161
## 4            -6           604          -310     -128      22.1    -161
##   total_accel_arm var_accel_arm avg_roll_arm stddev_roll_arm var_roll_arm
## 1              34            NA           NA              NA           NA
## 3              34            NA           NA              NA           NA
## 4              34            NA           NA              NA           NA
##   avg_pitch_arm stddev_pitch_arm var_pitch_arm avg_yaw_arm stddev_yaw_arm
## 1            NA               NA            NA          NA             NA
## 3            NA               NA            NA          NA             NA
## 4            NA               NA            NA          NA             NA
##   var_yaw_arm gyros_arm_x gyros_arm_y gyros_arm_z accel_arm_x accel_arm_y
## 1          NA        0.00        0.00       -0.02        -288         109
## 3          NA        0.02       -0.02       -0.02        -289         110
## 4          NA        0.02       -0.03        0.02        -289         111
##   accel_arm_z magnet_arm_x magnet_arm_y magnet_arm_z kurtosis_roll_arm
## 1        -123         -368          337          516                  
## 3        -126         -368          344          513                  
## 4        -123         -372          344          512                  
##   kurtosis_picth_arm kurtosis_yaw_arm skewness_roll_arm skewness_pitch_arm
## 1                                                                         
## 3                                                                         
## 4                                                                         
##   skewness_yaw_arm max_roll_arm max_picth_arm max_yaw_arm min_roll_arm
## 1                            NA            NA          NA           NA
## 3                            NA            NA          NA           NA
## 4                            NA            NA          NA           NA
##   min_pitch_arm min_yaw_arm amplitude_roll_arm amplitude_pitch_arm
## 1            NA          NA                 NA                  NA
## 3            NA          NA                 NA                  NA
## 4            NA          NA                 NA                  NA
##   amplitude_yaw_arm roll_dumbbell pitch_dumbbell yaw_dumbbell
## 1                NA      13.05217      -70.49400    -84.87394
## 3                NA      12.85075      -70.27812    -85.14078
## 4                NA      13.43120      -70.39379    -84.87363
##   kurtosis_roll_dumbbell kurtosis_picth_dumbbell kurtosis_yaw_dumbbell
## 1                                                                     
## 3                                                                     
## 4                                                                     
##   skewness_roll_dumbbell skewness_pitch_dumbbell skewness_yaw_dumbbell
## 1                                                                     
## 3                                                                     
## 4                                                                     
##   max_roll_dumbbell max_picth_dumbbell max_yaw_dumbbell min_roll_dumbbell
## 1                NA                 NA                                 NA
## 3                NA                 NA                                 NA
## 4                NA                 NA                                 NA
##   min_pitch_dumbbell min_yaw_dumbbell amplitude_roll_dumbbell
## 1                 NA                                       NA
## 3                 NA                                       NA
## 4                 NA                                       NA
##   amplitude_pitch_dumbbell amplitude_yaw_dumbbell total_accel_dumbbell
## 1                       NA                                          37
## 3                       NA                                          37
## 4                       NA                                          37
##   var_accel_dumbbell avg_roll_dumbbell stddev_roll_dumbbell var_roll_dumbbell
## 1                 NA                NA                   NA                NA
## 3                 NA                NA                   NA                NA
## 4                 NA                NA                   NA                NA
##   avg_pitch_dumbbell stddev_pitch_dumbbell var_pitch_dumbbell avg_yaw_dumbbell
## 1                 NA                    NA                 NA               NA
## 3                 NA                    NA                 NA               NA
## 4                 NA                    NA                 NA               NA
##   stddev_yaw_dumbbell var_yaw_dumbbell gyros_dumbbell_x gyros_dumbbell_y
## 1                  NA               NA                0            -0.02
## 3                  NA               NA                0            -0.02
## 4                  NA               NA                0            -0.02
##   gyros_dumbbell_z accel_dumbbell_x accel_dumbbell_y accel_dumbbell_z
## 1             0.00             -234               47             -271
## 3             0.00             -232               46             -270
## 4            -0.02             -232               48             -269
##   magnet_dumbbell_x magnet_dumbbell_y magnet_dumbbell_z roll_forearm
## 1              -559               293               -65         28.4
## 3              -561               298               -63         28.3
## 4              -552               303               -60         28.1
##   pitch_forearm yaw_forearm kurtosis_roll_forearm kurtosis_picth_forearm
## 1         -63.9        -153                                             
## 3         -63.9        -152                                             
## 4         -63.9        -152                                             
##   kurtosis_yaw_forearm skewness_roll_forearm skewness_pitch_forearm
## 1                                                                  
## 3                                                                  
## 4                                                                  
##   skewness_yaw_forearm max_roll_forearm max_picth_forearm max_yaw_forearm
## 1                                    NA                NA                
## 3                                    NA                NA                
## 4                                    NA                NA                
##   min_roll_forearm min_pitch_forearm min_yaw_forearm amplitude_roll_forearm
## 1               NA                NA                                     NA
## 3               NA                NA                                     NA
## 4               NA                NA                                     NA
##   amplitude_pitch_forearm amplitude_yaw_forearm total_accel_forearm
## 1                      NA                                        36
## 3                      NA                                        36
## 4                      NA                                        36
##   var_accel_forearm avg_roll_forearm stddev_roll_forearm var_roll_forearm
## 1                NA               NA                  NA               NA
## 3                NA               NA                  NA               NA
## 4                NA               NA                  NA               NA
##   avg_pitch_forearm stddev_pitch_forearm var_pitch_forearm avg_yaw_forearm
## 1                NA                   NA                NA              NA
## 3                NA                   NA                NA              NA
## 4                NA                   NA                NA              NA
##   stddev_yaw_forearm var_yaw_forearm gyros_forearm_x gyros_forearm_y
## 1                 NA              NA            0.03            0.00
## 3                 NA              NA            0.03           -0.02
## 4                 NA              NA            0.02           -0.02
##   gyros_forearm_z accel_forearm_x accel_forearm_y accel_forearm_z
## 1           -0.02             192             203            -215
## 3            0.00             196             204            -213
## 4            0.00             189             206            -214
##   magnet_forearm_x magnet_forearm_y magnet_forearm_z classe
## 1              -17              654              476      A
## 3              -18              658              469      A
## 4              -16              658              469      A
```

```r
colnames(validation)
```

```
##   [1] "X"                        "user_name"               
##   [3] "raw_timestamp_part_1"     "raw_timestamp_part_2"    
##   [5] "cvtd_timestamp"           "new_window"              
##   [7] "num_window"               "roll_belt"               
##   [9] "pitch_belt"               "yaw_belt"                
##  [11] "total_accel_belt"         "kurtosis_roll_belt"      
##  [13] "kurtosis_picth_belt"      "kurtosis_yaw_belt"       
##  [15] "skewness_roll_belt"       "skewness_roll_belt.1"    
##  [17] "skewness_yaw_belt"        "max_roll_belt"           
##  [19] "max_picth_belt"           "max_yaw_belt"            
##  [21] "min_roll_belt"            "min_pitch_belt"          
##  [23] "min_yaw_belt"             "amplitude_roll_belt"     
##  [25] "amplitude_pitch_belt"     "amplitude_yaw_belt"      
##  [27] "var_total_accel_belt"     "avg_roll_belt"           
##  [29] "stddev_roll_belt"         "var_roll_belt"           
##  [31] "avg_pitch_belt"           "stddev_pitch_belt"       
##  [33] "var_pitch_belt"           "avg_yaw_belt"            
##  [35] "stddev_yaw_belt"          "var_yaw_belt"            
##  [37] "gyros_belt_x"             "gyros_belt_y"            
##  [39] "gyros_belt_z"             "accel_belt_x"            
##  [41] "accel_belt_y"             "accel_belt_z"            
##  [43] "magnet_belt_x"            "magnet_belt_y"           
##  [45] "magnet_belt_z"            "roll_arm"                
##  [47] "pitch_arm"                "yaw_arm"                 
##  [49] "total_accel_arm"          "var_accel_arm"           
##  [51] "avg_roll_arm"             "stddev_roll_arm"         
##  [53] "var_roll_arm"             "avg_pitch_arm"           
##  [55] "stddev_pitch_arm"         "var_pitch_arm"           
##  [57] "avg_yaw_arm"              "stddev_yaw_arm"          
##  [59] "var_yaw_arm"              "gyros_arm_x"             
##  [61] "gyros_arm_y"              "gyros_arm_z"             
##  [63] "accel_arm_x"              "accel_arm_y"             
##  [65] "accel_arm_z"              "magnet_arm_x"            
##  [67] "magnet_arm_y"             "magnet_arm_z"            
##  [69] "kurtosis_roll_arm"        "kurtosis_picth_arm"      
##  [71] "kurtosis_yaw_arm"         "skewness_roll_arm"       
##  [73] "skewness_pitch_arm"       "skewness_yaw_arm"        
##  [75] "max_roll_arm"             "max_picth_arm"           
##  [77] "max_yaw_arm"              "min_roll_arm"            
##  [79] "min_pitch_arm"            "min_yaw_arm"             
##  [81] "amplitude_roll_arm"       "amplitude_pitch_arm"     
##  [83] "amplitude_yaw_arm"        "roll_dumbbell"           
##  [85] "pitch_dumbbell"           "yaw_dumbbell"            
##  [87] "kurtosis_roll_dumbbell"   "kurtosis_picth_dumbbell" 
##  [89] "kurtosis_yaw_dumbbell"    "skewness_roll_dumbbell"  
##  [91] "skewness_pitch_dumbbell"  "skewness_yaw_dumbbell"   
##  [93] "max_roll_dumbbell"        "max_picth_dumbbell"      
##  [95] "max_yaw_dumbbell"         "min_roll_dumbbell"       
##  [97] "min_pitch_dumbbell"       "min_yaw_dumbbell"        
##  [99] "amplitude_roll_dumbbell"  "amplitude_pitch_dumbbell"
## [101] "amplitude_yaw_dumbbell"   "total_accel_dumbbell"    
## [103] "var_accel_dumbbell"       "avg_roll_dumbbell"       
## [105] "stddev_roll_dumbbell"     "var_roll_dumbbell"       
## [107] "avg_pitch_dumbbell"       "stddev_pitch_dumbbell"   
## [109] "var_pitch_dumbbell"       "avg_yaw_dumbbell"        
## [111] "stddev_yaw_dumbbell"      "var_yaw_dumbbell"        
## [113] "gyros_dumbbell_x"         "gyros_dumbbell_y"        
## [115] "gyros_dumbbell_z"         "accel_dumbbell_x"        
## [117] "accel_dumbbell_y"         "accel_dumbbell_z"        
## [119] "magnet_dumbbell_x"        "magnet_dumbbell_y"       
## [121] "magnet_dumbbell_z"        "roll_forearm"            
## [123] "pitch_forearm"            "yaw_forearm"             
## [125] "kurtosis_roll_forearm"    "kurtosis_picth_forearm"  
## [127] "kurtosis_yaw_forearm"     "skewness_roll_forearm"   
## [129] "skewness_pitch_forearm"   "skewness_yaw_forearm"    
## [131] "max_roll_forearm"         "max_picth_forearm"       
## [133] "max_yaw_forearm"          "min_roll_forearm"        
## [135] "min_pitch_forearm"        "min_yaw_forearm"         
## [137] "amplitude_roll_forearm"   "amplitude_pitch_forearm" 
## [139] "amplitude_yaw_forearm"    "total_accel_forearm"     
## [141] "var_accel_forearm"        "avg_roll_forearm"        
## [143] "stddev_roll_forearm"      "var_roll_forearm"        
## [145] "avg_pitch_forearm"        "stddev_pitch_forearm"    
## [147] "var_pitch_forearm"        "avg_yaw_forearm"         
## [149] "stddev_yaw_forearm"       "var_yaw_forearm"         
## [151] "gyros_forearm_x"          "gyros_forearm_y"         
## [153] "gyros_forearm_z"          "accel_forearm_x"         
## [155] "accel_forearm_y"          "accel_forearm_z"         
## [157] "magnet_forearm_x"         "magnet_forearm_y"        
## [159] "magnet_forearm_z"         "problem_id"
```

```r
summary(training)
```

```
##        X          user_name         raw_timestamp_part_1 raw_timestamp_part_2
##  Min.   :    1   Length:14718       Min.   :1.322e+09    Min.   :   294      
##  1st Qu.: 4888   Class :character   1st Qu.:1.323e+09    1st Qu.:254247      
##  Median : 9812   Mode  :character   Median :1.323e+09    Median :494676      
##  Mean   : 9812                      Mean   :1.323e+09    Mean   :499705      
##  3rd Qu.:14703                      3rd Qu.:1.323e+09    3rd Qu.:748351      
##  Max.   :19622                      Max.   :1.323e+09    Max.   :998801      
##                                                                              
##  cvtd_timestamp      new_window          num_window      roll_belt    
##  Length:14718       Length:14718       Min.   :  1.0   Min.   :-28.9  
##  Class :character   Class :character   1st Qu.:221.0   1st Qu.:  1.1  
##  Mode  :character   Mode  :character   Median :424.0   Median :113.0  
##                                        Mean   :430.8   Mean   : 64.4  
##                                        3rd Qu.:645.0   3rd Qu.:123.0  
##                                        Max.   :864.0   Max.   :162.0  
##                                                                       
##    pitch_belt          yaw_belt       total_accel_belt kurtosis_roll_belt
##  Min.   :-55.8000   Min.   :-180.00   Min.   : 0.0     Length:14718      
##  1st Qu.:  1.7025   1st Qu.: -88.30   1st Qu.: 3.0     Class :character  
##  Median :  5.2800   Median : -13.00   Median :17.0     Mode  :character  
##  Mean   :  0.1938   Mean   : -11.02   Mean   :11.3                       
##  3rd Qu.: 14.8000   3rd Qu.:  13.30   3rd Qu.:18.0                       
##  Max.   : 60.3000   Max.   : 179.00   Max.   :28.0                       
##                                                                          
##  kurtosis_picth_belt kurtosis_yaw_belt  skewness_roll_belt skewness_roll_belt.1
##  Length:14718        Length:14718       Length:14718       Length:14718        
##  Class :character    Class :character   Class :character   Class :character    
##  Mode  :character    Mode  :character   Mode  :character   Mode  :character    
##                                                                                
##                                                                                
##                                                                                
##                                                                                
##  skewness_yaw_belt  max_roll_belt    max_picth_belt  max_yaw_belt      
##  Length:14718       Min.   :-94.10   Min.   : 3.00   Length:14718      
##  Class :character   1st Qu.:-88.00   1st Qu.: 5.00   Class :character  
##  Mode  :character   Median : -5.50   Median :18.00   Mode  :character  
##                     Mean   : -6.66   Mean   :12.71                     
##                     3rd Qu.: 14.70   3rd Qu.:19.00                     
##                     Max.   :180.00   Max.   :30.00                     
##                     NA's   :14413    NA's   :14413                     
##  min_roll_belt     min_pitch_belt  min_yaw_belt       amplitude_roll_belt
##  Min.   :-180.00   Min.   : 0.00   Length:14718       Min.   :  0.000    
##  1st Qu.: -88.40   1st Qu.: 3.00   Class :character   1st Qu.:  0.300    
##  Median :  -8.00   Median :16.00   Mode  :character   Median :  1.000    
##  Mean   : -10.99   Mean   :10.67                      Mean   :  4.326    
##  3rd Qu.:   3.50   3rd Qu.:17.00                      3rd Qu.:  2.000    
##  Max.   : 173.00   Max.   :23.00                      Max.   :360.000    
##  NA's   :14413     NA's   :14413                      NA's   :14413      
##  amplitude_pitch_belt amplitude_yaw_belt var_total_accel_belt avg_roll_belt   
##  Min.   : 0.000       Length:14718       Min.   : 0.000       Min.   :-27.40  
##  1st Qu.: 1.000       Class :character   1st Qu.: 0.100       1st Qu.:  1.10  
##  Median : 1.000       Mode  :character   Median : 0.200       Median :116.30  
##  Mean   : 2.046                          Mean   : 0.815       Mean   : 66.91  
##  3rd Qu.: 2.000                          3rd Qu.: 0.300       3rd Qu.:123.10  
##  Max.   :11.000                          Max.   :12.600       Max.   :157.40  
##  NA's   :14413                           NA's   :14413        NA's   :14413   
##  stddev_roll_belt var_roll_belt    avg_pitch_belt    stddev_pitch_belt
##  Min.   : 0.000   Min.   :  0.00   Min.   :-51.400   Min.   :0.000    
##  1st Qu.: 0.200   1st Qu.:  0.00   1st Qu.:  1.900   1st Qu.:0.200    
##  Median : 0.400   Median :  0.10   Median :  5.100   Median :0.300    
##  Mean   : 1.206   Mean   :  6.66   Mean   : -0.011   Mean   :0.597    
##  3rd Qu.: 0.700   3rd Qu.:  0.40   3rd Qu.: 14.800   3rd Qu.:0.700    
##  Max.   :14.200   Max.   :200.70   Max.   : 59.700   Max.   :3.600    
##  NA's   :14413    NA's   :14413    NA's   :14413     NA's   :14413    
##  var_pitch_belt    avg_yaw_belt      stddev_yaw_belt    var_yaw_belt     
##  Min.   : 0.000   Min.   :-138.300   Min.   :  0.000   Min.   :    0.00  
##  1st Qu.: 0.000   1st Qu.: -88.200   1st Qu.:  0.100   1st Qu.:    0.01  
##  Median : 0.100   Median :  -6.700   Median :  0.300   Median :    0.09  
##  Mean   : 0.761   Mean   :  -9.187   Mean   :  1.572   Mean   :  142.72  
##  3rd Qu.: 0.500   3rd Qu.:  10.600   3rd Qu.:  0.700   3rd Qu.:    0.42  
##  Max.   :13.100   Max.   : 173.500   Max.   :176.600   Max.   :31183.24  
##  NA's   :14413    NA's   :14413      NA's   :14413     NA's   :14413     
##   gyros_belt_x        gyros_belt_y       gyros_belt_z      accel_belt_x     
##  Min.   :-1.040000   Min.   :-0.64000   Min.   :-1.4600   Min.   :-120.000  
##  1st Qu.:-0.030000   1st Qu.: 0.00000   1st Qu.:-0.2000   1st Qu.: -21.000  
##  Median : 0.030000   Median : 0.02000   Median :-0.1000   Median : -15.000  
##  Mean   :-0.005336   Mean   : 0.03973   Mean   :-0.1302   Mean   :  -5.444  
##  3rd Qu.: 0.110000   3rd Qu.: 0.11000   3rd Qu.:-0.0200   3rd Qu.:  -5.000  
##  Max.   : 2.200000   Max.   : 0.64000   Max.   : 1.6200   Max.   :  81.000  
##                                                                             
##   accel_belt_y    accel_belt_z     magnet_belt_x    magnet_belt_y  
##  Min.   :-69.0   Min.   :-266.00   Min.   :-52.00   Min.   :354.0  
##  1st Qu.:  3.0   1st Qu.:-162.00   1st Qu.:  9.00   1st Qu.:582.0  
##  Median : 34.0   Median :-152.00   Median : 35.00   Median :601.0  
##  Mean   : 30.1   Mean   : -72.55   Mean   : 55.59   Mean   :593.8  
##  3rd Qu.: 61.0   3rd Qu.:  27.00   3rd Qu.: 60.00   3rd Qu.:610.0  
##  Max.   :164.0   Max.   : 105.00   Max.   :485.00   Max.   :673.0  
##                                                                    
##  magnet_belt_z       roll_arm         pitch_arm          yaw_arm         
##  Min.   :-623.0   Min.   :-180.00   Min.   :-88.800   Min.   :-180.0000  
##  1st Qu.:-375.0   1st Qu.: -31.80   1st Qu.:-26.000   1st Qu.: -43.3000  
##  Median :-319.0   Median :   0.00   Median :  0.000   Median :   0.0000  
##  Mean   :-345.3   Mean   :  17.69   Mean   : -4.676   Mean   :  -0.9092  
##  3rd Qu.:-306.0   3rd Qu.:  77.10   3rd Qu.: 11.100   3rd Qu.:  45.5750  
##  Max.   : 293.0   Max.   : 180.00   Max.   : 88.500   Max.   : 180.0000  
##                                                                          
##  total_accel_arm var_accel_arm      avg_roll_arm     stddev_roll_arm 
##  Min.   : 1.00   Min.   :  0.000   Min.   :-166.67   Min.   : 0.000  
##  1st Qu.:17.00   1st Qu.:  8.778   1st Qu.: -36.67   1st Qu.: 1.082  
##  Median :27.00   Median : 39.288   Median :   0.00   Median : 5.143  
##  Mean   :25.44   Mean   : 53.039   Mean   :  13.20   Mean   :10.102  
##  3rd Qu.:33.00   3rd Qu.: 74.007   3rd Qu.:  75.81   3rd Qu.:13.961  
##  Max.   :66.00   Max.   :331.699   Max.   : 163.33   Max.   :93.559  
##                  NA's   :14413     NA's   :14413     NA's   :14413   
##   var_roll_arm      avg_pitch_arm     stddev_pitch_arm var_pitch_arm     
##  Min.   :   0.000   Min.   :-81.773   Min.   : 0.000   Min.   :   0.000  
##  1st Qu.:   1.171   1st Qu.:-21.424   1st Qu.: 1.274   1st Qu.:   1.623  
##  Median :  26.447   Median :  0.000   Median : 7.960   Median :  63.355  
##  Mean   : 289.259   Mean   : -3.875   Mean   :10.355   Mean   : 198.580  
##  3rd Qu.: 194.906   3rd Qu.:  9.556   3rd Qu.:16.423   3rd Qu.: 269.711  
##  Max.   :8753.283   Max.   : 69.648   Max.   :43.412   Max.   :1884.565  
##  NA's   :14413      NA's   :14413     NA's   :14413    NA's   :14413     
##   avg_yaw_arm      stddev_yaw_arm     var_yaw_arm         gyros_arm_x      
##  Min.   :-173.44   Min.   :  0.000   Min.   :    0.000   Min.   :-6.37000  
##  1st Qu.: -31.64   1st Qu.:  1.874   1st Qu.:    3.511   1st Qu.:-1.35000  
##  Median :   0.00   Median : 16.146   Median :  260.704   Median : 0.08000  
##  Mean   :   2.12   Mean   : 21.986   Mean   : 1106.293   Mean   : 0.03452  
##  3rd Qu.:  38.85   3rd Qu.: 32.842   3rd Qu.: 1078.586   3rd Qu.: 1.57000  
##  Max.   : 152.00   Max.   :177.044   Max.   :31344.568   Max.   : 4.87000  
##  NA's   :14413     NA's   :14413     NA's   :14413                         
##   gyros_arm_y       gyros_arm_z      accel_arm_x       accel_arm_y     
##  Min.   :-3.4400   Min.   :-2.330   Min.   :-377.00   Min.   :-315.00  
##  1st Qu.:-0.7975   1st Qu.:-0.070   1st Qu.:-242.00   1st Qu.: -54.00  
##  Median :-0.2400   Median : 0.230   Median : -45.00   Median :  14.00  
##  Mean   :-0.2531   Mean   : 0.268   Mean   : -60.94   Mean   :  32.66  
##  3rd Qu.: 0.1400   3rd Qu.: 0.720   3rd Qu.:  82.00   3rd Qu.: 138.00  
##  Max.   : 2.8400   Max.   : 3.020   Max.   : 434.00   Max.   : 308.00  
##                                                                        
##   accel_arm_z       magnet_arm_x     magnet_arm_y     magnet_arm_z   
##  Min.   :-636.00   Min.   :-584.0   Min.   :-386.0   Min.   :-597.0  
##  1st Qu.:-141.00   1st Qu.:-298.0   1st Qu.:  -8.0   1st Qu.: 137.0  
##  Median : -46.00   Median : 284.5   Median : 200.0   Median : 445.0  
##  Mean   : -70.77   Mean   : 191.4   Mean   : 156.5   Mean   : 307.3  
##  3rd Qu.:  24.00   3rd Qu.: 638.0   3rd Qu.: 323.0   3rd Qu.: 545.0  
##  Max.   : 292.00   Max.   : 782.0   Max.   : 583.0   Max.   : 694.0  
##                                                                      
##  kurtosis_roll_arm  kurtosis_picth_arm kurtosis_yaw_arm   skewness_roll_arm 
##  Length:14718       Length:14718       Length:14718       Length:14718      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  skewness_pitch_arm skewness_yaw_arm    max_roll_arm    max_picth_arm    
##  Length:14718       Length:14718       Min.   :-73.10   Min.   :-173.00  
##  Class :character   Class :character   1st Qu.:  0.00   1st Qu.:  -5.30  
##  Mode  :character   Mode  :character   Median :  5.30   Median :  18.90  
##                                        Mean   : 11.97   Mean   :  35.05  
##                                        3rd Qu.: 27.40   3rd Qu.:  95.00  
##                                        Max.   : 85.50   Max.   : 180.00  
##                                        NA's   :14413    NA's   :14413    
##   max_yaw_arm     min_roll_arm    min_pitch_arm      min_yaw_arm   
##  Min.   : 4.00   Min.   :-88.80   Min.   :-180.00   Min.   : 1.00  
##  1st Qu.:29.00   1st Qu.:-41.40   1st Qu.: -71.80   1st Qu.: 7.00  
##  Median :34.00   Median :-21.50   Median : -32.80   Median :12.00  
##  Mean   :35.09   Mean   :-20.48   Mean   : -33.59   Mean   :14.59  
##  3rd Qu.:41.00   3rd Qu.:  0.00   3rd Qu.:   0.00   3rd Qu.:19.00  
##  Max.   :65.00   Max.   : 54.50   Max.   : 152.00   Max.   :38.00  
##  NA's   :14413   NA's   :14413    NA's   :14413     NA's   :14413  
##  amplitude_roll_arm amplitude_pitch_arm amplitude_yaw_arm roll_dumbbell    
##  Min.   :  0.00     Min.   :  0.00      Min.   : 0.0      Min.   :-152.03  
##  1st Qu.:  5.00     1st Qu.:  8.90      1st Qu.:12.0      1st Qu.: -17.58  
##  Median : 28.40     Median : 53.40      Median :21.0      Median :  48.39  
##  Mean   : 32.45     Mean   : 68.64      Mean   :20.5      Mean   :  24.11  
##  3rd Qu.: 51.50     3rd Qu.:112.70      3rd Qu.:28.0      3rd Qu.:  68.00  
##  Max.   :119.50     Max.   :360.00      Max.   :51.0      Max.   : 152.08  
##  NA's   :14413      NA's   :14413       NA's   :14413                      
##  pitch_dumbbell     yaw_dumbbell      kurtosis_roll_dumbbell
##  Min.   :-149.59   Min.   :-150.871   Length:14718          
##  1st Qu.: -41.11   1st Qu.: -77.667   Class :character      
##  Median : -21.13   Median :  -3.601   Mode  :character      
##  Mean   : -10.94   Mean   :   1.364                         
##  3rd Qu.:  17.43   3rd Qu.:  78.940                         
##  Max.   : 129.82   Max.   : 154.952                         
##                                                             
##  kurtosis_picth_dumbbell kurtosis_yaw_dumbbell skewness_roll_dumbbell
##  Length:14718            Length:14718          Length:14718          
##  Class :character        Class :character      Class :character      
##  Mode  :character        Mode  :character      Mode  :character      
##                                                                      
##                                                                      
##                                                                      
##                                                                      
##  skewness_pitch_dumbbell skewness_yaw_dumbbell max_roll_dumbbell
##  Length:14718            Length:14718          Min.   :-70.00   
##  Class :character        Class :character      1st Qu.:-27.50   
##  Mode  :character        Mode  :character      Median :  6.30   
##                                                Mean   : 11.87   
##                                                3rd Qu.: 50.70   
##                                                Max.   :129.80   
##                                                NA's   :14413    
##  max_picth_dumbbell max_yaw_dumbbell   min_roll_dumbbell min_pitch_dumbbell
##  Min.   :-112.90    Length:14718       Min.   :-149.60   Min.   :-147.00   
##  1st Qu.: -67.80    Class :character   1st Qu.: -59.00   1st Qu.: -92.00   
##  Median :  35.50    Mode  :character   Median : -43.30   Median : -66.90   
##  Mean   :  30.13                       Mean   : -40.42   Mean   : -33.55   
##  3rd Qu.: 133.30                       3rd Qu.: -25.10   3rd Qu.:  18.30   
##  Max.   : 155.00                       Max.   :  41.50   Max.   : 116.70   
##  NA's   :14413                         NA's   :14413     NA's   :14413     
##  min_yaw_dumbbell   amplitude_roll_dumbbell amplitude_pitch_dumbbell
##  Length:14718       Min.   :  0.00          Min.   :  0.00          
##  Class :character   1st Qu.: 13.36          1st Qu.: 16.55          
##  Mode  :character   Median : 33.22          Median : 39.27          
##                     Mean   : 52.29          Mean   : 63.67          
##                     3rd Qu.: 72.72          3rd Qu.: 93.83          
##                     Max.   :236.55          Max.   :270.84          
##                     NA's   :14413           NA's   :14413           
##  amplitude_yaw_dumbbell total_accel_dumbbell var_accel_dumbbell
##  Length:14718           Min.   : 0.00        Min.   : 0.000    
##  Class :character       1st Qu.: 4.00        1st Qu.: 0.336    
##  Mode  :character       Median :10.00        Median : 0.917    
##                         Mean   :13.74        Mean   : 3.505    
##                         3rd Qu.:19.00        3rd Qu.: 2.994    
##                         Max.   :58.00        Max.   :48.424    
##                                              NA's   :14413     
##  avg_roll_dumbbell stddev_roll_dumbbell var_roll_dumbbell avg_pitch_dumbbell
##  Min.   :-128.96   Min.   :  0.000      Min.   :    0.0   Min.   :-70.73    
##  1st Qu.: -11.10   1st Qu.:  4.278      1st Qu.:   18.3   1st Qu.:-43.04    
##  Median :  50.47   Median : 10.686      Median :  114.2   Median :-21.77    
##  Mean   :  25.71   Mean   : 19.701      Mean   :  936.5   Mean   :-13.18    
##  3rd Qu.:  63.78   3rd Qu.: 25.387      3rd Qu.:  644.5   3rd Qu.: 12.59    
##  Max.   : 125.99   Max.   :123.778      Max.   :15321.0   Max.   : 94.28    
##  NA's   :14413     NA's   :14413        NA's   :14413     NA's   :14413     
##  stddev_pitch_dumbbell var_pitch_dumbbell avg_yaw_dumbbell  
##  Min.   : 0.000        Min.   :   0.000   Min.   :-117.950  
##  1st Qu.: 3.045        1st Qu.:   9.271   1st Qu.: -76.844  
##  Median : 7.836        Median :  61.406   Median :  -2.694  
##  Mean   :12.550        Mean   : 330.617   Mean   :  -1.411  
##  3rd Qu.:18.630        3rd Qu.: 347.091   3rd Qu.:  70.874  
##  Max.   :82.680        Max.   :6836.023   Max.   : 130.879  
##  NA's   :14413         NA's   :14413      NA's   :14413     
##  stddev_yaw_dumbbell var_yaw_dumbbell   gyros_dumbbell_x    gyros_dumbbell_y  
##  Min.   :  0.000     Min.   :    0.00   Min.   :-204.0000   Min.   :-1.99000  
##  1st Qu.:  3.797     1st Qu.:   14.42   1st Qu.:  -0.0300   1st Qu.:-0.14000  
##  Median :  9.440     Median :   89.12   Median :   0.1300   Median : 0.03000  
##  Mean   : 15.938     Mean   :  543.58   Mean   :   0.1561   Mean   : 0.04765  
##  3rd Qu.: 23.556     3rd Qu.:  554.86   3rd Qu.:   0.3500   3rd Qu.: 0.21000  
##  Max.   :107.088     Max.   :11467.91   Max.   :   2.1700   Max.   :52.00000  
##  NA's   :14413       NA's   :14413                                            
##  gyros_dumbbell_z  accel_dumbbell_x accel_dumbbell_y  accel_dumbbell_z 
##  Min.   : -2.380   Min.   :-419.0   Min.   :-182.00   Min.   :-319.00  
##  1st Qu.: -0.310   1st Qu.: -51.0   1st Qu.:  -8.00   1st Qu.:-142.00  
##  Median : -0.130   Median :  -9.0   Median :  42.00   Median :  -1.00  
##  Mean   : -0.123   Mean   : -28.8   Mean   :  52.77   Mean   : -38.68  
##  3rd Qu.:  0.030   3rd Qu.:  11.0   3rd Qu.: 111.00   3rd Qu.:  37.00  
##  Max.   :317.000   Max.   : 235.0   Max.   : 310.00   Max.   : 318.00  
##                                                                        
##  magnet_dumbbell_x magnet_dumbbell_y magnet_dumbbell_z  roll_forearm    
##  Min.   :-643.0    Min.   :-3600.0   Min.   :-262.00   Min.   :-180.00  
##  1st Qu.:-536.0    1st Qu.:  232.0   1st Qu.: -45.00   1st Qu.:  -0.90  
##  Median :-480.0    Median :  311.0   Median :  13.00   Median :  20.80  
##  Mean   :-330.7    Mean   :  222.1   Mean   :  45.43   Mean   :  33.52  
##  3rd Qu.:-308.2    3rd Qu.:  390.0   3rd Qu.:  95.00   3rd Qu.: 140.00  
##  Max.   : 592.0    Max.   :  633.0   Max.   : 452.00   Max.   : 180.00  
##                                                                         
##  pitch_forearm     yaw_forearm      kurtosis_roll_forearm
##  Min.   :-72.50   Min.   :-180.00   Length:14718         
##  1st Qu.:  0.00   1st Qu.: -69.00   Class :character     
##  Median :  9.18   Median :   0.00   Mode  :character     
##  Mean   : 10.75   Mean   :  18.77                        
##  3rd Qu.: 28.50   3rd Qu.: 110.00                        
##  Max.   : 89.80   Max.   : 180.00                        
##                                                          
##  kurtosis_picth_forearm kurtosis_yaw_forearm skewness_roll_forearm
##  Length:14718           Length:14718         Length:14718         
##  Class :character       Class :character     Class :character     
##  Mode  :character       Mode  :character     Mode  :character     
##                                                                   
##                                                                   
##                                                                   
##                                                                   
##  skewness_pitch_forearm skewness_yaw_forearm max_roll_forearm max_picth_forearm
##  Length:14718           Length:14718         Min.   :-66.60   Min.   :-149.00  
##  Class :character       Class :character     1st Qu.:  0.00   1st Qu.:   0.00  
##  Mode  :character       Mode  :character     Median : 25.60   Median : 110.00  
##                                              Mean   : 23.39   Mean   :  78.02  
##                                              3rd Qu.: 45.00   3rd Qu.: 173.00  
##                                              Max.   : 89.80   Max.   : 180.00  
##                                              NA's   :14413    NA's   :14413    
##  max_yaw_forearm    min_roll_forearm  min_pitch_forearm min_yaw_forearm   
##  Length:14718       Min.   :-69.400   Min.   :-180.00   Length:14718      
##  Class :character   1st Qu.: -6.000   1st Qu.:-174.00   Class :character  
##  Mode  :character   Median :  0.000   Median : -54.00   Mode  :character  
##                     Mean   :  0.222   Mean   : -54.47                     
##                     3rd Qu.: 12.600   3rd Qu.:   0.00                     
##                     Max.   : 62.100   Max.   : 167.00                     
##                     NA's   :14413     NA's   :14413                       
##  amplitude_roll_forearm amplitude_pitch_forearm amplitude_yaw_forearm
##  Min.   :  0.00         Min.   :  0.0           Length:14718         
##  1st Qu.:  1.02         1st Qu.:  1.0           Class :character     
##  Median : 16.10         Median : 78.0           Mode  :character     
##  Mean   : 23.17         Mean   :132.5                                
##  3rd Qu.: 36.73         3rd Qu.:349.0                                
##  Max.   :126.00         Max.   :360.0                                
##  NA's   :14413          NA's   :14413                                
##  total_accel_forearm var_accel_forearm avg_roll_forearm   stddev_roll_forearm
##  Min.   :  0.00      Min.   :  0.000   Min.   :-177.234   Min.   :  0.000    
##  1st Qu.: 29.00      1st Qu.:  6.148   1st Qu.:  -0.045   1st Qu.:  0.320    
##  Median : 36.00      Median : 18.989   Median :  10.716   Median :  7.423    
##  Mean   : 34.74      Mean   : 31.778   Mean   :  33.872   Mean   : 41.606    
##  3rd Qu.: 41.00      3rd Qu.: 47.482   3rd Qu.: 113.062   3rd Qu.: 81.102    
##  Max.   :108.00      Max.   :172.606   Max.   : 177.256   Max.   :179.171    
##                      NA's   :14413     NA's   :14413      NA's   :14413      
##  var_roll_forearm   avg_pitch_forearm stddev_pitch_forearm var_pitch_forearm 
##  Min.   :    0.00   Min.   :-68.17    Min.   : 0.000       Min.   :   0.000  
##  1st Qu.:    0.10   1st Qu.:  0.00    1st Qu.: 0.298       1st Qu.:   0.089  
##  Median :   55.11   Median : 11.78    Median : 5.010       Median :  25.104  
##  Mean   : 5256.50   Mean   : 11.54    Mean   : 7.479       Mean   : 128.218  
##  3rd Qu.: 6577.60   3rd Qu.: 28.27    3rd Qu.:12.275       3rd Qu.: 150.676  
##  Max.   :32102.24   Max.   : 72.09    Max.   :47.745       Max.   :2279.617  
##  NA's   :14413      NA's   :14413     NA's   :14413        NA's   :14413     
##  avg_yaw_forearm   stddev_yaw_forearm var_yaw_forearm    gyros_forearm_x   
##  Min.   :-155.06   Min.   :  0.000    Min.   :    0.00   Min.   :-22.0000  
##  1st Qu.: -26.66   1st Qu.:  0.501    1st Qu.:    0.25   1st Qu.: -0.2200  
##  Median :   0.00   Median : 21.641    Median :  468.33   Median :  0.0500  
##  Mean   :  18.58   Mean   : 42.324    Mean   : 4302.97   Mean   :  0.1579  
##  3rd Qu.:  87.29   3rd Qu.: 69.314    3rd Qu.: 4804.47   3rd Qu.:  0.5600  
##  Max.   : 168.86   Max.   :197.508    Max.   :39009.33   Max.   :  3.9700  
##  NA's   :14413     NA's   :14413      NA's   :14413                        
##  gyros_forearm_y     gyros_forearm_z    accel_forearm_x   accel_forearm_y 
##  Min.   : -7.02000   Min.   : -8.0900   Min.   :-498.00   Min.   :-585.0  
##  1st Qu.: -1.45000   1st Qu.: -0.1800   1st Qu.:-179.00   1st Qu.:  54.0  
##  Median :  0.03000   Median :  0.0800   Median : -57.00   Median : 201.0  
##  Mean   :  0.09321   Mean   :  0.1582   Mean   : -62.52   Mean   : 162.8  
##  3rd Qu.:  1.64000   3rd Qu.:  0.4900   3rd Qu.:  74.75   3rd Qu.: 313.0  
##  Max.   :311.00000   Max.   :231.0000   Max.   : 477.00   Max.   : 923.0  
##                                                                           
##  accel_forearm_z magnet_forearm_x  magnet_forearm_y magnet_forearm_z classe  
##  Min.   :-410    Min.   :-1280.0   Min.   :-896.0   Min.   :-973.0   A:4185  
##  1st Qu.:-182    1st Qu.: -615.0   1st Qu.:  -3.0   1st Qu.: 192.0   B:2848  
##  Median : -41    Median : -379.0   Median : 591.5   Median : 509.0   C:2567  
##  Mean   : -56    Mean   : -312.7   Mean   : 377.6   Mean   : 394.4   D:2412  
##  3rd Qu.:  25    3rd Qu.:  -74.0   3rd Qu.: 737.0   3rd Qu.: 653.0   E:2706  
##  Max.   : 291    Max.   :  666.0   Max.   :1480.0   Max.   :1090.0           
## 
```

```r
#training$classe<-as.factor(training$classe)
#testing$classe<-as.factor(testing$classe)
```

#### A03 Data preparation

```r
training.nona.temp<-training[training$new_window!="yes",]
training.nona<-training.nona.temp[,!grepl("kurtosis|skewness|max|min|amplitude|var|avg|stddev|window|X|user|timestamp",colnames(training.nona.temp))]
rm(training.nona.temp)

testing.nona.temp<-testing[testing$new_window!="yes",]
testing.nona<-testing.nona.temp[,!grepl("kurtosis|skewness|max|min|amplitude|var|avg|stddev|window|X|user|timestamp",colnames(testing.nona.temp))]
rm(testing.nona.temp)

validation.nona.temp<-validation[validation$new_window!="yes",]
validation.nona<-validation.nona.temp[,!grepl("kurtosis|skewness|max|min|amplitude|var|avg|stddev|X|window|user|timestamp",colnames(validation.nona.temp))]
rm(validation.nona.temp)
```

#### A04 Creating models

```r
mod1<-train(classe~.,method="rpart",data=training.nona)
predict1<-predict(mod1,testing.nona)
Accuracy_mod1<-confusionMatrix(predict1,testing.nona$classe)$overall[1]
print(Accuracy_mod1)
fancyRpartPlot(mod1$finalModel)

cluster2<-makeCluster(detectCores()-1)
registerDoParallel(cluster2)
fitControl2<-trainControl(method="cv",number=5,allowParallel=TRUE)
system.time(mod2<-train(classe~.,method="gbm",data=training.nona,verbose=FALSE,trControl=fitControl2))
stopCluster(cluster2)
registerDoSEQ()
predict2<-predict(mod2,testing.nona)
Accuracy_mod2<-confusionMatrix(predict2,testing.nona$classe)$overall[1]
print(Accuracy_mod2)

cluster3<-makeCluster(detectCores()-1)
registerDoParallel(cluster3)
fitControl3<-trainControl(method="cv",number=5,allowParallel=TRUE)
system.time(mod3<-train(classe~.,method="rf",data=training.nona,trControl=fitControl3))
stopCluster(cluster3)
registerDoSEQ()
predict3<-predict(mod3,testing.nona)
Accuracy_mod3<-confusionMatrix(predict3,testing.nona$classe)$overall[1]
print(Accuracy_mod3)
```

#### A05 Predicting the classe type for the Validation dataset

```r
ValidationClasse<-c("B","A","B","A","A","E","D","B","A","A","B","C","B","A","E","E","A","B","B","B")
predictClasse1<-predict(mod1,validation.nona)
Model1Score<-sum(predictClasse1==ValidationClasse)
predictClasse2<-predict(mod2,validation.nona)
Model2Score<-sum(predictClasse2==ValidationClasse)
predictClasse3<-predict(mod3,validation.nona)
Model3Score<-sum(predictClasse3==ValidationClasse)
print( paste("Score for model 1: ", Model1Score))
print( paste("Score for model 2: ", Model2Score))
print( paste("Score for model 3: ", Model3Score))
```
