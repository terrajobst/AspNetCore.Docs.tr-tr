---
title: ASP.NET Core Razor SDK'sı
author: Rick-Anderson
description: Nasıl ASP.NET Core Razor sayfalar kodlama sayfa odaklı senaryolar daha kolay ve MVC kullanmaktan daha üretken hale getirdiğini öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: razor-pages/sdk
ms.openlocfilehash: 1f38d768d872175e20f5cb0cb679bc3d52696eb9
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090206"
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="35fc1-103">ASP.NET Core Razor SDK'sı</span><span class="sxs-lookup"><span data-stu-id="35fc1-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="35fc1-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="35fc1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="35fc1-105">[!INCLUDE[](~/includes/2.1-SDK.md)] İçerir `Microsoft.NET.Sdk.Razor` MSBuild SDK'sı (Razor SDK).</span><span class="sxs-lookup"><span data-stu-id="35fc1-105">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="35fc1-106">Razor SDK:</span><span class="sxs-lookup"><span data-stu-id="35fc1-106">The Razor SDK:</span></span>

* <span data-ttu-id="35fc1-107">Oluşturma, paketleme ve içeren proje yayımlama deneyimi standartlaştırır [Razor](xref:mvc/views/razor) dosyaları ASP.NET Core MVC tabanlı projeler için.</span><span class="sxs-lookup"><span data-stu-id="35fc1-107">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="35fc1-108">Bir dizi önceden tanımlanmış hedefleri, özellikler ve Razor dosyaları derleme özelleştirme izin veren öğeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="35fc1-108">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="35fc1-109">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="35fc1-109">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a><span data-ttu-id="35fc1-110">Razor SDK'sını kullanma</span><span class="sxs-lookup"><span data-stu-id="35fc1-110">Using the Razor SDK</span></span>

<span data-ttu-id="35fc1-111">Çoğu web uygulaması açıkça Razor SDK'ya başvurmak için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="35fc1-111">Most web apps aren't required to explicitly reference the Razor SDK.</span></span>

<span data-ttu-id="35fc1-112">Razor görünümleri ya da Razor sayfaları içeren sınıf kitaplıkları oluşturmak için Razor SDK'sını kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="35fc1-112">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="35fc1-113">Kullanım `Microsoft.NET.Sdk.Razor` yerine `Microsoft.NET.Sdk`:</span><span class="sxs-lookup"><span data-stu-id="35fc1-113">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    ...
  </Project>
  ```

* <span data-ttu-id="35fc1-114">Genellikle, bir paket başvurusu `Microsoft.AspNetCore.Mvc` oluşturmak ve Razor sayfaları ve Razor görünümleri derlemek için gereken ek bağımlılıklar almak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="35fc1-114">Typically, a package reference to `Microsoft.AspNetCore.Mvc` is required to receive additional dependencies that are required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="35fc1-115">En azından, paket başvuruları projenize eklemeniz gerekir:</span><span class="sxs-lookup"><span data-stu-id="35fc1-115">At a minimum, your project should add package references to:</span></span>

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
    
  <span data-ttu-id="35fc1-116">`Microsoft.AspNetCore.Razor.Design` Paket proje için Razor derleme görevleri ve hedefleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="35fc1-116">The `Microsoft.AspNetCore.Razor.Design` package provides the Razor compilation tasks and targets for the project.</span></span>

  <span data-ttu-id="35fc1-117">Yukarıdaki paketleri dahil `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="35fc1-117">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="35fc1-118">Aşağıdaki biçimlendirmede Razor dosyaları için bir ASP.NET Core Razor sayfalar uygulama oluşturmak için Razor SDK'sını kullanan bir proje dosyasını göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="35fc1-118">The following markup shows a project file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> <span data-ttu-id="35fc1-119">`Microsoft.AspNetCore.Razor.Design` Ve `Microsoft.AspNetCore.Mvc.Razor.Extensions` paketleri dahil edilecek [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="35fc1-119">The `Microsoft.AspNetCore.Razor.Design` and `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="35fc1-120">Ancak, daha az sürüm `Microsoft.AspNetCore.App` paket başvuru sağlayan bir metapackage en son sürümünü içermeyen App `Microsoft.AspNetCore.Razor.Design`.</span><span class="sxs-lookup"><span data-stu-id="35fc1-120">However, the version-less `Microsoft.AspNetCore.App` package reference provides a metapackage to the app that doesn't include the latest version of `Microsoft.AspNetCore.Razor.Design`.</span></span> <span data-ttu-id="35fc1-121">Projeleri sürümünün tutarlı olmasını başvurmalıdır `Microsoft.AspNetCore.Razor.Design` (veya `Microsoft.AspNetCore.Mvc`) böylece Razor yönelik en son derleme zamanı düzeltmeleri dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="35fc1-121">Projects must reference a consistent version of `Microsoft.AspNetCore.Razor.Design` (or `Microsoft.AspNetCore.Mvc`) so that the latest build-time fixes for Razor are included.</span></span> <span data-ttu-id="35fc1-122">Daha fazla bilgi için [bu GitHub sorunu](https://github.com/aspnet/Razor/issues/2553).</span><span class="sxs-lookup"><span data-stu-id="35fc1-122">For more information, see [this GitHub issue](https://github.com/aspnet/Razor/issues/2553).</span></span>

::: moniker-end

### <a name="properties"></a><span data-ttu-id="35fc1-123">Özellikler</span><span class="sxs-lookup"><span data-stu-id="35fc1-123">Properties</span></span>

<span data-ttu-id="35fc1-124">Aşağıdaki özellikler proje derlemesi bir parçası olarak Razor'ın SDK davranışı denetler:</span><span class="sxs-lookup"><span data-stu-id="35fc1-124">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="35fc1-125">`RazorCompileOnBuild` &ndash; Zaman `true`, derler ve Razor derleme proje oluşturma işleminin parçası olarak yayar.</span><span class="sxs-lookup"><span data-stu-id="35fc1-125">`RazorCompileOnBuild` &ndash; When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="35fc1-126">Varsayılan olarak `true`.</span><span class="sxs-lookup"><span data-stu-id="35fc1-126">Defaults to `true`.</span></span>
* <span data-ttu-id="35fc1-127">`RazorCompileOnPublish` &ndash; Zaman `true`, derler ve Razor derleme proje yayımlama işleminin parçası olarak yayar.</span><span class="sxs-lookup"><span data-stu-id="35fc1-127">`RazorCompileOnPublish` &ndash; When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="35fc1-128">Varsayılan olarak `true`.</span><span class="sxs-lookup"><span data-stu-id="35fc1-128">Defaults to `true`.</span></span>

<span data-ttu-id="35fc1-129">Özellikler ve öğeler aşağıdaki tabloda, giriş ve çıkış Razor SDK'sına yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="35fc1-129">The properties and items in the following table are used to configure inputs and output to the Razor SDK.</span></span>

| <span data-ttu-id="35fc1-130">Öğeler</span><span class="sxs-lookup"><span data-stu-id="35fc1-130">Items</span></span> | <span data-ttu-id="35fc1-131">Açıklama</span><span class="sxs-lookup"><span data-stu-id="35fc1-131">Description</span></span> |
| ----- | ----------- |
| `RazorGenerate` | <span data-ttu-id="35fc1-132">Öğe öğeleri (*.cshtml* dosyaları) girişleri kod oluşturma hedefleri olan.</span><span class="sxs-lookup"><span data-stu-id="35fc1-132">Item elements (*.cshtml* files) that are inputs to code generation targets.</span></span> |
| `RazorCompile` | <span data-ttu-id="35fc1-133">Öğe öğeleri (*.cs* dosyaları) girişleri Razor derleme hedefleri olan.</span><span class="sxs-lookup"><span data-stu-id="35fc1-133">Item elements (*.cs* files) that are inputs to  Razor compilation targets.</span></span> <span data-ttu-id="35fc1-134">Bu ItemGroup Razor bütünleştirilmiş kod içine derlenmiş için ek dosyaları belirtmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="35fc1-134">Use this ItemGroup to specify additional files to be compiled into the Razor assembly.</span></span> |
| `RazorTargetAssemblyAttribute` | <span data-ttu-id="35fc1-135">Kod için kullanılan öğeler için Razor derleme öznitelikleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="35fc1-135">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="35fc1-136">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="35fc1-136">For example:</span></span>  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | <span data-ttu-id="35fc1-137">Oluşturulan Razor derlemesine katıştırılmış kaynakları olarak eklenen öğeler.</span><span class="sxs-lookup"><span data-stu-id="35fc1-137">Item elements added as embedded resources to the generated Razor assembly.</span></span> |

| <span data-ttu-id="35fc1-138">Özellik</span><span class="sxs-lookup"><span data-stu-id="35fc1-138">Property</span></span> | <span data-ttu-id="35fc1-139">Açıklama</span><span class="sxs-lookup"><span data-stu-id="35fc1-139">Description</span></span> |
| -------- | ----------- |
| `RazorTargetName` | <span data-ttu-id="35fc1-140">Razor tarafından üretilen derleme dosya adı (uzantısı olmadan).</span><span class="sxs-lookup"><span data-stu-id="35fc1-140">File name (without extension) of the assembly produced by Razor.</span></span> | 
| `RazorOutputPath` | <span data-ttu-id="35fc1-141">Razor çıkış dizini.</span><span class="sxs-lookup"><span data-stu-id="35fc1-141">The Razor output directory.</span></span> |
| `RazorCompileToolset` | <span data-ttu-id="35fc1-142">Razor derlemesi oluşturmak için kullanılan araç kümesini belirlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="35fc1-142">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="35fc1-143">Geçerli değerler `Implicit`, `RazorSDK`, ve `PrecompilationTool`.</span><span class="sxs-lookup"><span data-stu-id="35fc1-143">Valid values are `Implicit`, `RazorSDK`, and `PrecompilationTool`.</span></span> |
| `EnableDefaultContentItems` | <span data-ttu-id="35fc1-144">Zaman `true`, aşağıdakiler gibi belirli dosya türlerini içerir *.cshtml* içerik olarak proje dosyaları.</span><span class="sxs-lookup"><span data-stu-id="35fc1-144">When `true`, includes certain file types, such as *.cshtml* files, as content in the project.</span></span> <span data-ttu-id="35fc1-145">Aracılığıyla başvurulduğunda `Microsoft.NET.Sdk.Web`, altında dosyaları *wwwroot* ve yapılandırma dosyaları da dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="35fc1-145">When referenced via `Microsoft.NET.Sdk.Web`, files under *wwwroot* and config files are also included.</span></span> |
| `EnableDefaultRazorGenerateItems` | <span data-ttu-id="35fc1-146">Zaman `true`, içeren *.cshtml* dosyalarını `Content` öğeler `RazorGenerate` öğeleri.</span><span class="sxs-lookup"><span data-stu-id="35fc1-146">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| `GenerateRazorTargetAssemblyInfo` | <span data-ttu-id="35fc1-147">Zaman `true`, oluşturur bir *.cs* tarafından belirtilen öznitelikler içeren dosya `RazorAssemblyAttribute` ve derleme çıkış dosyası içerir.</span><span class="sxs-lookup"><span data-stu-id="35fc1-147">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes the file in the compile output.</span></span> |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | <span data-ttu-id="35fc1-148">Zaman `true`, derleme öznitelikleri için varsayılan ayarı ekler `RazorAssemblyAttribute`.</span><span class="sxs-lookup"><span data-stu-id="35fc1-148">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| `CopyRazorGenerateFilesToPublishDirectory` | <span data-ttu-id="35fc1-149">Zaman `true`, kopya `RazorGenerate` öğeleri (*.cshtml*) dosyalarını yayımlama dizinine kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="35fc1-149">When `true`, copies `RazorGenerate` items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="35fc1-150">Genellikle, derleme zamanı veya yayımlama zamanı derleme katıldıkları, Razor dosyaları bir yayımlanan uygulama için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="35fc1-150">Typically, Razor files aren't required for a published app if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="35fc1-151">Varsayılan olarak `false`.</span><span class="sxs-lookup"><span data-stu-id="35fc1-151">Defaults to `false`.</span></span> |
| `CopyRefAssembliesToPublishDirectory` | <span data-ttu-id="35fc1-152">Zaman `true`, başvuru bütünleştirilmiş kod öğeleri Yayımla dizinine kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="35fc1-152">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="35fc1-153">Genellikle, Razor derleme, derleme zamanı veya yayımlama zamanı oluşursa başvuru bütünleştirilmiş kodları yayınlanmış bir uygulama için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="35fc1-153">Typically, reference assemblies aren't required for a published app if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="35fc1-154">Kümesine `true` yayımlanmış uygulamanızın çalışma zamanı derlemesi gerekiyorsa.</span><span class="sxs-lookup"><span data-stu-id="35fc1-154">Set to `true` if your published app requires runtime compilation.</span></span> <span data-ttu-id="35fc1-155">Örneğin, değer kümesine `true` uygulama değiştirirse *.cshtml* çalışma zamanında dosya veya katıştırılmış görünümlerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="35fc1-155">For example, set the value to `true` if the app modifies *.cshtml* files at runtime or uses embedded views.</span></span> <span data-ttu-id="35fc1-156">Varsayılan olarak `false`.</span><span class="sxs-lookup"><span data-stu-id="35fc1-156">Defaults to `false`.</span></span> |
| `IncludeRazorContentInPack` | <span data-ttu-id="35fc1-157">Zaman `true`, tüm Razor içerik öğeleri (*.cshtml* dosyaları) üretilen NuGet paketini eklenmek üzere işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="35fc1-157">When `true`, all Razor content items (*.cshtml* files) are marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="35fc1-158">Varsayılan olarak `false`.</span><span class="sxs-lookup"><span data-stu-id="35fc1-158">Defaults to `false`.</span></span> |
| `EmbedRazorGenerateSources` | <span data-ttu-id="35fc1-159">Zaman `true`, RazorGenerate ekler (*.cshtml*) öğeleri olarak oluşturulmuş Razor derlemesine katıştırılmış dosyaları.</span><span class="sxs-lookup"><span data-stu-id="35fc1-159">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="35fc1-160">Varsayılan olarak `false`.</span><span class="sxs-lookup"><span data-stu-id="35fc1-160">Defaults to `false`.</span></span> |
| `UseRazorBuildServer` | <span data-ttu-id="35fc1-161">Zaman `true`, kod oluşturma iş yüklerini boşaltmak üzere bir sürekli derleme sunucu işlemi kullanır.</span><span class="sxs-lookup"><span data-stu-id="35fc1-161">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="35fc1-162">Varsayılan olarak, değeri olarak `UseSharedCompilation`.</span><span class="sxs-lookup"><span data-stu-id="35fc1-162">Defaults to the value of `UseSharedCompilation`.</span></span> |

### <a name="targets"></a><span data-ttu-id="35fc1-163">Hedefler</span><span class="sxs-lookup"><span data-stu-id="35fc1-163">Targets</span></span>

<span data-ttu-id="35fc1-164">Razor SDK'sı iki birincil hedefleri tanımlar:</span><span class="sxs-lookup"><span data-stu-id="35fc1-164">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="35fc1-165">`RazorGenerate` &ndash; Kod oluşturur *.cs* dosyalarını `RazorGenerate` öğesi öğeleri.</span><span class="sxs-lookup"><span data-stu-id="35fc1-165">`RazorGenerate` &ndash; Code generates *.cs* files from `RazorGenerate` item elements.</span></span> <span data-ttu-id="35fc1-166">Kullanım `RazorGenerateDependsOn` özelliği ek hedefler belirtmek için önce veya sonra bu hedef çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="35fc1-166">Use `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="35fc1-167">`RazorCompile` &ndash; Oluşturulan derler *.cs* için bir Razor derleme dosyaları.</span><span class="sxs-lookup"><span data-stu-id="35fc1-167">`RazorCompile` &ndash; Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="35fc1-168">Kullanım `RazorCompileDependsOn` önce veya sonra bu hedef çalıştırabilirsiniz ek hedefleri belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="35fc1-168">Use `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>

### <a name="runtime-compilation-of-razor-views"></a><span data-ttu-id="35fc1-169">Razor görünüm çalışma zamanı derlemesi</span><span class="sxs-lookup"><span data-stu-id="35fc1-169">Runtime compilation of Razor views</span></span>

* <span data-ttu-id="35fc1-170">Varsayılan olarak, Razor SDK'sı, çalışma zamanı derleme gerçekleştirmek için gereken başvuru derlemelerini yayımlama değil.</span><span class="sxs-lookup"><span data-stu-id="35fc1-170">By default, the Razor SDK doesn't publish reference assemblies that are required to perform runtime compilation.</span></span> <span data-ttu-id="35fc1-171">Çalışma zamanı derleme sırasında uygulama modeli kullanır, bu derleme hataları sonuçları&mdash;Örneğin, uygulama yayımlandıktan sonra uygulamayı katıştırılmış görünümleri ya da değişiklikleri görünümler kullanır.</span><span class="sxs-lookup"><span data-stu-id="35fc1-171">This results in compilation failures when the application model relies on runtime compilation&mdash;for example, the app uses embedded views or changes views after the app is published.</span></span> <span data-ttu-id="35fc1-172">Ayarlama `CopyRefAssembliesToPublishDirectory` için `true` başvuru bütünleştirilmiş kodları yayımlamaya devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="35fc1-172">Set `CopyRefAssembliesToPublishDirectory` to `true` to continue publishing reference assemblies.</span></span>

* <span data-ttu-id="35fc1-173">Bir web uygulaması için uygulamanızın hedeflediği olun `Microsoft.NET.Sdk.Web` SDK.</span><span class="sxs-lookup"><span data-stu-id="35fc1-173">For a web app, ensure your app is targeting the `Microsoft.NET.Sdk.Web` SDK.</span></span>
