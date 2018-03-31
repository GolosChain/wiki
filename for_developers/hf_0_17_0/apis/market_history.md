# Описание API Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/plugins/market_history/include/golos/plugins/market_history/market_history_plugin.hpp)

Подготовил: [@korzunav](https://golos.io/@korzunav).

## market_history
### get_market_history

> Возвращает историю рынка для внутреннего рынка

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>uint32_t</li><li>time_point_sec</li><li>time_point_sec</li></ul>|[bucket_object](../objects/bucket_object.md)[]|

### get_market_history_buckets

> Возвращает размер секунд стакана(среза), отслеживаемых плагином.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
||flat_set|

### get_open_orders

> Отображает список заявок на внутренней бирже на покупку и продажу в сети для указанного пользователя.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>string</li></ul>|[limit_order](../objects/limit_order.md)[]|

### get_order_book

> Отображает список заявок на внутренней бирже на покупку и продажу в сети

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>uint32_t</li></ul>|[order_book](../objects/order_book.md)|

### get_recent_trades

> Возвращает N последних сделок для внутреннего рынка

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>uint32_t</li></ul>|[market_trade](../objects/market_trade.md)[]|

### get_ticker

> Возвращает рыночный тикет для внутреннего рынка 

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
||[market_ticker](../objects/market_ticker.md)|

### get_trade_history

> Возвращает историю торговли для внутреннего рынка

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>time_point_sec</li><li>time_point_sec</li><li>uint32_t</li></ul>|[market_trade](../objects/market_trade.md)[]|

### get_volume

> Возвращает объем рынка за последние 24 часа

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
||[market_volume](../objects/market_volume.md)|

