# Использование SDK с интерфейсом Namada

Интерфейс Namada - [это веб-интерфейс](https://github.com/anoma/namada-interface), разработанный фондом Anoma.

Он позволяет легко создавать и управлять собственной децентрализованной идентификацией, в том числе интегрироваться с приложением ledger.

Реализация интерфейса использует сочетание TypeScript и Rust для взаимодействия с SDK.

## Пример

```rust
// Import Sdk & Query
import { Query, Sdk } from "@namada/shared";
// Import wasm initialization function
import { init as initShared } from "@namada/shared/src/init";
 
async function init() {
    await initShared();
 
    const sdk = new Sdk(RPC_URL_OF_NAM_NODE);
    const query = new Query(RPC_URL_OF_NAM_NODE);
 
  // ...
}
 
init();
```

### Документация

Для получения более подробной информации ознакомьтесь с документацией, размещенной здесь.
