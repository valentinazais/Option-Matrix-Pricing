## For a backtest, I wanted to study the marginal profitability of lowering the moneyness of a put (to pay less for it). I derived the code for calls in order to test new strategies. 

# NDX 30D Options Pricing Pipeline

Batch-computes Black-Scholes prices for NDX tickers: ATM calls (95% moneyness), OTM puts (87.5%). From daily CSVs (spot, IV, dividends, 1M rfr). 2020-2025 data.

## Overview
- **Inputs**: 4 CSVs (`spot.csv`, `iv.csv`, `dividend.csv`, `rfr.csv`); ; sep, , decimals, mixed date formats (%m/%d/%Y or %d/%m/%Y).
- **Outputs**: `Output/NDX_ATM_Call30D_Prices_2020.csv`, `Output/NDX_OTM_Put30D_Prices_875Moneyness_2020_2025.csv`.
- **Params**: T=30/365 (~1M), r/q continuous (ln(1+x/100)), IV/100 decimal.
- **Tickers**: NDX spot columns; rfr='USGG1M_Index'; div NaN→0.

## Files
- `atm_calls.py`: 95% K=S*0.95 calls.
- `otm_puts.py`: 87.5% K=S*0.875 puts.
- Shared: Data cleaning (dates/NaN/comma), BS funcs, date filter (2020+).

## Run
```bash
pip install pandas numpy scipy
# Edit paths: spot_path etc.
python atm_calls.py
python otm_puts.py

Here are all the inputs you need for both codes: 

Spot price ;
Implied Volatility ;
Dividend yield ;
Risk-free rate ;
Time to maturity (T) ;
Moneyness (M)

NB: customize the access paths

Mechanics

Load/parse CSVs → float → align dates.
Common dates intersect.
Loop: S=spot, σ=IV, q=div, r=rfr → K=moneyness*S → BS price → DF.
NaN skip (S/σ/r).
Save ; sep, . decimal.



