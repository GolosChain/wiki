# Описание классов/структур Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/libraries/protocol/include/golos/protocol/block_header.hpp)

Подготовил: [@korzunav](https://golos.io/@korzunav).

## block_header


|Тип поля|Название поля|Описание|
|--------|-------------|--------|
|[block_id_type](block_id_type.md)|previous||
|time_point_sec|timestamp||
|string|witness||
|[checksum_type](checksum_type.md)|transaction_merkle_root||
|[block_header_extensions_type](block_header_extensions_type.md)|extensions||
