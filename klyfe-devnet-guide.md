# $KLYFE Token Launch — Devnet Quick Reference
## Kaya Lyfe Ecosystem | Delco Collective LLP
### SPL Token-2022 | 9 Decimals | 1,000,000,000 Fixed Supply

---

## What These Scripts Do

| Script | Purpose |
|--------|---------|
| `launch-klyfe-devnet.sh` | Steps 1–3: keypair → mint → ATA → initial tokens |
| `klyfe-allocate-pools.sh` | Mints full 1B supply across 6 tokenomics pools |

---

## Quick Start

```bash
# Step 1: Make scripts executable
chmod +x launch-klyfe-devnet.sh klyfe-allocate-pools.sh

# Step 2: Launch with your existing keypair (optional)
./launch-klyfe-devnet.sh ~/.config/solana/klyfe/mint-authority.json

# Or launch and let it generate a new keypair automatically
./launch-klyfe-devnet.sh

# Step 3: Allocate to tokenomics pools (after Step 2 completes)
./klyfe-allocate-pools.sh
```

---

## Step-by-Step Breakdown

### Step 1 — InitializeAccount (Mint Authority)
Your keypair becomes the **mint authority** — the wallet that controls
$KLYFE minting on Solana. This keypair is saved at:
```
~/.config/solana/klyfe/mint-authority.json
```
The public key of this wallet is the **mint authority address**.

### Step 2 — Create Token Mint
Creates the $KLYFE token on Solana using **Token-2022** (the modern
SPL standard with metadata extensions).

Key parameters:
- `--program-id TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb` — Token-2022
- `--decimals 9` — matches whitepaper spec
- `--enable-metadata` — on-chain name/symbol storage

On-chain metadata set:
- Name: `Kaya Lyfe`
- Symbol: `KLYFE`
- URI: `https://kaya.io/token/klyfe-metadata.json`

### Step 3 — Create ATA + Mint Tokens
Creates your **Associated Token Account (ATA)** — the wallet address
that holds your $KLYFE balance — then mints the devnet test supply.

```
Mint Address  ──owns──►  Token Mint (KLYFE definition)
                                │
                         ◄──holds──
                       Your ATA (balance holder)
                                │
                       ◄──controlled by──
                     Mint Authority (your keypair)
```

---

## Tokenomics Allocation (Per Whitepaper)

| Pool | Allocation | Tokens | Vesting |
|------|-----------|--------|---------|
| Customer Rewards | 40% | 400,000,000 | Drip release via smart contract |
| Team & Advisors | 30% | 300,000,000 | 12-mo cliff + 36-mo linear vest |
| Ecosystem & Treasury | 10% | 100,000,000 | Platform-governed release |
| Strategic Investors | 10% | 100,000,000 | Utility-aligned incentives |
| Retailer Incentives | 5% | 50,000,000 | POS/compliance rewards |
| Clinical Incentives | 5% | 50,000,000 | Patient data contributions |
| **TOTAL** | **100%** | **1,000,000,000** | **Fixed — no future minting** |

---

## Essential Commands

```bash
# Load your saved config
source ~/.config/solana/klyfe/klyfe-config.env

# Check your $KLYFE balance
spl-token balance $KLYFE_MINT

# Check total circulating supply
spl-token supply $KLYFE_MINT

# View full mint details (authority, supply, decimals)
spl-token display $KLYFE_MINT

# List all your token accounts
spl-token accounts

# Mint additional tokens (devnet only)
spl-token mint $KLYFE_MINT 1000000

# Transfer to another wallet
spl-token transfer $KLYFE_MINT 500 <RECIPIENT_WALLET_ADDRESS>

# Check SOL balance
solana balance

# Get devnet SOL if needed
solana airdrop 1
```

---

## Explorer Links (Devnet)

After running the scripts, verify on Solana Explorer:

```
# Token Mint
https://explorer.solana.com/address/<KLYFE_MINT>?cluster=devnet

# Your Token Account
https://explorer.solana.com/address/<YOUR_ATA>?cluster=devnet

# Mint Authority Wallet
https://explorer.solana.com/address/<MINT_AUTHORITY>?cluster=devnet
```

Replace `<KLYFE_MINT>` etc. with actual addresses from your config:
```bash
cat ~/.config/solana/klyfe/klyfe-config.env
```

---

## Files to Back Up

```
~/.config/solana/klyfe/
├── mint-authority.json        ⚠️  CRITICAL — Your private key
├── klyfe-config.env           Token addresses and config
└── pool-wallets/              Pool wallet keypairs (after allocation)
    ├── rewards-pool.json
    ├── team-advisors.json
    ├── ecosystem-treasury.json
    ├── strategic-investors.json
    ├── retailer-incentives.json
    └── clinical-incentives.json
```

**Never commit these files to git. Never share them.**

---

## Troubleshooting

| Issue | Fix |
|-------|-----|
| `Airdrop failed` | Wait 60s, retry; or use https://faucet.solana.com |
| `Insufficient funds` | Run `solana airdrop 1` 2-3 times |
| `Account already exists` | OK — script uses existing account |
| `RPC timeout` | Retry; check https://status.solana.com |
| `Metadata init fails` | CLI version issue — metadata set separately |
| Wrong network | Run `solana config set --url https://api.devnet.solana.com` |

---

## Mainnet Checklist (Before Launch)

- [ ] Squads Protocol 2-of-3 multisig replaces single keypair authority
- [ ] CertiK / OpenZeppelin smart contract audit completed
- [ ] Vesting contract deployed for Team & Advisors pool
- [ ] Token metadata JSON hosted at `https://kaya.io/token/klyfe-metadata.json`
- [ ] KYC/AML framework in place for investor pool access
- [ ] DAO governance module deployed
- [ ] Exchange listing agreements signed
- [ ] $DELCO parallel token minted and connected to ecosystem

---

*$KLYFE Token — Kaya Lyfe Ecosystem | Delco Collective LLP | Devnet*
