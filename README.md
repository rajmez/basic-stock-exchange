# Basic Stock Exchange

A small, in-memory stock exchange simulator written in Python.
It supports limit-order matching for buy and sell orders, tracks fill progress, and stores completed orders.

## Features

- Limit **buy** and **sell** order placement
- Per-symbol order books
  - Buy side prioritized by highest price (max-priority behavior)
  - Sell side prioritized by lowest price (min-priority behavior)
- Full and partial fills
- Per-order fields for:
  - `status` (`open`, `partially filled`, `filled`)
  - `filled_quantity`
  - `avg_price_got`
- Basic CLI for manual interaction
- Test coverage for common buy/sell matching paths

## Repository Layout

- `exchange.py` — core matching engine and order-book management
- `order.py` — `Order` dataclass (ID generation + execution metadata)
- `trade.py` — `Trade` dataclass (defined, but not fully wired into matching)
- `main.py` — basic command-line interface
- `test_buy.py` — tests for buy-side scenarios
- `test_sell.py` — tests for sell-side scenarios

## How Matching Works

### Incoming Buy Limit Order

A buy order attempts to match the best sell order(s) for the same symbol while:

- sell price is less than or equal to buy limit price
- buy quantity is still remaining

### Incoming Sell Limit Order

A sell order attempts to match the best buy order(s) for the same symbol while:

- buy price is greater than or equal to sell limit price
- sell quantity is still remaining

### Execution Behavior

During matching, the engine updates:

- remaining quantity on both sides
- cumulative filled quantity
- average execution price
- final status of each order

Any remaining unfilled quantity is kept on the corresponding order book.

## Run the App

```bash
python main.py
```

Current CLI notes:

- New limit order flow is implemented.
- Some menu options are placeholders/incomplete.

## Run Tests

```bash
pytest -q
```

## Known Limitations

- Order books and order history are in-memory only (no persistence).
- `Trade` objects are defined but trade recording is not fully integrated in matching flow.
- Matching logic is monolithic and can be refactored for readability and maintainability.
- Input validation and error handling are minimal.

## Suggested Next Improvements

1. Add complete market-order support.
2. Persist trades/orders (database or file-backed storage).
3. Emit and store a `Trade` record on every fill.
4. Refactor matching into smaller helper functions.
5. Expand tests for edge cases and invalid inputs.
