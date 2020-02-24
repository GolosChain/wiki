# Гайд для witness/seed ноды

## Устанавливаем Docker

```text
sudo apt-get update
```

```text
sudo apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```

```text
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

```text
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

```text
sudo apt-get update
```

```text
sudo apt-get install docker-ce -y
```

## Устанавливаем ноду

Скачиваем большую часть блоков напрямую с сервера \(чтобы не тратить сутки-двое на их получение и лишнюю нагрузку делегатских seed-нод\).

```text
sudo wget -P ~/home/blockchain https://files.rudex.org/golos-classic/blockchain/block_log
```

Создаем папку и добавляем файл c актуальными seed-нодами.

```text
mkdir ~/config && echo 'seed1.golos.blckchnd.com:30218
seed.golos.lexa.host:4243
95.216.200.20:3001
seed1.vvk.pp.ru:2001
seed.aleksw.space:4243
116.202.162.207:2001
94.130.24.42:2001
78.47.150.171:4243
95.217.7.47:4243
95.217.133.230:4243' | sudo tee -a ~/config/seednodes
```

### **Генерируем ключи**

Для облегчения получения ключей, вместо запуска ноды без них, использования команды `suggest_brain_key` в cli-wallet для генерирования, правки конфига и перезапуска ноды, можно сразу сгенерировать их [здесь](https://control.viz.world/tools/keys/).

![](https://lh6.googleusercontent.com/CiBEBEORRNyJRZDLDmgrPpqZ8k0yG6NR0doq88h26YaRpH5ioh-eOcFFT-ztCMgVA9u2PAAYnnBBOJ8wkKo10N2NYRPC7e5H3EZrdZiZOIQw_Az1lmUl6Tlut17nbMc5AroXUR5g)

Только в Public-key \(публичном ключе\) заменить три начальных символа `VIZ` на `GLS`. Этот ключ нам понадобится для объявления себя делегатом позднее, а Private-key \(приватный ключ\) уже на следующем шаге.

### **Загружаем конфиг**

Предварительно заменив значения `witness` и `private-key` на свои.   
****В качестве `witness` запишем **логин без @** от своего аккаунта на Голосе**,** `private-key` тот что сгенерировали на шаге выше.

```text
echo 'p2p-endpoint = 0.0.0.0:4243
webserver-thread-pool-size = 1
webserver-http-endpoint = 0.0.0.0:8090
webserver-ws-endpoint = 0.0.0.0:8091
read-wait-micro = 500000
max-read-wait-retries = 2
write-wait-micro = 500000
max-write-wait-retries = 3
single-write-thread = true
enable-plugins-on-push-transaction = false
shared-file-size = 2G
min-free-shared-file-size = 500M
inc-shared-file-size = 2G
block-num-check-free-size = 1000
plugin = chain p2p json_rpc webserver network_broadcast_api witness database_api witness_api
clear-votes-before-block = 4294967295
store-account-metadata = false
store-memo-in-savings-withdraws = false
replay-if-corrupted = true
skip-virtual-ops = true
enable-stale-production = false
witness = "ЛОГИН-ДЕЛЕГАТА"
private-key = ПРИВАТНЫЙ-КЛЮЧ-НОДЫ
mining-threads = 0
[log.console_appender.stderr]
stream=std_error
[log.file_appender.p2p]
filename=logs/p2p/p2p.log
[logger.default]
level=debug
appenders=stderr
[logger.p2p]
level=none
appenders=stderr' | sudo tee -a ~/config/config.ini
```

### **Запускаем контейнер**

```text
sudo docker run -it -p 4243:4243 -v ~/config:/etc/golosd -v ~/home/blockchain:/var/lib/golosd/blockchain -v ~/w/:/golosd/ --name golosd vizlex/golos-classic
```

Начнётся загрузка образа ноды и реплей \(наполнение файла оперативных данных `shared_memory.bin` из блоков\), который будет продолжаться от пары часов до суток \(в зависимости от производительности вашего сервера\). 

Можно оставить окно терминала или закрыть его, подключившись позднее и зайдя в логи ноды командой:

```text
sudo docker logs -f --tail 50 golosd
```

С появлением в логах ноды подобных сообщений о получении блоков, окно терминала можно закрыть \(всё в порядке\).

![](https://lh4.googleusercontent.com/xfrNK9kKgadic6F7mAiZ3notwNTdG1IxzMwWWtybsdox6VtV9HrXU5LLkQyLBWHPb-GLgKcv39spoG9Heavv4yTQItEQRyc01aJjCi21BfGorCxs6aGyPyYTxgkzea_8iv6QAFzd)

## **Работа с cli-wallet**

{% hint style="info" %}
Эти действия нужны лишь для первого запуска ноды.
{% endhint %}

Заходим в cli\_wallet ноды. 

```text
sudo docker exec -it golosd cli_wallet -w /golosd/wallet.json -s ws://localhost:8091
```

Добавляем свой пароль к cli\_wallet \(запишите его и сохраните\):

```text
set_password 123456789
```

Разблокируем доступ:

```text
unlock 123456789
```

Импортируем в cli\_wallet наш приватный активный ключ аккаунта-делегата \(ключ, который смотреть тут [https://golos.id/@ЛОГИН/permissions](https://golos.id/@%D0%9B%D0%9E%D0%93%D0%98%D0%9D/permissions), начинается с цифры 5\)**.**

```text
import_key 5Js............
```

Объявляем себя делегатом, с публичным ключом GLS \(который мы сгенерировали ранее в паре к приватному ключу для конфига ноды\)**.**

Нужно заменить логин, ссылку на пост/аккаунт делегата + публичный ключ.

```text
update_witness "ЛОГИН" "https://golos.id/@ЛОГИН" GLS............. true
```

Публикуем свой первый прайс-фид, заменив на свой логин-делегата.

```text
publish_feed ЛОГИН {"quote":"1.000 GOLOS", "base":"0.500 GBG"} true
```

Для выхода из cli\_wallet вводим команду:

```text
quit
```

## **Публикация прайсфидов**

Запускаем в докере ещё один контейнер, подробнее об этом удобном скрипте можно прочитать [здесь](../price-feed.md).

```text
sudo docker run -it -e NODE=ws://localhost:8091 -e WITNESS=ЛОГИН-ДЕЛЕГАТА -e KEY=ПРИВАТНЫЙ-АКТИВНЫЙ-КЛЮЧ --name feed --net=container:golosd vvk123/golos-witness-tools ./update_price_feed.py --monitor
```

\* нужно заменить логин и приватный активный ключ аккаунта-делегата на свои

Пример скриншота после выполнения команды:

![](https://lh6.googleusercontent.com/zmlDvPxwhoHPAvQwz4MzO8esAFGcO26_fWM1Mvb_5eMQxavdQb6HIBwDYuPEOo_zQXfIbRup0SvM_v150D4mSJ6UCwHcO6LCS2h_ZOWnSoY5D9YrjVRyL2urxo22qWxvxGPCnx7N)

После появления логов с калькуляцией курса GBG, закрываем окно терминала.

## **Изменения в конфиге ноды**

Заходим в конфиг командой**:**

```text
sudo nano ~/config/config.ini
```

Вносим нужные правки, нажимаем `Ctrl+O`, подтверждаем `Enter`, выходим `Ctrl+X`**.**

Перезапускаем контейнер ноды.

```text
sudo docker restart golosd
```

## **Делегатские параметры**

Заходим на [https://golos.id/~witnesses](https://golos.id/~witnesses) и напротив своего делегата в столбце “Параметры” нажимаем на значок настроек **\(**описание каждого параметра возникает при наведении мышкой\).

[Подробнее](../median-props.md) о значении медианных параметров.

Изменить параметры можно и через cli\_wallet ноды, заменив логин и выполнив команду:

```text
update_chain_properties ЛОГИН {"account_creation_fee":"1.000 GOLOS", "maximum_block_size":65536, "sbd_interest_rate":0, "create_account_min_golos_fee":"0.100 GOLOS", "create_account_min_delegation":"1.000 GOLOS", "create_account_delegation_time":2592000, "min_delegation":"1.000 GOLOS"} true
```

Параметры добавленные с 19 ХФ:

```text
update_chain_properties ЛОГИН {"max_referral_interest_rate":1000, "max_referral_term_sec":15552000, "min_referral_break_fee":"1.000 GOLOS", "max_referral_break_fee":"100.000 GOLOS", "posts_window":3, "posts_per_window":1, "comments_window":200, "comments_per_window":10, "votes_window":15, "votes_per_window":5, "auction_window_size":0, "max_delegated_vesting_interest_rate":8000, "custom_ops_bandwidth_multiplier":10, "min_curation_percent":7500, "max_curation_percent":7500, "curation_reward_curve":"square_root", "allow_distribute_auction_reward":true, "allow_return_auction_reward_to_fund":true} true
```

Параметры добавленные с 22 ХФ:

```text
update_chain_properties ЛОГИН {"worker_reward_percent":1000, "witness_reward_percent":1500, "vesting_reward_percent":6500, "worker_request_creation_fee":"100.000 GBG", "worker_request_approve_min_percent":1500, "sbd_debt_convert_rate":100, "vote_regeneration_per_day":10, "witness_skipping_reset_time":21600, "witness_idleness_time":7776000, "account_idleness_time":15552000} true
```

## **Обновление ноды**

Ставим “пустой ключ” для ноды чтобы приостановить подпись блоков через параметры на странице [https://golos.id/@lex/witness](https://golos.id/@lex/witness) \(заменив на свой логин\).  
  
****Или через cli\_wallet ноды командой

```text
update_witness "ЛОГИН" "https://golos.id" GLS1111111111111111111111111111111114T1Anm true
```

Останавливаем докер-контейнер ноды и удаляем его

```text
sudo docker stop golosd && sudo docker rm golosd
```

{% hint style="info" %}
Если в анонсе обновления было указано что нужен реплей, удаляем файл shared\_memory.bin
{% endhint %}

```text
sudo rm ~/home/blockchain/shared_memory.bin
```

Останавливаем докер-контейнер скрипта прайсфида и удаляем его

```text
sudo docker stop feed && sudo docker rm feed
```

Удаляем образы для ноды и скрипта прайсфида

```text
sudo docker rmi vizlex/golos-classic && sudo docker rmi vvk123/golos-witness-tools
```

Запускаем контейнер с новой версией

```text
sudo docker run -it -p 4243:4243 -v ~/config:/etc/golosd -v ~/home/blockchain:/var/lib/golosd/blockchain -v ~/w/:/golosd/ --name golosd vizlex/golos-classic
```

После появления логов вида `handle_block "Got 0 transactions on block 34563842 by ..."`, закрываем окно терминала. 

Возвращаем публичный ключ ноды GLS.............. который ранее сбрасывали через параметры на странице [https://golos.id/@lex/witness](https://golos.id/@lex/witness)  
  
или через cli\_wallet, заменив в команде ниже логин, ссылку на пост/аккаунт делегата + публичный ключ на свои:

```text
update_witness "ЛОГИН" "https://golos.id/@ЛОГИН" GLS................. true
```

Возвращаем докер-контейнер скрипта публикации прайсфида \(заменив логин и приватный активный ключ аккаунта-делегата на свои\)

```text
sudo docker run -it -e NODE=ws://localhost:8091 -e WITNESS=ЛОГИН-ДЕЛЕГАТА -e KEY=ПРИВАТНЫЙ-АКТИВНЫЙ-КЛЮЧ --name feed --net=container:golosd vvk123/golos-witness-tools ./update_price_feed.py --monitor
```

После появления логов с калькуляцией курса GBG, закрываем окно терминала.

