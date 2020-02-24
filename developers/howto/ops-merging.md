---
description: ...
---

# Как объединять операции в одну транзакцию

Автор: [@vik](https://golos.id/@vik)

Операции голосования своими 100+ аккаунтами, упакованные в ОДНУ транзакцию:

* **Экономит время \(100+ операций одновременно\)**
* **Экономит bandwidth \(новореги смогут совершать в**`X`**раз больше транзакций\)**
* **Экономит ресурсы среды скрипта \(браузер, сервер, приложение\)**

Для начала выбираем 1 свой основной аккаунт-ботовод и прописываем его логин своим ботам в постинг-авторити. Это можно сделать массово с помощью скрипта:  
[https://github.com/vikxx/bots/blob/master/sample/setBotMaster.js](https://github.com/vikxx/bots/blob/master/sample/setBotMaster.js)

Нужно заполнить массив

```text
const GolosBots = [
["логин бота1",["posting","5**************ACTIVE WIF****"]],
["логин бота 3",["posting","5*****************"]],

// бесконечный список аккаунтов...

["логин последнего бота",["posting","5******************"]]

]
```

В переменной прописываем аккаунты и ключи. Заполнять posting не обязательно, требуется только активный ключ._\(Я храню оба ключа для удобства, так как разные скрипты используя массовые действия смотрят список аккаунтов и обращаются к тому или иному ключу\)_

В переменную`const BOTMASTER = "логин ботовода"`записываем логин аккаунта, который будет иметь права на постинг.

![](https://images.golos.io/DQmTJgMMvw4nkmxtVrFdrxdZXKUNbGQYUdckWUh6gshCCMA/image.png)

После выполнения скрипта он запишется в постинг авторизацию

![](https://images.golos.io/DQmXx3zZjuKyG1sWTsciMG1GJyQkhTLMJPjV3UJwFXtFtYr/image.png)

Операция выше делается единожды и дальше вы можете используя только ОДИН постинг ключ своего аккаунта-ботовода подписать апвоты ВСЕХ ботов.

Пример на JS:  
[https://github.com/vikxx/bots/blob/master/sample/botVotes.js](https://github.com/vikxx/bots/blob/master/sample/botVotes.js)

Прописывать ключи всем аккаунтам не нужно, достаточно перечислить логины:

```text
const GolosBots = [
"vikx",
"vikxx",
// Endless list...
"vikxxx"
]
```

И для каждого логина сделаем свою операцию, записав их все в одну переменную **votes**

```text
for(let botname of GolosBots){

votes.push(["vote",
{
"voter":botname,
"author":author,
"weight":10000,
"permlink":permlink
}])

}
```

Затем сформируем одну транзакцию, добавим в нее все операции votes и подпишем все операции постинг ключем аккаунта-ботовода

```text
const unsignedTX = {
    'expiration': expire,    
    'extensions': [],
    'operations': votes,
    'ref_block_num': 60419,
    'ref_block_prefix': 2937707173               
   }

let signedTX = null

try {
    signedTX = golos.auth.signTransaction(unsignedTX,{"posting":wif})
}
catch (error) 
golos.api.broadcastTransactionSynchronous(signedTX)
```

Скрипт полностью здесь:  
[https://github.com/vikxx/bots/blob/master/sample/botVotes.js](https://github.com/vikxx/bots/blob/master/sample/botVotes.js)

По материалам [статьи](https://golos.id/ru--golos/@vik/ekonomim-resurs-akkaunta-i-servera-sovmeshaya-100-operacii-v-odnoi-tranzakcii)

