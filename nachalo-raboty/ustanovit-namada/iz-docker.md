# Из Docker

### Предварительные компоненты

Для запуска любых образов docker необходимо установить docker. Инструкции по установке docker для вашей машины можно [найти здесь.](https://docs.docker.com/get-docker/)

### Загрузка образа docker

Докер-образ Namada можно [найти здесь.](https://github.com/anoma/namada/pkgs/container/namada)

На вкладке `Tags` можно найти последнюю версию образа докера. Щелкните по ссылке для нужной версии Namada, которую вы пытаетесь установить. Например, если вы пытаетесь установить Namada `v0.16.0`, вы должны щелкнуть на ссылке `v0.16.0`.

Тег загруженного докер-образа можно узнать, запустив команду `docker images`. Метка будет находиться в первом столбце результата.

### Запуск образа docker

После загрузки образа docker будет полезно экспортировать некоторые переменные окружения:

```rust
export CHAIN_ID=<chain-id>
```

Следующая команда docker run запустит узел ledger:

```rust
docker run -P -i -t $DOCKER_IMAGE <namada command>
```

Где `<namada command>` - это любая команда, которую можно выполнить после namada в терминале. Например, если вы хотите запустить `namada client utils join-network --chain-id $CHAIN_ID`, вы должны выполнить:

```rust
docker run -P -i -t $DOCKER_IMAGE client utils join-network --chain-id $CHAIN_ID
```

Затем для выполнения любых других команд ledger можно выполнить команду:

```rust
docker /bin/bash -c "/bin/bash","-c", "<namada command>"
```

### Альтернативный метод (самостоятельная сборка образа docker)

В качестве альтернативы можно собрать образ docker самостоятельно!

Начните с экспорта некоторых переменных окружения:

```rust
export CHAIN_ID=<chain-id>export BRANCH=<namada-version>
```

Например, если требуется собрать docker-образ для `Namada v0.16.0` и `chain-id public-testnet-69.0.b20a1337aa1`, то нужно выполнить команду:

```rust
export CHAIN_ID=public-testnet-69.0.b20a1337aa1export BRANCH=v0.16.0
```

Затем можно собрать образ docker, выполнив команду:

{% code overflow="wrap" %}
```rust
git clone https://github.com/anoma/namada-sdk-starter.gitcd namada-sdk-starter/docker/namada-with-chain/docker build --build-arg BRANCH=$BRANCH --build-arg CHAIN_ID=$CHAIN_ID -t namada_testnet_image .
```
{% endcode %}

В результате образ будет сохранен в локальной папке docker images. Тег загруженного docker-образа можно узнать, выполнив команду docker images. Тег будет первым столбцом выходных данных.

Сохраните этот образ docker в качестве переменной окружения

```rust
export DOCKER_IMAGE=<tag>
```

Затем можно запустить образ docker, выполнив команду:

```rust
docker run -P -i -t $DOCKER_IMAGE
```
