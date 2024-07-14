![](../diagrams/10.png)

The **Controller** plays a crucial role within Hummingbot's Strategy V2 framework, serving as the orchestrator of the strategy's overall behavior. It interfaces with the `MarketDataProvider`, which includes OrderBook, Trades, and Candles, and forwards a series of `ExecutorActions` to the main strategy. The strategy then evaluates these actions, deciding to execute them based on its overarching rules and guidelines.

Users can now use controllers as sub-strategies allowing them to use multiple controllers in a single script or trade multiple pairs / configs in a single bot. 

## Background / Motivation

The main idea here is to have top-level reusable templates for certain types of strategies (e.g., directional, market making, arbitrage).

What you do for e.g. directional trading always follows the same pattern: you have an indicator or a combination of indicators and at a certain point decide to pull the trigger and either enter a trade or exit one. 

A Controller abstracts all that away from you so that you can purely focus on only the signal and configure the rest in a YAML file. This makes your strategy development much faster.

### Multiple Controllers, Risk Management, ...

A more advanced benefit of using controllers is that you can have multiple of them running in parallel, all orchestrated by a custom main strategy that does risk management, for example. This would enable you to run a portfolio of strategies and have centralized control over all sub-strategies (=Controllers).

This, of course, is just one example, but it illustrates the power that you can have by utilizing all components together beyond just the template idea.

## Base Classes

Currently, the controller base classes available are:

* [controller_base.py](https://github.com/hummingbot/hummingbot/blob/master/hummingbot/smart_components/controllers/controller_base.py): Defines `ControllerBase`
* [directional_trading_controller_base.py](https://github.com/hummingbot/hummingbot/blob/development/hummingbot/smart_components/controllers/directional_trading_controller_base.py): Designed for indicator-based directional strategies, inherits from `ControllerBase`
* [market_making_controller_base.py](https://github.com/hummingbot/hummingbot/blob/development/hummingbot/smart_components/controllers/market_making_controller_base.py): Designed for two-side market making strategies, inherits from `ControllerBase`

## Directional Trading Controllers

These strategies aim to profit from predicting the market's direction (up or down) and takes positions based on signals indicating the future price movement.

Suitable for strategies that rely on market trends, momentum, or other indicators predicting price movements. 

Customizing signal generation (`get_signal`) allows users to change various analytical models to generate trade signals and determine the conditions under which trades should be executed or stopped.

- [bollinger_v1](https://github.com/hummingbot/hummingbot/blob/development/controllers/directional_trading/bollinger_v1.py)
- [macd_bb_v1](https://github.com/hummingbot/hummingbot/blob/development/controllers/directional_trading/macd_bb_v1.py)
- [trend_follower_v1](https://github.com/hummingbot/hummingbot/blob/development/controllers/directional_trading/trend_follower_v1.py)
- [dman_v3](https://github.com/hummingbot/hummingbot/blob/development/controllers/directional_trading/dman_v3.py)

## Market Making Controllers

These strategies provide liquidity by placing buy and sell orders near the current market price, aiming to profit from the spread between these orders.

Customization involves defining how price levels are selected (`get_levels_to_execute`), how orders are priced and sized (`get_price_and_amount`), and when orders should be refreshed or stopped early.

User may also adjust the strategy based on market depth, volatility, and other market conditions to optimize spread and order placement.

- [pmm_simple](https://github.com/hummingbot/hummingbot/blob/development/controllers/market_making/pmm_simple.py)
- [pmm_dynamic](https://github.com/hummingbot/hummingbot/blob/development/controllers/market_making/pmm_dynamic.py)
- [dman_maker](https://github.com/hummingbot/hummingbot/blob/development/controllers/market_making/dman_maker.py)
- [dman_maker_v2](https://github.com/hummingbot/hummingbot/blob/master/controllers/market_making/dman_maker_v2.py)


## Other Controllers

- [xemm_multiple_levels](https://github.com/hummingbot/hummingbot/blob/master/controllers/generic/xemm_multiple_levels.py)