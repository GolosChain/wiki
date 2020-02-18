# Пример запуска тестовой сети \#2

Автор: [@ropox](https://golos.io/@ropox)

## Зачем требуется тестнет

Порой для писателей скриптов, ботов для голоса требуется отладить работу программ в тишине и спокойствии и в контролируемой среде. Не хочется, что бы бот к примеру перевел внезапно 10000 голосов неизвестно кому из-за глупой ошибки. Как это случилось однажды с доброботом. Опять же для регрессионных тестов, нужна возможность повторения сценариев с одинаковыми исходными данными, чтобы получить повторяемые, ожидаемые результаты. Ну и конечно тестирование ХФ, новой версии сети голоса. У нас сейчас актуальная версия сети 0.16.4, а новая версия будет 0.17.0. Если кто-то захочет протестировать новую версию, подключившись к реальной сети, то возможно нода с “неправильной” версией будет “саботировать” сеть.

Вот для этого и запускается testnet. С тестовыми пользователями уже имеющими на балансе средства, с измененными настройками, позволяющими ускорить тестирование.

К примеру можно установить уменьшение силы голоса не раз в неделю как в настоящей сети, а раз в 15 минут. Иначе бы пришлось ждать результатов теста неделями, а в тестнете всего 15 минут. Окно авторских вознаграждений можно сократить с недели, до получаса. Выгоды очевидны.

Тестнет не связан с рабочей сетью и не мешает работе действующей сети. Так же можно тестировать свой секретный проект так, что бы до поры до времени никто про него не узнал.

## Сборка образ докера

В официальном репозитории golos уже есть готовые файлы для docker, и предпочтительно использовать их, чтобы не городить свои скрипты для компиляции, бороться с зависимостями. Все для вас сделает docker. Как работать с докером я расписывать сильно не буду. Инструкций полно в интернете.

### Подготовка исходников

Для начала скачаем исходники golos. Для этого воспользуемся git-ом.

```text
$ git clone https://github.com/GolosChain/golos.git

Клонирование в «golos»…
remote: Counting objects: 22425, done.
remote: Compressing objects: 100% (150/150), done.
remote: Total 22425 (delta 132), reused 177 (delta 76), pack-reused 22152
Получение объектов: 100% (22425/22425), 13.47 MiB | 4.58 MiB/s, готово.
Определение изменений: 100% (15314/15314), готово.
```

У нас создастся папка golos с исходниками. Перейдем в нее и последующие комманды будет запускать в ней.

Нам нужно будет переключить на нужную нам ветку. В настоящий момент меня интересует версия 0.17.0.

![](https://imgp.golos.io/0x0/https://s3.postimg.org/83dc9q33n/047.png)

Как видно на скриншоте, есть branch под названием **golos-v0.17.0**. Вот на нее нам и надо переключиться. Для этого в корне папки с исходниками выполним следующую команду

```text
$ git checkout golos-v0.17.0
Ветка golos-v0.17.0 отслеживает внешнюю ветку golos-v0.17.0 из origin.
Переключено на новую ветку «golos-v0.17.0»
```

И теперь нам надо обновить локальные файлы до версий в репозитории на гитхабе

```text
$ git submodule update --init --recursive
Подмодуль «libraries/chainbase» (https://github.com/GolosChain/chainbase.git) зарегистрирован по пути «libraries/chainbase»
Подмодуль «libraries/fc» (https://github.com/GolosChain/fc.git) зарегистрирован по пути «libraries/fc»
Подмодуль «libraries/libcds» (https://github.com/khizmax/libcds.git) зарегистрирован по пути «libraries/libcds»
Клонирование в «/hom...
```

Теперь можно поменять нужные нам для теста параметры. К примеру я хочу тестировать скрипт, который должен запускаться после понижения силы голоса. Чтобы не ждать неделями, я сокращу интервал вывода СГ до 10 минут. Для этого открываю файл с настройками сети в редакторе.

```text
$ nano libraries/protocol/include/steemit/protocol/config.hpp
```

И ищу переменные со словам withdraw и почти сразу нахожу нужные мне.  
 ![](https://imgp.golos.io/0x0/https://s1.postimg.org/4vryqapwv/048.png)

Меня интересует конкретно вот эта строка

```text
#define STEEMIT_VESTING_WITHDRAW_INTERVAL_SECONDS (60*60*24*7) // 1 week per interval
```

Я исправляю, чтобы интервал был 10 минут и сохраняю файл

```text
#define STEEMIT_VESTING_WITHDRAW_INTERVAL_SECONDS (60*10) // 1 week per interval
```

### Сборка образа

В папке docker лежит файл для докера с инструкциями по сборке образа.

```text
$ ls docker/*
docker/Dockerfile-testnet
```

Все, что нам требуется, это запустить сборку

```text
$ docker build . -f docker/Dockerfile-testnet -t testnet-17
```

Спустя какое то время мы имеем готовый образ докера

![](https://imgp.golos.io/0x0/https://s4.postimg.org/7rt17rs0t/049.png)

## Запуск

Для начального запуска воспользуемся командой run докера

```text
$ docker run -it -p 127.0.0.1:9090:8090 -d --name testnet -t testnet-17
```

Собственно все понятно. -it включает интерактивность. “-p 127.0.0.1:9090:8090” включает переадресацию порта 8090 изнутри контейнера наружу, на порт 9090. Это дает возможность подключаться скриптами к тестнету используя URL вида ws://localhost:9090. Опция -d запускает контейнер отключенным от консоли в фоне. --name testnet задет простое имя контейнеру. -t testnet-17 задает имя образа, который будет использоваться для создания контейнера.

Если все нормально, после запуска контейнера докер распечатает ID контейнера и вернет управление в консоль.

Как я писал выше, в тестнете уже созданы пара тестовых пользователей. Самый важный cyberfounder. Естественно, чтобы можно было этим аккаунтом пользоваться, нужны ключи. Приватный и публичный. При запуске, golos распечатывает пару ключей в самом начале. Посмотреть их можно распечатав лог голоса.

```text
$ docker logs testnet
```

![](https://imgp.golos.io/0x0/https://s18.postimg.org/uo28fw5bd/050.png)

```text
initminer public key: GLS58g5rWYS3XFTuGDSxLVwiBiPLoAyCZgn6aB9Ueh8Hj5qwQA3r6
initminer private key: 5JVFFWRLwz6JoP9kguuRFfytToGU6cLgBVTL9t6NB3D3BQLbUBS
chain id: 5876894a41e6361bde2e73278f07340f2eb8b41c2facd29099de9deef6cdb679
blockchain version: 0.17.0
```

## Конфигурационный файл

Чтобы сеть заработала, нужен хотя бы один делегат, подписывающий блоки. Этим и будет заниматься аккаунт cyberfounder. Чтобы это заработало, нужно отредактировать конфигурационный файл.

Сначала зайдем в образ

```text
$ docker exec -it testnet bash
```

Конфигурационный файл config.ini лежит в папке /etc/golosd.

```text
vi /etc/golosd/config.ini
```

Изменим следующие параметры

```text
witness = "cyberfounder"
private-key = 5JVFFWRLwz6JoP9kguuRFfytToGU6cLgBVTL9t6NB3D3BQLbUBS
enable-stale-production = true
required-participation = 0
```

После чего перезапускаем контейнер

```text
$ docker stop testnet
$ docker start testnet
```

И видим, что сеть заработала и cyberfounder начал генерировать блоки

```text
$ docker logs testnet
```

![](https://imgp.golos.io/0x0/https://s3.postimg.org/kirac52ub/051.png)

Дождемся 17-го хардфорка, после чего можно пользоваться тестнетом

```text
1551001ms th_a       database.cpp:4890             apply_hardfork       ] HARDFORK 17 at block 36
1551002ms th_a       witness.cpp:205               block_production_loo ] Generated block #36 with timestamp 2017-07-29T15:25:51 at time 2017-07-29T15:25:51 by cyberfounder
```

Запускаем cli\_wallet, задаем пароль, и импортируем приватный ключ cyberfounder аккаунта

```text
$ docker exec -it testnet cli_wallet

Please use the set_password method to initialize a new wallet before continuing
new >>> set_password 123
set_password 123
null
locked >>> unlock 123
unlock 123
null
unlocked >>> import_key 5JVFFWRLwz6JoP9kguuRFfytToGU6cLgBVTL9t6NB3D3BQLbUBS
import_key 5JVFFWRLwz6JoP9kguuRFfytToGU6cLgBVTL9t6NB3D3BQLbUBS
2152613ms th_a       wallet.cpp:534                save_wallet_file     ] saving wallet to file wallet.json
true
unlocked >>> 


```

На аккаунте cyberfounder есть уже некая сумма голосов

```text
unlocked >>> list_account_balances cyberfounder

[
  "43306208.000 GOLOS"
]
```

Можно создать нового пользователя и перевести ему GOLOS

```text
unlocked >>> create_account cyberfounder ropox "{}" true
unlocked >>> transfer cyberfounder ropox "200000.000 GOLOS" "для тестов" true
{
  "ref_block_num": 338,
  "ref_block_prefix": 853465515,
  "expiration": "2017-07-29T15:41:27",
  "operations": [[
      "transfer",{
        "from": "cyberfounder",
        "to": "ropox",
        "amount": "200000.000 GOLOS",
        "memo": "для тестов"
      }
    ]
  ],
  "extensions": [],
  "signatures": [
    "20491f45c3f3f8d67f3a5c62f934c7c22312ddf392edde2f78a9962634a0ed7b4e4d2f6428282e0d0837893013c04e9e5e0057974c43a490b4abb4416091d5a18b"
  ],
  "transaction_id": "7d155661801e3b5a450d5c1f97378ee5a94a54b6",
  "block_num": 339,
  "transaction_num": 0
}


```

Ну и вишенка на торт, создаем свой, персональный токен

```text
unlocked >>> create_asset cyberfounder ROPOX 3 {"description": "Золото царя Гороха", "core_exchange_rate":{"base":"1.000 ROPOX","quote":"1.000 GOLOS"}} null true
```

```text

{
  "ref_block_num": 380,
  "ref_block_prefix": 3346277100,
  "expiration": "2017-07-29T15:43:33",
  "operations": [[
      "asset_create",{
        "issuer": "cyberfounder",
        "asset_name": "ROPOX",
        "precision": 3,
        "common_options": {
          "max_supply": "1000000000000000",
          "market_fee_percent": 0,
          "max_market_fee": "1000000000000000",
          "issuer_permissions": 79,
          "flags": 0,
          "core_exchange_rate": {
            "base": "1.000 ROPOX",
            "quote": "1.000 GOLOS"
          },
          "whitelist_authorities": [],
          "blacklist_authorities": [],
          "whitelist_markets": [],
          "blacklist_markets": [],
          "description": "Золото царя Гороха",
          "extensions": []
        },
        "is_prediction_market": false,
        "extensions": []
      }
    ]
  ],
  "extensions": [],
  "signatures": [
    "20225a9ce260f8434f3e83cf8e48c4e16557f38e5540fa9a9a53126c685b70f16b3619a95b50c7f7a611e9c26261fd669c4ad8496cdc45d95ea9d3ef12c5cc5e33"
  ]
}
```

Инстанциируем немного монет и переводим их тестовому пользователю

```text
unlocked >>> issue_asset cyberfounder "1000.000 ROPOX" "Акции" true
unlocked >>> transfer cyberfounder ropox "20.000 ROPOX" "Подарок" true
unlocked >>> list_account_balances ropox
[
  "0.000 GBG",
  "200000.000 GOLOS",
  "20.000 ROPOX"
]
```

Удачных вам экспериментов!

 По материалам [статьи](https://golos.io/ru--golos/@ropox/zapusk-testnet-golos). Автор [@ropox](https://golos.io/@ropox)

