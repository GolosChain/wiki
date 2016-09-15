cmake -DCMAKE_BUILD_TYPE=Release -DLOW_MEMORY_NODE=ON -DOPENSSL_ROOT_DIR='/usr/local/Cellar/openssl/1.0.2h_1/' .
brew install homebrew/versions/boost160
cmake -DCMAKE_BUILD_TYPE=Release -DLOW_MEMORY_NODE=ON -DOPENSSL_ROOT_DIR='/usr/local/Cellar/openssl/1.0.2h_1/' -DBOOST_ROOT='/usr/local/Cellar/boost160/1.60.0/' .

Before you should install XCode and Homebrew, than

Open your terminal and update Brew using `brew doctor` command.

Install steem dependecies using this command.
```bash
brew install boost cmake git openssl autoconf automake qt5
```

Create a symling for openssl using this command `brew link --force openssl`

Install berkeley-db and google-preftools, libtool, python3
```bash
brew install google-perftools berkeley-db libtool python3 readline
```

Clone the repository
```bash
git clone https://github.com/GolosChain/golos
```

Change directory to golos `cd golos`
Build golos
```bash
git submodule update --init --recursive
cmake -DCMAKE_BUILD_TYPE=Release -DLOW_MEMORY_NODE=ON .
make
```

Now you can run steem with `./steemd` command. Don't forget set config.ini in witness_node_data_dir

**OPENSSL NOTE** If you can't link openssl with brew than do this:

1. Restart your mac and press CMD+R, than open terminal and enter `csrutil disable` -> reboot again
1. Locate your brew openssl path (ex. /usr/local/Cellar/openssl/1.0.2h_1/bin/openssl)
1. Move original openssl `sudo mv /usr/bin/openssl /usr/bin/openssl_old`
1. Link new openssl `ln -s /usr/local/Cellar/openssl/1.0.2h_1/bin/openssl /usr/local/bin/openssl`
1. Enter `openssl version -a`, you should see:
```bash
OpenSSL 1.0.2h  3 May 2016
built on: reproducible build, date unspecified
platform: darwin64-x86_64-cc
...
```

```bash
cmake -DOPENSSL_ROOT_DIR='/usr/local/Cellar/openssl/1.0.2h_1/' -DCMAKE_BUILD_TYPE=Release -DLOW_MEMORY_NODE=ON .
```

**BOOST NOTE** If you have any error with boost:
```bash
brew uninstall boost
brew install homebrew/versions/boost160
```

```bash
cmake -DBOOST_ROOT='/usr/local/Cellar/boost160/1.60.0' -DCMAKE_BUILD_TYPE=Release -DLOW_MEMORY_NODE=ON .
```
