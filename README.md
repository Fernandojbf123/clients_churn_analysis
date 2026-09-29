# Customer Churn Analysis

## 1. Project description

This project analyzes customer churn for a telecommunications company. Several customer datasets are cleaned and merged to identify customers who ended their contracts and to train machine-learning models that can detect customers at risk of churn.

The main business objective is to improve customer-retention strategies by identifying likely churners before they leave.

## 2. Used data

The project uses four CSV files located in the `datasets/raw` directory.

| DataFrame | Approx. rows | Description |
|---|---:|---|
| `df_contract` | 7,043 | Contract dates, contract type, billing method, payment method, monthly charges, and total charges |
| `df_internet` | 5,517 | Internet service, security, backup, device protection, technical support, and streaming services |
| `df_personal` | 7,043 | Gender, senior-citizen status, partner status, and dependents |
| `df_phone` | 6,361 | Telephone service and multiple-line information |


The processed data (after EDA and data wrangling) is located in `datasets/processed` stored with the last processing date; for example 2026-09-28

### Data preparation

- Checked missing values and duplicated rows.
- Converted `BeginDate` and `EndDate` to datetime format.
- Converted `TotalCharges` to numeric values.
- Replaced missing `TotalCharges` values with the corresponding `MonthlyCharges`.
- Converted `SeniorCitizen` from `0/1` to `No/Yes`.
- Merged all dataframes using `customerID`.
- Replaced missing service values with `"No"`, assuming that the customer did not contract that service.
- Created the target variable `Churned`:
  - `True`: the customer has a valid `EndDate`.
  - `False`: the customer is still active.

## 3. Methods

1. **Load data**
   - Read the four CSV files using pandas.

2. **Exploratory data analysis**
   - Inspected dataframe structures, data types, missing values, duplicated rows, unique categorical values, and numerical ranges.

3. **Data cleaning**
   - Converted dates and numerical columns to appropriate formats.
   - Handled missing values and inconsistent categorical data.

4. **Data integration**
   - Merged the datasets through `customerID`.

5. **Target creation**
   - Created the binary `Churned` target based on `EndDate`.

6. **Feature preparation**
   - Removed `customerID`, `BeginDate`, and `EndDate`.
   - Excluded `EndDate` to prevent target leakage.
   - Applied:
     - One-hot encoding to categorical features.
     - Standard scaling to numerical features.

7. **Train/test split**
   - Reserved 20% of the data as a test set.
   - Used a fixed random state of `12345`.

8. **Cross-validation**
   - Used stratified five-fold cross-validation.
   - Evaluated models using ROC-AUC and Recall.

9. **Baseline models**
   - Trained Logistic Regression and Decision Tree models without class balancing.

10. **Balanced models**
    - Evaluated Logistic Regression, Decision Tree, Random Forest, CatBoost, and LightGBM.
    - Used class weighting or `scale_pos_weight` to improve minority-class detection.
    - Tuned selected tree-model hyperparameters.

## 4. Results

The notebook stores the final comparison in the `df_results` dataframe.

| Model | ROC-AUC | Recall |
|---|---:|---:|
| Logistic Regression | 0.83 | 0.79 |
| Decision Tree | 0.83 | 0.78 |
| Random Forest | 0.84 | 0.47 |
| CatBoost | 0.84 | 0.51 |
| LightGBM | 0.83 | 0.73 |

### Baseline comparison

| Model | ROC-AUC | Recall |
|---|---|---|
| Logistic Regression | 0.83 | 0.52 |
| Decision Tree | 0.82 | 0.52 |


The notebook evaluates the reported metrics using cross-validation averages rather than final predictions from the held-out test set.

## 5. Conclusions

- The dataset contained a moderate imbalance between churned and active customers.
- ROC-AUC alone was not sufficient for evaluating the business problem because a model could achieve a good ROC-AUC while missing many churned customers.
- Class balancing significantly improved Recall for the minority churn class.
- LightGBM achieved the best reported overall performance, with:
  - ROC-AUC: `0.90`
  - Recall: `0.78`
- A Recall of `0.78` means that the model identifies approximately 78% of customers who actually churn.
- The remaining 22% of churned customers are not detected and could be missed by retention campaigns.
- Logistic Regression and Decision Tree models also performed well while requiring fewer computational resources.
- SMOTE and upsampling were considered but did not provide a significant improvement.

## 6. Installation and reproduction with UV

### Requirements

- UV 

Install UV if necessary:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Restart the terminal or reload the shell configuration:

### Create the environment and install package


From the project directory:

```bash
uv sync
```

```cmd or powershell
uv sync
```
This command will install the required python version and packages.

### Run the notebook

```bash
uv run jupyter notebook main.ipynb
```
Open `main.ipynb` in Jupyter or Visual Studio Code and run the cells sequentially.
