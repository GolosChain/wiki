# Cli-wallet для HF18

_**Страница содержит справочную информацию о методах в cli\_wallet API для блокчейна версии HF18. Для каждого метода приводится перечень используемых в нем параметров с кратким их описанием, а также функциональное назначение этого метода.**_

## about\(\)

```cpp
variant_object about()const
```

Возвращает информацию о времени компиляции и о клиенте, в том числе версию клиента, версию git graphene/fc, версии boost, openssl и зависимостей.

## add\_operation\_copy\_to\_builder\_transaction\(\)

```cpp
void add_operation_copy_to_builder_transaction(
        transaction_handle_type src_handle,
        transaction_handle_type dst_handle,
        uint32_t op_index
)
```

Параметры:  
`src_handle` — уникальный номер конструктора, из которого копируется операция;  
`dst_handle` — уникальный номер конструктора, на который копируется операция;  
`op_index` — номер копируемой операции.

Используется для копирования операции между конструкторами транзакций.

## add\_operation\_to\_builder\_transaction\(\)

```cpp
void add_operation_to_builder_transaction(transaction_handle_type handle, const operation& op)
```

Параметры:  
`handle` — уникальный номер конструктора;  
`op` — добавляемая операция.

Используется для добавления операции в список операций конструктора транзакций.

## approve\_proposal\(\)

```cpp
signed_transaction approve_proposal(
        const std::string& author, const std::string& title, 
        const approval_delta& delta, 
        bool broadcast
)
```

Параметры:  
`author` — автор предложенной транзакции;  
`title` — заголовок предложенной на подпись транзакции;  
`delta` — список подписей, необходимых для одобрения \(или удаления\) транзакции.  
`broadcast` — “true”, если транзакция пересылается на демон; “false”, если выполняется базовый контроль с выдачей подписанной транзакции на консоль.

Возвращает подписанную версию транзакции.

## begin\_builder\_transaction\(\)

```cpp
transaction_handle_type wallet_api::begin_builder_transaction()
```

Метод вызывает операцию создания конструктора транзакций и возвращает уникальный номер созданного конструктора. Начальное значение уникального номера принимается равным «0» и увеличивается на единицу с каждым вызовом метода.

## cancel\_order\(\)

```cpp
annotated_signed_transaction wallet_api::cancel_order(
        string owner,
        uint32_t orderid,
        bool broadcast
)
```

Параметры:  
`owner` — имя аккаунта, созданного заявку на отмену транзакции;  
`orderid` — идентификационный номер заявки на отмену транзакции;  
`broadcast` — "true", если транзакция пересылается на демон.

Возвращает транзакцию, выполнение которой отменяется.

## cancel\_transfer\_from\_savings\(\)

```cpp
annotated_signed_transaction wallet_api::cancel_transfer_from_savings(
        string from,
        uint32_t request_id,
        bool broadcast
)
```

Параметры:  
`from` — имя аккаунта, инициировавшего отмену;  
`reques_id` — идентификационный номер заявки, используемый в вызове `transfer_from_savings()` для отмены или перевызова операции;  
`broadcast` — "true", если транзакция пересылается на демон.

Возвращает подписанную транзакцию, выполнение которой отменяется.

## change\_recovery\_account\(\)

```cpp
annotated_signed_transaction wallet_api::change_recovery_account(
        string owner,
        string new_recovery_account,
        bool broadcast
)
```

Параметры:  
`owner` — имя аккаунта;  
`new_recovery_account` — новое имя, которое будет присвоено восстановленному аккаунту;  
`broadcast`— "true", если транзакция пересылается на демон.

Возвращает подписанную транзакцию на изменение данных аккаунта.

## convert\_sbd\(\)

```cpp
annotated_signed_transaction wallet_api::convert_sbd(string from, asset amount, bool broadcast)
```

Параметры:  
`from` — имя аккаунта, запрашивающего конвертацию своей криптовалюты вида SBD;  
`amount` — сумма средств в виде SBD для конвертации;  
`broadcast` — "true", если транзакция пересылается на демон.

Используется для конвертации SBD в STEEM в соответствии с их отношением, приведенным в `current_median_history`, за неделю с момента выполнения операции.

## check\_memo\(\)

```cpp
void wallet_api::check_memo(
        const string& memo,
        const golos::api::account_api_object& account
)const
```

Параметры:  
`memo` — поле, записи которого проверяются;  
`account` — имя аккаунта.

Проверяет записи из поля `memo`, относящиеся к личным ключам аккаунта и импортированные в кошелек.

## create\_account\(\)

```cpp
annotated_signed_transaction wallet_api::create_account(
        string creator,
        string new_account_name,
        string json_meta,
        bool broadcast
)
```

Параметры:  
`creator` — пользователь, который создает новый аккаунт;  
`new_account_name` — имя нового аккаунта;  
`json_meta` — метаданные профиля нового аккаунта поля `json_metadata`;  
`broadcast` — "true", если транзакция пересылается на демон; "false", если выполняется базовый контроль с выдачей подписанной транзакции на консоль.

Используется для создания нового аккаунта и генерацией ключей `owner`, `active`, и `memo` для этого аккаунта. За создание аккаунта в качестве комиссионных отчислений с баланса кошелька создателя аккаунта \(автора\) снимается определенная сумма `fee`. Величина этих отчислений не может быть меньше значения параметра `account_creation_fee`, устанавливаемого по результатам голосования делегатов.

## create\_account\_delegated\(\)

```cpp
annotated_signed_transaction wallet_api::create_account_delegated(
        string creator, asset steem_fee,
        asset delegated_vests,
        string new_account_name,
        string json_meta,
        bool broadcast
)
```

Параметры:  
`creator` — пользователь, который создает новый аккаунт;  
`steem_fee` — сумма комиссионных отчислений в криптовалюте Голос, снимаемая с баланса кошелька пользователя за создание нового аккаунта и зачисляемая на баланс кошелька созданного аккаунта в криптовалюте Сила Голоса. Эта сумма не может быть возвращена обратно в кошелек создателя аккаунта;  
`delegated_vests` — сумма комиссионных отчислений в криптовалюте Сила Голоса, снимаемая с баланса кошелька пользователя за операцию делегирования и зачисляемая на баланс кошелька нового аккаунта в криптовалюте Сила Голоса. Эта сумма может быть возвращена обратно в кошелек создателя аккаунта по истечении определенного периода, устанавливаемого голосованием делегатов;  
`new_account_name` — имя нового аккаунта;  
`json_meta` — метаданные профиля нового аккаунта поля `json_metadata`;  
`bool broadcast` — "true", если транзакция пересылается на демон; "false", если выполняется базовый контроль с выдачей подписанной транзакции на консоль.

Используется для создания аккаунта с делегированием. Автоматически генерирует публичные ключи `owner`, `active`, `posting` и `memo` для нового аккаунта.

## create\_account\_with\_keys\(\)

```cpp
annotated_signed_transaction wallet_api::create_account_with_keys(
        string creator, string newname,
        string json_meta, asset fee,
        public_key_type owner,
        public_key_type active,
        public_key_type posting,
        public_key_type memo,
        bool broadcast
)const
```

Параметры:  
`creator` — пользователь, который создает новый аккаунт;  
`newname` — имя нового аккаунта;  
`json_meta` — метаданные профиля нового аккаунта поля `json_metadata`;  
`fee` — сумма комиссионных отчислений в криптовалюте Голос, снимаемая с баланса кошелька пользователя за создание нового аккаунта и зачисляемая на баланс кошелька созданного аккаунта в криптовалюте Сила Голоса. Эта сумма не может быть меньше значения `account_creation_fee`, устанавливаемого по результатам голосования делегатов;  
`owner` — значение публичного ключа `owner` нового аккаунта;  
`active` — значение публичного ключа `active` нового аккаунта;  
`posting` — значение публичного ключа `posting` нового аккаунта;  
`memo` — значение публичного ключа `memo` нового аккаунта;  
`broadcast` — "true", если транзакция пересылается на демон.

Используется для создания аккаунта, требуется явное задание ключей.

## create\_account\_with\_keys\_delegated\(\)

```cpp
annotated_signed_transaction wallet_api::create_account_with_keys_delegated(
        string creator, asset steem_fee,
        asset delegated_vests,
        string new_account_name,
        string json_meta, public_key_type owner,
        public_key_type active,
        public_key_type posting,
        public_key_type memo,
        bool broadcast
)const
```

Параметры:  
`creator` — пользователь, который создает новый аккаунт;  
`steem_fee` — сумма комиссионных отчислений в криптовалюте Голос, снимаемая с баланса кошелька пользователя за создание нового аккаунта и зачисляемая на баланс кошелька созданного аккаунта в криптовалюте Сила Голоса. Эта сумма не может быть возвращена обратно в кошелек создателя аккаунта;  
`delegated_vests` — сумма комиссионных отчислений в криптовалюте Сила Голоса, снимаемая с баланса кошелька пользователя за операцию делегирования и зачисляемая на баланс кошелька нового аккаунта в криптовалюте Сила Голоса. Эта сумма может быть возвращена обратно в кошелек создателя аккаунта по истечении определенного периода, устанавливаемого голосованием делегатов;  
`new_account_name` — имя нового аккаунта;  
`json_meta` — метаданные профиля нового аккаунта поля `json_metadata`;  
`owner` — значение публичного ключа `owner` нового аккаунта;  
`active` — значение публичного ключа `active` нового аккаунта;  
`posting` — значение публичного ключа `posting` нового аккаунта;  
`memo` — значение публичного ключа `memo` нового аккаунта;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для создания нового аккаунта через вызов операции `account_create`. Требуется явное задание ключей `owner`, `active`, `posting` и `memo` для нового аккаунта.

## create\_order\(\)

```cpp
annotated_signed_transaction wallet_api::create_order(
        string owner,
        uint32_t order_id,
        asset amount_to_sell,
        asset min_to_receive,
        bool fill_or_kill,
        uint32_t expiration,
        bool broadcast
)
```

Параметры:  
`owner` — имя аккаунта, создаваемого заказ;  
`order_id` — идентификационный номер заказа, присваиваемый аккаунтом, создающий этот заказ;  
`amount_to_sell` — сумма средств в виде SBD или STEAM, выставляемая на продажу;  
`min_to_receive` — минимальная сумма средств, предполагаемая к получению;  
`fill_or_kill` — “true”, если заказ следует отменить в случае его немедленного невыполнения;  
`expiration` — время отведенное на выполнение заказа, по истечении которого он будет отменен;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для создания заказа на продажу с кошелька ограниченной суммы средств в виде SBD или STEAM по цене в соответствии с отношением `amount_to_sell / min_to_receive`.  
Возвращает подписанную транзакцию.

## database\_info\(\)

```cpp
variant_object database_info()const
```

Возвращает справочную информацию о текущем статусе разделяемой памяти, в том числе: общий объем разделяемой памяти, размер свободного и зарезервированного пространства для определенных нужд, а также список индексов, хранящихся в разделяемой памяти \(название индекса, количество записей\).

## decline\_voting\_rights\(\)

```cpp
annotated_signed_transaction wallet_api::decline_voting_rights(
        string account,
        bool decline,
        bool broadcast
)
```

Параметры:  
`account` — имя аккаунта, который лишается права голоса;  
`decline` — “true”, если аккаунт лишается права голоса; `broadcast` — “true”, если транзакция пересылается на демон.

Используется для лишения права голоса аккаунта с указанным именем.  
Возвращает подписанную транзакцию.

## decrypt\_memo\(\)

```cpp
string wallet_api::decrypt_memo( string encrypted_memo )
```

Параметр:  
`encrypted_memo` — запись, приведенная для прочтения.

Возвращает текст в расшифрованном виде \(если это возможно\), находящийся в кошельке и зашифрованный с помощью одного из известных личных ключей.

## delegate\_vesting\_shares\(\)

```cpp
annotated_signed_transaction wallet_api::delegate_vesting_shares(
        string delegator,
        string delegatee,
        asset vesting_shares,
        bool broadcast
)
```

Параметры:  
`delegator` — имя аккаунта, который делегирует Силу Голоса;  
`delegatee` — имя аккаунта, на который делегируется Сила Голоса;  
`vesting_shares` — сумма делегирования;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для делегирования части криптовалюты Силы Голоса \(значение в GESTS\) с одного аккаунта на другой.

## encrypt\_keys\(\)

```cpp
void encrypt_keys()
```

Используется для шифрования текста \(примечания\) с помощью одного из личных ключей.

## escrow\_approve\(\)

```cpp
annotated_signed_transaction wallet_api::escrow_approve(
        string from,
        string to,
        string agent,
        string who,
        uint32_t escrow_id,
        bool approve,
        bool broadcast
)
```

Параметры:  
`from` — имя аккаунта, передающего средства на условное депонирование;  
`to` — имя аккаунта, принимающего средства на условное депонирование;  
`agent` — имя аккаунта, выступающего в качестве агента в случае возникновения спорных ситуаций;  
`who` — имя аккаунта, утверждающего сделку \(`to` или `agent`\);  
`escrow_id` — идентификационный номер операции;  
`approve` — “true”, если сделка утверждается. Иначе транзакция отменяется с возвратом средств аккаунту с именем `from`;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для утверждения предложенной транзакции с операцией передачи средств на условное депонирование. Средства могут быть разблокированы только после утверждения транзакции.  
Возвращает подписанную транзакцию.

## escrow\_dispute\(\)

```cpp
annotated_signed_transaction wallet_api::escrow_dispute(
        string from,
        string to,
        string agent,
        string who,
        uint32_t escrow_id,
        bool broadcast
)
```

Параметры:  
`from` — имя аккаунта, передавшего средства на условное депонирование;  
`to` — имя аккаунта, принявшего средства на условное депонирование;  
`agent` — имя аккаунта, выступающего в качестве агента в случае возникновения спорных ситуаций;  
`who` — имя аккаунта, открывшего спор \(`from` или `to`\);  
`escrow_id` — идентификационный номер операции;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для открытия спора по передачи средств на условное депонирование. Спор может быть открыт до истечения срока, отведенного на условное депонирование.  
Возвращает подписанную транзакцию.

## escrow\_release\(\)

```cpp
annotated_signed_transaction wallet_api::escrow_release(
        string from,
        string to,
        string agent,
        string who,
        string receiver,
        uint32_t escrow_id,
        asset sbd_amount,
        asset steem_amount,
        bool broadcast
)
```

Параметры:  
`from` — имя аккаунта, передавшего средства на условное депонирование;  
`to` — имя аккаунта, изначально принявшего средства на условное депонирование;  
`agent` — имя аккаунта, выступающего в качестве агента в случае возникновения спорных ситуаций;  
`who` — имя аккаунта, утвердившего сделку;  
`receiver` — имя аккаунта, в кошелек которого поступят отпущенные средства;  
`escrow_id` — идентификационный номер операции;  
`sbd_amount` — сумма отпущенных средств в виде SBD;  
`steem_amount` — сумма отпущенных средств в виде STEEM;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для разблокирования \(освобождение\) средств, передаваемых на условное депонирование.  
Возвращает подписанную транзакцию.

## escrow\_transfer\(\)

```cpp
annotated_signed_transaction wallet_api::escrow_transfer(
        string from,
        string to,
        string agent,
        uint32_t escrow_id,
        asset sbd_amount,
        asset steem_amount,
        asset fee,
        time_point_sec ratification_deadline,
        time_point_sec escrow_expiration,
        string json_meta,
        bool broadcast
)
```

Параметры:  
`from` — имя аккаунта, от которого передаются средства на условное депонирование;  
`to` — имя аккаунта, к которому передаются средства на условное депонирование;  
`agent` — имя аккаунта, выступающего в качестве агента в случае возникновения спорных ситуаций;  
`escrow_id` — идентификационный номер операции \(значения `from` и `escrow_id` должны составлять уникальную пару\);  
`sbd_amount` — сумма средств в виде SBD, передаваемых на условное депонирование;  
`steem_amount` — сумма средств в виде STEEM, передаваемых на условное депонирование;  
`fee` — комиссионные отчисления для агента;  
`ratification_deadline` — время, до наступления которого транзакция должна быть одобрена аккаунтами с именами `to` и `agent`;  
`escrow_expiration` — время, по истечении которого любая из сторон может потребовать возврат средств;  
`json_meta` — метаданные в кодировке JSON;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для передачи средств в виде STEEM или SBD на условное депонирование от одного аккаунта другому.  
Возвращает подписанную транзакцию.

## find\_first\_unused\_derived\_key\_index\(\)

```cpp
int find_first_unused_derived_key_index(const fc::ecc::private_key& parent_key)
```

Параметр:  
`parent_key` — родительский ключ.

Используется для генерации ключей, производных от родительского. Генерация ключей начинается с индекса «0» с последующим приращением его на единицу. Если очередные индексы будут отсутствовать \(обнаружение незарегистрированных ключей\) в блокчейне, процесс прекращается. Алгоритм поиска зарегистрированного ключа в блокчейне по очередному индексу предусматривает возможное отсутствие нескольких номеров \(наличие «дыр»\) в последовательности. Размер «дыры» не должен превышать более пяти подряд идущих номеров.

## follow\(\)

```cpp
annotated_signed_transaction wallet_api::follow(
        string& follower,
        string& following,
        const set<string>& what,
        bool broadcast
)
```

Параметры:  
`follower` — имя аккаунта, являющегося последователем другого;  
`following` — имя аккаунта, от которого наследуется набор вещей;  
`what` — набор вещей, который наследуется \(посты, комментарии, голоса, игнорирования\);  
`broadcast` — “true”, если транзакция пересылается на демон.

Помечает одного аккаунта как последователь \(наследник\) другого. Требует для последователя авторизацию ключом `posting`.  
Возвращает подписанную транзакцию.

## get\_account\(\)

```cpp
golos::api::account_api_object get_account(string account_name)const
```

Параметр:  
account\_name — имя аккаунта, информация о котором предоставляется.

Возвращает информацию об аккаунте с заданным именем, имеющемся в базе данных блокчейна.

## get\_account\_history\(\)

```cpp
map< uint32_t, golos::plugins::operation_history::applied_operation >
wallet_api::get_account_history(
        string account,
        uint32_t from,
        uint32_t limit
)
```

Параметры:  
`account` — аккаунт, история которого запрашивается;  
`from` — порядковый номер операции. «-1» — означает последний номер операции;  
`limit` — максимальное количество запрашиваемых элементов. Принимает значение 0-1000.

Возвращает историю всех действий в виде транзакций пользователя в сети GOLOS.

## get\_active\_witnesses\(\)

```cpp
vector< account_name_type > wallet_api::get_active_witnesses()const
```

Возвращает список всех активных делегатов, создавших блоки в текущем окружении \(21 блок\).

## get\_block \(\)

```cpp
optional<signed_block_with_info> wallet_api::get_block (uint32_t num)
```

Параметр:  
`num` — номер блока.

Возвращает расширенную информацию по блоку с номером `num`.

## get\_conversion\_requests\(\)

```cpp
vector< database_api::convert_request_api_object > wallet_api::get_conversion_requests(
        string owner_account
)
```

Параметр:  
`owner_account` — имя аккаунта, создавшего запросы.

Возвращает текущие запросы на конвертацию указанным пользователем, в том числе все ожидающие запросы

## get\_encrypted\_memo\(\)

```cpp
string wallet_api::get_encrypted_memo( string from, string to, string memo )
```

Параметры:  
`from` — имя аккаунта, с кошелька которого снимаются средства;  
`to` — имя аккаунта, в кошелек которого поступают средства;  
`memo` — текст заметки \(примечания\).

Возвращает заметку в зашифрованном виде, если ее текст начинается с символа «\#». В противном случае возвращает заметку в ее оригинальном виде.

## get\_feed\_history\(\)

```cpp
witness_api::feed_history_api_object wallet_api::get_feed_history()const
```

Возвращает историю конверсий GBG / GOLOS на блокчейне.

## get\_inbox\(\)

```cpp
vector<extended_message_object> wallet_api::get_inbox(
        const std::string& to,
        time_point newest,
        uint16_t limit,
        std::uint64_t offset
)
```

Параметры:  
`to` — имя аккаунта, для которого запрашиваются входящие к нему сообщения;  
`newest` — время, с момента которого запрашиваются сообщения;  
`limit` — пороговое значение на количество запрашиваемых сообщений;  
`offset` — смещение \(в секундах\) к заданному времени `newest`.

Используется для просмотра входящих личных сообщений для аккаунта с именем `to`, хранящихся в блокчейне.

## get\_miner\_queue\(\)

```cpp
vector<account_name_type> wallet_api::get_miner_queue()const
```

Возвращает список майнеров, ожидающих создание блоков.

## get\_open\_orders\(\)

```cpp
vector< database_api::extended_limit_order > wallet_api::get_open_orders( string owner )
```

Параметр:  
`owner` — пользователь, для которого отображается список заявок.

Возвращает список заявок на внутренней бирже на покупку и продажу в сети GOLOS для указанного пользователя.

## get\_ops\_in\_block\(\)

```cpp
vector< golos::plugins::operation_history::applied_operation > wallet_api::get_ops_in_block(
        uint32_t block_num,
        bool only_virtual
)
```

Параметры:  
`num` — номер \(позиция\) блока;  
`only_virtual` — только виртуальные операции.

Возвращает виртуальные операции в заданном блоке.

## get\_order\_book\(\)

```cpp
market_history::order_book wallet_api::get_order_book(uint32_t limit)
```

Параметр:  
`limit` — максимальное количество заказов для возврата заявок и запросов \(максимальное значение — 1000\).

Возвращает список заявок на внутренней бирже на покупку и продажу в сети GOLOS.

## get\_outbox\(\)

```cpp
vector<extended_message_object> wallet_api::get_outbox(
        const std::string& from,
        time_point newest,
        uint16_t limit,
        std::uint64_t offset
)
```

Параметры:  
`from` — имя аккаунта, для которого запрашиваются исходящие от него сообщения;  
`newest` — время, с момента которого запрашиваются сообщения;  
`limit` — пороговое значение на количество запрашиваемых сообщений;  
`offset` — смещение \(в секундах\) к заданному времени `newest`.

Используется для просмотра исходящих личных сообщений для аккаунта с именем `from`, хранящихся в блокчейне.

## get\_owner\_history\(\)

```cpp
vector< database_api::owner_authority_history_api_object > wallet_api::get_owner_history(
        string account
)const
```

Параметры:  
`account` — пользователь, история которого отображается.

Возвращает историю полномочий \(прав собственности\) пользователя в блокчейне.

## get\_private\_key\(\)

```cpp
fc::ecc::private_key get_private_key(const public_key_type pubkey)const
```

Параметр:  
`pubkey` — тип публичного ключа.

Используется для получения личного ключа в формате WIF, соответствующего публичному ключу. Личный ключ к этому моменту должен быть в кошельке.  
Возвращает подтверждение о наличие ключа или его отсутствие.

## get\_private\_key\_for\_account\(\)

```cpp
fc::ecc::private_key get_private_key_for_account(
        const golos::api::account_api_object& account
)const
```

Параметр:  
`account` — имя аккаунта, получающего ключ.

Возвращает данные ключа `active` в формате WIF.

## get\_private\_key\_from\_password\(\)

```cpp
pair<public_key_type,string> wallet_api::get_private_key_from_password(
        string account,
        string role,
        string password
)const
```

Параметры:  
`account` — имя аккаунта, который получает ключ;  
`role` — тип ключа \(`active` \| `owner` \| `posting` \| `memo`\);  
`password` — пароль, который будет использоваться во время генерации ключа.

Возвращает публичный ключ, соответствующий сгенерированному личному ключу, а также личный ключ в формате WIF.

## get\_proposed\_transactions\(\)

```cpp
std::vector<database_api::proposal_api_object> get_proposed_transactions(
        std::string account,
        uint32_t from,
        uint32_t limit
)
```

Параметры:  
`account` — аккаунт, информацию о предложенных транзакциях которого необходимо получить;  
`from` — начальный номер транзакции;  
`limit` — пороговое значение количества транзакций.

Используется для получения информации о всех предложенных транзакциях применительно к одному и тому же аккаунту. Для получения информации об ограниченном количестве транзакций, необходимо задать начальный номер транзакции `from` и пороговое значение `limit`.

## get\_prototype\_operation\(\)

```cpp
operation get_prototype_operation(string operation_type)
```

Параметр:  
`operation_type` — тип операции. Операция должна быть определена в файле `steem/chain/operations.hpp`.

Используется для получения и заполнения шаблона для операции, создаваемой с помощью вызова `add_operation_to_builder_transaction()`. Возвращает неинициализированный объект в виде заданной последовательности операций. Созданный объект может быть дополнен любой операцией с помощью вызова `add_operation_to_builder_transaction()`.

## get\_transaction\(\)

```cpp
annotated_signed_transaction wallet_api::get_transaction(transaction_id_type id)const
```

Параметр:  
id — идентификатор транзакции.

Возвращает информацию о транзакции с указанном идентификатором.

## get\_wallet\_filename\(\)

```cpp
string get_wallet_filename()
```

Возвращает текущее имя файла кошелька. Данное имя файла используется при автоматическом сохранении кошелька.

## get\_withdraw\_routes\(\)

```cpp
vector< database_api::withdraw_vesting_route_api_object > wallet_api::get_withdraw_routes(
        string account,
        database_api::withdraw_route_type type
)const
```

Параметры:  
`account` — пользователь, для которого запрашивается возврат;  
`type` — тип возврата \(задается строкой вида `incoming`, `outgoing` или `all`\).

Возвращает перечень переводов, осуществляющих на счет пользователя в зависимости от задаваемого типа возврата \(входящий, исходящий или все\).

## get\_witness\(\)

```cpp
optional < witness_api::witness_api_object > get_witness(string owner_account)
```

Параметр:  
`owner_account` — имя или идентификатор делегата.

Возвращает информацию о заданном делегате, хранимую в блокчейне.

## gethelp\(\)

```cpp
string wallet_api::gethelp(const string & method)const
```

Параметр:  
`method` — имя API команды, информация о которой запрашивается.

Возвращает справочную информацию по отдельной команде API.

## help\(\)

```cpp
string wallet_api::help()const
```

Возвращает на терминал справочный перечень всех команд, поддерживаемых приложением wallet API.  
Информация содержит команды, их аргументы и возвращаемые типы. Для получения более подробной информации по отдельной команде следует использовать `get_help()`.

## get\_withdraw\_routes\(\)

```cpp
vector< database_api::withdraw_vesting_route_api_object > wallet_api::get_withdraw_routes(
        string account,
        database_api::withdraw_route_type type
)const
```

Параметры:  
`account` — имя аккаунта, запрашивающего маршруты;  
`type` — тип вывода \(входящий, исходящий или все\).

Возвращает маршруты вывода средств для аккаунта.

## import\_key\(\)

```cpp
bool import_key(string wif_key)
```

Параметр:  
`wif_key` — личный ключ в формате WIF.

Используется для импорта личного ключа в формате WIF в кошелек для его дальнейшего использования аккаунтом при подписании транзакций.  
Пример использования команды:

```text
import_key 5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3
```

## info\(\)

```cpp
variant wallet_api::info()const
```

Возвращает актуальную информацию о блокчейне, в том числе:  
`witness_majority_version` — актуальное состояние делегирования;  
`hardfork_version` — версию HF Golos;  
`head_block_num` — актуальный номер блока;  
`head_block_id` — идентификатор блока;  
`head_block_age` — время жизни блока \(в секундах\);  
`median_sbd_price` — медианное значение SBD;  
`account_creation_fee` — размер комиссионных отчислений, требуемых на создание аккаунта без делегирования;  
`create_account_min_golos_fee` — минимальный размер комиссионных отчислений в криптовалюте Голос, требуемых на создание аккаунта с делегированием;  
`create_account_min_delegation` — минимально возможное количество Силы Голоса при создании аккаунта с делегированием;  
`create_account_delegation_time` — минимально возможное время \(в секундах\) «заморозки» делегированной Силы Голоса при создании аккаунта с делегированием;  
`min_delegation` — минимально возможное количество Силы Голоса для делегирования на аккаунт.

## is\_locked\(\)

```cpp
bool wallet_api::is_locked()const
```

Проверяет состояние кошелька: заблокирован или нет. Если кошелек находится в заблокированном состоянии, находящиеся в нем личные ключи использованы быть не могут. Состояние ключа изменяется вызовами `lock()` или `unlock()`.  
Возвращает “true”, если кошелек заблокирован.

## is\_new\(\)

```cpp
bool wallet_api::is_new()const
```

Проверяет состояние кошелька: новый \(пароль еще не установлен\) или нет. Установка пароля осуществляется с помощью вызова `set_password()`. Во время выполнении этой команды кошелек переводится в заблокированное состояние.  
Возвращает “true”, если кошелек новый.

## list\_accounts\(\)

```cpp
vector< account_name_type > wallet_api::list_accounts(const string& lowerbound, uint32_t limit)
```

Параметры:  
`lowerbound` — имя первого возвращаемого аккаунта. Если такое имя отсутствует, список будет начинаться с имени, непосредственно следующего за `lowerbound`;  
`limit` — значение, ограничивающее количество выводимых на монитор имен аккаунтов. Максимальное значение — 1000.

Используется для получения списка всех аккаунтов, зарегистрированных в блокчейне. Задание параметров `lowerbound` и `limit` позволяет формировать страницу в удобной для просмотра форме. Для просмотра всего списка имен аккаунтов рекомендуется вначале значение `lowerbound` устанавливать в виде пустой строки \(“”\). Затем на каждой итерации параметру `lowerbound` передавать последнее возвращаемое имя аккаунта для следующего вызова `list_accaunt()`.  
Возвращает список всех зарегистрированных в блокчейне имен аккаунтов с соответствующими им идентификаторами, отсортированный по именам в алфавитном порядке.

## list\_keys\(\)

```cpp
map<public_key_type, string> wallet_api::list_keys()
```

Используется для выдачи \(распечатки\) всех личных ключей, принадлежащих кошельку. Данные ключей выдаются в формате WIF. Ключи могут быть импортированы в другой кошелек вызовом `import_key`.  
Возвращает карту, содержащую личные ключи, индексированные по их публичному ключу.

## list\_my\_accounts\(\)

```cpp
vector< golos::api::account_api_object > wallet_api::list_my_accounts()
```

Используется для получения информации об аккаунтах с помощью личного ключа, имеющегося в кошельке. Кошелек должен быть предварительно разблокирован. Метод использует вызов `get_account()`.

## list\_witnesses\(\)

```cpp
vector< account_name_type > wallet_api::ist_witnesses(const string& lowerbound, uint32_t limit)
```

Параметры:  
`lowerbound` — имя первого возвращаемого делегата. Если такое имя отсутствует, список будет начинаться с имени, непосредственно следующего за `lowerbound`;  
`limit` — значение, ограничивающее количество выводимых на монитор имен делегатов. Максимальное значение — 1000.

Используется для получения списка всех имен делегатов, зарегистрированных в блокчейне. В списке также выделяются имена делегатов, голосующих на данный момент. Задание параметров `lowerbound` и `limit` позволяет формировать страницу в удобной для просмотра форме. Для просмотра всего списка имен делегатов рекомендуется вначале значение `lowerbound` устанавливать в виде пустой строки \(“”\). Затем на каждой итерации параметру `lowerbound` передавать последнее возвращаемое имя делегата для следующего вызова `list_witnesses()`.  
Возвращает список всех зарегистрированных в блокчейне имен делегатов с соответствующими им идентификаторами, отсортированный по именам в алфавитном порядке.

## load\_wallet\_file\(\)

```cpp
bool wallet_api::load_wallet_file(string wallet_filename)
```

Параметр:  
`wallet_filename` — имя файла кошелька в формате JSON, который загружается.

Используется для загрузки заданного кошелька платформы Graphene. Перед тем, как новый кошелек будет загружен, текущий кошелек закрывается. Если поле `wallet_filename` задано пустым, выполнится перезагрузка существующего файла кошелька.  
Возвращает “true”, если заданный кошелек успешно загружен.

## lock\(\)

```cpp
void wallet_api::lock()
```

Используется для немедленного блокирования кошелька.

## normalize\_brain\_key\(\)

```cpp
string normalize_brain_key(string s)
```

Параметр:  
`s` — brain-ключ в оригинальном формате.

Используется для преобразования формата brain-ключа с целью снижения риска возникновения ошибок при повторных вводах-выводах ключа из памяти.  
Оригинальный формат brain-ключа преобразуется в формат, используемый для генерации личных ключей. Новый формат содержит верхние регистры всех символов ASCII, а также упакованные последовательно идущие пробелы в один.  
Возвращает brain-ключ в нормализованном виде.

## post\_comment\(\)

```cpp
annotated_signed_transaction wallet_api::post_comment(
        string author,
        string permlink,
        string parent_author,
        string parent_permlink,
        string title, string body,
        string json,
        bool broadcast
)
```

Параметры:  
`author` — имя аккаунта, являющего автором комментария;  
`permlink` — ссылка на комментарий;  
`parent_author` — имя аккаунта комментария верхнего уровня. Пустая строка означает верхний уровень комментария;  
`parent_permlink` — категория ссылки \(для случая, если `parent_author` — пустая строка\);  
`title` — заголовок комментария;  
`body` — тело комментария;  
`json` — метаданные комментария в формате JSON;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для публикации или обновления комментария.  
Возвращает подписанную транзакцию.

## preview\_builder\_transaction\(\)

```cpp
transaction preview_builder_transaction(transaction_handle_type handle)
```

Параметр:  
`handle` — уникальный номер получаемого конструктора.

Возвращает уникальный номер для очередного создаваемого конструктора транзакций.

## propose\_builder\_transaction\(\)

```cpp
signed_transaction propose_builder_transaction(
        transaction_handle_type handle,
        std::string author,
        std::string title,
        std::string memo,
        time_point_sec expiration,
        time_point_sec review_period_time,
        bool broadcast
)
```

Параметры:  
`handle` — уникальный номер создаваемого конструктора транзакций;  
`author` — автор предлагаемой транзакции;  
`title` — заголовок предлагаемой транзакции;  
`memo` — примечание, текст которого дополняет смысловое значение заголовка;  
`expiration` — время, по истечении которого прекращается подписание транзакции;  
`review_period_time` — период, выделенный для подписания транзакции;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для создания конструктора предлагаемых транзакций. Возвращает созданный конструктор транзакций под номером `handle`.

## publish\_feed\(\)

```cpp
annotated_signed_transaction wallet_api::publish_feed(
        string witness,
        price exchange_rate,
        bool broadcast
)
```

Параметры:  
`witness` — делегат, публикующий ценовой тариф \(потолок котировок\);  
`exchange_rate` — предлагаемый курс обмена;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для публикации потолка котировок криптовалют.  
Делегат может публиковать потолок котировок криптовалют STEEM:SBD на бирже. Медианное значение этих котировок используется для обработки запросов на конвертацию SBD в STEEM.  
Возвращает подписанную транзакцию.

## quit\(\)

```cpp
void wallet_api::quit()
```

Ипользуется для выхода из кошелька.

## recover\_account\(\)

```cpp
annotated_signed_transaction wallet_api::recover_account(
        string account_to_recover,
        authority recent_authority,
        authority new_authority,
        bool broadcast
)
```

Параметры:  
`account_to_recover` — имя аккаунта; у которого восстанавливаются полномочия; `recent_authority` — недавние полномочия аккаунта; `new_authority` — новые полномочия, которые задаются в запросе на восстановление;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для восстановления полномочий \(авторизацию\) аккаунта с использованием запроса на восстановление, созданного самим аккаунтом. Синтаксис этой команды содержит сериализованный объект авторизации. Пример авторизации объекта показан в следующей строке:

```cpp
recover_account "account_to_recover"
    { " weight_threshold": 1,"account_auths": [], "key_auths": [["old_public_key",1]] }
    {"weight_threshold": 1,"account_auths": [], "key_auths": [["new_public_key",1]]} true
```

## remove\_builder\_transaction\(\)

```cpp
void remove_builder_transaction(transaction_handle_type handle)
```

Параметр:  
`handle` — уникальный номер конструктора транзакций. Этот номер уменьшается на единицу после каждого вызова метода.

Используется для удаления конструктора транзакций по заданному идентификационному номеру.

## replace\_operation\_in\_builder\_transaction\(\)

```cpp
void replace_operation_in_builder_transaction(
        transaction_handle_type handle,
        unsigned op_index,
        const operation& new_op
)
```

Параметры:  
`handle` — уникальный номер конструктора;  
`op_index` — номер операции в конструкторе транзакций, которую необходимо заменить;  
`op` — заменяющая операция.

Используется для замены операции под номером `op_index` в конструкторе транзакций на операцию `op`.

## request\_account\_recovery\(\)

```cpp
annotated_signed_transaction wallet_api::request_account_recovery(
        string recovery_account,
        string account_to_recover,
        authority new_authority,
        bool broadcast
)
```

Параметры:  
`recovery_account` — имя аккаунта, создающего запрос на восстановление;  
`account_to_recover` — имя аккаунта, который необходимо восстановить;  
`new_authority` — новая авторизация “owner” для восстанавливаемого аккаунта;  
`broadcast` — “true”, если транзакция пересылается на демон.

Создает запрос на восстановление аккаунта. Синтаксис этой команды содержит сериализованный объект авторизации. Передача авторизации приведена в следующем примере:

```cpp
request_account_recovery "recovery_account" "account_to_recover" 
{"weight_threshold": 1,"account_auths": [],  "key_auths": [["new_public_key",1]]} true
```

## save\_wallet\_file\(\)

```cpp
void wallet_api::save_wallet_file(string wallet_filename)
```

Параметр:  
`wallet_filename` — имя нового файла формата JSON, в который сохраняется кошелек.

Используется для сохранения кошелька в файл с заданным именем. Если поле `wallet_filename` задается пустым, кошелек сохраняется в старый файл. Для выполнения операции «Сохранить как …» следует использовать вызов `set_wallet_filename()`.

## serialize\_transaction\(\)

```cpp
string wallet_api::serialize_transaction(signed_transaction tx)const
```

Параметр:  
`tx` — подписанная транзакция для сериализации.

Конвертирует подписанную транзакцию в формате JSON в ее бинарное представление.  
Возвращает подписанную транзакцию в бинарном представлении, имеющую вид неструктурированной \(бесформенной\) строки, в которой могут содержаться нулевые символы.

## set\_password\(\)

```cpp
void wallet_api::set_password(string password)
```

Параметр:  
`password`— новый пароль.

Используется для установки нового пароля для кошелька. Кошелек должен быть либо новым, либо предварительно открытым командой `unlocked()`.

## set\_transaction\_expiration\(\)

```cpp
void set_transaction_expiration(uint32_t seconds)
```

Параметр:  
`seconds` — период времени в секундах.

Возвращает заданный период времени \(в секундах\) в будущем, обозначающим срок действия транзакции.

## set\_voting\_proxy\(\)

```cpp
signed_transaction set_voting_proxy(
        string account_to_modify,
        string voting_account,
        bool broadcast
)
```

Параметры:  
`account_to_modify`— имя или идентификационный номер аккаунта, чьи полномочия в голосовании изменяются;  
`voting_account` — имя аккаунта \(заместитель\), который наделяется полномочиями в голосовании. Пустая строка означает отсутствие заместителя;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для установки полномочий в голосовании для аккаунта. Если пользователь не желает принимать активное участие в голосовании, он может разрешить другому аккаунту \(заместителю\) использовать его долю голоса при голосовании. Наделение полномочиями заместителя в голосовании не удаляет предыдущие голоса аккаунта из блокчейна, они будут сохранены, но игнорированы. Если аккаунт позже аннулирует полномочия заместителя в голосовании, его предыдущие голоса снова вступят в силу.

## set\_withdraw\_vesting\_route\(\)

```cpp
annotated_signed_transaction wallet_api::set_withdraw_vesting_route(
        string from,
        string to,
        uint16_t percent,
        bool auto_vest,
        bool broadcast
)
```

Параметры:  
`from` — имя аккаунта, с кошелька которого снимаются средства в виде VESTS;  
`to` — имя аккаунта, в кошелек которого возвращаются средства в виде VESTS или STEEM;  
`percent` — процент отчисления от суммы возвращаемых средств аккаунту с именем “to”. Это значение должно устанавливаться в пределах от 1 до 100000 включительно \(например, 100 составляет 1 %, 10000 составляет 100 %\);  
`auto_vest` — “true”, если аккаунт с именем “from” должен получить средства в виде VESTS, и “false”, если в виде STEEM;  
`broadcast` — “true”, если подписанную транзакцию необходимо переслать на демон.

Используется для настройки маршрута вывода средств, полученных по наделенным акциям. При возврате наделенных акций получаемые средства будут перенаправлены к аккаунтам с именами “from” и “to” в соответствии с указанными весовыми долями.

## sign\_builder\_transaction\(\)

```cpp
signed_transaction wallet_api::sign_builder_transaction(
        transaction_handle_type handle,
        bool broadcast
)
```

Параметры:  
`handle` — уникальный номер конструктора транзакций;  
`broadcast` — “true”, если подписанные транзакции необходимо переслать на демон.

Используется для подписания всех транзакций в конструкторе транзакций. Возвращает подписанные транзакции в конструкторе транзакций под номером `handle`.

## sign\_transaction\(\)

```cpp
annotated_signed_transaction wallet_api::sign_transaction(signed_transaction tx, bool broadcast)
```

Параметры:  
`tx` — транзакция; предоставленная на подпись;  
`broadcast` — “true”, если подписанную транзакцию необходимо переслать на демон.

Используется для предоставления на подпись полностью сформированную транзакцию. Транзакция подписывается необходимыми ключами и пересылается при необходимости на демон.  
Возвращает подписанную версию транзакции.

## suggest\_brain\_key\(\)

```cpp
brain_key_info wallet_api::suggest_brain_key()const
```

Предлагает вариант безопасного brain-ключа для создания аккаунта. Используется для вызова метода `create_account_with_brain_key()`, который требует задания brain-ключа, а также длинную кодовую фразу, которая обеспечивает достаточную энтропию для генерации кипптографических ключей. Данный метод предложит сгенерированную случайным образом строку, которую следует сохранить.  
Возвращает предложенный вариант brain-ключа.

## transfer\(\)

```cpp
annotated_signed_transaction wallet_api::transfer(
        string from,
        string to,
        asset amount,
        string memo,
        bool broadcast
)
```

Параметры:  
`from` — имя аккаунта, с кошелька которого будут переводиться средства;  
`to` — имя аккаунта, в кошелек которого будут переводиться средства;  
`amount` — сумма переводимых средств \(например, 100.000 STEEM\);  
`memo` — запись в транзакции, зашифрованная публичным ключом “memo”;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для перевода средств в виде STEEM или SBD с кошелька одного аккаунта в кошелек другого.

## transfer\_from\_savings\(\)

```cpp
annotated_signed_transaction wallet_api::transfer_from_savings(
        string from,
        uint32_t request_id,
        string to,
        asset amount,
        string memo,
        bool broadcast
)
```

Параметры:  
`from` — имя аккаунта, инициировавшего передачу средств из сбережений;  
`request_id` — идентификационный номер операции, созданный аккаунтом `from`, который может быть использован для отмены операции, а также для ее повторного использования по завершении передачи;  
`to` — имя аккаунта, которому передаются средства;  
`amount` — сумма средств, подлежащих передачи;  
`memo` — текст примечания в транзакции, зашифрованный с помощью ключа `memo`;  
`broadcast` — "true", если транзакция пересылается на демон.

Возвращает подписанную транзакцию.

## transfer\_to\_savings\(\)

```cpp
annotated_signed_transaction wallet_api::transfer_to_savings(
        string from,
        string to,
        asset amount,
        string memo,
        bool broadcast
)
```

Параметры:  
`from` — имя аккаунта, инициировавшего перевод;  
`to` — имя аккаунта, получающего перевод;  
`amount` — сумма переводимых средств;  
`memo` — запись в транзакции, зашифрованная ключом “memo” и оставленная аккаунтом, инициировавшим перевод;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для сбережения средств. Операция перевода средств на сбережение выполняется немедленно. Обратная операция \(снятие сбережений\) выполняется в течение 72 часов.  
Возвращает подписанную транзакцию.

## transfer\_to\_vesting\(\)

```cpp
annotated_signed_transaction wallet_api::transfer_to_vesting(
        string from,
        string to,
        asset amount,
        bool broadcast
)
```

Параметры:  
`from` — имя аккаунта, от которого поступают средства в виде STEEM;  
`to` — имя аккаунта, к которому поступают средства в виде VESTS;  
`amount` — сумма средств в виде STEEM, выделяемая в инвестиционный фонд;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для передачи средств в виде STEEM в инвестиционный фонд, представленный наделяющими акциями \(VESTS\). VESTS обязаны облагаться отчислениями в размере не менее одного “coin” в год и еженедельным снятием средств в течение последующих двух лет.  
Возвращает подписанную транзакцию.

## try\_decrypt\_message\(\)

```cpp
 message_body wallet_api::try_decrypt_message(const message_api_obj& mo )
```

Параметр:  
`mo` — объект API сообщения для расшифровки.

Используется для получения текстовых сообщений, зашифрованных с помощью личного ключа “memo”. Выполняет проверку текстового сообщения на контрольную сумму.  
Возвращает расшифрованный текст в случае соответствия ключа.

## try\_get\_private\_key\(\)

```cpp
optional<fc::ecc::private_key> try_get_private_key(const public_key_type& id)const
```

Параметр:  
`id` — идентификационный номер публичного ключа.

Возвращает личный ключ в формате WIF, соответствующий публичному с заданным идентификационным номером, если публичный ключ имеется.

## unlock\(string password\)

```cpp
void wallet_api::unlock(string password)
```

Параметр:  
`password` — пароль, который ранее был установлен с помощью вызова `set_password()`.

Используется для разблокирования кошелька. Кошелек остается открытым до завершения работы программы либо до появления команды `lock()`.

## update\_account\(\)

```cpp
annotated_signed_transaction wallet_api::update_account(
        string accountname,
        string json_meta,
        public_key_type owner,
        public_key_type active,
        public_key_type posting,
        public_key_type memo,
        bool broadcast
)const
```

Параметры:  
`accountname` — имя аккаунта;  
`json_meta` — новые метаданные поля “json\_meta”, относящиеся к аккаунту;  
`owner` — новый публичный ключ типа `owner` для аккаунта;  
`active` — новый публичный ключ типа `active` для аккаунта;  
`posting` — новый публичный ключ типа `posting` для аккаунта;  
`memo` — новый публичный ключ типа `memo` для аккаунта;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для обновления метаданных и ключей существующего аккаунта.  
Возвращает подписанную транзакцию.

## update\_account\_auth\_account\(\)

```cpp
annotated_signed_transaction wallet_api::update_account_auth_account(
        string account_name,
        authority_type type,
        string auth_account,
        weight_type weight,
        bool broadcast
)
```

Параметры:  
`account_name` — имя аккаунта, чьи полномочия \(тип авторизации\) следует обновить;  
`type` — тип авторизации \(`owner`, `active` или `posting`\);  
`auth_account` — имя аккаунта, чьими полномочиями наделяется аккаунт с именем `account_name`;  
`weight` — вес, который должен иметь `auth_account` для авторизации. Вес «0» означает удаление `auth_account`;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для обновления полномочий существующего аккаунта в соответствии с полномочиями указанного аккаунта.  
Возвращает подписанную транзакцию.

## update\_account\_auth\_key\(\)

```cpp
annotated_signed_transaction wallet_api::update_account_auth_key(
        string account_name,
        authority_type type,
        public_key_type key,
        weight_type weight,
        bool broadcast
)
```

Параметры:  
`account_name` — имя аккаунта, чьи полномочия \(тип авторизации\) следует обновить;  
`type` — тип авторизации \(`owner`, `active` или `posting`\);  
`key` — публичный ключ, который добавляется к авторизации аккаунта;  
`weight` — вес, который должен иметь публичный ключ в авторизации. Вес «0» означает удаление ключа;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для обновления ключа авторизации для существующего аккаунта.  
Возвращает подписанную транзакцию.

## update\_account\_auth\_threshold\(\)

```cpp
annotated_signed_transaction wallet_api::update_account_auth_threshold(
        string account_name,
        authority_type type,
        uint32_t threshold,
        bool broadcast
)
```

Параметры:  
`account_name` — имя аккаунта, чьи полномочия \(тип авторизации\) следует обновить;  
`type` — тип авторизации \(`owner`, `active` или `posting`\);  
`threshold` — пороговое значение веса, необходимое для авторизации;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для обновления порогового значения веса авторизации для существующего аккаунта.  
Возвращает подписанную транзакцию.

## update\_account\_memo\_key\(\)

```cpp
 annotated_signed_transaction wallet_api::update_account_memo_key(
         string account_name,
         public_key_type key,
         bool broadcast
 )
```

Параметры:  
`account_name` — имя аккаунта, чей ключ будет обновлен;  
`key` — новый публичный ключ `memo`;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для обновления публичного ключа `memo` для существующего аккаунта.  
Возвращает подписанную транзакцию.

## update\_account\_meta\(\)

```cpp
        annotated_signed_transaction wallet_api::update_account_meta(
        string account_name,
        string json_meta,
        bool broadcast
)
```

Параметры:  
`account_name` — имя аккаунта, чьи метаданные будут обновлены;  
`json_meta` — новые метаданные JSON для аккаунта;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для обновления метаданных JSON для существующего аккаунта.  
Возвращает подписанную транзакцию.

## update\_chain\_properties\(\)

```cpp
annotated_signed_transaction wallet_api::update_chain_properties(
        string witness_account_name,
        const optional_chain_props& props,
        bool broadcast
)
```

Параметры:  
`witness_account_name` — имя делегата;  
`props` — перечень новых возможностей блокчейна, за которые голосует делегат;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для принятия новых возможностей блокчейна голосованием отдельного делегата.  
Возвращает подписанную транзакцию.

## update\_witness\(\)

```cpp
annotated_signed_transaction wallet_api::update_witness(
        string witness_name,
        string url,
        public_key_type block_signing_key,
        optional<chain_properties> props,
        bool broadcast
)
```

Параметры:  
`witness_name` — имя делегата;  
`url` — адрес URL, содержащий некоторую информацию о делегате. Пустая строка означает, что старая информация о делегате является актуальной;  
`block_signing_key` — публичный ключ для подписи нового блока. Значение в виде пустой строки блокирует блок;  
`props` — перечень новых возможностей блокчейна, за которые голосует делегат;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для обновления объекта, принадлежащего делегату с именем `witness_name`. Возвращает подписанную транзакцию.

## vote\(\)

```cpp
annotated_signed_transaction wallet_api::vote(
        string voter,
        string author,
        string permlink,
        int16_t weight,
        bool broadcast
)
```

Параметры:  
`voter` — имя голосующего аккаунта;  
`author` — автор комментария, за который нужно проголосовать;  
`permlink` — ссылка на комментарий, за который нужно проголосовать;  
`weight` — вес голоса. Значение должно быть отличным от нуля и находиться в пределах от -100 до 100 включительно;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для голосования за комментарий, оплачиваемый в STEEM.  
Возвращает подписанную транзакцию.

## vote\_for\_witness\(\)

```cpp
 annotated_signed_transaction wallet_api::vote_for_witness(
        string account_to_vote_with,
        string witness_to_vote_for,
        bool approve,
        bool broadcast
)
```

Параметры:  
`account_to_vote_with` — имя аккаунта, голосующего за делегата;  
`witness_to_vote_for` — делегат, за которого идет голосование;  
`approve` — “true”, если аккаунт голосует за делегата;  
`broadcast` — “true”, если транзакция пересылается на демон.

Используется для голосования за конкретного делегата. По умолчанию считается, что аккаунт не отдал ни позитивный, ни негативный голос за делегата. Аккаунту предоставляется возможность отдать либо позитивный, либо негативный голос. Голосование продолжается до тех пор, пока не будет предложено новое голосование.  
Возвращает подписанную транзакцию.

## withdraw\_vesting\(\)

```cpp
annotated_signed_transaction wallet_api::withdraw_vesting(
        string from, asset vesting_shares,
        bool broadcast
)
```

Параметры:  
`from` — имя аккаунта, с кошелька которого снимаются средства в виде VESTS;  
`vesting_shares` — сумма средств в виде VESTS, которая будет еженедельно сниматься с кошелька аккаунта в течение следующих двух лет \(сумма / 104 акции\) и зачисляться на депозит в виде VESTS;  
`broadcast` — “true”, если транзакция пересылается на демон.

Настраивает запрос на вывод средств. Запрос выполняется один раз в неделю в течение последующих двух лет. Возвращает подписанную транзакцию.

