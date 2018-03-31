# Описание классов/структур Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Автор: [@korzunav](https://golos.io/@korzunav). Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/plugins/database_api/include/golos/plugins/database_api/api_objects/account_recovery_request_api_object.hpp)
## account_recovery_request_api_object


|Тип поля|Название поля|Описание|
|--------|-------------|--------|
|account_recovery_request_object::id_type|id||
|account_name_type|account_to_recover||
|[authority](authority.md)|new_owner_authority||
|time_point_sec|expires||
