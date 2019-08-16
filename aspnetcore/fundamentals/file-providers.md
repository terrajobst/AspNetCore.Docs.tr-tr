---
title: ASP.NET Core dosya sağlayıcıları
author: guardrex
description: Dosya sağlayıcılarının kullanımı aracılığıyla dosya sistemi erişimini ASP.NET Core nasıl soyutleyeceğinizi öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/30/2019
uid: fundamentals/file-providers
ms.openlocfilehash: b93b2df7fad7c173f43ad69aec865f09de6c9c34
ms.sourcegitcommit: 7a46973998623aead757ad386fe33602b1658793
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69487586"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="6758d-103">ASP.NET Core dosya sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="6758d-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="6758d-104">[Steve Smith](https://ardalis.com/) ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="6758d-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6758d-105">ASP.NET Core dosya sistemi erişimini dosya sağlayıcılarının kullanımı üzerinden soyutlar.</span><span class="sxs-lookup"><span data-stu-id="6758d-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="6758d-106">Dosya sağlayıcıları ASP.NET Core Framework boyunca kullanılır:</span><span class="sxs-lookup"><span data-stu-id="6758d-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="6758d-107">[Ihostingenvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) , uygulamanın içerik kökünü ve Web kökünü türler olarak `IFileProvider` kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="6758d-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) exposes the app's content root and web root as `IFileProvider` types.</span></span>
* <span data-ttu-id="6758d-108">[Statik dosya ara yazılımı](xref:fundamentals/static-files) , statik dosyaları bulmak Için dosya sağlayıcılarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="6758d-108">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="6758d-109">[Razor](xref:mvc/views/razor) , sayfa ve görünümleri bulmak Için dosya sağlayıcılarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="6758d-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="6758d-110">.NET Core araçları, hangi dosyaların yayımlanacak olduğunu belirlemek için dosya sağlayıcılarını ve glob düzenlerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="6758d-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="6758d-111">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6758d-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="6758d-112">Dosya sağlayıcısı arabirimleri</span><span class="sxs-lookup"><span data-stu-id="6758d-112">File Provider interfaces</span></span>

<span data-ttu-id="6758d-113">Birincil arabirim [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)' dir.</span><span class="sxs-lookup"><span data-stu-id="6758d-113">The primary interface is [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> <span data-ttu-id="6758d-114">`IFileProvider`şunları yapmak için yöntemler sunar:</span><span class="sxs-lookup"><span data-stu-id="6758d-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="6758d-115">Dosya bilgilerini edinin ([ıfıleınfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span><span class="sxs-lookup"><span data-stu-id="6758d-115">Obtain file information ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span></span>
* <span data-ttu-id="6758d-116">Dizin bilgilerini ([ıdirectorycontents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)) alın.</span><span class="sxs-lookup"><span data-stu-id="6758d-116">Obtain directory information ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span></span>
* <span data-ttu-id="6758d-117">Değişiklik bildirimlerini ayarlayın (bir [ıchanneltoken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)kullanarak).</span><span class="sxs-lookup"><span data-stu-id="6758d-117">Set up change notifications (using an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span></span>

<span data-ttu-id="6758d-118">`IFileInfo`dosyalarla çalışma için yöntemler ve özellikler sağlar:</span><span class="sxs-lookup"><span data-stu-id="6758d-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* [<span data-ttu-id="6758d-119">Bulunur</span><span class="sxs-lookup"><span data-stu-id="6758d-119">Exists</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.exists)
* [<span data-ttu-id="6758d-120">IsDirectory</span><span class="sxs-lookup"><span data-stu-id="6758d-120">IsDirectory</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.isdirectory)
* [<span data-ttu-id="6758d-121">Ad</span><span class="sxs-lookup"><span data-stu-id="6758d-121">Name</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.name)
* <span data-ttu-id="6758d-122">[Uzunluk](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (bayt cinsinden)</span><span class="sxs-lookup"><span data-stu-id="6758d-122">[Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (in bytes)</span></span>
* <span data-ttu-id="6758d-123">[LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) tarihi</span><span class="sxs-lookup"><span data-stu-id="6758d-123">[LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) date</span></span>

<span data-ttu-id="6758d-124">[Ifileınfo. CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) yöntemini kullanarak dosyadan okuma yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6758d-124">You can read from the file using the [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) method.</span></span>

<span data-ttu-id="6758d-125">Örnek uygulama, uygulamasında `Startup.ConfigureServices` [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla uygulama genelinde kullanılmak üzere bir dosya sağlayıcısının nasıl yapılandırılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="6758d-125">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="6758d-126">Dosya sağlayıcısı uygulamaları</span><span class="sxs-lookup"><span data-stu-id="6758d-126">File Provider implementations</span></span>

<span data-ttu-id="6758d-127">Üç uygulaması `IFileProvider` mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="6758d-127">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="6758d-128">Uygulama</span><span class="sxs-lookup"><span data-stu-id="6758d-128">Implementation</span></span> | <span data-ttu-id="6758d-129">Açıklama</span><span class="sxs-lookup"><span data-stu-id="6758d-129">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="6758d-130">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="6758d-130">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="6758d-131">Fiziksel sağlayıcı, sistemin fiziksel dosyalarına erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6758d-131">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="6758d-132">Bildirimli Estembeddedfileprovider</span><span class="sxs-lookup"><span data-stu-id="6758d-132">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="6758d-133">Bildirimde yerleşik olarak bulunan dosyalara erişmek için bildirim eklenmiş sağlayıcı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6758d-133">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="6758d-134">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="6758d-134">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="6758d-135">Bileşik sağlayıcı, bir veya daha fazla sağlayıcıdan gelen dosyalara ve dizinlere Birleşik erişim sağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6758d-135">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="6758d-136">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="6758d-136">PhysicalFileProvider</span></span>

<span data-ttu-id="6758d-137">[Physicalfileprovider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) fiziksel dosya sistemine erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="6758d-137">The [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) provides access to the physical file system.</span></span> <span data-ttu-id="6758d-138">`PhysicalFileProvider`, [System. IO. File](/dotnet/api/system.io.file) türünü kullanır (fiziksel sağlayıcı için) ve tüm yollar bir dizin ve alt öğeleri için kapsamlar.</span><span class="sxs-lookup"><span data-stu-id="6758d-138">`PhysicalFileProvider` uses the [System.IO.File](/dotnet/api/system.io.file) type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="6758d-139">Bu kapsam, belirtilen dizin ve alt öğeleri dışındaki dosya sistemine erişimi engeller.</span><span class="sxs-lookup"><span data-stu-id="6758d-139">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="6758d-140">Bu sağlayıcıyı örnekledikten sonra, bir dizin yolu gereklidir ve sağlayıcı kullanılarak yapılan tüm isteklerin temel yolu olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="6758d-140">When instantiating this provider, a directory path is required and serves as the base path for all requests made using the provider.</span></span> <span data-ttu-id="6758d-141">Bir `PhysicalFileProvider` sağlayıcıyı doğrudan oluşturabilir veya [bağımlılık ekleme](xref:fundamentals/dependency-injection)aracılığıyla bir Oluşturucu `IFileProvider` içinde istek yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6758d-141">You can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="6758d-142">**Statik türler**</span><span class="sxs-lookup"><span data-stu-id="6758d-142">**Static types**</span></span>

<span data-ttu-id="6758d-143">Aşağıdaki kod, nasıl oluşturulacağını `PhysicalFileProvider` ve dizin içeriğini ve dosya bilgilerini almak için nasıl kullanılacağını gösterir:</span><span class="sxs-lookup"><span data-stu-id="6758d-143">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="6758d-144">Önceki örnekteki türler:</span><span class="sxs-lookup"><span data-stu-id="6758d-144">Types in the preceding example:</span></span>

* <span data-ttu-id="6758d-145">`provider`bir `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="6758d-145">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="6758d-146">`contents`bir `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="6758d-146">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="6758d-147">`fileInfo`bir `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="6758d-147">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="6758d-148">Dosya sağlayıcısı, tarafından `applicationRoot` belirtilen dizin üzerinden yinelemek veya bir dosyanın bilgilerini almak için çağırmak `GetFileInfo` üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6758d-148">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="6758d-149">Dosya sağlayıcısı, `applicationRoot` dizin dışında bir erişime sahip değil.</span><span class="sxs-lookup"><span data-stu-id="6758d-149">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="6758d-150">Örnek uygulama, sağlayıcıyı `Startup.ConfigureServices` [ıhostingenvironment. contentrootfileprovider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider)kullanarak uygulamanın sınıfında oluşturur:</span><span class="sxs-lookup"><span data-stu-id="6758d-150">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

<span data-ttu-id="6758d-151">**Bağımlılık ekleme ile dosya sağlayıcısı türlerini alma**</span><span class="sxs-lookup"><span data-stu-id="6758d-151">**Obtain File Provider types with dependency injection**</span></span>

<span data-ttu-id="6758d-152">Sağlayıcıyı herhangi bir sınıf oluşturucusuna ekleyin ve yerel bir alana atayın.</span><span class="sxs-lookup"><span data-stu-id="6758d-152">Inject the provider into any class constructor and assign it to a local field.</span></span> <span data-ttu-id="6758d-153">Dosyalara erişmek için sınıfın yöntemleri boyunca alanını kullanın.</span><span class="sxs-lookup"><span data-stu-id="6758d-153">Use the field throughout the class's methods to access files.</span></span>

<span data-ttu-id="6758d-154">Örnek uygulamada, `IndexModel` sınıfı uygulamanın temel yolu için Dizin `IFileProvider` içeriğini almak üzere bir örnek alır.</span><span class="sxs-lookup"><span data-stu-id="6758d-154">In the sample app, the `IndexModel` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="6758d-155">*Pages/Index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="6758d-155">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="6758d-156">, `IDirectoryContents` Sayfada tekrarlandırılır.</span><span class="sxs-lookup"><span data-stu-id="6758d-156">The `IDirectoryContents` are iterated in the page.</span></span>

<span data-ttu-id="6758d-157">*Sayfa/dizin. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6758d-157">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml?name=snippet1)]

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="6758d-158">Bildirimli Estembeddedfileprovider</span><span class="sxs-lookup"><span data-stu-id="6758d-158">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="6758d-159">Derlemelerinizin içine eklenmiş dosyalara erişmek için, bir [bildirim dosyası sağlayıcı](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6758d-159">The [ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="6758d-160">, `ManifestEmbeddedFileProvider` Gömülü dosyaların özgün yollarını yeniden oluşturmak için derlemeye derlenen bir bildirim kullanır.</span><span class="sxs-lookup"><span data-stu-id="6758d-160">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="6758d-161">Gömülü dosyaların bir bildirimini oluşturmak için `<GenerateEmbeddedFilesManifest>` özelliğini olarak `true`ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="6758d-161">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="6758d-162">[ &lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects)ile eklenecek dosyaları belirtin:</span><span class="sxs-lookup"><span data-stu-id="6758d-162">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="6758d-163">Derlemeye eklemek üzere bir veya daha fazla dosya belirtmek için [Glob desenlerini](#glob-patterns) kullanın.</span><span class="sxs-lookup"><span data-stu-id="6758d-163">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="6758d-164">Örnek uygulama bir `ManifestEmbeddedFileProvider` oluşturur ve şu anda yürütülmekte olan derlemeyi oluşturucusuna geçirir.</span><span class="sxs-lookup"><span data-stu-id="6758d-164">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="6758d-165">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="6758d-165">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="6758d-166">Ek aşırı yüklemeler şunları yapmanıza olanak sağlar:</span><span class="sxs-lookup"><span data-stu-id="6758d-166">Additional overloads allow you to:</span></span>

* <span data-ttu-id="6758d-167">Göreli bir dosya yolu belirtin.</span><span class="sxs-lookup"><span data-stu-id="6758d-167">Specify a relative file path.</span></span>
* <span data-ttu-id="6758d-168">Dosya kapsamını son değiştirilme tarihine kadar.</span><span class="sxs-lookup"><span data-stu-id="6758d-168">Scope files to a last modified date.</span></span>
* <span data-ttu-id="6758d-169">Katıştırılmış dosya bildirimini içeren gömülü kaynağı adlandırın.</span><span class="sxs-lookup"><span data-stu-id="6758d-169">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="6758d-170">Yüklemek</span><span class="sxs-lookup"><span data-stu-id="6758d-170">Overload</span></span> | <span data-ttu-id="6758d-171">Açıklama</span><span class="sxs-lookup"><span data-stu-id="6758d-171">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="6758d-172">Bildirimli Estembeddedfileprovider (derleme, dize)</span><span class="sxs-lookup"><span data-stu-id="6758d-172">ManifestEmbeddedFileProvider(Assembly, String)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_) | <span data-ttu-id="6758d-173">İsteğe bağlı `root` bir göreli yol parametresini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="6758d-173">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="6758d-174">Belirtilen yol altındaki kaynaklar için [getdirectorycontents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) çağrısı yapılacaköğesinibelirtin.`root`</span><span class="sxs-lookup"><span data-stu-id="6758d-174">Specify the `root` to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided path.</span></span> |
| [<span data-ttu-id="6758d-175">Bildirimli Estembeddedfileprovider (derleme, dize, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="6758d-175">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_DateTimeOffset_) | <span data-ttu-id="6758d-176">İsteğe bağlı `root` göreli yol parametresini `lastModified` ve Tarih ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) parametresini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="6758d-176">Accepts an optional `root` relative path parameter and a `lastModified` date ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) parameter.</span></span> <span data-ttu-id="6758d-177">Tarih kapsamları [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)tarafından döndürülen [ıfıleınfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) örneklerinin son değiştirilme tarihidir. `lastModified`</span><span class="sxs-lookup"><span data-stu-id="6758d-177">The `lastModified` date scopes the last modification date for the [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) instances returned by the [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> |
| [<span data-ttu-id="6758d-178">Bildirimli Estembeddedfileprovider (derleme, dize, dize, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="6758d-178">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_String_System_DateTimeOffset_) | <span data-ttu-id="6758d-179">İsteğe bağlı `root` bir göreli yolu, `lastModified` tarihi ve `manifestName` parametreleri kabul eder.</span><span class="sxs-lookup"><span data-stu-id="6758d-179">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="6758d-180">, `manifestName` Bildirimi içeren gömülü kaynağın adını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="6758d-180">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="6758d-181">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="6758d-181">CompositeFileProvider</span></span>

<span data-ttu-id="6758d-182">[Compositefileprovider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) , birden `IFileProvider` fazla sağlayıcıdan dosyalarla çalışmak için tek bir arabirim ortaya çıkaran örnekleri birleştirir.</span><span class="sxs-lookup"><span data-stu-id="6758d-182">The [CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="6758d-183">Oluştururken `CompositeFileProvider`bir veya daha fazla `IFileProvider` örneği oluşturucuya geçirin.</span><span class="sxs-lookup"><span data-stu-id="6758d-183">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="6758d-184">Örnek uygulamada, `PhysicalFileProvider` `ManifestEmbeddedFileProvider` ve, uygulamanın hizmet kapsayıcısında `CompositeFileProvider` kayıtlı bir dosya sağlar:</span><span class="sxs-lookup"><span data-stu-id="6758d-184">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="6758d-185">Değişiklikleri izle</span><span class="sxs-lookup"><span data-stu-id="6758d-185">Watch for changes</span></span>

<span data-ttu-id="6758d-186">[IFileProvider. Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) yöntemi, değişiklikler için bir veya daha fazla dosya ya da dizin izlemek üzere bir senaryo sağlar.</span><span class="sxs-lookup"><span data-stu-id="6758d-186">The [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="6758d-187">`Watch`birden çok dosya belirtmek için [Glob desenlerini](#glob-patterns) içerebilen bir yol dizesi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="6758d-187">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="6758d-188">`Watch`bir [ıchanneltoken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)döndürür.</span><span class="sxs-lookup"><span data-stu-id="6758d-188">`Watch` returns an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span></span> <span data-ttu-id="6758d-189">Değişiklik belirteci şunları gösterir:</span><span class="sxs-lookup"><span data-stu-id="6758d-189">The change token exposes:</span></span>

* <span data-ttu-id="6758d-190">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): Bir değişikliğin oluşup oluşmadığını tespit etmek için incelenebilir bir özellik.</span><span class="sxs-lookup"><span data-stu-id="6758d-190">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="6758d-191">[Registerchangecallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Belirtilen yol dizesinde değişiklikler algılandığında çağırılır.</span><span class="sxs-lookup"><span data-stu-id="6758d-191">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="6758d-192">Her değişiklik belirteci yalnızca, ilişkili geri çağırma işlemini tek bir değişikliğe yanıt olarak çağırır.</span><span class="sxs-lookup"><span data-stu-id="6758d-192">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="6758d-193">Sabit izlemeyi etkinleştirmek için bir [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) kullanın (aşağıda gösterildiği gibi) veya değişikliklere `IChangeToken` yanıt olarak örnekleri yeniden oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6758d-193">To enable constant monitoring, use a [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="6758d-194">Örnek uygulamada, *WatchConsole* konsol uygulaması bir metin dosyası her değiştirildiğinde bir ileti görüntüleyecek şekilde yapılandırılmıştır:</span><span class="sxs-lookup"><span data-stu-id="6758d-194">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="6758d-195">Docker kapsayıcıları ve ağ paylaşımları gibi bazı dosya sistemleri, değişiklik bildirimlerini güvenilir bir şekilde gönderemeyebilir.</span><span class="sxs-lookup"><span data-stu-id="6758d-195">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="6758d-196">Ortam değişkenini, her dört `1` saniyede `true` bir değişiklikler için dosya sistemini yoklamak üzere veya olarak ayarlayın (yapılandırılamaz). `DOTNET_USE_POLLING_FILE_WATCHER`</span><span class="sxs-lookup"><span data-stu-id="6758d-196">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="6758d-197">Glob desenleri</span><span class="sxs-lookup"><span data-stu-id="6758d-197">Glob patterns</span></span>

<span data-ttu-id="6758d-198">Dosya sistemi yolları, *Glob (veya glob) desenleri*adlı joker karakter desenleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="6758d-198">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="6758d-199">Bu desenlerle dosya gruplarını belirtin.</span><span class="sxs-lookup"><span data-stu-id="6758d-199">Specify groups of files with these patterns.</span></span> <span data-ttu-id="6758d-200">İki joker karakter şunlardır `*`: `**`</span><span class="sxs-lookup"><span data-stu-id="6758d-200">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="6758d-201">Geçerli klasör düzeyindeki her şeyi, dosya adını veya herhangi bir dosya uzantısını eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="6758d-201">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="6758d-202">Eşleşmeler, dosya yolundaki `/` `.` karakterler ile sonlandırılır.</span><span class="sxs-lookup"><span data-stu-id="6758d-202">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="6758d-203">Birden çok dizin düzeyindeki tüm öğeleri eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="6758d-203">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="6758d-204">, Bir Dizin hiyerarşisinde birçok dosya yinelemeli olarak eşleşmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6758d-204">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="6758d-205">**Glob deseninin örnekleri**</span><span class="sxs-lookup"><span data-stu-id="6758d-205">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="6758d-206">Belirli bir dizindeki belirli bir dosyayla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="6758d-206">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="6758d-207">Belirli bir dizinde *. txt* uzantılı tüm dosyaları eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="6758d-207">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="6758d-208">Dizinteki `appsettings.json` tüm dosyaları, *Dizin* klasörünün altında tam olarak bir düzey eşler.</span><span class="sxs-lookup"><span data-stu-id="6758d-208">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="6758d-209">*. Txt* uzantılı tüm dosyaları, *Dizin* klasörünün altında herhangi bir yerde buldu.</span><span class="sxs-lookup"><span data-stu-id="6758d-209">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>
