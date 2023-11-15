# Запустите свой узел в качестве валидатора genesis

После получения CHAIN\_ID можно присоединиться к тестовой сети. Если присоединяющийся узел зарегистрирован в файле genesis как genesis-validator, он сможет участвовать в консенсусе и производить блоки с самого начала цепи.

### Присоединение к сети

Будучи генезис-валидатором, можно присоединиться к сети с распределенным идентификатором `CHAIN_ID`. Допустим, этот `CHAIN_ID - namada-mainnet`.

В этом случае валидатор генезиса может присоединиться к сети с:

```rust
export CHAIN_ID="namada-mainnet"
namada client utils join-network \
--chain-id $CHAIN_ID --genesis-validator $ALIAS
```

#### Запуск узла и синхронизация

```rust
NAMADA_LOG=info CMT_LOG_LEVEL=p2p:none,pex:error NAMADA_CMT_STDOUT=true namada node ledger run
```

Дополнительно: Если требуется большее количество журналов, можно вместо этого выполнить команду

```rust
NAMADA_LOG=debug CMT_LOG_LEVEL=p2p:none,pex:error NAMADA_CMT_STDOUT=true namada node ledger run
```

А если необходимо сохранить журналы в файл, то можно выполнить команду:

```rust
TIMESTAMP=$(date +%s)
NAMADA_LOG=debug CMT_LOG_LEVEL=p2p:none,pex:error NAMADA_CMT_STDOUT=true namada node ledger run &> logs-${TIMESTAMP}.txt
tail -f -n 20 logs-${TIMESTAMP}.txt ## (in another shell)
```

#### При правильном запуске вы должны увидеть следующий журнал:

`[<timestamp>] This node is a validator ...`
