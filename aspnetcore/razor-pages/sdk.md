---
title: ASP.NET Core Razor SDK'sı
author: Rick-Anderson
description: Nasıl ASP.NET Core Razor sayfalar kodlama sayfa odaklı senaryolar daha kolay ve MVC kullanmaktan daha üretken hale getirdiğini öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/18/2019
uid: razor-pages/sdk
ms.openlocfilehash: fa69e4840377e0c1c8291c7ba9305a27bd3e6b82
ms.sourcegitcommit: 516f166c5f7cec54edf3d9c71e6e2ba53fb3b0e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67196371"
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="42984-103">ASP.NET Core Razor SDK'sı</span><span class="sxs-lookup"><span data-stu-id="42984-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="42984-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="42984-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="overview"></a><span data-ttu-id="42984-105">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="42984-105">Overview</span></span>

<span data-ttu-id="42984-106">[!INCLUDE[](~/includes/2.1-SDK.md)] İçerir `Microsoft.NET.Sdk.Razor` MSBuild SDK'sı (Razor SDK).</span><span class="sxs-lookup"><span data-stu-id="42984-106">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="42984-107">Razor SDK:</span><span class="sxs-lookup"><span data-stu-id="42984-107">The Razor SDK:</span></span>

* <span data-ttu-id="42984-108">Oluşturma, paketleme ve içeren proje yayımlama deneyimi standartlaştırır [Razor](xref:mvc/views/razor) dosyaları ASP.NET Core MVC tabanlı projeler için.</span><span class="sxs-lookup"><span data-stu-id="42984-108">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="42984-109">Bir dizi önceden tanımlanmış hedefleri, özellikler ve Razor dosyaları derleme özelleştirme izin veren öğeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="42984-109">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="42984-110">Razor SDK'sını içeren bir `<Content>` öğesi ile bir `Include` özniteliğini `**\*.cshtml` Glob deseni.</span><span class="sxs-lookup"><span data-stu-id="42984-110">The Razor SDK includes a `<Content>` element with an `Include` attribute set to the `**\*.cshtml` globbing pattern.</span></span> <span data-ttu-id="42984-111">Eşleşen dosyaları yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="42984-111">Matching files are published.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="42984-112">Razor SDK'sı içerir `<Content>` öğelerle `Include` öznitelikleri kümesine `**\*.cshtml` ve `**\*.razor` Glob desenlerinin.</span><span class="sxs-lookup"><span data-stu-id="42984-112">The Razor SDK includes `<Content>` elements with `Include` attributes set to the `**\*.cshtml` and `**\*.razor` globbing patterns.</span></span> <span data-ttu-id="42984-113">Eşleşen dosyaları yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="42984-113">Matching files are published.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="42984-114">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="42984-114">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a><span data-ttu-id="42984-115">Razor SDK'sını kullanma</span><span class="sxs-lookup"><span data-stu-id="42984-115">Use the Razor SDK</span></span>

<span data-ttu-id="42984-116">Çoğu web uygulaması açıkça Razor SDK'ya başvurmak için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="42984-116">Most web apps aren't required to explicitly reference the Razor SDK.</span></span>

<span data-ttu-id="42984-117">Razor görünümleri ya da Razor sayfaları içeren sınıf kitaplıkları oluşturmak için Razor SDK'sını kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="42984-117">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="42984-118">Kullanım `Microsoft.NET.Sdk.Razor` yerine `Microsoft.NET.Sdk`:</span><span class="sxs-lookup"><span data-stu-id="42984-118">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* <span data-ttu-id="42984-119">Genellikle, bir paket başvurusu `Microsoft.AspNetCore.Mvc` oluşturmak ve Razor sayfaları ve Razor görünümleri derlemek için gereken ek bağımlılıklar almak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="42984-119">Typically, a package reference to `Microsoft.AspNetCore.Mvc` is required to receive additional dependencies that are required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="42984-120">En azından, paket başvuruları projenize eklemeniz gerekir:</span><span class="sxs-lookup"><span data-stu-id="42984-120">At a minimum, your project should add package references to:</span></span>

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  <span data-ttu-id="42984-121">`Microsoft.AspNetCore.Razor.Design` Paket proje için Razor derleme görevleri ve hedefleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="42984-121">The `Microsoft.AspNetCore.Razor.Design` package provides the Razor compilation tasks and targets for the project.</span></span>

  <span data-ttu-id="42984-122">Yukarıdaki paketleri dahil `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="42984-122">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="42984-123">Aşağıdaki biçimlendirmede Razor dosyaları için bir ASP.NET Core Razor sayfalar uygulama oluşturmak için Razor SDK'sını kullanan bir proje dosyasını göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="42984-123">The following markup shows a project file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> <span data-ttu-id="42984-124">`Microsoft.AspNetCore.Razor.Design` Ve `Microsoft.AspNetCore.Mvc.Razor.Extensions` paketleri dahil edilecek [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="42984-124">The `Microsoft.AspNetCore.Razor.Design` and `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="42984-125">Ancak, daha az sürüm `Microsoft.AspNetCore.App` paket başvuru sağlayan bir metapackage en son sürümünü içermeyen App `Microsoft.AspNetCore.Razor.Design`.</span><span class="sxs-lookup"><span data-stu-id="42984-125">However, the version-less `Microsoft.AspNetCore.App` package reference provides a metapackage to the app that doesn't include the latest version of `Microsoft.AspNetCore.Razor.Design`.</span></span> <span data-ttu-id="42984-126">Projeleri sürümünün tutarlı olmasını başvurmalıdır `Microsoft.AspNetCore.Razor.Design` (veya `Microsoft.AspNetCore.Mvc`) böylece Razor yönelik en son derleme zamanı düzeltmeleri dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="42984-126">Projects must reference a consistent version of `Microsoft.AspNetCore.Razor.Design` (or `Microsoft.AspNetCore.Mvc`) so that the latest build-time fixes for Razor are included.</span></span> <span data-ttu-id="42984-127">Daha fazla bilgi için [bu GitHub sorunu](https://github.com/aspnet/Razor/issues/2553).</span><span class="sxs-lookup"><span data-stu-id="42984-127">For more information, see [this GitHub issue](https://github.com/aspnet/Razor/issues/2553).</span></span>

::: moniker-end

### <a name="properties"></a><span data-ttu-id="42984-128">Özellikler</span><span class="sxs-lookup"><span data-stu-id="42984-128">Properties</span></span>

<span data-ttu-id="42984-129">Aşağıdaki özellikler proje derlemesi bir parçası olarak Razor'ın SDK davranışı denetler:</span><span class="sxs-lookup"><span data-stu-id="42984-129">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="42984-130">`RazorCompileOnBuild` &ndash; Zaman `true`, derler ve Razor derleme proje oluşturma işleminin parçası olarak yayar.</span><span class="sxs-lookup"><span data-stu-id="42984-130">`RazorCompileOnBuild` &ndash; When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="42984-131">Varsayılan olarak `true`.</span><span class="sxs-lookup"><span data-stu-id="42984-131">Defaults to `true`.</span></span>
* <span data-ttu-id="42984-132">`RazorCompileOnPublish` &ndash; Zaman `true`, derler ve Razor derleme proje yayımlama işleminin parçası olarak yayar.</span><span class="sxs-lookup"><span data-stu-id="42984-132">`RazorCompileOnPublish` &ndash; When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="42984-133">Varsayılan olarak `true`.</span><span class="sxs-lookup"><span data-stu-id="42984-133">Defaults to `true`.</span></span>

<span data-ttu-id="42984-134">Özellikler ve öğeler aşağıdaki tabloda, giriş ve çıkış Razor SDK'sına yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="42984-134">The properties and items in the following table are used to configure inputs and output to the Razor SDK.</span></span>

| <span data-ttu-id="42984-135">Öğeler</span><span class="sxs-lookup"><span data-stu-id="42984-135">Items</span></span> | <span data-ttu-id="42984-136">Açıklama</span><span class="sxs-lookup"><span data-stu-id="42984-136">Description</span></span> |
| ----- | ----------- |
| `RazorGenerate` | <span data-ttu-id="42984-137">Öğe öğeleri ( *.cshtml* dosyaları) girişleri kod oluşturma hedefleri olan.</span><span class="sxs-lookup"><span data-stu-id="42984-137">Item elements (*.cshtml* files) that are inputs to code generation targets.</span></span> |
| `RazorCompile` | <span data-ttu-id="42984-138">Öğe öğeleri ( *.cs* dosyaları) girişleri Razor derleme hedefleri olan.</span><span class="sxs-lookup"><span data-stu-id="42984-138">Item elements (*.cs* files) that are inputs to Razor compilation targets.</span></span> <span data-ttu-id="42984-139">Bunu kullanın `ItemGroup` Razor bütünleştirilmiş kod içine derlenmiş için ek dosyaları belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="42984-139">Use this `ItemGroup` to specify additional files to be compiled into the Razor assembly.</span></span> |
| `RazorTargetAssemblyAttribute` | <span data-ttu-id="42984-140">Kod için kullanılan öğeler için Razor derleme öznitelikleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="42984-140">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="42984-141">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="42984-141">For example:</span></span>  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | <span data-ttu-id="42984-142">Oluşturulan Razor derlemesine katıştırılmış kaynakları olarak eklenen öğeler.</span><span class="sxs-lookup"><span data-stu-id="42984-142">Item elements added as embedded resources to the generated Razor assembly.</span></span> |

| <span data-ttu-id="42984-143">Özellik</span><span class="sxs-lookup"><span data-stu-id="42984-143">Property</span></span> | <span data-ttu-id="42984-144">Açıklama</span><span class="sxs-lookup"><span data-stu-id="42984-144">Description</span></span> |
| -------- | ----------- |
| `RazorTargetName` | <span data-ttu-id="42984-145">Razor tarafından üretilen derleme dosya adı (uzantısı olmadan).</span><span class="sxs-lookup"><span data-stu-id="42984-145">File name (without extension) of the assembly produced by Razor.</span></span> | 
| `RazorOutputPath` | <span data-ttu-id="42984-146">Razor çıkış dizini.</span><span class="sxs-lookup"><span data-stu-id="42984-146">The Razor output directory.</span></span> |
| `RazorCompileToolset` | <span data-ttu-id="42984-147">Razor derlemesi oluşturmak için kullanılan araç kümesini belirlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="42984-147">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="42984-148">Geçerli değerler `Implicit`, `RazorSDK`, ve `PrecompilationTool`.</span><span class="sxs-lookup"><span data-stu-id="42984-148">Valid values are `Implicit`, `RazorSDK`, and `PrecompilationTool`.</span></span> |
| [<span data-ttu-id="42984-149">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="42984-149">EnableDefaultContentItems</span></span>](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | <span data-ttu-id="42984-150">Varsayılan değer `true`.</span><span class="sxs-lookup"><span data-stu-id="42984-150">Default is `true`.</span></span> <span data-ttu-id="42984-151">Zaman `true`, içeren *web.config*, *.json*, ve *.cshtml* içerik olarak proje dosyaları.</span><span class="sxs-lookup"><span data-stu-id="42984-151">When `true`, includes *web.config*, *.json*, and *.cshtml* files as content in the project.</span></span> <span data-ttu-id="42984-152">Aracılığıyla başvurulduğunda `Microsoft.NET.Sdk.Web`, altında dosyaları *wwwroot* ve yapılandırma dosyaları da dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="42984-152">When referenced via `Microsoft.NET.Sdk.Web`, files under *wwwroot* and config files are also included.</span></span> |
| `EnableDefaultRazorGenerateItems` | <span data-ttu-id="42984-153">Zaman `true`, içeren *.cshtml* dosyalarını `Content` öğeler `RazorGenerate` öğeleri.</span><span class="sxs-lookup"><span data-stu-id="42984-153">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| `GenerateRazorTargetAssemblyInfo` | <span data-ttu-id="42984-154">Zaman `true`, oluşturur bir *.cs* tarafından belirtilen öznitelikler içeren dosya `RazorAssemblyAttribute` ve derleme çıkış dosyası içerir.</span><span class="sxs-lookup"><span data-stu-id="42984-154">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes the file in the compile output.</span></span> |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | <span data-ttu-id="42984-155">Zaman `true`, derleme öznitelikleri için varsayılan ayarı ekler `RazorAssemblyAttribute`.</span><span class="sxs-lookup"><span data-stu-id="42984-155">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| `CopyRazorGenerateFilesToPublishDirectory` | <span data-ttu-id="42984-156">Zaman `true`, kopya `RazorGenerate` öğeleri ( *.cshtml*) dosyalarını yayımlama dizinine kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="42984-156">When `true`, copies `RazorGenerate` items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="42984-157">Genellikle, derleme zamanı veya yayımlama zamanı derleme katıldıkları, Razor dosyaları bir yayımlanan uygulama için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="42984-157">Typically, Razor files aren't required for a published app if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="42984-158">Varsayılan olarak `false`.</span><span class="sxs-lookup"><span data-stu-id="42984-158">Defaults to `false`.</span></span> |
| `CopyRefAssembliesToPublishDirectory` | <span data-ttu-id="42984-159">Zaman `true`, başvuru bütünleştirilmiş kod öğeleri Yayımla dizinine kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="42984-159">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="42984-160">Genellikle, Razor derleme, derleme zamanı veya yayımlama zamanı oluşursa başvuru bütünleştirilmiş kodları yayınlanmış bir uygulama için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="42984-160">Typically, reference assemblies aren't required for a published app if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="42984-161">Kümesine `true` yayımlanmış uygulamanızın çalışma zamanı derlemesi gerekiyorsa.</span><span class="sxs-lookup"><span data-stu-id="42984-161">Set to `true` if your published app requires runtime compilation.</span></span> <span data-ttu-id="42984-162">Örneğin, değer kümesine `true` uygulama değiştirirse *.cshtml* çalışma zamanında dosya veya katıştırılmış görünümlerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="42984-162">For example, set the value to `true` if the app modifies *.cshtml* files at runtime or uses embedded views.</span></span> <span data-ttu-id="42984-163">Varsayılan olarak `false`.</span><span class="sxs-lookup"><span data-stu-id="42984-163">Defaults to `false`.</span></span> |
| `IncludeRazorContentInPack` | <span data-ttu-id="42984-164">Zaman `true`, tüm Razor içerik öğeleri ( *.cshtml* dosyaları) üretilen NuGet paketini eklenmek üzere işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="42984-164">When `true`, all Razor content items (*.cshtml* files) are marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="42984-165">Varsayılan olarak `false`.</span><span class="sxs-lookup"><span data-stu-id="42984-165">Defaults to `false`.</span></span> |
| `EmbedRazorGenerateSources` | <span data-ttu-id="42984-166">Zaman `true`, RazorGenerate ekler ( *.cshtml*) öğeleri olarak oluşturulmuş Razor derlemesine katıştırılmış dosyaları.</span><span class="sxs-lookup"><span data-stu-id="42984-166">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="42984-167">Varsayılan olarak `false`.</span><span class="sxs-lookup"><span data-stu-id="42984-167">Defaults to `false`.</span></span> |
| `UseRazorBuildServer` | <span data-ttu-id="42984-168">Zaman `true`, kod oluşturma iş yüklerini boşaltmak üzere bir sürekli derleme sunucu işlemi kullanır.</span><span class="sxs-lookup"><span data-stu-id="42984-168">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="42984-169">Varsayılan olarak, değeri olarak `UseSharedCompilation`.</span><span class="sxs-lookup"><span data-stu-id="42984-169">Defaults to the value of `UseSharedCompilation`.</span></span> |

<span data-ttu-id="42984-170">Özellikler hakkında daha fazla bilgi için bkz. [MSBuild özellikleri](/visualstudio/msbuild/msbuild-properties).</span><span class="sxs-lookup"><span data-stu-id="42984-170">For more information on properties, see [MSBuild properties](/visualstudio/msbuild/msbuild-properties).</span></span>

### <a name="targets"></a><span data-ttu-id="42984-171">Hedefler</span><span class="sxs-lookup"><span data-stu-id="42984-171">Targets</span></span>

<span data-ttu-id="42984-172">Razor SDK'sı iki birincil hedefleri tanımlar:</span><span class="sxs-lookup"><span data-stu-id="42984-172">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="42984-173">`RazorGenerate` &ndash; Kod oluşturur *.cs* dosyalarını `RazorGenerate` öğesi öğeleri.</span><span class="sxs-lookup"><span data-stu-id="42984-173">`RazorGenerate` &ndash; Code generates *.cs* files from `RazorGenerate` item elements.</span></span> <span data-ttu-id="42984-174">Kullanım `RazorGenerateDependsOn` özelliği ek hedefler belirtmek için önce veya sonra bu hedef çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="42984-174">Use `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="42984-175">`RazorCompile` &ndash; Oluşturulan derler *.cs* için bir Razor derleme dosyaları.</span><span class="sxs-lookup"><span data-stu-id="42984-175">`RazorCompile` &ndash; Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="42984-176">Kullanım `RazorCompileDependsOn` önce veya sonra bu hedef çalıştırabilirsiniz ek hedefleri belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="42984-176">Use `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>

### <a name="runtime-compilation-of-razor-views"></a><span data-ttu-id="42984-177">Razor görünüm çalışma zamanı derlemesi</span><span class="sxs-lookup"><span data-stu-id="42984-177">Runtime compilation of Razor views</span></span>

* <span data-ttu-id="42984-178">Varsayılan olarak, Razor SDK'sı, çalışma zamanı derleme gerçekleştirmek için gereken başvuru derlemelerini yayımlama değil.</span><span class="sxs-lookup"><span data-stu-id="42984-178">By default, the Razor SDK doesn't publish reference assemblies that are required to perform runtime compilation.</span></span> <span data-ttu-id="42984-179">Çalışma zamanı derleme sırasında uygulama modeli kullanır, bu derleme hataları sonuçları&mdash;Örneğin, uygulama yayımlandıktan sonra uygulamayı katıştırılmış görünümleri ya da değişiklikleri görünümler kullanır.</span><span class="sxs-lookup"><span data-stu-id="42984-179">This results in compilation failures when the application model relies on runtime compilation&mdash;for example, the app uses embedded views or changes views after the app is published.</span></span> <span data-ttu-id="42984-180">Ayarlama `CopyRefAssembliesToPublishDirectory` için `true` başvuru bütünleştirilmiş kodları yayımlamaya devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="42984-180">Set `CopyRefAssembliesToPublishDirectory` to `true` to continue publishing reference assemblies.</span></span>

* <span data-ttu-id="42984-181">Bir web uygulaması için uygulamanızın hedeflediği olun `Microsoft.NET.Sdk.Web` SDK.</span><span class="sxs-lookup"><span data-stu-id="42984-181">For a web app, ensure your app is targeting the `Microsoft.NET.Sdk.Web` SDK.</span></span>

## <a name="razor-language-version"></a><span data-ttu-id="42984-182">Razor dil sürümü</span><span class="sxs-lookup"><span data-stu-id="42984-182">Razor language version</span></span>

<span data-ttu-id="42984-183">Hedeflenirken `Microsoft.NET.Sdk.Web` SDK'sı, Razor dil sürümü çıkarılan gelen uygulamanın hedef framework sürümü.</span><span class="sxs-lookup"><span data-stu-id="42984-183">When targeting the `Microsoft.NET.Sdk.Web` SDK, the Razor language version is inferred from the the app's target framework version.</span></span> <span data-ttu-id="42984-184">Hedefleyen projeler için `Microsoft.NET.Sdk.Razor` SDK veya uygulama çıkarsanan değerinden farklı bir Razor dil sürümü gerektirdiğini nadir durumlarda, bir sürüm ayarlanarak yapılandırılabilir `<RazorLangVersion>` uygulamanın proje dosyasında özelliği:</span><span class="sxs-lookup"><span data-stu-id="42984-184">For projects targeting the `Microsoft.NET.Sdk.Razor` SDK or in the rare case that the app requires a different Razor language version than the inferred value, a version can be configured by setting the `<RazorLangVersion>` property in the app's project file:</span></span>

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

## <a name="additional-resources"></a><span data-ttu-id="42984-185">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="42984-185">Additional resources</span></span>

* [<span data-ttu-id="42984-186">.NET Core csproj biçimine eklemeler</span><span class="sxs-lookup"><span data-stu-id="42984-186">Additions to the csproj format for .NET Core</span></span>](/dotnet/core/tools/csproj)
* [<span data-ttu-id="42984-187">Yaygın MSBuild proje öğeleri</span><span class="sxs-lookup"><span data-stu-id="42984-187">Common MSBuild project items</span></span>](/visualstudio/msbuild/common-msbuild-project-items)
