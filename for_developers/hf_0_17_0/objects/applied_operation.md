# Описание классов/структур Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Автор: [@korzunav](https://golos.io/@korzunav). Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/plugins/database_api/include/golos/plugins/database_api/applied_operation.hpp)
## applied_operation


|Тип поля|Название поля|Описание|
|--------|-------------|--------|
|transaction_id_type|trx_id||
|uint32_t|block|= 0;|
|uint32_t|trx_in_block|= 0;|
|uint16_t|op_in_trx|= 0;|
|uint64_t|virtual_op|= 0;|
|time_point_sec|timestamp||
|[operation](operation.md)|op||
