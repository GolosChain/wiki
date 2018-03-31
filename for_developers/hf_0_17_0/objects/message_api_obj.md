# Описание классов/структур Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/plugins/private_message/include/golos/plugins/private_message/private_message_objects.hpp)

Подготовил: [@korzunav](https://golos.io/@korzunav).

## message_api_obj


|Тип поля|Название поля|Описание|
|--------|-------------|--------|
|[message_id_type](message_id_type.md)|id||
|account_name_type|from||
|account_name_type|to||
|public_key_type|from_memo_key||
|public_key_type|to_memo_key||
|uint64_t|sent_time||
|time_point_sec|receive_time||
|uint32_t|checksum||
|char|encrypted_message||
