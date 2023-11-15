# Заявка на получение статуса валидатора генезиса

Перед запуском тестовой сети вы можете подать заявку на участие в качестве валидатора генезиса.

### Настройка

Следуйте данному руководству по созданию файлов [валидатора "до генезиса"](../../rukovodstvo-dlya-operatorov/validatory-namada/nastroika-validatora-genesis.md).

После этого у вас будет файл `validator.toml`, содержимое которого будет выглядеть следующим образом:

```rust
[validator.1337-validator]
consensus_public_key = "00056fff5232da385d88428ca2bb2012a4d83cdf5c697864dde34b393333a72268"
eth_cold_key = "0103d985a8345ef505cf139c3dfbd5f5a1da73d2864c62ce9d0a98da73898a59f6f9"
eth_hot_key = "010241bb691e44dd3c4263522474e45751e97307e92326250f96c1bcd0a06880875d"
account_public_key = "00f1bd321be2e23b9503653dd50fcd5177ca43a0ade6da60108eaecde0d68abdc8"
protocol_public_key = "0054c213d2f8fe2dd3fc5a41a52fd2839cb49643d960d7f75e993202692c5d8783"
dkg_public_key = "6000000054eafa7320ddebf00c9487e5f7ea5107a8444f042b74caf9ed5679163f854577bf4d0992a8fd301ec4f3438c9934c617a2c71649178e536f7e2a8cdc1f8331139b7fd9b4d36861f0a9915d83f61d7f969219f0eba95bb6fa45595425923d4c0e"
commission_rate = "0.01"
max_commission_rate_change = "0.05"
net_address = "1.2.3.4:26656"
tendermint_node_key = "00e1a8fe1abceb700063ab4558baec680b64247e2fd9891962af552b9e49318d8d"
```

Этот файл содержит только открытую информацию и безопасен для публичного доступа.

{% hint style="warning" %}
<mark style="color:orange;">Поле tendermint\_node\_key названо так из соображений наследования. На самом деле это открытый ключ консенсуса CometBFT.</mark>
{% endhint %}

### Предоставление конфигурации

Если вы хотите стать валидатором genesis для тестовой сети, пожалуйста, сделайте pull request на [https://github.com/anoma/namada-testnets](https://github.com/anoma/namada-testnets), добавив свой файл `validator.toml` в соответствующую директорию (например, `namada-public-testnet-2` для второй публичной тестовой сети), переименовав его в `$alias.tom`l. Например, если вы выбрали для себя псевдоним "bertha", то отправьте файл с именем `bertha.toml`. Пример [PR можно посмотреть здесь.](https://github.com/anoma/namada-testnets/pull/29)

### Ожидание `CHAIN_ID`

Дождитесь распространения соответствующего `CHAIN_ID`.
