# Тестнет Namada Campfire

{% hint style="info" %}
<mark style="color:blue;">Тестнет Namada Campfire ⛺🔥 работает параллельно с тестнетом "Валидатор". Это тестовая сеть, управляемая сообществом, которая в настоящее время поддерживается и организуется</mark> [коллективом Luminara](https://luminara.icu/) <mark style="color:blue;">небольшой группой сторонников Namada. В Campfire обычно используется последняя версия (или предварительный релиз), которая может отличаться от версии тестовой сети валидатора. Посмотреть версию и многое другое можно на сайте</mark> [https://testnet.luminara.icu.](https://testnet.luminara.icu/)
{% endhint %}

### Присоединение

{% hint style="info" %}
<mark style="color:blue;">Самая актуальная документация по присоединению к Campfire ⛺🔥</mark> [находится здесь.](https://knowabl.notion.site/Campfire-testnet-5e4c1df53ab64b818a55bfcf36ccc550)
{% endhint %}

### Подготовка

{% hint style="warning" %}
<mark style="color:orange;">В настоящее время в документации Campfire ⛺🔥 подробно описана поддержка только машин с Ubuntu.</mark>
{% endhint %}

Пользователю необходимо установить[ Stack-Orchestrator,](https://github.com/vknowable/stack-orchestrator) который представляет собой общий инструмент для быстрого и гибкого запуска и управления стеком программного обеспечения в контейнерной среде. Документация находится в [README](https://github.com/vknowable/stack-orchestrator/README.md) репозитория, а установить инструмент можно, выполнив следующую команду:

```rust
git clone https://github.com/vknowable/stack-orchestrator.git
cd stack-orchestrator
scripts/namada-quick-install.sh
exit
```

После этого рекомендуется установить Namada с помощью следующего Docker-образа (при этом версию в этой команде может потребоваться [соответствующим образом обновить](https://testnet.luminara.icu/)):

```rust
export NAMADA_TAG=v0.23.1
docker pull spork666/namada:$NAMADA_TAG
docker image tag spork666/namada:$NAMADA_TAG cerc/namada:local
mkdir -p ~/.local/share/namada
```

### Запуск узла

После этого пользователь может выполнить следующую команду для запуска узла:

```rust
curl -o ~/luminara.env https://testnet.luminara.icu/luminara.env
laconic-so --stack public-namada deploy --env-file ~/luminara.env up
echo "export CONTAINER=$(docker ps -q)" | sudo tee -a /etc/profile
CONTAINER=$(docker ps -q)
```

Соответствующая и более подробная документация [находится здесь.](https://knowabl.notion.site/From-scratch-to-syncing-in-10-minutes-c0a56b34cdec447fbe2a5cd8f559f0bb)

### Взаимодействие с тестовой сетью

Коллектив Luminara также составил шпаргалку Namada CLI, которая доступна и полезна любому пользователю Namada.
