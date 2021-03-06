---
title: Использование средства привязки сериализации
ms.date: 03/30/2017
ms.assetid: ab46c087-200c-45bf-9c95-5a6cda6e8b98
ms.openlocfilehash: bfce2a14c8757250c520919c8ff2a4d7048a9d5c
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/16/2019
ms.locfileid: "74138648"
---
# <a name="usage-of-serialization-binder"></a>Использование средства привязки сериализации
В этом образце показано, как использовать <xref:System.Runtime.Serialization.SerializationBinder> для изменения версии универсального типа во время сериализации.  
  
## <a name="demonstrates"></a>Демонстрации  
 <xref:System.Runtime.Serialization.SerializationBinder>, <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>  
  
## <a name="discussion"></a>Обсуждение  
 В этом примере показано, как две сущности, предназначенные для разных версий .NET Framework, могут взаимодействовать с помощью двоичного модуля форматирования и связывателя сериализации.  
  
Этот пример был разработан с помощью удаленного взаимодействия .NET. Он состоит из сервера, предназначенного для .NET Framework 4, который реализует контракт с универсальными типами и двумя разными клиентами, один предназначен для .NET Framework 2,0 и другой целевой .NET Framework 4.  
  
 Сервер присоединяет средство привязки <xref:System.Runtime.Serialization.SerializationBinder> к двоичному форматировщику, чтобы иметь возможность соответствующим образом изменять версию типов при сериализации, в результате чего оба клиента смогут правильно десериализовать эти типы.  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>Настройка, построение и выполнение примера  
  
1. Чтобы выполнить клиент, щелкните правой кнопкой мыши решение Сбженериксвтс (6 проектов) и выберите пункт **Свойства**.  
  
2. В окне **Общие свойства**выберите **запускаемый проект**, а затем выберите **Несколько запускаемых проектов**.  
  
3. Сначала выберите **сервер** , затем **Client20** , а затем **Client40**. Выберите действие **Запуск** для этих трех проектов и оставьте для параметра остальные значение **нет**.  
  
4. Нажмите кнопку **ОК** , а затем нажмите клавишу F5, чтобы запустить пример.
