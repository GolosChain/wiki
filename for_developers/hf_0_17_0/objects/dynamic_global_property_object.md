# Описание классов/структур Golos | актуально для [hf 0.17.0](https://github.com/GolosChain/golos/releases/tag/v0.17.0)
Текст собран на основе [исходного кода](https://github.com/GolosChain/golos/tree/master/libraries/chain/include/golos/chain/global_property_object.hpp)

Подготовил: [@korzunav](https://golos.io/@korzunav).

## dynamic_global_property_object


|Тип поля|Название поля|Описание|
|--------|-------------|--------|
|id_type|id||
|uint32_t|head_block_number|= 0;|
|[block_id_type](block_id_type.md)|head_block_id||
|time_point_sec|time||
|account_name_type|current_witness||
|uint64_t|total_pow|The total POW accumulated, aka the sum of num_pow_witness at the time new POW is added (= -1;)|
|uint32_t|num_pow_witnesses|The current count of how many pending POW witnesses there are, determines the difficulty of doing pow (= 0;)|
|asset|virtual_supply|= asset(0, STEEM_SYMBOL);|
|asset|current_supply|= asset(0, STEEM_SYMBOL);|
|asset|confidential_supply|= asset(0, STEEM_SYMBOL); ///< total asset held in confidential balances|
|asset|current_sbd_supply|= asset(0, SBD_SYMBOL);|
|asset|confidential_sbd_supply|= asset(0, SBD_SYMBOL); ///< total asset held in confidential balances|
|asset|total_vesting_fund_steem|= asset(0, STEEM_SYMBOL);|
|asset|total_vesting_shares|= asset(0, VESTS_SYMBOL);|
|asset|total_reward_fund_steem|= asset(0, STEEM_SYMBOL);|
|uint128_t|total_reward_shares2|the running total of REWARD^2|
|uint16_t|sbd_interest_rate|This property defines the interest rate that SBD deposits receive. (= 0;)|
|uint16_t|sbd_print_rate|= STEEMIT_100_PERCENT;|
|uint32_t|average_block_size|Average block size is updated every block to be: average_block_size = (99 * average_block_size + new_block_size) / 100 This property is used to update the current_reserve_ratio to maintain approximately 50% or less utilization of network capacity. (= 0;)|
|uint32_t|maximum_block_size|Maximum block size is decided by the set of active witnesses which change every round. Each witness posts what they think the maximum size should be as part of their witness properties, the median size is chosen to be the maximum block size for the round. @note the minimum value for maximum_block_size is defined by the protocol to prevent the network from getting stuck by witnesses attempting to set this too low. (= 0;)|
|uint64_t|current_aslot|The current absolute slot number. Equal to the total number of slots since genesis. Also equal to the total number of missed slots plus head_block_number. (= 0;)|
|uint128_t|recent_slots_filled|used to compute witness participation.|
|uint8_t|participation_count|= 0; ///< Divide by 128 to compute participation percentage|
|uint32_t|last_irreversible_block_num|= 0;|
|uint64_t|max_virtual_bandwidth|The maximum bandwidth the blockchain can support is: max_bandwidth = maximum_block_size * STEEMIT_BANDWIDTH_AVERAGE_WINDOW_SECONDS / STEEMIT_BLOCK_INTERVAL The maximum virtual bandwidth is: max_bandwidth * current_reserve_ratio (= 0;)|
|uint64_t|current_reserve_ratio|Any time average_block_size <= 50% maximum_block_size this value grows by 1 until it reaches STEEMIT_MAX_RESERVE_RATIO. Any time average_block_size is greater than 50% it falls by 1%. Upward adjustments happen once per round, downward adjustments happen every block. (= 1;)|
|uint32_t|vote_regeneration_per_day|The number of votes regenerated per day. Any user voting slower than this rate will be "wasting" voting power through spillover; any user voting faster than this rate will have their votes reduced. (= 40;)|
