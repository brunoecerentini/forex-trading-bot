# Detailed Usage Guide

## Table of Contents
1. [Initial Setup](#initial-setup)
2. [Running Your First Bot](#running-your-first-bot)
3. [Understanding the Trading Strategy](#understanding-the-trading-strategy)
4. [Monitoring and Logging](#monitoring-and-logging)
5. [Customization Options](#customization-options)
6. [Troubleshooting](#troubleshooting)

## Initial Setup

### Step 1: MetaTrader 5 Installation

1. Download MetaTrader 5 from your broker's website
2. Install and launch the platform
3. Create a demo account (recommended for testing) or login to your live account

### Step 2: Python Environment

Ensure you have Python 3.12 or higher installed:

```bash
python --version  # Should show 3.12 or higher
```

### Step 3: Install Dependencies

```bash
# Create virtual environment
python -m venv venv

# Activate it
# Windows:
venv\Scripts\activate
# Linux/Mac:
source venv/bin/activate

# Install requirements
pip install -r requirements.txt
```

### Step 4: Configure Credentials

Create a `.env` file from the template:

```bash
cp .env.example .env
```

Edit `.env` with your actual credentials:

```env
MT5_ACCOUNT=50012345
MT5_PASSWORD=YourSecurePassword123
MT5_SERVER=YourBroker-MT5
LOT_SIZE=0.01
```

## Running Your First Bot

### Method 1: Jupyter Notebook (Recommended for Beginners)

1. **Launch Jupyter**:
```bash
jupyter notebook
```

2. **Open a trading bot notebook**:
   - Navigate to one of the `BEST----[SYMBOL].IPYNB` files
   - Recommended starting point: `BEST----XAUUSD.IPYNB`

3. **Modify the credentials section**:

Find this section in the notebook:
```python
account = YOUR_ACCOUNT_NUMBER  # Replace with your account
password = "YOUR_PASSWORD"  # Replace with your password
server = "YOUR_BROKER_SERVER"  # Replace with your server
```

Replace with environment variables:
```python
import os
from dotenv import load_dotenv

load_dotenv()

account = int(os.getenv('MT5_ACCOUNT'))
password = os.getenv('MT5_PASSWORD')
server = os.getenv('MT5_SERVER')
```

4. **Run all cells** (Kernel → Restart & Run All)

### Method 2: Command Line Interface

```bash
python utils/codex.py --help
```

Example usage:
```bash
python utils/codex.py \
  --symbol XAUUSD \
  --account 50012345 \
  --password "YourPassword" \
  --server "YourBroker-MT5"
```

## Understanding the Trading Strategy

### Zone Calculation Process

1. **Initial Data Collection**: The bot fetches the first 10 candles of the week (configurable via `n_candles`)

2. **Central Range (CR) Calculation**:
   ```
   CR_UPPER = max(close_prices)
   CR_BOT = min(close_prices)
   ```

3. **Zone (ZN) Calculation**:
   ```
   range = CR_UPPER - CR_BOT
   ZN_UPPER = CR_UPPER + range
   ZN_BOT = CR_BOT - range
   ```

4. **Extension Levels**:
   ```
   N1_100_UPPER = ZN_UPPER + range
   N1_100_BOT = ZN_BOT - range
   N2_100_UPPER = ZN_UPPER + (range * 2)
   N2_100_BOT = ZN_BOT - (range * 2)
   ```

### Trade Entry Logic

**BUY Signal Conditions**:
```python
if open_price < ZN_UPPER and close_price > ZN_UPPER:
    # Price broke above the upper zone
    sl = CR_BOT - spread
    tp = N1_100_UPPER
    send_buy_order(symbol, lot_size, sl, tp)
```

**SELL Signal Conditions**:
```python
if open_price > ZN_BOT and close_price < ZN_BOT:
    # Price broke below the lower zone
    sl = CR_UPPER + spread
    tp = N1_100_BOT
    send_sell_order(symbol, lot_size, sl, tp)
```

### Risk Management Rules

1. **Maximum 1 position** per symbol at a time
2. **Automatic Stop-Loss** on every trade
3. **Take-Profit** targets at N1 levels
4. **End-of-day closure** at 23:59 broker time
5. **No new trades** if a position is already open

## Monitoring and Logging

### Console Output

The bot provides real-time information:

```
Conectado à conta [YOUR_ACCOUNT]
Variáveis calculadas: cr_upper=3355.58, cr_bot=3338.25, zn_upper=3372.91, zn_bot=3320.92
horario servidor mt5: 2025-04-22 20:11:33+03:00
Verificando candle de 2025-04-22 20:11:00+03:00
Open price: 3390.88, Close price: 3396.37
Condição de compra atendida: Open=3371.38, Close=3376.38, SL=3336.25, TP=3407.57
Ordem de compra enviada: SL=3336.25, TP=3407.57
Aguardando condições de mercado...
```

### Generated Files

- **CSV Files**: `x_[SYMBOL].csv` - Historical price data
- **Charts**: `candlestick_chart_[SYMBOL].png` - Visual trade analysis
- **Logs**: Console output shows all activities

### Key Metrics to Monitor

1. **Connection Status**: "Conectado à conta XXXXX"
2. **Zone Levels**: Printed at startup
3. **Trade Execution**: Success/failure messages
4. **Memory Usage**: Periodic memory monitoring
5. **Position Status**: Open/closed position information

## Customization Options

### Adjustable Parameters

In each notebook, you can modify:

```python
# Symbol to trade
symbol = "XAUUSD"  # Change to EURUSD, GBPUSD, USDJPY

# Position sizing
lot_size = 0.01  # Increase with more capital

# Timeframes
timeframe = mt5.TIMEFRAME_M15  # Main analysis timeframe
timeframe_get_candle = mt5.TIMEFRAME_M1  # Signal detection timeframe

# Zone calculation
n_candles = 10  # Number of initial candles for zone detection

# Risk management
spread = 2  # Spread buffer for stop-loss (XAUUSD)
spread_tp = 1  # Spread buffer for take-profit

# Trading hours
end_of_day_time = "23:59"
start_of_day_time = "01:00"

# Week start date
start_date = datetime(2025, 4, 21, 0, 0, tzinfo=GMT_PLUS_3)
```

### Symbol-Specific Spreads

Different assets require different spread values:

```python
# Gold (XAUUSD)
spread = 2  # Typical spread in points

# Forex Majors (EURUSD, GBPUSD)
spread = 0.001  # Typical spread in price units

# USDJPY
spread = 0.1  # Typical spread in yen
```

## Troubleshooting

### Common Issues and Solutions

#### 1. "Falha na inicialização"

**Problem**: MetaTrader 5 connection failed

**Solutions**:
- Ensure MT5 is running
- Check if MT5 is not blocked by firewall
- Verify MT5 installation is complete
- Try restarting MT5

#### 2. "Falha ao conectar à conta"

**Problem**: Login credentials incorrect

**Solutions**:
- Verify account number in `.env`
- Check password (case-sensitive)
- Confirm server name (copy exactly from MT5)
- Ensure account is active (not expired)

#### 3. "Símbolo não disponível"

**Problem**: Trading pair not offered by broker

**Solutions**:
- Check if symbol is available in MT5 Market Watch
- Right-click Market Watch → Show All
- Verify symbol name spelling
- Some brokers use different suffixes (e.g., XAUUSDm)

#### 4. "Erro ao enviar ordem"

**Problem**: Trade execution failed

**Solutions**:
- Check account balance (insufficient funds)
- Verify market is open (not weekend/holiday)
- Ensure lot size is within broker limits
- Check if account has trading permissions

#### 5. "Nenhum dado retornado"

**Problem**: Historical data unavailable

**Solutions**:
- Check internet connection
- Verify date range is valid
- Ensure MT5 has downloaded historical data
- Try a different timeframe or smaller date range

#### 6. High Memory Usage

**Problem**: Memory grows over time

**Solutions**:
- The bot includes garbage collection (`gc.collect()`)
- Restart bot periodically (e.g., daily)
- Reduce chart generation frequency
- Comment out plot functions if not needed

### Debugging Tips

**Enable verbose logging**:
```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

**Check MT5 connection**:
```python
import MetaTrader5 as mt5
mt5.initialize()
print(mt5.terminal_info())
print(mt5.version())
```

**Test with minimal code**:
```python
import MetaTrader5 as mt5

if not mt5.initialize():
    print("MT5 initialization failed")
else:
    print("MT5 OK")
    
account = YOUR_ACCOUNT
password = "YOUR_PASSWORD"
server = "YOUR_SERVER"

if mt5.login(account, password, server):
    print("Login successful")
else:
    print(f"Login failed: {mt5.last_error()}")

mt5.shutdown()
```

## Advanced Usage

### Running Multiple Bots Simultaneously

You can run multiple bots for different symbols:

**Terminal 1**:
```bash
python utils/codex.py --symbol XAUUSD
```

**Terminal 2**:
```bash
python utils/codex.py --symbol EURUSD
```

**Terminal 3**:
```bash
python utils/codex.py --symbol GBPUSD
```

**⚠️ Warning**: Ensure sufficient account balance and proper risk management!

### Automated Startup (Windows)

Create a batch file `start_bot.bat`:

```batch
@echo off
cd C:\path\to\forex-trading-bot
call venv\Scripts\activate
python utils/codex.py --symbol XAUUSD
pause
```

Schedule it using Windows Task Scheduler to start on system boot.

### Automated Startup (Linux)

Create a systemd service `/etc/systemd/system/forex-bot.service`:

```ini
[Unit]
Description=Forex Trading Bot
After=network.target

[Service]
Type=simple
User=youruser
WorkingDirectory=/home/youruser/forex-trading-bot
Environment="PATH=/home/youruser/forex-trading-bot/venv/bin"
ExecStart=/home/youruser/forex-trading-bot/venv/bin/python utils/codex.py --symbol XAUUSD
Restart=always

[Install]
WantedBy=multi-user.target
```

Enable and start:
```bash
sudo systemctl enable forex-bot
sudo systemctl start forex-bot
```

## Safety Checklist

Before running live:

- [ ] Tested on demo account for at least 2 weeks
- [ ] Reviewed and understood all code
- [ ] Verified risk management parameters
- [ ] Set appropriate lot sizes
- [ ] Monitored bot behavior under various market conditions
- [ ] Prepared to monitor bot during first live hours
- [ ] Set up alerts for account balance changes
- [ ] Have emergency stop procedure ready

## Getting Help

If you encounter issues:

1. Check this guide and README.md
2. Review console output for error messages
3. Verify MT5 connection independently
4. Test with minimal code snippets
5. Check MetaTrader 5 documentation
6. Review broker-specific requirements

---

**Remember**: Algorithmic trading involves risk. Always test thoroughly and never risk more than you can afford to lose.

