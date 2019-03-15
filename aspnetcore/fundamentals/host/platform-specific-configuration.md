---
title: ASP.NET Core barındırma başlangıç derlemeleri kullanma
author: guardrex
description: Uygulama Ihostingstartup kullanarak dış bütünleştirilmiş koddan bir ASP.NET Core uygulaması geliştirmek nasıl keşfedin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 03/10/2019
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 6111ae77369608e828eebf6229b5702630bc63f8
ms.sourcegitcommit: 34bf9fc6ea814c039401fca174642f0acb14be3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57841468"
---
# <a name="use-hosting-startup-assemblies-in-aspnet-core"></a><span data-ttu-id="e6b3a-103">ASP.NET Core barındırma başlangıç derlemeleri kullanma</span><span class="sxs-lookup"><span data-stu-id="e6b3a-103">Use hosting startup assemblies in ASP.NET Core</span></span>

<span data-ttu-id="e6b3a-104">Tarafından [Luke Latham](https://github.com/guardrex) ve [Pavel Krymets](https://github.com/pakrym)</span><span class="sxs-lookup"><span data-stu-id="e6b3a-104">By [Luke Latham](https://github.com/guardrex) and [Pavel Krymets](https://github.com/pakrym)</span></span>

<span data-ttu-id="e6b3a-105">Bir [Ihostingstartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (Başlangıç barındıran) uygulama geliştirmeleri dış bütünleştirilmiş koddan bir uygulamanın başlangıçta ekler.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="e6b3a-106">Örneğin, bir harici kitaplık ek yapılandırma sağlayıcıları ya da bir uygulama hizmetlerini barındıran bir başlangıç uygulaması kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-106">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span>

<span data-ttu-id="e6b3a-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e6b3a-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="e6b3a-108">HostingStartup özniteliği</span><span class="sxs-lookup"><span data-stu-id="e6b3a-108">HostingStartup attribute</span></span>

<span data-ttu-id="e6b3a-109">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) öznitelik, çalışma zamanında etkinleştirmek için bir barındırma başlangıç derleme varlığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-109">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="e6b3a-110">Giriş derleme veya içeren derlemenin `Startup` sınıfı için taranan otomatik olarak `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-110">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="e6b3a-111">Aranacak derlemelerin listesini `HostingStartup` öznitelikleri içinde yapılandırmasından çalışma zamanında yüklendiği [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="e6b3a-111">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span></span> <span data-ttu-id="e6b3a-112">Keşiften çıkarmak için derleme listesini alanından yüklenen [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="e6b3a-112">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span></span> <span data-ttu-id="e6b3a-113">Daha fazla bilgi için [Web ana bilgisayarı: Başlangıç derlemeleri barındırma](xref:fundamentals/host/web-host#hosting-startup-assemblies) ve [Web ana bilgisayarı: Başlangıç barındırma derlemeleri hariç](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="e6b3a-113">For more information, see [Web Host: Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) and [Web Host: Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span></span>

<span data-ttu-id="e6b3a-114">Aşağıdaki örnekte, barındırma başlangıç derlemenin ad alanıdır `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-114">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="e6b3a-115">Barındırma başlatma kodunu içeren sınıf `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-115">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="e6b3a-116">`HostingStartup` Öznitelik genellikle barındırma başlangıç derleme içinde bulunan `IHostingStartup` uygulama sınıf dosyası.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-116">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="e6b3a-117">Yüklenen barındırma başlangıç derlemeler keşfedin</span><span class="sxs-lookup"><span data-stu-id="e6b3a-117">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="e6b3a-118">Yüklenen barındırma başlangıç derlemeleri bulmak için günlük kaydını etkinleştirmek ve uygulama günlüklerini kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-118">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="e6b3a-119">Derlemeler yüklenirken oluşan hataları günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-119">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="e6b3a-120">Yüklenen barındırma başlangıç derlemeler hata ayıklama düzeyinde kaydedilir ve tüm hatalar kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-120">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="e6b3a-121">Başlangıç derlemeleri barındırma otomatik yüklemeyi devre dışı</span><span class="sxs-lookup"><span data-stu-id="e6b3a-121">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="e6b3a-122">Başlangıç derlemeleri barındırma otomatik yüklemeyi devre dışı bırakmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-122">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="e6b3a-123">Tüm barındırma başlangıç derlemeleri yüklenmesini önlemek için aşağıdakilerden birini ayarlayın `true` veya `1`:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-123">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>
  * <span data-ttu-id="e6b3a-124">[Barındırma başlangıç önlemek](xref:fundamentals/host/web-host#prevent-hosting-startup) ana bilgisayar yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-124">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
  * <span data-ttu-id="e6b3a-125">`ASPNETCORE_PREVENTHOSTINGSTARTUP` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-125">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>
* <span data-ttu-id="e6b3a-126">Belirli barındırma başlangıç derlemeleri yüklenmesini önlemek için aşağıdakilerden birini başlangıçta hariç tutmak için başlangıç derlemeleri barındırma noktalı virgülle ayrılmış bir dizeye ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-126">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>
  * <span data-ttu-id="e6b3a-127">[Başlangıç hariç derlemeleri barındırma](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) ana bilgisayar yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-127">[Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) host configuration setting.</span></span>
  * <span data-ttu-id="e6b3a-128">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-128">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

<span data-ttu-id="e6b3a-129">Hem ana bilgisayar yapılandırma ayarı ve ortam değişkenini ayarlarsanız, ana bilgisayar ayarını davranışını denetler.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-129">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="e6b3a-130">Ana bilgisayar ayarı veya ortam değişkenini kullanarak barındırma başlangıç derlemeleri devre dışı bırakma, derlemenin genel olarak devre dışı bırakır ve uygulama çeşitli özelliklerini devre dışı bırakabilir.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-130">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="e6b3a-131">Proje</span><span class="sxs-lookup"><span data-stu-id="e6b3a-131">Project</span></span>

<span data-ttu-id="e6b3a-132">Bir barındırma başlatma aşağıdaki proje türlerini birini oluşturun:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-132">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="e6b3a-133">Sınıf kitaplığı</span><span class="sxs-lookup"><span data-stu-id="e6b3a-133">Class library</span></span>](#class-library)
* [<span data-ttu-id="e6b3a-134">Konsol uygulaması giriş noktası olmayan</span><span class="sxs-lookup"><span data-stu-id="e6b3a-134">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="e6b3a-135">Sınıf kitaplığı</span><span class="sxs-lookup"><span data-stu-id="e6b3a-135">Class library</span></span>

<span data-ttu-id="e6b3a-136">Bir barındırma başlangıç geliştirme'de sınıf kitaplığının sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-136">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="e6b3a-137">Kitaplığı içeren bir `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-137">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="e6b3a-138">[Örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) bir Razor sayfaları uygulamasını içeren *HostingStartupApp*ve bir sınıf kitaplığı *HostingStartupLibrary*.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-138">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="e6b3a-139">Sınıf kitaplığı:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-139">The class library:</span></span>

* <span data-ttu-id="e6b3a-140">Bir barındırma başlangıç sınıfı içeren `ServiceKeyInjection`, uygulayan `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-140">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="e6b3a-141">`ServiceKeyInjection` bellek içi yapılandırma Sağlayıcısı'nı kullanarak uygulamanın yapılandırmasına hizmet dizeleri çifti ekler ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span><span class="sxs-lookup"><span data-stu-id="e6b3a-141">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span></span>
* <span data-ttu-id="e6b3a-142">İçeren bir `HostingStartup` barındırma startup şirketinizin ad alanını ve sınıf tanımlayan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-142">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="e6b3a-143">`ServiceKeyInjection` Sınıfın [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) yöntemi kullanan bir [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) geliştirmeleri için uygulama ekleme.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-143">The `ServiceKeyInjection` class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span>

<span data-ttu-id="e6b3a-144">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-144">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="e6b3a-145">Uygulama dizin sayfasına okur ve Sınıf Kitaplığı'nızın barındırma başlangıç derlemesi tarafından ayarlanmış iki anahtarı için yapılandırma değerlerini işler:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-145">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="e6b3a-146">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-146">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="e6b3a-147">[Örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) de ayrı bir barındırma başlatma sağlayan bir NuGet paketi proje içerir *HostingStartupPackage*.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-147">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="e6b3a-148">Paket, daha önce açıklanan Sınıf Kitaplığı'nın aynı özelliklere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-148">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="e6b3a-149">Paketi:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-149">The package:</span></span>

* <span data-ttu-id="e6b3a-150">Bir barındırma başlangıç sınıfı içeren `ServiceKeyInjection`, uygulayan `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-150">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="e6b3a-151">`ServiceKeyInjection` Hizmet dizeleri çifti uygulamanın yapılandırmasına ekler.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-151">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="e6b3a-152">İçeren bir `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-152">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="e6b3a-153">*HostingStartupPackage/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-153">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="e6b3a-154">Uygulama dizin sayfasına okur ve paketin barındırma başlangıç derlemesi tarafından ayarlanmış iki anahtarı için yapılandırma değerlerini işler:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-154">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="e6b3a-155">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-155">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="e6b3a-156">Konsol uygulaması giriş noktası olmayan</span><span class="sxs-lookup"><span data-stu-id="e6b3a-156">Console app without an entry point</span></span>

<span data-ttu-id="e6b3a-157">*Bu yaklaşım, yalnızca .NET Framework .NET Core uygulamaları için kullanılabilir.*</span><span class="sxs-lookup"><span data-stu-id="e6b3a-157">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="e6b3a-158">Bir derleme zamanı başvurusu için etkinleştirme gerektirmeyen bir dinamik barındırma başlangıç geliştirmesi de içeren giriş noktası olmayan bir konsol uygulaması sağlanan bir `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-158">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point that contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="e6b3a-159">Konsol uygulaması yayımlama, çalışma zamanı Mağazası'ndan kullanılabilen bir barındırma başlangıç bütünleştirilmiş kod üretir.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-159">Publishing the console app produces a hosting startup assembly that can be consumed from the runtime store.</span></span>

<span data-ttu-id="e6b3a-160">Bir konsol uygulaması giriş noktası olmayan, çünkü bu işlemde kullanılır:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-160">A console app without an entry point is used in this process because:</span></span>

* <span data-ttu-id="e6b3a-161">Bağımlılıkları dosya barındırma başlangıç derlemedeki barındırma için başlangıç kullanmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-161">A dependencies file is required to consume the hosting startup in the hosting startup assembly.</span></span> <span data-ttu-id="e6b3a-162">Kitaplık değil bir uygulama yayımlama tarafından üretilen bir çalıştırılabilir uygulama varlık bağımlılıkları dosyasıdır.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-162">A dependencies file is a runnable app asset that's produced by publishing an app, not a library.</span></span>
* <span data-ttu-id="e6b3a-163">Bir kitaplık doğrudan eklenemez [çalışma zamanı Paket Deposu](/dotnet/core/deploying/runtime-store), paylaşılan çalışma zamanını hedefleyen çalıştırılabilir bir proje gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-163">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>

<span data-ttu-id="e6b3a-164">Dinamik barındırma başlangıç oluşturulmasını içinde:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-164">In the creation of a dynamic hosting startup:</span></span>

* <span data-ttu-id="e6b3a-165">Bir giriş noktası olmadan bir barındırma başlangıç derleme konsol uygulamasından oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-165">A hosting startup assembly is created from the console app without an entry point that:</span></span>
  * <span data-ttu-id="e6b3a-166">İçeren bir sınıfı içeren `IHostingStartup` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-166">Includes a class that contains the `IHostingStartup` implementation.</span></span>
  * <span data-ttu-id="e6b3a-167">İçeren bir [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) tanımlamak için öznitelik `IHostingStartup` uygulama sınıfı.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-167">Includes a [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute to identify the `IHostingStartup` implementation class.</span></span>
* <span data-ttu-id="e6b3a-168">Konsol uygulaması barındırma startup şirketinizin bağımlılıkları almak için yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-168">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="e6b3a-169">Kullanılmayan bağımlılıkları bağımlılıkları dosyasından atılır konsol uygulaması yayımlama bir sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-169">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
* <span data-ttu-id="e6b3a-170">Bağımlılıkları dosyası, çalışma zamanı barındırma başlangıç derleme konumunu ayarlamak için değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-170">The dependencies file is modified to set the runtime location of the hosting startup assembly.</span></span>
* <span data-ttu-id="e6b3a-171">Barındırma başlangıç derleme ve bağımlılıkları dosyası çalışma zamanı paketi deposuna yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-171">The hosting startup assembly and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="e6b3a-172">Barındırma başlangıç derleme ve bağımlılıkları dosyasını bulmak için bir ortam değişkenlerini çift içinde listelendikleri.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-172">To discover the hosting startup assembly and its dependencies file, they're listed in a pair of environment variables.</span></span>

<span data-ttu-id="e6b3a-173">Konsol uygulama başvuruları [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) paket:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-173">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

<span data-ttu-id="e6b3a-174">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) özniteliğini tanımlayan bir sınıf uygulaması `IHostingStartup` yüklenirken ve oluşturulurken yürütme [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="e6b3a-174">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="e6b3a-175">Aşağıdaki örnekte, ad alanı `StartupEnhancement`, ve sınıf `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-175">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="e6b3a-176">Arabirimini uygulayan bir sınıf `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-176">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="e6b3a-177">Sınıfın [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) yöntemi kullanan bir [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) geliştirmeleri için uygulama ekleme.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-177">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="e6b3a-178">`IHostingStartup.Configure` barındırma başlangıç derleme önce çalışma zamanı tarafından çağrılır `Startup.Configure` kullanıcı kodunda barındırma başlangıç derlemesi tarafından sağlanan herhangi bir yapılandırma üzerine yazmak kullanıcı kodu sağlar.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-178">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="e6b3a-179">Oluşturma sırasında bir `IHostingStartup` proje bağımlılıkları dosyasının (*\*. deps.json*) ayarlar `runtime` derlemeye konumunu *bin* klasörü:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-179">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="e6b3a-180">Dosyanın yalnızca bir parçası olarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-180">Only part of the file is shown.</span></span> <span data-ttu-id="e6b3a-181">Derleme adı örnekte `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-181">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="configuration-provided-by-the-hosting-startup"></a><span data-ttu-id="e6b3a-182">Barındırma için başlangıç tarafından sağlanan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="e6b3a-182">Configuration provided by the hosting startup</span></span>

<span data-ttu-id="e6b3a-183">İşleme barındırma startup şirketinizin yapılandırma öncelikli için istediğinize bağlı olarak, yapılandırma veya öncelikli için uygulamanın yapılandırma için iki yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-183">There are two approaches to handling configuration depending on whether you want the hosting startup's configuration to take precedence or the app's configuration to take precedence:</span></span>

1. <span data-ttu-id="e6b3a-184">Yapılandırma kullanarak uygulama sağlamak <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> uygulamanın sonra yapılandırmayı <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> temsilciler yürütün.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-184">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> to load the configuration after the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="e6b3a-185">Barındırma başlangıç yapılandırmasını, bu yaklaşımı kullanarak uygulamanın yapılandırma üzerinde önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-185">Hosting startup configuration takes priority over the app's configuration using this approach.</span></span>
1. <span data-ttu-id="e6b3a-186">Yapılandırma kullanarak uygulama sağlamak <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> önce uygulamanın yapılandırmasını <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> temsilciler yürütün.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-186">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> to load the configuration before the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="e6b3a-187">Uygulamanın yapılandırma değerleri, bu yaklaşımı kullanarak barındırma için başlangıç tarafından sağlanan üzerine öncelik alır.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-187">The app's configuration values take priority over those provided by the hosting startup using this approach.</span></span>

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
                    "From IHostingStartup: Higher priority than the app's configuration."},
            };

            config.AddInMemoryCollection(dict);
        });

        dict = new Dictionary<string, string>
        {
            {"ConfigurationKey2", 
                "From IHostingStartup: Lower priority than the app's configuration."},
        };

        var builtConfig = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        builder.UseConfiguration(builtConfig);
    }
}
```

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="e6b3a-188">Barındırma başlangıç derlemeyi belirtin</span><span class="sxs-lookup"><span data-stu-id="e6b3a-188">Specify the hosting startup assembly</span></span>

<span data-ttu-id="e6b3a-189">Bir sınıf kitaplığı - veya konsol uygulaması sağlanan-başlangıç barındırma için barındırma başlangıç derlemenin adını belirtin. `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-189">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="e6b3a-190">Ortam değişkenini derlemelerin noktalı virgülle ayrılmış bir listedir.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-190">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="e6b3a-191">Yalnızca barındırma başlangıç derlemeler için taranan `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-191">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="e6b3a-192">Örnek uygulama için *HostingStartupApp*, daha önce açıklanan barındırma startup'lar bulmak için ortam değişkeni şu değere ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-192">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="e6b3a-193">Bir barındırma başlangıç derleme kullanılarak da ayarlanabilir [barındırma başlangıç derlemeleri](xref:fundamentals/host/web-host#hosting-startup-assemblies) ana bilgisayar yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-193">A hosting startup assembly can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="e6b3a-194">Ne zaman birden çok barındırma başlangıç çeviren varsa, bunların [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) yöntemleri derlemeleri listelenen sırayla yürütülür.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-194">When multiple hosting startup assembles are present, their [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="e6b3a-195">Etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="e6b3a-195">Activation</span></span>

<span data-ttu-id="e6b3a-196">Başlangıç etkinleştirme barındırmak için Seçenekler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-196">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="e6b3a-197">[Çalışma zamanı deposu](#runtime-store) &ndash; etkinleştirme, etkinleştirme için bir derleme zamanı başvurusu gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-197">[Runtime store](#runtime-store) &ndash; Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="e6b3a-198">Örnek uygulama barındırma başlangıç derleme ve bağımlılıkları dosyalar bir klasöre yerleştirir *dağıtım*multimachine ortamında barındırma başlangıç dağıtımını kolaylaştırmak için.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-198">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="e6b3a-199">*Dağıtım* klasörü oluşturur veya değiştirir barındırma için başlangıç etkinleştirmek için dağıtım sistemi ortam değişkenlerini bir PowerShell betiğini de içerir.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-199">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="e6b3a-200">Etkinleştirme için gerekli derleme zamanı başvurusu</span><span class="sxs-lookup"><span data-stu-id="e6b3a-200">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="e6b3a-201">NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="e6b3a-201">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="e6b3a-202">Proje bin klasörü</span><span class="sxs-lookup"><span data-stu-id="e6b3a-202">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="e6b3a-203">Çalışma zamanı deposu</span><span class="sxs-lookup"><span data-stu-id="e6b3a-203">Runtime store</span></span>

<span data-ttu-id="e6b3a-204">Barındırma başlangıç uygulaması yerleştirilir [çalışma zamanı deposu](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="e6b3a-204">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="e6b3a-205">Gelişmiş uygulama tarafından bir derleme zamanı başvurusu derleme için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-205">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="e6b3a-206">Barındırma başlangıç oluşturulduktan sonra bir çalışma zamanı deposu bildirim proje dosyası kullanılarak oluşturulur ve [dotnet deposu](/dotnet/core/tools/dotnet-store) komutu.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-206">After the hosting startup is built, a runtime store is generated using the manifest project file and the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```console
dotnet store --manifest {MANIFEST FILE} --runtime {RUNTIME IDENTIFIER} --output {OUTPUT LOCATION} --skip-optimization
```

<span data-ttu-id="e6b3a-207">Örnek uygulamada (*RuntimeStore* Proje) şu komut kullanılır:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-207">In the sample app (*RuntimeStore* project) the following command is used:</span></span>

``` console
dotnet store --manifest store.manifest.csproj --runtime win7-x64 --output ./deployment/store --skip-optimization
```

<span data-ttu-id="e6b3a-208">Çalışma zamanı deposu bulmak çalışma zamanı için çalışma zamanı deponun konumunu eklenen `DOTNET_SHARED_STORE` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-208">For the runtime to discover the runtime store, the runtime store's location is added to the `DOTNET_SHARED_STORE` environment variable.</span></span>

<span data-ttu-id="e6b3a-209">**Değiştirme ve barındırma startup şirketinizin bağımlılıkları dosyasının yerleştirin**</span><span class="sxs-lookup"><span data-stu-id="e6b3a-209">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="e6b3a-210">Geliştirme için bir paket başvurusu olmadan geliştirme etkinleştirmek için çalışma zamanı ile ek bağımlılıklar belirtin `additionalDeps`.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-210">To activate the enhancement without a package reference to the enhancement, specify additional dependencies to the runtime with `additionalDeps`.</span></span> <span data-ttu-id="e6b3a-211">`additionalDeps` sağlar:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-211">`additionalDeps` allows you to:</span></span>

* <span data-ttu-id="e6b3a-212">Bir dizi ek sağlayarak uygulamanın kitaplığı grafı genişletmek  *\*. deps.json* uygulamanın ile kendi birleştirilecek dosyaları  *\*. deps.json* başlangıç dosyası.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-212">Extend the app's library graph by providing a set of additional *\*.deps.json* files to merge with the app's own *\*.deps.json* file on startup.</span></span>
* <span data-ttu-id="e6b3a-213">Barındırma başlangıç derlemesine bulunabilir ve yüklenebilir olun.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-213">Make the hosting startup assembly discoverable and loadable.</span></span>

<span data-ttu-id="e6b3a-214">Ek Bağımlılıklar dosyası oluşturmak için önerilen yaklaşımdır:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-214">The recommended approach for generating the additional dependencies file is to:</span></span>

 1. <span data-ttu-id="e6b3a-215">Yürütme `dotnet publish` önceki bölümde başvurulan çalışma zamanı deposu bildirim dosyası üzerinde.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-215">Execute `dotnet publish` on the runtime store manifest file referenced in the previous section.</span></span>
 1. <span data-ttu-id="e6b3a-216">Kitaplıklarından bildirim referansını kaldırın ve `runtime` ortaya çıkan bölüm  *\*deps.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-216">Remove the manifest reference from libraries and the `runtime` section of the resulting *\*deps.json* file.</span></span>

<span data-ttu-id="e6b3a-217">Örnek projesinde `store.manifest/1.0.0` özelliği kaldırılır `targets` ve `libraries` bölümü:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-217">In the example project, the `store.manifest/1.0.0` property is removed from the `targets` and `libraries` section:</span></span>

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

<span data-ttu-id="e6b3a-218">Bir yerde  *\*. deps.json* dosyasına şu konumda:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-218">Place the *\*.deps.json* file into the following location:</span></span>

```
{ADDITIONAL DEPENDENCIES PATH}/shared/{SHARED FRAMEWORK NAME}/{SHARED FRAMEWORK VERSION}/{ENHANCEMENT ASSEMBLY NAME}.deps.json
```

* <span data-ttu-id="e6b3a-219">`{ADDITIONAL DEPENDENCIES PATH}` &ndash; Eklenen konumu `DOTNET_ADDITIONAL_DEPS` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-219">`{ADDITIONAL DEPENDENCIES PATH}` &ndash; Location added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
* <span data-ttu-id="e6b3a-220">`{SHARED FRAMEWORK NAME}` &ndash; Bu ek bağımlılıklar dosyası için gerekli framework paylaşılan.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-220">`{SHARED FRAMEWORK NAME}` &ndash; Shared framework required for this additional dependencies file.</span></span>
* <span data-ttu-id="e6b3a-221">`{SHARED FRAMEWORK VERSION}` &ndash; Minimum paylaşılan framework sürümü.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-221">`{SHARED FRAMEWORK VERSION}` &ndash; Minimum shared framework version.</span></span>
* <span data-ttu-id="e6b3a-222">`{ENHANCEMENT ASSEMBLY NAME}` &ndash; Geliştirme'nın bütünleştirilmiş kod adı.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-222">`{ENHANCEMENT ASSEMBLY NAME}` &ndash; The enhancement's assembly name.</span></span>

<span data-ttu-id="e6b3a-223">Örnek uygulamada (*RuntimeStore* Proje), ek bağımlılıklar dosyası şu konuma yerleştirilir:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-223">In the sample app (*RuntimeStore* project), the additional dependencies file is placed into the following location:</span></span>

```
additionalDeps/shared/Microsoft.AspNetCore.App/2.1.0/StartupDiagnostics.deps.json
```

<span data-ttu-id="e6b3a-224">Ek Bağımlılıklar dosya konumunu eklenir çalışma zamanı depolama konumu bulmak çalışma zamanı için `DOTNET_ADDITIONAL_DEPS` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-224">For runtime to discover the runtime store location, the additional dependencies file location is added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="e6b3a-225">Örnek uygulamada (*RuntimeStore* Proje), çalışma zamanı deposu oluşturma ve dosya kullanarak gerçekleştirilir ek bağımlılıklar oluşturma bir [PowerShell](/powershell/scripting/powershell-scripting) betiği.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-225">In the sample app (*RuntimeStore* project), building the runtime store and generating the additional dependencies file is accomplished using a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span>

<span data-ttu-id="e6b3a-226">Çeşitli işletim sistemleri için ortam değişkenlerini ayarlama örnekleri için bkz: [birden fazla ortam kullanayım](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="e6b3a-226">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="e6b3a-227">**Dağıtım**</span><span class="sxs-lookup"><span data-stu-id="e6b3a-227">**Deployment**</span></span>

<span data-ttu-id="e6b3a-228">Bir barındırma başlatma multimachine ortamında dağıtımını kolaylaştırmak için örnek uygulamayı oluşturur. bir *dağıtım* içeren yayımlanan çıkış klasöründe:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-228">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="e6b3a-229">Barındırma başlangıç çalışma zamanı deposu.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-229">The hosting startup runtime store.</span></span>
* <span data-ttu-id="e6b3a-230">Barındırma başlangıç bağımlılıkları dosyası.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-230">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="e6b3a-231">Bir PowerShell Betiği oluşturur veya değiştirir `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, ve `DOTNET_ADDITIONAL_DEPS` barındırma başlangıç etkinleştirmeyi desteklemek için.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-231">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="e6b3a-232">Komut dosyası dağıtım sistemde bir yönetici PowerShell komut isteminden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-232">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="e6b3a-233">NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="e6b3a-233">NuGet package</span></span>

<span data-ttu-id="e6b3a-234">Bir NuGet paketi barındırma bir başlangıç geliştirmesi sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-234">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="e6b3a-235">Paketin bir `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-235">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="e6b3a-236">Paket tarafından sağlanan barındırma başlangıç türleri, aşağıdaki yaklaşımlardan birini kullanarak uygulama için kullanıma sunulur:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-236">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="e6b3a-237">Gelişmiş uygulamanın proje dosyası, barındırma başlangıç için bir paket başvurusu uygulamanın proje dosyası (bir derleme zamanı Başvurusu) sağlar.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-237">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="e6b3a-238">Yerinde derleme zamanı başvurusu ile barındırma başlangıç derleme ve tüm bağımlılıklarını uygulamanın bağımlılık dosyasına dahil edilir (*\*. deps.json*).</span><span class="sxs-lookup"><span data-stu-id="e6b3a-238">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="e6b3a-239">Bu yaklaşım, yayımlanan bir barındırma başlangıç derleme paketi uygulandığı [nuget.org](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="e6b3a-239">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="e6b3a-240">Barındırma startup şirketinizin bağımlılıkları dosyasının açıklandığı gibi gelişmiş uygulama için kullanılabilir hale getirileceğini [çalışma zamanı deposu](#runtime-store) bölümü (olmadan, bir derleme zamanı Başvurusu).</span><span class="sxs-lookup"><span data-stu-id="e6b3a-240">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="e6b3a-241">NuGet paketlerini ve çalışma zamanı mağazası hakkında daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-241">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="e6b3a-242">Platformlar arası araçlarla NuGet paketi oluşturma</span><span class="sxs-lookup"><span data-stu-id="e6b3a-242">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="e6b3a-243">Paket yayımlama</span><span class="sxs-lookup"><span data-stu-id="e6b3a-243">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="e6b3a-244">Çalışma zamanı paket deposu</span><span class="sxs-lookup"><span data-stu-id="e6b3a-244">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="e6b3a-245">Proje bin klasörü</span><span class="sxs-lookup"><span data-stu-id="e6b3a-245">Project bin folder</span></span>

<span data-ttu-id="e6b3a-246">Bir barındırma başlangıç geliştirmesi tarafından sağlanan bir *bin*-Gelişmiş Uygulama derlemesinde dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-246">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="e6b3a-247">Derleme tarafından sağlanan barındırma başlangıç türleri, aşağıdaki yaklaşımlardan birini kullanarak uygulama için kullanıma sunulur:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-247">The hosting startup types provided by the assembly are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="e6b3a-248">Gelişmiş uygulamanın proje dosyası, barındırma başlangıç (bir derleme zamanı başvuru) bir bütünleştirilmiş kod başvurusu yapar.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-248">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="e6b3a-249">Yerinde derleme zamanı başvurusu ile barındırma başlangıç derleme ve tüm bağımlılıklarını uygulamanın bağımlılık dosyasına dahil edilir (*\*. deps.json*).</span><span class="sxs-lookup"><span data-stu-id="e6b3a-249">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="e6b3a-250">Dağıtım senaryosu derlenmiş barındırma başlangıç kitaplığın derleme (DLL dosyası) taşımak için alıcı projeye ya da bir konuma kullanan proje tarafından erişilebilir çağırır ve barındırmak için bir derleme zamanı başvurusu yapılan bu yaklaşım uygulanır Startup şirketinizin derleme.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-250">This approach applies when the deployment scenario calls for moving the compiled hosting startup library's assembly (DLL file) to the consuming project or to a location accessible by the consuming project and a compile-time reference is made to the hosting startup's assembly.</span></span>
* <span data-ttu-id="e6b3a-251">Barındırma startup şirketinizin bağımlılıkları dosyasının açıklandığı gibi gelişmiş uygulama için kullanılabilir hale getirileceğini [çalışma zamanı deposu](#runtime-store) bölümü (olmadan, bir derleme zamanı Başvurusu).</span><span class="sxs-lookup"><span data-stu-id="e6b3a-251">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

## <a name="sample-code"></a><span data-ttu-id="e6b3a-252">Örnek kod</span><span class="sxs-lookup"><span data-stu-id="e6b3a-252">Sample code</span></span>

<span data-ttu-id="e6b3a-253">[Örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)) barındırma başlangıç uygulama senaryolarını gösterir:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-253">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="e6b3a-254">İki barındırma başlangıç derlemeleri (sınıf kitaplıkları), bellek içi yapılandırma anahtar-değer çiftleri her bir çifti ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-254">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="e6b3a-255">NuGet paketini (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="e6b3a-255">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="e6b3a-256">Sınıf kitaplığı (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="e6b3a-256">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="e6b3a-257">Bir barındırma başlatma deposu dağıtılan bir çalışma zamanı derleme etkinleştirilir (*StartupDiagnostics*).</span><span class="sxs-lookup"><span data-stu-id="e6b3a-257">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="e6b3a-258">Derleme, uygulamaya tanılama bilgileri sağlayan başlangıçta iki middlewares ekler:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-258">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="e6b3a-259">Kaydedilen Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="e6b3a-259">Registered services</span></span>
  * <span data-ttu-id="e6b3a-260">Adres (Düzen, konak, temel yolu, yol, sorgu dizesi)</span><span class="sxs-lookup"><span data-stu-id="e6b3a-260">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="e6b3a-261">Bağlantı (uzak IP, uzak bağlantı noktasını, yerel IP yerel bağlantı noktası, istemci sertifikası)</span><span class="sxs-lookup"><span data-stu-id="e6b3a-261">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="e6b3a-262">İstek üst bilgileri</span><span class="sxs-lookup"><span data-stu-id="e6b3a-262">Request headers</span></span>
  * <span data-ttu-id="e6b3a-263">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="e6b3a-263">Environment variables</span></span>

<span data-ttu-id="e6b3a-264">Örneği çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-264">To run the sample:</span></span>

<span data-ttu-id="e6b3a-265">**NuGet paketinden etkinleştirme**</span><span class="sxs-lookup"><span data-stu-id="e6b3a-265">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="e6b3a-266">Derleme *HostingStartupPackage* ile paket [dotnet paketi](/dotnet/core/tools/dotnet-pack) komutu.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-266">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="e6b3a-267">Paketin derleme adını eklemek *HostingStartupPackage* için `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-267">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="e6b3a-268">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-268">Compile and run the app.</span></span> <span data-ttu-id="e6b3a-269">Geliştirilmiş bir uygulamada (bir derleme zamanı Başvurusu) bir paket başvurusu yok.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-269">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="e6b3a-270">A `<PropertyGroup>` uygulama projesinde paket projenin çıkış dosyasını belirtir. (*.. / HostingStartupPackage/bin/Debug*) bir paket olarak.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-270">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="e6b3a-271">Bu paketi yüklenmeden kaydedilip paketini kullanmanıza olanak verir [nuget.org](https://www.nuget.org/). Notları HostingStartupApp'ın proje dosyasında daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-271">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```
1. <span data-ttu-id="e6b3a-272">Dizin sayfası tarafından işlenen hizmet yapılandırması anahtar değerleri paketin tarafından ayarlanan değerlerle eşleşen gözlemleyin `ServiceKeyInjection.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-272">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="e6b3a-273">Değişiklikler yaparsanız *HostingStartupPackage* proje ve yeniden derleyin, emin olmak için yerel NuGet paketi önbellekler temizlenir *HostingStartupApp* güncelleştirilmiş paket bir eski alır Paket yerel önbellekten.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-273">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="e6b3a-274">Yerel NuGet önbellekleri temizlemek için aşağıdakileri yürütün [dotnet nuget Yereller](/dotnet/core/tools/dotnet-nuget-locals) komutu:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-274">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```console
dotnet nuget locals all --clear
```

<span data-ttu-id="e6b3a-275">**Bir sınıf kitaplığından etkinleştirme**</span><span class="sxs-lookup"><span data-stu-id="e6b3a-275">**Activation from a class library**</span></span>

1. <span data-ttu-id="e6b3a-276">Derleme *HostingStartupLibrary* sınıf kitaplığı ile [dotnet derleme](/dotnet/core/tools/dotnet-build) komutu.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-276">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="e6b3a-277">Sınıf Kitaplığı'nızın derleme adını eklemek *HostingStartupLibrary* için `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-277">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="e6b3a-278">*Depo*-kopyalayarak Sınıf Kitaplığı'nızın derleme uygulamalarıyla *HostingStartupLibrary.dll* Sınıf Kitaplığı'nızın dosyasından çıktıyı uygulamanın derlenmiş *bin/Debug* klasör.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-278">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="e6b3a-279">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-279">Compile and run the app.</span></span> <span data-ttu-id="e6b3a-280">Bir `<ItemGroup>` uygulama projesinde sınıf kitaplığı'nızın derleme dosyasına başvurur (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (bir derleme zamanı Başvurusu).</span><span class="sxs-lookup"><span data-stu-id="e6b3a-280">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="e6b3a-281">Notları HostingStartupApp'ın proje dosyasında daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-281">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```
1. <span data-ttu-id="e6b3a-282">Dizin sayfası tarafından işlenen hizmet yapılandırması anahtar değerleri sınıf kitaplığının tarafından ayarlanan değerlerle eşleşen gözlemleyin `ServiceKeyInjection.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-282">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="e6b3a-283">**Mağaza tarafından dağıtılan bir çalışma zamanı derlemesindeki etkinleştirme**</span><span class="sxs-lookup"><span data-stu-id="e6b3a-283">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="e6b3a-284">*StartupDiagnostics* proje kullandığı [PowerShell](/powershell/scripting/powershell-scripting) değiştirmek için kendi *StartupDiagnostics.deps.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-284">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="e6b3a-285">PowerShell, Windows 7 SP1 ve Windows Server 2008 R2 SP1 ile başlayarak Windows üzerinde varsayılan olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-285">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="e6b3a-286">PowerShell diğer platformlarda edinmek için bkz. [Windows PowerShell'i yükleme](/powershell/scripting/setup/installing-powershell#powershell-core).</span><span class="sxs-lookup"><span data-stu-id="e6b3a-286">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span></span>
1. <span data-ttu-id="e6b3a-287">Derleme *StartupDiagnostics* proje.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-287">Build the *StartupDiagnostics* project.</span></span> <span data-ttu-id="e6b3a-288">Sonra projeyi oluşturulduğuna göre bir yapı hedefi proje dosyasında otomatik olarak:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-288">After the project is built, a build target in the project file automatically:</span></span>
   * <span data-ttu-id="e6b3a-289">Değiştirmek için PowerShell betiğini tetikler *StartupDiagnostics.deps.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-289">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="e6b3a-290">Taşır *StartupDiagnostics.deps.json* kullanıcı profilinin dosyasına *additionalDeps* klasör.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-290">Moves the *StartupDiagnostics.deps.json* file to the user profile's *additionalDeps* folder.</span></span>
1. <span data-ttu-id="e6b3a-291">Yürütme `dotnet store` derleme depolamak için barındırma startup şirketinizin dizinindeki bir command prompt ve bağımlılıklarını kullanıcı profilinin çalışma zamanı deposundaki komutu:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-291">Execute the `dotnet store` command at a command prompt in the hosting startup's directory to store the assembly and its dependencies in the user profile's runtime store:</span></span>

   ```console
   dotnet store --manifest StartupDiagnostics.csproj --runtime <RID>
   ```
   <span data-ttu-id="e6b3a-292">Windows için komut `win7-x64` [çalışma zamanı tanımlayıcı (RID)](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="e6b3a-292">For Windows, the command uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="e6b3a-293">Barındırma için başlangıç için farklı bir çalışma zamanı sağlanırken doğru RID değiştirin.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-293">When providing the hosting startup for a different runtime, substitute the correct RID.</span></span>
1. <span data-ttu-id="e6b3a-294">Ortam değişkenlerini ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="e6b3a-294">Set the environment variables:</span></span>
   * <span data-ttu-id="e6b3a-295">Derleme adını eklemek *StartupDiagnostics* için `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-295">Add the assembly name of *StartupDiagnostics* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="e6b3a-296">Windows üzerinde ayarlanmış `DOTNET_ADDITIONAL_DEPS` ortam değişkenine `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-296">On Windows, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`.</span></span> <span data-ttu-id="e6b3a-297">/ Linux, macOS üzerinde ayarlanmış `DOTNET_ADDITIONAL_DEPS` ortam değişkenine `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`burada `<USER>` barındırma başlangıç içeren kullanıcı profili.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-297">On macOS/Linux, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, where `<USER>` is the user profile that contains the hosting startup.</span></span>
1. <span data-ttu-id="e6b3a-298">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-298">Run the sample app.</span></span>
1. <span data-ttu-id="e6b3a-299">İstek `/services` uygulamanın görmek için uç nokta Hizmetleri kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-299">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="e6b3a-300">İstek `/diag` tanılama bilgileri görmek için uç nokta.</span><span class="sxs-lookup"><span data-stu-id="e6b3a-300">Request the `/diag` endpoint to see the diagnostic information.</span></span>
