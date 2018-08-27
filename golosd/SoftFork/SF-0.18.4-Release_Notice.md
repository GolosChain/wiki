# GOLOS·CORE
# The new version SoftFork 0.18.4

***Golos•Core has announced the new SoftFork version 0.18.0 has been released.
This page provides a detailed outline of some fixes and improvements in this version. These updates are approved by a majority of witnesses.***  


**Reindexing:**  
***SF 0.18.4 requires reindexing from all previous versions.***  


### In this article
<!-- toc -->  
****  

## Additional information in the notifications about signed blocks
In previous versions of SoftFork, a user could subscribe to receive up-to-date information as a notification about new signed blocks appeared in blockchain. The information in the notification was not complete enough and contained nothing about virtual operations in this block. The new SF-0.18.4 version provides a user with the ability to receive notifications about events (virtual operations) related to created block.

### Getting information from notifications about virtual operations in signed blocks

In previous SoftFork versions, a user could subscribe to receive up-to-date information in the form of notifications about new signed blocks appeared in the blockchain. It was done by calling the `set_block_applied_callback()` method. The fact is that the method returned data only about new signed operations in the block without any information about virtual operations of this block.  

In version SF-0.18.4, a configurable parameter `type` is added to the  `set_block_applied_callback()` method, which can take four values. Depending on a set value of this parameter, the user can receive the following information about a block:  
  * signed block;  
  * block header;  
  * virtual operations only;  
  * both a signed block and virtual operations.  

This impovement allows the user to receive not only full information about a signed block, but also a separate content at its discretion including the virtual operations.  


The following table shows correspondence between a set parameter value and a type of information received.    

Parameter value | Alternative parameter value | Notification data type  
:----------------------- |:--------------------------- |:------------------
«block» |  0  |  A signed block (the `signed_block` field)  
«header» | 1 | a block header  (the `block_header` field)  
«virtual_ops» | 2 | Only virtual operations (the `virtual_operations` field)  
«full» | 3 | Both a signed block and virtual operations (the `signed_block` field extended with `virtual_operations`)  

Any other specified value will be treated by the `set_block_applied_callback()` method as a «block».  

The `set_block_applied_callback()` method has been fixed while maintaining backwards compatibility with previous SF versions. This method has a parameter that was not previously used. Its field previously always contained a value of 0. In the new version, this field is assigned to the returned result type. If the field is set to 0, the returned result will match the result of previous versions. Adding three new values to this parameter extends the API capability.  


This method has the following form:  
```cpp
void set_block_applied_callback(
	block_applied_callback_result_type type
)
```
Parameter:  
`type` — a returned result type.  

### Getting notification about transactions on the node that are not in a block
A new operation has been implemented in the new version, this is a notification of a user about the blocks. This operation was developed earlier, but was blocked and never used.  

If a transaction was on a node, but was not approved for execution (not placed in a block), the user could not get information about that transaction by simple calling the `set_block_applied_callback()` method. This problem existed in previous versions and it is no longer in the new version.  

The new operation will send a message to the user about each incoming transaction before it will be apllied. To subscribe to this type of notification, the user has to call the API method `set_pending_transaction_callback()`. The method has no parameters.

### Getting information about rewarding a person who signed a block
A new virtual (not explicitly specified) operation `producer_reward_operation()` has been implemented in the new version. The virtual operation can be emitted on each of the blocks.  It notifies a user about rewarding (in GESTS) a person who signed a block. This is a producer of the block, which may be one of the following persons:  
  * a witness included in the approved list of witnesses;  
  * a witness, randomly chosen;  
  * a miner.  

The virtual operation looks as follows:
```cpp
struct producer_reward_operation {
	account_name_type producer;
	asset vesting_shares
};
```
Parameters:  
`producer` — a person who signed a block;  
`vesting_shares` — amount of reward (in GESTS).  

The virtual operation is stored in a block history and can be requested by the API method `get_ops_in_block()` to get the information about a block producer reward. This method has the following form:  
```cpp
std::vector<applied_operation> get_ops_in_block(
	uint32_t block_num,
	bool only_virtual
)
```
Parameters:   
`block_num` — a block identifier whose virtual operations are requested;  
`only_virtual` — «true», if only virtual operations are requested.  

## Improved error diagnosis   
In previous versions, diagnostic information about errors which occurred in the blockchain, was issued in the form of an uninformative text message.  The text of the message contained only general information and it was difficult for a user to identify the cause of the error.  

To solve this problem the solution has been implemented in the new SF version that provides a user with much more diagnostic information  about an error including a description of the level of the hierarchical structure of the block that is causing the error. The solution is based on breaking all the errors into categories and generating diagnostic information for each category. The information includes a description of the hierarchical structure level of the block that causes the error.  

All errors were analyzed directly in the places of their formation and then classified according to informative signs. As a result of the analysis, three classes of errors were identified:  
1. user errors in setting parameters;  
2. user errors when executing business logic operations (for example, exceeding a bandwidth);  
3. program text errors (internal blockchain errors that are not user-specific).  

Each of these error classes was divided into error subclasses and then into error categories for each subclass. The following error categories were created:  
1. unsupported operation by the blockchain;  
2. error of arguments number passed to a plugin;  
3. parameter value error (syntax checking of a specified parameter value, including: the presence of invalid characters in account names, a correctness of asset units writing, exceeding the maximum number of characters in a comment);  
4. excess of the limit for the operation result issuance;  
5. JSON-API request structure error (for example, an empty field, a request to a non-existent method);  
6. transaction structure violation;  
7. an absence of specified object (for example, an absence of an account with a specified name or transaction with a specified identifier);  
8. lack of a required HardFork version;  
9. business logic error, including:  
— lack of sufficient assets for an operation;  
— exceeding a bandwidth (for example, exceeding a number of posts or votes per a certain period);  
10. server error (for example, lack of access to a server);  
11. attempt to perform an operation on a blocked wallet;  
12. program code error, including:  
— error, which occurs in the using libraries;  
— code error (for example, invalid use of a function within a code).  

Diagnostic information has been generated for each of the error categories and added to the message catalog. Specific errors are separated within each of the category by additional signs. Besides, improved diagnostic information return system about an error that occurred when processing a request in the JSON format.  
Except for category errors under 12 number, all other errors are user-defined and can be analyzed directly by the user.  

This update provides a user with the most complete diagnostic information about an error.  

## New private message operations
Version SF-0.18.4 has a new plugin 'private_message_operations' that extends a user's ability to communicate with other users, as well as to process personal messages.  
The new plugin added the following functionality to a user:  
  * messaging with other users without using tokens;  
  * getting a subscription to (virtual) operations related to the operation of the plugin;  
  * getting a list of users  who are in communication;  
  * creating and setting up an account contact list;  
  * ability to obtain a history of messaging by a given user name;  
  * ability to obtain a history of messaging by a given certain signs (for example, by date or by number of messages);  
  * pagination of messages received during communication;  
  * editing a sent message;  
  * ability to delete a sent message (and mark it as deleted message).  

### Private message operations

#### Sending and editing private messages

The following method is used to send or edit user private messages:  
```cpp
struct private_message_operation {
	account_name_type from;  
	account_name_type to;  
	uint64_t nonce;   
	public_key_type from_memo_key;   
	public_key_type to_memo_key;  
	uint32_t checksum;  
	bool update;  
	vector<char> encrypted_message
};
```
Parameters:  
`from` — account name who sends a message;  
`to` — account name who receives a message;  
`nonce` — arbitrary integer value, unique value of the key part (recommended value is a current time in milliseconds);  
`from_memo_key` — public memo key of a sender account used to encrypt a message;  
`to_memo_key` — public memo key of a receiver account used to encrypt a message;  
`checksum` — a checksum is the encryption key result;  
`update` — "true" if the message is edited (fields `from`, `to`, `nonce`); "false" if the message is created;  
`encrypted_message` — result message.  

To edit private message a user has to specify a unique key (with the values `from`, `to` and `nonce`). To specify this key the user may follow the algorithm for obtaining the `memo` key for operations with funds transfers. 

Inside the encrypted messages there is the structure in JSON format that can be extended (modified). The structure has the following form:  
```cpp
struct  message {  
	string  subject;  
	string body  
};
```
#### Deleting a private message
This operation deletes user private messages from the personal mailboxes of the accounts that are either a sender or a recipient of the messages being deleted. The operation must include an account name that is requesting this operation. This operation is performed by calling the following method:  
```cpp
struct private_delete_message_operation {
	account_name_type requester;  
	account_name_type from;  
	account_name_type to;  
	uint64_t nonce;  
	time_point_sec start_date;  
	time_point_sec stop_date  
};
```
Parameters:  
`requester` — account name that requested the delete operation;  
`from` — account name who sends a message;  
`to` — account name who receives a message;  
`nonce` — a unique value part of the key (thereof fields `from`, `to`, `nonce`);  
`start_date` — timestamp from which messages should be deleted;  
`stop_date` — timestamp at which messages should be deleted.   

To delete a user private message the user has to specify a unique key (with the values `from`, `to` and `nonce`).  To delete a series of messages, the user has also to specify the `start_date-stop_date`message range.  

#### Marking private messages as read
A message (or series of messages) can be marked with a "read" label. This operation is similar to the delete message operation and is called by the following method:  
```cpp
struct private_mark_message_operation {
	account_name_type from;
	account_name_type to;
	uint64_t nonce;
	time_point_sec start_date;
	time_point_sec stop_date
};
```
Parameters:  
`from` — account name who sends a message;  
`to` — account name who receives a message;  
`nonce` — a unique value part of the key (thereof fields `from`, `to`, `nonce`);  
`start_date` — timestamp from which messages should be marked as read;  
`stop_date` — timestamp at which messages should be marked as read.   

To mark a user private message the user has to specify a unique key (with the values `from`, `to` and `nonce`). To mark a series of messages, the user has also to specify the `start_date-stop_date`message range. 

#### Creating and setting up an account contact list
A contact list for monitoring incoming and outgoing messages has been added to the system for processing private messages. The contact list for a user is created immediately after the first message sending (or receiving) with any of accounts. After each sending or receiving message this contact list is updated with new account who have been sharing this message.  

The user himself can add a new account to the contact list who was not yet in communication. The method that creates a new contact is as follows:
```cpp
struct private_contact_operation {
	account_name_type owner;
	account_name_type contact;
	private_contact_type type;
	string json_metadata
};
```
Parameters:  
`owner` — account name that owns the contact list;  
`contact` — account name to add to the contact list;  
`type` — type of contact (`pinned` is to add a contact name);  
`json_metadata` — data in JSON format about the account added to the contact list.

Allowed contact types:  
```cpp
enum private_contact_type {
	unknown = 1, // unknown account name
	pinned = 2,  // the contact that the user adds to the contact list personally
	ignored = 3  // ignored account name 
};
```
To remove a contact from the contact list, the user has to specify the `unknown` type in this method. All messages related to this account will be deleted.  
To add an account to the contact list, the user has to specify the type `pinned`.  
To stop receiving messages from a contact that exists in the contact list,  the user has to specify the type of `ignored`.  
To stop receiving messages from all accounts except those in the contact list, the user has to set up the contact list.  

For setting up a contact list the following method is used:  
```cpp
struct private_settings_operation {
	account_name_type owner;
	bool ignore_messages_from_unknown_contact
};
```
Parameters:  
`owner` — account name that owns the contact list;   
`ignore_messages_from_unknown_contact` — "true" to stop receiving messages from unknown contact. 

### Private message operations supported by the client application cli_wallet
#### Sending private messages
The following method is used in `cli_wallet` to send private messages: 
```cpp
send_private_message(
	string from, 
	string to, 
	message_body message,
	bool broadcast
);
```
Parameters:  
`from` — account name who sends a message;  
`to` — account name who receives a message;  
`message` — message in form ({"subject":"", "body":""});  
`broadcast` — "true" if the operation is forwarded to a daemon; "false" if the transaction is displayed.  

#### Editing private messages
The following method is used in `cli_wallet` to send private messages:  
```cpp
edit_private_message(
	string from, 
	string to, 
	uint64_t nonce, 
	message_body message, 
	bool broadcast
);
```
Parameters:  
`from` — account name who sends a message;    
`to` — account name who receives a message;  
`nonce` — a unique value part of the key (thereof fields `from`, `to`, `nonce`);  
`message` — message in form ({"subject":"", "body":""});  
`broadcast` — "true" if the operation is forwarded to a daemon; "false" if the transaction is displayed.  

#### Getting a list of incoming messages
The following method is used in `cli_wallet` to get a list of incoming messages:  
```cpp
get_private_inbox(
	string to, 
	message_box_query query
);
```
Parameters:  
`to` — account name who receives a message;  
`query` — search option.  

#### Getting a list of outgoing messages
The following method is used in `cli_wallet` to get a list of outgoing messages:  
```cpp
get_private_outbox(
	string from, 
	message_box_query query
);
```
Parameters:  
`from`  — account name who sends a message;  
`query`  — search option.  

#### Receiving a list from a message pipe
The following method is used in `cli_wallet` to receive a list from a message pipe:
```cpp
get_private_thread(
	string from, 
	string to, 
	message_thread_query query
);
```
Parameters:  
`from` — account name who sends a message;  
`to` — account name who receives a message;  
`query` — search option.  

#### Setting up private messages
The following method is used in `cli_wallet` to set up private messages:
```cpp
set_private_settings(
	string owner, 
	settings_api_object settings, 
	bool broadcast
);
```
Parameters:  
`owner` — account name that owns the contact list;  
`settings` — settings;  
`broadcast` — "true" if the operation is forwarded to a daemon; "false" if the transaction is displayed.  

#### Obtaining private message settings
The following method is used in `cli_wallet` to obtaine private message settings:
```cpp
get_private_settings(string owner)
```
Parameter:  
`owner` — account name that owns the contact list.  

#### Add or edit a contact in the private message contact list
The following method is used in `cli_wallet` to add or edit a contact in the contact list: 
```cpp
add_private_contact(
	string owner, 
	string contact, 
	private_contact_type type, 
	string json_metadata, 
	bool broadcast
)
```
Parameters:  
`owner` — account name that owns the contact list;  
`contact` — account name whose contact is added or edited in the contact list;   
`type` — type of contact (`unknown`, `pinned`, `ignored`);  
`json_metadata` —  data in JSON format about the account existed in the contact list;  
`broadcast` — "true" if the operation is forwarded to a daemon; "false" if the transaction is displayed.  

#### Obtaining a list of accounts existed in private message contact list
The following method is used in `cli_wallet` to obtaine a list of contacts: 
```cpp
get_private_contacts(
	string owner, 
	private_contact_type type, 
	int limit, 
	int offset
);
```
Parameters:  
`owner` — account name that owns the contact list;  
`type` — type of contact by which the list is formed;  
`limit` — maximum number of contacts in the formed list;  
`offset` — offset from the start of contacts in the contact list.

#### Obtaining information about a particular contact
The following method is used in `cli_wallet` to obtaine information about a particular contact:
```cpp
get_private_contact(
	string owner, 
	string contact
);
```
Parameters:  
`owner` — account name that owns the contact list;  
`contact` — contact account name.  

#### Deleting a private message from the inbox list
The following method is used in `cli_wallet` to delete a private message from the inbox:
```cpp
delete_inbox_private_message(
	string from, 
	string to, 
	uint64_t nonce, 
	bool broadcast
);
```
Parameters:  
`from` — account name who sends a message;  
`to` — account name who receives a message;  
`nonce` — a unique value part of the key (thereof fields `from`, `to`, `nonce`);  
`broadcast` — "true" if the operation is forwarded to a daemon; "false" if the transaction is displayed.  

#### Deleting a series of private message from the inbox list
The following method is used in `cli_wallet` to delete a series of private message from the inbox list:

```cpp
delete_inbox_private_messages(
	string from, 
	string to, 
	time_point_sec start_date, 
	time_point_sec stop_date, 
	bool broadcast
);
```
Parameters:  
`from` — account name who sends a message;  
`to` — account name who receives a message;  
`start_date` — timestamp from which incoming messages should be deleted;  
`stop_date` — timestamp at which incoming messages should be deleted;  
`broadcast` — "true" if the operation is forwarded to a daemon; "false" if the transaction is displayed.  

#### Deleting a private message from the sent list
The following method is used in `cli_wallet` to delete a private message from the sent list:
```cpp
delete_inbox_private_message(
	string from, 
	string to, 
	uint64_t nonce, 
	bool broadcast
);
```
Parameters:  
`from` — account name who sends a message;  
`to` — account name who receives a message;  
`nonce` — a unique value part of the key (thereof fields `from`, `to`, `nonce`);  
`broadcast` — "true" if the operation is forwarded to a daemon; "false" if the transaction is displayed.  

#### Deleting a series of private messages from the sent list
The following method is used in `cli_wallet` to delete a series of private messages from the sent list:

```cpp
delete_outbox_private_messages(
	string from, 
	string to, 
	time_point_sec start_date, 
	time_point_sec stop_date, 
	bool broadcast
);
```
Parameters:  
`from` — account name who sends a message;  
`to` — account name who receives a message;  
`start_date` — timestamp from which outgoing messages should be deleted;  
`stop_date` — timestamp at which outgoing messages should be deleted;  
`broadcast` — "true" if the operation is forwarded to a daemon; "false" if the transaction is displayed.  

#### Marking private messages as read
The operation is similar to the delete private message operation and is called by the following method:
```cpp
mark_private_message(
	string from, 
	string to, 
	const uint64_t nonce, 
	bool broadcast
);
```
Parameters:  
`from` — account name who sends a message;  
`to` — account name who receives a message;  
`nonce` — a unique value part of the key (thereof fields `from`, `to`, `nonce`);  
`broadcast` — "true" if the operation is forwarded to a daemon; "false" if the transaction is displayed.  

#### Marking a series of private messages as read
The operation is similar to the delete a series of private message operation and is called by the following method:
```cpp
mark_private_messages(
	string from, 
	string to, 
	time_point_sec start_date,
	time_point_sec stop_date, 
	bool broadcast
);
```
Parameters:  
`from` — account name who sends a message;  
`to`  — account name who receives a message;  
`start_date` — timestamp from which messages should be marked as read;  
`stop_date` — timestamp at which messages should be marked as read;  
`broadcast` — "true" if the operation is forwarded to a daemon; "false" if the transaction is displayed.

### User private messages API
#### Getting a list of private messages
When performing the operation of receiving the list of messages, a user receives a structure of the following form:
```cpp
struct message_api_object {
	account_name_type from;  
	account_name_type to;  
	uint64_t nonce;  
	public_key_type from_memo_key;  
	public_key_type to_memo_key;  
	uint32_t checksum;  
	std::vector<char> encrypted_message;  
	time_point_sec create_date;  
	time_point_sec receive_date;  
	time_point_sec read_date;  
	time_point_sec remove_date  
};
```
Parameters:  
`from` — account name who sends a message;  
`to` — account name who receives a message;  
`nonce` — a random number that is necessary for decrypting the message. This is  an integral part of the unique key (it contains the fields `from`,` to` and `nonce`);  
`from_memo_key` — public memo key of a sender account;  
`to_memo_key` — public memo key of a receiver account;  
`checksum` — a checksum is the encryption key result;  
`encrypted_message` — encrypted message;  
`create_date` — date and time when the message was created;  
`receive_date` — date and time when the message was received. If the message is not edited, this value is the same as the value of the `create_data` field;  
`read_date` — date and time the message was read;  
`remove_date` — date and time the message was deleted from the interlocutor's box.

#### View messages received from inbox and outbox

To view the messages stored in the inbox and outbox, the following structure is used:    
```cpp
struct message_box_query {
	set<string> select_accounts;   
	set<string> filter_accounts;  
	time_point_sec newest_date;  
	bool unread_only;  
	uint16_t limit;  
	uint32_t offset  
};
```
Parameters:  
`select_accounts` — a list of accounts names that is selected to see their messages;   
`filter_accounts` — a list of account names that are excluded from viewing their messages;
`newest_date` — date and time from which messages are displayed;  
`unread_only` — show only unread messages;  
`limit` — maximum number of messages issued;  
`offset` — offset from beginning of the list from which messages are issued.  

#### Get a list of incoming messages for a recipient account
To get a list of incoming messages for a recipient account, the following query form is used: 
```cpp
get_inbox(
	string to, 
	message_box_query
)
```
Parameters:  
`to` — account name who receives a message;  
`message_box_query` — request to receive messages;  

The result is a vector receiving of the following form: `vector<message_api_object>`.

#### Getting a list of outgoing messages for a sender account

The following query is used to get a list of outgoing messages for a sender account:  
```cpp
get_outbox(
	from, 
	message_box_query
)
```
Parameters:  
`from` — account name who sends a message;  
`message_box_query` — request to receive messages.  


The result is a vector receiving of the following form: `vector<message_api_object>`.

#### Filtering a message list 
A structure of following form is used for filtering the list of messages in a message pipe (between accounts `from` and `to`) is carried out using structures of the following form: 
```cpp
struct message_thread_query {
	time_point_sec newest_date;  
	bool unread_only;  
	uint16_t limit;  
	uint32_t offset
};
```
Parameters:  
`newest_date` — date and time from which messages are displayed;   
`unread_only` — show only unread messages;  
`limit` — maximum number of messages issued;  
`offset` — offset from beginning of the list from which messages are issued.
#### Obtaining a message flow between a sender and receiver of messages
Receiving a message stream (between accounts `from` and `to`) is performed using a method of the following form:
```cpp
get_thread(
	string from,
	string to, 
	message_thread_query
)
```
Parameters:  
`from` — account name who sends a message;  
`to` — account name who receives a message;  
`message_thread_query` — request to receive message flow.  

The result is a vector receiving of the following form: `vector<message_api_object>`.  

####  Getting up-to-date settings of the private messages module
The following API request is used to obtain the current module settings of private messages:  
```cpp
get_settings(string owner)
```
Parameter:  
`owner` — account name that owns the contact list.  


The result is a structure receiving of the following form:
```cpp
struct settings_api_object {
	bool ignore_messages_from_unknown_contact;
};
```
Parameter:  
`ignore_messages_from_unknown_contact` — "true" if messages from unknown contacts should be blocked.

#### Obtaining data on a size of the contact list

The following query form is used to get information about the contact list size:  
```cpp
get_contacts_size(string owner)
```
Parameter:  
`owner` — account name that owns the contact list.  

The result is a structure receiving of the following form:
```cpp
struct contacts_size_api_object {
	map<private_contact_type, contacts_size_info> size
};
```
Parameters:  
`size` —  size data of the contact list in the form of the following structure:  
```cpp
struct contacts_size_info {  
	int total_contacts;  
	int total_outbox_messages;  
	int unread_outbox_messages;  
	int total_inbox_messages;  
	int unread_inbox_messages  
};
```
`total_contacts` — total number of account (contact) names in the contact list;  
`total_outbox_messages` — total number of outgoing messages;  
`unread_outbox_messages` — total number of unread outgoing messages;  
`total_inbox_messages` — total number of incoming messages;  
`unread_inbox_messages` — total number of unread incoming messages.  

#### Getting information about a single contact
The following query form is used to obtain information about a specific contact:
```cpp
get_contact_info(
	string owner, 
	string contact
)
```
Parameters:  
`owner` — account name that owns the contact list;  
`contact` — account name you want to obtain the data.  

A result of the query is a structure of the following form:
```cpp
struct contact_api_object {
	account_name_type owner;
	account_name_type contact;
	string json_metadata;
	private_contact_type local_type;
	private_contact_type remote_type;
	contact_size_info size
};
```
Parameters:  
`owner` — account name that owns the contact list (up to 16 characters);   
`contact` — account name whose data are issued (up to 16 characters);  
`json_metadata` — account's data in JSON format;  
`local_type` — contact type in the account contact list with name  `owner`;  
`remote_type` — contact type in the account contact list with name `contact`;  
`size` —  size data of the contact list.


#### Getting a complete list of contacts that are in the contact list 

The following query form is used to obtain a complete list of account names (contacts) which are existed in the contact list:
```cpp
get_contacts(
	string owner,
	private_contact_type type,
	int limit,
	int offset
)
```
Parameters:  
`owner` — account name that owns the contact list;  
`type` — type of contacts the user want to get (`unknown`,  `pinned`, `ignored`);  
`limit` — maximum number of contacts to be issued;  
`offset` — offset relative to the beginning of the list from which the contacts are issued.

The result is receiving an array of structures of the following form `vector<contact_api_object>`.


#### Subscribing to events about private messages

To subscribe to private message events, the following call is used:
```cpp
set_callback(callback_query)
```
The `callback_jquery` parameter is a structure of the following form:
```cpp
struct callback_query {
	set<account_name_type> select_accounts;  
	set<account_name_type> filter_accounts;  
	set<callback_event_type> select_events;  
	set<callback_event_type> filter_events  
};  
```
Parameters:  
`select_accounts` — a list of account names that is selected to see their messages;  
`filter_accounts` — a list of account names that are excluded from viewing their messages;  
`select_events` — a list of events to monitor;  
`filter_events` — a list of events to exclude from monitoring.


A list of the events has the `enum` form:
```cpp
enum callback_event_type {
	message, 		// message monitoring
	mark,			// marking messages as read
	remove_inbox, 	// delete incoming messages
	remove_outbox, 	// delete outgoing messages
	contact			// adding account name to the contact list
};
```

If the events `message`, `mark`, `remove_inbox` and `remove_outbox` are occured the following struct will be received:
```cpp
struct callback_message_event {
	callback_event_type type;  // type of contact (`unknown`, `pinned`, `ignored`);  
	message_api_object message // message object 
};
```

If the event `contact` is occured the following struct will be received:  
```cpp
struct callback_contact_event {
	callback_event_type type;	// type of contact (`unknown`, `pinned`, `ignored`);  
	contact_api_object contact	// contact object
};
```
## Additional operations on reblogged posts

In previous versions, а user was given only small opportunities with operations on reblogged posts (for example, the user could not delete them or add their own comment). Version SF-0.18.4 provides the user with such features.  

### Deleting the reblogged post
 In the new version, unlike previous versions, the blogger is given the opportunity to remove reblogs, as well as third-party publications copied randomly to his blog.  To implement this feature, a new method `delete_reblog_operation()` has been added to the `follow` plugin. Calling this method is available from `cli_wallet` using evaluator `custom_json`. The operation of deleting the reblog is formed manually.

Example of calling the operation to delete a reblogged post:
```cpp
begin_builder_transaction
add_operation_to_builder_transaction 0 ["custom_json", {"required_posting_auths":["<reblogger-name>"], \
	"id": "follow", "json":"["delete_reblog", {"account":"<reblogger-name>", \
	"author":"<post-author-name>", "permlink":"<post>"}]"}]
sign_builder_transaction 0 true
``` 
In this example:  
`begin_builder_transaction` — a command to call the transaction, which includes the operation of deleting the reblogged post;  
`add_operation_to_builder_transaction` — a command that the deletion of the reblog that has the identification number “0”;  
`<reblogger-name>` — account name, who is a reblogger;  
`<post-author-name>` — account name of the original post;  
`<post>`  — reblogged post to be deleted;  
`sign_builder_transaction` — a command for signing transaction, which is sending into a demon.  

To delete more than one reblogged post, it needs to create a transaction with multiple operations.


### Adding a comment to the reblog

In the SF-0.18.4 version, unlike the previous ones, the author of the reblog is given the opportunity to add a comment to the reblog. For this purpose, the `reblog_operation` method has been modified in the` follow` plugin. Calling this method is available from `cli_wallet` using the `custom_json` evaluator. The operation of adding a comment to the reblog is formed manually.  

**Changes in the cli_wallet application**  

Calling the operation `reblog_operation` is possible from the `cli_wallet` application. To add a comment to the reblog, the following fields are added to the call:  

Field name    |   Type     |       Functionality  
:--------- |:---------- |:------------  
`body`     | string (UTF-8) | Contains a comment body to be added to the reblog  
`title`       | string (UTF-8) | Contains a comment title to be added to the reblog  
`json_metadata` | string (UTF-8) | Contains a comment metadata in JSON form to be added to the reblog  

**Note:** The `body` field is mandatory in the call for the reblog operation with the addition of a comment.  

A sample invocation of the reblog operation with adding a comment:
```cpp
begin_builder_transaction
add_operation_to_builder_transaction 0 ["custom_json", {"required_posting_auths":["<reblogger-name>"], \
	"id": "follow", "json":"["reblog", {"account":"<reblogger-name>","author":"<post-author-name>", \
	"permlink":"<post>","title":"<comment-title>","body":"<comment-body>"}]"}]
sign_builder_transaction 0 true
```
In this sample:  
`begin_builder_transaction` — a command to call the transaction, which includes the operation the post reblog with adding a comment;  
`add_operation_to_builder_transaction` — команда, формирующая операцию репоста с добавлением комментария, a command that the post reblog that has the identification number “0”;  
 `<reblogger-name>` — account name, who performs the post reblog;  
`<post-author-name>` — account name of the original post;  
`<post>`  — the post to be reblogged;  
`<comment-title>`  — comment title to be added to the reblog;  
`<comment-body>`  — comment body to be added to the reblog;  
`sign_builder_transaction` — a command for signing transaction, which is sending into a demon.  

**Changes in API methods:**  

Responses of API methods `get_blog` and `get_blog_entries` have been supplemented with the fields listed in the following table:  

Field name | Type  |  Functionality 
:------------- |:-------- |:---------------  
`reblog_title`  | string  | Contains the reblog title to which the comment is added  
 `reblog_body` | string  |   Contains the reblog body  
 `reblog_json_metadata` | string  |  Contains the reblog metadata  

Responses of API methods `get_feed`, `get_feed_entries`, `get_discussions_by_blog` and `get_discussions_by_feed` are supplemented with the array of objects `reblog_entries`, the structure of which is supplemented by the fields given in the following table:  

Field name | Type  |  Functionality  
:------------- |:-------- |:---------------  
`author`  | string  | Contains the account name who reblogged the post  
`reblog_title`  | string  | Contains the reblog title to which the comment is added  
 `reblog_body` | string  |   Contains the reblog body  
 `reblog_json_metadata` | string  |  Contains the reblog metadata 

## Ability to tune the configuration file for storing only the necessary information on a node

In order to save memory resources, a user is interested in storing on the blockchain node only necessary data, such as account metadata, memo text (a note) in the operations of savings withdraws, the latest updates of posts and comments. After the post is closed, such information becomes irrelevant and needs to be deleted.  

To store only such information on a blockchain Node, the user does not need to create a node in the full memory configuration option. In previous versions, the user could build a node in the LOW_MEM configuration option to reduce memory consumption. The disadvantage of this method was the following fact. To disable or enable the memory save mode on a node, the user each time had to rebuild the node in either standard or LOW_MEM configuration options. Because it took a long time to rebuild, this had a negative impact on the node performance.

### Ability to tune the configuration file for storing account metadata
New `store-account-metadata` and `store-account-metadata-list` flags have been added to the configuration file `config.ini`, as well as changes to the API library for storing account metadata on a blockchain node (without having to rebuild the node in the configuration option LOW_MEM). Using these flags provides more flexibility to configure a node to store account metadata in memory-saving mode.  
 
To enable or disable the account metadata storage mode, the user only needs to configure the `config.ini` file and restart a node. It is no longer need to restart the node after the synchronization is complete.  


**Changes in the config.ini file**  

The `config.ini` file has been updated with the custom flags given in the following table: 

Name | Type | Default value | Set value  
:-------------- |:------------- |:------------- |:----------------  
`store-account-metadata` | bool | — | "true" — enable the mode for storing metadata for all accounts specified in the flag list `store-account-metadata-list`. "false" —  enable the mode for storing metadata for all accounts  
`store-account-metadata-list` | string | — | List of account names for which metadata is stored  


**Changes in the API library**  

From now on the API method `account_api_object()` uses an empty value of json_metadata without having to set the LOW_MEM mode in case a blockchain node has the metadata storage mode disabled.

### Ability to tune the configuration file for storing the memo field in the operations of savings withdraws
New `store-memo-in-savings-withdraws` flag has been added to the configuration file `config.ini` for storing the `memo` field on a blockchain node for the operations of savings withdraws (without having to rebuild the node in the configuration option LOW_MEM). Using these flags provides more flexibility to configure a node to store account metadata in memory-saving mode.  

To enable or disable mode for storing the `memo` field message, the user only needs to configure the `config.ini` file and restart a node. It is no longer need to restart the node after the synchronization is complete.  


**Changes in the config.ini file**  

The `config.ini` file has been updated with the custom flag given in the following table:  

Name | Type | Default value | Set value  
:------------- |:------------------ |:----------- |:-----------  
`store-memo-in-savings-withdraws` | bool | true |  "true" — enable the mode for storing the `memo` field message for all operations of savings withdraws 

### Ability to tune the configuration file for storing changes in posts and comments, taking into account a history depth of these changes

In previous versions, a user had to rebuild a node each time to enable or disable the mode for storing comments of the `last_update` and `active` fields.  

New `store-comment-last-update` flag has been added to the configuration file `config.ini`, as well as changes to the API library for storing changes in posts and comments on a blockchain node.  

Using this flag allows the user to set a storage history depth based on a date of change and a date of the last update. To enable or disable the mode for storing comments of the `last_update` and `active` fields on a node, the user only needs to configure the `config.ini` file and restart the node. It is no longer need to restart the node after the synchronization is complete.


**Changes in the config.ini file**  

The `config.ini` file has been updated with the custom flag given in the following table:  


Name | Type | Default value | Set value  
:------------- |:------------------ |:----------- |:-----------  
`store-comment-last-update` | bool | true |  "true" — enable mode to store changes in posts and comments. "false" —  disable mode



**Changes in the API library**  

In SF-0.18.4, the `last_update` and `active` fields are optional and may not be present in the return value of the `comment_api_object()` API method.  

Changed names of the arguments presented in the following table.  
 
Old argument name (in previous version) | New argument name (in the SF-0.18.4 version)  
:----------- |:-------------------  
`discussion_helper::impl fill_comment_content` | `fill_comment_info`  
`discussion_helper::discussion_helper() fill_comment_content` | `fill_comment_info`

### Ability to tune the configuration file for storing history of payments for publications 
After the end of all payments for a post publication (to the author, as well as to the persons who took part in the voting), these data become irrelevant and are no longer used in the system. The user, if necessary, can store a history of publishing rewards on a node and use it at their discretion without having to rebuild the node in the configuration option LOW_MEM.  

New `store-comment-rewards` variable has been added to the configuration file `config.ini`, as well as changes to the API library. Using this variable allows the user to store the information about rewards on the blockchain node. From now on the user has to rebuild the node only once after setting configuration file variable.  


**Changes in the API library**  

New parameters (marked with the comment “new parameter”) has been added to the structure `comment_api_object`:
```cpp
struct comment_api_object {
	…
	asset total_payout_value;
	asset beneficiary_payout_value;
	asset beneficiary_gests_payout_value;
	asset curator_payout_value;
	asset curator_gests_payout_value; // new parameter

	share_type author_rewards;
	asset author_gbg_payout_value; // new parameter
	asset author_golos_payout_value; // new parameter
	asset author_gests_payout_value; // new parameter
	…
}
```
Parameters:  
`total_payout_value` —  total amount of payments in GBG;  
`beneficiary_payout_value` — payment to the beneficiary in GBG;  
`beneficiary_gests_payout_value` — payment to the beneficiary in GESTS;  
`curator_payout_value` — payment to the curator in GBG;  
`curator_gests_payout_value` — payment to the curator in GESTS.  
`author_rewards` — reward to the author in GOLOS;  
`author_gbg_payout_value` — payment to the author in GBG;  
`author_golos_payout_value` — payment to the author in GOLOS;  
`author_gests_payout_value` — payment to the author in GESTS.  

### Ability to tune the configuration file for storing both the post and comment contents
A user is given an opportunity to more flexibly configure the file `config.ini` to store the post or comment content until a certain time. New parameters `comment-title-depth`, `comment-body-depth`, `comment-json-metadata-depth` and `set-content-storage-depth-null-after-update` have been added to the configuration file `config.ini`.  

The first three parameters are used to specify a depth of the title, body, and metadata history (or comments), respectively. Specifying zero values for all these parameters in the configuration file means that the content should not be stored. A non-zero integer value of at least one of these parameters means that the corresponding content part is stored until a payment for this post (or comment).  

Setting the parameter `set-content-storing-depth-null-after-update` is used when the user needs to store a content only after updating it. Setting this option automatically cancels the first three options.  

To conserve memory resources, the user can store only a piece of the content without having to rebuild the node in the configuration option LOW_MEM.  
 
The parameters are given in the following table.  

Name | Type | Default value | Set value  
:------------- |:----------- |:----------- |:---------  
`comment-title-depth` | uint32_t | — | Maximum number of blocks to store titles  
`comment-body-depth` | uint32_t | — | Maximum number of blocks to store a post (or comment) body  
`comment-json-metadata-depth` | uint32_t | — | Maximum number of blocks to store metadata in the JSON format  
`set-content-storing-depth-null-after-update`  | bool | "false" | "true" if the content storage depth should be reset after content changes  

### Ability to tune the configuration file for removing obsolete votes

In previous versions, the option LOW_MEMORY_NODE was used to remove obsolete votes. This option could be set either on the command line or in the configuration file using the compile flag "-DLOW_MEMORY_NODE = ON / OFF".  
Obsolete votes were removed after a post closure and all necessary calculations were made. It was done to free a memory from outdated information and, consequently, reduce the time spent on initial synchronization.  

Also, the votes could be removed using another option `clear-votes-before-block, bpo::value<uint32_t>() - >default_value(0)`, which provided removal of all votes to a certain fixed block. The main disadvantage of this method was that it did not provide for removing the newly arrived votes, which were re-accumulated in the system and after a certain time they again would be unnecessary.  

New solution introduced in SF-0.18.4 version to remove obsolete votes. New parameter `clear-votes-old-n-blocks, bpo::value<uint32_t>() - >default_value(0xFFFFFF)` was added to configuration file `config.ini`. This parameter ensures the votes preservation till N blocks, and deleting votes in blocks over N.  
A correspondence between the specified value N and the operation is given in the following table.  

N value        | Number of blocks storable votes | Operation to be performed   |  Comment  
:------------- |:---------- |:---------- |:-------  
N=0          |  0   |  Removal of votes immediately after closing the post | Used when a user does not want to save votes  
N>0          | N | Removing votes from blocks older than N | Used to save votes of N blocks starting from the current block and remove votes in blocks older than N. The N value shows a difference (age) between the current block and the block that has a vote  
N=-1         | All blocks | Removal of votes is not performed | The value «-1» is converted to the maximum possible unsigned number (infinity analog). Used to store voices for a long time  


In all cases, the removal of votes is not performed until a post is closed.  

**Note:** This modification has been implemented at the request of delegates.  

### Ability to tune the configuration file for increasing the number of keywords or phrases to find the right information in the posts

In previous versions, the number of tags (keywords or phrases) in a request to a blockchain Node was limited to five.  which was not always sufficient for a more detailed description of the requested information in posts. This amount was not always sufficient for a more detailed description of the requested information in the posts.  

In the new version, the maximum number of tags in a request to a blockchain Node can be increased to 15 . In addition, the tag size is limited to 512 characters. New parameters `tags-number` and `tag-max-length` have been added to the `config.ini` file, allowing the user more flexibility to customize the operation of the blockchain. The user can specify the number and size of the tag depending on resources of the blockchain nodeю The specified number of tags must not exceed 15.  
 
**Changes in the config.ini file**  
The parameters `tags-number` and `tag-max-length` are given in the following table.  

Name | Type  | Default value  | Comment  
:------------ |:---------- |:---------- |:-----------   
`tags-number` | std::size_t  | 5  | This value can be increased to 15   
`tag-max-length`  | std::size_t  | 512  | It is not recommended to set tag size more than 512 characters  

If `tags-number` takes default value, the changes are backward compatible with previous version.


## Fixed old bug in the cli_wallet application for the case, when an account had a multiple authorities

In previous versions, an account (a user) who has multiple authorities was not able to sign a transaction with using own key when the names of third-party accounts were containing in the  `account_auths`  field.  
Transactions created by the user could only be signed with using keys of the third-party accounts existed in the `account_auths` field. When the user was trying to sign a transaction with own key, the application `cli_wallet` issued an error message.  
 
This flaw is no longer in the SF-0.18.4 version. There was modified the `annotated_signed_transaction()` method in the client application `cli_wallet`. The fix did not affect the input and output parameters of this method and did not change a call to this method. This modification provides ability to sign transactions with using the user's key as well as with using  keys of the accounts existed in the `account_auth` field.  

## Fixed bug in displaying the original blogger on the post feed

A user who subscribed to view posts of only certain blogger, could see other bloggers on the post feed.
This bug occured in the case when a blogger, for which the user had a subscription, reposted work of another blogger, for which the user had no subscription. As a result, the user could see another blogger on the post feed. The user was confused because of there was no any explain information on the post feed.  

This bug has been fixed in the 0.18.4 version. The post feed has been supplemented with the `"reblogged_by:<a name>"` field, which displays a name of the author who reposted a third-party author work. 

## Filtering the requested information about operations from account history

In previous versions, a user did not have ability to request the blockchain for a history of about only individual account operations.  Instead, the user could only request a history of all account operations performed over a period of time.  Many of the received operations were not directly related to the account (for example, voting for posts of this account, repetitive operations perceived as "noise"). To obtain information about an account operation of interest, the user had to create several requests for the blockchain and choose the necessary one from a variety of operations and abandon the rest.  


In order to speed up the search for necessary account operations a new parameter has been implemented in version SF-0.18.4 that can be used to filter operations on certain criteria.  

**Changes in the get_account_history API method**  

The optional parameter `query` was added to the `get_account_history` method to filter operations. By default, filtering is disabled. The method has the following form:  
```cpp
get_account_history(account_name, [from, [limit, [query]]])
```
The parameters `from` and` limit` in this method are optional and by default take the values "-1" and "100" respectively. This method can be called with one specified parameter (for example, `get_account_history(account)`) to get the last 100 account operations.  

The `query` parameter is a structure (object) and contains the following fields:  
  * `select_ops` — list of operations that need to be obtained. The value may contain  names of operations (including ending with "\_operation"), as well as the following key words:  
    * `ALL` — all operations;  
    * `REAL` — only explicitly defined operations;  
    * `VIRTUAL` — only virtual operations;  
  * `filter_ops` — list of operations to be deleted. Takes the same values as `select_ops`. This field is optional and defaults to the empty value;  
  * `direction` — "direction" of operation relative to the account (for example, the operation `vote` defines two accounts: one is who votes and another is whose post is voted for). This field is optional and takes the following values:  
    * `any` — any direction (default value is no filtering by direction);  
    * `sender` — determines the account as a sender (for example, `creator` or `voter`. Usually, this is account who creates the operation);  
    * `receiver`— determines the account as a receiver (for example, `created` or `voted`. Usually, this is account who is related to the operation but does not creates it);  
    * `dual` — determines the account as both sender and receiver at the same time (for example, `voting self post`. That is, the case where the operation ambiguously determines the account role).  

**Changes in the cli_wallet application**   

The `filter_account_history()` method was added to the application `cli_wallet`, which performs the same function as the method `get_account_history()`. Unlike the latter, it has the input parameter `query` to support filtering. In the body of that method, there is a call to the API method `get_account_history()`.  


Examples of calling the `filter_account_history()` method:  
```cpp
get_account_history cyberfounder -1 100
filter_account_history cyberfounder -1 100 {"select_ops":["REAL","interest"], "filter_ops":["transfer"]}
filter_account_history cyberfounder -1 100 {"direction":"receiver","filter_ops":["producer_reward"]}
```
The changes are backward compatible and retain the call and functionality of the `get_account_history()` method.

****
