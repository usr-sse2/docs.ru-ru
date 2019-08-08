---
title: Архитектурные принципы
description: Разработка современных веб-приложений с помощью ASP.NET Core и Azure | Архитектурные принципы
author: ardalis
ms.author: wiwagn
ms.date: 02/16/2019
ms.openlocfilehash: 74ff7196ce17807b98a975687a524041f15a7f5b
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68675581"
---
# <a name="architectural-principles"></a><span data-ttu-id="10a34-103">Архитектурные принципы</span><span class="sxs-lookup"><span data-stu-id="10a34-103">Architectural principles</span></span>

> <span data-ttu-id="10a34-104">"Если бы строители возводили здания так же, как программисты пишут программы, первый же дятел уничтожил бы всю цивилизацию".</span><span class="sxs-lookup"><span data-stu-id="10a34-104">"If builders built buildings the way programmers wrote programs, then the first woodpecker that came along would destroy civilization."</span></span>  
> <span data-ttu-id="10a34-105">_\- Джеральд Вайнберг (Gerald Weinberg)_</span><span class="sxs-lookup"><span data-stu-id="10a34-105">_\- Gerald Weinberg_</span></span>

<span data-ttu-id="10a34-106">При разработке архитектуры и проектировании программных решений важно учитывать удобство поддержки.</span><span class="sxs-lookup"><span data-stu-id="10a34-106">You should architect and design software solutions with maintainability in mind.</span></span> <span data-ttu-id="10a34-107">В этом разделе описываются принципы принятия решений о выборе архитектуры, которые помогут вам строить прозрачные и удобные в поддержке приложения.</span><span class="sxs-lookup"><span data-stu-id="10a34-107">The principles outlined in this section can help guide you toward architectural decisions that will result in clean, maintainable applications.</span></span> <span data-ttu-id="10a34-108">В общем случае эти принципы рекомендуют создавать приложения, состоящие из слабо связанных с другими частями приложения компонентов, взаимодействие между которыми осуществляется с использованием явных интерфейсов или систем обмена сообщениями.</span><span class="sxs-lookup"><span data-stu-id="10a34-108">Generally, these principles will guide you toward building applications out of discrete components that are not tightly coupled to other parts of your application, but rather communicate through explicit interfaces or messaging systems.</span></span>

## <a name="common-design-principles"></a><span data-ttu-id="10a34-109">Общие принципы проектирования</span><span class="sxs-lookup"><span data-stu-id="10a34-109">Common design principles</span></span>

### <a name="separation-of-concerns"></a><span data-ttu-id="10a34-110">Разделение задач</span><span class="sxs-lookup"><span data-stu-id="10a34-110">Separation of concerns</span></span>

<span data-ttu-id="10a34-111">Основополагающим принципом разработки является **разделение задач**.</span><span class="sxs-lookup"><span data-stu-id="10a34-111">A guiding principle when developing is **Separation of Concerns**.</span></span> <span data-ttu-id="10a34-112">Этот принцип подразумевает разделение программного обеспечения на компоненты в соответствии с выполняемыми ими функциями.</span><span class="sxs-lookup"><span data-stu-id="10a34-112">This principle asserts that software should be separated based on the kinds of work it performs.</span></span> <span data-ttu-id="10a34-113">Рассмотрим пример приложения, в котором используется логика выбора отображаемых пользователю элементов, а также осуществляется форматирование этих элементов для максимально эффективного показа.</span><span class="sxs-lookup"><span data-stu-id="10a34-113">For instance, consider an application that includes logic for identifying noteworthy items to display to the user, and which formats such items in a particular way to make them more noticeable.</span></span> <span data-ttu-id="10a34-114">Выбор форматируемых элементов должен быть отделен от самих функций форматирования, поскольку это разные задачи, не имеющие тесной связи.</span><span class="sxs-lookup"><span data-stu-id="10a34-114">The behavior responsible for choosing which items to format should be kept separate from the behavior responsible for formatting the items, since these are separate concerns that are only coincidentally related to one another.</span></span>

<span data-ttu-id="10a34-115">С точки зрения архитектуры для соблюдения этого принципа при проектировании следует отделять бизнес-логику от инфраструктуры и функций пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="10a34-115">Architecturally, applications can be logically built to follow this principle by separating core business behavior from infrastructure and user interface logic.</span></span> <span data-ttu-id="10a34-116">В идеальном случае бизнес-правила и логика должны размещаться в отдельном проекте, который не должен зависеть от других проектов в приложении.</span><span class="sxs-lookup"><span data-stu-id="10a34-116">Ideally, business rules and logic should reside in a separate project, which should not depend on other projects in the application.</span></span> <span data-ttu-id="10a34-117">Благодаря этому бизнес-модель легко тестировать и развивать, не затрагивая при этом реализацию возможностей более низкого уровня.</span><span class="sxs-lookup"><span data-stu-id="10a34-117">This helps ensure that the business model is easy to test and can evolve without being tightly coupled to low-level implementation details.</span></span> <span data-ttu-id="10a34-118">Разделение задач является одним из основных вопросов, который следует учитывать при использовании слоев архитектур приложения.</span><span class="sxs-lookup"><span data-stu-id="10a34-118">Separation of concerns is a key consideration behind the use of layers in application architectures.</span></span>

### <a name="encapsulation"></a><span data-ttu-id="10a34-119">Инкапсуляция</span><span class="sxs-lookup"><span data-stu-id="10a34-119">Encapsulation</span></span>

<span data-ttu-id="10a34-120">**Инкапсуляция** отдельных частей приложения позволяет изолировать их друг от друга.</span><span class="sxs-lookup"><span data-stu-id="10a34-120">Different parts of an application should use **encapsulation** to insulate them from other parts of the application.</span></span> <span data-ttu-id="10a34-121">Корректировка внутренней реализации компонентов и слоев приложения должна быть возможна без влияния на участников совместной работы при условии, что при этом не нарушаются внешние контракты.</span><span class="sxs-lookup"><span data-stu-id="10a34-121">Application components and layers should be able to adjust their internal implementation without breaking their collaborators as long as external contracts are not violated.</span></span> <span data-ttu-id="10a34-122">Правильно реализованная инкапсуляция позволяет получить слабо связанную модульную структуру приложения, поскольку объекты и пакеты можно с легкостью заменять альтернативными реализациями, если при этом сохраняется один и тот же интерфейс.</span><span class="sxs-lookup"><span data-stu-id="10a34-122">Proper use of encapsulation helps achieve loose coupling and modularity in application designs, since objects and packages can be replaced with alternative implementations so long as the same interface is maintained.</span></span>

<span data-ttu-id="10a34-123">Инкапсуляция классов реализуется посредством ограничения доступа извне к внутреннему состоянию класса.</span><span class="sxs-lookup"><span data-stu-id="10a34-123">In classes, encapsulation is achieved by limiting outside access to the class's internal state.</span></span> <span data-ttu-id="10a34-124">Если внешнему субъекту требуется изменить состояние объекта, он должен использовать четко определенную функцию (или метод задания свойств) вместо того, чтобы напрямую получать доступ к закрытому состоянию объекта.</span><span class="sxs-lookup"><span data-stu-id="10a34-124">If an outside actor wants to manipulate the state of the object, it should do so through a well-defined function (or property setter), rather than having direct access to the private state of the object.</span></span> <span data-ttu-id="10a34-125">Аналогичным образом, компоненты приложений и сами приложения должны предоставлять четко определенные интерфейсы, которые будут использоваться участниками совместной работы вместо того, чтобы допускать изменение их состояния напрямую.</span><span class="sxs-lookup"><span data-stu-id="10a34-125">Likewise, application components and applications themselves should expose well-defined interfaces for their collaborators to use, rather than allowing their state to be modified directly.</span></span> <span data-ttu-id="10a34-126">Это позволяет свободно модернизировать внутреннюю структуру приложения со временем, не беспокоясь о том, что это может нарушить функционирование других участников совместной работы (при условии, что соблюдаются условия открытых контрактов).</span><span class="sxs-lookup"><span data-stu-id="10a34-126">This frees the application's internal design to evolve over time without worrying that doing so will break collaborators, so long as the public contracts are maintained.</span></span>

### <a name="dependency-inversion"></a><span data-ttu-id="10a34-127">Инверсия зависимостей</span><span class="sxs-lookup"><span data-stu-id="10a34-127">Dependency inversion</span></span>

<span data-ttu-id="10a34-128">Зависимость в приложении должна быть направлена в сторону абстракции, а не на детали реализации.</span><span class="sxs-lookup"><span data-stu-id="10a34-128">The direction of dependency within the application should be in the direction of abstraction, not implementation details.</span></span> <span data-ttu-id="10a34-129">При написании большинства приложений направление зависимостей времени компиляции задается в сторону времени выполнения.</span><span class="sxs-lookup"><span data-stu-id="10a34-129">Most applications are written such that compile-time dependency flows in the direction of runtime execution.</span></span> <span data-ttu-id="10a34-130">Таким образом формируется схема прямых зависимостей.</span><span class="sxs-lookup"><span data-stu-id="10a34-130">This produces a direct dependency graph.</span></span> <span data-ttu-id="10a34-131">Это значит, что если модуль A вызывает функцию в модуле B, которая вызывает функцию в модуле C, то во время компиляции A будет зависеть от B, который, в свою очередь, зависит от C, как показано на рис. 4-1.</span><span class="sxs-lookup"><span data-stu-id="10a34-131">That is, if module A calls a function in module B, which calls a function in module C, then at compile time A will depend on B which will depend on C, as shown in Figure 4-1.</span></span>

![](./media/image4-1.png)

<span data-ttu-id="10a34-132">**Рис. 4-1.**</span><span class="sxs-lookup"><span data-stu-id="10a34-132">**Figure 4-1.**</span></span> <span data-ttu-id="10a34-133">Схема прямых зависимостей.</span><span class="sxs-lookup"><span data-stu-id="10a34-133">Direct dependency graph.</span></span>

<span data-ttu-id="10a34-134">Применение принципа инверсии зависимостей позволяет модулю A вызывать методы абстракции, которую реализует модуль B. Это значит, что модуль A может вызывать модуль B во время выполнения, однако B будет зависеть от интерфейса, управляемого модулем A, во время компиляции (таким образом, типовая зависимость времени компиляции *инвертируется*).</span><span class="sxs-lookup"><span data-stu-id="10a34-134">Applying the dependency inversion principle allows A to call methods on an abstraction that B implements, making it possible for A to call B at runtime, but for B to depend on an interface controlled by A at compile time (thus, *inverting* the typical compile-time dependency).</span></span> <span data-ttu-id="10a34-135">Во время выполнения поток выполнения программы остается неизменным, однако при этом легко могут быть подключены новые реализации интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="10a34-135">At run time, the flow of program execution remains unchanged, but the introduction of interfaces means that different implementations of these interfaces can easily be plugged in.</span></span>

![](./media/image4-2.png)

<span data-ttu-id="10a34-136">**Рис. 4-2.**</span><span class="sxs-lookup"><span data-stu-id="10a34-136">**Figure 4-2.**</span></span> <span data-ttu-id="10a34-137">Схема инвертированных зависимостей.</span><span class="sxs-lookup"><span data-stu-id="10a34-137">Inverted dependency graph.</span></span>

<span data-ttu-id="10a34-138">**Инверсия зависимостей** является важной частью процесса создания слабо связанных приложений, поскольку детали реализации могут описывать зависимости и реализовывать абстракции более высокого уровня, а не компоненты того же уровня.</span><span class="sxs-lookup"><span data-stu-id="10a34-138">**Dependency inversion** is a key part of building loosely-coupled applications, since implementation details can be written to depend on and implement higher level abstractions, rather than the other way around.</span></span> <span data-ttu-id="10a34-139">В результате получаются приложения с более высоким уровнем тестируемости, модульности и удобства в обслуживании.</span><span class="sxs-lookup"><span data-stu-id="10a34-139">The resulting applications are more testable, modular, and maintainable as a result.</span></span> <span data-ttu-id="10a34-140">Практика *внедрения зависимостей* базируется на соблюдении принципа инверсии зависимостей.</span><span class="sxs-lookup"><span data-stu-id="10a34-140">The practice of *dependency injection* is made possible by following the dependency inversion principle.</span></span>

### <a name="explicit-dependencies"></a><span data-ttu-id="10a34-141">Явные зависимости</span><span class="sxs-lookup"><span data-stu-id="10a34-141">Explicit dependencies</span></span>

<span data-ttu-id="10a34-142">**Методы и классы должны явно требовать наличия всех совместно работающих объектов, которые необходимы для их корректного функционирования.**</span><span class="sxs-lookup"><span data-stu-id="10a34-142">**Methods and classes should explicitly require any collaborating objects they need in order to function correctly.**</span></span> <span data-ttu-id="10a34-143">Благодаря конструкторам классов классы могут идентифицировать объекты, которые им необходимы для сохранения корректного состояния и правильного функционирования.</span><span class="sxs-lookup"><span data-stu-id="10a34-143">Class constructors provide an opportunity for classes to identify the things they need in order to be in a valid state and to function properly.</span></span> <span data-ttu-id="10a34-144">Если определены классы, которые могут конструироваться и вызываться, однако которые корректно работают только при наличии определенных глобальных или инфраструктурных компонентов, поведение таких классов будет *не до конца прозрачным* для клиентов.</span><span class="sxs-lookup"><span data-stu-id="10a34-144">If you define classes that can be constructed and called, but which will only function properly if certain global or infrastructure components are in place, these classes are being *dishonest* with their clients.</span></span> <span data-ttu-id="10a34-145">Контракт конструктора указывает клиенту на то, что ему требуются только заданные компоненты (если класс использует конструктор без параметров, не требуется ничего), однако во время выполнения выясняется, что объекту фактически требуется что-то еще.</span><span class="sxs-lookup"><span data-stu-id="10a34-145">The constructor contract is telling the client that it only needs the things specified (possibly nothing if the class is just using a parameterless constructor), but then at runtime it turns out the object really did need something else.</span></span>

<span data-ttu-id="10a34-146">При соблюдении принципа явных зависимостей ваши классы и методы будут прозрачны для клиентов, указывая все необходимые им для работы объекты.</span><span class="sxs-lookup"><span data-stu-id="10a34-146">By following the explicit dependencies principle, your classes and methods are being honest with their clients about what they need in order to function.</span></span> <span data-ttu-id="10a34-147">Благодаря этому код и контракты программирования становятся более понятными, поскольку пользователи будут уверены, что если предоставлены все требуемые в параметрах метода или конструктора объекты, во время выполнения такой метод или конструктор будет работать корректно.</span><span class="sxs-lookup"><span data-stu-id="10a34-147">This makes your code more self-documenting and your coding contracts more user-friendly, since users will come to trust that as long as they provide what's required in the form of method or constructor parameters, the objects they're working with will behave correctly at runtime.</span></span>

### <a name="single-responsibility"></a><span data-ttu-id="10a34-148">Единственная обязанность</span><span class="sxs-lookup"><span data-stu-id="10a34-148">Single responsibility</span></span>

<span data-ttu-id="10a34-149">Принцип единственной обязанности применяется к объектно-ориентированному проектированию, но также может рассматриваться и как архитектурный принцип аналогично разделению задач.</span><span class="sxs-lookup"><span data-stu-id="10a34-149">The single responsibility principle applies to object-oriented design, but can also be considered as an architectural principle similar to separation of concerns.</span></span> <span data-ttu-id="10a34-150">Этот принцип подразумевает, что объекты должны иметь только одну обязанность и только одну причину для изменения.</span><span class="sxs-lookup"><span data-stu-id="10a34-150">It states that objects should have only one responsibility and that they should have only one reason to change.</span></span> <span data-ttu-id="10a34-151">В частности, единственным сценарием, в котором должен изменяться объект, является обновление способа выполнения объектом его единственной обязанности.</span><span class="sxs-lookup"><span data-stu-id="10a34-151">Specifically, the only situation in which the object should change is if the manner in which it performs its one responsibility must be updated.</span></span> <span data-ttu-id="10a34-152">Соблюдение этого принципа позволяет создавать модульные системы с меньшей степенью связанности, поскольку многие виды нового поведения можно реализовать в виде новых классов, не добавляя дополнительные обязанности к существующим классам.</span><span class="sxs-lookup"><span data-stu-id="10a34-152">Following this principle helps to produce more loosely-coupled and modular systems, since many kinds of new behavior can be implemented as new classes, rather than by adding additional responsibility to existing classes.</span></span> <span data-ttu-id="10a34-153">Добавление новых классов всегда более безопасно по сравнению с изменением существующих, поскольку в этот момент никакой код еще не зависит от новых классов.</span><span class="sxs-lookup"><span data-stu-id="10a34-153">Adding new classes is always safer than changing existing classes, since no code yet depends on the new classes.</span></span>

<span data-ttu-id="10a34-154">В монолитном приложении принцип единственной обязанности может применяться на высоком уровне к слоям приложения.</span><span class="sxs-lookup"><span data-stu-id="10a34-154">In a monolithic application, we can apply the single responsibility principle at a high level to the layers in the application.</span></span> <span data-ttu-id="10a34-155">Обязанность представления должна оставаться в проекте пользовательского интерфейса, тогда как обязанность доступа к данным будет отнесена к проекту инфраструктуры.</span><span class="sxs-lookup"><span data-stu-id="10a34-155">Presentation responsibility should remain in the UI project, while data access responsibility should be kept within an infrastructure project.</span></span> <span data-ttu-id="10a34-156">Бизнес-логика должна находиться в основном проекте приложения, где ее можно тестировать и модернизировать независимо от других обязанностей.</span><span class="sxs-lookup"><span data-stu-id="10a34-156">Business logic should be kept in the application core project, where it can be easily tested and can evolve independently from other responsibilities.</span></span>

<span data-ttu-id="10a34-157">Доведение этого принципа до логической конечной точки в отношении архитектуры приложения позволяет получить микрослужбы.</span><span class="sxs-lookup"><span data-stu-id="10a34-157">When this principle is applied to application architecture, and taken to its logical endpoint, you get microservices.</span></span> <span data-ttu-id="10a34-158">Любая микрослужба должна иметь единственную обязанность.</span><span class="sxs-lookup"><span data-stu-id="10a34-158">A given microservice should have a single responsibility.</span></span> <span data-ttu-id="10a34-159">Если вам требуется расширить функциональные возможности системы, в большинстве случаев это рекомендуется делать путем добавления новых микрослужб, а не расширения обязанностей существующих.</span><span class="sxs-lookup"><span data-stu-id="10a34-159">If you need to extend the behavior of a system, it's usually better to do it by adding additional microservices, rather than by adding responsibility to an existing one.</span></span>

[<span data-ttu-id="10a34-160">Дополнительные сведения об архитектуре микрослужб</span><span class="sxs-lookup"><span data-stu-id="10a34-160">Learn more about microservices architecture</span></span>](https://aka.ms/MicroservicesEbook)

### <a name="dont-repeat-yourself-dry"></a><span data-ttu-id="10a34-161">Принцип "Не повторяйся"</span><span class="sxs-lookup"><span data-stu-id="10a34-161">Don't repeat yourself (DRY)</span></span>

<span data-ttu-id="10a34-162">В приложении не следует определять поведение, связанное с конкретной концепцией, в нескольких местах, поскольку это часто влечет за собой возникновение ошибок.</span><span class="sxs-lookup"><span data-stu-id="10a34-162">The application should avoid specifying behavior related to a particular concept in multiple places as this is a frequent source of errors.</span></span> <span data-ttu-id="10a34-163">В какой-то момент в связи с изменением требований потребуется изменить такое поведение, и в этом случае велик риск, что как минимум в одном месте это обновление не будет проведено, в результате чего вся система будет иметь несогласованное поведение.</span><span class="sxs-lookup"><span data-stu-id="10a34-163">At some point, a change in requirements will require changing this behavior and the likelihood that at least one instance of the behavior will fail to be updated will result in inconsistent behavior of the system.</span></span>

<span data-ttu-id="10a34-164">Вместо того чтобы дублировать логику, ее следует инкапсулировать в конструкции программирования.</span><span class="sxs-lookup"><span data-stu-id="10a34-164">Rather than duplicating logic, encapsulate it in a programming construct.</span></span> <span data-ttu-id="10a34-165">Такая конструкция должна быть единственным исполнителем нужного поведения и использоваться в любых других частях приложения в тех случаях, когда требуется реализовать это поведение.</span><span class="sxs-lookup"><span data-stu-id="10a34-165">Make this construct the single authority over this behavior, and have any other part of the application that requires this behavior use the new construct.</span></span>

> [!NOTE]
> <span data-ttu-id="10a34-166">Не рекомендуется связывать поведение, которое повторяется нерегулярно.</span><span class="sxs-lookup"><span data-stu-id="10a34-166">Avoid binding together behavior that is only coincidentally repetitive.</span></span> <span data-ttu-id="10a34-167">Например, если две разных константы имеют одно значение, вместо них не нужно использовать одну константу, поскольку с точки зрения концепции они ссылаются на разные объекты.</span><span class="sxs-lookup"><span data-stu-id="10a34-167">For example, just because two different constants both have the same value, that doesn't mean you should have only one constant, if conceptually they're referring to different things.</span></span>

### <a name="persistence-ignorance"></a><span data-ttu-id="10a34-168">Независимость сохраняемости</span><span class="sxs-lookup"><span data-stu-id="10a34-168">Persistence ignorance</span></span>

<span data-ttu-id="10a34-169">Принцип **независимости сохраняемости** относится к типам, для которых требуется сохранение состояния, однако код которых не зависит от выбираемой для этих целей технологии.</span><span class="sxs-lookup"><span data-stu-id="10a34-169">**Persistence ignorance** (PI) refers to types that need to be persisted, but whose code is unaffected by the choice of persistence technology.</span></span> <span data-ttu-id="10a34-170">В .NET такие типы иногда называются простыми объектами CLR (POCO), поскольку они не наследуются от конкретного базового класса и не реализуют определенный интерфейс.</span><span class="sxs-lookup"><span data-stu-id="10a34-170">Such types in .NET are sometimes referred to as Plain Old CLR Objects (POCOs), because they do not need to inherit from a particular base class or implement a particular interface.</span></span> <span data-ttu-id="10a34-171">Принцип независимости сохраняемости очень важен, поскольку он позволяет сохранять состояние бизнес-модели различными способами, благодаря чему увеличивается гибкость приложения.</span><span class="sxs-lookup"><span data-stu-id="10a34-171">Persistence ignorance is valuable because it allows the same business model to be persisted in multiple ways, offering additional flexibility to the application.</span></span> <span data-ttu-id="10a34-172">Способы сохраняемости со временем могут изменяться. Например, вместо одной технологии базы данных может использоваться другая, а также могут потребоваться дополнительные способы (например, в дополнение к реляционной базе данных могут использоваться кэш Redis или Azure DocumentDb).</span><span class="sxs-lookup"><span data-stu-id="10a34-172">Persistence choices might change over time, from one database technology to another, or additional forms of persistence might be required in addition to whatever the application started with (for example, using a Redis cache or Azure DocumentDb in addition to a relational database).</span></span>

<span data-ttu-id="10a34-173">Ниже приведены некоторые примеры нарушения этого принципа:</span><span class="sxs-lookup"><span data-stu-id="10a34-173">Some examples of violations of this principle include:</span></span>

- <span data-ttu-id="10a34-174">Обязательный базовый класс.</span><span class="sxs-lookup"><span data-stu-id="10a34-174">A required base class.</span></span>

- <span data-ttu-id="10a34-175">Обязательная реализация интерфейса.</span><span class="sxs-lookup"><span data-stu-id="10a34-175">A required interface implementation.</span></span>

- <span data-ttu-id="10a34-176">Классы, отвечающие за сохранение самих себя (например, шаблон активной записи).</span><span class="sxs-lookup"><span data-stu-id="10a34-176">Classes responsible for saving themselves (such as the Active Record pattern).</span></span>

- <span data-ttu-id="10a34-177">Требуется конструктор без параметров.</span><span class="sxs-lookup"><span data-stu-id="10a34-177">Required parameterless constructor.</span></span>

- <span data-ttu-id="10a34-178">Свойства, использующие ключевое слово virtual.</span><span class="sxs-lookup"><span data-stu-id="10a34-178">Properties requiring virtual keyword.</span></span>

- <span data-ttu-id="10a34-179">Обязательные атрибуты сохраняемости.</span><span class="sxs-lookup"><span data-stu-id="10a34-179">Persistence-specific required attributes.</span></span>

<span data-ttu-id="10a34-180">Обязательное использование любых из указанных возможностей увеличивает степень связанности между типами, для которых требуется сохраняемость, и применяемой для этих целей технологией, что усложняет реализацию новых стратегий доступа к данным в будущем.</span><span class="sxs-lookup"><span data-stu-id="10a34-180">The requirement that classes have any of the above features or behaviors adds coupling between the types to be persisted and the choice of persistence technology, making it more difficult to adopt new data access strategies in the future.</span></span>

### <a name="bounded-contexts"></a><span data-ttu-id="10a34-181">Ограниченные контексты</span><span class="sxs-lookup"><span data-stu-id="10a34-181">Bounded contexts</span></span>

<span data-ttu-id="10a34-182">Принцип **ограниченных контекстов** является центральным при проблемно-ориентированном проектировании.</span><span class="sxs-lookup"><span data-stu-id="10a34-182">**Bounded contexts** are a central pattern in Domain-Driven Design.</span></span> <span data-ttu-id="10a34-183">Его соблюдение позволяет решить проблему сложности в крупных приложениях или организациях за счет разбиения на отдельные концептуальные модули.</span><span class="sxs-lookup"><span data-stu-id="10a34-183">They provide a way of tackling complexity in large applications or organizations by breaking it up into separate conceptual modules.</span></span> <span data-ttu-id="10a34-184">Каждый такой модуль представляет контекст, который отделен от других контекстов (то есть ограничен) и может развиваться независимо.</span><span class="sxs-lookup"><span data-stu-id="10a34-184">Each conceptual module then represents a context which is separated from other contexts (hence, bounded), and can evolve independently.</span></span> <span data-ttu-id="10a34-185">В идеальном случае в каждом ограниченном контексте должны свободно выбираться имена используемых в нем концепций и обеспечиваться монопольный доступ к собственному хранилищу сохраняемости.</span><span class="sxs-lookup"><span data-stu-id="10a34-185">Each bounded context should ideally be free to choose its own names for concepts within it, and should have exclusive access to its own persistence store.</span></span>

<span data-ttu-id="10a34-186">Как минимум, в отдельном веб-приложении необходимо постараться реализовать собственный ограниченный контекст со своим хранилищем сохраняемости для бизнес-модели вместо того, чтобы использовать общую для всех приложений базу данных.</span><span class="sxs-lookup"><span data-stu-id="10a34-186">At a minimum, individual web applications should strive to be their own bounded context, with their own persistence store for their business model, rather than sharing a database with other applications.</span></span> <span data-ttu-id="10a34-187">Взаимодействие между ограниченными контекстами осуществляется посредством программных интерфейсов, а не за счет общей базы данных, благодаря чему бизнес-логика и события могут реагировать на происходящие изменения.</span><span class="sxs-lookup"><span data-stu-id="10a34-187">Communication between bounded contexts occurs through programmatic interfaces, rather than through a shared database, which allows for business logic and events to take place in response to changes that take place.</span></span> <span data-ttu-id="10a34-188">Ограниченные контексты тесно связаны с микрослужбами, которые в идеальном случае также реализуются в качестве отдельных ограниченных контекстов.</span><span class="sxs-lookup"><span data-stu-id="10a34-188">Bounded contexts map closely to microservices, which also are ideally implemented as their own individual bounded contexts.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="10a34-189">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="10a34-189">Additional resources</span></span>

* [<span data-ttu-id="10a34-190">Конструктивные шаблоны JAVA: принципы</span><span class="sxs-lookup"><span data-stu-id="10a34-190">JAVA Design Patterns: Principles</span></span>](https://java-design-patterns.com/principles/)
* [<span data-ttu-id="10a34-191">Ограниченный контекст</span><span class="sxs-lookup"><span data-stu-id="10a34-191">Bounded Context</span></span>](https://martinfowler.com/bliki/BoundedContext.html)

>[!div class="step-by-step"]
><span data-ttu-id="10a34-192">[Назад](choose-between-traditional-web-and-single-page-apps.md)
>[Вперед](common-web-application-architectures.md)</span><span class="sxs-lookup"><span data-stu-id="10a34-192">[Previous](choose-between-traditional-web-and-single-page-apps.md)
[Next](common-web-application-architectures.md)</span></span>