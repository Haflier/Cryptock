# Cryptock

A modular **cryptocurrency market analysis bot** that retrieves market data, calculates technical indicators, generates professional candlestick charts, and delivers them through Telegram.

### Generated Chart

![Generated market chart](screenshots/chart-btc-1d.jpg)

### Telegram Response

![Telegram bot response](screenshots/telegram-bot-response.jpg)

## Features

* 📊 Candlestick charts with configurable SMA
* 📈 Multiple timeframes: `1m`, `5m`, `15m`, `1h`, `4h`, `1d`, `1w`
* 💹 Cryptocurrency and stock/index market data
* 🤖 Telegram bot interface
* 🧩 Clean separation between Domain, Application, Infrastructure, and Presentation
* 🧪 Unit and integration tests

## Telegram Usage

Start the bot with:

```text
/start
```

Generate a chart:

```text
/chart BTC 4h
```

Other examples:

```text
/chart ETH 1h
/chart BTC 1d
/chart AAPL 1d
```

The bot returns the chart together with the latest price and 24-hour information.

## Architecture

TradingBot follows a layered architecture with clear separation of responsibilities and dependency inversion.

```text
TradingBot
│
├── src/
│   │
│   ├── TradingBot.Domain/
│   │   ├── Entities/
│   │   │   └── Candle
│   │   │
│   │   ├── ValueObjects/
│   │   │   └── TradingSymbol
│   │   │
│   │   ├── Enums/
│   │   │   ├── AssetType
│   │   │   └── Timeframe
│   │   │
│   │   └── Errors/
│   │
│   ├── TradingBot.Application/
│   │   ├── Abstractions/
│   │   │   ├── IPriceDataProvider
│   │   │   ├── IPriceDataProviderResolver
│   │   │   ├── IChartGenerator
│   │   │   ├── IChartService
│   │   │   ├── ISmaCalculator
│   │   │   ├── ICommandParser
│   │   │   └── ITelegramSender
│   │   │
│   │   ├── Services/
│   │   │   ├── ChartService
│   │   │   ├── CommandParser
│   │   │   ├── SmaCalculator
│   │   │   ├── SymbolResolver
│   │   │   └── TelegramBotHandler
│   │   │
│   │   ├── DTOs/
│   │   │   ├── ChartRequest
│   │   │   ├── ChartData
│   │   │   ├── GeneratedChart
│   │   │   └── TelegramUpdate
│   │   │
│   │   ├── Results/
│   │   └── Errors/
│   │
│   ├── TradingBot.Infrastructure/
│   │   ├── Providers/
│   │   │   ├── Binance/
│   │   │   │   └── BinancePriceDataProvider
│   │   │   │
│   │   │   ├── TwelveData/
│   │   │   │   ├── TwelveDataClient
│   │   │   │   └── TwelveDataPriceDataProvider
│   │   │   │
│   │   │   └── Yahoo/
│   │   │       └── Legacy provider implementation
│   │   │
│   │   ├── Charts/
│   │   │   └── ScottPlotChartGenerator
│   │   │
│   │   ├── Telegram/
│   │   │   └── TelegramSender
│   │   │
│   │   ├── Configuration/
│   │   └── DependencyInjection
│   │
│   └── TradingBot.Presentation/
│       ├── Telegram/
│       │   └── TelegramPollingService
│       │
│       ├── Program.cs
│       └── appsettings*.json
│
└── tests/
    ├── TradingBot.UnitTests/
    │   ├── Domain/
    │   ├── Services/
    │   └── Infrastructure/
    │
    └── TradingBot.IntegrationTests/
```

## Market Data

Currently supported providers include:

* **Binance** — cryptocurrency market data
* **Twelve Data** — stocks, indices, and additional market data

The provider resolver selects the appropriate provider based on the requested asset.

## Technology

* **.NET 8**
* **C#**
* **ASP.NET Core / Generic Host**
* **ScottPlot 5**
* **Telegram.Bot**
* **Twelve Data API**
* **Binance API**
* **xUnit**

## Testing

The project includes unit and integration tests covering:

* Command parsing
* Market-data providers
* Timeframe mapping
* Candle aggregation
* SMA calculation
* Chart generation
* Telegram handling

## Usage

First you need to get two api keys:

* 1.**Telegram** bot api key(get via https://t.me/BotFather)
* 2.**TwelveData** api key(get via https://twelvedata.com/account/api-keys)

Then inside `src/TradingBot.Presentation/appsettings.json` replace them with YOUR-API-KEY sections.

Run the complete test suite with:

```bash
dotnet test
```

Build the project with:

```bash
dotnet build
```

Run the project in root folder(inside TradingBot):

```bash
dotnet run --project src/TradingBot.Presentation --environment Development
```


## Project Status

🚧 **Active development**

The core market-data, chart generation, and Telegram delivery pipeline is implemented. Additional trading and analysis features are planned.
