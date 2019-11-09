---
title: ASP.NET Core dosya sağlayıcıları
author: guardrex
description: Dosya sağlayıcılarının kullanımı aracılığıyla dosya sistemi erişimini ASP.NET Core nasıl soyutleyeceğinizi öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: fundamentals/file-providers
ms.openlocfilehash: 531f7acd7a704a74e6142d201f613f05288deecb
ms.sourcegitcommit: 4818385c3cfe0805e15138a2c1785b62deeaab90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2019
ms.locfileid: "73896848"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="0652d-103">ASP.NET Core dosya sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="0652d-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="0652d-104">[Steve Smith](https://ardalis.com/) ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="0652d-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0652d-105">ASP.NET Core dosya sistemi erişimini dosya sağlayıcılarının kullanımı üzerinden soyutlar.</span><span class="sxs-lookup"><span data-stu-id="0652d-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="0652d-106">Dosya sağlayıcıları ASP.NET Core Framework boyunca kullanılır:</span><span class="sxs-lookup"><span data-stu-id="0652d-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="0652d-107">`IWebHostEnvironment`, uygulamanın [içerik kökünü](xref:fundamentals/index#content-root) ve [Web kökünü](xref:fundamentals/index#web-root) `IFileProvider` türleri olarak kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="0652d-107">`IWebHostEnvironment` exposes the app's [content root](xref:fundamentals/index#content-root) and [web root](xref:fundamentals/index#web-root) as `IFileProvider` types.</span></span>
* <span data-ttu-id="0652d-108">[Statik dosya ara yazılımı](xref:fundamentals/static-files) , statik dosyaları bulmak Için dosya sağlayıcılarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="0652d-108">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="0652d-109">[Razor](xref:mvc/views/razor) , sayfa ve görünümleri bulmak Için dosya sağlayıcılarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="0652d-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="0652d-110">.NET Core araçları, hangi dosyaların yayımlanacak olduğunu belirlemek için dosya sağlayıcılarını ve glob düzenlerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="0652d-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="0652d-111">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0652d-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="0652d-112">Dosya sağlayıcısı arabirimleri</span><span class="sxs-lookup"><span data-stu-id="0652d-112">File Provider interfaces</span></span>

<span data-ttu-id="0652d-113">Birincil arabirim <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="0652d-113">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="0652d-114">`IFileProvider` şu yöntemleri sunar:</span><span class="sxs-lookup"><span data-stu-id="0652d-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="0652d-115">Dosya bilgilerini edinin (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span><span class="sxs-lookup"><span data-stu-id="0652d-115">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="0652d-116">Dizin bilgilerini al (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span><span class="sxs-lookup"><span data-stu-id="0652d-116">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="0652d-117">Değişiklik bildirimlerini ayarlayın (<xref:Microsoft.Extensions.Primitives.IChangeToken>kullanarak).</span><span class="sxs-lookup"><span data-stu-id="0652d-117">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="0652d-118">`IFileInfo` dosyalarla çalışma için yöntemler ve özellikler sağlar:</span><span class="sxs-lookup"><span data-stu-id="0652d-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="0652d-119"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (bayt)</span><span class="sxs-lookup"><span data-stu-id="0652d-119"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="0652d-120"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> tarihi</span><span class="sxs-lookup"><span data-stu-id="0652d-120"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="0652d-121">[Ifileınfo. CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) yöntemini kullanarak dosyadan okuma yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0652d-121">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="0652d-122">Örnek uygulama, [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla uygulama genelinde kullanılmak üzere `Startup.ConfigureServices` bir dosya sağlayıcısının nasıl yapılandırılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="0652d-122">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="0652d-123">Dosya sağlayıcısı uygulamaları</span><span class="sxs-lookup"><span data-stu-id="0652d-123">File Provider implementations</span></span>

<span data-ttu-id="0652d-124">`IFileProvider` üç uygulaması mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="0652d-124">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="0652d-125">Uygulama</span><span class="sxs-lookup"><span data-stu-id="0652d-125">Implementation</span></span> | <span data-ttu-id="0652d-126">Açıklama</span><span class="sxs-lookup"><span data-stu-id="0652d-126">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="0652d-127">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="0652d-127">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="0652d-128">Fiziksel sağlayıcı, sistemin fiziksel dosyalarına erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0652d-128">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="0652d-129">Bildirimli Estembeddedfileprovider</span><span class="sxs-lookup"><span data-stu-id="0652d-129">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="0652d-130">Bildirimde yerleşik olarak bulunan dosyalara erişmek için bildirim eklenmiş sağlayıcı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0652d-130">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="0652d-131">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="0652d-131">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="0652d-132">Bileşik sağlayıcı, bir veya daha fazla sağlayıcıdan gelen dosyalara ve dizinlere Birleşik erişim sağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0652d-132">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="0652d-133">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="0652d-133">PhysicalFileProvider</span></span>

<span data-ttu-id="0652d-134"><xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, fiziksel dosya sistemine erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="0652d-134">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="0652d-135">`PhysicalFileProvider`, <xref:System.IO.File?displayProperty=fullName> türünü (fiziksel sağlayıcı için) ve tüm yolları bir dizine ve alt öğelerine kullanır.</span><span class="sxs-lookup"><span data-stu-id="0652d-135">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="0652d-136">Bu kapsam, belirtilen dizin ve alt öğeleri dışındaki dosya sistemine erişimi engeller.</span><span class="sxs-lookup"><span data-stu-id="0652d-136">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="0652d-137">`PhysicalFileProvider` oluşturmak ve kullanmak için en yaygın senaryo, [bağımlılık ekleme](xref:fundamentals/dependency-injection)aracılığıyla oluşturucuda bir `IFileProvider` istemaktır.</span><span class="sxs-lookup"><span data-stu-id="0652d-137">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="0652d-138">Bu sağlayıcıyı doğrudan örnekleyen bir dizin yolu gereklidir ve sağlayıcı kullanılarak yapılan tüm isteklerin temel yolu olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="0652d-138">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="0652d-139">Aşağıdaki kod, `PhysicalFileProvider` oluşturma ve dizin içeriğini ve dosya bilgilerini elde etmek için nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="0652d-139">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="0652d-140">Önceki örnekteki türler:</span><span class="sxs-lookup"><span data-stu-id="0652d-140">Types in the preceding example:</span></span>

* <span data-ttu-id="0652d-141">`provider` bir `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="0652d-141">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="0652d-142">`contents` bir `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="0652d-142">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="0652d-143">`fileInfo` bir `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="0652d-143">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="0652d-144">Dosya sağlayıcısı, `applicationRoot` tarafından belirtilen dizin üzerinden yinelemek veya bir dosyanın bilgilerini almak için `GetFileInfo` çağırmak üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0652d-144">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="0652d-145">Dosya sağlayıcısının `applicationRoot` Directory dışında erişimi yok.</span><span class="sxs-lookup"><span data-stu-id="0652d-145">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="0652d-146">Örnek uygulama, [ıhostingenvironment. ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider)kullanarak sağlayıcıyı uygulamanın `Startup.ConfigureServices` sınıfında oluşturur:</span><span class="sxs-lookup"><span data-stu-id="0652d-146">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="0652d-147">Bildirimli Estembeddedfileprovider</span><span class="sxs-lookup"><span data-stu-id="0652d-147">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="0652d-148"><xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider>, derlemeler içine katıştırılmış dosyalara erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0652d-148">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="0652d-149">`ManifestEmbeddedFileProvider` gömülü dosyaların özgün yollarını yeniden oluşturmak için derlemeye derlenen bir bildirim kullanır.</span><span class="sxs-lookup"><span data-stu-id="0652d-149">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="0652d-150">[Microsoft. Extensions. FileProviders. Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) paketi için projeye bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0652d-150">Add a package reference to the project for the [Microsoft.Extensions.FileProviders.Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) package.</span></span>

<span data-ttu-id="0652d-151">Katıştırılmış dosyaların bir bildirimini oluşturmak için `<GenerateEmbeddedFilesManifest>` özelliğini `true`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="0652d-151">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="0652d-152">[\<EmbeddedResource >](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects)eklemek için dosyaları belirtin:</span><span class="sxs-lookup"><span data-stu-id="0652d-152">Specify the files to embed with [\<EmbeddedResource>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="0652d-153">Derlemeye eklemek üzere bir veya daha fazla dosya belirtmek için [Glob desenlerini](#glob-patterns) kullanın.</span><span class="sxs-lookup"><span data-stu-id="0652d-153">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="0652d-154">Örnek uygulama bir `ManifestEmbeddedFileProvider` oluşturur ve şu anda yürütülen derlemeyi oluşturucuya geçirir.</span><span class="sxs-lookup"><span data-stu-id="0652d-154">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="0652d-155">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0652d-155">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(typeof(Program).Assembly);
```

<span data-ttu-id="0652d-156">Ek aşırı yüklemeler şunları yapmanıza olanak sağlar:</span><span class="sxs-lookup"><span data-stu-id="0652d-156">Additional overloads allow you to:</span></span>

* <span data-ttu-id="0652d-157">Göreli bir dosya yolu belirtin.</span><span class="sxs-lookup"><span data-stu-id="0652d-157">Specify a relative file path.</span></span>
* <span data-ttu-id="0652d-158">Dosya kapsamını son değiştirilme tarihine kadar.</span><span class="sxs-lookup"><span data-stu-id="0652d-158">Scope files to a last modified date.</span></span>
* <span data-ttu-id="0652d-159">Katıştırılmış dosya bildirimini içeren gömülü kaynağı adlandırın.</span><span class="sxs-lookup"><span data-stu-id="0652d-159">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="0652d-160">Yüklemek</span><span class="sxs-lookup"><span data-stu-id="0652d-160">Overload</span></span> | <span data-ttu-id="0652d-161">Açıklama</span><span class="sxs-lookup"><span data-stu-id="0652d-161">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="0652d-162">İsteğe bağlı `root` göreli yol parametresini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="0652d-162">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="0652d-163">Belirtilen yol altında bu kaynaklara <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> yapılan çağrıların kapsamını belirlemek için `root` belirtin.</span><span class="sxs-lookup"><span data-stu-id="0652d-163">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="0652d-164">İsteğe bağlı `root` göreli yol parametresini ve `lastModified` Tarih (<xref:System.DateTimeOffset>) parametresini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="0652d-164">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="0652d-165">`lastModified` tarihi, <xref:Microsoft.Extensions.FileProviders.IFileProvider>tarafından döndürülen <xref:Microsoft.Extensions.FileProviders.IFileInfo> örneklerinin son değiştirilme tarihini kapsamlar.</span><span class="sxs-lookup"><span data-stu-id="0652d-165">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="0652d-166">İsteğe bağlı `root` göreli yolunu, `lastModified` tarihini ve `manifestName` parametrelerini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="0652d-166">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="0652d-167">`manifestName`, bildirimi içeren gömülü kaynağın adını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="0652d-167">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="0652d-168">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="0652d-168">CompositeFileProvider</span></span>

<span data-ttu-id="0652d-169"><xref:Microsoft.Extensions.FileProviders.CompositeFileProvider>, birden fazla sağlayıcıdan dosyalarla çalışmak için tek bir arabirim ortaya çıkaran `IFileProvider` örneklerini birleştirir.</span><span class="sxs-lookup"><span data-stu-id="0652d-169">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="0652d-170">`CompositeFileProvider`oluştururken, bir veya daha fazla `IFileProvider` örneğini oluşturucusuna geçirin.</span><span class="sxs-lookup"><span data-stu-id="0652d-170">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="0652d-171">Örnek uygulamada, bir `PhysicalFileProvider` ve bir `ManifestEmbeddedFileProvider`, uygulamanın hizmet kapsayıcısına kayıtlı bir `CompositeFileProvider` dosya sağlar:</span><span class="sxs-lookup"><span data-stu-id="0652d-171">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="0652d-172">Değişiklikleri izle</span><span class="sxs-lookup"><span data-stu-id="0652d-172">Watch for changes</span></span>

<span data-ttu-id="0652d-173">[IFileProvider. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) yöntemi, değişiklikler için bir veya daha fazla dosya ya da dizin izlemek üzere bir senaryo sağlar.</span><span class="sxs-lookup"><span data-stu-id="0652d-173">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="0652d-174">`Watch`, birden çok dosya belirtmek için [Glob desenlerini](#glob-patterns) içerebilen bir yol dizesi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="0652d-174">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="0652d-175">`Watch` <xref:Microsoft.Extensions.Primitives.IChangeToken>döndürür.</span><span class="sxs-lookup"><span data-stu-id="0652d-175">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="0652d-176">Değişiklik belirteci şunları gösterir:</span><span class="sxs-lookup"><span data-stu-id="0652d-176">The change token exposes:</span></span>

* <span data-ttu-id="0652d-177"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> bir değişikliğin oluşup gerçekleşmediğine yönelik olarak incelenebilir bir özellik &ndash;.</span><span class="sxs-lookup"><span data-stu-id="0652d-177"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="0652d-178">Belirtilen yol dizesinde değişiklikler algılandığında <xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; çağırılır.</span><span class="sxs-lookup"><span data-stu-id="0652d-178"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="0652d-179">Her değişiklik belirteci yalnızca, ilişkili geri çağırma işlemini tek bir değişikliğe yanıt olarak çağırır.</span><span class="sxs-lookup"><span data-stu-id="0652d-179">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="0652d-180">Sabit izlemeyi etkinleştirmek için bir <xref:System.Threading.Tasks.TaskCompletionSource`1> kullanın (aşağıda gösterildiği gibi) veya değişikliklere yanıt olarak `IChangeToken` örnekleri yeniden oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0652d-180">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="0652d-181">Örnek uygulamada, *WatchConsole* konsol uygulaması bir metin dosyası her değiştirildiğinde bir ileti görüntüleyecek şekilde yapılandırılmıştır:</span><span class="sxs-lookup"><span data-stu-id="0652d-181">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/3.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="0652d-182">Docker kapsayıcıları ve ağ paylaşımları gibi bazı dosya sistemleri, değişiklik bildirimlerini güvenilir bir şekilde gönderemeyebilir.</span><span class="sxs-lookup"><span data-stu-id="0652d-182">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="0652d-183">Dosya sistemini her dört saniyede bir değişiklikler için yoklamak için `DOTNET_USE_POLLING_FILE_WATCHER` ortam değişkenini `1` veya `true` olarak ayarlayın (yapılandırılamaz).</span><span class="sxs-lookup"><span data-stu-id="0652d-183">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="0652d-184">Glob desenleri</span><span class="sxs-lookup"><span data-stu-id="0652d-184">Glob patterns</span></span>

<span data-ttu-id="0652d-185">Dosya sistemi yolları, *Glob (veya glob) desenleri*adlı joker karakter desenleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="0652d-185">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="0652d-186">Bu desenlerle dosya gruplarını belirtin.</span><span class="sxs-lookup"><span data-stu-id="0652d-186">Specify groups of files with these patterns.</span></span> <span data-ttu-id="0652d-187">İki joker karakter `*` ve `**`:</span><span class="sxs-lookup"><span data-stu-id="0652d-187">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="0652d-188">Geçerli klasör düzeyindeki her şeyi, dosya adını veya herhangi bir dosya uzantısını eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="0652d-188">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="0652d-189">Eşleşmeler dosya yolundaki `/` ve `.` karakterleri ile sonlandırılır.</span><span class="sxs-lookup"><span data-stu-id="0652d-189">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="0652d-190">Birden çok dizin düzeyindeki tüm öğeleri eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="0652d-190">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="0652d-191">, Bir Dizin hiyerarşisinde birçok dosya yinelemeli olarak eşleşmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0652d-191">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="0652d-192">**Glob deseninin örnekleri**</span><span class="sxs-lookup"><span data-stu-id="0652d-192">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="0652d-193">Belirli bir dizindeki belirli bir dosyayla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="0652d-193">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="0652d-194">Belirli bir dizinde *. txt* uzantılı tüm dosyaları eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="0652d-194">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="0652d-195">Dizinler içindeki tüm `appsettings.json` dosyalarla *Dizin* klasörünün altında tam olarak bir düzey eşleşir.</span><span class="sxs-lookup"><span data-stu-id="0652d-195">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="0652d-196">*. Txt* uzantılı tüm dosyaları, *Dizin* klasörünün altında herhangi bir yerde buldu.</span><span class="sxs-lookup"><span data-stu-id="0652d-196">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0652d-197">ASP.NET Core dosya sistemi erişimini dosya sağlayıcılarının kullanımı üzerinden soyutlar.</span><span class="sxs-lookup"><span data-stu-id="0652d-197">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="0652d-198">Dosya sağlayıcıları ASP.NET Core Framework boyunca kullanılır:</span><span class="sxs-lookup"><span data-stu-id="0652d-198">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="0652d-199"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment>, uygulamanın [içerik kökünü](xref:fundamentals/index#content-root) ve [Web kökünü](xref:fundamentals/index#web-root) `IFileProvider` türleri olarak kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="0652d-199"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> exposes the app's [content root](xref:fundamentals/index#content-root) and [web root](xref:fundamentals/index#web-root) as `IFileProvider` types.</span></span>
* <span data-ttu-id="0652d-200">[Statik dosya ara yazılımı](xref:fundamentals/static-files) , statik dosyaları bulmak Için dosya sağlayıcılarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="0652d-200">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="0652d-201">[Razor](xref:mvc/views/razor) , sayfa ve görünümleri bulmak Için dosya sağlayıcılarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="0652d-201">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="0652d-202">.NET Core araçları, hangi dosyaların yayımlanacak olduğunu belirlemek için dosya sağlayıcılarını ve glob düzenlerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="0652d-202">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="0652d-203">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0652d-203">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="0652d-204">Dosya sağlayıcısı arabirimleri</span><span class="sxs-lookup"><span data-stu-id="0652d-204">File Provider interfaces</span></span>

<span data-ttu-id="0652d-205">Birincil arabirim <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="0652d-205">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="0652d-206">`IFileProvider` şu yöntemleri sunar:</span><span class="sxs-lookup"><span data-stu-id="0652d-206">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="0652d-207">Dosya bilgilerini edinin (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span><span class="sxs-lookup"><span data-stu-id="0652d-207">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="0652d-208">Dizin bilgilerini al (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span><span class="sxs-lookup"><span data-stu-id="0652d-208">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="0652d-209">Değişiklik bildirimlerini ayarlayın (<xref:Microsoft.Extensions.Primitives.IChangeToken>kullanarak).</span><span class="sxs-lookup"><span data-stu-id="0652d-209">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="0652d-210">`IFileInfo` dosyalarla çalışma için yöntemler ve özellikler sağlar:</span><span class="sxs-lookup"><span data-stu-id="0652d-210">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="0652d-211"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (bayt)</span><span class="sxs-lookup"><span data-stu-id="0652d-211"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="0652d-212"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> tarihi</span><span class="sxs-lookup"><span data-stu-id="0652d-212"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="0652d-213">[Ifileınfo. CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) yöntemini kullanarak dosyadan okuma yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0652d-213">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="0652d-214">Örnek uygulama, [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla uygulama genelinde kullanılmak üzere `Startup.ConfigureServices` bir dosya sağlayıcısının nasıl yapılandırılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="0652d-214">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="0652d-215">Dosya sağlayıcısı uygulamaları</span><span class="sxs-lookup"><span data-stu-id="0652d-215">File Provider implementations</span></span>

<span data-ttu-id="0652d-216">`IFileProvider` üç uygulaması mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="0652d-216">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="0652d-217">Uygulama</span><span class="sxs-lookup"><span data-stu-id="0652d-217">Implementation</span></span> | <span data-ttu-id="0652d-218">Açıklama</span><span class="sxs-lookup"><span data-stu-id="0652d-218">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="0652d-219">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="0652d-219">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="0652d-220">Fiziksel sağlayıcı, sistemin fiziksel dosyalarına erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0652d-220">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="0652d-221">Bildirimli Estembeddedfileprovider</span><span class="sxs-lookup"><span data-stu-id="0652d-221">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="0652d-222">Bildirimde yerleşik olarak bulunan dosyalara erişmek için bildirim eklenmiş sağlayıcı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0652d-222">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="0652d-223">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="0652d-223">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="0652d-224">Bileşik sağlayıcı, bir veya daha fazla sağlayıcıdan gelen dosyalara ve dizinlere Birleşik erişim sağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0652d-224">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="0652d-225">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="0652d-225">PhysicalFileProvider</span></span>

<span data-ttu-id="0652d-226"><xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, fiziksel dosya sistemine erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="0652d-226">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="0652d-227">`PhysicalFileProvider`, <xref:System.IO.File?displayProperty=fullName> türünü (fiziksel sağlayıcı için) ve tüm yolları bir dizine ve alt öğelerine kullanır.</span><span class="sxs-lookup"><span data-stu-id="0652d-227">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="0652d-228">Bu kapsam, belirtilen dizin ve alt öğeleri dışındaki dosya sistemine erişimi engeller.</span><span class="sxs-lookup"><span data-stu-id="0652d-228">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="0652d-229">`PhysicalFileProvider` oluşturmak ve kullanmak için en yaygın senaryo, [bağımlılık ekleme](xref:fundamentals/dependency-injection)aracılığıyla oluşturucuda bir `IFileProvider` istemaktır.</span><span class="sxs-lookup"><span data-stu-id="0652d-229">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="0652d-230">Bu sağlayıcıyı doğrudan örnekleyen bir dizin yolu gereklidir ve sağlayıcı kullanılarak yapılan tüm isteklerin temel yolu olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="0652d-230">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="0652d-231">Aşağıdaki kod, `PhysicalFileProvider` oluşturma ve dizin içeriğini ve dosya bilgilerini elde etmek için nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="0652d-231">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="0652d-232">Önceki örnekteki türler:</span><span class="sxs-lookup"><span data-stu-id="0652d-232">Types in the preceding example:</span></span>

* <span data-ttu-id="0652d-233">`provider` bir `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="0652d-233">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="0652d-234">`contents` bir `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="0652d-234">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="0652d-235">`fileInfo` bir `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="0652d-235">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="0652d-236">Dosya sağlayıcısı, `applicationRoot` tarafından belirtilen dizin üzerinden yinelemek veya bir dosyanın bilgilerini almak için `GetFileInfo` çağırmak üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0652d-236">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="0652d-237">Dosya sağlayıcısının `applicationRoot` Directory dışında erişimi yok.</span><span class="sxs-lookup"><span data-stu-id="0652d-237">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="0652d-238">Örnek uygulama, [ıhostingenvironment. ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider)kullanarak sağlayıcıyı uygulamanın `Startup.ConfigureServices` sınıfında oluşturur:</span><span class="sxs-lookup"><span data-stu-id="0652d-238">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="0652d-239">Bildirimli Estembeddedfileprovider</span><span class="sxs-lookup"><span data-stu-id="0652d-239">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="0652d-240"><xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider>, derlemeler içine katıştırılmış dosyalara erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0652d-240">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="0652d-241">`ManifestEmbeddedFileProvider` gömülü dosyaların özgün yollarını yeniden oluşturmak için derlemeye derlenen bir bildirim kullanır.</span><span class="sxs-lookup"><span data-stu-id="0652d-241">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="0652d-242">Katıştırılmış dosyaların bir bildirimini oluşturmak için `<GenerateEmbeddedFilesManifest>` özelliğini `true`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="0652d-242">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="0652d-243">[&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects)eklemek için dosyaları belirtin:</span><span class="sxs-lookup"><span data-stu-id="0652d-243">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="0652d-244">Derlemeye eklemek üzere bir veya daha fazla dosya belirtmek için [Glob desenlerini](#glob-patterns) kullanın.</span><span class="sxs-lookup"><span data-stu-id="0652d-244">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="0652d-245">Örnek uygulama bir `ManifestEmbeddedFileProvider` oluşturur ve şu anda yürütülen derlemeyi oluşturucuya geçirir.</span><span class="sxs-lookup"><span data-stu-id="0652d-245">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="0652d-246">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0652d-246">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(typeof(Program).Assembly);
```

<span data-ttu-id="0652d-247">Ek aşırı yüklemeler şunları yapmanıza olanak sağlar:</span><span class="sxs-lookup"><span data-stu-id="0652d-247">Additional overloads allow you to:</span></span>

* <span data-ttu-id="0652d-248">Göreli bir dosya yolu belirtin.</span><span class="sxs-lookup"><span data-stu-id="0652d-248">Specify a relative file path.</span></span>
* <span data-ttu-id="0652d-249">Dosya kapsamını son değiştirilme tarihine kadar.</span><span class="sxs-lookup"><span data-stu-id="0652d-249">Scope files to a last modified date.</span></span>
* <span data-ttu-id="0652d-250">Katıştırılmış dosya bildirimini içeren gömülü kaynağı adlandırın.</span><span class="sxs-lookup"><span data-stu-id="0652d-250">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="0652d-251">Yüklemek</span><span class="sxs-lookup"><span data-stu-id="0652d-251">Overload</span></span> | <span data-ttu-id="0652d-252">Açıklama</span><span class="sxs-lookup"><span data-stu-id="0652d-252">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="0652d-253">İsteğe bağlı `root` göreli yol parametresini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="0652d-253">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="0652d-254">Belirtilen yol altında bu kaynaklara <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> yapılan çağrıların kapsamını belirlemek için `root` belirtin.</span><span class="sxs-lookup"><span data-stu-id="0652d-254">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="0652d-255">İsteğe bağlı `root` göreli yol parametresini ve `lastModified` Tarih (<xref:System.DateTimeOffset>) parametresini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="0652d-255">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="0652d-256">`lastModified` tarihi, <xref:Microsoft.Extensions.FileProviders.IFileProvider>tarafından döndürülen <xref:Microsoft.Extensions.FileProviders.IFileInfo> örneklerinin son değiştirilme tarihini kapsamlar.</span><span class="sxs-lookup"><span data-stu-id="0652d-256">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="0652d-257">İsteğe bağlı `root` göreli yolunu, `lastModified` tarihini ve `manifestName` parametrelerini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="0652d-257">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="0652d-258">`manifestName`, bildirimi içeren gömülü kaynağın adını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="0652d-258">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="0652d-259">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="0652d-259">CompositeFileProvider</span></span>

<span data-ttu-id="0652d-260"><xref:Microsoft.Extensions.FileProviders.CompositeFileProvider>, birden fazla sağlayıcıdan dosyalarla çalışmak için tek bir arabirim ortaya çıkaran `IFileProvider` örneklerini birleştirir.</span><span class="sxs-lookup"><span data-stu-id="0652d-260">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="0652d-261">`CompositeFileProvider`oluştururken, bir veya daha fazla `IFileProvider` örneğini oluşturucusuna geçirin.</span><span class="sxs-lookup"><span data-stu-id="0652d-261">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="0652d-262">Örnek uygulamada, bir `PhysicalFileProvider` ve bir `ManifestEmbeddedFileProvider`, uygulamanın hizmet kapsayıcısına kayıtlı bir `CompositeFileProvider` dosya sağlar:</span><span class="sxs-lookup"><span data-stu-id="0652d-262">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="0652d-263">Değişiklikleri izle</span><span class="sxs-lookup"><span data-stu-id="0652d-263">Watch for changes</span></span>

<span data-ttu-id="0652d-264">[IFileProvider. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) yöntemi, değişiklikler için bir veya daha fazla dosya ya da dizin izlemek üzere bir senaryo sağlar.</span><span class="sxs-lookup"><span data-stu-id="0652d-264">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="0652d-265">`Watch`, birden çok dosya belirtmek için [Glob desenlerini](#glob-patterns) içerebilen bir yol dizesi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="0652d-265">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="0652d-266">`Watch` <xref:Microsoft.Extensions.Primitives.IChangeToken>döndürür.</span><span class="sxs-lookup"><span data-stu-id="0652d-266">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="0652d-267">Değişiklik belirteci şunları gösterir:</span><span class="sxs-lookup"><span data-stu-id="0652d-267">The change token exposes:</span></span>

* <span data-ttu-id="0652d-268"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> bir değişikliğin oluşup gerçekleşmediğine yönelik olarak incelenebilir bir özellik &ndash;.</span><span class="sxs-lookup"><span data-stu-id="0652d-268"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="0652d-269">Belirtilen yol dizesinde değişiklikler algılandığında <xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; çağırılır.</span><span class="sxs-lookup"><span data-stu-id="0652d-269"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="0652d-270">Her değişiklik belirteci yalnızca, ilişkili geri çağırma işlemini tek bir değişikliğe yanıt olarak çağırır.</span><span class="sxs-lookup"><span data-stu-id="0652d-270">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="0652d-271">Sabit izlemeyi etkinleştirmek için bir <xref:System.Threading.Tasks.TaskCompletionSource`1> kullanın (aşağıda gösterildiği gibi) veya değişikliklere yanıt olarak `IChangeToken` örnekleri yeniden oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0652d-271">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="0652d-272">Örnek uygulamada, *WatchConsole* konsol uygulaması bir metin dosyası her değiştirildiğinde bir ileti görüntüleyecek şekilde yapılandırılmıştır:</span><span class="sxs-lookup"><span data-stu-id="0652d-272">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="0652d-273">Docker kapsayıcıları ve ağ paylaşımları gibi bazı dosya sistemleri, değişiklik bildirimlerini güvenilir bir şekilde gönderemeyebilir.</span><span class="sxs-lookup"><span data-stu-id="0652d-273">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="0652d-274">Dosya sistemini her dört saniyede bir değişiklikler için yoklamak için `DOTNET_USE_POLLING_FILE_WATCHER` ortam değişkenini `1` veya `true` olarak ayarlayın (yapılandırılamaz).</span><span class="sxs-lookup"><span data-stu-id="0652d-274">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="0652d-275">Glob desenleri</span><span class="sxs-lookup"><span data-stu-id="0652d-275">Glob patterns</span></span>

<span data-ttu-id="0652d-276">Dosya sistemi yolları, *Glob (veya glob) desenleri*adlı joker karakter desenleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="0652d-276">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="0652d-277">Bu desenlerle dosya gruplarını belirtin.</span><span class="sxs-lookup"><span data-stu-id="0652d-277">Specify groups of files with these patterns.</span></span> <span data-ttu-id="0652d-278">İki joker karakter `*` ve `**`:</span><span class="sxs-lookup"><span data-stu-id="0652d-278">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="0652d-279">Geçerli klasör düzeyindeki her şeyi, dosya adını veya herhangi bir dosya uzantısını eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="0652d-279">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="0652d-280">Eşleşmeler dosya yolundaki `/` ve `.` karakterleri ile sonlandırılır.</span><span class="sxs-lookup"><span data-stu-id="0652d-280">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="0652d-281">Birden çok dizin düzeyindeki tüm öğeleri eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="0652d-281">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="0652d-282">, Bir Dizin hiyerarşisinde birçok dosya yinelemeli olarak eşleşmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0652d-282">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="0652d-283">**Glob deseninin örnekleri**</span><span class="sxs-lookup"><span data-stu-id="0652d-283">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="0652d-284">Belirli bir dizindeki belirli bir dosyayla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="0652d-284">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="0652d-285">Belirli bir dizinde *. txt* uzantılı tüm dosyaları eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="0652d-285">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="0652d-286">Dizinler içindeki tüm `appsettings.json` dosyalarla *Dizin* klasörünün altında tam olarak bir düzey eşleşir.</span><span class="sxs-lookup"><span data-stu-id="0652d-286">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="0652d-287">*. Txt* uzantılı tüm dosyaları, *Dizin* klasörünün altında herhangi bir yerde buldu.</span><span class="sxs-lookup"><span data-stu-id="0652d-287">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end
