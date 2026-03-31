## Economic Analysis Notebooks

This repository supports the economic analysis for my final year project, where we are evaluating the feasibility of building a chemical plant. In particular, these notebooks are used to estimate more realistic operating-cost assumptions by analysing market price data, quantifying price uncertainty, and deriving location-specific adjustment factors for utilities.

### `calculating_variance_in_price.ipynb`
This notebook analyses historical crude oil price data to estimate price variability over time. It loads crude oil prices from `data/crude_oil_prices.csv`, converts the series into monthly averages, and calculates log returns between periods. From these returns, it computes key statistical measures such as variance, standard deviation, and standard error.

The notebook also performs a 12-month rolling variance analysis to study how volatility changes over time, and summarises the average rolling variance over the full dataset as well as over the most recent 10-year, 5-year, and 2-year periods. This helps provide a quantitative basis for incorporating feedstock price uncertainty into the broader economic analysis.

### `utilities_scaling_factor.ipynb`
This notebook derives a utility cost scaling factor to adjust US-based utility price assumptions to a Singapore context. It compares electricity, natural gas, and water prices between the US and Singapore using market data collected from multiple sources, with all values rebased into USD for consistency.

For each utility, the notebook fits simple linear regression models both with and without an intercept, then compares their suitability. The results suggest that the no-intercept model performs better, supporting the idea that the main difference between US and Singapore utility prices can be represented as a scaling factor. The notebook then uses the fitted coefficients to estimate correction factors for each utility and averages them to obtain an overall adjustment factor of about `6.256`. In practical terms, this means Singapore utility prices were estimated to be roughly 6.3 times higher than equivalent US prices, which is then used to scale utility costs in the plant economic analysis.

### Overall Goal
The overall goal of this repository is to support the economic evaluation of a proposed chemical plant by making cost assumptions more data-driven and locally relevant. Rather than relying on generic textbook values, the work here uses historical market data to account for both uncertainty in commodity prices and the difference between US reference prices and Singapore operating conditions.