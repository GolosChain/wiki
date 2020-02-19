# Плагины и их API

Плагины представляют собой универсальный инструмент расширения ноды и её возможностей. Часть из них лишь отдают данные, подготавливают индексы, отвечают на сложные запросы пользователей с фильтрацией данных, часть из них обрабатывают custom операции и могут предоставлять совершенно отдельный сервис. 

В данном разделе описаны некоторые доступные плагины GOLOS, предоставляющие пользователям доступ через API. Вы можете сами изучить API того или иного плагина, если будете следовать следующей инструкции:

* Открыть основной заголовочный файл плагина \([пример для database\_api](https://github.com/golos-blockchain/golos/blob/master/plugins/database_api/include/golos/plugins/database_api/plugin.hpp)\), изучить `DEFINE_API_ARGS` \(название API метода, тип возвращаемого значения\);
* Открыть основной файл плагина \([пример для database\_api](https://github.com/golos-blockchain/golos/blob/master/plugins/database_api/api.cpp#L152)\), изучить `DEFINE_API` \(проверка параметров запроса, `CHECK_ARGS_COUNT`, формирование возвращаемого значения определенного типа\);
* Изучить `plugin_initialize`, который может обрабатывать `boost::program_options::variables_map` для более тонкой настройки плагина через конфигурационный файл ноды.

## Протокол запросов

Все запросы должны быть сформированы в JSON и выполнены через RPC. Транспортный протокол зависит от настройки ноды, возможны варианты как JSON-RPC через стандартные HTTP запросы, так и через WebSocket.

Для этого в конфигурационном файле ноды должны быть подключены плагины: json\_rpc, webserver. Чтобы принимать транзакции от пользователей также должен быть включен плагин network\_broadcast\_api. Настройки для портов:

```text
# Количество потоков для клиентов rpc. Оптимальное значение *количество ядер минус 1*
webserver-thread-pool-size = 2

# IP:PORT для HTTP подключений
webserver-http-endpoint = 0.0.0.0:8090

# IP:PORT для WebSocket подключений
webserver-ws-endpoint = 0.0.0.0:8091
```

Чтобы обрабатывать запросы с поддержкой SSL, необходимо пробросить используемые порты через проксирующий сервер \(например, nginx или apache\), тогда станут возможными запросы через https/wss.

## Формирование API запроса

Правила формирования запросов к публичной ноде довольно простые:

```text
{"id":REQUEST_ID,"jsonrpc":"2.0","method":"call","params":["PLUGIN_NAME","PLUGIN_API_METHOD",[ARGS]]}
```

* REQUEST\_ID — номер запроса, носит необязательный характер \(можно все запросы нумеровать единицей\), но при соединении через web sockets \(ws\) позволяет ассоциировать ответы по запросам с аналогичным id;
* PLUGIN\_NAME — название плагина, к которому выполняется запрос \(например: database\_api, committee\_api\);
* PLUGIN\_API\_METHOD — название метода, обрабатывающего запрос \(например: get\_accounts из database\_api\);
* ARGS — массив упорядоченных параметров, передаваемый методу плагина.

## network\_broadcast\_api

Плагин, отвечающий за прием и рассылку между узлами сети подписанных блоков и транзакций

* **broadcast\_block** — передача подписанного блока \(signed\_block\) другим узлам сети;
* **broadcast\_transaction** — передача подписанной транзакции \(signed\_transaction\) другим узлам сети;
* **broadcast\_transaction\_synchronous** — передача подписанной транзакции \(signed\_transaction\) другим узлам сети \(ждет вхождения в блок и возвращает хэш транзакции, номер блока и номер транзакции в блоке, или возвращает false в случае истечения срока действия транзакции\);
* **broadcast\_transaction\_with\_callback** — то же, что и broadcast\_transaction\_synchronous, кроме проверки транзакции на валидность перед передачей в пул транзакций и регистрации метода обратного вызова \(callback\).

## database\_api

* **get\_account\_count** — возвращает количество аккаунтов в сети;
* **get\_accounts** — возвращает массив аккаунтов по запрошенным логинам \(отличается от **lookup\_account\_names** дополнительной информацией\);

Пример:

```javascript
{"id":1,"method":"call","jsonrpc":"2.0","params":["database_api","get_accounts",[["xel"]]]}
```

Ответ:

```javascript
[
  {
    "id": 89772,
    "name": "xel",
    "owner": {
      "weight_threshold": 1,
      "account_auths": [],
      "key_auths": [
        [
          "GLS53nbKGcMwXw8zWMNLBm5XXLTW9fqZeQT5J7dC3Qso7GwJ7F8Hb",
          1
        ]
      ]
    },
    "active": {
      "weight_threshold": 1,
      "account_auths": [],
      "key_auths": [
        [
          "GLS81ApmkpdaJfbCXe7ZkbaiMeVcaQFW361sqC9r414fxVq9jNH3i",
          1
        ]
      ]
    },
    "posting": {
      "weight_threshold": 1,
      "account_auths": [],
      "key_auths": [
        [
          "GLS7WLHYqZrf9RYW273X34Aee7mK9pet8t83xgQ8jxeeZXXG3cTvS",
          1
        ]
      ]
    },
    "memo_key": "GLS8iWbEMQB3WaXHLrCVyDoJAuPQhaiCby3g3PNL4h2hdWq7kiACb",
    "json_metadata": "{"profile":{}}",
    "proxy": "",
    "last_owner_update": "2019-05-10T14:10:30",
    "last_account_update": "2019-05-10T14:10:30",
    "created": "2017-12-06T01:22:18",
    "mined": false,
    "owner_challenged": false,
    "active_challenged": false,
    "last_owner_proved": "1970-01-01T00:00:00",
    "last_active_proved": "1970-01-01T00:00:00",
    "recovery_account": "lex",
    "last_account_recovery": "1970-01-01T00:00:00",
    "reset_account": "null",
    "comment_count": 4,
    "lifetime_vote_count": 0,
    "post_count": 0,
    "can_vote": true,
    "voting_power": 9802,
    "last_vote_time": "2020-01-05T12:10:48",
    "balance": "23.549 GOLOS",
    "savings_balance": "0.000 GOLOS",
    "sbd_balance": "0.000 GBG",
    "sbd_seconds": "32824410702",
    "sbd_seconds_last_update": "2020-02-17T12:02:54",
    "sbd_last_interest_payment": "2018-02-05T16:51:54",
    "savings_sbd_balance": "0.000 GBG",
    "savings_sbd_seconds": "0",
    "savings_sbd_seconds_last_update": "2019-12-11T16:05:15",
    "savings_sbd_last_interest_payment": "2019-12-11T16:05:15",
    "savings_withdraw_requests": 0,
    "vesting_shares": "341688.730149 GESTS",
    "delegated_vesting_shares": "0.000000 GESTS",
    "received_vesting_shares": "0.000000 GESTS",
    "vesting_withdraw_rate": "0.000000 GESTS",
    "next_vesting_withdrawal": "1969-12-31T23:59:59",
    "withdrawn": "185064241674",
    "to_withdraw": "185064241674",
    "withdraw_routes": 0,
    "benefaction_rewards": 0,
    "curation_rewards": 0,
    "delegation_rewards": 0,
    "posting_rewards": 0,
    "proxied_vsf_votes": [
      0,
      0,
      0,
      0
    ],
    "witnesses_voted_for": 0,
    "average_bandwidth": 2721294841,
    "average_market_bandwidth": 1080000000,
    "lifetime_bandwidth": "632909000000",
    "lifetime_market_bandwidth": "6119410000000",
    "last_bandwidth_update": "2020-02-18T10:58:18",
    "last_market_bandwidth_update": "2020-02-07T02:04:09",
    "last_comment": "2020-02-18T10:54:03",
    "last_post": "1970-01-01T00:00:00",
    "post_bandwidth": 0,
    "witness_votes": [],
    "reputation": 0,
    "posts_capacity": 3,
    "comments_capacity": 32112,
    "voting_capacity": 12,
    "referrer_account": "",
    "referrer_interest_rate": 0,
    "referral_end_date": "1970-01-01T00:00:00",
    "referral_break_fee": "0.000 GOLOS",
    "last_active_operation": "2020-02-17T12:02:54"
  }
]
```

* **get\_block** — возвращает информацию о блоке по его номеру;

Пример:

```javascript
{"id":1,"method":"call","jsonrpc":"2.0","params":["database_api","get_block",["34940353"]]}
```

Ответ:

```javascript
{
  "previous": "021525c0c311e480cde023eea5c2ba26e0d06c82",
  "timestamp": "2020-02-19T12:09:15",
  "witness": "pzm",
  "transaction_merkle_root": "0000000000000000000000000000000000000000",
  "extensions": [],
  "witness_signature": "1f17e84a65e103c9cc476d27a29dbba4e5100ab60c79015418a5eeab0cf867b1ee7b4407b408b6d2d28fed9f08820defadb7f4ba715f4c520bb49c3fa5e5e8412a",
  "transactions": []
}
```

* **get\_chain\_properties** — возвращает медианные значения голосуемых параметров сети;

Пример:

```javascript
{"id":1,"method":"call","jsonrpc":"2.0","params":["database_api","get_chain_properties",[]]}
```

Ответ:

```javascript
{
  "account_creation_fee": "1.000 GOLOS",
  "maximum_block_size": 65536,
  "sbd_interest_rate": 0,
  "create_account_min_golos_fee": "0.030 GOLOS",
  "create_account_min_delegation": "0.150 GOLOS",
  "create_account_delegation_time": 2592000,
  "min_delegation": "0.010 GOLOS",
  "max_referral_interest_rate": 1000,
  "max_referral_term_sec": 15552000,
  "min_referral_break_fee": "0.001 GOLOS",
  "max_referral_break_fee": "100.000 GOLOS",
  "posts_window": 7200,
  "posts_per_window": 2,
  "comments_window": 32767,
  "comments_per_window": 50,
  "votes_window": 15,
  "votes_per_window": 5,
  "auction_window_size": 0,
  "max_delegated_vesting_interest_rate": 8000,
  "custom_ops_bandwidth_multiplier": 10,
  "min_curation_percent": 7500,
  "max_curation_percent": 7500,
  "curation_reward_curve": "square_root",
  "allow_distribute_auction_reward": true,
  "allow_return_auction_reward_to_fund": true,
  "worker_reward_percent": 1000,
  "witness_reward_percent": 1500,
  "vesting_reward_percent": 7500,
  "worker_request_creation_fee": "100.000 GBG",
  "worker_request_approve_min_percent": 1500,
  "sbd_debt_convert_rate": 100,
  "vote_regeneration_per_day": 10,
  "witness_skipping_reset_time": 21600,
  "witness_idleness_time": 2592000,
  "account_idleness_time": 15552000
}
```

* **get\_dynamic\_global\_properties** — возвращает данные о динамических глобальных свойствах сети;

Пример:

```javascript
{"id":1,"method":"call","jsonrpc":"2.0","params":["database_api","get_dynamic_global_properties",[]]}
```

Ответ:

```javascript
{
  "id": 0,
  "head_block_number": 34940424,
  "head_block_id": "021526082557e35d8f65271a46ddfec351eeb2de",
  "time": "2020-02-19T12:12:48",
  "current_witness": "blockchained",
  "total_pow": 1719078,
  "num_pow_witnesses": 172,
  "virtual_supply": "223046315.397 GOLOS",
  "current_supply": "200741691.274 GOLOS",
  "confidential_supply": "0.000 GOLOS",
  "current_sbd_supply": "7129967.558 GBG",
  "confidential_sbd_supply": "0.000 GBG",
  "total_vesting_fund_steem": "80100561.317 GOLOS",
  "total_vesting_shares": "251334390154.901765 GESTS",
  "total_reward_fund_steem": "254.756 GOLOS",
  "total_reward_shares2": "21159194891210756",
  "sbd_interest_rate": 0,
  "sbd_print_rate": 0,
  "sbd_debt_percent": 4046,
  "average_block_size": 318,
  "maximum_block_size": 65536,
  "current_aslot": 35108656,
  "recent_slots_filled": "340282366920938463463374607431768211455",
  "participation_count": 128,
  "last_irreversible_block_num": 34940409,
  "max_virtual_bandwidth": "5986734968066277376",
  "current_reserve_ratio": 20000,
  "custom_ops_bandwidth_multiplier": 10,
  "is_forced_min_price": true,
  "transit_block_num": 29585430,
  "transit_witnesses": "76766b0000000000000000000000000078746172000000000000000000000000646d696c617368000000000000000000726f706f7800000000000000000000006c657800000000000000000000000000676f6c6f73636f72650000000000000076696b00000000000000000000000000737465657073686f740000000000000073746968692d696f0000000000000000676f6c6f73696f0000000000000000006c6164797a6172756c656d0000000000797564696e612d63617400000000000063726561746f720000000000000000006b756e6100000000000000000000000064656e69732d736b7269706e696b00007072696d7573000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  "worker_requests": [
    [
      "91600047785731",
      0
    ]
  ]
}
```

* **get\_escrow** — возвращает информацию о трехсторонней сделке по логину аккаунта и идентификатору сделки \(id\);
* **get\_expiring\_vesting\_delegations** — возвращает список возвращаемой делегированной доли \(по времени истечения возврата\);
* **get\_hardfork\_version** — возвращает версию текущего хардфорка;
* **get\_next\_scheduled\_hardfork** — возвращает следующий запланированный хардфорк;
* **get\_potential\_signatures** — возвращает потенциальные публичные ключи для подписи транзакции;
* **get\_recovery\_request** — возвращает данные по запросу на восстановление доступа к аккаунту;
* **get\_required\_signatures** — возвращает публичные ключи из предложенных, которые необходимы для подписи транзакции \(нужно направить транзакцию без подписи и предоставить открытые ключи\);
* **get\_transaction\_hex** — возвращает hex значение сырой транзакции;
* **get\_vesting\_delegations** — возвращает список делегированной доли аккаунта;
* **get\_withdraw\_routes** — возвращает массив путей понижения доли для аккаунта;
* **lookup\_account\_names** — возвращает массив аккаунтов по запрошенным логинам;
* **lookup\_accounts** — возвращает массив логинов аккаунтов по нижней границе с указанием количества возвращаемых элементов \(не более 1000\);
* **verify\_account\_authority** — проверяет подпись транзакции отдельным аккаунтом с указанием публичного ключа;
* **verify\_authority** — принимает подписанную транзакцию и возвращает TRUE, если транзакция подписана всеми необходимыми ключами;

## account\_by\_key

* **get\_key\_references** — возвращает массив логинов аккаунтов, которые содержат указанный публичный ключ.

Пример запроса:

```javascript
{"id":1,"method":"call","jsonrpc":"2.0","params":["account_by_key","get_key_references",[["GLS53nbKGcMwXw8zWMNLBm5XXLTW9fqZeQT5J7dC3Qso7GwJ7F8Hb"]]]}
```

Ответ:

```text
[
  [
    "xel"
  ]
]
```

## account\_history

* **get\_account\_history** — возвращает историю операций \(в том числе и виртуальных\), связанных с определенным аккаунтом. 

Пример запроса:

```text
{"id":1,"method":"call","jsonrpc":"2.0","params":["account_history","get_account_history",["xel","-1","1"]]}
```

Ответ:

```javascript
[
  [
    84,
    {
      "trx_id": "de0c1837f59023dd183e1f6f0d338f60432af2ab",
      "block": 34910086,
      "trx_in_block": 1,
      "op_in_trx": 1,
      "virtual_op": 0,
      "timestamp": "2020-02-18T10:54:03",
      "op": [
        "comment_options",
        {
          "author": "xel",
          "permlink": "re-lex-re-on0tole-re-lex-re-on0tole-re-lex-voznagrazhdeniya-za-shering-po-socsetyam-i-otkaz-ot-flagov-20200218t105406785z",
          "max_accepted_payout": "1000000.000 GBG",
          "percent_steem_dollars": 10000,
          "allow_votes": true,
          "allow_curation_rewards": true,
          "extensions": []
        }
      ]
    }
  ],
  [
    85,
    {
      "trx_id": "b35973bbf446a37d3d64492388231f7d5531f93b",
      "block": 34910171,
      "trx_in_block": 0,
      "op_in_trx": 0,
      "virtual_op": 0,
      "timestamp": "2020-02-18T10:58:18",
      "op": [
        "delete_comment",
        {
          "author": "xel",
          "permlink": "re-lex-re-on0tole-re-lex-re-on0tole-re-lex-voznagrazhdeniya-za-shering-po-socsetyam-i-otkaz-ot-flagov-20200218t105406785z"
        }
      ]
    }
  ]
]
```

## operation\_history

* **get\_ops\_in\_block** — возвращает массив операций по номеру блока \(можно указать необходимость показывать только виртуальные операции\);

Пример:

```javascript
{"id":1,"method":"call","jsonrpc":"2.0","params":["operation_history","get_ops_in_block",["31913871","false"]]}
```

Ответ:

```javascript
[
  {
    "trx_id": "0000000000000000000000000000000000000000",
    "block": 31913871,
    "trx_in_block": 65535,
    "op_in_trx": 0,
    "virtual_op": 1,
    "timestamp": "2019-11-05T23:23:18",
    "op": [
      "producer_reward",
      {
        "producer": "xanoxt",
        "vesting_shares": "488.592569 GESTS"
      }
    ]
  }
]
```

* **get\_transaction** — возвращает данные транзакции по её хэшу \(id\);

## witness\_api

* **get\_active\_witnesses** — возвращает массив делегатов в текущем раунде \(из 21 слота, при наличии такого количества делегатов\);

Пример:

```javascript
{"id":1,"method":"call","jsonrpc":"2.0","params":["witness_api","get_active_witnesses",[]]}
```

Ответ:

```javascript
[
  "blockchained",
  "litrbooh",
  "lex",
  "prizm.space",
  "aleksw",
  "pzm",
  "anyx",
  "miner-113",
  "ladyzarulem",
  "rise",
  "xanoxt",
  "on0tole",
  "primus",
  "lulz",
  "actual",
  "vvk",
  "lindsay",
  "erikkartmen",
  "arcange",
  "jackvote",
  "solox"
]
```

* **get\_witness\_by\_account** — возвращает делегата в соответствием с его логином;
* **get\_witness\_count** — возвращает количество аккаунтов, заявивших о желании быть делегатом;
* **get\_witness\_schedule** — возвращает очередь делегатов дополненную расчитанными медианными значениями параметров сети и мажориторной версией хардфорка;
* **get\_witnesses** — возвращает массив делегатов в соответствием с их id \(можно запросить нескольких в массиве\);
* **get\_witnesses\_by\_vote** — возвращает массив делегатов, отсортированных по потенциалу полученных голосов \(указывается левая граница списка и количество возвращаемых значений в массиве, но не более 100\);
* **lookup\_witness\_accounts** — возвращает массив логинов аккаунтов, которые заявляли себя как делегаты \(указывается левая граница списка и количество возвращаемых значений в массиве, но не более 1000\).

