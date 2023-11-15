# Отправка и получение токенов NAM

В Namada токены реализованы в виде счетов с [предикатом валидности токена](https://github.com/anoma/namada/blob/9b67281e359ebff5467cad57c866fbcf91eb80c8/shared/src/ledger/native\_vp/multitoken.rs#L30). Предикат валидности (VP) проверяет, в частности, что общий запас (токена) сохраняется в любой транзакции, в которой используется данный токен. Ваш кошелек будет предварительно загружен некоторыми адресами токенов, которые инициализируются в блоке genesis.

### Инициализация существующего счета

Если у вас уже есть ключ в кошельке, этот шаг можно пропустить. В противном случае сгенерируйте [новую пару ключей](./) прямо сейчас.

Затем отправьте транзакцию для инициализации нового созданного счета и сохраните его адрес с псевдонимом `establishment`. Открытый ключ `keysha` будет записан в хранилище счета для авторизации последующих транзакций. Эту транзакцию мы также подписываем ключом `keysha`.

```rust
namada client init-account \
  --alias establishment \
  --public-keys keysha \
  --signing-keys keysha \
  --threshold 1
```

После применения этой транзакции клиент автоматически увидит новый адрес, созданный в результате транзакции, и добавит его в кошелек с выбранным псевдонимом `establishment`. Данная команда использует готовый предикат [User Validity Predicate.](https://github.com/anoma/namada/blob/main/wasm/wasm\_source/src/vp\_user.rs)

### Отправить платеж

Чтобы отправить регулярный перевод токенов со своего счета на адрес `validator-1`:

```rust
namada client transfer \
  --source establishment \
  --target validator-1 \
  --token NAM \
  --amount 10 \
  --signing-keys keysha
```

Эта команда попытается найти и использовать ключ адреса источника для подписания транзакции.

### Посмотреть баланс

Запрос баланса токенов для конкретного токена и/или владельца:

```rust
namada client balance --token NAM --owner my-new-acc
```

{% hint style="info" %}
<mark style="color:blue;">Для любой клиентской команды, отправляющей транзакцию (</mark><mark style="color:blue;">`init-account`</mark><mark style="color:blue;">,</mark> <mark style="color:blue;">`transfer, tx`</mark><mark style="color:blue;">,</mark> <mark style="color:blue;">`update`</mark> <mark style="color:blue;">и PoS-транзакции), можно использовать флаг</mark> <mark style="color:blue;">`--dry-run-wrapper`</mark><mark style="color:blue;">, чтобы имитировать применение транзакции в блоке и посмотреть, что получится в результате</mark>
{% endhint %}

### Просмотр баланса всех известных адресов

При запросе баланса всех токенов можно увидеть адреса токенов, известных клиенту:

```rust
namada client balance
```
