---
title: ASP.NET core'da dosya sağlayıcıları
author: guardrex
description: Nasıl ASP.NET Core dosya sistemi erişimini kullanarak dosya sağlayıcıları soyutlar öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/30/2019
uid: fundamentals/file-providers
ms.openlocfilehash: 2ce40ea0d576d08a6b42c3eb6693754f2a0bddce
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "58809227"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="cd1c8-103">ASP.NET core'da dosya sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="cd1c8-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="cd1c8-104">Tarafından [Steve Smith](https://ardalis.com/) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="cd1c8-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="cd1c8-105">ASP.NET Core, dosya sistemi erişimini kullanarak dosya sağlayıcıları soyutlar.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="cd1c8-106">Dosya sağlayıcıları, ASP.NET Core framework kullanılır:</span><span class="sxs-lookup"><span data-stu-id="cd1c8-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="cd1c8-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) uygulamanın içerik kök ve web kökü olarak sunan `IFileProvider` türleri.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) exposes the app's content root and web root as `IFileProvider` types.</span></span>
* <span data-ttu-id="cd1c8-108">[Statik dosya ara yazılımlarını](xref:fundamentals/static-files) statik dosyaları bulmak üzere dosya sağlayıcıları kullanır.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-108">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="cd1c8-109">[Razor](xref:mvc/views/razor) sayfaları ve görünümlerini bulmak için dosya sağlayıcıları kullanır.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="cd1c8-110">.NET core araçları, hangi dosyaların yayımlandığını belirtmek için dosya sağlayıcıları ve glob desenleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="cd1c8-111">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cd1c8-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="cd1c8-112">Dosya sağlayıcısı arabirimleri</span><span class="sxs-lookup"><span data-stu-id="cd1c8-112">File Provider interfaces</span></span>

<span data-ttu-id="cd1c8-113">Birincil arabirimidir [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span><span class="sxs-lookup"><span data-stu-id="cd1c8-113">The primary interface is [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> <span data-ttu-id="cd1c8-114">`IFileProvider` yöntemlere kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="cd1c8-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="cd1c8-115">Dosya bilgileri elde ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span><span class="sxs-lookup"><span data-stu-id="cd1c8-115">Obtain file information ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span></span>
* <span data-ttu-id="cd1c8-116">Dizin bilgileri elde ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span><span class="sxs-lookup"><span data-stu-id="cd1c8-116">Obtain directory information ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span></span>
* <span data-ttu-id="cd1c8-117">Değişiklik bildirimlerini ayarlama (kullanarak bir [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span><span class="sxs-lookup"><span data-stu-id="cd1c8-117">Set up change notifications (using an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span></span>

<span data-ttu-id="cd1c8-118">`IFileInfo` dosyaları ile çalışma için yöntemleri ve özellikleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="cd1c8-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* [<span data-ttu-id="cd1c8-119">Var.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-119">Exists</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.exists)
* [<span data-ttu-id="cd1c8-120">IsDirectory</span><span class="sxs-lookup"><span data-stu-id="cd1c8-120">IsDirectory</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.isdirectory)
* [<span data-ttu-id="cd1c8-121">Ad</span><span class="sxs-lookup"><span data-stu-id="cd1c8-121">Name</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.name)
* <span data-ttu-id="cd1c8-122">[Uzunluğu](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (bayt cinsinden)</span><span class="sxs-lookup"><span data-stu-id="cd1c8-122">[Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (in bytes)</span></span>
* <span data-ttu-id="cd1c8-123">[LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) tarihi</span><span class="sxs-lookup"><span data-stu-id="cd1c8-123">[LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) date</span></span>

<span data-ttu-id="cd1c8-124">Kullanarak dosyayı okuyabilir [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-124">You can read from the file using the [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) method.</span></span>

<span data-ttu-id="cd1c8-125">Örnek uygulama, dosya sağlayıcı yapılandırma işlemi gösterilmektedir `Startup.ConfigureServices` uygulama boyunca kullanmanız için [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="cd1c8-125">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="cd1c8-126">Dosya sağlayıcısı uygulamaları</span><span class="sxs-lookup"><span data-stu-id="cd1c8-126">File Provider implementations</span></span>

<span data-ttu-id="cd1c8-127">Üç uygulamaları `IFileProvider` kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-127">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="cd1c8-128">Uygulama</span><span class="sxs-lookup"><span data-stu-id="cd1c8-128">Implementation</span></span> | <span data-ttu-id="cd1c8-129">Açıklama</span><span class="sxs-lookup"><span data-stu-id="cd1c8-129">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="cd1c8-130">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="cd1c8-130">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="cd1c8-131">Fiziksel sağlayıcısı, sistemin fiziksel dosyalara erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-131">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="cd1c8-132">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="cd1c8-132">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="cd1c8-133">Bildirim katıştırılmış sağlayıcı derlemeleri katıştırılmış dosyalara erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-133">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="cd1c8-134">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="cd1c8-134">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="cd1c8-135">Bileşik sağlayıcısı, bir veya daha fazla diğer sağlayıcılardan dosyalara ve dizinlere birleşik erişim sağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-135">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="cd1c8-136">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="cd1c8-136">PhysicalFileProvider</span></span>

<span data-ttu-id="cd1c8-137">[PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) fiziksel dosya sistemine erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-137">The [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) provides access to the physical file system.</span></span> <span data-ttu-id="cd1c8-138">`PhysicalFileProvider` kullanan [System.IO.File](/dotnet/api/system.io.file) (fiziksel sağlayıcısının) türünü ve bir dizin ve alt öğeleri için tüm yolları kapsamları.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-138">`PhysicalFileProvider` uses the [System.IO.File](/dotnet/api/system.io.file) type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="cd1c8-139">Bu kapsam dışında belirtilen dizin ve alt dosya sistemine erişimi engeller.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-139">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="cd1c8-140">Bu sağlayıcı örneği oluşturulurken bir dizin yolu gereklidir ve sağlayıcıyı kullanarak yapılan tüm isteklere ait temel yol olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-140">When instantiating this provider, a directory path is required and serves as the base path for all requests made using the provider.</span></span> <span data-ttu-id="cd1c8-141">Örneği oluşturabilir bir `PhysicalFileProvider` doğrudan sağlayıcısı veya isteyebilir bir `IFileProvider` bir Oluşturucuda [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="cd1c8-141">You can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="cd1c8-142">**Statik türler**</span><span class="sxs-lookup"><span data-stu-id="cd1c8-142">**Static types**</span></span>

<span data-ttu-id="cd1c8-143">Aşağıdaki kod nasıl oluşturulacağını gösterir. bir `PhysicalFileProvider` ve dizin içeriğini alıp dosya bilgileri kullanın:</span><span class="sxs-lookup"><span data-stu-id="cd1c8-143">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="cd1c8-144">Önceki örnekte türleri:</span><span class="sxs-lookup"><span data-stu-id="cd1c8-144">Types in the preceding example:</span></span>

* <span data-ttu-id="cd1c8-145">`provider` olan bir `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-145">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="cd1c8-146">`contents` olan bir `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-146">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="cd1c8-147">`fileInfo` olan bir `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-147">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="cd1c8-148">Dosya sağlayıcısı tarafından belirtilen dizin yinelemek için kullanılabilir `applicationRoot` veya çağrı `GetFileInfo` bir dosyanın bilgi edinme.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-148">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="cd1c8-149">Dosya sağlayıcısı dışında erişim yok `applicationRoot` dizin.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-149">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="cd1c8-150">Örnek uygulamayı, uygulamanın sağlayıcısı oluşturur `Startup.ConfigureServices` kullanarak [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span><span class="sxs-lookup"><span data-stu-id="cd1c8-150">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

<span data-ttu-id="cd1c8-151">**Bağımlılık ekleme dosya sağlayıcısı türleriyle alın**</span><span class="sxs-lookup"><span data-stu-id="cd1c8-151">**Obtain File Provider types with dependency injection**</span></span>

<span data-ttu-id="cd1c8-152">Herhangi bir sınıf oluşturucusuna sağlayıcı ekleme ve yerel bir alana atayın.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-152">Inject the provider into any class constructor and assign it to a local field.</span></span> <span data-ttu-id="cd1c8-153">Dosyalara erişmek için sınıfın yöntemlerini boyunca alanını kullanın.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-153">Use the field throughout the class's methods to access files.</span></span>

<span data-ttu-id="cd1c8-154">Örnek uygulamada `IndexModel` sınıfı bir `IFileProvider` uygulamanın taban yolu için dizin içeriğini almak için örnek.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-154">In the sample app, the `IndexModel` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="cd1c8-155">*Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="cd1c8-155">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="cd1c8-156">`IDirectoryContents` Sayfasında yinelenir.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-156">The `IDirectoryContents` are iterated in the page.</span></span>

<span data-ttu-id="cd1c8-157">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="cd1c8-157">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml?name=snippet1)]

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="cd1c8-158">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="cd1c8-158">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="cd1c8-159">[ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) gömülü bütünleştirilmiş kodlarında dosyalara erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-159">The [ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="cd1c8-160">`ManifestEmbeddedFileProvider` Bütünleştirilmiş kod içine derlenmiş bir bildirim ekli dosyalar özgün yollarını yeniden oluşturmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-160">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="cd1c8-161">Katıştırılmış dosyaların bir bildirim oluşturmak üzere `<GenerateEmbeddedFilesManifest>` özelliğini `true`.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-161">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="cd1c8-162">İle eklemek için dosyaları belirttiğiniz [ &lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span><span class="sxs-lookup"><span data-stu-id="cd1c8-162">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

<span data-ttu-id="cd1c8-163">Kullanım [glob desenleri](#glob-patterns) derlemesine gömmek için bir veya daha fazla dosyaları belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-163">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="cd1c8-164">Örnek uygulamayı oluşturur bir `ManifestEmbeddedFileProvider` ve şu anda çalıştırılan derlemenin yapıcısına geçirir.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-164">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="cd1c8-165">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="cd1c8-165">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="cd1c8-166">Ek aşırı yüklemeler sağlar:</span><span class="sxs-lookup"><span data-stu-id="cd1c8-166">Additional overloads allow you to:</span></span>

* <span data-ttu-id="cd1c8-167">Bir göreli dosya yolu belirtin.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-167">Specify a relative file path.</span></span>
* <span data-ttu-id="cd1c8-168">Son değiştirilme tarihi dosyalarını kapsam.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-168">Scope files to a last modified date.</span></span>
* <span data-ttu-id="cd1c8-169">Ekli dosya listesi içeren bir gömülü kaynak adı.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-169">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="cd1c8-170">aşırı yükleme</span><span class="sxs-lookup"><span data-stu-id="cd1c8-170">Overload</span></span> | <span data-ttu-id="cd1c8-171">Açıklama</span><span class="sxs-lookup"><span data-stu-id="cd1c8-171">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="cd1c8-172">ManifestEmbeddedFileProvider (bütünleştirilmiş kod, dize)</span><span class="sxs-lookup"><span data-stu-id="cd1c8-172">ManifestEmbeddedFileProvider(Assembly, String)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_) | <span data-ttu-id="cd1c8-173">İsteğe bağlı kabul `root` göreli yol parametresi.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-173">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="cd1c8-174">Belirtin `root` kapsam çağrıları için [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) sağlanan yol altında bu kaynaklara.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-174">Specify the `root` to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided path.</span></span> |
| [<span data-ttu-id="cd1c8-175">ManifestEmbeddedFileProvider (derleme, dize, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="cd1c8-175">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_DateTimeOffset_) | <span data-ttu-id="cd1c8-176">İsteğe bağlı kabul `root` göreli yol parametresi ve `lastModified` tarih ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) parametre.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-176">Accepts an optional `root` relative path parameter and a `lastModified` date ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) parameter.</span></span> <span data-ttu-id="cd1c8-177">`lastModified` Tarihi, son değiştirilme tarihi kapsamlar [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) tarafından döndürülen örnek [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span><span class="sxs-lookup"><span data-stu-id="cd1c8-177">The `lastModified` date scopes the last modification date for the [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) instances returned by the [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> |
| [<span data-ttu-id="cd1c8-178">ManifestEmbeddedFileProvider (derleme, String, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="cd1c8-178">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_String_System_DateTimeOffset_) | <span data-ttu-id="cd1c8-179">İsteğe bağlı kabul `root` göreli yol `lastModified` tarihi ve `manifestName` parametreleri.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-179">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="cd1c8-180">`manifestName` Bildirimini içeren katıştırılmış kaynağın adını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-180">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="cd1c8-181">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="cd1c8-181">CompositeFileProvider</span></span>

<span data-ttu-id="cd1c8-182">[CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) birleştirir `IFileProvider` örnekleri, birden fazla sağlayıcıdan alınan dosyalarla çalışmak için tek bir arabirim gösterme.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-182">The [CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="cd1c8-183">Oluştururken `CompositeFileProvider`, geçişi bir veya daha fazla `IFileProvider` oluşturucusuna örnekleri.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-183">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="cd1c8-184">Örnek uygulamada, bir `PhysicalFileProvider` ve `ManifestEmbeddedFileProvider` dosyaları sağlayan bir `CompositeFileProvider` uygulamanın service kapsayıcısında kayıtlı:</span><span class="sxs-lookup"><span data-stu-id="cd1c8-184">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="cd1c8-185">Değişiklikler için izleyin</span><span class="sxs-lookup"><span data-stu-id="cd1c8-185">Watch for changes</span></span>

<span data-ttu-id="cd1c8-186">[IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) yöntemi, bir veya daha fazla dosyaları veya dizinleri değişiklikleri izlemek için bir senaryo sağlar.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-186">The [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="cd1c8-187">`Watch` kullanabileceğiniz bir yol dizesini kabul eder [glob desenleri](#glob-patterns) birden çok dosyayı belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-187">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="cd1c8-188">`Watch` döndürür bir [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span><span class="sxs-lookup"><span data-stu-id="cd1c8-188">`Watch` returns an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span></span> <span data-ttu-id="cd1c8-189">Değişiklik belirteci çıkarır:</span><span class="sxs-lookup"><span data-stu-id="cd1c8-189">The change token exposes:</span></span>

* <span data-ttu-id="cd1c8-190">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): Bir değişiklik oluşup oluşmadığını belirlemek için denetlenecek özellik.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-190">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="cd1c8-191">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Belirtilen yol dizesini değişiklik algılandığında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-191">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="cd1c8-192">Her değişiklik belirteci yalnızca tek bir değişikliğe yanıt ilişkili geri çağırması çağırır.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-192">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="cd1c8-193">Sabit izlemeyi etkinleştirmek için bir [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (aşağıda gösterilen) veya yeniden `IChangeToken` değişikliklere yanıt olarak örnekleri.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-193">To enable constant monitoring, use a [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="cd1c8-194">Örnek uygulamada *WatchConsole* konsol uygulaması, bir metin dosyası her değiştirildiğinde bir ileti görüntülemek için yapılandırılmıştır:</span><span class="sxs-lookup"><span data-stu-id="cd1c8-194">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="cd1c8-195">Docker kapsayıcıları ve ağ paylaşımları gibi bazı dosya sistemleri değişiklik bildirimleri gönderebilmek değil.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-195">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="cd1c8-196">Ayarlama `DOTNET_USE_POLLING_FILE_WATCHER` ortam değişkenine `1` veya `true` değişikliklerin dosya sistemi (yapılandırılabilir) dört saniyede yoklamak için.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-196">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="cd1c8-197">Glob desenleri</span><span class="sxs-lookup"><span data-stu-id="cd1c8-197">Glob patterns</span></span>

<span data-ttu-id="cd1c8-198">Dosya sistemi yolları adında joker karakter düzenleri kullanmak *glob (veya Glob) desenlerini*.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-198">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="cd1c8-199">Bu modellerle dosya grupları belirtin.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-199">Specify groups of files with these patterns.</span></span> <span data-ttu-id="cd1c8-200">İki joker karakterler `*` ve `**`:</span><span class="sxs-lookup"><span data-stu-id="cd1c8-200">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="cd1c8-201">Herhangi bir şey geçerli klasör düzeyinde, herhangi bir dosya adı veya herhangi bir dosya uzantısı ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-201">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="cd1c8-202">Eşleşme tarafından sonlandırılır `/` ve `.` karakter dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-202">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="cd1c8-203">Herhangi bir şey, birden çok dizin düzeyleri arasında eşleşir.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-203">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="cd1c8-204">Yinelemeli olarak kullanılan dizin sıradüzeni içinde çok sayıda dosya eşleşmesi.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-204">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="cd1c8-205">**Glob deseni örnekleri**</span><span class="sxs-lookup"><span data-stu-id="cd1c8-205">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="cd1c8-206">Belirli bir dizinin belirli bir dosyayı eşleşir.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-206">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="cd1c8-207">Eşleşen tüm dosyaları *.txt* belirli bir dizine uzantı.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-207">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="cd1c8-208">Tüm eşleşen `appsettings.json` dizinleri tam olarak bir düzey alttaki dosyalarında *dizin* klasör.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-208">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="cd1c8-209">Eşleşen tüm dosyaları *.txt* uzantısı bulunan herhangi bir yere altında *dizin* klasör.</span><span class="sxs-lookup"><span data-stu-id="cd1c8-209">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>
