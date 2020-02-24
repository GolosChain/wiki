---
description: ...
---

# API part 4

Автор: [@asuleymanov](https://golos.io/@asuleymanov)

Приветствую постоянных читателей и вновь присоединившихся.  
В предыдущей [статье](https://golos.io/ru--otkrytyij-kod/@asuleymanov/opisanie-golosapi-chast-3) я обещал приоткрыть завесу тайны над тем как именно можно самому добавлять записи в блокчейн. Собственно я опишу команды из раздела Network\_Brodcast\_API и Login\_API. И попробую рассказать о процедуре генерации транзакции, её подписи и отправки в блокчейн.

## Команды раздела Network\_Brodcast\_API

_**broadcast\_block**_  
Параметры:`"method":"broadcast_block", "params":["signed_block"], "id":0`  
Описание: Скорее всего данная команда должна загружать в блокчейн собранный и подписанный блок. Но проверить его мне не удалось.

_**broadcast\_transaction**_  
Параметры:`"method":"broadcast_transaction", "params":["trx"], "id":1`  
Описание: Транзакция будет проверяться на достоверность в локальной базе данных до начала трансляции. Если он не может применяться локально, будет вызвана ошибка, и транзакция не будет транслироваться.

_**broadcast\_transaction\_synchronous**_  
Параметры:`"method":"broadcast_transaction_synchronous", "params":["trx"], "id":2`  
Описание: Этот вызов не будет возвращен до тех пор, пока транзакция не будет включена в блок.

_**broadcast\_transaction\_with\_callback**_  
Параметры:`"method":"broadcast_transaction_with_callback", "params":["confirmationCallback","trx"], "id":3`  
Описание: Эта версия широковещательной транзакции регистрирует метод обратного вызова, который будет вызываться, когда транзакция включена в блок. Метод обратного вызова включает идентификатор транзакции, номер блока и номер транзакции в блоке.

### Команды раздела Login\_API

_**login**_  
Параметры:`"method":"login", "params":["username","password"], "id":0`  
Описание: Позволяет подключаться к учетным записям в сети GOLOS.

_**get\_api\_by\_name**_  
Параметры:`"method":"get_api_by_name", "params":["apiname"], "id":1`  
Описание: Возвращает уникальный идентификатор API по его имени.Пример идентификатора "login\_api" или "follow\_api".

_**get\_version**_  
Параметры:`"method":"get_version", "params":[], "id":2`  
Описание: Возвращает данные о версии компонентов блокчейн

## Процедура подписания и отправки

Для примера я возьму самую простую операцию голосования. С её помощью я разберу как происходит генерация транзакции и её подписание.

### Операция

Первым шагом мы создаем операцию.  
Если посмотреть на саму операцию то она имеет вид

```text
['vote',
   {'author': 'author',
    'permlink': 'permlink',
    'voter': 'voter',
    'weight': weight}]
```

Тут можно четко определить:

* тип операции `vote`\(голосование\)
* собственно саму публикацию за которую производиться голосование \(параметры `author` и `permlink`\)
* кто голосует \(параметр `voter`\)
* и соответственно вес голоса который он готов отдать \(параметр `weight`\)

### Транзакция

Следующим шагом мы создаем непосредственно транзакцию с необходимой нам операцией. Транзакция может содержать от одной до нескольких операций. В нашем случае это 1 операция голосования и сгенерированная транзакция выглядит так:

```text
trx = {'ref_block_num': 36029,
    'ref_block_prefix': 1164960351,
    'expiration': '2017-06-13T12:24:17',
    'operations': [['vote',
                     {'author': 'xeroc',
                      'permlink': 'piston',
                      'voter': 'xeroc',
                      'weight': 10000}]],
    'extensions': [],
    'signatures': [],
}
```

Можно заметить что наша операция является частью массива`operations`.  
Давайте разберем те поля в транзакции которые у нас появились, но нам неизвестны.

* Параметр

  `ref_block_num`

  указывает на номер предыдущего блока.

* Параметр

  `ref_block_prefix`

  получаем из идентификатора блока, а именно последние 4 байта.

* Параметр

  `expiration`

  проставляет время истечения действия транзакции. Если до этого времени не произошло включение транзакции в блок, то она считается недействительной.

* Параметр

  `extensions`

  расширение.

* Параметр

  `signatures`

  собственно подпись. Данный параметр проставляется в момент подписания.

Примет получения параметров`ref_block_num`и`ref_block_prefix`на python:

```text
DGP = get_dynamic_global_properties()
ref_block_num = DGP["head_block_number"] 
&0xFFFF
ref_block_prefix = struct.unpack_from("<I", unhexlify(DGP["head_block_id"]), 4)[0]
```

Цель этих двух параметров для предотвращения повторения запросов.

### Сериализация \(приведение к жесткой структуре\)

Прежде чем переходить непосредственно к процессу подписи. Надо сериализовать \(привести к жесткой структуре\) транзакцию. Это необходимо чтобы на стороне ноды было проще проверить правильность подписания транзакции. Т.е. после такой обработки мы будем точно знать порядок в каком располагались поля при подписании.  
При этом из процесса сериализации исключается параметр`signatures`и`extensions`.

И так на примере нашей транзакции порядок будет таким \(Все примеры будут на языке python\):

1. Создание буфера

   `buf = b""`

2. Добавление в буфер параметра

   `ref_block_num`

   `buf += struct.pack("<H", trx["ref_block_num"])`

3. Добавление в буфер параметра

   `ref_block_prefix`

   `buf += struct.pack("<I", trx["ref_block_prefix"])`

4. Добавление в буфер параметра

   `expiration`

   . Сам параметр при этом преобразуется в целое число \(uint32\)

```text
timeformat = '%Y-%m-%dT%H:%M:%S%Z'
buf += struct.pack("<I", timegm(time.strptime((trx["expiration"] + "UTC"), timeformat)))
```

1. Добавление в буфер количество операций в нашей транзакции

   `buf += bytes(varint(len(trx["operations"])))`

2. Добавление непосредственно полей операции. Первое что добавляем это код операции. Он определяется из массива начинающегося с 0.

   **Список операций из исходников**

```text
                vote_operation,
                comment_operation,
                transfer_operation,
                transfer_to_vesting_operation,
                withdraw_vesting_operation,
                limit_order_create_operation,
                limit_order_cancel_operation,
                feed_publish_operation,
                convert_operation,
                account_create_operation,
                account_update_operation,
                witness_update_operation,
                account_witness_vote_operation,
                account_witness_proxy_operation,
                pow_operation,
                custom_operation,
                report_over_production_operation,
                delete_comment_operation,
                custom_json_operation,
                comment_options_operation,
                set_withdraw_vesting_route_operation,
                limit_order_create2_operation,
                challenge_authority_operation,
                prove_authority_operation,
                request_account_recovery_operation,
                recover_account_operation,
                change_recovery_account_operation,
                escrow_transfer_operation,
                escrow_dispute_operation,
                escrow_release_operation,
                pow2_operation,
                escrow_approve_operation,
                transfer_to_savings_operation,
                transfer_from_savings_operation,
                cancel_transfer_from_savings_operation,
                custom_binary_operation,
                decline_voting_rights_operation,
                reset_account_operation,
                set_reset_account_operation,

                /// virtual operations below this point
                fill_convert_request_operation,
                author_reward_operation,
                curation_reward_operation,
                comment_reward_operation,
                liquidity_reward_operation,
                interest_operation,
                fill_vesting_withdraw_operation,
                fill_order_operation,
                shutdown_witness_operation,
                fill_transfer_from_savings_operation,
                hardfork_operation,
                comment_payout_update_operation
```

Далее идут поля непосредственно из операции в определенном порядке. На данный момент я смог выявить порядок только для 3-х операций.

> Операция`vote`
>
> ```text
> (voter)
> (author)
> (permlink)
> (weight)
> ```
>
> Операция`comment`\(эта одна операция но может выполнять два действия. Непосредственно комментировать и создавать новую публикацию\)
>
> ```text
> (parent_author) // если этот параметр пустой то это считается созданием новой публикации
> (parent_permlink)
> (author)
> (permlink)
> (title)
> (body)
> (json_metadata)
> ```

В нашем случае это выглядит в таком виде :

```text
if op[0] == "vote":
    opdata = op[1]
    buf += (varint(len(opdata["voter"])) + bytes(opdata["voter"], "utf-8"))
    buf += (varint(len(opdata["author"])) + bytes(opdata["author"], "utf-8"))
    buf += (varint(len(opdata["permlink"])) + bytes(opdata["permlink"], "utf-8"))
    buf += struct.pack("<h", int(opdata["weight"]))
```

#### Результат

Приблизительный разбор результата:  
Во- первых , параметр ref\_block\_num\( 36029\)  
`bd8c..............................................................`

и параметр ref\_block\_prefix\( 1164960351\)  
`....5fe26f45......................................................`

Затем мы добавим время истечения транзакции`expiration`2016-08-08T12:24:17  
`............f179a857..............................................`

После этого нам нужно добавить количество операций \(01\)  
`....................01............................................`

И непосредственно саму операцию:  
Идентификатор операции \(00\)  
`......................00..........................................`

Голосующий`voter`  
`........................057865726f63..............................`

Автор публикации`author`  
`....................................057865726f63..................`

Ссылка на публикацию`permlink`  
`................................................06706973746f6e....`

и вес голоса`10000`  
`..............................................................1027`

Результат сериализации предстает приблизительно в таком виде:  
`bd8c5fe26f45f179a8570100057865726f63057865726f6306706973746f6e1027`

### Подписание

И вот самая тяжелая часть всего процесса. Для меня это пока тоже темный лес, но попробую описать хоть как то.  
Для подписания транзакции нам понадобится:

* Сериализованный буфер
* Идентификатор цепи chainid \(STEEM:

  `0000000000000000000000000000000000000000000000000000000000000000`

  ; GOLOS

  `782a3039b478c839e4cb0c941ff4eaeb7df40bdd68bd441afd444b9da763de12`

  \)

* Секретный ключ \(в нашем случае posting key\)

Вместо того чтобы подписывать непосредственно саму транзакцию мы подпишем 256SHA хэш, так называемый дайджест сообщения. Получить его можно таким способом:

```text
message = unhexlify(chainid) + buf
digest = hashlib.sha256(message).digest()
```

Теперь мы используя секретные ключи подписываем нашу транзакцию. Каждый секретный ключ приведет к одной подписи , которая должна быть добавлена в параметр signatures первоначальной транзакции.  
В нашем случае, мы просто работаем с одним закрытым ключом, представленным в WIF. Получим фактический двоичный закрытый ключ от WIF \(для простоты используется класс PrivateKey от steembase.account\)

```text
wifs = ["5JLw5dgQAx6rhZEgNN5C2ds1V47RweGshynFSWFbaMohsYsBvE8"]
sigs = []
for wif in wifs:
    p = bytes(PrivateKey(wif))  # binary representation of private key
    sk = ecdsa.SigningKey.from_string(p, curve=ecdsa.SECP256k1)
```

Теперь реализовываем цикл , который имеет решающее значение , поскольку система принимает только канонические подписи , и мы не имеем никакого способа узнать является ли подпись канонической. При этом мы получаем детерминированный параметр для подписания ECDSA и при создании этого параметра будем добавлять наш счетчик цикла в дайджест перед хэшированием.

```text
cnt = 0
i = 0
while 1 :
    cnt += 1
    # Deterministic k
    #
    k = ecdsa.rfc6979.generate_k(
        sk.curve.generator.order(),
        sk.privkey.secret_multiplier,
        hashlib.sha256,
        hashlib.sha256(digest + bytes([cnt])).digest())
```

Подпись генерируется с помощью соответствующего вызова подписи ECDSA:

```text
# Sign message

sigder = sk.sign_digest(
    digest,
    sigencode=ecdsa.util.sigencode_der,
    k=k)
```

Теперь создаем подпись,и проверяем является ли она канонической. Если это так, то мы разрываем цикл и продолжаем:

```text
# Reformating of signature

r, s = ecdsa.util.sigdecode_der(sigder, sk.curve.generator.order())
signature = ecdsa.util.sigencode_string(r, s, sk.curve.generator.order())

# Make sure signature is canonical!

lenR = sigder[3]
lenS = sigder[5 + lenR]
if lenR is 32 and lenS is 32 :
    # ........
```

После того, как мы обеспечили каноничность, мы добавляем дополнительные параметры. Это упрощает проверку подписи , так как она привязывает подпись к одному уникальному открытому ключу. Без этих параметров системе необходимо будет проверить несколько открытых ключей вместо одного. Мы добавляем 4 и 27 , чтобы осталась совместимость с другими протоколами и получаем нашу подпись.

```text
# Derive the recovery parameter

i = recoverPubkeyParameter(digest, signature, sk.get_verifying_key())
i += 4   \# compressed
i += 27  \# compact
break
```

После получения канонической подписи, мы форматируем её в шестнадцатеричном представлении и добавляем к нашей транзакции в параметр`signatures`. Этот вид подписи , называется компактной подписью.

```text
trx["signatures"].append(
    hexlify(
        struct.pack("<B", i) +
        signature
    ).decode("ascii")
)
```

### Отправка

После довольно сложной процедуры подписания, отправка кажется элементарным действием.  
Для того чтобы быть уверенными что проблем при отправке нету лучше всего использовать команду`broadcast_transaction_synchronous`\(так как при её использовании система возвращает ответ\), а параметр для команды будет являться непосредственно`trx`.  
В ответе будет вот такая структура:

```text
    ID       string `json:"id"`
    BlockNum uint32 `json:"block_num"`
    TrxNum   uint32 `json:"trx_num"`
    Expired  bool   `json:"expired"`
```

Этим постом я завершаю цикл по разбору GOLOS API

**Историческая справка**

* [Статья № 1](https://golos.io/ru--otkrytyij-kod/@asuleymanov/opisanie-api-golos-chast-1)
  * Начало разбора команд из раздела Database\_Api
* [Статья № 2](https://golos.io/ru--otkrytyij-kod/@asuleymanov/opisani-golosapi-chast-2)
  * Окончание разбора команд из раздела Database\_Api
* [Статья № 3](https://golos.io/ru--otkrytyij-kod/@asuleymanov/opisanie-golosapi-chast-3)
  * Разбор команд из разделов Market\_History\_API и Follow\_API

