# Настройка для API-ноды

Публичные API-ноды важная составляющая для блокчейна, особенно когда есть заинтересованность в развитии приложений \(клиентов, ботов, скриптов, игр и пр.\), которые порой повышают ценность для всего проекта.

В зависимости от целей, API-ноду можно поднять начиная с 2 ядер процессора, 8 Гб RAM и 80 Гб SSD памяти. Есть провайдеры, где подходящая VPS-ка обойдётся всего в [10$ за месяц](https://contabo.com/?show=configurator&vserver_id=229).

Ниже описан вариант установки публичной API-ноды для блокчейна GOLOS \(с хранением истории на неделю\). Для такой ноды, оптимальный вариант VPS-ка с 16 Гб RAM памяти.

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

Добавляем конфиг ноды \(указанные в нём `202800` блоков = неделя\). Какие плагины нужны для ваших целей, можно напр. уточнить в чате делегатов [https://t.me/golos\_witnesses](https://t.me/golos_witnesses)

```text
echo 'webserver-thread-pool-size = 2
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
plugin = chain p2p json_rpc webserver network_broadcast_api witness database_api witness_api follow social_network tags operation_history account_history market_history account_by_key worker_api
clear-votes-before-block = 4294967295
history-start-block = 34880000
comment-title-depth = 202800
comment-body-depth = 202800
comment-json-metadata-depth = 202800
history-blocks = 202800
replay-if-corrupted = true
skip-virtual-ops = false
enable-stale-production = false
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

Запускаем ноду в докер-контейнере.

```text
sudo docker run -it -p 127.0.0.1:8090:8090 -p 127.0.0.1:8091:8091 -v ~/config:/etc/golosd -v ~/home/blockchain:/var/lib/golosd/blockchain --name golosd vizlex/golos-classic
```

После загрузки докер-образа и реплея \(который занимает несколько часов\), с получением логов вида `handle_block "Got 0 transactions on block 35071930 by ..."` нода готова к работе.

## Устанавливаем Nginx

```text
sudo apt-add-repository ppa:nginx/stable -y
```

```text
sudo apt-get update
```

```text
sudo apt-get install nginx -y
```

Добавляем файл для своих настроек Nginx.

```text
sudo nano /etc/nginx/sites-enabled/node.conf
```

Копируем в него правила, предварительно заменив адрес `server_name` на свой субдомен/домен \(не забыв привязать его в настройках DNS к нашему IP сервера\). Бесплатные домены можно зарегистрировать напр. [здесь](http://www.freenom.com/ru/freeandpaiddomains.html).

```text
server {
listen 80;
server_name test.lexa.host;
location / {
if ($request_method = 'OPTIONS') {
add_header 'Access-Control-Allow-Origin' '*';
add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range';
add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';
add_header 'Access-Control-Max-Age' 1728000;
add_header 'Content-Type' 'text/plain; charset=utf-8';
add_header 'Content-Length' 0;
return 204;
}
if ($request_method = 'POST') {
add_header 'Access-Control-Allow-Origin' '*';
add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range';
add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';
}
if ($request_method = 'GET') {
add_header 'Access-Control-Allow-Origin' '*';
add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range';
add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';
}
proxy_http_version 1.1;
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_pass http://127.0.0.1:8090;
}
location /ws {
if ($request_method = 'OPTIONS') {
add_header 'Access-Control-Allow-Origin' '*';
add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
add_header 'Access-Control-Max-Age' 1728000;
add_header 'Content-Type' 'text/plain; charset=utf-8';
add_header 'Content-Length' 0;
return 204;
}
if ($request_method = 'POST') {
add_header 'Access-Control-Allow-Origin' '*';
add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range';
add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';
}
if ($request_method = 'GET') {
add_header 'Access-Control-Allow-Origin' '*';
add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range';
add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';
}
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_pass http://127.0.0.1:8091;
proxy_read_timeout 3600;
}
}
```

Сохраняем изменения `Ctrl+O`, подтверждаем `Enter`, выходим `Ctrl+X`.

## Устанавливаем Certbot

```text
sudo apt-get install software-properties-common
```

```text
sudo add-apt-repository ppa:certbot/certbot -y
```

```text
sudo apt-get update
```

```text
sudo apt-get install python-certbot-nginx -y
```

После следующей команды потребуется ввести:

1. E-mail, на который будут отправляться уведомления о необходимости продления сертификата; 
2. Согласиться с правилами сервиса введя `A и Enter`;
3. Отказаться от рассылки `N и Enter`;
4. Подтвердить добавление сертификатов к указанным доменам вводом `Enter`;
5. Отказаться от редиректа, введя `1 и Enter`.

```text
sudo certbot --nginx
```

Будут добавлены настройки в файл `node.conf`, которые можно перепроверить командой ниже и найти строки с пометкой `# managed by Certbot` в конце файла.

```text
sudo nano /etc/nginx/sites-enabled/node.conf
```

Выходим из файла `Ctrl+X`.

Перезапускаем Nginx.

```text
sudo systemctl restart nginx
```

Проверяем статус Nginx.

```text
sudo systemctl status nginx.service
```

Мы запустили публичную API-ноду, к которой можно подключаться как по адресу `https://test.lexa.host` \(RPC\) так и `wss://test.lexa.host/ws` \(WebSockets\).

При получении письма на e-mail о необходимости обновить сертификат \(раз в 90 дней\), это можно сделать командой:

```text
sudo certbot renew
```

## Есть вопросы?

Можно уточнить в чате делегатов [https://t.me/golos\_witnesses](https://t.me/golos_witnesses)

