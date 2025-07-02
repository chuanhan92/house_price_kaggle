# housing_price

1. preprocessing
   1.1 filling NA by observation on initial data given. eg: MasVnrType will filled with 'None' when MasVnrArea is '0', and when PoolArea is '0', PoolQC is filled with 'NA'.
   1.2 filling NA by using KKNImputing method:
     1.2.1 create a correlation matrix among features, select features with missing values as 'target' and features with correlation score of 0.4 with 'target' as 'features'.
     1.2.2 divide 'features' into 3 categories: categorical features, cateogorical with order features and numerical features , for different approach.
     1.2.3 fill NA with 'missing' temporarily, locate the missing value in 'target' in which row, place 'target' together with 'features', rearrange them into 3 categories define above. Note that only 'features' is filled NA with 'missing' at the moment, 'target left untouched.
     1.2.4 a pipeline is create with columntransformer that encode and scale, following the defined order, and KNNimputer in the pipeline as well.
     1.2.5 train and validation data fit_transform and transform with the pipeline accordingly. concat into temporary dataframe, inverse transform, then pick only the row that have missing value in 'target', replace the missing value with imputed value from temporary dataframe.
  1.3 create feature_engineering function
  1.4 create encoding function
    1.4.1 encoding categorical features with 2 features above with target encoding subject to SalePrice
   1.4.2 encoding cateogircal features with 2 features or below with onehotencoding, drop first, becoming boolean
  1.5 creating cluster for similar identity features. eg: grouping cond nd quality for several aspect and make a cluster of it.

2. cross validation
   2.1 provoke 1.1 first.
   2.2 create stratified k fold with 5 folds, provoke 1.2, 1.3, 1.4, 1.5 accordingly within the stratified K fold.
   2.3 make 3 runs:
     2.3.1 first run: to list the features importance, and then to eliminate features that hurt the prediction using reverse elimination method. mark down the selected features in each fold and get the sum on how frequent the features selected in each fold.
     2.3.2 second run: to use the generated rejection list and rerun the prediction again if we remove the features that appear 5 times, 4 and above, 3 and above, 2 and above and appear at least once, find the best mean score for this 5 runs.
     2.3.3 third run: to find the best parameter for the model for n_estimators, max_depth and learning_rate.

CV score (RMSE): fold 0: 0.1316, fold 1: 0.1325, fold 2: 0.1260, fold 3: 0.1212, fold 4: 0.1224
CV mean score (RMSE): 0.1267
kaggle competion score (RMSE): 0.1374
