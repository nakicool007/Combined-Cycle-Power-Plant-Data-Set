# DSCI 552 - Homework 2

Project setup for the Combined Cycle Power Plant homework, based on the supplied `Homework2 (1).pdf` (Instructor: Mohammad Reza Rajati). Complete the analysis and written responses yourself in the existing notebook. This README contains assignment requirements and environment instructions, not solutions.

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
|   `-- Kumbria_Nakul_HW2.ipynb
`-- requirements.txt
```

The folder layout follows the reference image, with names appropriate to this homework. The vertebral-column data files and KNN classification notebook shown in that image are not part of this assignment.

## Environment setup

From the project root in PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m jupyterlab
```

Open `notebook/Kumbria_Nakul_HW2.ipynb` using the virtual environment's Python kernel.

Dependencies support the assignment as follows:

- `jupyterlab` and `ipykernel`: notebook editing and execution.
- `numpy` and `pandas`: numerical operations and data handling.
- `openpyxl`: reading the supplied Excel workbook.
- `odfpy`: optional reading of the supplied ODS alternative.
- `matplotlib` and `seaborn`: plots.
- `scipy` and `statsmodels`: statistical tools, regression inference, and p-values.
- `scikit-learn`: regression, feature transformations, splitting, and error metrics.

The homework does not prescribe package versions. `requirements.txt` lists direct dependencies without pinning versions.

## Dataset requirements

Use the Combined Cycle Power Plant dataset supplied in `data/combined_cycle_power_plant/`. The XLSX and ODS files are alternative formats of the same dataset; they are not separate sets of observations.

**Use Sheet 1**, as required by the assignment's footnote. The five sheets are shuffled versions of the same dataset. Do not combine them as separate observations. The XLSX workbook is sufficient for the homework; the original ODS copy is retained.

The assignment describes temperature, ambient pressure, relative humidity, and exhaust vacuum as predictors of net hourly electrical energy output. Verify the workbook's column labels as part of your exploration.

## Assignment checklist

These items summarize the supplied PDF. Refer to it for the complete wording and footnotes. Check items off only after completing your own work.

- [ ] 1(a): Obtain the Combined Cycle Power Plant data and use Sheet 1.
- [ ] 1(b)(i): Report the row and column counts and explain what they represent.
- [ ] 1(b)(ii): Make pairwise scatterplots of all variables, including the response, and discuss your findings.
- [ ] 1(b)(iii): Tabulate each variable's mean, median, range, first and third quartiles, and interquartile range.
- [ ] 1(c): Fit a separate simple linear regression for each predictor; discuss significance, supporting plots, and potential outliers.
- [ ] 1(d): Fit multiple linear regression with all predictors; identify which coefficient null hypotheses can be rejected.
- [ ] 1(e): Compare simple and multiple regression coefficients in a plot, with simple coefficients on the x-axis and multiple coefficients on the y-axis.
- [ ] 1(f): Fit a cubic polynomial regression for each predictor and assess evidence of nonlinearity.
- [ ] 1(g): Fit a full linear model with all pairwise predictor interactions and assess their significance.
- [ ] 1(h): Randomly split the data into 70% training and 30% testing. Compare the all-predictor regression with a model including all pairwise interactions and quadratic terms; remove insignificant variables using p-values with care about interactions. Report training and test MSE for both models.
- [ ] 1(i): Run KNN regression with raw and normalized features, examine integer k values from 1 through 100, select the best fit, and plot training and test errors against 1/k.
- [ ] 1(j): Compare KNN regression with the linear regression model having the smallest test error and discuss the results.
- [ ] 2: Complete ISLR exercise 2.4.1.
- [ ] 3: Complete ISLR exercise 2.4.7.

The provided PDF names the ISLR exercises but does not reproduce their questions or specify the book edition. Use the edition assigned by your course. It does not specify a submission filename or export format; follow any separate course submission instructions.

## Dataset provenance

The original dataset notes describe measurements collected from a plant operating at full load between 2006 and 2011. The plant combines gas turbines, steam turbines, and heat recovery steam generators. The supplied shuffles support 5x2 cross-validation in the dataset's original studies; this homework instead explicitly directs you to Sheet 1 and specifies its own split in 1(h).

References retained from the original dataset notes:

- P?nar T?fekci. *Prediction of full load electrical power output of a base load operated combined cycle power plant using machine learning methods*. International Journal of Electrical Power & Energy Systems, 60, September 2014, pp. 126-140. DOI: 10.1016/j.ijepes.2014.02.027.
- Heysem Kaya, P?nar T?fekci, and Sad?k Fikret G?rgen. *Local and Global Learning Methods for Predicting Power of a Combined Gas & Steam Turbine*. ICETCEE 2012, Dubai, March 2012, pp. 13-18.
