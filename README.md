# La Tanda Chain

Blockchain soberana para servicios financieros descentralizados en Latinoamérica, construida con [Cosmos SDK](https://docs.cosmos.network) v0.53.6 y consenso [CometBFT](https://cometbft.com/).

## Testnet en Vivo

El nodo génesis está produciendo bloques. Buscamos operadores de nodos y validadores.

```bash
# One-line install (Ubuntu 22.04+)
wget -q https://latanda.online/chain/node-setup.sh -O node-setup.sh && chmod +x node-setup.sh && ./node-setup.sh
```

## Chain Details

| | |
|---|---|
| **Chain ID** | `latanda-testnet-1` |
| **Framework** | Cosmos SDK v0.53.6 / CometBFT |
| **Block time** | ~5 seconds |
| **Total supply** | 200,000,000 LTD (fixed, 0% inflation) |
| **Token denom** | `ultd` (1 LTD = 1,000,000 ultd) |
| **Address prefix** | `ltd` |
| **Min validator stake** | 50,000 LTD |
| **Binary** | `latandad` |

## Endpoints

| Service | URL |
|---------|-----|
| RPC | https://latanda.online/chain/rpc/ |
| REST API | https://latanda.online/chain/api/ |
| P2P | `483a8110c3cd93c8dd3801d935151e98656f5b67@168.231.67.201:26656` |
| gRPC | `168.231.67.201:9090` |
| Explorer | https://latanda.online/chain/ |
| Genesis | https://latanda.online/chain/genesis.json |

## Quick Start

### Hardware Requirements

- **Minimum:** 2 CPU, 4GB RAM, 50GB SSD
- **Recommended:** 4 CPU, 8GB RAM, 100GB SSD
- Cost: ~$5-10/month on any VPS provider

### Automated Install

```bash
wget -q https://latanda.online/chain/node-setup.sh -O node-setup.sh
chmod +x node-setup.sh
./node-setup.sh
```

The script installs Go, builds from source, initializes the node, downloads genesis, and starts the node via PM2.

### Manual Install

```bash
# Install Go 1.24+
wget https://go.dev/dl/go1.24.1.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.24.1.linux-amd64.tar.gz
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin

# Clone and build
git clone https://github.com/INDIGOAZUL/la-tanda-chain.git
cd la-tanda-chain
go build -o latandad ./cmd/latandad

# Initialize
./latandad init my-node --chain-id latanda-testnet-1

# Download genesis
wget -O ~/.latanda/config/genesis.json https://latanda.online/chain/genesis.json

# Set seed peer in ~/.latanda/config/config.toml
# seeds = "483a8110c3cd93c8dd3801d935151e98656f5b67@168.231.67.201:26656"

# Start
./latandad start
```

### Verify Sync

```bash
latandad status | jq '.sync_info.catching_up'
# Should return "false" when fully synced
```

## Becoming a Validator

After your node is synced:

```bash
# Create validator (requires 50K LTD — request testnet tokens in Discord)
latandad tx staking create-validator \
  --amount 50000000000ultd \
  --pubkey $(latandad tendermint show-validator) \
  --moniker "your-moniker" \
  --chain-id latanda-testnet-1 \
  --commission-rate 0.10 \
  --commission-max-rate 0.20 \
  --commission-max-change-rate 0.01 \
  --min-self-delegation 1 \
  --from your-key-name \
  --gas auto \
  --gas-adjustment 1.5
```

## Useful Commands

```bash
# Check node status
latandad status | jq '.sync_info'

# Check latest block
latandad status | jq '.sync_info.latest_block_height'

# Query balance
latandad query bank balances <address>

# Query validators
latandad query staking validators

# Send tokens
latandad tx bank send <from> <to> <amount>ultd --chain-id latanda-testnet-1
```

## Incentivized Testnet

We're running an incentivized testnet program (March-June 2026) with LTD rewards for node operators, validators, and infrastructure partners. See [Incentivized Testnet Program](https://latanda.online/chain/) for details.

## About La Tanda

La Tanda is a live fintech platform serving users in Honduras with:
- **Tandas** (ROSCAs) — rotating savings groups
- **Marketplace** — peer-to-peer services and products
- **Lottery** — community lottery system
- **AI Assistant** (MIA) — financial guidance

The chain migrates core financial logic to native Cosmos modules (`x/tanda`, `x/mercado`, `x/loteria`, `x/ltd`, `x/mineria`, `x/gobernanza`).

- **Platform:** https://latanda.online
- **Whitepaper:** https://latanda.online/whitepaper.html
- **API Docs:** https://latanda.online/docs
- **Dev Portal:** https://latanda.online/dev-dashboard.html

## Community

- **Discord:** https://discord.gg/latanda
- **Telegram:** https://t.me/latandahn
- **Twitter:** https://twitter.com/TandaWeb3
- **Email:** contact@latanda.online

## License

Copyright 2026 Ray-Banks LLC. All rights reserved.
