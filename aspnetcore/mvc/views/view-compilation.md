---
title: Razor dosya derleme ve ASP.NET Core içinde ön derlemesi
author: rick-anderson
description: Razor dosyaları ve ASP.NET Core uygulamasında Razor dosyası ön derlemesi yüklemenin nasıl yapılacağını önceden derleme avantajları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: mvc/views/view-compilation
ms.openlocfilehash: 6ef450a24f57c721021f77f6df5088574caa2645
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274046"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="9164f-103">ASP.NET Core Razor dosyası derleme</span><span class="sxs-lookup"><span data-stu-id="9164f-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="9164f-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9164f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"
<span data-ttu-id="9164f-105">İlişkili MVC görünümü çağrıldığında bir Razor dosya çalışma zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="9164f-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="9164f-106">Derleme zamanı Razor dosya yayımlama desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="9164f-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="9164f-107">Razor dosyaları isteğe bağlı olarak derlenmesi sırasında zaman yayımlama ve uygulama ile&mdash;ön derleme aracını kullanarak.</span><span class="sxs-lookup"><span data-stu-id="9164f-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>
::: moniker-end
::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="9164f-108">İlişkili Razor sayfasını veya MVC görünümü çağrıldığında bir Razor dosya çalışma zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="9164f-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="9164f-109">Derleme zamanı Razor dosya yayımlama desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="9164f-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="9164f-110">Razor dosyaları isteğe bağlı olarak derlenmesi sırasında zaman yayımlama ve uygulama ile&mdash;ön derleme aracını kullanarak.</span><span class="sxs-lookup"><span data-stu-id="9164f-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="9164f-111">İlişkili Razor sayfasını veya MVC görünümü çağrıldığında bir Razor dosya çalışma zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="9164f-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="9164f-112">Razor dosyaları her iki derleme sırasında derlenir ve zaman kullanarak yayımlamak [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="9164f-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>
::: moniker-end

## <a name="precompilation-considerations"></a><span data-ttu-id="9164f-113">Ön derleme dikkat edilecek noktalar</span><span class="sxs-lookup"><span data-stu-id="9164f-113">Precompilation considerations</span></span>

<span data-ttu-id="9164f-114">Razor dosyaları önceden derleme yan etkileri verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="9164f-114">The following are side effects of precompiling Razor files:</span></span>

* <span data-ttu-id="9164f-115">Bir küçük yayımlanan paket</span><span class="sxs-lookup"><span data-stu-id="9164f-115">A smaller published bundle</span></span>
* <span data-ttu-id="9164f-116">Daha hızlı bir başlangıç zamanı</span><span class="sxs-lookup"><span data-stu-id="9164f-116">A faster startup time</span></span>
* <span data-ttu-id="9164f-117">Razor dosyalarını düzenleyemezsiniz&mdash;ilişkili içeriği eksik yayımlanan paket gelen.</span><span class="sxs-lookup"><span data-stu-id="9164f-117">You can't edit Razor files&mdash;the associated content is absent from the published bundle.</span></span>

## <a name="deploy-precompiled-files"></a><span data-ttu-id="9164f-118">Önceden derlenmiş dosyaları dağıtma</span><span class="sxs-lookup"><span data-stu-id="9164f-118">Deploy precompiled files</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="9164f-119">Razor dosyaları oluşturma ve yayımlama-zamanı derlenmesini Razor SDK'sı tarafından varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="9164f-119">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="9164f-120">Bunlar güncelleştirdikten sonra Razor dosyaları düzenleme derleme zamanında desteklenir.</span><span class="sxs-lookup"><span data-stu-id="9164f-120">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="9164f-121">Varsayılan olarak, yalnızca derlenmiş *Views.dll* ve hiçbir *.cshtml* dosyaları ile uygulamanızı dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="9164f-121">By default, only the compiled *Views.dll* and no *.cshtml* files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9164f-122">Yalnızca ön derlemesi özgü özellikleri proje dosyasında ayarlandığında Razor SDK'sı etkili olur.</span><span class="sxs-lookup"><span data-stu-id="9164f-122">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="9164f-123">Örneğin, ayarı *.csproj* dosyanın `MvcRazorCompileOnPublish` özelliğine `true` Razor SDK'yı devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="9164f-123">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="9164f-124">Projeniz .NET Framework hedefliyorsa, yükleme [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet paketi:</span><span class="sxs-lookup"><span data-stu-id="9164f-124">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

<span data-ttu-id="9164f-125">[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]</span><span class="sxs-lookup"><span data-stu-id="9164f-125">[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]</span></span>

<span data-ttu-id="9164f-126">Projeniz .NET Core hedefliyorsa, hiçbir değişiklik gereklidir.</span><span class="sxs-lookup"><span data-stu-id="9164f-126">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="9164f-127">ASP.NET Core 2.x proje şablonları örtük olarak ayarlamak `MvcRazorCompileOnPublish` özelliğine `true` varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="9164f-127">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="9164f-128">Sonuç olarak, bu öğe güvenle alanından kaldırılabilir *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="9164f-128">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9164f-129">Razor dosyası ön derlemesi, gerçekleştirirken kullanılamıyorsa bir [müstakil dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="9164f-129">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>
::: moniker-end

::: moniker range="= aspnetcore-1.1"
<span data-ttu-id="9164f-130">Ayarlama `MvcRazorCompileOnPublish` özelliğine `true`, yükleyip [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="9164f-130">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="9164f-131">Aşağıdaki *.csproj* örnek bu ayarları vurgular:</span><span class="sxs-lookup"><span data-stu-id="9164f-131">The following *.csproj* sample highlights these settings:</span></span>

<span data-ttu-id="9164f-132">[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]</span><span class="sxs-lookup"><span data-stu-id="9164f-132">[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]</span></span>
::: moniker-end

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="9164f-133">Uygulama için hazırlama bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd) ile [.NET Core CLI yayımlama komutu](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="9164f-133">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="9164f-134">Örneğin, proje kökündeki aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="9164f-134">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="9164f-135">A *< project_name >. PrecompiledViews.dll* derlenmiş Razor dosyalarını içeren dosya ön derlemesi başarılı olduğunda oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9164f-135">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="9164f-136">Örneğin, aşağıdaki ekran görüntüsünde içeriğini gösterir *Index.cshtml* içinde *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="9164f-136">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![DLL içindeki Razor görünümleri](view-compilation/_static/razor-views-in-dll.png)
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="9164f-138">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="9164f-138">Additional resources</span></span>

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