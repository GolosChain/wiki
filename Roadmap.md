# План Тестирования

### Тестнет А (приватный)
Дата: 14 сентября - 19 сентября

Инфраструктура: 7 машин на Digital Ocean.

Тестовая группа: cyber.fund (6 человек)

Сценарий Тестирования:

1. Хардфорки:
Применено 13 хардфорков - Проверить логику после хардфорков
Накатывание 14 хардфорка -
2. Проверить
создание аккаунтов
старт сети
присоединение витнесов
3. протестировать 3 временных параметра выплат:
STEEMIT_CASHOUT_WINDOW_SECONDS_PRE_HF12 (60*60*24)  /// 1 daу - 30 мин
STEEMIT_SECOND_CASHOUT_WINDOW           (60*60*24*30) /// 30 days - 1 час
STEEMIT_MAX_CASHOUT_WINDOW_SECONDS      (60*60*24*14) /// 2 weeks - 2 часа
проверить выплату кураторам
4. Комиссия за создание аккаунта (влияние витнесов на этот показатель? @21xhipster
5. Восстановление аккаунтов. Протестировать параметры:
STEEMIT_OWNER_AUTH_RECOVERY_PERIOD                  fc::days(30) // - сек
STEEMIT_ACCOUNT_RECOVERY_REQUEST_EXPIRATION_PERIOD  fc::days(1) - сек
STEEMIT_OWNER_UPDATE_LIMIT                          fc::minutes(60) - сек
6. Проверка вестинга (до и после старта)
7. STEEMIT_START_MINER_VOTING_BLOCK (STEEMIT_BLOCKS_PER_DAY * 30)????
8. Проверить попадание витнеса в очередь в результате голосования
9. Проверить формирование пула витнес+майнер+случайный на раунд
10. Тестирование STEEMIT_HARDFORK_REQUIRED_WITNESSES
11. Вывод из power
12. STEEMIT_MAX_WITHDRAW_ROUTES 10???
13. STEEMIT_MAX_VOTE_CHANGES 5???
14. STEEMIT_UPVOTE_LOCKOUT                  (fc::minutes(1))
15. STEEMIT_REVERSE_AUCTION_WINDOW_SECONDS  (60*30) /// 30 minutes
16. STEEMIT_MIN_VOTE_INTERVAL_SEC           3
17. Тестирование изменения процентной ставки
18. Протестировать параметры ликвидности
19. STEEMIT_APR_PERCENT_MULTIPLY_PER_BLOCK ???
20. Параметры суплая???????!!!!
21. Работа витнесов (поставка данных)
22. STEEMIT_BLOCKCHAIN_PRECISION????
23. Протестировать распределение денег на аккаунты генезиса

### Тестнет В (публичный)
Дата: 19 сентября - 26\27 сентября

Тестовая группа: cyber.fund + группа энтузиастов и хакеров

### Продакшен - Тестнет С (недельный)
Дата: 27 сентября - 3 октября

В течении недели после запуска клеймится возможность перезапуска сети.

# Подготовка к запуску

## 1. Планирование
- [x] Roadmap @21xhipster
- [x] Команда @21xhipster
- [x] Домены @lomashuk
- [ ] Соглашение Steemit Inc @lomashuk  
- [ ] Анонсы @21xhipster

## 2. Адаптация веб-клиента @Undeadlol1 20 сентября
- [x] Функционал по локализации @Undeadlol1
- [ ] Интернационализация цен и валют @Undeadlol1
- [ ] Русификация тэгов @Undeadlol1
- [ ] Настройка Segment.com (GA, ЯМ) @Undeadlol1
- [x] Локализация @Undeadlol1 @21xhipster
- [x] Конфиг веб-клиента @Undeadlol1 @21xhipster
- [ ] Google Custom Search @tomarcafe
- [ ] Логинация через VK, FB, Reddit @tomarcafe
- [ ] Индексация Яндексом @tomarcafe @ValeryLitvin @Undeadlol1

## 3. Подготовка ICO @lomashuk 20 сентября
- [x] ! Бумага @21xhipster
- [x] ! Лого @lomashuk
- [x] Оригинальный код @lomashuk
- [ ] Перевод ролика @lomashuk
- [x] Перевод контента @mguryeva
- [x] Настройка Segment.com (GA, ЯМ) @Undeadlol1
- [x] Макет Лендинг @lomashuk
- [ ] Отрисовать Макет @lomashuk
- [ ] Реализация Лендинг @Undeadlol1
- [x] Получение адресов Blockcypher и запись их в блокчейн @tomarcafe
- [ ] Скрипт по распределению краудсейла @ValeryLitvin
- [x] Мультисиг Bitcoin @vitalylvov
- [ ] ! Мультисиг RSteem @ValeryLitvin

## 4. Подготовка блокчейна @ValeryLitvin
- [x] ! Тестовая сеть @ValeryLitvin
- [x] Snapshot блокчейна Steem
- [ ] ! Корректировка эмиссии @ValeryLitvin @21xhipster @vitalylvov
- [x] Отпиливание хардфорков @ValeryLitvin
- [ ] Sharedrop аккаунтов Steem @ValeryLitvin
- [ ] Сценарий тестирования @ValeryLitvin
- [ ] Скрипт по распределению шеардропа @ValeryLitvin
- [ ] Возможность увеличить дробность денежной единицы
- [ ] Развернуть существующий эксплорер @ValeryLitvin
- [ ] Steemd.com support @lomashuk
- [ ] Адаптация скрипта с ценами @tomarcafe
- [ ] Тестовая группа @21xhipster @ValeryLitvin
- [ ] Code Review by Dan

## 5. Инфраструктура для веб-клиента @tomarcafe 20 сентября
- [ ] Балансировщик @tomarcafe
- [ ] Memcached @tomarcafe
- [ ] Контейенера для нод. Docker. @ValeryLitvin (-> backlog)
- [ ] Wallarm @ValeryLitvin
- [ ] New Relic @ValeryLitvin

## 6. Юридическая защита @lomashuk
- [ ] Disclaimer
- [ ] License Agreement

# Запуск
## -1. Таймлайн
## 0. Киев

## 1. Выбор делегатов @vitalylvov
- Скрипт по ценовым фидам
- Статья с требованиям @21xhipster
- Настройка регулярных отчетов @21xhipster

## 2. Регистрации (инвайты, проверки, продажи)


## 2. Bounty Hunting Program @ValeryLitvin
- Уязвимости клиента
- Уязвимости блокчейна
- Уязвимости инфраструктуры

## 3. Партнерства @lomashuk
- Группа Facebook
- Группа Vkontakte
- This is Horosho
- СуперБлоггер
- Ронни

## 4. Маркетинг
- [ ] Целевка Adwords @lomashuk

## 5. Публикация Статей
- [ ] Coinfox @lomashuk
- [ ] Forklog @ValeryLitvin
- [ ] Habrahabr @21xhipster @ValeryLitvin
- [ ] Lepra
- [ ] Vc.ru
- [ ] Hopes & Fears
- [ ] Rusbase @21xhipster

# Формирование сообщества
Результатам формирования сообщества будет первая выплата!

## Расширение команды
- [ ] C++ developer

## 1. Формирование рыночной цены @lomashuk
- [ ] Листинг на Битрекс
- [ ] Листинг на Полоникс

## 2. Развитие системы делегатов

## 3. Поддержка локализации образовательного контента

## 4. Поддержка #introduceyourself

## 5. Внедрение системы поощрения разработчиков
