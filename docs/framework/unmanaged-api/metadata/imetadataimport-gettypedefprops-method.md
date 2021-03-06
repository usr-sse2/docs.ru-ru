---
title: Метод IMetaDataImport::GetTypeDefProps
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetTypeDefProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetTypeDefProps
helpviewer_keywords:
- GetTypeDefProps method [.NET Framework metadata]
- IMetaDataImport::GetTypeDefProps method [.NET Framework metadata]
ms.assetid: 00061a25-ba05-47a7-b984-fd916b06b149
topic_type:
- apiref
ms.openlocfilehash: c9ac624e17223def206e86fd92ee4fd2de7f6082
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/23/2019
ms.locfileid: "74436746"
---
# <a name="imetadataimportgettypedefprops-method"></a>Метод IMetaDataImport::GetTypeDefProps
Возвращает сведения о метаданных для <xref:System.Type>, представленных указанным маркером TypeDef.  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp  
HRESULT GetTypeDefProps (  
   [in]  mdTypeDef   td,  
   [out] LPWSTR      szTypeDef,  
   [in]  ULONG       cchTypeDef,  
   [out] ULONG       *pchTypeDef,  
   [out] DWORD       *pdwTypeDefFlags,  
   [out] mdToken     *ptkExtends  
);  
```  
  
## <a name="parameters"></a>Параметры  
 `td`  
 окне Токен TypeDef, представляющий тип, для которого возвращаются метаданные.  
  
 `szTypeDef`  
 заполняет Буфер, содержащий имя типа.  
  
 `cchTypeDef`  
 окне Размер в расширенных символах `szTypeDef`.  
  
 `pchTypeDef`  
 заполняет Число расширенных символов, возвращаемых в `szTypeDef`.  
  
 `pdwTypeDefFlags`  
 заполняет Указатель на любые флаги, изменяющие определение типа. Это значение является битовой маской из перечисления [кортипеаттр](../../../../docs/framework/unmanaged-api/metadata/cortypeattr-enumeration.md) .  
  
 `ptkExtends`  
 заполняет Токен метаданных TypeDef или TypeRef, представляющий базовый тип запрошенного типа.  
  
## <a name="requirements"></a>Требования  
 **Платформы:** см. раздел [Требования к системе](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Заголовок:** COR. h  
  
 **Библиотека:** Включается в качестве ресурса в библиотеку MsCorEE. dll  
  
 **Версии платформы .NET Framework:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>См. также

- [Интерфейс IMetaDataImport](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-interface.md)
- [Интерфейс IMetaDataImport2](../../../../docs/framework/unmanaged-api/metadata/imetadataimport2-interface.md)
