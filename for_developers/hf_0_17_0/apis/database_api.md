# Описание API Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/plugins/database_api/include/golos/plugins/database_api/plugin.hpp)

Подготовил: [@korzunav](https://golos.io/@korzunav).

## database_api
### get_account_bandwidth

> Отображает действия пользователя в зависимости от типа

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>string</li><li>bandwidth_type</li></ul>|[account_bandwidth_api_object](../objects/account_bandwidth_api_object.md)|

### get_account_count

> Возвращает количество зарегестрированных пользователей.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
||uint64_t|

### get_account_history

> История всех действий пользователя в сети в виде транзакций. При from = -1 будут показаны последние {limit+1} элементов истории. Параметр limit не должен превышать from (исключение from = -1), так как показываются предшествующие {from} элементы истории.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>std::string</li><li>uint64_t</li><li>uint32_t</li></ul>|get_account_history_return_type|

### get_accounts

> Возращает данные по заданным аккаунтам

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>vector\<std::string\></li></ul>|[extended_account](../objects/extended_account.md)[]|

### get_active_witnesses

> This API is a short-cut for returning all of the state required for a particular URL with a single query.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
||account_name_type[]|

### get_block

> Возвращает все данные о блоке включая транзакции

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>uint32_t</li></ul>|[signed_block](../objects/signed_block.md)|

### get_block_header

> Возвращает все данные о блоке

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>uint32_t</li></ul>|[block_header](../objects/block_header.md)|

### get_chain_properties

> Отображает комиссию за создание пользователя, максимальный размер блока и процентную ставку GBG.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
||chain_properties_17|

### get_config

> Отображает текущую конфигурацию узла.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
||variant_object|

### get_conversion_requests

> Возвращает текущие запросы на конвертацию указанным пользователем

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>std::string</li></ul>|[convert_request_api_object](../objects/convert_request_api_object.md)[]|

### get_current_median_history_price

> Отображает текущую медианную цену конвертации

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
||price_17|

### get_dynamic_global_properties

> Отображает информацию о текущем состоянии сети

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
||[dynamic_global_property_api_object](../objects/dynamic_global_property_api_object.md)|

### get_escrow

> Возвращает операции реализованные с помощью посредничества.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>account_name_type</li><li>uint32_t</li></ul>|[escrow_api_object](../objects/escrow_api_object.md)|

### get_feed_history

> Отображает историю конверсий

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
||[feed_history_api_object](../objects/feed_history_api_object.md)|

### get_hardfork_version

> Отображает текущую версию сети.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
||hardfork_version|

### get_miner_queue

> Создает список майнеров, ожидающих попасть в DPOW цепочку, чтобы создать блок.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
||account_name_type[]|

### get_next_scheduled_hardfork

> Отображает дату и версию HardFork

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
||[scheduled_hardfork](../objects/scheduled_hardfork.md)|

### get_ops_in_block

> Возвращает все операции в блоке, если параметр 'onlyVirtual' true то возвращает только виртуальные операции

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>uint32_t</li><li>bool</li></ul>|[applied_operation](../objects/applied_operation.md)[]|

### get_owner_history

> Отображает имя пользователя если он изменил право собственности на блокчейн

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>string</li></ul>|[owner_authority_history_api_object](../objects/owner_authority_history_api_object.md)[]|

### get_potential_signatures

> This method will return the set of all public keys that could possibly sign for a given transaction. This call can be used by wallets to filter their set of public keys to just the relevant subset prior to calling @ref get_required_signatures to get the minimum subset.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>signed_transaction</li></ul>|public_key_type[]|

### get_recovery_request

> Возвращает true если пользователь в статусе на восстановление.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>account_name_type</li></ul>|[account_recovery_request_api_object](../objects/account_recovery_request_api_object.md)|

### get_required_signatures

> This API will take a partially signed transaction and a set of public keys that the owner has the ability to sign for and return the minimal subset of public keys that should add signatures to the transaction.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>signed_transaction</li><li>flat_set\<public_key_type\></li></ul>|public_key_type[]|

### get_savings_withdraw_from

> Возвращает данные о выводах из 'СЕЙФА' для данного пользователя

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>string</li></ul>|[savings_withdraw_api_object](../objects/savings_withdraw_api_object.md)[]|

### get_savings_withdraw_to

> Возвращает данные о выводах из 'СЕЙФА' для данного пользователя

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>string</li></ul>|[savings_withdraw_api_object](../objects/savings_withdraw_api_object.md)[]|

### get_transaction

> Отображает детали транзакции по заданному ID транзакции.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>transaction_id_type</li></ul>|[annotated_signed_transaction](../objects/annotated_signed_transaction.md)|

### get_transaction_hex

> Отображает HEX строку транзакции.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>signed_transaction</li></ul>|string|

### get_withdraw_routes

> Возвращает все переводы на счету пользователя в зависимости от типа

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>string</li><li>withdraw_route_type</li></ul>|[withdraw_route](../objects/withdraw_route.md)[]|

### get_witness_by_account

> Отображает данные о делегате (если он им является) в соответствии с данными из запроса

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>std::string</li></ul>|[witness_api_object](../objects/witness_api_object.md)|

### get_witness_count

> Отображает количество делегатов.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
||uint64_t|

### get_witness_schedule

> Отображает текущее состояние делегирования.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
||[witness_schedule_api_object](../objects/witness_schedule_api_object.md)|

### get_witnesses

> Отображает данные о делегатах в соответствии с заданными ID

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>vector\<witness_object::id_type\></li></ul>|[witness_api_object](../objects/witness_api_object.md)[]|

### get_witnesses_by_vote

> Отображает ограниченный список делегатов одобряющих голосование.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>std::string</li><li>uint32_t</li></ul>|[witness_api_object](../objects/witness_api_object.md)[]|

### lookup_account_names

> Возращает данные по заданным аккаунтам

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>vector\<std::string\></li></ul>|[account_api_object](../objects/account_api_object.md)[]|

### lookup_accounts

> Возвращает имена пользователей близких к шаблону.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>account_name_type</li><li>uint32_t</li></ul>|string[]|

### lookup_witness_accounts

> Отображает ограниченный список пользователей, которые объявили о своем намерении работать в качестве делегата.

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>std::string</li><li>uint32_t</li></ul>|account_name_type[]|

### set_block_applied_callback

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>object</li></ul>|[void_type](../objects/void_type.md)|

### verify_account_authority

> Возвращает  true, если у пользователя есть достаточные полномочия для авторизации учетной записи

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>account_name_type</li><li>flat_set\<public_key_type\></li></ul>|bool|

### verify_authority

> Возвращает TRUE если транзакция подписана правильно

|Входные параметры|Возвращаемый обьект|
|-----------------|-------------------|
|<ul><li>signed_transaction</li></ul>|bool|

