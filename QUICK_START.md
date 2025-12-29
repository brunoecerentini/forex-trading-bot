# Quick Start Guide

Get your trading bot running in 5 minutes!

## Prerequisites

- Python 3.12 or higher installed
- MetaTrader 5 platform installed
- Demo or live trading account

## Installation

### 1. Clone Repository

```bash
git clone https://github.com/YOUR-USERNAME/forex-trading-bot.git
cd forex-trading-bot
```

### 2. Create Virtual Environment

**Windows:**
```bash
python -m venv venv
venv\Scripts\activate
```

**Linux/Mac:**
```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure Credentials

Copy the example environment file:
```bash
copy .env.example .env  # Windows
cp .env.example .env    # Linux/Mac
```

Edit `.env` with your MT5 credentials:
```env
MT5_ACCOUNT=50012345
MT5_PASSWORD=YourPassword123
MT5_SERVER=YourBroker-MT5
LOT_SIZE=0.01
```

## Running the Bot

### Option 1: Jupyter Notebook (Recommended)

```bash
jupyter notebook
```

Open `BEST----XAUUSD.IPYNB` and run all cells (Kernel → Restart & Run All)

### Option 2: Python Script

```bash
python codex.py --symbol XAUUSD
```

## What You Should See

```
Conectado à conta 50012345 / Connected to account 50012345
Variáveis calculadas: cr_upper=3355.58, cr_bot=3338.25
horario servidor mt5: 2025-04-22 20:11:33+03:00
Aguardando condições de mercado...
```

## Troubleshooting

**"Missing credentials" error**:
- Check your `.env` file exists
- Verify MT5_ACCOUNT, MT5_PASSWORD, MT5_SERVER are set

**"Failed to connect" error**:
- Ensure MetaTrader 5 is running
- Verify credentials are correct
- Check broker server name

**"Symbol not available" error**:
- Open MT5 → Market Watch → Right-click → Show All
- Verify symbol name (some brokers use suffixes like "XAUUSDm")

## Next Steps

- Read [USAGE.md](USAGE.md) for detailed configuration options
- Review [SECURITY.md](SECURITY.md) for credential management
- Check [README.md](README.md) for complete documentation

## ⚠️ Important

**Always test with a demo account first!**

This bot can place real trades. Make sure you understand the code and strategy before using real money.

---

Need help? Check the [full documentation](README.md) or [open an issue](https://github.com/YOUR-USERNAME/forex-trading-bot/issues).

