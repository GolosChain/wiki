# Запуск тестнета

#### Как запустить тестовую сеть
> Пошаговая инструкция для запуска частной тестовой сети. Эта инструкция предполагает, что вы уже начали работать с ПО. 

### Компиляция для запуска тестовой сети 
Тестовая сеть настраивается несколько иначе, чем реальная сеть. Вам придется настроить cmake для создания источника для тестовой сети:

cmake -DBUILD_STEEM_TESTNET=ON .
-- BUILD_STEEM_TESTNET: ON
--
--              Конфигурация для тестовой сети
-- Finished fc module configuration...
--
             Конфигурация для тестовой сети 

-- Configuring done
-- Generating done
-- Build files have been written to: ...

make


### Конфигурация Steem Daemon
Вначале запустите процесс steemd для генерации файла config.ini и распечатайте приватный ключ, сгенеренный по умолчанию.

cd programs/steemd
./steemd -d testnet 
------------------------------------------------------

            Запуск тестовой сети

------------------------------------------------------
initminer public key: TEST6LLegbAgLAy28EHrffBVuANFWcFgmqRMW13wBmTExqFE9SCkg4
initminer private key: 5JNHfZYKGaomSFvd4NUdQ9qMcEAC43kujbfjueTHpVapX1Kzq2n
chain id: 18dcf0a285365fc58b71f18b3d3fec954aa0c141c44e4e5cb4cf777b9eab274e
------------------------------------------------------
Отредактируйте файл testnet/config.ini и настройте эти ноды свидетелей на производство блоков, используя приватный ключ, указанный выше.

# имя ноды свидетеля, управляемой данным узлом (например, initminer )
  witness = "initminer"

# WIF приватный ключ (можете указать несколько раз)
  private-key = 5JNHfZYKGaomSFvd4NUdQ9qMcEAC43kujbfjueTHpVapX1Kzq2n

# Конечная точка для прослушивания для P2P ноды 
  p2p-endpoint = 0.0.0.0:3333

# Конечная точка для прослушивания для RPC 
  rpc-endpoint = 127.0.0.1:9876


Запуск ноды свидетеля 
После конфигурации можете запустить ноду свидетеля следующим образом:

 ./steemd -d testnet --enable-stale-production 


В этом время вы должны увидеть на экране следующее:

  ....
  2745098ms th_a  witness.cpp:212 block_production_loo ] Generated block #1 with timestamp 2016-03-28T22:45:45 at time 2016-03-28T22:45:45 by initminer
  2747995ms th_a  witness.cpp:212 block_production_loo ] Generated block #2 with timestamp 2016-03-28T22:45:48 at time 2016-03-28T22:45:48 by initminer
  2750994ms th_a  witness.cpp:212 block_production_loo ] Generated block #3 with timestamp 2016-03-28T22:45:51 at time 2016-03-28T22:45:51 by initminer
  ....
Запуск CLI кошелька
Первым шагом следует создать новый кошелек. Обратите внимание, что нода свидетеля должна быть запущена на том же компьютере.

cd programs/cli_wallet 
./cli_wallet --server-rpc-endpoint="ws://127.0.0.1:9876"
new >>> set_password "testpassword"
locked >>> unlock "testpassword"
unlocked >>> list_my_accounts 
[]
unlocked >>> 


До запуска команд требуется импортировать приватный ключ, который контролирует учетную запись initminer (ключ, который был распечатан вначале).

unlocked >>> import_key 5JNHfZYKGaomSFvd4NUdQ9qMcEAC43kujbfjueTHpVapX1Kzq2n
true

unlocked >>> list_keys
[[
    "TST6LLegbAgLAy28EHrffBVuANFWcFgmqRMW13wBmTExqFE9SCkg4",
    "5JNHfZYKGaomSFvd4NUdQ9qMcEAC43kujbfjueTHpVapX1Kzq2n"
  ]
]

unlocked >>> list_my_accounts
initminer                17.000 TESTS       1.000000 VESTS            0.000 TBD
-------------------------------------------------------------------------
TOTAL                    17.000 TESTS       1.000000 VESTS            0.000 TBD


Сейчас требуется проделать то же самое для всех учетных записей. После этого можно создавать учетные записи для тестирования 

unlocked >>> create_account "initminer" "scott" "" true
{
  "ref_block_num": 3673,
  "ref_block_prefix": 74705714,
  "expiration": "2016-02-16T21:54:04",
  "operations": [[
      "account_create",{
        "fee": "0.000 TESTS",
        "creator": "initminer",
        "new_account_name": "scott",
        "owner": {
          "weight_threshold": 1,
          "account_auths": [],
          "key_auths": [[
              "STM5Tqg19fydUJpheNLtsanWQi8AYdcK8FiWr8W1PdeUfmFQSSjAo",
              1
            ]
          ]
        },
        "active": {
          "weight_threshold": 1,
          "account_auths": [],
          "key_auths": [[
              "STM7Rc77FhVoEU2TDcJiRv9fDXgZUon8vAtPPsAXyxaHTWw7wLtWD",
              1
            ]
          ]
        },
        "memo_key": "STM5S9zfoMvfgenkGzwKLrKdeTrJRQnTJLAX9N4iXFVurEEVDKkkT",
        "json_metadata": ""
      }
    ]
  ],
  "extensions": [],
  "signatures": [
    "1f552f946600c4244231da804ba24be4c3e0fefdfc5ce61faa5528af775b32fa5f676a978323aef679b13f5c414d5bbed08fc9bfa5ae11ec5c1f8fe490a0a20829"
  ],
  "transaction_id": "b9aedb9a0d3f66f4588836bf307246c3b3e53a3f",
  "block_num": 3674,
  "transaction_num": 0
}

unlocked >>> list_my_accounts

initminer                49.000 TESTS       1.000000 VESTS            0.000 TBD
scott                     0.000 TESTS       0.000000 VESTS            0.000 TBD
-------------------------------------------------------------------------
TOTAL                    49.000 TESTS       1.000000 VESTS            0.000 TBD


Для проведения транзакций требуется отложенные TEST:

unlocked >>> transfer_to_vesting "initminer" "scott" "1.000 TESTS" true
{
  "ref_block_num": 367,
  "ref_block_prefix": 697512891,
  "expiration": "2016-03-29T17:32:30",
  "operations": [[
      "transfer_to_vesting",{
        "from": "initminer",
        "to": "scott",
        "amount": "1.000 TESTS"
      }
    ]
  ],
  "extensions": [],
  "signatures": [
    "1f5950f65c93f369beee1637e5608eac50e7e2753311c69a3085f1acf4c2bebc215bffa6b6bd4c2d6455a540ee371c582f4d5ae707ec24354990e075051003756c"
  ],
  "transaction_id": "c29efe38e4a67d40e877a85c0caf6acdac4ce758"
  "block_num": 368,
  "transaction_num": 0
}

unlocked >>> list_my_accounts
initminer               264.000 TESTS       1.000000 VESTS            0.000 TBD
scott                     0.000 TESTS       1.000000 VESTS            0.000 TBD
-------------------------------------------------------------------------
TOTAL                   264.000 TESTS       2.000000 VESTS            0.000 TBD


Для получения полного списка команд используйте команду «помощи»

Все операции кошелька будут заблокированы до тех пор, пока операция не будет включена в блокчейн или не появится сообщение об ошибке. Это займет в среднем около 1,5 секунды, но не более 6 секунд, в случае нормальной работы сети.

В результате операции кошелька содержат номер блока транзакции, подтвержденной блокчейн-системой.






