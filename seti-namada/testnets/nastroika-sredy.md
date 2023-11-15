# Настройка среды

{% hint style="info" %}
<mark style="color:blue;">Если вы не хотите собирать Namada из исходных файлов, вы можете установить Namada из двоичных файлов. Обратите внимание, что сборка из исходных файлов может быть сложным процессом и не рекомендуется для новичков.</mark>
{% endhint %}

Экспортируйте следующие переменные:

```rust
export NAMADA_TAG=v0.23.1
```

### Установка Namada

#### Установите предварительнае компоненты

* [Rust](https://www.rust-lang.org/tools/install)
* [CometBFT](../../nachalo-raboty/ustanovka-cometbft.md)
* [Protobuf](../../nachalo-raboty/ustanovit-namada/ustanovka-iz-binarnykh-failov/predvaritelnye-komponenty.md)

#### Клонируйте репозиторий namada и установите нужные версии

```
git clone https://github.com/anoma/namada && cd namada && git checkout $NAMADA_TAG
```

#### Сборка двоичных файлов

```rust
make install
```

* Возможно, потребуется установить некоторые дополнительные требования (linux):

```rust
sudo apt-get update -ysudo apt-get install build-essential make pkg-config libssl-dev libclang-dev -ycurl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

### Установка CometBFT

#### Инструкции по установке CometBFT см. в разделе [Установка CometBFT здесь.](../../nachalo-raboty/ustanovka-cometbft.md)

#### Скопируйте двоичные файлы namada и CometBFT куда-нибудь в $PATH (или используйте относительные пути). Этот шаг может потребоваться, а может и не потребоваться.

* Бинарные файлы namada можно найти в `/target/release`
* CometBFT скорее всего, в `$HOME/Downloads/cometbft`

### Проверьте портв

#### Откройте эти порты:

* 26656
* 26657

#### Чтобы проверить, открыты ли порты, можно настроить простой сервер и просмотреть порт с другого хоста

* внутри папки namada выполните команду

```rust
{ printf 'HTTP/1.0 200 OK\r\nContent-Length: %d\r\n\r\n' "$(wc -c < namada)"; cat namada; } | nc -l $PORT
```

* С другого хоста выполните одну из двух команд:
  * `nmap $IP -p$PORT`
  * `curl $IP:$PORT >/dev/null`

#### Проверка установки

* Убедитесь, что вы используете правильную версию CometBFT
  * `cometbft version` должен выводить `0.37.2`
* Убедитесь, что вы используете правильную версию Namada
  * `namada --version` должен выводить `Namada v0.23.1`
