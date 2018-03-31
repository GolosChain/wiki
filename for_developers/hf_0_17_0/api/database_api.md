# Описание API Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/plugins/database_api/include/golos/plugins/database_api/plugin.hpp)

Текс подготовил: [@korzunav](https://golos.io/@korzunav).

## database_api
|Название метода|Входные параметры|Возвращаемый обьект|Описание|
|---------------|-----------------|-------------------|--------|
|get_account_bandwidth|<ul><li>string</li><li>bandwidth_type</li></ul>|[account_bandwidth_api_object](../objects/account_bandwidth_api_object.md)|Отображает действия пользователя в зависимости от типа|
|get_account_count||uint64_t|Возвращает количество зарегестрированных пользователей.|
|get_account_history|<ul><li>std::string</li><li>uint64_t</li><li>uint32_t</li></ul>|get_account_history_return_type|История всех действий пользователя в сети в виде транзакций. При from = -1 будут показаны последние {limit+1} элементов истории. Параметр limit не должен превышать from (исключение from = -1), так как показываются предшествующие {from} элементы истории.|
|get_accounts|<ul><li>vector<std::string></li></ul>|[extended_account](../objects/extended_account.md)[]|Возращает данные по заданным аккаунтам|
|get_active_witnesses||account_name_type[]|This API is a short-cut for returning all of the state required for a particular URL with a single query.|
|get_block|<ul><li>uint32_t</li></ul>|[signed_block](../objects/signed_block.md)|Возвращает все данные о блоке включая транзакции|
|get_block_header|<ul><li>uint32_t</li></ul>|[block_header](../objects/block_header.md)|Возвращает все данные о блоке|
|get_chain_properties||chain_properties_17|Отображает комиссию за создание пользователя, максимальный размер блока и процентную ставку GBG.|
|get_config||variant_object|Отображает текущую конфигурацию узла.|
|get_conversion_requests|<ul><li>std::string</li></ul>|[convert_request_api_object](../objects/convert_request_api_object.md)[]|Возвращает текущие запросы на конвертацию указанным пользователем|
|get_current_median_history_price||price_17|Отображает текущую медианную цену конвертации|
|get_dynamic_global_properties||[dynamic_global_property_api_object](../objects/dynamic_global_property_api_object.md)|Отображает информацию о текущем состоянии сети|
|get_escrow|<ul><li>account_name_type</li><li>uint32_t</li></ul>|[escrow_api_object](../objects/escrow_api_object.md)|Возвращает операции реализованные с помощью посредничества.|
|get_feed_history||[feed_history_api_object](../objects/feed_history_api_object.md)|Отображает историю конверсий|
|get_hardfork_version||hardfork_version|Отображает текущую версию сети.|
|get_miner_queue||account_name_type[]|Создает список майнеров, ожидающих попасть в DPOW цепочку, чтобы создать блок.|
|get_next_scheduled_hardfork||[scheduled_hardfork](../objects/scheduled_hardfork.md)|Отображает дату и версию HardFork|
|get_ops_in_block|<ul><li>uint32_t</li><li>bool</li></ul>|[applied_operation](../objects/applied_operation.md)[]|Возвращает все операции в блоке, если параметр 'onlyVirtual' true то возвращает только виртуальные операции|
|get_owner_history|<ul><li>string</li></ul>|[owner_authority_history_api_object](../objects/owner_authority_history_api_object.md)[]|Отображает имя пользователя если он изменил право собственности на блокчейн|
|get_potential_signatures|<ul><li>signed_transaction</li></ul>|public_key_type[]|This method will return the set of all public keys that could possibly sign for a given transaction. This call can be used by wallets to filter their set of public keys to just the relevant subset prior to calling @ref get_required_signatures to get the minimum subset.|
|get_recovery_request|<ul><li>account_name_type</li></ul>|[account_recovery_request_api_object](../objects/account_recovery_request_api_object.md)|Возвращает true если пользователь в статусе на восстановление.|
|get_required_signatures|<ul><li>signed_transaction</li><li>flat_set<public_key_type></li></ul>|public_key_type[]|This API will take a partially signed transaction and a set of public keys that the owner has the ability to sign for and return the minimal subset of public keys that should add signatures to the transaction.|
|get_savings_withdraw_from|<ul><li>string</li></ul>|[savings_withdraw_api_object](../objects/savings_withdraw_api_object.md)[]|Возвращает данные о выводах из 'СЕЙФА' для данного пользователя|
|get_savings_withdraw_to|<ul><li>string</li></ul>|[savings_withdraw_api_object](../objects/savings_withdraw_api_object.md)[]|Возвращает данные о выводах из 'СЕЙФА' для данного пользователя|
|get_transaction|<ul><li>transaction_id_type</li></ul>|[annotated_signed_transaction](../objects/annotated_signed_transaction.md)|Отображает детали транзакции по заданному ID транзакции.|
|get_transaction_hex|<ul><li>signed_transaction</li></ul>|string|Отображает HEX строку транзакции.|
|get_withdraw_routes|<ul><li>string</li><li>withdraw_route_type</li></ul>|[withdraw_route](../objects/withdraw_route.md)[]|Возвращает все переводы на счету пользователя в зависимости от типа|
|get_witness_by_account|<ul><li>std::string</li></ul>|[witness_api_object](../objects/witness_api_object.md)|Отображает данные о делегате (если он им является) в соответствии с данными из запроса|
|get_witness_count||uint64_t|Отображает количество делегатов.|
|get_witness_schedule||[witness_schedule_api_object](../objects/witness_schedule_api_object.md)|Отображает текущее состояние делегирования.|
|get_witnesses|<ul><li>vector<witness_object::id_type></li></ul>|[witness_api_object](../objects/witness_api_object.md)[]|Отображает данные о делегатах в соответствии с заданными ID|
|get_witnesses_by_vote|<ul><li>std::string</li><li>uint32_t</li></ul>|[witness_api_object](../objects/witness_api_object.md)[]|Отображает ограниченный список делегатов одобряющих голосование.|
|lookup_account_names|<ul><li>vector<std::string></li></ul>|[account_api_object](../objects/account_api_object.md)[]|Возращает данные по заданным аккаунтам|
|lookup_accounts|<ul><li>account_name_type</li><li>uint32_t</li></ul>|string[]|Возвращает имена пользователей близких к шаблону.|
|lookup_witness_accounts|<ul><li>std::string</li><li>uint32_t</li></ul>|account_name_type[]|Отображает ограниченный список пользователей, которые объявили о своем намерении работать в качестве делегата.|
|set_block_applied_callback|<ul><li>object</li></ul>|[void_type](../objects/void_type.md)||
|verify_account_authority|<ul><li>account_name_type</li><li>flat_set<public_key_type></li></ul>|bool|Возвращает  true, если у пользователя есть достаточные полномочия для авторизации учетной записи|
|verify_authority|<ul><li>signed_transaction</li></ul>|bool|Возвращает TRUE если транзакция подписана правильно|
