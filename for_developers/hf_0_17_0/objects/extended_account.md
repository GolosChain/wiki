# Описание классов/структур Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/plugins/database_api/include/golos/plugins/database_api/state.hpp)
Текс подготовил: [@korzunav](https://golos.io/@korzunav).
## extended_account

Есть родительский класс: [account_api_object](account_api_object.md)

|Тип поля|Название поля|Описание|
|--------|-------------|--------|
|asset|vesting_balance|convert vesting_shares to vesting steem|
|share_type|reputation|= 0;|
|map<uint64_t,applied_operation>|transfer_history|transfer to/from vesting|
|map<uint64_t,applied_operation>|market_history|limit order / cancel / fill|
|map<uint64_t,applied_operation>|post_history||
|map<uint64_t,applied_operation>|vote_history||
|map<uint64_t,applied_operation>|other_history||
|string|witness_votes||
||tags_usage||
||guest_bloggers||
|map<uint32_t,extended_limit_order>|open_orders||
|string|comments|permlinks for this user|
|string|blog|blog posts for this user|
|string|feed|feed posts for this user|
|string|recent_replies|blog posts for this user|
|map<string,vector<string>>|blog_category|blog posts for this user|
|string|recommended|posts recommened for this user|
