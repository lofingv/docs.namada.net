# Перевод активов через IBC

Выполнить ibc-передачу можно с помощью Namada cli, выполнив команду `namadac ibc-transfer`. Предполагается, что для этого должен быть создан канал и запущен Hermes с правильной конфигурацией на двух узлах.

Для того чтобы выполнить IBC-передачу с помощью команды `ibc-transfer` в Namada, нам необходимо знать `base-dir` и ~~node~~ каждого экземпляра (а также другие параметры передачи). `base-dir` - это базовый каталог каждого узла, подробнее см. в разделе [базовый каталог](../rukovodstvo-dlya-operatorov/zapusk-polnogo-uzla/bazovyi-katalog.md). `node` - это `rpc_add`r ретранслятора. Вы можете выполнить команду:

```
grep "rpc_addr" ${HERMES_CONFIG}
```

для поиска адреса.

{% hint style="warning" %}
<mark style="color:orange;">Только для локального узла</mark>

<mark style="color:orange;">Чтобы найти адрес своей для цепочки A, можно выполнить следующую команду</mark>

```rust
export BASE_DIR_A = "${HERMES}/data/namada-a/.namada"
export LEDGER_ADDRESS_A = "$(grep "rpc_address" ${BASE_DIR_A}/${CHAIN_A_ID}/setup/validator-0/.namada/${CHAIN_A_ID}/config.toml)"
```
{% endhint %}

Идентификатор канала для этой цепочки будет зависеть от порядка создания каналов. Поскольку мы открыли только один канал, идентификатор канала - `channel-0`, но по мере создания новых каналов он будет увеличиваться на индекс, возрастающий на 1. Идентификатор канала должен передаваться ретранслятором.

Предполагая, что открытый канал - `channel-0`, можно сохранить его в переменной окружения, выполнив команду:

```
export CHANNEL_ID = "channel-0"
```

Межблокчейновые переводы из цепочки A могут быть осуществлены следующим образом:

```rust
namadac --base-dir ${BASE_DIR_A}
    ibc-transfer \
        --amount ${AMOUNT} \
        --source ${SOURCE_ALIAS} \
        --receiver ${RECEIVER_RAW_ADDRESS} \
        --token ${TOKEN_ALIAS} \
        --channel-id ${CHANNEL_ID} \
        --node ${LEDGER_ADDRESS_A}
```

Где указанные выше переменные в `${VARIABLE}` должны быть заменены соответствующими значениями. Необработанный адрес приемника можно найти по команде `namadaw --base-dir ${BASE_DIR_B} address find --alias ${RECEIVER`}.

Например:

```rust
namadac --base-dir ${BASE_DIR_A}
    ibc-transfer \
    --amount 100 \
    --source albert \
    --receiver atest1d9khqw36g56nqwpkgezrvvejg3p5xv2z8y6nydehxprygvp5g4znj3phxfpyv3pcgcunws2x0wwa76 \
    --token nam \
    --channel-id channel-0 \
    --node 127.0.0.1:27657
```

После отправки транзакции ретранслятор должен передать пакет в другую цепочку. Это делается автоматически ретранслятором, работающим под управлением Hermes. Если пакет так и не был успешно передан, средства возвращаются отправителю по истечении таймаута. Более подробная информация приведена в спецификации.

### Перенос активов обратно из цепочек на базе Cosmos-SDK

При переходе на цепочку, основанную на Cosmos-SDK, ibc-передача выполняется, как описано выше. Однако при обратном переносе из цепочки на базе Cosmos, очевидно, команда `namadac ibc-transfer` работать не будет. Вместо нее следует использовать команду[ gaiad.](https://github.com/cosmos/gaia)

```rust
gaiad tx ibc-transfer transfer transfer ${CHANNEL_ID} ${RECEIVER_RAW_ADDRESS} ${AMOUNT}${IBC_TOKEN_ADDRESS} --from ${COSMOS_ALIAS} --node ${COSMOS_RPC_ENDPOINT} --fees 5000uatom
```

например:

```rust
gaiad tx ibc-transfer transfer transfer channel-0 atest1d9khqw368qcyx3jxxu6njs2yxs6y2sjyxdzy2d338pp5yd35g9zrv334gceng3z9gvmryv2pfdddt4 10ibc/281545A262215A2D7041CE1B518DD4754EC7097A1C937BE9D9AB6F1F11B452DD --from my-cosmos-address --node https://rpc.sentry-01.theta-testnet.polypore.xyz:443 --fees 5000uatom
```
