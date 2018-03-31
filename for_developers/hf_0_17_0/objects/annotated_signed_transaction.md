# Описание классов/структур Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/libraries/protocol/include/golos/protocol/transaction.hpp)

Текс подготовил: [@korzunav](https://golos.io/@korzunav).

## annotated_signed_transaction

Есть родительский класс: [signed_transaction](signed_transaction.md)

|Тип поля|Название поля|Описание|
|--------|-------------|--------|
|transaction_id_type|transaction_id||
|uint32_t|block_num|= 0;|
|uint32_t|transaction_num|= 0;|
