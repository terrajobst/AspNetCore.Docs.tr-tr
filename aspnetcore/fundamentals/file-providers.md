---
title: ASP.NET Core dosya sağlayıcıları
author: guardrex
description: Dosya sağlayıcılarının kullanımı aracılığıyla dosya sistemi erişimini ASP.NET Core nasıl soyutleyeceğinizi öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: fundamentals/file-providers
ms.openlocfilehash: 3a92b44efc70d156596ee9fe80b4f6a65266e73d
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007170"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="5f131-103">ASP.NET Core dosya sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="5f131-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="5f131-104">[Steve Smith](https://ardalis.com/) ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="5f131-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5f131-105">ASP.NET Core dosya sistemi erişimini dosya sağlayıcılarının kullanımı üzerinden soyutlar.</span><span class="sxs-lookup"><span data-stu-id="5f131-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="5f131-106">Dosya sağlayıcıları ASP.NET Core Framework boyunca kullanılır:</span><span class="sxs-lookup"><span data-stu-id="5f131-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="5f131-107">`IWebHostEnvironment`, uygulamanın [içerik kökünü](xref:fundamentals/index#content-root) ve [Web kökünü](xref:fundamentals/index#web-root) `IFileProvider` türleri olarak gösterir.</span><span class="sxs-lookup"><span data-stu-id="5f131-107">`IWebHostEnvironment` exposes the app's [content root](xref:fundamentals/index#content-root) and [web root](xref:fundamentals/index#web-root) as `IFileProvider` types.</span></span>
* <span data-ttu-id="5f131-108">[Statik dosya ara yazılımı](xref:fundamentals/static-files) , statik dosyaları bulmak Için dosya sağlayıcılarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="5f131-108">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="5f131-109">[Razor](xref:mvc/views/razor) , sayfa ve görünümleri bulmak Için dosya sağlayıcılarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="5f131-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="5f131-110">.NET Core araçları, hangi dosyaların yayımlanacak olduğunu belirlemek için dosya sağlayıcılarını ve glob düzenlerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="5f131-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="5f131-111">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5f131-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="5f131-112">Dosya sağlayıcısı arabirimleri</span><span class="sxs-lookup"><span data-stu-id="5f131-112">File Provider interfaces</span></span>

<span data-ttu-id="5f131-113">Birincil arabirim <xref:Microsoft.Extensions.FileProviders.IFileProvider> ' dır.</span><span class="sxs-lookup"><span data-stu-id="5f131-113">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="5f131-114">`IFileProvider` şu yöntemleri sunar:</span><span class="sxs-lookup"><span data-stu-id="5f131-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="5f131-115">Dosya bilgilerini edinin (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span><span class="sxs-lookup"><span data-stu-id="5f131-115">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="5f131-116">Dizin bilgilerini al (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span><span class="sxs-lookup"><span data-stu-id="5f131-116">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="5f131-117">Değişiklik bildirimlerini ayarlayın (<xref:Microsoft.Extensions.Primitives.IChangeToken> kullanarak).</span><span class="sxs-lookup"><span data-stu-id="5f131-117">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="5f131-118">`IFileInfo`, dosyalarla çalışma için yöntemler ve özellikler sağlar:</span><span class="sxs-lookup"><span data-stu-id="5f131-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="5f131-119"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (bayt)</span><span class="sxs-lookup"><span data-stu-id="5f131-119"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="5f131-120"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> tarihi</span><span class="sxs-lookup"><span data-stu-id="5f131-120"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="5f131-121">[Ifileınfo. CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) yöntemini kullanarak dosyadan okuma yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f131-121">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="5f131-122">Örnek uygulama, [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla uygulama genelinde kullanılmak üzere `Startup.ConfigureServices` ' da bir dosya sağlayıcısının nasıl yapılandırılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="5f131-122">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="5f131-123">Dosya sağlayıcısı uygulamaları</span><span class="sxs-lookup"><span data-stu-id="5f131-123">File Provider implementations</span></span>

<span data-ttu-id="5f131-124">@No__t-0 ' ın üç uygulaması kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5f131-124">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="5f131-125">Uygulama</span><span class="sxs-lookup"><span data-stu-id="5f131-125">Implementation</span></span> | <span data-ttu-id="5f131-126">Açıklama</span><span class="sxs-lookup"><span data-stu-id="5f131-126">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="5f131-127">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="5f131-127">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="5f131-128">Fiziksel sağlayıcı, sistemin fiziksel dosyalarına erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5f131-128">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="5f131-129">Bildirimli Estembeddedfileprovider</span><span class="sxs-lookup"><span data-stu-id="5f131-129">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="5f131-130">Bildirimde yerleşik olarak bulunan dosyalara erişmek için bildirim eklenmiş sağlayıcı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5f131-130">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="5f131-131">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="5f131-131">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="5f131-132">Bileşik sağlayıcı, bir veya daha fazla sağlayıcıdan gelen dosyalara ve dizinlere Birleşik erişim sağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5f131-132">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="5f131-133">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="5f131-133">PhysicalFileProvider</span></span>

<span data-ttu-id="5f131-134">@No__t-0, fiziksel dosya sistemine erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="5f131-134">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="5f131-135">`PhysicalFileProvider` <xref:System.IO.File?displayProperty=fullName> türünü (fiziksel sağlayıcı için) kullanır ve tüm yolları bir dizin ve alt öğeleri için kapsamlar.</span><span class="sxs-lookup"><span data-stu-id="5f131-135">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="5f131-136">Bu kapsam, belirtilen dizin ve alt öğeleri dışındaki dosya sistemine erişimi engeller.</span><span class="sxs-lookup"><span data-stu-id="5f131-136">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="5f131-137">@No__t-0 oluşturma ve kullanma için en yaygın senaryo, [bağımlılık ekleme](xref:fundamentals/dependency-injection)aracılığıyla oluşturucuda bir `IFileProvider` isteğidir.</span><span class="sxs-lookup"><span data-stu-id="5f131-137">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="5f131-138">Bu sağlayıcıyı doğrudan örnekleyen bir dizin yolu gereklidir ve sağlayıcı kullanılarak yapılan tüm isteklerin temel yolu olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="5f131-138">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="5f131-139">Aşağıdaki kod, `PhysicalFileProvider` oluşturma ve dizin içeriğini ve dosya bilgilerini elde etmek için nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="5f131-139">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="5f131-140">Önceki örnekteki türler:</span><span class="sxs-lookup"><span data-stu-id="5f131-140">Types in the preceding example:</span></span>

* <span data-ttu-id="5f131-141">`provider`, bir `IFileProvider` ' dir.</span><span class="sxs-lookup"><span data-stu-id="5f131-141">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="5f131-142">`contents`, bir `IDirectoryContents` ' dir.</span><span class="sxs-lookup"><span data-stu-id="5f131-142">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="5f131-143">`fileInfo`, bir `IFileInfo` ' dir.</span><span class="sxs-lookup"><span data-stu-id="5f131-143">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="5f131-144">Dosya sağlayıcısı, bir dosyanın bilgilerini almak için `applicationRoot` tarafından belirtilen dizin üzerinden yinelemek veya `GetFileInfo` ' i çağırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5f131-144">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="5f131-145">Dosya sağlayıcısının `applicationRoot` dizininin dışında erişimi yok.</span><span class="sxs-lookup"><span data-stu-id="5f131-145">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="5f131-146">Örnek uygulama, [ıhostingenvironment. ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider)kullanarak, uygulamanın `Startup.ConfigureServices` sınıfında oluşturduğu sağlayıcıyı oluşturur:</span><span class="sxs-lookup"><span data-stu-id="5f131-146">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="5f131-147">Bildirimli Estembeddedfileprovider</span><span class="sxs-lookup"><span data-stu-id="5f131-147">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="5f131-148">@No__t-0, derlemeler içine katıştırılmış dosyalara erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5f131-148">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="5f131-149">@No__t-0, gömülü dosyaların özgün yollarını yeniden oluşturmak için derlemeye derlenen bir bildirim kullanır.</span><span class="sxs-lookup"><span data-stu-id="5f131-149">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="5f131-150">[Microsoft. Extensions. FileProviders. Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) paketi için projeye bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5f131-150">Add a package reference to the project for the [Microsoft.Extensions.FileProviders.Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) package.</span></span>

<span data-ttu-id="5f131-151">Katıştırılmış dosyaların bir bildirimini oluşturmak için `<GenerateEmbeddedFilesManifest>` özelliğini `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="5f131-151">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="5f131-152">[@No__t-1EmbeddedResource >](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects)eklemek için dosyaları belirtin:</span><span class="sxs-lookup"><span data-stu-id="5f131-152">Specify the files to embed with [\<EmbeddedResource>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="5f131-153">Derlemeye eklemek üzere bir veya daha fazla dosya belirtmek için [Glob desenlerini](#glob-patterns) kullanın.</span><span class="sxs-lookup"><span data-stu-id="5f131-153">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="5f131-154">Örnek uygulama bir `ManifestEmbeddedFileProvider` oluşturur ve şu anda yürütülen derlemeyi oluşturucuya geçirir.</span><span class="sxs-lookup"><span data-stu-id="5f131-154">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="5f131-155">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5f131-155">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="5f131-156">Ek aşırı yüklemeler şunları yapmanıza olanak sağlar:</span><span class="sxs-lookup"><span data-stu-id="5f131-156">Additional overloads allow you to:</span></span>

* <span data-ttu-id="5f131-157">Göreli bir dosya yolu belirtin.</span><span class="sxs-lookup"><span data-stu-id="5f131-157">Specify a relative file path.</span></span>
* <span data-ttu-id="5f131-158">Dosya kapsamını son değiştirilme tarihine kadar.</span><span class="sxs-lookup"><span data-stu-id="5f131-158">Scope files to a last modified date.</span></span>
* <span data-ttu-id="5f131-159">Katıştırılmış dosya bildirimini içeren gömülü kaynağı adlandırın.</span><span class="sxs-lookup"><span data-stu-id="5f131-159">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="5f131-160">Yüklemek</span><span class="sxs-lookup"><span data-stu-id="5f131-160">Overload</span></span> | <span data-ttu-id="5f131-161">Açıklama</span><span class="sxs-lookup"><span data-stu-id="5f131-161">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="5f131-162">İsteğe bağlı @no__t 0 göreli yol parametresini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="5f131-162">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="5f131-163">Belirtilen yol altındaki kaynaklarla <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> ' e yapılan çağrıların kapsamını belirlemek için `root` belirtin.</span><span class="sxs-lookup"><span data-stu-id="5f131-163">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="5f131-164">İsteğe bağlı @no__t 0 göreli yol parametresini ve `lastModified` Tarih (<xref:System.DateTimeOffset>) parametresini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="5f131-164">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="5f131-165">@No__t-0 tarihi kapsamları <xref:Microsoft.Extensions.FileProviders.IFileProvider> tarafından döndürülen <xref:Microsoft.Extensions.FileProviders.IFileInfo> örnekleri için son değiştirilme tarihi.</span><span class="sxs-lookup"><span data-stu-id="5f131-165">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="5f131-166">İsteğe bağlı @no__t 0 göreli yolunu, `lastModified` tarihini ve `manifestName` parametrelerini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="5f131-166">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="5f131-167">@No__t-0, bildirimi içeren gömülü kaynağın adını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="5f131-167">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="5f131-168">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="5f131-168">CompositeFileProvider</span></span>

<span data-ttu-id="5f131-169">@No__t-0, birden fazla sağlayıcıdan dosyalarla çalışmak için tek bir arabirim ortaya çıkaran `IFileProvider` örneklerini birleştirir.</span><span class="sxs-lookup"><span data-stu-id="5f131-169">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="5f131-170">@No__t-0 ' ı oluştururken bir veya daha fazla `IFileProvider` örneği oluşturucuya geçirin.</span><span class="sxs-lookup"><span data-stu-id="5f131-170">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="5f131-171">Örnek uygulamada, bir `PhysicalFileProvider` ve bir `ManifestEmbeddedFileProvider`, uygulamanın hizmet kapsayıcısına kaydedilen bir `CompositeFileProvider` ' ye dosya sağlar:</span><span class="sxs-lookup"><span data-stu-id="5f131-171">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="5f131-172">Değişiklikleri izle</span><span class="sxs-lookup"><span data-stu-id="5f131-172">Watch for changes</span></span>

<span data-ttu-id="5f131-173">[IFileProvider. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) yöntemi, değişiklikler için bir veya daha fazla dosya ya da dizin izlemek üzere bir senaryo sağlar.</span><span class="sxs-lookup"><span data-stu-id="5f131-173">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="5f131-174">`Watch`, birden çok dosya belirtmek için [Glob desenlerini](#glob-patterns) içerebilen bir yol dizesi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="5f131-174">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="5f131-175">`Watch` <xref:Microsoft.Extensions.Primitives.IChangeToken> döndürür.</span><span class="sxs-lookup"><span data-stu-id="5f131-175">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="5f131-176">Değişiklik belirteci şunları gösterir:</span><span class="sxs-lookup"><span data-stu-id="5f131-176">The change token exposes:</span></span>

* <span data-ttu-id="5f131-177"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; bir değişikliğin oluşup oluşmadığını tespit etmek üzere incelenebilir bir özellik.</span><span class="sxs-lookup"><span data-stu-id="5f131-177"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="5f131-178"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash;, belirtilen yol dizesinde değişiklikler algılandığında çağırılır.</span><span class="sxs-lookup"><span data-stu-id="5f131-178"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="5f131-179">Her değişiklik belirteci yalnızca, ilişkili geri çağırma işlemini tek bir değişikliğe yanıt olarak çağırır.</span><span class="sxs-lookup"><span data-stu-id="5f131-179">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="5f131-180">Sabit izlemeyi etkinleştirmek için bir <xref:System.Threading.Tasks.TaskCompletionSource`1> kullanın (aşağıda gösterildiği gibi) veya değişikliklere yanıt olarak `IChangeToken` örnekleri yeniden oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5f131-180">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="5f131-181">Örnek uygulamada, *WatchConsole* konsol uygulaması bir metin dosyası her değiştirildiğinde bir ileti görüntüleyecek şekilde yapılandırılmıştır:</span><span class="sxs-lookup"><span data-stu-id="5f131-181">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/3.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="5f131-182">Docker kapsayıcıları ve ağ paylaşımları gibi bazı dosya sistemleri, değişiklik bildirimlerini güvenilir bir şekilde gönderemeyebilir.</span><span class="sxs-lookup"><span data-stu-id="5f131-182">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="5f131-183">Dosya sistemini her dört saniyede bir değişiklikler için yoklamak için `DOTNET_USE_POLLING_FILE_WATCHER` ortam değişkenini `1` veya `true` olarak ayarlayın (yapılandırılamaz).</span><span class="sxs-lookup"><span data-stu-id="5f131-183">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="5f131-184">Glob desenleri</span><span class="sxs-lookup"><span data-stu-id="5f131-184">Glob patterns</span></span>

<span data-ttu-id="5f131-185">Dosya sistemi yolları, *Glob (veya glob) desenleri*adlı joker karakter desenleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="5f131-185">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="5f131-186">Bu desenlerle dosya gruplarını belirtin.</span><span class="sxs-lookup"><span data-stu-id="5f131-186">Specify groups of files with these patterns.</span></span> <span data-ttu-id="5f131-187">İki joker karakter `*` ve `**` ' dir:</span><span class="sxs-lookup"><span data-stu-id="5f131-187">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="5f131-188">Geçerli klasör düzeyindeki her şeyi, dosya adını veya herhangi bir dosya uzantısını eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="5f131-188">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="5f131-189">Eşleşmeler dosya yolundaki `/` ve `.` karakterleri ile sonlandırılır.</span><span class="sxs-lookup"><span data-stu-id="5f131-189">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="5f131-190">Birden çok dizin düzeyindeki tüm öğeleri eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="5f131-190">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="5f131-191">, Bir Dizin hiyerarşisinde birçok dosya yinelemeli olarak eşleşmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5f131-191">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="5f131-192">**Glob deseninin örnekleri**</span><span class="sxs-lookup"><span data-stu-id="5f131-192">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="5f131-193">Belirli bir dizindeki belirli bir dosyayla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="5f131-193">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="5f131-194">Belirli bir dizinde *. txt* uzantılı tüm dosyaları eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="5f131-194">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="5f131-195">Dizinler içindeki tüm `appsettings.json` dosyalarını, *Dizin* klasörünün altında tam olarak bir düzey eşler.</span><span class="sxs-lookup"><span data-stu-id="5f131-195">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="5f131-196">*. Txt* uzantılı tüm dosyaları, *Dizin* klasörünün altında herhangi bir yerde buldu.</span><span class="sxs-lookup"><span data-stu-id="5f131-196">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5f131-197">ASP.NET Core dosya sistemi erişimini dosya sağlayıcılarının kullanımı üzerinden soyutlar.</span><span class="sxs-lookup"><span data-stu-id="5f131-197">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="5f131-198">Dosya sağlayıcıları ASP.NET Core Framework boyunca kullanılır:</span><span class="sxs-lookup"><span data-stu-id="5f131-198">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="5f131-199"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment>, uygulamanın [içerik kökünü](xref:fundamentals/index#content-root) ve [Web kökünü](xref:fundamentals/index#web-root) `IFileProvider` türleri olarak gösterir.</span><span class="sxs-lookup"><span data-stu-id="5f131-199"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> exposes the app's [content root](xref:fundamentals/index#content-root) and [web root](xref:fundamentals/index#web-root) as `IFileProvider` types.</span></span>
* <span data-ttu-id="5f131-200">[Statik dosya ara yazılımı](xref:fundamentals/static-files) , statik dosyaları bulmak Için dosya sağlayıcılarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="5f131-200">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="5f131-201">[Razor](xref:mvc/views/razor) , sayfa ve görünümleri bulmak Için dosya sağlayıcılarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="5f131-201">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="5f131-202">.NET Core araçları, hangi dosyaların yayımlanacak olduğunu belirlemek için dosya sağlayıcılarını ve glob düzenlerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="5f131-202">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="5f131-203">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5f131-203">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="5f131-204">Dosya sağlayıcısı arabirimleri</span><span class="sxs-lookup"><span data-stu-id="5f131-204">File Provider interfaces</span></span>

<span data-ttu-id="5f131-205">Birincil arabirim <xref:Microsoft.Extensions.FileProviders.IFileProvider> ' dır.</span><span class="sxs-lookup"><span data-stu-id="5f131-205">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="5f131-206">`IFileProvider` şu yöntemleri sunar:</span><span class="sxs-lookup"><span data-stu-id="5f131-206">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="5f131-207">Dosya bilgilerini edinin (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span><span class="sxs-lookup"><span data-stu-id="5f131-207">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="5f131-208">Dizin bilgilerini al (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span><span class="sxs-lookup"><span data-stu-id="5f131-208">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="5f131-209">Değişiklik bildirimlerini ayarlayın (<xref:Microsoft.Extensions.Primitives.IChangeToken> kullanarak).</span><span class="sxs-lookup"><span data-stu-id="5f131-209">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="5f131-210">`IFileInfo`, dosyalarla çalışma için yöntemler ve özellikler sağlar:</span><span class="sxs-lookup"><span data-stu-id="5f131-210">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="5f131-211"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (bayt)</span><span class="sxs-lookup"><span data-stu-id="5f131-211"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="5f131-212"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> tarihi</span><span class="sxs-lookup"><span data-stu-id="5f131-212"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="5f131-213">[Ifileınfo. CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) yöntemini kullanarak dosyadan okuma yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f131-213">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="5f131-214">Örnek uygulama, [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla uygulama genelinde kullanılmak üzere `Startup.ConfigureServices` ' da bir dosya sağlayıcısının nasıl yapılandırılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="5f131-214">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="5f131-215">Dosya sağlayıcısı uygulamaları</span><span class="sxs-lookup"><span data-stu-id="5f131-215">File Provider implementations</span></span>

<span data-ttu-id="5f131-216">@No__t-0 ' ın üç uygulaması kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5f131-216">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="5f131-217">Uygulama</span><span class="sxs-lookup"><span data-stu-id="5f131-217">Implementation</span></span> | <span data-ttu-id="5f131-218">Açıklama</span><span class="sxs-lookup"><span data-stu-id="5f131-218">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="5f131-219">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="5f131-219">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="5f131-220">Fiziksel sağlayıcı, sistemin fiziksel dosyalarına erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5f131-220">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="5f131-221">Bildirimli Estembeddedfileprovider</span><span class="sxs-lookup"><span data-stu-id="5f131-221">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="5f131-222">Bildirimde yerleşik olarak bulunan dosyalara erişmek için bildirim eklenmiş sağlayıcı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5f131-222">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="5f131-223">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="5f131-223">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="5f131-224">Bileşik sağlayıcı, bir veya daha fazla sağlayıcıdan gelen dosyalara ve dizinlere Birleşik erişim sağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5f131-224">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="5f131-225">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="5f131-225">PhysicalFileProvider</span></span>

<span data-ttu-id="5f131-226">@No__t-0, fiziksel dosya sistemine erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="5f131-226">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="5f131-227">`PhysicalFileProvider` <xref:System.IO.File?displayProperty=fullName> türünü (fiziksel sağlayıcı için) kullanır ve tüm yolları bir dizin ve alt öğeleri için kapsamlar.</span><span class="sxs-lookup"><span data-stu-id="5f131-227">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="5f131-228">Bu kapsam, belirtilen dizin ve alt öğeleri dışındaki dosya sistemine erişimi engeller.</span><span class="sxs-lookup"><span data-stu-id="5f131-228">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="5f131-229">@No__t-0 oluşturma ve kullanma için en yaygın senaryo, [bağımlılık ekleme](xref:fundamentals/dependency-injection)aracılığıyla oluşturucuda bir `IFileProvider` isteğidir.</span><span class="sxs-lookup"><span data-stu-id="5f131-229">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="5f131-230">Bu sağlayıcıyı doğrudan örnekleyen bir dizin yolu gereklidir ve sağlayıcı kullanılarak yapılan tüm isteklerin temel yolu olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="5f131-230">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="5f131-231">Aşağıdaki kod, `PhysicalFileProvider` oluşturma ve dizin içeriğini ve dosya bilgilerini elde etmek için nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="5f131-231">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="5f131-232">Önceki örnekteki türler:</span><span class="sxs-lookup"><span data-stu-id="5f131-232">Types in the preceding example:</span></span>

* <span data-ttu-id="5f131-233">`provider`, bir `IFileProvider` ' dir.</span><span class="sxs-lookup"><span data-stu-id="5f131-233">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="5f131-234">`contents`, bir `IDirectoryContents` ' dir.</span><span class="sxs-lookup"><span data-stu-id="5f131-234">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="5f131-235">`fileInfo`, bir `IFileInfo` ' dir.</span><span class="sxs-lookup"><span data-stu-id="5f131-235">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="5f131-236">Dosya sağlayıcısı, bir dosyanın bilgilerini almak için `applicationRoot` tarafından belirtilen dizin üzerinden yinelemek veya `GetFileInfo` ' i çağırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5f131-236">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="5f131-237">Dosya sağlayıcısının `applicationRoot` dizininin dışında erişimi yok.</span><span class="sxs-lookup"><span data-stu-id="5f131-237">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="5f131-238">Örnek uygulama, [ıhostingenvironment. ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider)kullanarak, uygulamanın `Startup.ConfigureServices` sınıfında oluşturduğu sağlayıcıyı oluşturur:</span><span class="sxs-lookup"><span data-stu-id="5f131-238">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="5f131-239">Bildirimli Estembeddedfileprovider</span><span class="sxs-lookup"><span data-stu-id="5f131-239">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="5f131-240">@No__t-0, derlemeler içine katıştırılmış dosyalara erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5f131-240">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="5f131-241">@No__t-0, gömülü dosyaların özgün yollarını yeniden oluşturmak için derlemeye derlenen bir bildirim kullanır.</span><span class="sxs-lookup"><span data-stu-id="5f131-241">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="5f131-242">Katıştırılmış dosyaların bir bildirimini oluşturmak için `<GenerateEmbeddedFilesManifest>` özelliğini `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="5f131-242">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="5f131-243">[@No__t-1EmbeddedResource @ no__t-2](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects)ile eklenecek dosyaları belirtin:</span><span class="sxs-lookup"><span data-stu-id="5f131-243">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="5f131-244">Derlemeye eklemek üzere bir veya daha fazla dosya belirtmek için [Glob desenlerini](#glob-patterns) kullanın.</span><span class="sxs-lookup"><span data-stu-id="5f131-244">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="5f131-245">Örnek uygulama bir `ManifestEmbeddedFileProvider` oluşturur ve şu anda yürütülen derlemeyi oluşturucuya geçirir.</span><span class="sxs-lookup"><span data-stu-id="5f131-245">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="5f131-246">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5f131-246">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="5f131-247">Ek aşırı yüklemeler şunları yapmanıza olanak sağlar:</span><span class="sxs-lookup"><span data-stu-id="5f131-247">Additional overloads allow you to:</span></span>

* <span data-ttu-id="5f131-248">Göreli bir dosya yolu belirtin.</span><span class="sxs-lookup"><span data-stu-id="5f131-248">Specify a relative file path.</span></span>
* <span data-ttu-id="5f131-249">Dosya kapsamını son değiştirilme tarihine kadar.</span><span class="sxs-lookup"><span data-stu-id="5f131-249">Scope files to a last modified date.</span></span>
* <span data-ttu-id="5f131-250">Katıştırılmış dosya bildirimini içeren gömülü kaynağı adlandırın.</span><span class="sxs-lookup"><span data-stu-id="5f131-250">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="5f131-251">Yüklemek</span><span class="sxs-lookup"><span data-stu-id="5f131-251">Overload</span></span> | <span data-ttu-id="5f131-252">Açıklama</span><span class="sxs-lookup"><span data-stu-id="5f131-252">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="5f131-253">İsteğe bağlı @no__t 0 göreli yol parametresini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="5f131-253">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="5f131-254">Belirtilen yol altındaki kaynaklarla <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> ' e yapılan çağrıların kapsamını belirlemek için `root` belirtin.</span><span class="sxs-lookup"><span data-stu-id="5f131-254">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="5f131-255">İsteğe bağlı @no__t 0 göreli yol parametresini ve `lastModified` Tarih (<xref:System.DateTimeOffset>) parametresini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="5f131-255">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="5f131-256">@No__t-0 tarihi kapsamları <xref:Microsoft.Extensions.FileProviders.IFileProvider> tarafından döndürülen <xref:Microsoft.Extensions.FileProviders.IFileInfo> örnekleri için son değiştirilme tarihi.</span><span class="sxs-lookup"><span data-stu-id="5f131-256">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="5f131-257">İsteğe bağlı @no__t 0 göreli yolunu, `lastModified` tarihini ve `manifestName` parametrelerini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="5f131-257">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="5f131-258">@No__t-0, bildirimi içeren gömülü kaynağın adını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="5f131-258">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="5f131-259">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="5f131-259">CompositeFileProvider</span></span>

<span data-ttu-id="5f131-260">@No__t-0, birden fazla sağlayıcıdan dosyalarla çalışmak için tek bir arabirim ortaya çıkaran `IFileProvider` örneklerini birleştirir.</span><span class="sxs-lookup"><span data-stu-id="5f131-260">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="5f131-261">@No__t-0 ' ı oluştururken bir veya daha fazla `IFileProvider` örneği oluşturucuya geçirin.</span><span class="sxs-lookup"><span data-stu-id="5f131-261">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="5f131-262">Örnek uygulamada, bir `PhysicalFileProvider` ve bir `ManifestEmbeddedFileProvider`, uygulamanın hizmet kapsayıcısına kaydedilen bir `CompositeFileProvider` ' ye dosya sağlar:</span><span class="sxs-lookup"><span data-stu-id="5f131-262">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="5f131-263">Değişiklikleri izle</span><span class="sxs-lookup"><span data-stu-id="5f131-263">Watch for changes</span></span>

<span data-ttu-id="5f131-264">[IFileProvider. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) yöntemi, değişiklikler için bir veya daha fazla dosya ya da dizin izlemek üzere bir senaryo sağlar.</span><span class="sxs-lookup"><span data-stu-id="5f131-264">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="5f131-265">`Watch`, birden çok dosya belirtmek için [Glob desenlerini](#glob-patterns) içerebilen bir yol dizesi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="5f131-265">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="5f131-266">`Watch` <xref:Microsoft.Extensions.Primitives.IChangeToken> döndürür.</span><span class="sxs-lookup"><span data-stu-id="5f131-266">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="5f131-267">Değişiklik belirteci şunları gösterir:</span><span class="sxs-lookup"><span data-stu-id="5f131-267">The change token exposes:</span></span>

* <span data-ttu-id="5f131-268"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; bir değişikliğin oluşup oluşmadığını tespit etmek üzere incelenebilir bir özellik.</span><span class="sxs-lookup"><span data-stu-id="5f131-268"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="5f131-269"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash;, belirtilen yol dizesinde değişiklikler algılandığında çağırılır.</span><span class="sxs-lookup"><span data-stu-id="5f131-269"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="5f131-270">Her değişiklik belirteci yalnızca, ilişkili geri çağırma işlemini tek bir değişikliğe yanıt olarak çağırır.</span><span class="sxs-lookup"><span data-stu-id="5f131-270">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="5f131-271">Sabit izlemeyi etkinleştirmek için bir <xref:System.Threading.Tasks.TaskCompletionSource`1> kullanın (aşağıda gösterildiği gibi) veya değişikliklere yanıt olarak `IChangeToken` örnekleri yeniden oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5f131-271">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="5f131-272">Örnek uygulamada, *WatchConsole* konsol uygulaması bir metin dosyası her değiştirildiğinde bir ileti görüntüleyecek şekilde yapılandırılmıştır:</span><span class="sxs-lookup"><span data-stu-id="5f131-272">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="5f131-273">Docker kapsayıcıları ve ağ paylaşımları gibi bazı dosya sistemleri, değişiklik bildirimlerini güvenilir bir şekilde gönderemeyebilir.</span><span class="sxs-lookup"><span data-stu-id="5f131-273">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="5f131-274">Dosya sistemini her dört saniyede bir değişiklikler için yoklamak için `DOTNET_USE_POLLING_FILE_WATCHER` ortam değişkenini `1` veya `true` olarak ayarlayın (yapılandırılamaz).</span><span class="sxs-lookup"><span data-stu-id="5f131-274">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="5f131-275">Glob desenleri</span><span class="sxs-lookup"><span data-stu-id="5f131-275">Glob patterns</span></span>

<span data-ttu-id="5f131-276">Dosya sistemi yolları, *Glob (veya glob) desenleri*adlı joker karakter desenleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="5f131-276">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="5f131-277">Bu desenlerle dosya gruplarını belirtin.</span><span class="sxs-lookup"><span data-stu-id="5f131-277">Specify groups of files with these patterns.</span></span> <span data-ttu-id="5f131-278">İki joker karakter `*` ve `**` ' dir:</span><span class="sxs-lookup"><span data-stu-id="5f131-278">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="5f131-279">Geçerli klasör düzeyindeki her şeyi, dosya adını veya herhangi bir dosya uzantısını eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="5f131-279">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="5f131-280">Eşleşmeler dosya yolundaki `/` ve `.` karakterleri ile sonlandırılır.</span><span class="sxs-lookup"><span data-stu-id="5f131-280">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="5f131-281">Birden çok dizin düzeyindeki tüm öğeleri eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="5f131-281">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="5f131-282">, Bir Dizin hiyerarşisinde birçok dosya yinelemeli olarak eşleşmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5f131-282">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="5f131-283">**Glob deseninin örnekleri**</span><span class="sxs-lookup"><span data-stu-id="5f131-283">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="5f131-284">Belirli bir dizindeki belirli bir dosyayla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="5f131-284">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="5f131-285">Belirli bir dizinde *. txt* uzantılı tüm dosyaları eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="5f131-285">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="5f131-286">Dizinler içindeki tüm `appsettings.json` dosyalarını, *Dizin* klasörünün altında tam olarak bir düzey eşler.</span><span class="sxs-lookup"><span data-stu-id="5f131-286">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="5f131-287">*. Txt* uzantılı tüm dosyaları, *Dizin* klasörünün altında herhangi bir yerde buldu.</span><span class="sxs-lookup"><span data-stu-id="5f131-287">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end
