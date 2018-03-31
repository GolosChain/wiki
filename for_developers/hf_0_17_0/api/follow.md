# Описание API Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/plugins/follow/include/golos/plugins/follow/plugin.hpp)
Текс подготовил: [@korzunav](https://golos.io/@korzunav).
## follow
|Название метода|Входные параметры|Возвращаемый обьект|Описание|
|---------------|-----------------|-------------------|--------|
|get_account_reputations|<ul><li>account_name_type</li><li>uint32_t</li></ul>|[account_reputation](../objects/account_reputation.md)|Returns data about the reputation of users filtered by template.|
|get_blog|<ul><li>account_name_type</li><li>uint32_t</li><li>uint32_t</li></ul>|[comment_blog_entry](../objects/comment_blog_entry.md)|Returns the complete record data from the blog of the specified user.|
|get_blog_authors|<ul><li>account_name_type</li></ul>|blog_authors_r|Gets a list of authors that have had their content reblogged on a given blog account|
|get_blog_entries|<ul><li>account_name_type</li><li>uint32_t</li><li>uint32_t</li></ul>|[blog_entry](../objects/blog_entry.md)|Returns brief information about records from the blog of the specified user.|
|get_feed|<ul><li>account_name_type</li><li>uint32_t</li><li>uint32_t</li></ul>|[comment_feed_entry](../objects/comment_feed_entry.md)|Returns the complete record data from the specified user's tape.|
|get_feed_entries|<ul><li>account_name_type</li><li>uint32_t</li><li>uint32_t</li></ul>|[feed_entry](../objects/feed_entry.md)|Returns brief information about records from the specified user's tape|
|get_follow_count|<ul><li>account_name_type</li></ul>|[follow_count_api_obj](../objects/follow_count_api_obj.md)|Returns information about the number of subscribers and subscriptions of the specified user.|
|get_followers|<ul><li>account_name_type</li><li>account_name_type</li><li>follow_type</li><li>uint32_t</li></ul>|[follow_api_object](../objects/follow_api_object.md)|Returns the list: Either all of the subscribers of the user are 'following'. Or, if the user name is specified, the list of matching subscribers is returned in the parameter 'startFollower'.|
|get_following|<ul><li>account_name_type</li><li>account_name_type</li><li>follow_type</li><li>uint32_t</li></ul>|[follow_api_object](../objects/follow_api_object.md)||
|get_reblogged_by|<ul><li>account_name_type</li><li>std::string</li></ul>|account_name_type|Gets list of accounts that have reblogged a particular post|
