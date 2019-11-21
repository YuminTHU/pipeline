# Method

- Raw

- In order to remove the dimension of the expression data, we normalized the expression  data with RPKM method.Then we centered each differential expression gene by their sample-wise min and scaled by sample-wise max-min (using the MinMaxScaler class in the scikit-learn Python package). Differential splicing data and qPCR data did not handle.

  

- Modified

- Machine learning method was adopted to evaluate the classification performance of selected features. For RNA-seq data, RPKM of each gene was centered to zero with MinMaxScaler in scikit-learn package. delta-Ct values in qPCR data and psi values for alternative splicing events quantification were directly fed into the machine learning algorithms. 

  

- Raw

  We evaluated the 7 differentially expressed genes and 3 differential splicing genes on exoRbase and cfRNA dataset.First, we used cross validation to select the optimal classifier on two data sets. The whole cross-validation procedure was divided into two stages:performance evaluation of DT,SVM,LR and RF was performed in 50 outer cross-validation runs and optimization of hyper-parameters in each model was performed in 5 inner cross-validation runs. We used 5-fold cross-validation and repeated it 10 times to randomly divide samples into  50 class-balanced independent training and test sets in an 80–20% manner (using the RepeatedStratifiedKFold class in the scikit-learn Python package) during 50 outer cross-validation runs. We used 5-fold cross-validation to divide training dataset while the inner cross-validation loop.In inner cross-validation, we used grid search algorithm to optimize hyper-parameters (using the GridSearchCV class in the scikit-learn Python package). For decision tree, we searched number of maximum tree depth (parameter max_depth in sklearn.tree.DecisionTreeClassifier) between [3, 5, 7, 9]. For logistic regression, we searched regularization weight (parameter C in sklearn.linear_model.LogisticRegression) between [1e-5 and 1e5], incremented by 10 fold in each step.For SVM, we searched penalty parametert (parameter C in sklearn.svm.SVC) between [1e-5 and 1e5], incremented by 10 fold in each step. For random forest, we searched number of decision trees (parameter n_estimators in sklearn.ensemble.RandomForestClassifier) between [25, 50, 75]and maximum tree depth (parameter max_depth) between [3, 4, 5]. In the inner cross-validation loop, estimator’s score method is used to evaluate model.In outer cross-validation runs, AUROC is used to select model.Then we used the whole dataset as the training set and the other data set as the test set to evaluate the predictive performance of the selected genes across different data sets. we selected RF as the classifier and used 5-fold cross-validation to optimize number of decision trees and maximum tree depth.
  
  
  
- Modified

- We used cross validation for model selection. Samples were subjected to 5-fold cross-validation with 10 repeats ( RepeatedStratifiedKFold  in the scikit-learn package), yielding 50 class-balanced independent training and test sets in  a 80%–20% manner. AUROC was utilized as the performance metric. In each of these 50 runs, an additional 5-fold cross validation was applied for hyper-parameter optimization. We applied grid search strategy (GridSearchCV  in scikit-learn package)  to select hyper-parameters that maximize the classification score (score method of classifiers in scikit-learn package). The hyper-parameter space of each classifier was listed below:

  | classifier | hyper-parameter | search space     |implementation|
  | ---------- | --------------- | ---------------- | ---------- |
  | decision tree | max_depth | [3, 5, 7, 9] |sklearn.tree.DecisionTreeClassifier|
  | logistic regression | C | [1e-5,1e-4,1e-3,... 1e4,1e5] |sklearn.linear_model.LogisticRegression|
  | SVM | C | [1e-5,1e-4,1e-3,... 1e4,1e5] |sklearn.svm.SVC|
  | random forest | n_estimators | [25, 50, 75] |sklearn.ensemble.RandomForestClassifier|
  |  | max_depth | [3, 4, 5] |sklearn.ensemble.RandomForestClassifier|
  The same procedure was applied to exorbase and cfRNA dataset.

 

- raw
- In order to obtain better classification performance, we often need to combine different types of features. 52 qPCR data of 5 ncRNA and 6 miRNA include 26 healthy patients and 26 HCC patients. In order to determine the number of features and the best feature combination, we enumerated all feature combinations with the number of features 3,5,7, and 9. There are 1201 feature combinations. **We used 50 cross validation to evaluate the performance of different feature combinations in the RF model**. The whole cross-validation process was the same as above, divided into 50 outer cross-validation and 5 inner cross-validation. For feature combinations with different number, we only selected the one with a higher AUC mean in the test set with 50 cross-verifications. Due to the instability of random partition and small sample size, **we used the *level* one out cross-validation to obtain ROC curve and measured the prediction performance of different feature combinations**. The whole level one out cross-validation was a two-stage nested process. One sample is taken as the test set, the other samples are taken as the training set in outer level one out cross-validation runs (using the LeaveOneOut class in the scikit-learn Python package). The hyper-parameters of model were optimized by training set in 5 inner cross-validation runs. Finally, all the sample prediction results were merged to calculate the ROC curve of the entire data set, and the nine early sample prediction results were combined to calculate the ROC curve of the early data.
  - 最后用的到底是5折交叉验证重复10次还是LOO?




- Modified
- To determine the optimal feature subset among 11 features  (5ncRNA and 5 miRNA) in qPCR dataset, we traversed feature numbers of 3,5,7,9, and  all of the corresponding feature combinations (1201 in total). Due to the small sample size, we used leave one out cross validation (LeaveOneOut in scikit-learn package) to calculate AUROC as the measurement of prediction performance of different feature combinations. In each cross validation runs, the hyper-parameters were tuned the same way as described in model selection. 
