---
title: ASP.NET Core geçiş 1.x sürümünden 2.0 sürümüne
author: scottaddie
description: Bu makalede, önkoşulları ve ASP.NET Core 2.0 için bir ASP.NET Core 1.x projesi geçirmek için en yaygın özetlenmektedir.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/24/2018
uid: migration/1x-to-2x/index
ms.openlocfilehash: fb6157205ab5280eb982a61e834eea5074864830
ms.sourcegitcommit: a3926eae3f687013027a2828830c12a89add701f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65450959"
---
# <a name="migrate-from-aspnet-core-1x-to-20"></a><span data-ttu-id="5ea2b-103">ASP.NET Core geçiş 1.x sürümünden 2.0 sürümüne</span><span class="sxs-lookup"><span data-stu-id="5ea2b-103">Migrate from ASP.NET Core 1.x to 2.0</span></span>

<span data-ttu-id="5ea2b-104">Tarafından [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="5ea2b-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="5ea2b-105">Bu makalede, ASP.NET Core 2.0 için mevcut bir ASP.NET Core 1.x projesi güncelleştirme ile inceleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-105">In this article, we walk you through updating an existing ASP.NET Core 1.x project to ASP.NET Core 2.0.</span></span> <span data-ttu-id="5ea2b-106">ASP.NET Core 2.0 uygulamanızı geçişi avantajlarından yararlanmanıza olanak tanır [birçok yeni özellikler ve performans iyileştirmeleri](xref:aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="5ea2b-106">Migrating your application to ASP.NET Core 2.0 enables you to take advantage of [many new features and performance improvements](xref:aspnetcore-2.0).</span></span>

<span data-ttu-id="5ea2b-107">ASP.NET Core 1.x uygulamalara dışına sürüme özgü proje şablonları temel alır.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-107">Existing ASP.NET Core 1.x applications are based off of version-specific project templates.</span></span> <span data-ttu-id="5ea2b-108">ASP.NET Core framework geliştikçe, bu nedenle proje şablonları ve içerdiği Başlatıcı kodunu yapın.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-108">As the ASP.NET Core framework evolves, so do the project templates and the starter code contained within them.</span></span> <span data-ttu-id="5ea2b-109">ASP.NET Core framework güncelleştirmeye ek olarak, uygulamanız için kodu güncelleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-109">In addition to updating the ASP.NET Core framework, you need to update the code for your application.</span></span>

<a name="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="5ea2b-110">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="5ea2b-110">Prerequisites</span></span>

<span data-ttu-id="5ea2b-111">Bkz: [ASP.NET Core ile çalışmaya başlama](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="5ea2b-111">See [Get Started with ASP.NET Core](xref:getting-started).</span></span>

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a><span data-ttu-id="5ea2b-112">Hedef Çerçeve adı (TFM) güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="5ea2b-112">Update Target Framework Moniker (TFM)</span></span>

<span data-ttu-id="5ea2b-113">.NET Core'u hedefleyen projeler kullanması gereken [TFM](/dotnet/standard/frameworks#referring-to-frameworks) büyüktür veya eşittir .NET Core 2.0 sürümü.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-113">Projects targeting .NET Core should use the [TFM](/dotnet/standard/frameworks#referring-to-frameworks) of a version greater than or equal to .NET Core 2.0.</span></span> <span data-ttu-id="5ea2b-114">Arama `<TargetFramework>` düğümünde *.csproj* dosya ve kendi iç metinle `netcoreapp2.0`:</span><span class="sxs-lookup"><span data-stu-id="5ea2b-114">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `netcoreapp2.0`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=3)]

<span data-ttu-id="5ea2b-115">.NET Framework'ü hedefleyen projeleri TFM büyüktür veya eşittir .NET Framework 4.6.1 sürümü kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-115">Projects targeting .NET Framework should use the TFM of a version greater than or equal to .NET Framework 4.6.1.</span></span> <span data-ttu-id="5ea2b-116">Arama `<TargetFramework>` düğümünde *.csproj* dosya ve kendi iç metinle `net461`:</span><span class="sxs-lookup"><span data-stu-id="5ea2b-116">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `net461`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> <span data-ttu-id="5ea2b-117">.NET core 2.0 sunan bir çok büyük yüzey alanını daha .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-117">.NET Core 2.0 offers a much larger surface area than .NET Core 1.x.</span></span> <span data-ttu-id="5ea2b-118">.NET Framework yalnızca .NET Core 1.x, .NET Core 2.0 hedefleyen çalışması olasıdır API'leri eksik hedefliyorsanız.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-118">If you're targeting .NET Framework solely because of missing APIs in .NET Core 1.x, targeting .NET Core 2.0 is likely to work.</span></span>

<span data-ttu-id="5ea2b-119">Proje dosyası içeriyorsa `<RuntimeFrameworkVersion>1.{sub-version}</RuntimeFrameworkVersion>`, bkz: [bu GitHub sorunu](https://github.com/aspnet/AspNetCore/issues/3221#issuecomment-413094268).</span><span class="sxs-lookup"><span data-stu-id="5ea2b-119">If the project file contains `<RuntimeFrameworkVersion>1.{sub-version}</RuntimeFrameworkVersion>`, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/3221#issuecomment-413094268).</span></span>

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a><span data-ttu-id="5ea2b-120">.NET Core SDK'sı sürümünü global.json güncelleştirin</span><span class="sxs-lookup"><span data-stu-id="5ea2b-120">Update .NET Core SDK version in global.json</span></span>

<span data-ttu-id="5ea2b-121">Çözümünüze bağlı dayanıyorsa bir [ *global.json* ](/dotnet/core/tools/global-json) dosya belirli bir .NET Core SDK sürümünü hedeflemek, güncelleştirme, `version` özelliğinin makinenizde yüklü 2.0 sürümü kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="5ea2b-121">If your solution relies upon a [*global.json*](/dotnet/core/tools/global-json) file to target a specific .NET Core SDK version, update its `version` property to use the 2.0 version installed on your machine:</span></span>

[!code-json[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a><span data-ttu-id="5ea2b-122">Güncelleştirme paketi başvuruları</span><span class="sxs-lookup"><span data-stu-id="5ea2b-122">Update package references</span></span>

<span data-ttu-id="5ea2b-123">*.Csproj* 1.x proje dosyasında proje tarafından kullanılan her bir NuGet paketini listeler.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-123">The *.csproj* file in a 1.x project lists each NuGet package used by the project.</span></span>

<span data-ttu-id="5ea2b-124">.NET Core 2.0, tek bir hedefleyen ASP.NET Core 2.0 projesinde [metapackage](xref:fundamentals/metapackage) başvuru *.csproj* paketler koleksiyonunu yerini alır:</span><span class="sxs-lookup"><span data-stu-id="5ea2b-124">In an ASP.NET Core 2.0 project targeting .NET Core 2.0, a single [metapackage](xref:fundamentals/metapackage) reference in the *.csproj* file replaces the collection of packages:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=8-10)]

<span data-ttu-id="5ea2b-125">ASP.NET Core 2.0 ve Entity Framework Core 2.0 tüm özelliklerini metapackage dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-125">All the features of ASP.NET Core 2.0 and Entity Framework Core 2.0 are included in the metapackage.</span></span>

<span data-ttu-id="5ea2b-126">.NET Framework'ü hedefleyen ASP.NET Core 2.0 projeleri NuGet paketlerini tek tek başvurmak devam etmelidir.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-126">ASP.NET Core 2.0 projects targeting .NET Framework should continue to reference individual NuGet packages.</span></span> <span data-ttu-id="5ea2b-127">Güncelleştirme `Version` her öznitelik `<PackageReference />` 2.0.0 düğümü.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-127">Update the `Version` attribute of each `<PackageReference />` node to 2.0.0.</span></span>

<span data-ttu-id="5ea2b-128">Örneğin, listesi sunulmaktadır `<PackageReference />` .NET Framework'ü hedefleyen tipik bir ASP.NET Core 2.0 proje içinde kullanılan düğümler:</span><span class="sxs-lookup"><span data-stu-id="5ea2b-128">For example, here's the list of `<PackageReference />` nodes used in a typical ASP.NET Core 2.0 project targeting .NET Framework:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a><span data-ttu-id="5ea2b-129">.NET Core CLI araçları güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="5ea2b-129">Update .NET Core CLI tools</span></span>

<span data-ttu-id="5ea2b-130">İçinde *.csproj* dosyası, güncelleştirme `Version` her öznitelik `<DotNetCliToolReference />` 2.0.0 düğümü.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-130">In the *.csproj* file, update the `Version` attribute of each `<DotNetCliToolReference />` node to 2.0.0.</span></span>

<span data-ttu-id="5ea2b-131">Örneğin, .NET Core 2.0 hedefleyen tipik bir ASP.NET Core 2.0 projesinde kullanılan CLI araçları listesi şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="5ea2b-131">For example, here's the list of CLI tools used in a typical ASP.NET Core 2.0 project targeting .NET Core 2.0:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=12-16)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a><span data-ttu-id="5ea2b-132">Paketin hedef düşürme özelliği yeniden adlandır</span><span class="sxs-lookup"><span data-stu-id="5ea2b-132">Rename Package Target Fallback property</span></span>

<span data-ttu-id="5ea2b-133">*.Csproj* kullanılan 1.x projenin dosya bir `PackageTargetFallback` düğüm ve değişkeni:</span><span class="sxs-lookup"><span data-stu-id="5ea2b-133">The *.csproj* file of a 1.x project used a `PackageTargetFallback` node and variable:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=5)]

<span data-ttu-id="5ea2b-134">Düğüm ve değişkeni Yeniden Adlandır `AssetTargetFallback`:</span><span class="sxs-lookup"><span data-stu-id="5ea2b-134">Rename both the node and variable to `AssetTargetFallback`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=4)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a><span data-ttu-id="5ea2b-135">Program.CS'de Webhostbuilder'a güncelleştirme Main yöntemi</span><span class="sxs-lookup"><span data-stu-id="5ea2b-135">Update Main method in Program.cs</span></span>

<span data-ttu-id="5ea2b-136">1.x projelerinde `Main` yöntemi *Program.cs* şöyle Aranan:</span><span class="sxs-lookup"><span data-stu-id="5ea2b-136">In 1.x projects, the `Main` method of *Program.cs* looked like this:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

<span data-ttu-id="5ea2b-137">2.0 projelerinde `Main` yöntemi *Program.cs* basitleştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="5ea2b-137">In 2.0 projects, the `Main` method of *Program.cs* has been simplified:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program.cs?highlight=8-11)]

<span data-ttu-id="5ea2b-138">Bu yeni 2.0 Düzen benimsenmesini önemle tavsiye edilir ve gibi ürün özellikleri için gereklidir [Entity Framework (EF) Code Migrations](xref:data/ef-mvc/migrations) çalışılır.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-138">The adoption of this new 2.0 pattern is highly recommended and is required for product features like [Entity Framework (EF) Core Migrations](xref:data/ef-mvc/migrations) to work.</span></span> <span data-ttu-id="5ea2b-139">Örneğin, çalışan `Update-Database` Paket Yöneticisi konsolu penceresinde veya `dotnet ef database update` komut satırında (Projeler) için ASP.NET Core 2.0 dönüştürülür şu hata oluşturur:</span><span class="sxs-lookup"><span data-stu-id="5ea2b-139">For example, running `Update-Database` from the Package Manager Console window or `dotnet ef database update` from the command line (on projects converted to ASP.NET Core 2.0) generates the following error:</span></span>

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="add-modify-configuration"></a>

## <a name="add-configuration-providers"></a><span data-ttu-id="5ea2b-140">Yapılandırma Sağlayıcıları Ekle</span><span class="sxs-lookup"><span data-stu-id="5ea2b-140">Add configuration providers</span></span>

<span data-ttu-id="5ea2b-141">1.x projelerinde, bir yapılandırma sağlayıcısı bir uygulamaya ekleme yoluyla gerçekleştirilebilir `Startup` Oluşturucusu.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-141">In 1.x projects, adding configuration providers to an app was accomplished via the `Startup` constructor.</span></span> <span data-ttu-id="5ea2b-142">Bir örneğini oluşturmaktan adımla daha uğraşmanız `ConfigurationBuilder`geçerli (ortam değişkenleri, uygulama ayarları, vb.) sağlayıcıları yükleniyor ve üye başlatma `IConfigurationRoot`.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-142">The steps involved creating an instance of `ConfigurationBuilder`, loading applicable providers (environment variables, app settings, etc.), and initializing a member of `IConfigurationRoot`.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_1xStartup)]

<span data-ttu-id="5ea2b-143">Yukarıdaki örnekte yükler `Configuration` aracılığıyla yapılandırma ayarlarınızı üyesiyle *appsettings.json* herhangi yanı sıra *appsettings.\< EnvironmentName\>.json* dosya eşleşen `IHostingEnvironment.EnvironmentName` özelliği.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-143">The preceding example loads the `Configuration` member with configuration settings from *appsettings.json* as well as any *appsettings.\<EnvironmentName\>.json* file matching the `IHostingEnvironment.EnvironmentName` property.</span></span> <span data-ttu-id="5ea2b-144">Bu dosyalar aynı yolda konumudur *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-144">The location of these files is at the same path as *Startup.cs*.</span></span>

<span data-ttu-id="5ea2b-145">2.0 projelerinde 1.x projelerini devralınan ortak yapılandırma kod arkası olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-145">In 2.0 projects, the boilerplate configuration code inherent to 1.x projects runs behind-the-scenes.</span></span> <span data-ttu-id="5ea2b-146">Örneğin, ortam değişkenleri ve uygulama ayarlarını başlatma sırasında yüklenir.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-146">For example, environment variables and app settings are loaded at startup.</span></span> <span data-ttu-id="5ea2b-147">Eşdeğer *Startup.cs* kod azaltıldı `IConfiguration` eklenen örneği ile başlatma:</span><span class="sxs-lookup"><span data-stu-id="5ea2b-147">The equivalent *Startup.cs* code is reduced to `IConfiguration` initialization with the injected instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Startup.cs?name=snippet_2xStartup)]

<span data-ttu-id="5ea2b-148">Varsayılan sağlayıcıları tarafından eklenen kaldırmak için `WebHostBuilder.CreateDefaultBuilder`, çağırma `Clear` metodunda `IConfigurationBuilder.Sources` özelliği içinde `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-148">To remove the default providers added by `WebHostBuilder.CreateDefaultBuilder`, invoke the `Clear` method on the `IConfigurationBuilder.Sources` property inside of `ConfigureAppConfiguration`.</span></span> <span data-ttu-id="5ea2b-149">Geri sağlayıcılarının eklemek için yazılımınız `ConfigureAppConfiguration` yönteminde *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="5ea2b-149">To add providers back, utilize the `ConfigureAppConfiguration` method in *Program.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Program.cs?name=snippet_ProgramMainConfigProviders&highlight=9-14)]

<span data-ttu-id="5ea2b-150">Tarafından kullanılan yapılandırma `CreateDefaultBuilder` Yukarıdaki kod parçacığında yönteminde görülebilir [burada](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="5ea2b-150">The configuration used by the `CreateDefaultBuilder` method in the preceding code snippet can be seen [here](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).</span></span>

<span data-ttu-id="5ea2b-151">Daha fazla bilgi için [ASP.NET Core yapılandırmasında](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="5ea2b-151">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

<a name="db-init-code"></a>

## <a name="move-database-initialization-code"></a><span data-ttu-id="5ea2b-152">Veritabanı başlatma kodu Taşı</span><span class="sxs-lookup"><span data-stu-id="5ea2b-152">Move database initialization code</span></span>

<span data-ttu-id="5ea2b-153">1.x projelerinde kullanarak EF Core 1.x, gibi bir komutun `dotnet ef migrations add` şunları yapar:</span><span class="sxs-lookup"><span data-stu-id="5ea2b-153">In 1.x projects using EF Core 1.x, a command such as `dotnet ef migrations add` does the following:</span></span>

1. <span data-ttu-id="5ea2b-154">Örnekleyen bir `Startup` örneği</span><span class="sxs-lookup"><span data-stu-id="5ea2b-154">Instantiates a `Startup` instance</span></span>
1. <span data-ttu-id="5ea2b-155">Çağıran `ConfigureServices` tüm hizmetleri bağımlılık ekleme ile kaydetmek için yöntemi (dahil olmak üzere `DbContext` türleri)</span><span class="sxs-lookup"><span data-stu-id="5ea2b-155">Invokes the `ConfigureServices` method to register all services with dependency injection (including `DbContext` types)</span></span>
1. <span data-ttu-id="5ea2b-156">Önkoşul görevleri gerçekleştirir</span><span class="sxs-lookup"><span data-stu-id="5ea2b-156">Performs its requisite tasks</span></span>

<span data-ttu-id="5ea2b-157">EF Core 2.0 kullanarak 2.0 projelerinde `Program.BuildWebHost` uygulama hizmetlerini almak için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-157">In 2.0 projects using EF Core 2.0, `Program.BuildWebHost` is invoked to obtain the application services.</span></span> <span data-ttu-id="5ea2b-158">1.x bu çağırma ek yan etkisi olan `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-158">Unlike 1.x, this has the additional side effect of invoking `Startup.Configure`.</span></span> <span data-ttu-id="5ea2b-159">1.x uygulamanızı veritabanı başlatma kodunda çağırdıysanız, `Configure` yöntemi, beklenmeyen bir sorun meydana gelebilir.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-159">If your 1.x app invoked database initialization code in its `Configure` method, unexpected problems can occur.</span></span> <span data-ttu-id="5ea2b-160">Örneğin, veritabanı henüz mevcut değilse dengeli dağıtım kod EF Core geçişleri komutu yürütmeden önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-160">For example, if the database doesn't yet exist, the seeding code runs before the EF Core Migrations command execution.</span></span> <span data-ttu-id="5ea2b-161">Bu soruna neden olan bir `dotnet ef migrations list` veritabanı henüz mevcut değilse başarısız için komutu.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-161">This problem causes a `dotnet ef migrations list` command to fail if the database doesn't yet exist.</span></span>

<span data-ttu-id="5ea2b-162">Aşağıdaki 1.x çekirdek başlatma kodu göz önünde bulundurun `Configure` yöntemi *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5ea2b-162">Consider the following 1.x seed initialization code in the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_ConfigureSeedData&highlight=8)]

<span data-ttu-id="5ea2b-163">2.0 projelerinde taşıma `SeedData.Initialize` çağrısı `Main` yöntemi *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="5ea2b-163">In 2.0 projects, move the `SeedData.Initialize` call to the `Main` method of *Program.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program2.cs?name=snippet_Main2Code&highlight=10)]

<span data-ttu-id="5ea2b-164">İsteğe bağlı olarak 2.0 itibariyle, herhangi bir şey yapmak için hatalı bir uygulama olan `BuildWebHost` dışındaki derleme ve web ana bilgisayarı yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-164">As of 2.0, it's bad practice to do anything in `BuildWebHost` except build and configure the web host.</span></span> <span data-ttu-id="5ea2b-165">Uygulamayı çalıştırmayı olan her şeyi dışında işlenmesi gereken `BuildWebHost` &mdash; normalde `Main` yöntemi *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-165">Anything that's about running the application should be handled outside of `BuildWebHost` &mdash; typically in the `Main` method of *Program.cs*.</span></span>

<a name="view-compilation"></a>

## <a name="review-razor-view-compilation-setting"></a><span data-ttu-id="5ea2b-166">Razor görünümü derleme ayarları gözden geçirin</span><span class="sxs-lookup"><span data-stu-id="5ea2b-166">Review Razor view compilation setting</span></span>

<span data-ttu-id="5ea2b-167">Dayanıklılığı, daha hızlı uygulama başlatma süresi ve daha küçük yayımlanan paketleri ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-167">Faster application startup time and smaller published bundles are of utmost importance to you.</span></span> <span data-ttu-id="5ea2b-168">Bu nedenlerle, [Razor görünüm derlemesi](xref:mvc/views/view-compilation) ASP.NET Core 2.0 varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-168">For these reasons, [Razor view compilation](xref:mvc/views/view-compilation) is enabled by default in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="5ea2b-169">Ayarı `MvcRazorCompileOnPublish` özelliği true gereklidir artık.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-169">Setting the `MvcRazorCompileOnPublish` property to true is no longer required.</span></span> <span data-ttu-id="5ea2b-170">Öğesinden Görünüm derlemesi devre dışı bırakma sürece özellik kaldırılabilir *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-170">Unless you're disabling view compilation, the property may be removed from the *.csproj* file.</span></span>

<span data-ttu-id="5ea2b-171">.NET Framework'ü hedefleyen, açıkça başvuru yine [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet paketine, *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="5ea2b-171">When targeting .NET Framework, you still need to explicitly reference the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet package in your *.csproj* file:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a><span data-ttu-id="5ea2b-172">Application Insights "ışık yukarı" özellikleri güvenin</span><span class="sxs-lookup"><span data-stu-id="5ea2b-172">Rely on Application Insights "light-up" features</span></span>

<span data-ttu-id="5ea2b-173">Uygulama performansı izleme kolay kurulum büyük/küçük harf önemlidir.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-173">Effortless setup of application performance instrumentation is important.</span></span> <span data-ttu-id="5ea2b-174">Şimdi yeni güvenebilirsiniz [Application Insights](/azure/application-insights/app-insights-overview) "ışık yukarı" özellikleri Visual Studio 2017 araçları içinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-174">You can now rely on the new [Application Insights](/azure/application-insights/app-insights-overview) "light-up" features available in the Visual Studio 2017 tooling.</span></span>

<span data-ttu-id="5ea2b-175">ASP.NET Core 1.1 projelerini Visual Studio 2017'de oluşturulan varsayılan olarak Application Insights eklendi.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-175">ASP.NET Core 1.1 projects created in Visual Studio 2017 added Application Insights by default.</span></span> <span data-ttu-id="5ea2b-176">Application Insights SDK'sını doğrudan kullanmıyorsanız dışında *Program.cs* ve *Startup.cs*, şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="5ea2b-176">If you're not using the Application Insights SDK directly, outside of *Program.cs* and *Startup.cs*, follow these steps:</span></span>

1. <span data-ttu-id="5ea2b-177">.NET Core'u hedefleyen aşağıdaki kaldırmanız `<PackageReference />` düğümünden *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="5ea2b-177">If targeting .NET Core, remove the following `<PackageReference />` node from the *.csproj* file:</span></span>

    [!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=10)]

2. <span data-ttu-id="5ea2b-178">.NET Core'u hedefleyen, kaldırmanız `UseApplicationInsights` uzantısı yöntem çağrısından *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="5ea2b-178">If targeting .NET Core, remove the `UseApplicationInsights` extension method invocation from *Program.cs*:</span></span>

    [!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. <span data-ttu-id="5ea2b-179">Application Insights istemci tarafı API çağrısından kaldırmak *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-179">Remove the Application Insights client-side API call from *_Layout.cshtml*.</span></span> <span data-ttu-id="5ea2b-180">Bunu, aşağıdaki iki kod satırlarını oluşur:</span><span class="sxs-lookup"><span data-stu-id="5ea2b-180">It comprises the following two lines of code:</span></span>

    [!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Shared/_Layout.cshtml?range=1,19&dedent=4)]

<span data-ttu-id="5ea2b-181">Application Insights SDK'sını doğrudan kullanıyorsanız, bunu yapmak devam edin.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-181">If you are using the Application Insights SDK directly, continue to do so.</span></span> <span data-ttu-id="5ea2b-182">2.0 [metapackage](xref:fundamentals/metapackage) Application Insights'ın en son sürümünü içerir, böylece daha eski bir sürümü başvuran paket indirgeme hatası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-182">The 2.0 [metapackage](xref:fundamentals/metapackage) includes the latest version of Application Insights, so a package downgrade error appears if you're referencing an older version.</span></span>

<a name="auth-and-identity"></a>

## <a name="adopt-authenticationidentity-improvements"></a><span data-ttu-id="5ea2b-183">Kimlik doğrulama/kimlik geliştirmeleri benimseyin</span><span class="sxs-lookup"><span data-stu-id="5ea2b-183">Adopt authentication/Identity improvements</span></span>

<span data-ttu-id="5ea2b-184">ASP.NET Core 2.0, yeni bir kimlik doğrulama modeli ve bir dizi ASP.NET Core kimliği için önemli değişiklik vardır.</span><span class="sxs-lookup"><span data-stu-id="5ea2b-184">ASP.NET Core 2.0 has a new authentication model and a number of significant changes to ASP.NET Core Identity.</span></span> <span data-ttu-id="5ea2b-185">Projenizi etkin bireysel kullanıcı hesapları ile oluşturulan ya da kimlik doğrulaması ve kimlik, el ile eklediyseniz bkz [geçirme kimlik doğrulaması ve kimlik için ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span><span class="sxs-lookup"><span data-stu-id="5ea2b-185">If you created your project with Individual User Accounts enabled, or if you have manually added authentication or Identity, see [Migrate Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5ea2b-186">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5ea2b-186">Additional resources</span></span>

* [<span data-ttu-id="5ea2b-187">ASP.NET Core 2.0 bozucu değişiklikler</span><span class="sxs-lookup"><span data-stu-id="5ea2b-187">Breaking Changes in ASP.NET Core 2.0</span></span>](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
