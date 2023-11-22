---
description: Что такое Namada?
---

# Введение

Namada - это суверенный блокчейн, использующий алгоритм консенсуса [CometBFT](https://cometbft.com/)( Tendermint BFT). Namada позволяет осуществлять переводы между любыми несколькими активами с использованием [защищенного пула с множеством активов](https://medium.com/@lofingvv/%D0%BF%D0%BE%D0%B4%D1%80%D0%BE%D0%B1%D0%BD%D0%BE%D0%B5-%D0%BE%D0%B1%D1%8A%D1%8F%D1%81%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5-masp-cfe6fa4f4df3), созданного на основе схемы [Sapling.](https://z.cash/upgrade/sapling/)

В Namada реализована полная поддержка протокола IBC, встроенный мост с Ethereum, современная система proof-of-stake с автоматическим компаундингом вознаграждения и кубическим сокращением, механизм сигнализации об управлении с учетом доли, а также двойная система непрерывного/ретроактивного финансирования общественных благ.

Пользователи MASP получают вознаграждение за свой вклад в набор конфиденциальности в виде токенов нативного протокола (NAM).

Namada поддерживает множество различных кошельков. Одним из таких кошельков является "родной" кошелек, который существует для того, чтобы облегчить взаимодействие с протоколом в безопасной и приватной манере.

Кроме того, Namada поддерживает функцию Shielded Actions, которая позволяет пользователям большую часть времени хранить свои активы в приватном режиме на Namada, но иногда снимать защиту с определенных активов, чтобы взаимодействовать с существующими приложениями на прозрачных цепочках.

Подробнее о Namada можно узнать здесь (открывается в новой вкладке).

### Что такое Anoma?

Anoma - это архитектура децентрализованного обнаружения, решения и расчетов с сохранением конфиденциальности, ориентированная на намерения. С техническими характеристиками Anoma можно [ознакомиться здесь.](https://specs.anoma.net/)

### Как Namada связана с Anoma?

Anoma - это архитектура с полным стеком, рассчитанная на долгосрочную перспективу, в то время как Namada - это конкретная цепочка и набор функций, предназначенных для обеспечения практической конфиденциальности уже сейчас.

### Почему Namada?

Конфиденциальность должна быть стандартной и неотъемлемой частью систем, которые мы используем для совершения сделок, однако безопасной и удобной для пользователя системы конфиденциальности с несколькими активами в экосистеме блокчейн пока не существует. До сих пор у пользователей был выбор: либо суверенная цепочка, которая перевыпускает активы ([например, Zcash](https://z.cash/)), либо решение, сохраняющее конфиденциальность, построенное на существующей цепочке смарт-контрактов. Оба варианта имеют значительные компромиссы: в первом случае пользователи не могут совершать сделки с теми активами, с которыми они действительно хотят их совершать, а во втором - ограничения существующих платформ приводят к утечке метаданных, а протоколы могут быть дорогими и неудобными в использовании.

Namada поддерживает любые взаимозаменяемые и невзаимозаменяемые активы из IBC-совместимых блокчейнов, а также такие активы, передаваемые через собственный мост Ethereum (например, токены ERC20 и ERC721, соответственно). Как только активы оказываются на Namada, экранированные переводы становятся дешевыми, а все активы вносят вклад в один и тот же набор анонимности.

Пользователи Namada получают вознаграждения, сохраняют конфиденциальность активов и вносят вклад в общую приватность.

### Структура данной спецификации

Документы спецификации Namada состоят из четырех подразделов:

* Base ledger
* Управление
* Экранированный пул с несколькими активами
* Совместимость
* Экономика

Эта спецификация написана с использованием GitBook. Исходный текст можно найти в репозитории[ Namada Docs.](https://github.com/anoma/namada-docs/tree/master/packages/specs)
