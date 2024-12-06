# Stochastic Modelling final project

# Part 1. Option Pricing Models

## Overview
This section implements various option pricing models to calculate the values of European call and put options, including Vanilla, Digital Cash-or-Nothing (DCN), and Digital Asset-or-Nothing (DAN) options. The models are:

1. **Black-Scholes Model**: A lognormal model for pricing options.
2. **Bachelier Model**: A normal model for pricing options.
3. **Black-76 Model**: An interest rate option pricing model.
4. **Displaced-Diffusion Model**: Adjusts the Black-Scholes model for displaced underlying prices.

---

## Key Components

### 1. Black-Scholes Model
- **Lognormal model** for calculating option prices:
  - Vanilla options: Standard European call and put options.
  - Digital Cash-or-Nothing (DCN): Pays a fixed amount if the option expires in the money.
  - Digital Asset-or-Nothing (DAN): Pays the underlying asset if the option expires in the money.
- **Inputs**:
  - Spot price (`S`), Strike price (`K`), Volatility (`sigma`), Time to maturity (`T`), Risk-free rate (`r`), and Cash payout (`cash`).
- **Outputs**:
  - A DataFrame with calculated option prices for all types.

### 2. Bachelier Model
- **Normal model** for pricing options under linear dynamics:
  - Supports Vanilla, DCN, and DAN options.
- **Key Feature**: Useful for low-volatility markets and options with small relative strike differences.

### 3. Black-76 Model
- **Interest rate model** for forward contracts:
  - Uses the Black-Scholes formula adapted for forwards.
- **Application**: Popular for pricing interest rate swaptions.

### 4. Displaced-Diffusion Model
- **Adjustment of Black-Scholes**:
  - Incorporates a displacement parameter (`beta`) to shift the distribution of underlying prices.
  - Widely used in commodity markets and scenarios with skewed price distributions.

---

## Combined Option Pricing Function
- **Unified Interface**:
  - Function: `Option_value(S, K, sigma, T, r, func, cash, beta)`
  - Supports all models with adjustable parameters for displacement (`beta`) and cash payouts.
- **Flexibility**:
  - Allows users to switch between models (`Black_Scholes`, `Bachelier`, `Black76`, `Displaced_diffusion`) seamlessly.
- **Output**:
  - A DataFrame showing Call and Put prices for Vanilla, DCN, and DAN options.

---


# Part 2. Volatility Smile and SABR Model Calibration

## Overview
This section focuses on analyzing and calibrating volatility smile data using various models, including Black-Scholes, Displaced-Diffusion, and SABR models. The analysis involves calculating implied volatilities, fitting models, and visualizing the results to understand the market behavior for SPX and SPY options.

---

## Key Components

### 1. Market Volatility Smile
- **Objective**: Estimate implied volatilities from market data using Black-Scholes and Displaced-Diffusion pricing models.
- **Steps**:
  1. Extract relevant data from SPX and SPY option datasets.
  2. Use Brent's method to calculate implied volatilities for each option.
  3. Plot volatility smiles to visualize implied volatilities across strike prices for different maturities.
- **Models Used**:
  - **Black-Scholes**: Standard lognormal pricing.
  - **Displaced-Diffusion**: Adjusts the Black-Scholes model for skewed distributions.

---

### 2. ATM Volatility
- **Definition**: At-the-money (ATM) volatility is the implied volatility for strike prices closest to the current spot price.
- **Method**:
  - Identify the closest strike price to the spot price for each maturity.
  - Calculate implied volatility using the Black-Scholes model.
- **Result**: ATM volatility values for SPX and SPY options across different maturities.

---

### 3. Displaced-Diffusion Model Calibration
- **Objective**: Fit the Displaced-Diffusion model to market volatility data.
- **Key Parameters**:
  - **Implied Volatility (`σ`)**: Derived from the ATM volatility.
  - **Displacement Parameter (`β`)**: Calibrated using least-squares optimization.
- **Process**:
  1. Calibrate `β` for each maturity to minimize the error between market and model-implied volatilities.
  2. Plot market-implied volatilities against model-predicted volatilities for validation.
- **Results**:
  - Calibrated values of `β` and implied volatilities (`σ`) for SPX and SPY options.

---

### 4. SABR Model Calibration
- **Objective**: Calibrate the SABR model parameters to fit the market-implied volatilities.
- **Key Parameters**:
  - **Alpha (`α`)**: Determines the overall level of volatility.
  - **Beta (`β`)**: Fixed at 0.7, controls skewness.
  - **Rho (`ρ`)**: Measures correlation between stock returns and volatility.
  - **Nu (`ν`)**: Controls the curvature of the smile.
- **Process**:
  1. Minimize the sum of squared errors between market and SABR model-implied volatilities.
  2. Visualize the model fit for each maturity.
- **Results**:
  - Calibrated SABR parameters for SPX and SPY options.
  - Plots showing the fit of the SABR model to market data.

---

## Visualization: key Takeaways

### Volatility Smile
- **Plots**: Visualize implied volatilities for SPX and SPY options as a function of strike prices.
- **Insights**:
  - Implied volatility increases for deep in-the-money or out-of-the-money options, forming a smile shape.
  - Higher curvature for shorter maturities.

### Displaced-Diffusion Model
- **Effect of `β`**:
  - **Smaller `β`**: Steeper slopes in the volatility smile.
  - **Larger `β`**: Behavior closer to the lognormal Black-Scholes model.

### SABR Model
- **Effect of `ρ`**:
  - **Negative `ρ`**: Increases out-of-the-money put option prices and decreases out-of-the-money call option prices.
  - **Positive `ρ`**: Opposite effect.
- **Effect of `ν`**:
  - **Higher `ν`**: Increased curvature of the implied volatility graph.

---

