---
title: ASP.NET Core 'de Razor dosyası derlemesi
author: rick-anderson
description: ASP.NET Core uygulamasında Razor dosyalarının derlemesini nasıl oluştuğunu öğrenin.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: cd096bba5eb580c0a606699a2bf7c36442fb56f7
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661107"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="0d0a1-103">ASP.NET Core 'de Razor dosyası derlemesi</span><span class="sxs-lookup"><span data-stu-id="0d0a1-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="0d0a1-104">Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0d0a1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="0d0a1-105">Bir Razor dosyası, ilişkili MVC görünümü çağrıldığında çalışma zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="0d0a1-106">Derleme zamanı Razor dosyası yayımlama desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="0d0a1-107">Razor dosyaları isteğe bağlı olarak yayımlama zamanında derlenebilir ve ön derleme Aracı kullanılarak uygulama&mdash;dağıtılabilir.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0d0a1-108">Razor dosyası, ilişkili Razor sayfası veya MVC görünümü çağrıldığında çalışma zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="0d0a1-109">Derleme zamanı Razor dosyası yayımlama desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="0d0a1-110">Razor dosyaları isteğe bağlı olarak yayımlama zamanında derlenebilir ve ön derleme Aracı kullanılarak uygulama&mdash;dağıtılabilir.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="0d0a1-111">Razor dosyası, ilişkili Razor sayfası veya MVC görünümü çağrıldığında çalışma zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="0d0a1-112">Razor dosyaları, [Razor SDK](xref:razor-pages/sdk)kullanılarak hem derlemede hem de yayımlama zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0d0a1-113">*. Cshtml* uzantılı Razor dosyaları, [Razor SDK](xref:razor-pages/sdk)kullanılarak hem derlemede hem de yayımlama zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-113">Razor files with a *.cshtml* extension are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="0d0a1-114">Çalışma zamanı derlemesi, uygulamanız yapılandırılarak isteğe bağlı olarak etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-114">Runtime compilation may be optionally enabled by configuring your application.</span></span>

::: moniker-end

## <a name="razor-compilation"></a><span data-ttu-id="0d0a1-115">Razor derleme</span><span class="sxs-lookup"><span data-stu-id="0d0a1-115">Razor compilation</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0d0a1-116">Razor dosyalarının derleme ve yayımlama zamanı derlemesi, Razor SDK tarafından varsayılan olarak etkinleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-116">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="0d0a1-117">Etkinleştirildiğinde, çalışma zamanı derlemesi derleme zamanı derlemesini tamamlar ve bunlar düzenlendiklerinde, Razor dosyalarının güncelleştirilmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-117">When enabled, runtime compilation complements build-time compilation, allowing Razor files to be updated if they are edited.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="0d0a1-118">Razor dosyalarının derleme ve yayımlama zamanı derlemesi, Razor SDK tarafından varsayılan olarak etkinleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-118">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="0d0a1-119">Razor dosyalarını güncelleştirildikten sonra düzenlediğinizde derleme zamanında desteklenir.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-119">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="0d0a1-120">Varsayılan olarak, yalnızca derlenen *views. dll* ve No *. cshtml* dosyaları ya da Razor dosyalarını derlemek için gerekli derlemeler, uygulamanız ile dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-120">By default, only the compiled *Views.dll* and no *.cshtml* files or references assemblies required to compile Razor files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0d0a1-121">Ön derleme aracı kullanım dışı bırakılmıştır ve ASP.NET Core 3,0 ' de kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-121">The precompilation tool has been deprecated, and will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="0d0a1-122">[Razor SDK](xref:razor-pages/sdk)'ya geçiş yapmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-122">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="0d0a1-123">Razor SDK yalnızca proje dosyasında ön derleme özgü hiçbir özellik ayarlanmamışsa etkilidir.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-123">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="0d0a1-124">Örneğin, *. csproj* dosyasının `MvcRazorCompileOnPublish` özelliğinin `true` olarak ayarlanması Razor SDK 'sını devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-124">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0d0a1-125">Projeniz .NET Framework hedefliyorsa [Microsoft. AspNetCore. Mvc. Razor. ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet paketini yükler:</span><span class="sxs-lookup"><span data-stu-id="0d0a1-125">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

<span data-ttu-id="0d0a1-126">Projeniz .NET Core 'u hedefliyorsa, hiçbir değişiklik yapılması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-126">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="0d0a1-127">ASP.NET Core 2. x proje şablonları, varsayılan olarak `MvcRazorCompileOnPublish` özelliğini `true` olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-127">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="0d0a1-128">Sonuç olarak, bu öğe *. csproj* dosyasından güvenle kaldırılabilir.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-128">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0d0a1-129">Ön derleme aracı kullanım dışı bırakılmıştır ve ASP.NET Core 3,0 ' de kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-129">The precompilation tool has been deprecated, and will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="0d0a1-130">[Razor SDK](xref:razor-pages/sdk)'ya geçiş yapmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-130">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="0d0a1-131">ASP.NET Core 2,0 ' de [otomatik olarak barındırılan bir dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) gerçekleştirilirken Razor dosyası ön derlemesi kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-131">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="0d0a1-132">`MvcRazorCompileOnPublish` özelliğini `true`olarak ayarlayın ve [Microsoft. AspNetCore. Mvc. Razor. ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet paketini yükler.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-132">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="0d0a1-133">Aşağıdaki *. csproj* örneği bu ayarları vurgular:</span><span class="sxs-lookup"><span data-stu-id="0d0a1-133">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="0d0a1-134">[.NET Core CLI Publish komutuyla](/dotnet/core/tools/dotnet-publish)uygulamayı [çerçeveye bağlı bir dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd) için hazırlayın.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-134">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="0d0a1-135">Örneğin, proje kökünde aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="0d0a1-135">For example, execute the following command at the project root:</span></span>

```dotnetcli
dotnet publish -c Release
```

<span data-ttu-id="0d0a1-136">Bir *\<project_name >. Derlenen Razor dosyalarını içeren PrecompiledViews. dll* dosyası, ön derleme başarılı olduğunda üretilir.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-136">A *\<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="0d0a1-137">Örneğin, aşağıdaki ekran görüntüsünde *WebApplication1. PrecompiledViews. dll*içindeki *Index. cshtml* 'nin içerikleri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="0d0a1-137">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![DLL içinde Razor görünümleri](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="runtime-compilation"></a><span data-ttu-id="0d0a1-139">Çalışma zamanı derlemesi</span><span class="sxs-lookup"><span data-stu-id="0d0a1-139">Runtime compilation</span></span>

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="0d0a1-140">Yapı zamanı derlemesi, Razor dosyalarının çalışma zamanı derlemesi tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-140">Build-time compilation is supplemented by runtime compilation of Razor files.</span></span> <span data-ttu-id="0d0a1-141">ASP.NET Core MVC, bir *. cshtml* dosyasının Içeriği değiştiğinde Razor dosyalarını yeniden derleyerek.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-141">ASP.NET Core MVC will recompile Razor files when the contents of a *.cshtml* file change.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="0d0a1-142">Yapı zamanı derlemesi, Razor dosyalarının çalışma zamanı derlemesi tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-142">Build-time compilation is supplemented by runtime compilation of Razor files.</span></span> <span data-ttu-id="0d0a1-143"><xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange>, Razor dosyalarının (Razor görünümleri ve Razor Pages) yeniden derlendiğini ve dosyaların diskte değiştirilmesi durumunda güncelleştirilip güncelleştirilmediğini belirleyen bir değer alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-143">The <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> gets or sets a value that determines if Razor files (Razor views and Razor Pages) are recompiled and updated if files change on disk.</span></span>

<span data-ttu-id="0d0a1-144">Varsayılan değer için `true`:</span><span class="sxs-lookup"><span data-stu-id="0d0a1-144">The default value is `true` for:</span></span>

* <span data-ttu-id="0d0a1-145">Uygulamanın uyumluluk sürümü <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> veya daha önceki bir sürüme ayarlandıysa</span><span class="sxs-lookup"><span data-stu-id="0d0a1-145">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> or earlier</span></span>
* <span data-ttu-id="0d0a1-146">Uygulamanın uyumluluk sürümü <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> veya üzeri olarak ayarlandıysa ve uygulama geliştirme ortamındaysanız <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*>.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-146">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> or later and the app is in the Development environment <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*>.</span></span> <span data-ttu-id="0d0a1-147">Diğer bir deyişle, <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> açıkça ayarlanmamışsa Razor dosyaları geliştirme dışı ortamda yeniden derlenmemiş.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-147">In other words, Razor files wouldn't recompile in non-Development environment unless <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> is explicitly set.</span></span>

<span data-ttu-id="0d0a1-148">Uygulamanın uyumluluk sürümünü ayarlamaya yönelik rehberlik ve örnekler için bkz. <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-148">For guidance and examples of setting the app's compatibility version, see <xref:mvc/compatibility-version>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0d0a1-149">Tüm ortamlar ve yapılandırma modları için çalışma zamanı derlemesini etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="0d0a1-149">To enable runtime compilation for all environments and configuration modes:</span></span>

1. <span data-ttu-id="0d0a1-150">[Microsoft. AspNetCore. Mvc. Razor. RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet paketini yükler.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-150">Install the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet package.</span></span>

1. <span data-ttu-id="0d0a1-151">Projenin `Startup.ConfigureServices` yöntemini `AddRazorRuntimeCompilation`çağrısı içerecek şekilde güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-151">Update the project's `Startup.ConfigureServices` method to include a call to `AddRazorRuntimeCompilation`.</span></span> <span data-ttu-id="0d0a1-152">Örnek:</span><span class="sxs-lookup"><span data-stu-id="0d0a1-152">For example:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddRazorPages()
            .AddRazorRuntimeCompilation();

        // code omitted for brevity
    }
    ```

### <a name="conditionally-enable-runtime-compilation"></a><span data-ttu-id="0d0a1-153">Çalışma zamanı derlemesini koşullu olarak etkinleştir</span><span class="sxs-lookup"><span data-stu-id="0d0a1-153">Conditionally enable runtime compilation</span></span>

<span data-ttu-id="0d0a1-154">Çalışma zamanı derlemesi, yalnızca yerel geliştirme için kullanılabilir olacak şekilde etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-154">Runtime compilation can be enabled such that it's only available for local development.</span></span> <span data-ttu-id="0d0a1-155">Bu şekilde koşullu etkinleştirme, yayımlanan çıktının olmasını sağlar:</span><span class="sxs-lookup"><span data-stu-id="0d0a1-155">Conditionally enabling in this manner ensures that the published output:</span></span>

* <span data-ttu-id="0d0a1-156">Derlenmiş görünümleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-156">Uses compiled views.</span></span>
* <span data-ttu-id="0d0a1-157">Boyutu küçüktür.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-157">Is smaller in size.</span></span>
* <span data-ttu-id="0d0a1-158">, İzleyicileri dosyasını üretimde etkinleştirmez.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-158">Doesn't enable file watchers in production.</span></span>

<span data-ttu-id="0d0a1-159">Çalışma zamanı derlemesini ortam ve yapılandırma moduna göre etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="0d0a1-159">To enable runtime compilation based on the environment and configuration mode:</span></span>

1. <span data-ttu-id="0d0a1-160">Etkin `Configuration` değerini temel alarak [Microsoft. AspNetCore. Mvc. Razor. RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) paketine koşullu olarak başvurun:</span><span class="sxs-lookup"><span data-stu-id="0d0a1-160">Conditionally reference the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) package based on the active `Configuration` value:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation" Version="3.1.0" Condition="'$(Configuration)' == 'Debug'" />
    ```

1. <span data-ttu-id="0d0a1-161">Projenin `Startup.ConfigureServices` yöntemini `AddRazorRuntimeCompilation`çağrısı içerecek şekilde güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="0d0a1-161">Update the project's `Startup.ConfigureServices` method to include a call to `AddRazorRuntimeCompilation`.</span></span> <span data-ttu-id="0d0a1-162">`AddRazorRuntimeCompilation`, yalnızca `ASPNETCORE_ENVIRONMENT` değişkeni `Development`olarak ayarlandığında hata ayıklama modunda çalışacak şekilde koşullu olarak yürütün:</span><span class="sxs-lookup"><span data-stu-id="0d0a1-162">Conditionally execute `AddRazorRuntimeCompilation` such that it only runs in Debug mode when the `ASPNETCORE_ENVIRONMENT` variable is set to `Development`:</span></span>

    ```csharp
    public IWebHostEnvironment Env { get; set; }

    public void ConfigureServices(IServiceCollection services)
    {
        IMvcBuilder builder = services.AddRazorPages();

    #if DEBUG
        if (Env.IsDevelopment())
        {
            builder.AddRazorRuntimeCompilation();
        }
    #endif

        // code omitted for brevity
    }
    ```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="0d0a1-163">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="0d0a1-163">Additional resources</span></span>

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
