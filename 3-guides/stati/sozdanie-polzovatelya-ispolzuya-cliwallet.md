<h1>Создание нового пользователя, используя cli_wallet</h1>

Автор: <a href="https://golos.io/@ropox">@ropox</a>

<p>Я уже писал <a href="https://golos.io/ru--golos/@ropox/kak-sozdat-polzovatelya-golosa-cherez-cli-koshelek">тут</a>, как создать пользователя через cli_wallet, но я тогда честно говоря по незнанию переборщил - я тогда ждал пока нода не поднимется и не синхронизируется с блокчейном.</p>
<p>Поэтому я решил обновить статью. Эта статья вообщем-то повторяет старую, но отличие этой в том, что мы не будем ждать пока нода полностью синхронизируется. Вместо этого мы подключимся кошельком к официальной ноде wss://ws.golos.io или любой другой ноде. Отпадает конфигурирование, можно обойтись меньшим размером оперативки и вообще более слабым компьютером.</p>
<h2>Докер</h2>
<p>Как установить докер я не буду описывать. На своем компьютере (линукс, ubuntu 17) я установил докер из репозитория.</p>
<p><code>sudo apt install docker.io</code></p>
<p>Теперь как по <a href="https://golos.io/ru--otkrytyij-kod/@someguy123/golos-docker-golos-in-a-box-obnovlenie-do-versii-khardforka-16-03">инструкции</a> от <a href="/@someguy123">@someguy123</a>.</p>
<p>Выполнить следующие команды</p>
<pre><code>git clone https://github.com/Someguy123/golos-docker.git
cd golos-docker
./run.sh install
mkdir data/witness_node_data_dir/blockchain
./run.sh start
</code></pre>
<p>Если все было правильно сделано, то вы увидите, что то вроде</p>
<pre><code>Starting container...
golos-seed
</code></pre>
<p>Голос-нода теперь работает, но нам нужен только консольный кошелек - cli_wallet в контейнере. Чтобы “войти” в контейнер выполните следующую команду</p>
<pre><code>./run.sh enter
</code></pre>
<h2>Подготавливаем кошелек</h2>
<p>Если все нормально, то вы теперь находитесь в контейнере докера. По следующему пути <code>/golos/</code> находится точка монтирования директории golos-docker/data. Здесь мы сохраним файл с ключами.</p>
<p>Запустим cli_wallet</p>
<pre><code>/usr/local/bin/cli_wallet -w /golos/wallet.json -s wss://ws.golos.io
</code></pre>
<p>Где<br />
  -w /golos/wallet.json - Файл с ключами<br />
  -s wss://ws.golos.io - Мы подключаемся к офф. ноде</p>
<p>Кошелек запустится и предложит для начала задать пароль для файла с ключами.</p>
<pre><code>Please use the set_password method to initialize a new wallet before continuing
new &gt;&gt;&gt; 
</code></pre>
<p>Вводим <code>set_password 123456</code></p>
<p>Теперь в строке ввода, вместо “New”, стоит “locked &gt;&gt;&gt;”. Разблокируем файл введя пароль <code>unlock 123456</code>, после чего в строке ввода будет написано “unlocked &gt;&gt;&gt;”.</p>
<p>Проверить ноду можно командой <code>info</code>, должно выдать много полезной инфы о состоянии блокчейна.</p>
<p>Теперь собственно импортируем приватный, активный ключ.</p>
<pre><code>import_key 51f1f1f1f1f1f1f1f1f1f1f1f1f1f1f1f1f1f1f1f
</code></pre>
<p>И проверяем сработало ли</p>
<pre><code>list_my_accounts
ropox                    74.368 GOLOS 47303729.658692 GESTS          222.701 GBG
-------------------------------------------------------------------------
TOTAL                    74.368 GOLOS 47303729.658692 GESTS          222.701 GBG
</code></pre>
<h2>Создание пользователя</h2>
<p>Ну и собственно к чему мы шли. Проверяем существует ли аккаунт, который мы решили зарегистрировать.</p>
<pre><code>get_account tolstoy
2669058ms th_a       websocket_api.cpp:88          on_message           ] message: {&quot;id&quot;:21,&quot;result&quot;:[]} 
10 assert_exception: Assert Exception
!accounts.empty(): Unknown account
    {}
    th_a  wallet.cpp:396 get_account
</code></pre>
<p>Нету, значит столбим</p>
<pre><code>create_account ropox tolstoy &quot;{}&quot; true
</code></pre>
<p>Команда выдаст много инфы на экран и в частности четыре публичных ключа. Собственно команда при исполнении сама генерит эти четыре ключа: хозяйский, постинг, активный и мемо и сохраняет их в файле кошелька. Это видно в логе</p>
<pre><code>2745489ms th_a       wallet.cpp:470                save_wallet_file     ] saving wallet to file /golos/wallet.json
2745489ms th_a       wallet.cpp:470                save_wallet_file     ] saving wallet to file /golos/wallet.json
2745489ms th_a       wallet.cpp:470                save_wallet_file     ] saving wallet to file /golos/wallet.json
2745490ms th_a       wallet.cpp:470                save_wallet_file     ] saving wallet to file /golos/wallet.json
</code></pre>
<p>Смотрим список аккаунтов и убеждаемся, что с меня содрали 3 Голоса за регистрацию аккаунта и у меня теперь еще один новый аккаунт.</p>
<pre><code>list_my_accounts
ropox                    71.368 GOLOS 47303729.658692 GESTS          222.701 GBG
tolstoy                   0.000 GOLOS   11492.614025 GESTS            0.000 GBG
-------------------------------------------------------------------------
TOTAL                    71.368 GOLOS 47315222.272717 GESTS          222.701 GBG
</code></pre>
<p>Ну и смотрим список ключей</p>
<pre><code>list_keys
</code></pre>
<p>Можно их сохранить где нибудь у себя или распечатать. Если сейчас нажать Ctrl-C и ENTER и ввести <code>exit</code>, то мы покинем контейнер. В папочке <code>data</code> лежит вновь созданный wallet.json. Его тоже можно сохранить где нибудь и использовать повторно, если хотите создать нового пользователя или для перевода монет. Не забудьте только пароль - 123456 ))).</p>
<p>После окончании работы, не забудьте остановить контейнер.</p>
<pre><code>./run.sh stop
</code></pre>
<h2>Ключи</h2>
<p>Я лично храню wallet.json, так как им удобно пользоваться, если надо перекинуть монеты с аккаунта на аккаунт и тому подобных дел. Так же я сохранил выхлоп list_keys команды у себя в гугл док. Там мешанина из открытых и приватных ключей. Как узнать, какой ключ, что? Для этого к примеру идем на <a href="http://golosd.com/@tolstoy" rel="noopener">http://golosd.com/@tolstoy</a>. Прокручиваем до окошка &quot;Authorities&quot; и видим открытые ключи. Копируем постинг ключ и ищем его в списке ключей. Ниже находится приватный ключ.</p>
<p>Используя постинг ключ можно войти на сайт golos.io и писать комментарий, посты и апвотить. Чтобы сделать перевод потребуется активный ключ.</p>
<h2>Ошибки</h2>
<p>Бывает, что кошелек вылетает с такой ошибкой</p>
<pre><code>10 assert_exception: Assert Exception
!ec: websocket send failed: invalid state
    {&quot;msg&quot;:&quot;invalid state&quot;}
    th_a  websocket.cpp:164 send_message
</code></pre>
<p>Видимо иногда пропадает соединение с нодой и не восстанавливается. Помогает закрыть кошелек (Ctrl-C и Enter) и снова запустить.</p>

<hr>
По материалам <a href="https://golos.io/golos/@ropox/kak-sozdat-novogo-polzovatelya-golosa-ispolzuya-cliwallet">статьи</a>. 
Автор <a href="https://golos.io/@ropox">@ropox</a>
<hr>
