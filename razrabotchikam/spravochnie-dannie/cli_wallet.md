# cli\_wallet

См. также статью [Создание пользователя \(cli\_wallet\)](https://wiki.golos.io/3-guides/stati/sozdanie-polzovatelya-ispolzuya-cliwallet.html)

## cli\_walletCommands

### about\(\)

Returns info such as client version, git version of graphene/fc, version of boost, openssl. Returns compile time info and client and dependencies versions cancel\_order\(string owner, uint32\_t orderid, bool broadcast\)

Cancel an order created with create\_order

Parameters: owner: The name of the account owning the order to cancel\_order \(type: string\) orderid: The unique identifier assigned to the order by its creator \(type: uint32\_t\) broadcast: true if you wish to broadcast the transaction \(type: bool\)

### convert\_sbd\(string from, asset amount, bool broadcast\)

This method will convert SBD to STEEM at the current\_median\_history price one week from the time it is executed. This method depends upon there being a valid price feed.

Parameters: from: The account requesting conversion of its SBD i.e. "1.000 SBD" \(type: string\) amount: The amount of SBD to convert \(type: asset\) broadcast: true if you wish to broadcast the transaction \(type: bool\)

### create\_account\(string creator, string new\_account\_name, string json\_meta, bool broadcast\)

This method will generate new owner, active, and memo keys for the new account which will be controlable by this wallet. There is a fee associated with account creation that is paid by the creator. The current account creation fee can be found with the 'info' wallet command.

Parameters: creator: The account creating the new account \(type: string\) new\_account\_name: The name of the new account \(type: string\) json\_meta: JSON Metadata associated with the new account \(type: string\) broadcast: true if you wish to broadcast the transaction \(type: bool\)

### create\_account\_with\_keys\(string creator, string newname, string json\_meta, public\_key\_type owner, public\_key\_type active, public\_key\_type posting, public\_key\_type memo, bool broadcast\)

This method is used by faucets to create new accounts for other users which must provide their desired keys. The resulting account may not be controllable by this wallet. There is a fee associated with account creation that is paid by the creator. The current account creation fee can be found with the 'info' wallet command.

Parameters: creator: The account creating the new account \(type: string\) newname: The name of the new account \(type: string\) json\_meta: JSON Metadata associated with the new account \(type: string\) owner: public owner key of the new account \(type: public\_key\_type\) active: public active key of the new account \(type: public\_key\_type\) posting: public posting key of the new account \(type: public\_key\_type\) memo: public memo key of the new account \(type: public\_key\_type\) broadcast: true if you wish to broadcast the transaction \(type: bool\)

### create\_order\(string owner, uint32\_t order\_id, asset amount\_to\_sell, asset min\_to\_receive, bool fill\_or\_kill, uint32\_t expiration, bool broadcast\)

Creates a limit order at the price amount\_to\_sell / min\_to\_receive and will deduct amount\_to\_sell from account

Parameters: owner: The name of the account creating the order \(type: string\) order\_id: is a unique identifier assigned by the creator of the order, it can be reused after the order has been filled \(type: uint32\_t\) amount\_to\_sell: The amount of either SBD or STEEM you wish to sell \(type: asset\) min\_to\_receive: The amount of the other asset you will receive at a minimum \(type: asset\) fill\_or\_kill: true if you want the order to be killed if it cannot immediately be filled \(type: bool\) expiration: the time the order should expire if it has not been filled \(type: uint32\_t\) broadcast: true if you wish to broadcast the transaction \(type: bool\)

### get\_account\(string account\_name\)

Returns information about the given account.

Parameters: account\_name: the name of the account to provide information about \(type: string\)

Returns the public account data stored in the blockchain

### get\_account\_history\(string account, uint32\_t from, uint32\_t limit\)

Account operations have sequence numbers from 0 to N where N is the most recent operation. This method returns operations in the range \[from-limit, from\]

Parameters: account: - account whose history will be returned \(type: string\) from: - the absolute sequence number, -1 means most recent, limit is the number of operations before from. \(type: uint32\_t\) limit: - the maximum number of items that can be queried \(0 to 1000\], must be less than from \(type: uint32\_t\)

### get\_active\_witnesses\(\)

Returns the list of witnesses producing blocks in the current round \(21 Blocks\) get\_block\(uint32\_t num\)

Returns the information about a block

Parameters: num: Block num \(type: uint32\_t\)

Returns Public block data on the blockchain

### get\_conversion\_requests\(string owner\)

Returns conversion requests by an account

Parameters: owner: Account name of the account owning the requests \(type: string\)

Returns All pending conversion requests by account

### get\_feed\_history\(\)

Return the current price feed history

Returns Price feed history data on the blockchain

### get\_inbox\(string account, fc::time\_point newest, uint32\_t limit\)

No help defined for method get\_inbox get\_miner\_queue\(\)

Returns the queue of pow miners waiting to produce blocks.

### get\_order\_book\(uint32\_t limit\)

Gets the current order book for STEEM:SBD

Parameters: limit: Maximum number of orders to return for bids and asks. Max is 1000. \(type: uint32\_t\)

### get\_outbox\(string account, fc::time\_point newest, uint32\_t limit\)

No help defined for method get\_outbox

### get\_private\_key\(public\_key\_type pubkey\)

Get the WIF private key corresponding to a public key. The private key must already be in the wallet.

### get\_private\_key\_from\_password\(string account, string role, string password\)

Parameters: role: - active \| owner \| posting \| memo \(type: string\)

### get\_prototype\_operation\(string operation\_type\)

Returns an uninitialized object representing a given blockchain operation.

This returns a default-initialized object of the given type; it can be used during early development of the wallet when we don't yet have custom commands for creating all of the operations the blockchain supports.

Any operation the blockchain supports can be created using the transaction builder's 'add\_operation\_to\_builder\_transaction\(\)' , but to do that from the CLI you need to know what the JSON form of the operation looks like. This will give you a template you can fill in. It's better than nothing.

Parameters: operation\_type: the type of operation to return, must be one of the operations defined in 'steemit/chain/operations.hpp' \(e.g., "global\_parameters\_update\_operation"\) \(type: string\)

Returns a default-constructed operation of the given type

### get\_state\(string url\)

Returns the state info associated with the URL

### get\_transaction\(transaction\_id\_type trx\_id\)

Returns transaction by ID.

### get\_witness\(string owner\_account\)

Returns information about the given witness.

Parameters: owner\_account: the name or id of the witness account owner, or the id of the witness \(type: string\)

Returns the information about the witness stored in the block chain

### gethelp\(const string & method\)

Returns detailed help on a single API command.

Parameters: method: the name of the API command you want help with \(type: const string &\)

Returns a multi-line string suitable for displaying on a terminal

### help\(\)

Returns a list of all commands supported by the wallet API.

This lists each command, along with its arguments and return types. For more detailed help on a single command, use 'get\_help\(\)'

Returns a multi-line string suitable for displaying on a terminal

### import\_key\(string wif\_key\)

Imports a WIF Private Key into the wallet to be used to sign transactions by an account.

example: import\_key 5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3

Parameters: wif\_key: the WIF Private Key to import \(type: string\)

### info\(\)

Returns info about the current state of the blockchain

### is\_locked\(\)

Checks whether the wallet is locked \(is unable to use its private keys\).

This state can be changed by calling 'lock\(\)' or 'unlock\(\)'.

Returns true if the wallet is locked

### is\_new\(\)

Checks whether the wallet has just been created and has not yet had a password set.

Calling 'set\_password' will transition the wallet to the locked state.

Returns true if the wallet is new

### list\_accounts\(const string & lowerbound, uint32\_t limit\)

Lists all accounts registered in the blockchain. This returns a list of all account names and their account ids, sorted by account name.

Use the 'lowerbound' and limit parameters to page through the list. To retrieve all accounts, start by setting 'lowerbound' to the empty string '""', and then each iteration, pass the last account name returned as the 'lowerbound' for the next 'list\_accounts\(\)' call.

Parameters: lowerbound: the name of the first account to return. If the named account does not exist, the list will start at the account that comes after 'lowerbound' \(type: const string &\) limit: the maximum number of accounts to return \(max: 1000\) \(type: uint32\_t\)

Returns a list of accounts mapping account names to account ids

### list\_keys\(\)

Dumps all private keys owned by the wallet.

The keys are printed in WIF format. You can import these keys into another wallet using 'import\_key\(\)'

Returns a map containing the private keys, indexed by their public key

### list\_my\_accounts\(\)

Gets the account information for all accounts for which this wallet has a private key list\_witnesses\(const string & lowerbound, uint32\_t limit\)

Lists all witnesses registered in the blockchain. This returns a list of all account names that own witnesses, and the associated witness id, sorted by name. This lists witnesses whether they are currently voted in or not.

Use the 'lowerbound' and limit parameters to page through the list. To retrieve all witnesss, start by setting 'lowerbound' to the empty string '""', and then each iteration, pass the last witness name returned as the 'lowerbound' for the next 'list\_witnesss\(\)' call.

Parameters: lowerbound: the name of the first witness to return. If the named witness does not exist, the list will start at the witness that comes after 'lowerbound' \(type: const string &\) limit: the maximum number of witnesss to return \(max: 1000\) \(type: uint32\_t\)

Returns a list of witnesss mapping witness names to witness ids

### load\_wallet\_file\(string wallet\_filename\)

Loads a specified Graphene wallet.

The current wallet is closed before the new wallet is loaded.

Parameters: wallet\_filename: the filename of the wallet JSON file to load. If 'wallet\_filename' is empty, it reloads the existing wallet file \(type: string\)

Returns true if the specified wallet is loaded

### lock\(\)

Locks the wallet immediately.

### network\_add\_nodes\(const vector & nodes\)

No help defined for method network\_add\_nodes

### network\_get\_connected\_peers\(\)

No help defined for method network\_get\_connected\_peers

### normalize\_brain\_key\(string s\)

Transforms a brain key to reduce the chance of errors when re-entering the key from memory.

This takes a user-supplied brain key and normalizes it into the form used for generating private keys. In particular, this upper-cases all ASCII characters and collapses multiple spaces into one.

Parameters: s: the brain key as supplied by the user \(type: string\)

Returns the brain key in its normalized form

### post\_comment\(string author, string permlink, string parent\_author, string parent\_permlink, string title, string body, string json, bool broadcast\)

Post or update a comment.

Parameters: author: the name of the account authoring the comment \(type: string\) permlink: the accountwide unique permlink for the comment \(type: string\) parent\_author: can be null if this is a top level comment \(type: string\) parent\_permlink: becomes category if parent\_author is "" \(type: string\) title: the title of the comment \(type: string\) body: the body of the comment \(type: string\) json: the json metadata of the comment \(type: string\) broadcast: true if you wish to broadcast the transaction \(type: bool\)

### publish\_feed\(string witness, price exchange\_rate, bool broadcast\)

Post or update a comment.

Parameters: author: the name of the account authoring the comment \(type: string\) permlink: the accountwide unique permlink for the comment \(type: string\) parent\_author: can be null if this is a top level comment \(type: string\) parent\_permlink: becomes category if parent\_author is "" \(type: string\) title: the title of the comment \(type: string\) body: the body of the comment \(type: string\) json: the json metadata of the comment \(type: string\) broadcast: true if you wish to broadcast the transaction \(type: bool\)

unlocked &gt;&gt;&gt; gethelp publish\_feed gethelp publish\_feed

A witness can public a price feed for the STEEM:SBD market. The median price feed is used to process conversion requests from SBD to STEEM.

Parameters: witness: The witness publishing the price feed \(type: string\) exchange\_rate: The desired exchange rate \(type: price\) broadcast: true if you wish to broadcast the transaction \(type: bool\)

### save\_wallet\_file\(string wallet\_filename\)

Saves the current wallet to the given filename.

Parameters: wallet\_filename: the filename of the new wallet JSON file to create or overwrite. If 'wallet\_filename' is empty, save to the current filename. \(type: string\)

### send\_private\_message\(string from, string to, string subject, string body, bool broadcast\)

No help defined for method send\_private\_message

### serialize\_transaction\(signed\_transaction tx\)

Converts a signed\_transaction in JSON form to its binary representation.

TODO: I don't see a broadcast\_transaction\(\) function, do we need one?

Parameters: tx: the transaction to serialize \(type: signed\_transaction\)

Returns the binary form of the transaction. It will not be hex encoded, this returns a raw string that may have null characters embedded in it

### set\_password\(string password\)

Sets a new password on the wallet.

The wallet must be either 'new' or 'unlocked' to execute this command. set\_voting\_proxy\(string account\_to\_modify, string proxy, bool broadcast\)

Set the voting proxy for an account.

If a user does not wish to take an active part in voting, they can choose to allow another account to vote their stake.

Setting a vote proxy does not remove your previous votes from the blockchain, they remain there but are ignored. If you later null out your vote proxy, your previous votes will take effect again.

This setting can be changed at any time.

Parameters: account\_to\_modify: the name or id of the account to update \(type: string\) proxy: the name of account that should proxy to, or empty string to have no proxy \(type: string\) broadcast: true if you wish to broadcast the transaction \(type: bool\)

### sign\_transaction\(signed\_transaction tx, bool broadcast\)

Signs a transaction.

Given a fully-formed transaction that is only lacking signatures, this signs the transaction with the necessary keys and optionally broadcasts the transaction

Parameters: tx: the unsigned transaction \(type: signed\_transaction\) broadcast: true if you wish to broadcast the transaction \(type: bool\)

Returns the signed version of the transaction

### suggest\_brain\_key\(\)

Suggests a safe brain key to use for creating your account. 'create\_account\_with\_brain\_key\(\)' requires you to specify a 'brain key', a long passphrase that provides enough entropy to generate cyrptographic keys. This function will suggest a suitably random string that should be easy to write down \(and, with effort, memorize\).

Returns a suggested brain\_key

### transfer\(string from, string to, asset amount, string memo, bool broadcast\)

Transfer funds from one account to another. STEEM and SBD can be transferred.

Parameters: from: The account the funds are coming from \(type: string\) to: The account the funds are going to \(type: string\) amount: The funds being transferred. i.e. "100.000 STEEM" \(type: asset\) memo: A memo for the transactionm, encrypted with the to account's public memo key \(type: string\) broadcast: true if you wish to broadcast the transaction \(type: bool\)

### transfer\_to\_vesting\(string from, string to, asset amount, bool broadcast\)\

Transfer STEEM into a vesting fund represented by vesting shares \(VESTS\). VESTS are required to vesting for a minimum of one coin year and can be withdrawn once a week over a two year withdraw period. VESTS are protected against dilution up until 90% of STEEM is vesting.

Parameters: from: The account the STEEM is coming from \(type: string\) to: The account getting the VESTS \(type: string\) amount: The amount of STEEM to vest i.e. "100.00 STEEM" \(type: asset\) broadcast: true if you wish to broadcast the transaction \(type: bool\)

### unlock\(string password\)

The wallet remain unlocked until the 'lock' is called or the program exits.

Parameters: password: the password previously set with 'set\_password\(\)' \(type: string\)

### update\_account\(string accountname, string json\_meta, public\_key\_type owner, public\_key\_type active, public\_key\_type posting, public\_key\_type memo, bool broadcast\)

This method updates the keys of an existing account.

Parameters: accountname: The name of the account \(type: string\) json\_meta: New JSON Metadata to be associated with the account \(type: string\) owner: New public owner key for the account \(type: public\_key\_type\) active: New public active key for the account \(type: public\_key\_type\) posting: New public posting key for the account \(type: public\_key\_type\) memo: New public memo key for the account \(type: public\_key\_type\) broadcast: true if you wish to broadcast the transaction \(type: bool\)

### update\_account\_auth\_account\(string account\_name, authority\_type type, string auth\_account, weight\_type weight, bool broadcast\)

### update\_account\_auth\_key\(string account\_name, authority\_type type, public\_key\_type key, weight\_type weight, bool broadcast\)

This method updates the key of an authority for an exisiting account. Warning: You can create impossible authorities using this method. The method will fail if you create an impossible owner authority, but will allow impossible active and posting authorities.

Parameters: account\_name: The name of the account whose authority you wish to update \(type: string\) type: The authority type. e.g. owner, active, or posting \(type: authority\_type\) key: The public key to add to the authority \(type: public\_key\_type\) weight: The weight the key should have in the authority. A weight of 0 indicates the removal of the key. \(type: weight\_type\) broadcast: true if you wish to broadcast the transaction. \(type: bool\)

### update\_account\_auth\_threshold\(string account\_name, authority\_type type, uint32\_t threshold, bool broadcast\)

This method updates the weight threshold of an authority for an account. Warning: You can create impossible authorities using this method as well as implicitly met authorities. The method will fail if you create an implicitly true authority and if you create an impossible owner authoroty, but will allow impossible active and posting authorities.

Parameters: account\_name: The name of the account whose authority you wish to update \(type: string\) type: The authority type. e.g. owner, active, or posting \(type: authority\_type\) threshold: The weight threshold required for the authority to be met \(type: uint32\_t\) broadcast: true if you wish to broadcast the transaction \(type: bool\)

### update\_account\_memo\_key\(string account\_name, public\_key\_type key, bool broadcast\)

This method updates the memo key of an account

Parameters: account\_name: The name of the account you wish to update \(type: string\) key: The new memo public key \(type: public\_key\_type\) broadcast: true if you wish to broadcast the transaction \(type: bool\)

### update\_account\_meta\(string account\_name, string json\_meta, bool broadcast\)

This method updates the account JSON metadata

Parameters: account\_name: The name of the account you wish to update \(type: string\) json\_meta: The new JSON metadata for the account. This overrides existing metadata \(type: string\) broadcast: ture if you wish to broadcast the transaction \(type: bool\)

### update\_witness\(string witness\_name, string url, public\_key\_type block\_signing\_key, const chain\_properties & props, bool broadcast\)

Update a witness object owned by the given account.

Parameters: witness\_name: The name of the witness's owner account. Also accepts the ID of the owner account or the ID of the witness. \(type: string\) url: Same as for create\_witness. The empty string makes it remain the same. \(type: string\) block\_signing\_key: The new block signing public key. The empty string makes it remain the same. \(type: public\_key\_type\) props: The chain properties the witness is voting on. \(type: const chain\_properties &\) broadcast: true if you wish to broadcast the transaction. \(type: bool\)

### vote\(string voter, string author, string permlink, int16\_t weight, bool broadcast\)

Vote on a comment to be paid STEEM

Parameters: voter: The account voting \(type: string\) author: The author of the comment to be voted on \(type: string\) permlink: The permlink of the comment to be voted on. \(author, permlink\) is a unique pair \(type: string\) weight: The weight \[-100,100\] of the vote \(type: int16\_t\) broadcast: true if you wish to broadcast the transaction \(type: bool\)

### vote\_for\_witness\(string account\_to\_vote\_with, string witness\_to\_vote\_for, bool approve, bool broadcast\)

Vote for a witness to become a block producer. By default an account has not voted positively or negatively for a witness. The account can either vote for with positively votes or against with negative votes. The vote will remain until updated with another vote. Vote strength is determined by the accounts vesting shares.

Parameters: account\_to\_vote\_with: The account voting for a witness \(type: string\) witness\_to\_vote\_for: The witness that is being voted for \(type: string\) approve: true if the account is voting for the account to be able to be a block produce \(type: bool\) broadcast: true if you wish to broadcast the transaction \(type: bool\)

### withdraw\_vesting\(string from, asset vesting\_shares, bool broadcast\)

Set up a vesting withdraw request. The request is fulfilled once a week over the next two year \(104 weeks\).

Parameters: from: The account the VESTS are withdrawn from \(type: string\) vesting\_shares: The amount of VESTS to withdraw over the next two years. Each week \(amount/104\) shares are withdrawn and depositted back as STEEM. i.e. "10.000000 VESTS" \(type: asset\) broadcast: true if you wish to broadcast the transaction \(type: bool\)

