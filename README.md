# prop-trading-public

# Quantitative Trading System for Prediction Markets

A high-frequency, low-latency quantitative trading system built for prediction market event contracts. This system implements systematic trading strategies including arbitrage and market making, processing 1000+ events per second with sub-100µs decision times.

## Performance Summary

**Live Trading Results (2025)**
- **Returns**: 649% (5-figure capital deployment)
- **Sharpe Ratio**: 6.63 (daily annualized)
- **Alpha**: 6.5
- **Beta**: -0.06
- **Max Drawdown**: 4%
- **Holding Period**: < 1 day

This performance demonstrates a market-neutral strategy with exceptional risk-adjusted returns and minimal correlation to broader market movements.

## System Architecture

### Core Components

#### 1. Data Flow Manager
- **Real-time WebSocket integration** for live market data ingestion
- **Event-driven architecture** processing tick-by-tick order book updates
- **Multi-market data aggregation** from exchange APIs
- **Sub-millisecond data propagation** to strategy modules

#### 2. Strategy Framework
The system implements a modular strategy design with a base class architecture supporting:

- **Arbitrage Strategies**:
  - Cross-market arbitrage detection
  - Mutually exclusive contract arbitrage
  - Sub-penny pricing optimization

- **Market Making Strategies**:
  - Dynamic spread calculation based on inventory and volatility
  - Regime-switching adaptive parameters
  - Queue position management
  - Advanced order placement and cancellation logic

- **Hybrid Strategies**:
  - Combined market making with arbitrage detection
  - Multi-market pair trading
  - Event-driven position management

#### 3. Execution Engine
- **Asynchronous order execution** using Python's asyncio
- **Sub-100µs decision latency** from signal generation to order submission
- **Intelligent order routing** with retry logic and exponential backoff
- **Rate limiting** to comply with exchange API constraints (30 requests/second)
- **Connection pooling** for optimal throughput

#### 4. Risk Management System
- **Dynamic position sizing** based on:
  - Market volatility regimes
  - Current portfolio exposure
  - Event-specific risk parameters

- **Real-time P&L tracking**:
  - Mark-to-market valuation
  - Realized vs unrealized P&L
  - Per-strategy attribution

- **Automated circuit breakers**:
  - Maximum position limits
  - Drawdown thresholds
  - Loss limits per strategy

#### 5. Backtesting Engine
- **Tick-by-tick simulation** with realistic market microstructure:
  - Order book state reconstruction
  - Queue position tracking
  - Realistic fill simulation based on price-time priority

- **Multi-market support** for testing pair strategies
- **Parallel processing** using Python multiprocessing for parameter optimization
- **Grid search capabilities** for strategy calibration
- **Performance analytics**:
  - Sharpe ratio, alpha, beta calculations
  - Drawdown analysis
  - Trade-level statistics

#### 6. Monitoring Dashboard
- **Real-time web interface** for strategy monitoring
- **Live P&L visualization**
- **Position tracking** across all active strategies
- **Strategy control panel**:
  - Start/stop individual strategies
  - Update strategy parameters dynamically
  - Manual intervention capabilities

- **Performance metrics display**:
  - Win rate
  - Average trade duration
  - Fill rates
  - Current positions and exposure

## Technical Stack

### Core Technologies
- **Python 3.8+**: Primary language
- **asyncio**: Asynchronous I/O for concurrent operations
- **aiohttp**: HTTP client for REST API interactions
- **websockets**: WebSocket client for real-time data streams
- **pytz**: Timezone handling for event contracts

### Data Processing
- **Custom order book reconstruction** algorithms
- **Event-driven state machines** for market state tracking
- **Multi-threaded data pipeline** with thread-safe queue management

### Infrastructure
- **REST API integration** for market data and order placement
- **WebSocket streams** for real-time order book updates
- **Web server** (async) for monitoring and control

## Strategy Development Workflow

1. **Strategy Design**: Define trading logic using base strategy class
2. **Backtesting**: Validate on historical tick data with realistic fills
3. **Parameter Optimization**: Grid search using parallel processing
4. **Paper Trading**: Deploy with monitoring but no real capital
5. **Live Trading**: Gradual capital deployment with risk controls

## Key Features

### Low-Latency Execution
- Sub-100µs decision time from market data to order decision
- Optimized async event loop design
- Connection pooling and keep-alive for API calls
- Minimal dependencies in hot path

### Robust Error Handling
- Exponential backoff for API rate limits
- Automatic reconnection for WebSocket disconnections
- Graceful degradation on partial data loss
- Comprehensive logging for post-mortem analysis

### Scalability
- Concurrent strategy execution
- Multi-market monitoring and trading
- Horizontal scaling via multiple instances
- Configurable resource limits per strategy

### Production Safety
- Pre-flight validation of all orders
- Dry-run mode for testing
- Position limit enforcement
- Emergency stop mechanisms
- Audit trail of all actions

## System Performance Characteristics

- **Event Processing**: 1000+ market events/second
- **Decision Latency**: < 100µs (signal to decision)
- **Order Submission**: Sub-second round trip
- **Data Update Frequency**: Real-time (WebSocket push)
- **Uptime**: 99.9%+ during market hours

## Architecture Highlights

### Separation of Concerns
```
┌─────────────────────────────────────────────────┐
│           Monitoring Dashboard (Web UI)         │
└─────────────────────────────────────────────────┘
                       │
┌─────────────────────────────────────────────────┐
│              Strategy Controller                 │
│  ┌──────────┐ ┌──────────┐ ┌──────────────┐   │
│  │ Strategy │ │ Strategy │ │  Strategy    │   │
│  │    A     │ │    B     │ │      C       │   │
│  └──────────┘ └──────────┘ └──────────────┘   │
└─────────────────────────────────────────────────┘
                       │
┌─────────────────────────────────────────────────┐
│           Data Flow Manager                      │
│  ┌──────────────┐  ┌──────────────────────┐    │
│  │  WebSocket   │  │   REST API Client    │    │
│  │    Client    │  │   (Rate Limited)     │    │
│  └──────────────┘  └──────────────────────┘    │
└─────────────────────────────────────────────────┘
                       │
┌─────────────────────────────────────────────────┐
│              Exchange API                        │
└─────────────────────────────────────────────────┘
```

### Data Flow
1. **Market Data**: WebSocket → Data Manager → Strategy Modules
2. **Signals**: Strategy Logic → Execution Engine → REST API
3. **Fills**: WebSocket Fill Updates → Portfolio Manager → P&L Tracker
4. **Monitoring**: All Components → Web Dashboard

## Risk Management Philosophy

The system implements a defense-in-depth approach:

1. **Pre-trade validation**: Check all orders before submission
2. **Position limits**: Hard caps on exposure per market and globally
3. **Dynamic sizing**: Adjust position sizes based on volatility regime
4. **Drawdown protection**: Automatic strategy pause on threshold breach
5. **Manual override**: Web interface for emergency intervention

---

**Note**: This repository contains documentation only. The source code is proprietary and not open-sourced. This overview demonstrates the architectural approach and engineering principles behind a production quantitative trading system.


---
