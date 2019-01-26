---
title: Razor dosya derlemeyi ve kod içinde ASP.NET Core ön derlemesi
author: rick-anderson
description: Razor dosyaları ve Razor dosyası ön derleme içinde ASP.NET Core uygulaması yerine getirmeyi önceden derleme avantajları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: 2720708f8e58fdc55b82bfb56665005170e79934
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889762"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="36d2f-103">ASP.NET Core Razor dosyası derleme</span><span class="sxs-lookup"><span data-stu-id="36d2f-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="36d2f-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="36d2f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="36d2f-105">İlişkili bir MVC görünümü çağrıldığında bir Razor dosya çalışma zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="36d2f-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="36d2f-106">Derleme zamanı Razor dosya yayımlama desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="36d2f-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="36d2f-107">Razor dosyaları isteğe bağlı olarak derlenmesi sırasında zaman yayımlama ve uygulama ile birlikte dağıtılan&mdash;ön derleme aracını kullanarak.</span><span class="sxs-lookup"><span data-stu-id="36d2f-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="36d2f-108">İlişkili bir Razor sayfası veya MVC görünümü çağrıldığında bir Razor dosya çalışma zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="36d2f-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="36d2f-109">Derleme zamanı Razor dosya yayımlama desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="36d2f-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="36d2f-110">Razor dosyaları isteğe bağlı olarak derlenmesi sırasında zaman yayımlama ve uygulama ile birlikte dağıtılan&mdash;ön derleme aracını kullanarak.</span><span class="sxs-lookup"><span data-stu-id="36d2f-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="36d2f-111">İlişkili bir Razor sayfası veya MVC görünümü çağrıldığında bir Razor dosya çalışma zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="36d2f-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="36d2f-112">Razor dosyaları her iki yapı derlenir ve saat kullanarak yayımlama [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="36d2f-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>

::: moniker-end

## <a name="precompilation-considerations"></a><span data-ttu-id="36d2f-113">Ön derleme konuları</span><span class="sxs-lookup"><span data-stu-id="36d2f-113">Precompilation considerations</span></span>

<span data-ttu-id="36d2f-114">Razor dosyaları önceden derleme yan etkileri verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="36d2f-114">The following are side effects of precompiling Razor files:</span></span>

* <span data-ttu-id="36d2f-115">Bir küçük yayımlanan paket</span><span class="sxs-lookup"><span data-stu-id="36d2f-115">A smaller published bundle</span></span>
* <span data-ttu-id="36d2f-116">Hızlı bir başlatma süresi</span><span class="sxs-lookup"><span data-stu-id="36d2f-116">A faster startup time</span></span>
* <span data-ttu-id="36d2f-117">Razor dosyaları düzenleyemezsiniz&mdash;ilişkili içeriği eksik yayımlanan paket öğesinden.</span><span class="sxs-lookup"><span data-stu-id="36d2f-117">You can't edit Razor files&mdash;the associated content is absent from the published bundle.</span></span>

## <a name="deploy-precompiled-files"></a><span data-ttu-id="36d2f-118">Önceden derlenmiş dosyaları dağıtma</span><span class="sxs-lookup"><span data-stu-id="36d2f-118">Deploy precompiled files</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="36d2f-119">Razor dosyaları derleme ve yayımlama-zamanı derlemesini Razor SDK'sı tarafından varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="36d2f-119">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="36d2f-120">Bunlar güncelleştirdikten sonra Razor dosyaları düzenleme, derleme sırasında desteklenir.</span><span class="sxs-lookup"><span data-stu-id="36d2f-120">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="36d2f-121">Varsayılan olarak, yalnızca derlenmiş *Views.dll* ve hiçbir *.cshtml* dosyaları, uygulamanızla birlikte dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="36d2f-121">By default, only the compiled *Views.dll* and no *.cshtml* files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="36d2f-122">ASP.NET Core 3. 0'ön derleme aracı kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="36d2f-122">The precompilation tool will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="36d2f-123">Geçiş öneririz [Razor Sdk](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="36d2f-123">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="36d2f-124">Yalnızca proje dosyasında hiçbir ön derleme özgü özellikler ayarlandığında Razor SDK'sı etkili olur.</span><span class="sxs-lookup"><span data-stu-id="36d2f-124">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="36d2f-125">Örneğin, ayarı *.csproj* dosyanın `MvcRazorCompileOnPublish` özelliğini `true` Razor SDK'yı devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="36d2f-125">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="36d2f-126">Projeniz .NET Framework hedefliyorsa, yükleme [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet paketi:</span><span class="sxs-lookup"><span data-stu-id="36d2f-126">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

<span data-ttu-id="36d2f-127">.NET Core projenizi olmasını istiyorsanız, hiçbir değişiklik gereklidir.</span><span class="sxs-lookup"><span data-stu-id="36d2f-127">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="36d2f-128">ASP.NET Core 2.x proje şablonları örtük olarak ayarlanır `MvcRazorCompileOnPublish` özelliğini `true` varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="36d2f-128">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="36d2f-129">Sonuç olarak, bu öğe güvenli bir şekilde alanından kaldırılabilir *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="36d2f-129">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="36d2f-130">ASP.NET Core 3. 0'ön derleme aracı kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="36d2f-130">The precompilation tool will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="36d2f-131">Geçiş öneririz [Razor Sdk](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="36d2f-131">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="36d2f-132">Razor dosyası ön derleme, gerçekleştirirken kullanılamıyorsa bir [müstakil dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="36d2f-132">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="36d2f-133">Ayarlama `MvcRazorCompileOnPublish` özelliğini `true`, yükleyip [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="36d2f-133">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="36d2f-134">Aşağıdaki *.csproj* örnek bu ayarları vurgular:</span><span class="sxs-lookup"><span data-stu-id="36d2f-134">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="36d2f-135">Uygulama için hazırlama bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd) ile [.NET Core CLI komutu yayımlama](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="36d2f-135">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="36d2f-136">Örneğin, proje kök dizinini aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="36d2f-136">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="36d2f-137">A *< project_name >. PrecompiledViews.dll* derlenmiş Razor dosyalarını içeren dosya, bir ön derleme başarılı olduğunda oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="36d2f-137">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="36d2f-138">Örneğin, aşağıdaki ekran içeriğini gösterir *Index.cshtml* içinde *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="36d2f-138">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![DLL içindeki Razor görünümleri](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="recompile-razor-files-on-change"></a><span data-ttu-id="36d2f-140">Razor dosyalarda değişiklik yeniden derleyin</span><span class="sxs-lookup"><span data-stu-id="36d2f-140">Recompile Razor files on change</span></span>

<span data-ttu-id="36d2f-141"><xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> `AllowRecompilingViewsOnFileChange` Razor dosyaları (Razor görünümleri ve Razor sayfaları) ve yeniden derlenen dosyalar diskte değiştirirseniz güncelleştirilmiş belirleyen bir değer alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="36d2f-141">The <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> `AllowRecompilingViewsOnFileChange` gets or sets a value that determines if Razor files (Razor views and Razor Pages) are recompiled and updated if files change on disk.</span></span>

<span data-ttu-id="36d2f-142">Ayarlandığında `true`, [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) gözcüler Razor dosyalarında değişiklikler için yapılandırılmış <xref:Microsoft.Extensions.FileProviders.IFileProvider> örnekleri.</span><span class="sxs-lookup"><span data-stu-id="36d2f-142">When set to `true`, [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) watches for changes to Razor files in configured <xref:Microsoft.Extensions.FileProviders.IFileProvider> instances.</span></span>

<span data-ttu-id="36d2f-143">Varsayılan değer `true` için:</span><span class="sxs-lookup"><span data-stu-id="36d2f-143">The default value is `true` for:</span></span>

* <span data-ttu-id="36d2f-144">ASP.NET Core 2.1 veya daha eski uygulamaları.</span><span class="sxs-lookup"><span data-stu-id="36d2f-144">ASP.NET Core 2.1 or earlier apps.</span></span>
* <span data-ttu-id="36d2f-145">ASP.NET Core 2.2 veya üzeri uygulamaları geliştirme ortamında.</span><span class="sxs-lookup"><span data-stu-id="36d2f-145">ASP.NET Core 2.2 or later apps in the Development environment.</span></span>

<span data-ttu-id="36d2f-146">`AllowRecompilingViewsOnFileChange` Uyumluluk anahtarıyla ilişkili olan ve uygulama için yapılandırılan uyumluluk sürümüne bağlı olarak farklı bir davranış sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="36d2f-146">`AllowRecompilingViewsOnFileChange` is associated with a compatibility switch and can provide different behavior depending on the configured compatibility version for the app.</span></span> <span data-ttu-id="36d2f-147">Ayarlayarak, uygulama yapılandırma `AllowRecompilingViewsOnFileChange` uygulamanın uyumluluk sürümü tarafından kapsanan değeri önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="36d2f-147">Configuring the app by setting `AllowRecompilingViewsOnFileChange` takes precedence over the value implied by the app's compatibility version.</span></span>

<span data-ttu-id="36d2f-148">Cihazın uyumluluk sürümü ayarlanırsa <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> veya daha önce `AllowRecompilingViewsOnFileChange` ayarlanır `true` açıkça şekilde yapılandırılmadıkça.</span><span class="sxs-lookup"><span data-stu-id="36d2f-148">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> or earlier, `AllowRecompilingViewsOnFileChange` is set to `true` unless explicitly configured.</span></span>

<span data-ttu-id="36d2f-149">Cihazın uyumluluk sürümü ayarlanırsa `CompatibilityVersion.Version_2_2` ya da sonraki `AllowRecompilingViewsOnFileChange` ayarlanır `false` değeri açıkça yapılandırılmış ya da geliştirme ortamıdır.</span><span class="sxs-lookup"><span data-stu-id="36d2f-149">If the app's compatibility version is set to `CompatibilityVersion.Version_2_2` or later, `AllowRecompilingViewsOnFileChange` is set to `false` unless the environment is Development or the value is explicitly configured.</span></span>

<span data-ttu-id="36d2f-150">Yönergeler ve cihazın uyumluluk sürümü ayarlama örnekleri için bkz. <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="36d2f-150">For guidance and examples of setting the app's compatibility version, see <xref:mvc/compatibility-version>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="36d2f-151">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="36d2f-151">Additional resources</span></span>

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
