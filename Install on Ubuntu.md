You can use it line-by-line or create file steem-install.sh
All you have to do, is place the script wherever you want to install steem. Then place your config.ini next to it (optional)

Than do
```bash
chmod +x steem-install.sh && ./steem-install.sh
```

```bash
#!/bin/bash

# install dependencies
sudo apt-get -y upgrade && sudo apt-get -y install git cmake g++ python-dev autotools-dev libicu-dev build-essential libbz2-dev libboost-all-dev libssl-dev libncurses5-dev doxygen libreadline-dev dh-autoreconf screen

# remove old installation
rm -rf steemd
mkdir steemd
rm -rf rsteem

# pull fresh code, compile
git clone https://github.com/Rsteem/rsteem && cd rsteem && git checkout master && git submodule update --init --recursive && cmake -DCMAKE_BUILD_TYPE=Release -DLOW_MEMORY_NODE=ON . && make

# install new binaries
cp programs/steemd/steemd ../steemd/
cp programs/cli_wallet/cli_wallet ../steemd/

# go into steemd
cd ..
cd steemd/

# download the blockchain (original steem blockchain!)
wget http://www.steemitup.eu/witness_node_data_dir.tar.gz
tar -zxvf witness_node_data_dir.tar.gz
rm witness_node_data_dir.tar.gz

# apply config.ini if available
if [ -f ../config.ini ]
then
    cp -fv ../config.ini witness_node_data_dir/
fi

# prep the local blockchain
./steemd --replay
```

After installation, our target folder should be like this:
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

If blockchain forks, or there is a new version of steem out, all we have to do, is re-run the script.
```bash
./steem-install.sh
```
