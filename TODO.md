# Meridian Trading Bot - Implementation Roadmap

This is a learning-focused implementation plan. Each milestone builds on the previous one, introducing new Rust concepts gradually.

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                        Main Binary                              │
│                    (orchestrates everything)                    │
└─────────────────────────────────────────────────────────────────┘
                                │
        ┌───────────────────────┼───────────────────────┐
        │                       │                       │
        ▼                       ▼                       ▼
┌───────────────┐    ┌───────────────┐    ┌───────────────┐
│   Exchange    │    │   Strategy    │    │   Portfolio   │
│  (Alpaca API) │    │  (Signals)    │    │ (Risk Mgmt)   │
└───────────────┘    └───────────────┘    └───────────────┘
        │                       │                       │
        └───────────────────────┼───────────────────────┘
                                │
                                ▼
                    ┌───────────────────┐
                    │    Execution      │
                    │ (Order Manager)   │
                    └───────────────────┘
                                │
                                ▼
                    ┌───────────────────┐
                    │       Data        │
                    │ (Market Data)     │
                    └───────────────────┘
                                │
                                ▼
                    ┌───────────────────┐
                    │       Core        │
                    │ (Types & Traits)  │
                    └───────────────────┘
```

---

## Milestone 0: Rust Fundamentals (Before Starting)
**Goal**: Get comfortable with Rust basics

### Learning Resources
- [ ] Read Rust Book chapters 1-10 (ownership, borrowing, lifetimes)
- [ ] Complete rustlings exercises
- [ ] Understand `Result<T, E>` and `Option<T>` patterns
- [ ] Learn basic async/await with Tokio tutorial

### Key Concepts You'll Use
- **Ownership**: Values have a single owner; when owner goes out of scope, value is dropped
- **Borrowing**: References (`&T` and `&mut T`) let you use values without taking ownership
- **Traits**: Rust's interfaces - define behavior that types can implement
- **Enums**: Powerful sum types with pattern matching
- **Error handling**: `?` operator, `Result`, `thiserror` crate

---

## Milestone 1: Core Types & Traits
**Goal**: Define the foundational types everything else builds on

### Files to Implement
```
crates/core/src/
├── lib.rs              # Module exports
├── types/
│   ├── mod.rs          # Type re-exports
│   ├── decimal.rs      # Decimal wrapper with financial math
│   ├── instrument.rs   # Symbol, Exchange, Asset types
│   ├── order.rs        # Order types (Market, Limit, etc.)
│   ├── position.rs     # Position tracking
│   └── market_data.rs  # Candle, Trade, Quote types
├── events/
│   ├── mod.rs          # Event enum
│   ├── market.rs       # MarketEvent type
│   ├── execution.rs    # ExecutionEvent type
│   └── signal.rs       # SignalEvent type
└── traits/
    ├── mod.rs          # Trait re-exports
    ├── exchange.rs     # Exchange connector trait
    ├── strategy.rs     # Strategy trait
    └── portfolio.rs    # RiskManager trait
```

### Tasks
- [ ] 1.1 Create `Decimal` wrapper around `rust_decimal::Decimal`
- [ ] 1.2 Define `Symbol`, `Exchange`, `Asset` types
- [ ] 1.3 Create `OrderSide`, `OrderType`, `OrderStatus` enums
- [ ] 1.4 Define `Order` struct with builder pattern
- [ ] 1.5 Create `Position` struct
- [ ] 1.6 Define `Candle`, `Trade`, `Quote` market data types
- [ ] 1.7 Create unified `Event` enum (Market, Execution, Signal)
- [ ] 1.8 Define `Exchange` trait for connector abstraction
- [ ] 1.9 Define `Strategy` trait for signal generation
- [ ] 1.10 Define `RiskManager` trait for order validation

### Rust Concepts Introduced
- Structs and enums
- Derive macros (`Debug`, `Clone`, `Serialize`, `Deserialize`)
- The newtype pattern (wrapping types)
- Builder pattern
- Traits and trait bounds
- `thiserror` for custom errors

### Success Criteria
- [ ] `cargo build` succeeds
- [ ] All types can be serialized/deserialized
- [ ] Unit tests pass for type conversions

---

## Milestone 2: Alpaca Connection (Read-Only)
**Goal**: Connect to Alpaca and receive market data

### Files to Implement
```
crates/exchange/src/
├── lib.rs
├── error.rs            # Exchange errors
├── alpaca/
│   ├── mod.rs
│   ├── client.rs       # Alpaca REST client wrapper
│   ├── types.rs        # Alpaca-specific types
│   └── stream.rs       # WebSocket market data stream
└── normalized.rs       # Normalize Alpaca → Core types
```

### Tasks
- [ ] 2.1 Create `.env` file with Alpaca paper trading credentials
- [ ] 2.2 Wrap `apca` crate's client
- [ ] 2.3 Implement account info fetching
- [ ] 2.4 Implement asset/symbol lookup
- [ ] 2.5 Fetch historical bars (candles)
- [ ] 2.6 Subscribe to real-time trades via WebSocket
- [ ] 2.7 Normalize Alpaca types to Core types
- [ ] 2.8 Create example binary that prints live trades

### Rust Concepts Introduced
- Async/await with Tokio
- Streams (`tokio_stream`, `futures::Stream`)
- WebSocket handling
- Environment variables with `dotenvy`
- Error propagation with `?`

### Success Criteria
- [ ] Can connect to Alpaca paper trading
- [ ] Can fetch account balance
- [ ] Can receive real-time trade data
- [ ] Example prints: `AAPL Trade: $150.25 x 100 @ 2024-01-15T10:30:00Z`

---

## Milestone 3: Market Data Pipeline
**Goal**: Process and aggregate market data

### Files to Implement
```
crates/data/src/
├── lib.rs
├── feed.rs             # MarketDataFeed trait
├── aggregator.rs       # Candle aggregation from trades
├── orderbook.rs        # Order book (L1 for now)
├── snapshot.rs         # Point-in-time market snapshot
└── indicators/
    ├── mod.rs
    ├── sma.rs          # Simple Moving Average
    ├── ema.rs          # Exponential Moving Average
    └── rsi.rs          # Relative Strength Index
```

### Tasks
- [ ] 3.1 Create `MarketDataFeed` trait
- [ ] 3.2 Implement trade → candle aggregation
- [ ] 3.3 Build rolling window for indicators
- [ ] 3.4 Implement SMA (simple, good for learning)
- [ ] 3.5 Implement EMA (introduces state)
- [ ] 3.6 Implement RSI (combines multiple calculations)
- [ ] 3.7 Create `MarketSnapshot` for strategy consumption
- [ ] 3.8 Example: Print 1-minute candles with SMA(20)

### Rust Concepts Introduced
- Generics and type parameters
- Iterator adapters (`map`, `filter`, `fold`)
- State management in structs
- VecDeque for rolling windows
- Unit testing with `#[cfg(test)]`

### Success Criteria
- [ ] Trades aggregate into candles correctly
- [ ] SMA(20) calculates correctly
- [ ] RSI stays in 0-100 range
- [ ] Tests validate indicator calculations

---

## Milestone 4: Simple Strategy Framework
**Goal**: Create a pluggable strategy system

### Files to Implement
```
crates/strategy/src/
├── lib.rs
├── engine.rs           # Strategy execution engine
├── context.rs          # Read-only context for strategies
├── signals.rs          # Signal types (Buy, Sell, Hold)
└── strategies/
    ├── mod.rs
    ├── sma_crossover.rs  # SMA crossover strategy (learning)
    └── buy_and_hold.rs   # Simplest strategy (always hold)
```

### Tasks
- [ ] 4.1 Implement `Strategy` trait from core
- [ ] 4.2 Create `StrategyContext` (read-only market state)
- [ ] 4.3 Define `Signal` enum (Buy, Sell, Hold + metadata)
- [ ] 4.4 Implement `BuyAndHoldStrategy` (trivial baseline)
- [ ] 4.5 Implement `SmaCrossoverStrategy` (classic strategy)
- [ ] 4.6 Create strategy engine that runs strategies on events
- [ ] 4.7 Example: Run SMA crossover on live data, print signals

### Rust Concepts Introduced
- Trait objects (`Box<dyn Strategy>`)
- Dynamic dispatch vs static dispatch
- `impl Trait` return types
- Interior mutability (if needed)
- Pattern matching on enums

### Success Criteria
- [ ] Strategies generate signals from market data
- [ ] SMA crossover produces buy/sell signals correctly
- [ ] Can swap strategies without code changes
- [ ] Signals include reasoning/metadata

---

## Milestone 5: Risk Management
**Goal**: Validate orders before execution

### Files to Implement
```
crates/portfolio/src/
├── lib.rs
├── manager.rs          # Portfolio state manager
├── performance.rs      # PnL, returns calculation
└── risk/
    ├── mod.rs
    ├── limits.rs       # Position size limits
    ├── stops.rs        # Stop loss logic
    └── exposure.rs     # Max exposure checks
```

### Tasks
- [ ] 5.1 Implement `RiskManager` trait from core
- [ ] 5.2 Create `PortfolioState` (positions, cash, equity)
- [ ] 5.3 Implement max position size check
- [ ] 5.4 Implement max total exposure check
- [ ] 5.5 Add stop-loss price calculation
- [ ] 5.6 Calculate basic PnL (unrealized, realized)
- [ ] 5.7 Risk manager returns `Approved` or `Refused` with reason

### Rust Concepts Introduced
- State management across async boundaries
- `RwLock` and `Mutex` from `parking_lot`
- Composition over inheritance
- Result types with custom errors

### Success Criteria
- [ ] Risk manager blocks oversized orders
- [ ] Stop-loss prices are calculated correctly
- [ ] PnL calculations match expected values
- [ ] All refusals include clear reasons

---

## Milestone 6: Order Execution
**Goal**: Submit and track orders

### Files to Implement
```
crates/execution/src/
├── lib.rs
├── engine.rs           # Order execution engine
├── orders/
│   ├── mod.rs
│   ├── builder.rs      # Order construction
│   ├── validator.rs    # Pre-submission validation
│   └── tracker.rs      # Order state tracking
└── allocator.rs        # Position sizing
```

### Tasks
- [ ] 6.1 Implement order submission via Alpaca
- [ ] 6.2 Create order state machine (New → Submitted → Filled/Cancelled)
- [ ] 6.3 Track open orders in `DashMap`
- [ ] 6.4 Handle partial fills
- [ ] 6.5 Implement order cancellation
- [ ] 6.6 Create execution event stream
- [ ] 6.7 Basic position sizing (fixed fractional)

### Rust Concepts Introduced
- State machines with enums
- Concurrent data structures (`DashMap`)
- Channels for event propagation (`tokio::sync::mpsc`)
- Error recovery and retry logic

### Success Criteria
- [ ] Can submit market orders to Alpaca paper
- [ ] Order status updates are received
- [ ] Fills update portfolio state
- [ ] Cancellation works correctly

---

## Milestone 7: Event Loop & Integration
**Goal**: Wire everything together

### Files to Implement
```
src/
├── main.rs             # Application entry point
├── config.rs           # Configuration loading
└── engine.rs           # Main event loop
```

### Tasks
- [ ] 7.1 Create main event loop (inspired by barter-rs)
- [ ] 7.2 Wire: Market Data → Strategy → Risk → Execution
- [ ] 7.3 Handle graceful shutdown (Ctrl+C)
- [ ] 7.4 Add tracing/logging throughout
- [ ] 7.5 Configuration via TOML file
- [ ] 7.6 Paper trading end-to-end test
- [ ] 7.7 Add basic health checks

### Rust Concepts Introduced
- `tokio::select!` for concurrent operations
- Signal handling (`tokio::signal`)
- Configuration parsing
- Structured logging with `tracing`

### Success Criteria
- [ ] Bot runs continuously on paper trading
- [ ] Signals trigger orders
- [ ] Logs show full event flow
- [ ] Clean shutdown preserves state

---

## Milestone 8: Backtesting (Future)
**Goal**: Test strategies on historical data

### Tasks (Outline)
- [ ] 8.1 Historical data fetching and storage
- [ ] 8.2 Event replay system
- [ ] 8.3 Mock exchange for backtesting
- [ ] 8.4 Performance metrics (Sharpe, Sortino, Max Drawdown)
- [ ] 8.5 Backtest CLI command

---

## Milestone 9: Production Hardening (Future)
**Goal**: Make it reliable enough for real money

### Tasks (Outline)
- [ ] 9.1 Circuit breakers (halt on anomalies)
- [ ] 9.2 Rate limiting and backoff
- [ ] 9.3 Reconnection handling
- [ ] 9.4 State persistence and recovery
- [ ] 9.5 Metrics export (Prometheus)
- [ ] 9.6 Alerting integration
- [ ] 9.7 Multi-strategy support

---

## Milestone 10: Multi-Exchange Support (Future)
**Goal**: Trade on multiple venues

### Tasks (Outline)
- [ ] 10.1 Abstract exchange connector
- [ ] 10.2 Add Interactive Brokers support
- [ ] 10.3 Cross-exchange order routing
- [ ] 10.4 Arbitrage strategy example

---

## Development Guidelines

### Testing Strategy
1. **Unit tests**: Test individual functions and types
2. **Integration tests**: Test crate interactions
3. **Paper trading**: Test with real market data, fake money
4. **Backtests**: Test on historical data before live

### Code Quality
- Run `cargo clippy` before committing
- Run `cargo fmt` for consistent formatting
- Write doc comments for public APIs
- Keep functions small and focused

### Safety Rules
1. **NEVER use f64 for money** - Always `Decimal`
2. **NEVER store API keys in code** - Use `.env` files
3. **ALWAYS paper trade first** - Test before real money
4. **ALWAYS have stop-losses** - Protect capital
5. **ALWAYS log everything** - Debug production issues

---

## Quick Reference

### Running the Bot
```bash
# Development
cargo run

# With logging
RUST_LOG=debug cargo run

# Run tests
cargo test

# Run specific crate tests
cargo test -p meridian-core
```

### Alpaca Paper Trading Setup
```bash
# Create .env file
echo "APCA_API_KEY_ID=your_key" >> .env
echo "APCA_API_SECRET_KEY=your_secret" >> .env
echo "APCA_API_BASE_URL=https://paper-api.alpaca.markets" >> .env
```

### Useful Commands
```bash
# Check for issues
cargo clippy

# Format code
cargo fmt

# Generate docs
cargo doc --open

# Watch for changes
cargo watch -x check
```
