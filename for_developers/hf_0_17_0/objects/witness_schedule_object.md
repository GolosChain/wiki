# Описание классов/структур Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Автор: [@korzunav](https://golos.io/@korzunav). Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/libraries/chain/include/golos/chain/witness_objects.hpp)
## witness_schedule_object


|Тип поля|Название поля|Описание|
|--------|-------------|--------|
|id_type|id||
|uint128_t|current_virtual_time||
|uint32_t|next_shuffle_block_num|= 1;|
|account_name_type|current_shuffled_witnesses||
|uint8_t|num_scheduled_witnesses|= 1;|
|uint8_t|top19_weight|= 1;|
|uint8_t|timeshare_weight|= 5;|
|uint8_t|miner_weight|= 1;|
|uint32_t|witness_pay_normalization_factor|= 25;|
|[chain_properties](chain_properties.md)|median_props||
|[version](version.md)|majority_version||
