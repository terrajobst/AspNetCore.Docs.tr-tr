---
title: "ASP.NET Çekirdeği ve Entity Framework 6 ile çalışmaya başlama"
author: tdykstra
description: "Bu makalede, bir ASP.NET Core uygulamada Entity Framework 6 kullanmayı gösterir."
keywords: ASP.NET Core, Entity Framework EF 6
ms.author: tdykstra
manager: wpickett
ms.date: 02/24/2017
ms.topic: article
ms.assetid: 016cc836-4c43-45a4-b9a7-9efaf53350df
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/entity-framework-6
ms.openlocfilehash: 8abec95c591f20069e20eec55fd21503e74f8606
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a><span data-ttu-id="f4acb-104">ASP.NET Core ve Entity Framework 6 ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="f4acb-104">Getting started with ASP.NET Core and Entity Framework 6</span></span>

<span data-ttu-id="f4acb-105">Tarafından [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), ve [zel Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f4acb-105">By [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="f4acb-106">Bu makalede, bir ASP.NET Core uygulamada Entity Framework 6 kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="f4acb-106">This article shows how to use Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="overview"></a><span data-ttu-id="f4acb-107">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="f4acb-107">Overview</span></span>

<span data-ttu-id="f4acb-108">Entity Framework 6 .NET Core desteklemediğinden, Entity Framework 6 kullanmak için projeniz .NET Framework karşı derlemek var.</span><span class="sxs-lookup"><span data-stu-id="f4acb-108">To use Entity Framework 6, your project has to compile against .NET Framework, as Entity Framework 6 does not support .NET Core.</span></span> <span data-ttu-id="f4acb-109">Platformlar arası özelliklerine gereksinim duyarsanız için yükseltmeniz gerekmektedir [Entity Framework Çekirdek](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="f4acb-109">If you need cross-platform features you will need to upgrade to [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

<span data-ttu-id="f4acb-110">EF6 içerik yerleştirmek için Entity Framework 6 ASP.NET Core uygulamada kullanmak için önerilen yöntem olduğundan ve bir sınıf kitaplığı'nda modeli sınıfları hedefleyen tam framework projesi.</span><span class="sxs-lookup"><span data-stu-id="f4acb-110">The recommended way to use Entity Framework 6 in an ASP.NET Core application is to put the EF6 context and model classes in a class library project that targets the full framework.</span></span> <span data-ttu-id="f4acb-111">ASP.NET Core projeden sınıf kitaplığına bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f4acb-111">Add a reference to the class library from the ASP.NET Core project.</span></span> <span data-ttu-id="f4acb-112">Örnek bkz [EF6 ve ASP.NET Core projeleri ile Visual Studio çözümü](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span><span class="sxs-lookup"><span data-stu-id="f4acb-112">See the sample [Visual Studio solution with EF6 and ASP.NET Core projects](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span></span>

<span data-ttu-id="f4acb-113">.NET Core projeleri tüm EF6 gibi komutlar işlevselliğini desteklenmediği, ASP.NET Core projesinde EF6 bağlam içine *Enable-Migrations* gerektirir.</span><span class="sxs-lookup"><span data-stu-id="f4acb-113">You can't put an EF6 context in an ASP.NET Core project because .NET Core projects don't support all of the functionality that EF6 commands such as *Enable-Migrations* require.</span></span>

<span data-ttu-id="f4acb-114">EF6 içeriğiniz bulun proje türü ne olursa olsun yalnızca EF6 komut satırı araçları EF6 bağlamı ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="f4acb-114">Regardless of project type in which you locate your EF6 context, only EF6 command-line tools work with an EF6 context.</span></span> <span data-ttu-id="f4acb-115">Örneğin, `Scaffold-DbContext` yalnızca Entity Framework Çekirdek kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f4acb-115">For example, `Scaffold-DbContext` is only available in Entity Framework Core.</span></span> <span data-ttu-id="f4acb-116">Bir EF6 modeline tersine mühendislik bir veritabanı, ihtiyacınız varsa, bkz: [varolan bir veritabanına ilk kod](https://msdn.microsoft.com/jj200620).</span><span class="sxs-lookup"><span data-stu-id="f4acb-116">If you need to do reverse engineering of a database into an EF6 model, see [Code First to an Existing Database](https://msdn.microsoft.com/jj200620).</span></span>

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a><span data-ttu-id="f4acb-117">Başvuru tam framework ve ASP.NET Core projesinde EF6</span><span class="sxs-lookup"><span data-stu-id="f4acb-117">Reference full framework and EF6 in the ASP.NET Core project</span></span>

<span data-ttu-id="f4acb-118">ASP.NET Core projeniz .NET framework ve EF6 başvurmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f4acb-118">Your ASP.NET Core project needs to reference .NET framework and EF6.</span></span> <span data-ttu-id="f4acb-119">Örneğin, *.csproj* ASP.NET Core projenizin dosyasını aşağıdaki örneğe benzer görünür (dosya, yalnızca ilgili bölümlerini gösterilir).</span><span class="sxs-lookup"><span data-stu-id="f4acb-119">For example, the *.csproj* file of your ASP.NET Core project will look similar to the following example (only relevant parts of the file are shown).</span></span>

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

<span data-ttu-id="f4acb-120">Yeni bir proje oluşturuyorsanız kullanmak **ASP.NET çekirdek Web uygulaması (.NET Framework)** şablonu.</span><span class="sxs-lookup"><span data-stu-id="f4acb-120">If you’re creating a new project, use the **ASP.NET Core Web Application (.NET Framework)** template.</span></span>

## <a name="handle-connection-strings"></a><span data-ttu-id="f4acb-121">Bağlantı dizelerini işleme</span><span class="sxs-lookup"><span data-stu-id="f4acb-121">Handle connection strings</span></span>

<span data-ttu-id="f4acb-122">EF6 sınıf kitaplığı projesinde kullanacağınız EF6 komut satırı araçlarını varsayılan bir oluşturucu gerektirdiğinden, bağlam örneğini oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f4acb-122">The EF6 command-line tools that you'll use in the EF6 class library project require a default constructor so they can instantiate the context.</span></span> <span data-ttu-id="f4acb-123">Ancak belirtin, bağlam Oluşturucusu ASP.NET Core projede; bu durumda kullanılacak bağlantı dizesini bağlantı dizesinde geçirmenize olanak sağlayan bir parametresi olmalıdır istersiniz.</span><span class="sxs-lookup"><span data-stu-id="f4acb-123">But you'll probably want to specify the connection string to use in the ASP.NET Core project, in which case your context constructor must have a parameter that lets you pass in the connection string.</span></span> <span data-ttu-id="f4acb-124">Bir örnek verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="f4acb-124">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

<span data-ttu-id="f4acb-125">EF6 içeriğiniz bir parametresiz oluşturucuya sahip olmadığından, bir uygulama sunmak amacıyla EF6 projenizi sahip [Idbcontextfactory](https://msdn.microsoft.com/library/hh506876).</span><span class="sxs-lookup"><span data-stu-id="f4acb-125">Since your EF6 context doesn't have a parameterless constructor, your EF6 project has to provide an implementation of [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span></span> <span data-ttu-id="f4acb-126">EF6 komut satırı araçları bulun ve bu uygulama kullandığından, bağlam örneğini oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f4acb-126">The EF6 command-line tools will find and use that implementation so they can instantiate the context.</span></span> <span data-ttu-id="f4acb-127">Bir örnek verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="f4acb-127">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

<span data-ttu-id="f4acb-128">Bu örnek kodda `IDbContextFactory` uygulama sabit kodlanmış bağlantı dizesinde geçirir.</span><span class="sxs-lookup"><span data-stu-id="f4acb-128">In this sample code, the `IDbContextFactory` implementation passes in a hard-coded connection string.</span></span> <span data-ttu-id="f4acb-129">Bu komut satırı araçlarını kullanacağı bağlantı dizesidir.</span><span class="sxs-lookup"><span data-stu-id="f4acb-129">This is the connection string that the command-line tools will use.</span></span> <span data-ttu-id="f4acb-130">Sınıf kitaplığı çağrı yapan uygulamanın kullandığı aynı bağlantı dizesini kullandığından emin olmak üzere bir strateji uygulamak isteyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="f4acb-130">You'll want to implement a strategy to ensure that the class library uses the same connection string that the calling application uses.</span></span> <span data-ttu-id="f4acb-131">Örneğin, bir ortam değişkeni hem projelerinde gelen değeri alabilir.</span><span class="sxs-lookup"><span data-stu-id="f4acb-131">For example, you could get the value from an environment variable in both projects.</span></span>

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a><span data-ttu-id="f4acb-132">ASP.NET Core projesinde bağımlılık ekleme ayarlama</span><span class="sxs-lookup"><span data-stu-id="f4acb-132">Set up dependency injection in the ASP.NET Core project</span></span>

<span data-ttu-id="f4acb-133">Çekirdek projenin *haline* bağımlılık ekleme (dı) EF6 bağlamı kümesinde dosya `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f4acb-133">In the Core project's *Startup.cs* file, set up the EF6 context for dependency injection (DI) in `ConfigureServices`.</span></span> <span data-ttu-id="f4acb-134">EF bağlam nesneleri için bir istek başına ömrü kapsamlı.</span><span class="sxs-lookup"><span data-stu-id="f4acb-134">EF context objects should be scoped for a per-request lifetime.</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

<span data-ttu-id="f4acb-135">DI kullanarak denetleyicileriniz ardından bağlam örneği elde edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f4acb-135">You can then get an instance of the context in your controllers by using DI.</span></span> <span data-ttu-id="f4acb-136">Kod EF çekirdek bağlamı için ne yazarsınız için benzer:</span><span class="sxs-lookup"><span data-stu-id="f4acb-136">The code is similar to what you'd write for an EF Core context:</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a><span data-ttu-id="f4acb-137">Örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="f4acb-137">Sample application</span></span>

<span data-ttu-id="f4acb-138">Çalışan bir örnek uygulama için bkz: [örnek Visual Studio çözümü](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) bu makalede eşlik.</span><span class="sxs-lookup"><span data-stu-id="f4acb-138">For a working sample application, see the [sample Visual Studio solution](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) that accompanies this article.</span></span>

<span data-ttu-id="f4acb-139">Bu örnek, aşağıdaki adımlarda Visual Studio tarafından sıfırdan oluşturulabilir:</span><span class="sxs-lookup"><span data-stu-id="f4acb-139">This sample can be created from scratch by the following steps in Visual Studio:</span></span>

* <span data-ttu-id="f4acb-140">Bir çözüm oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f4acb-140">Create a solution.</span></span>

* <span data-ttu-id="f4acb-141">**Yeni Proje Ekle > Web > ASP.NET Core Web uygulaması (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="f4acb-141">**Add New Project > Web > ASP.NET Core Web Application (.NET Framework)**</span></span>

* <span data-ttu-id="f4acb-142">**Yeni Proje Ekle > Windows Klasik Masaüstü > sınıf kitaplığı (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="f4acb-142">**Add New Project > Windows Classic Desktop > Class Library (.NET Framework)**</span></span>

* <span data-ttu-id="f4acb-143">İçinde **Paket Yöneticisi Konsolu** (PMC) hem projeleri için komutu çalıştırmak `Install-Package Entityframework`.</span><span class="sxs-lookup"><span data-stu-id="f4acb-143">In **Package Manager Console** (PMC) for both projects, run the command `Install-Package Entityframework`.</span></span>

* <span data-ttu-id="f4acb-144">Sınıf kitaplığı projesinde veri modeli sınıfları ve bağlam sınıfını ve uygulaması oluşturma `IDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="f4acb-144">In the class library project, create data model classes and a context class, and an implementation of `IDbContextFactory`.</span></span>

* <span data-ttu-id="f4acb-145">Sınıf kitaplığı proje için PMC komutları çalıştırmanız `Enable-Migrations` ve `Add-Migration Initial`.</span><span class="sxs-lookup"><span data-stu-id="f4acb-145">In PMC for the class library project, run the commands `Enable-Migrations` and `Add-Migration Initial`.</span></span> <span data-ttu-id="f4acb-146">Başlangıç projesi olarak ASP.NET Core proje ayarlarsanız eklemek `-StartupProjectName EF6` bu komutları.</span><span class="sxs-lookup"><span data-stu-id="f4acb-146">If you have set the ASP.NET Core project as the startup project, add `-StartupProjectName EF6` to these commands.</span></span>

* <span data-ttu-id="f4acb-147">Çekirdek projesinde sınıf kitaplığı proje proje başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f4acb-147">In the Core project, add a project reference to the class library project.</span></span>

* <span data-ttu-id="f4acb-148">Çekirdek projesinde içinde *haline*, bağlam için dı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="f4acb-148">In the Core project, in *Startup.cs*, register the context for DI.</span></span>

* <span data-ttu-id="f4acb-149">Çekirdek projesinde içinde *appsettings.json*, bağlantı dizesi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f4acb-149">In the Core project, in *appsettings.json*, add the connection string.</span></span>

* <span data-ttu-id="f4acb-150">Çekirdek projesinde, denetleyici ve okuma ve veri yazma doğrulamak için görünümler ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f4acb-150">In the Core project, add a controller and view(s) to verify that you can read and write data.</span></span> <span data-ttu-id="f4acb-151">(ASP.NET Core MVC yapı iskelesi Sınıf Kitaplığı'ndan başvurulan EF6 bağlamına sahip çalışmaz unutmayın.)</span><span class="sxs-lookup"><span data-stu-id="f4acb-151">(Note that ASP.NET Core MVC scaffolding won't work with the EF6 context referenced from the class library.)</span></span>

## <a name="summary"></a><span data-ttu-id="f4acb-152">Özet</span><span class="sxs-lookup"><span data-stu-id="f4acb-152">Summary</span></span>

<span data-ttu-id="f4acb-153">Bu makalede, bir ASP.NET Core uygulamada Entity Framework 6 kullanmaya yönelik temel kılavuz sağlamıştır.</span><span class="sxs-lookup"><span data-stu-id="f4acb-153">This article has provided basic guidance for using Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f4acb-154">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="f4acb-154">Additional Resources</span></span>

* [<span data-ttu-id="f4acb-155">Entity Framework - kod tabanlı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f4acb-155">Entity Framework - Code-Based Configuration</span></span>](https://msdn.microsoft.com/data/jj680699.aspx)
