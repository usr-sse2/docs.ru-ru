---
title: Различия в возможностях очередей в Windows Vista, Windows Server 2003 и Windows XP
ms.date: 03/30/2017
helpviewer_keywords:
- queues [WCF], differences in operating systems
ms.assetid: aa809d93-d0a3-4ae6-a726-d015cca37c04
ms.openlocfilehash: 77491981bdef7d02da6894cbbee93c779972d803
ms.sourcegitcommit: a4f9b754059f0210e29ae0578363a27b9ba84b64
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/05/2019
ms.locfileid: "74837432"
---
# <a name="differences-in-queuing-features-in-windows-vista-windows-server-2003-and-windows-xp"></a>Различия в возможностях очередей в Windows Vista, Windows Server 2003 и Windows XP
В этом разделе перечислены различия в функциях очередей Windows Communication Foundation (WCF) между Windows Vista, [!INCLUDE[ws2003](../../../../includes/ws2003-md.md)]и [!INCLUDE[wxp](../../../../includes/wxp-md.md)].  
  
## <a name="application-specific-dead-letter-queue"></a>Очередь недоставленных сообщений, связанная с приложением  
 Сообщения могут бесконечно долго оставаться в очереди, если принимающее приложение не считывает их из очереди за отведенное время. Такое поведение не рекомендуется для чувствительных ко времени сообщений. Сообщения, чувствительные ко времени, имеют свойство `TimeToLive`, заданное в привязке с очередью. Это свойство указывает максимальный срок нахождения сообщений в очереди. Просроченные сообщения отправляются в специальную очередь, называемую очередью недоставленных сообщений. Сообщение также может быть помещено в очередь недоставленных сообщений и по другим причинам, таким как превышение квоты очереди или сбой проверки подлинности.  
  
 Как правило, единая общесистемная очередь недоставленных сообщений распространяется на все приложения с очередями, имеющие совместный доступ к диспетчеру очередей. Очередь недоставленных сообщений для каждого приложения обеспечивает более эффективную изоляцию между приложениями с очередями с совместным доступом к диспетчеру очередей, позволяя этим приложениям определять собственную очередь недоставленных сообщений, связанную с приложением. Приложение с совместным с другими приложениями доступом к очереди недоставленных сообщений должно просмотреть очередь, чтобы найти сообщения, относящиеся именно к нему. Благодаря очереди недоставленных приложений, относящейся к приложению, оно располагает точной информацией, что все сообщения в очереди недоставленных сообщений применимы именно к нему.  
  
 Windows Vista предоставляет очереди недоставленных сообщений для конкретных приложений. Очереди недоставленных сообщений, относящиеся к приложению, недоступны в [!INCLUDE[ws2003](../../../../includes/ws2003-md.md)] и [!INCLUDE[wxp](../../../../includes/wxp-md.md)], и приложения должны использовать общесистемную очередь недоставленных приложений.  
  
## <a name="poison-message-handling"></a>Обработка подозрительных сообщений  
 Подозрительное сообщение представляет собой сообщение, для которого превышено максимальное количество попыток доставки в принимающее приложение. Такая ситуация может возникнуть, когда приложение, считывающее сообщение из транзакционной очереди, не может сразу обработать сообщение из-за ошибок. Если приложение отменяет транзакцию, при которой было получено сообщение из очереди, оно возвращает сообщение обратно в очередь. После этого приложение пытается повторно извлечь сообщение в новой транзакции. Если причина ошибки не устранена, принимающее приложение может зациклиться, получая и прерывая прием одного и того же сообщения, до достижения максимального числа попыток доставки и пометки сообщения как подозрительного.  
  
 Основные различия между очередью сообщений (MSMQ) в Windows Vista, [!INCLUDE[ws2003](../../../../includes/ws2003-md.md)]и [!INCLUDE[wxp](../../../../includes/wxp-md.md)], которые относятся к обработке подозрительных событий, включают следующее:  
  
- MSMQ в Windows Vista поддерживает подочереди, тогда как [!INCLUDE[ws2003](../../../../includes/ws2003-md.md)] и [!INCLUDE[wxp](../../../../includes/wxp-md.md)] не поддерживают подочереди. Вложенные очереди используются при обработке подозрительных сообщений. Очереди повторных попыток и очередь подозрительных сообщений являются вложенными очередями очереди приложения, созданной на основе параметров обработки подозрительных сообщений. Значение `MaxRetryCycles` указывает, сколько необходимо создать вложенных очередей повторных попыток. Поэтому при работе с [!INCLUDE[ws2003](../../../../includes/ws2003-md.md)] или [!INCLUDE[wxp](../../../../includes/wxp-md.md)] параметр `MaxRetryCycles` игнорируется, а значение `ReceiveErrorHandling.Move` не допускается.  
  
- MSMQ в Windows Vista поддерживает отрицательное подтверждение, а [!INCLUDE[ws2003](../../../../includes/ws2003-md.md)] и [!INCLUDE[wxp](../../../../includes/wxp-md.md)] не поддерживаются. Уведомление о недоставке от диспетчера принимающей очереди приводит к тому, что диспетчер передающей очереди помещает сообщение в очередь недоставленных сообщений. Поэтому значение `ReceiveErrorHandling.Reject` не допускается в [!INCLUDE[ws2003](../../../../includes/ws2003-md.md)] и [!INCLUDE[wxp](../../../../includes/wxp-md.md)].  
  
- MSMQ в Windows Vista поддерживает свойство Message, которое хранит количество попыток доставки сообщений. Это свойство подсчета прекращений недоступно в [!INCLUDE[ws2003](../../../../includes/ws2003-md.md)] и [!INCLUDE[wxp](../../../../includes/wxp-md.md)]. WCF поддерживает счетчик прерываний в памяти, поэтому возможно, что это свойство не может содержать точное значение, если одно и то же сообщение считывается несколькими службами WCF в веб-ферме.  
  
## <a name="remote-transactional-read"></a>Удаленное чтение в транзакциях  
 MSMQ в Windows Vista поддерживает удаленные транзакционные операции чтения. Это позволяет приложению, считывающему из очереди, находиться на компьютере, отличном от компьютера, на котором размещена очередь. Этим обеспечивается возможность использования фермы служб, осуществляющих чтение из центральной очереди, что увеличивает общую производительность системы. Кроме того, если во время чтения и обработки сообщения возникает ошибка, такой подход позволяет откатить транзакцию, и сообщение останется в очереди для последующей обработки.  
  
## <a name="see-also"></a>См. также:

- [Использование очередей недоставленных сообщений для обработки сбоев при передаче сообщений](../../../../docs/framework/wcf/feature-details/using-dead-letter-queues-to-handle-message-transfer-failures.md)
- [Обработка подозрительных сообщений](../../../../docs/framework/wcf/feature-details/poison-message-handling.md)
