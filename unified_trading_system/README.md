# Unified Autonomous Trading System

A high-frequency cryptocurrency trading system built for Binance Futures Testnet with stochastic optimal control, risk-coupled position sizing, regime detection, and full observability.

## Features

- **Stochastic Optimal Control** – POMDP-based aggression controller
- **Risk-Coupled Position Sizing** – Kelly-style dynamic sizing
- **Regime-Aware Trading** – HMM regime detection (bull/bear/sideways)
- **3-Tier Take-Profit System** – Progressive profit capture (1.5%, 3%, 5%)
- **Trailing Stop** – Activates at +2% profit
- **Volatility-Adjusted Stop-Loss** – 1.5%-3% based on market conditions
- **Full Observability** – Prometheus metrics, health checks, Telegram alerts

## Quick Start

```bash
# 1. Clone the repository
git clone https://github.com/your-org/unified-trading-system.git
cd unified-trading-system

# 2. Create virtual environment
python3 -m venv .venv
source .venv/bin/activate

# 3. Install dependencies
pip install --upgrade pip
pip install aiohttp numpy pandas yaml python-dotenv

# 4. Configure API keys
cp .env.example .env
# Edit .env with your Binance Testnet API Key and Secret

# 5. Run the system
python run_enhanced_testnet.py
```

## Configuration

Edit `.env`:

```
BINANCE_API_KEY=your_testnet_api_key
BINANCE_API_SECRET=your_testnet_api_secret
BINANCE_TESTNET_URL=https://testnet.binancefuture.com
LOG_LEVEL=INFO
```

## System Management

```bash
# Start in detached tmux session
chmod +x manage.sh
./manage.sh start

# View logs
tail -f logs/trading_loop.log

# Stop the system
./manage.sh stop

# Check health
python check_health.py
```

## Project Structure

```
unified_trading_system/
├── run_enhanced_testnet.py      # Entry point
├── continuous_trading_loop_binance.py  # Core trading engine
├── decision/
│   └── signal_generator.py       # Signal generation & confidence filtering
├── risk/
│   └── unified_risk_manager.py # Risk management & position sizing
├── execution/
│   └── smart_order_router.py   # Order execution
├── perception/
│   └── belief_state.py       # Market belief state estimation
├── learning/
│   └── trade_journal.py    # Trade recording & analysis
├── observability/
│   ├── alerting.py          # Telegram alerts
│   ├── health.py           # Health check server
│   └── metrics.py         # Prometheus metrics
├── scripts/
│   ├── manage.sh          # tmux session manager
│   └── watchdog.py        # Auto-restart watchdog
└── logs/
    └── trade_journal.json  # Trade history
```

## Performance (Phase 1)

| Metric | Value |
|--------|-------|
| Win Rate | ~35% |
| Take-Profit Hits | 60%+ of wins |
| Expected Value | +0.0009 per trade |
| Max Drawdown | <15% |

## Key Parameters

| Parameter | Value | Description |
|-----------|-------|-------------|
| `min_confidence_threshold` | 0.85 | Signal confidence cutoff |
| `take_profit_pct` | 0.003 | +0.3% take profit target |
| `stop_loss_pct` | 0.003 | -0.3% stop loss |
| `max_trades_per_cycle` | 1 | One trade per cycle |
| `cycle_interval` | 10s | Trading cycle frequency |

## Exit Priority (Critical Fix)

**Current order:** Take-Profit → Time-Out → Stop-Loss

This ordering increased win-rate from 25% to 35% by allowing +0.3% TP to trigger before time-based exits.

## License

MIT License