# Описание API Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/plugins/follow/include/golos/plugins/follow/plugin.hpp)

Текс подготовил: [@korzunav](https://golos.io/@korzunav).

## follow
|Название метода|Входные параметры|Возвращаемый обьект|Описание|
|---------------|-----------------|-------------------|--------|
|get_account_reputations|<ul><li>account_name_type</li><li>uint32_t</li></ul>|[account_reputation](../objects/account_reputation.md)[]|Возвращает данные о репутации пользователей отфильтрованных по шаблону.|
|get_blog|<ul><li>account_name_type</li><li>uint32_t</li><li>uint32_t</li></ul>|[comment_blog_entry](../objects/comment_blog_entry.md)[]|Возвращает полные данные о записях из блога указанного пользователя.|
|get_blog_authors|<ul><li>account_name_type</li></ul>|blog_authors_r|Gets a list of authors that have had their content reblogged on a given blog account|
|get_blog_entries|<ul><li>account_name_type</li><li>uint32_t</li><li>uint32_t</li></ul>|[blog_entry](../objects/blog_entry.md)[]|Возвращает краткие данные о записях из блога указанного пользователя.|
|get_feed|<ul><li>account_name_type</li><li>uint32_t</li><li>uint32_t</li></ul>|[comment_feed_entry](../objects/comment_feed_entry.md)[]|Возвращает полные данные о записях из ленты указанного пользователя.|
|get_feed_entries|<ul><li>account_name_type</li><li>uint32_t</li><li>uint32_t</li></ul>|[feed_entry](../objects/feed_entry.md)[]|Возвращает краткие данные о записях из ленты указанного пользователя|
|get_follow_count|<ul><li>account_name_type</li></ul>|[follow_count_api_obj](../objects/follow_count_api_obj.md)|Возвращает данные о количестве подписчиков и подписок указанного пользователя.|
|get_followers|<ul><li>account_name_type</li><li>account_name_type</li><li>follow_type</li><li>uint32_t</li></ul>|[follow_api_object](../objects/follow_api_object.md)[]|Возвращает список: Либо всех подписчиков пользователя 'following'. Либо если указано имя пользователя в параметре 'startFollower' возвращается список совпадающих подписчиков.|
|get_following|<ul><li>account_name_type</li><li>account_name_type</li><li>follow_type</li><li>uint32_t</li></ul>|[follow_api_object](../objects/follow_api_object.md)[]||
|get_reblogged_by|<ul><li>account_name_type</li><li>std::string</li></ul>|account_name_type[]|Gets list of accounts that have reblogged a particular post|
