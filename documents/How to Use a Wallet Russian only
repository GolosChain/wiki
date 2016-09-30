#
#
Работа с кошельком через JSON RPC 
Пошаговая инструкция для операций с кошельком через JSON-RPC

В данном документе показано, как следует использовать cli_wallet через JSON-RPC через HTTP. Использование команды curl приведено в примере. 

Запуск тестовой сети 
В этом документе предполагается, что steemd процесс запущен на локальном компьютере и сконфигурирован таким образом, что конечная точка RPC использует порт 9876


#Настройка конечной точки RPC
rpc-endpoint = 127.0.0.1:9876

Запуск CLI кошелька с помощью сервера HTTP
Требуется запустить кошелёк CLI и убедится, что он настроен на работу с процессом steemd, запущенным на предыдущем шаге. Второй параметр - это конечная точка, через которую в функцию будут поступать данные о HTTP соединениях. 

./cli_wallet --rpc-server-endpoint="ws://127.0.0.1:9876"  --rpc-http-endpoint="127.0.0.1:6789" 
При желании заблокировать интерактивный ввод данных можно использовать 

./cli_wallet --rpc-server-endpoint="ws://127.0.0.1:9876"  --rpc-http-endpoint="127.0.0.1:6789" --daemon 

Дла запроса данных из API требуется получить в работу требуемый блок по номеру блока 

curl --data-binary '{"id":"1","method":"get_block","params":[1]}' \
    http://127.0.0.1:6789
 Открытие кошелька:
curl --data-binary '{"id":"1","method":"unlock","params":["testpassword"]}' \
    http://127.0.0.1:6789
Перевод средств:
curl --data-binary '{"id":"2","method":"transfer","params":["initminer","scott","1.234 TESTS", "memo", "true"]}' \
    http://127.0.0.1:6789

 {"id":2,"result":{"ref_block_num":1881,"ref_block_prefix":1518310624,"expiration":"2016-03-29T18:51:42","operations":[["transfer",{"from":"initminer","to":"scott","amount":"1.234 TESTS","memo":"memo"}]],"extensions":[],"signatures":["20035a77f64d6d03b0d874c576efe101d2dad6ae64fec09390cad2b471ede75527601239dd0ee4c2d16d76792406ae48b8d5e4cf41c8b3dd9b42a90ee9aca84a81"],"transaction_id":"c5153e84342d59bc0a4b4bb7872fc3165722d213","block_num":1882,"transaction_num":0}}


Можно использовать 
jsonprettyprint.com для приведения данныхв более дружелюбный вид:
{
 "id": 2,
 "result": {
   "ref_block_num": 1881,
   "ref_block_prefix": 1518310624,
   "expiration": "2016-03-29T18:51:42",
   "operations": [
     [
       "transfer",
       {
         "from": "initminer",
         "to": "scott",
         "amount": "1.234 TESTS",
         "memo": "memo"
       }
     ]
   ],
   "extensions": [
     
   ],
   "signatures": [
     "20035a77f64d6d03b0d874c576efe101d2dad6ae64fec09390cad2b471ede75527601239dd0ee4c2d16d76792406ae48b8d5e4cf41c8b3dd9b42a90ee9aca84a81"
   ],
   "transaction_id": "c5153e84342d59bc0a4b4bb7872fc3165722d213",
   "block_num": 1882,
   "transaction_num": 0
 }
}

Получение данных о транзакиции по номеру транзакиции:
curl --data-binary '{"id":"1","method":"get_transaction","params":["c5153e84342d59bc0a4b4bb7872fc3165722d213"]}' \
    http://127.0.0.1:6789

{"id":1,"result":{"ref_block_num":1881,"ref_block_prefix":1518310624,"expiration":"2016-03-29T18:51:42","operations":[["transfer",{"from":"initminer","to":"scott","amount":"1.234 TESTS","memo":"memo"}]],"extensions":[],"signatures":["20035a77f64d6d03b0d874c576efe101d2dad6ae64fec09390cad2b471ede75527601239dd0ee4c2d16d76792406ae48b8d5e4cf41c8b3dd9b42a90ee9aca84a81"],"transaction_id":"c5153e84342d59bc0a4b4bb7872fc3165722d213","block_num":1882,"transaction_num":0}}
 
