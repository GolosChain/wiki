# 📢 Мгновенная регистрация аккаунтов на GOLOS и STEEM. Без верификации и ограничений!

Автор: [@vik](https://golos.io/@vik)

## Этот простой скрипт поможет вам зарегистрировать любое количество новых аккаунтов на голосе.

![](https://imgp.golos.io/0x0/https://i.imgur.com/BxJ5UNF.jpg)

## Браузерная версия

### Голос [golos.cf/reg.html](https://golos.cf/reg.html)

### Steemit [golos.cf/steem](https://golos.cf/steem)

![](https://imgp.golos.io/0x0/https://i.imgur.com/ZV6OXXu.jpg)

![](https://imgp.golos.io/0x0/https://i.imgur.com/2lbzERi.png)

---

## Node JS

Поставьте nodeJS  
Установите библиотеку golos-js

`npm install golos-js`

Скачайте файл accountregistartor.js

[https://github.com/vikxx/robot/blob/master/accountregistartor.js](https://github.com/vikxx/robot/blob/master/accountregistartor.js)

Заполните необходимые поля и запустите  
`node accountregistartor.js`

```
// Придумайте логин и пароль для нового аккаунта
const NAME = "nickname123"
const PASS = "MyStrongPass1234567890"


const golos = require('golos-js')
golos.config.set('websocket', "wss://api.golos.cf")

// Данные создателя аккаунта
// Активный ключ создателя (вашего существующего аккаунта)
const wif= "5ouriuruemACTIVEKEYkjnfjnvnjfnvjnfjnvfjnvnjdu"

// Стоимость создания аккаунта.
// Эти средства пойдут в силу голоса созданному пользователю.
//Вы можете их увеличить при желании.
const fee= "3.000 GOLOS"

// Логин создателя (вашего существующего аккаунта)
const creator= "robot"


// Профиль пользователя. О себе, аватар, и т.д. , можно оставить пустым и заполнить позднее
const jsonMetadata= {

}


let x = golos.auth.generateKeys(NAME, PASS, ['owner','active','posting','memo'])
const ownerAuth = {
    weight_threshold: 1,
    account_auths: [],
    key_auths: [[x.owner, 1]]
}
const activeAuth = {
    weight_threshold: 1,
    account_auths: [],
    key_auths: [[x.active, 1]]
}
const postingAuth = {
    weight_threshold: 1,
    account_auths: [],
    key_auths: [[x.posting, 1]]
}
const memoKey= x.memo

golos.broadcast.accountCreate(wif, fee, creator, NAME, 
ownerAuth, 
activeAuth, 
postingAuth, 
memoKey, 
jsonMetadata, 
(err, result) =
>
 {
  if(err) return console.log(err);
    console.log(result)
});
```

> По материалвм [статьи](https://golos.io/ru--golos/@vik/mgnovennaya-registraciya-akkauntov-na-golos-i-steem-bez-verifikacii-i-ogranichenii)
>
> Автор [@vik](https://golos.io/@vik)



