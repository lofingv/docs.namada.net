# Развертывание локальной сети

### Необходимые условия

Для работы в локальной сети Namada должна быть установлена [из исходного кода](../nachalo-raboty/ustanovit-namada/ustanovka-iz-iskhodnykh-failov/).

Для этого существует специально написанный скрипт, который можно найти в разделе scripts в репозитории Namada.

### Установка зависимостей скрипта

Для успешной работы скрипта необходимо установить некоторые зависимости:

1. python3 должен быть установлен.
2. Должна быть установлена библиотека Python pip [https://pypi.org/project/toml/.](https://pypi.org/project/toml/)

Для работы скрипта потребуется конфигурационный файл genesis, который представляет собой TOML-файл, задающий параметры сети. Примеры таких файлов можно найти в репо anoma-network-config в каталоге `templates`.

### Модификация конфигурационного файла genesis

Для успешного запуска скрипта необходимо удалить из файла toml все секции валидаторов. Это связано с тем, что скрипт будет генерировать новый набор валидаторов для сети.

В приведенном ниже блоке кода показан пример конфигурационного файла genesis, который был модифицирован для удаления секции validators.

```rust
# Editing genesis config steps
# Developer network
genesis_time = "2021-12-20T15:00:00.00Z"
native_token = "NAM"
 
- # 3 genesis validators.
- [validator.validator-1]
- # Validator's token balance at genesis.
- tokens = 100000
- # Amount of the validator's genesis token balance which is not staked.
- non_staked_balance = 100000
- # VP for the validator account
- validator_vp = "vp_validator"
- # Commission rate for rewards
- commission_rate = 0.05
- # Maximum change per epoch in the commission rate
- max_commission_rate_change = 0.01
- # Public IP:port address
- net_address = "52.210.23.30:26656"
-
- [validator.validator-2]
- # Validator's token balance at genesis.
- tokens = 100000
- # Amount of the validator's genesis token balance which is not staked.
- non_staked_balance = 100000
- # VP for the validator account
- validator_vp = "vp_validator"
- # Commission rate for rewards
- commission_rate = 0.05
- # Maximum change per epoch in the commission rate
- max_commission_rate_change = 0.01
- # VP for the staking reward account
- staking_reward_vp = "vp_user"
- # Public IP:port address
- net_address = "63.32.203.239:26656"
-
- [validator.validator-3]
- # Validator's token balance at genesis.
- tokens = 100000
- # Amount of the validator's genesis token balance which is not staked.
- non_staked_balance = 100000
- # VP for the validator account
- validator_vp = "vp_validator"
- # Commission rate for rewards
- commission_rate = 0.05
- # Maximum change per epoch in the commission rate
- max_commission_rate_change = 0.01
- # VP for the staking reward account
- staking_reward_vp = "vp_user"
- # Public IP:port address
- net_address = "54.195.72.213:26656"
 
# Some tokens present at genesis.
 
[token.NAM]
address = "atest1v4ehgw36x3prswzxggunzv6pxqmnvdj9xvcyzvpsggeyvs3cg9qnywf589qnwvfsg5erg3fkl09rg5"
vp = "vp_token"
[token.NAM.balances]
atest1v4ehgw36gc6yxvpjxccyzvphxycrxw2xxsuyydesxgcnjs3cg9znwv3cxgmnj32yxy6rssf5tcqjm3 = 9223372036854
 
[token.BTC]
address = "atest1v4ehgw36xdzryve5gsc52veeg5cnsv2yx5eygvp38qcrvd29xy6rys6p8yc5xvp4xfpy2v694wgwcp"
vp = "vp_token"
[token.BTC.balances]
atest1v4ehgw36gc6yxvpjxccyzvphxycrxw2xxsuyydesxgcnjs3cg9znwv3cxgmnj32yxy6rssf5tcqjm3 = 9223372036854
 
[token.ETH]
address = "atest1v4ehgw36xqmr2d3nx3ryvd2xxgmrq33j8qcns33sxezrgv6zxdzrydjrxveygd2yxumrsdpsf9jc2p"
vp = "vp_token"
[token.ETH.balances]
atest1v4ehgw36gc6yxvpjxccyzvphxycrxw2xxsuyydesxgcnjs3cg9znwv3cxgmnj32yxy6rssf5tcqjm3 = 9223372036854
 
[token.DOT]
address = "atest1v4ehgw36gg6nvs2zgfpyxsfjgc65yv6pxy6nwwfsxgungdzrggeyzv35gveyxsjyxymyz335hur2jn"
vp = "vp_token"
[token.DOT.balances]
atest1v4ehgw36gc6yxvpjxccyzvphxycrxw2xxsuyydesxgcnjs3cg9znwv3cxgmnj32yxy6rssf5tcqjm3 = 9223372036854
 
[token.Schnitzel]
address = "atest1v4ehgw36xue5xvf5xvuyzvpjx5un2v3k8qeyvd3cxdqns32p89rrxd6xx9zngvpegccnzs699rdnnt"
vp = "vp_token"
[token.Schnitzel.balances]
atest1v4ehgw36gc6yxvpjxccyzvphxycrxw2xxsuyydesxgcnjs3cg9znwv3cxgmnj32yxy6rssf5tcqjm3 = 9223372036854
 
[token.Apfel]
address = "atest1v4ehgw36gfryydj9g3p5zv3kg9znyd358ycnzsfcggc5gvecgc6ygs2rxv6ry3zpg4zrwdfeumqcz9"
vp = "vp_token"
[token.Apfel.balances]
atest1v4ehgw36gc6yxvpjxccyzvphxycrxw2xxsuyydesxgcnjs3cg9znwv3cxgmnj32yxy6rssf5tcqjm3 = 9223372036854
 
[token.Kartoffel]
address = "atest1v4ehgw36gep5ysecxq6nyv3jg3zygv3e89qn2vp48pryxsf4xpznvve5gvmy23fs89pryvf5a6ht90"
public_key = ""
vp = "vp_token"
[token.Kartoffel.balances]
atest1v4ehgw36gc6yxvpjxccyzvphxycrxw2xxsuyydesxgcnjs3cg9znwv3cxgmnj32yxy6rssf5tcqjm3 = 9223372036854
 
# Some established accounts present at genesis.
 
[implicit.faucet]
address = "atest1v4ehgw36gc6yxvpjxccyzvphxycrxw2xxsuyydesxgcnjs3cg9znwv3cxgmnj32yxy6rssf5tcqjm3"
vp = "vp_user"
 
[established.masp]
address = "atest1v4ehgw36xaryysfsx5unvve4g5my2vjz89p52sjxxgenzd348yuyyv3hg3pnjs35g5unvde4ca36y5"
vp = "vp_masp"
 
# Wasm VP definitions
 
# Implicit VP
 
[wasm.vp_implicit]
filename = "vp_implicit.wasm"
 
# Default user VP in established accounts
 
[wasm.vp_user]
 
# filename (relative to wasm path used by the node)
 
filename = "vp_user.wasm"
 
# Validator VP
 
[wasm.vp_validator]
filename = "vp_validator.wasm"
 
# Token VP
 
[wasm.vp_token]
filename = "vp_token.wasm"
 
# MASP VP
 
[wasm.vp_masp]
filename = "vp_masp.wasm"
 
# General protocol parameters.
 
[parameters]
 
# Minimum number of blocks in an epoch.
 
min_num_of_blocks = 5
 
# Maximum expected time per block (in seconds).
 
max_expected_time_per_block = 11
 
# Implicit VP WASM name
 
implicit_vp = "vp_implicit"
 
# Expected number of epochs per year (also sets the min duration of an epoch in seconds)
 
epochs_per_year = 262800 # ~120 sec epoch duration (60*60*24\*365 / 120)
 
# The P gain factor in the Proof of Stake rewards controller
 
pos_gain_p = 0.1
 
# The D gain factor in the Proof of Stake rewards controller
 
pos_gain_d = 0.1
 
# The wrapper tx fixed fees
 
wrapper_tx_fees = "5"
 
# The max size of blocks in bytes
 
max_proposal_bytes = 5242880
vp_whitelist = []
tx_whitelist = []
max_public_key_per_account = 15
#Maximum number of signatures required per transaction
max_signature_per_tx = 15
 
# Proof of stake parameters.
 
[pos_params]
 
# Maximum number of active validators.
 
max_validator_slots = 128
 
# Pipeline length (in epochs). Any change in the validator set made in
 
# epoch 'n' will become active in epoch 'n + pipeline_len'.
 
pipeline_len = 2
 
# Unbonding length (in epochs). Validators may have their stake slashed
 
# for a fault in epoch 'n' up through epoch 'n + unbonding_len'.
 
unbonding_len = 4
 
# Votes per fundamental staking token (namnam)
 
tm_votes_per_token = 1.0
 
# Reward for proposing a block.
 
block_proposer_reward = 0.125
 
# Reward for voting on a block.
 
block_vote_reward = 0.1
 
# Maximum inflation rate per annum (10%)
 
max_inflation_rate = 0.1
 
# Targeted ratio of staked tokens to total tokens in the supply
 
target_staked_ratio = 0.6667
 
# Portion of a validator's stake that should be slashed on a duplicate
 
# vote.
 
duplicate_vote_min_slash_rate = 0.001
 
# Portion of a validator's stake that should be slashed on a light
 
# client attack.
 
light_client_attack_min_slash_rate = 0.001
cubic_slashing_window_length = 1
 
# Governance parameters.
 
[gov_params]
 
# minimum amount of xan token to lock
 
min_proposal_fund = 100
 
# proposal code size in kibibytes (KiB)
 
max_proposal_code_size = 300000
 
# min proposal period length in epochs
 
min_proposal_period = 3
 
# max proposal period length in epochs
 
max_proposal_period = 6
 
# maximum number of characters in the proposal content
 
max_proposal_content_size = 10000
 
# minimum epochs between end and grace epoch
 
min_proposal_grace_epochs = 0
```

### Сборка wasm

Сценарий также потребует собрать все файлы wasm для транзакций. Это можно сделать, выполнив следующую команду (находясь в каталоге namada):

```rust
make build-wasm-scripts
```

### Запуск скрипта

Сценарий называется `build_network.sh` и может быть запущен с помощью следующей команды:

```rust
# Ensure you are in the root of the namada repository directory
./scripts/build_network.sh <config_toml> <namada_dir> <OPTIONAL: base_dir>
```

Более конкретно, скрипт принимает три аргумента:

1. `config_toml`: путь к конфигурационному файлу (без валидатора) `genesis.`
2. `namada_dir`: путь к каталогу с двоичными файлами Namada. Если двоичные файлы были собраны с помощью `make build-release`, то это будет означать каталог `namada/target/release`. В противном случае, если они были собраны с помощью `make build`, то это будет каталог `namada/target/debug`.
3. base\_dir: (необязательный аргумент) путь к каталогу `BASE_DIR`, в котором хранятся все данные цепочки. Это необходимо только в том случае, если `BASE_DIR` не является значением по умолчанию, заданным командой `namadac utils default-base-dir`.

Например, пользователь MacOS выполнит команду примерно следующего вида:

```rust
./scripts/build_network.sh \
~/anoma-network-config/templates/edited_genesis_config.toml \
./target/release # Assuming the binaries were built using `make build-release`
```

### Запуск ledger

После выполнения скрипта в фоновом режиме будет запущен процесс python. Запустить ledger можно с помощью привычной команды:

```rust
target/release/namada ledger # Assuming the binaries were built using `make build-release`
```

### Очистка

После того как локальная сеть выполнила свое предназначение, ее можно очистить, выполнив следующие команды, находящиеся в функции cleanup скрипта:

```rust
    pkill -f ".hack/chains"
    rm -r .hack/chains
    rm local.*.tar.gz
```
