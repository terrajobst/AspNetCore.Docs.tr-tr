---
title: ASP.NET Core IHostingStartup ile dış bir derlemede uygulama geliştirmek
author: guardrex
description: IHostingStartup uygulaması kullanılarak bir dış derleme ASP.NET Core uygulama geliştirmek nasıl bulur.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 793169b491596cd7326d747a3f19d7fdaf7e2b65
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a><span data-ttu-id="19262-103">ASP.NET Core IHostingStartup ile dış bir derlemede uygulama geliştirmek</span><span class="sxs-lookup"><span data-stu-id="19262-103">Enhance an app from an external assembly in ASP.NET Core with IHostingStartup</span></span>

<span data-ttu-id="19262-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="19262-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="19262-105">Bir [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) uygulama sağlayan uygulamanın dışında bir dış derlemesinden başlatma sırasında bir uygulama için geliştirmeler ekleme `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="19262-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="19262-106">Örneğin, bir dış araçları kitaplığını kullanabilirsiniz bir `IHostingStartup` ek yapılandırma sağlayıcıları ya da bir uygulama hizmetlerini sağlamak için uygulama.</span><span class="sxs-lookup"><span data-stu-id="19262-106">For example, an external tooling library can use an `IHostingStartup` implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="19262-107">`IHostingStartup` *ASP.NET Core 2.0 bulunan ve üzerinde desteklenir.*</span><span class="sxs-lookup"><span data-stu-id="19262-107">`IHostingStartup` *is available in ASP.NET Core 2.0 and later.*</span></span>

<span data-ttu-id="19262-108">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="19262-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="19262-109">Yüklenen barındırma başlangıç derlemeleri Bul</span><span class="sxs-lookup"><span data-stu-id="19262-109">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="19262-110">Başlangıç derlemeleri barındırma kitaplıkları veya uygulama tarafından yüklenen bulmak için günlük kaydını etkinleştirmek ve uygulama günlüklerini denetleyin.</span><span class="sxs-lookup"><span data-stu-id="19262-110">To discover hosting startup assemblies loaded by the app or by libraries, enable logging and check the application logs.</span></span> <span data-ttu-id="19262-111">Derlemeleri yükleme sırasında oluşan hataları günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="19262-111">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="19262-112">Yüklenen barındırma başlangıç derlemeler hata ayıklama düzeyinde günlüğe kaydedilir ve tüm hatalar günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="19262-112">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

<span data-ttu-id="19262-113">Örnek uygulama okuma [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) içine bir `string` dizisi ve sonucu uygulamanın dizin sayfasını görüntüler:</span><span class="sxs-lookup"><span data-stu-id="19262-113">The sample app reads the [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) into a `string` array and displays the result in the app's Index page:</span></span>

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="19262-114">Başlangıç derlemeleri barındırma otomatik yükleme devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="19262-114">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="19262-115">Başlangıç derlemeleri barındırma otomatik yükleme devre dışı bırakmak için iki yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="19262-115">There are two ways to disable the automatic loading of hosting startup assemblies:</span></span>

* <span data-ttu-id="19262-116">Ayarlama [barındırma başlangıç engellemek](xref:fundamentals/host/web-host#prevent-hosting-startup) ana bilgisayar yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="19262-116">Set the [Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="19262-117">Ayarlama `ASPNETCORE_PREVENTHOSTINGSTARTUP` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="19262-117">Set the `ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

<span data-ttu-id="19262-118">Ana bilgisayar ayarını veya ortam değişkeni ayarlandığında `true` veya `1`, barındırma başlangıç derlemeler otomatik olarak yüklü değildir.</span><span class="sxs-lookup"><span data-stu-id="19262-118">When either the host setting or the environment variable is set to `true` or `1`, hosting startup assemblies aren't automatically loaded.</span></span> <span data-ttu-id="19262-119">Her ikisi de ayarlanırsa, ana bilgisayar ayarını davranışını denetler.</span><span class="sxs-lookup"><span data-stu-id="19262-119">If both are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="19262-120">Ana bilgisayar ayarı veya ortam değişkeni kullanarak barındırma başlangıç derlemeler devre dışı bırakma genel bunları devre dışı bırakır ve çeşitli özellikleri, bir uygulamanın devre dışı bırakabilir.</span><span class="sxs-lookup"><span data-stu-id="19262-120">Disabling hosting startup assemblies using the host setting or environment variable disables them globally and may disable several characteristics of an app.</span></span> <span data-ttu-id="19262-121">Seçmeli olarak kitaplığı kendi yapılandırma seçeneği sunmuyorsa kitaplığı tarafından eklenen bir barındırma başlangıç derleme devre dışı bırakmak şu anda mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="19262-121">It isn't currently possible to selectively disable a hosting startup assembly added by a library unless the library offers its own configuration option.</span></span> <span data-ttu-id="19262-122">Gelecek sürümlerinden seçerek barındırma başlangıç derlemeleri devre dışı bırakma özelliği sağlar (bkz [GitHub sorun aspnet/barındırma #1243](https://github.com/aspnet/Hosting/pull/1243)).</span><span class="sxs-lookup"><span data-stu-id="19262-122">A future release will offer the ability to selectively disable hosting startup assemblies (see [GitHub issue aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243)).</span></span>

## <a name="implement-ihostingstartup"></a><span data-ttu-id="19262-123">Uygulama IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="19262-123">Implement IHostingStartup</span></span>

### <a name="create-the-assembly"></a><span data-ttu-id="19262-124">Derleme oluşturma</span><span class="sxs-lookup"><span data-stu-id="19262-124">Create the assembly</span></span>

<span data-ttu-id="19262-125">Bir `IHostingStartup` geliştirme bütünleştirilmiş bir konsol uygulaması giriş noktası olmadan dayalı olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="19262-125">An `IHostingStartup` enhancement is deployed as an assembly based on a console app without an entry point.</span></span> <span data-ttu-id="19262-126">Derleme başvurularını [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) paketi:</span><span class="sxs-lookup"><span data-stu-id="19262-126">The assembly references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupEnhancement.csproj)]

<span data-ttu-id="19262-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) özniteliği tanımlayan bir sınıf uygulaması `IHostingStartup` yükleme ve oluştururken yürütme için [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="19262-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="19262-128">Aşağıdaki örnekte, ad alanıdır `StartupEnhancement`, ve sınıf `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="19262-128">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="19262-129">Bir sınıf uygular `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="19262-129">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="19262-130">Sınıfının [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) yöntemi kullanan bir [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) geliştirmeleri bir uygulamaya eklemek için:</span><span class="sxs-lookup"><span data-stu-id="19262-130">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app:</span></span>

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="19262-131">Oluştururken bir `IHostingStartup` proje, bağımlılıkları dosyası (*\*. deps.json*) ayarlar `runtime` derlemeye konumunu *bin* klasörü:</span><span class="sxs-lookup"><span data-stu-id="19262-131">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="19262-132">Dosyanın yalnızca bir kısmını gösterilir.</span><span class="sxs-lookup"><span data-stu-id="19262-132">Only part of the file is shown.</span></span> <span data-ttu-id="19262-133">Örnek derleme adı `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="19262-133">The assembly name in the example is `StartupEnhancement`.</span></span>

### <a name="update-the-dependencies-file"></a><span data-ttu-id="19262-134">Bağımlılıklar dosyasını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="19262-134">Update the dependencies file</span></span>

<span data-ttu-id="19262-135">Çalışma zamanı konumun belirtildiğinden  *\*. deps.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="19262-135">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="19262-136">Etkin geliştirme `runtime` öğesi geliştirme'nın çalışma zamanı derlemesi konumunu belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="19262-136">To active the enhancement, the `runtime` element must specify the location of the enhancement's runtime assembly.</span></span> <span data-ttu-id="19262-137">Önek `runtime` konumla `lib/netcoreapp2.0/`:</span><span class="sxs-lookup"><span data-stu-id="19262-137">Prefix the `runtime` location with `lib/netcoreapp2.0/`:</span></span>

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="19262-138">Örnek uygulama, değiştirilmesini  *\*. deps.json* dosyası tarafından gerçekleştirilir bir [PowerShell](/powershell/scripting/powershell-scripting) komut dosyası.</span><span class="sxs-lookup"><span data-stu-id="19262-138">In the sample app, modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="19262-139">PowerShell komut dosyası derleme hedef proje dosyasında tarafından otomatik olarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="19262-139">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

### <a name="enhancement-activation"></a><span data-ttu-id="19262-140">Geliştirme etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="19262-140">Enhancement activation</span></span>

<span data-ttu-id="19262-141">**Derleme dosyası yerleştirin**</span><span class="sxs-lookup"><span data-stu-id="19262-141">**Place the assembly file**</span></span>

<span data-ttu-id="19262-142">`IHostingStartup` Uygulaması'nın derleme dosyası olmalıdır *bin*-uygulamada dağıtılan veya yerleştirilen [çalışma zamanı deposu](/dotnet/core/deploying/runtime-store):</span><span class="sxs-lookup"><span data-stu-id="19262-142">The `IHostingStartup` implementation's assembly file must be *bin*-deployed in the app or placed in the [runtime store](/dotnet/core/deploying/runtime-store):</span></span>

<span data-ttu-id="19262-143">Kullanıcı başına kullanım için kullanıcı profilinin çalışma zamanı mağazada derleme koyun:</span><span class="sxs-lookup"><span data-stu-id="19262-143">For per-user use, place the assembly in the user profile's runtime store at:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="19262-144">Genel kullanım için derleme .NET Core yüklemenin çalışma zamanı depolama alanına yerleştir:</span><span class="sxs-lookup"><span data-stu-id="19262-144">For global use, place the assembly in the .NET Core installation's runtime store:</span></span>

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="19262-145">Derleme çalışma zamanı depoya dağıtırken, simgeler dosyası dağıtılan de ancak geliştirme çalışması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="19262-145">When deploying the assembly to the runtime store, the symbols file may be deployed as well but isn't required for the enhancement to work.</span></span>

<span data-ttu-id="19262-146">**Bağımlılıklar dosyasını yerleştirin**</span><span class="sxs-lookup"><span data-stu-id="19262-146">**Place the dependencies file**</span></span>

<span data-ttu-id="19262-147">Uygulama 's  *\*. deps.json* dosya erişilebilir bir yerde olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="19262-147">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="19262-148">Kullanıcı başına kullanım için dosyasına yerleştirin `additonalDeps` kullanıcı profilinin klasöründe `.dotnet` ayarları:</span><span class="sxs-lookup"><span data-stu-id="19262-148">For per-user use, place the file in the `additonalDeps` folder of the user profile's `.dotnet` settings:</span></span> 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="19262-149">Genel kullanım için dosyasına yerleştirin `additonalDeps` .NET Core yükleme klasörü:</span><span class="sxs-lookup"><span data-stu-id="19262-149">For global use, place the file in the `additonalDeps` folder of the .NET Core installation:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="19262-150">Sürüm Not `2.0.0`, hedef uygulamanın kullandığı paylaşılan çalışma zamanı sürümü yansıtır.</span><span class="sxs-lookup"><span data-stu-id="19262-150">Note the version, `2.0.0`, reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="19262-151">Paylaşılan çalışma zamanı gösterilen  *\*. runtimeconfig.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="19262-151">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="19262-152">Paylaşılan çalışma zamanı belirtildiğinden örnek uygulamasında *HostingStartupSample.runtimeconfig.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="19262-152">In the sample app, the shared runtime is specified in the *HostingStartupSample.runtimeconfig.json* file.</span></span>

<span data-ttu-id="19262-153">**Ortam değişkenlerini ayarlama**</span><span class="sxs-lookup"><span data-stu-id="19262-153">**Set environment variables**</span></span>

<span data-ttu-id="19262-154">Aşağıdaki ortam değişkenleri geliştirme kullanan uygulama bağlamında ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="19262-154">Set the following environment variables in the context of the app that uses the enhancement.</span></span>

<span data-ttu-id="19262-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span><span class="sxs-lookup"><span data-stu-id="19262-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span></span>

<span data-ttu-id="19262-156">Yalnızca barındırma başlangıç derlemeler için taranan `HostingStartupAttribute`.</span><span class="sxs-lookup"><span data-stu-id="19262-156">Only hosting startup assemblies are scanned for the `HostingStartupAttribute`.</span></span> <span data-ttu-id="19262-157">Bu ortam değişkeninde sağlanan uygulaması derleme adıdır.</span><span class="sxs-lookup"><span data-stu-id="19262-157">The assembly name of the implementation is provided in this environment variable.</span></span> <span data-ttu-id="19262-158">Bu değer örnek uygulaması ayarlar `StartupDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="19262-158">The sample app sets this value to `StartupDiagnostics`.</span></span>

<span data-ttu-id="19262-159">Değer kullanılarak da ayarlanabilir [barındırma başlangıç derlemeleri](xref:fundamentals/host/web-host#hosting-startup-assemblies) ana bilgisayar yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="19262-159">The value can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="19262-160">DOTNET\_EK\_DEPS</span><span class="sxs-lookup"><span data-stu-id="19262-160">DOTNET\_ADDITIONAL\_DEPS</span></span>

<span data-ttu-id="19262-161">Uygulaması'nın konumunu  *\*. deps.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="19262-161">The location of the implementation's *\*.deps.json* file.</span></span>

<span data-ttu-id="19262-162">Dosya bir kullanıcı profilin yerleştirilir *.dotnet* kullanıcı başına kullanım için klasör:</span><span class="sxs-lookup"><span data-stu-id="19262-162">If the file is placed in the user profile's *.dotnet* folder for per-user use:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

<span data-ttu-id="19262-163">.NET Core yükleme genel kullanım için dosya yerleştirilir gerekirse, dosyanın tam yolunu sağlayın:</span><span class="sxs-lookup"><span data-stu-id="19262-163">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

<span data-ttu-id="19262-164">Örnek uygulaması bu değer ayarlar:</span><span class="sxs-lookup"><span data-stu-id="19262-164">The sample app sets this value to:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

<span data-ttu-id="19262-165">Çeşitli işletim sistemleri için ortam değişkenlerini ayarlama örnekleri için bkz: [kullanan birden çok ortamlar](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="19262-165">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="sample-app"></a><span data-ttu-id="19262-166">Örnek uygulaması</span><span class="sxs-lookup"><span data-stu-id="19262-166">Sample app</span></span>

<span data-ttu-id="19262-167">[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)) kullanan `IHostingStartup` bir tanılama aracı oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="19262-167">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) uses `IHostingStartup` to create a diagnostics tool.</span></span> <span data-ttu-id="19262-168">Aracı uygulama tanılama bilgilerini başlangıçta iki middlewares ekler:</span><span class="sxs-lookup"><span data-stu-id="19262-168">The tool adds two middlewares to the app at startup that provide diagnostic information:</span></span>

* <span data-ttu-id="19262-169">Kaydedilen Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="19262-169">Registered services</span></span>
* <span data-ttu-id="19262-170">Adres: düzeni, ana bilgisayar, temel yolu, yol, sorgu dizesi</span><span class="sxs-lookup"><span data-stu-id="19262-170">Address: scheme, host, path base, path, query string</span></span>
* <span data-ttu-id="19262-171">Bağlantı: uzak IP, uzak bağlantı noktası, yerel IP yerel bağlantı noktası, istemci sertifikası</span><span class="sxs-lookup"><span data-stu-id="19262-171">Connection: remote IP, remote port, local IP, local port, client certificate</span></span>
* <span data-ttu-id="19262-172">İstek üstbilgileri</span><span class="sxs-lookup"><span data-stu-id="19262-172">Request headers</span></span>
* <span data-ttu-id="19262-173">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="19262-173">Environment variables</span></span>

<span data-ttu-id="19262-174">Örneği çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="19262-174">To run the sample:</span></span>

1. <span data-ttu-id="19262-175">Başlangıç tanılama projenin kullandığı [PowerShell](/powershell/scripting/powershell-scripting) değiştirmek için kendi *StartupDiagnostics.deps.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="19262-175">The Startup Diagnostic project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="19262-176">PowerShell, Windows işletim Sisteminin Windows 7 SP1 ve Windows Server 2008 R2 SP1 ile başlayarak varsayılan olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="19262-176">PowerShell is installed by default on Windows OS starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="19262-177">Diğer platformlarda PowerShell edinmek için bkz: [Windows PowerShell'i yükleme](/powershell/scripting/setup/installing-windows-powershell).</span><span class="sxs-lookup"><span data-stu-id="19262-177">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span></span>
2. <span data-ttu-id="19262-178">Başlangıç tanılama projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="19262-178">Build the Startup Diagnostic project.</span></span> <span data-ttu-id="19262-179">Proje derleme hedefi:</span><span class="sxs-lookup"><span data-stu-id="19262-179">A build target in the project file:</span></span>
   * <span data-ttu-id="19262-180">Derleme taşır ve kullanıcı profilinin çalışma zamanı deposuna dosyaları simgeler.</span><span class="sxs-lookup"><span data-stu-id="19262-180">Moves the assembly and symbols files to the user profile's runtime store.</span></span>
   * <span data-ttu-id="19262-181">PowerShell komut dosyasını değiştirmek tetikler *StartupDiagnostics.deps.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="19262-181">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="19262-182">Taşır *StartupDiagnostics.deps.json* kullanıcı profilinin dosyasına `additionalDeps` klasör.</span><span class="sxs-lookup"><span data-stu-id="19262-182">Moves the *StartupDiagnostics.deps.json* file to the user profile's `additionalDeps` folder.</span></span>
3. <span data-ttu-id="19262-183">Ortam değişkenleri ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="19262-183">Set the environment variables:</span></span>
    * <span data-ttu-id="19262-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span><span class="sxs-lookup"><span data-stu-id="19262-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span></span>
    * <span data-ttu-id="19262-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span><span class="sxs-lookup"><span data-stu-id="19262-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span></span>
4. <span data-ttu-id="19262-186">Örnek uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="19262-186">Run the sample app.</span></span>
5. <span data-ttu-id="19262-187">İstek `/services` uygulamanın görmek için uç kaydedilen Hizmetleri.</span><span class="sxs-lookup"><span data-stu-id="19262-187">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="19262-188">İstek `/diag` tanılama bilgilerini görmek için uç nokta.</span><span class="sxs-lookup"><span data-stu-id="19262-188">Request the `/diag` endpoint to see the diagnostic information.</span></span>
