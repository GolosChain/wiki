# API part 1

Автор: [@asuleymanov](https://golos.io/@asuleymanov)

Представляю Вам первую часть моей публикации. В данную часть не вошли 18 из 72 \(раздела Database\_API\) найденных мною команд. Их я опишу позже

_**get\_trending\_tags**_

Параметры:`"method":"get_trending_tags", "params":["after_tag","limit"], "id":4`

Описание: Отображает ограниченный список меток\(тэгов\) включающие словосочетания из первого параметра.

_**get\_block\_header**_

Параметры:`"method":"get_block_header", "params":["blocknumber"], "id":19`

Описание: Отображает краткую информацию по указанному блоку.

_**get\_block**_

Параметры:`"method":"get_block", "params":["blocknumber"], "id":20`

Описание: Отображает расширенную информацию по указанному блоку.

_**get\_ops\_in\_block**_

Параметры:`"method":"get_ops_in_block", "params":["block_num","only_virtual"], "id":21`

Описание: Команда должна отображать только операции в заданном блоке, но к сожалению ответ приходит пустой.

_**get\_state**_

Параметры:`"method":"get_state", "params":["path"], "id":22`

Описание: Отображает текущее состояние сети GOLOS. Оставить путь пустым для текущей информации.

_**get\_trending\_categories**_

Параметры:`"method":"get_trending_categories", "params":["searchafter","limit"], "id":23`

Описание: Позволяет искать ограниченное количество трендов категорий как текущие, так и прошлые.

_**get\_best\_categories**_

Параметры:`"method":"get_best_categories", "params":["after","limit"], "id":24`

Описание: Что именно делает данная команда мне пока непонятно.

_**get\_active\_categories**_

Параметры:`"method":"get_active_categories", "params":["after","limit"], "id":25`

Описание: Что именно делает данная команда мне пока непонятно.

_**get\_recent\_categories**_

Параметры:`"method":"get_recent_categories", "params":["after","limit"], "id":26`

Описание: К сожалению команда выдает все время пустой ответ. Так что неизвестно для чего она нужна. Мое понимание что она должна отображать ограниченное количество активностей в категории начиная с новой.

_**get\_config**_

Параметры:`"method":"get_config", "params":[], "id":27`

Описание: Отображает текущую конфигурацию узла.

_**get\_dynamic\_global\_properties**_

Параметры:`"method":"get_dynamic_global_properties", "params":[], "id":28`

Описание: Отображает различную информацию о текущем состоянии сети GOLOS.

_**get\_chain\_properties**_

Параметры:`"method":"get_chain_properties", "params":[], "id":29`

Описание: Отображает комиссию за создание пользователя, максимальный размер блока и процентную ставку GBG.

_**get\_feed\_history**_

Параметры:`"method":"get_feed_history", "params":[], "id":30`

Описание: Отображает историю конверсий GBG / GOLOS.

_**get\_current\_median\_history\_price**_

Параметры:`"method":"get_current_median_history_price", "params":[], "id":31`

Описание: Отображает текущую медианную цену конвертации GBG / GOLOS.

_**get\_witness\_schedule**_

Параметры:`"method":"get_witness_schedule", "params":[], "id":32`

Описание: Отображает текущее состояние делегирования.

_**get\_hardfork\_version**_

Параметры:`"method":"get_hardfork_version", "params":[], "id":33`

Описание: Отображает текущую версию ХФ GOLOS.

_**get\_next\_scheduled\_hardfork**_

Параметры:`"method":"get_next_scheduled_hardfork", "params":[], "id":34`

Описание: Отображает дату и версию ХФ GOLOS

_**get\_key\_references**_

Параметры:`"method":"get_key_references", "params":["key"], "id":35`

Описание: К сожалению сказать что делает данная команда не возможно. \(Любой её вызов приводит к ошибке\).

_**get\_accounts**_

Параметры:`"method":"get_accounts", "params":[["username"]], "id":36`

Описание: Отображает данные о пользователях указанных в запросе. Можно запрашивать сразу по нескольким пользователям разделив их запятыми.

_**get\_account\_references**_

Параметры:`"method":"get_account_references", "params":["accountid"], "id":37`

Описание: В настоящее время запрос возвращает ошибку. По идее должен выдавать информацию о пользователе по его ID.

_**lookup\_account\_names**_

Параметры:`"method":"lookup_account_names", "params":[["username"]], "id":38`

Описание: Отображает данные о пользователях указанных в запросе. Можно запрашивать сразу по нескольким пользователям разделив их запятыми.

_**lookup\_accounts**_

Параметры:`"method":"lookup_accounts", "params":["username","limit"], "id":39`

Описание: Действует как функция поиска для отображения имен пользователей, содержащих буквы, заданные в первом параметре. Второй параметр задает количество выдаваемых записей.

_**get\_account\_count**_

Параметры:`"method":"get_account_count", "params":[], "id":40`

Описание: Показывает количество пользователей зарегистрированных в сети GOLOS.

_**get\_conversion\_requests**_

Параметры:`"method":"get_conversion_requests", "params":["username"], "id":41`

Описание: Отображает текущие запросы на конвертацию указанным пользователем.

_**get\_account\_history**_

Параметры:`"method":"get_account_history", "params":["username","from","limit"], "id":42`

Описание: История всех действий пользователя в сети GOLOS в виде транзакций.

_**get\_owner\_history**_

Параметры:`"method":"get_owner_history", "params":["username"], "id":43}`

Описание: Отображает имя пользователя если он изменил право собственности на блокчейн.

_**get\_recovery\_request**_

Параметры:`"method":"get_recovery_request", "params":["username"], "id":44`

Описание: Если статус пользователя в настоящее время отмечен для восстановления, вернет true, в противном случае возвращается «null».

_**get\_escrow**_

Параметры:`"method":"get_escrow", "params":["from","escrow_id"], "id":45`

Описание: Данная команда должна отображать операции реализованные с помощью операций посредничества. К сожалению проверить эту команду нету возможности\(у меня\).

_**get\_withdraw\_routes**_

Параметры:`"method":"get_withdraw_routes", "params":["username","withdraw_route_type"], "id":46`

Описание: Команда по идее должна выдавать все переводы\(или только активные\) на счету пользователя в зависимости от типа\(второй параметр\). Который задается строкой вида **incoming**, **outgoing** или **all**. Но к сожалению никаких данных я в ответ не получал, так что сказать что и как выдается в ответ возможности нет.

_**get\_account\_bandwidth**_

Параметры:`"method":"get_account_bandwidth", "params":["username","bandwidth_type"], "id":47`

Описание: Отображает пропускную способность\(пока не понял что это\) пользователя в зависимости от типа.

Тип задается числом:

0- post

1- forum

2- market

3- old\_forum

4- old\_market

_**get\_savings\_withdraw\_from**_

Параметры:`"method":"get_savings_withdraw_from", "params":["username"], "id":48`

Описание: Команды "get\_savings\_withdraw\_from" и "get\_savings\_withdraw\_to" обе отображают данные о выводах из "СЕЙФА" для данного пользователя. Хотя как я понимаю одна из них должна отображать данные о переводе на счет "СЕЙФА" для данного пользователя.

_**get\_savings\_withdraw\_to**_

Параметры:`"method":"get_savings_withdraw_to", "params":["username"], "id":49`

Описание: Команды "get\_savings\_withdraw\_from" и "get\_savings\_withdraw\_to" обе отображают данные о выводах из "СЕЙФА" для данного пользователя. Хотя как я понимаю одна из них должна отображать данные о переводе на счет "СЕЙФА" для данного пользователя.

_**get\_order\_book**_

Параметры:`"method":"get_order_book", "params":["limit"], "id":50`

Описание: Отображает список заявок на внутренней бирже на покупку и продажу в сети GOLOS.

_**get\_open\_orders**_

Параметры:`"method":"get_open_orders", "params":["username"], "id":51`

Описание: Отображает список заявок на внутренней бирже на покупку и продажу в сети GOLOS для указанного пользователя.

_**get\_liquidity\_queue**_

Параметры:`"method":"get_liquidity_queue", "params":["startusername","limit"], "id":52`

Описание: Что именно делает данная команда мне пока непонятно.

_**get\_transaction\_hex**_

Параметры:`"method":"get_transaction_hex", "params":["trx"], "id":53`

Описание: Что именно делает данная команда мне пока непонятно. Так как не понятно что за параметр передается в запрос.

_**get\_transaction**_

Параметры:`"method":"get_transaction", "params":["txid"], "id":54`

Описание: Отображает детали транзакции по заданному ID транзакции.

_**get\_required\_signatures**_

Параметры:`"method":"get_required_signatures", "params":["trx", "availablekeys"], "id":55`

Описание: Что именно делает данная команда мне пока непонятно. Так как не понятно что за параметры передаются в запрос.

_**get\_potential\_signatures**_

Параметры:`"method":"get_potential_signatures", "params":["trx"], "id":56`

Описание: Что именно делает данная команда мне пока непонятно. Так как не понятно что за параметр передается в запрос.

_**verify\_authority**_

Параметры:`"method":"verify_authority", "params":["trx"], "id":57`

Описание: Что именно делает данная команда мне пока непонятно. Так как не понятно что за параметр передается в запрос.

_**verify\_account\_authority**_

Параметры:`"method":"verify_account_authority", "params":["userid/username","signer"], "id":58`

Описание: Что именно делает данная команда мне пока непонятно. Так как не понятно что за параметр передается в запрос.

_**get\_active\_votes**_

Параметры:`"method":"get_active_votes", "params":["username","permalink"], "id":59`

Описание: Отображает список пользователей проголосовавших за указанную запись.

_**get\_account\_votes**_

Параметры:`"method":"get_account_votes", "params":["username"], "id":60`

Описание: Отображает все голоса которые выставлены указанным пользователем.

_**get\_content**_

Параметры:`"method":"get_content", "params":["username","permalink"], "id":61`

Описание: Получает информацию о публикации, за исключением комментариев.

_**get\_content\_replies**_

Параметры:`"method":"get_content_replies", "params":["username","permalink"], "id":62`

Описание: Отображает список всех комментариев для выбранной публикации.

_**get\_discussions\_by\_author\_before\_date**_

Параметры:`"method":"get_discussions_by_author_before_date", "params":["username","start_permalink","before_date","limit"], "id":63`

Описание: Отображает ограниченное количество публикации \(четвертый параметр\) пользователя. Второй параметр задает линк стартовой публикации, если пусто то с самого начала. Третий параметр задает дату до какого момента показывать. Третий параметр является строкой, формат ввода \(YYYY-MM-DDTHH:MM:SS

_**get\_replies\_by\_last\_update**_

Параметры:`"method":"get_replies_by_last_update", "params":["username","start_permalink","limit"], "id":64`

Описание: Странное поведение команды.  
При указание первого и третьего параметра\(обязателен\) отображает ограниченное количество комментариев во всех публикациях заданного пользователя по времени поступления считая от последнего.  
При указании второго параметра отображает непосредственно публикацию и еще ограниченное количество публикаций сторонних авторов.

_**get\_witnesses**_

Параметры:`"method":"get_witnesses", "params":[["witnessid"]], "id":65`

Описание: Отображает данные о делегатах в соответствии с заданными ID. Можно запрашивать данные сразу по нескольким делегатам разделив их ID запятыми.

_**get\_witness\_by\_account**_

Параметры:`"method":"get_witness_by_account", "params":["username"], "id":66`

Описание: Отображает данные о делегате \(если он им является\) в соответствии с данными из запроса.

_**get\_witnesses\_by\_vote**_

Параметры:`"method":"get_witnesses_by_vote", "params":["username/blank", "limit"], "id":67 <br>`  
Описание: Отображает ограниченный список делегатов одобряющих голосование. Если первый параметр пуст то отображаются ведущие делегаты, если первый параметр указан то список начинается с указанного делегата.

_**lookup\_witness\_accounts**_

Параметры:`"method":"lookup_witness_accounts", "params":["search_username", "limit"], "id":68`

Описание: Отображает ограниченный список пользователей, которые объявили о своем намерении работать в качестве делегата.

_**get\_witness\_count**_

Параметры:`"method":"get_witness_count", "params":[], "id":69`

Описание: Отображает количество делегатов.

_**get\_active\_witnesses**_

Параметры:`"method":"get_active_witnesses", "params":[], "id":70`

Описание: Отображает список всех активных делегатов.

_**get\_miner\_queue**_

Параметры:`"method":"get_miner_queue", "params":[], "id":71`

Описание: Создает список майнеров, ожидающих попасть в DPOW цепочку, чтобы создать блок.

## Использование`curl`

Простой способ использовать команду - с помощью`curl`, используя следующий формат:  
`curl --data '{"jsonrpc": "2.0", <Параметры>}' https://ws.golos.io`

Историческая справка

* [Статья № 1](https://golos.io/ru--otkrytyij-kod/@asuleymanov/opisanie-api-golos-chast-1)
  * Начало разбора команд из раздела Database\_Api
* [Статья № 2](https://golos.io/ru--otkrytyij-kod/@asuleymanov/opisani-golosapi-chast-2)
  * Окончание разбора команд из раздела Database\_Api
* [Статья № 3](https://golos.io/ru--otkrytyij-kod/@asuleymanov/opisanie-golosapi-chast-3)
  * Разбор команд из разделов Market\_History\_API и Follow\_API
* [Статья №4](https://golos.io/ru--otkrytyij-kod/@asuleymanov/opisani-golosapi-chast-4)
  * Команды из раздела Network\_Brodcast\_API и Login\_API

