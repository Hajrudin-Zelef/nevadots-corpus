---
id: collect-261121-veille-ia/crypto-pqc/binance-security-incident-full-picture-6-3-billion-destination-confirmed-facts-and-withdrawal-status
title: "Binance Security Incident Full Picture: $6.3 Billion Destination, Confirmed Facts, and Withdrawal Status"
domain: crypto-pqc
role: reference
task: article
actors: []
dates: []
keywords: ["billion", "binance", "approximately", "incident", "security", "withdrawal", "been", "full"]
source: https://www.odaily.news/en/post/5213683
source_anchor: ""
source_lines: [1, 119]
section: "Crypto_PQC"
---
# Binance Security Incident Full Picture: $6.3 Billion Destination, Confirmed Facts, and Withdrawal Status

- **Core Viewpoint:** Binance's hot wallet saw approximately $6.3 billion transferred out, making it the largest publicly recorded crypto theft. The attacker did not obtain private keys, but instead took control of the signing process through EIP-7702 delegated authorization and compromise of an operations terminal, exposing risks in signing infrastructure; approximately $3 billion has been frozen but not yet recovered.
- **Key Elements:**
  1. The incident involves ten chains, with Ethereum accounting for approximately $3.79 billion as the largest portion; 18,270 BTC worth approximately $1.47 billion, and Tron approximately $628 million.
  2. TRM Labs counted approximately $2.7 billion frozen, while Binance reported approximately $3 billion, accounting for about 48% of the affected amount, almost entirely in centralized stablecoins.
  3. Before the freeze, the attacker sold approximately 325 million USDT at a discount of over 10% in exchange for approximately 122,300 ETH, with the remaining approximately $2.4 billion concentrated in BTC and ETH.
  4. Binance stated that no private key leakage was found, and the entry point points to a compromised DevOps engineer's laptop; authorizations related to the involved addresses have been revoked.
  5. TRM Labs attributed the incident with high confidence to the North Korean hacker group TraderTraitor, with methods similar to the Bybit and KelpDAO incidents, though no law enforcement determination has yet been made.
  6. Withdrawals have been suspended since October 29 at 22:15 UTC and are expected to begin resuming within 48 hours; Binance stated that all losses will be covered by the SAFU fund and company funds.
  7. The market dropped to a six-week low that evening, with Bitcoin hitting a low of approximately $76,650; leveraged liquidations across the market within 24 hours totaled approximately $1.4 billion.

## Overview

Binance suffered a security incident on October 29, 2026 in which funds were transferred out of its hot wallets. The official estimate puts the affected amount at approximately $6.3 billion, making it the largest crypto asset theft on public record. As of 07:00 UTC on October 30, withdrawals remained suspended; Binance said it expects to begin restoring them within 48 hours, while deposits have returned to normal operation per its latest service update. About $3 billion of the relevant assets have been frozen on-chain, and there is no public record of any assets being recovered and returned to the exchange. Binance has stated that it will cover all losses through the SAFU fund and company funds.

The technical pathway differs from most exchange theft cases. The transfers on Ethereum were carried out through an EIP-7702 delegation authorization; the attacker did not obtain the wallet's private keys, but instead had the hot wallet execute an asset sweep according to a contract they deployed. In its preliminary investigation, Binance said it found no evidence of private key leakage and pointed to a compromised DevOps engineer laptop as the entry point.

## Key Takeaways

## Incident Timeline

On-chain records, research firm assessments, and official statements advanced at different speeds in this incident. The table below is arranged in a unified time zone and labels the nature of each entry. All times are UTC, corresponding to Singapore time plus eight hours.

Two threads deserve separate attention. First, the issuers' freezing actions preceded the exchange's service suspension: Tether and Circle completed their first round of freezes within about forty minutes of the first large sweep, while the withdrawal suspension occurred at 22:15 UTC. Second, the wording from Binance executives shifted gradually; co-CEO Yi He used the word "attack" for the first time in a post at 00:24 UTC on October 30, whereas the company's main account had used "potential security issue" in earlier announcements.

## Amount Figures: The Difference Between Transferred Out, Frozen, Unfrozen, and Recovered

Publicly circulating figures range from $3.3 billion to $6.36 billion. The discrepancies stem from three sources: different chains covered by the statistics, different valuation points in time, and whether the statistic refers to the amount transferred out or the attacker wallets' holdings balance.

### Distribution of Frozen Assets

TRM Labs' statistics as of 00:30 UTC on October 30 break down the frozen amount as approximately $1.77 billion USDT from Tether, approximately $825 million USDC from Circle, 40.5 million RLUSD from Ripple, 27.8 million USD1 from World Liberty Financial, and 8,426 ETH and 74.6 million ARB handled by the Arbitrum Security Council, totaling approximately $2.7 billion. Binance reported a verified frozen amount of approximately $3 billion twenty-two minutes later; the difference between the two stems from the timing of the statistics rather than a conflict in methodology. First Digital froze the involved FDUSD at 02:21 UTC without disclosing the specific amount, and that portion is not included in the above breakdown.

### Frozen Does Not Equal Recovered

Frozen assets are in a blacklisted state on the issuer's contract; they are neither in the attacker's hands nor back on the exchange's books. For assets to return to Binance, the issuer must re-mint them or transfer them under judicial proceedings. This step took months in the February 2025 Bybit incident, where only $48.4 million of the $1.5 billion stolen was ultimately recovered. By Binance's figures, the currently frozen amount represents about 48% of the affected total, while the remainder is concentrated in Bitcoin and Ether, two assets with no centralized issuer and therefore no equivalent freezing pathway.

### The Attacker's Cash-Out Moves Before the Freezes Took Effect

TRM Labs recorded that before the issuers' freezes took effect, the attacker sold approximately $325 million USDT to market makers at a discount of over 10% in exchange for approximately 122,300 ETH, borrowed ETH against USDT collateral on Aave, and moved assets through lending protocols on the Tron chain and via Uniswap. This series of moves converted freezable centralized stablecoins into native assets that cannot be frozen, which is the direct reason the remaining $2.4 billion is concentrated in BTC and ETH.

## Attack Path

### EIP-7702 Delegation on Ethereum

EIP-7702 went live with Ethereum's Pectra upgrade in May 2025 and allows an ordinary account to authorize a smart contract to execute operations on its behalf. According to analysis by TRM Labs and Blockaid, the delegation transaction submitted by the attacker contained 50 pre-signed authorizations corresponding to 50 consecutive transaction nonces, ensuring that one of them would be in a valid state at execution. Once the authorization took effect, Binance's hot wallet completed the sweep according to the attacker-deployed contract logic, with the entire process finishing within about three and a half minutes after the delegation took effect. The relevant contracts were deployed earlier that day, and the associated addresses' initial funding came from small withdrawals from a mixing service.

This mechanism explains how "no private key leakage found" and "wallet drained" can both be true. The delegation authorization itself requires a valid signature from the wallet, but the attacker only needed the signing request to be approved, without touching the private key itself. Blockaid accordingly characterized the incident as key misuse of the signing infrastructure.

### The Transfers on the Other Nine Chains Cannot Be Explained by the Same Mechanism

TRM Labs' tally of transfers covers ten chains: approximately $3.79 billion on Ethereum, including 200,630 ETH and approximately $1.875 billion USDT; 18,270 BTC, approximately $1.47 billion; approximately $628 million on Tron; 2,334,600 XRP on the XRP Ledger, approximately $296 million; 970,500 SOL on Solana, approximately $96 million; approximately $39 million combined on Arbitrum, Optimism, and Base; approximately $28 million on Avalanche; and approximately $10 million on Polygon. Apart from Ethereum, the transfers on all other chains were conventional signed transfers, and EIP-7702 does not apply on those chains. TRM Labs therefore concluded that the more likely scenario is that the hot wallet signing infrastructure as a whole was compromised.

### Official Preliminary Conclusion

In its statement at 02:25 UTC on October 30, Binance said its preliminary investigation indicated the incident may have involved a compromised DevOps engineer laptop, through which the attacker entered a service container and submitted unauthorized withdrawal transactions, while stating that no evidence of any wallet private key leakage was found. Binance said it had revoked the authorizations of the involved addresses and would update its proof-of-reserves data after remediation is complete, with a full technical post-mortem to be published separately. As of 07:00 UTC on October 30, that post-mortem had not yet been released.

### Attribution

TRM Labs attributed the attack with high confidence to the North Korean hacking group TraderTraitor, based on on-chain and off-chain intelligence as well as similarities to the February 2025 Bybit incident and the April 2026 KelpDAO incident in terms of small mixing-service funding and pre-signing techniques. On-chain investigator ZachXBT and Blockaid also point toward North Korea. There is currently no formal determination by law enforcement.

## Practical Impact on Users

### Service Status

Binance suspended withdrawals at 22:15 UTC on October 29 and suspended deposits at 00:07 UTC on October 30. In a service update at 04:17 UTC on October 30, co-CEO Yi He said withdrawals would remain suspended until the team completes security upgrades to the withdrawal service and related addresses, with restoration expected to begin within 48 hours—a window corresponding to around 04:17 UTC on November 1; the same update said deposits were unaffected and operating normally. In its announcements at 00:07 UTC and 02:25 UTC, Binance had listed deposits as suspended, differing from the 04:17 UTC statement, and the company did not explain the discrepancy. As of 07:00 UTC on October 30, the withdrawal disruption had lasted about nine hours; if restoration is completed within the 48-hour window, the total disruption from the time of suspension would be approximately 54 hours. No suspension announcements were seen for spot or futures trading during this period.

### Loss Coverage

In its announcements, Binance said it would cover all losses through the SAFU fund and company funds and stated that user assets are safe. Founder Changpeng Zhao said on social media that as a shareholder he expects user funds to be fully covered and that the loss scale may be slightly higher than initially reported. According to DefiLlama data, Binance's platform assets before the incident were approximately $144 billion, and the affected amount represents about 4.4% of that. Market maker Wintermute CEO Evgeny Gaevoy said that even at three to four times the current scale, Binance could still absorb it. Both Wintermute and GSR disclosed exposure on Binance; GSR said it was below 5% of its net equity and Wintermute said below 20%, with neither disclosing amounts.

### Market Reaction

The market dropped to a six-week low on the night of the incident. As of 01:19 UTC on October 30, Bitcoin was at about $76,937, down 4.9% in twenty-four hours; Ether fell 5.7% to about $2,211; BNB fell about 8% to about $693; AAVE fell 11.3% and UNI fell 10.7%. Bitcoin touched an intraday low of about $76,650. The market subsequently recovered part of the decline; according to CoinGecko data, as of 06:35 UTC on October 30, Bitcoin was at $78,518, with the twenty-four-hour decline narrowing to 1.78%; Ether was at $2,245.53, down 2.79%; BNB was at $715.24, down 4.83%. CoinGlass tallied approximately $1.4 billion in market-wide leveraged liquidations in the twenty-four hours after the incident, a figure that includes routine liquidations unrelated to this event.

## Information That Can Be Verified After the Incident

### What Proof of Reserves Can Answer

Proof of reserves uses a Merkle tree to let users verify whether their balances are included in the exchange's declared liabilities and to compare declared liabilities against assets in on-chain visible addresses. It answers the asset-liability correspondence at a given point in time; it does not answer how hot wallet signing permissions are managed, how operations terminals are isolated, or when assets were replenished after the incident. Binance has said it will update its proof-of-reserves data after remediation is complete, and the timing and values of that update are the most directly verifiable items following this incident. Users who want to understand how such data is presented can refer to the disclosure structure used on MEXC's proof-of-reserves page.

### The Difference Between Authorization-Based Attacks and Private Key Leakage

For users holding on-chain wallets, the technical detail worth noting in this incident is the separation between authorization and private keys. Once an EIP-7702-type delegation is signed, the contract can subsequently move assets according to its established logic, while the private key never leaves its original location. Checking whether a wallet has historical delegation authorizations and revoking those no longer needed is an action ordinary users can complete on their own, independent of the exchange's internal security measures.

### Fund Distribution Arrangements

Exchanges cannot process withdrawals while services are suspended, which in this incident manifested as a withdrawal disruption lasting about nine hours from 22:15 UTC on October 29 and, based on the official restoration window, possibly totaling nearly 54 hours. By separating where funds are held according to purpose, users can reduce the liquidity impact of a single platform's service disruption. This month's wealth management and campaign offerings can be viewed on MEXC's campaign page.

## James Mitchell's Exclusive View

What this incident changed is not the common knowledge that exchange hot wallets carry risk, but the specific form that risk takes. Most exchange theft cases over the past decade can be attributed to improper private key custody, so defenses have focused on key sharding, hot-cold isolation, and multisig thresholds. In this incident, Binance says no private key leakage was found, yet the wallet was still drained, because what the attacker obtained was the ability to approve signing requests, not the keys themselves. This means that a system compliant with key management standards can still be penetrated entirely through the compromise of a single operations terminal.

I believe what the industry should note even more is the decisive role asset composition plays in recoverability. According to Binance and TRM Labs data, the approximately $3 billion frozen portion consists almost entirely of centrally issued stablecoins, while the approximately $2.4 billion still in the attacker's hands is concentrated in Bitcoin and Ether. The attacker's dumping of USDT for ETH at a discount of over 10% before the freezes took effect was precisely an operation targeting this difference. In other words, stablecoin issuers' freezing capability has become a substantive part of incident handling, and that capability does not apply at all to native assets. The ratio of stablecoins to native assets in exchange hot wallets is therefore simultaneously a liquidity issue and a risk-handling issue.

One more point needs to be distinguished: freezing is not recovery. In the Bybit incident, only $48.4 million of the $1.5 billion stolen ultimately returned to the platform, after months of issuer and judicial proceedings. The scale of the freezes this time is far larger than before, but returning from blacklisted status to the exchange's books still requires re-minting or judicial transfer. Judging the final outcome of this incident should be based on Binance's updated proof-of-reserves data and full technical post-mortem after remediation, not on the current frozen amount.

## Frequently Asked Questions

### When will Binance withdrawals resume?

In its service update at 04:17 UTC on October 30, Binance said it expects to begin restoring withdrawal services within 48 hours, corresponding to around 04:17 UTC on November 1, and stated that withdrawals will remain suspended until the team completes security upgrades to the withdrawal service and related addresses. Withdrawals have been suspended since 22:15 UTC on October 29. As of 07:00 UTC on October 30, withdrawals had not yet resumed, and the official side had not published a phased rollout by network or by asset.

### Can deposits be used now?

Binance's service update at 04:17 UTC on October 30 said deposits were unaffected and operating normally. In its announcements at 00:07 UTC and 02:25 UTC, the company had listed deposits as suspended; the two statements differ and Binance has not explained why. Users should rely on the latest status on Binance's official announcements page before depositing.

### Will users suffer asset losses in this incident?

In its announcements, Binance said it would cover all losses through the SAFU fund and company funds and stated that user assets are safe. Founder Changpeng Zhao also said he expects user funds to be fully covered. According to DefiLlama data, Binance's platform assets before the incident were approximately $144 billion, and the affected amount represents about 4.4%. Binance has not yet published detailed compensation arrangements or timelines.

### Why do different parties give different stolen amounts?

The discrepancies stem from three sources. The chains covered differ: Blockaid's $3.3 billion covers only Ethereum, Arbitrum, and Avalanche, while TRM Labs' $6.36 billion covers ten chains. The valuation points differ: the same batch of assets is valued at $6.36 billion at transfer-time prices and revalued at $6.23 billion at post-incident market prices. The statistical object differs: Arkham Intelligence's $5.84 billion is the attacker wallets' holdings balance at a particular moment, not the total transferred out.

### Does the $3 billion already frozen mean it has been recovered?

No. Freezing means the assets have been blacklisted on the issuer's contract and cannot be moved, but they have not returned to the exchange's books. Return requires the issuer to re-mint them or transfer them under judicial proceedings. In the Bybit incident
