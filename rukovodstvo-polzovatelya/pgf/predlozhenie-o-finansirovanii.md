# Предложение о финансировании

### Форматирование файла `proposal.json`

Ниже приведен пример PGFP-предложения, которое может быть подано стюардом. Обратите внимание, что такие предложения смогут подавать только стюарды.

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
    "data" :
        {
            "continuous" : [
                {
                    "target": {
                        "amount": 420,
                        "address": "<address-of-recipient>"
                    }
                    "action" : "add",
                },
            ],
            "retro" : [
                {
                    "target": {
                        "amount": 1337,
                        "address": "<address-of-recipient>"
                    }
                }
            ]
        },  
}
```

где `<address-of-recipient>` должен быть заменен на адрес получателя средств.

Сохраните этот файл под именем `PGF_proposal.json` по какому-либо запоминающемуся пути на вашей машине.

### Подача предложения

Для отправки заявки стюард может воспользоваться следующей командой:

```rust
namada client init-proposal \
    --pgf-proposal \
    --data-path PGF_proposal.json
```

В этом случае заявке присваивается `proposal-id`, который может быть использован для запроса заявки.

### Запрос предложения

Команда для запроса предложения выглядит следующим образом:

```rust
namada client query-proposal \
    --proposal-id <the-proposal-id>
```
