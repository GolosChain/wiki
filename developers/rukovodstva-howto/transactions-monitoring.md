# Мониторинг транзакций аккаунта

## Как мониторить учетную запись

Пошаговая инструкция для мониторинга учетной записи

В данном документе объяснено, как следует мониторить учетные записи и направлять средства на определённую учетную запись, используя Python или JavaScript. Это также полезное введение в тему блокчейна, используемого в Голосе.

## Блокчейн Голоса

Используемый в Голосе блокчейн является аналогом блокчейна Стима, и очень похож на технологию графена, используемую в других блокчейнах. Блокчейн состоит из хешируемых блоков, в которые записывается несколько транзакций. В отличии от блокчейна биткоина каждая транзакция в данном случае может выполнять определённые задачи \(трансфер, голосование, комментарии, перевод и прочее\).

В этом документе излагается, как следует читать и интерпретировать операции трансфера в случае запросов от пользователей на перевод средств. Для того, чтобы различать пользователей, используются memos, которые присваиваются каждому трансферу. Обратите внимание, что данные memos хранятся в блокчейне в виде обычного текста.

### Операции трансфера

Операции трансфера выглядят следующим образом:

```text
        {
         "from": "hello",
         "to": "world",
         "amount": "10.000 STEEM",
         "memo": "mymemo"
        }
```

В данном случае from и to используются для идентификации отправителя и получателя. Переменная amount - строка с пробелами, содержащая количество средств и символ ГОЛОС. Как указывалось выше, пользователь может присоединить к транзакции memo, которое будет сохранено в блокчейн в виде обычного текста.

### Операции

Каждая операция имеет идентификатор \(например, transfer\), определённые данные, специфические для данной операции, и группируется в массив операций:

```text
[
 [OperationType, {operation_data}],
 [OperationType, {operation_data}],
 [OperationType, {operation_data}],
]
```

Несколько операций могут быть сгруппированы вместе. При этом все они имеют форму: OperationType, {data}\]: """ \[ \["transfer", { "from": "hello", "to": "world", "amount": "10.000 STEEM", "memo": "mymemo" } \], \["transfer", { "from": "world", "to": "trade", "amount": "15.000 STEEM", "memo": "Gift!" } \] \] """

Набор операций выполняется в заданном порядке. Принимая во внимание тот факт, что в Голосе заложен последовательный порядок выполнения операций, все операции выполняются автоматически, согласно коду.

### Транзакции

Набор операций записывается в транзакцию, в которой имеются все необходимые подписи задействованных в транзакции учётных записей, срок окончания действия и параметры необходимые для алгоритма консенсуса TaPOS/DPOS. """ \[ {"ref\_block\_num": 29906, "ref\_block\_prefix": 1137201336, "expiration": "2016-03-30T07:15:00", "operations": \[\[ "transfer",{ "from": "hello", "to": "world", "amount": "10.000 STEEM", "memo": "mymemo" } \] \], "extensions": \[\], "signatures": \["20326......"\] } \] """

### Блок

Несколько транзакций от разных источников далее группируются в блок производителями блоков \(например, майнерами по алгоритму доказательства работы или делегатами\). Блок имеет обычные для блокчейн параметры, такие как корень дерева Меркель, хэш предыдущего блока и транзакции. """ { "previous": "000274d2b850c8433f4c908a12cc3d33e69a9191", "timestamp": "2016-03-30T07:14:33", "witness": "batel", "transaction\_merkle\_root": "f55d5d65e27b80306c8e33791eb2b24f58a94839", "extensions": \[\], "witness\_signature": "203b5ae231c....", "transactions": \[{ "ref\_block\_num": 29906, "ref\_block\_prefix": 1137201336, "expiration": "2016-03-30T07:15:00", "operations": \[\[ "transfer",{ "from": "hello", "to": "world", "amount": "10.000 STEEM", "memo": "mymemo" } \] \], "extensions": \[\], "signatures": \[ "20326d2fe6e6ba..." \] } \], "block\_id": "000274d3399c50585c47036a7d62fd6d8c5b30ad", "signing\_key": "STM767UyP27Tuak3MwJxfNcF8JH1CM2YMxtCAZoz8A5S8VZKQfZ8p", "transaction\_ids": \[ "64d45b5497252395e38ed23344575b5253b257c3" \] } """ Использование операции get\_block  возвращает идентификаторы ids транзакций \(то есть хэши подписанных отправителем транзакций\), по которым можно идентифицировать уникальным образом транзакцию и соответственно набор операций, содержащийся в ней.

### Мониторинг сбережений

Разобравшись с блокчейн Голоса, можно приступать к мониторингу сбережений

### Запуск ноды \(узла\)

Во-первых, запускать полную ноду требуется в ситуации, когда есть доверие к аппаратному обеспечению """ ./programs/steemd/steemd --rpc-endpoint=127.0.0.1:8092

Открыв конечную точку RPC, можно настроить связь с нодой посредством операций RPC-JSON

Запрос get\_config вернёт конфигурацию блокчейна с данными об интервалах блоков в секундах. Запрос get\_dynamic\_global\_properties вернёт данные с текущим номером головного блока и номером последнего блока, который не может быть изменён. Текущий блок - это последний блок, который был произведён сетью и подтверждён производителем блока. Последний блок, который не может быть изменен, - это блок, который подтверждён большинством майнеров и соответственно не может быть изменён без хард форка. Каждый блок более старший , чем последний блок, который может быть ещё отозван, эквивалентен точке проверки в биткоине. В начале за головным Блоком находится 30-50 блоков. После 30 первых дней после запуска сети за головням находится около 15 блоков.

### Обработка данных блока

Поскольку содержание блока не зашифровано, для мониторинга учетной записи требуется только обработать содержание каждого блока.

## Пример

Ниже приведён пример для мониторинга учетной записи.

### Python

Эта библиотека доступна по адресу [https://github.com/xeroc/python-steem](https://github.com/xeroc/python-steem)

""" from steemrpc import SteemRPC from pprint import pprint import time """ Параметры для установки соединения с steemd daemon. Запускайте steemd daemon с rpc-endpoint:

```text
      ./programs/steemd/steemd --rpc-endpoint=127.0.0.1:8092
```

Это откроет порт RPC \(на 8092\). В настоящий момент аутентификация не работает, поэтому рекомендуем ограничить доступ к локальному хостингу. Позже будет возможна аутентификация посредством username, password.

""" rpc = SteemRPC\("localhost", 8092, "", ""\)

"""

Последний блок, который вы обработали. Обработка будет продолжена с 'last\_block + 1\` """ last\_block = 160900

"""

Указывайте учетную запись для мониторинга """ watch\_account = "world"

def process\_block\(block, blockid\): """

Этот запрос обрабатывает блок, который может содержать множество транзакций

```text
     :param Object block: block data
     :param number blockid: block number
 """
 if "transactions" in block:
     for tx in block["transactions"]:
         #: Выбор операций 
         for opObj in tx["operations"]:
              #:Каждая операция - это массив данных в виде
             #:    [type, {data}]
             opType = opObj[0]
             op = opObj[1]

              # в данном случае обрабатываются только трансферы 
             if opType == "transfer":
                 process_transfer(op, block, blockid)
```

def process\_transfer\(op, block, blockid\): """

В данном случае обрабатываются действительные операции трансфера """ if op\["to"\] == watch_account: print\( "%d \| %s \| %s -&gt; %s: %s -- %s" % \( blockid, block\["timestamp"\], op\["from"\], op\["to"\], op\["amount"\], op\["memo"\] \) \) if **name** == '\_main_': """

Давайте узнаем, как часто генерятся блоки """ config = rpc.get\_config\(\) block\_interval = config\["STEEMIT\_BLOCK\_INTERVAL"\] """

Бесконечное зацикливание """ while True: """ Получите свойства цепочки для идентификации головного/последнего возможного к отмене блока """ props = rpc.get\_dynamic\_global\_properties\(\) """ Получайте номер блока. Здесь на выбор имеется

* head\_block\_number: последний блок
* last\_irreversible\_block\_num: блок, который подтверждён 2/3 всех производителей блоков, и потому безотзывной.

Предлагается использовать последнюю переменную """ block\_number = props\['last\_irreversible\_block\_num'\] """

Пропустите в цикле все блоки, которые возникли после блока, определенного выше """ while \(block\_number - last\_block\) &gt; 0: last\_block += 1 """ Получите полный блок """ block = rpc.get\_block\(last\_block\) """ Обработайте блок """ process\_block\(block, last\_block\) """ Пропустите один блок """ time.sleep\(block\_interval\) """

#### NodeJS

""" var rpc = require\('node-json-rpc'\);

var last\_block = 160900; var watch\_account = "world"; var options = { port: 8092, host: '127.0.0.1', path: '/rpc', strict: false };

var callrpc = function\(name, params, cb\) { client.call\( {"jsonrpc": "2.0", "method": name, "params": params, "id": 0}, function\(err, res\) { if \(err\) { cb\(err, null\) } else { cb\(null, res\["result"\]\) } } \); }

var process\_transfer = function\(op, block, blockid\) { console.log\(block\); if \(op\["to"\] == watch\_account\) { console.log\(blockid + " \| " + block\["timestamp"\] + " \| "+ op\["from"\] + " -&gt; " + op\["to"\] + ": " + op\["amount"\] + " -- " + op\["memo"\] \); } }

var process\_block = function\(block, blockid\) { console.log\(blockid\); console.log\(block\["transactions"\]\); for \(var tx in block\["transactions"\]\) { for \(var opObj in tx\["operations"\]\) { var opType = opObj\[0\]; var op = opObj\[1\];

```text
         console.log(op);

         if (opType == "transfer") {
             process_transfer(op, block, blockid);
         }
     }
 }
```

}

var start\_loop = function\(\) { callrpc\("get\_dynamic\_global\_properties", \[\], function\(err, props\) { var block\_number = props\["last\_irreversible\_block\_num"\]; while \(\(block\_number - last\_block\) &gt; 0\) { last\_block += 1 callrpc\("get\_block", \[last\_block\], function\(err, block\) { process\_block\(block, last\_block\); }\); } }\); }

var client = new rpc.Client\(options\);

var block\_interval;

callrpc\("get\_config", \[\], function \(err, config\) { if \(err\) { console.log\(err\); } else { block\_interval = config\["STEEMIT\_BLOCK\_INTERVAL"\] start\_loop\(\); } } \)

if **name** == '**main**': """ Давайте выясним, как часто генерятся блоки """ config = rpc.get\_config\(\) block\_interval = config\["STEEMIT\_BLOCK\_INTERVAL"\] """ Бесконечное зацикливание """ while True: """ Получите свойства цепочки для идентификации головного/последнего возможного к отмене блока """ props = rpc.get\_dynamic\_global\_properties\(\) """ Получайте номер блока. Здесь на выбор имеется

* head\_block\_number: последний блок
* last\_irreversible\_block\_num: блок, который подтверждён 2/3 всех производителей блоков, и потому безотзывной

  Предлагается использовать последнюю переменную

  """

  ```text
     block_number = props['last_irreversible_block_num']
  ```

  """

Пропустите в цикле все блоки, которые возникли после блока, определенного выше """ while \(block\_number - last\_block\) &gt; 0: last\_block += 1 """ Получите полный блок """ block = rpc.get\_block\(last\_block\) """ Обработайте блок """ process\_block\(block, last\_block\) """ Пропустите один блок """ time.sleep\(block\_interval\) """

#### NodeJS

""" var rpc = require\('node-json-rpc'\);

var last\_block = 160900; var watch\_account = "world"; var options = { port: 8092, host: '127.0.0.1', path: '/rpc', strict: false };

var callrpc = function\(name, params, cb\) { client.call\( {"jsonrpc": "2.0", "method": name, "params": params, "id": 0}, function\(err, res\) { if \(err\) { cb\(err, null\) } else { cb\(null, res\["result"\]\) } } \); }

var process\_transfer = function\(op, block, blockid\) { console.log\(block\); if \(op\["to"\] == watch\_account\) { console.log\(blockid + " \| " + block\["timestamp"\] + " \| "+ op\["from"\] + " -&gt; " + op\["to"\] + ": " + op\["amount"\] + " -- " + op\["memo"\] \); } }

var process\_block = function\(block, blockid\) { console.log\(blockid\); console.log\(block\["transactions"\]\); for \(var tx in block\["transactions"\]\) { for \(var opObj in tx\["operations"\]\) { var opType = opObj\[0\]; var op = opObj\[1\];

```text
         console.log(op);

         if (opType == "transfer") {
             process_transfer(op, block, blockid);
         }
     }
 }
```

}

var start\_loop = function\(\) { callrpc\("get\_dynamic\_global\_properties", \[\], function\(err, props\) { var block\_number = props\["last\_irreversible\_block\_num"\]; while \(\(block\_number - last\_block\) &gt; 0\) { last\_block += 1 callrpc\("get\_block", \[last\_block\], function\(err, block\) { process\_block\(block, last\_block\); }\); } }\); }

var client = new rpc.Client\(options\);

var block\_interval;

callrpc\("get\_config", \[\], function \(err, config\) { if \(err\) { console.log\(err\); } else { block\_interval = config\["STEEMIT\_BLOCK\_INTERVAL"\] start\_loop\(\); } } \) """

