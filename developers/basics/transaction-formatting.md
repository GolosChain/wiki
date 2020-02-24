# Формирование транзакций

Есть правила формирования транзакций, заложенные в протокол Golos. Данные, подписываемые криптографическим ключом, должны соответствовать всем структурам и правилам бинарного представления, заложенным в код. В данном разделе вы узнаете как кодируются ключи, как происходит бинарное представление данных или их подготовка.

## Структура транзакции для подписи

Структура данных, бинарное представление которой и нужно подписывать используя ключи соответствующих типов доступа используемых в аккаунтах вложенных операций, содержит следующие поля:

* **chain\_id** — идентификатор цепи, в Golos это fc::sha256::hash от строки `GOLOS`: `782a3039b478c839e4cb0c941ff4eaeb7df40bdd68bd441afd444b9da763de12`. Стоит отметить, что fc::sha256::hash преобразует строку в c\_str, добавляя в начало ее hex значения `56495A` длину строки, в итоге sha256 рассчитывается от hex значения `0356495A`;
* **tapos\_link** — [Transactions as Proof of Stake концепция](state.md) заключается в том, чтобы каждая транзакция ссылалась на конкретный блок, который должен быть в цепи для ее работы. Бинарное представление является отображением параметров транзакции ref\_block\_num и ref\_block\_prefix.
* **expiration** — unixtime экспирации транзакции \(она должна быть включена в цепь до времени экспирации\);
* **operations** — массив операций находящихся в транзакции, бинарное представление каждой операции состоит из всех аттрибутов операции согласно протоколу;
* **extensions** — массив служебных расширений транзакции \(не используется, поэтому в бинарном формате представляет собой hex значение массива без элементов: `00`\);

## Зачем необходим chain\_id

Открытый и свободный код блокчейн систем основанных на Graphene позволяют запускать новые цепочки, как без изменений, так и полностью переработанные с собственными механиками и экономикой. Более того, множество проектов запускают публичные тестовые цепочки для проверки изменений. Чтобы ноды не путались и транзакции из одной сети нельзя было применять в форке \(или цепи с аналогичными аккаунтами и ключами\) — существует идентификатор цепи, который присутствует как метка в каждой транзакции и операции подписи.

## Формат ключей

Приватные и публичные ключи в Golos находятся по [алгоритму ESDCA](https://ru.wikipedia.org/wiki/ECDSA) \([теория](https://habr.com/ru/post/188958/)\), и используют криптографию для проверки подписей набора данных. Многие разработчики не являясь специалистами в криптографии просто используют специализированные библиотеки, не вдаваясь в подробности.

Рассмотрим этапность преобразования приватного ключа \(состоящего из 32 байт\) в читаемый WIF формат:

* Ключу добавляем бинарный префикс в hex представлении `80`;
* Вычисляем sha256 хэш от sha256 хэша бинарного представления ключа для контрольной суммы \(checksum\);
* Добавляем в конец ключа первые 8 байт от контрольной суммы;
* Кодируем полученный бинарный результат через base58 алгоритм с использованием алфавита: `123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz`;

Этапность преобразование публичного ключа \(состоящего из 32 байт\) в читаемый формат:

* Получаем контрольную сумму хэшированием бинарного представления ключа [алгоритмом ripemd160](https://ru.wikipedia.org/wiki/RIPEMD-160);
* Добавляем в конец ключа первые 8 байт от контрольной суммы;
* Кодируем полученный бинарный результат через base58 алгоритм с использованием алфавита: `123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz`;
* Добавляем префикс сети \(строковое значение `GOLOS`\);

В библиотеке golos-js используется модуль `auth` \([ссылка на GitHub](https://github.com/golos-blockchain/golos-js/blob/master/src/auth/index.js)\), который позволяет предустановленными методами работать с ключами и подписывать данные.

## Представление разных типов данных в бинарном виде

* **string** — строковое значение в бинарном виде представляет собой байт содержащий длину строки и саму строку \(например, строковое значение логина аккаунта `escrow` будет соответствовать hex значению `06657363726f77` в бинарном представлении\);
* **integer** — числовое значение переворачивается в бинарном представлении и пустая размерность заполняется нулями \(если задан тип значения\). Например, процент апвоута указываемый в операции `upvote` представляет собой uint16\_t, для передачи которого достаточно 2 байта. Если необходимо передать значением 10.00%, то в целочисленном значении это будет 1000, в hex представлении `03EB`, перевернутое значение которого будет `EB03`. 
* **date** — поля дат из JSON представления записаны в ISO формате \(например, `2019-02-07T06:19:23` в часовом поясе UTC+0, он же GMT\). Для их бинарного значения записывается в десятичном формате unixtime по правилам представления **integer**. Пример `2019-02-07T06:19:23` в unixtime будет `1549520363` \(hex значение `5C5BCDEB`\), который в бинарном значении будет представлять собой hex: `EBCD5B5C`.
* **asset** — бинарное значение токенов GOLOS представляет собой последовательность значений: целочисленный integer размерностью 8 байт без точности \(0.012 будет представлять собой 12, 1.002 будет представлять собой 1002\). Например: `1.002 GOLOS` в JSON значении внутри операции будет иметь бинарное представление в hex: `EA030000000000000356495A`.
* **public\_key** — значение публичного ключа в бинарном представлении содержит 33 байта, первый байт — значение для восстанавления публичного ключа \([recovery id в ECDSA](https://crypto.stackexchange.com/questions/18105/how-does-recovering-the-public-key-from-an-ecdsa-signature-work)\), 16 байт — координаты точки публичного ключа по оси X, последние 16 байт по оси Y. Например, бинарное значение `026a1dbaacb805f145f9276025627102152840bb1aa09b7fac580f892d93b572b4` соответствует приватному ключу с recovery\_id равным `02` и точке с координатами X в hex представлении `6a1dbaacb805f145f927602562710215` и Y в hex представлении `2840bb1aa09b7fac580f892d93b572b4`. Что соответствует публичному ключу `GLS5hDwvV1PPUTmehSmZecaxo1ameBpCMNVmYHKK2bL1ppLGRvh85`.
* **operation\_type** — тип операции представляет собой целочисленное значение номера операции по протоколу GOLOS записанное в 1 байт \(подробнее читайте в разделе [Операции и их типы](operations.md)\). Например, операция `transfer` в бинарном виде будет иметь запись в hex `02`.

## Пример структуры транзакции

Разберем пример транзакции в формате JSON и её представление в бинарном виде:

```javascript
{"ref_block_num":9023,"ref_block_prefix":1971875185,"expiration":"2019-02-07T06:19:23","operations":[["transfer",{"from":"test1","to":"test2","amount":"1.002 GOLOS","memo":"<3"}]],"extensions":[]}
```

* **ref\_block\_num** — ссылка на номер блока после побитового «и» с hex `ffff` \(например, число 9023 в hex представлении `233F`, согласно представлению integer должно быть перевернуто, получаем `3F23`\);
* **ref\_block\_prefix** — 4 байта \(5, 6, 7, 8\) от бинарного состояния идентификатора блока в десятичном формате, которое можно получить API запросом `get_block_header` с номером следующего блока \(9024\) к плагину database\_api. Ответ будет содержать поле `previous` с идентификатором `0000233F716D887523BB63AD3E6107C96EDCFD8A` искомого блока. Берем `716D8875` для бинарного представления, переворачиваем байты — `75886D71` и переводим в десятичный формат для JSON: `1971875185`.
* **expiration** — unixtime экспирации транзакции. `2019-02-07T06:19:23` в unixtime будет `1549520363` \(hex значение `5C5BCDEB`\), который в бинарном значении будет перевернут и представлять собой hex: `EBCD5B5C`.
* **operations** — массив операций \(так как в массиве один элемент, hex: `01`\);
  * **transfer** — операция перевода токенов \(по нумерации операции в протоколе hex: `02`\);
    * **from** — логин аккаунта отправителя \(длина строки `test1` и hex представление: `057465737431`\);
    * **to** — логин аккаунта получателя \(длина строки `test2` и hex представление: `057465737432`\);
    * **amount** — передаваемое количество токенов GOLOS \(`1.002 GOLOS` в hex: `EA030000000000000356495A00000000`\);
    * **memo** — заметка для получателя \(длина строки `<3` и hex представление: `023C33`\);
* **extensions** — массив служебных расширений транзакции \(так как не используется и не имеет элементов, то имеет представление `00`\).

Итоговое бинарное представление данных транзакции в hex: `3F23716D8875EBCD5B5C0102057465737431057465737432EA030000000000000356495A00000000023C3300`;

Чтобы отправить транзакцию в блокчейн, необходимо дополнить данное представление **chain\_id** в начале и подписать приватным ключом. Полученную подпись необходимо добавить в JSON поле массив `signatures`, например:

```javascript
{"ref_block_num":9023,"ref_block_prefix":1971875185,"expiration":"2019-02-07T06:19:23","operations":[["transfer",{"from":"test1","to":"test2","amount":"1.002 GOLOS","memo":"<3"}]],"extensions":[],"signatures":["1f500f2a5d721e45c53e76fca786d690c7c0556f1923aa07c944e26614b50481d353e88f82e731be74c18e3fb8d117dc992a475991974b6e1364a66f5ccb618f83"]}
```

И передать этот JSON через API запрос `broadcast_transaction` плагину `network_broadcast_api`.

## Получение ref\_block\_num и ref\_block\_prefix для формирования транзакции

Нода блокчейна хранит идентификаторы последних 65537 блоков \(подробнее читайте [про концепцию TaPoS](state.md)\). Обычно разработчики ссылаются на один из последних блоков, обычно, выполняя очередь действий:

* Получают данные о состоянии системы через API запрос `get_dynamic_global_properties` к плагину `database_api`;
* Используя значение поля `head_block_number` минус 3 блока устанавливают для какого блока будут формировать ref\_block\_num и запрашивать его идентификатор;
* Получают идентификатор используемого блока, выполняя API запрос `get_block_header` к плагину `database_api`, запрашивая искомый блок плюс один блок \(так как заголовок каждого блока содержит ссылку на идентификатор прошлого блока, искомый идентификатор находится в следующем\);
* Из идентификатора формируют ref\_block\_prefix.

Большинство библиотек которые содержат абстракции для упрощения вызовов и трансляции транзакции в блокчейн делают это самостоятельно.

Пример получения `ref_block_num` и `ref_block_prefix` на PHP в [исходном коде библиотеки php-graphene-node-client](https://github.com/t3ran13/php-graphene-node-client/blob/d3521ad5ae8866771adb0cb5ffd4ccadf6c892dc/Tools/Transaction.php#L64).

## Порядок сериализации данных в операции

Все операции и их параметры записаны в протоколе GOLOS и находятся [в файле steem\_operations.hpp](https://github.com/golos-blockchain/golos/blob/master/libraries/protocol/include/golos/protocol/steem_operations.hpp).

Именно там можно изучить типы параметров и их требуемый порядок в операции. **Внимание!** Порядок параметров в структуре операции не совпадает с порядком параметров в самой операции. Рассмотрим пример на операции `escrow_transfer_operation`, структура операции \(часто перед операцией присутствует комментарий её описывающий\):

```cpp
/**
 *  The purpose of this operation is to enable someone to send money contingently to
 *  another individual. The funds leave the *from* account and go into a temporary balance
 *  where they are held until *from* releases it to *to* or *to* refunds it to *from*.
 *
 *  In the event of a dispute the *agent* can divide the funds between the to/from account.
 *  Disputes can be raised any time before or on the dispute deadline time, after the escrow
 *  has been approved by all parties.
 *
 *  This operation only creates a proposed escrow transfer. Both the *agent* and *to* must
 *  agree to the terms of the arrangement by approving the escrow.
 *
 *  The escrow agent is paid the fee on approval of all parties. It is up to the escrow agent
 *  to determine the fee.
 *
 *  Escrow transactions are uniquely identified by 'from' and 'escrow_id', the 'escrow_id' is defined
 *  by the sender.
 */
struct escrow_transfer_operation : public base_operation {
    account_name_type from;
    account_name_type to;
    account_name_type agent;
    uint32_t escrow_id = 30;

    asset token_amount = asset(0, TOKEN_SYMBOL);
    asset fee;

    time_point_sec ratification_deadline;
    time_point_sec escrow_expiration;

    string json_metadata;

    void validate() const;

    void get_required_active_authorities(flat_set<account_name_type> &a) const {
        a.insert(from);
    }
};
```

Порядок параметров в операции задается уже в конце файла методом:

```cpp
FC_REFLECT((graphene::protocol::escrow_transfer_operation), (from)(to)(token_amount)(escrow_id)(agent)(fee)(json_metadata)(ratification_deadline)(escrow_expiration));
```

Кроме описания структуры операции есть еще обработка параметров в методе `validate`, найти который можно [в файле steem\_operations.cpp](https://github.com/golos-blockchain/golos/blob/master/libraries/protocol/steem_operations.cpp#L588):

```cpp
void escrow_transfer_operation::validate() const {
    validate_account_name(from);
    validate_account_name(to);
    validate_account_name(agent);
    FC_ASSERT(fee.amount >= 0, "fee cannot be negative");
    FC_ASSERT(token_amount.amount >=
              0, "tokens amount cannot be negative");
    FC_ASSERT(from != agent &&
              to != agent, "agent must be a third party");
    FC_ASSERT(fee.symbol == TOKEN_SYMBOL, "fee must be TOKEN_SYMBOL");
    FC_ASSERT(token_amount.symbol ==
              TOKEN_SYMBOL, "amount must be TOKEN_SYMBOL");
    FC_ASSERT(ratification_deadline <
              escrow_expiration, "ratification deadline must be before escrow expiration");
    validate_json_metadata(json_metadata);
}
```

Большинство операций проверяют наличие подписи соответствующих полномочий, например в структуре `escrow_transfer_operation` присутствует проверка подписи инициатора операции \(поле `from`\) активных полномочий в методе `get_required_active_authorities`.

