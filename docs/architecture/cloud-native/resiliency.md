---
title: Устойчивость, ориентированная на облако
description: Создание архитектуры облачных приложений .NET для Azure | Устойчивость машинного кода в облаке
ms.date: 06/30/2019
ms.openlocfilehash: 680542abc5d8c43c577321d5ae834f0a13290da3
ms.sourcegitcommit: 55f438d4d00a34b9aca9eedaac3f85590bb11565
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "73841045"
---
# <a name="cloud-native-resiliency"></a>Устойчивость, ориентированная на облако

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

Устойчивость — это способность системы реагировать на сбои и продолжать работать. Это не относится к предотвращению сбоев. Но в этом случае это неизбежно в облачных системах и построении приложения для реагирования на него. Конечная цель обеспечения устойчивости — возврат приложения к полностью работоспособному состоянию после сбоя.

В отличие от традиционных монолитных приложений, где все работает вместе в одном процессе, облачные системы используют распределенную архитектуру, как показано на рисунке 6-1:

![Распределенное облако — собственная среда](./media/distributed-cloud-native-environment.png)

**Рис. 6-1.** Распределенное облако — собственная среда

На предыдущем рисунке обратите внимание, как каждый клиент, микрослужба и облачная [Служба резервного копирования](https://12factor.net/backing-services) выполняются как отдельный процесс, работающий на разных серверах и взаимодействующий через сетевые вызовы.

Итак, что может быть не так?

- Непредвиденная [задержка сети](https://www.techopedia.com/definition/8553/network-latency).
- Временные [сбои](https://docs.microsoft.com/azure/architecture/best-practices/transient-faults) (временные ошибки подключения к сети).
- Блокировка длительно выполняющейся синхронной операции.
- Ведущий процесс, который завершился сбоем, перезапускается или перемещается.
- Перегруженная микрослужба, которая не может ответить на короткое время.
- Операция onflight DevOps, такая как операция обновления или масштабирования.
- Операция Orchestrator, например перемещение службы с одного узла на другой.
- Аппаратные сбои на оборудовании товара.

При развертывании распределенных служб в облачной инфраструктуре факторы из предыдущего списка становятся очень реальными, и вам необходимо создать архитектуру и разработать защищенные решения.

В небольших распределенных системах сбой будет менее частым, но по мере увеличения и уменьшения размера системы можно столкнуться с большим количеством таких проблем, когда частичный сбой становится нормальным.

Поэтому ваше приложение и инфраструктура должны быть устойчивыми. В следующих разделах мы расскажем о защитных методах, которые можно добавить в приложение, и встроенных облачных функциях, которые можно использовать для подтверждения работы пользователя в виде маркеров.

>[!div class="step-by-step"]
>[Назад](azure-data-storage.md)
>[Вперед](application-resiliency-patterns.md)
