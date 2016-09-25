Скрипт для установки ноды блочейна Голос. 

Вы можете запустить команду за командой либо создать файл golos-install.sh и дать ему необходимые права: 

```bash
chmod +x golos-install.sh && ./golos-install.sh
```

Данный скрипт устанавливает все необходимые зависимости и скачивает последний релиз из основоного репозитория с мастер ветки (во время тестнета с тестовой ветки)

```bash
#!/bin/bash

# install dependencies == устанавливаем зависимости
sudo apt-get -y upgrade && sudo apt-get -y install git cmake g++ python-dev autotools-dev libicu-dev build-essential libbz2-dev libboost-all-dev libssl-dev libncurses5-dev doxygen libreadline-dev dh-autoreconf screen

# remove old installation == стираем папки предыдущих установок
rm -rf golos
mkdir golos

# pull fresh code, compile == тянем исходники с гх, компилим-собираем
git clone https://github.com/GolosChain/golos && cd golos && git checkout testnet3 && git submodule update --init --recursive && cmake -DCMAKE_BUILD_TYPE=Release . && make -j4

# install new binaries == копируем бинарники поудобнее
cp programs/steemd/steemd ../golosnode/
cp programs/cli_wallet/cli_wallet ../golosnode/

# go into golos == переходим в директорию со собранной нодой и воллетом
cd ..
cd golosnode/

# apply config.ini if available == используем конфигурационный файл, если он есть
if [ -f ../config.ini ]
then
    cp -fv ../config.ini witness_node_data_dir/
fi

# going deeper == запускаем
./steemd
```

После всего этого структура директорий будет примерно следующая - 

```bash
besson@ubuntu:~/Desktop/test$ tree -L 2
.
├── config.ini <=== (наш config.ini)
├── golos
│   ├── (source code files)
├── golosnode <=== (отсуда мы запускаем ноды и воллет)
│   ├── cli_wallet
│   ├── steemd
│   └── witness_node_data_dir  <=== (config.ini и блокчейн находится здесь)
└── steem-install.sh
```

Если вышел новый релиз с новым хардфорком - перезапускаем скрипт.

```bash
./golos-install.sh
```
