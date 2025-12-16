## For a backtest, I wanted to study the marginal profitability of lowering the moneyness of a put (to pay less for it). I derived the code for calls in order to test new strategies. 

# NDX 30D Options Backtest Pipeline

Modular Black-Scholes pricer for calls/puts at variable moneyness (M=100% ATM). Computes daily prices from spot/IV/div/rfr CSVs (2020+). For backtesting marginal profitability of OTM puts (lower M → cheaper premium).

## Purpose
- Study put strategies: Lower moneyness (e.g., M=85-95%) reduces premium cost → test PNL/edge.
- Calls derived analogously for symmetric testing (e.g., ITM/OTM calls).

## Code Structure
- **Inputs**:
  - CSVs: `spot.csv` (tickers), `iv.csv`, `dividend.csv`, `rfr.csv` ('USGG1M_Index').
  - Params: `T=30/365.0` (maturity), `M=90.0` (moneyness % → K=S*M/100).
- **Cleaning**: Parse mixed dates (; sep, ,→.), float cast, NaN filter, intersect dates.
- **BS Loop**: Per date/ticker → continuous r/q/σ → `black_scholes_call/put(S, K=S*M/100, T, r, q, σ)`.
- **Output**: DF (dates x tickers) → CSV (`Output/NDX_[Call|Put]30D_Prices_M{M:.1f}Moneyness_*.csv`).
- **Extensible**: Swap call/put, tweak M/T, add Greeks/P&L sim.

## Files
- `atm_calls.py`: Template for calls (set `M`, e.g., 95.0).
- `otm_puts.py`: Template for puts (set `M`, e.g., 87.5).
- Edit: Paths, `start_date`, `M` (input var), output filename.

## Run
```bash
pip install pandas numpy scipy
# Set paths, M=92.5, etc.
python calls.py  # Or copy/rename
