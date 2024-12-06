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

# Part 3. Static Replication and Option Pricing Models

## Overview
This section explores static replication using multiple option pricing models, including Black-Scholes, Bachelier, and SABR models. The analysis focuses on SPX and SPY options, leveraging SABR-calibrated parameters and volatility smiles from previous work. The results highlight integrated variance and the accuracy of model-based pricing.

---

## Key Components

### 1. Data Input and Preparation
- **Inputs**:
  - SPX and SPY option data with bid-ask prices and strike prices.
  - SABR model parameters (α, β, ρ, ν) for SPX and SPY at T = 45 days.
- **Objective**: Process market data to extract discount factors, forward prices, and implied volatilities.

---

### 2. Option Pricing Models
- **Models Used**:
  - **Black-Scholes**: Lognormal pricing model for European options.
  - **Bachelier**: Normal pricing model for options.
  - **SABR**: Stochastic volatility model calibrated to market-implied volatilities.
- **Implementation**:
  - Calculate ATM volatilities using Black-Scholes and Bachelier models.
  - Integrate model-specific pricing functions to estimate variance and validate results.

---

### 3. Integrated Variance Estimation
- **Process**:
  - Use market data to calculate implied volatilities for both SPX and SPY options.
  - Estimate integrated variance based on the models and compare results for consistency.
- **Key Functions**:
  - Static replication integrates the option pricing models over strike prices to estimate total variance.

---

### 4. Static Replication Pricing
- **Objective**: Price a custom contract defined by a payoff function \( h(K) = K^{1/3} + 1.5 \ln(K) + 10 \).
- **Methodology**:
  1. Define a second-order derivative of the payoff function \( h''(K) \).
  2. Integrate the product of \( h''(K) \) and model-based option prices across all strike prices.
  3. Adjust for \( h(S_0) \) to derive final prices.

---

## Key Insights

### ATM Volatility
- **Black-Scholes**: Provides consistent volatility estimates for SPX and SPY options.
- **Bachelier**: Exhibits significantly higher volatility levels due to the normal distribution assumption.

### Static Replication
- The static replication prices are slightly lower than the payoff function value \( h(S_0) \), as expected from theoretical constraints.
- All models produce consistent pricing, with minimal variance across methods.

### SABR Model
- SABR-calibrated parameters (α, β, ρ, ν) provide flexibility to capture volatility smiles and term structure.
- The model effectively integrates stochastic volatility effects into static replication.

---

## Conclusion
This project validates static replication using Black-Scholes, Bachelier, and SABR models. The results confirm that these models are consistent with theoretical expectations and provide robust pricing for SPX and SPY options. The integration of SABR parameters enhances the analysis by incorporating market volatility dynamics.
