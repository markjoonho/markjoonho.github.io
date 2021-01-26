---
name: Heart Failure Predicting
tools: [Logistic Regression, XGBoost]
image: /assets/img/heart_fail_kaggle/output_11_1.png
description: This is the samll project for fun using the Kaggle Dataset.
---

# Import Library

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

# 1. Prepare Dataset

### Kaggle API Setting

```python
# Set API Username, API Key
import os
os.environ['KAGGLE_USERNAME'] = 'markjoonholee'
os.environ['KAGGLE_KEY'] = '3041de6eb89e34c46bd4d8e4d9944962'
```

### Download Data

<https://www.kaggle.com/andrewmvd/heart-failure-clinical-data>

```python
# Download Datasets Using Kaggle API
!kaggle datasets download -d andrewmvd/heart-failure-clinical-data
# Unzip Download Dataset
!unzip '*.zip'
```

    heart-failure-clinical-data.zip: Skipping, found more recently modified local copy (use --force to force download)
    Archive:  heart-failure-clinical-data.zip
    replace heart_failure_clinical_records_dataset.csv? [y]es, [n]o, [A]ll, [N]one, [r]ename: y
      inflating: heart_failure_clinical_records_dataset.csv

### Read CSV

```python
# Using Pandas Library
df = pd.read_csv("heart_failure_clinical_records_dataset.csv")
df.head()
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>anaemia</th>
      <th>creatinine_phosphokinase</th>
      <th>diabetes</th>
      <th>ejection_fraction</th>
      <th>high_blood_pressure</th>
      <th>platelets</th>
      <th>serum_creatinine</th>
      <th>serum_sodium</th>
      <th>sex</th>
      <th>smoking</th>
      <th>time</th>
      <th>DEATH_EVENT</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>75.0</td>
      <td>0</td>
      <td>582</td>
      <td>0</td>
      <td>20</td>
      <td>1</td>
      <td>265000.00</td>
      <td>1.9</td>
      <td>130</td>
      <td>1</td>
      <td>0</td>
      <td>4</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>55.0</td>
      <td>0</td>
      <td>7861</td>
      <td>0</td>
      <td>38</td>
      <td>0</td>
      <td>263358.03</td>
      <td>1.1</td>
      <td>136</td>
      <td>1</td>
      <td>0</td>
      <td>6</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>65.0</td>
      <td>0</td>
      <td>146</td>
      <td>0</td>
      <td>20</td>
      <td>0</td>
      <td>162000.00</td>
      <td>1.3</td>
      <td>129</td>
      <td>1</td>
      <td>1</td>
      <td>7</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>50.0</td>
      <td>1</td>
      <td>111</td>
      <td>0</td>
      <td>20</td>
      <td>0</td>
      <td>210000.00</td>
      <td>1.9</td>
      <td>137</td>
      <td>1</td>
      <td>0</td>
      <td>7</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>65.0</td>
      <td>1</td>
      <td>160</td>
      <td>1</td>
      <td>20</td>
      <td>0</td>
      <td>327000.00</td>
      <td>2.7</td>
      <td>116</td>
      <td>0</td>
      <td>0</td>
      <td>8</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>

# 2. EDA and Statistical Approach

### Analyze Each Columns

```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 299 entries, 0 to 298
    Data columns (total 13 columns):
     #   Column                    Non-Null Count  Dtype
    ---  ------                    --------------  -----
     0   age                       299 non-null    float64
     1   anaemia                   299 non-null    int64
     2   creatinine_phosphokinase  299 non-null    int64
     3   diabetes                  299 non-null    int64
     4   ejection_fraction         299 non-null    int64
     5   high_blood_pressure       299 non-null    int64
     6   platelets                 299 non-null    float64
     7   serum_creatinine          299 non-null    float64
     8   serum_sodium              299 non-null    int64
     9   sex                       299 non-null    int64
     10  smoking                   299 non-null    int64
     11  time                      299 non-null    int64
     12  DEATH_EVENT               299 non-null    int64
    dtypes: float64(3), int64(10)
    memory usage: 30.5 KB

```python
df.describe()
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>anaemia</th>
      <th>creatinine_phosphokinase</th>
      <th>diabetes</th>
      <th>ejection_fraction</th>
      <th>high_blood_pressure</th>
      <th>platelets</th>
      <th>serum_creatinine</th>
      <th>serum_sodium</th>
      <th>sex</th>
      <th>smoking</th>
      <th>time</th>
      <th>DEATH_EVENT</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>299.000000</td>
      <td>299.000000</td>
      <td>299.000000</td>
      <td>299.000000</td>
      <td>299.000000</td>
      <td>299.000000</td>
      <td>299.000000</td>
      <td>299.00000</td>
      <td>299.000000</td>
      <td>299.000000</td>
      <td>299.00000</td>
      <td>299.000000</td>
      <td>299.00000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>60.833893</td>
      <td>0.431438</td>
      <td>581.839465</td>
      <td>0.418060</td>
      <td>38.083612</td>
      <td>0.351171</td>
      <td>263358.029264</td>
      <td>1.39388</td>
      <td>136.625418</td>
      <td>0.648829</td>
      <td>0.32107</td>
      <td>130.260870</td>
      <td>0.32107</td>
    </tr>
    <tr>
      <th>std</th>
      <td>11.894809</td>
      <td>0.496107</td>
      <td>970.287881</td>
      <td>0.494067</td>
      <td>11.834841</td>
      <td>0.478136</td>
      <td>97804.236869</td>
      <td>1.03451</td>
      <td>4.412477</td>
      <td>0.478136</td>
      <td>0.46767</td>
      <td>77.614208</td>
      <td>0.46767</td>
    </tr>
    <tr>
      <th>min</th>
      <td>40.000000</td>
      <td>0.000000</td>
      <td>23.000000</td>
      <td>0.000000</td>
      <td>14.000000</td>
      <td>0.000000</td>
      <td>25100.000000</td>
      <td>0.50000</td>
      <td>113.000000</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>4.000000</td>
      <td>0.00000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>51.000000</td>
      <td>0.000000</td>
      <td>116.500000</td>
      <td>0.000000</td>
      <td>30.000000</td>
      <td>0.000000</td>
      <td>212500.000000</td>
      <td>0.90000</td>
      <td>134.000000</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>73.000000</td>
      <td>0.00000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>60.000000</td>
      <td>0.000000</td>
      <td>250.000000</td>
      <td>0.000000</td>
      <td>38.000000</td>
      <td>0.000000</td>
      <td>262000.000000</td>
      <td>1.10000</td>
      <td>137.000000</td>
      <td>1.000000</td>
      <td>0.00000</td>
      <td>115.000000</td>
      <td>0.00000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>70.000000</td>
      <td>1.000000</td>
      <td>582.000000</td>
      <td>1.000000</td>
      <td>45.000000</td>
      <td>1.000000</td>
      <td>303500.000000</td>
      <td>1.40000</td>
      <td>140.000000</td>
      <td>1.000000</td>
      <td>1.00000</td>
      <td>203.000000</td>
      <td>1.00000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>95.000000</td>
      <td>1.000000</td>
      <td>7861.000000</td>
      <td>1.000000</td>
      <td>80.000000</td>
      <td>1.000000</td>
      <td>850000.000000</td>
      <td>9.40000</td>
      <td>148.000000</td>
      <td>1.000000</td>
      <td>1.00000</td>
      <td>285.000000</td>
      <td>1.00000</td>
    </tr>
  </tbody>
</table>
</div>

### Data Histogram (Continuous Data)

```python
sns.histplot(x='age', data=df, hue='DEATH_EVENT', kde=True)
```

    <matplotlib.axes._subplots.AxesSubplot at 0x7fb5dfd1c208>

<img src="/assets/img/heart_fail_kaggle/output_11_1.png">

```python
sns.histplot(data = df.loc[df['creatinine_phosphokinase'] < 3000, 'creatinine_phosphokinase'])
```

    <matplotlib.axes._subplots.AxesSubplot at 0x7fb5cae73c88>

<img src="/assets/img/heart_fail_kaggle/output_12_1.png">

```python
sns.histplot(x='ejection_fraction', data=df, bins=13, hue='DEATH_EVENT', kde=True)
```

    <matplotlib.axes._subplots.AxesSubplot at 0x7fb5cad8b748>

<img src="/assets/img/heart_fail_kaggle/output_13_1.png">

```python
sns.histplot(x='platelets', data=df, hue='DEATH_EVENT', kde=True)
```

    <matplotlib.axes._subplots.AxesSubplot at 0x7fb5caca05f8>

<img src="/assets/img/heart_fail_kaggle/output_14_1.png">

```python
sns.histplot(x='time', data=df, hue='DEATH_EVENT', kde=True)
```

    <matplotlib.axes._subplots.AxesSubplot at 0x7fb5b520c550>

<img src="/assets/img/heart_fail_kaggle/output_15_1.png">

```python
sns.jointplot(x='platelets', y='creatinine_phosphokinase', data=df, hue='DEATH_EVENT', alpha=0.3)
```

    <seaborn.axisgrid.JointGrid at 0x7fb5c1d27fd0>

<img src="/assets/img/heart_fail_kaggle/output_16_1.png">

```python
sns.jointplot(x='ejection_fraction', y='serum_creatinine', data=df, hue='DEATH_EVENT', alpha=0.3)
```

    <seaborn.axisgrid.JointGrid at 0x7fb5b4f09a58>

<img src="/assets/img/heart_fail_kaggle/output_17_1.png">

### BoxPlot (Categorical Data)

```python
sns.boxplot(x='DEATH_EVENT', y='ejection_fraction', data=df)
```

    <matplotlib.axes._subplots.AxesSubplot at 0x7fb5c1ac38d0>

<img src="/assets/img/heart_fail_kaggle/output_19_1.png">

```python
sns.boxplot(x='smoking', y='ejection_fraction', data=df)
```

    <matplotlib.axes._subplots.AxesSubplot at 0x7fb5c1a45ac8>

<img src="/assets/img/heart_fail_kaggle/output_20_1.png">

```python
sns.violinplot(x='DEATH_EVENT', y='ejection_fraction', data=df)
```

    <matplotlib.axes._subplots.AxesSubplot at 0x7fb5c199df28>

<img src="/assets/img/heart_fail_kaggle/output_21_1.png">

```python
sns.swarmplot(x='DEATH_EVENT', y='platelets', hue='smoking', data=df)
```

    /usr/local/lib/python3.6/dist-packages/seaborn/categorical.py:1296: UserWarning: 9.9% of the points cannot be placed; you may want to decrease the size of the markers or use stripplot.
      warnings.warn(msg, UserWarning)





    <matplotlib.axes._subplots.AxesSubplot at 0x7fb5c19ecf98>

<img src="/assets/img/heart_fail_kaggle/output_22_2.png">

# 3. Data Preprocessing

### Scailing StandardScaler

- Hence patient dead, time is not counting.
  Thus, time is not a good feature.

```python
from sklearn.preprocessing import StandardScaler
```

```python
df.columns
```

    Index(['age', 'anaemia', 'creatinine_phosphokinase', 'diabetes',
           'ejection_fraction', 'high_blood_pressure', 'platelets',
           'serum_creatinine', 'serum_sodium', 'sex', 'smoking', 'time',
           'DEATH_EVENT'],
          dtype='object')

```python
# Divide by numerical, categorical, output
X_num = df[['age', 'creatinine_phosphokinase', 'ejection_fraction', 'platelets', 'serum_creatinine', 'serum_sodium']]
X_cat = df[['anaemia', 'diabetes', 'high_blood_pressure', 'sex', 'smoking']]
y = df['DEATH_EVENT']
```

```python
# Scailing the Data
scaler = StandardScaler()
scaler.fit(X_num)
X_scaled = scaler.transform(X_num)
X_scaled = pd.DataFrame(data=X_scaled, index=X_num.index, columns= X_num.columns)
X = pd.concat([X_scaled, X_cat], axis=1)
X.head()
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>creatinine_phosphokinase</th>
      <th>ejection_fraction</th>
      <th>platelets</th>
      <th>serum_creatinine</th>
      <th>serum_sodium</th>
      <th>anaemia</th>
      <th>diabetes</th>
      <th>high_blood_pressure</th>
      <th>sex</th>
      <th>smoking</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.192945</td>
      <td>0.000166</td>
      <td>-1.530560</td>
      <td>1.681648e-02</td>
      <td>0.490057</td>
      <td>-1.504036</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-0.491279</td>
      <td>7.514640</td>
      <td>-0.007077</td>
      <td>7.535660e-09</td>
      <td>-0.284552</td>
      <td>-0.141976</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.350833</td>
      <td>-0.449939</td>
      <td>-1.530560</td>
      <td>-1.038073e+00</td>
      <td>-0.090900</td>
      <td>-1.731046</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.912335</td>
      <td>-0.486071</td>
      <td>-1.530560</td>
      <td>-5.464741e-01</td>
      <td>0.490057</td>
      <td>0.085034</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.350833</td>
      <td>-0.435486</td>
      <td>-1.530560</td>
      <td>6.517986e-01</td>
      <td>1.264666</td>
      <td>-4.682176</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>

### Train Test Split

```python
from sklearn.model_selection import train_test_split
```

```python
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=10)
```

# 4. Classification Modeling

### 1. Logistic Regression

```python
from sklearn.linear_model import LogisticRegression
```

```python
model_lr = LogisticRegression()
model_lr.fit(X_train, y_train)
```

    LogisticRegression(C=1.0, class_weight=None, dual=False, fit_intercept=True,
                       intercept_scaling=1, l1_ratio=None, max_iter=100,
                       multi_class='auto', n_jobs=None, penalty='l2',
                       random_state=None, solver='lbfgs', tol=0.0001, verbose=0,
                       warm_start=False)

### Evaluating Model

```python
from sklearn.metrics import classification_report
```

```python
pred = model_lr.predict(X_test)
print(classification_report(y_test, pred))
```

                  precision    recall  f1-score   support

               0       0.86      0.76      0.81        55
               1       0.50      0.65      0.57        20

        accuracy                           0.73        75
       macro avg       0.68      0.71      0.69        75
    weighted avg       0.76      0.73      0.74        75

### 2. XGBoost

```python
from xgboost import XGBClassifier
```

```python
model_xgb = XGBClassifier()
model_xgb.fit(X_train, y_train)
```

    XGBClassifier(base_score=0.5, booster='gbtree', colsample_bylevel=1,
                  colsample_bynode=1, colsample_bytree=1, gamma=0,
                  learning_rate=0.1, max_delta_step=0, max_depth=3,
                  min_child_weight=1, missing=None, n_estimators=100, n_jobs=1,
                  nthread=None, objective='binary:logistic', random_state=0,
                  reg_alpha=0, reg_lambda=1, scale_pos_weight=1, seed=None,
                  silent=None, subsample=1, verbosity=1)

### Evaluating Model

```python
pred = model_xgb.predict(X_test)
print(classification_report(y_test, pred))
```

                  precision    recall  f1-score   support

               0       0.89      0.73      0.80        55
               1       0.50      0.75      0.60        20

        accuracy                           0.73        75
       macro avg       0.69      0.74      0.70        75
    weighted avg       0.79      0.73      0.75        75

### Check Feature Importance

```python
plt.bar(X.columns, model_xgb.feature_importances_)
plt.xticks(rotation=90)
plt.show()
```

<img src="/assets/img/heart_fail_kaggle/output_44_0.png">

- Let's check the joint plot about the ejection_fraction and serum_creating.

# 5. Deep Analysis

### Precision-Recall Curve

```python
from sklearn.metrics import plot_precision_recall_curve
```

```python
fig = plt.figure()
ax = fig.gca()
plot_precision_recall_curve(model_lr, X_test, y_test, ax=ax)
plot_precision_recall_curve(model_xgb, X_test, y_test, ax=ax)
```

    <sklearn.metrics._plot.precision_recall_curve.PrecisionRecallDisplay at 0x7fb5b4e53710>

<img src="/assets/img/heart_fail_kaggle/output_48_1.png">

### Check ROC Curve

```python
from sklearn.metrics import plot_roc_curve
```

```python
fig = plt.figure()
ax = fig.gca()
plot_roc_curve(model_lr, X_test, y_test, ax=ax)
plot_roc_curve(model_xgb, X_test, y_test, ax=ax)
```

    <sklearn.metrics._plot.roc_curve.RocCurveDisplay at 0x7fb5b4cda048>

<img src="/assets/img/heart_fail_kaggle/output_51_1.png">
