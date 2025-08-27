# TRON Multicall3 Batch Metadata Fetcher

This project provides a Node.js script (`batchMeta.js`) to batch-fetch TRC-20 token metadata (name, symbol, decimals, totalSupply) from the TRON mainnet using the deployed [Multicall3 contract](https://tronscan.org/#/contract/TEazPvZwDjDtFeJupyo7QunvnrnUjPH8ED/code).

The script leverages **TronWeb** and **Web3** for ABI encoding/decoding, and uses the Multicall3 contract to minimize RPC calls.

---

## Features

- Fetch metadata for multiple TRC-20 tokens in **a single call**.
- Uses Multicall3's `tryAggregate` / `aggregate3` to batch constant calls.
- Supports both TronGrid and your own TRON full node.
- Outputs results in a console table with token, name, symbol, decimals, and totalSupply.

---

## Requirements

- Node.js (>= 20.x recommended)
- npm

Install dependencies:

```bash
npm install tronweb web3 dotenv
```

---

## Setup

1. Clone this repository or copy `batchMeta.js` and `README.md`.

2. Create a `.env` file in the project root:

```env
FULL_HOST=https://api.trongrid.io
TRON_GRID_KEY=                # TronGrid API key
MULTICALL3_T_ADDR=TEazPvZwDjDtFeJupyo7QunvnrnUjPH8ED
CALLER_T_ADDR=TYourAnyValidAddressHere
```

- `FULL_HOST` – your TRON RPC endpoint (TronGrid or custom node).
- `TRON_GRID_KEY` – TronGrid API key.
- `MULTICALL3_T_ADDR` – Multicall3 contract deployed on TRON mainnet.
- `CALLER_T_ADDR` – any valid TRON address (used as the `from` for constant calls).

3. Edit `batchMeta.js` to include the TRC-20 token addresses you want to fetch.

Example:

```js
const TOKENS = [
  "TNo59Khpq46FGf4sD7XSWYFNfYfbc8CqNK",
  "TDyvndWuvX5xTBwHPYJi7J3Yq8pq8yh62h",
];
```

---

## Usage

Run the script:

```bash
node batchMeta.js
```

Example output:

```
┌─────────┬─────────────────────────────────────┬─────────┬─────────┬───────────┬───────────────────────────────┐
│ (index) │               token                 │ success │  name   │  symbol  │           totalSupply          │
├─────────┼─────────────────────────────────────┼─────────┼─────────┼─────────┼───────────────────────────────┤
│    0    │ 'TNo59Khpq46FGf4sD7XSWYFNfYfbc8CqNK' │  true   │ 'Token' │  'TKN'   │ '1000000000000000000000000'   │
│    1    │ 'TDyvndWuvX5xTBwHPYJi7J3Yq8pq8yh62h' │  true   │ 'Demo'  │  'DMO'   │ '500000000000000000000000'    │
└─────────┴─────────────────────────────────────┴─────────┴─────────┴─────────┴───────────────────────────────┘
```

---

## Notes

- `totalSupply` is raw (integer). To humanize, divide by `10**decimals`.
- Some older tokens may encode `name`/`symbol` as `bytes32` – this script includes a fallback decoder.
- Respect TronGrid QPS limits (≈15 QPS per key). Use a local node for higher throughput.

---

## License

MIT
