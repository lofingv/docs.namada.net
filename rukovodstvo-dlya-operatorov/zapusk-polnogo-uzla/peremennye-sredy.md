# Переменные среды

По умолчанию при каждом запуске ledger namada будет применяться конфигурация, содержащаяся в конфигурационном файле ledger.

Пример конфигурационного файла ledger приведен ниже.

```rust
wasm_dir = "wasm"
 
[ledger]
genesis_time = "2023-06-29T17:00:00+00:00"
chain_id = "<namada-mainnet>"
 
[ledger.shell]
base_dir = "/Users/fraccaman/Library/Application Support/Namada"
storage_read_past_height_limit = 3600
db_dir = "db"
cometbft_dir = "cometbft"
tendermint_mode = "Full"
 
[ledger.cometbft]
proxy_app = "tcp://127.0.0.1:26658"
moniker = "1337-leet-1337"
db_backend = "goleveldb"
db_dir = "data"
log_level = "info"
log_format = "plain"
genesis_file = "config/genesis.json"
node_key_file = "config/node_key.json"
abci = "socket"
filter_peers = false
priv_validator_key_file = "config/priv_validator_key.json"
priv_validator_state_file = "data/priv_validator_state.json"
priv_validator_laddr = ""
 
[ledger.cometbft.rpc]
laddr = "tcp://127.0.0.1:26657"
cors_allowed_origins = []
cors_allowed_methods = ["HEAD", "GET", "POST"]
cors_allowed_headers = ["Origin", "Accept", "Content-Type", "X-Requested-With", "X-Server-Time"]
unsafe = false
max_open_connections = 900
max_subscription_clients = 100
max_subscriptions_per_client = 5
timeout_broadcast_tx_commit = "10000ms"
max_body_bytes = 1000000
max_header_bytes = 1048576
tls_cert_file = ""
tls_key_file = ""
pprof_laddr = ""
 
[ledger.cometbft.p2p]
laddr = "tcp://0.0.0.0:26656"
external_address = ""
seeds = ""
persistent_peers = "<peer-id>@<ip>:<port>, ..."
upnp = false
addr_book_file = "config/addrbook.json"
addr_book_strict = true
max_num_inbound_peers = 40
max_num_outbound_peers = 10
unconditional_peer_ids = ""
persistent_peers_max_dial_period = "0ms"
flush_throttle_timeout = "100ms"
max_packet_msg_payload_size = 1024
send_rate = 5120000
recv_rate = 5120000
pex = true
seed_mode = false
private_peer_ids = ""
allow_duplicate_ip = false
handshake_timeout = "20000ms"
dial_timeout = "3000ms"
 
[ledger.cometbft.mempool]
recheck = true
broadcast = true
wal_dir = ""
size = 5000
max_txs_bytes = 1073741824
cache_size = 10000
keep-invalid-txs-in-cache = false
max_tx_bytes = 1048576
max_batch_bytes = 0
 
[ledger.cometbft.consensus]
wal_file = "data/cs.wal/wal"
double_sign_check_height = 0
create_empty_blocks = true
create_empty_blocks_interval = "0ms"
peer_gossip_sleep_duration = "100ms"
peer_query_maj23_sleep_duration = "2000ms"
timeout_propose = "3000ms"
timeout_propose_delta = "500ms"
timeout_prevote = "1000ms"
timeout_prevote_delta = "500ms"
timeout_precommit = "1000ms"
timeout_precommit_delta = "500ms"
timeout_commit = "10000ms"
 
[ledger.cometbft.tx_index]
indexer = "null"
 
[ledger.cometbft.instrumentation]
prometheus = false
prometheus_listen_addr = ":26660"
max_open_connections = 3
namespace = "namada_tm"
 
[ledger.cometbft.statesync]
enable = false
rpc_servers = ""
trust_height = 0
trust_hash = ""
trust_period = "168h0m0s"
discovery_time = "15000ms"
temp_dir = ""
```

Однако есть возможность переопределить конфигурацию, задав переменные окружения. Любая переменная, находящаяся в конфигурации, может быть доступна через переменные окружения, которые строятся следующим образом.

### Конструирование переменных окружения

Имена распознаваемых переменных окружения получаются из ключей конфигурации путем:

* Добавить к ключу `NAMADA_`
* Записать каждую букву ключа в верхнем регистре. Например, `p2p_pex` становится `P2P_PEX`
* Вставить `__` для каждого вложенного значения. Например, `ledger.cometbft` становится `LEDGER__COMETBFT`

Таким образом, параметр `p2p_pex` в файле `[ledger.cometbft]` может быть установлен следующим образом:

```rust
NAMADA_LEDGER__COMETBFT__P2P_PEX=true # or false, depending on your heart's desires
```

в окружающей среде

{% hint style="warning" %}
<mark style="color:orange;">Примечание: В принципе, для имен переменных окружения можно использовать даже</mark> <mark style="color:orange;">`.`</mark> <mark style="color:orange;">Однако в Bash можно использовать только форму двойного подчеркивания, поскольку Bash не разрешает использовать точки в именах переменных окружения. Поэтому мы опускаем вариант с точкой.</mark>
{% endhint %}
