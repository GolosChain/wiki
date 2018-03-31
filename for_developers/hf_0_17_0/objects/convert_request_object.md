# Описание классов/структур Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Автор: [@korzunav](https://golos.io/@korzunav). Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/libraries/chain/include/golos/chain/steem_objects.hpp)
## convert_request_object


|Тип поля|Название поля|Описание|
|--------|-------------|--------|
|id_type|id||
|account_name_type|owner||
|uint32_t|requestid|= 0; ///< id set by owner, the owner,requestid pair must be unique|
|asset|amount||
|time_point_sec|conversion_date|at this time the feed_history_median_price * amount|
