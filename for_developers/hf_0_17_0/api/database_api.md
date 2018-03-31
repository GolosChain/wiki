# Описание API Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/plugins/database_api/include/golos/plugins/database_api/plugin.hpp)
Текс подготовил: [@korzunav](https://golos.io/@korzunav).
## database_api
|Название метода|Входные параметры|Возвращаемый обьект|Описание|
|---------------|-----------------|-------------------|--------|
|get_account_bandwidth|<ul><li>string</li><li>bandwidth_type</li></ul>|[account_bandwidth_api_object](../objects/account_bandwidth_api_object.md)|Displays user actions based on type|
|get_account_count||uint64_t|Balances //|
|get_account_history|<ul><li>std::string</li><li>uint64_t</li><li>uint32_t</li></ul>|get_account_history_return_type|The history of all user actions on the network in the form of transactions. If from = -1, then are last {limit+1} history elements are shown. Parameter limit should be less or equals {from} (except from = -1). This is because elements preceding {from} are shown. |
|get_accounts|<ul><li>vector<std::string></li></ul>|[extended_account](../objects/extended_account.md)|Accounts //|
|get_active_witnesses||account_name_type|This API is a short-cut for returning all of the state required for a particular URL with a single query.|
|get_block|<ul><li>uint32_t</li></ul>|[signed_block](../objects/signed_block.md)|Returns block for given number|
|get_block_header|<ul><li>uint32_t</li></ul>|[block_header](../objects/block_header.md)|Blocks and transactions //|
|get_chain_properties||chain_properties_17|Displays the commission for creating the user, the maximum block size and the GBG interest rate.|
|get_config||variant_object|Globals //|
|get_conversion_requests|<ul><li>std::string</li></ul>|[convert_request_api_object](../objects/convert_request_api_object.md)|Returns the current requests for conversion by the specified user|
|get_current_median_history_price||price_17|Displays the current median price of conversion|
|get_dynamic_global_properties||[dynamic_global_property_api_object](../objects/dynamic_global_property_api_object.md)|Displays information about the current network status.|
|get_escrow|<ul><li>account_name_type</li><li>uint32_t</li></ul>|[escrow_api_object](../objects/escrow_api_object.md)|Returns the operations implemented through mediation.|
|get_feed_history||[feed_history_api_object](../objects/feed_history_api_object.md)|Displays the conversion history|
|get_hardfork_version||hardfork_version|Displays the current version of the network.|
|get_miner_queue||account_name_type|Creates a list of the miners waiting to enter the DPOW chain to create the block.|
|get_next_scheduled_hardfork||[scheduled_hardfork](../objects/scheduled_hardfork.md)|Displays the date and version of HardFork|
|get_ops_in_block|<ul><li>uint32_t</li><li>bool</li></ul>|[applied_operation](../objects/applied_operation.md)|Returns all operations in the block, if the parameter 'onlyVirtual' is true, then returns only the virtual operations|
|get_owner_history|<ul><li>string</li></ul>|[owner_authority_history_api_object](../objects/owner_authority_history_api_object.md)|Displays the user name if he changed the ownership of the blockchain|
|get_potential_signatures|<ul><li>signed_transaction</li></ul>|public_key_type|This method will return the set of all public keys that could possibly sign for a given transaction. This call can be used by wallets to filter their set of public keys to just the relevant subset prior to calling @ref get_required_signatures to get the minimum subset.|
|get_recovery_request|<ul><li>account_name_type</li></ul>|[account_recovery_request_api_object](../objects/account_recovery_request_api_object.md)|Returns true if the user is in recovery status.|
|get_required_signatures|<ul><li>signed_transaction</li><li>flat_set<public_key_type></li></ul>|public_key_type|This API will take a partially signed transaction and a set of public keys that the owner has the ability to sign for and return the minimal subset of public keys that should add signatures to the transaction.|
|get_savings_withdraw_from|<ul><li>string</li></ul>|[savings_withdraw_api_object](../objects/savings_withdraw_api_object.md)|Returns the output data from 'SAFE' for this user|
|get_savings_withdraw_to|<ul><li>string</li></ul>|[savings_withdraw_api_object](../objects/savings_withdraw_api_object.md)|Returns the output data from 'SAFE' for this user|
|get_transaction|<ul><li>transaction_id_type</li></ul>|[annotated_signed_transaction](../objects/annotated_signed_transaction.md)|Displays transaction details for the specified transaction ID.|
|get_transaction_hex|<ul><li>signed_transaction</li></ul>|string|Assets // Authority / Validation // @brief Get a hexdump of the serialized binary form of a transaction|
|get_withdraw_routes|<ul><li>string</li><li>withdraw_route_type</li></ul>|[withdraw_route](../objects/withdraw_route.md)|Returns all transfers to the user's account, depending on the type|
|get_witness_by_account|<ul><li>std::string</li></ul>|[witness_api_object](../objects/witness_api_object.md)|Displays data about the delegate (if it is) according to the data from the request|
|get_witness_count||uint64_t|Displays the number of delegates.|
|get_witness_schedule||[witness_schedule_api_object](../objects/witness_schedule_api_object.md)|Displays the current delegation status.|
|get_witnesses|<ul><li>vector<witness_object::id_type></li></ul>|[witness_api_object](../objects/witness_api_object.md)|Witnesses //|
|get_witnesses_by_vote|<ul><li>std::string</li><li>uint32_t</li></ul>|[witness_api_object](../objects/witness_api_object.md)|Displays a limited list of delegates approving the vote.|
|lookup_account_names|<ul><li>vector<std::string></li></ul>|[account_api_object](../objects/account_api_object.md)|Returns data for specified accounts|
|lookup_accounts|<ul><li>account_name_type</li><li>uint32_t</li></ul>|string|Returns the names of users close to the phrase.|
|lookup_witness_accounts|<ul><li>std::string</li><li>uint32_t</li></ul>|account_name_type|Displays a limited list of users who have announced their intention to work as a delegate.|
|set_block_applied_callback|<ul><li>object</li></ul>|[void_type](../objects/void_type.md)||
|verify_account_authority|<ul><li>account_name_type</li><li>flat_set<public_key_type></li></ul>|bool|Return true if the signers have enough authority to authorize an account|
|verify_authority|<ul><li>signed_transaction</li></ul>|bool|Returns TRUE if the transaction is signed correctly|
