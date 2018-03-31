# Описание классов/структур Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/plugins/market_history/include/golos/plugins/market_history/market_history_objects.hpp)

## market_ticker


|Тип поля|Название поля|Описание|
|--------|-------------|--------|
|double|latest|= 0;|
|double|lowest_ask|= 0;|
|double|highest_bid|= 0;|
|double|percent_change|= 0;|
|asset|steem_volume|= asset(0, STEEM_SYMBOL);|
|asset|sbd_volume|= asset(0, SBD_SYMBOL);|

Подготовил: [@korzunav](https://golos.io/@korzunav).

