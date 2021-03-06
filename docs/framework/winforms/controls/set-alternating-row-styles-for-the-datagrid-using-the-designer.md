---
title: Практическое руководство. Установка стилей для чередующихся строк в элементе управления DataGridView формы Windows Forms с помощью конструктора
ms.date: 03/30/2017
helpviewer_keywords:
- ledger-like formats
- DataGridView control [Windows Forms], row style alternation
- Windows Forms, rows
- rows [Windows Forms], alternating
- data [Windows Forms], displaying
ms.assetid: 02373442-bf94-4470-9f8a-e44c4a9d5b88
ms.openlocfilehash: a24b4e6af98d2637ecae2352aaa881adcd1c1a45
ms.sourcegitcommit: cdf67135a98a5a51913dacddb58e004a3c867802
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/21/2019
ms.locfileid: "69666836"
---
# <a name="how-to-set-alternating-row-styles-for-the-windows-forms-datagridview-control-using-the-designer"></a>Практическое руководство. Установка стилей для чередующихся строк в элементе управления DataGridView формы Windows Forms с помощью конструктора

Табличные данные часто представляются в формате, похожем на формат книги, где чередующиеся строки имеют разные цвета фона. Такой формат позволяет проще определять, какие ячейки находятся в какой строке, что особенно удобно в широких таблицах со множеством столбцов.

С помощью элемента управления <xref:System.Windows.Forms.DataGridView> можно указать полные сведения о стиле для чередующихся строк. Для различения чередующихся строк можно использовать характеристики стиля, такие как цвет переднего плана и шрифт, а также цвет фона. Дополнительные сведения см. [в разделе Стили ячеек в элементе управления Windows Forms DataGridView](cell-styles-in-the-windows-forms-datagridview-control.md).

Для следующей процедуры требуется проект **приложения Windows** с формой, содержащей <xref:System.Windows.Forms.DataGridView> элемент управления. Сведения о настройке такого проекта см. в разделе [как Создайте проект](/visualstudio/ide/step-1-create-a-windows-forms-application-project) приложения Windows Forms и [выполните следующие действия. Добавьте элементы управления в](how-to-add-controls-to-windows-forms.md)Windows Forms.

### <a name="define-styles-for-alternating-rows"></a>Определение стилей для чередующихся строк

1. <xref:System.Windows.Forms.DataGridView> Выберите элемент управления в конструкторе.

2. В окне **Свойства** нажмите кнопку с многоточием (![кнопку с многоточием (...) в окно свойств Visual Studio.](./media/visual-studio-ellipsis-button.png)) рядом <xref:System.Windows.Forms.DataGridView.AlternatingRowsDefaultCellStyle%2A> со свойством.

3. В диалоговом окне **Построитель** стилей определите стиль, задав свойства, и используйте панель **предварительного просмотра** для подтверждения выбора. Указанные стили используются для каждой другой строки, отображаемой в элементе управления, начиная со второго.

4. Чтобы определить стили для оставшихся строк, повторите шаги 2 и 3 с помощью <xref:System.Windows.Forms.DataGridView.RowsDefaultCellStyle%2A> свойства.

    > [!NOTE]
    > Ячейки отображаются с использованием стилей, унаследованных от нескольких свойств. Дополнительные сведения о наследовании стилей см. [в разделе Стили ячеек в элементе управления Windows Forms DataGridView](cell-styles-in-the-windows-forms-datagridview-control.md).

## <a name="see-also"></a>См. также

- <xref:System.Windows.Forms.DataGridView>
- [Стили ячеек элемента управления DataGridView в Windows Forms](cell-styles-in-the-windows-forms-datagridview-control.md)
- [Базовое форматирование и оформление элемента управления DataGridView в Windows Forms](basic-formatting-and-styling-in-the-windows-forms-datagridview-control.md)
- [Использование конструктора с элементом управления DataGridView Windows Forms](using-the-designer-with-the-windows-forms-datagridview-control.md)
- [Практическое руководство. Создание проекта приложения Windows Forms](/visualstudio/ide/step-1-create-a-windows-forms-application-project).
- [Практическое руководство. Добавление элементов управления в Windows Forms](how-to-add-controls-to-windows-forms.md)
