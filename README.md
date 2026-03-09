# DevCredential dApp

A React + Vite frontend for the **IntelligentDevCredential** GenLayer intelligent contract.

## Stack

| Layer      | Tech                          |
|------------|-------------------------------|
| Framework  | React 18 + Vite               |
| Blockchain | GenLayer (`genlayer-js` SDK)  |
| Wallet     | MetaMask / MetaMask Flask     |
| Styling    | Plain CSS (design tokens)     |

## Project structure

```
src/
├── lib/
│   └── genlayer.js          # GenLayer client, wallet helpers, callRead/callWrite
├── hooks/
│   ├── useWallet.js         # MetaMask connection state
│   └── useCredential.js     # Fetch a single credential by wallet
├── components/
│   ├── Header.jsx           # Sticky header + nav + wallet button
│   ├── CredentialCard.jsx   # Credential display card
│   ├── Icons.jsx            # All SVG icons
│   └── Spinner.jsx          # Loading spinner
├── pages/
│   ├── VerifyPage.jsx       # Verify GitHub + issue credential
│   ├── LookupPage.jsx       # Look up any wallet's credential
│   ├── LeaderboardPage.jsx  # Ranked leaderboard
│   └── AnalyticsPage.jsx    # Aggregated stats + formula
├── styles/
│   └── global.css           # Design tokens + all shared styles
├── App.jsx                  # Root: tab routing
└── main.jsx                 # Entry point
```

## Setup

```bash
# 1. Install dependencies
npm install

# 2. Configure environment
cp .env.example .env
# Edit .env:
#   VITE_CONTRACT_ADDRESS=0x...   ← your deployed contract
#   VITE_LEADERBOARD_API=         ← optional indexer URL

# 3. Start dev server
npm run dev

# 4. Build for production
npm run build
```

## Wallet requirements

- **Testnet (Asimov):** MetaMask with the GenLayer Asimov network added
- **Local dev:** MetaMask Flask + GenLayer Studio running on localhost

## Network configuration

The app targets `testnetAsimov` by default. To switch networks, edit `src/lib/genlayer.js`:

```js
// For local Studio dev:
import { localnet } from "genlayer-js/chains";
// then: chain: localnet

// For testnet:
import { testnetAsimov } from "genlayer-js/chains";
// then: chain: testnetAsimov
```

## Leaderboard / Analytics

These tabs require an off-chain indexer because the contract stores credentials in a
`mapping(wallet → credential)` with no built-in iteration. Until you have an indexer,
the app falls back to mock data automatically. To connect a real source set
`VITE_LEADERBOARD_API` in your `.env` to a JSON endpoint returning an array of credential objects.
