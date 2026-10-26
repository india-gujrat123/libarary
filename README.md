# Indian Index Options Algo — Dhan starter

A **paper-first, strategy-agnostic foundation** for NIFTY, BANKNIFTY and SENSEX options using DhanHQ. This is not a finished trading bot and it does not contain your strategy yet.

## What is in this starter

- Configurable support for NIFTY/BANKNIFTY (NSE F&O) and SENSEX (BSE F&O); security IDs and lot sizes are not hard-coded.
- DhanHQ REST helpers for expiry lists and option-chain snapshots, with a conservative 3-second throttle.
- A small option-contract selector (ATM / OTM steps) that requires current contract metadata from Dhan's instrument master.
- Explicit entry risk checks and a paper broker that models a long-option buy at the ask and exit at the bid.
- A Dhan live-order adapter with **two explicit live gates**. The app's starter runner does not start a live loop or send orders.
- A HOLD-only strategy placeholder. No strategy has been invented on your behalf.

## Not implemented yet

- Your entry/exit rules, Hindi strategy-to-rules approval flow, and backtesting.
- A production market-data WebSocket loop, reconnect handling, candle builder, and live positions/order reconciliation.
- Exchange holiday/session calendar, fees/taxes, partial fills, slippage/market impact, and robust VPS monitoring.
- Production launch or any promise of profitability.

Paper fills are only an approximation: they use top-of-book ask/bid and do **not** include brokerage, STT, exchange charges, taxes, latency, queue position, or partial fills. Paper results are not a prediction of live results.

## Run the harmless starter

Requires Node.js 20 or newer.

```sh
cp .env.example .env
npm test
npm start
```

The starter only prints its configuration and confirms that no strategy/order loop is running. Set your own risk limits before paper entries can pass the risk gate. `MAX_DAILY_LOSS_INR=0` and `MAX_PREMIUM_EXPOSURE_INR=0` intentionally block entries.

## Dhan setup notes

- Use Dhan's current instrument master for underlying/option security IDs, expiries, lot sizes and tick sizes. Refresh it rather than relying on sample IDs.
- Dhan's Option Chain API has a limit of one unique request every 3 seconds; the client throttles option-chain/expiry-list requests accordingly. For live ticks, use the Dhan WebSocket feed and keep the option-chain endpoint for contract selection/slow-changing data.
- Dhan documents that order placement/modification/cancellation requires Static IP whitelisting. A live deployment therefore needs an eligible fixed public IP and broker-side setup; this Arena workspace is not a live-trading host.
- Dhan access tokens are time-limited. Keep secrets in a private local/VPS environment, never in chat, source control, or a browser dashboard.

References: [Dhan Option Chain](https://dhanhq.co/docs/v2/option-chain/), [Dhan Live Market Feed](https://dhanhq.co/docs/v2/live-market-feed/), [Dhan Orders](https://dhanhq.co/docs/v2/orders/), [Dhan Instruments](https://dhanhq.co/docs/v2/instruments/).

## Live-trading gate and compliance

The live adapter requires all of the following: `TRADING_MODE=live`, `ALLOW_LIVE_ORDERS=I_UNDERSTAND`, `LIVE_ORDER_ACK=I_UNDERSTAND_REAL_MONEY`, a separate runtime confirmation string, positive user-defined risk limits, a risk-approved order, valid Dhan credentials, and broker-side Static IP/API setup. The starter runner intentionally does not invoke this adapter.

SEBI's September 30, 2025 circular states that the retail algo framework and exchange operational modalities apply to all stock brokers from April 1, 2026. Before any live use, confirm the current requirements and your account/API eligibility directly with Dhan and the relevant exchange. This code is technical scaffolding, not legal or investment advice.

## When you provide a strategy

We will translate it into explicit rules and get your approval before enabling it. Please specify:

1. Index(es) and timeframe.
2. Exact entry condition(s), including candle-close/crossing semantics.
3. Option selection: CE/PE, expiry rule, ATM/OTM/ITM or delta, liquidity/spread filters.
4. Exit rules: stop loss, target, trailing stop, time exit, and what to do after partial fills.
5. Position sizing, maximum simultaneous trades, daily loss stop, and whether re-entry is allowed.

Natural-language instructions should become a reviewed rule spec; the LLM should not get unrestricted authority to place live orders.
# libarary
