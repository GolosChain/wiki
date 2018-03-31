# Описание классов/структур Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/plugins/database_api/include/golos/plugins/database_api/api_objects/account_api_object.hpp)

Текс подготовил: [@korzunav](https://golos.io/@korzunav).

## account_api_object


|Тип поля|Название поля|Описание|
|--------|-------------|--------|
|account_object::id_type|id||
|account_name_type|name||
|[authority](authority.md)|owner||
|[authority](authority.md)|active||
|[authority](authority.md)|posting||
|public_key_type|memo_key||
|string|json_metadata||
|account_name_type|proxy||
|time_point_sec|last_owner_update||
|time_point_sec|last_account_update||
|time_point_sec|created||
|bool|mined||
|bool|owner_challenged||
|bool|active_challenged||
|time_point_sec|last_owner_proved||
|time_point_sec|last_active_proved||
|account_name_type|recovery_account||
|account_name_type|reset_account||
|time_point_sec|last_account_recovery||
|uint32_t|comment_count||
|uint32_t|lifetime_vote_count||
|uint32_t|post_count||
|bool|can_vote||
|uint16_t|voting_power||
|time_point_sec|last_vote_time||
|asset|balance||
|asset|savings_balance||
|asset|sbd_balance||
|uint128_t|sbd_seconds||
|time_point_sec|sbd_seconds_last_update||
|time_point_sec|sbd_last_interest_payment||
|asset|savings_sbd_balance||
|uint128_t|savings_sbd_seconds||
|time_point_sec|savings_sbd_seconds_last_update||
|time_point_sec|savings_sbd_last_interest_payment||
|uint8_t|savings_withdraw_requests||
|share_type|curation_rewards||
|share_type|posting_rewards||
|asset|vesting_shares||
|asset|delegated_vesting_shares||
|asset|received_vesting_shares||
|asset|vesting_withdraw_rate||
|time_point_sec|next_vesting_withdrawal||
|share_type|withdrawn||
|share_type|to_withdraw||
|uint16_t|withdraw_routes||
|share_type|proxied_vsf_votes||
|uint16_t|witnesses_voted_for||
|share_type|average_bandwidth|= 0;|
|share_type|lifetime_bandwidth|= 0;|
|time_point_sec|last_bandwidth_update||
|share_type|average_market_bandwidth|= 0;|
|time_point_sec|last_market_bandwidth_update||
|time_point_sec|last_post||
|time_point_sec|last_root_post||
|share_type|post_bandwidth|= STEEMIT_100_PERCENT;|
|share_type|new_average_bandwidth||
|share_type|new_average_market_bandwidth||
