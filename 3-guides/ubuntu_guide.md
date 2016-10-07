## Скрипт установки клиента Голос на Ubuntu 16.04 LTS

Вы можете запустить команду за командой либо создать файл golos-install.sh и дать ему необходимые права: 

```bash
touch golos-install.sh
nano golos-install.sh
```

Заметка: самый простой способ сделать это с помощью редактора [nano](http://help.ubuntu.ru/wiki/nano)

Данный скрипт устанавливает все необходимые зависимости и скачивает последний релиз из основоного репозитория с мастер ветки (во время тестнета с тестовой ветки)

```bash
#!/bin/bash

# install dependencies
sudo apt-get -y upgrade && sudo apt-get -y install git cmake g++ python-dev autotools-dev libicu-dev build-essential libbz2-dev libboost-all-dev libssl-dev libncurses5-dev doxygen libreadline-dev dh-autoreconf screen

# remove old installation
rm -rf golos
mkdir golos

# pull fresh code, compile
git clone https://github.com/GolosChain/golos && cd golos && git checkout testnet4 && git submodule update --init --recursive && cmake -DCMAKE_BUILD_TYPE=Release . && make -j4

# install new binaries
cp programs/golosd/golosd ../golosnode/
cp programs/golosd/snapshot5392323.json ../golosnode/
cp programs/cli_wallet/cli_wallet ../golosnode/

# go into golos
cd ..
cd golosnode/

# apply config.ini if available
if [ -f ../config.ini ]
then
    cp -fv ../config.ini witness_node_data_dir/
fi
```
Заметка: для ускорения процесса вы можете запустить команду make с опцией -j4 (или больше в зависимости от вашего процессора)

```
chmod +x golos-install.sh && ./golos-install.sh
```

```
make -j8
```

После всего этого структура директорий будет следующая - 

```bash
$ tree -L 2
.
├── config.ini <=== (наш config.ini)
├── golos
│   ├── (source code files)
├── golosnode <=== (отсуда мы запускаем ноду и воллет)
│   ├── cli_wallet
│   ├── golosd
│   └── witness_node_data_dir  <=== (config.ini и блокчейн находится здесь)
└── golos-install.sh
```

Если вышел новый релиз с новым хардфорком - перезапускаем скрипт.

```bash
./golos-install.sh
```

## Пошаговая сборка клиента Голос на Ubuntu 14.04 LTS

_В этом обучающем материале изложено, как можно начать работать над Голосом, используя операционную систему Debian/Ubuntu. В случае использования другой операционной системы, основанной на Linux, возможна иная последовательность установки необходимых элементов._

### Установка зависимостей

Для успешной инсталляции Ubuntu 14.04 LTS требуется установить следующие дополнительные элементы :

```
sudo apt-get install gcc-4.9 g++-4.9 cmake make libbz2-dev libdb++-dev libdb-dev
sudo apt-get install libssl-dev openssl libreadline-dev autoconf libtool git

```
### Скачивание оригиналов

Оригиналы программного кода могут быть загружены с [гитхаба](https://github.com/GolosChain/golos).

```
git clone git@github.com:GolosChain/golos.git
```

### Сборка клиента Голоса

Для конфигурирования и компиляции требуется запустить cmake:

```
cmake -DBOOST_ROOT="$BOOST_ROOT" -DCMAKE_BUILD_TYPE=Release .
make
```

Обратите внимание, что переменная среды $BOOST_ROOT должна указывать на вашу установочную папку, если вы устанавливали её вручную.

### Обновление Голоса

Для загрузки обновлённого программного кода требуется запустить следующий код:

```
git fetch
git checkout <version>
git submodule update --init --recursive
cmake .
make
```

### Особенность разных версий ПО

#### Boost 1.60

Библиотека загрузочных файлов может быть устаревшей. В этом случае следует загрузить tar-архив для Boost 1.60.0.

```
export BOOST_ROOT=$HOME/opt/boost_1_60_0
sudo apt-get update
sudo apt-get install autotools-dev build-essential g++ libbz2-dev libicu-dev python-dev
wget -c 'http://sourceforge.net/projects/boost/files/boost/1.60.0/boost_1_60_0.tar.bz2/download' \
    -O boost_1_60_0.tar.bz2
tar xjf boost_1_60_0.tar.bz2
cd boost_1_60_0/
./bootstrap.sh "--prefix=$BOOST_ROOT"
./b2 install 
``` 

#### Ubuntu 14.04

Поскольку  g++-4.9 недоступна в 14.04 LTS , следует вначале сделать следующее:

```
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
If you get build failures due to abi incompatibilities, just use gcc 4.9

CC=gcc-4.9 CXX=g++-4.9 cmake .
```

#### Ubuntu 15.04


В Ubuntu 15.04 используются gcc 5, в которых c++11 ABI используется по дефолту, при этом загрузочные файлы были скомпилированы с использованием cxx11 ABI (это проблема во многих дистрибутивов). При возникновении ошибок, вызванных несовместимостью с abi, следует использовать  gcc 4.9

```
CC=gcc-4.9 CXX=g++-4.9 cmake .
```
