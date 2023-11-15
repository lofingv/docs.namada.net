---
description: Обновление
---

# История тестовых сетей

На этой странице описаны все шаги по установке, необходимые при различных обновлениях testnets.

### Последнее обновление

* Namada public testnet 14 (soft-upgrade hot-fix):
* С даты: 19 октября 2023 года 9:00 UTC
* Версия протокола Namada: v0.23.1
* Версия Cometbft: 0.37.2
* CHAIN\_ID: public-testnet-14.5d79b6958580

Как обновиться до последней версии testnet: Обновление является "мягким", т.е. в новых двоичных файлах нет никаких разрушающих консенсус изменений, и они обратно совместимы. Валидаторам рекомендуется обновиться до последней версии двоичных файлов, которую [можно найти здесь.](https://github.com/anoma/namada/releases/tag/v0.23.1)

Затем просто остановите текущую систему и перезапустите ее с новыми двоичными файлами. Это можно сделать, выполнив следующие команды:

```
OS="Linux" # Or OS="Darwin" for Mac
wget https://github.com/anoma/namada/releases/download/v0.23.1/namada-v0.23.1-${OS}-x86_64.tar.gz
tar -xvf namada-v0.23.1-${OS}-x86_64.tar.gz --strip-components 1 -C /usr/local/bin/ #sudo may be required
killall namadan
namada --version #should output v0.23.1
NAMADA_CMT_STDOUT=true namada node ledger run
```

{% hint style="info" %}
<mark style="color:blue;">Обратите внимание, что двоичные файлы</mark> <mark style="color:blue;">`v0.23.1`</mark> <mark style="color:blue;">для Linux требуют установки Ubuntu LTS, что означает, что некоторые старые версии Ubuntu будут несовместимы (только Ubuntu 22.04 или более поздние). В этом случае для обновления рекомендуется выполнить сборку из исходных файлов.</mark>
{% endhint %}

### Последние данные Testnet

* Namada public testnet 14:
  * From date: 5th of October 2023 17:00 UTC
  * Namada protocol version: `v0.23.0`
  * Cometbft version: `0.37.2`
  * CHAIN\_ID: `public-testnet-14.5d79b6958580`

### История тестнетов по времени

* Namada public testnet 13 (offline):
  * From date: 12th of September 2023 17:00 UTC
  * Namada protocol version: `v0.22.0`
  * Cometbft version: `0.37.2`
  * CHAIN\_ID: `public-testnet-13.facd514666d5`
* Namada public testnet 12:
  * From date: 17th of August 2023 17.00 UTC
  * Namada protocol version: `v0.21.1`
  * Cometbft version: `0.37.2`
  * CHAIN\_ID: `public-testnet-12.fedec12f3428`
* Namada public testnet 11:
  * From date: 2nd of August 2023 17.00 UTC
  * Namada protocol version: `v0.20.1`
  * Cometbft version: `0.37.2`
  * CHAIN\_ID: `public-testnet-11.cc649ddd49b0`
* Namada public testnet 10:
  * From date: 29th of June 2023 17.00 UTC
  * Namada protocol version: `v0.17.5`
  * Cometbft version: `v0.1.4-abciplus`
  * CHAIN\_ID: `public-testnet-10.3718993c3648`
* Namada public testnet 9:
  * From date: 20th of June 2023 17.00 UTC
  * Namada protocol version: `v0.17.3`
  * Cometbft version: `v0.1.4-abciplus`
  * CHAIN\_ID: `public-testnet-9.3718993c3648`
* Namada public testnet 8:
  * From date: 17th of May 2023 17.00 UTC
  * Namada protocol version: `v0.15.3`
  * Tendermint version: `v0.1.4-abciplus`
  * CHAIN\_ID: `public-testnet-8.0.b92ef72b820`
* Namada public testnet 7:
  * From date: 24th of April 2023 17.00 UTC
  * Namada protocol version: `v0.15.1`
  * Tendermint version: `v0.1.4-abciplus`
  * CHAIN\_ID: `public-testnet-7.0.3c5a38dc983`
* Namada public testnet 6:
  * From date: 29th of March 2023 17.00 UTC
  * Namada protocol version: `v0.14.3`
  * Tendermint version: `v0.1.4-abciplus`
  * CHAIN\_ID: `public-testnet-6.0.a0266444b06`
* Namada public testnet 5:
  * From date: 15th of March 2023
  * Namada protocol version: `v0.14.2`
  * Tendermint version: `v0.1.4-abciplus`
  * CHAIN\_ID: `public-testnet-5.0.d25aa64ace6`
* Namada public testnet 4:
  * From date: 22nd of February 2023
  * Namada protocol version: `v0.14.1`
  * Tendermint version: `v0.1.4-abciplus`
  * CHAIN\_ID: `public-testnet-4.0.16a35d789f4`
* Namada public testnet 3 hotfix (did not suffice):
  * From date: 13th of February 2023
  * Namada protocol version: `v0.13.4`
  * Tendermint version: `v0.1.4-abciplus`
  * CHAIN\_ID: `public-testnet-3.0.81edd4d6eb6`
* Namada public testnet 3:
  * From date: 9th of February 2023
  * Namada protocol version: `v0.13.3`
  * Tendermint version: `v0.1.4-abciplus`
  * CHAIN\_ID: `public-testnet-3.0.81edd4d6eb6`
*   Namada public testnet 2.1.2 hotfix:

    * From date: 25th of January 2023
    * Namada protocol version: `v0.13.3`
    * Tendermint version: `v0.1.4-abciplus`
    * CHAIN\_ID: `public-testnet-2.1.4014f207f6d`

    **В связи с обнаруженной ошибкой было выпущено** [**исправление**](https://github.com/anoma/namada/releases/tag/v0.13.3)**. Его необходимо было установить и применить перед**`18:00:00 UTC` on `2023-01-25`.
* Namada public testnet 2.1.2:
  * From date: 24th of January 2023
  * Namada protocol version: `v0.13.2`
  * Tendermint version: `v0.1.4-abciplus`
  * CHAIN\_ID: `public-testnet-2.1.4014f207f6d`
* Namada public testnet 2.1:
  * From date: 17th of January 2023
  * Namada protocol version: `v0.13.1-hardfork` (hardfork)
  * Tendermint version: `v0.1.4-abciplus`
  * CHAIN\_ID: `public-testnet-2.0.2feaf2d718c`

**Указанный хардфорк должен был вступить в силу на высоте блока 37370, но возникли некоторые проблемы. Подробнее об этом можно прочитать** [**здесь.**](https://blog.namada.net/namada-testnet-v0-13-0-upgrade-postmortem)

* Namada public testnet 2.0:
  * From date: 12th of January 2023
  * Namada protocol version: `v0.13.0`
  * Tendermint version: `v0.1.4-abciplus`
  * CHAIN\_ID: `public-testnet-2.0.2feaf2d718c`
* Namada public testnet 1:
  * Namada protocol version: `v0.12.0`
  * Tendermint version: `v0.1.4-abciplus`
  * Genesis time: 20th of December 2022 at 17:00 UTC
  * CHAIN\_ID: `public-testnet-1.0.05ab4adb9db`

### История обновлений:

19/05/2023 public-testnet-8 hot-fix

Из-за некоторых проблем с проверкой mempool тестовая сеть останавливалась на высоте блока 8073. Мы исправили эту проблему и выпустили горячую версию для подмножества валидаторов. Этого оказалось достаточно для продолжения работы тестовой сети. Правда, некоторым валидаторам пришлось заново синхронизировать сеть. Цепочка была запущена с chain-id public-testnet-8.0.b92ef72b820

24/04/2023 public-testnet-7 (offline)

Тестнет запущен 24/04/2023 в 17:00 UTC с валидаторами genesis из public-testnet-7. [Запущен с версией v0.15.1](https://github.com/anoma/namada/releases/tag/v0.15.1). Цепочка запущена с chain-id public-testnet-7.0.3c5a38dc983.

Задуманное исправление, призванное решить проблему хранения данных, было решено лишь частично. В результате была выпущена версия 0.15.3, в которой предполагалось устранить эти проблемы.

13/02/2023 public-testnet-3

09/02/2023 цепочка Namada public-testnet-3 остановилась из-за ошибки в реализации Proof of Stake при обработке граничного случая. За выходные команда смогла исправить и протестировать новый патч, устраняющий проблему. 13/02/2023 11:30 UTC нам удалось восстановить работу сети, заставив внутренних валидаторов обновиться до нового патча. Сейчас мы призываем валидаторов также обновиться до новой тестовой сети, что позволит вам взаимодействовать с восстановленной сетью.

### Обновление

Начните с остановки всех экземпляров узла namada

```
killall namadan
```

Сборка нового тега (или скачать двоичные [файлы здесь](https://github.com/anoma/namada/releases/tag/v0.13.4))

```
cd namada
export NAMADA_TAG=v0.13.4
make build-release
```

Скопируйте новые двоичные файлы по пути. Более подробные инструкции можно [найти здесь.](nastroika-sredy.md)

После этого на узле необходимо выполнить tesync из genesis (см. ниже).

### Как выполнить ресинхронизацию из genesis:

В качестве меры предосторожности сделайте резервную копию своих ключей от генезиса:

```
mkdir backup-pregenesis && cp -r .namada/pre-genesis backup-pregenesis/
```

Удалите соответствующую папку в файле .namada

```
rm -r .namada/public-testnet-3.0.81edd4d6eb6
rm .namada/public-testnet-3.0.81edd4d6eb6.toml
```

{% hint style="danger" %}
<mark style="color:red;">ВНИМАНИЕ: Не удаляйте всю папку .namada, так как в ней содержатся ключи предварительного генезиса. Если это будет сделано случайно, то придется скопировать файл backup-pregenesis.</mark> [<mark style="color:blue;">Подробнее см. в этой инструкции</mark>](../../rukovodstvo-dlya-operatorov/validatory-namada/zapustite-svoi-uzel-v-kachestve-validatora-genesis.md)
{% endhint %}

Повторно подключитесь к сети

```
export CHAIN_ID="public-testnet-3.0.81edd4d6eb6"
namada client utils join-network \
--chain-id $CHAIN_ID --genesis-validator $ALIAS
```

Запустите узел. Можно просто запустить узел снова, используя привычную команду

```
  NAMADA_CMT_STDOUT=true namada node ledger run
```

Пожалуйста, обращайтесь к нам с любыми вопросами, если они у вас возникнут. Данное обновление может быть выполнено асинхронно, но если вы хотите продолжить проверку цепочки и тестирование наших функций, необходимо выполнить описанные выше шаги.

### Hotfix for Testnet `public-testnet-2.1.4014f207f6d`

27/01/2023

Тестовая сеть с горячими исправлениями работала в течение недели, когда странная ошибка привела к остановке сети. Основная команда потратила 1 неделю на выяснение причины ошибки, и результат оказался весьма интересным. Если вам интересны подробности этой ошибки, пожалуйста, ознакомьтесь с [записью Рэя в его блоге здесь.](https://blog.namada.net/explaining-the-namada-0-13-3-consensus-fork/)

25/01/2023

Примерно в 06:15 UTC 25/01/2023 в активный набор валидаторов был запланирован валидатор с очень маленькой долей. По этому tx была обнаружена ошибка преобразования между машиной состояний Namada и Cometbft, которая приводила к сбою в работе узла. Для решения этой проблемы был выпущен [патч v0.13.3.](https://github.com/anoma/namada/releases/tag/v0.13.3)

23/01/2023

Новая тестовая сеть была выпущена раньше двухнедельного графика выпуска тестовых сетей из-за того, что приведенный ниже хардфорк не сработал так, как было задумано. Следуйте инструкциям по настройке [новой тестовой сети.](nastroika-sredy.md)

### Hardfork v0.13.1

Неактуально
