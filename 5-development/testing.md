ЧТО ТАКОЕ TEST CASE, CHECK LIST, CHEET SHEET
Обычно сценарий состоит из следующих пунктов:
- Предусловие
- Описание последовательности действий пользователя
- Ожидаемый результат (то, как система должна себя вести)

Сценарий тестирования клиента:
Регистрация
---Через фейсбук
+++Через ВК
---Через Реддит
Постинг
Написать заголовок и тело поста
Добавить теги
Отметить: проголосовать за свой пост
Отметить: получить 100% в Силе Голоса
Апвоут пост
Показать проголосовавших
Отображение выплат
Отметить пост флагом
Фолловинг
Зафолловить автора
Отображение в ленте постов авторов
Репост
Репост поста
Отображение в ленте
Комментирование
Добавить комментарий
Апвоут комментария
Комментарий комментария
Репост в соцсети
Репост в твиттер
Репост в фейсбук
Кошелек
Перевод Голоса в Силу Голоса себе
Перевод Голоса в Силу Голоса другому
Пауэр даун
Перевод GBG	в Стимы
Разрешения
Показать ключи
Пароль
Смена пароля
Войти под новым паролем
Возврат украденного аккаунта
Поиск по сайту
Поиск по тегам
Русские
Английские
Голосование за делегатов
Покупка на ICO


Вторая очередь
Купить Голоса

Сценарий тестирования блокчейна и cli_wallet:

Проверить логику после хардфорков
Проверить
Создание аккаунтов
Старт сети
Присоединение делегатов
Перевод голосов
Перевод голосов в Силу Голоса
Доставление ценовых фидов
Выплату кураторам
Голосование за делегатов
Вывод из power, power down
Протестировать 3 временных параметра выплат:
STEEMIT_CASHOUT_WINDOW_SECONDS
STEEMIT_SECOND_CASHOUT_WINDOW
STEEMIT_MAX_CASHOUT_WINDOW_SECONDS
Комиссия за создание аккаунта (влияние делегатов на этот показатель)
Восстановление аккаунтов. Протестировать параметры:
STEEMIT_OWNER_AUTH_RECOVERY_PERIOD
STEEMIT_ACCOUNT_RECOVERY_REQUEST_EXPIRATION_PERIOD
STEEMIT_OWNER_UPDATE_LIMIT
Проверка вестинга (до и после старта)
Проверить попадание делегата в очередь в результате голосования
STEEMIT_START_MINER_VOTING_BLOCK
Проверить формирование пула витнес+майнер+случайный на раунд
Тестирование STEEMIT_HARDFORK_REQUIRED_WITNESSES
Тестирование изменения процентной ставки
Протестировать параметры ликвидности
STEEMIT_APR_PERCENT_MULTIPLY_PER_BLOCK
Параметры суплая
Работа витнесов (поставка данных)
STEEMIT_BLOCKCHAIN_PRECISION
Протестировать распределение денег на аккаунты генезиса
Конфигурационный файл
Первая итерация
STEEMIT_GENESIS_TIME
STEEMIT_MINING_TIME
STEEMIT_CASHOUT_WINDOW_SECONDS
STEEMIT_SECOND_CASHOUT_WINDOW
STEEMIT_MAX_CASHOUT_WINDOW_SECONDS
STEEMIT_BLOCK_INTERVAL
STEEMIT_BLOCKS_PER_YEAR
STEEMIT_BLOCKS_PER_DAY
STEEMIT_START_VESTING_BLOCK
STEEMIT_START_MINER_VOTING_BLOCK
STEEMIT_FIRST_CASHOUT_TIME
STEEMIT_INIT_MINER_NAME
STEEMIT_NUM_INIT_MINERS
STEEMIT_MAX_VOTED_WITNESSES
STEEMIT_MAX_MINER_WITNESSES
STEEMIT_MAX_RUNNER_WITNESSES
STEEMIT_MAX_MINERS
STEEMIT_HARDFORK_REQUIRED_WITNESSES
STEEMIT_VESTING_WITHDRAW_INTERVALS
STEEMIT_VESTING_WITHDRAW_INTERVAL_SECONDS
STEEMIT_MIN_PAYOUT_SBD
STEEMIT_INIT_SUPPLY
(fc::time_point_sec(1475332220))
(fc::time_point_sec(1475332300))
(60*60) /// 1 hr
(60*60*3) /// 3 hours
(60*60*6) /// 6 hours
3
(365*24*60*60/STEEMIT_BLOCK_INTERVAL)
(24*60*60/STEEMIT_BLOCK_INTERVAL)
(STEEMIT_BLOCKS_PER_DAY/4)
(STEEMIT_BLOCKS_PER_DAY/3)
(fc::time_point_sec(1475346620))
"cyberfounder"
1
5
1
1

2
3
(60*60)
(asset(1,SBD_SYMBOL))
int64_t((STEEMIT_BLOCKS_PER_DAY*1/0.03 - STEEMIT_BLOCKS_PER_DAY*1)*1000)
