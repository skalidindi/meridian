# Meridian

A production-ready stock trading bot built in Rust, focusing on reliability, performance, and maintainability.

## Goals

- **Production-ready**: Capable of handling real money with robust risk management
- **High-performance**: Real-time market data processing with minimal latency
- **Reliable**: Proper error handling, recovery mechanisms, and fault tolerance
- **Extensible**: Modular design supporting multiple exchanges and strategies

## Tech Stack

- **Rust** with **Tokio** async runtime
- **WebSocket** connections for real-time market data
- **REST APIs** for order execution
- Event-driven architecture with type-safe abstractions

## Project Status

Currently in early development (Phase 1: Foundation).

## Exchange Support (Planned)

| Priority | Exchange | Status |
|----------|----------|--------|
| 1 | Kraken | Planned |
| 1 | Coinbase | Planned |
| 2 | Binance | Planned |
| 2 | Interactive Brokers | Planned |

## Building

```bash
cargo build
```

## Testing

```bash
cargo test
```

## License

MIT
