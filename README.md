# Stock Exchange Analyser

Native Windows GUI application for analysing historical stock data. Calculates SMA, RSI, generates BUY/SELL/HOLD signals, and visualises price trends with GDI charts.

## Features

| # | Button | What it does |
|---|---|---|
| 1 | **Load Stock CSV** | Load historical data from a CSV file |
| 2 | **30-Day Leaderboard** | Rank all loaded stocks by performance |
| 3 | **View + Graph** | Show technical indicators + open chart window |
| 4 | **Export Report** | Save text report + GDI chart as BMP |
| 5 | **Portfolio Table** | Side-by-side comparison of all stocks |
| 6 | **Exit** | Free memory and close |

## How to Run

```
stock_analyser.exe
```

Or double-click the .exe in File Explorer.

## How to Build

```bash
make
```

Or manually:

```bash
gcc -Wall -Wextra -O2 -Iinclude src/main.c src/parser.c src/analytics.c -o stock_analyser.exe -lgdi32 -mwindows
```

## Dependencies

- Windows (Win32 API for GUI + GDI for chart rendering)
- GCC (MinGW) — for compiling
- **No external libraries** — all GUI and graphics are pure Win32

## How to Use

1. Type CSV file path (e.g. `AAPL.csv`) and ticker (e.g. `AAPL`)
2. Click **1. Load Stock CSV** — stock appears in the dropdown
3. Load more stocks (NVDA, MSFT, GOOG) — dropdown updates automatically
4. Select a stock from the dropdown
5. Click **3. View + Graph** — see SMA, RSI, signal + a GDI chart opens
6. Click **4. Export Report** — saves `exports/<TICKER>_report.txt` + `<TICKER>_chart.bmp`
7. Click **5. Portfolio Table** — see all stocks compared in one table

## Sample Data

4 CSV files included with ~250 days of price data:

| File | Ticker |
|---|---|
| AAPL.csv | Apple |
| NVDA.csv | NVIDIA |
| MSFT.csv | Microsoft |
| GOOG.csv | Google |

Fetch more with:

```bash
pip install -r requirements.txt
python fetch_stock.py
```

## Technical Indicators

### Simple Moving Average (SMA)

Average of closing prices over N days. Used for 5-day (short-term) and 20-day (medium-term) trends.

### Relative Strength Index (RSI)

Momentum oscillator measuring speed of price changes over 14 days.
- RSI > 70 → overbought
- RSI < 30 → oversold

### Enhanced Signal Logic

Combines SMA crossover direction with RSI to confirm trades:

| SMA Direction | RSI Check | Signal |
|---|---|---|
| Up (SMA5 > SMA20) | Below 70 | BUY |
| Down (SMA5 < SMA20) | Above 30 | SELL |
| Conflicting | — | HOLD |

## Exports

Clicking **4. Export Report** generates two files per stock:

| File | Contents |
|---|---|
| `exports/AAPL_report.txt` | Latest prices, SMA, RSI, signal, summary statistics |
| `exports/AAPL_chart.bmp` | GDI line chart with close price, SMA5, SMA20 |

## Project Structure

```
├── include/
│   ├── common.h         # TradeRecord, Stock, Market structs
│   ├── parser.h         # CSV loading + memory cleanup
│   └── analytics.h      # Math, signals, chart, export
├── src/
│   ├── main.c           # Win32 GUI window + button handlers
│   ├── parser.c         # CSV parsing + dynamic memory
│   └── analytics.c      # SMA, RSI, ranking, GDI chart, export
├── exports/             # Generated reports + charts
├── reference/           # Viva prep docs (gitignored)
├── *.csv                # Sample stock data
├── fetch_stock.py       # Download any stock from Yahoo Finance
├── Makefile
└── README.md
```
