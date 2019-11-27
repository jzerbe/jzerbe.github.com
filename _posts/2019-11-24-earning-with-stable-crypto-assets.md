---
layout: post
title: earning with stable crypto assets
tags:
- crypto
- decentralized finance
- earn
published: true
---
What does one do in an economic environment where the national governments have to keep lending rates low
to manage their [crippling debt load](https://en.wikipedia.org/wiki/List_of_countries_by_external_debt),
artificially low lending rates have inflated equities past their [reasonable growth rates](https://www.investopedia.com/terms/p/price-earningsratio.asp),
and banks are not passing on the profits from printing an [order of magnitude of money](https://www.investopedia.com/terms/t/tier-1-leverage-ratio.asp)?

I tried finding an asset class that functions like a dividend paying stock, but without the high price.
For my risk tolerance profile, interest bearing crypto stable coin accounts are the solution.

## What is a stable coin?
In the United States of America we used to [back our currency with gold](https://www.investopedia.com/ask/answers/09/gold-standard.asp).
The thought was that the currency is back-stopped by something that has intrinsic value, or rather people trusted that gold would continue to be
trusted and valued by other people. Ultimately, all of our financial instruments are backed by everyone else's trust.
I guess people just have been trusting gold longer?

Think of a crypto stable asset or more commonly, _stablecoin_, as pegged to the value of some other asset.
The value of this stablecoin is that is represents one unit of something else and therefore the volatility
matches that of the underlying asset. A chain of trust.

[_Paxos Trust Company LLC_](https://www.paxos.com/) issues a crypto instrument
called [Paxos Standard (PAX)](https://www.paxos.com/pax/), running on the
[ERC-20](https://www.investopedia.com/news/what-erc20-and-what-does-it-mean-ethereum/) protocol, that
is backed by 1USD in Paxos' reserves. I trust the USD to not be significantly eroded by inflation in the medium term and
I trust that New York State's Department of Financial Services to [appropriately review Paxos Trust Company](https://www.dfs.ny.gov/about/press/pr1809101.htm).
Many other people seem to hold similar views: <https://coinmarketcap.com/currencies/paxos-standard/ratings/>

[Coinbase](https://www.coinbase.com/about) issues a crypto instrument called [USD Coin](https://www.centre.io/usdc),
also running on ERC-20 and backed 1:1 with USD. I place my trust in the large number of exchanges that accept USDC
and Coinbase's list of investors. People seem to trust this asset: <https://coinmarketcap.com/currencies/usd-coin/ratings/>

The big risk on my mind to assets running on ERC-20 comes down to network traffic: <https://hackernoon.com/explainer-why-is-my-ethereum-transaction-taking-so-long-qg1t632cz>

Or I suppose outright banning by national governments as their control on money wanes.

## What is the business model for crypto backed loans?
To earn interest, you need someone to be paying you for the opportunity cost of not having immediate access to your money.
Many early crypto adopters have large amounts of their wealth locked up in crypto instead of more liquid assets;
I and the IRS view _traditional_ crypto assets, such as BTC or ETH, more like stocks that one has to [pay capital gains or losses on](https://www.irs.gov/pub/irs-drop/n-14-21.pdf).

The original business model of earning interest on crypto assets was to get around the capital gains tax.
Instead of selling your crypto assets (and incurring a taxable event - capital gains or losses),
just borrow on them and use the cash to make a purchase;
you use your crypto assets as collateral for a loan (similar to how one uses a house as collateral for a refinance).
Enter one of the original players, Nexo, who pays [8% APY compounding daily on stablecoins](https://nexo.io/earn-interest).
The credibility of the company comes into question every now and again - <https://www.reddit.com/r/Nexo/comments/cvjg0q/red_flag_fake_statements_licensed_regulated/>, <https://www.reddit.com/r/Nexo/comments/cjp6sx/due_diligence_on_nexo_red_flags/> - but from personal experience,
they paid me a few thousand in interest and let me extract the principal and interest without issue.

Celsius emerged as a competitor paying slightly higher [APY compounding weekly](https://celsius.network/earn-interest-on-your-crypto/).
In addition to crypto backed loans, they bundle up assets under management and loan them to leveraged and short sellers.
You can earn slightly higher rates if you hold their coin (_staking_) and get paid in their coin, but I would rather take the assured payout
on stablecoins and not deal with price volatility that could evaporate my interest earnings.

Another strong player is [Crypto.com](https://platinum.crypto.com/r/j9cd8tadum) and they offer [higher APY for longer deposits](https://crypto.com/en/earn.html).
Unfortunately, they get you on withdrawal fees and no auto-compounding. Not sure exactly what rebundling of their
assets under management they do to make money. I don't stake the Crypto coins either.

## How do I make money?
I spread my risk by splitting invested wealth between stablecoins and interest bearing wallets.

### Earn on USDC with Nexo
1. Deposit USD into [Coinbase Pro](https://pro.coinbase.com) through an ACH transfer from a brokerage (Fidelity, Schwab) style bank account since they generally do not place restrictions like Chase does.
1. Convert USD into USDC: <https://support.pro.coinbase.com/customer/en/portal/articles/2958774-usd-coin-usdc-faq>
1. Open an account with [Nexo](https://nexo.io/?u=5cfd55180794714d172230f4).
1. Triple check the deposit address provided by Nexo and deposit: <https://support.nexo.io/hc/en-us/articles/360008118554-Where-do-I-see-the-address-where-I-need-to-deposit-the-collateral-for-my-Nexo-loan->

### Earn on PAX with Celsius
1. Create a [Paxos Wallet account](https://account.paxos.com/wallet).
1. Create a [Celsius account](https://celsiusnetwork.app.link/139363bfc0) through their app.
1. Wire USD from a Fidelity Cash Management Account to Paxos Trust Company.
Once you set up the wire transfer online, you will have to actually execute this order over the phone to include the memo provided by Paxos to identify your deposit account.
I say Fidelity, because they charge no domestic wire fees: <https://www.fidelity.com/cash-management/bank-wires>
1. Convert your USD to PAX at 1:1 and send to your Celsius deposit address.
