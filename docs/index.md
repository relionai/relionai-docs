---
sidebar-position: 1
title: Welcome
slug: /
---

## Introduction



<img src="https://res.cloudinary.com/doav6cltv/image/upload/v1645737615/brandmark-design-1000x0_ywje70.png" style="align: center;" />



Welcome to Relion.ai

Relion is a ML risk management platform that Relion's ML risk management platform uses synthetic data generation, fairness data enrichment, and state-of-the-art ML testing methods to keep ML models robust, fair, and compliant.

With Relion you can add compehensive ML behaviour testing, fairness testing,  ML drift monitoring, fairness monitoring, and alerting into your stack with just a few Lines of code.

## Key Features

- ** Robustness Testing: **Uncover hidden behavioural vulnerabilities and risks in machine learning pipelines using use-case specific pre-built test suites and synthetic data generation techniques to maximize feature space exploration.


- ** Fairness Testing: **Evaluate model fairness risks against protected classes without exposing sensitive data using Relion.ai's fairness API.

- ** Serverless ML Monitoring: **Monitor data drift, model drift and segment drift of production models without any additional infrastruce setup using Relion.ai's serverless instrumentation framework.

- ** Serverless Fairness Monitoring: **Fairness drift of production models without without exposing sensitive data or setting up additional infrastruce using Relion.ai's serverless instrumentation framework.

- ** ML Specific Compliance Testing: **Ensure that models are always in compliance with the latests industry regulations without expensive one-time audits using Relion.ai's database of industry compliance tests.

## Installation

To get started using Relionai you need to:

Generate a `RELION_API_KEY` by signing up to the <a href="https://relion.ai">Relion.ai</a> platform

`then`

Relion.ai's agent SDK can be installed using `pip` like so:

<div class="termy">

```console
$ pip install relionai

---> 100%
```

</div>

## Robustness Testing 

Relion.ai's agent SDK can be used to evaluate robustness of any machine learning model using a few lines of code. The types of tests to run depend on the input data types.

### Tabular data

Running a tabular data suite:

```python
os.environ["RELION_API_KEY"] = "YOUR_RELION API_KEY"

relionai.tabular.run_all_tests(
    df_ref: pd.DataFrame = X_train,
    label_ref: pd.Series = y_train,
    df_test: pd.DataFrame = X_test,
    label_test: pd.Series = y_test,
    pipeline: sklearn.pipeline.Pipeline = ml_pipeline_object(),
    model_name: str = "Demo"
    )
```

Tabular data robustness testing examples:

=== "Sklearn"

    ```python hl_lines="22-23"
    import sklearn
    import relionai
    from sklearn import datasets
    from sklearn.ensemble import GradientBoostingClassifier
    from sklearn.preprocessing import StandardScaler
    from sklearn.datasets import make_classification
    from sklearn.model_selection import train_test_split
    from sklearn.pipeline import Pipeline

    #create dataset
    X, y = make_classification()
    X_train, X_test, y_train, y_test = train_test_split(X, y)

    #train ml pipeline
    pipe = Pipeline([
        ('scaler', StandardScaler()),
        ('model', GradientBoostingClassifier())
    ])
    pipe.fit(X_train, y_train)
   
    # run relion.ai tests
    os.environ["RELION_API_KEY"] = "YOUR_RELION API_KEY"
    relionai.tabular.run_all_tests(
        df_ref = X_train,
        label_ref = y_train,
        df_test = X_test,
        label_test = y_test,
        pipeline = pipe,
        model_name = "sklearn_gbm_demo"
        )
    ```

=== "xgboost"

    ```python hl_lines="38-39"
    import pandas as pd
    import xgboost as xgb
    import relionai
    from sklearn.compose import ColumnTransformer
    from sklearn.model_selection import train_test_split, cross_val_score
    from sklearn.impute import SimpleImputer
    from sklearn.preprocessing import OneHotEncoder, MinMaxScaler
    from sklearn.decomposition import PCA
    from sklearn.pipeline import Pipeline
    

    # create dataset
    df = pd.read_csv('https://raw.githubusercontent.com/stedy/Machine-Learning-with-R-datasets/master/insurance.csv')
    y = df['charges']
    X = df.drop('charges', 1)
    X_train, X_test, y_train, y_test = train_test_split(X, y)

    # train ml pipeline
    prepocess_transformation_1 = ColumnTransformer(transformers =[
        ('cat', SimpleImputer(strategy ='most_frequent'), ['sex', 'smoker', 'region']),
        ('num', SimpleImputer(strategy ='median'), ['age', 'bmi', 'children']),   
        ], remainder ='passthrough'
    )
    prepocess_transformation_2 = ColumnTransformer(transformers =[
        ('enc', OneHotEncoder(sparse = False, drop ='first'), list(range(3))),
    ], remainder ='passthrough'
    )
    pipe = Pipeline(steps =[
        ('tf1', prepocess_transformation_1),
        ('tf2', prepocess_transformation_22),
        ('tf3', MinMaxScaler()), 
        ('pca', PCA()), 
        ('model', xgb.XGBClassifier()),
    ])
    pipe.fit(X_train, y_train)

    # run relion.ai tests
    os.environ["RELION_API_KEY"] = "YOUR_RELION API_KEY"
    relionai.tabular.run_all_tests(
        df_ref = X_train,
        label_ref = y_train,
        df_test = X_test,
        label_test = y_test,
        pipeline = pipe,
        model_name = "xgb_gbm_demo"
    ```

=== "LightGBM"

    ```python  hl_lines="36-37"
    import pandas as pd
    import relionai
    from lightgbm import LGBMClassifier
    from sklearn.compose import ColumnTransformer
    from sklearn.model_selection import train_test_split, cross_val_score
    from sklearn.impute import SimpleImputer
    from sklearn.preprocessing import OneHotEncoder, MinMaxScaler
    from sklearn.pipeline import Pipeline
    

    # create dataset
    df = pd.read_csv('https://raw.githubusercontent.com/stedy/Machine-Learning-with-R-datasets/master/insurance.csv')
    y = df['charges']
    X = df.drop('charges', 1)
    X_train, X_test, y_train, y_test = train_test_split(X, y)

    # train ml pipeline
    prepocess_transformation_1 = ColumnTransformer(transformers =[
        ('cat', SimpleImputer(strategy ='most_frequent'), ['sex', 'smoker', 'region']),
        ('num', SimpleImputer(strategy ='median'), ['age', 'bmi', 'children']),   
        ], remainder ='passthrough'
    )
    prepocess_transformation_2 = ColumnTransformer(transformers =[
        ('enc', OneHotEncoder(sparse = False, drop ='first'), list(range(3))),
    ], remainder ='passthrough'
    )
    pipe = Pipeline(steps =[
        ('tf1', prepocess_transformation_1),
        ('tf2', prepocess_transformation_22),
        ('tf3', MinMaxScaler()), 
        ('model', LGBMClassifier(n_jobs=-1)),
    ])
    pipe.fit(X_train, y_train)

    # run relion.ai tests
    os.environ["RELION_API_KEY"] = "YOUR_RELION API_KEY"
    relionai.tabular.run_all_tests(
        df_ref = X_train,
        label_ref = y_train,
        df_test = X_test,
        label_test = y_test,
        pipeline = pipe,
        model_name = "lightgbm_gbm_demo"
    ```


=== "Pytorch"

    ```python hl_lines="45-46"
    import numpy as np
    import relionai
    import torch.nn.functional as F
    from sklearn.datasets import make_classification
    from torch import nn
    from skorch import NeuralNetClassifier
    from sklearn.pipeline import Pipeline
    from sklearn.preprocessing import StandardScaler

    # create dataset
    X, y = make_classification(1000, 20, n_informative=10, random_state=0)
    X = X.astype(np.float32)
    y = y.astype(np.int64)
    X_train, X_test, y_train, y_test = train_test_split(X, y)

    # create model
    class MyModule(nn.Module):
        def __init__(self, num_units=10, nonlin=F.relu):
            super(MyModule, self).__init__()
            self.dense0 = nn.Linear(20, num_units)
            self.nonlin = nonlin
            self.dropout = nn.Dropout(0.5)
            self.dense1 = nn.Linear(num_units, 10)
            self.output = nn.Linear(10, 2)
        def forward(self, X, **kwargs):
            X = self.nonlin(self.dense0(X))
            X = self.dropout(X)
            X = F.relu(self.dense1(X))
            X = F.softmax(self.output(X))
            return X

    net = NeuralNetClassifier(
        MyModule,
        max_epochs=10,
        lr=0.1
    )

    # train ml pipeline
    pipe = Pipeline([
    ('scale', StandardScaler()),
    ('net', net),
    ])
    pipe.fit(X_train, y_train)

    # run relion.ai tests
    os.environ["RELION_API_KEY"] = "YOUR_RELION API_KEY"
    relionai.tabular.run_all_tests(
        df_ref = X_train,
        label_ref = y_train,
        df_test = X_test,
        label_test = y_test,
        pipeline = pipe,
        model_name = "pytorch_pipeline_demo"
        )

    ```

=== "Keras"

    ```python hl_lines="40-41"
    import pandas as pd
    import numpy as np
    import tensorflow as tf
    from keras import backend, callbacks, layers, models
    from keras.wrappers.scikit_learn import KerasClassifier
    from sklearn.pipeline import Pipeline
    from sklearn.preprocessing import StandardScaler()

    # create dataset
    X_train = np.random.random((1000, 3))
    y_train = pd.get_dummies(np.argmax(X_train[:, :3], axis=1)).values
    X_test = np.random.random((100, 3))
    y_test = pd.get_dummies(np.argmax(X_test[:, :3], axis=1)).values

    # create model
    sess = tf.Session()
    backend.set_session(sess)
    def model():
        model = models.Sequential([
            layers.Dense(64, input_dim=X_train.shape[1], activation='relu'),
            layers.Dropout(0.5),
            layers.Dense(64, activation='relu'),
            layers.Dropout(0.5),
            layers.Dense(1, activation='sigmoid')
        ])
        model.compile(loss='binary_crossentropy', optimizer='rmsprop', metrics=['accuracy'])
        return model
    
    # train pipeline
    early_stopping = callbacks.EarlyStopping(monitor='val_loss', patience=1, verbose=0, mode='auto')

    pipe = pipeline.Pipeline([
        ('rescale', StandardScaler()),
        ('nn', KerasClassifier(build_fn=model, nb_epoch=10, batch_size=128,
                            validation_split=0.2, callbacks=[early_stopping]))
    ])
    pipe.fit(X_train, y_train)

    # run relion.ai tests
    os.environ["RELION_API_KEY"] = "YOUR_RELION API_KEY"
    relionai.tabular.run_all_tests(
        df_ref = X_train,
        label_ref = y_train,
        df_test = X_test,
        label_test = y_test,
        pipeline = pipe,
        model_name = "keras_pipeline_demo"
        )
    ```

=== "Any (ONNX)"

    ```python
    # Full list of support frameworks: https://github.com/onnx/tutorials/blob/main/README.md#converting-to-onnx-format

    import relionai
    from sklearn.linear_model import LogisticRegression
    from sklearn.pipeline import Pipeline
    from sklearn.preprocessing import StandardScaler, OneHotEncoder
    from sklearn.impute import SimpleImputer
    from sklearn.decomposition import TruncatedSVD
    from sklearn.compose import ColumnTransformer
    from skl2onnx import convert_sklearn
    from skl2onnx.common.data_types import FloatTensorType, StringTensorType


    numeric_features = [0, 1, 2] # ["vA", "vB", "vC"]
    categorical_features = [3, 4] # ["vcat", "vcat2"]

    classifier = LogisticRegression(C=0.01, class_weight=dict(zip([False, True], [0.2, 0.8])),
                                    n_jobs=1, max_iter=10, solver='lbfgs', tol=1e-3)

    numeric_transformer = Pipeline(steps=[
        ('imputer', SimpleImputer(strategy='median')),
        ('scaler', StandardScaler())
    ])

    categorical_transformer = Pipeline(steps=[
        ('onehot', OneHotEncoder(sparse=True, handle_unknown='ignore')),
        ('tsvd', TruncatedSVD(n_components=1, algorithm='arpack', tol=1e-4))
    ])

    preprocessor = ColumnTransformer(
        transformers=[
            ('num', numeric_transformer, numeric_features),
            ('cat', categorical_transformer, categorical_features)
        ])

    pipe = Pipeline(steps=[
        ('precprocessor', preprocessor),
        ('classifier', classifier)
    ])

    initial_type = [('numfeat', FloatTensorType([None, 3])),
                    ('strfeat', StringTensorType([None, 2]))]
    model_onnx = convert_sklearn(model, initial_types=initial_type)
    ```

<div class="termy" style="height: 500px;overflow-y: scroll;">

```console
$ python example_code_above.py
Generating synthetic test dataset...
Running model performance tests...
Train AUC: 0.57  Test AUC: 0.57  Synthetic AUC: 0.57
Train Accuracy: 0.98  Test Accuracy: 0.98  Synthetic Accuracy: 0.57
Train Precision: 0.98  Test Precision: 0.98  Synthetic Precision: 0.57
============ MODEL PERFORMANCE TESTS =================
Description : Variation of model performance across datasets
Train Data vs. Test Data :
🟢 0.0% accuracy difference between datasets
🟢 0.0% accuracy difference between datasets
🟢 0.0% accuracy difference between datasets
Train Data vs. Synthetic Data :
🟢 0.0% accuracy difference between datasets
🟠 41.0% accuracy difference between datasets
🟠 41.0% accuracy difference between datasets
============= BIAS-VARIANCE DECOMPOSITION TESTS ===============
Description : Overfit or underfit indication
Train Data vs. Test Data :
⭕ 0.02 average expected loss between datasets
⭕ 0.02 average bias between datasets
⭕ 0.0 average variance between datasets
============== FEATURES CORRELARATION TESTS ================
Description : Feature correlation analysis
Train Data :
🟡 242 of 422 features are over 80.0% correlated
🟠 241 of 422 features are over 90.0% correlated
🟢 0 of 422 features are over 100.0% correlated
=============== FEATURE SPACE EXPLORATION TESTS ==================
Description : Underperforming intervals in feature space
Synthetic Data :
🟡 559 groups have over 10.0% lower AUC than overall population
🟢 0 groups have over 20.0% lower AUC than overall population
🟢 0 groups have over 50.0% lower AUC than overall population
========= CLASS IMBALANCE TESTS ==================
Description : Distribution of target classes
Train Data :
🔴 97.3% class imbalance identified in dataset
Test Data :
🔴 97.53% class imbalance identified in dataset
Synthetic Data :
🟢 0.0% class imbalance identified in dataset
============ FEATURES DISTRIBUTION TESTS ===================
Description : Distribution of ouliters in model features
Train Data :
🟡 351 of 422 features have outliers over 5x standard deviation
🟠 281 of 422 features have outliers over 10x standard deviation
🔴 8 of 422 features have outliers over 100x standard deviation
Test Data :
🟡 342 of 422 features have outliers over 5x standard deviation
🟠 277 of 422 features have outliers over 10x standard deviation
🔴 24 of 422 features have outliers over 100x standard deviation
================= COMPLETENESS TESTS =========================
Train Data :
🟢 0 of 422 features are over 10.0% missing
🟢 0 of 422 features are over 20.0% missing
🟢 0 of 422 train features are over 50.0% missing
Test Data :
🟢 0 of 422 features are over 10.0% missing
🟢 0 of 422 features are over 20.0% missing
🟢 0 of 422 train features are over 50.0% missing
=========== FEATURE IMPORTANCE DISTRIBUTION TESTS ==================
Description : Percentage of feature contibution to predictive performance
⭕ 3 features account for over 80% of performance
⭕ 15 features account for under 1% of performance
<font color="#FF0000">----Total Low Risks: 4, Total Med Risks: 5, Total High Risks: 4 ---</font>
```

</div>

### Timeseries data

Running a timeseries data suite:

```python
os.environ["RELION_API_KEY"] = "YOUR_RELION API_KEY" 

relionai.timeseries.run_all_tests(
    df: pd.DataFrame = df,
    date_col: str = 'name_of_date_column',
    quantity_col: str ='name_of_quantity_column', 
    model: Callable[pd.DataFrame, pd.Series] = ExponentialSmoothing,
    model_name: str = "timeseries_model_demo",
    train_period: Optional[int] = 365,
    test_period: Optional[int] = 120,
    freq: Optional[str] = 'days',
    segmentation_col: Optional[str] ='name_of_segmentation_col', 
    validation_split_date: Optional[datetime.date] = None
)
```

Timeseries data robustness testing examples:

=== "StatsModels"

    ```python hl_lines="9-10"
    import pandas as pd
    import relionai
    from statsmodels.tsa.holtwinters import ExponentialSmoothing

    # create dataset
    data_pdf = pd.read_csv('all_stocks_5yr.csv')

    # run relion.ai robustness tests
    os.environ["RELION_API_KEY"] = "YOUR_RELION API_KEY" 
    relionai.timeseries.run_all_tests(
        df: pd.DataFrame = df,
        date_col: str = 'name_of_date_column',
        quantity_col: str ='name_of_quantity_column', 
        model: Callable[pd.DataFrame, pd.Series] = ExponentialSmoothing,
        model_name: str = "timeseries_model_demo",
        train_period: Optional[int] = 365,
        test_period: Optional[int] = 120,
        freq: Optional[str] = 'days',
        segmentation_col: Optional[str] ='name_of_segmentation_col',
        validation_split_date: Optional[datetime.date] = None
    )
    ```

=== "Prophet"

    ```python hl_lines="9-10"
    import pandas as pd
    import relionai
    from statsmodels.tsa.holtwinters import ExponentialSmoothing

    # create dataset
    data_pdf = pd.read_csv('all_stocks_5yr.csv')

    # run relion.ai robustness tests
    os.environ["RELION_API_KEY"] = "YOUR_RELION API_KEY"
    relionai.timeseries.run_all_tests(
        df: pd.DataFrame = df,
        date_col: str = 'name_of_date_column',
        quantity_col: str ='name_of_quantity_column', 
        model: Callable[pd.DataFrame, pd.Series] = ExponentialSmoothing,
        model_name: str = "timeseries_model_demo",
        train_period: Optional[int] = 365,
        test_period: Optional[int] = 120,
        freq: Optional[str] = 'days',
        segmentation_col: Optional[str] ='name_of_segmentation_col',
        validation_split_date: Optional[datetime.date] = None
    )
    ```

=== "Custom (Any Forecaster)"

    ```python hl_lines="9-10"
    import pandas as pd
    import relionai
    from statsmodels.tsa.holtwinters import ExponentialSmoothing

    # create dataset
    data_pdf = pd.read_csv('all_stocks_5yr.csv')

    # run relion.ai robustness tests
    os.environ["RELION_API_KEY"] = "YOUR_RELION API_KEY"
    relionai.timeseries.run_all_tests(
        df: pd.DataFrame = df,
        date_col: str = 'name_of_date_column',
        quantity_col: str ='name_of_quantity_column', 
        model: Callable[pd.DataFrame, pd.Series] = ExponentialSmoothing,
        model_name: str = "timeseries_model_demo",
        train_period: Optional[int] = 365,
        test_period: Optional[int] = 120,
        freq: Optional[str] = 'days',
        segmentation_col: Optional[str] ='name_of_segmentation_col',
        validation_split_date: Optional[datetime.date] = None
    )
    ```

### Natural Language data

`Coming soon`

### RecSys data

`Coming soon`

### Image data

`Coming soon`

## Fairness Testing

Relion.ai's fairness API can be used to evaluate fairness of any machine learning model without exposing sensitive data using a sha256 hashed email and/or a 5 digit zipcode as an annonymized identifiers.

Running a tabular data suite:

```python
os.environ["RELION_API_KEY"] = "YOUR_RELION API_KEY" 

relionai.fairness.run_fairness_tests(
    df: pd.DataFrame = df,  
    label_col: str = "name_of_label_column",
    pred_col:str = "name_of_prediction_column",
    model_name: str = "Demo",
    email_col: Optional[str] = "name_of_sha256_email_column", 
    postal_col: Optional[str] = "name_zipcode_column"
    )
```

<div class="termy">

```console
$ python example_code_above.py
Gathering fairness enrichment features ...
======================= MODEL FAIRNESS TESTS =========================
Description : Model performance across fairness features
🟡 12% difference in model perfomance between features gender classes
🟠 30% difference in model perfomance between features age classes
🟢 7% difference in model perfomance between features income classes
🟠 50% difference in model perfomance between features locale classes
<font color="#FF0000">----Total Low Risks: 1, Total Med Risks: 2, Total High Risks: 0 ---</font>
```

</div>

## Serverless ML Monitoring

Relion.ai's agent SDK can be used to monitor models in production without the need for any additional infrastucture or integration:

```python
os.environ["RELION_API_KEY"] = "YOUR_RELION API_KEY" 
with relionai.Monitoring(model_name="demo_model", batch_rotation='day') as monitor:
    #dataframe
    monitor.df(df)

    #dict1
    monitor.dict(
        {
        "feature_name_1": 0.2, 
        "feature_name_2": "female",
        "feature_name_3": True,
        "prediction": 0.89
        }
    )

    #dict2
    monitor.dict(
        {
        "input": "example input text to model",
        "prediction": 0.89
        }
    )

    #images
    monitor.image("path/to/image.png")
```

## Serverless Fairness Monitoring

Relion.ai's agent SDK can be used to monitor fairness of models in production without the need for any additional infrastucture or integration:

```python
os.environ["RELION_API_KEY"] = "YOUR_RELION API_KEY"

with relionai.Fairness(model_name="example", batch_rotation='day') as fair_monitor:
    #dataframe
    fair_monitor.df(
        df,
        email_col="name_of_sha256_email_column",
        postal_col="name_zipcode_column",
        preds_col="name_of_prediction_column"
    ) 

    #dict
    fair_monitor.dict(
        {
        "email": "sha256 hashed email",
        "postal_code": "90210",
        "prediction": 0.89
        }
    )
```

## ML Specific Compliance Testing

`Coming Soon`