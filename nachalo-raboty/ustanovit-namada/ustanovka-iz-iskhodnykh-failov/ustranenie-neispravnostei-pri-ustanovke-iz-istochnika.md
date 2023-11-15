# Устранение неисправностей при установке из источника

### Недостаточно оперативной памяти

[Локальная сборка двоичных файлов](./) - задача, требующая больших вычислительных затрат и требующая от вашего компьютера больших усилий. Для компиляции обычно требуется не менее 16 Гбайт оперативной памяти, а в зависимости от оптимизации вашей машины может потребоваться и больше (для некоторых машин - чуть меньше). По этой причине компиляция иногда может не выполняться.

Ошибка:

```
 src/apps/namada lib could not compile due to previous errors. Exited with exit code:
```

является распространенной ошибкой, которая иногда означает, что при компиляции на компьютере закончилась память. Чтобы решить эту проблему, нужно закрыть все другие приложения и перекомпилировать один или два раза. В противном случае потребуется больше оперативной памяти.

### Компиляция в первый раз

Ошибки компиляции, связанные с отсутствием библиотек при первой сборке двоичных файлов, могут быть распространенной проблемой.

### Linker "CC" not found

Если вы столкнулись с ошибкой:

```
Entering directory '/root/namada/wasm/wasm_source'
RUSTFLAGS='-C link-arg=-s'  cargo build --release --target wasm32-unknown-unknown --target-dir 'target' --features tx_bond && \
cp "./target/wasm32-unknown-unknown/release/namada_wasm.wasm" ../tx_bond.wasm
   Compiling proc-macro2 v1.0.46
   Compiling quote v1.0.21
error: linker `cc` not found
  |
  = note: No such file or directory (os error 2)
error: could not compile `quote` due to previous error
warning: build failed, waiting for other jobs to finish...
error: could not compile `proc-macro2` due to previous error
```

Проблему можно решить, выполнив команду:

```rust
sudo apt install build-essential
```

Другим решением иногда может быть установка `libcland-dev`. Этого можно добиться с помощью:

```rust
sudo apt-get update -y
sudo apt-get install -y libclang-dev
```

### **WASM32-unknown-unknown**

Еще одна проблема, с которой может столкнуться компилятор, - это невозможность найти цель `wasm32-unknown-unknown`.

```
error[E0463]: can't find crate for `core`
  |
  = note: the `wasm32-unknown-unknown` target may not be installed
  = help: consider downloading the target with `rustup target add wasm32-unknown-unknown`
error[E0463]: can't find crate for `compiler_builtins`
For more information about this error, try `rustc --explain E0463`.
error: could not compile `cfg-if` due to 2 previous errors
```

Эта проблема может быть решена путем выполнения команды:

```rust
rustup target add wasm32-unknown-unknown
```

(Да, имя цели - `wasm32-unknown-unknown`. Это не компилятор не может определить версию/релиз).

**OpenSSL**

Если вы столкнулись с ошибкой

```
Could not find directory of OpenSSL installation, and this `-sys` crate cannot
  proceed without this knowledge. If OpenSSL is installed and this crate had
  trouble finding it,  you can set the `OPENSSL_DIR` environment variable for the
  compilation process.
  Make sure you also have the development packages of openssl installed.
  For example, `libssl-dev` on Ubuntu or `openssl-devel` on Fedora.
  If you're in a situation where you think the directory *should* be found
  automatically, please open a bug at https://github.com/sfackler/rust-openssl
  and include information about your system as well as this message.
```

Тогда необходимо установить пакеты разработки OpenSSL. Для Ubuntu это `libssl-dev`. Для Fedora это `openssl-devel`. Для других дистрибутивов обратитесь [к сайту OpenSSL.](https://www.openssl.org/)

Для Ubuntu это можно сделать следующим образом:

```rust
sudo apt-get install libssl-dev
```
