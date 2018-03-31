# Описание API Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/plugins/market_history/include/golos/plugins/market_history/market_history_plugin.hpp)
Текс подготовил: [@korzunav](https://golos.io/@korzunav).
## market_history
|Название метода|Входные параметры|Возвращаемый обьект|Описание|
|---------------|-----------------|-------------------|--------|
|get_market_history|<ul><li>uint32_t</li><li>time_point_sec</li><li>time_point_sec</li></ul>|[bucket_object](../objects/bucket_object.md)|Returns the market history for the internal market.|
|get_market_history_buckets||flat_set|Returns the bucket seconds being tracked by the plugin|
|get_open_orders|<ul><li>string</li></ul>|[limit_order](../objects/limit_order.md)|Displays a list of orders on the internal exchange for the purchase and sale on the network for the specified user.|
|get_order_book|<ul><li>uint32_t</li></ul>|[order_book](../objects/order_book.md)|Displays a list of applications on the internal exchange for the purchase and sale of the network|
|get_recent_trades|<ul><li>uint32_t</li></ul>|[market_trade](../objects/market_trade.md)|Returns the N most recent trades for the internal market.|
|get_ticker||[market_ticker](../objects/market_ticker.md)|Returns the market ticker for the internal market|
|get_trade_history|<ul><li>time_point_sec</li><li>time_point_sec</li><li>uint32_t</li></ul>|[market_trade](../objects/market_trade.md)|Returns the trade history for the internal market.|
|get_volume||[market_volume](../objects/market_volume.md)|Returns the market volume for the past 24 hours|
