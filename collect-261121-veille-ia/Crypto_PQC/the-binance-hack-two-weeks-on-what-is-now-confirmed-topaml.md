---
id: collect-261121-veille-ia/crypto-pqc/the-binance-hack-two-weeks-on-what-is-now-confirmed-topaml
title: "The Binance Hack Two Weeks On: What Is Now Confirmed — TopAML"
domain: crypto-pqc
role: reference
task: article
actors: []
dates: []
keywords: ["binance", "october", "about", "billion", "november", "says", "hack", "wallet"]
source: https://topaml.com/blog/binance-hack-two-weeks-later/
source_anchor: ""
source_lines: [1, 87]
section: "Crypto_PQC"
---
Two weeks after the 29 October 2026 hack, three things are confirmed. The FBI says North Korea did it. Binance now puts the loss at about $6.34 billion, of which about $2.85 billion has been frozen or intercepted.<sup>[1]</sup><sup>[2]</sup> Withdrawals reopened in phases from 1 November, but not yet for every token. Binance has published an initial report, not a final post-mortem. *Facts checked on 9 November 2026.*

Our first post covers the first 24 hours: Binance Hack: $6.3B Impact and What Users Should Know. This one covers what changed after that.

## Who did it, according to the FBI

On 31 October the FBI said North Korea was responsible for the theft of more than $6 billion in virtual assets from Binance on or about 29 October. It calls this activity "TraderTraitor", the same name it used for the 2025 Bybit theft.<sup>[1]</sup>

The FBI, the Secret Service and IRS-CI say the thieves are converting stolen assets mainly into ETH. They expect the funds to be laundered further through DeFi and instant swap services. The alert lists 608 Ethereum addresses, 19 Bitcoin addresses and one Solana address. It asks node operators, instant exchanges, DeFi services, analytics firms and other providers to block transactions with, or derived from, those addresses.<sup>[1]</sup>

We found no U.S. Treasury sanctions action tied to this theft as of 9 November.

## What Binance's initial report says

Binance published "Initial Findings and Response Update" on 3 November. It calls the attack a multi-stage campaign and says the investigation is still open and will get an independent review.<sup>[2]</sup>

The timeline, as Binance describes it:

- **June 2026.** A Wallet DevOps engineer downloaded a tampered Homebrew installation while installing an AI coding tool. Binance replaced the device and rotated credentials, but the engineer's role was exposed.
- **September.** Another engineer was approached on a job-hunting platform. After two fake interviews he was told to download an "assignment". It held malicious code that ran on his work computer.
- **20 October.** From that computer the attacker used an internal AI tool to reach two wallet engineers, and found a container deployment token.
- **29 October, 17:15–19:56 UTC.** The attacker deployed a container and launched the attack through an internal wallet signing service.<sup>[2]</sup>

The report also lists outflows from other blockchains: 233.46 million XRP, about 970,500 SOL, 44.2 million OP and 18,270 BTC. On Ethereum the attacker abused EIP-7702 to give its own contract partial control of a hot wallet. Pausing withdrawals did not cancel that on-chain permission. The last transfer, 2,961.9 ETH, settled at 23:56 UTC. Binance cleared the delegation at 00:10 UTC on 30 October.<sup>[2]</sup>

Binance says it found no evidence that wallet private keys leaked.<sup>[3]</sup>

## What the numbers are now

| Figure | 30 October | Now | 
|---|---|---|
| Total impact | about $6.3 billion <sup>[3]</sup> | about $6.34 billion <sup>[2]</sup> | 
| Frozen | about $3 billion <sup>[3]</sup> | about $2.85 billion "frozen or intercepted" <sup>[2]</sup> | 
| Losses covered by Binance | about 80% <sup>[3]</sup> | 98% as of 31 October <sup>[2]</sup> | 

Binance says the frozen figure is subject to reconciliation and price swings.<sup>[2]</sup> It paid for the replacement from part of the SAFU fund, part of its Futures insurance fund and its own treasury. It took a new Proof of Reserves snapshot at 00:00 UTC on 31 October. It says BTC, ETH, USDT, BNB, SOL, USDC, XRP, USD1, RLUSD and ZEC are fully backed.<sup>[4]</sup> The missing 2% is less liquid tokens that Binance is still buying.<sup>[2]</sup>

## Withdrawals

Binance began restoring withdrawals at 06:00 UTC on 1 November. By 12:36 UTC 15 networks and assets were open. Between 06:00 and 15:30 UTC users withdrew $4.649 billion and deposited $1.01 billion.<sup>[5]</sup>

More assets followed on 3 and 8 November.<sup>[6]</sup><sup>[7]</sup> On 6 November Binance said it aimed to reopen HYPE, INJ, APT, PLASMA, NEAR and LTC by the end of the following week.<sup>[8]</sup> On 9 November it reopened four networks for Binance Alpha.<sup>[9]</sup> If your token is still paused, check Binance's network status page, not a stranger's message.

## What the blockchain shows on 9 November

We queried the USDT and USDC contracts again on 9 November (Ethereum block 26,376,505). Both still blacklist the main receiving address, 0x5227f603C1eE34CE3C06eFe6477003BDfCCB2f8c. It still holds about 1.175 billion USDT and 823 million USDC, and they cannot move.<sup>[10]</sup><sup>[11]</sup>

The address that received about 195,460 ETH from it, 0x8532FA14ACAf483729C449b8458aedDb900543D4, is the first address on the FBI list.<sup>[1]</sup> On 9 November it held about 197,460 ETH and had never sent a transaction.<sup>[12]</sup> Nobody can freeze ETH, so that balance can move at any time.

## What is still not known

**Update 13 November.** On 12 November the U.S. Justice Department said it has court authorization to seize or restrain more than $1.2 billion of the stolen crypto, out of more than $2.7 billion frozen so far by government and private-sector efforts.<sup>[13]</sup> That is not yet a forfeiture; see DOJ moves to seize $1.2B stolen in the Binance hack.

**Update 19 November.** On 17 November the attackers' Ethereum wallets moved again after more than two weeks without activity. One address that had been idle since 30 October sent 34,279.66 ETH (about $63 million) to three new addresses; investigators MistTrack and Solid Intel spotted it, and CoinDesk reported that the new wallets were not linked to an exchange or a laundering service at that point.<sup>[14]</sup> Decrypt, citing Arkham, reported a second wallet moving another 40,600 ETH within about three hours, 74,880 ETH or roughly $137 million in total, and said some of it was starting to be swapped for bitcoin through THORChain.<sup>[15]</sup> If you receive ETH or BTC from strangers in the coming weeks, an AML check before you accept it matters more than usual.

Binance has not published its final post-incident report or the independent review.<sup>[16]</sup> It has not said how much of the $2.85 billion it expects to get back, or how. The FBI has not said where the funds stolen on Bitcoin, Solana, XRP Ledger and Optimism have gone.

## What to do

1. **Expect these coins in circulation for months.** The FBI says laundering through DeFi and instant swaps is the next step.<sup>[1]</sup> Funds that pass through them can reach a P2P buyer many hops later.
2. **Check an incoming address before you accept a large payment** , especially ETH and BTC. A good checker flags stolen funds and FBI-listed addresses. See how to check USDT before you receive it.
3. **Read the address yourself.** Etherscan labels the main receiving address as linked to this hack; see how to read an address on Etherscan and Tronscan.
4. **If an exchange holds your deposit** , answer its questions with documents. See why exchanges hold deposits.
5. **Use only Binance's official channels.** Binance asks users to rely on them for verified updates.<sup>[3]</sup> Ignore "compensation forms" and recovery offers from strangers.

We list the flagged addresses in our hacks and freezes tracker.

General information, not legal or financial advice. Rules differ by country and change; check the linked sources.

## Sources

1. FBI — North Korea Responsible for $6 Billion+ Binance Attack (31 Oct 2026). Checked 19 Nov 2026.
2. Binance (official X account) — 29 October Binance Security Incident: Initial Findings and Response Update (3 Nov 2026). Checked 19 Nov 2026.
3. Binance (official X account) — Security Incident: Recovery Update #2 (30 Oct 2026). Checked 19 Nov 2026.
4. Binance (official X account) — Security Incident: Update #3: 90% of Stolen Assets Replenished; Proof of Reserves Snapshot Provided (31 Oct 2026). Checked 19 Nov 2026.
5. Binance (official X account) — Withdrawals restored for 15 networks and assets; first-day volumes (1 Nov 2026). Checked 19 Nov 2026.
6. Binance (official X account) — Latest Withdrawal Update: ETH and SOL tokens re-enabled (3 Nov 2026). Checked 19 Nov 2026.
7. Binance (official X account) — Latest Withdrawal Update: HYPE and APT re-enabled (8 Nov 2026). Checked 19 Nov 2026.
8. Binance (official X account) — Update: working to reopen HYPE, INJ, APT, PLASMA, NEAR, LTC (6 Nov 2026). Checked 19 Nov 2026.
9. Binance (official X account) — Latest Withdrawal Update: networks re-enabled for Alpha (9 Nov 2026). Checked 19 Nov 2026.
10. Etherscan — Tether USD (USDT) contract: isBlackListed and balanceOf (9 Nov 2026). Checked 19 Nov 2026.
11. Etherscan — USD Coin (USDC) contract: isBlacklisted and balanceOf (9 Nov 2026). Checked 19 Nov 2026.
12. Etherscan — Address 0x8532FA14…0543D4 (9 Nov 2026). Checked 19 Nov 2026.
13. U.S. Department of Justice — Justice Department Announces Actions to Disrupt North Korea's Massive Theft of Cryptocurrency (12 Nov 2026). Checked 19 Nov 2026.
14. CoinDesk — Binance hacker moves $63 million in stolen ether after 2 weeks of silence (17 Nov 2026). Checked 19 Nov 2026.
15. Decrypt — Binance Hack Wallets Move $137 Million in Ethereum After Two Weeks of Silence (17 Nov 2026). Checked 19 Nov 2026.
16. Binance (official X account) — Security Incident: Recovery Update #4: Withdrawals Resume in Phases (1 Nov 2026). Checked 19 Nov 2026.
