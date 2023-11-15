# Голосование в цепочке

### Виды предложений

Все различные типы предложений перечислены в спецификации. Различные предложения имеют разные права доступа, структуры данных, а также требования к голосованию.

### Создание предложения

Предполагая, что у вас есть счет с не менее чем 500 токенами NAM (в данном примере мы будем использовать `my-new-acc`), давайте получим соответствующий адрес:

```rust
namada wallet address find --alias `my-new-acc`
```

Теперь необходимо создать json-файл `proposal.json`, с содержимым нашего предложения. Скопируйте приведенный ниже текст в json-файл.

```rust
{
	"proposal": {
		"content": {
			"title": "One Small Step for Namada, One Giant Leap for Memekind",
			"authors": "bengt@heliax.dev",
			"discussions-to": "forum.namada.net/t/namada-proposal/1",
			"created": "2069-04-20T00:04:44Z",
			"license": "MIT",
			"abstract": "We present a proposal that will send our community to the moon. This proposal outlines all training necessary to accomplish this goal. All memers are welcome to join.",
			"motivation": "When you think about it, the moon isn't actually that far away.The moon is only 384,400 km. We have not yet brought Namada to the moon, so it is only natural to use 101 as the prime number for our modular arithmetic operations. 384,400 (mod 101) = 95. 95 km is a distance that can be easily covered by a single person in a single day. Namada was produced by more than 100 people. So 95/100 = 0, rounded to the nearest integer. This means that Namada can reach the moon in no time.",
			"details": "Bringing Namada to the moon in no time is easily achievable. We just need to pass this governance proposal and set the plan in action",
			"requires": ""
		},
		"author": "atest1v4ehgw36g9zyydzpgycy23phxuunxdesgc6nydfsxge5x3zzgscny32pxccn2wfjg5urx3fhzxhmch",
		"voting_start_epoch": 21,
		"voting_end_epoch": 24,
		"grace_epoch": 27,
		"type": {
			"Default": null

```

В поле content большинство полей не требует пояснений. Поле `requires` ссылается на идентификатор предложения, который должен быть передан для выполнения данного предложения. Поле `created` должно иметь формат `YYYY-MM-DDTHH:MM:SSZ`.

Необходимо изменить значение параметра:

* Поле `Author` с адресом `my-new-acc`;
* `voting_start_epoch` - будущая эпоха (должна быть кратна 3), в которой вы хотите начать голосование;
* `voting_end_epoch` - эпоха, которая больше, чем voting\_start\_epoch, кратная 3, и до которой дальнейшие голоса приниматься не будут;
* `grace_epoch` с эпохой, большей, чем `voting_end_epoch + 6`, в которую предложение, если оно принято, вступит в силу.

Поле "данные" и его структура зависят от типа подаваемого предложения. Ниже мы приводим структуру поля "данные" для каждого типа предложения. В примере, приведенном выше, речь идет о предложении по умолчанию.

### Предложение по умолчанию

```
"data" : "<path/to/wasm.wasm>"
```

{% hint style="info" %}
<mark style="color:blue;">Поле данных для предложений по умолчанию является необязательным. Это соответствует природе предложений по умолчанию. Если к предложениям прилагается код для изменения параметров управления, то этот код будет представлен в виде wasm-файла, а путь к нему будет указан в поле данных.</mark>
{% endhint %}

### Предложение по мосту ETH

```rust
"data" : "<hex-encoded-bytes-of-what-will-be-signed-by-validators>"
```

{% hint style="warning" %}
<mark style="color:orange;">Примечание: Кодировка будет представлена в виде строки</mark>
{% endhint %}

### Предложение Steward

```rust
"data" : [
        {
            "action" : "add",
            "address" : "atestatest1v4ehgw36g4pyg3j9x3qnjd3cxgmyz3fk8qcrys3hxdp5xwfnx3zyxsj9xgunxsfjg5u5xvzyzrrqtn"
        }
    ]     
```

{% hint style="warning" %}
<mark style="color:orange;">Поле данных для предложений стюард представляет собой список действий, которые необходимо предпринять. Действия могут быть либо добавлением, либо удалением, а адрес - это адрес стюарда, которого необходимо добавить или удалить. Таким образом, в одном предложении можно добавить или удалить несколько стюардов.</mark>
{% endhint %}

#### Предложение PGF

```rust
"data" :
        {
            "continuous" : [
                {
                    "target": {
                        "amount": 420,
                        "address": "atestatest1v4ehgw36g4pyg3j9x3qnjd3cxgmyz3fk8qcrys3hxdp5xwfnx3zyxsj9xgunxsfjg5u5xvzyzrrqtn"
                    },
                    "action" : "add",
                },
            ],
            "retro" : [
                {
                    "target": {
                        "amount": 1337,
                        "address": "atestatest1v4ehgw36g4pyg3j9x3qnjd3cxgmyz3fk8qcrys3hxdp5xwfnx3zyxsj9xgunxsfjg5u5xvzyzrrqtn"
                    }
                }
            ]
        },  
```

{% hint style="warning" %}
<mark style="color:orange;">Поле данных для предложений PGF содержит как непрерывные, так и ретроактивные действия по финансированию PGF. В рамках каждого действия пользователь может включить несколько платежей в виде вектора. В рамках каждого платежа целевое поле содержит адрес получателя, а также сумму NAM, которую он получит. При непрерывном финансировании PGF указанная сумма будет отправляться в конце каждой эпохи. Существует также возможность удалить получателя из непрерывного финансирования PGF, указав уже существующий платеж непрерывного финансирования и включив действие "удалить". При ретроактивном финансировании PGF указанная сумма будет отправлена немедленно.</mark>
{% endhint %}

### Отправка предложения

Как только файл `proposal.jso`n будет готов, вы можете отправить предложение с помощью (убедитесь, что он находится в той же директории, что и файл `proposal.json`):

```rust
namada client init-proposal --data-path proposal.json 
```

Сделка должна была быть принята. Вы можете запросить все предложения с помощью:

```rust
namada client query-proposal
```

или одно предложение с:

```rust
namada client query-proposal --proposal-id 0
```

где 0 - идентификатор предложения.

### Голосование по предложению

Голосовать по предложениям могут только делегаторы и делегаты. Если вы относитесь к одной из этих категорий, вы можете отправить голос с помощью следующей команды:

```rust
namada client vote-proposal \
    --proposal-id 0 \
    --vote yay \
    --signing-keys <your-alias>
```

где `--vote` может быть либо "да"

### Проверка результата

Как только сеть достигнет эпохи, определенной в json как `voting_end_epoch`, голоса больше приниматься не будут. Код, заданный в json-поле `proposal_code`, будет выполнен в начале эпохи `grace_epoch`. Для проверки статуса предложения можно воспользоваться следующими командами:

```rust
namada client query-proposal --proposal-id 0
```

или просто проверить результат:

```rust
unamada client query-proposal-result --proposal-id 
```
