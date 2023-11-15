# Индексатор Namada

В сотрудничестве с компанией[ Zondax](https://zondax.ch/) был создан индексатор для блокчейна Namada.

Индексатор Namada (он же `namadexer`) постоянно запрашивает данные блокчейна Namada и вместе с SDK способен отображать блоки, транзакции и другую ценную информацию в реляционной базе данных (postgres).

Это особенно удобно для проведения аналитических операций с блокчейном, в том числе для хранения исторических данных в удобном для запросов виде.

### Настройка

Исходный код индексатора namada можно [найти здесь](https://github.com/zondax/namadexer), и он прост в настройке.

Индексатор `namadexe`r лучше всего работает вместе с [Docker](../nachalo-raboty/ustanovit-namada/iz-docker.md)

```rust
git clone https://github.com/Zondax/namadexer.git
cd namadexer
make compose
```

### Запуск сервера и базы данных

После запуска DockerFile остается только настроить базу данных postgres, а также сервер, который будет запрашивать базу данных.

Убедитесь, что `postgres` [установлен на локальной машине](https://www.postgresql.org/download/).

#### Запустите postgres в docker

```rust
make postgres 
# or run (and change arguments, e.g port):
# docker run --name postgres -e POSTGRES_PASSWORD=wow -e POSTGRES_DB=blockchain -p 5432:5432 -d postgres
```

После того как сервер postgres запущен, необходимо настроить сервер, который будет выполнять запросы к базе данных postgres.

Для настройки сервера выполните следующую команду:

```rust
make run_server
```

В случае успеха сервер должен быть запущен как `daemon` на localhost по порту `30303`.

### Запуск индексатора

Прежде всего, убедитесь, что файл `Settings.toml` внутри `config/Settings.toml` настроен правильно.

```rust
log_level = "info"
network = "public-testnet-14"
 
[database]
host = "0.0.0.0:5435"
user = "postgres"
password = "wow"
dbname = "blockchain"
# Optional field to configure a timeout if database connection 
# fails.
connection_timeout = 20
 
 
[server]
serve_at = "0.0.0.0"
port = 30303
 
[indexer]
tendermint_addr = "0.0.0.0"
port = 26657
 
[jaeger]
enable = false
host = "localhost"
port = 6831
 
[prometheus]
host = "0.0.0.0"
port = 9000
```

{% hint style="warning" %}
<mark style="color:orange;">👀 Интерпретация</mark> <mark style="color:orange;">**toml**</mark>

<mark style="color:orange;">Важно изменить следующие параметры:</mark>

<mark style="color:orange;">`indexer.tendermint_addr`</mark> <mark style="color:orange;">- Это должен быть адрес и соответствующий порт синхронизированного полного узла Namada</mark>

<mark style="color:orange;">`database.host`</mark> <mark style="color:orange;">- Это должен быть tcp-адрес (с портом), на котором запущена база данных postgres.</mark>
{% endhint %}

После завершения настройки можно запустить индексатор

```rust
make run_indexer
```

### Запрос к базе данных

Предустановленные конечные точки для запросов к базе данных описаны в [документации здесь.](https://github.com/Zondax/namadexer/blob/main/docs/04-server.md)
