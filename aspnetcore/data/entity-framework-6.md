---
title: "ASP.NET Çekirdeği ve Entity Framework 6 ile çalışmaya başlama"
author: tdykstra
description: "Bu makalede, bir ASP.NET Core uygulamada Entity Framework 6 kullanmayı gösterir."
ms.author: tdykstra
manager: wpickett
ms.date: 02/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/entity-framework-6
ms.openlocfilehash: 51445b8c110ad618aeb680148ccf4304a45ee16e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a><span data-ttu-id="d6b42-103">ASP.NET Core ve Entity Framework 6 ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="d6b42-103">Getting started with ASP.NET Core and Entity Framework 6</span></span>

<span data-ttu-id="d6b42-104">Tarafından [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), ve [zel Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d6b42-104">By [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="d6b42-105">Bu makalede, bir ASP.NET Core uygulamada Entity Framework 6 kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="d6b42-105">This article shows how to use Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="overview"></a><span data-ttu-id="d6b42-106">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="d6b42-106">Overview</span></span>

<span data-ttu-id="d6b42-107">Entity Framework 6 .NET Core desteklemediğinden, Entity Framework 6 kullanmak için projeniz .NET Framework karşı derlemek var.</span><span class="sxs-lookup"><span data-stu-id="d6b42-107">To use Entity Framework 6, your project has to compile against .NET Framework, as Entity Framework 6 does not support .NET Core.</span></span> <span data-ttu-id="d6b42-108">Platformlar arası özelliklerine gereksinim duyarsanız için yükseltmeniz gerekmektedir [Entity Framework Çekirdek](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="d6b42-108">If you need cross-platform features you will need to upgrade to [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

<span data-ttu-id="d6b42-109">EF6 içerik yerleştirmek için Entity Framework 6 ASP.NET Core uygulamada kullanmak için önerilen yöntem olduğundan ve bir sınıf kitaplığı'nda modeli sınıfları hedefleyen tam framework projesi.</span><span class="sxs-lookup"><span data-stu-id="d6b42-109">The recommended way to use Entity Framework 6 in an ASP.NET Core application is to put the EF6 context and model classes in a class library project that targets the full framework.</span></span> <span data-ttu-id="d6b42-110">ASP.NET Core projeden sınıf kitaplığına bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d6b42-110">Add a reference to the class library from the ASP.NET Core project.</span></span> <span data-ttu-id="d6b42-111">Örnek bkz [EF6 ve ASP.NET Core projeleri ile Visual Studio çözümü](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span><span class="sxs-lookup"><span data-stu-id="d6b42-111">See the sample [Visual Studio solution with EF6 and ASP.NET Core projects](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span></span>

<span data-ttu-id="d6b42-112">.NET Core projeleri tüm EF6 gibi komutlar işlevselliğini desteklenmediği, ASP.NET Core projesinde EF6 bağlam içine *Enable-Migrations* gerektirir.</span><span class="sxs-lookup"><span data-stu-id="d6b42-112">You can't put an EF6 context in an ASP.NET Core project because .NET Core projects don't support all of the functionality that EF6 commands such as *Enable-Migrations* require.</span></span>

<span data-ttu-id="d6b42-113">EF6 içeriğiniz bulun proje türü ne olursa olsun yalnızca EF6 komut satırı araçları EF6 bağlamı ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="d6b42-113">Regardless of project type in which you locate your EF6 context, only EF6 command-line tools work with an EF6 context.</span></span> <span data-ttu-id="d6b42-114">Örneğin, `Scaffold-DbContext` yalnızca Entity Framework Çekirdek kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d6b42-114">For example, `Scaffold-DbContext` is only available in Entity Framework Core.</span></span> <span data-ttu-id="d6b42-115">Bir EF6 modeline tersine mühendislik bir veritabanı, ihtiyacınız varsa, bkz: [varolan bir veritabanına ilk kod](https://msdn.microsoft.com/jj200620).</span><span class="sxs-lookup"><span data-stu-id="d6b42-115">If you need to do reverse engineering of a database into an EF6 model, see [Code First to an Existing Database](https://msdn.microsoft.com/jj200620).</span></span>

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a><span data-ttu-id="d6b42-116">Başvuru tam framework ve ASP.NET Core projesinde EF6</span><span class="sxs-lookup"><span data-stu-id="d6b42-116">Reference full framework and EF6 in the ASP.NET Core project</span></span>

<span data-ttu-id="d6b42-117">ASP.NET Core projeniz .NET framework ve EF6 başvurmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d6b42-117">Your ASP.NET Core project needs to reference .NET framework and EF6.</span></span> <span data-ttu-id="d6b42-118">Örneğin, *.csproj* ASP.NET Core projenizin dosyasını aşağıdaki örneğe benzer görünür (dosya, yalnızca ilgili bölümlerini gösterilir).</span><span class="sxs-lookup"><span data-stu-id="d6b42-118">For example, the *.csproj* file of your ASP.NET Core project will look similar to the following example (only relevant parts of the file are shown).</span></span>

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

<span data-ttu-id="d6b42-119">Yeni bir proje oluşturuyorsanız kullanmak **ASP.NET çekirdek Web uygulaması (.NET Framework)** şablonu.</span><span class="sxs-lookup"><span data-stu-id="d6b42-119">If you’re creating a new project, use the **ASP.NET Core Web Application (.NET Framework)** template.</span></span>

## <a name="handle-connection-strings"></a><span data-ttu-id="d6b42-120">Bağlantı dizelerini işleme</span><span class="sxs-lookup"><span data-stu-id="d6b42-120">Handle connection strings</span></span>

<span data-ttu-id="d6b42-121">EF6 sınıf kitaplığı projesinde kullanacağınız EF6 komut satırı araçlarını varsayılan bir oluşturucu gerektirdiğinden, bağlam örneğini oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d6b42-121">The EF6 command-line tools that you'll use in the EF6 class library project require a default constructor so they can instantiate the context.</span></span> <span data-ttu-id="d6b42-122">Ancak belirtin, bağlam Oluşturucusu ASP.NET Core projede; bu durumda kullanılacak bağlantı dizesini bağlantı dizesinde geçirmenize olanak sağlayan bir parametresi olmalıdır istersiniz.</span><span class="sxs-lookup"><span data-stu-id="d6b42-122">But you'll probably want to specify the connection string to use in the ASP.NET Core project, in which case your context constructor must have a parameter that lets you pass in the connection string.</span></span> <span data-ttu-id="d6b42-123">Bir örnek verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d6b42-123">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

<span data-ttu-id="d6b42-124">EF6 içeriğiniz bir parametresiz oluşturucuya sahip olmadığından, bir uygulama sunmak amacıyla EF6 projenizi sahip [Idbcontextfactory](https://msdn.microsoft.com/library/hh506876).</span><span class="sxs-lookup"><span data-stu-id="d6b42-124">Since your EF6 context doesn't have a parameterless constructor, your EF6 project has to provide an implementation of [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span></span> <span data-ttu-id="d6b42-125">EF6 komut satırı araçları bulun ve bu uygulama kullandığından, bağlam örneğini oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d6b42-125">The EF6 command-line tools will find and use that implementation so they can instantiate the context.</span></span> <span data-ttu-id="d6b42-126">Bir örnek verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d6b42-126">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

<span data-ttu-id="d6b42-127">Bu örnek kodda `IDbContextFactory` uygulama sabit kodlanmış bağlantı dizesinde geçirir.</span><span class="sxs-lookup"><span data-stu-id="d6b42-127">In this sample code, the `IDbContextFactory` implementation passes in a hard-coded connection string.</span></span> <span data-ttu-id="d6b42-128">Bu komut satırı araçlarını kullanacağı bağlantı dizesidir.</span><span class="sxs-lookup"><span data-stu-id="d6b42-128">This is the connection string that the command-line tools will use.</span></span> <span data-ttu-id="d6b42-129">Sınıf kitaplığı çağrı yapan uygulamanın kullandığı aynı bağlantı dizesini kullandığından emin olmak üzere bir strateji uygulamak isteyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="d6b42-129">You'll want to implement a strategy to ensure that the class library uses the same connection string that the calling application uses.</span></span> <span data-ttu-id="d6b42-130">Örneğin, bir ortam değişkeni hem projelerinde gelen değeri alabilir.</span><span class="sxs-lookup"><span data-stu-id="d6b42-130">For example, you could get the value from an environment variable in both projects.</span></span>

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a><span data-ttu-id="d6b42-131">ASP.NET Core projesinde bağımlılık ekleme ayarlama</span><span class="sxs-lookup"><span data-stu-id="d6b42-131">Set up dependency injection in the ASP.NET Core project</span></span>

<span data-ttu-id="d6b42-132">Çekirdek projenin *haline* bağımlılık ekleme (dı) EF6 bağlamı kümesinde dosya `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d6b42-132">In the Core project's *Startup.cs* file, set up the EF6 context for dependency injection (DI) in `ConfigureServices`.</span></span> <span data-ttu-id="d6b42-133">EF bağlam nesneleri için bir istek başına ömrü kapsamlı.</span><span class="sxs-lookup"><span data-stu-id="d6b42-133">EF context objects should be scoped for a per-request lifetime.</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

<span data-ttu-id="d6b42-134">DI kullanarak denetleyicileriniz ardından bağlam örneği elde edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d6b42-134">You can then get an instance of the context in your controllers by using DI.</span></span> <span data-ttu-id="d6b42-135">Kod EF çekirdek bağlamı için ne yazarsınız için benzer:</span><span class="sxs-lookup"><span data-stu-id="d6b42-135">The code is similar to what you'd write for an EF Core context:</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a><span data-ttu-id="d6b42-136">Örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="d6b42-136">Sample application</span></span>

<span data-ttu-id="d6b42-137">Çalışan bir örnek uygulama için bkz: [örnek Visual Studio çözümü](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) bu makalede eşlik.</span><span class="sxs-lookup"><span data-stu-id="d6b42-137">For a working sample application, see the [sample Visual Studio solution](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) that accompanies this article.</span></span>

<span data-ttu-id="d6b42-138">Bu örnek, aşağıdaki adımlarda Visual Studio tarafından sıfırdan oluşturulabilir:</span><span class="sxs-lookup"><span data-stu-id="d6b42-138">This sample can be created from scratch by the following steps in Visual Studio:</span></span>

* <span data-ttu-id="d6b42-139">Bir çözüm oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d6b42-139">Create a solution.</span></span>

* <span data-ttu-id="d6b42-140">**Yeni Proje Ekle > Web > ASP.NET Core Web uygulaması (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="d6b42-140">**Add New Project > Web > ASP.NET Core Web Application (.NET Framework)**</span></span>

* <span data-ttu-id="d6b42-141">**Yeni Proje Ekle > Windows Klasik Masaüstü > sınıf kitaplığı (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="d6b42-141">**Add New Project > Windows Classic Desktop > Class Library (.NET Framework)**</span></span>

* <span data-ttu-id="d6b42-142">İçinde **Paket Yöneticisi Konsolu** (PMC) hem projeleri için komutu çalıştırmak `Install-Package Entityframework`.</span><span class="sxs-lookup"><span data-stu-id="d6b42-142">In **Package Manager Console** (PMC) for both projects, run the command `Install-Package Entityframework`.</span></span>

* <span data-ttu-id="d6b42-143">Sınıf kitaplığı projesinde veri modeli sınıfları ve bağlam sınıfını ve uygulaması oluşturma `IDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="d6b42-143">In the class library project, create data model classes and a context class, and an implementation of `IDbContextFactory`.</span></span>

* <span data-ttu-id="d6b42-144">Sınıf kitaplığı proje için PMC komutları çalıştırmanız `Enable-Migrations` ve `Add-Migration Initial`.</span><span class="sxs-lookup"><span data-stu-id="d6b42-144">In PMC for the class library project, run the commands `Enable-Migrations` and `Add-Migration Initial`.</span></span> <span data-ttu-id="d6b42-145">Başlangıç projesi olarak ASP.NET Core proje ayarlarsanız eklemek `-StartupProjectName EF6` bu komutları.</span><span class="sxs-lookup"><span data-stu-id="d6b42-145">If you have set the ASP.NET Core project as the startup project, add `-StartupProjectName EF6` to these commands.</span></span>

* <span data-ttu-id="d6b42-146">Çekirdek projesinde sınıf kitaplığı proje proje başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d6b42-146">In the Core project, add a project reference to the class library project.</span></span>

* <span data-ttu-id="d6b42-147">Çekirdek projesinde içinde *haline*, bağlam için dı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="d6b42-147">In the Core project, in *Startup.cs*, register the context for DI.</span></span>

* <span data-ttu-id="d6b42-148">Çekirdek projesinde içinde *appsettings.json*, bağlantı dizesi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d6b42-148">In the Core project, in *appsettings.json*, add the connection string.</span></span>

* <span data-ttu-id="d6b42-149">Çekirdek projesinde, denetleyici ve okuma ve veri yazma doğrulamak için görünümler ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d6b42-149">In the Core project, add a controller and view(s) to verify that you can read and write data.</span></span> <span data-ttu-id="d6b42-150">(ASP.NET Core MVC yapı iskelesi Sınıf Kitaplığı'ndan başvurulan EF6 bağlamına sahip çalışmaz unutmayın.)</span><span class="sxs-lookup"><span data-stu-id="d6b42-150">(Note that ASP.NET Core MVC scaffolding won't work with the EF6 context referenced from the class library.)</span></span>

## <a name="summary"></a><span data-ttu-id="d6b42-151">Özet</span><span class="sxs-lookup"><span data-stu-id="d6b42-151">Summary</span></span>

<span data-ttu-id="d6b42-152">Bu makalede, bir ASP.NET Core uygulamada Entity Framework 6 kullanmaya yönelik temel kılavuz sağlamıştır.</span><span class="sxs-lookup"><span data-stu-id="d6b42-152">This article has provided basic guidance for using Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d6b42-153">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d6b42-153">Additional Resources</span></span>

* [<span data-ttu-id="d6b42-154">Entity Framework - kod tabanlı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d6b42-154">Entity Framework - Code-Based Configuration</span></span>](https://msdn.microsoft.com/data/jj680699.aspx)
