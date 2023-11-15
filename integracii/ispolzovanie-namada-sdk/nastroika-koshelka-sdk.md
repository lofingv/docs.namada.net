# Настройка кошелька SDK

### Соответствующий импорт

#### Общий импорт

```rust
// namada_sdk::Namada is a high level interface in order to interact with the Namada SDK
use namada_sdk::Namada;
// The NamadaImpl provides convenient implementations to frequently used Namada SDK interactons
use namada_sdk::NamadaImpl;
```

#### Импорт по кошельку

```rust
// SecretKey, common and SchemeType give access to Namada cryptographic keys and their relevant implementations. Namada supports ED25519 and SECP256K1 keys.
use namada_sdk::core::types::key::common::SecretKey;
use namada_sdk::core::types::key::{common, SchemeType};
// Filesystem wallet utilities (stores the path of the wallet on the filesystem)
use namada_sdk::wallet::fs::FsWalletUtils;
```

### Создание кошелька из мнемоники

SDK позволяет создать кошелек из мнемонической фразы. Мнемоническая фраза - это фраза из 24 слов, которая может быть использована для восстановления кошелька.

```rust
let mnemonic = Mnemonic::from_phrase(MNEMONIC_CODE, namada_sdk::bip39::Language::English)
```

```rust
// Assuming a cometbft node is running on localhost:26657
let http_client = HttpClient::new("http://localhost:26657").unwrap();
// Assuming wallet.toml exists in the current directory
let mut wallet = FsWalletUtils::new(PathBuf::from("wallet.toml"));
// The key can be generated from the wallet by passing in the mnemonic phrase
let (_key_alias, sk) = NamadaImpl::new(&http_client, &mut wallet, &mut shielded_ctx, &NullIo)
            .wallet_mut()
            .await
            .derive_key_from_user_mnemonic_code(
                SchemeType::Ed25519,
                Some(alias),
                false,
                Some(derivation_path),
                Some((mnemonic.clone(), Zeroizing::new("".to_owned()))),
                None,
            )
            .expect("unable to derive key from mnemonic code")
            .unwrap();
```

Во второй части приведенной выше функции ключ получается из мнемонической фразы. Псевдоним - это имя ключа, который будет храниться в кошельке. `derivation_path` - путь к ключу в кошельке HD. Мнемоника - мнемоническая фраза, сгенерированная ранее. `shielded_ctx` - контекст для экранированных транзакций. `NullIo` - контекст ввода-вывода для кошелька.

### Создание нового кошелька и сохранение его в файловой системе

Также можно создать sdk-кошелек с нуля. Это более сложный процесс, поскольку требуется сгенерировать новое хранилище, в котором будет существовать кошелек.

```rust
use std::path::PathBuf;
 
use namada::{
    sdk::wallet::{
        alias::Alias, ConfirmationResponse, GenRestoreKeyError, Store, StoredKeypair, Wallet,
        WalletUtils,
    },
    types::{
        address::Address,
        key::{common::SecretKey, PublicKeyHash},
    },
};
use rand::rngs::OsRng;
 
pub struct SdkWallet {
    pub wallet: Wallet<SdkWalletUtils>,
}
 
impl SdkWallet {
    pub fn new(sk: SecretKey, nam_address: Address) -> Self {
        let store = Store::default();
        let mut wallet = Wallet::new(PathBuf::new(), store);
        let stored_keypair = StoredKeypair::Raw(sk.clone());
        let pk_hash = PublicKeyHash::from(&sk.to_public());
        let alias = "alice".to_string();
        wallet.insert_keypair(alias, stored_keypair, pk_hash, true);
        wallet.add_address("nam", nam_address, true);
        Self { wallet }
    }
}
 
pub struct SdkWalletUtils {}
 
impl WalletUtils for SdkWalletUtils {
    type Storage = PathBuf;
 
    type Rng = OsRng;
 
    fn read_decryption_password() -> zeroize::Zeroizing<std::string::String> {
        panic!("attempted to prompt for password in non-interactive mode");
    }
 
    fn read_encryption_password() -> zeroize::Zeroizing<std::string::String> {
        panic!("attempted to prompt for password in non-interactive mode");
    }
 
    fn read_alias(_prompt_msg: &str) -> std::string::String {
        panic!("attempted to prompt for alias in non-interactive mode");
    }
 
    fn read_mnemonic_code() -> std::result::Result<namada::bip39::Mnemonic, GenRestoreKeyError> {
        panic!("attempted to prompt for mnemonic in non-interactive mode");
    }
 
    fn read_mnemonic_passphrase(_confirm: bool) -> zeroize::Zeroizing<std::string::String> {
        panic!("attempted to prompt for mnemonic in non-interactive mode");
    }
 
    fn show_overwrite_confirmation(
        _alias: &Alias,
        _alias_for: &str,
    ) -> namada::sdk::wallet::store::ConfirmationResponse {
        // Automatically replace aliases in non-interactive mode
        ConfirmationResponse::Replace
    }
}
```

Приведенный выше код позволяет нам теперь создать любой экземпляр `SdkWalle`t, просто передав секретный ключ и адрес токена `NAM`. Если мы хотим осуществлять переводы с помощью других токенов, то необходимо добавить и эти адреса.
