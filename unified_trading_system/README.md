# Unified Trading System

**POMDP-based Autonomous Trading System with Kelly Position Sizing & Regime Detection**

[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/downloads/)
[![Binance Testnet](https://img.shields.io/badge/binance-testnet-orange.svg)](https://testnet.binancefuture.com/)
[![Status: Running](https://img.shields.io/badge/status-running-brightgreen.svg)]()

---

## Current Status

| Attribute | Value |
|-----------|-------|
| **Status** | ✅ RUNNING (PID 1031751, tmux: `trading`) |
| **Cycles Completed** | 34,333+ |
| **Account Balance** | $4,919.50 (crossWalletBalance) |
| **Leverage** | 20x default (15x–25x range) |
| **Python Version** | 3.12.3 |
| **Trading Pairs** | 12 (BTC, ETH, BNB, SOL, ADA, XRP, DOGE, DOT, AVAX, LINK, LTC, BCH) |
| **Cycle Interval** | 10 seconds |
| **Win Rate** | ~35% (post Phase 1 fix) |
| **Expected Value** | +0.0009 per trade |

---

## Quick Start (5 Steps — <30 Minutes)

```bash
# 1. Clone the repository
git clone <repository-url>
cd unified_trading_system

# 2. Create virtual environment
python3 -m venv .venv
source .venv/bin/activate

# 3. Install ALL dependencies (14 packages)
pip install --upgrade pip
pip install -r requirements.txt

# 4. Configure your Binance Testnet API keys
cp .env.example .env
nano .env   # Edit with your keys from https://testnet.binancefuture.com/

# 5. Run the system
python3 run_enhanced_testnet.py
```

---

## Architecture Overview

```
Market Data (Binance API)
       ↓
Perception Layer → BeliefState (expected_return, confidence, regime)
       ↓
Decision Layer → SignalGenerator (action, confidence, quantity)
       ↓
Risk Layer → RiskManifold + SafetyGovernor (position sizing, limits)
       ↓
Execution Layer → Binance Order Placement (market orders)
       ↓
Learning Layer → TradeJournal (persistence, ML training)
       ↓
Observability → Telegram Alerts, Prometheus Metrics, Health Checks
```

---

## Key Features

| Feature | Description |
|---------|-------------|
| **POMDP Belief State** | Bayesian filter updating regime probabilities (8 regimes) |
| **LVR Microstructure** | OFI, I*, L*, S* features + depth imbalance |
| **Kelly Position Sizing** | Dynamic sizing: confidence-based + regime modifier + hourly modifier |
| **Regime Detection** | HMM + KMeans + GaussianMixture (8 regimes: BULL/BEAR/SIDEWAYS x HIGH/LOW_VOL + CRISIS + RECOVERY) |
| **Enhanced Exit Strategy** | TP (0.3%) → Time (regime-based 30–300s) → SL (1.5–3%) → Trailing (2% activation) |
| **Multi-Model ML** | XGBoost + LSTM + Transformer + RandomForest ensemble |
| **Safety & Governance** | 5-level risk system (NORMAL→CRITICAL), pre-trade checks, emergency stop |
| **Full Observability** | Telegram alerts, Prometheus metrics (:9090), Health checks (:8080) |

---

## Performance Metrics

| Metric | Current Value | Target |
|--------|--------------|--------|
| Win Rate | ~35% | ≥65% |
| Expected Value | +0.0009/trade | ≥+0.005/trade |
| Sharpe Ratio | ~1.2 | ≥3.0 |
| Max Drawdown | <15% | <10% |
| Trades/Day | ~10-25 | ~100+ |

---

## Documentation Index

| Document | Description |
|----------|-------------|
| [Installation Guide](docs/INSTALLATION.md) | Fresh server setup (OS packages, venv, 14 deps) |
| [Configuration Reference](docs/CONFIGURATION.md) | All env vars, config files, parameters |
| [Portability Checklist](docs/PORTABILITY.md) | Clone-and-run on a new server in 12 steps |
| [Deployment Guide](docs/deployment/DEPLOYMENT_GUIDE.md) | tmux, systemd, Docker deployment |
| [Mathematical Foundations](docs/research/MATHEMATICAL_FOUNDATIONS.md) | POMDP, Kelly, regime transition matrix |
| [Risk Budget](docs/risk/RISK_BUDGET.md) | Leverage constraints, drawdown waterfall, position limits |
| [ML API Complete](docs/reference/ML_API_COMPLETE.md) | All 299 ML functions/classes documented |
| [Algorithm Analysis](docs/research/ALGORITHM_ANALYSIS.md) | O(n) analysis, event loop, complexity |
| [Data Pipeline](docs/data/DATA_PIPELINE.md) | Market data flow, feature store, provenance chain |
| [SRE Runbook](docs/runbooks/SRE_RUNBOOK.md) | Scenarios, incident response, health check fix |
| [Online Learning](docs/learning/ONLINE_LEARNING.md) | Drift detection, model versioning, ensemble |
| [CFA Attestation](docs/compliance/CFA_ATTESTATION.md) | CFA standards, ethics, disclosure compliance |
| [Troubleshooting](docs/reference/TROUBLESHOOTING.md) | Common issues, fixes, known problems |

---

## Project Structure

```
unified_trading_system/
├── run_enhanced_testnet.py          # Entry point (START HERE)
├── continuous_trading_loop_binance.py  # Core engine (1,932 lines)
├── requirements.txt                  # 14 Python dependencies
├── .env                              # API keys (gitignored)
├── .env.example                     # Template for .env
├── config/
│   ├── unified.yaml                # System + risk + signal config (244 lines)
│   ├── trading_params.yaml          # Regime risk, time maps, vol multipliers
│   └── learning.yaml                # Learning system parameters
├── perception/
│   ├── belief_state.py           # POMDP belief state (486 lines)
│   └── enhanced_belief_state.py  # Volatility modeling
├── decision/
│   └── signal_generator.py       # Signal generation (502 lines)
├── risk/
│   ├── unified_risk_manager.py  # Risk manifold (1,399 lines)
│   └── enhanced_risk_manager.py  # Advanced risk controls
├── execution/
│   └── smart_order_router.py    # Order placement (688 lines)
├── learning/
│   ├── trade_journal.py         # Trade recording (217 lines)
│   ├── return_predictor.py       # PyTorch NN models
│   ├── ensemble_trainer.py      # XGBoost + LSTM + Transformer
│   ├── regime_detector.py        # HMM + KMeans + GMM
│   └── kronos_integration.py     # Time-series forecasting
├── safety/
│   └── governance.py             # Safety checks, emergency stop (405 lines)
├── observability/
│   ├── alerting.py              # Telegram alerts (461 lines)
│   ├── health.py                # HTTP health checks (347 lines)
│   └── metrics.py               # Prometheus metrics (389 lines)
├── docs/                            # Complete documentation (15 documents)
├── logs/                           # Trade journal, cycle logs
└── manage.sh                       # tmux session manager
```

---

## Support & Compliance

- **CFA Institute Code of Ethics**: Compliant (Standards I(C) & VI)
- **Data Provenance**: Real testnet data only (synthetic removed in Phase 1)
- **Performance Disclaimer**: "Metrics based on Testnet data. Not indicative of live trading results."

---

*System Version: 3.2.0 | Last Updated: 2026-05-04 | Status: RUNNING*
