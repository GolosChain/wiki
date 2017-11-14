# Настройка нод
<!-- toc -->

#### Настройка нод майнера и делегата для работы в Голосе: рабочие конфиги и пояснения по установке

> Материал взят отсюда:
https://golos.io/ru--golos/@primus/nastroika-nod-mainera-i-delegata-dlya-raboty-v-golose-rabochie-konfigi-i-poyasneniya-po-ustanovke

Устанавливаем необходимые библиотеки и зависимости:

```
sudo apt-get update
sudo apt-get install git cmake g++ python-dev autotools-dev libicu-dev build-essential libbz2-dev libboost-all-dev libssl-dev libncurses5-dev doxygen libreadline-dev dh-autoreconf screen
sudo
```

Скачиваем исходный код Голоса из официального репозитория:

```

git clone https://github.com/GolosChain/golos.git golos-14.2
cd golos-14.2
git submodule update --init --recursive
```

Генерируем файлы для управления сборкой:

```

cmake -DCMAKE_BUILD_TYPE=Release .
```


И завершаем компиляцию (придётся подождать некоторое время):
```


make```


Для запуска нод я использую отдельную директорию, в которой лежат конфигурационные файлы и сам блокчейн Голоса. Пусть это будет директория по имени golos, давайте создадим её:
```


cd
mkdir golos
cd golos```


Запуск ноды прозводится командой:
```


/path/to/golos-14.2/programs/golosd/golosd
где /path/to/ - это путь в системе до папки golos-14.2```

 созданной на этапе скачивания исходного кода Голоса из официального репозитория.

Прежде чем запускать Голос, скопируйте в эту папку файл со снэпшотом сети:
```


cp /path/to/golos-14.2/programs/golosd/golosd/snapshot5392323.json .```


И теперь можно запускать golosd:
```


/path/to/golos-14.2/programs/golosd/golosd```


При первом запуске golosd создаст директорию witness_node_data_dir для хранения блокчейна. Прервите работу golosd и создайте (в любом текстовом редакторе, я использую nano) конфигурационный файл в этой директории:
```


nano witness_node_data_dir/config.ini```

Конфигурационный файл для ноды майнера:

```


# Endpoint for P2P node to listen on
# p2p-endpoint = 

# Maxmimum number of incoming connections on P2P endpoint
# p2p-max-connections = 

# P2P nodes to connect to on startup (may specify multiple times)
# seed-node = 

seed-node = 5.9.18.213:4243
seed-node = 52.32.75.69:4243
seed-node = 52.57.156.202:4243
seed-node = 88.99.13.48:4243
seed-node = golos-seed.arcange.eu:4243
seed-node = golos-seed.esteem.ws:4243
seed-node = golosnode.com:4243
seed-node = 138.68.101.115:4243
seed-node = golos.imcoins.org:2001
seed-node = 178.62.224.148:4242

# Pairs of [BLOCK_NUM,BLOCK_ID] that should be enforced as checkpoints.
# checkpoint = 

# Endpoint for websocket RPC to listen on
# rpc-endpoint = 

rpc-endpoint = 127.0.0.1:9090

# Endpoint for TLS websocket RPC to listen on
# rpc-tls-endpoint = 

# The TLS certificate file for this server
# server-pem = 

# Password for this certificate
# server-pem-password = 

# Block signing key to use for init witnesses, overrides genesis file
# dbg-init-key = 

# API user specification, may be specified multiple times
# api-user = 

# Set an API to be publicly available, may be specified multiple times
public-api = database_api login_api

# Plugin(s) to enable, may be specified multiple times
enable-plugin = witness account_history

# Defines a range of accounts to track as a json pair ["from","to"] [from,to)
# track-account-range = 

# RPC endpoint of a trusted validating node (required)
# trusted-node = 

# Track market history by grouping orders into buckets of equal size measured in seconds specified as a JSON array of numbers
bucket-size = [15,60,300,3600,86400]

# How far back in time to track history for each bucket size, measured in the number of buckets (default: 5760)
history-per-size = 5760

# Defines a range of accounts to private messages to/from as a json pair ["from","to"] [from,to)

# Enable block production, even if the chain is stale.
enable-stale-production = false

# Percent of witnesses (0-99) that must be participating in order to produce blocks
required-participation = false

# name of witness controlled by this node (e.g. initwitness )
# witness = 

witness = "alcotester"

# name of miner and its private key (e.g. ["account","WIF PRIVATE KEY"] )
# miner = 

miner = ["alcotester","5JGyvx65iLqRgX77Yzr2kDYqEoEwG3PhnxDJ2Nb97SbVb5CbGKs"]

# Number of threads to use for proof of work mining
# mining-threads = 

mining-threads = 2

# WIF PRIVATE KEY to be used by one or more witnesses or miners
# private-key = 

# Account creation fee to be voted on upon successful POW - Minimum fee is 100.000 STEEM (written as 100000)

miner-account-creation-fee = 3000

# Maximum block size (in bytes) to be voted on upon successful POW - Max block size must be between 128 KB and 750 MB
# miner-maximum-block-size = 

# SBD interest rate to be vote on upon successful POW - Default interest rate is 10% (written as 1000)
# miner-sbd-interest-rate = 

# declare an appender named "stderr" that writes messages to the console
[log.console_appender.stderr]
stream=std_error

# declare an appender named "p2p" that writes messages to p2p.log
[log.file_appender.p2p]
filename=logs/p2p/p2p.log
# filename can be absolute or relative to this config file

# route any messages logged to the default logger to the "stderr" logger we
# declared above, if they are info level are higher
[logger.default]
level=warn
appenders=stderr

# route messages sent to the "p2p" logger to the p2p appender declared above
[logger.p2p]
level=warn
appenders=p2p
mining-threads = 2 - количество ядер вашего CPU
```

Note: Актуальный список seed node можете найти тут:
https://github.com/GolosChain/golos/blob/master/documentation/seednodes


Note: WIF PRIVATE KEY можно найти в вашем аккаунте на golos.io, щелкните по иконке своего пользователя в правом верхнем углу, выбирете "КОШЕЛЕК", затем щелкните на вкладку "РАЗРЕШЕНИЯ", вам нужен приватный ключ "ОБСУЖДАЕМОЕ" (нажмите "ПОКАЗАТЬ ПРИВАТНЫЙ КЛЮЧ") -- это и будет WIF PRIVATE KEY.


Конфигурационный файл для ноды делегата:
```



# Endpoint for P2P node to listen on
# p2p-endpoint = 

# Maxmimum number of incoming connections on P2P endpoint
# p2p-max-connections = 

# P2P nodes to connect to on startup (may specify multiple times)
# seed-node = 


seed-node = 5.9.18.213:4243
seed-node = 52.32.75.69:4243
seed-node = 52.57.156.202:4243
seed-node = 88.99.13.48:4243
seed-node = golos-seed.arcange.eu:4243
seed-node = golos-seed.esteem.ws:4243
seed-node = golosnode.com:4243
seed-node = 138.68.101.115:4243
seed-node = golos.imcoins.org:2001
seed-node = 178.62.224.148:4242

# Pairs of [BLOCK_NUM,BLOCK_ID] that should be enforced as checkpoints.
# checkpoint = 

# Endpoint for websocket RPC to listen on
rpc-endpoint =  127.0.0.1:9090

# Endpoint for TLS websocket RPC to listen on
# rpc-tls-endpoint = 

# The TLS certificate file for this server
# server-pem = 

# Password for this certificate
# server-pem-password = 

# Block signing key to use for init witnesses, overrides genesis file
# dbg-init-key = 

# API user specification, may be specified multiple times
# api-user = 

# Set an API to be publicly available, may be specified multiple times
# public-api = database_api login_api

# Plugin(s) to enable, may be specified multiple times

enable-plugin = witness
enable-plugin = account_history
enable-plugin = follow
enable-plugin = market_history
enable-plugin = private_message
enable-plugin = tags
public-api = database_api login_api market_history_api tags_api follow_api

# Defines a range of accounts to track as a json pair ["from","to"] [from,to)
# track-account-range = 

# RPC endpoint of a trusted validating node (required)
# trusted-node = 

# Track market history by grouping orders into buckets of equal size measured in seconds specified as a JSON array of numbers
bucket-size = [15,60,300,3600,86400]

# How far back in time to track history for each bucket size, measured in the number of buckets (default: 1000)
history-per-size = 1000

# Defines a range of accounts to private messages to/from as a json pair ["from","to"] [from,to)
# pm-account-range = 

# Enable block production, even if the chain is stale.
enable-stale-production = false

# Percent of witnesses (0-99) that must be participating in order to produce blocks
required-participation = false

# name of witness controlled by this node (e.g. initwitness )

witness = "primus"

# name of miner and its private key (e.g. ["account","WIF PRIVATE KEY"] )
# miner = 

# Number of threads to use for proof of work mining
# mining-threads = 

# WIF PRIVATE KEY to be used by one or more witnesses or miners

private-key = 5KeVwZewUmGsP8UzowRJdUNh1dCq7McweYJrPgokVsJiyRTCm6W

# Account creation fee to be voted on upon successful POW - Minimum fee is 100.000 STEEM (written as 100000)

miner-account-creation-fee = 3000

# Maximum block size (in bytes) to be voted on upon successful POW - Max block size must be between 128 KB and 750 MB
# miner-maximum-block-size = 

# SBD interest rate to be vote on upon successful POW - Default interest rate is 10% (written as 1000)
# miner-sbd-interest-rate = 

# declare an appender named "stderr" that writes messages to the console
[log.console_appender.stderr]
stream=std_error

# declare an appender named "p2p" that writes messages to p2p.log
[log.file_appender.p2p]
filename=logs/p2p/p2p.log
# filename can be absolute or relative to this config file

# route any messages logged to the default logger to the "stderr" logger we
# declared above, if they are info level are higher
[logger.default]
level=warn
appenders=stderr

# route messages sent to the "p2p" logger to the p2p appender declared above
[logger.p2p]
level=warn
appenders=p2p```


