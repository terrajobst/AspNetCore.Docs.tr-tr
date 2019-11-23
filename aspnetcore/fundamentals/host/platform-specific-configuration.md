---
title: ASP.NET Core barındırma başlangıç derlemeleri kullanma
author: guardrex
description: Uygulama Ihostingstartup kullanarak dış bütünleştirilmiş koddan bir ASP.NET Core uygulaması geliştirmek nasıl keşfedin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 09/26/2019
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: c1ba742dda64296348898ec6a15ba725501dcb4f
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391011"
---
# <a name="use-hosting-startup-assemblies-in-aspnet-core"></a><span data-ttu-id="093d6-103">ASP.NET Core barındırma başlangıç derlemeleri kullanma</span><span class="sxs-lookup"><span data-stu-id="093d6-103">Use hosting startup assemblies in ASP.NET Core</span></span>

<span data-ttu-id="093d6-104">Tarafından [Luke Latham](https://github.com/guardrex) ve [Pavel Krymets](https://github.com/pakrym)</span><span class="sxs-lookup"><span data-stu-id="093d6-104">By [Luke Latham](https://github.com/guardrex) and [Pavel Krymets](https://github.com/pakrym)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="093d6-105"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> (barındırma başlatma) uygulaması, bir dış derlemeden başlatma sırasında bir uygulamaya iyileştirmeler ekler.</span><span class="sxs-lookup"><span data-stu-id="093d6-105">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="093d6-106">Örneğin, bir harici kitaplık ek yapılandırma sağlayıcıları ya da bir uygulama hizmetlerini barındıran bir başlangıç uygulaması kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="093d6-106">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span>

<span data-ttu-id="093d6-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="093d6-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="093d6-108">HostingStartup özniteliği</span><span class="sxs-lookup"><span data-stu-id="093d6-108">HostingStartup attribute</span></span>

<span data-ttu-id="093d6-109">A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) öznitelik, çalışma zamanında etkinleştirmek için bir barındırma başlangıç derleme varlığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="093d6-109">A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="093d6-110">Giriş derleme veya içeren derlemenin `Startup` sınıfı için taranan otomatik olarak `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="093d6-110">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="093d6-111">Aranacak derlemelerin listesini `HostingStartup` öznitelikleri içinde yapılandırmasından çalışma zamanında yüklendiği [WebHostDefaults.HostingStartupAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupAssembliesKey).</span><span class="sxs-lookup"><span data-stu-id="093d6-111">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupAssembliesKey).</span></span> <span data-ttu-id="093d6-112">Keşiften çıkarmak için derleme listesini alanından yüklenen [WebHostDefaults.HostingStartupExcludeAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupExcludeAssembliesKey).</span><span class="sxs-lookup"><span data-stu-id="093d6-112">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupExcludeAssembliesKey).</span></span>

<span data-ttu-id="093d6-113">Aşağıdaki örnekte, barındırma başlangıç derlemenin ad alanıdır `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="093d6-113">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="093d6-114">Barındırma başlatma kodunu içeren sınıf `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="093d6-114">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="093d6-115">`HostingStartup` Öznitelik genellikle barındırma başlangıç derleme içinde bulunan `IHostingStartup` uygulama sınıf dosyası.</span><span class="sxs-lookup"><span data-stu-id="093d6-115">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="093d6-116">Yüklenen barındırma başlangıç derlemeler keşfedin</span><span class="sxs-lookup"><span data-stu-id="093d6-116">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="093d6-117">Yüklenen barındırma başlangıç derlemeleri bulmak için günlük kaydını etkinleştirmek ve uygulama günlüklerini kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="093d6-117">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="093d6-118">Derlemeler yüklenirken oluşan hataları günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="093d6-118">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="093d6-119">Yüklenen barındırma başlangıç derlemeler hata ayıklama düzeyinde kaydedilir ve tüm hatalar kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="093d6-119">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="093d6-120">Başlangıç derlemeleri barındırma otomatik yüklemeyi devre dışı</span><span class="sxs-lookup"><span data-stu-id="093d6-120">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="093d6-121">Başlangıç derlemeleri barındırma otomatik yüklemeyi devre dışı bırakmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="093d6-121">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="093d6-122">Tüm barındırma başlangıç derlemeleri yüklenmesini önlemek için aşağıdakilerden birini ayarlayın `true` veya `1`:</span><span class="sxs-lookup"><span data-stu-id="093d6-122">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>

  * <span data-ttu-id="093d6-123">Barındırma başlangıç ana bilgisayar yapılandırma ayarını önle:</span><span class="sxs-lookup"><span data-stu-id="093d6-123">Prevent Hosting Startup host configuration setting:</span></span>

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

  * <span data-ttu-id="093d6-124">`ASPNETCORE_PREVENTHOSTINGSTARTUP` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="093d6-124">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

* <span data-ttu-id="093d6-125">Belirli barındırma başlangıç derlemeleri yüklenmesini önlemek için aşağıdakilerden birini başlangıçta hariç tutmak için başlangıç derlemeleri barındırma noktalı virgülle ayrılmış bir dizeye ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="093d6-125">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>

  * <span data-ttu-id="093d6-126">Barındırma başlatma derlemeleri dışlama ana bilgisayar yapılandırma ayarı:</span><span class="sxs-lookup"><span data-stu-id="093d6-126">Hosting Startup Exclude Assemblies host configuration setting:</span></span>

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

  * <span data-ttu-id="093d6-127">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="093d6-127">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

<span data-ttu-id="093d6-128">Hem ana bilgisayar yapılandırma ayarı ve ortam değişkenini ayarlarsanız, ana bilgisayar ayarını davranışını denetler.</span><span class="sxs-lookup"><span data-stu-id="093d6-128">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="093d6-129">Ana bilgisayar ayarı veya ortam değişkenini kullanarak barındırma başlangıç derlemeleri devre dışı bırakma, derlemenin genel olarak devre dışı bırakır ve uygulama çeşitli özelliklerini devre dışı bırakabilir.</span><span class="sxs-lookup"><span data-stu-id="093d6-129">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="093d6-130">Project</span><span class="sxs-lookup"><span data-stu-id="093d6-130">Project</span></span>

<span data-ttu-id="093d6-131">Bir barındırma başlatma aşağıdaki proje türlerini birini oluşturun:</span><span class="sxs-lookup"><span data-stu-id="093d6-131">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="093d6-132">Sınıf kitaplığı</span><span class="sxs-lookup"><span data-stu-id="093d6-132">Class library</span></span>](#class-library)
* [<span data-ttu-id="093d6-133">Konsol uygulaması giriş noktası olmayan</span><span class="sxs-lookup"><span data-stu-id="093d6-133">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="093d6-134">Sınıf kitaplığı</span><span class="sxs-lookup"><span data-stu-id="093d6-134">Class library</span></span>

<span data-ttu-id="093d6-135">Bir barındırma başlangıç geliştirme'de sınıf kitaplığının sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="093d6-135">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="093d6-136">Kitaplığı içeren bir `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="093d6-136">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="093d6-137">[Örnek kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) bir Razor sayfaları uygulamasını içeren *HostingStartupApp*ve bir sınıf kitaplığı *HostingStartupLibrary*.</span><span class="sxs-lookup"><span data-stu-id="093d6-137">The [sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="093d6-138">Sınıf kitaplığı:</span><span class="sxs-lookup"><span data-stu-id="093d6-138">The class library:</span></span>

* <span data-ttu-id="093d6-139">Bir barındırma başlangıç sınıfı içeren `ServiceKeyInjection`, uygulayan `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="093d6-139">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="093d6-140">`ServiceKeyInjection` bellek içi yapılandırma Sağlayıcısı'nı kullanarak uygulamanın yapılandırmasına hizmet dizeleri çifti ekler ([AddInMemoryCollection](xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*)).</span><span class="sxs-lookup"><span data-stu-id="093d6-140">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*)).</span></span>
* <span data-ttu-id="093d6-141">İçeren bir `HostingStartup` barındırma startup şirketinizin ad alanını ve sınıf tanımlayan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="093d6-141">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="093d6-142">`ServiceKeyInjection` sınıfının <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> yöntemi bir uygulamaya geliştirmeler eklemek için bir <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> kullanır.</span><span class="sxs-lookup"><span data-stu-id="093d6-142">The `ServiceKeyInjection` class's <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> method uses an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> to add enhancements to an app.</span></span>

<span data-ttu-id="093d6-143">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="093d6-143">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="093d6-144">Uygulama dizin sayfasına okur ve Sınıf Kitaplığı'nızın barındırma başlangıç derlemesi tarafından ayarlanmış iki anahtarı için yapılandırma değerlerini işler:</span><span class="sxs-lookup"><span data-stu-id="093d6-144">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="093d6-145">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="093d6-145">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="093d6-146">[Örnek kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) de ayrı bir barındırma başlatma sağlayan bir NuGet paketi proje içerir *HostingStartupPackage*.</span><span class="sxs-lookup"><span data-stu-id="093d6-146">The [sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="093d6-147">Paket, daha önce açıklanan Sınıf Kitaplığı'nın aynı özelliklere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="093d6-147">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="093d6-148">Paketi:</span><span class="sxs-lookup"><span data-stu-id="093d6-148">The package:</span></span>

* <span data-ttu-id="093d6-149">Bir barındırma başlangıç sınıfı içeren `ServiceKeyInjection`, uygulayan `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="093d6-149">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="093d6-150">`ServiceKeyInjection` Hizmet dizeleri çifti uygulamanın yapılandırmasına ekler.</span><span class="sxs-lookup"><span data-stu-id="093d6-150">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="093d6-151">İçeren bir `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="093d6-151">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="093d6-152">*HostingStartupPackage/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="093d6-152">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="093d6-153">Uygulama dizin sayfasına okur ve paketin barındırma başlangıç derlemesi tarafından ayarlanmış iki anahtarı için yapılandırma değerlerini işler:</span><span class="sxs-lookup"><span data-stu-id="093d6-153">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="093d6-154">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="093d6-154">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="093d6-155">Konsol uygulaması giriş noktası olmayan</span><span class="sxs-lookup"><span data-stu-id="093d6-155">Console app without an entry point</span></span>

<span data-ttu-id="093d6-156">*Bu yaklaşım, yalnızca .NET Framework .NET Core uygulamaları için kullanılabilir.*</span><span class="sxs-lookup"><span data-stu-id="093d6-156">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="093d6-157">Bir derleme zamanı başvurusu için etkinleştirme gerektirmeyen bir dinamik barındırma başlangıç geliştirmesi de içeren giriş noktası olmayan bir konsol uygulaması sağlanan bir `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="093d6-157">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point that contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="093d6-158">Konsol uygulaması yayımlama, çalışma zamanı Mağazası'ndan kullanılabilen bir barındırma başlangıç bütünleştirilmiş kod üretir.</span><span class="sxs-lookup"><span data-stu-id="093d6-158">Publishing the console app produces a hosting startup assembly that can be consumed from the runtime store.</span></span>

<span data-ttu-id="093d6-159">Bir konsol uygulaması giriş noktası olmayan, çünkü bu işlemde kullanılır:</span><span class="sxs-lookup"><span data-stu-id="093d6-159">A console app without an entry point is used in this process because:</span></span>

* <span data-ttu-id="093d6-160">Bağımlılıkları dosya barındırma başlangıç derlemedeki barındırma için başlangıç kullanmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="093d6-160">A dependencies file is required to consume the hosting startup in the hosting startup assembly.</span></span> <span data-ttu-id="093d6-161">Kitaplık değil bir uygulama yayımlama tarafından üretilen bir çalıştırılabilir uygulama varlık bağımlılıkları dosyasıdır.</span><span class="sxs-lookup"><span data-stu-id="093d6-161">A dependencies file is a runnable app asset that's produced by publishing an app, not a library.</span></span>
* <span data-ttu-id="093d6-162">Bir kitaplık doğrudan eklenemez [çalışma zamanı Paket Deposu](/dotnet/core/deploying/runtime-store), paylaşılan çalışma zamanını hedefleyen çalıştırılabilir bir proje gerektirir.</span><span class="sxs-lookup"><span data-stu-id="093d6-162">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>

<span data-ttu-id="093d6-163">Dinamik barındırma başlangıç oluşturulmasını içinde:</span><span class="sxs-lookup"><span data-stu-id="093d6-163">In the creation of a dynamic hosting startup:</span></span>

* <span data-ttu-id="093d6-164">Bir giriş noktası olmadan bir barındırma başlangıç derleme konsol uygulamasından oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="093d6-164">A hosting startup assembly is created from the console app without an entry point that:</span></span>
  * <span data-ttu-id="093d6-165">İçeren bir sınıfı içeren `IHostingStartup` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="093d6-165">Includes a class that contains the `IHostingStartup` implementation.</span></span>
  * <span data-ttu-id="093d6-166">İçeren bir [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) tanımlamak için öznitelik `IHostingStartup` uygulama sınıfı.</span><span class="sxs-lookup"><span data-stu-id="093d6-166">Includes a [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute to identify the `IHostingStartup` implementation class.</span></span>
* <span data-ttu-id="093d6-167">Konsol uygulaması barındırma startup şirketinizin bağımlılıkları almak için yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="093d6-167">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="093d6-168">Kullanılmayan bağımlılıkları bağımlılıkları dosyasından atılır konsol uygulaması yayımlama bir sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="093d6-168">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
* <span data-ttu-id="093d6-169">Bağımlılıkları dosyası, çalışma zamanı barındırma başlangıç derleme konumunu ayarlamak için değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="093d6-169">The dependencies file is modified to set the runtime location of the hosting startup assembly.</span></span>
* <span data-ttu-id="093d6-170">Barındırma başlangıç derleme ve bağımlılıkları dosyası çalışma zamanı paketi deposuna yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="093d6-170">The hosting startup assembly and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="093d6-171">Barındırma başlangıç derleme ve bağımlılıkları dosyasını bulmak için bir ortam değişkenlerini çift içinde listelendikleri.</span><span class="sxs-lookup"><span data-stu-id="093d6-171">To discover the hosting startup assembly and its dependencies file, they're listed in a pair of environment variables.</span></span>

<span data-ttu-id="093d6-172">Konsol uygulama başvuruları [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) paket:</span><span class="sxs-lookup"><span data-stu-id="093d6-172">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.csproj)]

<span data-ttu-id="093d6-173">[Hostingstartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) özniteliği, bir sınıfı <xref:Microsoft.AspNetCore.Hosting.IWebHost>oluştururken yükleme ve yürütme için `IHostingStartup` bir uygulama olarak tanımlar.</span><span class="sxs-lookup"><span data-stu-id="093d6-173">A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the <xref:Microsoft.AspNetCore.Hosting.IWebHost>.</span></span> <span data-ttu-id="093d6-174">Aşağıdaki örnekte, ad alanı `StartupEnhancement`, ve sınıf `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="093d6-174">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="093d6-175">Arabirimini uygulayan bir sınıf `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="093d6-175">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="093d6-176">Sınıfın <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> yöntemi bir uygulamaya iyileştirmeler eklemek için bir <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> kullanır.</span><span class="sxs-lookup"><span data-stu-id="093d6-176">The class's <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> method uses an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> to add enhancements to an app.</span></span> <span data-ttu-id="093d6-177">`IHostingStartup.Configure` barındırma başlangıç derleme önce çalışma zamanı tarafından çağrılır `Startup.Configure` kullanıcı kodunda barındırma başlangıç derlemesi tarafından sağlanan herhangi bir yapılandırma üzerine yazmak kullanıcı kodu sağlar.</span><span class="sxs-lookup"><span data-stu-id="093d6-177">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="093d6-178">Bir `IHostingStartup` projesi oluştururken, bağımlılıklar dosyası ( *. Deps. JSON*) derlemenin `runtime` konumunu *bin* klasörüne ayarlar:</span><span class="sxs-lookup"><span data-stu-id="093d6-178">When building an `IHostingStartup` project, the dependencies file (*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="093d6-179">Dosyanın yalnızca bir parçası olarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="093d6-179">Only part of the file is shown.</span></span> <span data-ttu-id="093d6-180">Derleme adı örnekte `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="093d6-180">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="configuration-provided-by-the-hosting-startup"></a><span data-ttu-id="093d6-181">Barındırma başlatma tarafından belirtilen yapılandırma</span><span class="sxs-lookup"><span data-stu-id="093d6-181">Configuration provided by the hosting startup</span></span>

<span data-ttu-id="093d6-182">Yapılandırma işlemi, barındırma başlatmasının yapılandırmasının öncelikli olmasını mı yoksa uygulamanın yapılandırmasının öncelikli olmasını mı istediğinize bağlı olarak iki yaklaşımdan yararlanabilir:</span><span class="sxs-lookup"><span data-stu-id="093d6-182">There are two approaches to handling configuration depending on whether you want the hosting startup's configuration to take precedence or the app's configuration to take precedence:</span></span>

1. <span data-ttu-id="093d6-183">Uygulamanın <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> temsilcileri çalıştırıldıktan sonra yapılandırmayı yüklemek için <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> kullanarak uygulamaya yapılandırma sağlayın.</span><span class="sxs-lookup"><span data-stu-id="093d6-183">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> to load the configuration after the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="093d6-184">Barındırma başlangıç yapılandırması, uygulamanın yapılandırmasına bu yaklaşımı kullanarak öncelik kazanır.</span><span class="sxs-lookup"><span data-stu-id="093d6-184">Hosting startup configuration takes priority over the app's configuration using this approach.</span></span>
1. <span data-ttu-id="093d6-185">Uygulamanın <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> temsilcileri yürütmeden önce yapılandırmayı yüklemek için <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> kullanarak uygulamaya yapılandırma sağlayın.</span><span class="sxs-lookup"><span data-stu-id="093d6-185">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> to load the configuration before the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="093d6-186">Uygulamanın yapılandırma değerleri, bu yaklaşımı kullanarak barındırma başlatma tarafından sağlananlara göre önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="093d6-186">The app's configuration values take priority over those provided by the hosting startup using this approach.</span></span>

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

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="093d6-187">Barındırma başlangıç derlemeyi belirtin</span><span class="sxs-lookup"><span data-stu-id="093d6-187">Specify the hosting startup assembly</span></span>

<span data-ttu-id="093d6-188">Bir sınıf kitaplığı - veya konsol uygulaması sağlanan-başlangıç barındırma için barındırma başlangıç derlemenin adını belirtin. `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="093d6-188">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="093d6-189">Ortam değişkenini derlemelerin noktalı virgülle ayrılmış bir listedir.</span><span class="sxs-lookup"><span data-stu-id="093d6-189">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="093d6-190">Yalnızca barındırma başlangıç derlemeler için taranan `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="093d6-190">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="093d6-191">Örnek uygulama için *HostingStartupApp*, daha önce açıklanan barındırma startup'lar bulmak için ortam değişkeni şu değere ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="093d6-191">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="093d6-192">Barındırma başlangıç bütünleştirilmiş kodları, barındırma başlangıç derlemeleri ana bilgisayar yapılandırma ayarı kullanılarak da ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="093d6-192">A hosting startup assembly can also be set using the Hosting Startup Assemblies host configuration setting:</span></span>

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

<span data-ttu-id="093d6-193">Birden çok barındırma başlatması varsa, <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> yöntemleri derlemelerin listelendiği sırada yürütülür.</span><span class="sxs-lookup"><span data-stu-id="093d6-193">When multiple hosting startup assembles are present, their <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="093d6-194">Etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="093d6-194">Activation</span></span>

<span data-ttu-id="093d6-195">Başlangıç etkinleştirme barındırmak için Seçenekler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="093d6-195">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="093d6-196">[Çalışma zamanı deposu](#runtime-store) &ndash; etkinleştirme, etkinleştirme için bir derleme zamanı başvurusu gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="093d6-196">[Runtime store](#runtime-store) &ndash; Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="093d6-197">Örnek uygulama barındırma başlangıç derleme ve bağımlılıkları dosyalar bir klasöre yerleştirir *dağıtım*multimachine ortamında barındırma başlangıç dağıtımını kolaylaştırmak için.</span><span class="sxs-lookup"><span data-stu-id="093d6-197">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="093d6-198">*Dağıtım* klasörü oluşturur veya değiştirir barındırma için başlangıç etkinleştirmek için dağıtım sistemi ortam değişkenlerini bir PowerShell betiğini de içerir.</span><span class="sxs-lookup"><span data-stu-id="093d6-198">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="093d6-199">Etkinleştirme için gerekli derleme zamanı başvurusu</span><span class="sxs-lookup"><span data-stu-id="093d6-199">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="093d6-200">NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="093d6-200">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="093d6-201">Proje bin klasörü</span><span class="sxs-lookup"><span data-stu-id="093d6-201">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="093d6-202">Çalışma zamanı deposu</span><span class="sxs-lookup"><span data-stu-id="093d6-202">Runtime store</span></span>

<span data-ttu-id="093d6-203">Barındırma başlangıç uygulaması yerleştirilir [çalışma zamanı deposu](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="093d6-203">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="093d6-204">Gelişmiş uygulama tarafından bir derleme zamanı başvurusu derleme için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="093d6-204">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="093d6-205">Barındırma başlangıç oluşturulduktan sonra bir çalışma zamanı deposu bildirim proje dosyası kullanılarak oluşturulur ve [dotnet deposu](/dotnet/core/tools/dotnet-store) komutu.</span><span class="sxs-lookup"><span data-stu-id="093d6-205">After the hosting startup is built, a runtime store is generated using the manifest project file and the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```dotnetcli
dotnet store --manifest {MANIFEST FILE} --runtime {RUNTIME IDENTIFIER} --output {OUTPUT LOCATION} --skip-optimization
```

<span data-ttu-id="093d6-206">Örnek uygulamada (*RuntimeStore* Proje) şu komut kullanılır:</span><span class="sxs-lookup"><span data-stu-id="093d6-206">In the sample app (*RuntimeStore* project) the following command is used:</span></span>

```dotnetcli
dotnet store --manifest store.manifest.csproj --runtime win7-x64 --output ./deployment/store --skip-optimization
```

<span data-ttu-id="093d6-207">Çalışma zamanı deposu bulmak çalışma zamanı için çalışma zamanı deponun konumunu eklenen `DOTNET_SHARED_STORE` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="093d6-207">For the runtime to discover the runtime store, the runtime store's location is added to the `DOTNET_SHARED_STORE` environment variable.</span></span>

<span data-ttu-id="093d6-208">**Değiştirme ve barındırma startup şirketinizin bağımlılıkları dosyasının yerleştirin**</span><span class="sxs-lookup"><span data-stu-id="093d6-208">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="093d6-209">Geliştirme için bir paket başvurusu olmadan geliştirme etkinleştirmek için çalışma zamanı ile ek bağımlılıklar belirtin `additionalDeps`.</span><span class="sxs-lookup"><span data-stu-id="093d6-209">To activate the enhancement without a package reference to the enhancement, specify additional dependencies to the runtime with `additionalDeps`.</span></span> <span data-ttu-id="093d6-210">`additionalDeps` sağlar:</span><span class="sxs-lookup"><span data-stu-id="093d6-210">`additionalDeps` allows you to:</span></span>

* <span data-ttu-id="093d6-211">Başlangıçta uygulamanın kendi *. Deps* . JSON dosyasıyla birleştirilecek bir dizi ek *. Deps. JSON* dosyası sağlayarak uygulamanın kitaplık grafiğini genişletin.</span><span class="sxs-lookup"><span data-stu-id="093d6-211">Extend the app's library graph by providing a set of additional *.deps.json* files to merge with the app's own *.deps.json* file on startup.</span></span>
* <span data-ttu-id="093d6-212">Barındırma başlangıç derlemesine bulunabilir ve yüklenebilir olun.</span><span class="sxs-lookup"><span data-stu-id="093d6-212">Make the hosting startup assembly discoverable and loadable.</span></span>

<span data-ttu-id="093d6-213">Ek Bağımlılıklar dosyası oluşturmak için önerilen yaklaşımdır:</span><span class="sxs-lookup"><span data-stu-id="093d6-213">The recommended approach for generating the additional dependencies file is to:</span></span>

 1. <span data-ttu-id="093d6-214">Yürütme `dotnet publish` önceki bölümde başvurulan çalışma zamanı deposu bildirim dosyası üzerinde.</span><span class="sxs-lookup"><span data-stu-id="093d6-214">Execute `dotnet publish` on the runtime store manifest file referenced in the previous section.</span></span>
 1. <span data-ttu-id="093d6-215">Bildirimlerin bildirim başvurusunu ve sonuçta elde edilen *. Deps. JSON* dosyasının `runtime` bölümünü kaldırın.</span><span class="sxs-lookup"><span data-stu-id="093d6-215">Remove the manifest reference from libraries and the `runtime` section of the resulting *.deps.json* file.</span></span>

<span data-ttu-id="093d6-216">Örnek projesinde `store.manifest/1.0.0` özelliği kaldırılır `targets` ve `libraries` bölümü:</span><span class="sxs-lookup"><span data-stu-id="093d6-216">In the example project, the `store.manifest/1.0.0` property is removed from the `targets` and `libraries` section:</span></span>

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

<span data-ttu-id="093d6-217">*. Deps. JSON* dosyasını şu konuma yerleştirin:</span><span class="sxs-lookup"><span data-stu-id="093d6-217">Place the *.deps.json* file into the following location:</span></span>

```
{ADDITIONAL DEPENDENCIES PATH}/shared/{SHARED FRAMEWORK NAME}/{SHARED FRAMEWORK VERSION}/{ENHANCEMENT ASSEMBLY NAME}.deps.json
```

* <span data-ttu-id="093d6-218">`{ADDITIONAL DEPENDENCIES PATH}` &ndash; Eklenen konumu `DOTNET_ADDITIONAL_DEPS` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="093d6-218">`{ADDITIONAL DEPENDENCIES PATH}` &ndash; Location added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
* <span data-ttu-id="093d6-219">`{SHARED FRAMEWORK NAME}` &ndash; Bu ek bağımlılıklar dosyası için gerekli framework paylaşılan.</span><span class="sxs-lookup"><span data-stu-id="093d6-219">`{SHARED FRAMEWORK NAME}` &ndash; Shared framework required for this additional dependencies file.</span></span>
* <span data-ttu-id="093d6-220">`{SHARED FRAMEWORK VERSION}` &ndash; Minimum paylaşılan framework sürümü.</span><span class="sxs-lookup"><span data-stu-id="093d6-220">`{SHARED FRAMEWORK VERSION}` &ndash; Minimum shared framework version.</span></span>
* <span data-ttu-id="093d6-221">`{ENHANCEMENT ASSEMBLY NAME}` &ndash; Geliştirme'nın bütünleştirilmiş kod adı.</span><span class="sxs-lookup"><span data-stu-id="093d6-221">`{ENHANCEMENT ASSEMBLY NAME}` &ndash; The enhancement's assembly name.</span></span>

<span data-ttu-id="093d6-222">Örnek uygulamada (*RuntimeStore* Proje), ek bağımlılıklar dosyası şu konuma yerleştirilir:</span><span class="sxs-lookup"><span data-stu-id="093d6-222">In the sample app (*RuntimeStore* project), the additional dependencies file is placed into the following location:</span></span>

```
deployment/additionalDeps/shared/Microsoft.AspNetCore.App/3.0.0/StartupDiagnostics.deps.json
```

<span data-ttu-id="093d6-223">Ek Bağımlılıklar dosya konumunu eklenir çalışma zamanı depolama konumu bulmak çalışma zamanı için `DOTNET_ADDITIONAL_DEPS` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="093d6-223">For runtime to discover the runtime store location, the additional dependencies file location is added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="093d6-224">Örnek uygulamada (*RuntimeStore* Proje), çalışma zamanı deposu oluşturma ve dosya kullanarak gerçekleştirilir ek bağımlılıklar oluşturma bir [PowerShell](/powershell/scripting/powershell-scripting) betiği.</span><span class="sxs-lookup"><span data-stu-id="093d6-224">In the sample app (*RuntimeStore* project), building the runtime store and generating the additional dependencies file is accomplished using a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span>

<span data-ttu-id="093d6-225">Çeşitli işletim sistemleri için ortam değişkenlerini ayarlama örnekleri için bkz: [birden fazla ortam kullanayım](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="093d6-225">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="093d6-226">**Dağıtım**</span><span class="sxs-lookup"><span data-stu-id="093d6-226">**Deployment**</span></span>

<span data-ttu-id="093d6-227">Bir barındırma başlatma multimachine ortamında dağıtımını kolaylaştırmak için örnek uygulamayı oluşturur. bir *dağıtım* içeren yayımlanan çıkış klasöründe:</span><span class="sxs-lookup"><span data-stu-id="093d6-227">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="093d6-228">Barındırma başlangıç çalışma zamanı deposu.</span><span class="sxs-lookup"><span data-stu-id="093d6-228">The hosting startup runtime store.</span></span>
* <span data-ttu-id="093d6-229">Barındırma başlangıç bağımlılıkları dosyası.</span><span class="sxs-lookup"><span data-stu-id="093d6-229">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="093d6-230">Bir PowerShell Betiği oluşturur veya değiştirir `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, ve `DOTNET_ADDITIONAL_DEPS` barındırma başlangıç etkinleştirmeyi desteklemek için.</span><span class="sxs-lookup"><span data-stu-id="093d6-230">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="093d6-231">Komut dosyası dağıtım sistemde bir yönetici PowerShell komut isteminden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="093d6-231">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="093d6-232">NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="093d6-232">NuGet package</span></span>

<span data-ttu-id="093d6-233">Bir NuGet paketi barındırma bir başlangıç geliştirmesi sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="093d6-233">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="093d6-234">Paketin bir `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="093d6-234">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="093d6-235">Paket tarafından sağlanan barındırma başlangıç türleri, aşağıdaki yaklaşımlardan birini kullanarak uygulama için kullanıma sunulur:</span><span class="sxs-lookup"><span data-stu-id="093d6-235">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="093d6-236">Gelişmiş uygulamanın proje dosyası, barındırma başlangıç için bir paket başvurusu uygulamanın proje dosyası (bir derleme zamanı Başvurusu) sağlar.</span><span class="sxs-lookup"><span data-stu-id="093d6-236">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="093d6-237">Derleme zamanı başvurusu yerinde olduğunda, barındırma başlangıç derlemesi ve tüm bağımlılıkları uygulamanın bağımlılık dosyasına ( *. Deps. JSON*) dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="093d6-237">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="093d6-238">Bu yaklaşım, yayımlanan bir barındırma başlangıç derleme paketi uygulandığı [nuget.org](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="093d6-238">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="093d6-239">Barındırma startup şirketinizin bağımlılıkları dosyasının açıklandığı gibi gelişmiş uygulama için kullanılabilir hale getirileceğini [çalışma zamanı deposu](#runtime-store) bölümü (olmadan, bir derleme zamanı Başvurusu).</span><span class="sxs-lookup"><span data-stu-id="093d6-239">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="093d6-240">NuGet paketlerini ve çalışma zamanı mağazası hakkında daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="093d6-240">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="093d6-241">Platformlar arası araçlarla NuGet paketi oluşturma</span><span class="sxs-lookup"><span data-stu-id="093d6-241">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="093d6-242">Paket yayımlama</span><span class="sxs-lookup"><span data-stu-id="093d6-242">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="093d6-243">Çalışma zamanı paket deposu</span><span class="sxs-lookup"><span data-stu-id="093d6-243">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="093d6-244">Proje bin klasörü</span><span class="sxs-lookup"><span data-stu-id="093d6-244">Project bin folder</span></span>

<span data-ttu-id="093d6-245">Bir barındırma başlangıç geliştirmesi tarafından sağlanan bir *bin*-Gelişmiş Uygulama derlemesinde dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="093d6-245">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="093d6-246">Derleme tarafından sağlanan barındırma başlangıç türleri, aşağıdaki yaklaşımlardan biri kullanılarak uygulama için kullanılabilir hale getirilir:</span><span class="sxs-lookup"><span data-stu-id="093d6-246">The hosting startup types provided by the assembly are made available to the app using one of the following approaches:</span></span>

* <span data-ttu-id="093d6-247">Gelişmiş uygulamanın proje dosyası, barındırma başlangıç (bir derleme zamanı başvuru) bir bütünleştirilmiş kod başvurusu yapar.</span><span class="sxs-lookup"><span data-stu-id="093d6-247">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="093d6-248">Derleme zamanı başvurusu yerinde olduğunda, barındırma başlangıç derlemesi ve tüm bağımlılıkları uygulamanın bağımlılık dosyasına ( *. Deps. JSON*) dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="093d6-248">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="093d6-249">Bu yaklaşım, dağıtım senaryosu barındırma başlatmasının derlemesine ( *. dll* dosyası) bir derleme zamanı başvurusu yapmak ve derlemeyi şu şekilde taşımak için çağırdığında geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="093d6-249">This approach applies when the deployment scenario calls for making a compile-time reference to the hosting startup's assembly (*.dll* file) and moving the assembly to either:</span></span>
  * <span data-ttu-id="093d6-250">Tüketen proje.</span><span class="sxs-lookup"><span data-stu-id="093d6-250">The consuming project.</span></span>
  * <span data-ttu-id="093d6-251">Tüketim Projesi tarafından erişilebilen bir konum.</span><span class="sxs-lookup"><span data-stu-id="093d6-251">A location accessible by the consuming project.</span></span>
* <span data-ttu-id="093d6-252">Barındırma startup şirketinizin bağımlılıkları dosyasının açıklandığı gibi gelişmiş uygulama için kullanılabilir hale getirileceğini [çalışma zamanı deposu](#runtime-store) bölümü (olmadan, bir derleme zamanı Başvurusu).</span><span class="sxs-lookup"><span data-stu-id="093d6-252">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>
* <span data-ttu-id="093d6-253">.NET Framework hedeflenirken, derleme varsayılan yükleme bağlamında yüklenebilir olur; bu, .NET Framework, derlemenin aşağıdaki konumlardan birinde bulunduğu anlamına gelir:</span><span class="sxs-lookup"><span data-stu-id="093d6-253">When targeting the .NET Framework, the assembly is loadable in the default load context, which on .NET Framework means that the assembly is located at either of the following locations:</span></span>
  * <span data-ttu-id="093d6-254">Uygulama temel yolu, uygulamanın yürütülebilir dosyasının ( *. exe*) bulunduğu *bin* klasörünü &ndash;.</span><span class="sxs-lookup"><span data-stu-id="093d6-254">Application base path &ndash; The *bin* folder where the app's executable (*.exe*) is located.</span></span>
  * <span data-ttu-id="093d6-255">Genel bütünleştirilmiş kod önbelleği (GAC) &ndash; GAC, birkaç .NET Framework uygulamanın paylaştığı derlemeleri depolar.</span><span class="sxs-lookup"><span data-stu-id="093d6-255">Global Assembly Cache (GAC) &ndash; The GAC stores assemblies that several .NET Framework apps share.</span></span> <span data-ttu-id="093d6-256">Daha fazla bilgi için, bkz. [nasıl yapılır: bir derlemeyi genel derleme önbelleğine yüklemek](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) .NET Framework belgeleri.</span><span class="sxs-lookup"><span data-stu-id="093d6-256">For more information, see [How to: Install an assembly into the global assembly cache](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) in the .NET Framework documentation.</span></span>

## <a name="sample-code"></a><span data-ttu-id="093d6-257">Örnek kod</span><span class="sxs-lookup"><span data-stu-id="093d6-257">Sample code</span></span>

<span data-ttu-id="093d6-258">[Örnek kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)) barındırma başlangıç uygulama senaryolarını gösterir:</span><span class="sxs-lookup"><span data-stu-id="093d6-258">The [sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="093d6-259">İki barındırma başlangıç derlemeleri (sınıf kitaplıkları), bellek içi yapılandırma anahtar-değer çiftleri her bir çifti ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="093d6-259">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="093d6-260">NuGet paketini (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="093d6-260">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="093d6-261">Sınıf kitaplığı (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="093d6-261">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="093d6-262">Bir barındırma başlatma deposu dağıtılan bir çalışma zamanı derleme etkinleştirilir (*StartupDiagnostics*).</span><span class="sxs-lookup"><span data-stu-id="093d6-262">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="093d6-263">Derleme, uygulamaya tanılama bilgileri sağlayan başlangıçta iki middlewares ekler:</span><span class="sxs-lookup"><span data-stu-id="093d6-263">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="093d6-264">Kaydedilen Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="093d6-264">Registered services</span></span>
  * <span data-ttu-id="093d6-265">Adres (Düzen, konak, temel yolu, yol, sorgu dizesi)</span><span class="sxs-lookup"><span data-stu-id="093d6-265">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="093d6-266">Bağlantı (uzak IP, uzak bağlantı noktasını, yerel IP yerel bağlantı noktası, istemci sertifikası)</span><span class="sxs-lookup"><span data-stu-id="093d6-266">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="093d6-267">İstek üst bilgileri</span><span class="sxs-lookup"><span data-stu-id="093d6-267">Request headers</span></span>
  * <span data-ttu-id="093d6-268">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="093d6-268">Environment variables</span></span>

<span data-ttu-id="093d6-269">Örneği çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="093d6-269">To run the sample:</span></span>

<span data-ttu-id="093d6-270">**NuGet paketinden etkinleştirme**</span><span class="sxs-lookup"><span data-stu-id="093d6-270">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="093d6-271">Derleme *HostingStartupPackage* ile paket [dotnet paketi](/dotnet/core/tools/dotnet-pack) komutu.</span><span class="sxs-lookup"><span data-stu-id="093d6-271">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="093d6-272">Paketin derleme adını eklemek *HostingStartupPackage* için `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="093d6-272">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="093d6-273">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="093d6-273">Compile and run the app.</span></span> <span data-ttu-id="093d6-274">Geliştirilmiş bir uygulamada (bir derleme zamanı Başvurusu) bir paket başvurusu yok.</span><span class="sxs-lookup"><span data-stu-id="093d6-274">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="093d6-275">A `<PropertyGroup>` uygulama projesinde paket projenin çıkış dosyasını belirtir. ( *.. / HostingStartupPackage/bin/Debug*) bir paket olarak.</span><span class="sxs-lookup"><span data-stu-id="093d6-275">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="093d6-276">Bu, uygulamanın paketi [NuGet.org](https://www.nuget.org/)'e yüklemeden paketi kullanmasına izin verir. Daha fazla bilgi için HostingStartupApp öğesinin proje dosyasındaki notlara bakın.</span><span class="sxs-lookup"><span data-stu-id="093d6-276">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```

1. <span data-ttu-id="093d6-277">Dizin sayfası tarafından işlenen hizmet yapılandırması anahtar değerleri paketin tarafından ayarlanan değerlerle eşleşen gözlemleyin `ServiceKeyInjection.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="093d6-277">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="093d6-278">Değişiklikler yaparsanız *HostingStartupPackage* proje ve yeniden derleyin, emin olmak için yerel NuGet paketi önbellekler temizlenir *HostingStartupApp* güncelleştirilmiş paket bir eski alır Paket yerel önbellekten.</span><span class="sxs-lookup"><span data-stu-id="093d6-278">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="093d6-279">Yerel NuGet önbellekleri temizlemek için aşağıdakileri yürütün [dotnet nuget Yereller](/dotnet/core/tools/dotnet-nuget-locals) komutu:</span><span class="sxs-lookup"><span data-stu-id="093d6-279">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```dotnetcli
dotnet nuget locals all --clear
```

<span data-ttu-id="093d6-280">**Bir sınıf kitaplığından etkinleştirme**</span><span class="sxs-lookup"><span data-stu-id="093d6-280">**Activation from a class library**</span></span>

1. <span data-ttu-id="093d6-281">Derleme *HostingStartupLibrary* sınıf kitaplığı ile [dotnet derleme](/dotnet/core/tools/dotnet-build) komutu.</span><span class="sxs-lookup"><span data-stu-id="093d6-281">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="093d6-282">Sınıf Kitaplığı'nızın derleme adını eklemek *HostingStartupLibrary* için `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="093d6-282">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="093d6-283">*Depo*-kopyalayarak Sınıf Kitaplığı'nızın derleme uygulamalarıyla *HostingStartupLibrary.dll* Sınıf Kitaplığı'nızın dosyasından çıktıyı uygulamanın derlenmiş *bin/Debug* klasör.</span><span class="sxs-lookup"><span data-stu-id="093d6-283">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="093d6-284">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="093d6-284">Compile and run the app.</span></span> <span data-ttu-id="093d6-285">Uygulamanın proje dosyasındaki bir `<ItemGroup>`, sınıf kitaplığının derlemesine ( *.\Bin\debug\netcoreapp3,\hostingstartuplibrary.dll*) başvurur (bir derleme zamanı başvurusu).</span><span class="sxs-lookup"><span data-stu-id="093d6-285">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp3.0\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="093d6-286">Notları HostingStartupApp'ın proje dosyasında daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="093d6-286">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\\bin\\Debug\\netcoreapp3.0\\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp3.0\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion> 
     </Reference>
   </ItemGroup>
   ```

1. <span data-ttu-id="093d6-287">Dizin sayfası tarafından işlenen hizmet yapılandırması anahtar değerleri sınıf kitaplığının tarafından ayarlanan değerlerle eşleşen gözlemleyin `ServiceKeyInjection.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="093d6-287">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="093d6-288">**Mağaza tarafından dağıtılan bir çalışma zamanı derlemesindeki etkinleştirme**</span><span class="sxs-lookup"><span data-stu-id="093d6-288">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="093d6-289">*StartupDiagnostics* proje kullandığı [PowerShell](/powershell/scripting/powershell-scripting) değiştirmek için kendi *StartupDiagnostics.deps.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="093d6-289">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="093d6-290">PowerShell, Windows 7 SP1 ve Windows Server 2008 R2 SP1 ile başlayarak Windows üzerinde varsayılan olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="093d6-290">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="093d6-291">PowerShell diğer platformlarda edinmek için bkz. [Windows PowerShell'i yükleme](/powershell/scripting/setup/installing-powershell#powershell-core).</span><span class="sxs-lookup"><span data-stu-id="093d6-291">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span></span>
1. <span data-ttu-id="093d6-292">*Runtimesyürüme* klasöründe *Build. ps1* betiğini yürütün.</span><span class="sxs-lookup"><span data-stu-id="093d6-292">Execute the *build.ps1* script in the *RuntimeStore* folder.</span></span> <span data-ttu-id="093d6-293">Betik:</span><span class="sxs-lookup"><span data-stu-id="093d6-293">The script:</span></span>
   * <span data-ttu-id="093d6-294">`StartupDiagnostics` paketini *obj\packages* klasöründe oluşturur.</span><span class="sxs-lookup"><span data-stu-id="093d6-294">Generates the `StartupDiagnostics` package in the *obj\packages* folder.</span></span>
   * <span data-ttu-id="093d6-295">*Mağaza* klasöründeki `StartupDiagnostics` çalışma zamanı deposunu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="093d6-295">Generates the runtime store for `StartupDiagnostics` in the *store* folder.</span></span> <span data-ttu-id="093d6-296">Betikteki `dotnet store` komutu, Windows 'a dağıtılan bir barındırma başlatması için `win7-x64` [çalışma zamanı tanımlayıcısı 'nı (RID)](/dotnet/core/rid-catalog) kullanır.</span><span class="sxs-lookup"><span data-stu-id="093d6-296">The `dotnet store` command in the script uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog) for a hosting startup deployed to Windows.</span></span> <span data-ttu-id="093d6-297">Farklı bir çalışma zamanı için barındırma başlangıcını sağlarken, betiğin 37. satırındaki doğru RID 'yi yerine koyun.</span><span class="sxs-lookup"><span data-stu-id="093d6-297">When providing the hosting startup for a different runtime, substitute the correct RID on line 37 of the script.</span></span> <span data-ttu-id="093d6-298">`StartupDiagnostics` çalışma zamanı deposu daha sonra derlemenin tüketilebileceği makinede kullanıcının veya sistem çalışma zamanı deposuna taşınır.</span><span class="sxs-lookup"><span data-stu-id="093d6-298">The runtime store for `StartupDiagnostics` would later be moved to the user's or system's runtime store on the machine where the assembly will be consumed.</span></span> <span data-ttu-id="093d6-299">`StartupDiagnostics` derlemesinin Kullanıcı çalışma zamanı deposu yüklemesi konumu *. DotNet/Store/x64/netcoreapp 3.0/startupdiagnostics/1.0.0/LIB/netcoreapp 3.0/startupdiagnostics. dll*' dir.</span><span class="sxs-lookup"><span data-stu-id="093d6-299">The user runtime store install location for the `StartupDiagnostics` assembly is *.dotnet/store/x64/netcoreapp3.0/startupdiagnostics/1.0.0/lib/netcoreapp3.0/StartupDiagnostics.dll*.</span></span>
   * <span data-ttu-id="093d6-300">*Additionaldeps* klasöründeki `StartupDiagnostics` için `additionalDeps` üretir.</span><span class="sxs-lookup"><span data-stu-id="093d6-300">Generates the `additionalDeps` for `StartupDiagnostics` in the *additionalDeps* folder.</span></span> <span data-ttu-id="093d6-301">Ek bağımlılıklar daha sonra kullanıcının veya sistem ek bağımlılıklarına taşınır.</span><span class="sxs-lookup"><span data-stu-id="093d6-301">The additional dependencies would later be moved to the user's or system's additional dependencies.</span></span> <span data-ttu-id="093d6-302">Kullanıcı `StartupDiagnostics` ek bağımlılıklar yüklemesi konumu *. DotNet/x64/additionalDeps/startupdiagnostics/Shared/Microsoft. NETCore. app/3.0.0/StartupDiagnostics. Deps. JSON*olur.</span><span class="sxs-lookup"><span data-stu-id="093d6-302">The user `StartupDiagnostics` additional dependencies install location is *.dotnet/x64/additionalDeps/StartupDiagnostics/shared/Microsoft.NETCore.App/3.0.0/StartupDiagnostics.deps.json*.</span></span>
   * <span data-ttu-id="093d6-303">*Dağıtım* klasörüne *Deploy. ps1* dosyasını koyar.</span><span class="sxs-lookup"><span data-stu-id="093d6-303">Places the *deploy.ps1* file in the *deployment* folder.</span></span>
1. <span data-ttu-id="093d6-304">*Dağıtım* klasöründe *Deploy. ps1* betiğini çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="093d6-304">Run the *deploy.ps1* script in the *deployment* folder.</span></span> <span data-ttu-id="093d6-305">Betik şunu ekler:</span><span class="sxs-lookup"><span data-stu-id="093d6-305">The script appends:</span></span>
   * <span data-ttu-id="093d6-306">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkenine `StartupDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="093d6-306">`StartupDiagnostics` to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="093d6-307">Barındırma başlangıç bağımlılıkları yolu (Runtimessımında projenin *dağıtım* klasöründe) `DOTNET_ADDITIONAL_DEPS` ortam değişkenine.</span><span class="sxs-lookup"><span data-stu-id="093d6-307">The hosting startup dependencies path (in the RuntimeStore project's *deployment* folder) to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
   * <span data-ttu-id="093d6-308">Çalışma zamanı depolama yolu (Runtimes, projenin *dağıtım* klasöründe) `DOTNET_SHARED_STORE` ortam değişkenine.</span><span class="sxs-lookup"><span data-stu-id="093d6-308">The runtime store path (in the RuntimeStore project's *deployment* folder) to the `DOTNET_SHARED_STORE` environment variable.</span></span>
1. <span data-ttu-id="093d6-309">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="093d6-309">Run the sample app.</span></span>
1. <span data-ttu-id="093d6-310">İstek `/services` uygulamanın görmek için uç nokta Hizmetleri kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="093d6-310">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="093d6-311">İstek `/diag` tanılama bilgileri görmek için uç nokta.</span><span class="sxs-lookup"><span data-stu-id="093d6-311">Request the `/diag` endpoint to see the diagnostic information.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="093d6-312"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> (barındırma başlatma) uygulaması, bir dış derlemeden başlatma sırasında bir uygulamaya iyileştirmeler ekler.</span><span class="sxs-lookup"><span data-stu-id="093d6-312">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="093d6-313">Örneğin, bir harici kitaplık ek yapılandırma sağlayıcıları ya da bir uygulama hizmetlerini barındıran bir başlangıç uygulaması kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="093d6-313">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span>

<span data-ttu-id="093d6-314">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="093d6-314">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="093d6-315">HostingStartup özniteliği</span><span class="sxs-lookup"><span data-stu-id="093d6-315">HostingStartup attribute</span></span>

<span data-ttu-id="093d6-316">A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) öznitelik, çalışma zamanında etkinleştirmek için bir barındırma başlangıç derleme varlığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="093d6-316">A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="093d6-317">Giriş derleme veya içeren derlemenin `Startup` sınıfı için taranan otomatik olarak `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="093d6-317">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="093d6-318">Aranacak derlemelerin listesini `HostingStartup` öznitelikleri içinde yapılandırmasından çalışma zamanında yüklendiği [WebHostDefaults.HostingStartupAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupAssembliesKey).</span><span class="sxs-lookup"><span data-stu-id="093d6-318">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupAssembliesKey).</span></span> <span data-ttu-id="093d6-319">Keşiften çıkarmak için derleme listesini alanından yüklenen [WebHostDefaults.HostingStartupExcludeAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupExcludeAssembliesKey).</span><span class="sxs-lookup"><span data-stu-id="093d6-319">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupExcludeAssembliesKey).</span></span> <span data-ttu-id="093d6-320">Daha fazla bilgi için bkz. [Web ana bilgisayarı: barındırma başlangıç derlemeleri](xref:fundamentals/host/web-host#hosting-startup-assemblies) ve [Web ana bilgisayarı: Başlangıç hariç derlemeleri barındırma](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="093d6-320">For more information, see [Web Host: Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) and [Web Host: Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span></span>

<span data-ttu-id="093d6-321">Aşağıdaki örnekte, barındırma başlangıç derlemenin ad alanıdır `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="093d6-321">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="093d6-322">Barındırma başlatma kodunu içeren sınıf `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="093d6-322">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="093d6-323">`HostingStartup` Öznitelik genellikle barındırma başlangıç derleme içinde bulunan `IHostingStartup` uygulama sınıf dosyası.</span><span class="sxs-lookup"><span data-stu-id="093d6-323">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="093d6-324">Yüklenen barındırma başlangıç derlemeler keşfedin</span><span class="sxs-lookup"><span data-stu-id="093d6-324">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="093d6-325">Yüklenen barındırma başlangıç derlemeleri bulmak için günlük kaydını etkinleştirmek ve uygulama günlüklerini kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="093d6-325">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="093d6-326">Derlemeler yüklenirken oluşan hataları günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="093d6-326">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="093d6-327">Yüklenen barındırma başlangıç derlemeler hata ayıklama düzeyinde kaydedilir ve tüm hatalar kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="093d6-327">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="093d6-328">Başlangıç derlemeleri barındırma otomatik yüklemeyi devre dışı</span><span class="sxs-lookup"><span data-stu-id="093d6-328">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="093d6-329">Başlangıç derlemeleri barındırma otomatik yüklemeyi devre dışı bırakmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="093d6-329">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="093d6-330">Tüm barındırma başlangıç derlemeleri yüklenmesini önlemek için aşağıdakilerden birini ayarlayın `true` veya `1`:</span><span class="sxs-lookup"><span data-stu-id="093d6-330">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>
  * <span data-ttu-id="093d6-331">[Barındırma başlangıç önlemek](xref:fundamentals/host/web-host#prevent-hosting-startup) ana bilgisayar yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="093d6-331">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
  * <span data-ttu-id="093d6-332">`ASPNETCORE_PREVENTHOSTINGSTARTUP` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="093d6-332">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>
* <span data-ttu-id="093d6-333">Belirli barındırma başlangıç derlemeleri yüklenmesini önlemek için aşağıdakilerden birini başlangıçta hariç tutmak için başlangıç derlemeleri barındırma noktalı virgülle ayrılmış bir dizeye ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="093d6-333">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>
  * <span data-ttu-id="093d6-334">[Başlangıç hariç derlemeleri barındırma](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) ana bilgisayar yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="093d6-334">[Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) host configuration setting.</span></span>
  * <span data-ttu-id="093d6-335">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="093d6-335">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

<span data-ttu-id="093d6-336">Hem ana bilgisayar yapılandırma ayarı ve ortam değişkenini ayarlarsanız, ana bilgisayar ayarını davranışını denetler.</span><span class="sxs-lookup"><span data-stu-id="093d6-336">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="093d6-337">Ana bilgisayar ayarı veya ortam değişkenini kullanarak barındırma başlangıç derlemeleri devre dışı bırakma, derlemenin genel olarak devre dışı bırakır ve uygulama çeşitli özelliklerini devre dışı bırakabilir.</span><span class="sxs-lookup"><span data-stu-id="093d6-337">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="093d6-338">Project</span><span class="sxs-lookup"><span data-stu-id="093d6-338">Project</span></span>

<span data-ttu-id="093d6-339">Bir barındırma başlatma aşağıdaki proje türlerini birini oluşturun:</span><span class="sxs-lookup"><span data-stu-id="093d6-339">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="093d6-340">Sınıf kitaplığı</span><span class="sxs-lookup"><span data-stu-id="093d6-340">Class library</span></span>](#class-library)
* [<span data-ttu-id="093d6-341">Konsol uygulaması giriş noktası olmayan</span><span class="sxs-lookup"><span data-stu-id="093d6-341">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="093d6-342">Sınıf kitaplığı</span><span class="sxs-lookup"><span data-stu-id="093d6-342">Class library</span></span>

<span data-ttu-id="093d6-343">Bir barındırma başlangıç geliştirme'de sınıf kitaplığının sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="093d6-343">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="093d6-344">Kitaplığı içeren bir `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="093d6-344">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="093d6-345">[Örnek kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) bir Razor sayfaları uygulamasını içeren *HostingStartupApp*ve bir sınıf kitaplığı *HostingStartupLibrary*.</span><span class="sxs-lookup"><span data-stu-id="093d6-345">The [sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="093d6-346">Sınıf kitaplığı:</span><span class="sxs-lookup"><span data-stu-id="093d6-346">The class library:</span></span>

* <span data-ttu-id="093d6-347">Bir barındırma başlangıç sınıfı içeren `ServiceKeyInjection`, uygulayan `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="093d6-347">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="093d6-348">`ServiceKeyInjection` bellek içi yapılandırma Sağlayıcısı'nı kullanarak uygulamanın yapılandırmasına hizmet dizeleri çifti ekler ([AddInMemoryCollection](xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*)).</span><span class="sxs-lookup"><span data-stu-id="093d6-348">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*)).</span></span>
* <span data-ttu-id="093d6-349">İçeren bir `HostingStartup` barındırma startup şirketinizin ad alanını ve sınıf tanımlayan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="093d6-349">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="093d6-350">`ServiceKeyInjection` sınıfının <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> yöntemi bir uygulamaya geliştirmeler eklemek için bir <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> kullanır.</span><span class="sxs-lookup"><span data-stu-id="093d6-350">The `ServiceKeyInjection` class's <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> method uses an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> to add enhancements to an app.</span></span>

<span data-ttu-id="093d6-351">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="093d6-351">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="093d6-352">Uygulama dizin sayfasına okur ve Sınıf Kitaplığı'nızın barındırma başlangıç derlemesi tarafından ayarlanmış iki anahtarı için yapılandırma değerlerini işler:</span><span class="sxs-lookup"><span data-stu-id="093d6-352">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="093d6-353">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="093d6-353">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="093d6-354">[Örnek kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) de ayrı bir barındırma başlatma sağlayan bir NuGet paketi proje içerir *HostingStartupPackage*.</span><span class="sxs-lookup"><span data-stu-id="093d6-354">The [sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="093d6-355">Paket, daha önce açıklanan Sınıf Kitaplığı'nın aynı özelliklere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="093d6-355">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="093d6-356">Paketi:</span><span class="sxs-lookup"><span data-stu-id="093d6-356">The package:</span></span>

* <span data-ttu-id="093d6-357">Bir barındırma başlangıç sınıfı içeren `ServiceKeyInjection`, uygulayan `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="093d6-357">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="093d6-358">`ServiceKeyInjection` Hizmet dizeleri çifti uygulamanın yapılandırmasına ekler.</span><span class="sxs-lookup"><span data-stu-id="093d6-358">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="093d6-359">İçeren bir `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="093d6-359">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="093d6-360">*HostingStartupPackage/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="093d6-360">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="093d6-361">Uygulama dizin sayfasına okur ve paketin barındırma başlangıç derlemesi tarafından ayarlanmış iki anahtarı için yapılandırma değerlerini işler:</span><span class="sxs-lookup"><span data-stu-id="093d6-361">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="093d6-362">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="093d6-362">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="093d6-363">Konsol uygulaması giriş noktası olmayan</span><span class="sxs-lookup"><span data-stu-id="093d6-363">Console app without an entry point</span></span>

<span data-ttu-id="093d6-364">*Bu yaklaşım, yalnızca .NET Framework .NET Core uygulamaları için kullanılabilir.*</span><span class="sxs-lookup"><span data-stu-id="093d6-364">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="093d6-365">Bir derleme zamanı başvurusu için etkinleştirme gerektirmeyen bir dinamik barındırma başlangıç geliştirmesi de içeren giriş noktası olmayan bir konsol uygulaması sağlanan bir `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="093d6-365">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point that contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="093d6-366">Konsol uygulaması yayımlama, çalışma zamanı Mağazası'ndan kullanılabilen bir barındırma başlangıç bütünleştirilmiş kod üretir.</span><span class="sxs-lookup"><span data-stu-id="093d6-366">Publishing the console app produces a hosting startup assembly that can be consumed from the runtime store.</span></span>

<span data-ttu-id="093d6-367">Bir konsol uygulaması giriş noktası olmayan, çünkü bu işlemde kullanılır:</span><span class="sxs-lookup"><span data-stu-id="093d6-367">A console app without an entry point is used in this process because:</span></span>

* <span data-ttu-id="093d6-368">Bağımlılıkları dosya barındırma başlangıç derlemedeki barındırma için başlangıç kullanmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="093d6-368">A dependencies file is required to consume the hosting startup in the hosting startup assembly.</span></span> <span data-ttu-id="093d6-369">Kitaplık değil bir uygulama yayımlama tarafından üretilen bir çalıştırılabilir uygulama varlık bağımlılıkları dosyasıdır.</span><span class="sxs-lookup"><span data-stu-id="093d6-369">A dependencies file is a runnable app asset that's produced by publishing an app, not a library.</span></span>
* <span data-ttu-id="093d6-370">Bir kitaplık doğrudan eklenemez [çalışma zamanı Paket Deposu](/dotnet/core/deploying/runtime-store), paylaşılan çalışma zamanını hedefleyen çalıştırılabilir bir proje gerektirir.</span><span class="sxs-lookup"><span data-stu-id="093d6-370">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>

<span data-ttu-id="093d6-371">Dinamik barındırma başlangıç oluşturulmasını içinde:</span><span class="sxs-lookup"><span data-stu-id="093d6-371">In the creation of a dynamic hosting startup:</span></span>

* <span data-ttu-id="093d6-372">Bir giriş noktası olmadan bir barındırma başlangıç derleme konsol uygulamasından oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="093d6-372">A hosting startup assembly is created from the console app without an entry point that:</span></span>
  * <span data-ttu-id="093d6-373">İçeren bir sınıfı içeren `IHostingStartup` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="093d6-373">Includes a class that contains the `IHostingStartup` implementation.</span></span>
  * <span data-ttu-id="093d6-374">İçeren bir [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) tanımlamak için öznitelik `IHostingStartup` uygulama sınıfı.</span><span class="sxs-lookup"><span data-stu-id="093d6-374">Includes a [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute to identify the `IHostingStartup` implementation class.</span></span>
* <span data-ttu-id="093d6-375">Konsol uygulaması barındırma startup şirketinizin bağımlılıkları almak için yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="093d6-375">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="093d6-376">Kullanılmayan bağımlılıkları bağımlılıkları dosyasından atılır konsol uygulaması yayımlama bir sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="093d6-376">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
* <span data-ttu-id="093d6-377">Bağımlılıkları dosyası, çalışma zamanı barındırma başlangıç derleme konumunu ayarlamak için değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="093d6-377">The dependencies file is modified to set the runtime location of the hosting startup assembly.</span></span>
* <span data-ttu-id="093d6-378">Barındırma başlangıç derleme ve bağımlılıkları dosyası çalışma zamanı paketi deposuna yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="093d6-378">The hosting startup assembly and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="093d6-379">Barındırma başlangıç derleme ve bağımlılıkları dosyasını bulmak için bir ortam değişkenlerini çift içinde listelendikleri.</span><span class="sxs-lookup"><span data-stu-id="093d6-379">To discover the hosting startup assembly and its dependencies file, they're listed in a pair of environment variables.</span></span>

<span data-ttu-id="093d6-380">Konsol uygulama başvuruları [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) paket:</span><span class="sxs-lookup"><span data-stu-id="093d6-380">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

<span data-ttu-id="093d6-381">[Hostingstartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) özniteliği, bir sınıfı <xref:Microsoft.AspNetCore.Hosting.IWebHost>oluştururken yükleme ve yürütme için `IHostingStartup` bir uygulama olarak tanımlar.</span><span class="sxs-lookup"><span data-stu-id="093d6-381">A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the <xref:Microsoft.AspNetCore.Hosting.IWebHost>.</span></span> <span data-ttu-id="093d6-382">Aşağıdaki örnekte, ad alanı `StartupEnhancement`, ve sınıf `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="093d6-382">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="093d6-383">Arabirimini uygulayan bir sınıf `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="093d6-383">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="093d6-384">Sınıfın <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> yöntemi bir uygulamaya iyileştirmeler eklemek için bir <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> kullanır.</span><span class="sxs-lookup"><span data-stu-id="093d6-384">The class's <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> method uses an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> to add enhancements to an app.</span></span> <span data-ttu-id="093d6-385">`IHostingStartup.Configure` barındırma başlangıç derleme önce çalışma zamanı tarafından çağrılır `Startup.Configure` kullanıcı kodunda barındırma başlangıç derlemesi tarafından sağlanan herhangi bir yapılandırma üzerine yazmak kullanıcı kodu sağlar.</span><span class="sxs-lookup"><span data-stu-id="093d6-385">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="093d6-386">Bir `IHostingStartup` projesi oluştururken, bağımlılıklar dosyası ( *. Deps. JSON*) derlemenin `runtime` konumunu *bin* klasörüne ayarlar:</span><span class="sxs-lookup"><span data-stu-id="093d6-386">When building an `IHostingStartup` project, the dependencies file (*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="093d6-387">Dosyanın yalnızca bir parçası olarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="093d6-387">Only part of the file is shown.</span></span> <span data-ttu-id="093d6-388">Derleme adı örnekte `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="093d6-388">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="configuration-provided-by-the-hosting-startup"></a><span data-ttu-id="093d6-389">Barındırma başlatma tarafından belirtilen yapılandırma</span><span class="sxs-lookup"><span data-stu-id="093d6-389">Configuration provided by the hosting startup</span></span>

<span data-ttu-id="093d6-390">Yapılandırma işlemi, barındırma başlatmasının yapılandırmasının öncelikli olmasını mı yoksa uygulamanın yapılandırmasının öncelikli olmasını mı istediğinize bağlı olarak iki yaklaşımdan yararlanabilir:</span><span class="sxs-lookup"><span data-stu-id="093d6-390">There are two approaches to handling configuration depending on whether you want the hosting startup's configuration to take precedence or the app's configuration to take precedence:</span></span>

1. <span data-ttu-id="093d6-391">Uygulamanın <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> temsilcileri çalıştırıldıktan sonra yapılandırmayı yüklemek için <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> kullanarak uygulamaya yapılandırma sağlayın.</span><span class="sxs-lookup"><span data-stu-id="093d6-391">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> to load the configuration after the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="093d6-392">Barındırma başlangıç yapılandırması, uygulamanın yapılandırmasına bu yaklaşımı kullanarak öncelik kazanır.</span><span class="sxs-lookup"><span data-stu-id="093d6-392">Hosting startup configuration takes priority over the app's configuration using this approach.</span></span>
1. <span data-ttu-id="093d6-393">Uygulamanın <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> temsilcileri yürütmeden önce yapılandırmayı yüklemek için <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> kullanarak uygulamaya yapılandırma sağlayın.</span><span class="sxs-lookup"><span data-stu-id="093d6-393">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> to load the configuration before the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="093d6-394">Uygulamanın yapılandırma değerleri, bu yaklaşımı kullanarak barındırma başlatma tarafından sağlananlara göre önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="093d6-394">The app's configuration values take priority over those provided by the hosting startup using this approach.</span></span>

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

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="093d6-395">Barındırma başlangıç derlemeyi belirtin</span><span class="sxs-lookup"><span data-stu-id="093d6-395">Specify the hosting startup assembly</span></span>

<span data-ttu-id="093d6-396">Bir sınıf kitaplığı - veya konsol uygulaması sağlanan-başlangıç barındırma için barındırma başlangıç derlemenin adını belirtin. `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="093d6-396">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="093d6-397">Ortam değişkenini derlemelerin noktalı virgülle ayrılmış bir listedir.</span><span class="sxs-lookup"><span data-stu-id="093d6-397">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="093d6-398">Yalnızca barındırma başlangıç derlemeler için taranan `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="093d6-398">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="093d6-399">Örnek uygulama için *HostingStartupApp*, daha önce açıklanan barındırma startup'lar bulmak için ortam değişkeni şu değere ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="093d6-399">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="093d6-400">Bir barındırma başlangıç derleme kullanılarak da ayarlanabilir [barındırma başlangıç derlemeleri](xref:fundamentals/host/web-host#hosting-startup-assemblies) ana bilgisayar yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="093d6-400">A hosting startup assembly can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="093d6-401">Birden çok barındırma başlatması varsa, <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> yöntemleri derlemelerin listelendiği sırada yürütülür.</span><span class="sxs-lookup"><span data-stu-id="093d6-401">When multiple hosting startup assembles are present, their <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="093d6-402">Etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="093d6-402">Activation</span></span>

<span data-ttu-id="093d6-403">Başlangıç etkinleştirme barındırmak için Seçenekler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="093d6-403">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="093d6-404">[Çalışma zamanı deposu](#runtime-store) &ndash; etkinleştirme, etkinleştirme için bir derleme zamanı başvurusu gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="093d6-404">[Runtime store](#runtime-store) &ndash; Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="093d6-405">Örnek uygulama barındırma başlangıç derleme ve bağımlılıkları dosyalar bir klasöre yerleştirir *dağıtım*multimachine ortamında barındırma başlangıç dağıtımını kolaylaştırmak için.</span><span class="sxs-lookup"><span data-stu-id="093d6-405">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="093d6-406">*Dağıtım* klasörü oluşturur veya değiştirir barındırma için başlangıç etkinleştirmek için dağıtım sistemi ortam değişkenlerini bir PowerShell betiğini de içerir.</span><span class="sxs-lookup"><span data-stu-id="093d6-406">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="093d6-407">Etkinleştirme için gerekli derleme zamanı başvurusu</span><span class="sxs-lookup"><span data-stu-id="093d6-407">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="093d6-408">NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="093d6-408">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="093d6-409">Proje bin klasörü</span><span class="sxs-lookup"><span data-stu-id="093d6-409">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="093d6-410">Çalışma zamanı deposu</span><span class="sxs-lookup"><span data-stu-id="093d6-410">Runtime store</span></span>

<span data-ttu-id="093d6-411">Barındırma başlangıç uygulaması yerleştirilir [çalışma zamanı deposu](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="093d6-411">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="093d6-412">Gelişmiş uygulama tarafından bir derleme zamanı başvurusu derleme için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="093d6-412">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="093d6-413">Barındırma başlangıç oluşturulduktan sonra bir çalışma zamanı deposu bildirim proje dosyası kullanılarak oluşturulur ve [dotnet deposu](/dotnet/core/tools/dotnet-store) komutu.</span><span class="sxs-lookup"><span data-stu-id="093d6-413">After the hosting startup is built, a runtime store is generated using the manifest project file and the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```dotnetcli
dotnet store --manifest {MANIFEST FILE} --runtime {RUNTIME IDENTIFIER} --output {OUTPUT LOCATION} --skip-optimization
```

<span data-ttu-id="093d6-414">Örnek uygulamada (*RuntimeStore* Proje) şu komut kullanılır:</span><span class="sxs-lookup"><span data-stu-id="093d6-414">In the sample app (*RuntimeStore* project) the following command is used:</span></span>

```dotnetcli
dotnet store --manifest store.manifest.csproj --runtime win7-x64 --output ./deployment/store --skip-optimization
```

<span data-ttu-id="093d6-415">Çalışma zamanı deposu bulmak çalışma zamanı için çalışma zamanı deponun konumunu eklenen `DOTNET_SHARED_STORE` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="093d6-415">For the runtime to discover the runtime store, the runtime store's location is added to the `DOTNET_SHARED_STORE` environment variable.</span></span>

<span data-ttu-id="093d6-416">**Değiştirme ve barındırma startup şirketinizin bağımlılıkları dosyasının yerleştirin**</span><span class="sxs-lookup"><span data-stu-id="093d6-416">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="093d6-417">Geliştirme için bir paket başvurusu olmadan geliştirme etkinleştirmek için çalışma zamanı ile ek bağımlılıklar belirtin `additionalDeps`.</span><span class="sxs-lookup"><span data-stu-id="093d6-417">To activate the enhancement without a package reference to the enhancement, specify additional dependencies to the runtime with `additionalDeps`.</span></span> <span data-ttu-id="093d6-418">`additionalDeps` sağlar:</span><span class="sxs-lookup"><span data-stu-id="093d6-418">`additionalDeps` allows you to:</span></span>

* <span data-ttu-id="093d6-419">Başlangıçta uygulamanın kendi *. Deps* . JSON dosyasıyla birleştirilecek bir dizi ek *. Deps. JSON* dosyası sağlayarak uygulamanın kitaplık grafiğini genişletin.</span><span class="sxs-lookup"><span data-stu-id="093d6-419">Extend the app's library graph by providing a set of additional *.deps.json* files to merge with the app's own *.deps.json* file on startup.</span></span>
* <span data-ttu-id="093d6-420">Barındırma başlangıç derlemesine bulunabilir ve yüklenebilir olun.</span><span class="sxs-lookup"><span data-stu-id="093d6-420">Make the hosting startup assembly discoverable and loadable.</span></span>

<span data-ttu-id="093d6-421">Ek Bağımlılıklar dosyası oluşturmak için önerilen yaklaşımdır:</span><span class="sxs-lookup"><span data-stu-id="093d6-421">The recommended approach for generating the additional dependencies file is to:</span></span>

 1. <span data-ttu-id="093d6-422">Yürütme `dotnet publish` önceki bölümde başvurulan çalışma zamanı deposu bildirim dosyası üzerinde.</span><span class="sxs-lookup"><span data-stu-id="093d6-422">Execute `dotnet publish` on the runtime store manifest file referenced in the previous section.</span></span>
 1. <span data-ttu-id="093d6-423">Bildirimlerin bildirim başvurusunu ve sonuçta elde edilen *. Deps. JSON* dosyasının `runtime` bölümünü kaldırın.</span><span class="sxs-lookup"><span data-stu-id="093d6-423">Remove the manifest reference from libraries and the `runtime` section of the resulting *.deps.json* file.</span></span>

<span data-ttu-id="093d6-424">Örnek projesinde `store.manifest/1.0.0` özelliği kaldırılır `targets` ve `libraries` bölümü:</span><span class="sxs-lookup"><span data-stu-id="093d6-424">In the example project, the `store.manifest/1.0.0` property is removed from the `targets` and `libraries` section:</span></span>

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

<span data-ttu-id="093d6-425">*. Deps. JSON* dosyasını şu konuma yerleştirin:</span><span class="sxs-lookup"><span data-stu-id="093d6-425">Place the *.deps.json* file into the following location:</span></span>

```
{ADDITIONAL DEPENDENCIES PATH}/shared/{SHARED FRAMEWORK NAME}/{SHARED FRAMEWORK VERSION}/{ENHANCEMENT ASSEMBLY NAME}.deps.json
```

* <span data-ttu-id="093d6-426">`{ADDITIONAL DEPENDENCIES PATH}` &ndash; Eklenen konumu `DOTNET_ADDITIONAL_DEPS` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="093d6-426">`{ADDITIONAL DEPENDENCIES PATH}` &ndash; Location added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
* <span data-ttu-id="093d6-427">`{SHARED FRAMEWORK NAME}` &ndash; Bu ek bağımlılıklar dosyası için gerekli framework paylaşılan.</span><span class="sxs-lookup"><span data-stu-id="093d6-427">`{SHARED FRAMEWORK NAME}` &ndash; Shared framework required for this additional dependencies file.</span></span>
* <span data-ttu-id="093d6-428">`{SHARED FRAMEWORK VERSION}` &ndash; Minimum paylaşılan framework sürümü.</span><span class="sxs-lookup"><span data-stu-id="093d6-428">`{SHARED FRAMEWORK VERSION}` &ndash; Minimum shared framework version.</span></span>
* <span data-ttu-id="093d6-429">`{ENHANCEMENT ASSEMBLY NAME}` &ndash; Geliştirme'nın bütünleştirilmiş kod adı.</span><span class="sxs-lookup"><span data-stu-id="093d6-429">`{ENHANCEMENT ASSEMBLY NAME}` &ndash; The enhancement's assembly name.</span></span>

<span data-ttu-id="093d6-430">Örnek uygulamada (*RuntimeStore* Proje), ek bağımlılıklar dosyası şu konuma yerleştirilir:</span><span class="sxs-lookup"><span data-stu-id="093d6-430">In the sample app (*RuntimeStore* project), the additional dependencies file is placed into the following location:</span></span>

```
deployment/additionalDeps/shared/Microsoft.AspNetCore.App/2.1.0/StartupDiagnostics.deps.json
```

<span data-ttu-id="093d6-431">Ek Bağımlılıklar dosya konumunu eklenir çalışma zamanı depolama konumu bulmak çalışma zamanı için `DOTNET_ADDITIONAL_DEPS` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="093d6-431">For runtime to discover the runtime store location, the additional dependencies file location is added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="093d6-432">Örnek uygulamada (*RuntimeStore* Proje), çalışma zamanı deposu oluşturma ve dosya kullanarak gerçekleştirilir ek bağımlılıklar oluşturma bir [PowerShell](/powershell/scripting/powershell-scripting) betiği.</span><span class="sxs-lookup"><span data-stu-id="093d6-432">In the sample app (*RuntimeStore* project), building the runtime store and generating the additional dependencies file is accomplished using a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span>

<span data-ttu-id="093d6-433">Çeşitli işletim sistemleri için ortam değişkenlerini ayarlama örnekleri için bkz: [birden fazla ortam kullanayım](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="093d6-433">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="093d6-434">**Dağıtım**</span><span class="sxs-lookup"><span data-stu-id="093d6-434">**Deployment**</span></span>

<span data-ttu-id="093d6-435">Bir barındırma başlatma multimachine ortamında dağıtımını kolaylaştırmak için örnek uygulamayı oluşturur. bir *dağıtım* içeren yayımlanan çıkış klasöründe:</span><span class="sxs-lookup"><span data-stu-id="093d6-435">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="093d6-436">Barındırma başlangıç çalışma zamanı deposu.</span><span class="sxs-lookup"><span data-stu-id="093d6-436">The hosting startup runtime store.</span></span>
* <span data-ttu-id="093d6-437">Barındırma başlangıç bağımlılıkları dosyası.</span><span class="sxs-lookup"><span data-stu-id="093d6-437">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="093d6-438">Bir PowerShell Betiği oluşturur veya değiştirir `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, ve `DOTNET_ADDITIONAL_DEPS` barındırma başlangıç etkinleştirmeyi desteklemek için.</span><span class="sxs-lookup"><span data-stu-id="093d6-438">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="093d6-439">Komut dosyası dağıtım sistemde bir yönetici PowerShell komut isteminden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="093d6-439">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="093d6-440">NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="093d6-440">NuGet package</span></span>

<span data-ttu-id="093d6-441">Bir NuGet paketi barındırma bir başlangıç geliştirmesi sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="093d6-441">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="093d6-442">Paketin bir `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="093d6-442">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="093d6-443">Paket tarafından sağlanan barındırma başlangıç türleri, aşağıdaki yaklaşımlardan birini kullanarak uygulama için kullanıma sunulur:</span><span class="sxs-lookup"><span data-stu-id="093d6-443">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="093d6-444">Gelişmiş uygulamanın proje dosyası, barındırma başlangıç için bir paket başvurusu uygulamanın proje dosyası (bir derleme zamanı Başvurusu) sağlar.</span><span class="sxs-lookup"><span data-stu-id="093d6-444">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="093d6-445">Derleme zamanı başvurusu yerinde olduğunda, barındırma başlangıç derlemesi ve tüm bağımlılıkları uygulamanın bağımlılık dosyasına ( *. Deps. JSON*) dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="093d6-445">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="093d6-446">Bu yaklaşım, yayımlanan bir barındırma başlangıç derleme paketi uygulandığı [nuget.org](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="093d6-446">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="093d6-447">Barındırma startup şirketinizin bağımlılıkları dosyasının açıklandığı gibi gelişmiş uygulama için kullanılabilir hale getirileceğini [çalışma zamanı deposu](#runtime-store) bölümü (olmadan, bir derleme zamanı Başvurusu).</span><span class="sxs-lookup"><span data-stu-id="093d6-447">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="093d6-448">NuGet paketlerini ve çalışma zamanı mağazası hakkında daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="093d6-448">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="093d6-449">Platformlar arası araçlarla NuGet paketi oluşturma</span><span class="sxs-lookup"><span data-stu-id="093d6-449">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="093d6-450">Paket yayımlama</span><span class="sxs-lookup"><span data-stu-id="093d6-450">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="093d6-451">Çalışma zamanı paket deposu</span><span class="sxs-lookup"><span data-stu-id="093d6-451">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="093d6-452">Proje bin klasörü</span><span class="sxs-lookup"><span data-stu-id="093d6-452">Project bin folder</span></span>

<span data-ttu-id="093d6-453">Bir barındırma başlangıç geliştirmesi tarafından sağlanan bir *bin*-Gelişmiş Uygulama derlemesinde dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="093d6-453">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="093d6-454">Derleme tarafından sağlanan barındırma başlangıç türleri, aşağıdaki yaklaşımlardan biri kullanılarak uygulama için kullanılabilir hale getirilir:</span><span class="sxs-lookup"><span data-stu-id="093d6-454">The hosting startup types provided by the assembly are made available to the app using one of the following approaches:</span></span>

* <span data-ttu-id="093d6-455">Gelişmiş uygulamanın proje dosyası, barındırma başlangıç (bir derleme zamanı başvuru) bir bütünleştirilmiş kod başvurusu yapar.</span><span class="sxs-lookup"><span data-stu-id="093d6-455">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="093d6-456">Derleme zamanı başvurusu yerinde olduğunda, barındırma başlangıç derlemesi ve tüm bağımlılıkları uygulamanın bağımlılık dosyasına ( *. Deps. JSON*) dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="093d6-456">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="093d6-457">Bu yaklaşım, dağıtım senaryosu barındırma başlatmasının derlemesine ( *. dll* dosyası) bir derleme zamanı başvurusu yapmak ve derlemeyi şu şekilde taşımak için çağırdığında geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="093d6-457">This approach applies when the deployment scenario calls for making a compile-time reference to the hosting startup's assembly (*.dll* file) and moving the assembly to either:</span></span>
  * <span data-ttu-id="093d6-458">Tüketen proje.</span><span class="sxs-lookup"><span data-stu-id="093d6-458">The consuming project.</span></span>
  * <span data-ttu-id="093d6-459">Tüketim Projesi tarafından erişilebilen bir konum.</span><span class="sxs-lookup"><span data-stu-id="093d6-459">A location accessible by the consuming project.</span></span>
* <span data-ttu-id="093d6-460">Barındırma startup şirketinizin bağımlılıkları dosyasının açıklandığı gibi gelişmiş uygulama için kullanılabilir hale getirileceğini [çalışma zamanı deposu](#runtime-store) bölümü (olmadan, bir derleme zamanı Başvurusu).</span><span class="sxs-lookup"><span data-stu-id="093d6-460">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>
* <span data-ttu-id="093d6-461">.NET Framework hedeflenirken, derleme varsayılan yükleme bağlamında yüklenebilir olur; bu, .NET Framework, derlemenin aşağıdaki konumlardan birinde bulunduğu anlamına gelir:</span><span class="sxs-lookup"><span data-stu-id="093d6-461">When targeting the .NET Framework, the assembly is loadable in the default load context, which on .NET Framework means that the assembly is located at either of the following locations:</span></span>
  * <span data-ttu-id="093d6-462">Uygulama temel yolu, uygulamanın yürütülebilir dosyasının ( *. exe*) bulunduğu *bin* klasörünü &ndash;.</span><span class="sxs-lookup"><span data-stu-id="093d6-462">Application base path &ndash; The *bin* folder where the app's executable (*.exe*) is located.</span></span>
  * <span data-ttu-id="093d6-463">Genel bütünleştirilmiş kod önbelleği (GAC) &ndash; GAC, birkaç .NET Framework uygulamanın paylaştığı derlemeleri depolar.</span><span class="sxs-lookup"><span data-stu-id="093d6-463">Global Assembly Cache (GAC) &ndash; The GAC stores assemblies that several .NET Framework apps share.</span></span> <span data-ttu-id="093d6-464">Daha fazla bilgi için, bkz. [nasıl yapılır: bir derlemeyi genel derleme önbelleğine yüklemek](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) .NET Framework belgeleri.</span><span class="sxs-lookup"><span data-stu-id="093d6-464">For more information, see [How to: Install an assembly into the global assembly cache](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) in the .NET Framework documentation.</span></span>

## <a name="sample-code"></a><span data-ttu-id="093d6-465">Örnek kod</span><span class="sxs-lookup"><span data-stu-id="093d6-465">Sample code</span></span>

<span data-ttu-id="093d6-466">[Örnek kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)) barındırma başlangıç uygulama senaryolarını gösterir:</span><span class="sxs-lookup"><span data-stu-id="093d6-466">The [sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="093d6-467">İki barındırma başlangıç derlemeleri (sınıf kitaplıkları), bellek içi yapılandırma anahtar-değer çiftleri her bir çifti ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="093d6-467">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="093d6-468">NuGet paketini (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="093d6-468">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="093d6-469">Sınıf kitaplığı (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="093d6-469">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="093d6-470">Bir barındırma başlatma deposu dağıtılan bir çalışma zamanı derleme etkinleştirilir (*StartupDiagnostics*).</span><span class="sxs-lookup"><span data-stu-id="093d6-470">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="093d6-471">Derleme, uygulamaya tanılama bilgileri sağlayan başlangıçta iki middlewares ekler:</span><span class="sxs-lookup"><span data-stu-id="093d6-471">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="093d6-472">Kaydedilen Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="093d6-472">Registered services</span></span>
  * <span data-ttu-id="093d6-473">Adres (Düzen, konak, temel yolu, yol, sorgu dizesi)</span><span class="sxs-lookup"><span data-stu-id="093d6-473">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="093d6-474">Bağlantı (uzak IP, uzak bağlantı noktasını, yerel IP yerel bağlantı noktası, istemci sertifikası)</span><span class="sxs-lookup"><span data-stu-id="093d6-474">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="093d6-475">İstek üst bilgileri</span><span class="sxs-lookup"><span data-stu-id="093d6-475">Request headers</span></span>
  * <span data-ttu-id="093d6-476">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="093d6-476">Environment variables</span></span>

<span data-ttu-id="093d6-477">Örneği çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="093d6-477">To run the sample:</span></span>

<span data-ttu-id="093d6-478">**NuGet paketinden etkinleştirme**</span><span class="sxs-lookup"><span data-stu-id="093d6-478">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="093d6-479">Derleme *HostingStartupPackage* ile paket [dotnet paketi](/dotnet/core/tools/dotnet-pack) komutu.</span><span class="sxs-lookup"><span data-stu-id="093d6-479">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="093d6-480">Paketin derleme adını eklemek *HostingStartupPackage* için `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="093d6-480">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="093d6-481">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="093d6-481">Compile and run the app.</span></span> <span data-ttu-id="093d6-482">Geliştirilmiş bir uygulamada (bir derleme zamanı Başvurusu) bir paket başvurusu yok.</span><span class="sxs-lookup"><span data-stu-id="093d6-482">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="093d6-483">A `<PropertyGroup>` uygulama projesinde paket projenin çıkış dosyasını belirtir. ( *.. / HostingStartupPackage/bin/Debug*) bir paket olarak.</span><span class="sxs-lookup"><span data-stu-id="093d6-483">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="093d6-484">Bu, uygulamanın paketi [NuGet.org](https://www.nuget.org/)'e yüklemeden paketi kullanmasına izin verir. Daha fazla bilgi için HostingStartupApp öğesinin proje dosyasındaki notlara bakın.</span><span class="sxs-lookup"><span data-stu-id="093d6-484">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```

1. <span data-ttu-id="093d6-485">Dizin sayfası tarafından işlenen hizmet yapılandırması anahtar değerleri paketin tarafından ayarlanan değerlerle eşleşen gözlemleyin `ServiceKeyInjection.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="093d6-485">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="093d6-486">Değişiklikler yaparsanız *HostingStartupPackage* proje ve yeniden derleyin, emin olmak için yerel NuGet paketi önbellekler temizlenir *HostingStartupApp* güncelleştirilmiş paket bir eski alır Paket yerel önbellekten.</span><span class="sxs-lookup"><span data-stu-id="093d6-486">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="093d6-487">Yerel NuGet önbellekleri temizlemek için aşağıdakileri yürütün [dotnet nuget Yereller](/dotnet/core/tools/dotnet-nuget-locals) komutu:</span><span class="sxs-lookup"><span data-stu-id="093d6-487">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```dotnetcli
dotnet nuget locals all --clear
```

<span data-ttu-id="093d6-488">**Bir sınıf kitaplığından etkinleştirme**</span><span class="sxs-lookup"><span data-stu-id="093d6-488">**Activation from a class library**</span></span>

1. <span data-ttu-id="093d6-489">Derleme *HostingStartupLibrary* sınıf kitaplığı ile [dotnet derleme](/dotnet/core/tools/dotnet-build) komutu.</span><span class="sxs-lookup"><span data-stu-id="093d6-489">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="093d6-490">Sınıf Kitaplığı'nızın derleme adını eklemek *HostingStartupLibrary* için `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="093d6-490">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="093d6-491">*Depo*-kopyalayarak Sınıf Kitaplığı'nızın derleme uygulamalarıyla *HostingStartupLibrary.dll* Sınıf Kitaplığı'nızın dosyasından çıktıyı uygulamanın derlenmiş *bin/Debug* klasör.</span><span class="sxs-lookup"><span data-stu-id="093d6-491">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="093d6-492">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="093d6-492">Compile and run the app.</span></span> <span data-ttu-id="093d6-493">Bir `<ItemGroup>` uygulama projesinde sınıf kitaplığı'nızın derleme dosyasına başvurur ( *.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (bir derleme zamanı Başvurusu).</span><span class="sxs-lookup"><span data-stu-id="093d6-493">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="093d6-494">Notları HostingStartupApp'ın proje dosyasında daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="093d6-494">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\\bin\\Debug\\netcoreapp2.1\\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```

1. <span data-ttu-id="093d6-495">Dizin sayfası tarafından işlenen hizmet yapılandırması anahtar değerleri sınıf kitaplığının tarafından ayarlanan değerlerle eşleşen gözlemleyin `ServiceKeyInjection.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="093d6-495">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="093d6-496">**Mağaza tarafından dağıtılan bir çalışma zamanı derlemesindeki etkinleştirme**</span><span class="sxs-lookup"><span data-stu-id="093d6-496">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="093d6-497">*StartupDiagnostics* proje kullandığı [PowerShell](/powershell/scripting/powershell-scripting) değiştirmek için kendi *StartupDiagnostics.deps.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="093d6-497">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="093d6-498">PowerShell, Windows 7 SP1 ve Windows Server 2008 R2 SP1 ile başlayarak Windows üzerinde varsayılan olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="093d6-498">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="093d6-499">PowerShell diğer platformlarda edinmek için bkz. [Windows PowerShell'i yükleme](/powershell/scripting/setup/installing-powershell#powershell-core).</span><span class="sxs-lookup"><span data-stu-id="093d6-499">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span></span>
1. <span data-ttu-id="093d6-500">*Runtimesyürüme* klasöründe *Build. ps1* betiğini yürütün.</span><span class="sxs-lookup"><span data-stu-id="093d6-500">Execute the *build.ps1* script in the *RuntimeStore* folder.</span></span> <span data-ttu-id="093d6-501">Betik:</span><span class="sxs-lookup"><span data-stu-id="093d6-501">The script:</span></span>
   * <span data-ttu-id="093d6-502">`StartupDiagnostics` paketini *obj\packages* klasöründe oluşturur.</span><span class="sxs-lookup"><span data-stu-id="093d6-502">Generates the `StartupDiagnostics` package in the *obj\packages* folder.</span></span>
   * <span data-ttu-id="093d6-503">*Mağaza* klasöründeki `StartupDiagnostics` çalışma zamanı deposunu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="093d6-503">Generates the runtime store for `StartupDiagnostics` in the *store* folder.</span></span> <span data-ttu-id="093d6-504">Betikteki `dotnet store` komutu, Windows 'a dağıtılan bir barındırma başlatması için `win7-x64` [çalışma zamanı tanımlayıcısı 'nı (RID)](/dotnet/core/rid-catalog) kullanır.</span><span class="sxs-lookup"><span data-stu-id="093d6-504">The `dotnet store` command in the script uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog) for a hosting startup deployed to Windows.</span></span> <span data-ttu-id="093d6-505">Farklı bir çalışma zamanı için barındırma başlangıcını sağlarken, betiğin 37. satırındaki doğru RID 'yi yerine koyun.</span><span class="sxs-lookup"><span data-stu-id="093d6-505">When providing the hosting startup for a different runtime, substitute the correct RID on line 37 of the script.</span></span> <span data-ttu-id="093d6-506">`StartupDiagnostics` çalışma zamanı deposu daha sonra derlemenin tüketilebileceği makinede kullanıcının veya sistem çalışma zamanı deposuna taşınır.</span><span class="sxs-lookup"><span data-stu-id="093d6-506">The runtime store for `StartupDiagnostics` would later be moved to the user's or system's runtime store on the machine where the assembly will be consumed.</span></span> <span data-ttu-id="093d6-507">`StartupDiagnostics` derlemesinin Kullanıcı çalışma zamanı deposu yüklemesi konumu *. DotNet/Store/x64/netcoreapp 2.2/startupdiagnostics/1.0.0/LIB/netcoreapp 2.2/startupdiagnostics. dll*' dir.</span><span class="sxs-lookup"><span data-stu-id="093d6-507">The user runtime store install location for the `StartupDiagnostics` assembly is *.dotnet/store/x64/netcoreapp2.2/startupdiagnostics/1.0.0/lib/netcoreapp2.2/StartupDiagnostics.dll*.</span></span>
   * <span data-ttu-id="093d6-508">*Additionaldeps* klasöründeki `StartupDiagnostics` için `additionalDeps` üretir.</span><span class="sxs-lookup"><span data-stu-id="093d6-508">Generates the `additionalDeps` for `StartupDiagnostics` in the *additionalDeps* folder.</span></span> <span data-ttu-id="093d6-509">Ek bağımlılıklar daha sonra kullanıcının veya sistem ek bağımlılıklarına taşınır.</span><span class="sxs-lookup"><span data-stu-id="093d6-509">The additional dependencies would later be moved to the user's or system's additional dependencies.</span></span> <span data-ttu-id="093d6-510">Kullanıcı `StartupDiagnostics` ek bağımlılıklar yüklemesi konumu *. DotNet/x64/additionalDeps/startupdiagnostics/Shared/Microsoft. NETCore. App/2.2.0/StartupDiagnostics. Deps. JSON*olur.</span><span class="sxs-lookup"><span data-stu-id="093d6-510">The user `StartupDiagnostics` additional dependencies install location is *.dotnet/x64/additionalDeps/StartupDiagnostics/shared/Microsoft.NETCore.App/2.2.0/StartupDiagnostics.deps.json*.</span></span>
   * <span data-ttu-id="093d6-511">*Dağıtım* klasörüne *Deploy. ps1* dosyasını koyar.</span><span class="sxs-lookup"><span data-stu-id="093d6-511">Places the *deploy.ps1* file in the *deployment* folder.</span></span>
1. <span data-ttu-id="093d6-512">*Dağıtım* klasöründe *Deploy. ps1* betiğini çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="093d6-512">Run the *deploy.ps1* script in the *deployment* folder.</span></span> <span data-ttu-id="093d6-513">Betik şunu ekler:</span><span class="sxs-lookup"><span data-stu-id="093d6-513">The script appends:</span></span>
   * <span data-ttu-id="093d6-514">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkenine `StartupDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="093d6-514">`StartupDiagnostics` to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="093d6-515">Barındırma başlangıç bağımlılıkları yolu (Runtimessımında projenin *dağıtım* klasöründe) `DOTNET_ADDITIONAL_DEPS` ortam değişkenine.</span><span class="sxs-lookup"><span data-stu-id="093d6-515">The hosting startup dependencies path (in the RuntimeStore project's *deployment* folder) to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
   * <span data-ttu-id="093d6-516">Çalışma zamanı depolama yolu (Runtimes, projenin *dağıtım* klasöründe) `DOTNET_SHARED_STORE` ortam değişkenine.</span><span class="sxs-lookup"><span data-stu-id="093d6-516">The runtime store path (in the RuntimeStore project's *deployment* folder) to the `DOTNET_SHARED_STORE` environment variable.</span></span>
1. <span data-ttu-id="093d6-517">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="093d6-517">Run the sample app.</span></span>
1. <span data-ttu-id="093d6-518">İstek `/services` uygulamanın görmek için uç nokta Hizmetleri kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="093d6-518">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="093d6-519">İstek `/diag` tanılama bilgileri görmek için uç nokta.</span><span class="sxs-lookup"><span data-stu-id="093d6-519">Request the `/diag` endpoint to see the diagnostic information.</span></span>

::: moniker-end
