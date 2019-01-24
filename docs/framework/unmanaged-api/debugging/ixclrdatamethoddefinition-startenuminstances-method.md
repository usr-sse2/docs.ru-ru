---
title: Метод IXCLRDataMethodDefinition::StartEnumInstances
ms.date: 01/16/2019
api.name:
- IXCLRDataMethodDefinition::StartEnumInstances Method
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- IXCLRDataMethodDefinition::StartEnumInstances Method
helpviewer.keywords:
- IXCLRDataMethodDefinition::StartEnumInstances Method [.NET Framework debugging]
topic_type:
- apiref
author: cshung
ms.author: andrewau
ms.openlocfilehash: c78af112e9239143c4854e34e9b6aa8e99344ec3
ms.sourcegitcommit: 6b308cf6d627d78ee36dbbae8972a310ac7fd6c8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2019
ms.locfileid: "54623655"
---
# <a name="ixclrdatamethoddefinitionstartenuminstances-method"></a><span data-ttu-id="15285-102">Метод IXCLRDataMethodDefinition::StartEnumInstances</span><span class="sxs-lookup"><span data-stu-id="15285-102">IXCLRDataMethodDefinition::StartEnumInstances Method</span></span>

<span data-ttu-id="15285-103">Предоставляет дескриптор для перечисления экземпляров метода для заданного `IXCLRDataAppDomain`.</span><span class="sxs-lookup"><span data-stu-id="15285-103">Provides a handle for the enumeration of method instances for a given `IXCLRDataAppDomain`.</span></span>

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="syntax"></a><span data-ttu-id="15285-104">Синтаксис</span><span class="sxs-lookup"><span data-stu-id="15285-104">Syntax</span></span>

```
HRESULT StartEnumInstances(
    [in] IXCLRDataAppDomain* appDomain,
    [out] CLRDATA_ENUM *handle
);
```

### <a name="parameters"></a><span data-ttu-id="15285-105">Параметры</span><span class="sxs-lookup"><span data-stu-id="15285-105">Parameters</span></span>

<span data-ttu-id="15285-106">`appDomain` [in] Домен приложения для перечисления.</span><span class="sxs-lookup"><span data-stu-id="15285-106">`appDomain` [in] An AppDomain for the enumeration.</span></span>

<span data-ttu-id="15285-107">`handle` [out] Дескриптор для перечисления экземпляров.</span><span class="sxs-lookup"><span data-stu-id="15285-107">`handle` [out] A handle for enumerating the instances.</span></span>

## <a name="remarks"></a><span data-ttu-id="15285-108">Примечания</span><span class="sxs-lookup"><span data-stu-id="15285-108">Remarks</span></span>

<span data-ttu-id="15285-109">Указанный метод является частью `IXCLRDataMethodDefinition` интерфейса и соответствует третьем слоте таблицы виртуальных методов.</span><span class="sxs-lookup"><span data-stu-id="15285-109">The provided method is part of the `IXCLRDataMethodDefinition` interface and corresponds to the third slot of the virtual method table.</span></span>

## <a name="requirements"></a><span data-ttu-id="15285-110">Требования</span><span class="sxs-lookup"><span data-stu-id="15285-110">Requirements</span></span>

<span data-ttu-id="15285-111">**Платформы:** См. раздел [Требования к системе](../../../../docs/framework/get-started/system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="15285-111">**Platforms:** See [System Requirements](../../../../docs/framework/get-started/system-requirements.md).</span></span>  
<span data-ttu-id="15285-112">**Заголовок.** Нет</span><span class="sxs-lookup"><span data-stu-id="15285-112">**Header:** None</span></span>  
<span data-ttu-id="15285-113">**Библиотека:** Нет</span><span class="sxs-lookup"><span data-stu-id="15285-113">**Library:** None</span></span>  
<span data-ttu-id="15285-114">**Версии платформы .NET Framework:** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]</span><span class="sxs-lookup"><span data-stu-id="15285-114">**.NET Framework Versions:** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]</span></span>  

## <a name="see-also"></a><span data-ttu-id="15285-115">См. также</span><span class="sxs-lookup"><span data-stu-id="15285-115">See also</span></span>

- [<span data-ttu-id="15285-116">Перечисление CLRDataSourceType</span><span class="sxs-lookup"><span data-stu-id="15285-116">CLRDataSourceType Enumeration</span></span>](../../../../docs/framework/unmanaged-api/debugging/clrdatasourcetype-enumeration.md)
- [<span data-ttu-id="15285-117">Отладка</span><span class="sxs-lookup"><span data-stu-id="15285-117">Debugging</span></span>](../../../../docs/framework/unmanaged-api/debugging/index.md)
- [<span data-ttu-id="15285-118">Интерфейс IXCLRDataMethodDefinition</span><span class="sxs-lookup"><span data-stu-id="15285-118">IXCLRDataMethodDefinition Interface</span></span>](../../../../docs/framework/unmanaged-api/debugging/ixclrdatamethoddefinition-interface.md)