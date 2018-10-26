---
title: ASP.NET Core ve Entity Framework 6 ile çalışmaya başlama
author: rick-anderson
description: Bu makalede, bir ASP.NET Core uygulaması Entity Framework 6 kullanmayı gösterir.
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: data/entity-framework-6
ms.openlocfilehash: b7679afbe4c364386fe8f16d22d7e9797a3e0c27
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090065"
---
# <a name="get-started-with-aspnet-core-and-entity-framework-6"></a><span data-ttu-id="d358a-103">ASP.NET Core ve Entity Framework 6 ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="d358a-103">Get Started with ASP.NET Core and Entity Framework 6</span></span>

<span data-ttu-id="d358a-104">Tarafından [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), ve [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d358a-104">By [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="d358a-105">Bu makalede, bir ASP.NET Core uygulaması Entity Framework 6 kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="d358a-105">This article shows how to use Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="overview"></a><span data-ttu-id="d358a-106">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="d358a-106">Overview</span></span>

<span data-ttu-id="d358a-107">Entity Framework 6 .NET Core desteklemediğinden Entity Framework 6 kullanmak için projeniz .NET Framework karşı derleme gerekir.</span><span class="sxs-lookup"><span data-stu-id="d358a-107">To use Entity Framework 6, your project has to compile against .NET Framework, as Entity Framework 6 doesn't support .NET Core.</span></span> <span data-ttu-id="d358a-108">Platformlar arası özelliklerine ihtiyacınız varsa, yükseltme gerekecektir [Entity Framework Core](/ef/).</span><span class="sxs-lookup"><span data-stu-id="d358a-108">If you need cross-platform features you will need to upgrade to [Entity Framework Core](/ef/).</span></span>

<span data-ttu-id="d358a-109">Entity Framework 6 içinde ASP.NET Core uygulamasını kullanmak için önerilen yöntem EF6 bağlam eklemektir ve model sınıfları bir sınıf kitaplığı'nda, Framework'ün tamamını hedefleyen proje.</span><span class="sxs-lookup"><span data-stu-id="d358a-109">The recommended way to use Entity Framework 6 in an ASP.NET Core application is to put the EF6 context and model classes in a class library project that targets the full framework.</span></span> <span data-ttu-id="d358a-110">Sınıf kitaplığı ASP.NET Core projesi bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d358a-110">Add a reference to the class library from the ASP.NET Core project.</span></span> <span data-ttu-id="d358a-111">Örnek görmek [EF6 ve ASP.NET Core projeleri Visual Studio çözümüyle](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span><span class="sxs-lookup"><span data-stu-id="d358a-111">See the sample [Visual Studio solution with EF6 and ASP.NET Core projects](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span></span>

<span data-ttu-id="d358a-112">.NET Core projeleri EF6 gibi komutlar işlevlerin desteklenmediği bir ASP.NET Core projesinde EF6 bağlam konulamıyor *etkinleştir geçişleri* gerektirir.</span><span class="sxs-lookup"><span data-stu-id="d358a-112">You can't put an EF6 context in an ASP.NET Core project because .NET Core projects don't support all of the functionality that EF6 commands such as *Enable-Migrations* require.</span></span>

<span data-ttu-id="d358a-113">EF6 Bağlamınızı bulmanıza, proje türü ne olursa olsun yalnızca EF6 komut satırı araçlarını bir EF6 bağlamı ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="d358a-113">Regardless of project type in which you locate your EF6 context, only EF6 command-line tools work with an EF6 context.</span></span> <span data-ttu-id="d358a-114">Örneğin, `Scaffold-DbContext` yalnızca Entity Framework Core içinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d358a-114">For example, `Scaffold-DbContext` is only available in Entity Framework Core.</span></span> <span data-ttu-id="d358a-115">Bir EF6 modele tersine mühendislik, bir veritabanının gerekiyorsa bkz [var olan bir veritabanına ilk kod](https://msdn.microsoft.com/jj200620).</span><span class="sxs-lookup"><span data-stu-id="d358a-115">If you need to do reverse engineering of a database into an EF6 model, see [Code First to an Existing Database](https://msdn.microsoft.com/jj200620).</span></span>

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a><span data-ttu-id="d358a-116">Tam başvuru framework ve ASP.NET Core projesinde EF6</span><span class="sxs-lookup"><span data-stu-id="d358a-116">Reference full framework and EF6 in the ASP.NET Core project</span></span>

<span data-ttu-id="d358a-117">ASP.NET Core projeniz .NET framework ve EF6 başvurmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d358a-117">Your ASP.NET Core project needs to reference .NET framework and EF6.</span></span> <span data-ttu-id="d358a-118">Örneğin, *.csproj* ASP.NET Core proje dosyası aşağıdaki örneğe benzer görünür (yalnızca ilgili bölümlerin dosyanın gösterilir).</span><span class="sxs-lookup"><span data-stu-id="d358a-118">For example, the *.csproj* file of your ASP.NET Core project will look similar to the following example (only relevant parts of the file are shown).</span></span>

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

<span data-ttu-id="d358a-119">Yeni bir proje oluştururken **ASP.NET Core Web uygulaması (.NET Framework)** şablonu.</span><span class="sxs-lookup"><span data-stu-id="d358a-119">When creating a new project, use the **ASP.NET Core Web Application (.NET Framework)** template.</span></span>

## <a name="handle-connection-strings"></a><span data-ttu-id="d358a-120">Bağlantı dizeleri işleme</span><span class="sxs-lookup"><span data-stu-id="d358a-120">Handle connection strings</span></span>

<span data-ttu-id="d358a-121">EF6 sınıf kitaplığı projesinde kullanacağınız EF6 komut satırı araçlarını bir varsayılan oluşturucu gerektirdiğinden, bağlam örneğini oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="d358a-121">The EF6 command-line tools that you'll use in the EF6 class library project require a default constructor so they can instantiate the context.</span></span> <span data-ttu-id="d358a-122">Ancak, içerik oluşturucu durumda ASP.NET Core projesinde kullanılacak bağlantı dizesini bağlantı dizesine geçmenizi sağlayan bir parametreye sahip olmalıdır belirtmek büyük olasılıkla istersiniz.</span><span class="sxs-lookup"><span data-stu-id="d358a-122">But you'll probably want to specify the connection string to use in the ASP.NET Core project, in which case your context constructor must have a parameter that lets you pass in the connection string.</span></span> <span data-ttu-id="d358a-123">Bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d358a-123">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

<span data-ttu-id="d358a-124">EF6 Bağlamınızı parametresiz bir oluşturucu olmadığı bir uygulamasını sağlamak üzere EF6 projenizi sahip [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span><span class="sxs-lookup"><span data-stu-id="d358a-124">Since your EF6 context doesn't have a parameterless constructor, your EF6 project has to provide an implementation of [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span></span> <span data-ttu-id="d358a-125">EF6 komut satırı araçlarını bulun ve bağlam örneğini oluşturabilir, böylece bu uygulamayı kullanın.</span><span class="sxs-lookup"><span data-stu-id="d358a-125">The EF6 command-line tools will find and use that implementation so they can instantiate the context.</span></span> <span data-ttu-id="d358a-126">Bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d358a-126">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

<span data-ttu-id="d358a-127">Bu örnek kodda `IDbContextFactory` uygulaması bir sabit kodlanmış bağlantı dizesine geçirir.</span><span class="sxs-lookup"><span data-stu-id="d358a-127">In this sample code, the `IDbContextFactory` implementation passes in a hard-coded connection string.</span></span> <span data-ttu-id="d358a-128">Bu komut satırı araçlarını kullanan bağlantı dizesidir.</span><span class="sxs-lookup"><span data-stu-id="d358a-128">This is the connection string that the command-line tools will use.</span></span> <span data-ttu-id="d358a-129">Sınıf Kitaplığı arama uygulamanın kullandığı aynı bağlantı dizesine kullanmasını sağlamak için bir strateji yürütmelidir isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d358a-129">You'll want to implement a strategy to ensure that the class library uses the same connection string that the calling application uses.</span></span> <span data-ttu-id="d358a-130">Örneğin, bir ortam değişkeninden hem projelerde değer alabilir.</span><span class="sxs-lookup"><span data-stu-id="d358a-130">For example, you could get the value from an environment variable in both projects.</span></span>

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a><span data-ttu-id="d358a-131">ASP.NET Core projesi bağımlılık ekleme ayarlama</span><span class="sxs-lookup"><span data-stu-id="d358a-131">Set up dependency injection in the ASP.NET Core project</span></span>

<span data-ttu-id="d358a-132">Temel projenin *Startup.cs* EF6 bağlamı için bağımlılık ekleme (dı) kümesinde dosya `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d358a-132">In the Core project's *Startup.cs* file, set up the EF6 context for dependency injection (DI) in `ConfigureServices`.</span></span> <span data-ttu-id="d358a-133">EF bağlam nesneleri için bir istek başına ömrü kapsamında olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d358a-133">EF context objects should be scoped for a per-request lifetime.</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

<span data-ttu-id="d358a-134">Ardından bağlam örneğini, DI kullanarak denetleyicilerinizi alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d358a-134">You can then get an instance of the context in your controllers by using DI.</span></span> <span data-ttu-id="d358a-135">Kod, bir EF Core bağlamının ne yazmalısınız için benzer:</span><span class="sxs-lookup"><span data-stu-id="d358a-135">The code is similar to what you'd write for an EF Core context:</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a><span data-ttu-id="d358a-136">Örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="d358a-136">Sample application</span></span>

<span data-ttu-id="d358a-137">Çalışan bir örnek uygulama için bkz. [örnek Visual Studio çözümü](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) bu makalede, eşlik.</span><span class="sxs-lookup"><span data-stu-id="d358a-137">For a working sample application, see the [sample Visual Studio solution](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) that accompanies this article.</span></span>

<span data-ttu-id="d358a-138">Bu örnek, aşağıdaki adımlarda Visual Studio tarafından sıfırdan oluşturulabilir:</span><span class="sxs-lookup"><span data-stu-id="d358a-138">This sample can be created from scratch by the following steps in Visual Studio:</span></span>

* <span data-ttu-id="d358a-139">Bir çözüm oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d358a-139">Create a solution.</span></span>

* <span data-ttu-id="d358a-140">**Ekleme** > **yeni proje** > **Web** > **ASP.NET Core Web uygulaması**</span><span class="sxs-lookup"><span data-stu-id="d358a-140">**Add** > **New Project** > **Web** > **ASP.NET Core Web Application**</span></span>
  * <span data-ttu-id="d358a-141">Proje şablonu seçimi iletişim kutusunda, açılır menüde API ve .NET Framework'ü seçin</span><span class="sxs-lookup"><span data-stu-id="d358a-141">In project template selection dialog, select API and .NET Framework in dropdown</span></span>

* <span data-ttu-id="d358a-142">**Ekleme** > **yeni proje** > **Windows Masaüstü** > **sınıf kitaplığı (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="d358a-142">**Add** > **New Project** > **Windows Desktop** > **Class Library (.NET Framework)**</span></span>

* <span data-ttu-id="d358a-143">İçinde **Paket Yöneticisi Konsolu** (PMC) iki proje için komutu çalıştırmak `Install-Package Entityframework`.</span><span class="sxs-lookup"><span data-stu-id="d358a-143">In **Package Manager Console** (PMC) for both projects, run the command `Install-Package Entityframework`.</span></span>

* <span data-ttu-id="d358a-144">Sınıf kitaplığı projesinde veri modeli sınıfları ve bağlam sınıfını ve uygulaması oluşturma `IDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="d358a-144">In the class library project, create data model classes and a context class, and an implementation of `IDbContextFactory`.</span></span>

* <span data-ttu-id="d358a-145">Sınıf kitaplığı projesi için PMC'de komutları çalıştırmak `Enable-Migrations` ve `Add-Migration Initial`.</span><span class="sxs-lookup"><span data-stu-id="d358a-145">In PMC for the class library project, run the commands `Enable-Migrations` and `Add-Migration Initial`.</span></span> <span data-ttu-id="d358a-146">ASP.NET Core projesi başlangıç projesi olarak ayarladıysanız, ekleme `-StartupProjectName EF6` bu komutların.</span><span class="sxs-lookup"><span data-stu-id="d358a-146">If you have set the ASP.NET Core project as the startup project, add `-StartupProjectName EF6` to these commands.</span></span>

* <span data-ttu-id="d358a-147">Core projesinde sınıf kitaplığı projesine bir proje başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d358a-147">In the Core project, add a project reference to the class library project.</span></span>

* <span data-ttu-id="d358a-148">Core projesinde içinde *Startup.cs*, bağlam DI için kaydolun.</span><span class="sxs-lookup"><span data-stu-id="d358a-148">In the Core project, in *Startup.cs*, register the context for DI.</span></span>

* <span data-ttu-id="d358a-149">Core projesinde içinde *appsettings.json*, bağlantı dizesi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d358a-149">In the Core project, in *appsettings.json*, add the connection string.</span></span>

* <span data-ttu-id="d358a-150">Bir denetleyici ve okuma ve yazma veri doğrulamak için görünümleri çekirdek proje ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d358a-150">In the Core project, add a controller and view(s) to verify that you can read and write data.</span></span> <span data-ttu-id="d358a-151">(Unutmayın. ASP.NET Core MVC yapı iskelesi Sınıf Kitaplığı'ndan başvurulan EF6 bağlamı ile çalışmaz.)</span><span class="sxs-lookup"><span data-stu-id="d358a-151">(Note that ASP.NET Core MVC scaffolding won't work with the EF6 context referenced from the class library.)</span></span>

## <a name="summary"></a><span data-ttu-id="d358a-152">Özet</span><span class="sxs-lookup"><span data-stu-id="d358a-152">Summary</span></span>

<span data-ttu-id="d358a-153">Bu makalede, bir ASP.NET Core uygulaması'nda Entity Framework 6 kullanma için temel kılavuz hazırladı.</span><span class="sxs-lookup"><span data-stu-id="d358a-153">This article has provided basic guidance for using Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d358a-154">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d358a-154">Additional resources</span></span>

* [<span data-ttu-id="d358a-155">Entity Framework - kod tabanlı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d358a-155">Entity Framework - Code-Based Configuration</span></span>](https://msdn.microsoft.com/data/jj680699.aspx)
