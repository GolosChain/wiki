# Описание API Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Автор: [@korzunav](https://golos.io/@korzunav). Текст собран на основе исходного кода https://github.com/GolosChain/golos/tree/master/plugins/database_api/include/golos/plugins/database_api/plugin.hpp
## database_api
|Название метода|Входные параметры|Возвращаемый обьект|Описание|
|---------------|-----------------|-------------------|--------|
|get_account_bandwidth|<ul><li>string</li><li>bandwidth_type</li></ul>|[account_bandwidth_api_object](../objects/account_bandwidth_api_object.md)||
|get_account_count||uint64_t|Balances //|
|get_account_history|<ul><li>std::string</li><li>uint64_t</li><li>uint32_t</li></ul>|get_account_history_return_type||
|get_accounts|<ul><li>vector<std::string></li></ul>|[extended_account](../objects/extended_account.md)|Accounts //|
|get_active_witnesses||account_name_type|This API is a short-cut for returning all of the state required for a particular URL with a single query.|
|get_block|<ul><li>uint32_t</li></ul>|[signed_block](../objects/signed_block.md)||
|get_block_header|<ul><li>uint32_t</li></ul>|[block_header](../objects/block_header.md)|Blocks and transactions //|
|get_chain_properties||chain_properties_17||
|get_config||variant_object|Globals //|
|get_conversion_requests|<ul><li>std::string</li></ul>|[convert_request_api_object](../objects/convert_request_api_object.md)||
|get_current_median_history_price||price_17||
|get_dynamic_global_properties||[dynamic_global_property_api_object](../objects/dynamic_global_property_api_object.md)||
|get_escrow|<ul><li>account_name_type</li><li>uint32_t</li></ul>|[escrow_api_object](../objects/escrow_api_object.md)||
|get_feed_history||[feed_history_api_object](../objects/feed_history_api_object.md)||
|get_hardfork_version||hardfork_version||
|get_miner_queue||account_name_type||
|get_next_scheduled_hardfork||[scheduled_hardfork](../objects/scheduled_hardfork.md)||
|get_ops_in_block|<ul><li>uint32_t</li><li>bool</li></ul>|[applied_operation](../objects/applied_operation.md)||
|get_owner_history|<ul><li>string</li></ul>|[owner_authority_history_api_object](../objects/owner_authority_history_api_object.md)||
|get_potential_signatures|<ul><li>signed_transaction</li></ul>|public_key_type|This method will return the set of all public keys that could possibly sign for a given transaction. This call can be used by wallets to filter their set of public keys to just the relevant subset prior to calling @ref get_required_signatures to get the minimum subset.|
|get_recovery_request|<ul><li>account_name_type</li></ul>|[account_recovery_request_api_object](../objects/account_recovery_request_api_object.md)||
|get_required_signatures|<ul><li>signed_transaction</li><li>flat_set<public_key_type></li></ul>|public_key_type|This API will take a partially signed transaction and a set of public keys that the owner has the ability to sign for and return the minimal subset of public keys that should add signatures to the transaction.|
|get_savings_withdraw_from|<ul><li>string</li></ul>|[savings_withdraw_api_object](../objects/savings_withdraw_api_object.md)||
|get_savings_withdraw_to|<ul><li>string</li></ul>|[savings_withdraw_api_object](../objects/savings_withdraw_api_object.md)||
|get_transaction|<ul><li>transaction_id_type</li></ul>|[annotated_signed_transaction](../objects/annotated_signed_transaction.md)||
|get_transaction_hex|<ul><li>signed_transaction</li></ul>|string|Assets // Authority / Validation // @brief Get a hexdump of the serialized binary form of a transaction|
|get_withdraw_routes|<ul><li>string</li><li>withdraw_route_type</li></ul>|[withdraw_route](../objects/withdraw_route.md)||
|get_witness_by_account|<ul><li>std::string</li></ul>|[witness_api_object](../objects/witness_api_object.md)||
|get_witness_count||uint64_t||
|get_witness_schedule||[witness_schedule_api_object](../objects/witness_schedule_api_object.md)||
|get_witnesses|<ul><li>vector<witness_object::id_type></li></ul>|[witness_api_object](../objects/witness_api_object.md)|Witnesses //|
|get_witnesses_by_vote|<ul><li>std::string</li><li>uint32_t</li></ul>|[witness_api_object](../objects/witness_api_object.md)||
|lookup_account_names|<ul><li>vector<std::string></li></ul>|[account_api_object](../objects/account_api_object.md)||
|lookup_accounts|<ul><li>account_name_type</li><li>uint32_t</li></ul>|string||
|lookup_witness_accounts|<ul><li>std::string</li><li>uint32_t</li></ul>|account_name_type||
|set_block_applied_callback|<ul><li>object</li></ul>|[void_type](../objects/void_type.md)||
|verify_account_authority|<ul><li>account_name_type</li><li>flat_set<public_key_type></li></ul>|bool||
|verify_authority|<ul><li>signed_transaction</li></ul>|bool||
