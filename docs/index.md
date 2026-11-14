#SET Trading Competition

SET is a 56-day crypto algo-trading competition. You trade your own money on your own exchange accounts, and you are ranked on how well you traded, not on how much you made.
<br><br>
It is free to enter. There is no entry fee, no deposit to SET, and nothing you give us can move your funds.

<hr>

## When it runs

| Event | Date / Time |
| :--- | :--- |
| Registration opens | Now |
| Trading starts | 23 November 2026, 12:00 UTC |
| Trading ends | 18 January 2027, 12:00 UTC |
| Registration closes | 17 January 2027, 12:00 UTC |

That is exactly 56 days, noon to noon.
<br><br>
**You can join late.** Registration stays open until 24 hours before the end, and joining late is not a disadvantage in itself. You are scored on the days you traded, not penalised for the ones you missed. Twenty strong days beats fifty-six indifferent ones.
<br><br>
Only trading inside the window counts. Anything you do before 23 November or after 18 January is invisible to your score.

<hr>

## What you need

| Venue | What it is | What you give SET |
| :--- | :--- | :--- |
| **Coinbase Spot** | Buying and selling the asset itself | A View-only CDP key: key name and private key |
| **Coinbase Perpetuals** | Perpetual futures, a separate portfolio from spot | The same, scoped to your perps portfolio |
| **Lighter**| Perpetuals only, there is no spot market| A read-only token beginning `ro:` |

You can enter on one venue or all three. Each is registered and scored separately, and your Sharpe is computed across everything you register as one portfolio.

### Minimum deposit 

There is no entry fee and no deposit to SET. You fund your own exchange account and the money never leaves your control.
<br><br>
To be ranked you need 100 USDC in the account. Below that you still appear, still get scored, and still count, just in the ***Not yet qualified*** tab rather than the ranking. Put 100 USDC in and you move up.
<br><br>
It is measured against the highest your account has ever been worth, so once you have qualified, a bad day cannot send you back down.
<br><br>
You also need 500 USDC of traded volume to be ranked. Both sides of a round trip count, so opening a 250 USDC position and closing it covers it. It is there because a Sharpe ratio can be flattered by trading almost nothing, and the whole point is to see you trade.

### About the referral links

Opening your account through SET's link is how the competition is funded. It costs you nothing.

- **Coinbase:** if you already have an account, just add SET as your referrer. You do not need a new one.
- **Ligher:** Lighter fixes the referral code when the account is created and it cannot be added afterwards, so this one does have to be a new account.

<hr>

## Keys
**Nothing you give SET can move your funds.**

| Venue | Details |
| :--- | :--- |
| **Coinbase** | Your key's permissions are checked against Coinbase when you register. If it can trade or transfer, it is refused. If the check itself fails, that is also a refusal, never a pass |
| **Lighter** | A `ro:` token is incapable of signing. There is nothing to check, because there is nothing it could do |

- **What SET can see:** your balances, and your closed orders, on the accounts you registered.
- **What SET cannot do**: place an order, cancel one, move funds between your accounts, or withdraw anything. There is no code in our system that can trade.
- **How your keys are stored**: verified against the exchange first, then encrypted before they are written down. They are never shown back to you or to anyone else.
- **What happens to the data**: The privacy notice at set.trade/privacy is the full account: what we collect, who receives it, how long we hold it and how to have it deleted. The short version is that your trading records go to the partner firms, your email address and your keys do not, and everything we hold is deleted within 30 days of the end. Email contact@set.trade to have yours removed sooner.

When you are finished, revoke your keys. Do it at the exchange. That is all it takes: our access ends immediately, you do not need our permission or our help, and it works the same way if you want to leave mid-competition.


!!! warning
    Never send anyone a Lighter wallet private key. Not us, not anyone. A wallet key cannot be revoked. The form refuses anything that does not start with ro: for exactly this reason.


<hr>

## Rules
- **Read-only keys only.** Anything that can trade or withdraw is rejected when we check it against the exchange.
- **Open your accounts through SET's link**, or add SET as referrer where the exchange allows it.
- **Use the account you already have**. You do not need a brand new one, and nothing you traded before the competition is held against you. See "What counts" below.
!!! info
    The one exception is Lighter, which fixes the referral code when an account is created and will not let you add one later. A Lighter entry needs a new account. Coinbase lets you add a referrer to an account you already have, so keep yours.

### What counts

Only what you do between 23 November and 18 January. Everything else is invisible to your score.

- A trade filled before the start is not in your volume or your trade count.
- A position you were already holding does not show up as profit. Your balance at the start is your starting line, and it already includes whatever that position is worth. Closing it during the competition moves your score by zero.
- Nothing after the end date counts.
- 100 USDC in the account to be ranked. See above.
- 500 USDC of traded volume to be ranked. Both sides of a round trip count. See above.
- Trade your own account. One entry per person.
- Your display name and avatar are public. Your email and your keys never are.

### Rankings
The trading records go to our market-maker partners, who read them and decide for themselves who is interesting. A high placing is **not** an offer, a guarantee of one, or a promise that anyone will be in touch. And you do not have to win to be worth reading. Partners are looking for how somebody trades, not just who came first, so a strong 20-day showing from a late entrant can be more interesting to them than a mediocre 56-day one. Place twelfth with a clean, disciplined record and you may well get the call.

<hr>

- **56-Day Live Market Sprint**: Test and deploy your automated trading algorithms in real market conditions across an 8-week competition window.

- **$100 Minimum Starting Capital**: Participate with an accessible $100 balance spread across Coinbase and Lighter.

- **Ranking Matrix**: Performance is evaluated using PnL%, volume, and Sharpe Ratio.

- **Earn an Industry Internship**: Top-ranked participants earn an internship with our industry partners.

