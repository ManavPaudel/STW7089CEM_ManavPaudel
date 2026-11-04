# Cloud Server Power Consumption Modelling

**Statistical modelling with polynomial regression, model selection, predictive intervals, and Approximate Bayesian Computation (ABC)**

A reproducible Python/Jupyter analysis of simulated cloud-server power consumption for **STW7089CEM — Introduction to Statistical Methods for Data Science**.

**Author:** Manav Paudel  
**Student ID:** 260543  
**Institution:** Softwarica College of IT & E-Commerce, in collaboration with Coventry University

## Project overview

This project examines how four simulated telemetry inputs (`x1`, `x2`, `x3`, and `x4`) relate to a power-consumption response (`y`). Five coursework-specified polynomial regression candidates are evaluated to select a model that balances goodness of fit with complexity. The selected model is tested on held-out observations, and rejection ABC is used to investigate uncertainty in two fitted parameters.

The notebook is organized into **small, task-focused code cells with individual numerical or graphical outputs**. Detailed explanations and interpretations belong to the accompanying coursework report; the notebook focuses on the reproducible calculations and figures.

## Analysis workflow

### Task 1 — Preliminary data analysis

- Load and align `x.csv`, `y.csv`, and `time.csv` by row order.
- Check data integrity and display descriptive statistics.
- Plot each telemetry input and the response separately over time.
- Plot individual histograms with density estimates for all five signals.
- Plot each input against power consumption and display a Pearson correlation heatmap.

### Task 2 — Polynomial regression and model selection

- Construct the **five candidate equations specified in the coursework**.
- Estimate coefficients by ordinary least squares (OLS), using standardized polynomial features for numerical fitting and reporting coefficients on the original scale.
- Calculate residual sum of squares (RSS), RMSE, Gaussian residual variance, and maximized log-likelihood.
- Compare the candidates using **AIC**, **BIC**, residual histograms, and normal Q–Q plots.
- Choose a preferred model and examine an optional nested-model F-test.
- Refit the selected model on a **70% training / 30% testing** split, then evaluate predictions and plot **95% individual prediction intervals**.
- Include supplementary comparisons with an untransformed linear baseline and a chronological train–test split.

### Task 3 — Rejection Approximate Bayesian Computation

- Identify the two largest absolute **original-scale** least-squares parameters of the selected model, **including its intercept**.
- Fix the other parameters at their full-data OLS estimates.
- Define uniform, data-informed priors around the selected estimates.
- Simulate Gaussian least-squares summary statistics and apply a Mahalanobis-distance rejection threshold.
- Display accepted-parameter summaries, two marginal posteriors, and their joint posterior distribution.

## Main results

| Metric | Result |
|---|---:|
| Selected candidate | **Model 2** |
| AIC | 60,609.90 |
| BIC | 60,645.95 |
| Held-out test RMSE | 5.0831 |
| Held-out test MAE | 4.0531 |
| Held-out test R² | 0.999966 |
| Coverage of nominal 95% prediction intervals | 94.63% |
| ABC proposals | 600,000 |
| Accepted ABC proposals | 1,291 |

The fitted full-data equation is:

\[
\hat{y} = 120.022108 + 4.480472x_4 + 1.200184x_1^3 + 0.800003x_3^4
\]

Model 5 achieves a marginally lower in-sample RSS, but both AIC and BIC prefer the more parsimonious Model 2.

**Note:** These numerical results are specific to the supplied simulated dataset; they are not validated predictions for real-world cloud-server hardware.

## Repository contents

```text
.
├── README.md
├── Jupyter_Notebook.ipynb
├── x.csv                 # Coursework dataset; add locally if distribution is restricted
├── y.csv                 # Coursework dataset; add locally if distribution is restricted
└── time.csv              # Coursework dataset; add locally if distribution is restricted
```

The three CSV files are required to run the notebook. They are coursework-provided files and **should not be uploaded publicly without permission**. If they are not included in the repository, download or obtain them from the authorized coursework materials and place them alongside the notebook.

## Requirements

- Python 3
- Jupyter Notebook or JupyterLab
- NumPy
- pandas
- Matplotlib
- seaborn
- SciPy
- scikit-learn

Install the dependencies in your Python environment:

```bash
python -m pip install notebook numpy pandas matplotlib seaborn scipy scikit-learn
```

## How to run

1. Clone the repository (or download the project as a ZIP).
2. Put `x.csv`, `y.csv`, and `time.csv` in the same directory as `Jupyter_Notebook.ipynb`.
3. Open a terminal in that directory and start Jupyter:

   ```bash
   jupyter notebook
   ```

4. Open `Jupyter_Notebook.ipynb`.
5. Choose **Kernel → Restart & Run All** (the wording may vary by Jupyter version). Execute the notebook **from top to bottom**, because later cells depend on variables created earlier. For example, the Gaussian log-likelihood calculation uses `residual_data` and `rss_df` from the preceding RSS section.
6. Review the plots and numerical outputs under each task heading.

The notebook uses fixed random seeds (`42`) for the specified randomized analyses to support repeatability.

## Methodological notes

- `x1`–`x4` are the original input labels. The files do not independently establish specific physical telemetry meanings or measurement units.
- Feature names such as `x3_quad` in the code represent a **fourth-power term (`x3**4`)**, not a square; the code's explicit expressions determine the model.
- The 95% intervals in Task 2 describe **individual future observations**, whereas the 95% ABC credible intervals describe uncertainty in selected model parameters.
- The ABC implementation uses Gaussian simulated **OLS summaries** instead of generating and refitting 10,000 individual observations for every proposal. This relies on the Gaussian model assumptions and fixes the remaining coefficients and residual variance.
- The ABC priors are informed by the same dataset, so the posterior analysis is an illustrative coursework exercise rather than an independent Bayesian validation.

## Academic context

This repository provides the **executable analysis and outputs**. The accompanying written assignment contains the statistical interpretations, discussion, limitations, and references. The results should be assessed in the context of the coursework's simulated data and prescribed candidate models.
