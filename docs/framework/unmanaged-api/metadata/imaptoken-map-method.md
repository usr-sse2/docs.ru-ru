---
title: Метод IMapToken::Map
ms.date: 03/30/2017
api_name:
- IMapToken.Map
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMapToken::Map
helpviewer_keywords:
- IMapToken::Map method [.NET Framework metadata]
- Map method [.NET Framework metadata]
ms.assetid: b9b4bf2f-1098-43d6-9619-a99b4bda1940
topic_type:
- apiref
ms.openlocfilehash: fd362beb9f8fd7a1f2076eb6490a96c0358520e4
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/23/2019
ms.locfileid: "74432148"
---
# <a name="imaptokenmap-method"></a>Метод IMapToken::Map
Сопоставляет связь между сборками, используя подписи метаданных.  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp  
HRESULT Map (  
    [in]  mdToken tkImp,   
    [in]  mdToken tkEmit  
);  
```  
  
## <a name="parameters"></a>Параметры  
 `tkImp`  
 окне Токен метаданных, представляющий импортированный объект кода.  
  
 `tkEmit`  
 окне Токен метаданных, представляющий созданный объект кода.  
  
## <a name="remarks"></a>Примечания  
 Когда повторное отображение токена происходит во время слияния, исходный маркер ограничивается в импортированной (исходной) области метаданных, а новый маркер — в области исрожденных (целевых) метаданных.  
  
## <a name="requirements"></a>Требования  
 **Платформы:** см. раздел [Требования к системе](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Заголовок:** COR. h  
  
 **Библиотека:** Используется в качестве ресурса в MsCorEE. dll  
  
 **Версии платформы .NET Framework:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>См. также:

- [Интерфейс IMapToken](../../../../docs/framework/unmanaged-api/metadata/imaptoken-interface.md)
