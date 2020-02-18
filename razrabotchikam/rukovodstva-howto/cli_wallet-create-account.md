# Создание аккаунта через cli\_wallet

Автор: [@ropox](https://golos.io/@ropox)

Я уже писал [тут](https://golos.io/ru--golos/@ropox/kak-sozdat-polzovatelya-golosa-cherez-cli-koshelek), как создать пользователя через cli\_wallet, но я тогда честно говоря по незнанию переборщил - я тогда ждал пока нода не поднимется и не синхронизируется с блокчейном.

Поэтому я решил обновить статью. Эта статья вообщем-то повторяет старую, но отличие этой в том, что мы не будем ждать пока нода полностью синхронизируется. Вместо этого мы подключимся кошельком к официальной ноде wss://ws.golos.io или любой другой ноде. Отпадает конфигурирование, можно обойтись меньшим размером оперативки и вообще более слабым компьютером.

## Докер

Как установить докер я не буду описывать. На своем компьютере \(линукс, ubuntu 17\) я установил докер из репозитория.

`sudo apt install docker.io`

Теперь как по [инструкции](https://golos.io/ru--otkrytyij-kod/@someguy123/golos-docker-golos-in-a-box-obnovlenie-do-versii-khardforka-16-03) от [@someguy123](https://github.com/golos-blockchain/wiki/tree/3dbb2f2a042981607f6ae50283ab37510a67a70d/@someguy123/README.md).

Выполнить следующие команды

```text
git clone https://github.com/Someguy123/golos-docker.git
cd golos-docker
./run.sh install
mkdir data/witness_node_data_dir/blockchain
./run.sh start
```

Если все было правильно сделано, то вы увидите, что то вроде

```text
Starting container...
golos-seed
```

Голос-нода теперь работает, но нам нужен только консольный кошелек - cli\_wallet в контейнере. Чтобы “войти” в контейнер выполните следующую команду

```text
./run.sh enter
```

## Подготавливаем кошелек

Если все нормально, то вы теперь находитесь в контейнере докера. По следующему пути `/golos/` находится точка монтирования директории golos-docker/data. Здесь мы сохраним файл с ключами.

Запустим cli\_wallet

```text
/usr/local/bin/cli_wallet -w /golos/wallet.json -s wss://ws.golos.io
```

Где  
 -w /golos/wallet.json - Файл с ключами  
 -s wss://ws.golos.io - Мы подключаемся к офф. ноде

Кошелек запустится и предложит для начала задать пароль для файла с ключами.

```text
Please use the set_password method to initialize a new wallet before continuing
new >>> 
```

Вводим `set_password 123456`

Теперь в строке ввода, вместо “New”, стоит “locked &gt;&gt;&gt;”. Разблокируем файл введя пароль `unlock 123456`, после чего в строке ввода будет написано “unlocked &gt;&gt;&gt;”.

Проверить ноду можно командой `info`, должно выдать много полезной инфы о состоянии блокчейна.

Теперь собственно импортируем приватный, активный ключ.

```text
import_key 51f1f1f1f1f1f1f1f1f1f1f1f1f1f1f1f1f1f1f1f
```

И проверяем сработало ли

```text
list_my_accounts
ropox                    74.368 GOLOS 47303729.658692 GESTS          222.701 GBG
-------------------------------------------------------------------------
TOTAL                    74.368 GOLOS 47303729.658692 GESTS          222.701 GBG
```

## Создание пользователя

Ну и собственно к чему мы шли. Проверяем существует ли аккаунт, который мы решили зарегистрировать.

```text
get_account tolstoy
2669058ms th_a       websocket_api.cpp:88          on_message           ] message: {"id":21,"result":[]} 
10 assert_exception: Assert Exception
!accounts.empty(): Unknown account
    {}
    th_a  wallet.cpp:396 get_account
```

Нету, значит столбим

```text
create_account ropox tolstoy "{}" true
```

Команда выдаст много инфы на экран и в частности четыре публичных ключа. Собственно команда при исполнении сама генерит эти четыре ключа: хозяйский, постинг, активный и мемо и сохраняет их в файле кошелька. Это видно в логе

```text
2745489ms th_a       wallet.cpp:470                save_wallet_file     ] saving wallet to file /golos/wallet.json
2745489ms th_a       wallet.cpp:470                save_wallet_file     ] saving wallet to file /golos/wallet.json
2745489ms th_a       wallet.cpp:470                save_wallet_file     ] saving wallet to file /golos/wallet.json
2745490ms th_a       wallet.cpp:470                save_wallet_file     ] saving wallet to file /golos/wallet.json
```

Смотрим список аккаунтов и убеждаемся, что с меня содрали 3 Голоса за регистрацию аккаунта и у меня теперь еще один новый аккаунт.

```text
list_my_accounts
ropox                    71.368 GOLOS 47303729.658692 GESTS          222.701 GBG
tolstoy                   0.000 GOLOS   11492.614025 GESTS            0.000 GBG
-------------------------------------------------------------------------
TOTAL                    71.368 GOLOS 47315222.272717 GESTS          222.701 GBG
```

Ну и смотрим список ключей

```text
list_keys
```

Можно их сохранить где нибудь у себя или распечатать. Если сейчас нажать Ctrl-C и ENTER и ввести `exit`, то мы покинем контейнер. В папочке `data` лежит вновь созданный wallet.json. Его тоже можно сохранить где нибудь и использовать повторно, если хотите создать нового пользователя или для перевода монет. Не забудьте только пароль - 123456 \)\)\).

После окончании работы, не забудьте остановить контейнер.

```text
./run.sh stop
```

## Ключи

Я лично храню wallet.json, так как им удобно пользоваться, если надо перекинуть монеты с аккаунта на аккаунт и тому подобных дел. Так же я сохранил выхлоп list\_keys команды у себя в гугл док. Там мешанина из открытых и приватных ключей. Как узнать, какой ключ, что? Для этого к примеру идем на [http://golosd.com/@tolstoy](http://golosd.com/@tolstoy). Прокручиваем до окошка "Authorities" и видим открытые ключи. Копируем постинг ключ и ищем его в списке ключей. Ниже находится приватный ключ.

Используя постинг ключ можно войти на сайт golos.io и писать комментарий, посты и апвотить. Чтобы сделать перевод потребуется активный ключ.

## Ошибки

Бывает, что кошелек вылетает с такой ошибкой

```text
10 assert_exception: Assert Exception
!ec: websocket send failed: invalid state
    {"msg":"invalid state"}
    th_a  websocket.cpp:164 send_message
```

Видимо иногда пропадает соединение с нодой и не восстанавливается. Помогает закрыть кошелек \(Ctrl-C и Enter\) и снова запустить.

 По материалам [статьи](https://golos.io/golos/@ropox/kak-sozdat-novogo-polzovatelya-golosa-ispolzuya-cliwallet). Автор [@ropox](https://golos.io/@ropox)

