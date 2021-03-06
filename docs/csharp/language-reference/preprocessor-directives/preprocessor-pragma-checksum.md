---
title: '#Справочник по C#. #pragma checksum'
ms.custom: seodec18
ms.date: 07/20/2015
f1_keywords:
- '#pragma checksum'
helpviewer_keywords:
- '#pragma checksum [C#]'
ms.assetid: 3673e4ca-6098-4ec1-890f-8fceb2a794a2
ms.openlocfilehash: 4103b6262fc5085c1204f423a36c9c5c2053b497
ms.sourcegitcommit: 986f836f72ef10876878bd6217174e41464c145a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/19/2019
ms.locfileid: "69605650"
---
# <a name="pragma-checksum-c-reference"></a>#pragma checksum (Справочник по C#)
Создает контрольные суммы для исходных файлов, чтобы помочь с отладкой страниц ASP.NET.  
  
## <a name="syntax"></a>Синтаксис  
  
```csharp
#pragma checksum "filename" "{guid}" "checksum bytes"  
```  
  
## <a name="parameters"></a>Параметры  
 `"filename"`  
 Имя файла, который требует отслеживания изменений или обновлений.  
  
 `"{guid}"`  
 Глобальный уникальный идентификатор (GUID) для хэш-алгоритма.  
  
 `"checksum_bytes"`  
 Строка шестнадцатеричных цифр, представляющая байты контрольной суммы. Должно быть четным числом шестнадцатеричных цифр. Нечетное число цифр вызовет предупреждение во время компиляции, и директива будет пропущена.  
  
## <a name="remarks"></a>Примечания  
 Отладчик Visual Studio использует контрольную сумму, чтобы подтвердить нахождение правильного источника. Компилятор вычисляет контрольную сумму для исходного файла, а затем передает результат в файл базы данных (PDB) программы. Отладчик затем использует PDB-файл для сравнения с контрольной суммой, вычисленной им для исходного файла.  
  
 Это решение не работает для проектов ASP.NET, так как рассчитанная контрольная сумма относится к созданному исходному файлу, а не файлу ASPX. Чтобы решить эту проблему, `#pragma checksum` предоставляет поддержку контрольных сумм для страниц ASP.NET.  
  
 При создании проекта ASP.NET в Visual C# созданный исходный файл содержит контрольную сумму для ASPX-файла, из которого создается источник. Затем компилятор записывает эти данные в PDB-файл.  
  
 Если компилятор не обнаруживает директиву `#pragma checksum` в файле, он вычисляет контрольную сумму и записывает значение в PDB-файл.  
  
## <a name="example"></a>Пример  
  
```csharp
class TestClass  
{  
    static int Main()  
    {  
        #pragma checksum "file.cs" "{406EA660-64CF-4C82-B6F0-42D48172A799}" "ab007f1d23d9" // New checksum  
    }  
}  
```  
  
## <a name="see-also"></a>См. также

- [Справочник по C#](../index.md)
- [Руководство по программированию на C#](../../programming-guide/index.md)
- [Директивы препроцессора C#](./index.md)
