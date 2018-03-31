# Описание классов/структур Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/libraries/protocol/include/golos/protocol/transaction.hpp)

Текс подготовил: [@korzunav](https://golos.io/@korzunav).

## transaction


|Тип поля|Название поля|Описание|
|--------|-------------|--------|
|uint16_t|ref_block_num|= 0;|
|uint32_t|ref_block_prefix|= 0;|
|time_point_sec|expiration||
|[operation](operation.md)|operations||
|[extensions_type](extensions_type.md)|extensions||
