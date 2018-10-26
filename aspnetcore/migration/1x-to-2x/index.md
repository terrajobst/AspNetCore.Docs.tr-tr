---
title: ASP.NET Core geçiş 1.x sürümünden 2.0 sürümüne
author: scottaddie
description: Bu makalede, önkoşulları ve ASP.NET Core 2.0 için bir ASP.NET Core 1.x projesi geçirmek için en yaygın özetlenmektedir.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/24/2018
uid: migration/1x-to-2x/index
ms.openlocfilehash: f5bd2bc9862a7487658125e14837798886efad11
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090536"
---
# <a name="migrate-from-aspnet-core-1x-to-20"></a><span data-ttu-id="cedfd-103">ASP.NET Core geçiş 1.x sürümünden 2.0 sürümüne</span><span class="sxs-lookup"><span data-stu-id="cedfd-103">Migrate from ASP.NET Core 1.x to 2.0</span></span>

<span data-ttu-id="cedfd-104">Tarafından [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="cedfd-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="cedfd-105">Bu makalede, ASP.NET Core 2.0 için mevcut bir ASP.NET Core 1.x projesi güncelleştirme ile inceleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="cedfd-105">In this article, we walk you through updating an existing ASP.NET Core 1.x project to ASP.NET Core 2.0.</span></span> <span data-ttu-id="cedfd-106">ASP.NET Core 2.0 uygulamanızı geçişi avantajlarından yararlanmanıza olanak tanır [birçok yeni özellikler ve performans iyileştirmeleri](xref:aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="cedfd-106">Migrating your application to ASP.NET Core 2.0 enables you to take advantage of [many new features and performance improvements](xref:aspnetcore-2.0).</span></span>

<span data-ttu-id="cedfd-107">ASP.NET Core 1.x uygulamalara dışına sürüme özgü proje şablonları temel alır.</span><span class="sxs-lookup"><span data-stu-id="cedfd-107">Existing ASP.NET Core 1.x applications are based off of version-specific project templates.</span></span> <span data-ttu-id="cedfd-108">ASP.NET Core framework geliştikçe, bu nedenle proje şablonları ve içerdiği Başlatıcı kodunu yapın.</span><span class="sxs-lookup"><span data-stu-id="cedfd-108">As the ASP.NET Core framework evolves, so do the project templates and the starter code contained within them.</span></span> <span data-ttu-id="cedfd-109">ASP.NET Core framework güncelleştirmeye ek olarak, uygulamanız için kodu güncelleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="cedfd-109">In addition to updating the ASP.NET Core framework, you need to update the code for your application.</span></span>

<a name="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="cedfd-110">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="cedfd-110">Prerequisites</span></span>

<span data-ttu-id="cedfd-111">Bkz: [ASP.NET Core ile çalışmaya başlama](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="cedfd-111">See [Get Started with ASP.NET Core](xref:getting-started).</span></span>

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a><span data-ttu-id="cedfd-112">Hedef Çerçeve adı (TFM) güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="cedfd-112">Update Target Framework Moniker (TFM)</span></span>

<span data-ttu-id="cedfd-113">.NET Core'u hedefleyen projeler kullanması gereken [TFM](/dotnet/standard/frameworks#referring-to-frameworks) büyüktür veya eşittir .NET Core 2.0 sürümü.</span><span class="sxs-lookup"><span data-stu-id="cedfd-113">Projects targeting .NET Core should use the [TFM](/dotnet/standard/frameworks#referring-to-frameworks) of a version greater than or equal to .NET Core 2.0.</span></span> <span data-ttu-id="cedfd-114">Arama `<TargetFramework>` düğümünde *.csproj* dosya ve kendi iç metinle `netcoreapp2.0`:</span><span class="sxs-lookup"><span data-stu-id="cedfd-114">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `netcoreapp2.0`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=3)]

<span data-ttu-id="cedfd-115">.NET Framework'ü hedefleyen projeleri TFM büyüktür veya eşittir .NET Framework 4.6.1 sürümü kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="cedfd-115">Projects targeting .NET Framework should use the TFM of a version greater than or equal to .NET Framework 4.6.1.</span></span> <span data-ttu-id="cedfd-116">Arama `<TargetFramework>` düğümünde *.csproj* dosya ve kendi iç metinle `net461`:</span><span class="sxs-lookup"><span data-stu-id="cedfd-116">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `net461`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> <span data-ttu-id="cedfd-117">.NET core 2.0 sunan bir çok büyük yüzey alanını daha .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="cedfd-117">.NET Core 2.0 offers a much larger surface area than .NET Core 1.x.</span></span> <span data-ttu-id="cedfd-118">.NET Framework yalnızca .NET Core 1.x, .NET Core 2.0 hedefleyen çalışması olasıdır API'leri eksik hedefliyorsanız.</span><span class="sxs-lookup"><span data-stu-id="cedfd-118">If you're targeting .NET Framework solely because of missing APIs in .NET Core 1.x, targeting .NET Core 2.0 is likely to work.</span></span>

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a><span data-ttu-id="cedfd-119">.NET Core SDK'sı sürümünü global.json güncelleştirin</span><span class="sxs-lookup"><span data-stu-id="cedfd-119">Update .NET Core SDK version in global.json</span></span>

<span data-ttu-id="cedfd-120">Çözümünüze bağlı dayanıyorsa bir [ *global.json* ](/dotnet/core/tools/global-json) dosya belirli bir .NET Core SDK sürümünü hedeflemek, güncelleştirme, `version` özelliğinin makinenizde yüklü 2.0 sürümü kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="cedfd-120">If your solution relies upon a [*global.json*](/dotnet/core/tools/global-json) file to target a specific .NET Core SDK version, update its `version` property to use the 2.0 version installed on your machine:</span></span>

[!code-json[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a><span data-ttu-id="cedfd-121">Güncelleştirme paketi başvuruları</span><span class="sxs-lookup"><span data-stu-id="cedfd-121">Update package references</span></span>

<span data-ttu-id="cedfd-122">*.Csproj* 1.x proje dosyasında proje tarafından kullanılan her bir NuGet paketini listeler.</span><span class="sxs-lookup"><span data-stu-id="cedfd-122">The *.csproj* file in a 1.x project lists each NuGet package used by the project.</span></span>

<span data-ttu-id="cedfd-123">.NET Core 2.0, tek bir hedefleyen ASP.NET Core 2.0 projesinde [metapackage](xref:fundamentals/metapackage) başvuru *.csproj* paketler koleksiyonunu yerini alır:</span><span class="sxs-lookup"><span data-stu-id="cedfd-123">In an ASP.NET Core 2.0 project targeting .NET Core 2.0, a single [metapackage](xref:fundamentals/metapackage) reference in the *.csproj* file replaces the collection of packages:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=8-10)]

<span data-ttu-id="cedfd-124">ASP.NET Core 2.0 ve Entity Framework Core 2.0 tüm özelliklerini metapackage dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="cedfd-124">All the features of ASP.NET Core 2.0 and Entity Framework Core 2.0 are included in the metapackage.</span></span>

<span data-ttu-id="cedfd-125">.NET Framework'ü hedefleyen ASP.NET Core 2.0 projeleri NuGet paketlerini tek tek başvurmak devam etmelidir.</span><span class="sxs-lookup"><span data-stu-id="cedfd-125">ASP.NET Core 2.0 projects targeting .NET Framework should continue to reference individual NuGet packages.</span></span> <span data-ttu-id="cedfd-126">Güncelleştirme `Version` her öznitelik `<PackageReference />` 2.0.0 düğümü.</span><span class="sxs-lookup"><span data-stu-id="cedfd-126">Update the `Version` attribute of each `<PackageReference />` node to 2.0.0.</span></span>

<span data-ttu-id="cedfd-127">Örneğin, listesi sunulmaktadır `<PackageReference />` .NET Framework'ü hedefleyen tipik bir ASP.NET Core 2.0 proje içinde kullanılan düğümler:</span><span class="sxs-lookup"><span data-stu-id="cedfd-127">For example, here's the list of `<PackageReference />` nodes used in a typical ASP.NET Core 2.0 project targeting .NET Framework:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a><span data-ttu-id="cedfd-128">.NET Core CLI araçları güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="cedfd-128">Update .NET Core CLI tools</span></span>

<span data-ttu-id="cedfd-129">İçinde *.csproj* dosyası, güncelleştirme `Version` her öznitelik `<DotNetCliToolReference />` 2.0.0 düğümü.</span><span class="sxs-lookup"><span data-stu-id="cedfd-129">In the *.csproj* file, update the `Version` attribute of each `<DotNetCliToolReference />` node to 2.0.0.</span></span>

<span data-ttu-id="cedfd-130">Örneğin, .NET Core 2.0 hedefleyen tipik bir ASP.NET Core 2.0 projesinde kullanılan CLI araçları listesi şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="cedfd-130">For example, here's the list of CLI tools used in a typical ASP.NET Core 2.0 project targeting .NET Core 2.0:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=12-16)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a><span data-ttu-id="cedfd-131">Paketin hedef düşürme özelliği yeniden adlandır</span><span class="sxs-lookup"><span data-stu-id="cedfd-131">Rename Package Target Fallback property</span></span>

<span data-ttu-id="cedfd-132">*.Csproj* kullanılan 1.x projenin dosya bir `PackageTargetFallback` düğüm ve değişkeni:</span><span class="sxs-lookup"><span data-stu-id="cedfd-132">The *.csproj* file of a 1.x project used a `PackageTargetFallback` node and variable:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=5)]

<span data-ttu-id="cedfd-133">Düğüm ve değişkeni Yeniden Adlandır `AssetTargetFallback`:</span><span class="sxs-lookup"><span data-stu-id="cedfd-133">Rename both the node and variable to `AssetTargetFallback`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=4)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a><span data-ttu-id="cedfd-134">Program.CS'de Webhostbuilder'a güncelleştirme Main yöntemi</span><span class="sxs-lookup"><span data-stu-id="cedfd-134">Update Main method in Program.cs</span></span>

<span data-ttu-id="cedfd-135">1.x projelerinde `Main` yöntemi *Program.cs* şöyle Aranan:</span><span class="sxs-lookup"><span data-stu-id="cedfd-135">In 1.x projects, the `Main` method of *Program.cs* looked like this:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

<span data-ttu-id="cedfd-136">2.0 projelerinde `Main` yöntemi *Program.cs* basitleştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="cedfd-136">In 2.0 projects, the `Main` method of *Program.cs* has been simplified:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program.cs?highlight=8-11)]

<span data-ttu-id="cedfd-137">Bu yeni 2.0 Düzen benimsenmesini önemle tavsiye edilir ve gibi ürün özellikleri için gereklidir [Entity Framework (EF) Code Migrations](xref:data/ef-mvc/migrations) çalışılır.</span><span class="sxs-lookup"><span data-stu-id="cedfd-137">The adoption of this new 2.0 pattern is highly recommended and is required for product features like [Entity Framework (EF) Core Migrations](xref:data/ef-mvc/migrations) to work.</span></span> <span data-ttu-id="cedfd-138">Örneğin, çalışan `Update-Database` Paket Yöneticisi konsolu penceresinde veya `dotnet ef database update` komut satırında (Projeler) için ASP.NET Core 2.0 dönüştürülür şu hata oluşturur:</span><span class="sxs-lookup"><span data-stu-id="cedfd-138">For example, running `Update-Database` from the Package Manager Console window or `dotnet ef database update` from the command line (on projects converted to ASP.NET Core 2.0) generates the following error:</span></span>

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="add-modify-configuration"></a>

## <a name="add-configuration-providers"></a><span data-ttu-id="cedfd-139">Yapılandırma Sağlayıcıları Ekle</span><span class="sxs-lookup"><span data-stu-id="cedfd-139">Add configuration providers</span></span>

<span data-ttu-id="cedfd-140">1.x projelerinde, bir yapılandırma sağlayıcısı bir uygulamaya ekleme yoluyla gerçekleştirilebilir `Startup` Oluşturucusu.</span><span class="sxs-lookup"><span data-stu-id="cedfd-140">In 1.x projects, adding configuration providers to an app was accomplished via the `Startup` constructor.</span></span> <span data-ttu-id="cedfd-141">Bir örneğini oluşturmaktan adımla daha uğraşmanız `ConfigurationBuilder`geçerli (ortam değişkenleri, uygulama ayarları, vb.) sağlayıcıları yükleniyor ve üye başlatma `IConfigurationRoot`.</span><span class="sxs-lookup"><span data-stu-id="cedfd-141">The steps involved creating an instance of `ConfigurationBuilder`, loading applicable providers (environment variables, app settings, etc.), and initializing a member of `IConfigurationRoot`.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_1xStartup)]

<span data-ttu-id="cedfd-142">Yukarıdaki örnekte yükler `Configuration` aracılığıyla yapılandırma ayarlarınızı üyesiyle *appsettings.json* herhangi yanı sıra *appsettings.\< EnvironmentName\>.json* dosya eşleşen `IHostingEnvironment.EnvironmentName` özelliği.</span><span class="sxs-lookup"><span data-stu-id="cedfd-142">The preceding example loads the `Configuration` member with configuration settings from *appsettings.json* as well as any *appsettings.\<EnvironmentName\>.json* file matching the `IHostingEnvironment.EnvironmentName` property.</span></span> <span data-ttu-id="cedfd-143">Bu dosyalar aynı yolda konumudur *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="cedfd-143">The location of these files is at the same path as *Startup.cs*.</span></span>

<span data-ttu-id="cedfd-144">2.0 projelerinde 1.x projelerini devralınan ortak yapılandırma kod arkası olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="cedfd-144">In 2.0 projects, the boilerplate configuration code inherent to 1.x projects runs behind-the-scenes.</span></span> <span data-ttu-id="cedfd-145">Örneğin, ortam değişkenleri ve uygulama ayarlarını başlatma sırasında yüklenir.</span><span class="sxs-lookup"><span data-stu-id="cedfd-145">For example, environment variables and app settings are loaded at startup.</span></span> <span data-ttu-id="cedfd-146">Eşdeğer *Startup.cs* kod azaltıldı `IConfiguration` eklenen örneği ile başlatma:</span><span class="sxs-lookup"><span data-stu-id="cedfd-146">The equivalent *Startup.cs* code is reduced to `IConfiguration` initialization with the injected instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Startup.cs?name=snippet_2xStartup)]

<span data-ttu-id="cedfd-147">Varsayılan sağlayıcıları tarafından eklenen kaldırmak için `WebHostBuilder.CreateDefaultBuilder`, çağırma `Clear` metodunda `IConfigurationBuilder.Sources` özelliği içinde `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="cedfd-147">To remove the default providers added by `WebHostBuilder.CreateDefaultBuilder`, invoke the `Clear` method on the `IConfigurationBuilder.Sources` property inside of `ConfigureAppConfiguration`.</span></span> <span data-ttu-id="cedfd-148">Geri sağlayıcılarının eklemek için yazılımınız `ConfigureAppConfiguration` yönteminde *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="cedfd-148">To add providers back, utilize the `ConfigureAppConfiguration` method in *Program.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Program.cs?name=snippet_ProgramMainConfigProviders&highlight=9-14)]

<span data-ttu-id="cedfd-149">Tarafından kullanılan yapılandırma `CreateDefaultBuilder` Yukarıdaki kod parçacığında yönteminde görülebilir [burada](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="cedfd-149">The configuration used by the `CreateDefaultBuilder` method in the preceding code snippet can be seen [here](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).</span></span>

<span data-ttu-id="cedfd-150">Daha fazla bilgi için [ASP.NET Core yapılandırmasında](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="cedfd-150">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

<a name="db-init-code"></a>

## <a name="move-database-initialization-code"></a><span data-ttu-id="cedfd-151">Veritabanı başlatma kodu Taşı</span><span class="sxs-lookup"><span data-stu-id="cedfd-151">Move database initialization code</span></span>

<span data-ttu-id="cedfd-152">1.x projelerinde kullanarak EF Core 1.x, gibi bir komutun `dotnet ef migrations add` şunları yapar:</span><span class="sxs-lookup"><span data-stu-id="cedfd-152">In 1.x projects using EF Core 1.x, a command such as `dotnet ef migrations add` does the following:</span></span>

1. <span data-ttu-id="cedfd-153">Örnekleyen bir `Startup` örneği</span><span class="sxs-lookup"><span data-stu-id="cedfd-153">Instantiates a `Startup` instance</span></span>
1. <span data-ttu-id="cedfd-154">Çağıran `ConfigureServices` tüm hizmetleri bağımlılık ekleme ile kaydetmek için yöntemi (dahil olmak üzere `DbContext` türleri)</span><span class="sxs-lookup"><span data-stu-id="cedfd-154">Invokes the `ConfigureServices` method to register all services with dependency injection (including `DbContext` types)</span></span>
1. <span data-ttu-id="cedfd-155">Önkoşul görevleri gerçekleştirir</span><span class="sxs-lookup"><span data-stu-id="cedfd-155">Performs its requisite tasks</span></span>

<span data-ttu-id="cedfd-156">EF Core 2.0 kullanarak 2.0 projelerinde `Program.BuildWebHost` uygulama hizmetlerini almak için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="cedfd-156">In 2.0 projects using EF Core 2.0, `Program.BuildWebHost` is invoked to obtain the application services.</span></span> <span data-ttu-id="cedfd-157">1.x bu çağırma ek yan etkisi olan `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="cedfd-157">Unlike 1.x, this has the additional side effect of invoking `Startup.Configure`.</span></span> <span data-ttu-id="cedfd-158">1.x uygulamanızı veritabanı başlatma kodunda çağırdıysanız, `Configure` yöntemi, beklenmeyen bir sorun meydana gelebilir.</span><span class="sxs-lookup"><span data-stu-id="cedfd-158">If your 1.x app invoked database initialization code in its `Configure` method, unexpected problems can occur.</span></span> <span data-ttu-id="cedfd-159">Örneğin, veritabanı henüz mevcut değilse dengeli dağıtım kod EF Core geçişleri komutu yürütmeden önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="cedfd-159">For example, if the database doesn't yet exist, the seeding code runs before the EF Core Migrations command execution.</span></span> <span data-ttu-id="cedfd-160">Bu soruna neden olan bir `dotnet ef migrations list` veritabanı henüz mevcut değilse başarısız için komutu.</span><span class="sxs-lookup"><span data-stu-id="cedfd-160">This problem causes a `dotnet ef migrations list` command to fail if the database doesn't yet exist.</span></span>

<span data-ttu-id="cedfd-161">Aşağıdaki 1.x çekirdek başlatma kodu göz önünde bulundurun `Configure` yöntemi *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="cedfd-161">Consider the following 1.x seed initialization code in the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_ConfigureSeedData&highlight=8)]

<span data-ttu-id="cedfd-162">2.0 projelerinde taşıma `SeedData.Initialize` çağrısı `Main` yöntemi *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="cedfd-162">In 2.0 projects, move the `SeedData.Initialize` call to the `Main` method of *Program.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program2.cs?name=snippet_Main2Code&highlight=10)]

<span data-ttu-id="cedfd-163">İsteğe bağlı olarak 2.0 itibariyle, herhangi bir şey yapmak için hatalı bir uygulama olan `BuildWebHost` dışındaki derleme ve web ana bilgisayarı yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="cedfd-163">As of 2.0, it's bad practice to do anything in `BuildWebHost` except build and configure the web host.</span></span> <span data-ttu-id="cedfd-164">Uygulamayı çalıştırmayı olan her şeyi dışında işlenmesi gereken `BuildWebHost` &mdash; normalde `Main` yöntemi *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="cedfd-164">Anything that's about running the application should be handled outside of `BuildWebHost` &mdash; typically in the `Main` method of *Program.cs*.</span></span>

<a name="view-compilation"></a>

## <a name="review-razor-view-compilation-setting"></a><span data-ttu-id="cedfd-165">Razor görünümü derleme ayarları gözden geçirin</span><span class="sxs-lookup"><span data-stu-id="cedfd-165">Review Razor view compilation setting</span></span>

<span data-ttu-id="cedfd-166">Dayanıklılığı, daha hızlı uygulama başlatma süresi ve daha küçük yayımlanan paketleri ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="cedfd-166">Faster application startup time and smaller published bundles are of utmost importance to you.</span></span> <span data-ttu-id="cedfd-167">Bu nedenlerle, [Razor görünüm derlemesi](xref:mvc/views/view-compilation) ASP.NET Core 2.0 varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="cedfd-167">For these reasons, [Razor view compilation](xref:mvc/views/view-compilation) is enabled by default in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="cedfd-168">Ayarı `MvcRazorCompileOnPublish` özelliği true gereklidir artık.</span><span class="sxs-lookup"><span data-stu-id="cedfd-168">Setting the `MvcRazorCompileOnPublish` property to true is no longer required.</span></span> <span data-ttu-id="cedfd-169">Öğesinden Görünüm derlemesi devre dışı bırakma sürece özellik kaldırılabilir *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="cedfd-169">Unless you're disabling view compilation, the property may be removed from the *.csproj* file.</span></span>

<span data-ttu-id="cedfd-170">.NET Framework'ü hedefleyen, açıkça başvuru yine [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet paketine, *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="cedfd-170">When targeting .NET Framework, you still need to explicitly reference the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet package in your *.csproj* file:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a><span data-ttu-id="cedfd-171">Application Insights "ışık yukarı" özellikleri güvenin</span><span class="sxs-lookup"><span data-stu-id="cedfd-171">Rely on Application Insights "light-up" features</span></span>

<span data-ttu-id="cedfd-172">Uygulama performansı izleme kolay kurulum büyük/küçük harf önemlidir.</span><span class="sxs-lookup"><span data-stu-id="cedfd-172">Effortless setup of application performance instrumentation is important.</span></span> <span data-ttu-id="cedfd-173">Şimdi yeni güvenebilirsiniz [Application Insights](/azure/application-insights/app-insights-overview) "ışık yukarı" özellikleri Visual Studio 2017 araçları içinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="cedfd-173">You can now rely on the new [Application Insights](/azure/application-insights/app-insights-overview) "light-up" features available in the Visual Studio 2017 tooling.</span></span>

<span data-ttu-id="cedfd-174">ASP.NET Core 1.1 projelerini Visual Studio 2017'de oluşturulan varsayılan olarak Application Insights eklendi.</span><span class="sxs-lookup"><span data-stu-id="cedfd-174">ASP.NET Core 1.1 projects created in Visual Studio 2017 added Application Insights by default.</span></span> <span data-ttu-id="cedfd-175">Application Insights SDK'sını doğrudan kullanmıyorsanız dışında *Program.cs* ve *Startup.cs*, şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="cedfd-175">If you're not using the Application Insights SDK directly, outside of *Program.cs* and *Startup.cs*, follow these steps:</span></span>

1. <span data-ttu-id="cedfd-176">.NET Core'u hedefleyen aşağıdaki kaldırmanız `<PackageReference />` düğümünden *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="cedfd-176">If targeting .NET Core, remove the following `<PackageReference />` node from the *.csproj* file:</span></span>

    [!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=10)]

2. <span data-ttu-id="cedfd-177">.NET Core'u hedefleyen, kaldırmanız `UseApplicationInsights` uzantısı yöntem çağrısından *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="cedfd-177">If targeting .NET Core, remove the `UseApplicationInsights` extension method invocation from *Program.cs*:</span></span>

    [!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. <span data-ttu-id="cedfd-178">Application Insights istemci tarafı API çağrısından kaldırmak *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="cedfd-178">Remove the Application Insights client-side API call from *_Layout.cshtml*.</span></span> <span data-ttu-id="cedfd-179">Bunu, aşağıdaki iki kod satırlarını oluşur:</span><span class="sxs-lookup"><span data-stu-id="cedfd-179">It comprises the following two lines of code:</span></span>

    [!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Shared/_Layout.cshtml?range=1,19&dedent=4)]

<span data-ttu-id="cedfd-180">Application Insights SDK'sını doğrudan kullanıyorsanız, bunu yapmak devam edin.</span><span class="sxs-lookup"><span data-stu-id="cedfd-180">If you are using the Application Insights SDK directly, continue to do so.</span></span> <span data-ttu-id="cedfd-181">2.0 [metapackage](xref:fundamentals/metapackage) Application Insights'ın en son sürümünü içerir, böylece daha eski bir sürümü başvuran paket indirgeme hatası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="cedfd-181">The 2.0 [metapackage](xref:fundamentals/metapackage) includes the latest version of Application Insights, so a package downgrade error appears if you're referencing an older version.</span></span>

<a name="auth-and-identity"></a>

## <a name="adopt-authenticationidentity-improvements"></a><span data-ttu-id="cedfd-182">Kimlik doğrulama/kimlik geliştirmeleri benimseyin</span><span class="sxs-lookup"><span data-stu-id="cedfd-182">Adopt authentication/Identity improvements</span></span>

<span data-ttu-id="cedfd-183">ASP.NET Core 2.0, yeni bir kimlik doğrulama modeli ve bir dizi ASP.NET Core kimliği için önemli değişiklik vardır.</span><span class="sxs-lookup"><span data-stu-id="cedfd-183">ASP.NET Core 2.0 has a new authentication model and a number of significant changes to ASP.NET Core Identity.</span></span> <span data-ttu-id="cedfd-184">Projenizi etkin bireysel kullanıcı hesapları ile oluşturulan ya da kimlik doğrulaması ve kimlik, el ile eklediyseniz bkz [geçirme kimlik doğrulaması ve kimlik için ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span><span class="sxs-lookup"><span data-stu-id="cedfd-184">If you created your project with Individual User Accounts enabled, or if you have manually added authentication or Identity, see [Migrate Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cedfd-185">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="cedfd-185">Additional resources</span></span>

* [<span data-ttu-id="cedfd-186">ASP.NET Core 2.0 bozucu değişiklikler</span><span class="sxs-lookup"><span data-stu-id="cedfd-186">Breaking Changes in ASP.NET Core 2.0</span></span>](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
