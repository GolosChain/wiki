# Описание API Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Автор: [@korzunav](https://golos.io/@korzunav). Текст собран на основе исходного кода https://github.com/GolosChain/golos/tree/master/plugins/market_history/include/golos/plugins/market_history/market_history_plugin.hpp
## market_history
|Название метода|Входные параметры|Возвращаемый обьект|Описание|
|---------------|-----------------|-------------------|--------|
|get_market_history|<ul><li>uint32_t</li><li>time_point_sec</li><li>time_point_sec</li></ul>|[bucket_object](../objects/bucket_object.md)||
|get_market_history_buckets||flat_set||
|get_open_orders|<ul><li>string</li></ul>|[limit_order](../objects/limit_order.md)||
|get_order_book|<ul><li>uint32_t</li></ul>|[order_book](../objects/order_book.md)||
|get_recent_trades|<ul><li>uint32_t</li></ul>|[market_trade](../objects/market_trade.md)||
|get_ticker||[market_ticker](../objects/market_ticker.md)||
|get_trade_history|<ul><li>time_point_sec</li><li>time_point_sec</li><li>uint32_t</li></ul>|[market_trade](../objects/market_trade.md)||
|get_volume||[market_volume](../objects/market_volume.md)||
