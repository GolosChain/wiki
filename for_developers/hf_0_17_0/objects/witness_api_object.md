# Описание классов/структур Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/plugins/database_api/include/golos/plugins/database_api/api_objects/witness_api_object.hpp)

Подготовил: [@korzunav](https://golos.io/@korzunav).

## witness_api_object


|Тип поля|Название поля|Описание|
|--------|-------------|--------|
|witness_object::id_type|id||
|account_name_type|owner||
|time_point_sec|created||
|string|url||
|uint32_t|total_missed||
|uint64_t|last_aslot||
|uint64_t|last_confirmed_block_num||
|uint64_t|pow_worker||
|public_key_type|signing_key||
|[chain_properties](chain_properties.md)|props||
|[price](price.md)|sbd_exchange_rate||
|time_point_sec|last_sbd_exchange_update||
|share_type|votes||
|uint128_t|virtual_last_update||
|uint128_t|virtual_position||
|uint128_t|virtual_scheduled_time||
|[digest_type](digest_type.md)|last_work||
|[version](version.md)|running_version||
|hardfork_version|hardfork_version_vote||
|time_point_sec|hardfork_time_vote||
