---
title: Создание собственных элементов управления
ms.date: 03/30/2017
helpviewer_keywords:
- controls [Windows Forms], user controls
- controls [Windows Forms], types of
- composite controls [Windows Forms]
- extended controls [Windows Forms]
- controls [Windows Forms], extended
- user controls [Windows Forms]
- custom controls [Windows Forms]
- controls [Windows Forms], composite
ms.assetid: 3cea09e5-4344-4ccb-9858-b66ccac210ff
ms.openlocfilehash: 106da550fc6e6c50bc40e103e1f855059a9ffa9c
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/22/2019
ms.locfileid: "69950093"
---
# <a name="varieties-of-custom-controls"></a>Создание собственных элементов управления
Платформа .NET Framework предоставляет возможности разработки и реализации новых элементов управления. С ее помощью можно расширить функциональные возможности привычных пользовательских элементов управления, а также уже существующих элементов управления через наследование. Кроме того, она позволяет писать настраиваемые элементы управления с собственной отрисовкой.  
  
 Выбор типа создаваемого элемента управления может быть затруднителен. В этом разделе описываются различия между типами элементов управления, которые можно использовать для наследования, и рассказывается, как выбрать тип элемента управления для конкретного проекта.  
  
> [!NOTE]
> Дополнительные сведения о создании элемента управления для использования в Web Forms см. в разделе [Разработка пользовательских серверных элементов управления ASP.NET](https://docs.microsoft.com/previous-versions/aspnet/zt27tfhy(v=vs.100)).  
  
## <a name="base-control-class"></a>Базовый класс элемента управления  
 <xref:System.Windows.Forms.Control> Класс является базовым классом для элементов управления Windows Forms. Он обеспечивает инфраструктуру, необходимую для визуального отображения элементов управления в приложениях Windows Forms.  
  
 <xref:System.Windows.Forms.Control> Класс выполняет следующие задачи для предоставления визуального отображения в Windows Forms приложениях:  
  
- Обеспечивает обработку окон.  
  
- Управляет маршрутизацией сообщений.  
  
- Предоставляет события мыши и клавиатуры, а также многие другие события пользовательского интерфейса.  
  
- Предоставляет расширенные функции размещения.  
  
- Содержит множество свойств, характерных для визуального отображения <xref:System.Windows.Forms.Control.ForeColor%2A>, <xref:System.Windows.Forms.Control.BackColor%2A>таких <xref:System.Windows.Forms.Control.Height%2A>как, <xref:System.Windows.Forms.Control.Width%2A>, и.  
  
- Обеспечивает безопасность и поддержку потоков, необходимые для того, чтобы элемент управления Windows Forms действовал как элемент управления Microsoft® ActiveX®.  
  
 Поскольку существенная часть инфраструктуры предоставляется базовым классом, разрабатывать собственные элементы управления Windows Forms довольно просто.  
  
## <a name="kinds-of-controls"></a>Типы элементов управления  
 Windows Forms поддерживает три типа элементов управления, определяемых пользователем: *составные*, *расширенные* и *настраиваемые*. В следующих разделах описывается каждый тип элемента управления и приводятся рекомендации по выбору типа для использования в проектах.  
  
### <a name="composite-controls"></a>Составные элементы управления  
 Составной элемент управления — это коллекция элементов управления Windows Forms, инкапсулированных в общий контейнер. Этот тип элементов управления иногда называют *пользовательским элементом управления*. Элементы, входящие в составной элемент управления, называются *составляющими*.  
  
 Составной элементе управления содержит все унаследованные функциональные возможности, связанные с каждым входящим в него элементом управления Windows Forms, и позволяет выборочно представлять и связывать их свойства. Кроме того, составной элемент управления предоставляет немало функций для обработки событий клавиатуры по умолчанию, не требуя дополнительной разработки с вашей стороны.  
  
 Например, составной элемент управления можно собрать таким образом, чтобы он отображал данные адреса клиента из базы данных. Этот элемент управления может включать <xref:System.Windows.Forms.DataGridView> элемент управления для вывода полей базы данных <xref:System.Windows.Forms.BindingSource> , для управления привязкой к источнику <xref:System.Windows.Forms.BindingNavigator> данных и для перемещения по записям. Вы можете выборочно предоставить свойства привязки данных, а также упаковать и повторно использовать весь элемент управления в других приложениях. Пример такого рода составного элемента управления см. в разделе [как Применение атрибутов в элементах](how-to-apply-attributes-in-windows-forms-controls.md)управления Windows Forms.  
  
 Чтобы создать составной элемент управления, необходимо создать <xref:System.Windows.Forms.UserControl> класс, производный от класса. <xref:System.Windows.Forms.UserControl> Базовый класс обеспечивает маршрутизацию клавиатуры для дочерних элементов управления и позволяет дочерним элементам управления работать как с группой. Дополнительные сведения см. в разделе [Разработка составного элемента Windows Forms](developing-a-composite-windows-forms-control.md).  
  
 **Рекомендация**  
  
 Наследование класса <xref:System.Windows.Forms.UserControl> имеет смысл в следующих случаях:  
  
- — если нужно объединить функциональные возможности нескольких элементов управления Windows Forms в один блок для повторного использования.  
  
### <a name="extended-controls"></a>Расширенные элементы управления  
 Элемент управления можно унаследовать от любого существующего элемента управления Windows Forms. Такой подход позволяет сохранить все функциональные возможности, унаследованные от элемента управления Windows Forms, и расширить их путем добавления пользовательских свойств, методов или других функций. С помощью этого параметра можно переопределить логику отрисовки базового элемента управления, а затем расширить его пользовательский интерфейс, изменив его внешний вид.  
  
 Например, можно создать элемент управления, производный от <xref:System.Windows.Forms.Button> элемента управления, который отслеживает, сколько раз пользователь щелкнул его.  
  
 В некоторых элементах управления можно также добавить пользовательский внешний вид к графическому пользовательскому интерфейсу элемента управления, переопределив <xref:System.Windows.Forms.Control.OnPaint%2A> метод базового класса. Для расширенной кнопки, отслеживающей нажатия клавиш, можно <xref:System.Windows.Forms.Control.OnPaint%2A> переопределить метод для вызова базовой <xref:System.Windows.Forms.Control.OnPaint%2A>реализации, а затем нарисовать число щелчков <xref:System.Windows.Forms.Button> в одном углу клиентской области элемента управления.  
  
 **Рекомендация**  
  
 Наследование элементов управления Windows Forms имеет смысл в следующих случаях:  
  
- — если большинство необходимых функций аналогичны функциям уже существующего элемента управления Windows Forms;  
  
- — если нестандартный графический интерфейс не требуется или необходимо разработать новый интерфейс для существующего элемента управления.  
  
### <a name="custom-controls"></a>Пользовательские элементы управления  
 Другим способом создания элемента управления является его создание с самого начала путем наследования от <xref:System.Windows.Forms.Control>. <xref:System.Windows.Forms.Control> Класс предоставляет все основные функциональные возможности, необходимые элементам управления, включая события обработки мыши и клавиатуры, но не связанные с контролем функции или графический интерфейс.  
  
 Создание элемента управления путем наследования от <xref:System.Windows.Forms.Control> класса требует гораздо больше усилий, чем наследование от <xref:System.Windows.Forms.UserControl> или существующего элемента управления Windows Forms. Поскольку значительную часть реализации вы выполняете сами, ваш элемент управления может быть более гибким, чем составной или расширенный, и вы можете его адаптировать к конкретным задачам.  
  
 Для реализации пользовательского элемента управления необходимо написать код для <xref:System.Windows.Forms.Control.OnPaint%2A> события элемента управления, а также любой необходимый для конкретного компонента код. Кроме того, можно переопределить <xref:System.Windows.Forms.Control.WndProc%2A> метод и напрямую управлять сообщениями Windows. Это самый эффективный способ создания элементов управления, однако для того, чтобы использовать его эффективно, необходимо знание API Microsoft Win32®.  
  
 Примером нестандартного элемента управления служит элемент управления "Часы", который выглядит и действует как часы со стрелками. Пользовательское рисование вызывается для того, чтобы стрелки часов переводились в ответ на <xref:System.Windows.Forms.Timer.Tick> события из внутреннего <xref:System.Windows.Forms.Timer> компонента. Дополнительные сведения см. в разделе [Практическое руководство. Разработка простого Windows Forms элемента управления](how-to-develop-a-simple-windows-forms-control.md).  
  
 **Рекомендация**  
  
 Наследование класса <xref:System.Windows.Forms.Control> имеет смысл в следующих случаях:  
  
- — если требуется создать пользовательское графическое представление элемента управления;  
  
- — если требуется реализовать пользовательские функциональные возможности, которые недоступны в стандартных элементах управления.  
  
### <a name="activex-controls"></a>Элементы управления ActiveX  
 Несмотря на то что инфраструктура Windows Forms оптимизирована для размещения элементов управления Windows Forms, элементы управления ActiveX также можно использовать. Эта задача поддерживается в Visual Studio. Дополнительные сведения см. в разделе [Практическое руководство. Добавление элементов управления ActiveX в](how-to-add-activex-controls-to-windows-forms.md)Windows Forms.  
  
### <a name="windowless-controls"></a>Элементы управления без окон  
 Технологии Microsoft Visual Basic® 6.0 и ActiveX поддерживают элементы управления *без окон*. В Windows Forms элементы управления без окон не поддерживаются.  
  
## <a name="custom-design-experience"></a>Пользовательская среда разработки  
 Если вам требуется пользовательская среда разработки, вы можете создать свой собственный конструктор. Для составных элементов управления необходимо создать класс пользовательского конструктора из <xref:System.Windows.Forms.Design.ParentControlDesigner> <xref:System.Windows.Forms.Design.DocumentDesigner> классов или. Для расширенных и настраиваемых элементов управления создайте класс пользовательского конструктора из <xref:System.Windows.Forms.Design.ControlDesigner> класса.  
  
 Используйте, <xref:System.ComponentModel.DesignerAttribute> чтобы связать элемент управления с конструктором. Дополнительные сведения см. в статьях [расширение поддержки времени разработки](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/37899azc(v=vs.120)) и [инструкции. Создайте элемент управления Windows Forms, который использует преимущества функций](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/307hck25(v=vs.120))времени разработки.  
  
## <a name="see-also"></a>См. также

- [Разработка пользовательских элементов управления Windows Forms в .NET Framework](developing-custom-windows-forms-controls.md)
- [Практическое руководство. Разработка простого элемента управления Windows Forms](how-to-develop-a-simple-windows-forms-control.md)
- [Разработка составного элемента Windows Forms](developing-a-composite-windows-forms-control.md)
- [Расширения поддержки времени разработки](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/37899azc(v=vs.120))
- [Практическое руководство. Создание элемента управления Windows Forms, который использует преимущества функций времени разработки](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/307hck25(v=vs.120))
