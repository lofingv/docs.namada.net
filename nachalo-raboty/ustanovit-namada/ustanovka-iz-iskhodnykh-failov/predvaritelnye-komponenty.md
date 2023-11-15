# Предварительные компоненты

Если вы хотите установить Namada из исходного кода, то сначала вам придется установить некоторые зависимости:

* [Rust](https://www.rust-lang.org/tools/install)
* [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* [Clang](https://clang.llvm.org/get\_started.html)
* [OpenSSL](https://www.openssl.org/source/)
* [LLVM](https://releases.llvm.org/download.html)

### Rust

По окончании установки убедитесь, что каталог `bin Cargo $HOME/.cargo/bin` доступен в переменной окружения PATH. Для продолжения работы можно либо перезапустить оболочку, либо выполнить команду `source $HOME/.cargo/env`.

Если у вас уже установлен Rust, убедитесь, что вы используете последнюю версию, выполнив команду:

```rust
rustup update
```

### Оставшиеся компоненты

Затем установите оставшиеся компоненты.

**Ubuntu:** выполнение следующей команды приведет к установке всего необходимого:

{% code overflow="wrap" %}
```rust
sudo apt-get install -y make git-core libssl-dev pkg-config libclang-12-dev build-essential protobuf-compiler
```
{% endcode %}

**Mac**: установка инструментов командной строки Xcode должна обеспечить вас практически всем необходимым:

```rust
xcode-select --install
```

`protoc` также требуется. На Mac его можно установить с помощью `Homebrew`:

```rust
brew install protobuf
```

При выполнении

```rust
protoc --version
```

Он должен выводить как минимум:

```rust
libprotoc 3.12.0
```

Другие варианты установки см. в документе [protoc-installation do](https://grpc.io/docs/protoc-installation/)
