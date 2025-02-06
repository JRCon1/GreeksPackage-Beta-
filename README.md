# Options Greeks Package (Beta)

This repository provides a collection of functions to calculate first, second, and third-order Greeks for options pricing. The package is currently in **beta**, and all functions must be manually copied into your environment. 

## Features

- **Options Data Extraction**: Uses `download_options` to retrieve option chains from Yahoo Finance.
- **First-Order Greeks**: Refer to `py_vollib` for standard Delta, Gamma, Vega, Theta, and Rho calculations.
- **Second-Order Greeks**: Computes Vanna, Volga, Charm, Veta.
- **Third-Order Greeks**: Computes Color, Speed, Ultima, Zomma.
- **Black-Scholes Framework**: The calculations are based on the Black-Scholes model, assuming constant volatility and risk-free rates.

## Beta Version Notice

- The package is still in development. **Functions must be manually copied** into your working script.
- The calculations rely on the **data extracted using `download_options`**. Ensure you fetch option chains through this function before performing any analysis.

## Installation

```bash
pip install py_vollib yfinance pandas numpy scipy
```

## Data Extraction
The download_options function retrieves and filters option chains from Yahoo Finance based on:
 - Option Type: Calls ('c') or Puts ('p').
- Days to Expiry: Filters contracts with a maximum expiration period.
- Moneyness Bounds: Filters options within a given range relative to the underlying price.

```bash
filtered_options = download_options('AAPL', opt_type='c', max_days=60, lower_moneyness=0.95, upper_moneyness=1.05)
```

## Greeks Calculations
First-Order Greeks (Standard)
For Delta, Gamma, Vega, Theta, and Rho, refer to py_vollib:

```bash
from py_vollib.black_scholes.greeks.analytical import delta, gamma, vega, theta, rho
```

## Second-Order Greeks
 # Greek	Description
- Vanna	Measures the sensitivity of Vega to underlying price changes.
- Volga	Measures how Vega changes with respect to volatility.
- Charm	Captures the rate of change of Delta with time.
- Veta	Sensitivity of Vega to time decay.

Example usage:
```bash
vanna_value = vanna(row, ticker='AAPL')
```

## Third-Order Greeks
 # Greek	Description
- Color	Measures how Gamma changes with time.
- Speed	Rate of change of Gamma with the underlying price.
- Ultima	Sensitivity of Vanna to volatility.
- Zomma	Sensitivity of Gamma to volatility.

Example usage:
```bash
zomma_value = zomma(row, ticker='AAPL')
```
Example usage:

# Download option chain
```bash
filtered_options = download_options('AAPL', opt_type='c', max_days=60)

# Compute second-order Greek (Vanna) for each option
filtered_options['Vanna'] = filtered_options.apply(
    lambda row: vanna(row, ticker='AAPL'), axis=1
)

# Compute third-order Greek (Zomma)
filtered_options['Zomma'] = filtered_options.apply(
    lambda row: zomma(row, ticker='AAPL'), axis=1
)

# Display results
print(filtered_options[['strike', 'Vanna', 'Zomma']])
```

References
Black-Scholes Model
Yahoo Finance API
py_vollib Documentation
Disclaimer
This package is in beta and should not be used for live trading without verification. The accuracy of results depends on market data quality and model assumptions.

Future Enhancements
Convert functions into a fully installable package.
Expand support for alternative pricing models (e.g., Heston, SABR).
Improve handling of missing or imprecise data in Yahoo Finance.
