---
title: ASP.NET Core dosya sağlayıcıları
author: guardrex
description: Dosya sağlayıcılarının kullanımı aracılığıyla dosya sistemi erişimini ASP.NET Core nasıl soyutleyeceğinizi öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/26/2019
uid: fundamentals/file-providers
ms.openlocfilehash: 44c439dce893d486668bf8ac3f20cdf7952c5186
ms.sourcegitcommit: 0774a61a3a6c1412a7da0e7d932dc60c506441fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70059091"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="14792-103">ASP.NET Core dosya sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="14792-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="14792-104">[Steve Smith](https://ardalis.com/) ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="14792-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="14792-105">ASP.NET Core dosya sistemi erişimini dosya sağlayıcılarının kullanımı üzerinden soyutlar.</span><span class="sxs-lookup"><span data-stu-id="14792-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="14792-106">Dosya sağlayıcıları ASP.NET Core Framework boyunca kullanılır:</span><span class="sxs-lookup"><span data-stu-id="14792-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="14792-107">`IWebHostEnvironment`uygulamanın içerik kökünü ve Web kökünü türler olarak `IFileProvider` gösterir.</span><span class="sxs-lookup"><span data-stu-id="14792-107">`IWebHostEnvironment` exposes the app's content root and web root as `IFileProvider` types.</span></span>
* <span data-ttu-id="14792-108">[Statik dosya ara yazılımı](xref:fundamentals/static-files) , statik dosyaları bulmak Için dosya sağlayıcılarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="14792-108">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="14792-109">[Razor](xref:mvc/views/razor) , sayfa ve görünümleri bulmak Için dosya sağlayıcılarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="14792-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="14792-110">.NET Core araçları, hangi dosyaların yayımlanacak olduğunu belirlemek için dosya sağlayıcılarını ve glob düzenlerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="14792-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="14792-111">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="14792-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="14792-112">Dosya sağlayıcısı arabirimleri</span><span class="sxs-lookup"><span data-stu-id="14792-112">File Provider interfaces</span></span>

<span data-ttu-id="14792-113">Birincil arabirim <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="14792-113">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="14792-114">`IFileProvider`şunları yapmak için yöntemler sunar:</span><span class="sxs-lookup"><span data-stu-id="14792-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="14792-115">Dosya bilgilerini edinin (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span><span class="sxs-lookup"><span data-stu-id="14792-115">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="14792-116">Dizin bilgilerini (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>) alın.</span><span class="sxs-lookup"><span data-stu-id="14792-116">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="14792-117">Değişiklik bildirimlerini ayarlayın (bir <xref:Microsoft.Extensions.Primitives.IChangeToken>kullanarak).</span><span class="sxs-lookup"><span data-stu-id="14792-117">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="14792-118">`IFileInfo`dosyalarla çalışma için yöntemler ve özellikler sağlar:</span><span class="sxs-lookup"><span data-stu-id="14792-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="14792-119"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length>(bayt cinsinden)</span><span class="sxs-lookup"><span data-stu-id="14792-119"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="14792-120"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified>güncel</span><span class="sxs-lookup"><span data-stu-id="14792-120"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="14792-121">[Ifileınfo. CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) yöntemini kullanarak dosyadan okuma yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="14792-121">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="14792-122">Örnek uygulama, uygulamasında `Startup.ConfigureServices` [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla uygulama genelinde kullanılmak üzere bir dosya sağlayıcısının nasıl yapılandırılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="14792-122">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="14792-123">Dosya sağlayıcısı uygulamaları</span><span class="sxs-lookup"><span data-stu-id="14792-123">File Provider implementations</span></span>

<span data-ttu-id="14792-124">Üç uygulaması `IFileProvider` mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="14792-124">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="14792-125">Uygulama</span><span class="sxs-lookup"><span data-stu-id="14792-125">Implementation</span></span> | <span data-ttu-id="14792-126">Açıklama</span><span class="sxs-lookup"><span data-stu-id="14792-126">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="14792-127">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="14792-127">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="14792-128">Fiziksel sağlayıcı, sistemin fiziksel dosyalarına erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="14792-128">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="14792-129">Bildirimli Estembeddedfileprovider</span><span class="sxs-lookup"><span data-stu-id="14792-129">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="14792-130">Bildirimde yerleşik olarak bulunan dosyalara erişmek için bildirim eklenmiş sağlayıcı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="14792-130">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="14792-131">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="14792-131">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="14792-132">Bileşik sağlayıcı, bir veya daha fazla sağlayıcıdan gelen dosyalara ve dizinlere Birleşik erişim sağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="14792-132">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="14792-133">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="14792-133">PhysicalFileProvider</span></span>

<span data-ttu-id="14792-134">Fiziksel dosya sistemine erişim sağlar.<xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider></span><span class="sxs-lookup"><span data-stu-id="14792-134">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="14792-135">`PhysicalFileProvider`, <xref:System.IO.File?displayProperty=fullName> türü (fiziksel sağlayıcı için) ve tüm yolları bir dizine ve alt öğelerine kullanır.</span><span class="sxs-lookup"><span data-stu-id="14792-135">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="14792-136">Bu kapsam, belirtilen dizin ve alt öğeleri dışındaki dosya sistemine erişimi engeller.</span><span class="sxs-lookup"><span data-stu-id="14792-136">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="14792-137">Oluşturma ve kullanma `PhysicalFileProvider` için en yaygın senaryo, [bağımlılık ekleme](xref:fundamentals/dependency-injection)aracılığıyla bir Oluşturucu `IFileProvider` içinde istekte bulunur.</span><span class="sxs-lookup"><span data-stu-id="14792-137">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="14792-138">Bu sağlayıcıyı doğrudan örnekleyen bir dizin yolu gereklidir ve sağlayıcı kullanılarak yapılan tüm isteklerin temel yolu olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="14792-138">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="14792-139">Aşağıdaki kod, nasıl oluşturulacağını `PhysicalFileProvider` ve dizin içeriğini ve dosya bilgilerini almak için nasıl kullanılacağını gösterir:</span><span class="sxs-lookup"><span data-stu-id="14792-139">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="14792-140">Önceki örnekteki türler:</span><span class="sxs-lookup"><span data-stu-id="14792-140">Types in the preceding example:</span></span>

* <span data-ttu-id="14792-141">`provider`bir `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="14792-141">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="14792-142">`contents`bir `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="14792-142">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="14792-143">`fileInfo`bir `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="14792-143">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="14792-144">Dosya sağlayıcısı, tarafından `applicationRoot` belirtilen dizin üzerinden yinelemek veya bir dosyanın bilgilerini almak için çağırmak `GetFileInfo` üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="14792-144">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="14792-145">Dosya sağlayıcısı, `applicationRoot` dizin dışında bir erişime sahip değil.</span><span class="sxs-lookup"><span data-stu-id="14792-145">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="14792-146">Örnek uygulama, sağlayıcıyı `Startup.ConfigureServices` [ıhostingenvironment. contentrootfileprovider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider)kullanarak uygulamanın sınıfında oluşturur:</span><span class="sxs-lookup"><span data-stu-id="14792-146">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="14792-147">Bildirimli Estembeddedfileprovider</span><span class="sxs-lookup"><span data-stu-id="14792-147">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="14792-148">, <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> Derlemeler içine katıştırılmış dosyalara erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="14792-148">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="14792-149">, `ManifestEmbeddedFileProvider` Gömülü dosyaların özgün yollarını yeniden oluşturmak için derlemeye derlenen bir bildirim kullanır.</span><span class="sxs-lookup"><span data-stu-id="14792-149">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="14792-150">[Microsoft. Extensions. FileProviders. Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) paketi için projeye bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="14792-150">Add a package reference to the project for the [Microsoft.Extensions.FileProviders.Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) package.</span></span>

<span data-ttu-id="14792-151">Gömülü dosyaların bir bildirimini oluşturmak için `<GenerateEmbeddedFilesManifest>` özelliğini olarak `true`ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="14792-151">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="14792-152">[ \<EmbeddedResource >](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects)eklenecek dosyaları belirtin:</span><span class="sxs-lookup"><span data-stu-id="14792-152">Specify the files to embed with [\<EmbeddedResource>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="14792-153">Derlemeye eklemek üzere bir veya daha fazla dosya belirtmek için [Glob desenlerini](#glob-patterns) kullanın.</span><span class="sxs-lookup"><span data-stu-id="14792-153">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="14792-154">Örnek uygulama bir `ManifestEmbeddedFileProvider` oluşturur ve şu anda yürütülmekte olan derlemeyi oluşturucusuna geçirir.</span><span class="sxs-lookup"><span data-stu-id="14792-154">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="14792-155">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="14792-155">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="14792-156">Ek aşırı yüklemeler şunları yapmanıza olanak sağlar:</span><span class="sxs-lookup"><span data-stu-id="14792-156">Additional overloads allow you to:</span></span>

* <span data-ttu-id="14792-157">Göreli bir dosya yolu belirtin.</span><span class="sxs-lookup"><span data-stu-id="14792-157">Specify a relative file path.</span></span>
* <span data-ttu-id="14792-158">Dosya kapsamını son değiştirilme tarihine kadar.</span><span class="sxs-lookup"><span data-stu-id="14792-158">Scope files to a last modified date.</span></span>
* <span data-ttu-id="14792-159">Katıştırılmış dosya bildirimini içeren gömülü kaynağı adlandırın.</span><span class="sxs-lookup"><span data-stu-id="14792-159">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="14792-160">Yüklemek</span><span class="sxs-lookup"><span data-stu-id="14792-160">Overload</span></span> | <span data-ttu-id="14792-161">Açıklama</span><span class="sxs-lookup"><span data-stu-id="14792-161">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="14792-162">İsteğe bağlı `root` bir göreli yol parametresini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="14792-162">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="14792-163">Belirtilen yol altında bu kaynaklara yönelik <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> olarak yapılan çağrıları belirtin. `root`</span><span class="sxs-lookup"><span data-stu-id="14792-163">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="14792-164">İsteğe bağlı `root` göreli yol parametresini ve bir `lastModified` Date (<xref:System.DateTimeOffset>) parametresini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="14792-164">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="14792-165">Tarih kapsamları <xref:Microsoft.Extensions.FileProviders.IFileInfo> tarafından<xref:Microsoft.Extensions.FileProviders.IFileProvider>döndürülen örneklerin son değiştirilme tarihidir. `lastModified`</span><span class="sxs-lookup"><span data-stu-id="14792-165">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="14792-166">İsteğe bağlı `root` bir göreli yolu, `lastModified` tarihi ve `manifestName` parametreleri kabul eder.</span><span class="sxs-lookup"><span data-stu-id="14792-166">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="14792-167">, `manifestName` Bildirimi içeren gömülü kaynağın adını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="14792-167">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="14792-168">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="14792-168">CompositeFileProvider</span></span>

<span data-ttu-id="14792-169">, Birden `IFileProvider` çok sağlayıcıdan dosyalarla çalışmak için tek bir arabirim ortaya çıkaran örnekleri birleştirir.<xref:Microsoft.Extensions.FileProviders.CompositeFileProvider></span><span class="sxs-lookup"><span data-stu-id="14792-169">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="14792-170">Oluştururken `CompositeFileProvider`bir veya daha fazla `IFileProvider` örneği oluşturucuya geçirin.</span><span class="sxs-lookup"><span data-stu-id="14792-170">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="14792-171">Örnek uygulamada, `PhysicalFileProvider` `ManifestEmbeddedFileProvider` ve, uygulamanın hizmet kapsayıcısında `CompositeFileProvider` kayıtlı bir dosya sağlar:</span><span class="sxs-lookup"><span data-stu-id="14792-171">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="14792-172">Değişiklikleri izle</span><span class="sxs-lookup"><span data-stu-id="14792-172">Watch for changes</span></span>

<span data-ttu-id="14792-173">[IFileProvider. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) yöntemi, değişiklikler için bir veya daha fazla dosya ya da dizin izlemek üzere bir senaryo sağlar.</span><span class="sxs-lookup"><span data-stu-id="14792-173">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="14792-174">`Watch`birden çok dosya belirtmek için [Glob desenlerini](#glob-patterns) içerebilen bir yol dizesi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="14792-174">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="14792-175">`Watch`<xref:Microsoft.Extensions.Primitives.IChangeToken>döndürür.</span><span class="sxs-lookup"><span data-stu-id="14792-175">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="14792-176">Değişiklik belirteci şunları gösterir:</span><span class="sxs-lookup"><span data-stu-id="14792-176">The change token exposes:</span></span>

* <span data-ttu-id="14792-177"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged>&ndash; Bir değişikliğin oluşup oluşmadığını tespit etmek için incelenebilir bir özellik.</span><span class="sxs-lookup"><span data-stu-id="14792-177"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="14792-178"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*>&ndash; Belirtilen yol dizesinde değişiklikler algılandığında çağırılır.</span><span class="sxs-lookup"><span data-stu-id="14792-178"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="14792-179">Her değişiklik belirteci yalnızca, ilişkili geri çağırma işlemini tek bir değişikliğe yanıt olarak çağırır.</span><span class="sxs-lookup"><span data-stu-id="14792-179">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="14792-180">Sabit izlemeyi etkinleştirmek için bir <xref:System.Threading.Tasks.TaskCompletionSource`1> (aşağıda gösterilmiştir) kullanın veya değişiklikleri yanıt olarak yeniden oluşturun. `IChangeToken`</span><span class="sxs-lookup"><span data-stu-id="14792-180">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="14792-181">Örnek uygulamada, *WatchConsole* konsol uygulaması bir metin dosyası her değiştirildiğinde bir ileti görüntüleyecek şekilde yapılandırılmıştır:</span><span class="sxs-lookup"><span data-stu-id="14792-181">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/3.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="14792-182">Docker kapsayıcıları ve ağ paylaşımları gibi bazı dosya sistemleri, değişiklik bildirimlerini güvenilir bir şekilde gönderemeyebilir.</span><span class="sxs-lookup"><span data-stu-id="14792-182">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="14792-183">Ortam değişkenini, her dört `1` saniyede `true` bir değişiklikler için dosya sistemini yoklamak üzere veya olarak ayarlayın (yapılandırılamaz). `DOTNET_USE_POLLING_FILE_WATCHER`</span><span class="sxs-lookup"><span data-stu-id="14792-183">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="14792-184">Glob desenleri</span><span class="sxs-lookup"><span data-stu-id="14792-184">Glob patterns</span></span>

<span data-ttu-id="14792-185">Dosya sistemi yolları, *Glob (veya glob) desenleri*adlı joker karakter desenleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="14792-185">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="14792-186">Bu desenlerle dosya gruplarını belirtin.</span><span class="sxs-lookup"><span data-stu-id="14792-186">Specify groups of files with these patterns.</span></span> <span data-ttu-id="14792-187">İki joker karakter şunlardır `*`: `**`</span><span class="sxs-lookup"><span data-stu-id="14792-187">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="14792-188">Geçerli klasör düzeyindeki her şeyi, dosya adını veya herhangi bir dosya uzantısını eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="14792-188">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="14792-189">Eşleşmeler, dosya yolundaki `/` `.` karakterler ile sonlandırılır.</span><span class="sxs-lookup"><span data-stu-id="14792-189">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="14792-190">Birden çok dizin düzeyindeki tüm öğeleri eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="14792-190">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="14792-191">, Bir Dizin hiyerarşisinde birçok dosya yinelemeli olarak eşleşmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="14792-191">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="14792-192">**Glob deseninin örnekleri**</span><span class="sxs-lookup"><span data-stu-id="14792-192">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="14792-193">Belirli bir dizindeki belirli bir dosyayla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="14792-193">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="14792-194">Belirli bir dizinde *. txt* uzantılı tüm dosyaları eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="14792-194">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="14792-195">Dizinteki `appsettings.json` tüm dosyaları, *Dizin* klasörünün altında tam olarak bir düzey eşler.</span><span class="sxs-lookup"><span data-stu-id="14792-195">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="14792-196">*. Txt* uzantılı tüm dosyaları, *Dizin* klasörünün altında herhangi bir yerde buldu.</span><span class="sxs-lookup"><span data-stu-id="14792-196">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="14792-197">ASP.NET Core dosya sistemi erişimini dosya sağlayıcılarının kullanımı üzerinden soyutlar.</span><span class="sxs-lookup"><span data-stu-id="14792-197">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="14792-198">Dosya sağlayıcıları ASP.NET Core Framework boyunca kullanılır:</span><span class="sxs-lookup"><span data-stu-id="14792-198">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="14792-199"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment>uygulamanın içerik kökünü ve Web kökünü türler olarak `IFileProvider` gösterir.</span><span class="sxs-lookup"><span data-stu-id="14792-199"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> exposes the app's content root and web root as `IFileProvider` types.</span></span>
* <span data-ttu-id="14792-200">[Statik dosya ara yazılımı](xref:fundamentals/static-files) , statik dosyaları bulmak Için dosya sağlayıcılarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="14792-200">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="14792-201">[Razor](xref:mvc/views/razor) , sayfa ve görünümleri bulmak Için dosya sağlayıcılarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="14792-201">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="14792-202">.NET Core araçları, hangi dosyaların yayımlanacak olduğunu belirlemek için dosya sağlayıcılarını ve glob düzenlerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="14792-202">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="14792-203">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="14792-203">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="14792-204">Dosya sağlayıcısı arabirimleri</span><span class="sxs-lookup"><span data-stu-id="14792-204">File Provider interfaces</span></span>

<span data-ttu-id="14792-205">Birincil arabirim <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="14792-205">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="14792-206">`IFileProvider`şunları yapmak için yöntemler sunar:</span><span class="sxs-lookup"><span data-stu-id="14792-206">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="14792-207">Dosya bilgilerini edinin (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span><span class="sxs-lookup"><span data-stu-id="14792-207">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="14792-208">Dizin bilgilerini (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>) alın.</span><span class="sxs-lookup"><span data-stu-id="14792-208">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="14792-209">Değişiklik bildirimlerini ayarlayın (bir <xref:Microsoft.Extensions.Primitives.IChangeToken>kullanarak).</span><span class="sxs-lookup"><span data-stu-id="14792-209">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="14792-210">`IFileInfo`dosyalarla çalışma için yöntemler ve özellikler sağlar:</span><span class="sxs-lookup"><span data-stu-id="14792-210">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="14792-211"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length>(bayt cinsinden)</span><span class="sxs-lookup"><span data-stu-id="14792-211"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="14792-212"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified>güncel</span><span class="sxs-lookup"><span data-stu-id="14792-212"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="14792-213">[Ifileınfo. CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) yöntemini kullanarak dosyadan okuma yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="14792-213">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="14792-214">Örnek uygulama, uygulamasında `Startup.ConfigureServices` [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla uygulama genelinde kullanılmak üzere bir dosya sağlayıcısının nasıl yapılandırılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="14792-214">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="14792-215">Dosya sağlayıcısı uygulamaları</span><span class="sxs-lookup"><span data-stu-id="14792-215">File Provider implementations</span></span>

<span data-ttu-id="14792-216">Üç uygulaması `IFileProvider` mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="14792-216">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="14792-217">Uygulama</span><span class="sxs-lookup"><span data-stu-id="14792-217">Implementation</span></span> | <span data-ttu-id="14792-218">Açıklama</span><span class="sxs-lookup"><span data-stu-id="14792-218">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="14792-219">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="14792-219">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="14792-220">Fiziksel sağlayıcı, sistemin fiziksel dosyalarına erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="14792-220">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="14792-221">Bildirimli Estembeddedfileprovider</span><span class="sxs-lookup"><span data-stu-id="14792-221">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="14792-222">Bildirimde yerleşik olarak bulunan dosyalara erişmek için bildirim eklenmiş sağlayıcı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="14792-222">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="14792-223">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="14792-223">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="14792-224">Bileşik sağlayıcı, bir veya daha fazla sağlayıcıdan gelen dosyalara ve dizinlere Birleşik erişim sağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="14792-224">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="14792-225">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="14792-225">PhysicalFileProvider</span></span>

<span data-ttu-id="14792-226">Fiziksel dosya sistemine erişim sağlar.<xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider></span><span class="sxs-lookup"><span data-stu-id="14792-226">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="14792-227">`PhysicalFileProvider`, <xref:System.IO.File?displayProperty=fullName> türü (fiziksel sağlayıcı için) ve tüm yolları bir dizine ve alt öğelerine kullanır.</span><span class="sxs-lookup"><span data-stu-id="14792-227">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="14792-228">Bu kapsam, belirtilen dizin ve alt öğeleri dışındaki dosya sistemine erişimi engeller.</span><span class="sxs-lookup"><span data-stu-id="14792-228">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="14792-229">Oluşturma ve kullanma `PhysicalFileProvider` için en yaygın senaryo, [bağımlılık ekleme](xref:fundamentals/dependency-injection)aracılığıyla bir Oluşturucu `IFileProvider` içinde istekte bulunur.</span><span class="sxs-lookup"><span data-stu-id="14792-229">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="14792-230">Bu sağlayıcıyı doğrudan örnekleyen bir dizin yolu gereklidir ve sağlayıcı kullanılarak yapılan tüm isteklerin temel yolu olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="14792-230">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="14792-231">Aşağıdaki kod, nasıl oluşturulacağını `PhysicalFileProvider` ve dizin içeriğini ve dosya bilgilerini almak için nasıl kullanılacağını gösterir:</span><span class="sxs-lookup"><span data-stu-id="14792-231">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="14792-232">Önceki örnekteki türler:</span><span class="sxs-lookup"><span data-stu-id="14792-232">Types in the preceding example:</span></span>

* <span data-ttu-id="14792-233">`provider`bir `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="14792-233">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="14792-234">`contents`bir `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="14792-234">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="14792-235">`fileInfo`bir `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="14792-235">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="14792-236">Dosya sağlayıcısı, tarafından `applicationRoot` belirtilen dizin üzerinden yinelemek veya bir dosyanın bilgilerini almak için çağırmak `GetFileInfo` üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="14792-236">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="14792-237">Dosya sağlayıcısı, `applicationRoot` dizin dışında bir erişime sahip değil.</span><span class="sxs-lookup"><span data-stu-id="14792-237">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="14792-238">Örnek uygulama, sağlayıcıyı `Startup.ConfigureServices` [ıhostingenvironment. contentrootfileprovider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider)kullanarak uygulamanın sınıfında oluşturur:</span><span class="sxs-lookup"><span data-stu-id="14792-238">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="14792-239">Bildirimli Estembeddedfileprovider</span><span class="sxs-lookup"><span data-stu-id="14792-239">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="14792-240">, <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> Derlemeler içine katıştırılmış dosyalara erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="14792-240">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="14792-241">, `ManifestEmbeddedFileProvider` Gömülü dosyaların özgün yollarını yeniden oluşturmak için derlemeye derlenen bir bildirim kullanır.</span><span class="sxs-lookup"><span data-stu-id="14792-241">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="14792-242">Gömülü dosyaların bir bildirimini oluşturmak için `<GenerateEmbeddedFilesManifest>` özelliğini olarak `true`ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="14792-242">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="14792-243">[ &lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects)ile eklenecek dosyaları belirtin:</span><span class="sxs-lookup"><span data-stu-id="14792-243">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="14792-244">Derlemeye eklemek üzere bir veya daha fazla dosya belirtmek için [Glob desenlerini](#glob-patterns) kullanın.</span><span class="sxs-lookup"><span data-stu-id="14792-244">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="14792-245">Örnek uygulama bir `ManifestEmbeddedFileProvider` oluşturur ve şu anda yürütülmekte olan derlemeyi oluşturucusuna geçirir.</span><span class="sxs-lookup"><span data-stu-id="14792-245">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="14792-246">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="14792-246">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="14792-247">Ek aşırı yüklemeler şunları yapmanıza olanak sağlar:</span><span class="sxs-lookup"><span data-stu-id="14792-247">Additional overloads allow you to:</span></span>

* <span data-ttu-id="14792-248">Göreli bir dosya yolu belirtin.</span><span class="sxs-lookup"><span data-stu-id="14792-248">Specify a relative file path.</span></span>
* <span data-ttu-id="14792-249">Dosya kapsamını son değiştirilme tarihine kadar.</span><span class="sxs-lookup"><span data-stu-id="14792-249">Scope files to a last modified date.</span></span>
* <span data-ttu-id="14792-250">Katıştırılmış dosya bildirimini içeren gömülü kaynağı adlandırın.</span><span class="sxs-lookup"><span data-stu-id="14792-250">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="14792-251">Yüklemek</span><span class="sxs-lookup"><span data-stu-id="14792-251">Overload</span></span> | <span data-ttu-id="14792-252">Açıklama</span><span class="sxs-lookup"><span data-stu-id="14792-252">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="14792-253">İsteğe bağlı `root` bir göreli yol parametresini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="14792-253">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="14792-254">Belirtilen yol altında bu kaynaklara yönelik <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> olarak yapılan çağrıları belirtin. `root`</span><span class="sxs-lookup"><span data-stu-id="14792-254">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="14792-255">İsteğe bağlı `root` göreli yol parametresini ve bir `lastModified` Date (<xref:System.DateTimeOffset>) parametresini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="14792-255">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="14792-256">Tarih kapsamları <xref:Microsoft.Extensions.FileProviders.IFileInfo> tarafından<xref:Microsoft.Extensions.FileProviders.IFileProvider>döndürülen örneklerin son değiştirilme tarihidir. `lastModified`</span><span class="sxs-lookup"><span data-stu-id="14792-256">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="14792-257">İsteğe bağlı `root` bir göreli yolu, `lastModified` tarihi ve `manifestName` parametreleri kabul eder.</span><span class="sxs-lookup"><span data-stu-id="14792-257">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="14792-258">, `manifestName` Bildirimi içeren gömülü kaynağın adını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="14792-258">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="14792-259">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="14792-259">CompositeFileProvider</span></span>

<span data-ttu-id="14792-260">, Birden `IFileProvider` çok sağlayıcıdan dosyalarla çalışmak için tek bir arabirim ortaya çıkaran örnekleri birleştirir.<xref:Microsoft.Extensions.FileProviders.CompositeFileProvider></span><span class="sxs-lookup"><span data-stu-id="14792-260">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="14792-261">Oluştururken `CompositeFileProvider`bir veya daha fazla `IFileProvider` örneği oluşturucuya geçirin.</span><span class="sxs-lookup"><span data-stu-id="14792-261">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="14792-262">Örnek uygulamada, `PhysicalFileProvider` `ManifestEmbeddedFileProvider` ve, uygulamanın hizmet kapsayıcısında `CompositeFileProvider` kayıtlı bir dosya sağlar:</span><span class="sxs-lookup"><span data-stu-id="14792-262">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="14792-263">Değişiklikleri izle</span><span class="sxs-lookup"><span data-stu-id="14792-263">Watch for changes</span></span>

<span data-ttu-id="14792-264">[IFileProvider. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) yöntemi, değişiklikler için bir veya daha fazla dosya ya da dizin izlemek üzere bir senaryo sağlar.</span><span class="sxs-lookup"><span data-stu-id="14792-264">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="14792-265">`Watch`birden çok dosya belirtmek için [Glob desenlerini](#glob-patterns) içerebilen bir yol dizesi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="14792-265">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="14792-266">`Watch`<xref:Microsoft.Extensions.Primitives.IChangeToken>döndürür.</span><span class="sxs-lookup"><span data-stu-id="14792-266">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="14792-267">Değişiklik belirteci şunları gösterir:</span><span class="sxs-lookup"><span data-stu-id="14792-267">The change token exposes:</span></span>

* <span data-ttu-id="14792-268"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged>&ndash; Bir değişikliğin oluşup oluşmadığını tespit etmek için incelenebilir bir özellik.</span><span class="sxs-lookup"><span data-stu-id="14792-268"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="14792-269"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*>&ndash; Belirtilen yol dizesinde değişiklikler algılandığında çağırılır.</span><span class="sxs-lookup"><span data-stu-id="14792-269"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="14792-270">Her değişiklik belirteci yalnızca, ilişkili geri çağırma işlemini tek bir değişikliğe yanıt olarak çağırır.</span><span class="sxs-lookup"><span data-stu-id="14792-270">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="14792-271">Sabit izlemeyi etkinleştirmek için bir <xref:System.Threading.Tasks.TaskCompletionSource`1> (aşağıda gösterilmiştir) kullanın veya değişiklikleri yanıt olarak yeniden oluşturun. `IChangeToken`</span><span class="sxs-lookup"><span data-stu-id="14792-271">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="14792-272">Örnek uygulamada, *WatchConsole* konsol uygulaması bir metin dosyası her değiştirildiğinde bir ileti görüntüleyecek şekilde yapılandırılmıştır:</span><span class="sxs-lookup"><span data-stu-id="14792-272">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="14792-273">Docker kapsayıcıları ve ağ paylaşımları gibi bazı dosya sistemleri, değişiklik bildirimlerini güvenilir bir şekilde gönderemeyebilir.</span><span class="sxs-lookup"><span data-stu-id="14792-273">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="14792-274">Ortam değişkenini, her dört `1` saniyede `true` bir değişiklikler için dosya sistemini yoklamak üzere veya olarak ayarlayın (yapılandırılamaz). `DOTNET_USE_POLLING_FILE_WATCHER`</span><span class="sxs-lookup"><span data-stu-id="14792-274">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="14792-275">Glob desenleri</span><span class="sxs-lookup"><span data-stu-id="14792-275">Glob patterns</span></span>

<span data-ttu-id="14792-276">Dosya sistemi yolları, *Glob (veya glob) desenleri*adlı joker karakter desenleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="14792-276">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="14792-277">Bu desenlerle dosya gruplarını belirtin.</span><span class="sxs-lookup"><span data-stu-id="14792-277">Specify groups of files with these patterns.</span></span> <span data-ttu-id="14792-278">İki joker karakter şunlardır `*`: `**`</span><span class="sxs-lookup"><span data-stu-id="14792-278">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="14792-279">Geçerli klasör düzeyindeki her şeyi, dosya adını veya herhangi bir dosya uzantısını eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="14792-279">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="14792-280">Eşleşmeler, dosya yolundaki `/` `.` karakterler ile sonlandırılır.</span><span class="sxs-lookup"><span data-stu-id="14792-280">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="14792-281">Birden çok dizin düzeyindeki tüm öğeleri eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="14792-281">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="14792-282">, Bir Dizin hiyerarşisinde birçok dosya yinelemeli olarak eşleşmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="14792-282">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="14792-283">**Glob deseninin örnekleri**</span><span class="sxs-lookup"><span data-stu-id="14792-283">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="14792-284">Belirli bir dizindeki belirli bir dosyayla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="14792-284">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="14792-285">Belirli bir dizinde *. txt* uzantılı tüm dosyaları eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="14792-285">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="14792-286">Dizinteki `appsettings.json` tüm dosyaları, *Dizin* klasörünün altında tam olarak bir düzey eşler.</span><span class="sxs-lookup"><span data-stu-id="14792-286">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="14792-287">*. Txt* uzantılı tüm dosyaları, *Dizin* klasörünün altında herhangi bir yerde buldu.</span><span class="sxs-lookup"><span data-stu-id="14792-287">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end
