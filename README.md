## Economic Analysis Notebooks

This repository supports the economic analysis for a final year project evaluating the feasibility of building a chemical plant. The recent work in this repo focuses on moving from a static deterministic cash flow model to a stochastic discounted cash flow model driven by market and operating uncertainty.

The overall workflow is:

1. define the main stochastic drivers and estimate their uncertainty from data
2. build correlations / covariance between the key market price drivers
3. simulate many future operating scenarios with Monte Carlo
4. convert those simulated scenarios into discounted cash flow outcomes
5. visualize annual and cumulative cash flow profiles

AI-assisted research was used heavily to speed up data gathering and market review. Supporting AI-generated reports are stored under `Papers/`.

## Main Files

### `defining_stochastic_drivers.ipynb`

This notebook defines the uncertainty structure used in the plant economics model.

Key tasks covered in the notebook:

- explains the shift from static NPV to dynamic NPV / Monte Carlo valuation
- identifies the main stochastic drivers, including:
  - iBAP selling price
  - hydrofluoric acid price
  - throughput assumptions
  - inflation assumptions
- uses crude oil log-price variance as a proxy basis for iBAP price uncertainty
- assembles hydrofluoric acid price data and compares alternative proxies for iBAP
- tests benzene and aromatics indices for multicollinearity
- selects the aromatics index as the preferred iBAP proxy because of its high correlation with benzene and its better downstream relevance
- computes the covariance matrix of log returns for hydrofluoric acid price and the aromatics proxy

This notebook is the statistical foundation for the Monte Carlo model.

### `monte_carlo_multidimGBM.ipynb`

This notebook implements the stochastic discounted cash flow simulation.

It currently contains two related modeling ideas:

- an earlier multidimensional GBM template for jointly simulating multiple drivers
- a later joint simulation section that is closer to the project assumptions and is the more relevant workflow

The notebook now:

- sets up four core drivers:
  - `price_of_ibap`
  - `ibap_throughput`
  - `price_of_HF`
  - `HF_throughput`
- validates covariance / correlation inputs
- separates initial price levels from drift rates to avoid artificial price explosion
- simulates:
  - `price_of_ibap` and `price_of_HF` as path-dependent GBM processes
  - `ibap_throughput` and `HF_throughput` as path-dependent arithmetic level processes
- allows correlated latent shocks across the jointly simulated drivers
- computes period-by-period discounted cash flow using:
  - simulated revenue
  - inflated raw material cost
  - discounting over time
- aggregates discounted cash flow into total present value per simulation
- summarizes the simulation with descriptive statistics and histograms
- plots:
  - sample simulated paths for each driver
  - average simulated paths across all simulations

In practical terms, this notebook is the main project file for scenario generation and dynamic project valuation.

### `Creating CFD.Rmd`

This R Markdown file is used to create discounted cash flow style plots from model output stored in Excel.

The file currently:

- loads discounted cash flow data from `data/Creating CFD plots.xlsx`
- produces line / lollipop style visualizations of:
  - annual discounted cash flow
  - cumulative discounted cash flow
- serves as a lightweight visualization script for presenting CFD-style plant economics outputs

This file is mainly for communication and presentation of the cash flow profile rather than for building the stochastic model itself.

## Suggested Reading Order

If you are new to the repo, the most useful order is:

1. `defining_stochastic_drivers.ipynb`
2. `monte_carlo_multidimGBM.ipynb`
3. `Creating CFD.Rmd`

That order follows the actual logic of the project:

- estimate uncertainty
- simulate uncertainty
- visualize the financial consequences

## Current Modeling Direction

The current version of the project is best understood as a stochastic plant economics workflow rather than a simple spreadsheet valuation. Instead of assuming one fixed set of prices and production values, the model generates many possible operating futures and estimates a distribution of total project value.

This makes it possible to study:

- expected present value
- upside / downside cases
- sensitivity to price and throughput uncertainty
- the effect of correlated market movements on profitability
