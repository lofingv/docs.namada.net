# Базовый каталог

Базовый каталог на Namada - это каталог, в котором хранятся все данные, относящиеся к конкретной сети. Это каталог, который создается сразу после присоединения к сети (например, с помощью `namadac utils join-network`).

Начиная с последней версии Namada, базовый каталог находится в следующих местах:

{% hint style="info" %}
<mark style="color:blue;">Технически, правильным каталогом будет тот, который назначен переменной</mark> <mark style="color:blue;">`$XDG_DATA_HOME`</mark><mark style="color:blue;">, но если вы не задали эту переменную, то по умолчанию будет выбран один из нижеперечисленных.</mark>
{% endhint %}

### Быстрый способ

Найти базовый каталог можно, выполнив следующую команду:

```rust
namadac utils default-base-dir
```

Который должен соответствовать одному из следующих каталогов:

#### Linux

```rust
$HOME/.local/share/namada
```

**MacOS**

```rust
$HOME/Library/Application\ Support/Namada
```

### Что ожидать

В этих папках вы должны увидеть следующие файлы и папки:

```rust
global-config.toml
<some-chain-id>/
<some-chain-id>.toml
pre-genesis # If you are a pre-genesis validator
```
