# Роли нод

В зависимости от конфигурации, golosd может выполнять несколько функций. Эти функции можно выделить в отдельные роли, которые описаны ниже.

## seed node

Seed-нода обеспечивает хранение блокчейна и его "сидирование", т.е. отдачу всей истории блоков клиенту, а также приём и распространение транзакций в сети. seed-нода должна иметь возможность приёма соединений, т.е. быть доступной по белому IP-адресу.

Для минимизации потребления памяти, seed-нода может быть собрана с опцией `-DLOW_MEMORY_NODE=TRUE`

Опции конфигурационного файла, относящиеся к seed-ноде:

```text
# Endpoint for P2P node to listen on
# p2p-endpoint = 0.0.0.0:2001

# Maxmimum number of incoming connections on P2P endpoint
# p2p-max-connections =
```

## API node

Основной задачей API-ноды является обслуживание клиентских приложений через API. Для включения API используются следующие опции конфигурационного файла:

```text
public-api = database_api login_api market_history_api tags_api follow_api
enable-plugin = account_history follow market_history private_message tags
```

## witness node

Нода, обеспечивающая работу делегата сети Голос. Характеризуется включенным плагином witness, указанным именем делегата и его приватного ключа:

```text
enable-plugin = witness
witness = "foo"
private-key = WIF PRIVATE KEY
```

Директива `witness` может быть указана несколько раз.

## miner node

Нода, обеспечивающая работу pow-майнера и одноимённого делегата.

```text
enable-plugin = witness
witness = "foo"
miner = ["foo", "WIF PRIVATE KEY"]
```

Директива `miner` может быть указана несколько раз.

