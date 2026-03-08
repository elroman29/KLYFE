# KayaLyfe ($KLYFE) Minting

This project contains the scripts and instructions to create, mint, and add metadata to the $KLYFE Solana token.

## Process Overview

1.  **Environment Setup**: Install Solana Tools and Node.js.
2.  **Token Creation**: Run a script to create the token on the Solana Testnet.
3.  **Metadata Addition**: Run a script to add the token name, symbol, and logo so it appears correctly in wallets.

---

## Step 1: Environment Setup

### 1. Install Solana Tool Suite

You need the Solana CLI to interact with the Solana network from your terminal.

```bash
sh -c "$(curl -sSfL https://release.solana.com/v1.18.17/install)"
```

After installation, add the Solana CLI to your system's PATH. The installer will provide the command, which will look something like this (for macOS with zsh):

```bash
export PATH="/Users/your-username/.local/share/solana/install/active_release/bin:$PATH"
```

Close and reopen your terminal, then verify the installation:

```bash
solana --version
spl-token --version
```

### 2. Install Node.js & Yarn

We'll use Node.js and TypeScript to handle the metadata. If you don't have Node.js (v18+) and yarn installed, you can get them from [nodejs.org](https://nodejs.org/) and [yarnpkg.com](https://yarnpkg.com/getting-started/install).

---

## Step 2: Prepare Your Assets

### 1. The Minting Wallet

The process will generate a new wallet for you at `mint-authority.json`. This wallet will be the authority for your new token. **It is critical to back up this file**, as it contains the private key that controls your token. Losing it means losing control.

### 2. Token Logo

Place your token logo in the `assets/` directory. It should be a PNG file. For this project, let's name it `kaya.png`.

---

## Step 3: Create and Mint the Token

The `mint-kaya.sh` script will handle the entire token creation process.

### Configuration

Before running the script, open `mint-kaya.sh` and you can review the configuration variables at the top. The defaults are set for $KLYFE.

-   `RECIPIENT_WALLET`: The address that will receive the first batch of tokens. It's pre-filled with the Testnet address you provided.
-   `TOKEN_SUPPLY`: The total number of tokens to mint. It's set to 1 billion.

### Run the script

Open your terminal and run the script:

```bash
bash mint-kaya.sh
```

This script will:
1.  Set your Solana CLI configuration to use the **Testnet**.
2.  Create a new wallet (`mint-authority.json`) if it doesn't exist.
3.  Airdrop 2 Testnet SOL to this wallet for fees.
4.  Create the $KLYFE token "mint".
5.  Create a token account for the recipient wallet.
6.  Mint the total supply to the recipient's token account.
7.  Disable future minting, fixing the total supply.

At the end, it will output the **Token Mint Address**. **SAVE THIS ADDRESS!** You will need it for the next step.

---

## Step 4: Add Token Metadata

Now we will add the name, symbol, and image to your token.

### 1. Install Dependencies

Navigate to the `metadata` directory and install the required Node.js packages.

```bash
cd metadata
yarn install
```

### 2. Configure Metadata

This project uses a service called `bundlr.network` to upload metadata to Arweave, a permanent data storage solution. To use it, you need to fund a special account on their network with some Testnet SOL.

First, get your mint authority address:
```bash
solana address -k ../mint-authority.json
```

Then, from the `metadata` directory, run the following command to see how much SOL is in your bundlr account (it will be 0).
```bash
yarn ts-node add_metadata.ts balance
```

Now, fund your bundlr account with at least 0.05 Testnet SOL.
```bash
yarn ts-node add_metadata.ts fund
```
*Note: This command will ask you to confirm the transaction to send SOL to bundlr.*


### 3. Upload and Attach Metadata

Now, run the final command. Replace `<YOUR_TOKEN_MINT_ADDRESS>` with the address you saved from Step 3.

```bash
yarn ts-node add_metadata.ts upload-and-set -t <YOUR_TOKEN_MINT_ADDRESS>
```

This script will:
1.  Upload your `kaya.png` and a generated `metadata.json` file to Arweave via Bundlr.
2.  Create the official Metadata Account for your token on the Solana blockchain, pointing to the Arweave URL.

After a few moments, your token should appear correctly with its name, symbol, and logo in Phantom and other Solana wallets on the Testnet! 
