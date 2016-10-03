Перед тем как начать, установите пожалуйста XCode [скачать](https://developer.apple.com/download) или через App Store
и введите в консоли:
```bash
sudo xcodebuild -license accept
```

Установите Homebrew [отсуда](http://brew.sh/) или введите:
```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Установите зависимости Голоса:
```bash
brew install cmake git openssl autoconf automake qt5
```

```bash
brew install google-perftools berkeley-db libtool python3 readline
```

```bash
brew install homebrew/versions/boost160
```

Склонируйте репозиторий в вашу рабочую директорию:
```bash
git clone https://github.com/GolosChain/golos
```

И собирети проект (это может занять от 10 до 30 минут):
```bash
cd golos
git submodule update --init --recursive
cmake -DBOOST_ROOT='/usr/local/Cellar/boost160/1.60.0' -DOPENSSL_ROOT_DIR='/usr/local/Cellar/openssl/1.0.2h_1/'  -DCMAKE_BUILD_TYPE=Release .
make
```
Заметка: для ускорения процесса можете запустить команду make с опцией -j4 или больше (зависит от вашего процессора)
```bash
make -j4
```

Теперь вы можете запусить голос:
```bash
./steemd
```

Или с помощью утилиты [screen](http://help.ubuntu.ru/wiki/screen)
```bash
screen -dmS golos ./steemd
```

После запуска у вас запусится нода и инициализируются аккаунты из файлы snapshot.json в генезис, также у вас создатся папка 
witness_node_data_dir с конфигурационным файлом config.ini

Отредактируйте его со следующими настройками:
[дописать инструкцию по конфигурированию config.ini]
