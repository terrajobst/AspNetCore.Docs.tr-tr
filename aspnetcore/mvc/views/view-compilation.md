---
title: ASP.NET Core Razor dosyası derleme
author: rick-anderson
description: Razor dosyaları derleme içinde ASP.NET Core uygulaması nasıl gerçekleştirildiğini öğrenin.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/02/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: 11195f00e922f6817a0fa0988fad9d8082dea30a
ms.sourcegitcommit: a467828b5e4eaae291d961ffe2279a571900de23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58142328"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="92fe2-103">ASP.NET Core Razor dosyası derleme</span><span class="sxs-lookup"><span data-stu-id="92fe2-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="92fe2-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="92fe2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="92fe2-105">İlişkili bir MVC görünümü çağrıldığında bir Razor dosya çalışma zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="92fe2-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="92fe2-106">Derleme zamanı Razor dosya yayımlama desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="92fe2-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="92fe2-107">Razor dosyaları isteğe bağlı olarak derlenmesi sırasında zaman yayımlama ve uygulama ile birlikte dağıtılan&mdash;ön derleme aracını kullanarak.</span><span class="sxs-lookup"><span data-stu-id="92fe2-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="92fe2-108">İlişkili bir Razor sayfası veya MVC görünümü çağrıldığında bir Razor dosya çalışma zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="92fe2-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="92fe2-109">Derleme zamanı Razor dosya yayımlama desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="92fe2-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="92fe2-110">Razor dosyaları isteğe bağlı olarak derlenmesi sırasında zaman yayımlama ve uygulama ile birlikte dağıtılan&mdash;ön derleme aracını kullanarak.</span><span class="sxs-lookup"><span data-stu-id="92fe2-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="92fe2-111">İlişkili bir Razor sayfası veya MVC görünümü çağrıldığında bir Razor dosya çalışma zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="92fe2-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="92fe2-112">Razor dosyaları her iki yapı derlenir ve saat kullanarak yayımlama [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="92fe2-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="92fe2-113">Razor dosyaları her iki yapı derlenir ve saat kullanarak yayımlama [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="92fe2-113">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="92fe2-114">Çalışma zamanı derlemesi, isteğe bağlı olarak, uygulamanızın yapılandırarak etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="92fe2-114">Runtime compilation may be optionally enabled by configuring your application.</span></span>

::: moniker-end

## <a name="razor-compilation"></a><span data-ttu-id="92fe2-115">Razor derleme</span><span class="sxs-lookup"><span data-stu-id="92fe2-115">Razor compilation</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="92fe2-116">Razor dosyaları derleme ve yayımlama-zamanı derlemesini Razor SDK'sı tarafından varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="92fe2-116">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="92fe2-117">Etkinleştirildiğinde, çalışma zamanı derlemesi tamamlayıcı editied olmaları durumunda güncelleştirilecek Razor dosyaları izin vererek zamanında derleme oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="92fe2-117">When enabled, runtime compilation, will complement build time compilation allowing Razor files to be updated if they are editied.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="92fe2-118">Razor dosyaları derleme ve yayımlama-zamanı derlemesini Razor SDK'sı tarafından varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="92fe2-118">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="92fe2-119">Bunlar güncelleştirdikten sonra Razor dosyaları düzenleme, derleme sırasında desteklenir.</span><span class="sxs-lookup"><span data-stu-id="92fe2-119">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="92fe2-120">Varsayılan olarak, yalnızca derlenmiş *Views.dll* ve hiçbir *.cshtml* Razor dosyalarını derlemek için gerekli dosyaları veya başvuru derlemeleri, uygulamanızla birlikte dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="92fe2-120">By default, only the compiled *Views.dll* and no *.cshtml* files or references assemblies required to compile Razor files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="92fe2-121">Ön derleme araç kullanım dışı bırakıldı ve ASP.NET Core 3. 0'kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="92fe2-121">The precompilation tool has been deprecated, and will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="92fe2-122">Geçiş öneririz [Razor Sdk](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="92fe2-122">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="92fe2-123">Yalnızca proje dosyasında hiçbir ön derleme özgü özellikler ayarlandığında Razor SDK'sı etkili olur.</span><span class="sxs-lookup"><span data-stu-id="92fe2-123">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="92fe2-124">Örneğin, ayarı *.csproj* dosyanın `MvcRazorCompileOnPublish` özelliğini `true` Razor SDK'yı devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="92fe2-124">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="92fe2-125">Projeniz .NET Framework hedefliyorsa, yükleme [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet paketi:</span><span class="sxs-lookup"><span data-stu-id="92fe2-125">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

<span data-ttu-id="92fe2-126">.NET Core projenizi olmasını istiyorsanız, hiçbir değişiklik gereklidir.</span><span class="sxs-lookup"><span data-stu-id="92fe2-126">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="92fe2-127">ASP.NET Core 2.x proje şablonları örtük olarak ayarlanır `MvcRazorCompileOnPublish` özelliğini `true` varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="92fe2-127">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="92fe2-128">Sonuç olarak, bu öğe güvenli bir şekilde alanından kaldırılabilir *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="92fe2-128">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="92fe2-129">Ön derleme araç kullanım dışı bırakıldı ve ASP.NET Core 3. 0'kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="92fe2-129">The precompilation tool has been deprecated, and will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="92fe2-130">Geçiş öneririz [Razor Sdk](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="92fe2-130">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="92fe2-131">Razor dosyası ön derleme, gerçekleştirirken kullanılamıyorsa bir [müstakil dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="92fe2-131">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="92fe2-132">Ayarlama `MvcRazorCompileOnPublish` özelliğini `true`, yükleyip [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="92fe2-132">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="92fe2-133">Aşağıdaki *.csproj* örnek bu ayarları vurgular:</span><span class="sxs-lookup"><span data-stu-id="92fe2-133">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="92fe2-134">Uygulama için hazırlama bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd) ile [.NET Core CLI komutu yayımlama](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="92fe2-134">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="92fe2-135">Örneğin, proje kök dizinini aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="92fe2-135">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="92fe2-136">A  *\<project_name >. PrecompiledViews.dll* derlenmiş Razor dosyalarını içeren dosya, bir ön derleme başarılı olduğunda oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="92fe2-136">A *\<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="92fe2-137">Örneğin, aşağıdaki ekran içeriğini gösterir *Index.cshtml* içinde *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="92fe2-137">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![DLL içindeki Razor görünümleri](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="runtime-compilation"></a><span data-ttu-id="92fe2-139">Çalışma zamanı derlemesi</span><span class="sxs-lookup"><span data-stu-id="92fe2-139">Runtime compilation</span></span>

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="92fe2-140">Derleme zamanında derleme Razor dosyaları çalışma zamanı derlemesi tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="92fe2-140">Build-time compilation is supplemented by runtime compilation of Razor files.</span></span> <span data-ttu-id="92fe2-141">ASP.NET Core MVC yeniden derleyin Razor ne zaman dosya içeriğini bir *.cshtml* dosya değişikliği.</span><span class="sxs-lookup"><span data-stu-id="92fe2-141">ASP.NET Core MVC will recompile Razor files when the contents of a *.cshtml* file change.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="92fe2-142">Derleme zamanında derleme Razor dosyaları çalışma zamanı derlemesi tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="92fe2-142">Build-time compilation is supplemented by runtime compilation of Razor files.</span></span> <span data-ttu-id="92fe2-143"><xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> Razor dosyaları (Razor görünümleri ve Razor sayfaları) ve yeniden derlenen dosyalar diskte değiştirirseniz güncelleştirilmiş belirleyen bir değer alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="92fe2-143">The <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> gets or sets a value that determines if Razor files (Razor views and Razor Pages) are recompiled and updated if files change on disk.</span></span>

<span data-ttu-id="92fe2-144">Varsayılan değer `true` için:</span><span class="sxs-lookup"><span data-stu-id="92fe2-144">The default value is `true` for:</span></span>

* <span data-ttu-id="92fe2-145">Cihazın uyumluluk sürümü ayarlanırsa <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> veya önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="92fe2-145">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> or earlier</span></span>
* <span data-ttu-id="92fe2-146">Cihazın uyumluluk sürümü çok kümesi ayarlanmış olup olmadığını <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> veya üzeri ve uygulama geliştirme ortamında <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*>.</span><span class="sxs-lookup"><span data-stu-id="92fe2-146">If the app's compatibility version is set to set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> or later and the app is in the Development environment <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*>.</span></span> <span data-ttu-id="92fe2-147">Diğer bir deyişle, Razor dosyaları olmayan geliştirme ortamında sürece yeniden derlemeniz değil <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> açıkça ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="92fe2-147">In other words, Razor files would not recompile in non-Development environment unless <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> is explicitly set.</span></span>

<span data-ttu-id="92fe2-148">Yönergeler ve cihazın uyumluluk sürümü ayarlama örnekleri için bkz. <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="92fe2-148">For guidance and examples of setting the app's compatibility version, see <xref:mvc/compatibility-version>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="92fe2-149">Çalışma zamanı derlemesi kullanarak etkin `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation` paket.</span><span class="sxs-lookup"><span data-stu-id="92fe2-149">Runtime compilation is enabled using the `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation` package.</span></span> <span data-ttu-id="92fe2-150">Çalışma zamanı derlemesi etkinleştirmek için uygulamaları gerekir:</span><span class="sxs-lookup"><span data-stu-id="92fe2-150">To enable runtime compilation, apps must:</span></span>

* <span data-ttu-id="92fe2-151">Yükleme [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="92fe2-151">Install the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet package.</span></span>
* <span data-ttu-id="92fe2-152">Uygulamanın güncelleştirme `ConfigureServices` çağrısı içerecek şekilde `AddMvcRazorRuntimeCompilation`:</span><span class="sxs-lookup"><span data-stu-id="92fe2-152">Update the application's `ConfigureServices` to include a call to `AddMvcRazorRuntimeCompilation`:</span></span>

  ```csharp
  services
      .AddMvc()
      .AddRazorRuntimeCompilation()
  ```

<span data-ttu-id="92fe2-153">Çalışma zamanı derlemesi dağıtıldığında çalışacak şekilde ayarlamak için proje dosyaları ayrıca uygulamaları değiştirmelisiniz `PreserveCompilationReferences` için `true`.</span><span class="sxs-lookup"><span data-stu-id="92fe2-153">For runtime compilation to work when deployed, apps must additionally modify their project files to set the `PreserveCompilationReferences` to `true`.</span></span>

[!code-xml[](view-compilation/sample/RuntimeCompilation.csproj?highlight=3)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="92fe2-154">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="92fe2-154">Additional resources</span></span>

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
