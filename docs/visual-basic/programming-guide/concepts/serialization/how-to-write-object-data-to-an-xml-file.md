---
title: Практическое руководство. Запись данных объекта в XML-файл
ms.date: 07/20/2015
ms.assetid: f7966480-5ed2-43ac-9894-33427436de2a
ms.openlocfilehash: b2181a74c83782cf4737b2a94fc5fb08fee28a10
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345452"
---
# <a name="how-to-write-object-data-to-an-xml-file-visual-basic"></a>Руководство. запись данных объекта в XML-файл (Visual Basic)
Показывает, как записать объект из класса в XML-файл с помощью класса <xref:System.Xml.Serialization.XmlSerializer>.  
  
## <a name="example"></a>Пример  
  
```vb  
Public Module XMLWrite  
  
    Sub Main()  
        WriteXML()  
    End Sub  
  
    Public Class Book  
        Public Title As String  
    End Class  
  
    Public Sub WriteXML()  
        Dim overview As New Book  
        overview.Title = "Serialization Overview"  
        Dim writer As New System.Xml.Serialization.XmlSerializer(GetType(Book))  
        Dim file As New System.IO.StreamWriter(  
            "c:\temp\SerializationOverview.xml")  
        writer.Serialize(file, overview)  
        file.Close()  
    End Sub  
End Module  
```  
  
## <a name="compiling-the-code"></a>Компиляция кода  
 У класса должен быть открытый конструктор без параметров.  
  
## <a name="robust-programming"></a>Отказоустойчивость  
 При следующих условиях возможно возникновение исключения:  
  
- В сериализуемом классе нет открытого конструктора без параметров.  
  
- Файл существует и является файлом только для чтения (<xref:System.IO.IOException>).  
  
- Слишком длинный путь (<xref:System.IO.PathTooLongException>).  
  
- Диск заполнен (<xref:System.IO.IOException>).  
  
## <a name="net-framework-security"></a>Безопасность платформы .NET Framework  
 В этом примере создается файл (если файл отсутствует). Если приложению требуется создать файл, оно должно иметь доступ к каталогу для создания файлов (`Create`). Если файл уже существует, то приложению достаточно иметь лишь доступ для записи файлов (`Write`), т. е. меньшие привилегии. Безопаснее создавать файл во время развертывания, если это возможно, а также предоставлять доступ `Read` к отдельному файлу вместо доступа `Create` к папке.  
  
## <a name="see-also"></a>См. также

- <xref:System.IO.StreamWriter>
- [How to: Read Object Data from an XML File (Visual Basic)](../../../../visual-basic/programming-guide/concepts/serialization/how-to-read-object-data-from-an-xml-file.md) (Практическое руководство. Чтение данных объекта из XML-файла (Visual Basic))
- [Сериализация (Visual Basic)](../../../../visual-basic/programming-guide/concepts/serialization/index.md)
