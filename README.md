# Raffle Smart Contract

A Solana-based smart contract system for conducting NFT raffles. Participants can purchase tickets using SOL, BOOGA, or ZION tokens. Winners can either receive the raffle NFT as a reward or purchase NFTs at half price.

## Overview

This project implements a decentralized raffle system on the Solana blockchain, enabling creators to host NFT raffles with flexible payment options. The system supports multiple token types for ticket purchases and provides automated winner selection and reward distribution.

## Features

- **Multi-token Support**: Purchase raffle tickets using SOL, BOOGA, or ZION tokens
- **Flexible Rewards**: Winners can receive NFTs or purchase them at a discounted rate
- **Automated Winner Selection**: Built-in random winner selection mechanism
- **Time-based Raffles**: Configurable start and end times for each raffle
- **Whitelist Support**: Optional whitelist functionality for exclusive raffles
- **Creator Controls**: Raffle creators can update periods and withdraw NFTs if no tickets are sold

## Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** (v14 or higher)
- **Yarn** package manager
- **TypeScript** and **ts-node** (install globally: `npm install -g typescript ts-node`)
- **Solana CLI** tools
- **Anchor Framework** (Solana development framework)

## Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd raffle-smart-contract
```

2. Install dependencies:
```bash
cd raffle
yarn install
```

3. Configure your Solana wallet:
   - Ensure your Solana wallet keypair is available
   - Update the `ANCHOR_WALLET` environment variable in `package.json` to point to your wallet keypair file
   - Default test path: `~/.config/solana/id.json` (modify as needed)

## Project Structure

```
raffle/
├── cli/
│   ├── scripts.ts      # Main script source for all functionality
│   ├── types.ts        # Program account type declarations
│   └── raffle.json     # IDL for JavaScript bindings
├── programs/            # Solana program source code
├── tests/              # Test files
└── migrations/         # Anchor migration scripts
```

## Usage

### Running Scripts

The main functionality is implemented in `/cli/scripts.ts`. To use the CLI:

1. Modify the commands in the main function of `scripts.ts` to call the desired functions
2. Ensure the `ANCHOR_WALLET` environment variable is correctly set in `package.json`
3. Run the script:
```bash
yarn ts-node
```

## Smart Contract Functions

### Contract Owner Operations

#### Initialize Project
For first-time use, the contract owner must initialize the smart contract to allocate global accounts.

```typescript
initProject()
```

### Raffle Creator Operations

#### Create Raffle
Creates a new raffle and transfers the NFT to the Program Derived Address (PDA). The raffle data is stored on-chain.

```typescript
createRaffle(
    userAddress: PublicKey,
    nft_mint: PublicKey,
    ticketPriceSol: number,
    ticketPriceBooga: number,
    ticketPriceZion: number,
    endTimestamp: number,
    winnerCount: number,
    whitelisted: number,
    max: number
)
```

**Parameters:**
- `userAddress`: Public key of the raffle creator
- `nft_mint`: Public key of the NFT mint address
- `ticketPriceSol`: Ticket price in SOL (lamports)
- `ticketPriceBooga`: Ticket price in BOOGA tokens
- `ticketPriceZion`: Ticket price in ZION tokens
- `endTimestamp`: Unix timestamp for raffle end time
- `winnerCount`: Number of winners to select
- `whitelisted`: Whitelist configuration flag
- `max`: Maximum number of tickets that can be sold

#### Update Raffle Period
Allows the creator to modify the end time of an existing raffle.

```typescript
updateRafflePeriod(
    userAddress: PublicKey,
    nft_mint: PublicKey,
    endTimestamp: number
)
```

#### Withdraw NFT
Enables the creator to withdraw the NFT from the PDA if no tickets were sold and the raffle end time has passed.

```typescript
withdrawNft(
    userAddress: PublicKey,
    nft_mint: PublicKey
)
```

### Participant Operations

#### Buy Ticket
Allows users to purchase raffle tickets. Payment is sent to the raffle creator in SOL and/or tokens.

```typescript
buyTicket(
    userAddress: PublicKey,
    nft_mint: PublicKey,
    amount: number
)
```

**Parameters:**
- `userAddress`: Public key of the ticket buyer
- `nft_mint`: Public key of the NFT mint address for the raffle
- `amount`: Number of tickets to purchase

#### Reveal Winners
Reveals the selected winners for a completed raffle.

```typescript
revealWinner(
    userAddress: PublicKey,
    nft_mint: PublicKey
)
```

### Winner Operations

#### Claim Reward
Allows winners to claim their rewards (NFT or discounted purchase option).

```typescript
claimReward(
    userAddress: PublicKey,
    nft_mint: PublicKey
)
```

## Development

### Building the Program

```bash
anchor build
```

### Running Tests

```bash
anchor test
```

### Deploying

```bash
anchor deploy
```

## Security Considerations

- Always verify the smart contract code before interacting with deployed instances
- Ensure proper keypair management and never commit private keys
- Test thoroughly on devnet before deploying to mainnet
- Review all transaction parameters before signing

## Contributing

Contributions are welcome! Please ensure that:
- Code follows the existing style guidelines
- All tests pass before submitting
- New features include appropriate tests
- Documentation is updated for any API changes

## License

[Specify your license here]

## Support

For issues, questions, or contributions, please open an issue on the repository.
