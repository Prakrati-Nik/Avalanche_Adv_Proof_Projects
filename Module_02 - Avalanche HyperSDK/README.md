<p align="center">
  <img width="90%" alt="tokenvm" src="assets/logo.png">
</p>
<p align="center">
  Mint, Transfer, and Trade Tokens Seamlessly, All On-Chain
</p>
<p align="center">
  <a href="https://github.com/ava-labs/hypersdk/actions/workflows/tokenvm-static-analysis.yml"><img src="https://github.com/ava-labs/hypersdk/actions/workflows/tokenvm-static-analysis.yml/badge.svg" /></a>
  <a href="https://github.com/ava-labs/hypersdk/actions/workflows/tokenvm-unit-tests.yml"><img src="https://github.com/ava-labs/hypersdk/actions/workflows/tokenvm-unit-tests.yml/badge.svg" /></a>
  <a href="https://github.com/ava-labs/hypersdk/actions/workflows/tokenvm-sync-tests.yml"><img src="https://github.com/ava-labs/hypersdk/actions/workflows/tokenvm-sync-tests.yml/badge.svg" /></a>
  <a href="https://github.com/ava-labs/hypersdk/actions/workflows/tokenvm-load-tests.yml"><img src="https://github.com/ava-labs/hypersdk/actions/workflows/tokenvm-load-tests.yml/badge.svg" /></a>
</p>

---

The [`tokenvm`](./examples/tokenvm) demonstrates the integration of `hypersdk` in building token-based applications. With `tokenvm`, users can mint, transfer, and trade custom tokens directly on-chain. It supports creating tokens, modifying metadata, minting more, or burning them. Furthermore, it includes an on-chain exchange for creating and fulfilling token trade orders. 

The bundled CLI tool and RPC support enable seamless interaction by maintaining an in-memory order book synchronized with blockchain data. The concise logic for order matching ensures efficiency, making `tokenvm` an excellent resource for exploring exchanges and blockchain interactions.

## Status
`tokenvm` is currently in **ALPHA**. It is not production-ready and remains under active development. Expect significant updates as optimizations and audits progress.

## Features

### Token Minting
Create, mint, transfer, and manage user-generated tokens effortlessly. Token owners hold administrative control, allowing them to mint new tokens, update metadata, or transfer ownership. Optimized storage ensures efficient handling, with each balance requiring only 72 bytes of state. 

### On-Chain Trading
Enable seamless trading between custom tokens with on-chain order creation and fulfillment. Offers are managed through an efficient in-memory order book accessible via RPC. Each order entry is compact, requiring 152 bytes of state, and supports parallel execution for enhanced performance.

#### Order Book and Trading
- **In-Memory Order Book:** Maintains trade orders for specified token pairs. 
- **Sandwich Resistance:** Ensures secure order execution without manipulation.
- **Partial Fills and Refunds:** Handles overfills securely, refunding unused inputs.
- **Expiring Fills:** Offers time-scoped transactions for enhanced control.

### Avalanche Warp Messaging (AWM) Support
`tokenvm` leverages Avalanche Warp Messaging for secure, decentralized asset transfers across subnets without trusted intermediaries. The unique design prevents minting exploits and ensures efficient communication. Users can also tip relayers and perform asset swaps during imports.

---

## Demos

### Setting Up Your `tokenvm`
Launch your `tokenvm` Subnet using:
```bash
./scripts/run.sh
```
By default, all funds are allocated to a demo account. For single-subnet testing, run:
```bash
MODE="run-single" ./scripts/run.sh
```

### Building the CLI
Compile the `token-cli` with:
```bash
./scripts/build.sh
```

### Adding Keys and Chains
Import the demo private key and connect chains:
```bash
./build/token-cli key import demo.pk
./build/token-cli chain import-anr
```

### Mint and Trade Tokens
1. **Create an Asset**  
   Use the command below to create a custom asset:
   ```bash
   ./build/token-cli action create-asset
   ```
   The output displays the asset creation details.

Continue exploring the token minting, management, and trading features for a complete understanding of `tokenvm`.


Let me know if you need further adjustments or additions!
