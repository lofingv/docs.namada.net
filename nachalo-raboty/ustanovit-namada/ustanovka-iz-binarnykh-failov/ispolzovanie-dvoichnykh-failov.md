# Использование двоичных файлов

После установки у вас должны быть следующие двоичные файлы:

| Binary    | Description                                                                    |
| --------- | ------------------------------------------------------------------------------ |
| `namada`  | The main binary that can be used to interact with all the components of Namada |
| `namadan` | The ledger node                                                                |
| `namadac` | The client                                                                     |
| `namadaw` | The wallet                                                                     |
| `namadar` | The ethereum bridge relayer                                                    |

Главный двоичный файл `namada` имеет подкоманды для всех остальных двоичных файлов. Поэтому следующие команды эквивалентны:

| `namada` command | `namada`x equivalent |
| ---------------- | -------------------- |
| `namada client`  | `namadac`            |
| `namada node`    | `namadan`            |
| `namada wallet`  | `namadaw`            |
| `namada relayer` | `namadar`            |

Для изучения интерфейса командной строки на уровне любой подкоманды может быть добавлен аргумент `--help`, позволяющий узнать все возможные подкоманды и/или аргументы.
