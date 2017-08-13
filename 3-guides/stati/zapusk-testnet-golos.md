<h1>Запуск тестнета Голос</h1>

Автор: <a href="https://golos.io/@ropox">@ropox</a>

<h2>Зачем требуется тестнет</h2>

<p>Порой для писателей скриптов, ботов для голоса требуется отладить работу программ в тишине и спокойствии и в контролируемой среде. Не хочется, что бы бот к примеру перевел внезапно 10000 голосов неизвестно кому из-за глупой ошибки. Как это случилось однажды с доброботом. Опять же для регрессионных тестов, нужна возможность повторения сценариев с одинаковыми исходными данными, чтобы получить повторяемые, ожидаемые результаты. Ну и конечно тестирование ХФ, новой версии сети голоса. У нас сейчас актуальная версия сети 0.16.4, а новая версия будет 0.17.0. Если кто-то захочет протестировать новую версию, подключившись к реальной сети, то возможно нода с “неправильной” версией будет “саботировать” сеть.</p>
<p>Вот для этого и запускается testnet. С тестовыми пользователями уже имеющими на балансе средства, с измененными настройками, позволяющими ускорить тестирование.</p>
<p>К примеру можно установить уменьшение силы голоса не раз в неделю как в настоящей сети, а раз в 15 минут. Иначе бы пришлось ждать результатов теста неделями, а в тестнете всего 15 минут. Окно авторских вознаграждений можно сократить с недели, до получаса. Выгоды очевидны.</p>
<p>Тестнет не связан с рабочей сетью и не мешает работе действующей сети. Так же можно тестировать свой секретный проект так, что бы до поры до времени никто про него не узнал.</p>
<h2>Сборка образ докера</h2>
<p>В официальном репозитории golos уже есть готовые файлы для docker, и предпочтительно использовать их, чтобы не городить свои скрипты для компиляции, бороться с зависимостями. Все для вас сделает docker. Как работать с докером я расписывать сильно не буду. Инструкций полно в интернете.</p>
<h3>Подготовка исходников</h3>
<p>Для начала скачаем исходники golos. Для этого воспользуемся git-ом.</p>
<pre><code>$ git clone https://github.com/GolosChain/golos.git

Клонирование в «golos»…
remote: Counting objects: 22425, done.
remote: Compressing objects: 100% (150/150), done.
remote: Total 22425 (delta 132), reused 177 (delta 76), pack-reused 22152
Получение объектов: 100% (22425/22425), 13.47 MiB | 4.58 MiB/s, готово.
Определение изменений: 100% (15314/15314), готово.
</code></pre>
<p>У нас создастся папка golos с исходниками. Перейдем в нее и последующие комманды будет запускать в ней.</p>
<p>Нам нужно будет переключить на нужную нам ветку. В настоящий момент меня интересует версия 0.17.0.</p>
<p><img src="https://imgp.golos.io/0x0/https://s3.postimg.org/83dc9q33n/047.png" /></p>
<p>Как видно на скриншоте, есть branch под названием <strong>golos-v0.17.0</strong>. Вот на нее нам и надо переключиться. Для этого в корне папки с исходниками выполним следующую команду</p>
<pre><code>$ git checkout golos-v0.17.0
Ветка golos-v0.17.0 отслеживает внешнюю ветку golos-v0.17.0 из origin.
Переключено на новую ветку «golos-v0.17.0»
</code></pre>
<p>И теперь нам надо обновить локальные файлы до версий в репозитории на гитхабе</p>
<pre><code>$ git submodule update --init --recursive
Подмодуль «libraries/chainbase» (https://github.com/GolosChain/chainbase.git) зарегистрирован по пути «libraries/chainbase»
Подмодуль «libraries/fc» (https://github.com/GolosChain/fc.git) зарегистрирован по пути «libraries/fc»
Подмодуль «libraries/libcds» (https://github.com/khizmax/libcds.git) зарегистрирован по пути «libraries/libcds»
Клонирование в «/hom...
</code></pre>
<p>Теперь можно поменять нужные нам для теста параметры. К примеру я хочу тестировать скрипт, который должен запускаться после понижения силы голоса. Чтобы не ждать неделями, я сокращу интервал вывода СГ до 10 минут. Для этого открываю файл с настройками сети в редакторе.</p>
<pre><code>$ nano libraries/protocol/include/steemit/protocol/config.hpp
</code></pre>
<p>И ищу переменные со словам withdraw и почти сразу нахожу нужные мне.<br />
<img src="https://imgp.golos.io/0x0/https://s1.postimg.org/4vryqapwv/048.png" /></p>
<p>Меня интересует конкретно вот эта строка</p>
<pre><code>#define STEEMIT_VESTING_WITHDRAW_INTERVAL_SECONDS (60*60*24*7) // 1 week per interval
</code></pre>
<p>Я исправляю, чтобы интервал был 10 минут и сохраняю файл</p>
<pre><code>#define STEEMIT_VESTING_WITHDRAW_INTERVAL_SECONDS (60*10) // 1 week per interval
</code></pre>
<h3>Сборка образа</h3>
<p>В папке docker лежит файл для докера с инструкциями по сборке образа.</p>
<pre><code>$ ls docker/*
docker/Dockerfile-testnet
</code></pre>
<p>Все, что нам требуется, это запустить сборку</p>
<pre><code>$ docker build . -f docker/Dockerfile-testnet -t testnet-17
</code></pre>
<p>Спустя какое то время мы имеем готовый образ докера</p>
<p><img src="https://imgp.golos.io/0x0/https://s4.postimg.org/7rt17rs0t/049.png" /></p>
<h2>Запуск</h2>
<p>Для начального запуска воспользуемся командой run докера</p>
<pre><code>$ docker run -it -p 127.0.0.1:9090:8090 -d --name testnet -t testnet-17
</code></pre>
<p>Собственно все понятно. -it включает интерактивность. “-p 127.0.0.1:9090:8090” включает переадресацию порта 8090 изнутри контейнера наружу, на порт 9090. Это дает возможность подключаться скриптами к тестнету используя URL вида ws://localhost:9090. Опция -d запускает контейнер отключенным от консоли в фоне. --name testnet задет простое имя контейнеру. -t testnet-17 задает имя образа, который будет использоваться для создания контейнера.</p>
<p>Если все нормально, после запуска контейнера докер распечатает ID контейнера и вернет управление в консоль.</p>
<p>Как я писал выше, в тестнете уже созданы пара тестовых пользователей. Самый важный cyberfounder. Естественно, чтобы можно было этим аккаунтом пользоваться, нужны ключи. Приватный и публичный. При запуске, golos распечатывает пару ключей в самом начале. Посмотреть их можно распечатав лог голоса.</p>
<pre><code>$ docker logs testnet
</code></pre>
<p><img src="https://imgp.golos.io/0x0/https://s18.postimg.org/uo28fw5bd/050.png" /></p>
<pre><code>initminer public key: GLS58g5rWYS3XFTuGDSxLVwiBiPLoAyCZgn6aB9Ueh8Hj5qwQA3r6
initminer private key: 5JVFFWRLwz6JoP9kguuRFfytToGU6cLgBVTL9t6NB3D3BQLbUBS
chain id: 5876894a41e6361bde2e73278f07340f2eb8b41c2facd29099de9deef6cdb679
blockchain version: 0.17.0
</code></pre>
<h2>Конфигурационный файл</h2>
<p>Чтобы сеть заработала, нужен хотя бы один делегат, подписывающий блоки. Этим и будет заниматься аккаунт cyberfounder. Чтобы это заработало, нужно отредактировать конфигурационный файл.</p>
<p>Сначала зайдем в образ</p>
<pre><code>$ docker exec -it testnet bash
</code></pre>
<p>Конфигурационный файл config.ini лежит в папке /etc/golosd.</p>
<pre><code>vi /etc/golosd/config.ini
</code></pre>
<p>Изменим следующие параметры</p>
<pre><code>witness = &quot;cyberfounder&quot;
private-key = 5JVFFWRLwz6JoP9kguuRFfytToGU6cLgBVTL9t6NB3D3BQLbUBS
enable-stale-production = true
required-participation = 0
</code></pre>
<p>После чего перезапускаем контейнер</p>
<pre><code>$ docker stop testnet
$ docker start testnet
</code></pre>
<p>И видим, что сеть заработала и cyberfounder начал генерировать блоки</p>
<pre><code>$ docker logs testnet
</code></pre>
<p><img src="https://imgp.golos.io/0x0/https://s3.postimg.org/kirac52ub/051.png" /></p>
<p>Дождемся 17-го хардфорка, после чего можно пользоваться тестнетом</p>
<pre><code>1551001ms th_a       database.cpp:4890             apply_hardfork       ] HARDFORK 17 at block 36
1551002ms th_a       witness.cpp:205               block_production_loo ] Generated block #36 with timestamp 2017-07-29T15:25:51 at time 2017-07-29T15:25:51 by cyberfounder
</code></pre>
<p>Запускаем cli_wallet, задаем пароль, и импортируем приватный ключ cyberfounder аккаунта</p>
<pre><code>$ docker exec -it testnet cli_wallet

Please use the set_password method to initialize a new wallet before continuing
new &gt;&gt;&gt; set_password 123
set_password 123
null
locked &gt;&gt;&gt; unlock 123
unlock 123
null
unlocked &gt;&gt;&gt; import_key 5JVFFWRLwz6JoP9kguuRFfytToGU6cLgBVTL9t6NB3D3BQLbUBS
import_key 5JVFFWRLwz6JoP9kguuRFfytToGU6cLgBVTL9t6NB3D3BQLbUBS
2152613ms th_a       wallet.cpp:534                save_wallet_file     ] saving wallet to file wallet.json
true
unlocked &gt;&gt;&gt; 


</code></pre>
<p>На аккаунте cyberfounder есть уже некая сумма голосов</p>
<pre><code>unlocked &gt;&gt;&gt; list_account_balances cyberfounder

[
  &quot;43306208.000 GOLOS&quot;
]
</code></pre>
<p>Можно создать нового пользователя и перевести ему GOLOS</p>
<pre><code>unlocked &gt;&gt;&gt; create_account cyberfounder ropox &quot;{}&quot; true
unlocked &gt;&gt;&gt; transfer cyberfounder ropox &quot;200000.000 GOLOS&quot; &quot;для тестов&quot; true
{
  &quot;ref_block_num&quot;: 338,
  &quot;ref_block_prefix&quot;: 853465515,
  &quot;expiration&quot;: &quot;2017-07-29T15:41:27&quot;,
  &quot;operations&quot;: [[
      &quot;transfer&quot;,{
        &quot;from&quot;: &quot;cyberfounder&quot;,
        &quot;to&quot;: &quot;ropox&quot;,
        &quot;amount&quot;: &quot;200000.000 GOLOS&quot;,
        &quot;memo&quot;: &quot;для тестов&quot;
      }
    ]
  ],
  &quot;extensions&quot;: [],
  &quot;signatures&quot;: [
    &quot;20491f45c3f3f8d67f3a5c62f934c7c22312ddf392edde2f78a9962634a0ed7b4e4d2f6428282e0d0837893013c04e9e5e0057974c43a490b4abb4416091d5a18b&quot;
  ],
  &quot;transaction_id&quot;: &quot;7d155661801e3b5a450d5c1f97378ee5a94a54b6&quot;,
  &quot;block_num&quot;: 339,
  &quot;transaction_num&quot;: 0
}


</code></pre>
<p>Ну и вишенка на торт, создаем свой, персональный токен</p>
<pre><code>unlocked &gt;&gt;&gt; create_asset cyberfounder ROPOX 3 {&quot;description&quot;: &quot;Золото царя Гороха&quot;, &quot;core_exchange_rate&quot;:{&quot;base&quot;:&quot;1.000 ROPOX&quot;,&quot;quote&quot;:&quot;1.000 GOLOS&quot;}} null true

{
  &quot;ref_block_num&quot;: 380,
  &quot;ref_block_prefix&quot;: 3346277100,
  &quot;expiration&quot;: &quot;2017-07-29T15:43:33&quot;,
  &quot;operations&quot;: [[
      &quot;asset_create&quot;,{
        &quot;issuer&quot;: &quot;cyberfounder&quot;,
        &quot;asset_name&quot;: &quot;ROPOX&quot;,
        &quot;precision&quot;: 3,
        &quot;common_options&quot;: {
          &quot;max_supply&quot;: &quot;1000000000000000&quot;,
          &quot;market_fee_percent&quot;: 0,
          &quot;max_market_fee&quot;: &quot;1000000000000000&quot;,
          &quot;issuer_permissions&quot;: 79,
          &quot;flags&quot;: 0,
          &quot;core_exchange_rate&quot;: {
            &quot;base&quot;: &quot;1.000 ROPOX&quot;,
            &quot;quote&quot;: &quot;1.000 GOLOS&quot;
          },
          &quot;whitelist_authorities&quot;: [],
          &quot;blacklist_authorities&quot;: [],
          &quot;whitelist_markets&quot;: [],
          &quot;blacklist_markets&quot;: [],
          &quot;description&quot;: &quot;Золото царя Гороха&quot;,
          &quot;extensions&quot;: []
        },
        &quot;is_prediction_market&quot;: false,
        &quot;extensions&quot;: []
      }
    ]
  ],
  &quot;extensions&quot;: [],
  &quot;signatures&quot;: [
    &quot;20225a9ce260f8434f3e83cf8e48c4e16557f38e5540fa9a9a53126c685b70f16b3619a95b50c7f7a611e9c26261fd669c4ad8496cdc45d95ea9d3ef12c5cc5e33&quot;
  ]
}
</code></pre>
<p>Инстанциируем немного монет и переводим их тестовому пользователю</p>
<pre><code>unlocked &gt;&gt;&gt; issue_asset cyberfounder &quot;1000.000 ROPOX&quot; &quot;Акции&quot; true
unlocked &gt;&gt;&gt; transfer cyberfounder ropox &quot;20.000 ROPOX&quot; &quot;Подарок&quot; true
unlocked &gt;&gt;&gt; list_account_balances ropox
[
  &quot;0.000 GBG&quot;,
  &quot;200000.000 GOLOS&quot;,
  &quot;20.000 ROPOX&quot;
]
</code></pre>
<p>Удачных вам экспериментов!</p></div></div></div></span>


<hr>
По материалам <a href=https://golos.io/ru--smaijliki/@lebekons/smailiki-na-golose-vsya-podborka-v-udobnoi-tablice">статьи</a> 
Автор <a href="https://golos.io/@ropox">@ropox</a>
<hr>
