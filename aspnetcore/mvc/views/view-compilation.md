---
title: ASP.NET Core 'de Razor dosyası derlemesi
author: rick-anderson
description: ASP.NET Core uygulamasında Razor dosyalarının derlemesini nasıl oluştuğunu öğrenin.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/20/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: 0aa632bce32ef44f65d92639284c64c1d00e952e
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080820"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="e598b-103">ASP.NET Core 'de Razor dosyası derlemesi</span><span class="sxs-lookup"><span data-stu-id="e598b-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="e598b-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e598b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="e598b-105">Bir Razor dosyası, ilişkili MVC görünümü çağrıldığında çalışma zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="e598b-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="e598b-106">Derleme zamanı Razor dosyası yayımlama desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="e598b-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="e598b-107">Razor dosyaları isteğe bağlı olarak yayımlama zamanında derlenebilir ve ön derleme Aracı kullanılarak&mdash;uygulamayla birlikte dağıtılabilir.</span><span class="sxs-lookup"><span data-stu-id="e598b-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="e598b-108">Razor dosyası, ilişkili Razor sayfası veya MVC görünümü çağrıldığında çalışma zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="e598b-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="e598b-109">Derleme zamanı Razor dosyası yayımlama desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="e598b-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="e598b-110">Razor dosyaları isteğe bağlı olarak yayımlama zamanında derlenebilir ve ön derleme Aracı kullanılarak&mdash;uygulamayla birlikte dağıtılabilir.</span><span class="sxs-lookup"><span data-stu-id="e598b-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="e598b-111">Razor dosyası, ilişkili Razor sayfası veya MVC görünümü çağrıldığında çalışma zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="e598b-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="e598b-112">Razor dosyaları, [Razor SDK](xref:razor-pages/sdk)kullanılarak hem derlemede hem de yayımlama zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="e598b-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e598b-113">Razor dosyaları, [Razor SDK](xref:razor-pages/sdk)kullanılarak hem derlemede hem de yayımlama zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="e598b-113">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="e598b-114">Çalışma zamanı derlemesi, uygulamanız yapılandırılarak isteğe bağlı olarak etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="e598b-114">Runtime compilation may be optionally enabled by configuring your application.</span></span>

::: moniker-end

## <a name="razor-compilation"></a><span data-ttu-id="e598b-115">Razor derleme</span><span class="sxs-lookup"><span data-stu-id="e598b-115">Razor compilation</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="e598b-116">Razor dosyalarının derleme ve yayımlama zamanı derlemesi, Razor SDK tarafından varsayılan olarak etkinleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="e598b-116">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="e598b-117">Etkinleştirildiğinde, çalışma zamanı derlemesi derleme zamanı derlemesini tamamlar ve bunlar düzenlendiklerinde, Razor dosyalarının güncelleştirilmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="e598b-117">When enabled, runtime compilation complements build-time compilation, allowing Razor files to be updated if they are edited.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="e598b-118">Razor dosyalarının derleme ve yayımlama zamanı derlemesi, Razor SDK tarafından varsayılan olarak etkinleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="e598b-118">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="e598b-119">Razor dosyalarını güncelleştirildikten sonra düzenlediğinizde derleme zamanında desteklenir.</span><span class="sxs-lookup"><span data-stu-id="e598b-119">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="e598b-120">Varsayılan olarak, yalnızca derlenen *views. dll* ve No *. cshtml* dosyaları ya da Razor dosyalarını derlemek için gerekli derlemeler, uygulamanız ile dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="e598b-120">By default, only the compiled *Views.dll* and no *.cshtml* files or references assemblies required to compile Razor files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e598b-121">Ön derleme aracı kullanım dışı bırakılmıştır ve ASP.NET Core 3,0 ' de kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="e598b-121">The precompilation tool has been deprecated, and will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="e598b-122">[Razor SDK](xref:razor-pages/sdk)'ya geçiş yapmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="e598b-122">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="e598b-123">Razor SDK yalnızca proje dosyasında ön derleme özgü hiçbir özellik ayarlanmamışsa etkilidir.</span><span class="sxs-lookup"><span data-stu-id="e598b-123">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="e598b-124">Örneğin, *. csproj* dosyasının `MvcRazorCompileOnPublish` özelliği Razor SDK 'sını devre dışı `true` bırakacak şekilde ayarlanıyor.</span><span class="sxs-lookup"><span data-stu-id="e598b-124">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="e598b-125">Projeniz .NET Framework hedefliyorsa [Microsoft. AspNetCore. Mvc. Razor. ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet paketini yükler:</span><span class="sxs-lookup"><span data-stu-id="e598b-125">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

<span data-ttu-id="e598b-126">Projeniz .NET Core 'u hedefliyorsa, hiçbir değişiklik yapılması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="e598b-126">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="e598b-127">ASP.NET Core 2. x proje şablonları `MvcRazorCompileOnPublish` `true` özelliği varsayılan olarak olarak olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="e598b-127">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="e598b-128">Sonuç olarak, bu öğe *. csproj* dosyasından güvenle kaldırılabilir.</span><span class="sxs-lookup"><span data-stu-id="e598b-128">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e598b-129">Ön derleme aracı kullanım dışı bırakılmıştır ve ASP.NET Core 3,0 ' de kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="e598b-129">The precompilation tool has been deprecated, and will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="e598b-130">[Razor SDK](xref:razor-pages/sdk)'ya geçiş yapmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="e598b-130">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="e598b-131">ASP.NET Core 2,0 ' de [otomatik olarak barındırılan bir dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) gerçekleştirilirken Razor dosyası ön derlemesi kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="e598b-131">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="e598b-132">Özelliğini olarak `true`ayarlayın ve [Microsoft. aspnetcore. Mvc. Razor. viewcompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet paketini yükler. `MvcRazorCompileOnPublish`</span><span class="sxs-lookup"><span data-stu-id="e598b-132">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="e598b-133">Aşağıdaki *. csproj* örneği bu ayarları vurgular:</span><span class="sxs-lookup"><span data-stu-id="e598b-133">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="e598b-134">[.NET Core CLI Publish komutuyla](/dotnet/core/tools/dotnet-publish)uygulamayı [çerçeveye bağlı bir dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd) için hazırlayın.</span><span class="sxs-lookup"><span data-stu-id="e598b-134">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="e598b-135">Örneğin, proje kökünde aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="e598b-135">For example, execute the following command at the project root:</span></span>

```dotnetcli
dotnet publish -c Release
```

<span data-ttu-id="e598b-136">*Bir\<Project_Name >. Derlenen Razor dosyalarını içeren PrecompiledViews. dll* dosyası, ön derleme başarılı olduğunda üretilir.</span><span class="sxs-lookup"><span data-stu-id="e598b-136">A *\<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="e598b-137">Örneğin, aşağıdaki ekran görüntüsünde *WebApplication1. PrecompiledViews. dll*içindeki *Index. cshtml* 'nin içerikleri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="e598b-137">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![DLL içinde Razor görünümleri](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="runtime-compilation"></a><span data-ttu-id="e598b-139">Çalışma zamanı derlemesi</span><span class="sxs-lookup"><span data-stu-id="e598b-139">Runtime compilation</span></span>

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="e598b-140">Yapı zamanı derlemesi, Razor dosyalarının çalışma zamanı derlemesi tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="e598b-140">Build-time compilation is supplemented by runtime compilation of Razor files.</span></span> <span data-ttu-id="e598b-141">ASP.NET Core MVC, bir *. cshtml* dosyasının Içeriği değiştiğinde Razor dosyalarını yeniden derleyerek.</span><span class="sxs-lookup"><span data-stu-id="e598b-141">ASP.NET Core MVC will recompile Razor files when the contents of a *.cshtml* file change.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="e598b-142">Yapı zamanı derlemesi, Razor dosyalarının çalışma zamanı derlemesi tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="e598b-142">Build-time compilation is supplemented by runtime compilation of Razor files.</span></span> <span data-ttu-id="e598b-143"><xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> ,<xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> Razor dosyalarının (Razor görünümleri ve Razor Pages) yeniden derlendiğini ve dosyaların diskte değiştirilmesi durumunda güncelleştirilip güncelleştirilmediğini belirleyen bir değer alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="e598b-143">The <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> gets or sets a value that determines if Razor files (Razor views and Razor Pages) are recompiled and updated if files change on disk.</span></span>

<span data-ttu-id="e598b-144">Varsayılan değer `true` :</span><span class="sxs-lookup"><span data-stu-id="e598b-144">The default value is `true` for:</span></span>

* <span data-ttu-id="e598b-145">Uygulamanın uyumluluk sürümü <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> veya daha önceki bir sürüme ayarlandıysa</span><span class="sxs-lookup"><span data-stu-id="e598b-145">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> or earlier</span></span>
* <span data-ttu-id="e598b-146">Uygulamanın uyumluluk sürümü veya üzeri olarak <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> ayarlandıysa ve uygulama geliştirme ortamındaysanız. <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*></span><span class="sxs-lookup"><span data-stu-id="e598b-146">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> or later and the app is in the Development environment <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*>.</span></span> <span data-ttu-id="e598b-147">Diğer bir deyişle, açıkça ayarlanmamışsa Razor dosyaları geliştirme dışı ortamda <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> yeniden derlenmez.</span><span class="sxs-lookup"><span data-stu-id="e598b-147">In other words, Razor files would not recompile in non-Development environment unless <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> is explicitly set.</span></span>

<span data-ttu-id="e598b-148">Uygulamanın uyumluluk sürümünü ayarlamaya yönelik rehberlik ve örnekler için bkz <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="e598b-148">For guidance and examples of setting the app's compatibility version, see <xref:mvc/compatibility-version>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e598b-149">Çalışma zamanı derlemesi `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation` paket kullanılarak etkinleştirildi.</span><span class="sxs-lookup"><span data-stu-id="e598b-149">Runtime compilation is enabled using the `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation` package.</span></span> <span data-ttu-id="e598b-150">Çalışma zamanı derlemesini etkinleştirmek için uygulamalar şu şekilde olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="e598b-150">To enable runtime compilation, apps must:</span></span>

* <span data-ttu-id="e598b-151">[Microsoft. AspNetCore. Mvc. Razor. RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet paketini yükler.</span><span class="sxs-lookup"><span data-stu-id="e598b-151">Install the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet package.</span></span>
* <span data-ttu-id="e598b-152">Projenin `Startup.ConfigureServices` metodunu bir `AddRazorRuntimeCompilation`çağrı içerecek şekilde güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="e598b-152">Update the project's `Startup.ConfigureServices` method to include a call to `AddRazorRuntimeCompilation`:</span></span>

  ```csharp
  services
      .AddControllersWithViews()
      .AddRazorRuntimeCompilation();
  ```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="e598b-153">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="e598b-153">Additional resources</span></span>

::: moniker range="= aspnetcore-1.1"

* <xref:mvc/views/overview>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end
