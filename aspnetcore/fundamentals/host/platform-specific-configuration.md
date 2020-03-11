---
title: ASP.NET Core barındırma başlangıç derlemeleri kullanma
author: rick-anderson
description: Uygulama Ihostingstartup kullanarak dış bütünleştirilmiş koddan bir ASP.NET Core uygulaması geliştirmek nasıl keşfedin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 09/26/2019
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 71fd5cf1934b5374e0a393e055db23b98c03b62f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660400"
---
# <a name="use-hosting-startup-assemblies-in-aspnet-core"></a><span data-ttu-id="a4be8-103">ASP.NET Core barındırma başlangıç derlemeleri kullanma</span><span class="sxs-lookup"><span data-stu-id="a4be8-103">Use hosting startup assemblies in ASP.NET Core</span></span>

<span data-ttu-id="a4be8-104">[Palete kroni](https://github.com/pakrym)</span><span class="sxs-lookup"><span data-stu-id="a4be8-104">By [Pavel Krymets](https://github.com/pakrym)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a4be8-105"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> (barındırma başlatma) uygulaması, bir dış derlemeden başlatma sırasında bir uygulamaya iyileştirmeler ekler.</span><span class="sxs-lookup"><span data-stu-id="a4be8-105">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="a4be8-106">Örneğin, bir harici kitaplık ek yapılandırma sağlayıcıları ya da bir uygulama hizmetlerini barındıran bir başlangıç uygulaması kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a4be8-106">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span>

<span data-ttu-id="a4be8-107">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a4be8-107">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="a4be8-108">HostingStartup özniteliği</span><span class="sxs-lookup"><span data-stu-id="a4be8-108">HostingStartup attribute</span></span>

<span data-ttu-id="a4be8-109">[Hostingstartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) özniteliği, çalışma zamanında etkinleştirilecek bir barındırma başlangıç derlemesinin varlığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-109">A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="a4be8-110">Giriş derlemesi veya `Startup` sınıfını içeren derleme `HostingStartup` özniteliği için otomatik olarak taranır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-110">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="a4be8-111">`HostingStartup` özniteliklerin aranacağı derlemelerin listesi [Webhostdefaults. HostingStartupAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupAssembliesKey)içindeki yapılandırmadan çalışma zamanında yüklenir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-111">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupAssembliesKey).</span></span> <span data-ttu-id="a4be8-112">Bulmadan dışlanacak derlemelerin listesi [Webhostdefaults. Hostingstartupexcludederlemelieskey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupExcludeAssembliesKey)öğesinden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="a4be8-112">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupExcludeAssembliesKey).</span></span>

<span data-ttu-id="a4be8-113">Aşağıdaki örnekte, barındırma başlangıç derlemesinin ad alanı `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="a4be8-113">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="a4be8-114">Barındırma başlangıç kodunu içeren sınıf `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="a4be8-114">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="a4be8-115">`HostingStartup` özniteliği genellikle barındırma başlangıç derlemesinin `IHostingStartup` uygulama sınıfı dosyasında bulunur.</span><span class="sxs-lookup"><span data-stu-id="a4be8-115">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="a4be8-116">Yüklenen barındırma başlangıç derlemeler keşfedin</span><span class="sxs-lookup"><span data-stu-id="a4be8-116">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="a4be8-117">Yüklenen barındırma başlangıç derlemeleri bulmak için günlük kaydını etkinleştirmek ve uygulama günlüklerini kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-117">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="a4be8-118">Derlemeler yüklenirken oluşan hataları günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-118">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="a4be8-119">Yüklenen barındırma başlangıç derlemeler hata ayıklama düzeyinde kaydedilir ve tüm hatalar kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-119">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="a4be8-120">Başlangıç derlemeleri barındırma otomatik yüklemeyi devre dışı</span><span class="sxs-lookup"><span data-stu-id="a4be8-120">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="a4be8-121">Başlangıç derlemeleri barındırma otomatik yüklemeyi devre dışı bırakmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="a4be8-121">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="a4be8-122">Tüm barındırma başlangıç derlemelerinin yüklenmesini engellemek için aşağıdakilerden birini `true` veya `1`olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="a4be8-122">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>

  * <span data-ttu-id="a4be8-123">Barındırma başlangıç ana bilgisayar yapılandırma ayarını önle:</span><span class="sxs-lookup"><span data-stu-id="a4be8-123">Prevent Hosting Startup host configuration setting:</span></span>

    ```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseSetting(
                        WebHostDefaults.PreventHostingStartupKey, "true")
                    .UseStartup<Startup>();
            });
    ```

  * <span data-ttu-id="a4be8-124">ortam değişkeni `ASPNETCORE_PREVENTHOSTINGSTARTUP`.</span><span class="sxs-lookup"><span data-stu-id="a4be8-124">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

* <span data-ttu-id="a4be8-125">Belirli barındırma başlangıç derlemeleri yüklenmesini önlemek için aşağıdakilerden birini başlangıçta hariç tutmak için başlangıç derlemeleri barındırma noktalı virgülle ayrılmış bir dizeye ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="a4be8-125">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>

  * <span data-ttu-id="a4be8-126">Barındırma başlatma derlemeleri dışlama ana bilgisayar yapılandırma ayarı:</span><span class="sxs-lookup"><span data-stu-id="a4be8-126">Hosting Startup Exclude Assemblies host configuration setting:</span></span>

    ```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseSetting(
                        WebHostDefaults.HostingStartupExcludeAssembliesKey, 
                        "{ASSEMBLY1;ASSEMBLY2; ...}")
                    .UseStartup<Startup>();
            });
    ```

  * <span data-ttu-id="a4be8-127">ortam değişkeni `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="a4be8-127">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

<span data-ttu-id="a4be8-128">Hem ana bilgisayar yapılandırma ayarı ve ortam değişkenini ayarlarsanız, ana bilgisayar ayarını davranışını denetler.</span><span class="sxs-lookup"><span data-stu-id="a4be8-128">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="a4be8-129">Ana bilgisayar ayarı veya ortam değişkenini kullanarak barındırma başlangıç derlemeleri devre dışı bırakma, derlemenin genel olarak devre dışı bırakır ve uygulama çeşitli özelliklerini devre dışı bırakabilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-129">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="a4be8-130">Project</span><span class="sxs-lookup"><span data-stu-id="a4be8-130">Project</span></span>

<span data-ttu-id="a4be8-131">Bir barındırma başlatma aşağıdaki proje türlerini birini oluşturun:</span><span class="sxs-lookup"><span data-stu-id="a4be8-131">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="a4be8-132">Sınıf kitaplığı</span><span class="sxs-lookup"><span data-stu-id="a4be8-132">Class library</span></span>](#class-library)
* [<span data-ttu-id="a4be8-133">Giriş noktası olmayan konsol uygulaması</span><span class="sxs-lookup"><span data-stu-id="a4be8-133">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="a4be8-134">Sınıf kitaplığı</span><span class="sxs-lookup"><span data-stu-id="a4be8-134">Class library</span></span>

<span data-ttu-id="a4be8-135">Bir barındırma başlangıç geliştirme'de sınıf kitaplığının sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-135">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="a4be8-136">Kitaplık bir `HostingStartup` özniteliği içerir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-136">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="a4be8-137">[Örnek kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) bir Razor Pages uygulaması, *Hostingstartupapp*ve bir sınıf kitaplığı, *hostingstartuplibrary*içerir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-137">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="a4be8-138">Sınıf kitaplığı:</span><span class="sxs-lookup"><span data-stu-id="a4be8-138">The class library:</span></span>

* <span data-ttu-id="a4be8-139">`IHostingStartup`uygulayan `ServiceKeyInjection`bir barındırma başlangıç sınıfı içerir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-139">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="a4be8-140">`ServiceKeyInjection`, bellek içi yapılandırma sağlayıcısını ([Addınmemorycollection](xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*)) kullanarak uygulama yapılandırmasına bir hizmet dizesi çifti ekler.</span><span class="sxs-lookup"><span data-stu-id="a4be8-140">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*)).</span></span>
* <span data-ttu-id="a4be8-141">, Barındırma başlatmasının ad alanını ve sınıfını tanımlayan bir `HostingStartup` özniteliği içerir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-141">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="a4be8-142">`ServiceKeyInjection` sınıfının <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> yöntemi bir uygulamaya geliştirmeler eklemek için bir <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> kullanır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-142">The `ServiceKeyInjection` class's <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> method uses an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> to add enhancements to an app.</span></span>

<span data-ttu-id="a4be8-143">*Hostingstartuplibrary/ServiceKeyInjection. cs*:</span><span class="sxs-lookup"><span data-stu-id="a4be8-143">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="a4be8-144">Uygulama dizin sayfasına okur ve Sınıf Kitaplığı'nızın barındırma başlangıç derlemesi tarafından ayarlanmış iki anahtarı için yapılandırma değerlerini işler:</span><span class="sxs-lookup"><span data-stu-id="a4be8-144">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="a4be8-145">*Hostingstartupapp/Pages/Index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="a4be8-145">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="a4be8-146">[Örnek kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ayrıca ayrı bir barındırma başlatma, *Hostingstartuppackage*sağlayan bir NuGet paket projesi içerir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-146">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="a4be8-147">Paket, daha önce açıklanan Sınıf Kitaplığı'nın aynı özelliklere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-147">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="a4be8-148">Paketi:</span><span class="sxs-lookup"><span data-stu-id="a4be8-148">The package:</span></span>

* <span data-ttu-id="a4be8-149">`IHostingStartup`uygulayan `ServiceKeyInjection`bir barındırma başlangıç sınıfı içerir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-149">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="a4be8-150">`ServiceKeyInjection`, uygulamanın yapılandırmasına bir çift hizmet dizesi ekler.</span><span class="sxs-lookup"><span data-stu-id="a4be8-150">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="a4be8-151">Bir `HostingStartup` özniteliği içerir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-151">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="a4be8-152">*Hostingstartuppackage/ServiceKeyInjection. cs*:</span><span class="sxs-lookup"><span data-stu-id="a4be8-152">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="a4be8-153">Uygulama dizin sayfasına okur ve paketin barındırma başlangıç derlemesi tarafından ayarlanmış iki anahtarı için yapılandırma değerlerini işler:</span><span class="sxs-lookup"><span data-stu-id="a4be8-153">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="a4be8-154">*Hostingstartupapp/Pages/Index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="a4be8-154">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="a4be8-155">Konsol uygulaması giriş noktası olmayan</span><span class="sxs-lookup"><span data-stu-id="a4be8-155">Console app without an entry point</span></span>

<span data-ttu-id="a4be8-156">*Bu yaklaşım, .NET Framework değil yalnızca .NET Core uygulamaları için kullanılabilir.*</span><span class="sxs-lookup"><span data-stu-id="a4be8-156">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="a4be8-157">Etkinleştirme için derleme zamanı başvurusu gerektirmeyen dinamik barındırma başlatma geliştirmesi, bir `HostingStartup` özniteliği içeren giriş noktası olmadan bir konsol uygulamasında sağlanabilmesi.</span><span class="sxs-lookup"><span data-stu-id="a4be8-157">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point that contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="a4be8-158">Konsol uygulaması yayımlama, çalışma zamanı Mağazası'ndan kullanılabilen bir barındırma başlangıç bütünleştirilmiş kod üretir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-158">Publishing the console app produces a hosting startup assembly that can be consumed from the runtime store.</span></span>

<span data-ttu-id="a4be8-159">Bir konsol uygulaması giriş noktası olmayan, çünkü bu işlemde kullanılır:</span><span class="sxs-lookup"><span data-stu-id="a4be8-159">A console app without an entry point is used in this process because:</span></span>

* <span data-ttu-id="a4be8-160">Bağımlılıkları dosya barındırma başlangıç derlemedeki barındırma için başlangıç kullanmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-160">A dependencies file is required to consume the hosting startup in the hosting startup assembly.</span></span> <span data-ttu-id="a4be8-161">Kitaplık değil bir uygulama yayımlama tarafından üretilen bir çalıştırılabilir uygulama varlık bağımlılıkları dosyasıdır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-161">A dependencies file is a runnable app asset that's produced by publishing an app, not a library.</span></span>
* <span data-ttu-id="a4be8-162">Bir kitaplık, paylaşılan çalışma zamanını hedefleyen bir çalıştırılabilir proje gerektiren [çalışma zamanı paket deposuna](/dotnet/core/deploying/runtime-store)eklenemez.</span><span class="sxs-lookup"><span data-stu-id="a4be8-162">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>

<span data-ttu-id="a4be8-163">Dinamik barındırma başlangıç oluşturulmasını içinde:</span><span class="sxs-lookup"><span data-stu-id="a4be8-163">In the creation of a dynamic hosting startup:</span></span>

* <span data-ttu-id="a4be8-164">Bir giriş noktası olmadan bir barındırma başlangıç derleme konsol uygulamasından oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="a4be8-164">A hosting startup assembly is created from the console app without an entry point that:</span></span>
  * <span data-ttu-id="a4be8-165">`IHostingStartup` uygulamasını içeren bir sınıf içerir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-165">Includes a class that contains the `IHostingStartup` implementation.</span></span>
  * <span data-ttu-id="a4be8-166">`IHostingStartup` uygulama sınıfını tanımlamak için bir [Hostingstartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) özniteliği içerir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-166">Includes a [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute to identify the `IHostingStartup` implementation class.</span></span>
* <span data-ttu-id="a4be8-167">Konsol uygulaması barındırma startup şirketinizin bağımlılıkları almak için yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-167">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="a4be8-168">Kullanılmayan bağımlılıkları bağımlılıkları dosyasından atılır konsol uygulaması yayımlama bir sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="a4be8-168">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
* <span data-ttu-id="a4be8-169">Bağımlılıkları dosyası, çalışma zamanı barındırma başlangıç derleme konumunu ayarlamak için değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-169">The dependencies file is modified to set the runtime location of the hosting startup assembly.</span></span>
* <span data-ttu-id="a4be8-170">Barındırma başlangıç derleme ve bağımlılıkları dosyası çalışma zamanı paketi deposuna yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-170">The hosting startup assembly and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="a4be8-171">Barındırma başlangıç derleme ve bağımlılıkları dosyasını bulmak için bir ortam değişkenlerini çift içinde listelendikleri.</span><span class="sxs-lookup"><span data-stu-id="a4be8-171">To discover the hosting startup assembly and its dependencies file, they're listed in a pair of environment variables.</span></span>

<span data-ttu-id="a4be8-172">Konsol uygulaması [Microsoft. AspNetCore. Hosting. soyutlamalar](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) paketine başvurur:</span><span class="sxs-lookup"><span data-stu-id="a4be8-172">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.csproj)]

<span data-ttu-id="a4be8-173">[Hostingstartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) özniteliği, bir sınıfı <xref:Microsoft.AspNetCore.Hosting.IWebHost>oluştururken yükleme ve yürütme için `IHostingStartup` bir uygulama olarak tanımlar.</span><span class="sxs-lookup"><span data-stu-id="a4be8-173">A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the <xref:Microsoft.AspNetCore.Hosting.IWebHost>.</span></span> <span data-ttu-id="a4be8-174">Aşağıdaki örnekte, ad alanı `StartupEnhancement`ve sınıf `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="a4be8-174">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="a4be8-175">Bir sınıf `IHostingStartup`uygular.</span><span class="sxs-lookup"><span data-stu-id="a4be8-175">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="a4be8-176">Sınıfın <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> yöntemi bir uygulamaya iyileştirmeler eklemek için bir <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> kullanır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-176">The class's <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> method uses an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> to add enhancements to an app.</span></span> <span data-ttu-id="a4be8-177">barındırma başlangıç derlemesinde `IHostingStartup.Configure`, Kullanıcı kodundaki `Startup.Configure` önce çalışma zamanı tarafından çağrılır, bu da kullanıcı kodunun barındırma başlangıç derlemesi tarafından verilen yapılandırmanın üzerine yazılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-177">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="a4be8-178">Bir `IHostingStartup` projesi oluştururken, bağımlılıklar dosyası ( *. Deps. JSON*) derlemenin `runtime` konumunu *bin* klasörüne ayarlar:</span><span class="sxs-lookup"><span data-stu-id="a4be8-178">When building an `IHostingStartup` project, the dependencies file (*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="a4be8-179">Dosyanın yalnızca bir parçası olarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-179">Only part of the file is shown.</span></span> <span data-ttu-id="a4be8-180">Örnekteki derleme adı `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="a4be8-180">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="configuration-provided-by-the-hosting-startup"></a><span data-ttu-id="a4be8-181">Barındırma başlatma tarafından belirtilen yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a4be8-181">Configuration provided by the hosting startup</span></span>

<span data-ttu-id="a4be8-182">Yapılandırma işlemi, barındırma başlatmasının yapılandırmasının öncelikli olmasını mı yoksa uygulamanın yapılandırmasının öncelikli olmasını mı istediğinize bağlı olarak iki yaklaşımdan yararlanabilir:</span><span class="sxs-lookup"><span data-stu-id="a4be8-182">There are two approaches to handling configuration depending on whether you want the hosting startup's configuration to take precedence or the app's configuration to take precedence:</span></span>

1. <span data-ttu-id="a4be8-183">Uygulamanın <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> temsilcileri çalıştırıldıktan sonra yapılandırmayı yüklemek için <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> kullanarak uygulamaya yapılandırma sağlayın.</span><span class="sxs-lookup"><span data-stu-id="a4be8-183">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> to load the configuration after the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="a4be8-184">Barındırma başlangıç yapılandırması, uygulamanın yapılandırmasına bu yaklaşımı kullanarak öncelik kazanır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-184">Hosting startup configuration takes priority over the app's configuration using this approach.</span></span>
1. <span data-ttu-id="a4be8-185">Uygulamanın <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> temsilcileri yürütmeden önce yapılandırmayı yüklemek için <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> kullanarak uygulamaya yapılandırma sağlayın.</span><span class="sxs-lookup"><span data-stu-id="a4be8-185">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> to load the configuration before the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="a4be8-186">Uygulamanın yapılandırma değerleri, bu yaklaşımı kullanarak barındırma başlatma tarafından sağlananlara göre önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-186">The app's configuration values take priority over those provided by the hosting startup using this approach.</span></span>

```csharp
public class ConfigurationInjection : IHostingStartup
{
    public void Configure(IWebHostBuilder builder)
    {
        Dictionary<string, string> dict;

        builder.ConfigureAppConfiguration(config =>
        {
            dict = new Dictionary<string, string>
            {
                {"ConfigurationKey1", 
                    "From IHostingStartup: Higher priority " +
                    "than the app's configuration."},
            };

            config.AddInMemoryCollection(dict);
        });

        dict = new Dictionary<string, string>
        {
            {"ConfigurationKey2", 
                "From IHostingStartup: Lower priority " +
                "than the app's configuration."},
        };

        var builtConfig = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        builder.UseConfiguration(builtConfig);
    }
}
```

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="a4be8-187">Barındırma başlangıç derlemeyi belirtin</span><span class="sxs-lookup"><span data-stu-id="a4be8-187">Specify the hosting startup assembly</span></span>

<span data-ttu-id="a4be8-188">Bir sınıf kitaplığı ya da konsol uygulaması tarafından sağlanan bir barındırma başlatması için, `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeninde barındırma başlangıç derlemesinin adını belirtin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-188">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="a4be8-189">Ortam değişkenini derlemelerin noktalı virgülle ayrılmış bir listedir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-189">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="a4be8-190">`HostingStartup` özniteliği için yalnızca barındırma başlangıç derlemeleri taranır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-190">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="a4be8-191">Daha önce açıklanan barındırma başlangıç pencerelerini öğrenmek için *Hostingstartupapp*örnek uygulaması için, ortam değişkeni aşağıdaki değere ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="a4be8-191">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="a4be8-192">Barındırma başlangıç bütünleştirilmiş kodları, barındırma başlangıç derlemeleri ana bilgisayar yapılandırma ayarı kullanılarak da ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="a4be8-192">A hosting startup assembly can also be set using the Hosting Startup Assemblies host configuration setting:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseSetting(
                    WebHostDefaults.HostingStartupAssembliesKey, 
                    "{ASSEMBLY1;ASSEMBLY2; ...}")
                .UseStartup<Startup>();
        });
```

<span data-ttu-id="a4be8-193">Birden çok barındırma başlatması varsa, <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> yöntemleri derlemelerin listelendiği sırada yürütülür.</span><span class="sxs-lookup"><span data-stu-id="a4be8-193">When multiple hosting startup assembles are present, their <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="a4be8-194">Etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="a4be8-194">Activation</span></span>

<span data-ttu-id="a4be8-195">Başlangıç etkinleştirme barındırmak için Seçenekler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="a4be8-195">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="a4be8-196">[Çalışma zamanı deposu](#runtime-store) &ndash; etkinleştirme, etkinleştirme için derleme zamanı başvurusu gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="a4be8-196">[Runtime store](#runtime-store) &ndash; Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="a4be8-197">Örnek uygulama, çok makineli bir ortamda barındırma başlatmasının dağıtımını kolaylaştırmak için barındırma başlangıç derlemesini ve bağımlılıklar dosyalarını bir klasöre, *dağıtımına*koyar.</span><span class="sxs-lookup"><span data-stu-id="a4be8-197">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="a4be8-198">*Dağıtım* klasörü Ayrıca, barındırma başlangıcını etkinleştirmek için dağıtım sistemindeki ortam değişkenlerini oluşturan veya değiştiren bir PowerShell betiği içerir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-198">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="a4be8-199">Etkinleştirme için gerekli derleme zamanı başvurusu</span><span class="sxs-lookup"><span data-stu-id="a4be8-199">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="a4be8-200">NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="a4be8-200">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="a4be8-201">Proje bin klasörü</span><span class="sxs-lookup"><span data-stu-id="a4be8-201">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="a4be8-202">Çalışma zamanı deposu</span><span class="sxs-lookup"><span data-stu-id="a4be8-202">Runtime store</span></span>

<span data-ttu-id="a4be8-203">Barındırma başlangıç uygulamasının [çalışma zamanı deposuna](/dotnet/core/deploying/runtime-store)yerleştirilmesi.</span><span class="sxs-lookup"><span data-stu-id="a4be8-203">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="a4be8-204">Gelişmiş uygulama tarafından bir derleme zamanı başvurusu derleme için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-204">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="a4be8-205">Barındırma başlatma oluşturulduktan sonra, bildirim proje dosyası ve [DotNet Store](/dotnet/core/tools/dotnet-store) komutu kullanılarak bir çalışma zamanı deposu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a4be8-205">After the hosting startup is built, a runtime store is generated using the manifest project file and the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```dotnetcli
dotnet store --manifest {MANIFEST FILE} --runtime {RUNTIME IDENTIFIER} --output {OUTPUT LOCATION} --skip-optimization
```

<span data-ttu-id="a4be8-206">Örnek uygulamada (*Runtimesstore* Projesi) aşağıdaki komut kullanılır:</span><span class="sxs-lookup"><span data-stu-id="a4be8-206">In the sample app (*RuntimeStore* project) the following command is used:</span></span>

```dotnetcli
dotnet store --manifest store.manifest.csproj --runtime win7-x64 --output ./deployment/store --skip-optimization
```

<span data-ttu-id="a4be8-207">Çalışma zamanının çalışma zamanı deposunu bulması için, çalışma zamanı deposunun konumu `DOTNET_SHARED_STORE` ortam değişkenine eklenir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-207">For the runtime to discover the runtime store, the runtime store's location is added to the `DOTNET_SHARED_STORE` environment variable.</span></span>

<span data-ttu-id="a4be8-208">**Barındırma başlatmasının bağımlılıklar dosyasını değiştirme ve yerleştirme**</span><span class="sxs-lookup"><span data-stu-id="a4be8-208">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="a4be8-209">Geliştirmede bir paket başvurusu olmadan geliştirmeyi etkinleştirmek için, `additionalDeps`çalışma zamanına ek bağımlılıklar belirtin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-209">To activate the enhancement without a package reference to the enhancement, specify additional dependencies to the runtime with `additionalDeps`.</span></span> <span data-ttu-id="a4be8-210">`additionalDeps` şunları yapmanıza olanak sağlar:</span><span class="sxs-lookup"><span data-stu-id="a4be8-210">`additionalDeps` allows you to:</span></span>

* <span data-ttu-id="a4be8-211">Başlangıçta uygulamanın kendi *. Deps* . JSON dosyasıyla birleştirilecek bir dizi ek *. Deps. JSON* dosyası sağlayarak uygulamanın kitaplık grafiğini genişletin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-211">Extend the app's library graph by providing a set of additional *.deps.json* files to merge with the app's own *.deps.json* file on startup.</span></span>
* <span data-ttu-id="a4be8-212">Barındırma başlangıç derlemesine bulunabilir ve yüklenebilir olun.</span><span class="sxs-lookup"><span data-stu-id="a4be8-212">Make the hosting startup assembly discoverable and loadable.</span></span>

<span data-ttu-id="a4be8-213">Ek Bağımlılıklar dosyası oluşturmak için önerilen yaklaşımdır:</span><span class="sxs-lookup"><span data-stu-id="a4be8-213">The recommended approach for generating the additional dependencies file is to:</span></span>

 1. <span data-ttu-id="a4be8-214">Önceki bölümde başvurulan çalışma zamanı deposu bildirim dosyasında `dotnet publish` yürütün.</span><span class="sxs-lookup"><span data-stu-id="a4be8-214">Execute `dotnet publish` on the runtime store manifest file referenced in the previous section.</span></span>
 1. <span data-ttu-id="a4be8-215">Bildirimlerin bildirim başvurusunu ve sonuçta elde edilen *. Deps. JSON* dosyasının `runtime` bölümünü kaldırın.</span><span class="sxs-lookup"><span data-stu-id="a4be8-215">Remove the manifest reference from libraries and the `runtime` section of the resulting *.deps.json* file.</span></span>

<span data-ttu-id="a4be8-216">Örnek projede `store.manifest/1.0.0` özelliği `targets` ve `libraries` bölümünden kaldırılır:</span><span class="sxs-lookup"><span data-stu-id="a4be8-216">In the example project, the `store.manifest/1.0.0` property is removed from the `targets` and `libraries` section:</span></span>

```json
{
  "runtimeTarget": {
    "name": ".NETCoreApp,Version=v3.0",
    "signature": ""
  },
  "compilationOptions": {},
  "targets": {
    ".NETCoreApp,Version=v3.0": {
      "store.manifest/1.0.0": {
        "dependencies": {
          "StartupDiagnostics": "1.0.0"
        },
        "runtime": {
          "store.manifest.dll": {}
        }
      },
      "StartupDiagnostics/1.0.0": {
        "runtime": {
          "lib/netcoreapp3.0/StartupDiagnostics.dll": {
            "assemblyVersion": "1.0.0.0",
            "fileVersion": "1.0.0.0"
          }
        }
      }
    }
  },
  "libraries": {
    "store.manifest/1.0.0": {
      "type": "project",
      "serviceable": false,
      "sha512": ""
    },
    "StartupDiagnostics/1.0.0": {
      "type": "package",
      "serviceable": true,
      "sha512": "sha512-xrhzuNSyM5/f4ZswhooJ9dmIYLP64wMnqUJSyTKVDKDVj5T+qtzypl8JmM/aFJLLpYrf0FYpVWvGujd7/FfMEw==",
      "path": "startupdiagnostics/1.0.0",
      "hashPath": "startupdiagnostics.1.0.0.nupkg.sha512"
    }
  }
}
```

<span data-ttu-id="a4be8-217">*. Deps. JSON* dosyasını şu konuma yerleştirin:</span><span class="sxs-lookup"><span data-stu-id="a4be8-217">Place the *.deps.json* file into the following location:</span></span>

```
{ADDITIONAL DEPENDENCIES PATH}/shared/{SHARED FRAMEWORK NAME}/{SHARED FRAMEWORK VERSION}/{ENHANCEMENT ASSEMBLY NAME}.deps.json
```

* <span data-ttu-id="a4be8-218">`DOTNET_ADDITIONAL_DEPS` ortam değişkenine eklenen `{ADDITIONAL DEPENDENCIES PATH}` &ndash; konumu.</span><span class="sxs-lookup"><span data-stu-id="a4be8-218">`{ADDITIONAL DEPENDENCIES PATH}` &ndash; Location added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
* <span data-ttu-id="a4be8-219">Bu ek bağımlılıklar dosyası için gerekli `{SHARED FRAMEWORK NAME}` &ndash; paylaşılan çerçeve.</span><span class="sxs-lookup"><span data-stu-id="a4be8-219">`{SHARED FRAMEWORK NAME}` &ndash; Shared framework required for this additional dependencies file.</span></span>
* <span data-ttu-id="a4be8-220">`{SHARED FRAMEWORK VERSION}` &ndash; en düşük paylaşılan Framework sürümü.</span><span class="sxs-lookup"><span data-stu-id="a4be8-220">`{SHARED FRAMEWORK VERSION}` &ndash; Minimum shared framework version.</span></span>
* <span data-ttu-id="a4be8-221">geliştirmesinin derleme adı &ndash; `{ENHANCEMENT ASSEMBLY NAME}`.</span><span class="sxs-lookup"><span data-stu-id="a4be8-221">`{ENHANCEMENT ASSEMBLY NAME}` &ndash; The enhancement's assembly name.</span></span>

<span data-ttu-id="a4be8-222">Örnek uygulamada (*Runtimesbir* proje), ek bağımlılıklar dosyası aşağıdaki konuma yerleştirilir:</span><span class="sxs-lookup"><span data-stu-id="a4be8-222">In the sample app (*RuntimeStore* project), the additional dependencies file is placed into the following location:</span></span>

```
deployment/additionalDeps/shared/Microsoft.AspNetCore.App/3.0.0/StartupDiagnostics.deps.json
```

<span data-ttu-id="a4be8-223">Çalışma zamanı depo konumunu bulması için, ek bağımlılıklar dosya konumu `DOTNET_ADDITIONAL_DEPS` ortam değişkenine eklenir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-223">For runtime to discover the runtime store location, the additional dependencies file location is added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="a4be8-224">Örnek uygulamada (*Runtimesbir* proje), çalışma zamanı deposunun oluşturulması ve ek bağımlılıklar dosyası oluşturulması bir [PowerShell](/powershell/scripting/powershell-scripting) betiği kullanılarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-224">In the sample app (*RuntimeStore* project), building the runtime store and generating the additional dependencies file is accomplished using a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span>

<span data-ttu-id="a4be8-225">Çeşitli işletim sistemleri için ortam değişkenlerinin nasıl ayarlanbileceğine ilişkin örnekler için, bkz. [birden çok ortam kullanma](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="a4be8-225">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="a4be8-226">**Dağıtım**</span><span class="sxs-lookup"><span data-stu-id="a4be8-226">**Deployment**</span></span>

<span data-ttu-id="a4be8-227">Çoklu makine ortamında bir barındırma başlatmasının dağıtımını kolaylaştırmak için, örnek uygulama, yayımlanan çıktıda şunları içeren bir *dağıtım* klasörü oluşturur:</span><span class="sxs-lookup"><span data-stu-id="a4be8-227">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="a4be8-228">Barındırma başlangıç çalışma zamanı deposu.</span><span class="sxs-lookup"><span data-stu-id="a4be8-228">The hosting startup runtime store.</span></span>
* <span data-ttu-id="a4be8-229">Barındırma başlangıç bağımlılıkları dosyası.</span><span class="sxs-lookup"><span data-stu-id="a4be8-229">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="a4be8-230">Barındırma başlangıcını etkinleştirmeyi desteklemek için `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`ve `DOTNET_ADDITIONAL_DEPS` oluşturan veya değiştiren bir PowerShell betiği.</span><span class="sxs-lookup"><span data-stu-id="a4be8-230">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="a4be8-231">Komut dosyası dağıtım sistemde bir yönetici PowerShell komut isteminden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a4be8-231">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="a4be8-232">NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="a4be8-232">NuGet package</span></span>

<span data-ttu-id="a4be8-233">Bir NuGet paketi barındırma bir başlangıç geliştirmesi sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-233">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="a4be8-234">Pakette bir `HostingStartup` özniteliği vardır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-234">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="a4be8-235">Paket tarafından sağlanan barındırma başlangıç türleri, aşağıdaki yaklaşımlardan birini kullanarak uygulama için kullanıma sunulur:</span><span class="sxs-lookup"><span data-stu-id="a4be8-235">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="a4be8-236">Gelişmiş uygulamanın proje dosyası, barındırma başlangıç için bir paket başvurusu uygulamanın proje dosyası (bir derleme zamanı Başvurusu) sağlar.</span><span class="sxs-lookup"><span data-stu-id="a4be8-236">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="a4be8-237">Derleme zamanı başvurusu yerinde olduğunda, barındırma başlangıç derlemesi ve tüm bağımlılıkları uygulamanın bağımlılık dosyasına ( *. Deps. JSON*) dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-237">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="a4be8-238">Bu yaklaşım, [NuGet.org](https://www.nuget.org/)'e yayınlanan bir barındırma başlangıç derleme paketi için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-238">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="a4be8-239">Barındırma başlatmasının bağımlılıklar dosyası, [çalışma zamanı deposu](#runtime-store) bölümünde açıklandığı gibi gelişmiş uygulama için kullanılabilir hale getirilir (derleme zamanı başvurusu olmadan).</span><span class="sxs-lookup"><span data-stu-id="a4be8-239">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="a4be8-240">NuGet paketlerini ve çalışma zamanı mağazası hakkında daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="a4be8-240">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="a4be8-241">Platformlar arası araçlarla bir NuGet paketi oluşturma</span><span class="sxs-lookup"><span data-stu-id="a4be8-241">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="a4be8-242">Paketler yayımlanıyor</span><span class="sxs-lookup"><span data-stu-id="a4be8-242">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="a4be8-243">Çalışma zamanı paket deposu</span><span class="sxs-lookup"><span data-stu-id="a4be8-243">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="a4be8-244">Proje bin klasörü</span><span class="sxs-lookup"><span data-stu-id="a4be8-244">Project bin folder</span></span>

<span data-ttu-id="a4be8-245">Bir barındırma başlatma geliştirmesi, gelişmiş uygulamada, *bin*ile dağıtılan bir derleme tarafından sağlanıyor.</span><span class="sxs-lookup"><span data-stu-id="a4be8-245">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="a4be8-246">Derleme tarafından sağlanan barındırma başlangıç türleri, aşağıdaki yaklaşımlardan biri kullanılarak uygulama için kullanılabilir hale getirilir:</span><span class="sxs-lookup"><span data-stu-id="a4be8-246">The hosting startup types provided by the assembly are made available to the app using one of the following approaches:</span></span>

* <span data-ttu-id="a4be8-247">Gelişmiş uygulamanın proje dosyası, barındırma başlangıç (bir derleme zamanı başvuru) bir bütünleştirilmiş kod başvurusu yapar.</span><span class="sxs-lookup"><span data-stu-id="a4be8-247">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="a4be8-248">Derleme zamanı başvurusu yerinde olduğunda, barındırma başlangıç derlemesi ve tüm bağımlılıkları uygulamanın bağımlılık dosyasına ( *. Deps. JSON*) dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-248">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="a4be8-249">Bu yaklaşım, dağıtım senaryosu barındırma başlatmasının derlemesine ( *. dll* dosyası) bir derleme zamanı başvurusu yapmak ve derlemeyi şu şekilde taşımak için çağırdığında geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="a4be8-249">This approach applies when the deployment scenario calls for making a compile-time reference to the hosting startup's assembly (*.dll* file) and moving the assembly to either:</span></span>
  * <span data-ttu-id="a4be8-250">Tüketen proje.</span><span class="sxs-lookup"><span data-stu-id="a4be8-250">The consuming project.</span></span>
  * <span data-ttu-id="a4be8-251">Tüketim Projesi tarafından erişilebilen bir konum.</span><span class="sxs-lookup"><span data-stu-id="a4be8-251">A location accessible by the consuming project.</span></span>
* <span data-ttu-id="a4be8-252">Barındırma başlatmasının bağımlılıklar dosyası, [çalışma zamanı deposu](#runtime-store) bölümünde açıklandığı gibi gelişmiş uygulama için kullanılabilir hale getirilir (derleme zamanı başvurusu olmadan).</span><span class="sxs-lookup"><span data-stu-id="a4be8-252">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>
* <span data-ttu-id="a4be8-253">.NET Framework hedeflenirken, derleme varsayılan yükleme bağlamında yüklenebilir olur; bu, .NET Framework, derlemenin aşağıdaki konumlardan birinde bulunduğu anlamına gelir:</span><span class="sxs-lookup"><span data-stu-id="a4be8-253">When targeting the .NET Framework, the assembly is loadable in the default load context, which on .NET Framework means that the assembly is located at either of the following locations:</span></span>
  * <span data-ttu-id="a4be8-254">Uygulama temel yolu, uygulamanın yürütülebilir dosyasının ( *. exe*) bulunduğu *bin* klasörünü &ndash;.</span><span class="sxs-lookup"><span data-stu-id="a4be8-254">Application base path &ndash; The *bin* folder where the app's executable (*.exe*) is located.</span></span>
  * <span data-ttu-id="a4be8-255">Genel bütünleştirilmiş kod önbelleği (GAC) &ndash; GAC, birkaç .NET Framework uygulamanın paylaştığı derlemeleri depolar.</span><span class="sxs-lookup"><span data-stu-id="a4be8-255">Global Assembly Cache (GAC) &ndash; The GAC stores assemblies that several .NET Framework apps share.</span></span> <span data-ttu-id="a4be8-256">Daha fazla bilgi için, bkz. [nasıl yapılır: bir derlemeyi genel derleme önbelleğine yüklemek](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) .NET Framework belgeleri.</span><span class="sxs-lookup"><span data-stu-id="a4be8-256">For more information, see [How to: Install an assembly into the global assembly cache](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) in the .NET Framework documentation.</span></span>

## <a name="sample-code"></a><span data-ttu-id="a4be8-257">Örnek kod</span><span class="sxs-lookup"><span data-stu-id="a4be8-257">Sample code</span></span>

<span data-ttu-id="a4be8-258">[Örnek kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample)), başlangıç uygulama senaryolarını barındırma gösterir:</span><span class="sxs-lookup"><span data-stu-id="a4be8-258">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="a4be8-259">İki barındırma başlangıç derlemeleri (sınıf kitaplıkları), bellek içi yapılandırma anahtar-değer çiftleri her bir çifti ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="a4be8-259">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="a4be8-260">NuGet paketi (*Hostingstartuppackage*)</span><span class="sxs-lookup"><span data-stu-id="a4be8-260">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="a4be8-261">Sınıf kitaplığı (*Hostingstartuplibrary*)</span><span class="sxs-lookup"><span data-stu-id="a4be8-261">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="a4be8-262">Bir barındırma başlatması, çalışma zamanı deposu tarafından dağıtılan bir derlemeden (*Startupdiagnostics*) etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-262">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="a4be8-263">Derleme, uygulamaya tanılama bilgileri sağlayan başlangıçta iki middlewares ekler:</span><span class="sxs-lookup"><span data-stu-id="a4be8-263">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="a4be8-264">Kaydedilen Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="a4be8-264">Registered services</span></span>
  * <span data-ttu-id="a4be8-265">Adres (Düzen, konak, temel yolu, yol, sorgu dizesi)</span><span class="sxs-lookup"><span data-stu-id="a4be8-265">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="a4be8-266">Bağlantı (uzak IP, uzak bağlantı noktasını, yerel IP yerel bağlantı noktası, istemci sertifikası)</span><span class="sxs-lookup"><span data-stu-id="a4be8-266">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="a4be8-267">İstek üst bilgileri</span><span class="sxs-lookup"><span data-stu-id="a4be8-267">Request headers</span></span>
  * <span data-ttu-id="a4be8-268">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="a4be8-268">Environment variables</span></span>

<span data-ttu-id="a4be8-269">Örneği çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="a4be8-269">To run the sample:</span></span>

<span data-ttu-id="a4be8-270">**NuGet paketinden etkinleştirme**</span><span class="sxs-lookup"><span data-stu-id="a4be8-270">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="a4be8-271">,, [DotNet paketi](/dotnet/core/tools/dotnet-pack) komutuyla *hostingstartuppackage* paketini derleyin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-271">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="a4be8-272">*Hostingstartuppackage* 'in derleme adını `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkenine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-272">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="a4be8-273">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a4be8-273">Compile and run the app.</span></span> <span data-ttu-id="a4be8-274">Geliştirilmiş bir uygulamada (bir derleme zamanı Başvurusu) bir paket başvurusu yok.</span><span class="sxs-lookup"><span data-stu-id="a4be8-274">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="a4be8-275">Uygulamanın proje dosyasındaki bir `<PropertyGroup>` paket projesinin çıkışını belirtir ( *.. /HostingStartupPackage/bin/Debug*) bir paket kaynağı olarak.</span><span class="sxs-lookup"><span data-stu-id="a4be8-275">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="a4be8-276">Bu, uygulamanın paketi [NuGet.org](https://www.nuget.org/)'e yüklemeden paketi kullanmasına izin verir. Daha fazla bilgi için HostingStartupApp öğesinin proje dosyasındaki notlara bakın.</span><span class="sxs-lookup"><span data-stu-id="a4be8-276">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```

1. <span data-ttu-id="a4be8-277">Dizin sayfası tarafından oluşturulan hizmet yapılandırma anahtarı değerlerinin, paketin `ServiceKeyInjection.Configure` yöntemi tarafından ayarlanan değerlerle eşleştiğini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-277">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="a4be8-278">*Hostingstartuppackage* projesinde değişiklik yaparsanız ve yeniden derleyseniz, *Hostingstartupapp* ' ın yerel önbellekten eski bir paket değil, güncelleştirilmiş paketi aldığından emin olmak için yerel NuGet paket önbelleklerini temizleyin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-278">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="a4be8-279">Yerel NuGet önbelleklerini temizlemek için aşağıdaki [DotNet NuGet Yereller](/dotnet/core/tools/dotnet-nuget-locals) komutunu yürütün:</span><span class="sxs-lookup"><span data-stu-id="a4be8-279">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```dotnetcli
dotnet nuget locals all --clear
```

<span data-ttu-id="a4be8-280">**Bir sınıf kitaplığından etkinleştirme**</span><span class="sxs-lookup"><span data-stu-id="a4be8-280">**Activation from a class library**</span></span>

1. <span data-ttu-id="a4be8-281">, [DotNet Build](/dotnet/core/tools/dotnet-build) komutuyla *hostingstartuplibrary* sınıf kitaplığını derleyin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-281">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="a4be8-282">*Hostingstartuplibrary* 'in sınıf kitaplığının derleme adını `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkenine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-282">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="a4be8-283">*bin*- *hostingstartuplibrary. dll* dosyasını sınıf kitaplığının derlenmiş çıktısından uygulamanın *bin/Debug* klasörüne kopyalayarak uygulamaya sınıf kitaplığının derlemesini dağıtın.</span><span class="sxs-lookup"><span data-stu-id="a4be8-283">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="a4be8-284">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a4be8-284">Compile and run the app.</span></span> <span data-ttu-id="a4be8-285">Uygulamanın proje dosyasındaki bir `<ItemGroup>`, sınıf kitaplığının derlemesine ( *.\Bin\debug\netcoreapp3,\hostingstartuplibrary.dll*) başvurur (bir derleme zamanı başvurusu).</span><span class="sxs-lookup"><span data-stu-id="a4be8-285">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp3.0\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="a4be8-286">Notları HostingStartupApp'ın proje dosyasında daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="a4be8-286">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\\bin\\Debug\\netcoreapp3.0\\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp3.0\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion> 
     </Reference>
   </ItemGroup>
   ```

1. <span data-ttu-id="a4be8-287">Dizin sayfası tarafından oluşturulan hizmet yapılandırma anahtarı değerlerinin, sınıf kitaplığının `ServiceKeyInjection.Configure` yöntemi tarafından ayarlanan değerlerle eşleştiğini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-287">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="a4be8-288">**Çalışma zamanı deposu tarafından dağıtılan bir derlemeden etkinleştirme**</span><span class="sxs-lookup"><span data-stu-id="a4be8-288">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="a4be8-289">*Startupdiagnostics* projesi, *startupdiagnostics. Deps. JSON* dosyasını değiştirmek için [PowerShell](/powershell/scripting/powershell-scripting) kullanır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-289">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="a4be8-290">PowerShell, Windows 7 SP1 ve Windows Server 2008 R2 SP1 ile başlayarak Windows üzerinde varsayılan olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-290">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="a4be8-291">Diğer platformlarda PowerShell 'i almak için bkz. [Windows PowerShell 'ı yükleme](/powershell/scripting/setup/installing-powershell#powershell-core).</span><span class="sxs-lookup"><span data-stu-id="a4be8-291">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span></span>
1. <span data-ttu-id="a4be8-292">*Runtimesyürüme* klasöründe *Build. ps1* betiğini yürütün.</span><span class="sxs-lookup"><span data-stu-id="a4be8-292">Execute the *build.ps1* script in the *RuntimeStore* folder.</span></span> <span data-ttu-id="a4be8-293">Betik:</span><span class="sxs-lookup"><span data-stu-id="a4be8-293">The script:</span></span>
   * <span data-ttu-id="a4be8-294">`StartupDiagnostics` paketini *obj\packages* klasöründe oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a4be8-294">Generates the `StartupDiagnostics` package in the *obj\packages* folder.</span></span>
   * <span data-ttu-id="a4be8-295">*Mağaza* klasöründeki `StartupDiagnostics` çalışma zamanı deposunu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a4be8-295">Generates the runtime store for `StartupDiagnostics` in the *store* folder.</span></span> <span data-ttu-id="a4be8-296">Betikteki `dotnet store` komutu, Windows 'a dağıtılan bir barındırma başlatması için `win7-x64` [çalışma zamanı tanımlayıcısı 'nı (RID)](/dotnet/core/rid-catalog) kullanır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-296">The `dotnet store` command in the script uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog) for a hosting startup deployed to Windows.</span></span> <span data-ttu-id="a4be8-297">Farklı bir çalışma zamanı için barındırma başlangıcını sağlarken, betiğin 37. satırındaki doğru RID 'yi yerine koyun.</span><span class="sxs-lookup"><span data-stu-id="a4be8-297">When providing the hosting startup for a different runtime, substitute the correct RID on line 37 of the script.</span></span> <span data-ttu-id="a4be8-298">`StartupDiagnostics` çalışma zamanı deposu daha sonra derlemenin tüketilebileceği makinede kullanıcının veya sistem çalışma zamanı deposuna taşınır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-298">The runtime store for `StartupDiagnostics` would later be moved to the user's or system's runtime store on the machine where the assembly will be consumed.</span></span> <span data-ttu-id="a4be8-299">`StartupDiagnostics` derlemesinin Kullanıcı çalışma zamanı deposu yüklemesi konumu *. DotNet/Store/x64/netcoreapp 3.0/startupdiagnostics/1.0.0/LIB/netcoreapp 3.0/startupdiagnostics. dll*' dir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-299">The user runtime store install location for the `StartupDiagnostics` assembly is *.dotnet/store/x64/netcoreapp3.0/startupdiagnostics/1.0.0/lib/netcoreapp3.0/StartupDiagnostics.dll*.</span></span>
   * <span data-ttu-id="a4be8-300">*Additionaldeps* klasöründeki `StartupDiagnostics` için `additionalDeps` üretir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-300">Generates the `additionalDeps` for `StartupDiagnostics` in the *additionalDeps* folder.</span></span> <span data-ttu-id="a4be8-301">Ek bağımlılıklar daha sonra kullanıcının veya sistem ek bağımlılıklarına taşınır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-301">The additional dependencies would later be moved to the user's or system's additional dependencies.</span></span> <span data-ttu-id="a4be8-302">Kullanıcı `StartupDiagnostics` ek bağımlılıklar yüklemesi konumu *. DotNet/x64/additionalDeps/startupdiagnostics/Shared/Microsoft. NETCore. app/3.0.0/StartupDiagnostics. Deps. JSON*olur.</span><span class="sxs-lookup"><span data-stu-id="a4be8-302">The user `StartupDiagnostics` additional dependencies install location is *.dotnet/x64/additionalDeps/StartupDiagnostics/shared/Microsoft.NETCore.App/3.0.0/StartupDiagnostics.deps.json*.</span></span>
   * <span data-ttu-id="a4be8-303">*Dağıtım* klasörüne *Deploy. ps1* dosyasını koyar.</span><span class="sxs-lookup"><span data-stu-id="a4be8-303">Places the *deploy.ps1* file in the *deployment* folder.</span></span>
1. <span data-ttu-id="a4be8-304">*Dağıtım* klasöründe *Deploy. ps1* betiğini çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a4be8-304">Run the *deploy.ps1* script in the *deployment* folder.</span></span> <span data-ttu-id="a4be8-305">Betik şunu ekler:</span><span class="sxs-lookup"><span data-stu-id="a4be8-305">The script appends:</span></span>
   * <span data-ttu-id="a4be8-306">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkenine `StartupDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="a4be8-306">`StartupDiagnostics` to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="a4be8-307">Barındırma başlangıç bağımlılıkları yolu (Runtimessımında projenin *dağıtım* klasöründe) `DOTNET_ADDITIONAL_DEPS` ortam değişkenine.</span><span class="sxs-lookup"><span data-stu-id="a4be8-307">The hosting startup dependencies path (in the RuntimeStore project's *deployment* folder) to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
   * <span data-ttu-id="a4be8-308">Çalışma zamanı depolama yolu (Runtimes, projenin *dağıtım* klasöründe) `DOTNET_SHARED_STORE` ortam değişkenine.</span><span class="sxs-lookup"><span data-stu-id="a4be8-308">The runtime store path (in the RuntimeStore project's *deployment* folder) to the `DOTNET_SHARED_STORE` environment variable.</span></span>
1. <span data-ttu-id="a4be8-309">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a4be8-309">Run the sample app.</span></span>
1. <span data-ttu-id="a4be8-310">Uygulamanın kayıtlı hizmetlerini görmek için `/services` uç noktası isteyin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-310">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="a4be8-311">Tanılama bilgilerini görmek için `/diag` uç noktası isteyin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-311">Request the `/diag` endpoint to see the diagnostic information.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a4be8-312"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> (barındırma başlatma) uygulaması, bir dış derlemeden başlatma sırasında bir uygulamaya iyileştirmeler ekler.</span><span class="sxs-lookup"><span data-stu-id="a4be8-312">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="a4be8-313">Örneğin, bir harici kitaplık ek yapılandırma sağlayıcıları ya da bir uygulama hizmetlerini barındıran bir başlangıç uygulaması kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a4be8-313">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span>

<span data-ttu-id="a4be8-314">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a4be8-314">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="a4be8-315">HostingStartup özniteliği</span><span class="sxs-lookup"><span data-stu-id="a4be8-315">HostingStartup attribute</span></span>

<span data-ttu-id="a4be8-316">[Hostingstartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) özniteliği, çalışma zamanında etkinleştirilecek bir barındırma başlangıç derlemesinin varlığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-316">A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="a4be8-317">Giriş derlemesi veya `Startup` sınıfını içeren derleme `HostingStartup` özniteliği için otomatik olarak taranır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-317">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="a4be8-318">`HostingStartup` özniteliklerin aranacağı derlemelerin listesi [Webhostdefaults. HostingStartupAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupAssembliesKey)içindeki yapılandırmadan çalışma zamanında yüklenir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-318">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupAssembliesKey).</span></span> <span data-ttu-id="a4be8-319">Bulmadan dışlanacak derlemelerin listesi [Webhostdefaults. Hostingstartupexcludederlemelieskey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupExcludeAssembliesKey)öğesinden yüklendi.</span><span class="sxs-lookup"><span data-stu-id="a4be8-319">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupExcludeAssembliesKey).</span></span> <span data-ttu-id="a4be8-320">Daha fazla bilgi için bkz. [Web Konağı: barındırma başlangıç derlemeleri](xref:fundamentals/host/web-host#hosting-startup-assemblies) ve [Web Konağı: barındırma başlatma derlemeleri çıkarma](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="a4be8-320">For more information, see [Web Host: Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) and [Web Host: Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span></span>

<span data-ttu-id="a4be8-321">Aşağıdaki örnekte, barındırma başlangıç derlemesinin ad alanı `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="a4be8-321">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="a4be8-322">Barındırma başlangıç kodunu içeren sınıf `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="a4be8-322">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="a4be8-323">`HostingStartup` özniteliği genellikle barındırma başlangıç derlemesinin `IHostingStartup` uygulama sınıfı dosyasında bulunur.</span><span class="sxs-lookup"><span data-stu-id="a4be8-323">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="a4be8-324">Yüklenen barındırma başlangıç derlemeler keşfedin</span><span class="sxs-lookup"><span data-stu-id="a4be8-324">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="a4be8-325">Yüklenen barındırma başlangıç derlemeleri bulmak için günlük kaydını etkinleştirmek ve uygulama günlüklerini kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-325">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="a4be8-326">Derlemeler yüklenirken oluşan hataları günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-326">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="a4be8-327">Yüklenen barındırma başlangıç derlemeler hata ayıklama düzeyinde kaydedilir ve tüm hatalar kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-327">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="a4be8-328">Başlangıç derlemeleri barındırma otomatik yüklemeyi devre dışı</span><span class="sxs-lookup"><span data-stu-id="a4be8-328">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="a4be8-329">Başlangıç derlemeleri barındırma otomatik yüklemeyi devre dışı bırakmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="a4be8-329">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="a4be8-330">Tüm barındırma başlangıç derlemelerinin yüklenmesini engellemek için aşağıdakilerden birini `true` veya `1`olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="a4be8-330">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>
  * <span data-ttu-id="a4be8-331">[Barındırma başlangıç](xref:fundamentals/host/web-host#prevent-hosting-startup) ana bilgisayar yapılandırma ayarını önleyin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-331">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
  * <span data-ttu-id="a4be8-332">ortam değişkeni `ASPNETCORE_PREVENTHOSTINGSTARTUP`.</span><span class="sxs-lookup"><span data-stu-id="a4be8-332">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>
* <span data-ttu-id="a4be8-333">Belirli barındırma başlangıç derlemeleri yüklenmesini önlemek için aşağıdakilerden birini başlangıçta hariç tutmak için başlangıç derlemeleri barındırma noktalı virgülle ayrılmış bir dizeye ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="a4be8-333">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>
  * <span data-ttu-id="a4be8-334">[Barındırma başlatma derlemeleri](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) ana bilgisayar yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="a4be8-334">[Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) host configuration setting.</span></span>
  * <span data-ttu-id="a4be8-335">ortam değişkeni `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="a4be8-335">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

<span data-ttu-id="a4be8-336">Hem ana bilgisayar yapılandırma ayarı ve ortam değişkenini ayarlarsanız, ana bilgisayar ayarını davranışını denetler.</span><span class="sxs-lookup"><span data-stu-id="a4be8-336">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="a4be8-337">Ana bilgisayar ayarı veya ortam değişkenini kullanarak barındırma başlangıç derlemeleri devre dışı bırakma, derlemenin genel olarak devre dışı bırakır ve uygulama çeşitli özelliklerini devre dışı bırakabilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-337">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="a4be8-338">Project</span><span class="sxs-lookup"><span data-stu-id="a4be8-338">Project</span></span>

<span data-ttu-id="a4be8-339">Bir barındırma başlatma aşağıdaki proje türlerini birini oluşturun:</span><span class="sxs-lookup"><span data-stu-id="a4be8-339">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="a4be8-340">Sınıf kitaplığı</span><span class="sxs-lookup"><span data-stu-id="a4be8-340">Class library</span></span>](#class-library)
* [<span data-ttu-id="a4be8-341">Giriş noktası olmayan konsol uygulaması</span><span class="sxs-lookup"><span data-stu-id="a4be8-341">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="a4be8-342">Sınıf kitaplığı</span><span class="sxs-lookup"><span data-stu-id="a4be8-342">Class library</span></span>

<span data-ttu-id="a4be8-343">Bir barındırma başlangıç geliştirme'de sınıf kitaplığının sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-343">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="a4be8-344">Kitaplık bir `HostingStartup` özniteliği içerir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-344">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="a4be8-345">[Örnek kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) bir Razor Pages uygulaması, *Hostingstartupapp*ve bir sınıf kitaplığı, *hostingstartuplibrary*içerir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-345">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="a4be8-346">Sınıf kitaplığı:</span><span class="sxs-lookup"><span data-stu-id="a4be8-346">The class library:</span></span>

* <span data-ttu-id="a4be8-347">`IHostingStartup`uygulayan `ServiceKeyInjection`bir barındırma başlangıç sınıfı içerir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-347">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="a4be8-348">`ServiceKeyInjection`, bellek içi yapılandırma sağlayıcısını ([Addınmemorycollection](xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*)) kullanarak uygulama yapılandırmasına bir hizmet dizesi çifti ekler.</span><span class="sxs-lookup"><span data-stu-id="a4be8-348">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*)).</span></span>
* <span data-ttu-id="a4be8-349">, Barındırma başlatmasının ad alanını ve sınıfını tanımlayan bir `HostingStartup` özniteliği içerir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-349">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="a4be8-350">`ServiceKeyInjection` sınıfının <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> yöntemi bir uygulamaya geliştirmeler eklemek için bir <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> kullanır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-350">The `ServiceKeyInjection` class's <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> method uses an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> to add enhancements to an app.</span></span>

<span data-ttu-id="a4be8-351">*Hostingstartuplibrary/ServiceKeyInjection. cs*:</span><span class="sxs-lookup"><span data-stu-id="a4be8-351">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="a4be8-352">Uygulama dizin sayfasına okur ve Sınıf Kitaplığı'nızın barındırma başlangıç derlemesi tarafından ayarlanmış iki anahtarı için yapılandırma değerlerini işler:</span><span class="sxs-lookup"><span data-stu-id="a4be8-352">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="a4be8-353">*Hostingstartupapp/Pages/Index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="a4be8-353">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="a4be8-354">[Örnek kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ayrıca ayrı bir barındırma başlatma, *Hostingstartuppackage*sağlayan bir NuGet paket projesi içerir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-354">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="a4be8-355">Paket, daha önce açıklanan Sınıf Kitaplığı'nın aynı özelliklere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-355">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="a4be8-356">Paketi:</span><span class="sxs-lookup"><span data-stu-id="a4be8-356">The package:</span></span>

* <span data-ttu-id="a4be8-357">`IHostingStartup`uygulayan `ServiceKeyInjection`bir barındırma başlangıç sınıfı içerir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-357">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="a4be8-358">`ServiceKeyInjection`, uygulamanın yapılandırmasına bir çift hizmet dizesi ekler.</span><span class="sxs-lookup"><span data-stu-id="a4be8-358">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="a4be8-359">Bir `HostingStartup` özniteliği içerir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-359">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="a4be8-360">*Hostingstartuppackage/ServiceKeyInjection. cs*:</span><span class="sxs-lookup"><span data-stu-id="a4be8-360">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="a4be8-361">Uygulama dizin sayfasına okur ve paketin barındırma başlangıç derlemesi tarafından ayarlanmış iki anahtarı için yapılandırma değerlerini işler:</span><span class="sxs-lookup"><span data-stu-id="a4be8-361">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="a4be8-362">*Hostingstartupapp/Pages/Index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="a4be8-362">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="a4be8-363">Konsol uygulaması giriş noktası olmayan</span><span class="sxs-lookup"><span data-stu-id="a4be8-363">Console app without an entry point</span></span>

<span data-ttu-id="a4be8-364">*Bu yaklaşım, .NET Framework değil yalnızca .NET Core uygulamaları için kullanılabilir.*</span><span class="sxs-lookup"><span data-stu-id="a4be8-364">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="a4be8-365">Etkinleştirme için derleme zamanı başvurusu gerektirmeyen dinamik barındırma başlatma geliştirmesi, bir `HostingStartup` özniteliği içeren giriş noktası olmadan bir konsol uygulamasında sağlanabilmesi.</span><span class="sxs-lookup"><span data-stu-id="a4be8-365">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point that contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="a4be8-366">Konsol uygulaması yayımlama, çalışma zamanı Mağazası'ndan kullanılabilen bir barındırma başlangıç bütünleştirilmiş kod üretir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-366">Publishing the console app produces a hosting startup assembly that can be consumed from the runtime store.</span></span>

<span data-ttu-id="a4be8-367">Bir konsol uygulaması giriş noktası olmayan, çünkü bu işlemde kullanılır:</span><span class="sxs-lookup"><span data-stu-id="a4be8-367">A console app without an entry point is used in this process because:</span></span>

* <span data-ttu-id="a4be8-368">Bağımlılıkları dosya barındırma başlangıç derlemedeki barındırma için başlangıç kullanmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-368">A dependencies file is required to consume the hosting startup in the hosting startup assembly.</span></span> <span data-ttu-id="a4be8-369">Kitaplık değil bir uygulama yayımlama tarafından üretilen bir çalıştırılabilir uygulama varlık bağımlılıkları dosyasıdır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-369">A dependencies file is a runnable app asset that's produced by publishing an app, not a library.</span></span>
* <span data-ttu-id="a4be8-370">Bir kitaplık, paylaşılan çalışma zamanını hedefleyen bir çalıştırılabilir proje gerektiren [çalışma zamanı paket deposuna](/dotnet/core/deploying/runtime-store)eklenemez.</span><span class="sxs-lookup"><span data-stu-id="a4be8-370">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>

<span data-ttu-id="a4be8-371">Dinamik barındırma başlangıç oluşturulmasını içinde:</span><span class="sxs-lookup"><span data-stu-id="a4be8-371">In the creation of a dynamic hosting startup:</span></span>

* <span data-ttu-id="a4be8-372">Bir giriş noktası olmadan bir barındırma başlangıç derleme konsol uygulamasından oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="a4be8-372">A hosting startup assembly is created from the console app without an entry point that:</span></span>
  * <span data-ttu-id="a4be8-373">`IHostingStartup` uygulamasını içeren bir sınıf içerir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-373">Includes a class that contains the `IHostingStartup` implementation.</span></span>
  * <span data-ttu-id="a4be8-374">`IHostingStartup` uygulama sınıfını tanımlamak için bir [Hostingstartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) özniteliği içerir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-374">Includes a [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute to identify the `IHostingStartup` implementation class.</span></span>
* <span data-ttu-id="a4be8-375">Konsol uygulaması barındırma startup şirketinizin bağımlılıkları almak için yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-375">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="a4be8-376">Kullanılmayan bağımlılıkları bağımlılıkları dosyasından atılır konsol uygulaması yayımlama bir sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="a4be8-376">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
* <span data-ttu-id="a4be8-377">Bağımlılıkları dosyası, çalışma zamanı barındırma başlangıç derleme konumunu ayarlamak için değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-377">The dependencies file is modified to set the runtime location of the hosting startup assembly.</span></span>
* <span data-ttu-id="a4be8-378">Barındırma başlangıç derleme ve bağımlılıkları dosyası çalışma zamanı paketi deposuna yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-378">The hosting startup assembly and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="a4be8-379">Barındırma başlangıç derleme ve bağımlılıkları dosyasını bulmak için bir ortam değişkenlerini çift içinde listelendikleri.</span><span class="sxs-lookup"><span data-stu-id="a4be8-379">To discover the hosting startup assembly and its dependencies file, they're listed in a pair of environment variables.</span></span>

<span data-ttu-id="a4be8-380">Konsol uygulaması [Microsoft. AspNetCore. Hosting. soyutlamalar](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) paketine başvurur:</span><span class="sxs-lookup"><span data-stu-id="a4be8-380">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

<span data-ttu-id="a4be8-381">[Hostingstartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) özniteliği, bir sınıfı <xref:Microsoft.AspNetCore.Hosting.IWebHost>oluştururken yükleme ve yürütme için `IHostingStartup` bir uygulama olarak tanımlar.</span><span class="sxs-lookup"><span data-stu-id="a4be8-381">A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the <xref:Microsoft.AspNetCore.Hosting.IWebHost>.</span></span> <span data-ttu-id="a4be8-382">Aşağıdaki örnekte, ad alanı `StartupEnhancement`ve sınıf `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="a4be8-382">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="a4be8-383">Bir sınıf `IHostingStartup`uygular.</span><span class="sxs-lookup"><span data-stu-id="a4be8-383">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="a4be8-384">Sınıfın <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> yöntemi bir uygulamaya iyileştirmeler eklemek için bir <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> kullanır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-384">The class's <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> method uses an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> to add enhancements to an app.</span></span> <span data-ttu-id="a4be8-385">barındırma başlangıç derlemesinde `IHostingStartup.Configure`, Kullanıcı kodundaki `Startup.Configure` önce çalışma zamanı tarafından çağrılır, bu da kullanıcı kodunun barındırma başlangıç derlemesi tarafından verilen yapılandırmanın üzerine yazılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-385">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="a4be8-386">Bir `IHostingStartup` projesi oluştururken, bağımlılıklar dosyası ( *. Deps. JSON*) derlemenin `runtime` konumunu *bin* klasörüne ayarlar:</span><span class="sxs-lookup"><span data-stu-id="a4be8-386">When building an `IHostingStartup` project, the dependencies file (*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="a4be8-387">Dosyanın yalnızca bir parçası olarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-387">Only part of the file is shown.</span></span> <span data-ttu-id="a4be8-388">Örnekteki derleme adı `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="a4be8-388">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="configuration-provided-by-the-hosting-startup"></a><span data-ttu-id="a4be8-389">Barındırma başlatma tarafından belirtilen yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a4be8-389">Configuration provided by the hosting startup</span></span>

<span data-ttu-id="a4be8-390">Yapılandırma işlemi, barındırma başlatmasının yapılandırmasının öncelikli olmasını mı yoksa uygulamanın yapılandırmasının öncelikli olmasını mı istediğinize bağlı olarak iki yaklaşımdan yararlanabilir:</span><span class="sxs-lookup"><span data-stu-id="a4be8-390">There are two approaches to handling configuration depending on whether you want the hosting startup's configuration to take precedence or the app's configuration to take precedence:</span></span>

1. <span data-ttu-id="a4be8-391">Uygulamanın <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> temsilcileri çalıştırıldıktan sonra yapılandırmayı yüklemek için <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> kullanarak uygulamaya yapılandırma sağlayın.</span><span class="sxs-lookup"><span data-stu-id="a4be8-391">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> to load the configuration after the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="a4be8-392">Barındırma başlangıç yapılandırması, uygulamanın yapılandırmasına bu yaklaşımı kullanarak öncelik kazanır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-392">Hosting startup configuration takes priority over the app's configuration using this approach.</span></span>
1. <span data-ttu-id="a4be8-393">Uygulamanın <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> temsilcileri yürütmeden önce yapılandırmayı yüklemek için <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> kullanarak uygulamaya yapılandırma sağlayın.</span><span class="sxs-lookup"><span data-stu-id="a4be8-393">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> to load the configuration before the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="a4be8-394">Uygulamanın yapılandırma değerleri, bu yaklaşımı kullanarak barındırma başlatma tarafından sağlananlara göre önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-394">The app's configuration values take priority over those provided by the hosting startup using this approach.</span></span>

```csharp
public class ConfigurationInjection : IHostingStartup
{
    public void Configure(IWebHostBuilder builder)
    {
        Dictionary<string, string> dict;

        builder.ConfigureAppConfiguration(config =>
        {
            dict = new Dictionary<string, string>
            {
                {"ConfigurationKey1", 
                    "From IHostingStartup: Higher priority " +
                    "than the app's configuration."},
            };

            config.AddInMemoryCollection(dict);
        });

        dict = new Dictionary<string, string>
        {
            {"ConfigurationKey2", 
                "From IHostingStartup: Lower priority " +
                "than the app's configuration."},
        };

        var builtConfig = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        builder.UseConfiguration(builtConfig);
    }
}
```

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="a4be8-395">Barındırma başlangıç derlemeyi belirtin</span><span class="sxs-lookup"><span data-stu-id="a4be8-395">Specify the hosting startup assembly</span></span>

<span data-ttu-id="a4be8-396">Bir sınıf kitaplığı ya da konsol uygulaması tarafından sağlanan bir barındırma başlatması için, `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeninde barındırma başlangıç derlemesinin adını belirtin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-396">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="a4be8-397">Ortam değişkenini derlemelerin noktalı virgülle ayrılmış bir listedir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-397">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="a4be8-398">`HostingStartup` özniteliği için yalnızca barındırma başlangıç derlemeleri taranır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-398">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="a4be8-399">Daha önce açıklanan barındırma başlangıç pencerelerini öğrenmek için *Hostingstartupapp*örnek uygulaması için, ortam değişkeni aşağıdaki değere ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="a4be8-399">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="a4be8-400">Barındırma başlangıç bütünleştirilmiş [kodları barındırma başlangıç derlemeleri](xref:fundamentals/host/web-host#hosting-startup-assemblies) ana bilgisayar yapılandırma ayarı kullanılarak da ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-400">A hosting startup assembly can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="a4be8-401">Birden çok barındırma başlatması varsa, <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> yöntemleri derlemelerin listelendiği sırada yürütülür.</span><span class="sxs-lookup"><span data-stu-id="a4be8-401">When multiple hosting startup assembles are present, their <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="a4be8-402">Etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="a4be8-402">Activation</span></span>

<span data-ttu-id="a4be8-403">Başlangıç etkinleştirme barındırmak için Seçenekler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="a4be8-403">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="a4be8-404">[Çalışma zamanı deposu](#runtime-store) &ndash; etkinleştirme, etkinleştirme için derleme zamanı başvurusu gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="a4be8-404">[Runtime store](#runtime-store) &ndash; Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="a4be8-405">Örnek uygulama, çok makineli bir ortamda barındırma başlatmasının dağıtımını kolaylaştırmak için barındırma başlangıç derlemesini ve bağımlılıklar dosyalarını bir klasöre, *dağıtımına*koyar.</span><span class="sxs-lookup"><span data-stu-id="a4be8-405">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="a4be8-406">*Dağıtım* klasörü Ayrıca, barındırma başlangıcını etkinleştirmek için dağıtım sistemindeki ortam değişkenlerini oluşturan veya değiştiren bir PowerShell betiği içerir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-406">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="a4be8-407">Etkinleştirme için gerekli derleme zamanı başvurusu</span><span class="sxs-lookup"><span data-stu-id="a4be8-407">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="a4be8-408">NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="a4be8-408">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="a4be8-409">Proje bin klasörü</span><span class="sxs-lookup"><span data-stu-id="a4be8-409">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="a4be8-410">Çalışma zamanı deposu</span><span class="sxs-lookup"><span data-stu-id="a4be8-410">Runtime store</span></span>

<span data-ttu-id="a4be8-411">Barındırma başlangıç uygulamasının [çalışma zamanı deposuna](/dotnet/core/deploying/runtime-store)yerleştirilmesi.</span><span class="sxs-lookup"><span data-stu-id="a4be8-411">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="a4be8-412">Gelişmiş uygulama tarafından bir derleme zamanı başvurusu derleme için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-412">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="a4be8-413">Barındırma başlatma oluşturulduktan sonra, bildirim proje dosyası ve [DotNet Store](/dotnet/core/tools/dotnet-store) komutu kullanılarak bir çalışma zamanı deposu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a4be8-413">After the hosting startup is built, a runtime store is generated using the manifest project file and the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```dotnetcli
dotnet store --manifest {MANIFEST FILE} --runtime {RUNTIME IDENTIFIER} --output {OUTPUT LOCATION} --skip-optimization
```

<span data-ttu-id="a4be8-414">Örnek uygulamada (*Runtimesstore* Projesi) aşağıdaki komut kullanılır:</span><span class="sxs-lookup"><span data-stu-id="a4be8-414">In the sample app (*RuntimeStore* project) the following command is used:</span></span>

```dotnetcli
dotnet store --manifest store.manifest.csproj --runtime win7-x64 --output ./deployment/store --skip-optimization
```

<span data-ttu-id="a4be8-415">Çalışma zamanının çalışma zamanı deposunu bulması için, çalışma zamanı deposunun konumu `DOTNET_SHARED_STORE` ortam değişkenine eklenir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-415">For the runtime to discover the runtime store, the runtime store's location is added to the `DOTNET_SHARED_STORE` environment variable.</span></span>

<span data-ttu-id="a4be8-416">**Barındırma başlatmasının bağımlılıklar dosyasını değiştirme ve yerleştirme**</span><span class="sxs-lookup"><span data-stu-id="a4be8-416">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="a4be8-417">Geliştirmede bir paket başvurusu olmadan geliştirmeyi etkinleştirmek için, `additionalDeps`çalışma zamanına ek bağımlılıklar belirtin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-417">To activate the enhancement without a package reference to the enhancement, specify additional dependencies to the runtime with `additionalDeps`.</span></span> <span data-ttu-id="a4be8-418">`additionalDeps` şunları yapmanıza olanak sağlar:</span><span class="sxs-lookup"><span data-stu-id="a4be8-418">`additionalDeps` allows you to:</span></span>

* <span data-ttu-id="a4be8-419">Başlangıçta uygulamanın kendi *. Deps* . JSON dosyasıyla birleştirilecek bir dizi ek *. Deps. JSON* dosyası sağlayarak uygulamanın kitaplık grafiğini genişletin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-419">Extend the app's library graph by providing a set of additional *.deps.json* files to merge with the app's own *.deps.json* file on startup.</span></span>
* <span data-ttu-id="a4be8-420">Barındırma başlangıç derlemesine bulunabilir ve yüklenebilir olun.</span><span class="sxs-lookup"><span data-stu-id="a4be8-420">Make the hosting startup assembly discoverable and loadable.</span></span>

<span data-ttu-id="a4be8-421">Ek Bağımlılıklar dosyası oluşturmak için önerilen yaklaşımdır:</span><span class="sxs-lookup"><span data-stu-id="a4be8-421">The recommended approach for generating the additional dependencies file is to:</span></span>

 1. <span data-ttu-id="a4be8-422">Önceki bölümde başvurulan çalışma zamanı deposu bildirim dosyasında `dotnet publish` yürütün.</span><span class="sxs-lookup"><span data-stu-id="a4be8-422">Execute `dotnet publish` on the runtime store manifest file referenced in the previous section.</span></span>
 1. <span data-ttu-id="a4be8-423">Bildirimlerin bildirim başvurusunu ve sonuçta elde edilen *. Deps. JSON* dosyasının `runtime` bölümünü kaldırın.</span><span class="sxs-lookup"><span data-stu-id="a4be8-423">Remove the manifest reference from libraries and the `runtime` section of the resulting *.deps.json* file.</span></span>

<span data-ttu-id="a4be8-424">Örnek projede `store.manifest/1.0.0` özelliği `targets` ve `libraries` bölümünden kaldırılır:</span><span class="sxs-lookup"><span data-stu-id="a4be8-424">In the example project, the `store.manifest/1.0.0` property is removed from the `targets` and `libraries` section:</span></span>

```json
{
  "runtimeTarget": {
    "name": ".NETCoreApp,Version=v2.1",
    "signature": "4ea77c7b75ad1895ae1ea65e6ba2399010514f99"
  },
  "compilationOptions": {},
  "targets": {
    ".NETCoreApp,Version=v2.1": {
      "store.manifest/1.0.0": {
        "dependencies": {
          "StartupDiagnostics": "1.0.0"
        },
        "runtime": {
          "store.manifest.dll": {}
        }
      },
      "StartupDiagnostics/1.0.0": {
        "runtime": {
          "lib/netcoreapp2.1/StartupDiagnostics.dll": {
            "assemblyVersion": "1.0.0.0",
            "fileVersion": "1.0.0.0"
          }
        }
      }
    }
  },
  "libraries": {
    "store.manifest/1.0.0": {
      "type": "project",
      "serviceable": false,
      "sha512": ""
    },
    "StartupDiagnostics/1.0.0": {
      "type": "package",
      "serviceable": true,
      "sha512": "sha512-oiQr60vBQW7+nBTmgKLSldj06WNLRTdhOZpAdEbCuapoZ+M2DJH2uQbRLvFT8EGAAv4TAKzNtcztpx5YOgBXQQ==",
      "path": "startupdiagnostics/1.0.0",
      "hashPath": "startupdiagnostics.1.0.0.nupkg.sha512"
    }
  }
}
```

<span data-ttu-id="a4be8-425">*. Deps. JSON* dosyasını şu konuma yerleştirin:</span><span class="sxs-lookup"><span data-stu-id="a4be8-425">Place the *.deps.json* file into the following location:</span></span>

```
{ADDITIONAL DEPENDENCIES PATH}/shared/{SHARED FRAMEWORK NAME}/{SHARED FRAMEWORK VERSION}/{ENHANCEMENT ASSEMBLY NAME}.deps.json
```

* <span data-ttu-id="a4be8-426">`DOTNET_ADDITIONAL_DEPS` ortam değişkenine eklenen `{ADDITIONAL DEPENDENCIES PATH}` &ndash; konumu.</span><span class="sxs-lookup"><span data-stu-id="a4be8-426">`{ADDITIONAL DEPENDENCIES PATH}` &ndash; Location added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
* <span data-ttu-id="a4be8-427">Bu ek bağımlılıklar dosyası için gerekli `{SHARED FRAMEWORK NAME}` &ndash; paylaşılan çerçeve.</span><span class="sxs-lookup"><span data-stu-id="a4be8-427">`{SHARED FRAMEWORK NAME}` &ndash; Shared framework required for this additional dependencies file.</span></span>
* <span data-ttu-id="a4be8-428">`{SHARED FRAMEWORK VERSION}` &ndash; en düşük paylaşılan Framework sürümü.</span><span class="sxs-lookup"><span data-stu-id="a4be8-428">`{SHARED FRAMEWORK VERSION}` &ndash; Minimum shared framework version.</span></span>
* <span data-ttu-id="a4be8-429">geliştirmesinin derleme adı &ndash; `{ENHANCEMENT ASSEMBLY NAME}`.</span><span class="sxs-lookup"><span data-stu-id="a4be8-429">`{ENHANCEMENT ASSEMBLY NAME}` &ndash; The enhancement's assembly name.</span></span>

<span data-ttu-id="a4be8-430">Örnek uygulamada (*Runtimesbir* proje), ek bağımlılıklar dosyası aşağıdaki konuma yerleştirilir:</span><span class="sxs-lookup"><span data-stu-id="a4be8-430">In the sample app (*RuntimeStore* project), the additional dependencies file is placed into the following location:</span></span>

```
deployment/additionalDeps/shared/Microsoft.AspNetCore.App/2.1.0/StartupDiagnostics.deps.json
```

<span data-ttu-id="a4be8-431">Çalışma zamanı depo konumunu bulması için, ek bağımlılıklar dosya konumu `DOTNET_ADDITIONAL_DEPS` ortam değişkenine eklenir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-431">For runtime to discover the runtime store location, the additional dependencies file location is added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="a4be8-432">Örnek uygulamada (*Runtimesbir* proje), çalışma zamanı deposunun oluşturulması ve ek bağımlılıklar dosyası oluşturulması bir [PowerShell](/powershell/scripting/powershell-scripting) betiği kullanılarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-432">In the sample app (*RuntimeStore* project), building the runtime store and generating the additional dependencies file is accomplished using a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span>

<span data-ttu-id="a4be8-433">Çeşitli işletim sistemleri için ortam değişkenlerinin nasıl ayarlanbileceğine ilişkin örnekler için, bkz. [birden çok ortam kullanma](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="a4be8-433">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="a4be8-434">**Dağıtım**</span><span class="sxs-lookup"><span data-stu-id="a4be8-434">**Deployment**</span></span>

<span data-ttu-id="a4be8-435">Çoklu makine ortamında bir barındırma başlatmasının dağıtımını kolaylaştırmak için, örnek uygulama, yayımlanan çıktıda şunları içeren bir *dağıtım* klasörü oluşturur:</span><span class="sxs-lookup"><span data-stu-id="a4be8-435">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="a4be8-436">Barındırma başlangıç çalışma zamanı deposu.</span><span class="sxs-lookup"><span data-stu-id="a4be8-436">The hosting startup runtime store.</span></span>
* <span data-ttu-id="a4be8-437">Barındırma başlangıç bağımlılıkları dosyası.</span><span class="sxs-lookup"><span data-stu-id="a4be8-437">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="a4be8-438">Barındırma başlangıcını etkinleştirmeyi desteklemek için `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`ve `DOTNET_ADDITIONAL_DEPS` oluşturan veya değiştiren bir PowerShell betiği.</span><span class="sxs-lookup"><span data-stu-id="a4be8-438">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="a4be8-439">Komut dosyası dağıtım sistemde bir yönetici PowerShell komut isteminden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a4be8-439">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="a4be8-440">NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="a4be8-440">NuGet package</span></span>

<span data-ttu-id="a4be8-441">Bir NuGet paketi barındırma bir başlangıç geliştirmesi sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-441">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="a4be8-442">Pakette bir `HostingStartup` özniteliği vardır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-442">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="a4be8-443">Paket tarafından sağlanan barındırma başlangıç türleri, aşağıdaki yaklaşımlardan birini kullanarak uygulama için kullanıma sunulur:</span><span class="sxs-lookup"><span data-stu-id="a4be8-443">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="a4be8-444">Gelişmiş uygulamanın proje dosyası, barındırma başlangıç için bir paket başvurusu uygulamanın proje dosyası (bir derleme zamanı Başvurusu) sağlar.</span><span class="sxs-lookup"><span data-stu-id="a4be8-444">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="a4be8-445">Derleme zamanı başvurusu yerinde olduğunda, barındırma başlangıç derlemesi ve tüm bağımlılıkları uygulamanın bağımlılık dosyasına ( *. Deps. JSON*) dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-445">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="a4be8-446">Bu yaklaşım, [NuGet.org](https://www.nuget.org/)'e yayınlanan bir barındırma başlangıç derleme paketi için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-446">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="a4be8-447">Barındırma başlatmasının bağımlılıklar dosyası, [çalışma zamanı deposu](#runtime-store) bölümünde açıklandığı gibi gelişmiş uygulama için kullanılabilir hale getirilir (derleme zamanı başvurusu olmadan).</span><span class="sxs-lookup"><span data-stu-id="a4be8-447">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="a4be8-448">NuGet paketlerini ve çalışma zamanı mağazası hakkında daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="a4be8-448">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="a4be8-449">Platformlar arası araçlarla bir NuGet paketi oluşturma</span><span class="sxs-lookup"><span data-stu-id="a4be8-449">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="a4be8-450">Paketler yayımlanıyor</span><span class="sxs-lookup"><span data-stu-id="a4be8-450">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="a4be8-451">Çalışma zamanı paket deposu</span><span class="sxs-lookup"><span data-stu-id="a4be8-451">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="a4be8-452">Proje bin klasörü</span><span class="sxs-lookup"><span data-stu-id="a4be8-452">Project bin folder</span></span>

<span data-ttu-id="a4be8-453">Bir barındırma başlatma geliştirmesi, gelişmiş uygulamada, *bin*ile dağıtılan bir derleme tarafından sağlanıyor.</span><span class="sxs-lookup"><span data-stu-id="a4be8-453">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="a4be8-454">Derleme tarafından sağlanan barındırma başlangıç türleri, aşağıdaki yaklaşımlardan biri kullanılarak uygulama için kullanılabilir hale getirilir:</span><span class="sxs-lookup"><span data-stu-id="a4be8-454">The hosting startup types provided by the assembly are made available to the app using one of the following approaches:</span></span>

* <span data-ttu-id="a4be8-455">Gelişmiş uygulamanın proje dosyası, barındırma başlangıç (bir derleme zamanı başvuru) bir bütünleştirilmiş kod başvurusu yapar.</span><span class="sxs-lookup"><span data-stu-id="a4be8-455">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="a4be8-456">Derleme zamanı başvurusu yerinde olduğunda, barındırma başlangıç derlemesi ve tüm bağımlılıkları uygulamanın bağımlılık dosyasına ( *. Deps. JSON*) dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-456">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="a4be8-457">Bu yaklaşım, dağıtım senaryosu barındırma başlatmasının derlemesine ( *. dll* dosyası) bir derleme zamanı başvurusu yapmak ve derlemeyi şu şekilde taşımak için çağırdığında geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="a4be8-457">This approach applies when the deployment scenario calls for making a compile-time reference to the hosting startup's assembly (*.dll* file) and moving the assembly to either:</span></span>
  * <span data-ttu-id="a4be8-458">Tüketen proje.</span><span class="sxs-lookup"><span data-stu-id="a4be8-458">The consuming project.</span></span>
  * <span data-ttu-id="a4be8-459">Tüketim Projesi tarafından erişilebilen bir konum.</span><span class="sxs-lookup"><span data-stu-id="a4be8-459">A location accessible by the consuming project.</span></span>
* <span data-ttu-id="a4be8-460">Barındırma başlatmasının bağımlılıklar dosyası, [çalışma zamanı deposu](#runtime-store) bölümünde açıklandığı gibi gelişmiş uygulama için kullanılabilir hale getirilir (derleme zamanı başvurusu olmadan).</span><span class="sxs-lookup"><span data-stu-id="a4be8-460">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>
* <span data-ttu-id="a4be8-461">.NET Framework hedeflenirken, derleme varsayılan yükleme bağlamında yüklenebilir olur; bu, .NET Framework, derlemenin aşağıdaki konumlardan birinde bulunduğu anlamına gelir:</span><span class="sxs-lookup"><span data-stu-id="a4be8-461">When targeting the .NET Framework, the assembly is loadable in the default load context, which on .NET Framework means that the assembly is located at either of the following locations:</span></span>
  * <span data-ttu-id="a4be8-462">Uygulama temel yolu, uygulamanın yürütülebilir dosyasının ( *. exe*) bulunduğu *bin* klasörünü &ndash;.</span><span class="sxs-lookup"><span data-stu-id="a4be8-462">Application base path &ndash; The *bin* folder where the app's executable (*.exe*) is located.</span></span>
  * <span data-ttu-id="a4be8-463">Genel bütünleştirilmiş kod önbelleği (GAC) &ndash; GAC, birkaç .NET Framework uygulamanın paylaştığı derlemeleri depolar.</span><span class="sxs-lookup"><span data-stu-id="a4be8-463">Global Assembly Cache (GAC) &ndash; The GAC stores assemblies that several .NET Framework apps share.</span></span> <span data-ttu-id="a4be8-464">Daha fazla bilgi için, bkz. [nasıl yapılır: bir derlemeyi genel derleme önbelleğine yüklemek](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) .NET Framework belgeleri.</span><span class="sxs-lookup"><span data-stu-id="a4be8-464">For more information, see [How to: Install an assembly into the global assembly cache](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) in the .NET Framework documentation.</span></span>

## <a name="sample-code"></a><span data-ttu-id="a4be8-465">Örnek kod</span><span class="sxs-lookup"><span data-stu-id="a4be8-465">Sample code</span></span>

<span data-ttu-id="a4be8-466">[Örnek kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample)), başlangıç uygulama senaryolarını barındırma gösterir:</span><span class="sxs-lookup"><span data-stu-id="a4be8-466">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="a4be8-467">İki barındırma başlangıç derlemeleri (sınıf kitaplıkları), bellek içi yapılandırma anahtar-değer çiftleri her bir çifti ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="a4be8-467">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="a4be8-468">NuGet paketi (*Hostingstartuppackage*)</span><span class="sxs-lookup"><span data-stu-id="a4be8-468">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="a4be8-469">Sınıf kitaplığı (*Hostingstartuplibrary*)</span><span class="sxs-lookup"><span data-stu-id="a4be8-469">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="a4be8-470">Bir barındırma başlatması, çalışma zamanı deposu tarafından dağıtılan bir derlemeden (*Startupdiagnostics*) etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-470">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="a4be8-471">Derleme, uygulamaya tanılama bilgileri sağlayan başlangıçta iki middlewares ekler:</span><span class="sxs-lookup"><span data-stu-id="a4be8-471">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="a4be8-472">Kaydedilen Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="a4be8-472">Registered services</span></span>
  * <span data-ttu-id="a4be8-473">Adres (Düzen, konak, temel yolu, yol, sorgu dizesi)</span><span class="sxs-lookup"><span data-stu-id="a4be8-473">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="a4be8-474">Bağlantı (uzak IP, uzak bağlantı noktasını, yerel IP yerel bağlantı noktası, istemci sertifikası)</span><span class="sxs-lookup"><span data-stu-id="a4be8-474">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="a4be8-475">İstek üst bilgileri</span><span class="sxs-lookup"><span data-stu-id="a4be8-475">Request headers</span></span>
  * <span data-ttu-id="a4be8-476">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="a4be8-476">Environment variables</span></span>

<span data-ttu-id="a4be8-477">Örneği çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="a4be8-477">To run the sample:</span></span>

<span data-ttu-id="a4be8-478">**NuGet paketinden etkinleştirme**</span><span class="sxs-lookup"><span data-stu-id="a4be8-478">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="a4be8-479">,, [DotNet paketi](/dotnet/core/tools/dotnet-pack) komutuyla *hostingstartuppackage* paketini derleyin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-479">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="a4be8-480">*Hostingstartuppackage* 'in derleme adını `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkenine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-480">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="a4be8-481">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a4be8-481">Compile and run the app.</span></span> <span data-ttu-id="a4be8-482">Geliştirilmiş bir uygulamada (bir derleme zamanı Başvurusu) bir paket başvurusu yok.</span><span class="sxs-lookup"><span data-stu-id="a4be8-482">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="a4be8-483">Uygulamanın proje dosyasındaki bir `<PropertyGroup>` paket projesinin çıkışını belirtir ( *.. /HostingStartupPackage/bin/Debug*) bir paket kaynağı olarak.</span><span class="sxs-lookup"><span data-stu-id="a4be8-483">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="a4be8-484">Bu, uygulamanın paketi [NuGet.org](https://www.nuget.org/)'e yüklemeden paketi kullanmasına izin verir. Daha fazla bilgi için HostingStartupApp öğesinin proje dosyasındaki notlara bakın.</span><span class="sxs-lookup"><span data-stu-id="a4be8-484">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```

1. <span data-ttu-id="a4be8-485">Dizin sayfası tarafından oluşturulan hizmet yapılandırma anahtarı değerlerinin, paketin `ServiceKeyInjection.Configure` yöntemi tarafından ayarlanan değerlerle eşleştiğini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-485">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="a4be8-486">*Hostingstartuppackage* projesinde değişiklik yaparsanız ve yeniden derleyseniz, *Hostingstartupapp* ' ın yerel önbellekten eski bir paket değil, güncelleştirilmiş paketi aldığından emin olmak için yerel NuGet paket önbelleklerini temizleyin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-486">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="a4be8-487">Yerel NuGet önbelleklerini temizlemek için aşağıdaki [DotNet NuGet Yereller](/dotnet/core/tools/dotnet-nuget-locals) komutunu yürütün:</span><span class="sxs-lookup"><span data-stu-id="a4be8-487">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```dotnetcli
dotnet nuget locals all --clear
```

<span data-ttu-id="a4be8-488">**Bir sınıf kitaplığından etkinleştirme**</span><span class="sxs-lookup"><span data-stu-id="a4be8-488">**Activation from a class library**</span></span>

1. <span data-ttu-id="a4be8-489">, [DotNet Build](/dotnet/core/tools/dotnet-build) komutuyla *hostingstartuplibrary* sınıf kitaplığını derleyin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-489">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="a4be8-490">*Hostingstartuplibrary* 'in sınıf kitaplığının derleme adını `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkenine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-490">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="a4be8-491">*bin*- *hostingstartuplibrary. dll* dosyasını sınıf kitaplığının derlenmiş çıktısından uygulamanın *bin/Debug* klasörüne kopyalayarak uygulamaya sınıf kitaplığının derlemesini dağıtın.</span><span class="sxs-lookup"><span data-stu-id="a4be8-491">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="a4be8-492">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a4be8-492">Compile and run the app.</span></span> <span data-ttu-id="a4be8-493">Uygulamanın proje dosyasındaki bir `<ItemGroup>`, sınıf kitaplığının derlemesine ( *.\Bin\debug\netcoreapp2,\hostingstartuplibrary.dll*) başvurur (bir derleme zamanı başvurusu).</span><span class="sxs-lookup"><span data-stu-id="a4be8-493">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="a4be8-494">Notları HostingStartupApp'ın proje dosyasında daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="a4be8-494">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\\bin\\Debug\\netcoreapp2.1\\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```

1. <span data-ttu-id="a4be8-495">Dizin sayfası tarafından oluşturulan hizmet yapılandırma anahtarı değerlerinin, sınıf kitaplığının `ServiceKeyInjection.Configure` yöntemi tarafından ayarlanan değerlerle eşleştiğini gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-495">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="a4be8-496">**Çalışma zamanı deposu tarafından dağıtılan bir derlemeden etkinleştirme**</span><span class="sxs-lookup"><span data-stu-id="a4be8-496">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="a4be8-497">*Startupdiagnostics* projesi, *startupdiagnostics. Deps. JSON* dosyasını değiştirmek için [PowerShell](/powershell/scripting/powershell-scripting) kullanır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-497">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="a4be8-498">PowerShell, Windows 7 SP1 ve Windows Server 2008 R2 SP1 ile başlayarak Windows üzerinde varsayılan olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-498">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="a4be8-499">Diğer platformlarda PowerShell 'i almak için bkz. [Windows PowerShell 'ı yükleme](/powershell/scripting/setup/installing-powershell#powershell-core).</span><span class="sxs-lookup"><span data-stu-id="a4be8-499">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span></span>
1. <span data-ttu-id="a4be8-500">*Runtimesyürüme* klasöründe *Build. ps1* betiğini yürütün.</span><span class="sxs-lookup"><span data-stu-id="a4be8-500">Execute the *build.ps1* script in the *RuntimeStore* folder.</span></span> <span data-ttu-id="a4be8-501">Betik:</span><span class="sxs-lookup"><span data-stu-id="a4be8-501">The script:</span></span>
   * <span data-ttu-id="a4be8-502">`StartupDiagnostics` paketini *obj\packages* klasöründe oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a4be8-502">Generates the `StartupDiagnostics` package in the *obj\packages* folder.</span></span>
   * <span data-ttu-id="a4be8-503">*Mağaza* klasöründeki `StartupDiagnostics` çalışma zamanı deposunu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a4be8-503">Generates the runtime store for `StartupDiagnostics` in the *store* folder.</span></span> <span data-ttu-id="a4be8-504">Betikteki `dotnet store` komutu, Windows 'a dağıtılan bir barındırma başlatması için `win7-x64` [çalışma zamanı tanımlayıcısı 'nı (RID)](/dotnet/core/rid-catalog) kullanır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-504">The `dotnet store` command in the script uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog) for a hosting startup deployed to Windows.</span></span> <span data-ttu-id="a4be8-505">Farklı bir çalışma zamanı için barındırma başlangıcını sağlarken, betiğin 37. satırındaki doğru RID 'yi yerine koyun.</span><span class="sxs-lookup"><span data-stu-id="a4be8-505">When providing the hosting startup for a different runtime, substitute the correct RID on line 37 of the script.</span></span> <span data-ttu-id="a4be8-506">`StartupDiagnostics` çalışma zamanı deposu daha sonra derlemenin tüketilebileceği makinede kullanıcının veya sistem çalışma zamanı deposuna taşınır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-506">The runtime store for `StartupDiagnostics` would later be moved to the user's or system's runtime store on the machine where the assembly will be consumed.</span></span> <span data-ttu-id="a4be8-507">`StartupDiagnostics` derlemesinin Kullanıcı çalışma zamanı deposu yüklemesi konumu *. DotNet/Store/x64/netcoreapp 2.2/startupdiagnostics/1.0.0/LIB/netcoreapp 2.2/startupdiagnostics. dll*' dir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-507">The user runtime store install location for the `StartupDiagnostics` assembly is *.dotnet/store/x64/netcoreapp2.2/startupdiagnostics/1.0.0/lib/netcoreapp2.2/StartupDiagnostics.dll*.</span></span>
   * <span data-ttu-id="a4be8-508">*Additionaldeps* klasöründeki `StartupDiagnostics` için `additionalDeps` üretir.</span><span class="sxs-lookup"><span data-stu-id="a4be8-508">Generates the `additionalDeps` for `StartupDiagnostics` in the *additionalDeps* folder.</span></span> <span data-ttu-id="a4be8-509">Ek bağımlılıklar daha sonra kullanıcının veya sistem ek bağımlılıklarına taşınır.</span><span class="sxs-lookup"><span data-stu-id="a4be8-509">The additional dependencies would later be moved to the user's or system's additional dependencies.</span></span> <span data-ttu-id="a4be8-510">Kullanıcı `StartupDiagnostics` ek bağımlılıklar yüklemesi konumu *. DotNet/x64/additionalDeps/startupdiagnostics/Shared/Microsoft. NETCore. App/2.2.0/StartupDiagnostics. Deps. JSON*olur.</span><span class="sxs-lookup"><span data-stu-id="a4be8-510">The user `StartupDiagnostics` additional dependencies install location is *.dotnet/x64/additionalDeps/StartupDiagnostics/shared/Microsoft.NETCore.App/2.2.0/StartupDiagnostics.deps.json*.</span></span>
   * <span data-ttu-id="a4be8-511">*Dağıtım* klasörüne *Deploy. ps1* dosyasını koyar.</span><span class="sxs-lookup"><span data-stu-id="a4be8-511">Places the *deploy.ps1* file in the *deployment* folder.</span></span>
1. <span data-ttu-id="a4be8-512">*Dağıtım* klasöründe *Deploy. ps1* betiğini çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a4be8-512">Run the *deploy.ps1* script in the *deployment* folder.</span></span> <span data-ttu-id="a4be8-513">Betik şunu ekler:</span><span class="sxs-lookup"><span data-stu-id="a4be8-513">The script appends:</span></span>
   * <span data-ttu-id="a4be8-514">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkenine `StartupDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="a4be8-514">`StartupDiagnostics` to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="a4be8-515">Barındırma başlangıç bağımlılıkları yolu (Runtimessımında projenin *dağıtım* klasöründe) `DOTNET_ADDITIONAL_DEPS` ortam değişkenine.</span><span class="sxs-lookup"><span data-stu-id="a4be8-515">The hosting startup dependencies path (in the RuntimeStore project's *deployment* folder) to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
   * <span data-ttu-id="a4be8-516">Çalışma zamanı depolama yolu (Runtimes, projenin *dağıtım* klasöründe) `DOTNET_SHARED_STORE` ortam değişkenine.</span><span class="sxs-lookup"><span data-stu-id="a4be8-516">The runtime store path (in the RuntimeStore project's *deployment* folder) to the `DOTNET_SHARED_STORE` environment variable.</span></span>
1. <span data-ttu-id="a4be8-517">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a4be8-517">Run the sample app.</span></span>
1. <span data-ttu-id="a4be8-518">Uygulamanın kayıtlı hizmetlerini görmek için `/services` uç noktası isteyin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-518">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="a4be8-519">Tanılama bilgilerini görmek için `/diag` uç noktası isteyin.</span><span class="sxs-lookup"><span data-stu-id="a4be8-519">Request the `/diag` endpoint to see the diagnostic information.</span></span>

::: moniker-end
