# Установка из бинарных файлов

### Предварительные требования

Перед установкой Namada с помощью двоичных файлов необходимо установить некоторые предварительные компоненты.

К ним относятся:

* [CometBFT](../../ustanovka-cometbft.md)
* [GLIBC](predvaritelnye-komponenty.md)

### Загрузка двоичных файлов

Теперь, когда все зависимости установлены, вы можете загрузить последний бинарный релиз с [нашей страницы релизов](https://github.com/anoma/namada/releases), выбрав соответствующую архитектуру.

Приведенный ниже код загрузит все двоичные файлы последней версии для всех поддерживаемых операционных систем:

```rust
OPERATING_SYSTEM="Linux" # or "Darwin" for MacOS
latest_release_url=$(curl -s "https://api.github.com/repos/anoma/namada/releases/latest" | grep "browser_download_url" | cut -d '"' -f 4 | grep "$OPERATING_SYSTEM")
wget "$latest_release_url"
```

### Размещение двоичных файлов в `$PATH`

Для машин на ubuntu и mac для размещения namada в path должны работать следующие команды.

Зайдите в каталог, содержащий двоичные файлы:

```rust
sudo cp ./namada* /usr/local/bin/
```
