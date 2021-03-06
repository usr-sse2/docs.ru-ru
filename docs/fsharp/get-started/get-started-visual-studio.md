---
title: Начало работы с F# Visual Studio
description: Узнайте, как использовать F# с Visual Studio.
ms.date: 07/03/2018
ms.openlocfilehash: 80b4fc5b7631eace719832fe32003cad578ead27
ms.sourcegitcommit: 93762e1a0dae1b5f64d82eebb7b705a6d566d839
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/27/2019
ms.locfileid: "74552822"
---
# <a name="get-started-with-f-in-visual-studio"></a>Начало работы с F# Visual Studio

F#и визуальные F# средства поддерживаются в интегрированной среде разработки Visual Studio.

Для начала убедитесь, что у вас [установлена Visual Studio с F# ](install-fsharp.md#install-f-with-visual-studio).

## <a name="creating-a-console-application"></a>Создание консольного приложения

Одним из самых основных проектов в Visual Studio является консольное приложение.  Вот как это делается.  После открытия Visual Studio:

1. В меню **файл** наведите указатель мыши на пункт **создать**и выберите **проект**.

2. В диалоговом окне Новый проект в разделе **шаблоны**отобразится **визуальный F#** элемент.  Выберите этот параметр F# , чтобы отобразить шаблоны.

3. Выберите **консольное приложение .NET Core** или **консольное приложение**.

4. Нажмите кнопку **ОК** , чтобы создать F# проект.  Теперь вы увидите F# проект в Обозреватель решений.

## <a name="writing-your-code"></a>Написание кода

Приступим к работе, сначала напишем некоторый код.  Убедитесь, что файл `Program.fs` открыт, и замените его содержимое следующим:

[!code-fsharp[HelloSquare](~/samples/snippets/fsharp/getting-started/hello-square.fs)]

В предыдущем примере кода определена функция `square`, которая принимает входные данные с именем `x` и умножает ее на саму себя.  Поскольку F# использует [вывод типа](../language-reference/type-inference.md), указывать тип `x` не нужно.  F# Компилятор понимает типы, в которых умножение является допустимым, и присвоит тип `x` в зависимости от того, как вызывается `square`.  При наведении указателя мыши на `square`вы должны увидеть следующее:

```fsharp
val square: x:int -> int
```

Это называется сигнатурой типа функции.  Его можно прочитать следующим образом: "Square — это функция, которая принимает целое число с именем" x "и создает целое число".  Обратите внимание, что компилятор выдает `square` тип `int` для Now, так как умножение не является универсальным для *всех* типов, а является универсальным по отношению к закрытому набору типов.  F# Компилятор выбрал `int` на этом этапе, но при вызове `square` с другим типом входных данных, например `float`, будет настроена сигнатура типа.

Определена другая функция `main`, которая снабжена атрибутом `EntryPoint`, чтобы сообщить F# компилятору, что должно начаться выполнение программы.  Он соответствует тому же соглашению, что и другие [языки программирования в стиле C](https://en.wikipedia.org/wiki/Entry_point#C_and_C.2B.2B), где аргументы командной строки могут передаваться в эту функцию, и возвращается целочисленный код (обычно `0`).

Эта функция вызывает функцию `square` с аргументом `12`.  Затем F# компилятор назначает тип `square` `int -> int` (то есть функция, которая принимает `int` и создает `int`).  Вызов `printfn` является отформатированной функцией печати, которая использует строку формата, аналогичную языку программирования в стиле C, параметры, соответствующие указанным в строке формата, а затем выводят результат и новую строку.

## <a name="running-your-code"></a>Выполнение кода

Вы можете запустить код и просмотреть результаты, нажав клавиши **Ctrl**+**F5**.  Это запускает программу без отладки и позволяет просматривать результаты.  Кроме того, можно выбрать пункт **Отладка** пункта меню верхнего уровня в Visual Studio и выбрать **Запуск без отладки**.

Теперь в окне консоли, которое было извлечено Visual Studio, должны отобразиться следующие сведения:

```console
12 squared is 144!
```

Поздравляем!  Вы создали первый F# проект в Visual Studio, записали F# функцию на печать результатов вызова этой функции и запустили проект, чтобы увидеть некоторые результаты.

## <a name="next-steps"></a>Следующие шаги

Если вы еще этого не сделали, ознакомьтесь с [обзором F# ](../tour.md), в котором рассматриваются некоторые основные функции F# языка.  Он предоставляет обзор некоторых возможностей F#и предоставляет примеры кода, которые можно скопировать в Visual Studio и выполнить.  Дополнительные сведения о F# документации [ F# ](../index.yml)можно получить на главной странице документация.

## <a name="see-also"></a>См. также:

- [Обзор языка F#](../tour.md)
- [F#Справочник по языку](../language-reference/index.md)
- [Определение типа](../language-reference/type-inference.md)
- [Справочник по символам и операторам](../language-reference/symbol-and-operator-reference/index.md)
