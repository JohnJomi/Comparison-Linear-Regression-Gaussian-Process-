# Temperature vs Energy Consumption — Regression Analysis

This repository contains an MSE lab exercise comparing **Linear Regression** and **Gaussian Process Regression (GPR)** for modeling the relationship between ambient temperature and energy consumption.

## Overview

The goal of this lab is to:
1. Explore and visualize the relationship between temperature and energy consumption.
2. Fit an Ordinary Least Squares (OLS) linear regression model.
3. Fit a Gaussian Process regression model with multiple kernels.
4. Compare both models on predictive accuracy (MAE, RMSE) and their ability to quantify uncertainty.
5. Critically analyze the results and recommend a model for real-time deployment.

## Dataset

A small synthetic dataset of 10 observations:

| Temperature (°C) | 10 | 12 | 15 | 18 | 20 | 23 | 25 | 28 | 30 | 32 |
|---|---|---|---|---|---|---|---|---|---|---|
| Energy Consumption | 82 | 79 | 74 | 71 | 68 | 64 | 61 | 57 | 54 | 62 |

Note: consumption decreases steadily from 10–30°C but rises again at 32°C, indicating a **non-linear** relationship (likely a cooling-load effect at higher temperatures).

## Methods

### 1. Linear Regression
- Simple OLS regression fit using `sklearn.linear_model.LinearRegression`.
- Regression equation derived and used to predict energy consumption at 17°C, 22°C, and 29°C.
- Evaluated using MAE and RMSE on the training set.

### 2. Gaussian Process Regression
- Implemented using `sklearn.gaussian_process.GaussianProcessRegressor`.
- Two kernels tested and compared:
  - **RBF (Radial Basis Function)** — assumes smooth, infinitely differentiable functions.
  - **Matérn (ν = 1.5)** — allows a rougher, more locally flexible fit.
- Each kernel combined with a `WhiteKernel` to account for observation noise.
- Predictions generated for the same test temperatures (17°C, 22°C, 29°C), including predictive mean and 95% confidence intervals.
- Evaluated using MAE and RMSE on the training set.

## Results Summary

| Metric | Linear Regression | GP (RBF) | GP (Matérn) |
|---|---|---|---|
| MAE | See notebook output | See notebook output | See notebook output |
| RMSE | See notebook output | See notebook output | See notebook output |
| Uncertainty estimate | Not provided | Yes (95% CI) | Yes (95% CI) |
| Handles non-linearity | No | Yes | Yes |

*(Exact metric values are generated when running the notebook — see `notebook.ipynb` / Colab output.)*

## Key Findings

- **Predictive performance**: GP regression outperforms linear regression on both MAE and RMSE, since the true relationship is non-linear and linear regression cannot capture the upward bend near 30–32°C.
- **Uncertainty quantification**: Linear regression produces only a point estimate. GP regression is a probabilistic model — it places a prior over possible functions and updates it with observed data, naturally producing a predictive mean and variance (confidence interval) at every point.
- **Effect of kernel choice**: The RBF kernel produces smoother, more conservative predictions, while the Matérn kernel adapts more responsively to local changes such as the rise at 32°C.
- **Deployment recommendation**: For real-time energy monitoring, **GP regression with the Matérn kernel** is recommended, since it combines better predictive accuracy with built-in uncertainty estimates — useful for flagging low-confidence predictions or detecting when live data falls outside the training range.

## Repository Contents

```
├── README.md              # This file
├── notebook.ipynb          # Google Colab / Jupyter notebook with full code and plots
```

## Requirements

```
numpy
matplotlib
scikit-learn
```

## How to Run

1. Open `notebook.ipynb` in Google Colab or Jupyter.
2. Run all cells sequentially — data loading, linear regression, GP regression (both kernels), and comparison plots will execute in order.
3. Review printed MAE/RMSE values and generated plots for each model.

## Author

MSE Lab Assignment — Regression & Gaussian Process Modeling
