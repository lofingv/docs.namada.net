---
description: Namada Ledger
---

# Запуск полного узла

Для того чтобы осуществлять какие-либо взаимодействия с блокчейном Namada через Namada-клиент namadac, необходимо, чтобы ledger был запущен.

Чтобы запустить локальный узел Namada ledger, можно выполнить команду:

```
namada ledger
```

{% hint style="warning" %}
<mark style="color:orange;">Примечание: Перед запуском необходимо</mark> [<mark style="color:blue;">подключиться к сети</mark>](../../seti-namada/)<mark style="color:orange;">. Если сеть не была настроена, выдается ошибка.</mark>

<mark style="color:orange;">Узел попытается подключиться к узлам действующих валидаторов и другим узлам сети и синхронизироваться с последним блоком.</mark>

<mark style="color:orange;">По умолчанию ledger будет хранить свою конфигурацию и состояние в вашем</mark> [<mark style="color:blue;">базовом каталоге</mark>](bazovyi-katalog.md)<mark style="color:blue;">.</mark> <mark style="color:orange;">Для его изменения можно использовать глобальный аргумент</mark> <mark style="color:orange;">`CLI --base-dir`</mark> <mark style="color:orange;">или переменную окружения</mark> <mark style="color:orange;">`BASE_DIR`</mark><mark style="color:orange;">.</mark>

<mark style="color:orange;">Если у вас нет собственного</mark> <mark style="color:orange;">`base_di`</mark><mark style="color:orange;">r, вы можете экспортировать переменную окружения</mark> <mark style="color:orange;">`BASE_DIR`</mark> <mark style="color:orange;">следующим образом:</mark>

```rust
export BASE_DIR=$(namadac utils default-base-dir)
```
{% endhint %}

При первом запуске будут загружены MASP-параметры. Это необходимо для создания доказательств с нулевым разглашением, требуемых для проведения защищенных транзакций.

### Wasm-файлы Ledger

Также будет загружен блок genesis, содержащий начальное состояние блокчейна. Для этого необходимо получить доступ к файлам WASM, которые используются в блоке genesis. Эти файлы включены в релиз и не должны быть изменены, иначе ваш узел выйдет из строя с ошибкой консенсуса на блоке genesis. По умолчанию предполагается, что они находятся в каталоге wasm внутри каталога цепочки, находящегося в базовом каталоге, т.е. `$BASE_DIR/$CHAIN_ID/wasm`. Каталог wasm также может быть задан с помощью глобального аргумента `CLI --wasm-dir`, [переменной окружения](peremennye-sredy.md) `NAMADA_WASM_DIR` или в конфигурационном файле.

### Конфигурация ledger

Конфигурация ledger хранится в файле `$BASE_DIR/$CHAIN_ID/config.toml` (с параметром по умолчанию `--base-dir`). Он создается при подключении к сети. Вы можете модифицировать этот файл, чтобы изменить конфигурацию своего узла. Все значения также могут быть заданы с помощью [переменных окружения](peremennye-sredy.md).
