# Options Greeks Package – Version Updates  

This document outlines the key **updates and improvements** from **Version 1 to Version 2** of the **Options Greeks Package**.

---

## Key Updates in Version 2  

### 1. Bulk Greek Computation Functions  
**New functions introduced** to compute multiple Greeks in a single function call:  
- **`first_order()`** – Computes **Delta, Gamma, Vega, Theta, and Rho** at once.  
- **`second_order()`** – Computes **Vanna, Volga, Charm, Veta, and Gamma** together.  
- **`third_order()`** – Computes **Color, Speed, Ultima, and Zomma** in bulk.  
- **`greeks()`** – Computes all Greeks (first, second, and third order) in a single function.  

**Benefits:**  
- **Improves efficiency** by reducing redundant calculations.  
- **Ensures consistency** across all calculations.  

#### Example Usage  
```python
filtered_options[['Delta', 'Gamma', 'Vega', 'Theta', 'Rho']] = \
    filtered_options.apply(lambda row: first_order(row, ticker='AAPL'), axis=1)

filtered_options[['Vanna', 'Volga', 'Veta', 'Charm', 'Gamma']] = \
    filtered_options.apply(lambda row: second_order(row, ticker='AAPL'), axis=1)

filtered_options[['Color', 'Speed', 'Ultima', 'Zomma']] = \
    filtered_options.apply(lambda row: third_order(row, ticker='AAPL'), axis=1)

filtered_options = filtered_options.apply(lambda row: greeks(row, ticker='AAPL'), axis=1)
```
### 2. `option_type` Notation Standardized  
- Updated **all functions** to use **'c'/'p'** instead of **'call'/'put'** for consistency with `py_vollib`.  
- Prevents function errors when working with `py_vollib` Greek calculations.  

#### Before (V1):  
```python
vega('call', S, K, T, r, sigma)
```
### After (V1)
```python
vega('c', S, K, T, r, sigma)
```

### 3. Enhanced `download_options()` Function  
**New Optional Parameter:** `price`  

- Adds a new column **`Stock Price`**, storing the underlying price for each row.  
- Helps users visualize **ITM vs. OTM options** alongside strike prices.  
- **Rounded `impliedVolatility` to 4 decimal places** for cleaner output.  

#### Example Usage  
```python
filtered_options = download_options('AAPL', opt_type='c', max_days=60, price=True)
```

### 4. Debugging & Formula Adjustments  
- Fixed NaN values in Theta.  
- Adjusted Volga, Veta, and Charm for better accuracy.  
- Refined calculations for third-order Greeks.  

---

## Summary of Changes  

| Feature | V1 (Previous) | V2 (Updated) |
|----------|-------------|-------------|
| **Greek Computation** | Greeks calculated individually | **Bulk computation via `first_order()`, `second_order()`, `third_order()`, and `greeks()`** |
| **Option Type Notation** | Used `'call'/'put'` | Uses `'c'/'p'` for `py_vollib` compatibility |
| **`download_options()`** | Did not store stock price | **Added `price` option** |
| **Data Formatting** | `impliedVolatility` was unrounded | **Rounded `impliedVolatility` to 4 decimals** |
| **Greek Accuracy** | Some formulas had inconsistencies | **Refined Volga, Veta, Charm, and third-order Greeks** |




