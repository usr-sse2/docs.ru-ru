---
title: Руководство по программированию на C#. Передача параметров
ms.custom: seodec18
ms.date: 07/20/2015
helpviewer_keywords:
- parameters [C#], passing
- passing parameters [C#]
- arguments [C#]
- methods [C#], passing parameters
- C# language, method parameters
ms.assetid: a5c3003f-7441-4710-b8b1-c79de77e0b77
ms.openlocfilehash: 22f58bda5aa5b60248902a4130f3ea9b6caa65cf
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/01/2019
ms.locfileid: "73419127"
---
# <a name="passing-parameters-c-programming-guide"></a>Передача параметров (Руководство по программированию в C#)
В C# аргументы могут передаваться параметрам либо по значению, либо по ссылке. Передача по ссылке позволяет изменять и сохранять измененные значения параметров членов функций, методов, свойств, индексаторов, операторов и конструкторов в вызывающей среде. Чтобы передать параметр по ссылке, намереваясь изменить значение, используйте ключевое слово `ref` или `out`. Чтобы передать по ссылке, намереваясь предотвратить копирование, но не изменение значения, используйте модификатор `in`. Для простоты в этих примерах используется только ключевое слово `ref`. Дополнительные сведения о различиях между ключевыми словами `in`, `ref` и `out` см. в разделах [in](../../language-reference/keywords/in-parameter-modifier.md), [ref](../../language-reference/keywords/ref.md), [out](../../language-reference/keywords/out-parameter-modifier.md).  
  
 В приведенном ниже примере показано различие между параметрами-значениями и ссылочными параметрами.  
  
 [!code-csharp[csProgGuideParameters#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideParameters/CS/Parameters.cs#10)]  
  
 Дополнительные сведения см. в следующих разделах:  
  
- [Передача параметров типа значения](./passing-value-type-parameters.md)  
  
- [Передача параметров ссылочного типа](./passing-reference-type-parameters.md)  
  
## <a name="c-language-specification"></a>Спецификация языка C#  

Дополнительные сведения см. в разделе [Список аргументов](~/_csharplang/spec/expressions.md#argument-lists) в [Спецификации языка C#](/dotnet/csharp/language-reference/language-specification/introduction). Спецификация языка является предписывающим источником информации о синтаксисе и использовании языка C#.
  
## <a name="see-also"></a>См. также

- [Руководство по программированию на C#](../index.md)
- [Методы](./methods.md)
