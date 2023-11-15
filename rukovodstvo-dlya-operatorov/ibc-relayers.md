# IBC Relayers

Работа ретранслятора на Namada

В этом документе описывается работа ретранслятора для протокола межблокчейн-коммуникаций (IBC) с Namada. В этой документации рассматривается возможность создания соединений по протоколу IBC, а также настройка локальных экземпляров Namada для целей тестирования.

В этом документе описаны основные шаги по использованию IBC с Namada:

1. [Настройка Hermes](ibc-relayers.md#nastroika-hermes)
2. [Установка Hermes](ibc-relayers.md#ustanovka-hermes)
3. [Настройка ретранслятора](ibc-relayers.md#nastroika-retranslyatora)
4. [Запуск ретранслятора](ibc-relayers.md#zapusk-retranslyatora)
5. [Настройка локальных экземпляров Namada](ibc-relayers.md#nastroika-lokalnykh-instansov-namada-s-pomoshyu-skripta-hermes)

Описанное ниже предназначено для тех, кто хочет организовать ретрансляцию IBC-сообщений между двумя цепочками Namada. Разумеется, это можно сделать между любыми двумя IBC-совместимыми цепочками (например, цепочкой Cosmos). В этом случае для осуществления передачи пакетов необходимо, чтобы узел работал как на цепочке назначения, так и на цепочке источника. Ниже мы рассмотрим, во-первых, как включить такое соединение между двумя уже существующими цепочками с помощью Hermes, а во-вторых, как настроить два локальных экземпляра Namada или объединить два уже существующих экземпляра Namada для этой цели.

### Настройка Hermes

Hermes - это ретранслятор IBC, предназначенный для передачи IBC-пакетов между цепочками (инстансами). В Namada используется [форк Hermes](https://github.com/heliaxdev/hermes/tree/1.6.0-namada), поддерживающий инстансы Namada. Перед началом ретрансляции пакетов пользователю необходимо выполнить следующие действия по настройке и запуску Hermes.

1. Создать файл конфигурации Hermes
2. Создать IBC-клиент/соединение/канал между инстансами
3. Запустить Hermes

#### Создать файл конфигурации Hermes

Одним из важнейших элементов головоломки является создание файла `config.tom`l, описывающего, какие будут установлены соединения, за которые будет отвечать ретранслятор.

```
export HERMES_CONFIG="<choose path for hermes config>/config.toml"
touch $HERMES_CONFIG
```

Если путь к файлу не указан, то по умолчанию считается `~/.hermes/config.toml`.

Пример конфигурационного файла приведен ниже. По сути, в конфигурационном файле для Namada вы меняете только идентификаторы цепочек, адреса RPC и имена ключей. Если у вас нет узлов, настройте их вручную или с помощью [наших скриптов](ibc-relayers.md#nastroika-lokalnykh-ekzemplyarov-namada-s-pomoshyu-skripta-hermes).

```rust
[global]
log_level = 'info'
 
[mode]
 
[mode.clients]
enabled = true
refresh = true
misbehaviour = true
 
[mode.connections]
enabled = false
 
[mode.channels]
enabled = false
 
[mode.packets]
enabled = true
clear_interval = 10
clear_on_start = false
tx_confirmation = true
 
[telemetry]
enabled = false
host = '127.0.0.1'
port = 3001
 
[[chains]]
id = 'namada-test.0a4c6786dbda39f786'  # set your chain ID
type = 'namada'
rpc_addr = 'http://127.0.0.1:27657'  # set the IP and the port of the chain
grpc_addr = 'http://127.0.0.1:9090'  # not used for now
event_source = { mode = 'push', url = 'ws://127.0.0.1:27657/websocket', batch_delay = '500ms' }  # set the IP and the port of the chain
account_prefix = ''  # not used
key_name = 'relayer'  # The key is an account name you made
store_prefix = 'ibc'
gas_price = { price = 0.001, denom = 'nam' }  # not used for now
 
[[chains]]
id = 'namada-test.647287156defa8728c'
type = 'namada'
rpc_addr = 'http://127.0.0.1:28657'
grpc_addr = 'http://127.0.0.1:9090'
event_source = { mode = 'push', url = 'ws://127.0.0.1:28657/websocket', batch_delay = '500ms' }
account_prefix = ''
key_name = 'relayer'
store_prefix = 'ibc'
gas_price = { price = 0.001, denom = 'nam' }
```

Путь к файлу конфигурации, который сохраняется в переменной `$HERMES_CONFIG`, пригодится в дальнейшем.

{% hint style="info" %}
<mark style="color:blue;">Интерпретация</mark> <mark style="color:blue;">**toml**</mark>

<mark style="color:blue;">Каждая конфигурация цепочки задается в объекте</mark> <mark style="color:blue;">`[[chains]]`</mark><mark style="color:blue;">. Это те кусочки головоломки, которые вы хотите сохранить:</mark>

<mark style="color:blue;">`chains.id`</mark> <mark style="color:blue;">- имя цепочки</mark>

<mark style="color:blue;">`chains.rpc_address`</mark> <mark style="color:blue;">- порт, через который осуществляется связь с каналом, и который будет являться аргументом для ledger\_address в Namada при взаимодействии (это станет ясно позже).</mark>

<mark style="color:blue;">**Обязательно измените IP-адрес на IP-адрес вашей локальной машины, на которой запущен этот узел**</mark><mark style="color:blue;">!</mark>

<mark style="color:blue;">`chains.key_name`</mark> <mark style="color:blue;">задает ключ подписывающего, который подписывает транзакцию от ретранслятора. Ключ должен быть сгенерирован перед запуском ретранслятора.</mark>

<mark style="color:blue;">`event_source`</mark> <mark style="color:blue;">задает URL веб-сокета цепочки. Для корректной работы Hermes он должен совпадать с адресом rpc\_address.</mark>
{% endhint %}

### Создание IBC-клиента/соединения/канала между экземплярами

В Hermes CLI имеются команды для их создания. Перед созданием узел каждого экземпляра должен быть запущен по указанным rpc-адресам. Если у вас нет узлов, установите их вручную или с помощью [наших скриптов](ibc-relayers.md#nastroika-lokalnykh-ekzemplyarov-namada-s-pomoshyu-skripta-hermes).

#### Экспорт переменных окружения

Пользователю, осуществляющему ретрансляцию, необходимо сохранить некоторые переменные окружения. К ним относятся:

```rust
export CHAIN_A_ID="<replace-with-chain-a-id>"
export CHAIN_B_ID="<replace-with-chain-b-id>"
export HERMES_CONFIG="<replace-with-hermes-config-path>"
```

### Установка Hermes

Перед проведением любых операций с IBC необходимо загрузить бинарный файл форка Hermes компании Heliax или собрать его из исходных файлов.

#### Из бинарных файлов

Вы можете загрузить последний бинарный релиз с[ нашей страницы релизов](https://github.com/heliaxdev/hermes/releases), выбрав соответствующую архитектуру.

Например.

```rust
export TAG="v1.6.0-namada-beta3"
export ARCH="x86_64-unknown-linux-gnu" # or "aarch64-apple-darwin"
curl -Lo /tmp/hermes.tar.gz https://github.com/heliaxdev/hermes/releases/download/${TAG}/hermes-${TAG}-${ARCH}.tar.gz
tar -xvzf /tmp/hermes.tar.gz -C /usr/local/bin
```

{% hint style="info" %}
<mark style="color:blue;">В некоторых системах каталог /usr/local/bin является защищенным. В этом случае может потребоваться выполнить приведенную выше команду с правами sudo. Т.е.</mark>

```
sudo tar -xvzf /tmp/hermes.tar.gz -C /usr/local/bin
```

<mark style="color:blue;">Это справедливо и для команды cp ./target/release/hermes /usr/local/bin/, приведенной ниже (см. комментарий).</mark>
{% endhint %}

### Из источника

```rust
export TAG="v1.6.0-namada-beta3"
 
git clone https://github.com/heliaxdev/hermes.git
git checkout $TAG
cd hermes
cargo build --release --bin hermes
export HERMES=$(pwd) # if needed
```

Проверьте двоичный код:

```rust
./target/release/hermes --version #or sudo cp ./target/release/hermes /usr/local/bin/
```

{% hint style="info" %}
<mark style="color:blue;">Теперь рекомендуется добавить hermes в $PATH, чтобы он вызывался без каких-либо префиксов. Для пользователей ubuntu это можно сделать следующим образом</mark>

```
cp ./target/release/hermes /usr/local/bin/
```
{% endhint %}

### Настройка ретранслятора

#### Создайте каталог `namada_wallet` и цепочку каталогов для хранения каждого кошелька ретранслятора wallet.toml

Для работы ретранслятора необходимо иметь каталог кошелька для хранения ключей ретранслятора. Это можно сделать, выполнив команду:

```rust
# in the Hermes folder
mkdir namada_wallet
mkdir -p ~/.hermes/namada_wallet/$CHAIN_A_ID
mkdir -p ~/.hermes/namada_wallet/$CHAIN_B_ID
```

{% hint style="info" %}
<mark style="color:blue;">This step is only needed for namada chains. For cosmos based chains, it is advised to add the key directly to hermes.</mark>

```rust
./hermes --config $HERMES_CONFIG keys add --chain "<name-of-chain>" --key-file "<path-to-key>" --overwrite
```
{% endhint %}

### Создание учетной записи ретранслятора

В каждой цепочке должна существовать учетная запись ретранслятора. На цепочке namada это можно сделать, выполнив команду:

```rust
namadaw key gen --alias relayer
```

В результате будет сгенерирован ключ для учетной записи ретранслятора. Ключ будет храниться в файле `wallet.toml`, который находится в [базовом каталоге узла](zapusk-polnogo-uzla/bazovyi-katalog.md), в папке `chain-id`. Например, если `chain-id - namada-test.0a4c6786dbda39f786`, то `wallet.toml` будет находиться в каталоге `$HOME/.local/share/namada/namada-test.0a4c6786dbda39f786/wallet.toml` (на машине ubuntu, где `base-dir` не был настроен должным образом).

Теперь необходимо скопировать этот файл кошелька в каталог `namada_wallet`, который был создан выше, для каждой цепочки. Продолжая этот пример, первый кошелек можно скопировать, выполнив команду:

```rust
cp $HOME/.local/share/namada/$CHAIN_A_ID/wallet.toml ~/.hermes/namada_wallet/$CHAIN_A_ID/wallet.toml
# Make sure this is done for both wallets on each chain!
```

Теперь можно приступить к настройке клиента.

### Создание канала IBC

Команда "создать канал" (см. ниже) создает не только канал IBC, но и необходимое клиентское соединение IBC.

```rust
hermes --config $HERMES_CONFIG \
  create channel \
  --a-chain $CHAIN_A_ID \
  --b-chain $CHAIN_B_ID \
  --a-port transfer \
  --b-port transfer \
  --new-client-connection --yes
```

{% hint style="info" %}
<mark style="color:blue;">Обратите внимание, что указанные выше CHAIN\_ID будут зависеть от вашей собственной установки, поэтому проверьте это сами!</mark>
{% endhint %}

После завершения создания можно увидеть идентификаторы каналов. Например, в следующем тексте показано, что в цепи A `namada-test.0a4c6786dbda39f786` создан канал с идентификатором `7`, а в цепи B `namada-test.647287156defa8728c` создан канал с идентификатором `12`. Идентификаторы каналов понадобятся для передачи данных по IBC. Это означает, что для передачи данных из цепочки A в цепочку B необходимо указать в качестве идентификатора канала канал-7 (префикс `channel-` всегда обязателен), а для передачи данных из цепочки B в цепочку A - канал-12.

```rust
SUCCESS Channel {
    ordering: Unordered,
    a_side: ChannelSide {
        chain: BaseChainHandle {
            chain_id: ChainId {
                id: "namada-test.0a4c6786dbda39f786",
                version: 0,
            },
            runtime_sender: Sender { .. },
        },
        client_id: ClientId(
            "07-tendermint-0",
        ),
        connection_id: ConnectionId(
            "connection-3",
        ),
        port_id: PortId(
            "transfer",
        ),
        channel_id: Some(
            ChannelId(
                "channel-7",
            ),
        ),
        version: None,
    },
    b_side: ChannelSide {
        chain: BaseChainHandle {
            chain_id: ChainId {
                id: "namada-test.647287156defa8728c",
                version: 0,
            },
            runtime_sender: Sender { .. },
        },
        client_id: ClientId(
            "07-tendermint-1",
        ),
        connection_id: ConnectionId(
            "connection-2",
        ),
        port_id: PortId(
            "transfer",
        ),
        channel_id: Some(
            ChannelId(
                "channel-12",
            ),
        ),
        version: None,
    },
    connection_delay: 0ns,
}
```

### Запуск ретранслятора

После запуска Hermes осуществляет мониторинг экземпляров через узлы и ретранслирует пакеты в соответствии с отслеживаемыми событиями.

```rust
hermes --config $HERMES_CONFIG start
```

Более подробную информацию о Hermes можно найти [в официальном документе](https://hermes.informal.systems/).

После синхронизации вы можете создать канал и запустить Hermes, как описано выше.

```rust
# create a channel
hermes --config $HERMES_CONFIG \
  create channel \
  --a-chain $CHAIN_A_ID \
  --b-chain $CHAIN_B_ID \
  --a-port transfer \
  --b-port transfer \
  --new-client-connection --yes
```

#### Перевод активов через IBC

Появилась возможность [перевода активов между двумя цепочками](../rukovodstvo-polzovatelya/perevod-aktivov-cherez-ibc.md).

### Настройка локальных экземпляров Namada с помощью скрипта hermes

Скрипт `setup-namada` создаст два экземпляра с одним узлом валидатора, скопирует необходимые файлы для Hermes и создаст счет для Hermes на каждом ledger. Кроме того, в каталоге hermes будет создан файл конфигурации `Hermes config_for_namada.toml`.

Для начала необходимо экспортировать некоторые переменные окружения:

```
export NAMADA_DIR="<path-to-namada-source-directory>"export TAG="v1.6.0-namada-beta3"
```

```rust
git clone https://github.com/heliaxdev/hermes.gitgit checkout $TAG # The branch is the same as our Hermescd hermes./scripts/setup-namada $NAMADA_DIR $CHAIN_ID_A $CHAIN_ID_B
```

В этом случае пользователю не нужно ждать синхронизации. Если на счету ретранслятора на каждом экземпляре имеется достаточный баланс, пользователь может создать канал и сразу же запустить Hermes, как было описано выше. Идентификаторы цепочек инстансов пользователь находит в конфигурационном файле `config_for_namada.toml`. Можно выполнить команду `grep "id" ${HERMES_CONFIG}`.

```rust
# create a channel
hermes --config $HERMES_CONFIG \
  create channel \
  --a-chain $CHAIN_A_ID \
  --b-chain $CHAIN_B_ID \
  --a-port transfer \
  --b-port transfer \
  --new-client-connection --yes
 
# Run Hermes
hermes --config $HERMES_CONFIG start
```

Файлы данных и конфигурации каждого узла находятся в папке `hermes/data/namada-*/.namada.`

Для того чтобы закрыть все ledger, настроенные скриптом, можно выполнить команду:

```rust
killall namadan
```
