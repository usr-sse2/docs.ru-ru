---
title: Инфраструктура связи для слоя взаимодействия между службами
description: Узнайте, как технологии сетки служб упрощают взаимодействие с микрослужбами, встроенные в облако
author: robvet
ms.date: 09/10/2019
ms.openlocfilehash: a9192bf9f5827d05b2453c796c72e11782f9f911
ms.sourcegitcommit: 559259da2738a7b33a46c0130e51d336091c2097
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/22/2019
ms.locfileid: "73841285"
---
# <a name="service-mesh-communication-infrastructure"></a>Инфраструктура связи для слоя взаимодействия между службами

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

В этой главе мы изучили проблемы взаимодействия микрослужб. Мы говорили, что группы разработчиков должны учитывать, как взаимодействуют друг с другом серверные службы. В идеале, чем меньше взаимодействие между службами, тем лучше. Тем не менее, не всегда возможно, что серверные службы часто зависят друг от друга для выполнения операций.

Мы изучили различные подходы к реализации синхронного обмена данными по протоколу HTTP и асинхронного обмена сообщениями. В каждом из вариантов разработчик полагается на реализацию кода связи. Код связи сложен и требует много времени. Неправильные решения могут привести к значительным проблемам с производительностью.

Более современный подход к информационным центрам микрослужб вокруг новой и быстро развивающейся технологии, ориентированной на *службу*. [Сетка служб](https://www.nginx.com/blog/what-is-a-service-mesh/) — это настраиваемый уровень инфраструктуры со встроенными возможностями для взаимодействия между службами, обеспечения устойчивости и многих перекрестных проблем. Он перенесет ответственность за эти проблемы из микрослужб и уровня сети обслуживания. Связь отделяется от микрослужб.

Ключевым компонентом сетки службы является прокси-сервер. В облачном приложении экземпляр прокси-сервера обычно размещается вместе с каждой микрослужбой. Хотя они выполняются в отдельных процессах, эти два тесно связаны и используют один и тот же жизненный цикл. Этот шаблон, известный как [шаблон расширения](https://docs.microsoft.com/azure/architecture/patterns/sidecar)и показан на рисунке 4-23.

![Сеть службы с побочным автомобилем](./media/service-mesh-with-side-car.png)

**Рис. 4-23**. Сеть службы с побочным автомобилем

Обратите внимание, что на предыдущем рисунке показано, как сообщения перехватываются прокси-сервером, работающим параллельно с каждой микрослужбой. Каждый прокси-сервер можно настроить с помощью правил трафика, относящихся к микрослужбе. Он распознает сообщения и может маршрутизировать их во всех службах и во внешнем мире.

Помимо управления взаимодействием между службами, сетка служб обеспечивает поддержку обнаружения служб и балансировки нагрузки.

После настройки сеть службы сильно функционирует. Сетка получает соответствующий пул экземпляров из конечной точки обнаружения службы. Он отправляет запрос к определенному экземпляру службы, записывая тип задержки и ответа результата. Он выбирает экземпляр, который, скорее всего, возвращает быстрый ответ на основе различных факторов, включая наблюдаемую задержку для последних запросов.

Сетка служб управляет проблемами трафика, связи и сети на уровне приложения. Он понимает сообщения и запросы. Как правило, сеть службы интегрируется с контейнером Orchestrator. Kubernetes поддерживает расширяемую архитектуру, в которой можно добавить сетку службы.

В главе 6 мы подробно расскажем о технологиях сетки служб, включая обсуждение ее архитектуры и доступных реализаций с открытым исходным кодом.

## <a name="summary"></a>Сводка

В этой главе мы обсуждали собственные шаблоны связи в облаке. Мы начали с изучения того, как клиентские клиенты взаимодействуют с внутренними микрослужбами. Кстати, мы говорили о платформах шлюза API и обмене данными в режиме реального времени. Затем мы рассматривали, как микрослужбы взаимодействуют с другими серверными службами. Мы рассматривали синхронный обмен данными по протоколу HTTP и асинхронный обмен сообщениями между службами. Мы рассматривали gRPC, предстоящую технологию в собственном мире в облаке. Наконец, мы представили новую и быстро развивающуюся технологию, которая позволяет оптимизировать взаимодействие микрослужб.

Особое внимание было уделено управляемым службам Azure, которые могут помочь в реализации взаимодействия в собственных системах в облаке:

- [Шлюз приложений Azure](https://docs.microsoft.com/azure/application-gateway/overview)
- [Управление API Azure](https://azure.microsoft.com/services/api-management/)
- [Служба Azure SignalR](https://azure.microsoft.com/services/signalr-service/)
- [Очереди службы хранилища Azure](https://docs.microsoft.com/azure/storage/queues/storage-queues-introduction)
- [Служебная шина Azure](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview)
- [Сетка событий Azure](https://docs.microsoft.com/azure/event-grid/overview)
- [Концентратор событий Azure](https://azure.microsoft.com/services/event-hubs/)

Далее мы перейдем к распределенным данным в собственных облачных системах, а также к преимуществам и задачам, которые они представляют.

### <a name="references"></a>Ссылки

- [Микрослужбы .NET: архитектура контейнерных приложений .NET](https://dotnet.microsoft.com/download/thank-you/microservices-architecture-ebook)

- [Проектирование взаимодействия между службами для микрослужб](https://docs.microsoft.com/azure/architecture/microservices/design/interservice-communication)

- [Служба Azure SignalR — полностью управляемая служба для добавления функциональности в режиме реального времени](https://azure.microsoft.com/blog/azure-signalr-service-a-fully-managed-service-to-add-real-time-functionality/)

- [Контроллер входящего трафика шлюза API Azure](https://azure.github.io/application-gateway-kubernetes-ingress/)

- [О входящих данных в службе Azure Kubernetes (AKS)](https://vincentlauzon.com/2018/10/10/about-ingress-in-azure-kubernetes-service-aks/)

- [Практические gRPC](https://www.worldcat.org/title/practical-grpc/oclc/1042342319)

- [Документация по gRPC](https://grpc.io/docs/guides/)

- [gRPC для разработчиков WCF](https://bing.com) [Книга gRPC меток]

- [Сравнение служб gRPC с API-интерфейсами HTTP](https://docs.microsoft.com/aspnet/core/grpc/comparison?view=aspnetcore-3.0)

>[!div class="step-by-step"]
>[Назад](rest-grpc.md)
>[Вперед](distributed-data.md)
