---
title: Дополнительное форматирование текста
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- formatting [WPF]
- text [WPF]
- typography [WPF], text formatting
ms.assetid: f0a7986e-f5b2-485c-a27d-f8e922022212
ms.openlocfilehash: 2c120c6d71cb22bc38909f980b2f6faf2b5c3663
ms.sourcegitcommit: 2e95559d957a1a942e490c5fd916df04b39d73a9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2019
ms.locfileid: "72395219"
---
# <a name="advanced-text-formatting"></a>Дополнительное форматирование текста
Windows Presentation Foundation (WPF) предоставляет надежный набор интерфейсов API для включения текста в приложение. Макет и [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)]APIs, такие как <xref:System.Windows.Controls.TextBlock>, предоставляют наиболее распространенные и общие элементы использования для представления текста. API рисования, например <xref:System.Windows.Media.GlyphRunDrawing> и <xref:System.Windows.Media.FormattedText>, предоставляют средства для включения форматированного текста в рисунки. На самом продвинутом уровне [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] предоставляет расширяемый механизм форматирования текста для управления каждым аспектом представления текста, например управления хранилищем текста, управления форматированием текста и внедренными объектами.  
  
 В этом разделе приводятся общие сведения о форматировании текста [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]. Основное внимание уделяется реализации клиента и использованию механизма форматирования текста [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)].  
  
> [!NOTE]
> Все примеры кода в этом документе можно найти в [примере расширенного форматирования текста](https://go.microsoft.com/fwlink/?LinkID=159965).  

<a name="prereq"></a>   
## <a name="prerequisites"></a>Необходимые компоненты  
 В этом разделе предполагается, что вы знакомы с интерфейсами API более высокого уровня, используемыми для представления текста. Большинство пользовательских сценариев не потребует дополнительных интерфейсов API форматирования текста, обсуждаемых в этом разделе. Общие сведения о различных текстовых интерфейсах API см. [в разделе документы в WPF](documents-in-wpf.md).  
  
<a name="section1"></a>   
## <a name="advanced-text-formatting"></a>Дополнительное форматирование текста  
 Элементы управления Text Layout и [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] в [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] предоставляют свойства форматирования, позволяющие легко включать в приложение форматированный текст. Эти элементы управления предоставляют ряд свойств для обработки представления текста, включая гарнитуру, размер и цвет. В обычных условиях эти элементы управления могут обрабатывать большинство вариантов представления текста в приложении. Тем не менее некоторые расширенные сценарии требуют управления хранением текста, а также его представлением. [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] предоставляет расширяемый механизм форматирования текста для этой цели.  
  
 Функции расширенного форматирования текста, доступные в [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)], состоят из подсистемы форматирования текста, хранилища текста, текстовых запусков и свойств форматирования. Модуль форматирования текста, <xref:System.Windows.Media.TextFormatting.TextFormatter>, создает строки текста, используемые для представления. Это достигается путем инициации процесса форматирования строк и вызова <xref:System.Windows.Media.TextFormatting.TextFormatter.FormatLine%2A> модуля форматирования текста. Модуль форматирования текста получает текстовые фрагменты из хранилища текста, вызывая метод <xref:System.Windows.Media.TextFormatting.TextSource.GetTextRun%2A> в магазине. Затем объекты <xref:System.Windows.Media.TextFormatting.TextRun> формируются в объекты <xref:System.Windows.Media.TextFormatting.TextLine> с помощью модуля форматирования текста и передаются в приложение для проверки или вывода.  
  
<a name="section2"></a>   
## <a name="using-the-text-formatter"></a>Использование модуля форматирования текста  
 <xref:System.Windows.Media.TextFormatting.TextFormatter> является подсистемой форматирования текста [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] и предоставляет службы для форматирования и разбиения текстовых строк. Модуль форматирования текста может обрабатывать различные форматы текстовых символов и стили абзацев и включает поддержку международного макета текста.  
  
 В отличие от традиционного текстового API, <xref:System.Windows.Media.TextFormatting.TextFormatter> взаимодействует с клиентом макета текста через набор методов обратного вызова. Он требует, чтобы клиент предоставил эти методы в реализации класса <xref:System.Windows.Media.TextFormatting.TextSource>. На следующей схеме показано взаимодействие макета текста между клиентским приложением и <xref:System.Windows.Media.TextFormatting.TextFormatter>.  
  
 ![Схема клиента структуры текста и TextFormatter](./media/advanced-text-formatting/text-layout-textformatter-interaction.png)  
  
 Модуль форматирования текста используется для извлечения форматированных текстовых строк из хранилища текста, что является реализацией <xref:System.Windows.Media.TextFormatting.TextSource>. Для этого сначала создается экземпляр модуля форматирования текста с помощью метода <xref:System.Windows.Media.TextFormatting.TextFormatter.Create%2A>. Этот метод создает экземпляр модуля форматирования текста и задает максимальные значения высоты и ширины строки. Как только экземпляр модуля форматирования текста будет создан, процесс создания строки начнется с вызова метода <xref:System.Windows.Media.TextFormatting.TextFormatter.FormatLine%2A>. <xref:System.Windows.Media.TextFormatting.TextFormatter> выполняет обратный вызов к источнику текста, чтобы получить текст и параметры форматирования для фрагментов текста, образующих строку.  
  
 В следующем примере показан процесс форматирования хранилища текста. Объект <xref:System.Windows.Media.TextFormatting.TextFormatter> используется для получения текстовых строк из хранилища текста и последующего форматирования текстовой строки для рисования в <xref:System.Windows.Media.DrawingContext>.  
  
 [!code-csharp[TextFormatterExample#100](~/samples/snippets/csharp/VS_Snippets_Wpf/TextFormatterExample/CSharp/Window1.xaml.cs#100)]
 [!code-vb[TextFormatterExample#100](~/samples/snippets/visualbasic/VS_Snippets_Wpf/TextFormatterExample/VisualBasic/Window1.xaml.vb#100)]  
  
<a name="section3"></a>   
## <a name="implementing-the-client-text-store"></a>Реализация клиентского хранилища текста  
 При расширении механизма форматирования текста необходимо реализовать все аспекты хранилища текста и управлять ими. Это довольно сложная задача. Хранилище текста отвечает за отслеживание свойств фрагментов текста, свойств абзаца, встроенных объектов и других подобных элементов. Он также предоставляет модуль форматирования текста с отдельными объектами <xref:System.Windows.Media.TextFormatting.TextRun>, которые модуль форматирования текста использует для создания объектов <xref:System.Windows.Media.TextFormatting.TextLine>.  
  
 Чтобы обрабатывать виртуализацию хранилища текста, хранилище текста должно быть производным от <xref:System.Windows.Media.TextFormatting.TextSource>. <xref:System.Windows.Media.TextFormatting.TextSource> определяет метод, используемый модулем форматирования текста для получения текстовых запусков из хранилища текста. <xref:System.Windows.Media.TextFormatting.TextSource.GetTextRun%2A> — это метод, используемый модулем форматирования текста для получения текстовых запусков, используемых в форматировании строк. Вызов <xref:System.Windows.Media.TextFormatting.TextSource.GetTextRun%2A> многократно выполняется модулем форматирования текста, пока не произойдет одно из следующих условий:  
  
- Возвращается <xref:System.Windows.Media.TextFormatting.TextEndOfLine> или подкласс.  
  
- Суммарная ширина выполнения текста превышает максимальную ширину линии, указанную в вызове для создания модуля форматирования текста, или вызов метода <xref:System.Windows.Media.TextFormatting.TextFormatter.FormatLine%2A> модуля форматирования текста.  
  
- Возвращается последовательность символов новой строки в Юникоде, например "CF", "LF" или "CRLF".  
  
<a name="section4"></a>   
## <a name="providing-text-runs"></a>Предоставление фрагментов текста  
 Ядром процесса форматирования текста является взаимодействие между модулем форматирования текста и хранилищем текста. Ваша реализация <xref:System.Windows.Media.TextFormatting.TextSource> предоставляет модуль форматирования текста с объектами <xref:System.Windows.Media.TextFormatting.TextRun> и свойствами, используемыми для форматирования текстовых запусков. Это взаимодействие обрабатывается методом <xref:System.Windows.Media.TextFormatting.TextSource.GetTextRun%2A>, который вызывается модулем форматирования текста.  
  
 В следующей таблице показаны некоторые предопределенные объекты <xref:System.Windows.Media.TextFormatting.TextRun>.  
  
|Тип TextRun|Использование|  
|------------------|-----------|  
|<xref:System.Windows.Media.TextFormatting.TextCharacters>|Специализированный фрагмент текста, который используется для передачи представления глифов символов обратно в модуль форматирования текста.|  
|<xref:System.Windows.Media.TextFormatting.TextEmbeddedObject>|Специализированный фрагмент текста, который используется для предоставления содержимого, в котором измерение, проверка нажатия и рисование выполняются в целом (например, кнопка или изображение в тексте).|  
|<xref:System.Windows.Media.TextFormatting.TextEndOfLine>|Специализированный фрагмент текста, который используется для обозначения конца строки.|  
|<xref:System.Windows.Media.TextFormatting.TextEndOfParagraph>|Специализированный фрагмент текста, который используется для обозначения конца абзаца.|  
|<xref:System.Windows.Media.TextFormatting.TextEndOfSegment>|Специализированный тестовый запуск, используемый для обозначения конца сегмента, например для завершения области, затронутой предыдущим <xref:System.Windows.Media.TextFormatting.TextModifier>.|  
|<xref:System.Windows.Media.TextFormatting.TextHidden>|Специализированный фрагмент текста, который используется для отметки диапазона скрытых символов.|  
|<xref:System.Windows.Media.TextFormatting.TextModifier>|Специализированный фрагмент текста, который используется для изменения свойств фрагментов текста в своей области. Область расширяется до следующего сопоставления <xref:System.Windows.Media.TextFormatting.TextEndOfSegment> или следующего <xref:System.Windows.Media.TextFormatting.TextEndOfParagraph>.|  
  
 Любой из предопределенных объектов <xref:System.Windows.Media.TextFormatting.TextRun> может быть подклассом. Это позволяет источнику текста предоставлять модуль форматирования текста с фрагментами текста, включающими пользовательские данные.  
  
 В следующем примере показан метод <xref:System.Windows.Media.TextFormatting.TextSource.GetTextRun%2A>. Это хранилище текста возвращает объекты <xref:System.Windows.Media.TextFormatting.TextRun> в модуль форматирования текста для обработки.  
  
 [!code-csharp[TextFormatterExample#101](~/samples/snippets/csharp/VS_Snippets_Wpf/TextFormatterExample/CSharp/CustomTextSource.cs#101)]
 [!code-vb[TextFormatterExample#101](~/samples/snippets/visualbasic/VS_Snippets_Wpf/TextFormatterExample/VisualBasic/CustomTextSource.vb#101)]  
  
> [!NOTE]
> В этом примере хранилище текста предоставляет одни и те же свойства текста всему тексту. Расширенные хранилища текста могут потребоваться для реализации собственного управления диапазонами, чтобы позволить отдельным символам иметь разные свойства.  
  
<a name="section5"></a>   
## <a name="specifying-formatting-properties"></a>Задание свойств форматирования  
 объекты <xref:System.Windows.Media.TextFormatting.TextRun> форматируются с помощью свойств, предоставляемых хранилищем текста. Эти свойства бывают двух типов: <xref:System.Windows.Media.TextFormatting.TextParagraphProperties> и <xref:System.Windows.Media.TextFormatting.TextRunProperties>. <xref:System.Windows.Media.TextFormatting.TextParagraphProperties> обрабатывайте Инклюзивные свойства абзаца, такие как <xref:System.Windows.TextAlignment> и <xref:System.Windows.FlowDirection>. <xref:System.Windows.Media.TextFormatting.TextRunProperties> — это свойства, которые могут различаться для каждого текста, выполняемого в абзаце, например кисть переднего плана, <xref:System.Windows.Media.Typeface> и размер шрифта. Для реализации типов свойств пользовательского абзаца и пользовательского текста приложение должно создавать классы, производные от <xref:System.Windows.Media.TextFormatting.TextParagraphProperties> и <xref:System.Windows.Media.TextFormatting.TextRunProperties> соответственно.  
  
## <a name="see-also"></a>См. также

- [Оформление в WPF](typography-in-wpf.md)
- [Документы в WPF](documents-in-wpf.md)
