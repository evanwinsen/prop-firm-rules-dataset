# Prop firm rules & prices dataset

A free, verified table of **prop trading firm evaluation prices, profit targets, drawdown limits and trading rules**.
One row per firm, product and account size: **986 rows, 62 firms** across futures, forex/CFD, crypto and stocks, with **61 columns** including challenge steps, time limits, soft/hard daily loss limits, drawdown locks, news windows, hedging/copy-trading/scalping rules, payout terms and caps, fee refunds, restricted countries and derived metrics such as cost to get funded.

- Source and live version: https://propfirmvs.com/data
- Download: [`prop-firm-rules.csv`](prop-firm-rules.csv) (also at https://propfirmvs.com/data/prop-firm-rules.csv)
- License: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Please credit **PropFirmVS** with a link to https://propfirmvs.com/data.
- Snapshot: 2026-10-18 (firm rules re-checked 2026-10-17 against each firm's own pages). Rules change often; the live file on propfirmvs.com is the current version.

## Columns

| Column | Meaning |
| --- | --- |
| `firm` | Firm name |
| `market` | futures, forex, crypto or stocks |
| `program` | Product / program name when a firm sells several (may be empty) |
| `account_size_usd` | Nominal account size in USD |
| `eval_price_usd` | Evaluation price in USD (one-time, or per month where the firm bills monthly) |
| `profit_target_usd` | Profit target in USD |
| `max_drawdown_usd` | Maximum drawdown in USD |
| `daily_loss_limit_usd` | Daily loss limit in USD; empty = none or unverified |
| `drawdown_type` | Static balance, Trailing end-of-day or Trailing intraday |
| `consistency_percent` | Consistency rule threshold in percent; empty = none or unverified |
| `activation_fee_usd` | Activation / funded-account fee in USD; empty = none or unverified |
| `challenge_steps` | Number of evaluation phases: 0 = instant funding, 1, 2 or 3 |
| `time_limit_days` | Evaluation time limit in calendar days; 0 = no time limit; empty = unverified |
| `phase2_profit_target_usd` | Phase 2 profit target in USD (2+ step programs only) |
| `max_micro_contracts` | Maximum micro contracts allowed on this account size (futures); empty = not stated |
| `funded_drawdown_type` | Drawdown type of the funded phase when it differs from the evaluation |
| `funded_consistency_percent` | Consistency threshold for the funded/payout phase in percent; 0 = none; empty = unknown or same as evaluation |
| `price_per_1k_account_usd` | Derived: evaluation price per $1,000 of account size |
| `profit_target_to_drawdown` | Derived: profit target divided by max drawdown (lower = easier; 0 = no target) |
| `rule_strictness` | Derived: Lenient, Moderate, Strict or Very strict; transparent point count, see https://propfirmvs.com/methodology#derived |
| `hedging` | yes, restricted or no; empty = unverified |
| `copy_trading` | yes, restricted or no; empty = unverified |
| `scalping` | yes, restricted or no; empty = unverified |
| `martingale` | yes, restricted or no; empty = unverified |
| `stop_loss_required` | yes or no; empty = unverified |
| `min_trade_duration_sec` | Minimum trade duration in seconds; 0 = none; empty = unverified |
| `inactivity_days` | Days without a trade before the account may be closed; empty = unverified |
| `max_risk_per_trade_pct` | Maximum risk per trade as % of balance; empty = no such rule or unverified |
| `fee_refund` | Is the evaluation fee refunded (usually with the first payout): yes, partial or no |
| `first_payout_days` | Earliest day count from funded start to first payout request (calendar vs trading days differ per firm) |
| `min_payout_usd` | Minimum payout request in USD |
| `max_payout_usd` | Maximum per payout request in USD; empty = no cap stated |
| `payout_methods` | Payout methods, comma-separated: bank_wire, crypto, paypal, rise, wise, card, other |
| `funded_monthly_fee_usd` | Recurring fee after funding in USD; 0 = none; empty = unverified |
| `data_provider` | Futures data feed provider; empty = not stated |
| `leverage` | Leverage by asset class (forex/CFD/crypto) |
| `commissions` | Commissions / fees (forex/CFD/crypto) |
| `swap_free` | Swap-free account: yes, addon or no |
| `restricted_countries` | Countries whose residents cannot join, as published by the firm; empty = not found |
| `firm_page` | URL of the firm's review page on propfirmvs.com |
| `last_verified` | Date the firm rules were last checked against its own pages (YYYY-MM-DD) |
| `eval_billing` | one_time or monthly (evaluation billed as a subscription) |
| `price_per_1k_drawdown_usd` | Derived: evaluation list price per $1,000 of max drawdown |
| `cost_to_get_funded_usd` | Derived: evaluation list price (x 2 months if billed monthly) + activation fee; empty when the activation fee is unknown |
| `reset_fee_usd` | Reset fee for this product and size in USD |
| `funded_start_contracts` | Futures: contracts allowed at the start of the funded account |
| `payout_cap_usd` | Cap on the first payout(s) for this size in USD |
| `currency` | Pricing currency when the firm does not price in USD (EUR or GBP); empty = USD |
| `native_price` | Price in the firm's own currency; eval_price_usd is then an approximate USD conversion (ECB reference rate) |
| `dll_breach` | soft (trading paused for the day) or hard (account fails); empty = unverified |
| `dll_basis` | What the daily loss is measured from: balance, equity, higher_of, eod_balance, other |
| `dd_lock_trigger` | When the trailing drawdown stops trailing: none, balance_threshold, target_reached, first_payout |
| `dd_lock_offset_usd` | Drawdown floor locks at start balance + this amount (USD, main program) |
| `news_window_eval_min` | Minutes around high-impact news with restricted trading in the evaluation; 0 = no restriction |
| `news_window_funded_min` | Same for funded accounts; 0 = no restriction |
| `consistency_base` | What the consistency % is measured against: total_profit, profit_target, positive_days, other |
| `consistency_effect` | What breaking it does: blocks_payout, raises_target, fails_account, other |
| `commission_rt_es_usd` | Futures: round-turn commission per ES contract in USD |
| `data_feeds` | Futures data/connection feeds (Rithmic, Tradovate, CQG, ...) |
| `max_payouts` | Maximum number of payouts before the account closes or moves live; 0 = unlimited |
| `funded_account_type` | simulated, live or mixed (per the firm's terms) |

**Empty cells mean unverified or not applicable, never guessed.** Prices and rules change often: check the firm's own site before buying.

## Quick start

```python
import pandas as pd
df = pd.read_csv("https://propfirmvs.com/data/prop-firm-rules.csv")
# cheapest 50K futures evaluations
print(df[(df.market == "futures") & (df.account_size_usd == 50000)].nsmallest(5, "eval_price_usd")[["firm", "program", "eval_price_usd", "drawdown_type"]])
# 1-step programs that allow hedging, lowest price per $1,000
print(df[(df.challenge_steps == 1) & (df.hedging == "yes")].nsmallest(5, "price_per_1k_account_usd")[["firm", "program", "account_size_usd", "price_per_1k_account_usd"]])
```

## How the data is made

Every firm is checked against its own public pages; a nightly job flags changes, and rule changes are shown only after human review. Derived columns are computed from the published rules only. Method: https://propfirmvs.com/methodology. Recent changes: https://propfirmvs.com/pulse.

## Cite

> PropFirmVS (2026). *Prop firm rules & prices dataset*. https://propfirmvs.com/data. CC BY 4.0.

Disclosure: PropFirmVS earns commission from some firms' affiliate links. The dataset itself is not influenced by it; see https://propfirmvs.com/learn/how-we-make-money.
