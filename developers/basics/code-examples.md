# Примеры кода

Начинающим разработчикам всегда рекомендуется прочесть документации по той или иной библиотеке. Это помогает как понять работу библиотеки, так и запомнить возможности, которые можно использовать при разработке приложений. В данном разделе описаны наиболее популярные запросы в виде примеров. Наиболее используемая библиотека для приложений Golos — [golos-js](https://github.com/golos-blockchain/golos-js), поэтому примеры с её использованием.

Подробная документация на английском с указанием всех методов и их аттрибутов [доступно по ссылке](https://github.com/golos-blockchain/golos-js/tree/master/doc).

### Подключение библиотеки

В зависимости от серверного \(nodejs\) или браузерного \(js\) использования библиотеку нужно подключать разными способами.

Для nodejs актуальной инструкцией будет установка библиотеки через `npm install golos-classic-js --save` и подключением её в js файле через `var golos = require('golos-classic-js');`.

Для js подключения можно либо самому собрать webpack библиотеки через консоль `npm build`, либо воспользоваться уже собранной библиотекой от [jsDelivr CND](https://cdn.jsdelivr.net/npm/golos-classic-js@latest/dist/golos.min.js) или [Unpkg CDN](https://unpkg.com/golos-classic-js@latest/dist/golos.min.js). Просто добавьте к файлу script и укажите url библиотеки: `<script type="text/javascript" src="https://unpkg.com/golos-classic-js@latest/dist/golos.min.js"></script>`, после чего у вас будет доступ через консоль к глобальной переменной `golos`.

### Использование публичной ноды

Пока у вашего приложения нет большого потока пользователей, разумно использовать публичные API-ноды, список от делегатов [здесь](https://golos.id/nodes).

В нём указаны адреса для JSON-RPC запросов через WebSocket over SSL, для запросов через HTTPS просто замените `wss` на `https`и уберите `ws` в конце.

Пример настройки для работы с нодой `https://api.golos.blckchnd.com/`:

```javascript
var api_gate='https://api.golos.blckchnd.com/';
golos.config.set('websocket',api_gate);
```

### API-запросы

В разделе [Плагины и их API](plugins-api.md) были перечислены основные плагины и запросы к ним — все они доступны в библиотеке. Для того, чтобы выполнить тот или иной запрос, достаточно перевести его название в [CamelCase](https://ru.wikipedia.org/wiki/CamelCase).

Например, если вы решили выполнить запрос get\_database\_info к плагину database\_api, то вам необходимо выполнить код:

```javascript
golos.api.getDatabaseInfo(function(err,response){
    if(!err){
        //получен ответ
        console.log(response);
    }
    else{
        //ошибка
        console.log(err);
    }
});
```

В случае, если запрос требует входных данных, то вы добавляете их в начало вызова.

### Транслирование транзакций \(broadcast\)

Для каждой операции из протокола Golos существует отдельный метод в библиотеке golos-js, который принимает приватный ключ \(для подписи транзакции\) и параметры операции. Название операции, аналогично API методам, должно быть переведено в [формат CamelCase](https://ru.wikipedia.org/wiki/CamelCase). Пример кода для трансляции \(broadcast\) операции `account_metadata` \(запись в блокчейн мета-данных аккаунта\):

```javascript
var posting_key='5K...';//приватный ключ
var user_login='test';//логин аккаунта
var metadata={'name':'Тестовый аккаунт','photo':'https://cdn.pixabay.com/photo.jpg'};
golos.broadcast.accountMetadata(posting_key,user_login,JSON.stringify(metadata),function(err,result){
    if(!err){
        //транзакция принята публичной нодой
        console.log(result);
    }
    else{
        //нода не приняла транзакцию
        console.log(err);
    }
});
```

### Динамические глобальные свойства сети

Часть новичков хотят периодически опрашивать ноду и получать актуальные данные о DGP \(Dynamic Global Properties\), чтобы на основе этого показывать новые блоки, исполнять условия по необратимому блоку или подсвечивать в списке делегатов последнего, кто подписал блок. Для этого достаточно запрашивать данные по таймеру каждые 3 секунды \(время между блоками\):

```javascript
var dgp={}
function update_dgp(auto=false){
    golos.api.getDynamicGlobalProperties(function(err,response){
        if(!err){
            dgp=response;
        }
    });
    if(auto){
        setTimeout("update_dgp(true)",3000);
    }
}
update_dgp(true);
```

### Работа с ключами

Криптографические ключи представляют собой координаты X и Y которые записаны в общепринятом формате DER на эллептической кривой secp256k1 \(в качестве хеш-функции служит SHA-256\). В библиотеке golos-js преобразования и работа с ключами относятся к модулю `golos.auth`.

В Graphene экосистеме придумали механизм человекочитаемых паролей. По причинам перебора использовать их не рекомендуется, поэтому, чтобы усложнить множественный перебор приватных ключей к публичному пришли к определенным правилам формирования ключей в виде конкатенации строк: логин аккаунта, пароль \(сложный\), тип доступа.

Часть приложений условились использовать эти правила, таким образом упрощая доступ пользователем к разным возможностям аккаунта по общему паролю. Например, пользователь test зарегистрирован используя общий пароль PK3452JENDK332. При авторизации в приложении используя эти логин и пароль, приложение может самостоятельно сформировать ключи нужного типа доступа, просто используя конкатенацию строк. Пользователь хочет перевести токены? Приложение формирует налету приватный активный ключ по строке testPK3452JENDK332active. Пользователь награждает кого-то? Приложение формирует приватный постинг ключ по строке testPK3452JENDK332posting. Это упрощает доступ для пользователя по общему паролю, но лишает гибкости и подвергает опасности аккаунт. Типы доступа имеют разные полномочия и при компрометации доверенного окружения доверенного окружения пользователя или сайта доступ к аккаунту может быть перехвачен.

### Регистрация аккаунта

Обычно, при регистрации пользователя, приложение генерирует пароль самостоятельно. Но бывает исключения, когда приложение позволяет использовать свой пароль для генерации ключей. Библиотека позволяет самостоятельно указать строки для генерации ключа в методе `golos.auth.toWif(account_login,general_pass,auth_type);`. В примере ниже представлена функция для генерации случайного пароля заданной длины и генерации ключа по нему без привязки к пользователю и типу доступа:

```javascript
function pass_gen(length=100,to_wif=true){
    let charset='abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789+-=_:;.,@!^&*$';
    let ret='';
    for (var i=0,n=charset.length;i<length;++i){
        ret+=charset.charAt(Math.floor(Math.random()*n));
    }
    if(!to_wif){
        return ret;
    }
    let wif=golos.auth.toWif('',ret,'');
    return wif;
}
```

Получить публичный ключ по заданному приватному можно методом `golos.auth.wifToPublic(wif)`. Для тех приложений, которые хотят формировать ключи по кокатенации логина, пароля и типа доступа, существует метод `golos.auth.getPrivateKeys(account_login,general_pass,auth_types);`. Метод возвращает массив по шаблону result._type_ для приватных ключей \(которые нужно передать пользователю\) и result.\_type\_Pubkey для публичных ключей \(которые нужно транслировать в блокчейн для сохранения за аккаунтом пользователя\).

### Конвертация токенов GOLOS в долю сети

Для автоматической конвертации всех доступных токенов GOLOS в долю сети VESTS необходимо запросить информацию об аккаунте и конвертировать их себе же в долю операцией transfer\_to\_vesting:

```javascript
var current_user='test';
var active_key='5K...';//приватный активный ключ
golos.api.getAccounts([current_user],function(err,response){
    if(!err){
        //получен ответ
        if(typeof response[0] !== 'undefined'){
            if('0.000 GOLOS'!=response[0].balance){
                golos.broadcast.transferToVesting(active_key,current_user,current_user,response[0].balance,function(err,result){
                    if(!err){
                        console.log('конвертация в долю сети',response[0].balance);
                        console.log(result);
                    }
                    else{
                        console.log(err);
                    }
                });
            }
            else{
                console.log('баланс на нуле');
            }
        }
        else{
            console.log('аккаунт не найден',current_user);
        }
    }
    else{
        //ошибка
        console.log(err);
    }
});
```

### Перевод токенов

Пример перевода 1.000 GOLOS из баланса аккаунта в фонд воркеров:

```javascript
var current_user='test';
var active_key='5K...';//приватный активный ключ
var target='workers';
var fixed_amount='1.000 GOLOS';
var memo='Заметка';//utf-8 включая emoji
golos.broadcast.transfer(active_key,current_user,target,fixed_amount,memo,function(err,result){
    if(!err){
        //получен ответ
        console.log(result);
    }
    else{
        //ошибка
        console.log(err);
    }
});
```

### Смена ключей аккаунта

Бывает, что есть необходимость сменить доступы у аккаунта. Это может быть по причине добавления нового ключа, делегирование управления или создание условий для мульти-подписного управления аккаунтом \(когда для выполнения операций требуется подпись несколькими ключами\).

Полномочия для выполнения операций имеют следующую структуру:

* _weight\_threshold_ — необходимый вес для одобрения транзакции с операциями нужного типа;
* _account\_auths_ — массив аккаунтов и их весов. Аккаунты могут добавить вес транзакции при добавлении к ней подписи ключом;
* _key\_auths_ — массив публичных ключей и их весов.

Система проверяет транзакцию, операции в ней, проверяет наличие подписей задейственных аккаунтов и достаточным ли весом они обладают, для совершения действия положенного типа.

Пример полномочий с одним ключом:

```javascript
{
    "weight_threshold": 1,
    "account_auths": [],
    "key_auths": [
        ["GLS6LWhhUzKmYM5VcxtC2FpEjzr71imfb7DeGA9yodeqnvtP2SYjA", 1]
    ]
}
```

Если у аккаунта будет записаны эти полномочия в тип доступа posting, то для совершения операции награждения блокчейн будет требовать подпись транзакции ключом `5KRLZitDd5c9uZzDgTMF4se4eVewENtZ29GbCuKwbT3msRbtLgi` \(которому соответствует указанный в полномочиях публичный ключ `GLS6LWhhUzKmYM5VcxtC2FpEjzr71imfb7DeGA9yodeqnvtP2SYjA`\).

При делегировании управления другому аккаунту, например `test`, необходимо изменить полномочия на:

```javascript
{
    "weight_threshold": 1,
    "account_auths": [
        ["test", 1]
    ],
    "key_auths": [
        ["GLS6LWhhUzKmYM5VcxtC2FpEjzr71imfb7DeGA9yodeqnvtP2SYjA", 1]
    ]
}
```

После этого блокчейн будет требовать подпись либо указанным ключом, либо ключом аналогичного типа доступа аккаунта `test`.

Мульти-подписное управление предполагает усложнение полномочий, например для управления 2 из 3 можно использовать полномочие:

```javascript
{
    "weight_threshold": 4,
    "account_auths": [],
    "key_auths": [
        ["GLS6LWhhUzKmYM5VcxtC2FpEjzr71imfb7DeGA9yodeqnvtP2SYjA", 2],
        ["GLS5mK1zLnYHy7PbnsxRpS4NbKjEoH2J9eBmgSjVKJ5BKQpLLj9T4", 2],
        ["GLS4uiqeDPsoteSFVbTWPBUbmfzxYkJyXYmA6B1pAFWZ59n4iBuUK", 2]
    ]
}
```

Для того чтобы транзакция была принята блокчейном, необходимо добавить подписи как минимум 2 из 3 указанных ключей. В этом примере публичному ключу `GLS4uiqeDPsoteSFVbTWPBUbmfzxYkJyXYmA6B1pAFWZ59n4iBuUK` соответствует приватный ключ `5KMBKopgd56MZvV8FYhp5AP7AWFyLKiybqRnZYgjXukw34VRE78`.

### Передача права голосования прокси \(proxy\)

Если пользователь не принимает участие в выборе делегатов, он может делегировать право распоряжаться его долей другому аккаунту. Для этого существует операция `account_witness_proxy`, ей может воспользоваться приложение регистратор, чтобы не загружать своего пользователя лишней информацией об устройстве блокчейн-системы.

Если пользователь решит самостоятельно участвовать в выборе делегатов, то первый же голос за делегата отменит прокси.

```javascript
var account_login='test';
var active_key='5K...';
var proxy_login='proxy';
golos.broadcast.accountWitnessProxy(active_key,account_login,proxy_login,function(err,result){
    if(!err){
        console.log(result);
    }
    else{
        console.log(err);
    }
});
```

### Custom операции

Когда разработчикам нужно ввести свою структуру в блокчейн, сделать децентрализированное приложение \(dApp\), которое будет мониторить блоки и учитывать операции в сети — они могут использовать custom операции. Custom операция имеет гибкую структуру:

Разработчики могут сами придумать структуры данных, протокол команд и их учет через custom операции.

### Трехсторонние Escrow сделки

Трехсторонние сделки работают по принципу проверки выполнения условий гарантом \(`agent`\). Получатель и гарант должны подтвердить начало сделки операцией `escrow_approve` \(гарант получает комиссию на этом этапе\), иначе при достижении даты дедлайна ратификации \(`ratification_deadline`\) происходит возврат всех токенов инициатору сделки.

Если наступает момент спора, то отправитель или получатель могут инициировать разбирательство операцией `escrow_dispute`, после чего принятие решения по сделке передается гаранту \(именно он решает кто и сколько токенов получит\). Если сделка повисла и долгое время не разрешается — договор истекает \(`escrow_expiration`\) и всеми токенами управляет либо агент \(если был открыт диспут\), либо любая из сторон сделки.

Создание escrow перевода:

```javascript
var account_login='test';
var active_key='5K...';
var receiver_login='receiver';
var token_amount='100.000 GOLOS';//количество передаваемых токенов
var escrow_id=1;//номер escrow назначается вручную для согласования заявки (uint32)
var agent_login='agent';
var fee='10.000 GOLOS';//комиссия гаранта
var json_metadata='{}';//дополнительные мета-данные в формате json
var ratification_deadline=new Date().toISOString().substr(0,19);//дата принудительной окончания сделки и возврата средств если гарант и получатель не подтвердили участие в сделке (дедлайн в формате ISO вида 2019-10-17T13:30:18)
var escrow_expiration=new Date().toISOString().substr(0,19);//дата окончания принятия решения по сделке, после чего невозможно инициировать спор (дедлайн в формате ISO вида 2019-10-17T13:30:18)
golos.broadcast.escrowTransfer(active_key,account_login,receiver_login,token_amount,escrow_id,agent_login,fee,json_metadata,ratification_deadline,escrow_expiration,function(err,result){
    if(!err){
        console.log(result);
    }
    else{
        console.log(err);
    }
});
```

Подтверждение участие в сделке по предложенным условиям \(свое участие должны подтвердить гарант и получатель операцией `escrow_approve`\):

```javascript
var account_login='test';
var receiver_login='receiver';
var agent_login='agent';
var escrow_id=1;//номер escrow назначается вручную для согласования заявки (uint32)

var who=agent_login;//кто подтверждает свое участие
var active_key='5K...';//ключ подтверждающий стороны (who)

var approve=true;//false в случае отказа от участия в сделке
golos.broadcast.escrowApprove(active_key,account_login,receiver_login,agent_login,who,escrow_id,approve,function(err,result){
    if(!err){
        console.log(result);
    }
    else{
        console.log(err);
    }
});
```

Требование диспута \(открыть спор по сделке может отправитель или получатель операцией `escrow_dispute`\):

```javascript
var account_login='test';
var receiver_login='receiver';
var agent_login='agent';
var escrow_id=1;//номер escrow назначается вручную для согласования заявки (uint32)


var who=receiver_login;//кто подтверждает свое участие
var active_key='5K...';//ключ подтверждающий стороны (who)

golos.broadcast.escrowDispute(active_key,account_login,receiver_login,agent_login,who,escrow_id,function(err,result){
    if(!err){
        console.log(result);
    }
    else{
        console.log(err);
    }
});
```

Отпустить средства \(операция `escrow_release`\):

```javascript
var account_login='test';
var receiver_login='receiver';
var agent_login='agent';
var escrow_id=1;//номер escrow назначается вручную для согласования заявки (uint32)
var token_amount='100.000 VIZ';//количество передаваемых токенов

var who=receiver_login;//кто решил отпустить средства (если открыт диспут, то только гарант решает кому и сколько перевести)
var active_key='5K...';//ключ инициатора операции (who)
var receiver=account_login;//получателем средств может быть только другая сторона сделки до истечения срока сделки, иначе — любая из сторон сделки

golos.broadcast.escrowRelease(active_key,account_login,receiver_login,agent_login,who,receiver,escrow_id,token_amount,function(err,result){
    if(!err){
        console.log(result);
    }
    else{
        console.log(err);
    }
});
```

Чтобы получить информацию об escrow, необходимо вызвать API запрос `get_escrow` плагина `database_api`:

```javascript
var account_login='test';
var escrow_id=1;
golos.api.getEscrow(account_login,escrow_id,function(err,response){
    if(!err){
        //получен ответ
        console.log(response);
    }
    else{
        //ошибка
        console.log(err);
    }
});
```

### Система предложенных операций \(proposal\)

Для управления системой предложенных операций существует 3 операции: создания предложения, предоставление подписи \(обновление\), удаление предложения. После создания предложения блокчейн система будет ожидать все необходимые подписи для осуществления операций заложенных внутрь предложения, после чего выполнит их. Если подошел срок истечения, то предложение не будет выполнено. Системой предложенных операций пользуются для управления мультиподписными аккаунтами. Для создания предложения используйте операцию `proposal_create`:

```javascript
var account_login='test';
var active_key='5K...';//активный приватный ключ
var title='payments-14';//наименование предложения (играет роль идентификатора, должно быть уникальным)
var memo='Платежи по договору №14';

var expiration_date=new Date();//дата экспирации, когда предложенные операции будут отклонены или выполнены при получении всех необходимых подписей
expiration_date.setDate(expiration_date.getDate() + 10);//плюс десять дней от текущего момента
var expiration_time=expiration_date.toISOString().substr(0,19);//дата истечения предложения в формате ISO
console.log('expiration_time',expiration_time);

var proposed_operations=[];
proposed_operations.push({'op':['transfer',{'from':'escrow','to':'test','amount':'10.000 GOLOS','memo':memo}]});
proposed_operations.push({'op':['transfer',{'from':'escrow2','to':'test','amount':'10.000 GOLOS','memo':memo}]});

var review_period_date=new Date(expiration_date.getTime() - 10);//дата прекращения приема подписей (минус 10 секунд до даты экспирации)
var review_period_time=review_period_date.toISOString().substr(0,19);//дата истечения предложения в формате ISO
console.log('review_period_time',review_period_time);

var extensions=[];

golos.broadcast.proposalCreate(active_key,account_login,title,memo,expiration_time,proposed_operations,review_period_time,extensions,function(err,result){
    console.log(err,result);
});
```

Чтобы получить информацию о предложениях сделанных пользователем, нужно выполнить API запрос `get_proposed_transactions` к плагину `database_api` \(будет возвращен массив предложений\):

```javascript
var looking_account='test';
var from=0;
var limit=100;
golos.api.getProposedTransactions(looking_account,from,limit,function(err,response){
    console.log(err,response);
});
```

Система предложений поддерживает все существующие операции в блокчейне, но не позволяет смешивать операции требующие разных полномочий \(например, операции `tranfer`, требующие active полномочия, и операции `upvote`, требующие posting полномочия\). Предоставить подпись нужного типа доступа можно операцией `proposal_update` указав логин подписавшего транзакцию в массиве на добавление или удаления из списка подтвердивших предложение \(для этого есть 4 типа массивов: active, owner, posting и key для использования одиночных ключей\). Как только предоставлены все необходимые подписи — операции из предложения будут исполнены \(при условии, что не указан период `review_period_time`\), в случае ошибки выполнения повторная попытка будет предпринята при экспирации. Пример:

```javascript
var account_login='escrow';
var active_key='5K...';//активный приватный ключ

var proposal_author='test';//автор предложения
var proposal_title='payments-14';//идентификатор предложения

var active_approvals_to_add=[];//список аккаунтов подписавших данную транзакцию активным типом доступа для подтверждения предложения
var active_approvals_to_remove=[];//список аккаунтов для удаления из списка подтвердивших предложение
var owner_approvals_to_add=[];
var owner_approvals_to_remove=[];
var posting_approvals_to_add=[];
var posting_approvals_to_remove=[];
var key_approvals_to_add=[];
var key_approvals_to_remove=[];
var extensions=[];

active_approvals_to_add.push(account_login);

golos.broadcast.proposalUpdate(active_key,proposal_author,proposal_title,active_approvals_to_add,active_approvals_to_remove,master_approvals_to_add,master_approvals_to_remove,regular_approvals_to_add,regular_approvals_to_remove,key_approvals_to_add,key_approvals_to_remove,extensions,function(err,result){
    console.log(err,result);
});
```

Предложение может удалить заявитель или любой участник, чья подпись требуется. Для этого достаточно выполнить операцию `proposal_delete` подписав её активным ключом:

```javascript
var account_login='escrow2';//участник предложения
var active_key='5K...';//активный приватный ключ

var proposal_author='test';//автор предложения
var proposal_title='payments-14';//идентификатор предложения

var extensions=[];

golos.broadcast.proposalDelete(active_key,proposal_author,proposal_title,account_login,extensions,function(err,result){
        console.log(err,result);
});
```

## js запросы к публичной ноде без библиотеки

Если вашему приложению не требуется криптография и подпись транзакций, то вы можете использовать нативные средства для json-rpc запросов через js.

Пример для WebSocket соединения:

```javascript
var api_gate='wss://golos.lexa.host/ws';
var latency_start=new Date().getTime();
var latency=-1;
var socket = new WebSocket(api_gate);
socket.onmessage=function(event){
    latency=new Date().getTime() - latency_start;
    let json=JSON.parse(event.data);
    if(json.result){
        console.log(json.result);
    }
    else{
        console.log(json.error);
    }
    socket.close();
}
socket.onopen=function(){
    socket.send('{"id":1,"method":"call","jsonrpc":"2.0","params":["database_api","get_dynamic_global_properties",[]]}');
};
```

Пример для HTTP соединения:

```javascript
var api_gate='https://golos.lexa.host/';
var latency_start=new Date().getTime();
var latency=-1;
var xhr = new XMLHttpRequest();
xhr.overrideMimeType('text/plain');
xhr.open('POST',api_gate);
xhr.setRequestHeader('accept','application/json, text/plain, */*');
xhr.setRequestHeader('content-type','application/json');
xhr.onreadystatechange=function(){
    if(4==xhr.readyState && 200==xhr.status){
        latency=new Date().getTime() - latency_start;
        console.log(xhr);
        let json=JSON.parse(xhr.response);
        if(json.result){
            console.log(json.result);
        }
        else{
            console.log(json.error);
        }
    }
}
xhr.send('{"id":1,"method":"call","jsonrpc":"2.0","params":["database_api","get_dynamic_global_properties",[]]}');
```

