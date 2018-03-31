# Описание классов/структур Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Автор: [@korzunav](https://golos.io/@korzunav). Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/plugins/database_api/include/golos/plugins/database_api/api_objects/savings_withdraw_api_object.hpp)
## savings_withdraw_api_object


|Тип поля|Название поля|Описание|
|--------|-------------|--------|
|savings_withdraw_object::id_type|id||
|account_name_type|from||
|account_name_type|to||
|string|memo||
|uint32_t|request_id|= 0;|
|asset|amount||
|time_point_sec|complete||
