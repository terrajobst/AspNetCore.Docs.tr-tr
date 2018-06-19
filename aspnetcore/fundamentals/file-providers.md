---
title: ASP.NET Core dosya sağlayıcıları
author: ardalis
description: ASP.NET Core dosya sistemi erişimini dosyasını sağlayıcıları kullanımı ile nasıl soyutlar öğrenin.
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/file-providers
ms.openlocfilehash: cdbffdadd9616fe941809d67dc2c0bbd52149561
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
ms.locfileid: "29724577"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="eff39-103">ASP.NET Core dosya sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="eff39-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="eff39-104">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="eff39-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="eff39-105">ASP.NET Core dosya sistemi erişimini dosyasını sağlayıcıları kullanımıyla soyutlar.</span><span class="sxs-lookup"><span data-stu-id="eff39-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span>

<span data-ttu-id="eff39-106">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="eff39-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-abstractions"></a><span data-ttu-id="eff39-107">Dosya sağlayıcısı soyutlamalar</span><span class="sxs-lookup"><span data-stu-id="eff39-107">File Provider abstractions</span></span>

<span data-ttu-id="eff39-108">Dosya bir Özet dosya sistemleri sağlayıcılarıdır.</span><span class="sxs-lookup"><span data-stu-id="eff39-108">File Providers are an abstraction over file systems.</span></span> <span data-ttu-id="eff39-109">Ana arabirim `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="eff39-109">The main interface is `IFileProvider`.</span></span> <span data-ttu-id="eff39-110">`IFileProvider` Dosya bilgileri almak için yöntemleri gösterir (`IFileInfo`), dizin bilgilerini (`IDirectoryContents`) ve değişiklik bildirimlerini ayarlama (kullanarak bir `IChangeToken`).</span><span class="sxs-lookup"><span data-stu-id="eff39-110">`IFileProvider` exposes methods to get file information (`IFileInfo`), directory information (`IDirectoryContents`), and to set up change notifications (using an `IChangeToken`).</span></span>

<span data-ttu-id="eff39-111">`IFileInfo` yöntemleri ve özellikleri hakkında belirli dosyaları veya dizinleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="eff39-111">`IFileInfo` provides methods and properties about individual files or directories.</span></span> <span data-ttu-id="eff39-112">İki Boole özelliğe sahip `Exists` ve `IsDirectory`, dosyanın açıklayan özelliklerinin yanı sıra `Name`, `Length` (bayt cinsinden), ve `LastModified` tarih.</span><span class="sxs-lookup"><span data-stu-id="eff39-112">It has two boolean properties, `Exists` and `IsDirectory`, as well as properties describing the file's `Name`, `Length` (in bytes), and `LastModified` date.</span></span> <span data-ttu-id="eff39-113">Kullanarak dosya okuyabilir, `CreateReadStream` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="eff39-113">You can read from the file using its `CreateReadStream` method.</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="eff39-114">Dosya sağlayıcısı uygulamaları</span><span class="sxs-lookup"><span data-stu-id="eff39-114">File Provider implementations</span></span>

<span data-ttu-id="eff39-115">Üç uygulamaları `IFileProvider` kullanılabilir: fiziksel, katıştırılmış ve bileşik.</span><span class="sxs-lookup"><span data-stu-id="eff39-115">Three implementations of `IFileProvider` are available: Physical, Embedded, and Composite.</span></span> <span data-ttu-id="eff39-116">Fiziksel sağlayıcısı gerçek sistemin dosyalara erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="eff39-116">The physical provider is used to access the actual system's files.</span></span> <span data-ttu-id="eff39-117">Katıştırılmış sağlayıcısı derlemelerde katıştırılmış dosyalara erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="eff39-117">The embedded provider is used to access files embedded in assemblies.</span></span> <span data-ttu-id="eff39-118">Bileşik sağlayıcısı, bir veya daha fazla diğer sağlayıcılardan gelen dosyaları ve dizinleri birleşik erişim sağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="eff39-118">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span>

### <a name="physicalfileprovider"></a><span data-ttu-id="eff39-119">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="eff39-119">PhysicalFileProvider</span></span>

<span data-ttu-id="eff39-120">`PhysicalFileProvider` Fiziksel dosya sistemindeki erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="eff39-120">The `PhysicalFileProvider` provides access to the physical file system.</span></span> <span data-ttu-id="eff39-121">Sarmaladığı `System.IO.File` bir dizin ve alt öğelerini tüm yollara kapsamı türü (fiziksel sağlayıcı için).</span><span class="sxs-lookup"><span data-stu-id="eff39-121">It wraps the `System.IO.File` type (for the physical provider), scoping all paths to a directory and its children.</span></span> <span data-ttu-id="eff39-122">Bu kapsam, belirli bir dizin ve bu sınır dışında dosya sistemine erişimi engelleme alt öğelerini erişimi sınırlar.</span><span class="sxs-lookup"><span data-stu-id="eff39-122">This scoping limits access to a certain directory and its children, preventing access to the file system outside of this boundary.</span></span> <span data-ttu-id="eff39-123">Bu sağlayıcı başlatılırken bu sağlayıcı için tüm istekler için taban yol yapılan (ve hangi bu yolu dışında erişimi kısıtlayan gibi) hizmet veren bir dizin yolu ile sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="eff39-123">When instantiating this provider, you must provide it with a directory path, which serves as the base path for all requests made to this provider (and which restricts access outside of this path).</span></span> <span data-ttu-id="eff39-124">Bir ASP.NET Core uygulama örneği bir `PhysicalFileProvider` doğrudan sağlayıcısı veya isteyebileceği bir `IFileProvider` bir denetleyici veya hizmetin oluşturucu kullanılarak [bağımlılık ekleme](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="eff39-124">In an ASP.NET Core app, you can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a Controller or service's constructor through [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="eff39-125">İkinci yaklaşımı genellikle daha esnek ve test edilebilir bir çözüm sunacak.</span><span class="sxs-lookup"><span data-stu-id="eff39-125">The latter approach will typically yield a more flexible and testable solution.</span></span>

<span data-ttu-id="eff39-126">Aşağıdaki örnekte nasıl oluşturulacağını gösterir bir `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="eff39-126">The sample below shows how to create a `PhysicalFileProvider`.</span></span>


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

<span data-ttu-id="eff39-127">Dizin içeriğini yineleme ya da bir alt yolu sağlayarak bir belirli dosyanın bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="eff39-127">You can iterate through its directory contents or get a specific file's information by providing a subpath.</span></span>

<span data-ttu-id="eff39-128">Sağlayıcı bir denetleyicisinden istemek için denetleyicinin oluşturucuda belirtin ve bir yerel alan atayın.</span><span class="sxs-lookup"><span data-stu-id="eff39-128">To request a provider from a controller, specify it in the controller's constructor and assign it to a local field.</span></span> <span data-ttu-id="eff39-129">Eylem yöntemleri yerel örneğinin kullanın:</span><span class="sxs-lookup"><span data-stu-id="eff39-129">Use the local instance from your action methods:</span></span>

[!code-csharp[](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

<span data-ttu-id="eff39-130">Ardından, uygulamanın içinde sağlayıcısı oluşturma `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="eff39-130">Then, create the provider in the app's `Startup` class:</span></span>

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

<span data-ttu-id="eff39-131">İçinde *Index.cshtml* görüntülemek için yinelemek `IDirectoryContents` sağlanan:</span><span class="sxs-lookup"><span data-stu-id="eff39-131">In the *Index.cshtml* view, iterate through the `IDirectoryContents` provided:</span></span>

[!code-html[](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

<span data-ttu-id="eff39-132">Sonuç:</span><span class="sxs-lookup"><span data-stu-id="eff39-132">The result:</span></span>

![Dosya sağlayıcı örnek uygulama listeleme fiziksel dosyalar ve klasörler](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a><span data-ttu-id="eff39-134">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="eff39-134">EmbeddedFileProvider</span></span>

<span data-ttu-id="eff39-135">`EmbeddedFileProvider` Derlemelerde katıştırılmış dosyalara erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="eff39-135">The `EmbeddedFileProvider` is used to access files embedded in assemblies.</span></span> <span data-ttu-id="eff39-136">.NET çekirdek ile derlemedeki dosyaları ekleme `<EmbeddedResource>` öğesinde *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="eff39-136">In .NET Core, you embed files in an assembly with the `<EmbeddedResource>` element in the *.csproj* file:</span></span>

[!code-json[](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

<span data-ttu-id="eff39-137">Kullanabileceğiniz [genelleme desenleri](#globbing-patterns) derlemede katıştırmak için dosyaları belirtirken.</span><span class="sxs-lookup"><span data-stu-id="eff39-137">You can use [globbing patterns](#globbing-patterns) when specifying files to embed in the assembly.</span></span> <span data-ttu-id="eff39-138">Bu düzenleri, bir veya daha fazla eşleştirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="eff39-138">These patterns can be used to match one or more files.</span></span>

> [!NOTE]
> <span data-ttu-id="eff39-139">Şimdiye kadar gerçekte kendi derlemesindeki projenizdeki her .js dosya eklemek istersiniz düşüktür; Yukarıdaki örnekte, yalnızca tanıtım amacıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="eff39-139">It's unlikely you would ever want to actually embed every .js file in your project in its assembly; the above sample is for demo purposes only.</span></span>

<span data-ttu-id="eff39-140">Oluştururken bir `EmbeddedFileProvider`, kendi oluşturucuya okumak derleme geçirin.</span><span class="sxs-lookup"><span data-stu-id="eff39-140">When creating an `EmbeddedFileProvider`, pass the assembly it will read to its constructor.</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="eff39-141">Yukarıdaki kod parçacığında nasıl oluşturulduğunu gösteren bir `EmbeddedFileProvider` şu anda yürütülen bütünleştirilmiş erişim.</span><span class="sxs-lookup"><span data-stu-id="eff39-141">The snippet above demonstrates how to create an `EmbeddedFileProvider` with access to the currently executing assembly.</span></span>

<span data-ttu-id="eff39-142">Kullanılacak örnek uygulama güncelleştirme bir `EmbeddedFileProvider` sonuçları aşağıdaki çıktı:</span><span class="sxs-lookup"><span data-stu-id="eff39-142">Updating the sample app to use an `EmbeddedFileProvider` results in the following output:</span></span>

![Dosya sağlayıcısı örnek uygulaması ekli dosyaları listeleme](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> <span data-ttu-id="eff39-144">Katıştırılmış kaynakları dizinleri sunmayın.</span><span class="sxs-lookup"><span data-stu-id="eff39-144">Embedded resources don't expose directories.</span></span> <span data-ttu-id="eff39-145">(Aracılığıyla kendi ad alanı) kaynak yolunu kullanarak kendi filename yerine, katıştırılmış `.` ayırıcılar.</span><span class="sxs-lookup"><span data-stu-id="eff39-145">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span>

> [!TIP]
> <span data-ttu-id="eff39-146">`EmbeddedFileProvider` Oluşturucusu isteğe bağlı bir kabul `baseNamespace` parametresi.</span><span class="sxs-lookup"><span data-stu-id="eff39-146">The `EmbeddedFileProvider` constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="eff39-147">Bu belirtme çağrıları kapsamını `GetDirectoryContents` bu kaynaklara sağlanan ad alanı altında.</span><span class="sxs-lookup"><span data-stu-id="eff39-147">Specifying this will scope calls to `GetDirectoryContents` to those resources under the provided namespace.</span></span>

### <a name="compositefileprovider"></a><span data-ttu-id="eff39-148">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="eff39-148">CompositeFileProvider</span></span>

<span data-ttu-id="eff39-149">`CompositeFileProvider` Birleştirir `IFileProvider` örnekleri, birden çok sağlayıcı dosyalarıyla çalışmak için tek bir arabirim gösterme.</span><span class="sxs-lookup"><span data-stu-id="eff39-149">The `CompositeFileProvider` combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="eff39-150">Oluştururken `CompositeFileProvider`, bir veya daha fazla geçirdiğiniz `IFileProvider` kendi oluşturucusunu örnekleri:</span><span class="sxs-lookup"><span data-stu-id="eff39-150">When creating the `CompositeFileProvider`, you pass one or more `IFileProvider` instances to its constructor:</span></span>

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

<span data-ttu-id="eff39-151">Kullanılacak örnek uygulama güncelleştirme bir `CompositeFileProvider` , önceden yapılandırılmış her iki fiziksel ve katıştırılmış sağlayıcıları içerir, sonuçları aşağıdaki çıktı:</span><span class="sxs-lookup"><span data-stu-id="eff39-151">Updating the sample app to use a `CompositeFileProvider` that includes both the physical and embedded providers configured previously, results in the following output:</span></span>

![Dosya sağlayıcısı örnek uygulaması fiziksel dosyaları ve klasörleri ve ekli dosyaları listeleme](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a><span data-ttu-id="eff39-153">Değişiklikleri izleme</span><span class="sxs-lookup"><span data-stu-id="eff39-153">Watching for changes</span></span>

<span data-ttu-id="eff39-154">`IFileProvider` `Watch` Yöntemi bir veya daha fazla dosyaları veya dizinleri değişiklikleri izlemek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="eff39-154">The `IFileProvider` `Watch` method provides a way to watch one or more files or directories for changes.</span></span> <span data-ttu-id="eff39-155">Bu yöntemi kullanabilirsiniz bir yol dizesini kabul [genelleme desenleri](#globbing-patterns) birden çok dosya ve döndürür belirtmek için bir `IChangeToken`.</span><span class="sxs-lookup"><span data-stu-id="eff39-155">This method accepts a path string, which can use [globbing patterns](#globbing-patterns) to specify multiple files, and returns an `IChangeToken`.</span></span> <span data-ttu-id="eff39-156">Bu belirteç sunan bir `HasChanged` Denetlenmekte, özellik ve `RegisterChangeCallback` değişiklikleri belirtilen yol dizesini algılandığında çağrılan yöntemi.</span><span class="sxs-lookup"><span data-stu-id="eff39-156">This token exposes a `HasChanged` property that can be inspected, and a `RegisterChangeCallback` method that's called when changes are detected to the specified path string.</span></span> <span data-ttu-id="eff39-157">Her değişiklik belirteci yalnızca ilişkili geri çağırma yanıt olarak tek bir değişiklik çağırdığı unutmayın.</span><span class="sxs-lookup"><span data-stu-id="eff39-157">Note that each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="eff39-158">Sabit izlemeyi etkinleştirmek için kullanabileceğiniz bir `TaskCompletionSource` aşağıda gösterildiği gibi veya yeniden oluşturun `IChangeToken` değişikliklere yanıt örneği.</span><span class="sxs-lookup"><span data-stu-id="eff39-158">To enable constant monitoring, you can use a `TaskCompletionSource` as shown below, or re-create `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="eff39-159">Bu makalenin örnekte, bir metin dosyası değiştirildiğinde bir ileti görüntülemek için bir konsol uygulaması yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="eff39-159">In this article's sample, a console application is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="eff39-160">Birkaç kez dosyayı kaydettikten sonra sonucu:</span><span class="sxs-lookup"><span data-stu-id="eff39-160">The result, after saving the file several times:</span></span>

![Dotnet Çalıştır yürütme uygulama quotes.txt dosyasındaki değişiklikleri izleme gösterir ve dosya beş kez değişti sonra komut penceresini açın.](file-providers/_static/watch-console.png)

> [!NOTE]
> <span data-ttu-id="eff39-162">Docker kapsayıcıları ve ağ paylaşımları gibi bazı dosya sistemleri değişiklik bildirimleri gönderebilmek değil.</span><span class="sxs-lookup"><span data-stu-id="eff39-162">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="eff39-163">Ayarlama `DOTNET_USE_POLLINGFILEWATCHER` ortam değişkenine `1` veya `true` 4 saniyede değişiklikleri için dosya sistemi yoklamak için.</span><span class="sxs-lookup"><span data-stu-id="eff39-163">Set the `DOTNET_USE_POLLINGFILEWATCHER` environment variable to `1` or `true` to poll the file system for changes every 4 seconds.</span></span>

## <a name="globbing-patterns"></a><span data-ttu-id="eff39-164">Genelleme desenleri</span><span class="sxs-lookup"><span data-stu-id="eff39-164">Globbing patterns</span></span>

<span data-ttu-id="eff39-165">Dosya sistemi yolları kullanın adında joker karakter düzenleri *genelleme desenleri*.</span><span class="sxs-lookup"><span data-stu-id="eff39-165">File system paths use wildcard patterns called *globbing patterns*.</span></span> <span data-ttu-id="eff39-166">Bu basit desenleri dosya gruplarını belirtmek üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="eff39-166">These simple patterns can be used to specify groups of files.</span></span> <span data-ttu-id="eff39-167">İki joker karakterler `*` ve `**`.</span><span class="sxs-lookup"><span data-stu-id="eff39-167">The two wildcard characters are `*` and `**`.</span></span>

**`*`**

   <span data-ttu-id="eff39-168">Herhangi bir şey geçerli klasör düzeyinde veya dosya ya da herhangi bir dosya uzantısına eşleşir.</span><span class="sxs-lookup"><span data-stu-id="eff39-168">Matches anything at the current folder level, or any filename, or any file extension.</span></span> <span data-ttu-id="eff39-169">Eşleşme tarafından sonlandırıldı `/` ve `.` dosya yolu karakter.</span><span class="sxs-lookup"><span data-stu-id="eff39-169">Matches are terminated by `/` and `.` characters in the file path.</span></span>

<strong><code>**</code></strong>

   <span data-ttu-id="eff39-170">Herhangi bir şey arasında birden çok dizin düzeyleri eşleşir.</span><span class="sxs-lookup"><span data-stu-id="eff39-170">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="eff39-171">Yinelemeli olarak kullanılabilir bir dizin hiyerarşisi içinde çok sayıda dosya eşleşmesi.</span><span class="sxs-lookup"><span data-stu-id="eff39-171">Can be used to recursively match many files within a directory hierarchy.</span></span>

### <a name="globbing-pattern-examples"></a><span data-ttu-id="eff39-172">Genelleme düzeni örnekleri</span><span class="sxs-lookup"><span data-stu-id="eff39-172">Globbing pattern examples</span></span>

**`directory/file.txt`**

   <span data-ttu-id="eff39-173">Belirli bir dizine belirli bir dosyayı eşleşir.</span><span class="sxs-lookup"><span data-stu-id="eff39-173">Matches a specific file in a specific directory.</span></span>

**<code>directory/*.txt</code>**

   <span data-ttu-id="eff39-174">Tüm dosyaları eşleşen `.txt` uzantısı'nda belirli bir dizin.</span><span class="sxs-lookup"><span data-stu-id="eff39-174">Matches all files with `.txt` extension in a specific directory.</span></span>

**`directory/*/bower.json`**

   <span data-ttu-id="eff39-175">Tüm eşleşen `bower.json` dizinleri tam olarak bir düzey alttaki dosyalarında `directory` dizin.</span><span class="sxs-lookup"><span data-stu-id="eff39-175">Matches all `bower.json` files in directories exactly one level below the `directory` directory.</span></span>

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   <span data-ttu-id="eff39-176">Tüm dosyaları eşleşen `.txt` uzantısı bulunan herhangi bir yere altında `directory` dizin.</span><span class="sxs-lookup"><span data-stu-id="eff39-176">Matches all files with `.txt` extension found anywhere under the `directory` directory.</span></span>

## <a name="file-provider-usage-in-aspnet-core"></a><span data-ttu-id="eff39-177">ASP.NET Core sağlayıcısı kullanım dosyası</span><span class="sxs-lookup"><span data-stu-id="eff39-177">File Provider usage in ASP.NET Core</span></span>

<span data-ttu-id="eff39-178">ASP.NET Core çeşitli bölümlerini dosya sağlayıcıları kullanır.</span><span class="sxs-lookup"><span data-stu-id="eff39-178">Several parts of ASP.NET Core utilize file providers.</span></span> <span data-ttu-id="eff39-179">`IHostingEnvironment` uygulamanın içerik kök ve web kökü olarak kullanıma sunar `IFileProvider` türleri.</span><span class="sxs-lookup"><span data-stu-id="eff39-179">`IHostingEnvironment` exposes the app's content root and web root as `IFileProvider` types.</span></span> <span data-ttu-id="eff39-180">Statik dosya ara yazılımı dosya sağlayıcıları statik dosyaları bulmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="eff39-180">The static files middleware uses file providers to locate static files.</span></span> <span data-ttu-id="eff39-181">Razor yapar kullanımına ağırlık `IFileProvider` görünümleri bulunmasında.</span><span class="sxs-lookup"><span data-stu-id="eff39-181">Razor makes heavy use of `IFileProvider` in locating views.</span></span> <span data-ttu-id="eff39-182">DotNet'in işlevselliğini kullanır dosya sağlayıcıları ve genelleme desenleri hangi dosyaların yayımlanması belirtmek için yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="eff39-182">Dotnet's publish functionality uses file providers and globbing patterns to specify which files should be published.</span></span>

## <a name="recommendations-for-use-in-apps"></a><span data-ttu-id="eff39-183">Uygulamaları kullanmak için öneriler</span><span class="sxs-lookup"><span data-stu-id="eff39-183">Recommendations for use in apps</span></span>

<span data-ttu-id="eff39-184">ASP.NET Core uygulamanızı dosya sistemi erişimi gerektiriyorsa, örneği isteyebilir `IFileProvider` bağımlılık ekleme aracılığıyla ve ardından yöntemlerinden erişim gerçekleştirmek için bu örnekte gösterildiği gibi kullanın.</span><span class="sxs-lookup"><span data-stu-id="eff39-184">If your ASP.NET Core app requires file system access, you can request an instance of `IFileProvider` through dependency injection, and then use its methods to perform the access, as shown in this sample.</span></span> <span data-ttu-id="eff39-185">Bu sağlayıcı uygulama başlatıldığında ve uygulamanızı başlatır uygulama türleri sayısını azaltan bir kez yapılandırmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="eff39-185">This allows you to configure the provider once, when the app starts up, and reduces the number of implementation types your app instantiates.</span></span>
