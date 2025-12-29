# Forex Trading Bot - Multi-Currency Support

[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/downloads/)
[![MetaTrader5](https://img.shields.io/badge/MetaTrader-5-green.svg)](https://www.metatrader5.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A sophisticated automated trading system that implements support/resistance zone-based strategies for multiple forex currency pairs and Gold (XAUUSD). The bot uses technical analysis to identify trading opportunities and executes trades automatically via MetaTrader 5.

## 🎯 Project Overview

This project is a professional-grade algorithmic trading system designed for the forex market. It demonstrates advanced concepts in:
- **Algorithmic Trading**: Automated trade execution based on technical indicators
- **Risk Management**: Position sizing, stop-loss, and take-profit management
- **Multi-Asset Support**: Trading across XAUUSD, EURUSD, GBPUSD, and USDJPY
- **Real-time Market Monitoring**: Continuous price analysis and trade signal generation
- **Azure ML Integration**: Cloud-based model training and deployment capabilities

## 📊 Supported Trading Pairs

- **XAUUSD** (Gold/USD) - Primary focus with advanced zone detection
- **EURUSD** (Euro/USD) - Major forex pair
- **GBPUSD** (British Pound/USD) - Major forex pair
- **USDJPY** (USD/Japanese Yen) - Major forex pair

## ✨ Key Features

### Trading Strategy
- **Support/Resistance Zone Detection**: Identifies key price levels from historical data
- **Zone-Based Entry Signals**: Enters trades when price breaks through critical zones
- **Dynamic Stop-Loss & Take-Profit**: Automatically calculated based on market volatility
- **Position Management**: Tracks open positions and prevents over-trading
- **End-of-Day Position Closure**: Risk management through daily reset

### Technical Implementation
- **Real-time Data Processing**: Connects to MetaTrader 5 for live market data
- **Candlestick Pattern Analysis**: Uses M1 and M15 timeframes for signal generation
- **Memory Optimization**: Efficient garbage collection for long-running operations
- **Timezone Management**: Handles GMT+3 broker time with proper conversion
- **Error Handling**: Robust error recovery and logging mechanisms

### Visualization & Analysis
- **Candlestick Charts**: Visual representation of support/resistance levels
- **Trade Signal Markers**: Buy/sell signals highlighted on charts
- **Weekly Performance Plots**: Historical analysis of trading patterns
- **Confusion Matrices**: Model performance evaluation (ML version)

## 🛠️ Technology Stack

- **Python 3.12+**: Core programming language
- **MetaTrader5**: Trading platform integration
- **pandas**: Data manipulation and analysis
- **matplotlib**: Chart generation and visualization
- **pytz**: Timezone handling
- **Azure ML** (Optional): Cloud-based model training
- **scikit-learn** (ML version): Machine learning algorithms

## 📁 Project Structure

```
forex-trading-bot/
│
├── trading_bots/           # Main trading bot implementations
│   ├── BEST----XAUUSD.IPYNB
│   ├── BEST----EURUSD.IPYNB
│   ├── BEST----GBPUSD.IPYNB
│   └── BEST----USDJPY.IPYNB
│
├── machine_learning/       # ML model training and evaluation
│   ├── train.py
│   ├── RFE_feature_selection.py
│   └── generate_dataset.py
│
├── azure_ml/              # Azure ML integration
│   └── AzureML_artefatos.ipynb
│
├── utils/                 # Utility scripts
│   └── codex.py          # Command-line interface tool
│
├── OLD_V1/               # Legacy versions (for reference)
│
├── docs/                 # Documentation
│   └── USAGE.md
│
├── .env.example          # Environment variables template
├── .gitignore           # Git ignore rules
├── requirements.txt     # Python dependencies
└── README.md           # This file
```

## 🚀 Quick Start

### Prerequisites

1. **Python 3.12 or higher**
2. **MetaTrader 5 platform** installed and configured
3. **Demo or Live Trading Account** from a broker supporting MT5

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/forex-trading-bot.git
cd forex-trading-bot
```

2. **Create and activate virtual environment**
```bash
python -m venv venv
# On Windows
venv\Scripts\activate
# On Linux/Mac
source venv/bin/activate
```

3. **Install dependencies**
```bash
pip install -r requirements.txt
```

4. **Configure credentials**
```bash
# Copy the example environment file
cp .env.example .env

# Edit .env with your credentials
# Never commit .env to version control!
```

### Configuration

Edit the `.env` file with your MetaTrader 5 credentials:

```env
MT5_ACCOUNT=your_account_number
MT5_PASSWORD=your_password
MT5_SERVER=your_broker_server
LOT_SIZE=0.01
RISK_PERCENTAGE=2
```

## 💻 Usage

### Running a Trading Bot

**Option 1: Using Jupyter Notebooks (Recommended for testing)**

```bash
jupyter notebook
# Open any of the BEST----[SYMBOL].IPYNB files
# Update the credentials section with environment variables
# Run all cells
```

**Option 2: Using Python Script**

```bash
python utils/codex.py --symbol XAUUSD --account YOUR_ACCOUNT --password YOUR_PASSWORD --server YOUR_SERVER
```

### Command-Line Interface

The `codex.py` utility provides a flexible CLI for bot operations:

```bash
# Run with default settings
python utils/codex.py --symbol XAUUSD

# Specify trading parameters
python utils/codex.py --symbol EURUSD --lot-size 0.05 --timeframe M15

# Run in backtest mode (if implemented)
python utils/codex.py --symbol GBPUSD --backtest --start-date 2024-01-01
```

### Training ML Models (Advanced)

```bash
# Generate dataset from historical data
python machine_learning/generate_dataset.py

# Train the model
python machine_learning/train.py

# Feature selection using RFE
python machine_learning/RFE_feature_selection.py
```

## 📈 Trading Strategy Explained

### Zone Calculation

The bot identifies key price levels using the first N candles of the week:

1. **CR (Central Range)**: High and low of initial candles
2. **ZN (Zone)**: Extension above and below CR by the range distance
3. **N1, N2, N3 Levels**: Fibonacci-like extensions for take-profit targets

### Entry Signals

**Buy Signal**:
- Price opens below ZN_UPPER
- Price closes above ZN_UPPER
- Stop Loss: CR_BOT - spread
- Take Profit: N1_100_UPPER

**Sell Signal**:
- Price opens above ZN_BOT
- Price closes below ZN_BOT
- Stop Loss: CR_UPPER + spread
- Take Profit: N1_100_BOT

### Risk Management

- Maximum 1 open position per symbol
- Automatic position closure at end of trading day (23:59 broker time)
- Dynamic lot sizing based on account equity
- Stop-loss orders on every trade

## 🔒 Security Best Practices

⚠️ **IMPORTANT**: Never commit sensitive information to version control

- ✅ Use environment variables for credentials
- ✅ Add `.env` to `.gitignore`
- ✅ Use demo accounts for testing
- ✅ Review all files before pushing to GitHub
- ❌ Never hardcode passwords or account numbers
- ❌ Never commit actual trading data (CSVs, models with fitted data)

## 📊 Performance Monitoring

The bot generates several outputs for performance analysis:

- **Candlestick Charts**: Visual representation of trades
- **CSV Files**: Historical price data and trade logs
- **Console Logs**: Real-time trade execution information

Example output:
```
Conectado à conta [YOUR_ACCOUNT]
Variáveis calculadas: cr_upper=3355.58, cr_bot=3338.25
Condição de compra atendida: Open=3368.77, Close=3373.77, SL=3336.25, TP=3407.57
Ordem de compra enviada: SL=3336.25, TP=3407.57
```

## 🧪 Testing

⚠️ **Always test with a demo account first!**

1. Open a demo account with your broker
2. Configure the bot with demo credentials
3. Monitor performance for at least 1-2 weeks
4. Analyze trade logs and performance metrics
5. Only after successful demo trading, consider live deployment

## 📝 Known Limitations

- Requires active MetaTrader 5 connection
- Timezone handling assumes broker uses GMT+3
- No built-in backtest engine (uses live/demo only)
- Memory usage can grow with extended runtime (optimized with garbage collection)
- Requires manual monitoring of broker connection stability

## 🔮 Future Enhancements

- [ ] Implement comprehensive backtesting engine
- [ ] Add machine learning-based signal filtering
- [ ] Multi-timeframe analysis integration
- [ ] Telegram/Discord notifications
- [ ] Web dashboard for monitoring
- [ ] Advanced risk management (trailing stops, pyramiding)
- [ ] Support for additional asset classes (crypto, stocks)

## 🤝 Contributing

This is a personal portfolio project, but suggestions and feedback are welcome! Feel free to:

1. Fork the repository
2. Create a feature branch
3. Submit a pull request with improvements

## ⚖️ Legal Disclaimer

**THIS SOFTWARE IS FOR EDUCATIONAL PURPOSES ONLY.**

- Trading forex and CFDs carries significant risk
- Past performance does not guarantee future results
- Only trade with money you can afford to lose
- This bot is provided "as-is" without warranty
- The author is not responsible for any financial losses
- Always comply with your local financial regulations

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 👨‍💻 Author

**Bruno** - Algorithmic Trading Enthusiast & Python Developer

- Experienced in quantitative finance and algorithmic trading
- Proficient in Python, pandas, and financial market analysis
- Familiar with MetaTrader 5 API and Azure ML
- Passionate about automation and systematic trading strategies

## 📞 Contact

For professional inquiries or collaboration opportunities:

- GitHub: [Your GitHub Profile]
- LinkedIn: [Your LinkedIn Profile]
- Email: [Your Professional Email]

---

**⭐ If you find this project interesting, please consider giving it a star!**

*Developed as part of a professional portfolio to demonstrate expertise in algorithmic trading, Python development, and financial technology.*

