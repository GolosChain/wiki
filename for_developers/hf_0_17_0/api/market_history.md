# Описание API Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/plugins/market_history/include/golos/plugins/market_history/market_history_plugin.hpp)

Текс подготовил: [@korzunav](https://golos.io/@korzunav).

## market_history
|Название метода|Входные параметры|Возвращаемый обьект|Описание|
|---------------|-----------------|-------------------|--------|
|get_market_history|<ul><li>uint32_t</li><li>time_point_sec</li><li>time_point_sec</li></ul>|[bucket_object](../objects/bucket_object.md)[]|Возвращает историю рынка для внутреннего рынка|
|get_market_history_buckets||flat_set|Возвращает размер секунд стакана(среза), отслеживаемых плагином.|
|get_open_orders|<ul><li>string</li></ul>|[limit_order](../objects/limit_order.md)[]|Отображает список заявок на внутренней бирже на покупку и продажу в сети для указанного пользователя.|
|get_order_book|<ul><li>uint32_t</li></ul>|[order_book](../objects/order_book.md)|Отображает список заявок на внутренней бирже на покупку и продажу в сети|
|get_recent_trades|<ul><li>uint32_t</li></ul>|[market_trade](../objects/market_trade.md)[]|Возвращает N последних сделок для внутреннего рынка|
|get_ticker||[market_ticker](../objects/market_ticker.md)|Возвращает рыночный тикет для внутреннего рынка |
|get_trade_history|<ul><li>time_point_sec</li><li>time_point_sec</li><li>uint32_t</li></ul>|[market_trade](../objects/market_trade.md)[]|Возвращает историю торговли для внутреннего рынка|
|get_volume||[market_volume](../objects/market_volume.md)|Возвращает объем рынка за последние 24 часа|
