# La Tanda Chain

[![Cosmos SDK](https://img.shields.io/badge/Cosmos%20SDK-v0.53.6-blue?logo=cosmos)](https://docs.cosmos.network)
[![CometBFT](https://img.shields.io/badge/CometBFT-consensus-purple)](https://cometbft.com/)
[![Testnet](https://img.shields.io/badge/testnet-live-brightgreen)](#testnet-en-vivo)
[![Go](https://img.shields.io/badge/Go-1.24+-00ADD8?logo=go&logoColor=white)](https://go.dev/)
[![License](https://img.shields.io/badge/license-All%20Rights%20Reserved-red)](#license)

Blockchain soberana para servicios financieros descentralizados en Latinoamérica, construida con [Cosmos SDK](https://docs.cosmos.network) v0.53.6 y consenso [CometBFT](https://cometbft.com/).

## Testnet en Vivo

7+ validadores activos, testnet incentivado en progreso. Buscamos operadores de nodos y validadores.

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
| **Min self-delegation** | 1 LTD (testnet — delegation from project) |
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

1. Join [Discord](https://discord.gg/Ve9M2ZSYC2) and share your sync status
2. We'll send you LTD for gas + validator creation
3. Create your validator using the JSON method:

```bash
# Create validator.json
cat > validator.json << 'EOF'
{
  "pubkey": {"@type":"/cosmos.crypto.ed25519.PubKey","key":"YOUR_PUBKEY_HERE"},
  "amount": "1000000ultd",
  "moniker": "your-moniker",
  "commission-rate": "0.10",
  "commission-max-rate": "0.20",
  "commission-max-change-rate": "0.01",
  "min-self-delegation": "1"
}
EOF

# Get your pubkey
latandad tendermint show-validator

# Submit (replace YOUR_KEY with your key name)
latandad tx staking create-validator validator.json \
  --from YOUR_KEY \
  --keyring-backend test \
  --chain-id latanda-testnet-1 \
  --gas auto \
  --gas-adjustment 1.5 \
  --fees 500ultd
```

4. Once active, we'll delegate from the genesis wallet to give you voting power

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

## Incentivized Testnet (March-June 2026)

Validators receive delegation from the project's genesis wallet during testnet. Self-delegation is minimal (1 LTD) — the project delegates voting power to active validators.

| Tier | Requirement | Delegation | Slots |
|------|------------|------------|-------|
| **Full Node** | Run synced node, 95% uptime | 500 LTD | 20 |
| **Validator** | Full node + create validator, 95% uptime | 2,000 LTD | 10 |
| **Infra Partner** | Validator + public RPC/API, explorer, statesync, or guides | 5,000 LTD | 5 |
| **Bug Reporter** | Valid chain/consensus bugs | 100-1,000 LTD | Open |

Join [Discord](https://discord.gg/Ve9M2ZSYC2) or [Telegram](https://t.me/latandahn) to register your node and get started.

## About La Tanda

La Tanda is a live fintech platform in Honduras — tandas (ROSCAs), marketplace, lottery, and AI assistant — with 140+ API endpoints serving real users. The chain migrates core financial logic to native Cosmos modules.

[Platform](https://latanda.online) · [Whitepaper](https://latanda.online/whitepaper.html) · [API Docs](https://latanda.online/docs) · [Dev Portal](https://latanda.online/dev-dashboard.html)

## Community

- **Discord:** https://discord.gg/Ve9M2ZSYC2
- **Telegram:** https://t.me/latandahn
- **Twitter:** https://twitter.com/TandaWeb3
- **Email:** contact@latanda.online

## License

See [LICENSE](LICENSE).
