# CLI кошелек
<!-- toc -->


### cli_walletCommands

#### about()

Returns info such as client version, git version of graphene/fc, version of
boost, openssl.
Returns compile time info and client and dependencies versions
cancel_order(string owner, uint32_t orderid, bool broadcast)

 Cancel an order created with create_order

Parameters:
    owner: The name of the account owning the order to cancel_order (type:
    string)
    orderid: The unique identifier assigned to the order by its creator
    (type: uint32_t)
    broadcast: true if you wish to broadcast the transaction (type: bool) 
#### convert_sbd(string from, asset amount, bool broadcast)

This method will convert SBD to STEEM at the current_median_history price
one week from the time it is executed. This method depends upon there being
a valid price feed.

Parameters:
    from: The account requesting conversion of its SBD i.e. "1.000 SBD"
    (type: string)
    amount: The amount of SBD to convert (type: asset)
    broadcast: true if you wish to broadcast the transaction (type: bool)
#### create_account(string creator, string new_account_name, string json_meta, bool broadcast)

This method will generate new owner, active, and memo keys for the new
account which will be controlable by this wallet. There is a fee associated
with account creation that is paid by the creator. The current account
creation fee can be found with the 'info' wallet command.

Parameters:
    creator: The account creating the new account (type: string)
    new_account_name: The name of the new account (type: string)
    json_meta: JSON Metadata associated with the new account (type: string)
    broadcast: true if you wish to broadcast the transaction (type: bool)
#### create_account_with_keys(string creator, string newname, string json_meta, public_key_type owner, public_key_type active, public_key_type posting, public_key_type memo, bool broadcast)

This method is used by faucets to create new accounts for other users which
must provide their desired keys. The resulting account may not be
controllable by this wallet. There is a fee associated with account
creation that is paid by the creator. The current account creation fee can
be found with the 'info' wallet command.

Parameters:
    creator: The account creating the new account (type: string)
    newname: The name of the new account (type: string)
    json_meta: JSON Metadata associated with the new account (type: string)
    owner: public owner key of the new account (type: public_key_type)
    active: public active key of the new account (type: public_key_type)
    posting: public posting key of the new account (type: public_key_type)
    memo: public memo key of the new account (type: public_key_type)
    broadcast: true if you wish to broadcast the transaction (type: bool)
#### create_order(string owner, uint32_t order_id, asset amount_to_sell, asset min_to_receive, bool fill_or_kill, uint32_t expiration, bool broadcast)

Creates a limit order at the price amount_to_sell / min_to_receive and will
deduct amount_to_sell from account

Parameters:
    owner: The name of the account creating the order (type: string)
    order_id: is a unique identifier assigned by the creator of the order,
    it can be reused after the order has been filled (type: uint32_t)
    amount_to_sell: The amount of either SBD or STEEM you wish to sell
    (type: asset)
    min_to_receive: The amount of the other asset you will receive at a
    minimum (type: asset)
    fill_or_kill: true if you want the order to be killed if it cannot
    immediately be filled (type: bool)
    expiration: the time the order should expire if it has not been filled
    (type: uint32_t)
    broadcast: true if you wish to broadcast the transaction (type: bool)
#### get_account(string account_name)

Returns information about the given account.

Parameters:
    account_name: the name of the account to provide information about
    (type: string)

Returns
    the public account data stored in the blockchain
#### get_account_history(string account, uint32_t from, uint32_t limit)

Account operations have sequence numbers from 0 to N where N is the most
recent operation. This method returns operations in the range [from-limit,
from]

Parameters:
    account: - account whose history will be returned (type: string)
    from: - the absolute sequence number, -1 means most recent, limit is
    the number of operations before from. (type: uint32_t)
    limit: - the maximum number of items that can be queried (0 to 1000],
    must be less than from (type: uint32_t)
#### get_active_witnesses()

Returns the list of witnesses producing blocks in the current round (21
Blocks)
get_block(uint32_t num)

Returns the information about a block

Parameters:
    num: Block num (type: uint32_t)

Returns
    Public block data on the blockchain
#### get_conversion_requests(string owner)

Returns conversion requests by an account

Parameters:
    owner: Account name of the account owning the requests (type: string)

Returns
    All pending conversion requests by account
#### get_feed_history()

Return the current price feed history

Returns
    Price feed history data on the blockchain
#### get_inbox(string account, fc::time_point newest, uint32_t limit)

No help defined for method get_inbox
get_miner_queue()

Returns the queue of pow miners waiting to produce blocks.
#### get_order_book(uint32_t limit)

Gets the current order book for STEEM:SBD

Parameters:
    limit: Maximum number of orders to return for bids and asks. Max is
    1000. (type: uint32_t)
#### get_outbox(string account, fc::time_point newest, uint32_t limit)

No help defined for method get_outbox
#### get_private_key(public_key_type pubkey)

Get the WIF private key corresponding to a public key. The private key must already be in the wallet.
#### get_private_key_from_password(string account, string role, string password)

Parameters:
    role: - active | owner | posting | memo (type: string)
#### get_prototype_operation(string operation_type)

Returns an uninitialized object representing a given blockchain operation.

This returns a default-initialized object of the given type; it can be used during early development of the wallet when we don't yet have custom commands for creating all of the operations the blockchain supports.

Any operation the blockchain supports can be created using the transaction builder's 'add_operation_to_builder_transaction()' , but to do that from the CLI you need to know what the JSON form of the operation looks like.
This will give you a template you can fill in. It's better than nothing.

Parameters:
    operation_type: the type of operation to return, must be one of the
    operations defined in 'steemit/chain/operations.hpp' (e.g.,
    "global_parameters_update_operation") (type: string)

Returns
    a default-constructed operation of the given type
#### get_state(string url)

Returns the state info associated with the URL
#### get_transaction(transaction_id_type trx_id)

Returns transaction by ID.
#### get_witness(string owner_account)

Returns information about the given witness.

Parameters:
    owner_account: the name or id of the witness account owner, or the id
    of the witness (type: string)

Returns
    the information about the witness stored in the block chain
#### gethelp(const string & method)

Returns detailed help on a single API command.

Parameters:
    method: the name of the API command you want help with (type: const
    string &)

Returns
    a multi-line string suitable for displaying on a terminal
#### help()

Returns a list of all commands supported by the wallet API.

This lists each command, along with its arguments and return types. For
more detailed help on a single command, use 'get_help()'

Returns
    a multi-line string suitable for displaying on a terminal
#### import_key(string wif_key)

Imports a WIF Private Key into the wallet to be used to sign transactions
by an account.

example: import_key 5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3

Parameters:
    wif_key: the WIF Private Key to import (type: string)
#### info()

Returns info about the current state of the blockchain
#### is_locked()

Checks whether the wallet is locked (is unable to use its private keys).

This state can be changed by calling 'lock()' or 'unlock()'.

Returns
    true if the wallet is locked
#### is_new()

Checks whether the wallet has just been created and has not yet had a
password set.

Calling 'set_password' will transition the wallet to the locked state.

Returns
    true if the wallet is new
#### list_accounts(const string & lowerbound, uint32_t limit)

Lists all accounts registered in the blockchain. This returns a list of all
account names and their account ids, sorted by account name.

Use the 'lowerbound' and limit parameters to page through the list. To
retrieve all accounts, start by setting 'lowerbound' to the empty string
'""', and then each iteration, pass the last account name returned as the
'lowerbound' for the next 'list_accounts()' call.

Parameters:
    lowerbound: the name of the first account to return. If the named
    account does not exist, the list will start at the account that
    comes after 'lowerbound' (type: const string &)
    limit: the maximum number of accounts to return (max: 1000) (type:
    uint32_t)

Returns
    a list of accounts mapping account names to account ids
#### list_keys()

Dumps all private keys owned by the wallet.

The keys are printed in WIF format. You can import these keys into another
wallet using 'import_key()'

Returns
    a map containing the private keys, indexed by their public key
#### list_my_accounts()

Gets the account information for all accounts for which this wallet has a
private key
list_witnesses(const string & lowerbound, uint32_t limit)

Lists all witnesses registered in the blockchain. This returns a list of
all account names that own witnesses, and the associated witness id, sorted
by name. This lists witnesses whether they are currently voted in or not.

Use the 'lowerbound' and limit parameters to page through the list. To
retrieve all witnesss, start by setting 'lowerbound' to the empty string
'""', and then each iteration, pass the last witness name returned as the
'lowerbound' for the next 'list_witnesss()' call.

Parameters:
    lowerbound: the name of the first witness to return. If the named
    witness does not exist, the list will start at the witness that
    comes after 'lowerbound' (type: const string &)
    limit: the maximum number of witnesss to return (max: 1000) (type:
    uint32_t)

Returns
    a list of witnesss mapping witness names to witness ids
#### load_wallet_file(string wallet_filename)

Loads a specified Graphene wallet.

The current wallet is closed before the new wallet is loaded.

Parameters:
    wallet_filename: the filename of the wallet JSON file to load. If
    'wallet_filename' is empty, it reloads the existing wallet file
    (type: string)

Returns
    true if the specified wallet is loaded
#### lock()

Locks the wallet immediately.
#### network_add_nodes(const vector<string> & nodes)

No help defined for method network_add_nodes
#### network_get_connected_peers()

No help defined for method network_get_connected_peers
#### normalize_brain_key(string s)

Transforms a brain key to reduce the chance of errors when re-entering the
key from memory.

This takes a user-supplied brain key and normalizes it into the form used
for generating private keys. In particular, this upper-cases all ASCII
characters and collapses multiple spaces into one.

Parameters:
    s: the brain key as supplied by the user (type: string)

Returns
    the brain key in its normalized form
#### post_comment(string author, string permlink, string parent_author, string parent_permlink, string title, string body, string json, bool broadcast)

Post or update a comment.

Parameters:
    author: the name of the account authoring the comment (type: string)
    permlink: the accountwide unique permlink for the comment (type:
    string)
    parent_author: can be null if this is a top level comment (type:
    string)
    parent_permlink: becomes category if parent_author is "" (type: string)
    title: the title of the comment (type: string)
    body: the body of the comment (type: string)
    json: the json metadata of the comment (type: string)
    broadcast: true if you wish to broadcast the transaction (type: bool)
#### publish_feed(string witness, price exchange_rate, bool broadcast)

Post or update a comment.

Parameters:
    author: the name of the account authoring the comment (type: string)
    permlink: the accountwide unique permlink for the comment (type:
    string)
    parent_author: can be null if this is a top level comment (type:
    string)
    parent_permlink: becomes category if parent_author is "" (type: string)
    title: the title of the comment (type: string)
    body: the body of the comment (type: string)
    json: the json metadata of the comment (type: string)
    broadcast: true if you wish to broadcast the transaction (type: bool)

unlocked >>> gethelp publish_feed
gethelp publish_feed

A witness can public a price feed for the STEEM:SBD market. The median
price feed is used to process conversion requests from SBD to STEEM.

Parameters:
    witness: The witness publishing the price feed (type: string)
    exchange_rate: The desired exchange rate (type: price)
    broadcast: true if you wish to broadcast the transaction (type: bool)
#### save_wallet_file(string wallet_filename)

Saves the current wallet to the given filename.

Parameters:
    wallet_filename: the filename of the new wallet JSON file to create or
    overwrite. If 'wallet_filename' is empty, save to the current
    filename. (type: string)
#### send_private_message(string from, string to, string subject, string body, bool broadcast)

No help defined for method send_private_message
#### serialize_transaction(signed_transaction tx)

Converts a signed_transaction in JSON form to its binary representation.

TODO: I don't see a broadcast_transaction() function, do we need one?

Parameters:
    tx: the transaction to serialize (type: signed_transaction)

Returns
    the binary form of the transaction. It will not be hex encoded, this
    returns a raw string that may have null characters embedded in it
#### set_password(string password)

Sets a new password on the wallet.

The wallet must be either 'new' or 'unlocked' to execute this command.
set_voting_proxy(string account_to_modify, string proxy, bool broadcast)

Set the voting proxy for an account.

If a user does not wish to take an active part in voting, they can choose
to allow another account to vote their stake.

Setting a vote proxy does not remove your previous votes from the
blockchain, they remain there but are ignored. If you later null out your
vote proxy, your previous votes will take effect again.

This setting can be changed at any time.

Parameters:
    account_to_modify: the name or id of the account to update (type:
    string)
    proxy: the name of account that should proxy to, or empty string to
    have no proxy (type: string)
    broadcast: true if you wish to broadcast the transaction (type: bool)
#### sign_transaction(signed_transaction tx, bool broadcast)

Signs a transaction.

Given a fully-formed transaction that is only lacking signatures, this
signs the transaction with the necessary keys and optionally broadcasts the
transaction

Parameters:
    tx: the unsigned transaction (type: signed_transaction)
    broadcast: true if you wish to broadcast the transaction (type: bool)

Returns
    the signed version of the transaction
#### suggest_brain_key()

Suggests a safe brain key to use for creating your account.
'create_account_with_brain_key()' requires you to specify a 'brain key', a
long passphrase that provides enough entropy to generate cyrptographic
keys. This function will suggest a suitably random string that should be
easy to write down (and, with effort, memorize).

Returns
    a suggested brain_key
#### transfer(string from, string to, asset amount, string memo, bool broadcast)

Transfer funds from one account to another. STEEM and SBD can be
transferred.

Parameters:
    from: The account the funds are coming from (type: string)
    to: The account the funds are going to (type: string)
    amount: The funds being transferred. i.e. "100.000 STEEM" (type: asset)
    memo: A memo for the transactionm, encrypted with the to account's
    public memo key (type: string)
    broadcast: true if you wish to broadcast the transaction (type: bool)
#### transfer_to_vesting(string from, string to, asset amount, bool broadcast)\

Transfer STEEM into a vesting fund represented by vesting shares (VESTS).
VESTS are required to vesting for a minimum of one coin year and can be
withdrawn once a week over a two year withdraw period. VESTS are protected
against dilution up until 90% of STEEM is vesting.

Parameters:
    from: The account the STEEM is coming from (type: string)
    to: The account getting the VESTS (type: string)
    amount: The amount of STEEM to vest i.e. "100.00 STEEM" (type: asset)
    broadcast: true if you wish to broadcast the transaction (type: bool)
#### unlock(string password)

The wallet remain unlocked until the 'lock' is called or the program exits.

Parameters:
    password: the password previously set with 'set_password()' (type:
    string)
#### update_account(string accountname, string json_meta, public_key_type owner, public_key_type active, public_key_type posting, public_key_type memo, bool broadcast)

This method updates the keys of an existing account.

Parameters:
    accountname: The name of the account (type: string)
    json_meta: New JSON Metadata to be associated with the account (type:
    string)
    owner: New public owner key for the account (type: public_key_type)
    active: New public active key for the account (type: public_key_type)
    posting: New public posting key for the account (type: public_key_type)
    memo: New public memo key for the account (type: public_key_type)
    broadcast: true if you wish to broadcast the transaction (type: bool)
#### update_account_auth_account(string account_name, authority_type type, string auth_account, weight_type weight, bool broadcast)


#### update_account_auth_key(string account_name, authority_type type, public_key_type key, weight_type weight, bool broadcast)

This method updates the key of an authority for an exisiting account.
Warning: You can create impossible authorities using this method. The
method will fail if you create an impossible owner authority, but will
allow impossible active and posting authorities.

Parameters:
    account_name: The name of the account whose authority you wish to
    update (type: string)
    type: The authority type. e.g. owner, active, or posting (type:
    authority_type)
    key: The public key to add to the authority (type: public_key_type)
    weight: The weight the key should have in the authority. A weight of 0
    indicates the removal of the key. (type: weight_type)
    broadcast: true if you wish to broadcast the transaction. (type: bool)
#### update_account_auth_threshold(string account_name, authority_type type, uint32_t threshold, bool broadcast)

This method updates the weight threshold of an authority for an account.
Warning: You can create impossible authorities using this method as well as
implicitly met authorities. The method will fail if you create an
implicitly true authority and if you create an impossible owner authoroty,
but will allow impossible active and posting authorities.

Parameters:
    account_name: The name of the account whose authority you wish to
    update (type: string)
    type: The authority type. e.g. owner, active, or posting (type:
    authority_type)
    threshold: The weight threshold required for the authority to be met
    (type: uint32_t)
    broadcast: true if you wish to broadcast the transaction (type: bool)
#### update_account_memo_key(string account_name, public_key_type key, bool broadcast)

This method updates the memo key of an account

Parameters:
    account_name: The name of the account you wish to update (type: string)
    key: The new memo public key (type: public_key_type)
    broadcast: true if you wish to broadcast the transaction (type: bool)
#### update_account_meta(string account_name, string json_meta, bool broadcast)

This method updates the account JSON metadata

Parameters:
    account_name: The name of the account you wish to update (type: string)
    json_meta: The new JSON metadata for the account. This overrides
    existing metadata (type: string)
    broadcast: ture if you wish to broadcast the transaction (type: bool)
#### update_witness(string witness_name, string url, public_key_type block_signing_key, const chain_properties & props, bool broadcast)

Update a witness object owned by the given account.

Parameters:
    witness_name: The name of the witness's owner account. Also accepts the
    ID of the owner account or the ID of the witness. (type: string)
    url: Same as for create_witness. The empty string makes it remain the
    same. (type: string)
    block_signing_key: The new block signing public key. The empty string
    makes it remain the same. (type: public_key_type)
    props: The chain properties the witness is voting on. (type: const
    chain_properties &)
    broadcast: true if you wish to broadcast the transaction. (type: bool)
#### vote(string voter, string author, string permlink, int16_t weight, bool broadcast)

Vote on a comment to be paid STEEM

Parameters:
    voter: The account voting (type: string)
    author: The author of the comment to be voted on (type: string)
    permlink: The permlink of the comment to be voted on. (author,
    permlink) is a unique pair (type: string)
    weight: The weight [-100,100] of the vote (type: int16_t)
    broadcast: true if you wish to broadcast the transaction (type: bool)
#### vote_for_witness(string account_to_vote_with, string witness_to_vote_for, bool approve, bool broadcast)

Vote for a witness to become a block producer. By default an account has
not voted positively or negatively for a witness. The account can either
vote for with positively votes or against with negative votes. The vote
will remain until updated with another vote. Vote strength is determined by
the accounts vesting shares.

Parameters:
    account_to_vote_with: The account voting for a witness (type: string)
    witness_to_vote_for: The witness that is being voted for (type: string)
    approve: true if the account is voting for the account to be able to be
    a block produce (type: bool)
    broadcast: true if you wish to broadcast the transaction (type: bool)
#### withdraw_vesting(string from, asset vesting_shares, bool broadcast)

Set up a vesting withdraw request. The request is fulfilled once a week
over the next two year (104 weeks).

Parameters:
    from: The account the VESTS are withdrawn from (type: string)
    vesting_shares: The amount of VESTS to withdraw over the next two
    years. Each week (amount/104) shares are withdrawn and depositted
    back as STEEM. i.e. "10.000000 VESTS" (type: asset)
    broadcast: true if you wish to broadcast the transaction (type: bool)