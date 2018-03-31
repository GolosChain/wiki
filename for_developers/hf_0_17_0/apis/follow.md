# Описание API Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/plugins/follow/include/golos/plugins/follow/plugin.hpp)

Подготовил: [@korzunav](https://golos.io/@korzunav).

## follow
### get_account_reputations

> Возвращает данные о репутации пользователей отфильтрованных по шаблону.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>account_name_type</li><li>uint32_t</li></ul>|[account_reputation](../objects/account_reputation.md)[]|

### get_blog

> Возвращает полные данные о записях из блога указанного пользователя.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>account_name_type</li><li>uint32_t</li><li>uint32_t</li></ul>|[comment_blog_entry](../objects/comment_blog_entry.md)[]|

### get_blog_authors

> Gets a list of authors that have had their content reblogged on a given blog account

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>account_name_type</li></ul>|blog_authors_r|

### get_blog_entries

> Возвращает краткие данные о записях из блога указанного пользователя.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>account_name_type</li><li>uint32_t</li><li>uint32_t</li></ul>|[blog_entry](../objects/blog_entry.md)[]|

### get_feed

> Возвращает полные данные о записях из ленты указанного пользователя.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>account_name_type</li><li>uint32_t</li><li>uint32_t</li></ul>|[comment_feed_entry](../objects/comment_feed_entry.md)[]|

### get_feed_entries

> Возвращает краткие данные о записях из ленты указанного пользователя

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>account_name_type</li><li>uint32_t</li><li>uint32_t</li></ul>|[feed_entry](../objects/feed_entry.md)[]|

### get_follow_count

> Возвращает данные о количестве подписчиков и подписок указанного пользователя.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>account_name_type</li></ul>|[follow_count_api_obj](../objects/follow_count_api_obj.md)|

### get_followers

> Возвращает список: Либо всех подписчиков пользователя 'following'. Либо если указано имя пользователя в параметре 'startFollower' возвращается список совпадающих подписчиков.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>account_name_type</li><li>account_name_type</li><li>follow_type</li><li>uint32_t</li></ul>|[follow_api_object](../objects/follow_api_object.md)[]|

### get_following

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>account_name_type</li><li>account_name_type</li><li>follow_type</li><li>uint32_t</li></ul>|[follow_api_object](../objects/follow_api_object.md)[]|

### get_reblogged_by

> Gets list of accounts that have reblogged a particular post

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>account_name_type</li><li>std::string</li></ul>|account_name_type[]|

