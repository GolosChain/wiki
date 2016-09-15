You can find script for inflation at **_programs/util_**

To run this script, enter:
```bash
 ./inflation_model > model.json
```
It will generate file _model.json_ for you, which contains 10-year supply data. You can plot this data with following command:
```bash
python3 inflation_plot.py model.json
```
**NOTE #1** You should have already installed _matplotlib_ for python3.
```bash
brew install homebrew/python/numpy --with-python3
```

```bash
brew install homebrew/python/matplotlib --with-python3
```

Also check [stackoverflow](http://stackoverflow.com/a/8606026)

**NOTE #2** If you would like to look into model.json use SublimeText editor.

**Explanation of output**

```bash
{"rvec":["929159090641","8360617424769","929159090641","8360617424769","197985103985","1780051544865","195077031513","1755693283617","179687790278","1615357001502"],"b":68585000,"s":"24303404786580"}
```

rvec shows total number of **STEEM satoshis** created since genesis for:

- Curation rewards
- Vesting rewards balancing curation rewards
- Content rewards
- Vesting rewards balancing content rewards
- Producer rewards
- Vesting rewards balancing producer rewards
- Liquidity rewards
- Vesting rewards balancing liquidity rewards
- PoW rewards
- Vesting rewards balancing PoW rewards

**b** is **block** number

**s** is **total** supply

**Some possible sources of inaccuracy, the direction and estimated relative sizes of these effects:**

- Missed blocks not modeled (lowers STEEM supply, small)
- Miner queue length very approximately modeled (assumed to go to 100 during the first blocks and then stay there) (may lower or raise STEEM supply, very small)
- Creation / destruction of STEEM used to back SBD not modeled (moves STEEM supply in direction opposite to changes in dollar value of 1 STEEM, large)
- Interest paid to SBD not modeled (raises STEEM supply, medium)
- Lost / forgotten private keys / wallets and deliberate burning of STEEM not modeled (lowers STEEM supply, unknown but likely small)
- Possible bugs or mismatches with implementation (unknown)
