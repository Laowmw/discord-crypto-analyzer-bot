# CryptoBot – Discord Ichimoku Cloud Analyzer

CryptoBot is a smart bot that uses **Binance market data** to do **Ichimoku Cloud technical analysis** and share the results on a **Discord server**.

It runs fully on **Node.js** and can automatically analyze markets at regular time intervals.


## Features

* **Binance API integration** – Real-time price, volume, and candlestick data
* **Ichimoku Cloud analysis** – Detects market trend direction and support/resistance levels
* **Discord commands** – Commands for price, volume, analysis, and help
* **Automatic alerts** – Sends Ichimoku updates every 15 minutes for selected markets
* **Fully Node.js-based** – No R or external services required
* **Easy setup** – Simple configuration using JSON files


## Example Commands

| Command            | Description                                          |
| ------------------ | ---------------------------------------------------- |
| `!help`            | Shows the list of available commands                 |
| `!price BTCUSDT`   | Shows the current price of the given market          |
| `!priceUSD ETHBTC` | Shows the market price in USD                        |
| `!vol BTCUSDT`     | Shows the 24-hour trading volume                     |
| `!markets`         | Lists all supported markets                          |
| `!ichi BTCUSDT 1h` | Runs Ichimoku Cloud analysis (default timeframe: 1h) |
| `!alert`           | Turns Ichimoku auto alerts on or off                 |


## Installation Steps

### Requirements

Make sure these programs are installed:

* [Node.js](https://nodejs.org/) (v18+ recommended)
* [npm](https://www.npmjs.com/)

### Download the repository

### Install dependencies

### Git Installation

discord.io requires Git to be installed on your system.

👉 Install Git from here: https://git-scm.com/install/windows

After installation, open your command line and run:

```bash
npm install
```

If you see an error saying "git not found", make sure Git is added to your system PATH and try again.

### Create configuration files

In the main project folder, create these 3 files:

#### 📁 `auth.json`

```json
{
  "token": "YOUR_AUTH_TOKEN_HERE"
}
```

> Get your bot token from the [Discord Developer Portal](https://discord.com/developers/applications) and paste it here.


#### 📁 `config.json`

```json
{
  "alert_channelID": "YOUR_ALERT_CHANNEL_HERE"
}
```

> The Discord channel ID where the bot will send Ichimoku alerts.
> (Right-click the channel name in Discord → “Copy ID”)


#### 📁 `markets.json`

```json
{
  "markets": [
    "BTCUSDT","ETHUSDT","BNBUSDT","LTCBTC","NEOBTC","QTUMETH","EOSETH","OMGBTC","ZRXBTC","ADAUSDT"
  ]
}
```

> Add here the market pairs you want to analyze.


### Add Binance API keys

Edit this part in `index.js`:

```js
binance.options({
  APIKEY: 'YOUR_BINANCE_API_KEY',
  APISECRET: 'YOUR_BINANCE_SECRET_KEY'
});
```

You can create your API keys from the [Binance API Management](https://www.binance.com/en/my/settings/api-management) page.


### Run the bot

```bash
node index.js
```

> When the bot starts, it connects to Discord and shows “✅ Connected” in the console.
> You can now use the commands in your Discord server.

## Automatic Ichimoku Alert System

By default, the bot runs Ichimoku analysis **every 15 minutes** (`0,15,30,45 * * * *`).
This is set inside `index.js`:

```js
const ALERT_FREQUENCY = "0,15,30,45 * * * *";
const interval = "15m"; // 15m, 30m, 1h, 4h, 1d ...
```

You can change the timing as you like.
Alerts are sent to the channel defined in `config.json` (`alert_channelID`).


## Project Structure

```
CryptoBot/
│
├── index.js             # Main Discord + Binance bot file
├── ichimoku.js          # Ichimoku Cloud calculations
│
├── auth.json            # Discord bot token
├── config.json          # Alert channel ID
├── markets.json         # Market list to track
│
├── package.json         # Node.js dependencies
└── README.md            # This file :)
```


## Technical Details

The Ichimoku indicator calculates 5 lines:

| Line                          | Meaning          | Formula                                                  |
| ----------------------------- | ---------------- | -------------------------------------------------------- |
| **Tenkan-sen (Turning Line)** | Short-term trend | (Highest High + Lowest Low) / 2 (20 candles)             |
| **Kijun-sen (Base Line)**     | Mid-term trend   | (Highest High + Lowest Low) / 2 (60 candles)             |
| **Senkou Span A**             | Upper cloud line | (Tenkan + Kijun) / 2 (shifted 60 forward)                |
| **Senkou Span B**             | Lower cloud line | (Highest High + Lowest Low) / 2 (120 candles + 60 shift) |
| **Chikou Span**               | Lagging line     | Closing price shifted 60 candles backward                |

The bot checks the last two candle closes to see if **the price is inside, above, or below the cloud**,
and detects trend changes (like “broken through” or “bounced off”).


## Development Plan

* [ ] **Slash commands** (Discord.js v14 support)
* [ ] **Chart images** (Plotly.js or QuickChart.io integration)
* [ ] **Data storage (MongoDB)** — save past analyses
* [ ] **Web dashboard** — view analysis history
* [ ] **Multi-timeframe analysis** — compare 15m, 1h, 4h, 1d


## Disclaimer

> This project is for **education and research purposes only.**
> The provided analyses are **not financial advice.**
> Cryptocurrency markets are high-risk — always do your own research.


## Contributing

1. Fork this project
2. Create a new branch: `git checkout -b feature/new-feature`
3. Make your changes and commit them
4. Open a Pull Request 🎉


## License

Published under the **MIT License**.

> “**Markets are clouds — Ichimoku lets you see through them.**” ☁️💹