#Getting Started

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
To trade via [Hummingbot](https://github.com/hummingbot/hummingbot), view the [installation guide](https://hummingbot.org/installation/), or [developer guide](https://hummingbot.org/hummingbot-api/quickstart/) 

```py
import asyncio
import os

from hummingbot_api_client import HummingbotAPIClient

ACCOUNT = "master_account"


async def main():
    # Initialize client
    client = HummingbotAPIClient(
        base_url="http://localhost:8000",
        username="admin",
        password="admin"
    )
    await client.init()
    try:
        # Step 1: Add exchange account
        print("🔑 Adding Exchange Account...")
        try:
            account = await client.accounts.add_account(account_name=ACCOUNT)
            print(f"✓ Account '{account['account_name']}' added successfully!")
        except Exception as e:
            if "Account already exists" in str(e):
                print("ℹ️ Account already exists, continuing...")
            else:
                raise

        # Step 2: Add credentials for Coinbase (spot)
        print("\n🔑 Adding Coinbase Credentials...")
        cred_result = await client.accounts.add_credential(
            account_name=ACCOUNT,
            connector_name="coinbase_advanced_trade",
            credentials={
                "coinbase_advanced_trade_api_key": os.environ["COINBASE_API_KEY"],        # organizations/.../apiKeys/...
                "coinbase_advanced_trade_api_secret": os.environ["COINBASE_API_SECRET"],  # -----BEGIN EC PRIVATE KEY-----...
                "use_auth_for_public_endpoints": False,
            },
        )
        print(f"✓ Credentials added: {cred_result}")

        # Step 3: Add credentials for Lighter (perpetuals)
        print("\n🔑 Adding Lighter Credentials...")
        cred_result = await client.accounts.add_credential(
            account_name=ACCOUNT,
            connector_name="lighter_perpetual",
            credentials={
                "lighter_perpetual_l1_address": os.environ["LIGHTER_L1_ADDRESS"],                  # 0x... wallet that owns the account
                "lighter_perpetual_api_key_index": int(os.environ["LIGHTER_API_KEY_INDEX"]),
                "lighter_perpetual_api_private_key": os.environ["LIGHTER_API_PRIVATE_KEY"],
                "lighter_perpetual_account_limit": "Standard",   # your account type: Standard, Plus, Premium or Builder
            },
        )
        print(f"✓ Credentials added: {cred_result}")

        # Wait for account sync
        await asyncio.sleep(2)

        # Step 4: View portfolio
        print("\n📊 Fetching Portfolio...")
        portfolio = await client.portfolio.get_state()
        print("Portfolio State:")
        for account, exchanges in portfolio.items():
            for exchange, balances in exchanges.items():
                for balance in balances:
                    print(f"  {account}/{exchange}: {balance['units']:.2f} {balance['token']} (${balance['value']:.2f})")

        # Step 5: Get trading rules
        print("\n📏 Getting Trading Rules...")
        for connector, pair in (("coinbase_advanced_trade", "BTC-USDC"),
                                ("lighter_perpetual", "ETH-USD")):
            rules = await client.connectors.get_trading_rules(
                connector_name=connector,
                trading_pairs=[pair]
            )
            print(f"  {connector} {pair}:")
            print(f"    Min Order Size: {rules[pair]['min_order_size']}")
            print(f"    Min Price Increment: {rules[pair]['min_price_increment']}")

        # Step 6: Place limit orders - real money, priced well below the market
        print("\n💱 Placing Limit Orders...")
        spot_order = await client.trading.place_order(
            account_name=ACCOUNT,
            connector_name="coinbase_advanced_trade",
            trading_pair="BTC-USDC",
            trade_type="BUY",
            amount=0.0001,
            order_type="LIMIT",
            price=20000
        )
        perp_order = await client.trading.place_order(
            account_name=ACCOUNT,
            connector_name="lighter_perpetual",
            trading_pair="ETH-USD",
            trade_type="BUY",
            amount=0.02,
            order_type="LIMIT",
            price=1000,
            position_action="OPEN"
        )
        for order in (spot_order, perp_order):
            print(f"✓ Order {order['order_id']} submitted")
            print(f"  Status: {order['status']}")
            print(f"  Type: {order['order_type']} {order['trade_type']}")
            print(f"  Amount: {order['amount']} @ ${order['price']}")

    except Exception as e:
        print(f"❌ Error: {e}")
    finally:
        await client.close()

# Run the complete example
if __name__ == "__main__":
    asyncio.run(main())

```