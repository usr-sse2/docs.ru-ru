---
title: Установка .NET Core на SLES 15 — диспетчер пакетов — .NET Core
description: Используйте диспетчер пакетов для установки пакета SDK для .NET Core и среды выполнения на SLES 15.
author: thraka
ms.author: adegeo
ms.date: 12/04/2019
ms.openlocfilehash: 5551ce8cffa92d4efb6bbe9db2a4887f4b26cd6e
ms.sourcegitcommit: a4f9b754059f0210e29ae0578363a27b9ba84b64
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/05/2019
ms.locfileid: "74998903"
---
# <a name="sles-15-package-manager---install-net-core"></a>Диспетчер пакетов SLES 15 — установка .NET Core

[!INCLUDE [package-manager-switcher](./includes/package-manager-switcher.md)]

Эта статья описывает, как использовать диспетчер пакетов для установки .NET Core на SLES 15. Если вы устанавливаете среду выполнения, мы рекомендуем установить [среду выполнения ASP.NET Core](#install-the-aspnet-core-runtime), так как она включает в себя среды выполнения .NET Core и ASP.NET Core.

## <a name="register-microsoft-key-and-feed"></a>Регистрация ключа Майкрософт и веб-канала

Перед установкой .NET нужно сделать следующее:

- зарегистрировать ключ Майкрософт;
- зарегистрировать репозиторий продуктов;
- установить необходимые зависимости.

Данную операцию достаточно выполнить один раз для каждого компьютера.

Откройте терминал и выполните приведенную ниже команду.

```bash
sudo rpm -Uvh https://packages.microsoft.com/config/sles/15/packages-microsoft-prod.rpm
```

## <a name="install-the-net-core-sdk"></a>Установка пакета SDK для .NET Core

Обновите продукты, доступные для установки, а затем установите пакет SDK для .NET Core. В терминале выполните приведенную ниже команду.

```bash
sudo zypper install dotnet-sdk-3.1
```

## <a name="install-the-aspnet-core-runtime"></a>Установка среды выполнения ASP.NET Core

Обновите продукты, доступные для установки, а затем установите среду выполнения ASP.NET. В терминале выполните приведенную ниже команду.

```bash
sudo zypper install aspnetcore-runtime-3.1
```

## <a name="install-the-net-core-runtime"></a>Установка среды выполнения .NET Core

Обновите продукты, доступные для установки, а затем установите среду выполнения .NET Core. В терминале выполните приведенную ниже команду.

```bash
sudo zypper install dotnet-runtime-3.1
```

## <a name="how-to-install-other-versions"></a>Установка других версий

[!INCLUDE [package-manager-switcher](./includes/package-manager-heading-hack-pkgname.md)]
