# Описание классов/структур Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/libraries/chain/include/golos/chain/steem_objects.hpp)

Подготовил: [@korzunav](https://golos.io/@korzunav).

## escrow_object


|Тип поля|Название поля|Описание|
|--------|-------------|--------|
|id_type|id||
|uint32_t|escrow_id|= 20;|
|account_name_type|from||
|account_name_type|to||
|account_name_type|agent||
|time_point_sec|ratification_deadline||
|time_point_sec|escrow_expiration||
|asset|sbd_balance||
|asset|steem_balance||
|asset|pending_fee||
|bool|to_approved|= false;|
|bool|agent_approved|= false;|
|bool|disputed|= false;|
