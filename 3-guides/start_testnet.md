# Запуск тестнета

#### Как запустить тестовую сеть

> Пошаговая инструкция для запуска частной тестовой сети. Эта инструкция предполагает, что вы уже начали работать с ПО.
>
> См. также статью [Запуск testnet Golos](https://wiki.golos.io/3-guides/stati/zapusk-testnet-golos.html)

### Компиляция для запуска тестовой сети

Тестовая сеть настраивается несколько иначе, чем реальная сеть. Вам придется настроить cmake для создания источника для тестовой сети:

cmake -DBUILD\_STEEM\_TESTNET=ON .

## -- BUILD\_STEEM\_TESTNET: ON

--              Конфигурация для тестовой сети

## -- Finished fc module configuration...

```
         Конфигурация для тестовой сети 
```

-- Configuring done  
-- Generating done  
-- Build files have been written to: ...

make

### Конфигурация Steem Daemon

Сначала запустите процесс steemd для генерации файла config.ini и распечатайте приватный ключ, сгенеренный по умолчанию.

cd programs/steemd

## ./steemd -d testnet

```
        Запуск тестовой сети
```

---

initminer public key: TEST6LLegbAgLAy28EHrffBVuANFWcFgmqRMW13wBmTExqFE9SCkg4  
initminer private key: 5JNHfZYKGaomSFvd4NUdQ9qMcEAC43kujbfjueTHpVapX1Kzq2n

## chain id: 18dcf0a285365fc58b71f18b3d3fec954aa0c141c44e4e5cb4cf777b9eab274e

Отредактируйте файл testnet/config.ini и настройте эти ноды свидетелей на производство блоков, используя приватный ключ, указанный выше.

# имя ноды свидетеля, управляемой данным узлом \(например, initminer \)

witness = "initminer"

# WIF приватный ключ \(можете указать несколько раз\)

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
  2745098ms th\_a  witness.cpp:212 block\_production\_loo \] Generated block \#1 with timestamp 2016-03-28T22:45:45 at time 2016-03-28T22:45:45 by initminer  
  2747995ms th\_a  witness.cpp:212 block\_production\_loo \] Generated block \#2 with timestamp 2016-03-28T22:45:48 at time 2016-03-28T22:45:48 by initminer  
  2750994ms th\_a  witness.cpp:212 block\_production\_loo \] Generated block \#3 with timestamp 2016-03-28T22:45:51 at time 2016-03-28T22:45:51 by initminer  
  ....  
Запуск CLI кошелька  
Первым шагом следует создать новый кошелек. Обратите внимание, что нода свидетеля должна быть запущена на том же компьютере.

cd programs/cli\_wallet   
./cli\_wallet --server-rpc-endpoint="ws://127.0.0.1:9876"  
new &gt;&gt;&gt; set\_password "testpassword"  
locked &gt;&gt;&gt; unlock "testpassword"  
unlocked &gt;&gt;&gt; list\_my\_accounts   
\[\]  
unlocked &gt;&gt;&gt;

До запуска команд требуется импортировать приватный ключ, который контролирует учетную запись initminer \(ключ, который был распечатан в начале\).

unlocked &gt;&gt;&gt; import\_key 5JNHfZYKGaomSFvd4NUdQ9qMcEAC43kujbfjueTHpVapX1Kzq2n  
true

unlocked &gt;&gt;&gt; list\_keys  
\[\[  
    "TST6LLegbAgLAy28EHrffBVuANFWcFgmqRMW13wBmTExqFE9SCkg4",  
    "5JNHfZYKGaomSFvd4NUdQ9qMcEAC43kujbfjueTHpVapX1Kzq2n"  
  \]  
\]

unlocked &gt;&gt;&gt; list\_my\_accounts

## initminer                17.000 TESTS       1.000000 VESTS            0.000 TBD

TOTAL                    17.000 TESTS       1.000000 VESTS            0.000 TBD

Сейчас требуется проделать то же самое для всех учетных записей. После этого можно создавать учетные записи для тестирования

unlocked &gt;&gt;&gt; create\_account "initminer" "scott" "" true  
{  
  "ref\_block\_num": 3673,  
  "ref\_block\_prefix": 74705714,  
  "expiration": "2016-02-16T21:54:04",  
  "operations": \[\[  
      "account\_create",{  
        "fee": "0.000 TESTS",  
        "creator": "initminer",  
        "new\_account\_name": "scott",  
        "owner": {  
          "weight\_threshold": 1,  
          "account\_auths": \[\],  
          "key\_auths": \[\[  
              "STM5Tqg19fydUJpheNLtsanWQi8AYdcK8FiWr8W1PdeUfmFQSSjAo",  
              1  
            \]  
          \]  
        },  
        "active": {  
          "weight\_threshold": 1,  
          "account\_auths": \[\],  
          "key\_auths": \[\[  
              "STM7Rc77FhVoEU2TDcJiRv9fDXgZUon8vAtPPsAXyxaHTWw7wLtWD",  
              1  
            \]  
          \]  
        },  
        "memo\_key": "STM5S9zfoMvfgenkGzwKLrKdeTrJRQnTJLAX9N4iXFVurEEVDKkkT",  
        "json\_metadata": ""  
      }  
    \]  
  \],  
  "extensions": \[\],  
  "signatures": \[  
    "1f552f946600c4244231da804ba24be4c3e0fefdfc5ce61faa5528af775b32fa5f676a978323aef679b13f5c414d5bbed08fc9bfa5ae11ec5c1f8fe490a0a20829"  
  \],  
  "transaction\_id": "b9aedb9a0d3f66f4588836bf307246c3b3e53a3f",  
  "block\_num": 3674,  
  "transaction\_num": 0  
}

unlocked &gt;&gt;&gt; list\_my\_accounts

initminer                49.000 TESTS       1.000000 VESTS            0.000 TBD

## scott                     0.000 TESTS       0.000000 VESTS            0.000 TBD

TOTAL                    49.000 TESTS       1.000000 VESTS            0.000 TBD

Для проведения транзакций требуется отложенные TEST:

unlocked &gt;&gt;&gt; transfer\_to\_vesting "initminer" "scott" "1.000 TESTS" true  
{  
  "ref\_block\_num": 367,  
  "ref\_block\_prefix": 697512891,  
  "expiration": "2016-03-29T17:32:30",  
  "operations": \[\[  
      "transfer\_to\_vesting",{  
        "from": "initminer",  
        "to": "scott",  
        "amount": "1.000 TESTS"  
      }  
    \]  
  \],  
  "extensions": \[\],  
  "signatures": \[  
    "1f5950f65c93f369beee1637e5608eac50e7e2753311c69a3085f1acf4c2bebc215bffa6b6bd4c2d6455a540ee371c582f4d5ae707ec24354990e075051003756c"  
  \],  
  "transaction\_id": "c29efe38e4a67d40e877a85c0caf6acdac4ce758"  
  "block\_num": 368,  
  "transaction\_num": 0  
}

unlocked &gt;&gt;&gt; list\_my\_accounts  
initminer               264.000 TESTS       1.000000 VESTS            0.000 TBD

## scott                     0.000 TESTS       1.000000 VESTS            0.000 TBD

TOTAL                   264.000 TESTS       2.000000 VESTS            0.000 TBD

Для получения полного списка команд используйте команду «помощи»

Все операции кошелька будут заблокированы до тех пор, пока операция не будет включена в блокчейн или не появится сообщение об ошибке. Это займет в среднем около 1,5 секунды, но не более 6 секунд, в случае нормальной работы сети.

В результате операции кошелька содержат номер блока транзакции, подтвержденной блокчейн-системой.

