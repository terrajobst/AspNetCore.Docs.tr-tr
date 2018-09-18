---
title: Razor dosya derlemeyi ve kod içinde ASP.NET Core ön derlemesi
author: rick-anderson
description: Razor dosyaları ve Razor dosyası ön derleme içinde ASP.NET Core uygulaması yerine getirmeyi önceden derleme avantajları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: mvc/views/view-compilation
ms.openlocfilehash: f5888cf43d8d8192acedaa33b3fa0f313737fc9b
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011293"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="b0308-103">ASP.NET Core Razor dosyası derleme</span><span class="sxs-lookup"><span data-stu-id="b0308-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="b0308-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b0308-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="b0308-105">İlişkili bir MVC görünümü çağrıldığında bir Razor dosya çalışma zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="b0308-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="b0308-106">Derleme zamanı Razor dosya yayımlama desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="b0308-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="b0308-107">Razor dosyaları isteğe bağlı olarak derlenmesi sırasında zaman yayımlama ve uygulama ile birlikte dağıtılan&mdash;ön derleme aracını kullanarak.</span><span class="sxs-lookup"><span data-stu-id="b0308-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="b0308-108">İlişkili bir Razor sayfası veya MVC görünümü çağrıldığında bir Razor dosya çalışma zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="b0308-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="b0308-109">Derleme zamanı Razor dosya yayımlama desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="b0308-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="b0308-110">Razor dosyaları isteğe bağlı olarak derlenmesi sırasında zaman yayımlama ve uygulama ile birlikte dağıtılan&mdash;ön derleme aracını kullanarak.</span><span class="sxs-lookup"><span data-stu-id="b0308-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b0308-111">İlişkili bir Razor sayfası veya MVC görünümü çağrıldığında bir Razor dosya çalışma zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="b0308-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="b0308-112">Razor dosyaları her iki yapı derlenir ve saat kullanarak yayımlama [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="b0308-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>

::: moniker-end

## <a name="precompilation-considerations"></a><span data-ttu-id="b0308-113">Ön derleme konuları</span><span class="sxs-lookup"><span data-stu-id="b0308-113">Precompilation considerations</span></span>

<span data-ttu-id="b0308-114">Razor dosyaları önceden derleme yan etkileri verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="b0308-114">The following are side effects of precompiling Razor files:</span></span>

* <span data-ttu-id="b0308-115">Bir küçük yayımlanan paket</span><span class="sxs-lookup"><span data-stu-id="b0308-115">A smaller published bundle</span></span>
* <span data-ttu-id="b0308-116">Hızlı bir başlatma süresi</span><span class="sxs-lookup"><span data-stu-id="b0308-116">A faster startup time</span></span>
* <span data-ttu-id="b0308-117">Razor dosyaları düzenleyemezsiniz&mdash;ilişkili içeriği eksik yayımlanan paket öğesinden.</span><span class="sxs-lookup"><span data-stu-id="b0308-117">You can't edit Razor files&mdash;the associated content is absent from the published bundle.</span></span>

## <a name="deploy-precompiled-files"></a><span data-ttu-id="b0308-118">Önceden derlenmiş dosyaları dağıtma</span><span class="sxs-lookup"><span data-stu-id="b0308-118">Deploy precompiled files</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b0308-119">Razor dosyaları derleme ve yayımlama-zamanı derlemesini Razor SDK'sı tarafından varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="b0308-119">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="b0308-120">Bunlar güncelleştirdikten sonra Razor dosyaları düzenleme, derleme sırasında desteklenir.</span><span class="sxs-lookup"><span data-stu-id="b0308-120">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="b0308-121">Varsayılan olarak, yalnızca derlenmiş *Views.dll* ve hiçbir *.cshtml* dosyaları, uygulamanızla birlikte dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="b0308-121">By default, only the compiled *Views.dll* and no *.cshtml* files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b0308-122">ASP.NET Core 3. 0'ön derleme aracı kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="b0308-122">The precompilation tool will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="b0308-123">Geçiş öneririz [Razor Sdk](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="b0308-123">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="b0308-124">Yalnızca proje dosyasında hiçbir ön derleme özgü özellikler ayarlandığında Razor SDK'sı etkili olur.</span><span class="sxs-lookup"><span data-stu-id="b0308-124">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="b0308-125">Örneğin, ayarı *.csproj* dosyanın `MvcRazorCompileOnPublish` özelliğini `true` Razor SDK'yı devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="b0308-125">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="b0308-126">Projeniz .NET Framework hedefliyorsa, yükleme [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet paketi:</span><span class="sxs-lookup"><span data-stu-id="b0308-126">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

<span data-ttu-id="b0308-127">.NET Core projenizi olmasını istiyorsanız, hiçbir değişiklik gereklidir.</span><span class="sxs-lookup"><span data-stu-id="b0308-127">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="b0308-128">ASP.NET Core 2.x proje şablonları örtük olarak ayarlanır `MvcRazorCompileOnPublish` özelliğini `true` varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="b0308-128">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="b0308-129">Sonuç olarak, bu öğe güvenli bir şekilde alanından kaldırılabilir *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="b0308-129">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b0308-130">ASP.NET Core 3. 0'ön derleme aracı kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="b0308-130">The precompilation tool will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="b0308-131">Geçiş öneririz [Razor Sdk](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="b0308-131">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="b0308-132">Razor dosyası ön derleme, gerçekleştirirken kullanılamıyorsa bir [müstakil dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="b0308-132">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="b0308-133">Ayarlama `MvcRazorCompileOnPublish` özelliğini `true`, yükleyip [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="b0308-133">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="b0308-134">Aşağıdaki *.csproj* örnek bu ayarları vurgular:</span><span class="sxs-lookup"><span data-stu-id="b0308-134">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="b0308-135">Uygulama için hazırlama bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd) ile [.NET Core CLI komutu yayımlama](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="b0308-135">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="b0308-136">Örneğin, proje kök dizinini aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="b0308-136">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="b0308-137">A *< project_name >. PrecompiledViews.dll* derlenmiş Razor dosyalarını içeren dosya, bir ön derleme başarılı olduğunda oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b0308-137">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="b0308-138">Örneğin, aşağıdaki ekran içeriğini gösterir *Index.cshtml* içinde *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="b0308-138">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![DLL içindeki Razor görünümleri](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="b0308-140">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b0308-140">Additional resources</span></span>

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
