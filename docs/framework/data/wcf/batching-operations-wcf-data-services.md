---
title: Пакетные операции (службы данных WCF)
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, client library
ms.assetid: 962a49d1-cc11-4b96-bc7d-071dd6607d6c
ms.openlocfilehash: 48c0a5f1573f64396971e1265dbf467300a98406
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2019
ms.locfileid: "74569354"
---
# <a name="batching-operations-wcf-data-services"></a>Пакетные операции (службы данных WCF)
Open Data Protocol (OData) поддерживает пакетную обработку запросов к службе на основе OData. Дополнительные сведения см. в разделе [OData: пакетная обработка](https://go.microsoft.com/fwlink/?LinkId=186075). В WCF Data Services каждая операция, использующая <xref:System.Data.Services.Client.DataServiceContext>, например выполнение запроса или сохранение изменений, приводит к отправке в службу данных отдельного запроса. Для создания логической области видимости для набора операций можно явно определить пакеты операций. Это гарантирует, что все операции в пакете отправляются в службу данных в одном HTTP-запросе, что позволяет серверу обрабатывать операции атомарно и сокращает количество обращений к службе данных.  
  
## <a name="batching-query-operations"></a>Операции пакетных запросов  
 Для выполнения нескольких запросов в одном пакете необходимо создать каждый запрос пакета в виде отдельного экземпляра класса <xref:System.Data.Services.Client.DataServiceRequest%601>. При создании запроса таким способом сам он определяется как URI и соответствует правилам адресации ресурсов. Дополнительные сведения см. в разделе [доступ к ресурсам службы данных](accessing-data-service-resources-wcf-data-services.md). Пакетные запросы отправляются в службу данных при вызове метода <xref:System.Data.Services.Client.DataServiceContext.ExecuteBatch%2A>, содержащего объекты запросов. Этот метод возвращает объект <xref:System.Data.Services.Client.DataServiceResponse>, представляющий собой коллекцию объектов <xref:System.Data.Services.Client.QueryOperationResponse%601>, каждый из которых содержит ответ на индивидуальный запрос в пакете, то есть либо коллекцию возвращаемых объектов, либо сведения об ошибке. Если отдельная операция в пакете завершается с ошибкой, сведения об ошибке возвращаются в объекте <xref:System.Data.Services.Client.QueryOperationResponse%601> для конкретной операции, в то время как оставшиеся операции все равно выполняются. Дополнительные сведения см. в разделе [инструкции. выполнение запросов в пакете](how-to-execute-queries-in-a-batch-wcf-data-services.md).  
  
 Пакетные запросы также могут быть выполнены асинхронно. Дополнительные сведения см. в разделе [асинхронные операции](asynchronous-operations-wcf-data-services.md).  
  
## <a name="batching-the-savechanges-operation"></a>Пакетирование операции SaveChanges  
 При вызове метода <xref:System.Data.Services.Client.DataServiceContext.SaveChanges%2A> все изменения, отслеживаемые контекстом, преобразуются в операции на основе RESTFUL, которые отправляются как запросы к службе OData. По умолчанию эти изменения не отправляются в одном сообщении запроса. Чтобы все изменения отправлялись в одном запросе, необходимо вызвать метод <xref:System.Data.Services.Client.DataServiceContext.SaveChanges%28System.Data.Services.Client.SaveChangesOptions%29> и включить значение <xref:System.Data.Services.Client.SaveChangesOptions.Batch> в перечисление <xref:System.Data.Services.Client.SaveChangesOptions>, которое указывается в методе.  
  
 Можно также сохранить пакет изменений асинхронно. Дополнительные сведения см. в разделе [асинхронные операции](asynchronous-operations-wcf-data-services.md).  
  
## <a name="see-also"></a>См. также:

- [Библиотека клиентов служб данных WCF](wcf-data-services-client-library.md)
