Large-Scale Click-Through Rate (CTR) Prediction
1. Introduction
Click-Through Rate (CTR) prediction is a fundamental task in digital advertising and recommendation systems. Accurately estimating the likelihood of user interactions is critical for optimizing revenue and engagement. This project develops a scalable machine learning pipeline using XGBoost and TensorFlow to handle the immense volume of ad impression data while ensuring efficient memory usage.

2. Dataset OverviewThe project utilizes large-scale advertising logs processed in a high-performance computing environment.Feature Composition: 13 Numerical continuous-valued attributes ($I1-I13$) and 26 Categorical discrete attributes ($C1-C26$).Target Variable: Binary classification (1 = Clicked, 0 = Not Clicked).Scale: Approximately 400 million rows (~100 GB) of data processed.Processing: Data is loaded in chunks of 1,000,000 rows to accommodate memory constraints.

3. Data PreprocessingTo ensure data quality and model stability, the following steps were implemented:Handling Missing Values: Numerical features were imputed with the median; categorical features were replaced with "Unknown".Encoding: Applied Ordinal Encoding to categorical variables for compatibility with tree-based models.Scaling: Applied MinMax Scaling to numerical features to normalize ranges between $[0,1]$.

4. Model Development
4.1 XGBoost Configuration
Chosen for its efficiency with large tabular data and built-in regularization:

Method: tree_method: "hist" (optimized for large-scale data).

Parameters: 100 estimators, learning rate of 0.05, and a max depth of 5.

4.2 TensorFlow Configuration
A deep learning model designed to capture non-linear relationships:

Architecture: Sequential Neural Network with hidden layers (256, 128, 64), ReLU activation, and Dropout.

Techniques: SMOTE for class balancing, Early Stopping, and Learning Rate Reduction.

4.3 Incremental Training Strategy
A chunk-wise strategy was adopted to process massive data volumes:

Read data in batches of 1,000,000 rows.

Apply independent preprocessing to each chunk.

Split into 90% training and 10% test data.

Update model parameters iteratively with each new chunk.

5. Experimental ResultsPerformance was evaluated on an aggregated test set using Accuracy and ROC-AUC scores.
6. Model	           Accuracy	   ROC-AUC Score	  Total Rows Processed
XGBoost Classifier	 0.7381	     0.7089	          ~396,355,554 (~100GB)
TensorFlow Model	   0.6571	     0.6445	          ~100,000,000 (~25GB)

7. Conclusion & Future Directions
This study demonstrates that incremental learning allows for training on massive datasets without exceeding memory limits.

Key Takeaway: Both models efficiently handle large-scale tabular data, but XGBoost provided higher accuracy and ROC-AUC scores in this environment.

Future Work: Implementing Bayesian Optimization (Optuna) for hyperparameter tuning and exploring Transformer-based architectures like DeepFM.
