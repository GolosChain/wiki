# Описание классов/структур Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/plugins/follow/include/golos/plugins/follow/follow_api_object.hpp)

Подготовил: [@korzunav](https://golos.io/@korzunav).

## comment_feed_entry


|Тип поля|Название поля|Описание|
|--------|-------------|--------|
|social_network::comment_api_object|comment||
|string|reblog_by||
|time_point_sec|reblog_on||
|uint32_t|entry_id|= 0;|
