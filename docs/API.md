---
title: SushiSwap Exchange REST API
summary: Overview of Public API Endpoints for interacting with SushiSwap
version: 0.3.1
chainId: 0x01
---

# SushiSwap Endpoints

## [`/swap/summary`](https://7ob2ikxqn7.execute-api.us-east-1.amazonaws.com/dev//swap/summary)

Returns data for the top ~1000 SushiSwap pairs, sorted by reserves. Results are
edge cached for 15 minutes.

### Request

`GET https://7ob2ikxqn7.execute-api.us-east-1.amazonaws.com/dev//swap/summary`

### Response

```json5
{
  '0x..._0x...': {
    // the asset ids of the ERC20 tokens (i.e. token addresses), joined by an underscore
    last_price: '1.234', // denominated in token0/token1
    base_volume: '123.456', // last 24h volume denominated in token0
    quote_volume: '1234.56', // last 24h volume denominated in token1
  },
  // ...
}
```

## [`/swap/assets`](https://7ob2ikxqn7.execute-api.us-east-1.amazonaws.com/dev/swap/assets)

Returns the tokens in the top ~1000 pairs on SushiSwap, sorted by reserves.
Results are edge cached for 24 hours.

### Request

`GET https://7ob2ikxqn7.execute-api.us-east-1.amazonaws.com/dev/swap/assets`

### Response

```json5
{
  // ...,
  '0x...': {
    // the address of the ERC20 token
    name: '...', // not necessarily included for ERC20 tokens
    symbol: '...', // not necessarily included for ERC20 tokens
    id: '0x...', // the address of the ERC20 token
    maker_fee: '0', // always 0
    taker_fee: '0.003', // always 0.003 i.e. .3%
  },
  // ...
}
```

## [`/swap/tickers`](https://7ob2ikxqn7.execute-api.us-east-1.amazonaws.com/dev/swap/tickers)

Returns data for the top ~1000 SushiSwap pairs, sorted by reserves. Results are
edge cached for 1 minute.

### Request

`GET https://7ob2ikxqn7.execute-api.us-east-1.amazonaws.com/dev/swap/tickers`

### Response

```json5
{
  '0x..._0x...': {
    // the asset ids of ETH and ERC20 tokens, joined by an underscore
    base_name: '...', // token0 name
    base_symbol: '...', // token0 symbol
    base_id: '0x...', // token0 address
    quote_name: '...', // token1 name
    quote_symbol: '...', // token1 symbol
    quote_id: '0x...', // token1 address
    last_price: '1.234', // the mid price as token1/token0
    base_volume: '123.456', // denominated in token0
    quote_volume: '1234.56', // denominated in token1
  },
  // ...
}
```

## `/swap/orderbook/:pair`

Returns simulated orderbook data for the given SushiSwap pair. Since Uniswap has
a continuous orderbook, fixed amounts in an interval are chosen for bids and
asks, and prices are derived from the SushiSwap formula (accounting for both
slippage and fees paid to LPs). Results are edge cached for 240 minutes.

### Request

`GET https://7ob2ikxqn7.execute-api.us-east-1.amazonaws.com/dev/swap/orderbook/:pair`

### URL Parameters

- `pair`: The asset ids of two ERC20 tokens, joined by an underscore, e.g.
  `0x..._0x...`. The first token address is considered the base in the response.

### Response

```json5
{
  timestamp: 1234567, // UNIX timestamp of the response
  bids: [
    ['12', '1.2'], // denominated in base token, quote token/base token
    ['12', '1.1'], // denominated in base token, quote token/base token
    // ...
  ],
  asks: [
    ['12', '1.3'], // denominated in base token, quote token/base token
    ['12', '1.4'], // denominated in base token, quote token/base token
    // ...
  ],
}
```

## `/swap/trades/:pair`

Returns all swaps in the last 24 hours for the given SushiSwap pair. Results are
edge cached for 15 minutes.

The pair address is the address of the two tokens in either order. The first
address is considered the base in the response.

### URL Parameters

- `pair`: The asset ids of two ERC20 tokens, joined by an underscore, e.g.
  `0x..._0x...`. The first token address is considered the base in the response.

### Request

`GET https://7ob2ikxqn7.execute-api.us-east-1.amazonaws.com/dev/swap/trades/:pair`

### Response

```json5
[
  {
    trade_id: '...',
    price: '1.234', // denominated in quote token/base token
    base_volume: '123.456', // denominated in base token
    quote_volume: '1234.56', // denominated in quote token
    trade_timestamp: 1234567, // UNIX timestamp
    type: 'buy', // "buy"/"sell"/"borrow-both"/"???"
  },
  // ...
]
```


weth: 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2
