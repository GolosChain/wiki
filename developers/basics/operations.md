# Операции и их типы

Операции в блокчейне GOLOS делятся на обычные и виртуальные. Обычные операции попадают в блокчейн через подписанную транзакцию участника сети. 

Виртуальные операции генерируются нодой когда срабатывают определенные условия в коде и носят больше информационный характер для участников сети. 

## Нумерация операций

В протоколе GOLOS [есть нумерация операций](https://github.com/golos-blockchain/golos/blob/master/libraries/protocol/include/golos/protocol/operations.hpp#L14) \(начинается с нуля\), там находятся как обычные, так и виртуальные операции:

* vote: 0
* comment: 1
* transfer: 2
* transfer\_to\_vesting: 3
* withdraw\_vesting: 4
* limit\_order\_create: 5
* limit\_order\_cancel: 6
* feed\_publish: 7
* convert: 8
* account\_create: 9
* account\_update: 10
* witness\_update: 11
* account\_witness\_vote: 12
* account\_witness\_proxy: 13
* pow: 14
* custom: 15
* report\_over\_production: 16
* delete\_comment: 17
* custom\_json: 18
* comment\_options: 19
* set\_withdraw\_vesting\_route: 20
* limit\_order\_create2: 21
* challenge\_authority: 22
* prove\_authority: 23
* request\_account\_recovery: 24
* recover\_account: 25
* change\_recovery\_account: 26
* escrow\_transfer: 27
* escrow\_dispute: 28
* escrow\_release: 29
* pow2: 30
* escrow\_approve: 31
* transfer\_to\_savings: 32
* transfer\_from\_savings: 33
* cancel\_transfer\_from\_savings: 34
* custom\_binary: 35
* decline\_voting\_rights: 36
* reset\_account: 37
* set\_reset\_account: 38
* delegate\_vesting\_shares: 39
* account\_create\_with\_delegation: 40
* account\_metadata: 41
* proposal\_create: 42
* proposal\_update: 43
* proposal\_delete: 44
* chain\_properties\_update: 45
* break\_free\_referral: 46
* delegate\_vesting\_shares\_with\_interest: 47
* reject\_vesting\_shares\_delegation: 48
* transit\_to\_cyberway: 49
* worker\_request: 50
* worker\_request\_delete: 51
* worker\_request\_vote: 52
* fill\_convert\_request: 53
* author\_reward: 54
* curation\_reward: 55
* comment\_reward: 56
* liquidity\_reward: 57
* interest: 58
* fill\_vesting\_withdraw: 59
* fill\_order: 60
* shutdown\_witness: 61
* fill\_transfer\_from\_savings: 62
* hardfork: 63
* comment\_payout\_update: 64
* comment\_benefactor\_reward: 65
* return\_vesting\_delegation: 66
* producer\_reward: 67
* delegation\_reward: 68
* auction\_window\_reward: 69

Номер операции нужен для низко-уровневого формирования транзакций и их подписи \(подробнее в разделе [Формирование транзакций](transaction-formatting.md)\).

