# Описание классов/структур Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/libraries/chain/include/golos/chain/steem_objects.hpp)
Текс подготовил: [@korzunav](https://golos.io/@korzunav).
## limit_order_object


|Тип поля|Название поля|Описание|
|--------|-------------|--------|
|id_type|id||
|time_point_sec|created||
|time_point_sec|expiration||
|account_name_type|seller||
|uint32_t|orderid|= 0;|
|share_type|for_sale|asset id is sell_price.base.symbol|
|[price](price.md)|sell_price||
