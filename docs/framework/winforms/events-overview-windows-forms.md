---
title: Общие сведения о событиях (Windows Forms)
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Forms, event handling
- events [Windows Forms], about events
- delegates [Windows Forms], multicast
- delegates [Windows Forms], events and
- multicast event delegates
- Windows Forms controls, events
ms.assetid: 814a6a43-a312-4791-88d8-f75f9a4f8c4c
ms.openlocfilehash: 92942066b5f08ada0154781ae54b5d8494944ca1
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/22/2019
ms.locfileid: "69963465"
---
# <a name="events-overview-windows-forms"></a>Общие сведения о событиях (Windows Forms)
Событие — это действие, требующее реагирования или "обработки" в коде. События могут генерироваться действиями пользователя (например, нажатием кнопки мыши или клавиши на клавиатуре), программным кодом или системой.

 Приложения, управляемые событиями, выполняют код в ответ на событие. Каждая форма и элемент управления имеют предопределенный набор событий, который можно запрограммировать. Если возникает такое событие, а в соответствующем обработчике событий имеется код, этот код выполняется.

 Типы порождаемых объектом событий могут варьироваться, но многие их них стандартны для большинства элементов управления. Например, большинство объектов обработают событие <xref:System.Windows.Forms.Control.Click>. Если пользователь откроет форму, в форме сработает код обработчика события <xref:System.Windows.Forms.Control.Click>.

> [!NOTE]
> Многие события возникают вместе с другими событиями. Например, при возникновении события <xref:System.Windows.Forms.Control.DoubleClick> возникают также события <xref:System.Windows.Forms.Control.MouseDown>, <xref:System.Windows.Forms.Control.MouseUp> и <xref:System.Windows.Forms.Control.Click>.

 Сведения о том, как вызывать и использовать событие, см. в разделе [события](../../standard/events/index.md).

## <a name="delegates-and-their-role"></a>Делегаты и их роли
 Делегаты — это классы, обычно используемые в .NET Framework для создания механизмов обработки событий. Делегаты приблизительно соответствуют указателям на функции, обычно используемым C++ в визуальных и других объектно-ориентированных языках. В отличие от указателей функций делегаты объектно-ориентированы, типобезопасны и безопасны. К тому же, если указатель функций содержит только ссылку на определенную функцию, то делегат содержит ссылку на объект и ссылки на один или несколько методов в этом объекте.

 Эта модель событий использует *делегаты* для привязки событий к методам, которые используются для их обработки. Делегаты позволяют другим классам записывать уведомление о событии, определяя метод обработки. При возникновении события делегат вызывает соответствующий метод. Дополнительные сведения об определении делегатов см. в разделе [события](../../standard/events/index.md).

 Делегаты можно связать с одним или несколькими методами, создав так называемую многоадресную рассылку. При создании делегата для события обычно создается многоадресное событие (или Windows). Редким исключением является событие, вызывающее выполнение определенной процедуры (например, отображение диалогового окна), которая не будет логически повторяться несколько раз за событие. Сведения о создании делегата многоадресной рассылки см. [в разделе как Объединение делегатов (многоадресные делегаты)](../../csharp/programming-guide/delegates/how-to-combine-delegates-multicast-delegates.md).

 Делегат многоадресной рассылки поддерживает список вызова методов, к которым он привязан. Делегат многоадресной рассылки поддерживает метод <xref:System.Delegate.Combine%2A>, позволяющий добавить метод в список вызова, и метод <xref:System.Delegate.Remove%2A>, позволяющий его удалить.

 Когда приложение регистрирует событие, элемент управления порождает это событие, вызывая для него делегат. Делегат, в свою очередь, вызывает соответствующий метод. В самом распространенном случае (делегат многоадресной рассылки) делегат вызывает каждый метод связки из списка вызова по очереди, что обеспечивает уведомление один-ко-многим. Данная стратегия означает, что элементу управления не нужно вести список целевых объектов для уведомления о событии, поскольку записью и уведомлением занимается делегат.

 Делегаты также позволяют связать с одним методом несколько событий, чтобы использовать уведомление по типу многие-к-одному. Например, событие нажатия на кнопку и событие выбора команды в меню вызывают один и тот же делегат, который вызывает один и тот же метод, обрабатывающий эти события одинаковым образом.

 В делегатах используется динамический механизм связки: во время выполнения делегат может быть связан с любым методом, подпись которого совпадает с подписью обработчика событий. С помощью этой функции можно устанавливать или изменять метод связки в зависимости от условий и динамически привязывать обработчик событий к элементу управления.

## <a name="see-also"></a>См. также

- [Создание обработчиков событий в Windows Forms](creating-event-handlers-in-windows-forms.md)
- [Общие сведения об обработчиках событий](event-handlers-overview-windows-forms.md)
