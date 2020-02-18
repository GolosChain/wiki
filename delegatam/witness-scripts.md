# Скрипты для делегатов

## Скрипты обновления price feed

На данной странице аггрегируются скрипты для делегатов для публикации ценового фида GBG/GOLOS. Зачем нужен ценовой фид и какова в этом роль делегатов, читайте в статье [Что такое GBG](https://github.com/golos-blockchain/wiki/tree/3dbb2f2a042981607f6ae50283ab37510a67a70d/what-is-gbg.md).

Не существует какого-либо единого одобренного скрипта. Делегаты вольны устанавливать курс цены исходя из своего понимания ценообразования на золото.

## Скрипты, выложенные на github

### golos-witness-tools \(@vvk\)

* [Проект на github](https://github.com/bitfag/golos-witness-tools/blob/master/PRICEFEED.md)
* [Анонс](https://golos.io/golostools/@vvk/anons-novogo-skripta-obnovleniya-price-feed-i-proekta-golos-witness-tools)
* Не требует наличия _cli\_wallet_
* Написан на python
* Удобен для запуска из cron
* Возможность работы в виде docker-контейнера,

Для получения цены золота используется goldprice.org, а если он недоступен, то цена берётся с Центробанка РФ. Для получения цен USD/BTC и BTC/GOLOS используется coinmarketcap, а если он недоступен, то данные берутся с бирж.

### golospricefeed \(@roelandp\)

[Проект на github](https://github.com/roelandp/golospricefeed)

Скрипт на python, требует запущенного cli\_wallet.

Цену золота берёт с goldprice.org, прочие курсы - с нескольких различных бирж.

### Golos Feed JS \(@someguy123\)

[Проект на github](https://github.com/Someguy123/golosfeed-js).

Написан на Node.JS, предусмотрен запуск в docker-контиейнере. Используется форкнутая версия GolosJS-Lib.

Курс золота берётся с forexpf.ru, курсы GOLOS, BTC - с различных бирж.

## Скрипты без отдельного SCM-репозитория

### witness-php-publish-feed \(@on1x\)

[Анонс](https://golos.io/goldvoice/@on1x/witness-php-publish-feed)

Скрипт на php.

Курс золота берётся от Центробанка РФ, курсы GOLOS, BTC - с coinmarketcap.org.

### Скрипт @primus

[Анонс](https://golos.io/ru--golos/@primus/onlain-monitoring-kraudseila-skolko-golosov-vy-mozhete-kupit-za-1000-rublei-ili-1-btc-plyus-avtomatizaciya-prais-fidov-dlya).

Скрипт на perl, берёт готовые данные из внешнего источника, требует запущенного в фоне cli\_wallet.

### Скрипт @on0tole

[Анонс](https://golos.io/ru--delegaty/@on0tole/predstavlyayu-skript-dlya-avtomaticheskogo-obnovleniya-price-feed-dlya-delegatov) [Обновлённая версия by @litvintech](https://golos.io/ru--delegaty/@litvintech/obnovlennyi-skript-dlya-avtomaticheskoi-postavki-cenovykh-fidov-dlya-delegatov)

Скрипт на bash, требует запущенного cli\_wallet. **Скрипт явно устарел.**

### Скрипт @xtar

[Анонс](https://golos.io/ru--delegaty/@xtar/bash-skript-dlya-avtomaticheskoi-publikacii-fida)

bash, требует запущенного cli\_wallet. Берёт готовые данные с внешнего источника, самостоятельное вычисление цены отсутствует.

