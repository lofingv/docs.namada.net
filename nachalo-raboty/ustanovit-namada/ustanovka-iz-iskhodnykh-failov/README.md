# Установка из исходных файлов

<mark style="color:orange;">Обратите внимание, что, хотя установка из исходных файлов является рекомендуемым способом установки namada, она может оказаться сложной и не рекомендуется новичкам. При первой установке процесс установки может занять до часа. Сборка двоичных файлов из исходных также может занять много времени, в зависимости от особенностей вашей машины.</mark>

### Предварительные требования

Убедитесь, что у вас загружены и установлены необходимые [предварительные компоненты.](predvaritelnye-komponenty.md)

### Установка Namada

Теперь, когда все необходимые компоненты установлены, можно клонировать исходный код из [репозитория Namada ](https://github.com/anoma/namada)и собрать его:

```rust
git clone https://github.com/anoma/namada.git
cd namada 
make install
```

{% hint style="info" %}
Во время внутренних и частных тестовых сетей проверьте последнюю ветку тестовой сети, используя `git checkout $NAMADA_TESTNET_BRANCH`. Где `$NAMADA_TESTNET_BRANCH`имя ветки testnet, которое будет указано в [документации testnet](https://docs.namada.net/introduction/testnets/environment-setup)[.](http://127.0.0.1:5000/o/hYGsDiLsn2ZyCLtb9GPH/s/E2YyCuHBNtFiT9RGgoko/)
{% endhint %}

### Добавление двоичных файлов в`$PATH`

Двоичные файлы следует добавлять в `$PATH`с помощью команды `make install`. Однако, если это по какой-то причине не сработало, решением может быть копирование двоичных файлов, `namada/target/release`например `$HOME/.local/bin/`:

```rust
cp namada/target/release/namada* $HOME/.local/bin/
```

### Использование двоичных файлов

Дополнительную информацию см. на странице, посвященной[ использованию двоичных файлов namada](../ustanovka-iz-binarnykh-failov/ispolzovanie-dvoichnykh-failov.md).

### Поиск неисправностей

Дополнительную информацию см. на странице устранения неполадок [при сборке из исходного кода](ustranenie-neispravnostei-pri-ustanovke-iz-istochnika.md).
