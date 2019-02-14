---
title: Создание приложения .NET Core с подключаемыми модулями
description: Узнайте, как создать приложение .NET Core, которое поддерживает подключаемые модули.
author: jkoritzinsky
ms.author: jekoritz
ms.date: 01/28/2019
ms.openlocfilehash: f2997c778b87ecd88c0fd2fadf491763066a4950
ms.sourcegitcommit: facefcacd7ae2e5645e463bc841df213c505ffd4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2019
ms.locfileid: "55739601"
---
# <a name="create-a-net-core-application-with-plugins"></a><span data-ttu-id="c6b8f-103">Создание приложения .NET Core с подключаемыми модулями</span><span class="sxs-lookup"><span data-stu-id="c6b8f-103">Create a .NET Core application with plugins</span></span>

<span data-ttu-id="c6b8f-104">В этом учебнике демонстрируется выполнение следующих действий:</span><span class="sxs-lookup"><span data-stu-id="c6b8f-104">This tutorial shows you how to:</span></span>

- <span data-ttu-id="c6b8f-105">Создание структуры проекта для поддержки подключаемых модулей.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-105">Structure a project to support plugins.</span></span>
- <span data-ttu-id="c6b8f-106">Создание пользовательского <xref:System.Runtime.Loader.AssemblyLoadContext> для загрузки каждого подключаемого модуля.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-106">Create a custom <xref:System.Runtime.Loader.AssemblyLoadContext> to load each plugin.</span></span>
- <span data-ttu-id="c6b8f-107">Использование типа `System.Runtime.Loader.AssemblyDependencyResolver`, чтобы разрешить зависимости для подключаемых модулей.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-107">Use the `System.Runtime.Loader.AssemblyDependencyResolver` type to allow plugins to have dependencies.</span></span>
- <span data-ttu-id="c6b8f-108">Создание подключаемых модулей, которые можно легко развернуть путем копирования артефактов сборки.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-108">Author plugins that can be easily deployed by just copying the build artifacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c6b8f-109">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="c6b8f-109">Prerequisites</span></span>

- <span data-ttu-id="c6b8f-110">Установите [пакет SDK для .NET Core 3.0, предварительная версия 2](https://www.microsoft.com/net/core) или более новой версии.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-110">Install the [.NET Core 3.0 Preview 2 SDK](https://www.microsoft.com/net/core) or a newer version.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="c6b8f-111">Создание приложения</span><span class="sxs-lookup"><span data-stu-id="c6b8f-111">Create the application</span></span>

<span data-ttu-id="c6b8f-112">Первым шагом является создание приложения:</span><span class="sxs-lookup"><span data-stu-id="c6b8f-112">The first step is to create the application:</span></span>

1. <span data-ttu-id="c6b8f-113">Создайте новую папку и в этой папке выполните `dotnet new console -o AppWithPlugin`.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-113">Create a new folder, and in that folder run `dotnet new console -o AppWithPlugin`.</span></span> 
2. <span data-ttu-id="c6b8f-114">Чтобы упросить создание проекта, создайте файл решения Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-114">To make building the project easier, create a Visual Studio solution file.</span></span> <span data-ttu-id="c6b8f-115">Запустите `dotnet new sln` в этой же папке.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-115">Run `dotnet new sln` in the same folder.</span></span> 
3. <span data-ttu-id="c6b8f-116">Запустите `dotnet sln add AppWithPlugin/AppWithPlugin.csproj`, чтобы добавить проект приложения в решение.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-116">Run `dotnet sln add AppWithPlugin/AppWithPlugin.csproj` to add the app project to the solution.</span></span>

<span data-ttu-id="c6b8f-117">Теперь можно заполнить каркас нашего приложения.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-117">Now we can fill in the skeleton of our application.</span></span> <span data-ttu-id="c6b8f-118">В файле *AppWithPlugin/Program.cs* замените существующий код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="c6b8f-118">Replace the code in the *AppWithPlugin/Program.cs* file with the following code:</span></span>

```csharp
using PluginBase;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Reflection;

namespace AppWithPlugin
{
    class Program
    {
        static void Main(string[] args)
        {
            try
            {
                if (args.Length == 1 && args[0] == "/d")
                {
                    Console.WriteLine("Waiting for any key...");
                    Console.ReadLine();
                }

                // Load commands from plugins.

                if (args.Length == 0)
                {
                    Console.WriteLine("Commands: ");
                    // Output the loaded commands.
                }
                else
                {
                    foreach (string commandName in args)
                    {
                        Console.WriteLine($"-- {commandName} --");

                        // Execute the command with the name passed as an argument.

                        Console.WriteLine();
                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex);
            }
        }
    }
}

```

## <a name="create-the-plugin-interfaces"></a><span data-ttu-id="c6b8f-119">Создание интерфейсов подключаемых модулей</span><span class="sxs-lookup"><span data-stu-id="c6b8f-119">Create the plugin interfaces</span></span>

<span data-ttu-id="c6b8f-120">Следующий шаг при создании приложения с подключаемыми модулями — определение интерфейса, который подключаемые модули должны реализовывать.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-120">The next step in building an app with plugins is defining the interface the plugins need to implement.</span></span> <span data-ttu-id="c6b8f-121">Мы рекомендуем создать библиотеку классов, содержащую типы, которые вы планируете использовать для обмена данными между приложением и подключаемыми модулями.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-121">We suggest that you make a class library that contains any types that you plan to use for communicating between your app and plugins.</span></span> <span data-ttu-id="c6b8f-122">Это разделение позволяет публиковать интерфейс подключаемого модуля как пакет без передачи всего приложения.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-122">This division allows you to publish your plugin interface as a package without having to ship your full application.</span></span>

<span data-ttu-id="c6b8f-123">В корневой папке проекта запустите `dotnet new classlib -o PluginBase`.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-123">In the root folder of the project, run `dotnet new classlib -o PluginBase`.</span></span> <span data-ttu-id="c6b8f-124">Также запустите `dotnet sln add PluginBase/PluginBase.csproj`, чтобы добавить проект в файл решения.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-124">Also, run `dotnet sln add PluginBase/PluginBase.csproj` to add the project to the solution file.</span></span> <span data-ttu-id="c6b8f-125">Удалите файл `PluginBase/Class1.cs` и создайте новый файл в папке `PluginBase` с именем `ICommand.cs` со следующим определением интерфейса:</span><span class="sxs-lookup"><span data-stu-id="c6b8f-125">Delete the `PluginBase/Class1.cs` file, and create a new file in the `PluginBase` folder named `ICommand.cs` with the following interface definition:</span></span>

[!code-csharp[the-plugin-interface](~/samples/core/extensions/AppWithPlugin/PluginBase/ICommand.cs)]

<span data-ttu-id="c6b8f-126">Этот интерфейс `ICommand` является интерфейсом, который будут реализовывать все подключаемые модули.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-126">This `ICommand` interface is the interface that all of the plugins will implement.</span></span>

<span data-ttu-id="c6b8f-127">Теперь, когда интерфейс `ICommand` определен, в проект приложения можно добавить другие элементы.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-127">Now that the `ICommand` interface is defined, the application project can be filled in a little more.</span></span> <span data-ttu-id="c6b8f-128">Добавьте ссылку из проекта `AppWithPlugin` на проект `PluginBase` с командой `dotnet add AppWithPlugin\AppWithPlugin.csproj reference PluginBase\PluginBase.csproj` из корневой папки.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-128">Add a reference from the `AppWithPlugin` project to the `PluginBase` project with the `dotnet add AppWithPlugin\AppWithPlugin.csproj reference PluginBase\PluginBase.csproj`  command from the root folder.</span></span>

<span data-ttu-id="c6b8f-129">Замените комментарий `// Load commands from plugins` на следующий фрагмент кода, чтобы он мог загрузить подключаемые модули из указанных путей к файлам:</span><span class="sxs-lookup"><span data-stu-id="c6b8f-129">Replace the `// Load commands from plugins` comment with the following code snippet to enable it to load plugins from given file paths:</span></span>

```csharp
string[] pluginPaths = new string[]
{
    // Paths to plugins to load.
};

IEnumerable<ICommand> commands = pluginPaths.SelectMany(pluginPath =>
{
    Assembly pluginAssembly = LoadPlugin(pluginPath);
    return CreateCommands(pluginAssembly);
}).ToList();
```



<span data-ttu-id="c6b8f-130">Затем замените комментарий `// Output the loaded commands` следующим фрагментом кода:</span><span class="sxs-lookup"><span data-stu-id="c6b8f-130">Then replace the `// Output the loaded commands` comment with the following code snippet:</span></span>

```csharp
foreach (ICommand command in commands)
{
    Console.WriteLine($"{command.Name}\t - {command.Description}");
}
```

<span data-ttu-id="c6b8f-131">Замените комментарий `// Execute the command with the name passed as an argument` следующим фрагментом кода:</span><span class="sxs-lookup"><span data-stu-id="c6b8f-131">Replace the `// Execute the command with the name passed as an argument` comment with the following snippet:</span></span>

```csharp
ICommand command = commands.FirstOrDefault(c => c.Name == commandName);
if (command == null)
{
    Console.WriteLine("No such command is known.");
    return;
}

command.Execute();
```

<span data-ttu-id="c6b8f-132">И, наконец, добавьте статические методы в класс `Program` с именем `LoadPlugin` и `CreateCommands`, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="c6b8f-132">And finally, add static methods to the `Program` class named `LoadPlugin` and `CreateCommands`, as shown here:</span></span>

```csharp
static Assembly LoadPlugin(string relativePath)
{
    throw new NotImplementedException();
}

static IEnumerable<ICommand> CreateCommands(Assembly assembly)
{
    int count = 0;

    foreach (Type type in assembly.GetTypes())
    {
        if (typeof(ICommand).IsAssignableFrom(type))
        {
            ICommand result = Activator.CreateInstance(type) as ICommand;
            if (result != null)
            {
                count++;
                yield return result;
            }
        }
    }

    if (count == 0)
    {
        string availableTypes = string.Join(",", assembly.GetTypes().Select(t => t.FullName));
        throw new ApplicationException(
            $"Can't find any type which implements ICommand in {assembly} from {assembly.Location}.\n" +
            $"Available types: {availableTypes}");
    }
}
```

## <a name="load-plugins"></a><span data-ttu-id="c6b8f-133">Загрузка подключаемых модулей</span><span class="sxs-lookup"><span data-stu-id="c6b8f-133">Load plugins</span></span>

<span data-ttu-id="c6b8f-134">Теперь приложение может правильно загрузить и создать экземпляры команд из загруженных сборок подключаемых модулей, но оно по-прежнему не может загрузить сборки подключаемых модулей.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-134">Now the application can correctly load and instantiate commands from loaded plugin assemblies, but it is still unable to load the plugin assemblies.</span></span> <span data-ttu-id="c6b8f-135">Создайте файл с именем *PluginLoadContext.cs* в папке *AppWithPlugin* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="c6b8f-135">Create a file named *PluginLoadContext.cs* in the *AppWithPlugin* folder with the following contents:</span></span>

[!code-csharp[loading-plugins](~/samples/core/extensions/AppWithPlugin/AppWithPlugin/PluginLoadContext.cs)]

<span data-ttu-id="c6b8f-136">Тип `PluginLoadContext` наследуется от класса <xref:System.Runtime.Loader.AssemblyLoadContext>.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-136">The `PluginLoadContext` type derives from <xref:System.Runtime.Loader.AssemblyLoadContext>.</span></span> <span data-ttu-id="c6b8f-137">Тип `AssemblyLoadContext` — это специальный тип в среде выполнения, который позволяет разработчикам изолировать загруженные сборки в разные группы, чтобы версии сборок не конфликтовали друг с другом.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-137">The `AssemblyLoadContext` type is a special type in the runtime that allows developers to isolate loaded assemblies into different groups to ensure that assembly versions do not conflict.</span></span> <span data-ttu-id="c6b8f-138">Кроме того, пользовательский `AssemblyLoadContext` может выбирать различные пути для загрузки сборок и переопределять поведение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-138">Additionally, a custom `AssemblyLoadContext` can choose different paths to load assemblies from and override the default behavior.</span></span> <span data-ttu-id="c6b8f-139">`PluginLoadContext` использует экземпляр типа `AssemblyDependencyResolver`, появившегося в .NET Core 3.0, для разрешения имен сборок в пути.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-139">The `PluginLoadContext` uses an instance of the `AssemblyDependencyResolver` type introduced in .NET Core 3.0 to resolve assembly names to paths.</span></span> <span data-ttu-id="c6b8f-140">Объект `AssemblyDependencyResolver` создается с путем к библиотеке классов .NET.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-140">The `AssemblyDependencyResolver` object is constructed with the path to a .NET class library.</span></span> <span data-ttu-id="c6b8f-141">Он разрешает сборки и собственные библиотеки в относительные пути на основе файла *deps.json* для библиотеки классов, путь которой был передан конструктору `AssemblyDependencyResolver`.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-141">It resolves assemblies and native libraries to their relative paths based on the *deps.json* file for the class library whose path was passed to the `AssemblyDependencyResolver` constructor.</span></span> <span data-ttu-id="c6b8f-142">Пользовательский `AssemblyLoadContext` позволяет подключаемым модулям иметь собственные зависимости, а `AssemblyDependencyResolver` упрощает правильную загрузку зависимостей.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-142">The custom `AssemblyLoadContext` enables plugins to have their own dependencies, and the `AssemblyDependencyResolver` makes it easy to correctly load the dependencies.</span></span>

<span data-ttu-id="c6b8f-143">Теперь, когда у проекта `AppWithPlugin` есть тип `PluginLoadContext`, дополните метод `Program.LoadPlugin` следующим текстом:</span><span class="sxs-lookup"><span data-stu-id="c6b8f-143">Now that the `AppWithPlugin` project has the `PluginLoadContext` type, update the `Program.LoadPlugin` method with the following body:</span></span>

```csharp
static Assembly LoadPlugin(string relativePath)
{
    // Navigate up to the solution root
    string root = Path.GetFullPath(Path.Combine(
        Path.GetDirectoryName(
            Path.GetDirectoryName(
                Path.GetDirectoryName(
                    Path.GetDirectoryName(
                        Path.GetDirectoryName(typeof(Program).Assembly.Location)))))));

    string pluginLocation = Path.GetFullPath(Path.Combine(root, relativePath.Replace('\\', Path.DirectorySeparatorChar)));
    Console.WriteLine($"Loading commands from: {pluginLocation}");
    PluginLoadContext loadContext = new PluginLoadContext(pluginLocation);
    return loadContext.LoadFromAssemblyName(new AssemblyName(Path.GetFileNameWithoutExtension(pluginLocation)));
}
```

<span data-ttu-id="c6b8f-144">Если использовать другой экземпляр `PluginLoadContext` для каждого подключаемого модуля, подключаемые модули смогут иметь разные или даже конфликтующие зависимости без проблем.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-144">By using a different `PluginLoadContext` instance for each plugin, the plugins can have different or even conflicting dependencies without issue.</span></span>

## <a name="create-a-simple-plugin-with-no-dependencies"></a><span data-ttu-id="c6b8f-145">Создание простого подключаемого модуля без зависимостей</span><span class="sxs-lookup"><span data-stu-id="c6b8f-145">Create a simple plugin with no dependencies</span></span>

<span data-ttu-id="c6b8f-146">В корневой папке сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="c6b8f-146">Back in the root folder, do the following:</span></span>

1. <span data-ttu-id="c6b8f-147">Запустите `dotnet new classlib -o HelloPlugin`, чтобы создать новый проект библиотеки классов с именем `HelloPlugin`.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-147">Run `dotnet new classlib -o HelloPlugin` to create a new class library project named `HelloPlugin`.</span></span>
2. <span data-ttu-id="c6b8f-148">Запустите `dotnet sln add HelloPlugin/HelloPlugin.csproj`, чтобы добавить проект в решение `AppWithPlugin`.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-148">Run `dotnet sln add HelloPlugin/HelloPlugin.csproj` to add the project to the `AppWithPlugin` solution.</span></span> 
3. <span data-ttu-id="c6b8f-149">Замените файл *HelloPlugin/Class1.cs* на файл с именем *HelloCommand.cs* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="c6b8f-149">Replace the *HelloPlugin/Class1.cs* file with a file named *HelloCommand.cs* with the following contents:</span></span>

[!code-csharp[the-hello-plugin](~/samples/core/extensions/AppWithPlugin/HelloPlugin/HelloCommand.cs)]

<span data-ttu-id="c6b8f-150">Теперь откройте файл *HelloPlugin.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-150">Now, open the *HelloPlugin.csproj* file.</span></span> <span data-ttu-id="c6b8f-151">Он должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="c6b8f-151">It should look similar to the following:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netcoreapp3.0</TargetFramework>
  </PropertyGroup>

</Project>

```

<span data-ttu-id="c6b8f-152">Между тегами `<Project>` добавьте следующие элементы:</span><span class="sxs-lookup"><span data-stu-id="c6b8f-152">In between the `<Project>` tags, add the following elements:</span></span>

```xml
<ItemGroup>
<ProjectReference Include="..\PluginBase\PluginBase.csproj">
    <Private>false</Private>
</ProjectReference>
</ItemGroup>
```

<span data-ttu-id="c6b8f-153">Элемент `<Private>false</Private>` очень важен.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-153">The `<Private>false</Private>` element is very important.</span></span> <span data-ttu-id="c6b8f-154">Он сообщает MSBuild, что не нужно копировать *PluginBase.dll* в выходной каталог для HelloPlugin.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-154">This tells MSBuild to not copy *PluginBase.dll* to the output directory for HelloPlugin.</span></span> <span data-ttu-id="c6b8f-155">Если сборка *PluginBase.dll* присутствует в выходном каталоге, `PluginLoadContext` найдет там сборку и загрузит ее при загрузке сборки *HelloPlugin.dll*.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-155">If the *PluginBase.dll* assembly is present in the output directory, `PluginLoadContext` will find the assembly there and load it when it loads the *HelloPlugin.dll* assembly.</span></span> <span data-ttu-id="c6b8f-156">На этом этапе тип `HelloPlugin.HelloCommand` реализует интерфейс `ICommand` из файла *PluginBase.dll* в выходном каталоге проекта `HelloPlugin`, а не интерфейс `ICommand`, загруженный в контекст загрузки по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-156">At this point, the `HelloPlugin.HelloCommand` type will implement the `ICommand` interface from the *PluginBase.dll* in the output directory of the `HelloPlugin` project, not the `ICommand` interface that is loaded into the default load context.</span></span> <span data-ttu-id="c6b8f-157">Так как среда выполнения считает эти два типа разными типами из разных сборок, метод `AppWithPlugin.Program.CreateCommands` не найдет команды.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-157">Since the runtime sees these two types as different types from different assemblies, the `AppWithPlugin.Program.CreateCommands` method will not find the commands.</span></span> <span data-ttu-id="c6b8f-158">Поэтому для ссылки на сборку, содержащую интерфейсы подключаемого модуля, требуются метаданные `<Private>false</Private>`.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-158">As a result, the `<Private>false</Private>` metadata is required for the reference to the assembly containing the plugin interfaces.</span></span>

<span data-ttu-id="c6b8f-159">Теперь, когда проект `HelloPlugin` завершен, мы должны обновить проект `AppWithPlugin`, чтобы знать, где находится подключаемый модуль `HelloPlugin`.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-159">Now that the `HelloPlugin` project is complete, we should update the `AppWithPlugin` project to know where the `HelloPlugin` plugin can be found.</span></span> <span data-ttu-id="c6b8f-160">После комментария `// Paths to plugins to load` добавьте `@"HelloPlugin\bin\Debug\netcoreapp3.0\HelloPlugin.dll"` как элемент массива `pluginPaths`.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-160">After the `// Paths to plugins to load` comment, add `@"HelloPlugin\bin\Debug\netcoreapp3.0\HelloPlugin.dll"` as an element of the `pluginPaths` array.</span></span>

## <a name="create-a-plugin-with-library-dependencies"></a><span data-ttu-id="c6b8f-161">Создание подключаемого модуля с зависимостями библиотек</span><span class="sxs-lookup"><span data-stu-id="c6b8f-161">Create a plugin with library dependencies</span></span>

<span data-ttu-id="c6b8f-162">Почти все подключаемые модули сложнее, чем простая программа "Hello World", и многие подключаемые модули имеют зависимости от других библиотек.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-162">Almost all plugins are more complex than a simple "Hello World", and many plugins have dependencies on other libraries.</span></span> <span data-ttu-id="c6b8f-163">Подключаемые модули `JsonPlugin` и `OldJson` в этом образце — это два примера подключаемых модулей с зависимостями пакета NuGet от `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-163">The `JsonPlugin` and `OldJson` plugin projects in the sample show two examples of plugins with NuGet package dependencies on `Newtonsoft.Json`.</span></span> <span data-ttu-id="c6b8f-164">В самих файлах проекта нет особых сведений для ссылок проекта, и (после добавления пути подключаемого модуля к массиву `pluginPaths`) подключаемые модули запускаются идеально, даже при запуске в том же выполнении приложения AppWithPlugin.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-164">The project files themselves do not have any special information for the project references, and (after adding the plugin paths to the `pluginPaths` array) the plugins run perfectly, even if run in the same execution of the AppWithPlugin app.</span></span> <span data-ttu-id="c6b8f-165">Но эти проекты не копируют сборки со ссылками в выходной каталог, поэтому сборки должны присутствовать на компьютере пользователя, чтобы функционировать.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-165">However, these projects do not copy the referenced assemblies to their output directory, so the assemblies need to be present on the user's machine for the plugins to work.</span></span> <span data-ttu-id="c6b8f-166">Существует два способа обойти эту проблему.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-166">There are two ways to work around this problem.</span></span> <span data-ttu-id="c6b8f-167">Первый вариант — использовать команду `dotnet publish` для публикации библиотеки классов.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-167">The first option is to use the `dotnet publish` command to publish the class library.</span></span> <span data-ttu-id="c6b8f-168">Кроме того, если вы хотите иметь возможность использовать выходные данные `dotnet build` для подключаемого модуля, можно добавить свойство `<CopyLocalLockFileAssemblies>true</CopyLocalLockFileAssemblies>` между тегами `<PropertyGroup>` в файле проекта подключаемого модуля.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-168">Alternatively, if you want to be able to use the output of `dotnet build` for your plugin, you can add the `<CopyLocalLockFileAssemblies>true</CopyLocalLockFileAssemblies>` property between the `<PropertyGroup>` tags in the plugin's project file.</span></span> <span data-ttu-id="c6b8f-169">См. проект подключаемого модуля `XcopyablePlugin` в качестве примера.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-169">See the `XcopyablePlugin` plugin project for an example.</span></span>

## <a name="other-plugin-examples-in-the-sample"></a><span data-ttu-id="c6b8f-170">Другие примеры подключаемого модуля в образце</span><span class="sxs-lookup"><span data-stu-id="c6b8f-170">Other plugin examples in the sample</span></span>

<span data-ttu-id="c6b8f-171">Объект `AssemblyDependencyResolver` также может разрешить собственные библиотеки, включенные в пакеты NuGet, и локализованные вспомогательные сборки.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-171">The `AssemblyDependencyResolver` object can also resolve native libraries included in NuGet packages as well as localized satellite assemblies.</span></span> <span data-ttu-id="c6b8f-172">`UVPlugin` и `FrenchPlugin` демонстрируют эти сценарии.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-172">The `UVPlugin` and `FrenchPlugin` demonstrate these scenarios respectively.</span></span>

## <a name="how-to-reference-a-plugin-interface-assembly-defined-in-a-nuget-package"></a><span data-ttu-id="c6b8f-173">Способ создания ссылок на сборку интерфейса подключаемого модуля, определенную в пакете NuGet</span><span class="sxs-lookup"><span data-stu-id="c6b8f-173">How to reference a plugin interface assembly defined in a NuGet package</span></span>

<span data-ttu-id="c6b8f-174">Предположим, у вас есть приложение A, и интерфейс его подключаемого модуля определен в пакете NuGet `A.PluginBase`.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-174">Let's say that there is an app A that has a plugin interface defined in the NuGet package named `A.PluginBase`.</span></span> <span data-ttu-id="c6b8f-175">Как правильно сослаться на пакет в проекте подключаемого модуля?</span><span class="sxs-lookup"><span data-stu-id="c6b8f-175">How do you reference the package correctly in your plugin project?</span></span> <span data-ttu-id="c6b8f-176">Для ссылок проекта использование метаданных `<Private>false</Private>` в элементе `ProjectReference` в файле проекта не позволило скопировать DLL-файл в выходные данные.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-176">For project references, using the `<Private>false</Private>` metadata on the `ProjectReference` element in the project file prevented the dll from being copied to the output.</span></span>

<span data-ttu-id="c6b8f-177">Чтобы правильно сослаться на пакет `A.PluginBase`, необходимо изменить элемент `<PackageReference>` в файле проекта на следующий код:</span><span class="sxs-lookup"><span data-stu-id="c6b8f-177">To correctly reference the `A.PluginBase` package, you want to change the `<PackageReference>` element in the project file to the following:</span></span>

```xml
<PackageReference Include="A.PluginBase" Version="1.0.0">
    <ExcludeAssets>runtime</ExcludeAssets>
</PackageReference>
```

<span data-ttu-id="c6b8f-178">Так сборки `A.PluginBase` не копируются в выходной каталог подключаемого модуля, и подключаемый модуль использует версию A для `A.PluginBase`.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-178">This prevents the `A.PluginBase` assemblies from being copied to the output directory of your plugin and ensures that your plugin will use A's version of `A.PluginBase`.</span></span>

## <a name="plugin-target-framework-recommendations"></a><span data-ttu-id="c6b8f-179">Рекомендации для целевой платформы подключаемого модуля</span><span class="sxs-lookup"><span data-stu-id="c6b8f-179">Plugin target framework recommendations</span></span>

<span data-ttu-id="c6b8f-180">Так как загрузка зависимости подключаемого модуля использует файл *deps.json*, с целевой платформой подключаемого модуля не все так просто.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-180">Because plugin dependency loading uses the *deps.json* file, there is a gotcha related to the plugin's target framework.</span></span> <span data-ttu-id="c6b8f-181">В частности, подключаемые модули должны быть нацелены на среду выполнения, такую как .NET Core 3.0, а не на версию .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-181">Specifically, your plugins should target a runtime, such as .NET Core 3.0, instead of a version of .NET Standard.</span></span> <span data-ttu-id="c6b8f-182">Файл `deps.json` создается в зависимости от целевой платформы проекта, и поскольку многие пакеты, совместимые с .NET Standard, ссылаются на сборки для .NET Standard и сборки реализации для конкретных сред выполнения, `deps.json` может неправильно распознавать сборки реализации или принимать версию сборки .NET Standard вместо ожидаемой версии .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c6b8f-182">The `deps.json` file is generated based on which framework the project targets, and since many .NET Standard-compatible packages ship reference assemblies for building against .NET Standard and implementation assemblies for specific runtimes, the `deps.json` may not correctly see implementation assemblies, or it may grab the .NET Standard version of an assembly instead of the .NET Core version you expect.</span></span>