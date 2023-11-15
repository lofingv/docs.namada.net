# Конструирование трансферов

Теперь, когда кошелек и клиент настроены, мы можем создать среду, необходимую для создания переводов. Это может быть несколько сложно, но приведенный ниже шаблонный код должен помочь: Для генерации переводов потребуются следующие импорты:

```rust
use namada_sdk::args::InputAmount;
```

После того как пользователь[ создал учетную запись](generaciya-schetov.md) и соответствующую структуру, cоздать и отправить транзакции перевода не составляет особого труда.

```rust
let mut namada = NamadaImpl::new(&http_client, &mut wallet, &mut shielded_ctx, &NullIo)
        .await
        .expect("unable to construct Namada object")
        .chain_id(ChainId::from_str("public-testnet-14.5d79b6958580").unwrap());
// Transfer the given amount of native tokens from the source account to the
// destination account.
async fn gen_transfer<'a>(
    namada: &impl Namada<'a>,
    source: &Account,
    destination: &Account,
    amount: InputAmount,
) -> std::result::Result<ProcessTxResponse, namada_sdk::error::Error> {
    let mut transfer_tx_builder = namada
        .new_transfer(
            TransferSource::Address(Address::from(&source.public_key)),
            TransferTarget::Address(Address::from(&destination.public_key)),
            namada.native_token(),
            amount,
        )
        .signing_keys(vec![source.private_key.clone()]);
    let (mut transfer_tx, signing_data, _epoch) = transfer_tx_builder
        .build(namada)
        .await
        .expect("unable to build transfer");
    namada
        .sign(&mut transfer_tx, &transfer_tx_builder.tx, signing_data)
        .await
        .expect("unable to sign reveal pk tx");
    namada.submit(transfer_tx, &transfer_tx_builder.tx).await
}
```

Аналогичным образом могут быть построены и другие транзакции.

### Экранированные переводы

Для того чтобы сделать передачу экранированной, необходимо вместо прозрачных адресов и ключей использовать экранированные.

Важно, чтобы в качестве источника использовался экранированный расширенный ключ `SpendingKey`.

```rust
use namada::types::masp::{ExtendedSpendingKey, PaymentAddress, TransferSource, TransferTarget};
 
// Make sure to replace 'secret-ex' with an actual Namada extended spending key
let source = ExtendedSpendingKey::from_str("secret-ex").unwrap();
// Make sure to replace "payment-addr-ex" with an actual Namada payment address
let destination = PaymentAddress::from_str("payment-addr-ex").unwrap();
let fee_payer = 
let mut transfer_tx_builder = namada
        .new_transfer(
            TransferSource::ExtendedSpendingKey(source),
            TransferTarget::Address(Address::from(&destination.public_key)),
            namada.native_token(),
            amount,
        )
        // Make sure to replace "transparent-address-ex" with an actual Namada transparent address
        .signing_keys(vec![Address::from_str("transparent-address-ex").ok()]);
```
