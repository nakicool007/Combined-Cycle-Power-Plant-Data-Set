# DSCI 552 - Homework 2

**Author:** Nakul Kumbria

**GitHub:** nakicool007

Analysis of the Combined Cycle Power Plant dataset using exploratory plots, linear and polynomial regression, interaction models, and KNN regression. The notebook contains the implementation, saved outputs, and written interpretations for Problem 1, along with answers to ISLR exercises 2.4.1 and 2.4.7.

## Project structure

```text
.
|-- README.md
|-- .gitignore
|-- data/
|   `-- combined_cycle_power_plant/
|       |-- Folds5x2_pp.xlsx
|       `-- Folds5x2_pp.ods
|-- notebook/
|   |-- Kumbria_Nakul_HW2.ipynb
|   |-- coefficient_comparison.png
|   |-- nonlinear_regression.png
|   `-- knn_regression_errors.png
`-- requirements.txt
```

The local `.venv/` environment is excluded from version control. The PNG files are saved plot artifacts; the notebook also displays plots inline.

## Dataset

The analysis reads the first worksheet (`sheet_name=0`, Sheet 1) of `Folds5x2_pp.xlsx`: **9,568 observations and 5 variables**. Each row contains hourly average measurements from the power plant operating at full load.

- **AT:** Ambient temperature (degrees Celsius).
- **V:** Exhaust vacuum (cm Hg).
- **AP:** Ambient pressure (millibars).
- **RH:** Relative humidity (%).
- **PE:** Net electrical energy output (MW), the response variable.

AT, V, AP, and RH are the predictors. Only the first sheet is used; the workbook's shuffled sheets are not combined. The ODS file is retained as an alternative dataset format and is not loaded by the notebook. Potential outliers are flagged for discussion but retained in the analysis.

## Completed work

- [x] **1(a): Dataset preparation.** Dataset files are included, and the notebook loads Sheet 1.
- [x] **1(b): Exploration.** Reports the dimensions and variable meanings, plots all ten unique variable pairs, discusses relationships, and calculates mean, median, range, quartiles, and interquartile range.
- [x] **1(c): Simple linear regression.** Fits PE separately against each predictor; reports slopes, standard errors, 95% confidence intervals, p-values, and R-squared. Includes fitted-line and residual plots, flags absolute standardized residuals above 3, and discusses potential outliers.
- [x] **1(d): Multiple linear regression.** Fits all four predictors together and interprets coefficients and their significance.
- [x] **1(e): Coefficient comparison.** Plots simple versus multiple regression coefficients and explains changes, including the RH sign reversal.
- [x] **1(f): Nonlinearity.** Fits separate cubic regressions using centered and scaled predictors, compares them with linear fits, and jointly tests the squared and cubed terms using F-tests.
- [x] **1(g): Interactions.** Fits all four main effects and six pairwise interactions, reports individual interaction tests, and compares the interaction model with the additive model using a joint F-test.
- [x] **1(h): Regression model selection.** Creates a reproducible 70/30 train/test split, compares the four-predictor baseline with a quadratic and interaction model, and performs backward elimination at a 5% significance level while preserving the main effects required by retained higher-order terms.
- [x] **1(i): KNN regression.** Evaluates every integer k from 1 through 100 with raw and min-max normalized features, reports the best k for each, and plots training and test MSE against 1/k.
- [x] **1(j): Model comparison.** Compares the best regression and KNN results and discusses prediction, interpretability, and evaluation limitations.
- [x] **Problem 2 / ISLR 2.4.1.** Written answers on flexible versus inflexible methods for different sample sizes, predictor counts, nonlinear relationships, and noise levels.
- [x] **Problem 3 / ISLR 2.4.7.** Calculates Euclidean distances, orders neighbors, predicts Green for K = 1 and Red for K = 3, and discusses K for a nonlinear decision boundary.

## Results recorded in the notebook

Temperature is the strongest individual linear predictor, with R-squared approximately **0.899**. All four simple-regression slopes are significant at 5%. Using all four predictors increases R-squared to approximately **0.929**, and all four coefficients remain significant. RH changes from a positive simple-regression slope to a negative slope after adjusting for the other predictors.

The cubic-model joint tests indicate nonlinearity for all four predictors, although the improvement for RH is small. In the full interaction model, **AT:V, AT:RH, V:AP, and AP:RH** are significant at 5%; R-squared increases to approximately **0.9363**.

For prediction, the notebook uses `np.random.default_rng(42)` to shuffle the observations, assigning **6,697 to training** and **2,871 to testing**. Regression scaling and KNN normalization are fitted on training data only. All models below use that same split; MSE is measured in **MW-squared**.

- **Linear regression, all four predictors:** training MSE **20.5575**; test MSE **21.2777**.
- **Selected quadratic and interaction regression:** training MSE **17.8569**; test MSE **18.7383**.
- **Raw-feature KNN, k = 7:** training MSE **11.6932**; test MSE **16.9652**.
- **Normalized-feature KNN, k = 6:** training MSE **9.9151**; test MSE **15.5542**.

Backward elimination removes **AT:AP**, **V:RH**, and **V-squared**, in that order. The selected regression retains the intercept, all four main effects, AT-squared, AP-squared, RH-squared, and AT:V, AT:RH, V:AP, and AP:RH.

Normalized KNN with **k = 6** has the lowest recorded test MSE, approximately **17% lower** than the selected regression model. These results describe one split. Because k was selected using test MSE, that test set also served as a tuning set; an independent performance estimate would require training-only cross-validation followed by evaluation on a separate test set. The notebook discusses this limitation. Regression significance tests use the usual model assumptions and a 5% cutoff without a multiple-testing adjustment.

## Environment and running the notebook

From the project root in PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m jupyterlab
```

Open `notebook/Kumbria_Nakul_HW2.ipynb`, select the virtual environment's Python kernel, and run the cells from top to bottom. Later sections reuse data, fitted models, and the train/test split created earlier.

**Data path:** The notebook currently reads the workbook using an absolute Windows path under the author's project directory. If you move or clone the project, update the `pd.read_excel` path before running. When the kernel's working directory is `notebook/`, the relative path is `../data/combined_cycle_power_plant/Folds5x2_pp.xlsx`.

The first code cell also contains a `%pip install` command for core analysis packages. `requirements.txt` provides the full environment, including JupyterLab and optional ODS support:

- `jupyterlab`, `ipykernel`: notebook editing and execution.
- `numpy`, `pandas`: numerical operations and data handling.
- `openpyxl`, `odfpy`: Excel and optional ODS readers.
- `matplotlib`, `seaborn`: plotting.
- `scipy`, `statsmodels`: regression inference and statistical tests.
- `scikit-learn`: KNN regression, scaling, and machine learning utilities.

Dependencies are not version-pinned, so exact numerical output can vary slightly across environments. The plotting cells display figures with `plt.show()`; they do not automatically overwrite the saved PNG files.

## Dataset provenance

The dataset notes describe measurements collected between 2006 and 2011 from a combined cycle power plant operating at full load, combining gas turbines, steam turbines, and heat recovery steam generators.

References retained from the original dataset documentation:

- Pinar Tufekci. *Prediction of full load electrical power output of a base load operated combined cycle power plant using machine learning methods*. International Journal of Electrical Power & Energy Systems, 60, September 2014, pp. 126-140. DOI: 10.1016/j.ijepes.2014.02.027.
- Heysem Kaya, Pinar Tufekci, and Sadik Fikret Gurgen. *Local and Global Learning Methods for Predicting Power of a Combined Gas & Steam Turbine*. ICETCEE 2012, Dubai, March 2012, pp. 13-18.
