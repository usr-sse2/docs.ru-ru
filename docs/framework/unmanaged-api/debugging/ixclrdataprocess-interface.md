---
title: Интерфейс IXCLRDataProcess
ms.date: 01/16/2019
api.name:
- IXCLRDataProcess Interface
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- IXCLRDataProcess Interface
helpviewer.keywords:
- IXCLRDataProcess Interface [.NET Framework debugging]
topic_type:
- apiref
author: cshung
ms.author: andrewau
ms.openlocfilehash: 8c98dd42b4ac5593d96478c2286f49ad216a4bc1
ms.sourcegitcommit: b56d59ad42140d277f2acbd003b74d655fdbc9f1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2019
ms.locfileid: "54416634"
---
# <a name="ixclrdataprocess-interface"></a><span data-ttu-id="24c05-102">Интерфейс IXCLRDataProcess</span><span class="sxs-lookup"><span data-stu-id="24c05-102">IXCLRDataProcess Interface</span></span>

<span data-ttu-id="24c05-103">Предоставляет методы для запроса на получение сведений о процессе.</span><span class="sxs-lookup"><span data-stu-id="24c05-103">Provides methods for querying information about a process.</span></span>

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="methods"></a><span data-ttu-id="24c05-104">Методы</span><span class="sxs-lookup"><span data-stu-id="24c05-104">Methods</span></span>

| <span data-ttu-id="24c05-105">Метод</span><span class="sxs-lookup"><span data-stu-id="24c05-105">Method</span></span>                                                                                                                                               | <span data-ttu-id="24c05-106">Описание:</span><span class="sxs-lookup"><span data-stu-id="24c05-106">Description</span></span>                                                                                     |
| ---------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| [<span data-ttu-id="24c05-107">GetAppDomainByUniqueId</span><span class="sxs-lookup"><span data-stu-id="24c05-107">GetAppDomainByUniqueId</span></span>](../../../../docs/framework/unmanaged-api/debugging/ixclrdataprocess-getappdomainbyuniqueid-method.md)                       | <span data-ttu-id="24c05-108">Получает `AppDomain` в процессе по ее уникальному идентификатору.</span><span class="sxs-lookup"><span data-stu-id="24c05-108">Gets an `AppDomain` in a process by its unique id.</span></span>                                              |
| [<span data-ttu-id="24c05-109">StartEnumModules</span><span class="sxs-lookup"><span data-stu-id="24c05-109">StartEnumModules</span></span>](../../../../docs/framework/unmanaged-api/debugging/ixclrdataprocess-startenummodules-method.md)                                   | <span data-ttu-id="24c05-110">Предоставляет маркер, чтобы перечислить модули, процесса.</span><span class="sxs-lookup"><span data-stu-id="24c05-110">Provides a handle to enumerate the modules of a process.</span></span>                                        |
| [<span data-ttu-id="24c05-111">EnumModule</span><span class="sxs-lookup"><span data-stu-id="24c05-111">EnumModule</span></span>](../../../../docs/framework/unmanaged-api/debugging/ixclrdataprocess-enummodule-method.md)                                               | <span data-ttu-id="24c05-112">Перечисляет модули этого процесса.</span><span class="sxs-lookup"><span data-stu-id="24c05-112">Enumerates the modules of this process.</span></span>                                                         |
| [<span data-ttu-id="24c05-113">EndEnumModules</span><span class="sxs-lookup"><span data-stu-id="24c05-113">EndEnumModules</span></span>](../../../../docs/framework/unmanaged-api/debugging/ixclrdataprocess-endenummodules-method.md)                                       | <span data-ttu-id="24c05-114">Освобождает ресурсы, используемые внутренней итераторы, используемые при перечислении модуля.</span><span class="sxs-lookup"><span data-stu-id="24c05-114">Releases the resources used by internal iterators used during module enumeration.</span></span>               |
| [<span data-ttu-id="24c05-115">StartEnumMethodInstancesByAddress</span><span class="sxs-lookup"><span data-stu-id="24c05-115">StartEnumMethodInstancesByAddress</span></span>](../../../../docs/framework/unmanaged-api/debugging/ixclrdataprocess-startenummethodinstancesbyaddress-method.md) | <span data-ttu-id="24c05-116">Предоставляет дескриптор для перечисления экземпляров метод `AppDomain` начиная с данного адреса.</span><span class="sxs-lookup"><span data-stu-id="24c05-116">Provides a handle to enumerate the method instances of `AppDomain` starting at a given address.</span></span> |
| [<span data-ttu-id="24c05-117">EnumMethodInstanceByAddress</span><span class="sxs-lookup"><span data-stu-id="24c05-117">EnumMethodInstanceByAddress</span></span>](../../../../docs/framework/unmanaged-api/debugging/ixclrdataprocess-enummethodinstancebyaddress-method.md)             | <span data-ttu-id="24c05-118">Перечисление экземпляров метод класса этот процесс, начиная с смещение адреса.</span><span class="sxs-lookup"><span data-stu-id="24c05-118">Enumerates the method instances of this process starting at an address offset.</span></span>                  |
| [<span data-ttu-id="24c05-119">EndEnumMethodInstancesByAddress</span><span class="sxs-lookup"><span data-stu-id="24c05-119">EndEnumMethodInstancesByAddress</span></span>](../../../../docs/framework/unmanaged-api/debugging/ixclrdataprocess-endenummethodinstancesbyaddress-method.md)     | <span data-ttu-id="24c05-120">Освобождает ресурсы, используемые внутренней итераторы, используется в процессе экземпляра перечисления.</span><span class="sxs-lookup"><span data-stu-id="24c05-120">Releases the resources used by internal iterators used during instance enumeration.</span></span>             |

## <a name="remarks"></a><span data-ttu-id="24c05-121">Примечания</span><span class="sxs-lookup"><span data-stu-id="24c05-121">Remarks</span></span>

<span data-ttu-id="24c05-122">Этот интерфейс находится внутри среды выполнения и не предоставляется через любой заголовков или библиотек.</span><span class="sxs-lookup"><span data-stu-id="24c05-122">This interface lives inside the runtime and is not exposed through any headers or library files.</span></span> <span data-ttu-id="24c05-123">Однако это COM-интерфейс, наследуемый от `IUnknown` с идентификатором GUID `5c552ab6-fc09-4cb3-8e36-22fa03c798b7` , можно получить с помощью обычных механизмов COM.</span><span class="sxs-lookup"><span data-stu-id="24c05-123">However, it's a COM interface that derives from `IUnknown` with GUID `5c552ab6-fc09-4cb3-8e36-22fa03c798b7` that can be obtained through the usual COM mechanisms.</span></span>

## <a name="requirements"></a><span data-ttu-id="24c05-124">Требования</span><span class="sxs-lookup"><span data-stu-id="24c05-124">Requirements</span></span>

<span data-ttu-id="24c05-125">**Платформы:** См. раздел [Требования к системе](../../../../docs/framework/get-started/system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="24c05-125">**Platforms:** See [System Requirements](../../../../docs/framework/get-started/system-requirements.md).</span></span>   
<span data-ttu-id="24c05-126">**Заголовок.** Нет</span><span class="sxs-lookup"><span data-stu-id="24c05-126">**Header:** None</span></span>  
<span data-ttu-id="24c05-127">**Библиотека:** Нет</span><span class="sxs-lookup"><span data-stu-id="24c05-127">**Library:** None</span></span>  
<span data-ttu-id="24c05-128">**Версии платформы .NET Framework:** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]</span><span class="sxs-lookup"><span data-stu-id="24c05-128">**.NET Framework Versions:** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]</span></span>  

## <a name="see-also"></a><span data-ttu-id="24c05-129">См. также</span><span class="sxs-lookup"><span data-stu-id="24c05-129">See Also</span></span>

- [<span data-ttu-id="24c05-130">Отладка</span><span class="sxs-lookup"><span data-stu-id="24c05-130">Debugging</span></span>](../../../../docs/framework/unmanaged-api/debugging/index.md)
- [<span data-ttu-id="24c05-131">Интерфейсы отладки</span><span class="sxs-lookup"><span data-stu-id="24c05-131">Debugging Interfaces</span></span>](../../../../docs/framework/unmanaged-api/debugging/debugging-interfaces.md)