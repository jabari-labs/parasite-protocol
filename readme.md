# parasite protocol

private prediction markets on solana

---

author: francis
version: 1.0
date: february 2026

---

## table of contents

1. what is this
2. the problem
3. the solution
4. how it works
5. architecture
6. privacy layer
7. market types
8. integrations
9. user experience
10. revenue model
11. tech stack
12. roadmap
13. security
14. closing thoughts

---

## what is this

parasite is a prediction market where no one can see your bets.

you can bet on crypto prices, solana ecosystem events, and real-world outcomes. your positions stay private until the market resolves. no one knows what you bet, how much you bet, or which side you took.

the protocol plugs into existing infrastructure - pyth for prices, switchboard for on-chain data, kalshi for real-world events. it feeds off the entire ecosystem. the more solana grows, the more markets parasite can offer.

built on top of seedless wallet for passkey authentication and gasless transactions. no seed phrases. no gas fees. just predictions.

---

## the problem

prediction markets are broken in three ways.

**no privacy**

every major platform exposes your positions publicly. polymarket shows everything on-chain. kalshi requires kyc. drift is fully transparent.

this causes real problems:
- whales get front-run
- everyone copies "smart money" instead of thinking for themselves
- controversial bets can have social or professional consequences
- no true price discovery, just herd behavior

**bad user experience**

current platforms require seed phrases, gas fees, and desktop-first interfaces. onboarding takes minutes when it should take seconds.

**fragmented markets**

different platforms for different bet types. one for crypto, another for real-world events, another for defi metrics. no unified experience.

---

## the solution

parasite fixes all three.

private betting through stealth addresses. your bets are invisible to everyone except you.

simple onboarding through passkey authentication. login with your fingerprint. no seed phrases.

gasless transactions. you never pay fees.

all markets in one place. crypto prices, solana ecosystem metrics, and real-world events.

the name "parasite" reflects how the protocol works. it attaches to existing infrastructure and grows with it. pyth, switchboard, kalshi, seedless - parasite feeds off all of them.

---

## how it works

the flow is simple.

**connect**

open the app. authenticate with your fingerprint or face id. your seedless wallet connects automatically.

**deposit**

deposit usdc. funds route to a stealth address. there is no visible link between your main wallet and your betting wallet.

**bet**

select a market. enter your amount. place the bet. your position is stored encrypted - only you can see it.

**resolution**

when the market resolves, winners get paid to their stealth address. you can withdraw to any wallet or keep funds for more bets.

**privacy maintained**

at no point is there an on-chain trail connecting your identity to your bet.

---

## architecture

three layers.

**client layer**

web app built with next.js. mobile app built with react native. both integrate seedless wallet for authentication and transactions.

**backend layer**

node.js api server. handles market aggregation, encrypted position storage, bet routing, and settlement processing. the backend cannot see your positions - they are encrypted client-side before storage.

**integration layer**

pyth for crypto price feeds. switchboard for custom solana metrics. kalshi api for real-world events.

```
client (web/mobile)
       |
       v
seedless wallet (passkey + stealth + gasless)
       |
       v
parasite api server
       |
       v
pyth / switchboard / kalshi
```

no custom solana program. no audit dependency. ships fast.

---

## privacy layer

parasite uses stealth addresses to hide your bets.

**how stealth addresses work**

you have a meta-address made of two public keys - a scan key and a spend key.

for each bet, a unique stealth address is derived. the derivation is deterministic but the resulting address is unlinkable to your main wallet.

funds sent to your stealth address look like any random address on-chain. no one can connect it to you.

you can spend from the stealth address using a derived private key. still no public link to your identity.

**what stays hidden**

- your bet amount
- your bet direction (yes/no)
- your identity
- your position size
- your win/loss history

only you can see this data.

**what stays public**

- total market volume (healthy markets need visible liquidity)
- current odds (users need to see prices)
- resolution outcomes (verifiable fairness)

**encryption**

positions are encrypted client-side before being sent to the backend. even parasite operators cannot see individual bets. only your passkey can decrypt your data.

---

## market types

three categories at launch.

**crypto prices**

direct price predictions using pyth oracle.

examples:
- will sol be above 200 on march 31
- will btc hit 150k this year
- will eth flip sol in market cap

resolution happens automatically when pyth reports the price at the specified time.

**solana ecosystem**

on-chain metrics using switchboard and direct rpc queries.

examples:
- will jupiter exceed 5b monthly volume
- will marinade tvl reach 2b
- will helium hit 1m hotspots
- will [nft collection] floor exceed 100 sol

resolution uses verifiable on-chain data.

**real-world events**

global events through kalshi integration.

examples:
- federal reserve rate decisions
- election outcomes
- sports results
- economic indicators

resolution uses kalshi's regulated process.

**custom markets (future)**

user-created markets with community resolution. this comes later.

---

## integrations

**seedless wallet**

parasite is built on seedless. this provides:
- passkey authentication (no seed phrases)
- stealth addresses (privacy layer)
- gasless transactions (no fees)
- mobile-first design

the same infrastructure, extended for predictions.

**pyth network**

real-time price feeds for crypto assets. sol, btc, eth, and major solana tokens. used for market creation, real-time odds, and settlement.

**switchboard**

custom oracle data for solana-specific metrics. protocol tvl, dex volumes, validator stats. permissionless oracle creation means any metric can become a market.

**kalshi**

regulated prediction market for real-world events. parasite acts as a private interface to kalshi markets. users bet through parasite, parasite routes to kalshi, settlement flows back privately.

kalshi provides regulatory compliance, deep liquidity, and reliable resolution.

---

## user experience

**onboarding**

download the app or visit the site. tap create account. scan your fingerprint. done.

no seed phrases to write down. no gas fees to worry about. thirty seconds from zero to ready.

**placing a bet**

select a market. see the current odds. enter your amount. see your potential payout. tap to confirm. your fingerprint authorizes the transaction.

your position appears in your private dashboard. no one else can see it.

**managing positions**

view all your active bets. see current value, profit/loss, time remaining. everything encrypted and private.

**withdrawing**

send funds from your stealth address to any wallet. or keep them in parasite for more bets.

---

## revenue model

**fees**

| type | amount | when |
|------|--------|------|
| trading fee | 2% | on winning positions only |
| withdrawal fee | 0.1% | when withdrawing to external wallet |

winners-only fee structure. if you lose, you already lost money - no extra fee. if you win, 2% of your profit goes to the protocol. this aligns incentives.

**future revenue**

premium features like advanced analytics and api access. protocol integrations where other apps license the privacy layer. token with governance and fee sharing comes later - product first.

---

## tech stack

**frontend**

web: next.js, typescript, tailwind css, zustand for state
mobile: react native with expo, typescript
charts: lightweight charts from tradingview
wallet: seedless sdk

**backend**

runtime: node.js with fastify
language: typescript
database: postgresql with encrypted position storage
cache: redis for market data
queue: bullmq for settlement jobs
auth: passkey verification through seedless

**integrations**

pyth sdk for price feeds
switchboard sdk for custom data
kalshi rest api for real-world markets
solana web3.js for blockchain interaction
kora paymaster for gasless transactions

**infrastructure**

frontend on vercel
backend on railway
database on supabase or railway postgres
monitoring with axiom
ci/cd through github actions

---

## roadmap

**phase 1: foundation (weeks 1-3)**

project setup and architecture. backend api scaffolding. pyth integration for price feeds. basic web ui. usdc deposits and withdrawals. first market: sol price prediction.

deliverable: working prototype with one market type.

**phase 2: privacy layer (weeks 4-5)**

stealth address integration from seedless. encrypted position storage. private betting flow. position management ui.

deliverable: private betting functional.

**phase 3: seedless integration (weeks 6-7)**

passkey authentication. gasless transactions. mobile app in react native. seamless wallet experience.

deliverable: full seedless integration with mobile app.

**phase 4: market expansion (weeks 8-10)**

kalshi api integration. real-world event markets. switchboard integration. solana ecosystem markets. more price prediction markets.

deliverable: multi-market platform.

**phase 5: growth (weeks 11+)**

marketing launch. community building. kalshi grant application. additional oracle integrations. custom market creation beta.

deliverable: public launch.

---

## security

**funds safety**

no custom solana program means no smart contract risk. positions are encrypted client-side so the backend cannot steal funds. multiple oracle sources prevent manipulation. kalshi is regulated and well-funded.

**privacy safety**

proper key derivation prevents stealth address linking. no address reuse. encryption at rest with minimal data retention. standard https with no special traffic patterns.

**operational safety**

positions stored safely and can resume after outages. clear resolution rules with oracle-based settlement. kalshi integration provides compliance layer for real-world markets.

---

## closing thoughts

prediction markets work better when positions are private.

true price discovery happens when bets reflect genuine conviction, not herd behavior from copying visible whale positions. parasite makes this possible.

the protocol feeds off existing infrastructure. pyth, switchboard, kalshi, seedless - all of it combined into one private prediction experience. no custom programs. no audit delays. just building on what already works.

privacy is the feature. simplicity is the experience. solana is the foundation.

i built this brick by brick. now it is time to ship.

---

links

github: github.com/francis-codex/parasite-protocol
seedless: github.com/francis-codex/seedless
twitter: @seedless_wallet

---

this document describes a planned protocol. features and timelines may change. prediction markets involve risk of loss. only bet what you can afford to lose.

---

last updated: february 2026
