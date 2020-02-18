# Кэширование API

Автор: [@vik](https://golos.io/@vik)

**Суть решаемой проблемы:**    
![](https://imgp.golos.io/0x0/https://s17.postimg.org/byltwz7rh/image.png)

**Решение:**

В конце поста будет приведен простой код для node.js который позволит оптимизировать работу множества скриптов на одной ноде.

![](https://imgp.golos.io/0x0/https://s2.postimg.org/qf2uq63pz/image.png)

В последнее время моими [телеграм ботами для голосования](https://golos.io/ru--boty/@vik/vozobnovlenie-raboty-kuratorskikh-botov-dlya-telegram) пользуются все больше людей и естественно нагрузка возросла в 10-ки раз по сравнению даже с прошлым месяцем. Я поднял [дополнительные ноды](https://golos.io/ru--golos/@vik/etika-botovodstva-na-golose-i-ekonomiya-resursa-pablik-nod-robot-delegat-za-kotorogo-ne-nuzhno-golosovat), распределил нагрузки, добавил мощности серверам, но узкое место оказалось в самой пропускной способности нод.

Обратите внимание на скрин, там изредка мелькают строки белого цвета. Это череда новых блоков. Желтые же строки - это обращение к ноде.

![](https://imgp.golos.io/0x0/https://s21.postimg.org/5qtwcvmmt/blocks.gif)

Сейчас на сервере работают примерно 10 ботов, в каждом примерно 20 пользователей, это 200 практически одновременных запросов каждые 3 секунды 24/7. Такую нагрузку нода очень часто не выдерживает и боты пропускают транзакции или становятся на вынужденную паузу, а пользователи сердятся на несовершенство скриптов :\)

Так как каждый пользователь бота "опрашивает" новые блоки с интервалом в 3 секунды, то сделать работу комфортной можно либо устанавливая по ноде на каждых 20-30 человек, что экономически абсурдно, либо **пересмотреть принципы работы с опрашиванием блоков**.

Решение я выбрал такое:

Вместо сотен отдельных сессий пользователей работает **только один ОТДЕЛЬНЫЙ скрипт,** который опрашивает блоки. И сохраняет данные блока в базу данных Redis, все это происходит за доли миллисекунд, так как redis при правильной настройке не просто база, а мощный кэширующий инструмент.  
Далее, каждый из пользователей бота обращается не к ноде, а уже к redis, чтобы получить данные блока с операциями блокчейна. Таким образом, нагрузка должна упасть в 10-ки, а то и больше раз.

**Разработчикам и владельцам нод**

Суть метода простая, на сервере работает javascript который с интервалом в 3 секунды обновляет в базе данных содержимое свежего блока на golos.io  
Далее вместо того, чтобы подключаться всеми своими скриптами одновременно к одной ноде, вы берете данные о блоках из базы данных, а нода работает с минимальной нагрузкой, словно ее использует лишь один скрипт.

Вы можете использовать любую базу данных, как-то mongo, mysql, rethink и другие, для себя же я выбрал именно redis, для меня она проверенный инструмент, способный держать нагрузку во время кэширования большого объема данных.

![](https://imgp.golos.io/0x0/https://redislabs.com/wp-content/uploads/2014/04/redis_proven_performance_2.png)

Весь код для постоянного сохранения текущего блока в redis уместился в несколько десятков строк

![](https://imgp.golos.io/0x0/https://s13.postimg.org/tii4r8vad/code.jpg)

```text
require('events').EventEmitter.prototype._maxListeners = 100000
const Promise = require("bluebird")
const _ = require('lodash')
const golos = require("golos-js")
const redis = require("redis")
const client = redis.createClient()
golos.config.set('websocket','ws://localhost:9090')
let trig = {existBlock:true}
const dynamicSnap = new Promise((resolve, reject) =>
 {
    golos.api.getDynamicGlobalProperties((err, res) =>
 {
        if (err) {console.log(err)}
        else {
            resolve(res)
        }
    })
})
const FIRSTBLOCK = n =>
 n.head_block_number
const SENDBLOCK = currentblock =>
 {
    golos.api.getBlock(currentblock, (err, result) =>
 {
        if (err) {
            console.log(err) 
            }   else if (result === null){
                trig.existBlock = false
                }else {
        let JSONblock = JSON.stringify(result)
        client.hmset("GolosLastBlock", "data",JSONblock);
        console.log(JSONblock)
        trig.existBlock = true
            }
    })
}
const NEXTBLOCKS = firstblock =>
 {
    let currentblock = firstblock
    setInterval(() =>
 {
if(trig.existBlock){
currentblock++
}else{
console.warn(`Проблема блока ${currentblock}`)}
SENDBLOCK(currentblock)
}, 3000)
}
dynamicSnap
    .then(FIRSTBLOCK)
    .then(NEXTBLOCKS)
    .catch(e => console.log(e));
```

Далее вам достаточно брать данные блока из базы redis

```text
client.hgetall("GolosLastBlock", function(err, LB) {
let LastBlockRedis = JSON.parse(LB.data)
            filterOperations(LastBlockRedis)
}
```

Где предполагается, что функция filterOperations обрабатывается вашим скриптом.

> По материалам [статьи](https://golos.io/ru--golos/@vik/zapusk-mnozhestva-mnogopotochnykh-zhivykh-skriptov-na-odnoi-node-reshenie-dlya-mashtabiruemosti-botov-golosa).
>
> Автор [@vik](https://golos.io/@vik)

