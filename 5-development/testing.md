# Тестирование
<!-- toc -->

- [Что такое TEST CASE, CHECK LIST, CHEET SHEET](http://bugscatcher.net/archives/2480)

###### Обычно сценарий состоит из следующих пунктов:
     * Предусловие
     * Описание последовательности действий пользователя
     * Ожидаемый результат (то, как система должна себя вести)

###### Сценарий тестирования клиента:
1. Регистрация
     * Через фейсбук
     * Через ВК
2. Постинг
     * Написать заголовок и тело поста
     * Добавить теги
     * Отметить: проголосовать за свой пост
     * Отметить: получить 100% в Силе Голоса
     * Апвоут пост
     * Показать проголосовавших
     * Отображение выплат
     * Отметить пост флагом
3. Фолловинг
     * Зафолловить автора
     * Отображение в ленте постов авторов
4. Репост
     * Репост поста
     * Отображение в ленте
5. Комментирование
     * Добавить комментарий
     * Апвоут комментария
     * Комментарий комментария
6. Репост в соцсети
     * Репост в твиттер
     * Репост в фейсбук
7. Кошелек
     * Перевод Голоса в Силу Голоса себе
     * Перевод Голоса в Силу Голоса другому
     * Пауэр даун
     * Перевод GBG	в Стимы
8. Разрешения
     * Показать ключи
9. Пароль
     * Смена пароля
     * Войти под новым паролем
10. Возврат украденного аккаунта
11. Поиск по сайту
12. Поиск по тегам
     * Русские
     * Английские
13. Голосование за делегатов
14. Покупка на ICO
15. Купить Голоса

###### Сценарий тестирования блокчейна и cli_wallet:

1. Проверить логику после хардфорков
2. Проверить
     * Создание аккаунтов
     * Старт сети
     * Присоединение делегатов
     * Перевод голосов
     * Перевод голосов в Силу Голоса
     * Доставление ценовых фидов
     * Выплату кураторам
     * Голосование за делегатов
     * Вывод из power, power down
3. Протестировать 3 временных параметра выплат:
     * STEEMIT_CASHOUT_WINDOW_SECONDS
     * STEEMIT_SECOND_CASHOUT_WINDOW
     * STEEMIT_MAX_CASHOUT_WINDOW_SECONDS
4. Комиссия за создание аккаунта (влияние делегатов на этот показатель)
5. Восстановление аккаунтов. Протестировать параметры:
     * STEEMIT_OWNER_AUTH_RECOVERY_PERIOD
     * STEEMIT_ACCOUNT_RECOVERY_REQUEST_EXPIRATION_PERIOD
     * STEEMIT_OWNER_UPDATE_LIMIT
6. Проверка вестинга (до и после старта)
7. Проверить попадание делегата в очередь в результате голосования
     * STEEMIT_START_MINER_VOTING_BLOCK
     * Проверить формирование пула витнес+майнер+случайный на раунд
8. Тестирование STEEMIT_HARDFORK_REQUIRED_WITNESSES
9. Тестирование изменения процентной ставки
10. Протестировать параметры ликвидности
11. STEEMIT_APR_PERCENT_MULTIPLY_PER_BLOCK
12. Параметры суплая
13. Работа витнесов (поставка данных)
14. STEEMIT_BLOCKCHAIN_PRECISION
15. Протестировать распределение денег на аккаунты генезиса

##### Конфигурационный файл: Первая итерация
/

STEEMIT_GENESIS_TIME   |  (fc::time_point_sec(1475332220)) 
-----------------------|----------------------------------
STEEMIT_MINING_TIME |    (fc::time_point_sec(1475332300)) 
STEEMIT_CASHOUT_WINDOW_SECONDS |   (60*60) /// 1 hr   
STEEMIT_SECOND_CASHOUT_WINDOW |   (60*60*3) /// 3 hours   
STEEMIT_MAX_CASHOUT_WINDOW_SECONDS |    (60*60*6) /// 6 hours  
STEEMIT_START_VESTING_BLOCK |   (STEEMIT_BLOCKS_PER_DAY/4)   
STEEMIT_START_MINER_VOTING_BLOCK |   (STEEMIT_BLOCKS_PER_DAY/3)   
STEEMIT_FIRST_CASHOUT_TIME |  (fc::time_point_sec(1475346620))    
STEEMIT_INIT_MINER_NAME |  "cyberfounder"    
STEEMIT_NUM_INIT_MINERS |    1  
STEEMIT_MAX_VOTED_WITNESSES |  5    
STEEMIT_MAX_MINER_WITNESSES |   1   
STEEMIT_MAX_RUNNER_WITNESSES |   1   
STEEMIT_HARDFORK_REQUIRED_WITNESSES | 2  
STEEMIT_VESTING_WITHDRAW_INTERVALS |   3   
STEEMIT_VESTING_WITHDRAW_INTERVAL_SECONDS |  (60*60)    
STEEMIT_MIN_PAYOUT_SBD |   (asset(1,SBD_SYMBOL))   
STEEMIT_INIT_SUPPLY |   int64_t((STEEMIT_BLOCKS_PER_DAY*1/0.03 - STEEMIT_BLOCKS_PER_DAY*1)*1000)   
