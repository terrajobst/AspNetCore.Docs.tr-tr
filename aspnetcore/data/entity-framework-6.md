---
title: ASP.NET Core ve Entity Framework 6 ile çalışmaya başlama
author: rick-anderson
description: Bu makalede, Entity Framework 6 ' nın ASP.NET Core bir uygulamada nasıl kullanılacağı gösterilmektedir.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/entity-framework-6
ms.openlocfilehash: ace937e72efa2343e50b11d52ebc0a2530505758
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975590"
---
# <a name="get-started-with-aspnet-core-and-entity-framework-6"></a><span data-ttu-id="a7e63-103">ASP.NET Core ve Entity Framework 6 ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="a7e63-103">Get Started with ASP.NET Core and Entity Framework 6</span></span>

<span data-ttu-id="a7e63-104">By [Paweł Grudzień](https://github.com/pgrudzien12), [Davmien Pontıex](https://github.com/DamienPontifex)ve [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a7e63-104">By [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="a7e63-105">Bu makalede, Entity Framework 6 ' nın ASP.NET Core bir uygulamada nasıl kullanılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="a7e63-105">This article shows how to use Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="overview"></a><span data-ttu-id="a7e63-106">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="a7e63-106">Overview</span></span>

<span data-ttu-id="a7e63-107">Entity Framework 6 ' yı kullanmak için, Entity Framework 6 ' nın .NET Core 'u desteklemediği için, projenizin .NET Framework karşı derlenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="a7e63-107">To use Entity Framework 6, your project has to compile against .NET Framework, as Entity Framework 6 doesn't support .NET Core.</span></span> <span data-ttu-id="a7e63-108">Platformlar arası özelliklere ihtiyacınız varsa [Entity Framework Core](/ef/)yükseltmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a7e63-108">If you need cross-platform features you will need to upgrade to [Entity Framework Core](/ef/).</span></span>

<span data-ttu-id="a7e63-109">ASP.NET Core uygulamasında 6 Entity Framework kullanmanın önerilen yolu, tam Framework 'ü hedefleyen bir sınıf kitaplığı projesine EF6 bağlamını ve model sınıflarını koykullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="a7e63-109">The recommended way to use Entity Framework 6 in an ASP.NET Core application is to put the EF6 context and model classes in a class library project that targets the full framework.</span></span> <span data-ttu-id="a7e63-110">ASP.NET Core projesinden sınıf kitaplığına bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a7e63-110">Add a reference to the class library from the ASP.NET Core project.</span></span> <span data-ttu-id="a7e63-111">Örnek [Visual Studio çözümüne EF6 ve ASP.NET Core projelerine](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/entity-framework-6/sample/)bakın.</span><span class="sxs-lookup"><span data-stu-id="a7e63-111">See the sample [Visual Studio solution with EF6 and ASP.NET Core projects](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span></span>

<span data-ttu-id="a7e63-112">.NET Core projeleri, *Enable-geçişler* gibi EF6 komutlarının tüm işlevlerini desteklemediğinden, bir ASP.NET Core projesine EF6 bağlamı koyamazsınız.</span><span class="sxs-lookup"><span data-stu-id="a7e63-112">You can't put an EF6 context in an ASP.NET Core project because .NET Core projects don't support all of the functionality that EF6 commands such as *Enable-Migrations* require.</span></span>

<span data-ttu-id="a7e63-113">EF6 bağlamını bulmakta olduğunuz proje türünden bağımsız olarak, yalnızca EF6 komut satırı araçları bir EF6 bağlamıyla çalışır.</span><span class="sxs-lookup"><span data-stu-id="a7e63-113">Regardless of project type in which you locate your EF6 context, only EF6 command-line tools work with an EF6 context.</span></span> <span data-ttu-id="a7e63-114">Örneğin, `Scaffold-DbContext` yalnızca Entity Framework Core kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a7e63-114">For example, `Scaffold-DbContext` is only available in Entity Framework Core.</span></span> <span data-ttu-id="a7e63-115">Bir veritabanının EF6 modeline ters mühendislik uygulamanız gerekiyorsa bkz. [var olan bir veritabanına Code First](https://msdn.microsoft.com/jj200620).</span><span class="sxs-lookup"><span data-stu-id="a7e63-115">If you need to do reverse engineering of a database into an EF6 model, see [Code First to an Existing Database](https://msdn.microsoft.com/jj200620).</span></span>

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a><span data-ttu-id="a7e63-116">ASP.NET Core projesindeki tam Framework ve EF6 başvurusu</span><span class="sxs-lookup"><span data-stu-id="a7e63-116">Reference full framework and EF6 in the ASP.NET Core project</span></span>

<span data-ttu-id="a7e63-117">ASP.NET Core projenizin .NET Framework ve EF6 'e başvurması gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="a7e63-117">Your ASP.NET Core project needs to reference .NET framework and EF6.</span></span> <span data-ttu-id="a7e63-118">Örneğin, ASP.NET Core projenizin *. csproj* dosyası aşağıdaki örneğe benzer olacaktır (yalnızca dosyanın ilgili bölümleri gösterilir).</span><span class="sxs-lookup"><span data-stu-id="a7e63-118">For example, the *.csproj* file of your ASP.NET Core project will look similar to the following example (only relevant parts of the file are shown).</span></span>

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

<span data-ttu-id="a7e63-119">Yeni bir proje oluştururken **ASP.NET Core Web uygulaması (.NET Framework)** şablonunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="a7e63-119">When creating a new project, use the **ASP.NET Core Web Application (.NET Framework)** template.</span></span>

## <a name="handle-connection-strings"></a><span data-ttu-id="a7e63-120">Bağlantı dizelerini işle</span><span class="sxs-lookup"><span data-stu-id="a7e63-120">Handle connection strings</span></span>

<span data-ttu-id="a7e63-121">EF6 sınıf kitaplığı projesinde kullanacağınız EF6 komut satırı araçları, bağlamı örneklenebilen bir varsayılan Oluşturucu gerektirir.</span><span class="sxs-lookup"><span data-stu-id="a7e63-121">The EF6 command-line tools that you'll use in the EF6 class library project require a default constructor so they can instantiate the context.</span></span> <span data-ttu-id="a7e63-122">Ancak büyük olasılıkla, ASP.NET Core projesinde kullanılacak bağlantı dizesini belirtmek isteyeceksiniz. Bu durumda, bağlam oluşturucunun bağlantı dizesinde geçiş yapmanızı sağlayan bir parametreye sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a7e63-122">But you'll probably want to specify the connection string to use in the ASP.NET Core project, in which case your context constructor must have a parameter that lets you pass in the connection string.</span></span> <span data-ttu-id="a7e63-123">İşte bir örnek.</span><span class="sxs-lookup"><span data-stu-id="a7e63-123">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

<span data-ttu-id="a7e63-124">EF6 içeriğiniz parametresiz bir oluşturucuya sahip olmadığından, EF6 projenizin bir [ıdbcontextfactory](https://msdn.microsoft.com/library/hh506876)uygulamasını sağlaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a7e63-124">Since your EF6 context doesn't have a parameterless constructor, your EF6 project has to provide an implementation of [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span></span> <span data-ttu-id="a7e63-125">EF6 komut satırı araçları, bağlamı örneklebilmeleri için bu uygulamayı bulup kullanacaktır.</span><span class="sxs-lookup"><span data-stu-id="a7e63-125">The EF6 command-line tools will find and use that implementation so they can instantiate the context.</span></span> <span data-ttu-id="a7e63-126">İşte bir örnek.</span><span class="sxs-lookup"><span data-stu-id="a7e63-126">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

<span data-ttu-id="a7e63-127">Bu örnek kodda, `IDbContextFactory` uygulama sabit kodlanmış bir bağlantı dizesinde geçirilir.</span><span class="sxs-lookup"><span data-stu-id="a7e63-127">In this sample code, the `IDbContextFactory` implementation passes in a hard-coded connection string.</span></span> <span data-ttu-id="a7e63-128">Bu, komut satırı araçlarının kullanacağı bağlantı dizesidir.</span><span class="sxs-lookup"><span data-stu-id="a7e63-128">This is the connection string that the command-line tools will use.</span></span> <span data-ttu-id="a7e63-129">Sınıf kitaplığının çağıran uygulamanın kullandığı bağlantı dizesini kullandığından emin olmak için bir strateji uygulamak isteyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="a7e63-129">You'll want to implement a strategy to ensure that the class library uses the same connection string that the calling application uses.</span></span> <span data-ttu-id="a7e63-130">Örneğin, her iki projedeki bir ortam değişkeninden değeri alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a7e63-130">For example, you could get the value from an environment variable in both projects.</span></span>

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a><span data-ttu-id="a7e63-131">ASP.NET Core projesine bağımlılık ekleme işlemini ayarlama</span><span class="sxs-lookup"><span data-stu-id="a7e63-131">Set up dependency injection in the ASP.NET Core project</span></span>

<span data-ttu-id="a7e63-132">Çekirdek projenin *Startup.cs* dosyasında, içindeki `ConfigureServices`bağımlılık ekleme (dı) için EF6 bağlamını ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="a7e63-132">In the Core project's *Startup.cs* file, set up the EF6 context for dependency injection (DI) in `ConfigureServices`.</span></span> <span data-ttu-id="a7e63-133">EF bağlam nesneleri, istek başına ömür için kapsamı belirlenmiş olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a7e63-133">EF context objects should be scoped for a per-request lifetime.</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

<span data-ttu-id="a7e63-134">Daha sonra, DI kullanarak denetleyicilerinizdeki bağlamın bir örneğini alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a7e63-134">You can then get an instance of the context in your controllers by using DI.</span></span> <span data-ttu-id="a7e63-135">Kod, EF Core bağlamı için yazdıklarınız ile benzerdir:</span><span class="sxs-lookup"><span data-stu-id="a7e63-135">The code is similar to what you'd write for an EF Core context:</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a><span data-ttu-id="a7e63-136">Örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="a7e63-136">Sample application</span></span>

<span data-ttu-id="a7e63-137">Çalışan bir örnek uygulama için, bu makaleye eşlik eden [örnek Visual Studio çözümü](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) ' ne bakın.</span><span class="sxs-lookup"><span data-stu-id="a7e63-137">For a working sample application, see the [sample Visual Studio solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) that accompanies this article.</span></span>

<span data-ttu-id="a7e63-138">Bu örnek, Visual Studio 'da aşağıdaki adımlarla sıfırdan oluşturulabilir:</span><span class="sxs-lookup"><span data-stu-id="a7e63-138">This sample can be created from scratch by the following steps in Visual Studio:</span></span>

* <span data-ttu-id="a7e63-139">Bir çözüm oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a7e63-139">Create a solution.</span></span>

* <span data-ttu-id="a7e63-140"> > **Yeni Project**WebASP.NETCore > Web uygulaması Ekle > </span><span class="sxs-lookup"><span data-stu-id="a7e63-140">**Add** > **New Project** > **Web** > **ASP.NET Core Web Application**</span></span>
  * <span data-ttu-id="a7e63-141">Proje şablonu seçimi iletişim kutusunda, açılan menüde API ve .NET Framework seçin</span><span class="sxs-lookup"><span data-stu-id="a7e63-141">In project template selection dialog, select API and .NET Framework in dropdown</span></span>

* <span data-ttu-id="a7e63-142"> > **Yeni proje**EkleWindows > Masaüstü**sınıf kitaplığı (.NET Framework)**  > </span><span class="sxs-lookup"><span data-stu-id="a7e63-142">**Add** > **New Project** > **Windows Desktop** > **Class Library (.NET Framework)**</span></span>

* <span data-ttu-id="a7e63-143">Her iki proje için de **Paket Yöneticisi konsolunda** (PMC) komutunu `Install-Package Entityframework`çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a7e63-143">In **Package Manager Console** (PMC) for both projects, run the command `Install-Package Entityframework`.</span></span>

* <span data-ttu-id="a7e63-144">Sınıf kitaplığı projesinde, veri modeli sınıfları ve bağlam sınıfı ve uygulamasını `IDbContextFactory`oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a7e63-144">In the class library project, create data model classes and a context class, and an implementation of `IDbContextFactory`.</span></span>

* <span data-ttu-id="a7e63-145">Sınıf kitaplığı projesi için PMC 'de, ve `Enable-Migrations` `Add-Migration Initial`komutlarını çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a7e63-145">In PMC for the class library project, run the commands `Enable-Migrations` and `Add-Migration Initial`.</span></span> <span data-ttu-id="a7e63-146">ASP.NET Core projesini başlangıç projesi olarak ayarladıysanız, bu komutlara ekleyin `-StartupProjectName EF6` .</span><span class="sxs-lookup"><span data-stu-id="a7e63-146">If you have set the ASP.NET Core project as the startup project, add `-StartupProjectName EF6` to these commands.</span></span>

* <span data-ttu-id="a7e63-147">Çekirdek projede, sınıf kitaplığı projesine bir proje başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a7e63-147">In the Core project, add a project reference to the class library project.</span></span>

* <span data-ttu-id="a7e63-148">Çekirdek projede, *Startup.cs*IÇINDE, dı için bağlamını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="a7e63-148">In the Core project, in *Startup.cs*, register the context for DI.</span></span>

* <span data-ttu-id="a7e63-149">Core projesinde, *appSettings. JSON*içinde bağlantı dizesini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a7e63-149">In the Core project, in *appsettings.json*, add the connection string.</span></span>

* <span data-ttu-id="a7e63-150">Temel projede, verileri okuyabildiğinizi ve yazabildiğinizi doğrulamak için bir denetleyici ve görünüm ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a7e63-150">In the Core project, add a controller and view(s) to verify that you can read and write data.</span></span> <span data-ttu-id="a7e63-151">(ASP.NET Core MVC yapı iskelesi, sınıf kitaplığından başvurulan EF6 bağlamıyla çalışmaz.)</span><span class="sxs-lookup"><span data-stu-id="a7e63-151">(Note that ASP.NET Core MVC scaffolding won't work with the EF6 context referenced from the class library.)</span></span>

## <a name="summary"></a><span data-ttu-id="a7e63-152">Özet</span><span class="sxs-lookup"><span data-stu-id="a7e63-152">Summary</span></span>

<span data-ttu-id="a7e63-153">Bu makale, bir ASP.NET Core uygulamasında 6 Entity Framework kullanmak için temel rehberlik sağlamıştır.</span><span class="sxs-lookup"><span data-stu-id="a7e63-153">This article has provided basic guidance for using Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a7e63-154">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a7e63-154">Additional resources</span></span>

* [<span data-ttu-id="a7e63-155">Kod tabanlı Entity Framework yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a7e63-155">Entity Framework - Code-Based Configuration</span></span>](https://msdn.microsoft.com/data/jj680699.aspx)
