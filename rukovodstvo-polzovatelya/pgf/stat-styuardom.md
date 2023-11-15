# Стать стюардом

Управляющий общественными благами может состоять из произвольного числа людей, а может быть и одним человеком. Единственным требованием является то, что мультиподписная учетная запись стюарда должна быть избрана сообществом посредством предложения по управлению.

Поэтому первым шагом на пути к статусу стюарда является создание учетной записи с несколькими подписями. Это можно сделать с помощью команд, приведенных в документации [по multisgnature.](../prozrachnye-scheta/scheta-s-neskolkimi-podpisyami-na-namada.md)

После создания мультисигнатурной учетной записи стюард может подать предложение по управлению для избрания этой учетной записи в качестве стюарда.

### Предложение по управлению

Предложение по управлению, необходимое для избрания нового стюарда, - это `StewardProposal`.

#### Формирование файла `proposal.json` для предложения стюарда

Файл `steward_proposal.json` содержит информацию о предложении. Он представляет собой JSON-файл со следующей структурой:

```rust
{
    "proposal" :{
        "content": {
            "title": "Stewie for Steward 2024",
            "authors": "stewie@heliax.dev",
            "discussions-to": "forum.namada.net/t/stewies-manifesto/1",
            "created": "2024-01-01T00:00:01Z",
            "license": "MIT",
            "abstract": "Stewie is running for steward, with a focus on technical research. The technical research I will be focused on will definitely not be for weapons of mass destruction. There is some possibility however that I may be focusing somewhat on open source software for weapons of mass destruction.",
            "motivation": "Nobody knows technical research better than me. Trust me. I know it. I have the best technical research. I will be the best steward. Last night, Namada called me and said, Stewie, thank you. I will make public goods funding great again",
            "details": "As a genius baby, I possess an unmatched level of intelligence and a visionary mindset. I will utilize these qualities to solve the most complex problems, and direct public goods funding towards weapons of mass destruction ... i mean open source software for weapons of mass destruction",
        },
        "author": "stewie",
        "voting_start_epoch": 3,
        "voting_end_epoch": 6,
        "grace_epoch": 12,
    },
    "data" : [
        {
            "action" : "add",
            "address" : "atestatest1v4ehgw36g4pyg3j9x3qnjd3cxgmyz3fk8qcrys3hxdp5xwfnx3zyxsj9xgunxsfjg5u5xvzyzrrqtn"
        }
    ]     
}
```

Поле "data" содержит структуру, позволяющую либо добавить, либо удалить мультиподписной счет из списка стюардов. В данном случае " `"action"` это `"add"`, а `"address"` - это адрес счета с мультиподписью, который будет избран стюардом. Если `"action"` это `"remove"`, то `"address"` - это адрес счета мультиподписи, который будет удален из списка стюардов.

{% hint style="warning" %}
<mark style="color:orange;">В поле мотивации и аннотации важно четко указать, на каком виде финансирования общественных благ будет сфокусирована работа стюарда. Направления финансирования общественных благ можно найти в спецификации финансирования общественных благ</mark>
{% endhint %}

#### Отправка предложения в сеть

Команда CLI для отправки предложения имеет следующий вид:

```rust
namadac init-proposal \
        --pgf-stewards \
        --data-path <path/to/steward_proposal.json>
```

где `<path/to/steward_proposal.json>` - путь к файлу `steward_proposal.json`.

#### Стать избранным

После подачи предложения оно будет вынесено на[ голосование сообщества.](golosovanie-za-styuardov-i-predlozheniya-pgf.md) Если предложение пройдет, то аккаунт станет стюардом. Если предложение не пройдет, то аккаунт не станет стюардом.

После того как аккаунт с несколькими подписями станет избранным (что произойдет по окончании `grace_epoch`), он сможет подавать заявки в пул финансирования общественных благ ([см. подача заявок](predlozhenie-o-finansirovanii.md)).

### Потеря управления

Существует 4 способа, с помощью которых управляющий может потерять свое управление:

1. Снять с себя полномочия стюарда
2. Иметь значительное провальное предложение по финансированию(например из имеющихся 23 голосов сообщества все 23 проглосовали против)
3. Выбывают по предложению руководства
4. Истечение срока полномочий.

Уход с поста стюарда может быть осуществлен в любой момент. Через CLI это можно сделать с помощью команды:

```rust
namadac resign-steward --steward my-steward-address
```

Подробнее о других способах потери управления читайте в спецификации.
