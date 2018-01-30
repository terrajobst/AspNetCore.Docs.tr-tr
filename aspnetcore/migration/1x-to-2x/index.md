---
title: "ASP.NET çekirdek geçirme 1.x 2.0"
author: scottaddie
description: "Bu makalede, bir ASP.NET Core 1.x proje için ASP.NET Core 2.0 geçirme en yaygın adımları ve önkoşullar özetlenmektedir."
manager: wpickett
ms.author: scaddie
ms.date: 10/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/1x-to-2x/index
ms.openlocfilehash: a88d22c88689d20376fec748b05fc4b5ecca3510
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="migrating-from-aspnet-core-1x-to-aspnet-core-20"></a><span data-ttu-id="8b51a-103">ASP.NET çekirdek geçirme 1.x ASP.NET Core 2.0 için</span><span class="sxs-lookup"><span data-stu-id="8b51a-103">Migrating from ASP.NET Core 1.x to ASP.NET Core 2.0</span></span>

<span data-ttu-id="8b51a-104">Tarafından [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="8b51a-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="8b51a-105">Bu makalede, sizi, ASP.NET Core 2.0 için mevcut bir ASP.NET Core 1.x projesini güncelleştirme ile gösterilecektir.</span><span class="sxs-lookup"><span data-stu-id="8b51a-105">In this article, we'll walk you through updating an existing ASP.NET Core 1.x project to ASP.NET Core 2.0.</span></span> <span data-ttu-id="8b51a-106">ASP.NET Core 2.0 uygulamanıza geçirme sağlar yararlanmak [birçok yeni özellik ve performans iyileştirmeleri](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="8b51a-106">Migrating your application to ASP.NET Core 2.0 enables you to take advantage of [many new features and performance improvements](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0).</span></span> 

<span data-ttu-id="8b51a-107">Mevcut ASP.NET Core 1.x uygulamaları sürüme özgü proje şablonları dışına temel alır.</span><span class="sxs-lookup"><span data-stu-id="8b51a-107">Existing ASP.NET Core 1.x applications are based off of version-specific project templates.</span></span> <span data-ttu-id="8b51a-108">Bu nedenle ASP.NET Core framework geliştikçe proje şablonları ve içerdikleri Başlatıcı kodu yapın.</span><span class="sxs-lookup"><span data-stu-id="8b51a-108">As the ASP.NET Core framework evolves, so do the project templates and the starter code contained within them.</span></span> <span data-ttu-id="8b51a-109">ASP.NET Core framework güncelleştirmeye ek olarak, uygulamanız için kod güncelleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="8b51a-109">In addition to updating the ASP.NET Core framework, you need to update the code for your application.</span></span>

<a name="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="8b51a-110">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="8b51a-110">Prerequisites</span></span>
<span data-ttu-id="8b51a-111">Lütfen bakın [ASP.NET Core ile çalışmaya başlama](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="8b51a-111">Please see [Getting Started with ASP.NET Core](xref:getting-started).</span></span>

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a><span data-ttu-id="8b51a-112">Hedef Framework ad (TFM) güncelleştir</span><span class="sxs-lookup"><span data-stu-id="8b51a-112">Update Target Framework Moniker (TFM)</span></span>
<span data-ttu-id="8b51a-113">Proje .NET Core hedefleme kullanması gereken [TFM](/dotnet/standard/frameworks#referring-to-frameworks) .NET Core 2.0 eşit veya daha büyük bir sürümü.</span><span class="sxs-lookup"><span data-stu-id="8b51a-113">Projects targeting .NET Core should use the [TFM](/dotnet/standard/frameworks#referring-to-frameworks) of a version greater than or equal to .NET Core 2.0.</span></span> <span data-ttu-id="8b51a-114">Arama `<TargetFramework>` düğümünde *.csproj* dosya ve kendi iç metinle `netcoreapp2.0`:</span><span class="sxs-lookup"><span data-stu-id="8b51a-114">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `netcoreapp2.0`:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=3)]

<span data-ttu-id="8b51a-115">Proje .NET Framework hedefleme .NET Framework 4.6.1 eşit veya üzeri bir sürümü TFM kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="8b51a-115">Projects targeting .NET Framework should use the TFM of a version greater than or equal to .NET Framework 4.6.1.</span></span> <span data-ttu-id="8b51a-116">Arama `<TargetFramework>` düğümünde *.csproj* dosya ve kendi iç metinle `net461`:</span><span class="sxs-lookup"><span data-stu-id="8b51a-116">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `net461`:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> <span data-ttu-id="8b51a-117">.NET core 2.0 sunan bir kadar büyük yüzey alanını .NET Core daha 1.x.</span><span class="sxs-lookup"><span data-stu-id="8b51a-117">.NET Core 2.0 offers a much larger surface area than .NET Core 1.x.</span></span> <span data-ttu-id="8b51a-118">Yalnızca .NET .NET Core 2.0 hedefleme 1.x çalışması olasıdır Çekirdek API'leri eksik nedeniyle .NET Framework hedefleme durumunda.</span><span class="sxs-lookup"><span data-stu-id="8b51a-118">If you're targeting .NET Framework solely because of missing APIs in .NET Core 1.x, targeting .NET Core 2.0 is likely to work.</span></span>

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a><span data-ttu-id="8b51a-119">.NET Core SDK sürümünde global.json güncelleştir</span><span class="sxs-lookup"><span data-stu-id="8b51a-119">Update .NET Core SDK version in global.json</span></span>
<span data-ttu-id="8b51a-120">Çözümünüzü bağlı dayalıysa bir [ *global.json* ](https://docs.microsoft.com/dotnet/core/tools/global-json) belirli bir .NET Core SDK sürümü hedeflemek için güncelleştirme dosyası, `version` özelliğinin makinenize yüklü 2.0 sürümü kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="8b51a-120">If your solution relies upon a [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) file to target a specific .NET Core SDK version, update its `version` property to use the 2.0 version installed on your machine:</span></span>

[!code-json[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a><span data-ttu-id="8b51a-121">Güncelleştirme paketi başvuruları</span><span class="sxs-lookup"><span data-stu-id="8b51a-121">Update package references</span></span>
<span data-ttu-id="8b51a-122">*.Csproj* 1.x proje dosyasında projesi tarafından kullanılan her NuGet paketini listeler.</span><span class="sxs-lookup"><span data-stu-id="8b51a-122">The *.csproj* file in a 1.x project lists each NuGet package used by the project.</span></span>

<span data-ttu-id="8b51a-123">.NET Core 2.0, tek bir hedefleme ASP.NET Core 2.0 projesinde [metapackage](xref:fundamentals/metapackage) başvuru *.csproj* paketleri koleksiyonunu yerini alır:</span><span class="sxs-lookup"><span data-stu-id="8b51a-123">In an ASP.NET Core 2.0 project targeting .NET Core 2.0, a single [metapackage](xref:fundamentals/metapackage) reference in the *.csproj* file replaces the collection of packages:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=8-10)]

<span data-ttu-id="8b51a-124">ASP.NET Core 2.0 ve Entity Framework Çekirdek 2. 0'in tüm özelliklerini metapackage dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="8b51a-124">All the features of ASP.NET Core 2.0 and Entity Framework Core 2.0 are included in the metapackage.</span></span>

<span data-ttu-id="8b51a-125">ASP.NET Core 2.0 proje .NET Framework hedefleme tek tek NuGet paketlerini başvurmaya devam etmelidir.</span><span class="sxs-lookup"><span data-stu-id="8b51a-125">ASP.NET Core 2.0 projects targeting .NET Framework should continue to reference individual NuGet packages.</span></span> <span data-ttu-id="8b51a-126">Güncelleştirme `Version` her özniteliği `<PackageReference />` 2.0.0 düğüme.</span><span class="sxs-lookup"><span data-stu-id="8b51a-126">Update the `Version` attribute of each `<PackageReference />` node to 2.0.0.</span></span>

<span data-ttu-id="8b51a-127">Örneğin, listesi aşağıdadır `<PackageReference />` .NET Framework'ü hedefleme tipik bir ASP.NET Core 2.0 projesinde kullanılan düğümleri:</span><span class="sxs-lookup"><span data-stu-id="8b51a-127">For example, here's the list of `<PackageReference />` nodes used in a typical ASP.NET Core 2.0 project targeting .NET Framework:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a><span data-ttu-id="8b51a-128">.NET Core CLI araçlarını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="8b51a-128">Update .NET Core CLI tools</span></span>
<span data-ttu-id="8b51a-129">İçinde *.csproj* dosya, güncelleştirme `Version` her özniteliği `<DotNetCliToolReference />` 2.0.0 düğüme.</span><span class="sxs-lookup"><span data-stu-id="8b51a-129">In the *.csproj* file, update the `Version` attribute of each `<DotNetCliToolReference />` node to 2.0.0.</span></span>

<span data-ttu-id="8b51a-130">Örneğin, .NET Core 2.0 hedefleme tipik bir ASP.NET Core 2.0 projesinde kullanılan CLI araçlarını listesi aşağıdadır:</span><span class="sxs-lookup"><span data-stu-id="8b51a-130">For example, here's the list of CLI tools used in a typical ASP.NET Core 2.0 project targeting .NET Core 2.0:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=12-16)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a><span data-ttu-id="8b51a-131">Paketi hedef geri dönüş özelliği yeniden adlandırma</span><span class="sxs-lookup"><span data-stu-id="8b51a-131">Rename Package Target Fallback property</span></span>
<span data-ttu-id="8b51a-132">*.Csproj* kullanılan 1.x projenin dosya bir `PackageTargetFallback` düğümü ve değişken:</span><span class="sxs-lookup"><span data-stu-id="8b51a-132">The *.csproj* file of a 1.x project used a `PackageTargetFallback` node and variable:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=5)]

<span data-ttu-id="8b51a-133">Düğüm ve değişkenine yeniden adlandırmak `AssetTargetFallback`:</span><span class="sxs-lookup"><span data-stu-id="8b51a-133">Rename both the node and variable to `AssetTargetFallback`:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=4)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a><span data-ttu-id="8b51a-134">Program.cs içinde güncelleştirme Main yöntemi</span><span class="sxs-lookup"><span data-stu-id="8b51a-134">Update Main method in Program.cs</span></span>
<span data-ttu-id="8b51a-135">1.x projelerinde `Main` yöntemi *Program.cs* şöyle Aranan:</span><span class="sxs-lookup"><span data-stu-id="8b51a-135">In 1.x projects, the `Main` method of *Program.cs* looked like this:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

<span data-ttu-id="8b51a-136">2.0 projelerinde `Main` yöntemi *Program.cs* basitleştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="8b51a-136">In 2.0 projects, the `Main` method of *Program.cs* has been simplified:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program.cs?highlight=8-11)]

<span data-ttu-id="8b51a-137">Bu yeni 2.0 deseni benimsenmesi önerilir ve gibi ürün özellikleri için gereklidir [Entity Framework (EF) çekirdek geçişler](xref:data/ef-mvc/migrations) çalışmak için.</span><span class="sxs-lookup"><span data-stu-id="8b51a-137">The adoption of this new 2.0 pattern is highly recommended and is required for product features like [Entity Framework (EF) Core Migrations](xref:data/ef-mvc/migrations) to work.</span></span> <span data-ttu-id="8b51a-138">Örneğin, çalışan `Update-Database` Paket Yöneticisi konsolu penceresinden veya `dotnet ef database update` komuttan satırda (Projeler) ASP.NET Core 2.0 dönüştürülen aşağıdaki hata oluşturur:</span><span class="sxs-lookup"><span data-stu-id="8b51a-138">For example, running `Update-Database` from the Package Manager Console window or `dotnet ef database update` from the command line (on projects converted to ASP.NET Core 2.0) generates the following error:</span></span>

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="add-modify-configuration"></a>

## <a name="add-configuration-providers"></a><span data-ttu-id="8b51a-139">Yapılandırma Sağlayıcıları Ekle</span><span class="sxs-lookup"><span data-stu-id="8b51a-139">Add configuration providers</span></span>
<span data-ttu-id="8b51a-140">1.x projelerinde yapılandırma sağlayıcısı için uygulama ekleme aracılığıyla gerçekleştirilmiştir `Startup` Oluşturucusu.</span><span class="sxs-lookup"><span data-stu-id="8b51a-140">In 1.x projects, adding configuration providers to an app was accomplished via the `Startup` constructor.</span></span> <span data-ttu-id="8b51a-141">Örneği oluşturma adımları dahil `ConfigurationBuilder`geçerli sağlayıcıları (ortam değişkenleri, uygulama ayarları, vb.) yükleme ve üyesi başlatma `IConfigurationRoot`.</span><span class="sxs-lookup"><span data-stu-id="8b51a-141">The steps involved creating an instance of `ConfigurationBuilder`, loading applicable providers (environment variables, app settings, etc.), and initializing a member of `IConfigurationRoot`.</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_1xStartup)]

<span data-ttu-id="8b51a-142">Önceki örnekte yükler `Configuration` yapılandırma ayarlarından üyesiyle *appsettings.json* herhangi yanı sıra *appsettings.\< EnvironmentName\>.json* dosya eşleşen `IHostingEnvironment.EnvironmentName` özelliği.</span><span class="sxs-lookup"><span data-stu-id="8b51a-142">The preceding example loads the `Configuration` member with configuration settings from *appsettings.json* as well as any *appsettings.\<EnvironmentName\>.json* file matching the `IHostingEnvironment.EnvironmentName` property.</span></span> <span data-ttu-id="8b51a-143">Aynı yol bu dosyalarının konumunu altındadır *haline*.</span><span class="sxs-lookup"><span data-stu-id="8b51a-143">The location of these files is at the same path as *Startup.cs*.</span></span>

<span data-ttu-id="8b51a-144">2.0 projelerinde 1.x projelerine devralınmış Demirbaş yapılandırma kodu Perde Arkası çalışır.</span><span class="sxs-lookup"><span data-stu-id="8b51a-144">In 2.0 projects, the boilerplate configuration code inherent to 1.x projects runs behind-the-scenes.</span></span> <span data-ttu-id="8b51a-145">Örneğin, ortam değişkenleri ve uygulama ayarlarını başlangıçta yüklenir.</span><span class="sxs-lookup"><span data-stu-id="8b51a-145">For example, environment variables and app settings are loaded at startup.</span></span> <span data-ttu-id="8b51a-146">Eşdeğer *haline* kodu daha az `IConfiguration` eklenen örneği ile başlatma:</span><span class="sxs-lookup"><span data-stu-id="8b51a-146">The equivalent *Startup.cs* code is reduced to `IConfiguration` initialization with the injected instance:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Startup.cs?name=snippet_2xStartup)]

<span data-ttu-id="8b51a-147">Tarafından eklenen varsayılan sağlayıcı kaldırmak için `WebHostBuilder.CreateDefaultBuilder`, çağırma `Clear` yöntemi `IConfigurationBuilder.Sources` özelliği içine `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="8b51a-147">To remove the default providers added by `WebHostBuilder.CreateDefaultBuilder`, invoke the `Clear` method on the `IConfigurationBuilder.Sources` property inside of `ConfigureAppConfiguration`.</span></span> <span data-ttu-id="8b51a-148">Geri sağlayıcıları eklemek için kullanan `ConfigureAppConfiguration` yönteminde *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="8b51a-148">To add providers back, utilize the `ConfigureAppConfiguration` method in *Program.cs*:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Program.cs?name=snippet_ProgramMainConfigProviders&highlight=9-14)]

<span data-ttu-id="8b51a-149">Tarafından kullanılan yapılandırma `CreateDefaultBuilder` önceki kod parçacığını yönteminde görülme [burada](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="8b51a-149">The configuration used by the `CreateDefaultBuilder` method in the preceding code snippet can be seen [here](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).</span></span>

<span data-ttu-id="8b51a-150">Daha fazla bilgi için bkz: [ASP.NET Core yapılandırmasında](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="8b51a-150">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

<a name="db-init-code"></a>

## <a name="move-database-initialization-code"></a><span data-ttu-id="8b51a-151">Veritabanı başlatma kodu taşıyın</span><span class="sxs-lookup"><span data-stu-id="8b51a-151">Move database initialization code</span></span>
<span data-ttu-id="8b51a-152">EF çekirdek kullanarak 1.x projelerinde 1.x, gibi bir komutun `dotnet ef migrations add` şunları yapar:</span><span class="sxs-lookup"><span data-stu-id="8b51a-152">In 1.x projects using EF Core 1.x, a command such as `dotnet ef migrations add` does the following:</span></span>
1. <span data-ttu-id="8b51a-153">Başlatır bir `Startup` örneği</span><span class="sxs-lookup"><span data-stu-id="8b51a-153">Instantiates a `Startup` instance</span></span>
2. <span data-ttu-id="8b51a-154">Çağırır `ConfigureServices` tüm hizmetleri bağımlılık ekleme ile kaydetmek için yöntemi (de dahil olmak üzere `DbContext` türleri)</span><span class="sxs-lookup"><span data-stu-id="8b51a-154">Invokes the `ConfigureServices` method to register all services with dependency injection (including `DbContext` types)</span></span>
3. <span data-ttu-id="8b51a-155">Gerekli görevleri gerçekleştirir</span><span class="sxs-lookup"><span data-stu-id="8b51a-155">Performs its requisite tasks</span></span>

<span data-ttu-id="8b51a-156">EF çekirdek 2.0 kullanan 2.0 projelerinde `Program.BuildWebHost` uygulama hizmetlerini almak için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="8b51a-156">In 2.0 projects using EF Core 2.0, `Program.BuildWebHost` is invoked to obtain the application services.</span></span> <span data-ttu-id="8b51a-157">1.x, bu ek yan çağırma etkisi `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="8b51a-157">Unlike 1.x, this has the additional side effect of invoking `Startup.Configure`.</span></span> <span data-ttu-id="8b51a-158">1.x uygulamanızı veritabanı başlatma kodda başlatılırsa, `Configure` yöntemi, beklenmeyen sorunlar oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="8b51a-158">If your 1.x app invoked database initialization code in its `Configure` method, unexpected problems can occur.</span></span> <span data-ttu-id="8b51a-159">Örneğin, veritabanı henüz yoksa, dengeli dağıtım kodu EF çekirdek geçişler komut yürütme önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="8b51a-159">For example, if the database doesn't yet exist, the seeding code runs before the EF Core Migrations command execution.</span></span> <span data-ttu-id="8b51a-160">Bu soruna neden olan bir `dotnet ef migrations list` veritabanı henüz yoksa başarısız komutu.</span><span class="sxs-lookup"><span data-stu-id="8b51a-160">This problem causes a `dotnet ef migrations list` command to fail if the database doesn't yet exist.</span></span>

<span data-ttu-id="8b51a-161">Aşağıdaki 1.x çekirdek başlatma kodda göz önünde bulundurun `Configure` yöntemi *haline*:</span><span class="sxs-lookup"><span data-stu-id="8b51a-161">Consider the following 1.x seed initialization code in the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_ConfigureSeedData&highlight=8)]

<span data-ttu-id="8b51a-162">2.0 projelerinde taşıma `SeedData.Initialize` çağrısı `Main` yöntemi *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="8b51a-162">In 2.0 projects, move the `SeedData.Initialize` call to the `Main` method of *Program.cs*:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program2.cs?name=snippet_Main2Code&highlight=10)]

<span data-ttu-id="8b51a-163">2.0 sürümünden itibaren onu bir şey yapmanız hatalı deneyimdir `BuildWebHost` dışındaki derleme ve web ana bilgisayarı yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="8b51a-163">As of 2.0, it's bad practice to do anything in `BuildWebHost` except build and configure the web host.</span></span> <span data-ttu-id="8b51a-164">Uygulamayı çalıştırma hakkında olan her şeyi dışında işlenmesi gereken `BuildWebHost` &mdash; genellikle içinde `Main` yöntemi *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="8b51a-164">Anything that's about running the application should be handled outside of `BuildWebHost` &mdash; typically in the `Main` method of *Program.cs*.</span></span>

<a name="view-compilation"></a>

## <a name="review-razor-view-compilation-setting"></a><span data-ttu-id="8b51a-165">Razor görünüm derleme ayarları gözden geçirin</span><span class="sxs-lookup"><span data-stu-id="8b51a-165">Review Razor view compilation setting</span></span>
<span data-ttu-id="8b51a-166">Daha hızlı uygulama başlangıç zamanı ve daha küçük yayımlanan paket sizin için utmost öneme sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="8b51a-166">Faster application startup time and smaller published bundles are of utmost importance to you.</span></span> <span data-ttu-id="8b51a-167">Bu nedenlerle, [Razor görünüm derleme](xref:mvc/views/view-compilation) ASP.NET Core 2.0 varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="8b51a-167">For these reasons, [Razor view compilation](xref:mvc/views/view-compilation) is enabled by default in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="8b51a-168">Ayarı `MvcRazorCompileOnPublish` true özelliktir artık gerekli.</span><span class="sxs-lookup"><span data-stu-id="8b51a-168">Setting the `MvcRazorCompileOnPublish` property to true is no longer required.</span></span> <span data-ttu-id="8b51a-169">Öğesinden Görünüm derleme devre dışı bırakma sürece özellik kaldırılabilir *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="8b51a-169">Unless you're disabling view compilation, the property may be removed from the *.csproj* file.</span></span>

<span data-ttu-id="8b51a-170">.NET Framework hedeflerken yine açıkça başvuru gerekir [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet paketi, *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="8b51a-170">When targeting .NET Framework, you still need to explicitly reference the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet package in your *.csproj* file:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a><span data-ttu-id="8b51a-171">Application Insights "ışık li" özellikleri kullanır</span><span class="sxs-lookup"><span data-stu-id="8b51a-171">Rely on Application Insights "light-up" features</span></span>
<span data-ttu-id="8b51a-172">Uygulama performansı izleme zahmetsiz Kurulumu önemlidir.</span><span class="sxs-lookup"><span data-stu-id="8b51a-172">Effortless setup of application performance instrumentation is important.</span></span> <span data-ttu-id="8b51a-173">Şimdi yeni güvenebilirsiniz [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "ışık li" özellikleri Visual Studio 2017 araç oluşturmada kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8b51a-173">You can now rely on the new [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "light-up" features available in the Visual Studio 2017 tooling.</span></span>

<span data-ttu-id="8b51a-174">Visual Studio 2017 içinde oluşturulan ASP.NET Core 1.1 projeleri, varsayılan olarak Application Insights eklendi.</span><span class="sxs-lookup"><span data-stu-id="8b51a-174">ASP.NET Core 1.1 projects created in Visual Studio 2017 added Application Insights by default.</span></span> <span data-ttu-id="8b51a-175">Application Insights SDK'sı, doğrudan kullanmıyorsanız dışında *Program.cs* ve *haline*, şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="8b51a-175">If you're not using the Application Insights SDK directly, outside of *Program.cs* and *Startup.cs*, follow these steps:</span></span>

1. <span data-ttu-id="8b51a-176">.NET Core hedefleme, aşağıdaki kaldırmanız `<PackageReference />` düğümden *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="8b51a-176">If targeting .NET Core, remove the following `<PackageReference />` node from the *.csproj* file:</span></span>
    
    [!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=10)]

2. <span data-ttu-id="8b51a-177">.NET Core hedefleme, kaldırmanız `UseApplicationInsights` uzantısı yöntemi çağrısından *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="8b51a-177">If targeting .NET Core, remove the `UseApplicationInsights` extension method invocation from *Program.cs*:</span></span>

    [!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. <span data-ttu-id="8b51a-178">Application Insights istemci-tarafı API çağrısından kaldırmak *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8b51a-178">Remove the Application Insights client-side API call from *_Layout.cshtml*.</span></span> <span data-ttu-id="8b51a-179">Aşağıdaki iki kod satırı oluşur:</span><span class="sxs-lookup"><span data-stu-id="8b51a-179">It comprises the following two lines of code:</span></span>

    [!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Shared/_Layout.cshtml?range=1,19&dedent=4)]

<span data-ttu-id="8b51a-180">Application Insights SDK'sı doğrudan kullanıyorsanız, bunu yapmak devam edin.</span><span class="sxs-lookup"><span data-stu-id="8b51a-180">If you are using the Application Insights SDK directly, continue to do so.</span></span> <span data-ttu-id="8b51a-181">2.0 [metapackage](xref:fundamentals/metapackage) Application Insights'ın en son sürümünü içerir, böylece daha eski bir sürümüne başvuruyor paket indirgeme hatası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8b51a-181">The 2.0 [metapackage](xref:fundamentals/metapackage) includes the latest version of Application Insights, so a package downgrade error appears if you're referencing an older version.</span></span>

<a name="auth-and-identity"></a>

## <a name="adopt-authenticationidentity-improvements"></a><span data-ttu-id="8b51a-182">Kimlik doğrulamasının iyileştirmeler benimsemeyi</span><span class="sxs-lookup"><span data-stu-id="8b51a-182">Adopt authentication/Identity improvements</span></span>
<span data-ttu-id="8b51a-183">ASP.NET Core 2.0 yeni bir kimlik doğrulama modeli ve ASP.NET Core kimliği önemli bir değişiklik sayısı vardır.</span><span class="sxs-lookup"><span data-stu-id="8b51a-183">ASP.NET Core 2.0 has a new authentication model and a number of significant changes to ASP.NET Core Identity.</span></span> <span data-ttu-id="8b51a-184">Projenizi etkin bireysel kullanıcı hesapları ile oluşturulan ya da kimlik doğrulama ve kimlik, el ile eklediyseniz bkz [geçirme kimlik doğrulaması ve ASP.NET Core 2.0 kimliğine](xref:migration/1x-to-2x/identity-2x).</span><span class="sxs-lookup"><span data-stu-id="8b51a-184">If you created your project with Individual User Accounts enabled, or if you have manually added authentication or Identity, see [Migrating Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8b51a-185">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="8b51a-185">Additional resources</span></span>

* [<span data-ttu-id="8b51a-186">ASP.NET Core 2.0 yeni değişiklikler</span><span class="sxs-lookup"><span data-stu-id="8b51a-186">Breaking Changes in ASP.NET Core 2.0</span></span>](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
