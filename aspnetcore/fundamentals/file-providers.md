---
title: ASP.NET core'da dosya sağlayıcıları
author: guardrex
description: Nasıl ASP.NET Core dosya sistemi erişimini kullanarak dosya sağlayıcıları soyutlar öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 08/01/2018
uid: fundamentals/file-providers
ms.openlocfilehash: 512229cfe7d7efdcd9050fa13dbdbf793be29a0b
ms.sourcegitcommit: 571d76fbbff05e84406b6d909c8fe9cbea2c8ff1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39410162"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="533f1-103">ASP.NET core'da dosya sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="533f1-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="533f1-104">Tarafından [Steve Smith](https://ardalis.com/) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="533f1-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="533f1-105">ASP.NET Core, dosya sistemi erişimini kullanarak dosya sağlayıcıları soyutlar.</span><span class="sxs-lookup"><span data-stu-id="533f1-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="533f1-106">Dosya sağlayıcıları, ASP.NET Core framework kullanılır:</span><span class="sxs-lookup"><span data-stu-id="533f1-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="533f1-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) uygulamanın içerik kök ve web kökü olarak sunan `IFileProvider` türleri.</span><span class="sxs-lookup"><span data-stu-id="533f1-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) exposes the app's content root and web root as `IFileProvider` types.</span></span>
* <span data-ttu-id="533f1-108">[Statik dosya ara yazılımı](xref:fundamentals/static-files) statik dosyaları bulmak üzere dosya sağlayıcıları kullanır.</span><span class="sxs-lookup"><span data-stu-id="533f1-108">[Static Files Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="533f1-109">[Razor](xref:mvc/views/razor) sayfaları ve görünümlerini bulmak için dosya sağlayıcıları kullanır.</span><span class="sxs-lookup"><span data-stu-id="533f1-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="533f1-110">.NET core araçları, hangi dosyaların yayımlandığını belirtmek için dosya sağlayıcıları ve glob desenleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="533f1-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="533f1-111">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="533f1-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="533f1-112">Dosya sağlayıcısı arabirimleri</span><span class="sxs-lookup"><span data-stu-id="533f1-112">File Provider interfaces</span></span>

<span data-ttu-id="533f1-113">Birincil arabirimidir [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span><span class="sxs-lookup"><span data-stu-id="533f1-113">The primary interface is [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> <span data-ttu-id="533f1-114">`IFileProvider` yöntemlere kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="533f1-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="533f1-115">Dosya bilgileri elde ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span><span class="sxs-lookup"><span data-stu-id="533f1-115">Obtain file information ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span></span>
* <span data-ttu-id="533f1-116">Dizin bilgileri elde ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span><span class="sxs-lookup"><span data-stu-id="533f1-116">Obtain directory information ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span></span>
* <span data-ttu-id="533f1-117">Değişiklik bildirimlerini ayarlama (kullanarak bir [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span><span class="sxs-lookup"><span data-stu-id="533f1-117">Set up change notifications (using an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span></span>

<span data-ttu-id="533f1-118">`IFileInfo` dosyaları ile çalışma için yöntemleri ve özellikleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="533f1-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* [<span data-ttu-id="533f1-119">Var.</span><span class="sxs-lookup"><span data-stu-id="533f1-119">Exists</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.exists)
* [<span data-ttu-id="533f1-120">IsDirectory</span><span class="sxs-lookup"><span data-stu-id="533f1-120">IsDirectory</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.isdirectory)
* [<span data-ttu-id="533f1-121">Adı</span><span class="sxs-lookup"><span data-stu-id="533f1-121">Name</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.name)
* <span data-ttu-id="533f1-122">[Uzunluğu](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (bayt cinsinden)</span><span class="sxs-lookup"><span data-stu-id="533f1-122">[Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (in bytes)</span></span>
* <span data-ttu-id="533f1-123">[LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) tarihi</span><span class="sxs-lookup"><span data-stu-id="533f1-123">[LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) date</span></span>

<span data-ttu-id="533f1-124">Kullanarak dosyayı okuyabilir [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="533f1-124">You can read from the file using the [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) method.</span></span>

<span data-ttu-id="533f1-125">Örnek uygulama, dosya sağlayıcı yapılandırma işlemi gösterilmektedir `Startup.ConfigureServices` uygulama boyunca kullanmanız için [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="533f1-125">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="533f1-126">Dosya sağlayıcısı uygulamaları</span><span class="sxs-lookup"><span data-stu-id="533f1-126">File Provider implementations</span></span>

<span data-ttu-id="533f1-127">Üç uygulamaları `IFileProvider` kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="533f1-127">Three implementations of `IFileProvider` are available.</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="533f1-128">Uygulama</span><span class="sxs-lookup"><span data-stu-id="533f1-128">Implementation</span></span> | <span data-ttu-id="533f1-129">Açıklama</span><span class="sxs-lookup"><span data-stu-id="533f1-129">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="533f1-130">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="533f1-130">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="533f1-131">Fiziksel sağlayıcısı, sistemin fiziksel dosyalara erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="533f1-131">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="533f1-132">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="533f1-132">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="533f1-133">Bildirim katıştırılmış sağlayıcı derlemeleri katıştırılmış dosyalara erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="533f1-133">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="533f1-134">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="533f1-134">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="533f1-135">Bileşik sağlayıcısı, bir veya daha fazla diğer sağlayıcılardan dosyalara ve dizinlere birleşik erişim sağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="533f1-135">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="533f1-136">Uygulama</span><span class="sxs-lookup"><span data-stu-id="533f1-136">Implementation</span></span> | <span data-ttu-id="533f1-137">Açıklama</span><span class="sxs-lookup"><span data-stu-id="533f1-137">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="533f1-138">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="533f1-138">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="533f1-139">Fiziksel sağlayıcısı, sistemin fiziksel dosyalara erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="533f1-139">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="533f1-140">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="533f1-140">EmbeddedFileProvider</span></span>](#embeddedfileprovider) | <span data-ttu-id="533f1-141">Katıştırılmış sağlayıcı derlemeleri katıştırılmış dosyalara erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="533f1-141">The embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="533f1-142">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="533f1-142">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="533f1-143">Bileşik sağlayıcısı, bir veya daha fazla diğer sağlayıcılardan dosyalara ve dizinlere birleşik erişim sağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="533f1-143">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

::: moniker-end

### <a name="physicalfileprovider"></a><span data-ttu-id="533f1-144">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="533f1-144">PhysicalFileProvider</span></span>

<span data-ttu-id="533f1-145">[PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) fiziksel dosya sistemine erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="533f1-145">The [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) provides access to the physical file system.</span></span> <span data-ttu-id="533f1-146">`PhysicalFileProvider` kullanan [System.IO.File](/dotnet/api/system.io.file) (fiziksel sağlayıcısının) türünü ve bir dizin ve alt öğeleri için tüm yolları kapsamları.</span><span class="sxs-lookup"><span data-stu-id="533f1-146">`PhysicalFileProvider` uses the [System.IO.File](/dotnet/api/system.io.file) type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="533f1-147">Bu kapsam dışında belirtilen dizin ve alt dosya sistemine erişimi engeller.</span><span class="sxs-lookup"><span data-stu-id="533f1-147">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="533f1-148">Bu sağlayıcı örneği oluşturulurken bir dizin yolu gereklidir ve sağlayıcıyı kullanarak yapılan tüm isteklere ait temel yol olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="533f1-148">When instantiating this provider, a directory path is required and serves as the base path for all requests made using the provider.</span></span> <span data-ttu-id="533f1-149">Örneği oluşturabilir bir `PhysicalFileProvider` doğrudan sağlayıcısı veya isteyebilir bir `IFileProvider` bir Oluşturucuda [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="533f1-149">You can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="533f1-150">**Statik türler**</span><span class="sxs-lookup"><span data-stu-id="533f1-150">**Static types**</span></span>

<span data-ttu-id="533f1-151">Aşağıdaki kod nasıl oluşturulacağını gösterir. bir `PhysicalFileProvider` ve dizin içeriğini alıp dosya bilgileri kullanın:</span><span class="sxs-lookup"><span data-stu-id="533f1-151">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="533f1-152">Önceki örnekte türleri:</span><span class="sxs-lookup"><span data-stu-id="533f1-152">Types in the preceding example:</span></span>

* <span data-ttu-id="533f1-153">`provider` olan bir `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="533f1-153">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="533f1-154">`contents` olan bir `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="533f1-154">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="533f1-155">`fileInfo` olan bir `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="533f1-155">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="533f1-156">Dosya sağlayıcısı tarafından belirtilen dizin yinelemek için kullanılabilir `applicationRoot` veya çağrı `GetFileInfo` bir dosyanın bilgi edinme.</span><span class="sxs-lookup"><span data-stu-id="533f1-156">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="533f1-157">Dosya sağlayıcısı dışında erişim yok `applicationRoot` dizin.</span><span class="sxs-lookup"><span data-stu-id="533f1-157">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="533f1-158">Örnek uygulamayı, uygulamanın sağlayıcısı oluşturur `Startup.ConfigureServices` kullanarak [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span><span class="sxs-lookup"><span data-stu-id="533f1-158">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

<span data-ttu-id="533f1-159">**Bağımlılık ekleme dosya sağlayıcısı türleriyle alın**</span><span class="sxs-lookup"><span data-stu-id="533f1-159">**Obtain File Provider types with dependency injection**</span></span>

<span data-ttu-id="533f1-160">Herhangi bir sınıf oluşturucusuna sağlayıcı ekleme ve yerel bir alana atayın.</span><span class="sxs-lookup"><span data-stu-id="533f1-160">Inject the provider into any class constructor and assign it to a local field.</span></span> <span data-ttu-id="533f1-161">Dosyalara erişmek için sınıfın yöntemlerini boyunca alanını kullanın.</span><span class="sxs-lookup"><span data-stu-id="533f1-161">Use the field throughout the class's methods to access files.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="533f1-162">Örnek uygulamada `IndexModel` sınıfı bir `IFileProvider` uygulamanın taban yolu için dizin içeriğini almak için örnek.</span><span class="sxs-lookup"><span data-stu-id="533f1-162">In the sample app, the `IndexModel` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="533f1-163">*Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="533f1-163">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="533f1-164">`IDirectoryContents` Sayfasında yinelenir.</span><span class="sxs-lookup"><span data-stu-id="533f1-164">The `IDirectoryContents` are iterated in the page.</span></span>

<span data-ttu-id="533f1-165">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="533f1-165">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="533f1-166">Örnek uygulamada `HomeController` sınıfı bir `IFileProvider` uygulamanın taban yolu için dizin içeriğini almak için örnek.</span><span class="sxs-lookup"><span data-stu-id="533f1-166">In the sample app, the `HomeController` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="533f1-167">*Controllers/HomeController.cs*:</span><span class="sxs-lookup"><span data-stu-id="533f1-167">*Controllers/HomeController.cs*:</span></span>

[!code-csharp[](file-providers/samples/1.x/FileProviderSample/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="533f1-168">`IDirectoryContents` Görünümünde yinelenir.</span><span class="sxs-lookup"><span data-stu-id="533f1-168">The `IDirectoryContents` are iterated in the view.</span></span>

<span data-ttu-id="533f1-169">*Views/Home/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="533f1-169">*Views/Home/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/1.x/FileProviderSample/Views/Home/Index.cshtml?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="533f1-170">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="533f1-170">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="533f1-171">[ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) gömülü bütünleştirilmiş kodlarında dosyalara erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="533f1-171">The [ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="533f1-172">`ManifestEmbeddedFileProvider` Bütünleştirilmiş kod içine derlenmiş bir bildirim ekli dosyalar özgün yollarını yeniden oluşturmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="533f1-172">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

> [!NOTE]
> <span data-ttu-id="533f1-173">`ManifestEmbeddedFileProvider` ASP.NET Core 2.1 veya üzeri sürümlerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="533f1-173">The `ManifestEmbeddedFileProvider` is available in ASP.NET Core 2.1 or later.</span></span> <span data-ttu-id="533f1-174">ASP.NET Core 2.0 derlemede gömülü dosyalara veya önceki sürümlerinde, [ASP.NET Core 1.x sürümü bu konunun](xref:fundamentals/file-providers?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="533f1-174">To access files embedded in assemblies in ASP.NET Core 2.0 or earlier, see the [ASP.NET Core 1.x version of this topic](xref:fundamentals/file-providers?view=aspnetcore-1.1).</span></span>

<span data-ttu-id="533f1-175">Katıştırılmış dosyaların bir bildirim oluşturmak üzere `<GenerateEmbeddedFilesManifest>` özelliğini `true`.</span><span class="sxs-lookup"><span data-stu-id="533f1-175">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="533f1-176">İle eklemek için dosyaları belirttiğiniz [ &lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span><span class="sxs-lookup"><span data-stu-id="533f1-176">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

<span data-ttu-id="533f1-177">Kullanım [glob desenleri](#glob-patterns) derlemesine gömmek için bir veya daha fazla dosyaları belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="533f1-177">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="533f1-178">Örnek uygulamayı oluşturur bir `ManifestEmbeddedFileProvider` ve şu anda çalıştırılan derlemenin yapıcısına geçirir.</span><span class="sxs-lookup"><span data-stu-id="533f1-178">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="533f1-179">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="533f1-179">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="533f1-180">Ek aşırı yüklemeler sağlar:</span><span class="sxs-lookup"><span data-stu-id="533f1-180">Additional overloads allow you to:</span></span>

* <span data-ttu-id="533f1-181">Bir göreli dosya yolu belirtin.</span><span class="sxs-lookup"><span data-stu-id="533f1-181">Specify a relative file path.</span></span>
* <span data-ttu-id="533f1-182">Son değiştirilme tarihi dosyalarını kapsam.</span><span class="sxs-lookup"><span data-stu-id="533f1-182">Scope files to a last modified date.</span></span>
* <span data-ttu-id="533f1-183">Ekli dosya listesi içeren bir gömülü kaynak adı.</span><span class="sxs-lookup"><span data-stu-id="533f1-183">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="533f1-184">aşırı yükleme</span><span class="sxs-lookup"><span data-stu-id="533f1-184">Overload</span></span> | <span data-ttu-id="533f1-185">Açıklama</span><span class="sxs-lookup"><span data-stu-id="533f1-185">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="533f1-186">ManifestEmbeddedFileProvider (bütünleştirilmiş kod, dize)</span><span class="sxs-lookup"><span data-stu-id="533f1-186">ManifestEmbeddedFileProvider(Assembly, String)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_) | <span data-ttu-id="533f1-187">İsteğe bağlı kabul `root` göreli yol parametresi.</span><span class="sxs-lookup"><span data-stu-id="533f1-187">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="533f1-188">Belirtin `root` kapsam çağrıları için [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) sağlanan yol altında bu kaynaklara.</span><span class="sxs-lookup"><span data-stu-id="533f1-188">Specify the `root` to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided path.</span></span> |
| [<span data-ttu-id="533f1-189">ManifestEmbeddedFileProvider (derleme, dize, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="533f1-189">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_DateTimeOffset_) | <span data-ttu-id="533f1-190">İsteğe bağlı kabul `root` göreli yol parametresi ve `lastModified` tarih ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) parametre.</span><span class="sxs-lookup"><span data-stu-id="533f1-190">Accepts an optional `root` relative path parameter and a `lastModified` date ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) parameter.</span></span> <span data-ttu-id="533f1-191">`lastModified` Tarihi, son değiştirilme tarihi kapsamlar [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) tarafından döndürülen örnek [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span><span class="sxs-lookup"><span data-stu-id="533f1-191">The `lastModified` date scopes the last modification date for the [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) instances returned by the [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> |
| [<span data-ttu-id="533f1-192">ManifestEmbeddedFileProvider (derleme, String, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="533f1-192">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_String_System_DateTimeOffset_) | <span data-ttu-id="533f1-193">İsteğe bağlı kabul `root` göreli yol `lastModified` tarihi ve `manifestName` parametreleri.</span><span class="sxs-lookup"><span data-stu-id="533f1-193">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="533f1-194">`manifestName` Bildirimini içeren katıştırılmış kaynağın adını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="533f1-194">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

### <a name="embeddedfileprovider"></a><span data-ttu-id="533f1-195">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="533f1-195">EmbeddedFileProvider</span></span>

<span data-ttu-id="533f1-196">[EmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider) gömülü bütünleştirilmiş kodlarında dosyalara erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="533f1-196">The [EmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="533f1-197">İle eklemek için dosyaları belirttiğiniz [ &lt;EmbeddedResource&gt; ](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) özelliği proje dosyasında:</span><span class="sxs-lookup"><span data-stu-id="533f1-197">Specify the files to embed with the [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) property in the project file:</span></span>

```xml
<ItemGroup>
  <EmbeddedResource Include="Resource.txt" />
</ItemGroup>
```

<span data-ttu-id="533f1-198">Kullanım [glob desenleri](#glob-patterns) derlemesine gömmek için bir veya daha fazla dosyaları belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="533f1-198">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="533f1-199">Örnek uygulamayı oluşturur bir `EmbeddedFileProvider` ve şu anda çalıştırılan derlemenin yapıcısına geçirir.</span><span class="sxs-lookup"><span data-stu-id="533f1-199">The sample app creates an `EmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="533f1-200">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="533f1-200">*Startup.cs*:</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="533f1-201">Gömülü kaynak dizinleri sunmayın.</span><span class="sxs-lookup"><span data-stu-id="533f1-201">Embedded resources don't expose directories.</span></span> <span data-ttu-id="533f1-202">(Ad) aracılığıyla bir kaynağın yolunu kendi dosya adı kullanılarak bunun yerine, katıştırılmış `.` ayırıcı.</span><span class="sxs-lookup"><span data-stu-id="533f1-202">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span> <span data-ttu-id="533f1-203">Örnek uygulamada `baseNamespace` olduğu `FileProviderSample.`.</span><span class="sxs-lookup"><span data-stu-id="533f1-203">In the sample app, the `baseNamespace` is `FileProviderSample.`.</span></span>

<span data-ttu-id="533f1-204">[EmbeddedFileProvider (bütünleştirilmiş kod, String)](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_EmbeddedFileProvider__ctor_System_Reflection_Assembly_) Oluşturucusu isteğe bağlı kabul `baseNamespace` parametresi.</span><span class="sxs-lookup"><span data-stu-id="533f1-204">The [EmbeddedFileProvider(Assembly, String)](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_EmbeddedFileProvider__ctor_System_Reflection_Assembly_) constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="533f1-205">Kapsam çağrıları için temel ad alanı belirtmek [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) bu kaynaklara sağlanan ad alanı altında.</span><span class="sxs-lookup"><span data-stu-id="533f1-205">Specify the base namespace to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided namespace.</span></span>

::: moniker-end

### <a name="compositefileprovider"></a><span data-ttu-id="533f1-206">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="533f1-206">CompositeFileProvider</span></span>

<span data-ttu-id="533f1-207">[CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) birleştirir `IFileProvider` örnekleri, birden fazla sağlayıcıdan alınan dosyalarla çalışmak için tek bir arabirim gösterme.</span><span class="sxs-lookup"><span data-stu-id="533f1-207">The [CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="533f1-208">Oluştururken `CompositeFileProvider`, geçişi bir veya daha fazla `IFileProvider` oluşturucusuna örnekleri.</span><span class="sxs-lookup"><span data-stu-id="533f1-208">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="533f1-209">Örnek uygulamada, bir `PhysicalFileProvider` ve `ManifestEmbeddedFileProvider` dosyaları sağlayan bir `CompositeFileProvider` uygulamanın service kapsayıcısında kayıtlı:</span><span class="sxs-lookup"><span data-stu-id="533f1-209">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="533f1-210">Örnek uygulamada, bir `PhysicalFileProvider` ve `EmbeddedFileProvider` dosyaları sağlayan bir `CompositeFileProvider` uygulamanın service kapsayıcısında kayıtlı:</span><span class="sxs-lookup"><span data-stu-id="533f1-210">In the sample app, a `PhysicalFileProvider` and an `EmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/1.x/FileProviderSample/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="watch-for-changes"></a><span data-ttu-id="533f1-211">Değişiklikler için izleyin</span><span class="sxs-lookup"><span data-stu-id="533f1-211">Watch for changes</span></span>

<span data-ttu-id="533f1-212">[IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) yöntemi, bir veya daha fazla dosyaları veya dizinleri değişiklikleri izlemek için bir senaryo sağlar.</span><span class="sxs-lookup"><span data-stu-id="533f1-212">The [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="533f1-213">`Watch` kullanabileceğiniz bir yol dizesini kabul eder [glob desenleri](#glob-patterns) birden çok dosyayı belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="533f1-213">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="533f1-214">`Watch` döndürür bir [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span><span class="sxs-lookup"><span data-stu-id="533f1-214">`Watch` returns an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span></span> <span data-ttu-id="533f1-215">Değişiklik belirteci çıkarır:</span><span class="sxs-lookup"><span data-stu-id="533f1-215">The change token exposes:</span></span>

* <span data-ttu-id="533f1-216">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): bir değişiklik oluşup oluşmadığını belirlemek için denetlenecek özellik.</span><span class="sxs-lookup"><span data-stu-id="533f1-216">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="533f1-217">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Belirtilen yol dizesini değişiklik algılandığında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="533f1-217">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="533f1-218">Her değişiklik belirteci yalnızca tek bir değişikliğe yanıt ilişkili geri çağırması çağırır.</span><span class="sxs-lookup"><span data-stu-id="533f1-218">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="533f1-219">Sabit izlemeyi etkinleştirmek için bir [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (aşağıda gösterilen) veya yeniden `IChangeToken` değişikliklere yanıt olarak örnekleri.</span><span class="sxs-lookup"><span data-stu-id="533f1-219">To enable constant monitoring, use a [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="533f1-220">Örnek uygulamada *WatchConsole* konsol uygulaması, bir metin dosyası her değiştirildiğinde bir ileti görüntülemek için yapılandırılmıştır:</span><span class="sxs-lookup"><span data-stu-id="533f1-220">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](file-providers/samples/1.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

::: moniker-end

<span data-ttu-id="533f1-221">Docker kapsayıcıları ve ağ paylaşımları gibi bazı dosya sistemleri değişiklik bildirimleri gönderebilmek değil.</span><span class="sxs-lookup"><span data-stu-id="533f1-221">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="533f1-222">Ayarlama `DOTNET_USE_POLLING_FILE_WATCHER` ortam değişkenine `1` veya `true` değişikliklerin dosya sistemi (yapılandırılabilir) dört saniyede yoklamak için.</span><span class="sxs-lookup"><span data-stu-id="533f1-222">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="533f1-223">Glob desenleri</span><span class="sxs-lookup"><span data-stu-id="533f1-223">Glob patterns</span></span>

<span data-ttu-id="533f1-224">Dosya sistemi yolları adında joker karakter düzenleri kullanmak *glob (veya Glob) desenlerini*.</span><span class="sxs-lookup"><span data-stu-id="533f1-224">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="533f1-225">Bu modellerle dosya grupları belirtin.</span><span class="sxs-lookup"><span data-stu-id="533f1-225">Specify groups of files with these patterns.</span></span> <span data-ttu-id="533f1-226">İki joker karakterler `*` ve `**`:</span><span class="sxs-lookup"><span data-stu-id="533f1-226">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="533f1-227">Herhangi bir şey geçerli klasör düzeyinde, herhangi bir dosya adı veya herhangi bir dosya uzantısı ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="533f1-227">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="533f1-228">Eşleşme tarafından sonlandırılır `/` ve `.` karakter dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="533f1-228">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="533f1-229">Herhangi bir şey, birden çok dizin düzeyleri arasında eşleşir.</span><span class="sxs-lookup"><span data-stu-id="533f1-229">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="533f1-230">Yinelemeli olarak kullanılan dizin sıradüzeni içinde çok sayıda dosya eşleşmesi.</span><span class="sxs-lookup"><span data-stu-id="533f1-230">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="533f1-231">**Glob deseni örnekleri**</span><span class="sxs-lookup"><span data-stu-id="533f1-231">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="533f1-232">Belirli bir dizinin belirli bir dosyayı eşleşir.</span><span class="sxs-lookup"><span data-stu-id="533f1-232">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="533f1-233">Eşleşen tüm dosyaları *.txt* belirli bir dizine uzantı.</span><span class="sxs-lookup"><span data-stu-id="533f1-233">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="533f1-234">Tüm eşleşen `appsettings.json` dizinleri tam olarak bir düzey alttaki dosyalarında *dizin* klasör.</span><span class="sxs-lookup"><span data-stu-id="533f1-234">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="533f1-235">Eşleşen tüm dosyaları *.txt* uzantısı bulunan herhangi bir yere altında *dizin* klasör.</span><span class="sxs-lookup"><span data-stu-id="533f1-235">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>
