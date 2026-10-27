#Quick Setup

Provided below are templates on how to connect to Coinbase and Lighter via [CCXT](https://github.com/ccxt/ccxt) or [Hummingbot](https://github.com/hummingbot/hummingbot).
<br><br>
***Note:*** The following templates are reference examples; participants should independently verify all details. 
<hr>

##Using CCXT
To trade via [CCXT](https://github.com/ccxt/ccxt)
```py title="Coinbase"
import os
import ccxt

# Coinbase issues separate API keys for spot and for perpetuals.
spot = ccxt.coinbase({
    "apiKey": os.environ["COINBASE_SPOT_API_KEY"],
    "secret": os.environ["COINBASE_SPOT_API_SECRET"],
})


# Perpetuals run on Coinbase's derivatives gateway, which ccxt reaches through
# coinbaseinternational. The override works around a bug in ccxt 4.5.90 that
# breaks the gateway login in Python. Remove it once ccxt fixes it.
class CoinbasePerps(ccxt.coinbaseinternational):
    @staticmethod
    def extend(*args):
        return ccxt.Exchange.extend(*({} if a is None else a for a in args))


perps = CoinbasePerps({
    "apiKey": os.environ["COINBASE_PERP_API_KEY"],
    "secret": os.environ["COINBASE_PERP_API_SECRET"],
})

print(spot.fetch_balance()["total"])
print(perps.fetch_balance()["USDC"])   # read USDC only - see below
print(perps.fetch_positions())

# Placing orders (real money):
# spot.create_order("BTC/USDC", "limit", "buy", 0.0001, 50000)
# perps.create_order("ETH/USDC:USDC", "limit", "buy", 0.01, 2000)
```

```py title="Lighter"
import os
import ccxt

lighter = ccxt.lighter({
    "privateKey": os.environ["LIGHTER_WALLET_PRIVATE_KEY"],
    "options": {
        "accountIndex": int(os.environ["LIGHTER_ACCOUNT_INDEX"]),
        "apiKeyIndex": 5,                                  # any free index, 4-254
        "libraryPath": os.environ["LIGHTER_SIGNER_PATH"],  # Lighter's signing library
        "builderFee": False,                               # turn off ccxt's own fee
    },
})

print(lighter.fetch_balance()["USDC"])
print(lighter.fetch_positions())

# Placing an order (real money):
# lighter.create_order("ETH/USDC:USDC", "limit", "buy", 0.01, 2000)
```

<hr>

##Using Hummingbot
To trade via [Hummingbot](https://github.com/hummingbot/hummingbot), view the [installation guide](https://hummingbot.org/installation/).