---
title: Директивы компилятора
description: Сведения о F# директивах препроцессора языка, директивах условной компиляции, директивах Line и директивах компилятора.
ms.date: 12/10/2018
ms.openlocfilehash: 16db2efb2fee2c2c5e94aa98eb0a13183a4e0e0b
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68630401"
---
# <a name="compiler-directives"></a>Директивы компилятора

В этом разделе описываются директивы процессора и директивы компилятора.

## <a name="preprocessor-directives"></a>Директивы препроцессора

Директива препроцессора дополняется префиксом с символом «#» и отображается в строке сама по себе. Она интерпретируется препроцессором, который запускается перед самим компилятором.

В следующей таблице перечислены директивы препроцессора, имеющиеся в F#.

|Директива|Описание|
|---------|-----------|
|`#if`*символ*|Поддерживает условную компиляцию. Код в разделе после `#if` включается, если *символ* определен. Символ также может быть инвертирован с помощью `!`.|
|`#else`|Поддерживает условную компиляцию. Помечает раздел кода, который следует включить, если символ, используемый с предыдущей директивой `#if`, не определен.|
|`#endif`|Поддерживает условную компиляцию. Помечает конец условного раздела кода.|
|`#`штрих *int*,<br/>`#`штрих *тип int* *строка*,<br/>`#`штрих *тип int* *буквальная строка*|Указывает исходную строку кода и имя файла для отладки. Эта возможность предназначена для средств, создающих исходный код F#.|
|`#nowarn`*варнингкоде*|Отключает предупреждение или предупреждения компилятора. Чтобы отключить предупреждение, найдите его номер в выходных данных компилятора и заключите его в кавычки. Не указывайте префикс «FS». Чтобы отключить несколько номеров предупреждений в одной строке, заключите каждый номер в кавычки и отделяйте каждую строку пробелом. Например:

`#nowarn "9" "40"`

Результат отключения предупреждения применяется ко всему файлу, включая фрагменты файла, предшествующие директиве. |

## <a name="conditional-compilation-directives"></a>Директивы условной компиляции

Код, Деактивируемый одной из этих директив, недоступен в редакторе Visual Studio Code.

> [!NOTE]
> Поведение директив условной компиляции отличается от их поведения в других языках. Например, нельзя использовать логические выражения с символами, а `true` и `false` не имеют особого значения. Символы, используемые в директиве `if`, должны задаваться с помощью командной строки или в параметрах проекта; в этом языке отсутствует директива препроцессора `define`.

В следующем коде демонстрируется применение директив `#if`, `#else` и `#endif`. В данном примере код содержит две версии определения `function1`. Если `VERSION1` определяется с помощью [параметра компилятора-define](https://msdn.microsoft.com/library/434394ae-0d4a-459c-a684-bffede519a04), активируется код между `#if` директивой и `#else` директивой. В противном случае активируется код между директивами `#else` и `#endif`.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet7301.fs)]

В языке F# отсутствует директива препроцессора `#define`. Вы должны использовать параметр компилятора или параметры проекта для определения символов, используемых директивой `#if`.

Директивы условной компиляции не могут быть вложенными. В директивах препроцессора отступ не важен.

Можно также инвертировать символ с помощью `!`. В этом примере значением строки является нечто, только если _не_ выполняется отладка:

```fsharp
#if !DEBUG
let str = "Not debugging!"
#else
let str = "Debugging!"
#endif
```

## <a name="line-directives"></a>Директивы строк

При сборке компилятор сообщает об ошибках в коде F #, ссылаясь на номера строк, в которых возникли ошибки. Номера строк начинаются с 1 для первой строки в файле. Тем не менее при создании исходного кода F# из другого средства номера строк в сформированном коде обычно не представляют интереса, поскольку ошибки в сформированном коде F#, скорее всего, проистекают из другого источника. Директива `#line` позволяет разработчикам средств, формирующих исходный код F#, передавать сведения о номерах исходных строк и исходных файлах в сформированный код F#.

При использовании директивы `#line` необходимо заключать имена файлов в кавычки. Если в начале строки не указывается токен verbatim (`@`), то чтобы использовать в пути символы обратной косой черты, необходимо их экранировать, указывая две обратные косые черты вместо одной. Далее приводятся допустимые токены строк. В этих примерах предполагается, что исходный файл `Script1` при запуске в соответствующем средстве приводит к автоматическому созданию файла кода F# и что код в месте расположения этих директив формируется из некоторых токенов в строке 25 файла `Script1`.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet7303.fs)]

Эти токены указывают, что код F#, сформированный в этом месте, является производным от некоторых конструкций в строке `25` или рядом с ней в файле `Script1`.

## <a name="compiler-directives"></a>Директивы компилятора

Директивы компилятора сходны с директивами препроцессора, поскольку они имеют префикс в виде знака #, но они не интерпретируются препроцессором; компилировать и действовать в соответствии с этими директивами должен компилятор.

В следующей таблице перечислены директивы компилятора, доступные в языке F#.

|Директива|Описание|
|---------|-----------|
|`#light`["on"&#124;"OFF"]|Включает или отключает упрощенный синтаксис для совместимости с другими версиями ML. Упрощенный синтаксис включен по умолчанию. Подробный синтаксис всегда включен. Таким образом, вы можете использовать как упрощенный, так и подробный синтаксис. Директива `#light` сама по себе эквивалентна `#light "on"`. Если указана директива `#light "off"`, то для всех языковых конструкций необходимо использовать подробный синтаксис. Синтаксис в документации по F# представлен исходя из предположения, что используется упрощенный синтаксис. Дополнительные сведения см. в разделе [подробный синтаксис](verbose-syntax.md).|

Директивы интерпретатора (FSI. exe) см. [в разделе интерактивное F#программирование с помощью ](../tutorials/fsharp-interactive/index.md).

## <a name="see-also"></a>См. также

- [Справочник по языку F#](index.md)
- [Параметры компилятора](compiler-options.md)
