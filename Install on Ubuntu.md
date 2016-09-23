Скрипт для установки ноды (узла) блочейна `голос`. 

Можно запускать к консоли строчка за строчкой, 

но лучше скопировать текст в файл, назавать файл `steem-install.sh` - потом запустить вот такой командой
```bash
chmod +x steem-install.sh && ./steem-install.sh
```


Сам скрипт вот.

```bash
#!/bin/bash

# install dependencies == cтавим зависимости
sudo apt-get -y upgrade && sudo apt-get -y install git cmake g++ python-dev autotools-dev libicu-dev build-essential libbz2-dev libboost-all-dev libssl-dev libncurses5-dev doxygen libreadline-dev dh-autoreconf screen

# remove old installation == стираем папки предыдущих установок
rm -rf steemd
mkdir steemd
rm -rf rsteem # Эту точно надо?

# pull fresh code, compile == тянем исходники с гх, компилим-собираем
git clone https://github.com/Rsteem/rsteem && cd rsteem && git checkout master && git submodule update --init --recursive && cmake -DCMAKE_BUILD_TYPE=Release -DLOW_MEMORY_NODE=ON . && make

# install new binaries == копируем бинарники поудобнее
cp programs/steemd/steemd ../steemd/
cp programs/cli_wallet/cli_wallet ../steemd/

# go into steemd == переходим в папку главной программы :) как эта штука называется?
cd ..
cd steemd/

# download the blockchain (original steem blockchain!) == качаем свежий дамп блокчейна
wget http://www.steemitup.eu/witness_node_data_dir.tar.gz # == Это что такое? почему ссылка не русифицирована? :)
tar -zxvf witness_node_data_dir.tar.gz
rm witness_node_data_dir.tar.gz

# apply config.ini if available == используем конфиг-файл, если он есть
if [ -f ../config.ini ]
then
    cp -fv ../config.ini witness_node_data_dir/
fi

# prep the local blockchain == чота делаем там
./steemd --replay
```

_After installation, our target folder should be like this:_
После всего этого структура папки будет примерно такая - 

```bash
besson@ubuntu:~/Desktop/test$ tree -L 2
.
├── config.ini <=== (our config.ini backup)
├── steem
│   ├── (source code files)
├── steemd <=== (we probably only care about this folder)
│   ├── cli_wallet
│   ├── steemd
│   └── witness_node_data_dir  <=== (config.ini and blockchain in here)
└── steem-install.sh
```

_If blockchain forks, or there is a new version of steem out, all we have to do, is re-run the script._
если случился хардфорк, или вышла новая версия программы, просто перезапускаем скрипт.

```bash
./steem-install.sh
```
