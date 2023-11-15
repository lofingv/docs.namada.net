# Генерация счетов

### Представление счетов

Представление счетов с помощью Namada SDK очень простое. Счет в Namada определяется его открытым ключом (ключами) и закрытым ключом (закрытыми ключами) (множественное число для мультиподписей). Открытый ключ используется для идентификации счета, а закрытый ключ - для подписания транзакций. В приведенном ниже фрагменте мы представляем счет с помощью открытого и закрытого ключей.

```rust
use namada_sdk::core::types::key::common::{PublicKey, SecretKey};
struct SimpleAccount {
    public_key: PublicKey,
    private_key: SecretKey
}
```

Для счета с несколькими подписями мы можем представить это в виде вектора ключей.

```rust
use namada_sdk::core::types::key::common::{PublicKey, SecretKey};
struct MultisigAccount {
    public_keys: Vec<PublicKey>,
    private_keys: Vec<SecretKey>
}
```

Счета с несколькими подписями, поскольку они инициализируются транзакцией на цепи, всегда будут иметь открытый ключ. Однако если пары ключей генерируются в автономном режиме, то для раскрытия открытого ключа пользователю необходимо провести транзакцию. В связи с этим полезно добавить поле `revealed` в структуру счета.

```rust
use namada_sdk::core::types::key::common::{PublicKey, SecretKey};
struct Account {
    public_key: PublicKey,
    private_key: SecretKey,
    revealed: bool
}
```

### Раскрытие открытого ключа implicit счета

Для того чтобы раскрыть открытый ключ implicit счета, пользователь должен отправить транзакцию.

```rust
use namada_sdk::io::NullIo;
use namada_sdk::NamadaImpl;
use namada_sdk::core::types::chain::ChainId;
 
 
// Define the namada implementation (assuming we have a wallet, http_client, and shielded_ctx)
let mut namada = NamadaImpl::new(&http_client, &mut wallet, &mut shielded_ctx, &NullIo)
        .await
        .expect("unable to construct Namada object")
        .chain_id(ChainId::from_str("public-testnet-14.5d79b6958580").unwrap());
 
// Generate an account (assuming sk is a SecretKey)
let account = Account {
            public_key: sk.to_public(),
            private_key: sk,
            revealed: false,
};
 
// Build the reveal pk transaction using the NamadaImpl object
let reveal_tx_builder = namada
    .new_reveal_pk(account.public_key.clone())
    .signing_keys(vec![account.private_key.clone()]);
let (mut reveal_tx, signing_data, _) = reveal_tx_builder
    .build(namada)
    .await
    .expect("unable to build reveal pk tx");
// Sign the transaction
namada
    .sign(&mut reveal_tx, &reveal_tx_builder.tx, signing_data)
    .await
    .expect("unable to sign reveal pk tx");
// Submit the signed tx to the ledger for execution
namada.submit(reveal_tx.clone(), reveal_tx_builder)
```

После раскрытия открытого ключа учетная запись может быть использована для подписания транзакций.ru
