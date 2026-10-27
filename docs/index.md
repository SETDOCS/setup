#Quick Setup

<hr>

##Using CCXT
To start trading with [CCXT](https://github.com/ccxt/ccxt)
```py title="Coinbase"
import ccxt

def connect_coinbase():
    # Initialize Coinbase instance
    coinbase = ccxt.coinbase({
        'apiKey': 'YOUR_COINBASE_API_KEY',
        'secret': 'YOUR_COINBASE_SECRET_KEY',
        'enableRateLimit': True,
    })

    try:
        # Load available trading markets
        coinbase.load_markets()
        print("Connected to Coinbase successfully!")

        # Fetch market ticker (Public Data)
        symbol = 'BTC/USD'
        ticker = coinbase.fetch_ticker(symbol)
        print(f"\n[Coinbase] {symbol} Price: ${ticker['last']}")

        # Fetch account balances (Private Data)
        # balance = coinbase.fetch_balance()
        # print("[Coinbase] Account Balances:", balance['total'])

    except Exception as e:
        print(f"Coinbase Error: {e}")
```

```py title="Lighter"
def connect_lighter():
    # Lighter requires the native C/Go binary compiled for signing operations.
    # Download the dynamic library for your platform from:
    # https://github.com/ccxt/lighter-wasm
    
    lighter = ccxt.lighter({
        'privateKey': '0xYOUR_L1_PRIVATE_KEY',  # Your Ethereum wallet private key
        'enableRateLimit': True,
        'options': {
            # Provide path to the compiled signer library (.dylib / .so / .dll)
            'libraryPath': '/path/to/lighter-signer-darwin-arm64.dylib', 
            # Optional: Specify accountIndex if using a sub-account (default is auto-fetched)
            # 'accountIndex': 0, 
        }
    })

    try:
        # Load available markets
        lighter.load_markets()
        print("\nConnected to Lighter successfully!")

        # Fetch ticker for a DEX pair (e.g., BTC/USDC perps or spot)
        symbol = 'BTC/USDC'
        if symbol in lighter.markets:
            ticker = lighter.fetch_ticker(symbol)
            print(f"[Lighter] {symbol} Price: ${ticker['last']}")
        else:
            # Print available symbols on Lighter
            available_symbols = list(lighter.markets.keys())[:5]
            print(f"[Lighter] Sample available markets: {available_symbols}")

        # Fetch balances (requires privateKey & signer binary)
        # balance = lighter.fetch_balance()
        # print("[Lighter] Balances:", balance)

    except Exception as e:
        print(f"Lighter Error: {e}")
```

<hr>

##Using Hummingbot
To start trading with [Hummingbot](https://github.com/hummingbot/hummingbot), view the [installation guide](https://hummingbot.org/installation/).