---
title: ASP.NET core'da Ihostingstartup ile dış bütünleştirilmiş koddan uygulama geliştirin
author: guardrex
description: Uygulama Ihostingstartup kullanarak dış bütünleştirilmiş koddan bir ASP.NET Core uygulaması geliştirmek nasıl keşfedin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 2eddfa03b28564fcca7cc098e353b05e23b7c6f6
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336246"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a><span data-ttu-id="2cb8c-103">ASP.NET core'da Ihostingstartup ile dış bütünleştirilmiş koddan uygulama geliştirin</span><span class="sxs-lookup"><span data-stu-id="2cb8c-103">Enhance an app from an external assembly in ASP.NET Core with IHostingStartup</span></span>

<span data-ttu-id="2cb8c-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2cb8c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2cb8c-105">Bir [Ihostingstartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (Başlangıç barındıran) uygulama geliştirmeleri dış bütünleştirilmiş koddan bir uygulamanın başlangıçta ekler.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="2cb8c-106">Örneğin, bir harici kitaplık ek yapılandırma sağlayıcıları ya da bir uygulama hizmetlerini barındıran bir başlangıç uygulaması kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-106">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="2cb8c-107">`IHostingStartup` *ASP.NET Core 2.0 veya sonraki sürümlerinde kullanılabilir.*</span><span class="sxs-lookup"><span data-stu-id="2cb8c-107">`IHostingStartup` *is available in ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="2cb8c-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2cb8c-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="2cb8c-109">HostingStartup özniteliği</span><span class="sxs-lookup"><span data-stu-id="2cb8c-109">HostingStartup attribute</span></span>

<span data-ttu-id="2cb8c-110">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) öznitelik, çalışma zamanında etkinleştirmek için bir barındırma başlangıç derleme varlığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-110">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="2cb8c-111">Giriş derleme veya içeren derlemenin `Startup` sınıfı için taranan otomatik olarak `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-111">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="2cb8c-112">Aranacak derlemelerin listesini `HostingStartup` öznitelikleri içinde yapılandırmasından çalışma zamanında yüklendiği [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="2cb8c-112">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span></span> <span data-ttu-id="2cb8c-113">Keşiften çıkarmak için derleme listesini alanından yüklenen [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="2cb8c-113">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span></span> <span data-ttu-id="2cb8c-114">Daha fazla bilgi için bkz. [Web ana bilgisayarı: barındırma başlangıç derlemeleri](xref:fundamentals/host/web-host#hosting-startup-assemblies) ve [Web ana bilgisayarı: Başlangıç hariç derlemeleri barındırma](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="2cb8c-114">For more information, see [Web Host: Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) and [Web Host: Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span></span>

<span data-ttu-id="2cb8c-115">Aşağıdaki örnekte, barındırma başlangıç derlemenin ad alanıdır `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-115">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="2cb8c-116">Barındırma başlatma kodunu içeren sınıf `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-116">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="2cb8c-117">`HostingStartup` Öznitelik genellikle barındırma başlangıç derleme içinde bulunan `IHostingStartup` uygulama sınıf dosyası.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-117">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="2cb8c-118">Yüklenen barındırma başlangıç derlemeler keşfedin</span><span class="sxs-lookup"><span data-stu-id="2cb8c-118">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="2cb8c-119">Yüklenen barındırma başlangıç derlemeleri bulmak için günlük kaydını etkinleştirmek ve uygulama günlüklerini kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-119">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="2cb8c-120">Derlemeler yüklenirken oluşan hataları günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-120">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="2cb8c-121">Yüklenen barındırma başlangıç derlemeler hata ayıklama düzeyinde kaydedilir ve tüm hatalar kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-121">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="2cb8c-122">Başlangıç derlemeleri barındırma otomatik yüklemeyi devre dışı</span><span class="sxs-lookup"><span data-stu-id="2cb8c-122">Disable automatic loading of hosting startup assemblies</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2cb8c-123">Başlangıç derlemeleri barındırma otomatik yüklemeyi devre dışı bırakmak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-123">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="2cb8c-124">Tüm barındırma başlangıç derlemeleri yüklenmesini önlemek için aşağıdakilerden birini ayarlayın `true` veya `1`:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-124">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>
  * <span data-ttu-id="2cb8c-125">[Barındırma başlangıç önlemek](xref:fundamentals/host/web-host#prevent-hosting-startup) ana bilgisayar yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-125">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
  * <span data-ttu-id="2cb8c-126">`ASPNETCORE_PREVENTHOSTINGSTARTUP` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-126">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>
* <span data-ttu-id="2cb8c-127">Belirli barındırma başlangıç derlemeleri yüklenmesini önlemek için aşağıdakilerden birini başlangıçta hariç tutmak için başlangıç derlemeleri barındırma noktalı virgülle ayrılmış bir dizeye ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-127">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>
  * <span data-ttu-id="2cb8c-128">[Başlangıç hariç derlemeleri barındırma](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) ana bilgisayar yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-128">[Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) host configuration setting.</span></span>
  * <span data-ttu-id="2cb8c-129">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-129">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="2cb8c-130">Başlangıç derlemeleri barındırma otomatik yüklemeyi devre dışı bırakmak için aşağıdakilerden birini ayarlayın `true` veya `1`:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-130">To disable automatic loading of hosting startup assemblies, set one of the following to `true` or `1`:</span></span>

* <span data-ttu-id="2cb8c-131">[Barındırma başlangıç önlemek](xref:fundamentals/host/web-host#prevent-hosting-startup) ana bilgisayar yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-131">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="2cb8c-132">`ASPNETCORE_PREVENTHOSTINGSTARTUP` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-132">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

::: moniker-end

<span data-ttu-id="2cb8c-133">Hem ana bilgisayar yapılandırma ayarı ve ortam değişkenini ayarlarsanız, ana bilgisayar ayarını davranışını denetler.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-133">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="2cb8c-134">Ana bilgisayar ayarı veya ortam değişkenini kullanarak barındırma başlangıç derlemeleri devre dışı bırakma, derlemenin genel olarak devre dışı bırakır ve uygulama çeşitli özelliklerini devre dışı bırakabilir.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-134">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="2cb8c-135">Proje</span><span class="sxs-lookup"><span data-stu-id="2cb8c-135">Project</span></span>

<span data-ttu-id="2cb8c-136">Bir barındırma başlatma aşağıdaki proje türlerini birini oluşturun:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-136">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="2cb8c-137">Sınıf kitaplığı</span><span class="sxs-lookup"><span data-stu-id="2cb8c-137">Class library</span></span>](#class-library)
* [<span data-ttu-id="2cb8c-138">Konsol uygulaması giriş noktası olmayan</span><span class="sxs-lookup"><span data-stu-id="2cb8c-138">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="2cb8c-139">Sınıf kitaplığı</span><span class="sxs-lookup"><span data-stu-id="2cb8c-139">Class library</span></span>

<span data-ttu-id="2cb8c-140">Bir barındırma başlangıç geliştirme'de sınıf kitaplığının sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-140">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="2cb8c-141">Kitaplığı içeren bir `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-141">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="2cb8c-142">[Örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) bir Razor sayfaları uygulamasını içeren *HostingStartupApp*ve bir sınıf kitaplığı *HostingStartupLibrary*.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-142">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="2cb8c-143">Sınıf kitaplığı:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-143">The class library:</span></span>

* <span data-ttu-id="2cb8c-144">Bir barındırma başlangıç sınıfı içeren `ServiceKeyInjection`, uygulayan `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-144">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="2cb8c-145">`ServiceKeyInjection` bellek içi yapılandırma Sağlayıcısı'nı kullanarak uygulamanın yapılandırmasına hizmet dizeleri çifti ekler ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span><span class="sxs-lookup"><span data-stu-id="2cb8c-145">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span></span>
* <span data-ttu-id="2cb8c-146">İçeren bir `HostingStartup` barındırma startup şirketinizin ad alanını ve sınıf tanımlayan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-146">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="2cb8c-147">`ServiceKeyInjection` Sınıfın [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) yöntemi kullanan bir [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) geliştirmeleri için uygulama ekleme.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-147">The `ServiceKeyInjection` class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="2cb8c-148">`IHostingStartup.Configure` barındırma başlangıç derleme önce çalışma zamanı tarafından çağrılır `Startup.Configure` kullanıcı kodunda barındırma başlangıç derlemesi tarafından sağlanan herhangi bir yapılandırma üzerine yazmak kullanıcı kodu sağlar.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-148">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

<span data-ttu-id="2cb8c-149">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-149">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="2cb8c-150">Uygulama dizin sayfasına okur ve Sınıf Kitaplığı'nızın barındırma başlangıç derlemesi tarafından ayarlanmış iki anahtarı için yapılandırma değerlerini işler:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-150">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="2cb8c-151">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-151">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="2cb8c-152">[Örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) de ayrı bir barındırma başlatma sağlayan bir NuGet paketi proje içerir *HostingStartupPackage*.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-152">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="2cb8c-153">Paket, daha önce açıklanan Sınıf Kitaplığı'nın aynı özelliklere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-153">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="2cb8c-154">Paketi:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-154">The package:</span></span>

* <span data-ttu-id="2cb8c-155">Bir barındırma başlangıç sınıfı içeren `ServiceKeyInjection`, uygulayan `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-155">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="2cb8c-156">`ServiceKeyInjection` Hizmet dizeleri çifti uygulamanın yapılandırmasına ekler.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-156">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="2cb8c-157">İçeren bir `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-157">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="2cb8c-158">*HostingStartupPackage/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-158">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="2cb8c-159">Uygulama dizin sayfasına okur ve paketin barındırma başlangıç derlemesi tarafından ayarlanmış iki anahtarı için yapılandırma değerlerini işler:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-159">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="2cb8c-160">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-160">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="2cb8c-161">Konsol uygulaması giriş noktası olmayan</span><span class="sxs-lookup"><span data-stu-id="2cb8c-161">Console app without an entry point</span></span>

<span data-ttu-id="2cb8c-162">*Bu yaklaşım, yalnızca .NET Framework .NET Core uygulamaları için kullanılabilir.*</span><span class="sxs-lookup"><span data-stu-id="2cb8c-162">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="2cb8c-163">Bir konsol uygulaması giriş noktası olmayan bir derleme zamanı başvurusu için etkinleştirme gerektirmeyen bir dinamik barındırma başlangıç geliştirmesi sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-163">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point.</span></span> <span data-ttu-id="2cb8c-164">Uygulamayı içeren bir `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-164">The app contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="2cb8c-165">Dinamik barındırma başlangıç oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-165">To create a dynamic hosting startup:</span></span>

1. <span data-ttu-id="2cb8c-166">Bir uygulama kitaplığı içeren sınıfından oluşturulur `IHostingStartup` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-166">An implementation library is created from the class that contains the `IHostingStartup` implementation.</span></span> <span data-ttu-id="2cb8c-167">Uygulama kitaplığı, normal bir paket olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-167">The implementation library is treated as a normal package.</span></span>
1. <span data-ttu-id="2cb8c-168">Bir konsol uygulaması giriş noktası olmayan uygulama kitaplığı paketi başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-168">A console app without an entry point references the implementation library package.</span></span> <span data-ttu-id="2cb8c-169">Bir konsol uygulaması için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-169">A console app is used because:</span></span>
   * <span data-ttu-id="2cb8c-170">Bağımlılıkları dosyasının bir çalıştırılabilir uygulama varlık olduğundan kitaplık bağımlılıkları dosyasının furnish olamaz.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-170">A dependencies file is a runnable app asset, so a library can't furnish a dependencies file.</span></span>
   * <span data-ttu-id="2cb8c-171">Bir kitaplık doğrudan eklenemez [çalışma zamanı Paket Deposu](/dotnet/core/deploying/runtime-store), paylaşılan çalışma zamanını hedefleyen çalıştırılabilir bir proje gerektirir.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-171">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>
1. <span data-ttu-id="2cb8c-172">Konsol uygulaması barındırma startup şirketinizin bağımlılıkları almak için yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-172">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="2cb8c-173">Kullanılmayan bağımlılıkları bağımlılıkları dosyasından atılır konsol uygulaması yayımlama bir sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-173">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
1. <span data-ttu-id="2cb8c-174">Uygulama ve onun bağımlılıklarını dosyası çalışma zamanı paketi deposuna yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-174">The app and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="2cb8c-175">Barındırma başlangıç derleme ve bağımlılıkları dosyasını bulmak için bunlar ortam değişkenlerinin bir çift başvurulan.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-175">To discover the hosting startup assembly and its dependencies file, they're referenced in a pair of environment variables.</span></span>

<span data-ttu-id="2cb8c-176">Konsol uygulama başvuruları [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) paket:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-176">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

<span data-ttu-id="2cb8c-177">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) özniteliğini tanımlayan bir sınıf uygulaması `IHostingStartup` yüklenirken ve oluşturulurken yürütme [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="2cb8c-177">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="2cb8c-178">Aşağıdaki örnekte, ad alanı `StartupEnhancement`, ve sınıf `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-178">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="2cb8c-179">Arabirimini uygulayan bir sınıf `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-179">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="2cb8c-180">Sınıfın [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) yöntemi kullanan bir [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) geliştirmeleri için uygulama ekleme.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-180">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="2cb8c-181">`IHostingStartup.Configure` barındırma başlangıç derleme önce çalışma zamanı tarafından çağrılır `Startup.Configure` kullanıcı kodunda barındırma başlangıç derlemesi tarafından sağlanan herhangi bir yapılandırma üzerine yazmak kullanıcı kodu sağlar.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-181">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="2cb8c-182">Oluşturma sırasında bir `IHostingStartup` proje bağımlılıkları dosyasının (*\*. deps.json*) ayarlar `runtime` derlemeye konumunu *bin* klasörü:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-182">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="2cb8c-183">Dosyanın yalnızca bir parçası olarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-183">Only part of the file is shown.</span></span> <span data-ttu-id="2cb8c-184">Derleme adı örnekte `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-184">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="2cb8c-185">Barındırma başlangıç derlemeyi belirtin</span><span class="sxs-lookup"><span data-stu-id="2cb8c-185">Specify the hosting startup assembly</span></span>

<span data-ttu-id="2cb8c-186">Bir sınıf kitaplığı - veya konsol uygulaması sağlanan-başlangıç barındırma için barındırma başlangıç derlemenin adını belirtin. `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-186">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="2cb8c-187">Ortam değişkenini derlemelerin noktalı virgülle ayrılmış bir listedir.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-187">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="2cb8c-188">Yalnızca barındırma başlangıç derlemeler için taranan `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-188">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="2cb8c-189">Örnek uygulama için *HostingStartupApp*, daha önce açıklanan barındırma startup'lar bulmak için ortam değişkeni şu değere ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-189">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="2cb8c-190">Bir barındırma başlangıç derleme kullanılarak da ayarlanabilir [barındırma başlangıç derlemeleri](xref:fundamentals/host/web-host#hosting-startup-assemblies) ana bilgisayar yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-190">A hosting startup assembly can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="2cb8c-191">Ne zaman birden çok barındırma başlangıç çeviren varsa, bunların [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) yöntemleri derlemeleri listelenen sırayla yürütülür.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-191">When multiple hosting startup assembles are present, their [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="2cb8c-192">Etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="2cb8c-192">Activation</span></span>

<span data-ttu-id="2cb8c-193">Başlangıç etkinleştirme barındırmak için Seçenekler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-193">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="2cb8c-194">[Çalışma zamanı deposu](#runtime-store) &ndash; etkinleştirme, etkinleştirme için bir derleme zamanı başvurusu gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-194">[Runtime store](#runtime-store) &ndash; Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="2cb8c-195">Örnek uygulama barındırma başlangıç derleme ve bağımlılıkları dosyalar bir klasöre yerleştirir *dağıtım*multimachine ortamında barındırma başlangıç dağıtımını kolaylaştırmak için.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-195">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="2cb8c-196">*Dağıtım* klasörü oluşturur veya değiştirir barındırma için başlangıç etkinleştirmek için dağıtım sistemi ortam değişkenlerini bir PowerShell betiğini de içerir.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-196">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="2cb8c-197">Etkinleştirme için gerekli derleme zamanı başvurusu</span><span class="sxs-lookup"><span data-stu-id="2cb8c-197">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="2cb8c-198">NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="2cb8c-198">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="2cb8c-199">Proje bin klasörü</span><span class="sxs-lookup"><span data-stu-id="2cb8c-199">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="2cb8c-200">Çalışma zamanı deposu</span><span class="sxs-lookup"><span data-stu-id="2cb8c-200">Runtime store</span></span>

<span data-ttu-id="2cb8c-201">Barındırma başlangıç uygulaması yerleştirilir [çalışma zamanı deposu](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="2cb8c-201">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="2cb8c-202">Gelişmiş uygulama tarafından bir derleme zamanı başvurusu derleme için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-202">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="2cb8c-203">Barındırma başlangıç oluşturulduktan sonra bildirim dosyasını barındırma startup şirketinizin proje dosyası gören [dotnet deposu](/dotnet/core/tools/dotnet-store) komutu.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-203">After the hosting startup is built, the hosting startup's project file serves as the manifest file for the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```console
dotnet store --manifest <PROJECT_FILE> --runtime <RUNTIME_IDENTIFIER>
```

<span data-ttu-id="2cb8c-204">Bu komut, barındırma başlangıç derleme ve kullanıcı profilinin çalışma zamanı mağazada paylaşılan Framework'teki parçası olmayan diğer bağımlılıkları yerleştirir:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-204">This command places the hosting startup assembly and other dependencies that aren't part of the shared framework in the user profile's runtime store at:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2cb8c-205">Windows</span><span class="sxs-lookup"><span data-stu-id="2cb8c-205">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="2cb8c-206">macOS</span><span class="sxs-lookup"><span data-stu-id="2cb8c-206">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="2cb8c-207">Linux</span><span class="sxs-lookup"><span data-stu-id="2cb8c-207">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

<span data-ttu-id="2cb8c-208">Derleme ve bağımlılıkları genel kullanım için yerleştirmek isterse ekleme `-o|--output` seçeneğini `dotnet store` komutunu şu yola sahip:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-208">If you desire to place the assembly and dependencies for global use, add the `-o|--output` option to the `dotnet store` command with the following path:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2cb8c-209">Windows</span><span class="sxs-lookup"><span data-stu-id="2cb8c-209">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="2cb8c-210">macOS</span><span class="sxs-lookup"><span data-stu-id="2cb8c-210">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="2cb8c-211">Linux</span><span class="sxs-lookup"><span data-stu-id="2cb8c-211">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

<span data-ttu-id="2cb8c-212">**Değiştirme ve barındırma startup şirketinizin bağımlılıkları dosyasının yerleştirin**</span><span class="sxs-lookup"><span data-stu-id="2cb8c-212">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="2cb8c-213">Çalışma zamanı konumun belirtildiğinden  *\*. deps.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-213">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="2cb8c-214">Geliştirmesini etkinleştirmek için `runtime` öğesi geliştirme'nın çalışma zamanı derleme konumunu belirtin.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-214">To activate the enhancement, the `runtime` element must specify the location of the enhancement's runtime assembly.</span></span> <span data-ttu-id="2cb8c-215">Önek `runtime` konumuyla `lib/<TARGET_FRAMEWORK_MONIKER>/`:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-215">Prefix the `runtime` location with `lib/<TARGET_FRAMEWORK_MONIKER>/`:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="2cb8c-216">Örnek kodda (*StartupDiagnostics* Proje), değiştirilmesini  *\*. deps.json* dosya tarafından gerçekleştirilir bir [PowerShell](/powershell/scripting/powershell-scripting) betiği.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-216">In the sample code (*StartupDiagnostics* project), modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="2cb8c-217">PowerShell Betiği, bir yapı hedefi proje dosyasında tarafından otomatik olarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-217">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

<span data-ttu-id="2cb8c-218">Uygulama kullanıcının  *\*. deps.json* dosya erişilebilir bir konumda olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-218">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="2cb8c-219">Kullanıcı başına kullanım için dosyaya koyun *additonalDeps* kullanıcı profilinin klasöründe `.dotnet` ayarları:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-219">For per-user use, place the file in the *additonalDeps* folder of the user profile's `.dotnet` settings:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2cb8c-220">Windows</span><span class="sxs-lookup"><span data-stu-id="2cb8c-220">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="2cb8c-221">macOS</span><span class="sxs-lookup"><span data-stu-id="2cb8c-221">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="2cb8c-222">Linux</span><span class="sxs-lookup"><span data-stu-id="2cb8c-222">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

<span data-ttu-id="2cb8c-223">Genel kullanım için dosyayı yerleştirmek *additonalDeps* .NET Core yükleme klasörü:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-223">For global use, place the file in the *additonalDeps* folder of the .NET Core installation:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2cb8c-224">Windows</span><span class="sxs-lookup"><span data-stu-id="2cb8c-224">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="2cb8c-225">macOS</span><span class="sxs-lookup"><span data-stu-id="2cb8c-225">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="2cb8c-226">Linux</span><span class="sxs-lookup"><span data-stu-id="2cb8c-226">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

<span data-ttu-id="2cb8c-227">Paylaşılan framework sürümünü hedef uygulamanın kullandığı paylaşılan çalışma zamanı sürümü yansıtır.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-227">The shared framework version reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="2cb8c-228">Paylaşılan çalışma zamanı gösterilen  *\*. runtimeconfig.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-228">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="2cb8c-229">Örnek uygulamada (*HostingStartupApp*), paylaşılan çalışma zamanı içinde belirtilen *HostingStartupApp.runtimeconfig.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-229">In the sample app (*HostingStartupApp*), the shared runtime is specified in the *HostingStartupApp.runtimeconfig.json* file.</span></span>

<span data-ttu-id="2cb8c-230">**Barındırma startup şirketinizin bağımlılık dosya listesi**</span><span class="sxs-lookup"><span data-stu-id="2cb8c-230">**List the hosting startup's dependencies file**</span></span>

<span data-ttu-id="2cb8c-231">Uygulama kullanıcının konumunu  *\*. deps.json* dosya listelendiğinden `DOTNET_ADDITIONAL_DEPS` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-231">The location of the implementation's *\*.deps.json* file is listed in the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="2cb8c-232">Dosya bir kullanıcı profilin yerleştirilir *.dotnet* klasörü ortam değişken değerini ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-232">If the file is placed in the user profile's *.dotnet* folder, set the environment variable's value to:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2cb8c-233">Windows</span><span class="sxs-lookup"><span data-stu-id="2cb8c-233">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="2cb8c-234">macOS</span><span class="sxs-lookup"><span data-stu-id="2cb8c-234">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="2cb8c-235">Linux</span><span class="sxs-lookup"><span data-stu-id="2cb8c-235">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

---

<span data-ttu-id="2cb8c-236">.NET Core yüklemesinde genel kullanım dosya yerleştirilirse, dosyanın tam yolunu sağlayın:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-236">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2cb8c-237">Windows</span><span class="sxs-lookup"><span data-stu-id="2cb8c-237">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="macostabmacos"></a>[<span data-ttu-id="2cb8c-238">macOS</span><span class="sxs-lookup"><span data-stu-id="2cb8c-238">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="2cb8c-239">Linux</span><span class="sxs-lookup"><span data-stu-id="2cb8c-239">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

---

<span data-ttu-id="2cb8c-240">Örnek uygulama için (*HostingStartupApp*) bağımlılıkları dosyayı bulmak için (*HostingStartupApp.runtimeconfig.json*), kullanıcının profilinde bağımlılıkları dosyasının yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-240">For the sample app (*HostingStartupApp*) to find the dependencies file (*HostingStartupApp.runtimeconfig.json*), the dependencies file is placed in the user's profile.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2cb8c-241">Windows</span><span class="sxs-lookup"><span data-stu-id="2cb8c-241">Windows</span></span>](#tab/windows)

<span data-ttu-id="2cb8c-242">Ayarlama `DOTNET_ADDITIONAL_DEPS` ortam değişkeni şu değere:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-242">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="2cb8c-243">macOS</span><span class="sxs-lookup"><span data-stu-id="2cb8c-243">macOS</span></span>](#tab/macos)

<span data-ttu-id="2cb8c-244">Ayarlama `DOTNET_ADDITIONAL_DEPS` ortam değişkeni şu değere:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-244">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="2cb8c-245">Linux</span><span class="sxs-lookup"><span data-stu-id="2cb8c-245">Linux</span></span>](#tab/linux)

<span data-ttu-id="2cb8c-246">Ayarlama `DOTNET_ADDITIONAL_DEPS` ortam değişkeni şu değere:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-246">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

---

<span data-ttu-id="2cb8c-247">Çeşitli işletim sistemleri için ortam değişkenlerini ayarlama örnekleri için bkz: [birden fazla ortam kullanayım](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="2cb8c-247">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="2cb8c-248">**Dağıtım**</span><span class="sxs-lookup"><span data-stu-id="2cb8c-248">**Deployment**</span></span>

<span data-ttu-id="2cb8c-249">Bir barındırma başlatma multimachine ortamında dağıtımını kolaylaştırmak için örnek uygulamayı oluşturur. bir *dağıtım* içeren yayımlanan çıkış klasöründe:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-249">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="2cb8c-250">Barındırma başlangıç derleme.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-250">The hosting startup assembly.</span></span>
* <span data-ttu-id="2cb8c-251">Barındırma başlangıç bağımlılıkları dosyası.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-251">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="2cb8c-252">Bir PowerShell Betiği oluşturur veya değiştirir `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ve `DOTNET_ADDITIONAL_DEPS` barındırma başlangıç etkinleştirmeyi desteklemek için.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-252">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="2cb8c-253">Komut dosyası dağıtım sistemde bir yönetici PowerShell komut isteminden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-253">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="2cb8c-254">NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="2cb8c-254">NuGet package</span></span>

<span data-ttu-id="2cb8c-255">Bir NuGet paketi barındırma bir başlangıç geliştirmesi sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-255">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="2cb8c-256">Paketin bir `HostingStartup` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-256">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="2cb8c-257">Paket tarafından sağlanan barındırma başlangıç türleri, aşağıdaki yaklaşımlardan birini kullanarak uygulama için kullanıma sunulur:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-257">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="2cb8c-258">Gelişmiş uygulamanın proje dosyası, barındırma başlangıç için bir paket başvurusu uygulamanın proje dosyası (bir derleme zamanı Başvurusu) sağlar.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-258">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="2cb8c-259">Yerinde derleme zamanı başvurusu ile barındırma başlangıç derleme ve tüm bağımlılıklarını uygulamanın bağımlılık dosyasına dahil edilir (*\*. deps.json*).</span><span class="sxs-lookup"><span data-stu-id="2cb8c-259">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="2cb8c-260">Bu yaklaşım, yayımlanan bir barındırma başlangıç derleme paketi uygulandığı [nuget.org](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="2cb8c-260">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="2cb8c-261">Barındırma startup şirketinizin bağımlılıkları dosyasının açıklandığı gibi gelişmiş uygulama için kullanılabilir hale getirileceğini [çalışma zamanı deposu](#runtime-store) bölümü (olmadan, bir derleme zamanı Başvurusu).</span><span class="sxs-lookup"><span data-stu-id="2cb8c-261">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="2cb8c-262">NuGet paketlerini ve çalışma zamanı mağazası hakkında daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-262">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="2cb8c-263">Platformlar arası araçlarla NuGet paketi oluşturma</span><span class="sxs-lookup"><span data-stu-id="2cb8c-263">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="2cb8c-264">Paket yayımlama</span><span class="sxs-lookup"><span data-stu-id="2cb8c-264">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="2cb8c-265">Çalışma zamanı paket deposu</span><span class="sxs-lookup"><span data-stu-id="2cb8c-265">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="2cb8c-266">Proje bin klasörü</span><span class="sxs-lookup"><span data-stu-id="2cb8c-266">Project bin folder</span></span>

<span data-ttu-id="2cb8c-267">Bir barındırma başlangıç geliştirmesi tarafından sağlanan bir *bin*-Gelişmiş Uygulama derlemesinde dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-267">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="2cb8c-268">Derleme tarafından sağlanan barındırma başlangıç türleri, aşağıdaki yaklaşımlardan birini kullanarak uygulama için kullanıma sunulur:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-268">The hosting startup types provided by the assembly are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="2cb8c-269">Gelişmiş uygulamanın proje dosyası, barındırma başlangıç (bir derleme zamanı başvuru) bir bütünleştirilmiş kod başvurusu yapar.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-269">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="2cb8c-270">Yerinde derleme zamanı başvurusu ile barındırma başlangıç derleme ve tüm bağımlılıklarını uygulamanın bağımlılık dosyasına dahil edilir (*\*. deps.json*).</span><span class="sxs-lookup"><span data-stu-id="2cb8c-270">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="2cb8c-271">Dağıtım senaryosu derlenmiş barındırma başlangıç kitaplığın derleme (DLL dosyası) taşımak için alıcı projeye ya da bir konuma kullanan proje tarafından erişilebilir çağırır ve barındırmak için bir derleme zamanı başvurusu yapılan bu yaklaşım uygulanır Startup şirketinizin derleme.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-271">This approach applies when the deployment scenario calls for moving the compiled hosting startup library's assembly (DLL file) to the consuming project or to a location accessible by the consuming project and a compile-time reference is made to the hosting startup's assembly.</span></span>
* <span data-ttu-id="2cb8c-272">Barındırma startup şirketinizin bağımlılıkları dosyasının açıklandığı gibi gelişmiş uygulama için kullanılabilir hale getirileceğini [çalışma zamanı deposu](#runtime-store) bölümü (olmadan, bir derleme zamanı Başvurusu).</span><span class="sxs-lookup"><span data-stu-id="2cb8c-272">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

## <a name="sample-code"></a><span data-ttu-id="2cb8c-273">Örnek kod</span><span class="sxs-lookup"><span data-stu-id="2cb8c-273">Sample code</span></span>

<span data-ttu-id="2cb8c-274">[Örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)) barındırma başlangıç uygulama senaryolarını gösterir:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-274">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="2cb8c-275">İki barındırma başlangıç derlemeleri (sınıf kitaplıkları), bellek içi yapılandırma anahtar-değer çiftleri her bir çifti ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-275">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="2cb8c-276">NuGet paketini (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="2cb8c-276">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="2cb8c-277">Sınıf kitaplığı (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="2cb8c-277">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="2cb8c-278">Bir barındırma başlatma deposu dağıtılan bir çalışma zamanı derleme etkinleştirilir (*StartupDiagnostics*).</span><span class="sxs-lookup"><span data-stu-id="2cb8c-278">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="2cb8c-279">Derleme, uygulamaya tanılama bilgileri sağlayan başlangıçta iki middlewares ekler:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-279">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="2cb8c-280">Kaydedilen Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="2cb8c-280">Registered services</span></span>
  * <span data-ttu-id="2cb8c-281">Adres (Düzen, konak, temel yolu, yol, sorgu dizesi)</span><span class="sxs-lookup"><span data-stu-id="2cb8c-281">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="2cb8c-282">Bağlantı (uzak IP, uzak bağlantı noktasını, yerel IP yerel bağlantı noktası, istemci sertifikası)</span><span class="sxs-lookup"><span data-stu-id="2cb8c-282">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="2cb8c-283">İstek üst bilgileri</span><span class="sxs-lookup"><span data-stu-id="2cb8c-283">Request headers</span></span>
  * <span data-ttu-id="2cb8c-284">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="2cb8c-284">Environment variables</span></span>

<span data-ttu-id="2cb8c-285">Örneği çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-285">To run the sample:</span></span>

<span data-ttu-id="2cb8c-286">**NuGet paketinden etkinleştirme**</span><span class="sxs-lookup"><span data-stu-id="2cb8c-286">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="2cb8c-287">Derleme *HostingStartupPackage* ile paket [dotnet paketi](/dotnet/core/tools/dotnet-pack) komutu.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-287">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="2cb8c-288">Paketin derleme adını eklemek *HostingStartupPackage* için `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-288">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="2cb8c-289">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-289">Compile and run the app.</span></span> <span data-ttu-id="2cb8c-290">Geliştirilmiş bir uygulamada (bir derleme zamanı Başvurusu) bir paket başvurusu yok.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-290">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="2cb8c-291">A `<PropertyGroup>` uygulama projesinde paket projenin çıkış dosyasını belirtir. (*.. / HostingStartupPackage/bin/Debug*) bir paket olarak.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-291">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="2cb8c-292">Bu paketi yüklenmeden kaydedilip paketini kullanmanıza olanak verir [nuget.org](https://www.nuget.org/). Notları HostingStartupApp'ın proje dosyasında daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-292">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```
1. <span data-ttu-id="2cb8c-293">Dizin sayfası tarafından işlenen hizmet yapılandırması anahtar değerleri paketin tarafından ayarlanan değerlerle eşleşen gözlemleyin `ServiceKeyInjection.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-293">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="2cb8c-294">Değişiklikler yaparsanız *HostingStartupPackage* proje ve yeniden derleyin, emin olmak için yerel NuGet paketi önbellekler temizlenir *HostingStartupApp* güncelleştirilmiş paket bir eski alır Paket yerel önbellekten.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-294">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="2cb8c-295">Yerel NuGet önbellekleri temizlemek için aşağıdakileri yürütün [dotnet nuget Yereller](/dotnet/core/tools/dotnet-nuget-locals) komutu:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-295">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```console
dotnet nuget locals all --clear
```

<span data-ttu-id="2cb8c-296">**Bir sınıf kitaplığından etkinleştirme**</span><span class="sxs-lookup"><span data-stu-id="2cb8c-296">**Activation from a class library**</span></span>

1. <span data-ttu-id="2cb8c-297">Derleme *HostingStartupLibrary* sınıf kitaplığı ile [dotnet derleme](/dotnet/core/tools/dotnet-build) komutu.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-297">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="2cb8c-298">Sınıf Kitaplığı'nızın derleme adını eklemek *HostingStartupLibrary* için `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-298">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="2cb8c-299">*Depo*-kopyalayarak Sınıf Kitaplığı'nızın derleme uygulamalarıyla *HostingStartupLibrary.dll* Sınıf Kitaplığı'nızın dosyasından çıktıyı uygulamanın derlenmiş *bin/Debug* klasör.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-299">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="2cb8c-300">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-300">Compile and run the app.</span></span> <span data-ttu-id="2cb8c-301">Bir `<ItemGroup>` uygulama projesinde sınıf kitaplığı'nızın derleme dosyasına başvurur (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (bir derleme zamanı Başvurusu).</span><span class="sxs-lookup"><span data-stu-id="2cb8c-301">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="2cb8c-302">Notları HostingStartupApp'ın proje dosyasında daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-302">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```
1. <span data-ttu-id="2cb8c-303">Dizin sayfası tarafından işlenen hizmet yapılandırması anahtar değerleri sınıf kitaplığının tarafından ayarlanan değerlerle eşleşen gözlemleyin `ServiceKeyInjection.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-303">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="2cb8c-304">**Mağaza tarafından dağıtılan bir çalışma zamanı derlemesindeki etkinleştirme**</span><span class="sxs-lookup"><span data-stu-id="2cb8c-304">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="2cb8c-305">*StartupDiagnostics* proje kullandığı [PowerShell](/powershell/scripting/powershell-scripting) değiştirmek için kendi *StartupDiagnostics.deps.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-305">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="2cb8c-306">PowerShell, Windows 7 SP1 ve Windows Server 2008 R2 SP1 ile başlayarak Windows üzerinde varsayılan olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-306">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="2cb8c-307">PowerShell diğer platformlarda edinmek için bkz. [Windows PowerShell'i yükleme](/powershell/scripting/setup/installing-powershell#powershell-core).</span><span class="sxs-lookup"><span data-stu-id="2cb8c-307">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span></span>
1. <span data-ttu-id="2cb8c-308">Derleme *StartupDiagnostics* proje.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-308">Build the *StartupDiagnostics* project.</span></span> <span data-ttu-id="2cb8c-309">Sonra projeyi oluşturulduğuna göre bir yapı hedefi proje dosyasında otomatik olarak:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-309">After the project is built, a build target in the project file automatically:</span></span>
   * <span data-ttu-id="2cb8c-310">Değiştirmek için PowerShell betiğini tetikler *StartupDiagnostics.deps.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-310">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="2cb8c-311">Taşır *StartupDiagnostics.deps.json* kullanıcı profilinin dosyasına *additionalDeps* klasör.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-311">Moves the *StartupDiagnostics.deps.json* file to the user profile's *additionalDeps* folder.</span></span>
1. <span data-ttu-id="2cb8c-312">Yürütme `dotnet store` derleme depolamak için barındırma startup şirketinizin dizinindeki bir command prompt ve bağımlılıklarını kullanıcı profilinin çalışma zamanı deposundaki komutu:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-312">Execute the `dotnet store` command at a command prompt in the hosting startup's directory to store the assembly and its dependencies in the user profile's runtime store:</span></span>

   ```console
   dotnet store --manifest StartupDiagnostics.csproj --runtime <RID>
   ```
   <span data-ttu-id="2cb8c-313">Windows için komut `win7-x64` [çalışma zamanı tanımlayıcı (RID)](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="2cb8c-313">For Windows, the command uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="2cb8c-314">Barındırma için başlangıç için farklı bir çalışma zamanı sağlanırken doğru RID değiştirin.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-314">When providing the hosting startup for a different runtime, substitute the correct RID.</span></span>
1. <span data-ttu-id="2cb8c-315">Ortam değişkenlerini ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="2cb8c-315">Set the environment variables:</span></span>
   * <span data-ttu-id="2cb8c-316">Derleme adını eklemek *StartupDiagnostics* için `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-316">Add the assembly name of *StartupDiagnostics* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="2cb8c-317">Windows üzerinde ayarlanmış `DOTNET_ADDITIONAL_DEPS` ortam değişkenine `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-317">On Windows, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`.</span></span> <span data-ttu-id="2cb8c-318">/ Linux, macOS üzerinde ayarlanmış `DOTNET_ADDITIONAL_DEPS` ortam değişkenine `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`burada `<USER>` barındırma başlangıç içeren kullanıcı profili.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-318">On macOS/Linux, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, where `<USER>` is the user profile that contains the hosting startup.</span></span>
1. <span data-ttu-id="2cb8c-319">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-319">Run the sample app.</span></span>
1. <span data-ttu-id="2cb8c-320">İstek `/services` uygulamanın görmek için uç nokta Hizmetleri kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-320">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="2cb8c-321">İstek `/diag` tanılama bilgileri görmek için uç nokta.</span><span class="sxs-lookup"><span data-stu-id="2cb8c-321">Request the `/diag` endpoint to see the diagnostic information.</span></span>
