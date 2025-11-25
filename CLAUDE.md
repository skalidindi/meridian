# Meridian Project - Claude Context

## Project Overview
Building a production-ready stock trading bot in Rust with a focus on reliability, performance, and maintainability.

## Goals

### Primary Objectives
- Build a **production-ready** trading bot capable of handling real money
- Create a **custom implementation** optimized for our specific needs
- Develop **robust risk management** to protect capital
- Implement **high-performance** data processing for real-time market analysis
- Ensure **reliability** with proper error handling and recovery mechanisms

### Learning Objectives
- Master async Rust patterns for financial applications
- Understand exchange API integrations and WebSocket protocols
- Learn from barter-rs architecture while building custom solution
- Develop expertise in algorithmic trading systems

## Technical Stack

### Core Dependencies (Already Added)
- **tokio** (1.47): Async runtime with full features
- **reqwest** (0.12): HTTP client for REST APIs
- **serde/serde_json** (1.0): Serialization/deserialization
- **tokio-tungstenite** (0.24): WebSocket support

### Additional Dependencies to Consider
- **rust_decimal**: Precise decimal handling for financial calculations
- **chrono**: Timestamp and time zone management
- **tracing**: Structured logging and diagnostics
- **sqlx**: Database persistence (PostgreSQL/SQLite)
- **redis**: Caching and pub/sub for distributed systems

## Development Approach

### Phase 1: Foundation & Learning (Current)
- ✅ Set up Rust project with Tokio
- ✅ Research barter-rs for architectural patterns
- Study exchange APIs and data formats
- Design core abstractions and interfaces

### Phase 2: Core Implementation
- **Exchange Connectors**: Custom WebSocket/REST integrations
- **Data Pipeline**: Normalize and process market data streams
- **Order Management**: Execution, tracking, and reconciliation
- **Strategy Framework**: Pluggable strategy interface

### Phase 3: Production Features
- **Risk Management**: Position sizing, stop losses, exposure limits
- **Monitoring**: Health checks, metrics, alerting
- **Backtesting**: Historical data testing infrastructure
- **Persistence**: Trade history, state recovery

### Phase 4: Advanced Features
- **Multi-exchange**: Arbitrage and best execution
- **ML Integration**: Predictive models and signals
- **Portfolio Management**: Multi-asset allocation
- **Performance Analytics**: Sharpe, Sortino, drawdown analysis

## Key Design Decisions

### Why Custom Implementation (Not Barter-rs)
- **Production Ready**: Barter-rs explicitly disclaims production use
- **Full Control**: Complete ownership of critical trading logic
- **Exchange Support**: Flexibility to add any exchange (Kraken, IBKR, etc.)
- **Regulatory Compliance**: Ability to meet specific requirements
- **Custom Features**: Implement proprietary strategies and optimizations

### Architecture Principles
- **Event-Driven**: Reactive to market events with minimal latency
- **Modular Design**: Separate concerns (data, strategy, execution)
- **Type Safety**: Leverage Rust's type system for correctness
- **Zero-Copy**: Minimize allocations in hot paths
- **Fault Tolerant**: Graceful degradation and recovery

### What We Learn from Barter-rs
- Event-driven architecture with Tokio
- Normalized data structures across exchanges
- Strategy and RiskManager trait abstractions
- Backtesting infrastructure design
- Performance metrics calculation

## Exchange Support Plan

### Priority 1 (Initial Implementation)
- **Kraken**: Strong API, good liquidity, reliable
- **Coinbase**: Major US exchange, regulatory compliant

### Priority 2 (Expansion)
- **Binance**: Largest volume, global reach
- **Interactive Brokers**: Traditional markets access

### Priority 3 (Specialized)
- **FTX**: Derivatives and advanced products
- **Bybit**: Crypto derivatives focus

## Risk Management Philosophy
- **Capital Preservation First**: Never risk more than X% per trade
- **Position Sizing**: Kelly criterion or fixed fractional
- **Stop Losses**: Always define maximum loss
- **Correlation Limits**: Avoid concentrated exposure
- **Circuit Breakers**: Halt on unusual market conditions

## Development Guidelines

### Code Quality
- Comprehensive error handling with `Result<T, E>`
- Unit tests for all business logic
- Integration tests for exchange connections
- Property-based testing for strategies

### Performance
- Profile and optimize hot paths
- Minimize allocations in market data processing
- Use concurrent processing where beneficial
- Cache frequently accessed data

### Security
- Never log sensitive credentials
- Encrypt API keys at rest
- Use environment variables for configuration
- Implement rate limiting and backoff

## Current Status
- Project initialized with Tokio and core dependencies
- Researched barter-rs and made architectural decisions
- Ready to begin Phase 2: Core Implementation

## Next Steps
1. Design exchange connector interface
2. Implement Kraken WebSocket integration
3. Build data normalization layer
4. Create basic strategy framework
5. Develop order management system

## Notes for Future Claude Sessions
- This is a production trading system - prioritize reliability and correctness
- We're building custom, not using barter-rs (except as reference)
- Focus on clean abstractions that allow for future flexibility
- Always consider risk management implications
- Performance matters but not at the expense of correctness