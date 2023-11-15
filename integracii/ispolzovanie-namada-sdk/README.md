# Использование Namada SDK

Комплект разработки программного обеспечения (SDK) Namada можно найти в репо `namada` по пути [`namada/shared`](https://github.com/anoma/namada/tree/main/shared). SDK написан на языке Rust и может быть использован для взаимодействия с блокчейном Namada: создания транзакций, их подписания и отправки в сеть.

### Быстрый старт

Хорошей отправной точкой для ознакомления с SDK является [репо namada interface.](https://github.com/anoma/namada-interface/tree/main/packages/shared/lib/src/sdk) Этот репозиторий содержит простое веб-приложение, использующее SDK для взаимодействия с блокчейном Namada. Однако следует отметить дополнительную сложность, возникающую из-за интеграции в приложение javascript с помощью[ wasm-bindgen,](https://rustwasm.github.io/docs/wasm-bindgen/) что не является необходимым.

### Установка

Namada SDK можно установить, создав новый проект Rust и добавив в файл Cargo.toml следующее:

```rust
[package]
name = "namada-sdk-starter"
version = "0.1.0"
edition = "2021"
 
# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html
 
[dependencies]
async-std = "1.11.0"
async-trait = "0.1.51"
borsh = "0.9.0"
file-lock = "2.0.2"
futures = "0.3.28"
getrandom = { version = "0.2" }
masp_primitives = { git = "https://github.com/anoma/masp.git", rev = "50acc5028fbcd52a05970fe7991c7850ab04358e" }
masp_proofs = { git = "https://github.com/anoma/masp.git", rev = "50acc5028fbcd52a05970fe7991c7850ab04358e", features = ["download-params"]}
# Make sure the rev is to the latest version of namada in the below repo
namada_sdk = { git = "https://github.com/anoma/namada.git", rev = "v0.24.0", default-features = false, features = ["abciplus", "namada-sdk", "std"] }
rand = {version = "0.8", default-features = false}
rand_core = {version = "0.6", default-features = false}
tendermint-config = {git="https://github.com/heliaxdev/tendermint-rs.git", rev="b7d1e5afc6f2ccb3fd1545c2174bab1cc48d7fa7"}
tendermint-rpc = {git="https://github.com/heliaxdev/tendermint-rs.git", rev="b7d1e5afc6f2ccb3fd1545c2174bab1cc48d7fa7", features = ["http-client"]}
thiserror = "1.0.38"
tokio = {version = "1.8.2", default-features = false}
toml = "0.5.8"
zeroize = "1.5.5"
 
[patch.crates-io]
borsh = {git = "https://github.com/heliaxdev/borsh-rs.git", rev = "cd5223e5103c4f139e0c54cf8259b7ec5ec4073a"}
borsh-derive = {git = "https://github.com/heliaxdev/borsh-rs.git", rev = "cd5223e5103c4f139e0c54cf8259b7ec5ec4073a"}
borsh-derive-internal = {git = "https://github.com/heliaxdev/borsh-rs.git", rev = "cd5223e5103c4f139e0c54cf8259b7ec5ec4073a"}
borsh-schema-derive-internal = {git = "https://github.com/heliaxdev/borsh-rs.git", rev = "cd5223e5103c4f139e0c54cf8259b7ec5ec4073a"}
 
```

После установки sdk можно использовать его для взаимодействия с блокчейном Namada.

### Оглавление:

* [Настройка клиента](nastroika-klienta-sdk.md)
* [Настройка кошелька](nastroika-koshelka-sdk.md)
* [Создание переводов](konstruirovanie-transferov.md)
