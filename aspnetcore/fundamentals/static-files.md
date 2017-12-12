---
title: "ASP.NET Core statik dosyaları ile çalışma"
author: rick-anderson
description: "ASP.NET Core statik dosyaları ile çalışmayı öğrenin."
keywords: "ASP.NET Core, statik dosyalar, statik varlıklar, HTML, CSS, JavaScript"
ms.author: riande
manager: wpickett
ms.date: 4/07/2017
ms.topic: article
ms.assetid: e32245c7-4eee-4831-bd2e-915dbf9f5f70
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/static-files
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c0751576a1391f26f045c3f8c42ea39c0ff6e5d9
ms.sourcegitcommit: e4fb6b13be56a0fb2f2778623740a047d6489227
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="working-with-static-files-in-aspnet-core"></a><span data-ttu-id="8aee2-104">ASP.NET Core statik dosyaları ile çalışma</span><span class="sxs-lookup"><span data-stu-id="8aee2-104">Working with static files in ASP.NET Core</span></span>

<a name="fundamentals-static-files"></a>

<span data-ttu-id="8aee2-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8aee2-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8aee2-106">HTML, CSS, görüntü ve JavaScript gibi statik dosyaları ASP.NET Core uygulama doğrudan istemcilere hizmet verebilir varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="8aee2-106">Static files, such as HTML, CSS, image, and JavaScript, are assets that an ASP.NET Core app can serve directly to clients.</span></span>

<span data-ttu-id="8aee2-107">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8aee2-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="serving-static-files"></a><span data-ttu-id="8aee2-108">Statik dosyaları sunma</span><span class="sxs-lookup"><span data-stu-id="8aee2-108">Serving static files</span></span>

<span data-ttu-id="8aee2-109">Statik dosyalar genellikle yerleştirilir `web root` (*\<içeriği kök > / wwwroot*) klasör.</span><span class="sxs-lookup"><span data-stu-id="8aee2-109">Static files are typically located in the `web root` (*\<content-root>/wwwroot*) folder.</span></span> <span data-ttu-id="8aee2-110">Bkz: [içerik kök](xref:fundamentals/index#content-root) ve [Web kök](xref:fundamentals/index#web-root) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="8aee2-110">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span> <span data-ttu-id="8aee2-111">Geçerli dizin olarak içerik kök genellikle ayarlamak için projenizin `web root` geliştirme sırasında bulundu.</span><span class="sxs-lookup"><span data-stu-id="8aee2-111">You generally set the content root to be the current directory so that your project's `web root` will be found while in development.</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]

<span data-ttu-id="8aee2-112">Statik dosyalar, altında herhangi bir klasörde depolanabilir `web root` ve o kökü için göreli bir yol ile erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="8aee2-112">Static files can be stored in any folder under the `web root` and accessed with a relative path to that root.</span></span> <span data-ttu-id="8aee2-113">Örneğin, Visual Studio kullanarak bir varsayılan Web uygulaması projesi oluşturduğunuzda, içinde oluşturulan çeşitli klasörler vardır *wwwroot* klasör - *css*, *görüntüleri*, ve *js*.</span><span class="sxs-lookup"><span data-stu-id="8aee2-113">For example, when you create a default Web application project using Visual Studio, there are several folders created within the *wwwroot*  folder - *css*, *images*, and *js*.</span></span> <span data-ttu-id="8aee2-114">Görüntüde erişmek için URI *görüntüleri* alt:</span><span class="sxs-lookup"><span data-stu-id="8aee2-114">The URI to access an image in the *images* subfolder:</span></span>

* `http://<app>/images/<imageFileName>`
* `http://localhost:9189/images/banner3.svg`

<span data-ttu-id="8aee2-115">Statik dosyaların sunulması sırayla yapılandırmanız gerekir [ara yazılım](middleware.md) ardışık düzene statik dosyaları eklemek için.</span><span class="sxs-lookup"><span data-stu-id="8aee2-115">In order for static files to be served, you must configure the [Middleware](middleware.md) to add static files to the pipeline.</span></span> <span data-ttu-id="8aee2-116">Statik dosya ara yazılımlarını bir bağımlılık ekleyerek yapılandırılabilir *Microsoft.AspNetCore.StaticFiles* paketini proje ve ardından arama `UseStaticFiles` uzantısı yönteminden `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="8aee2-116">The static file middleware can be configured by adding a dependency on the *Microsoft.AspNetCore.StaticFiles* package to your project and then calling the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]

<span data-ttu-id="8aee2-117">`app.UseStaticFiles();`dosyaları yapar `web root` (*wwwroot* varsayılan olarak) servable.</span><span class="sxs-lookup"><span data-stu-id="8aee2-117">`app.UseStaticFiles();` makes the files in `web root` (*wwwroot* by default) servable.</span></span> <span data-ttu-id="8aee2-118">Daha sonra diğer dizin içeriği ile servable nasıl göstereceğiz `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="8aee2-118">Later I'll show how to make other directory contents servable with `UseStaticFiles`.</span></span>

<span data-ttu-id="8aee2-119">"Microsoft.AspNetCore.StaticFiles" NuGet paketini eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="8aee2-119">You must include the NuGet package "Microsoft.AspNetCore.StaticFiles".</span></span>

> [!NOTE]
> <span data-ttu-id="8aee2-120">`web root`Varsayılan olarak *wwwroot* dizin, ancak ayarlayabilirsiniz `web root` ile dizin `UseWebRoot`.</span><span class="sxs-lookup"><span data-stu-id="8aee2-120">`web root` defaults to the *wwwroot* directory, but you can set the `web root` directory with `UseWebRoot`.</span></span>

<span data-ttu-id="8aee2-121">Hizmet istediğiniz statik dosyaların nerede dışında bir proje hiyerarşisi olduğunu varsayalım `web root`.</span><span class="sxs-lookup"><span data-stu-id="8aee2-121">Suppose you have a project hierarchy where the static files you wish to serve are outside the `web root`.</span></span> <span data-ttu-id="8aee2-122">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="8aee2-122">For example:</span></span>

* <span data-ttu-id="8aee2-123">wwwroot</span><span class="sxs-lookup"><span data-stu-id="8aee2-123">wwwroot</span></span>
  * <span data-ttu-id="8aee2-124">CSS</span><span class="sxs-lookup"><span data-stu-id="8aee2-124">css</span></span>
  * <span data-ttu-id="8aee2-125">görüntüler</span><span class="sxs-lookup"><span data-stu-id="8aee2-125">images</span></span>
  * <span data-ttu-id="8aee2-126">...</span><span class="sxs-lookup"><span data-stu-id="8aee2-126">...</span></span>
* <span data-ttu-id="8aee2-127">MyStaticFiles</span><span class="sxs-lookup"><span data-stu-id="8aee2-127">MyStaticFiles</span></span>
  * <span data-ttu-id="8aee2-128">Test.PNG</span><span class="sxs-lookup"><span data-stu-id="8aee2-128">test.png</span></span>

<span data-ttu-id="8aee2-129">Bir isteğin erişmek *test.png*, statik dosya ara yazılımı aşağıdaki gibi yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="8aee2-129">For a request to access *test.png*, configure the static files middleware as follows:</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]

<span data-ttu-id="8aee2-130">Bir istek `http://<app>/StaticFiles/test.png` görecek *test.png* dosya.</span><span class="sxs-lookup"><span data-stu-id="8aee2-130">A request to `http://<app>/StaticFiles/test.png` will serve the *test.png* file.</span></span>

<span data-ttu-id="8aee2-131">`StaticFileOptions()`Yanıt Üstbilgileri ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8aee2-131">`StaticFileOptions()` can set response headers.</span></span> <span data-ttu-id="8aee2-132">Örneğin, aşağıdaki kodu gelen statik dosya ayarlar *wwwroot* klasörü ve kümelerini `Cache-Control` başlığı 10 dakika (600 saniye olarak) olmalarını genel olarak alınabilir:</span><span class="sxs-lookup"><span data-stu-id="8aee2-132">For example, the code below sets up static file serving from the *wwwroot* folder and sets the `Cache-Control` header to make them publicly cacheable for 10 minutes (600 seconds):</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]

<span data-ttu-id="8aee2-133">[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) yöntemi edinilebilir [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paket.</span><span class="sxs-lookup"><span data-stu-id="8aee2-133">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method is available from the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span> <span data-ttu-id="8aee2-134">Ekleme `using Microsoft.AspNetCore.Http;` için *csharp* yöntemi kullanılamıyorsa, dosya.</span><span class="sxs-lookup"><span data-stu-id="8aee2-134">Add `using Microsoft.AspNetCore.Http;` to your *csharp* file if the method is unavailable.</span></span>

![Cache-Control üstbilgisinin gösteren yanıt üstbilgilerini eklendi](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="8aee2-136">Statik dosya yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="8aee2-136">Static file authorization</span></span>

<span data-ttu-id="8aee2-137">Statik dosya modülü sağlar **hiçbir** yetkilendirme denetimleri.</span><span class="sxs-lookup"><span data-stu-id="8aee2-137">The static file module provides **no** authorization checks.</span></span> <span data-ttu-id="8aee2-138">Herhangi bir dosya sunulan işlem tarafından altında dahil olmak üzere *wwwroot* genel olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8aee2-138">Any files served by it, including those under *wwwroot* are publicly available.</span></span> <span data-ttu-id="8aee2-139">Dosyalara hizmet üzerinde Yetkilendirme göre:</span><span class="sxs-lookup"><span data-stu-id="8aee2-139">To serve files based on authorization:</span></span>

* <span data-ttu-id="8aee2-140">Bunları dışında depolamak *wwwroot* ve statik dosya ara yazılımı için erişilebilir olan herhangi bir dizin **ve**</span><span class="sxs-lookup"><span data-stu-id="8aee2-140">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>

* <span data-ttu-id="8aee2-141">Döndüren bir denetleyici eylemi hizmet bir `FileResult` yetkilendirme burada uygulanır</span><span class="sxs-lookup"><span data-stu-id="8aee2-141">Serve them through a controller action, returning a `FileResult` where authorization is applied</span></span>

## <a name="enabling-directory-browsing"></a><span data-ttu-id="8aee2-142">Dizin taramayı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="8aee2-142">Enabling directory browsing</span></span>

<span data-ttu-id="8aee2-143">Dizin tarama dizin ve belirli bir dizindeki dosyaların listesini görmek, web uygulamanızın verir.</span><span class="sxs-lookup"><span data-stu-id="8aee2-143">Directory browsing allows the user of your web app to see a list of directories and files within a specified directory.</span></span> <span data-ttu-id="8aee2-144">Dizin tarama varsayılan olarak devre dışıdır güvenlik nedenleriyle (bkz [konuları](#considerations)).</span><span class="sxs-lookup"><span data-stu-id="8aee2-144">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="8aee2-145">Dizin taramayı etkinleştirmek için arama `UseDirectoryBrowser` uzantısı yönteminden `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="8aee2-145">To enable directory browsing, call the `UseDirectoryBrowser` extension method from  `Startup.Configure`:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet1)]

<span data-ttu-id="8aee2-146">Gerekli hizmetler çağırarak ekleyin `AddDirectoryBrowser` uzantısı yönteminden `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8aee2-146">And add required services by calling `AddDirectoryBrowser` extension method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet2)]

<span data-ttu-id="8aee2-147">Yukarıdaki kod, dizin tarama verir *wwwroot/görüntüleri* klasörü URL http:// kullanarak\<uygulama > / her dosya ve klasör için bağlantılar ile birlikte MyImages:</span><span class="sxs-lookup"><span data-stu-id="8aee2-147">The code above allows directory browsing of the *wwwroot/images* folder using the URL http://\<app>/MyImages, with links to each file and folder:</span></span>

![Dizin tarama](static-files/_static/dir-browse.png)

<span data-ttu-id="8aee2-149">Bkz: [konuları](#considerations) güvenlik gözatma etkinleştirirken riskleri üzerinde.</span><span class="sxs-lookup"><span data-stu-id="8aee2-149">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="8aee2-150">İki Not `app.UseStaticFiles` çağrıları.</span><span class="sxs-lookup"><span data-stu-id="8aee2-150">Note the two `app.UseStaticFiles` calls.</span></span> <span data-ttu-id="8aee2-151">Birinci CSS, görüntüler ve JavaScript hizmet vermek için gereken *wwwroot* klasörü ve Dizin tarama için ikinci çağrı *wwwroot/görüntüleri* klasörü URL http:// kullanarak\<uygulama > / MyImages:</span><span class="sxs-lookup"><span data-stu-id="8aee2-151">The first one is required to serve the CSS, images and JavaScript in the *wwwroot* folder, and the second call for directory browsing of the *wwwroot/images* folder using the URL http://\<app>/MyImages:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]

## <a name="serving-a-default-document"></a><span data-ttu-id="8aee2-152">Hizmet veren bir varsayılan belge</span><span class="sxs-lookup"><span data-stu-id="8aee2-152">Serving a default document</span></span>

<span data-ttu-id="8aee2-153">Varsayılan giriş sayfası ayarı site ziyaretçilerini sitenizi ziyaret eden başlatmak için bir yer sağlar.</span><span class="sxs-lookup"><span data-stu-id="8aee2-153">Setting a default home page gives site visitors a place to start when visiting your site.</span></span> <span data-ttu-id="8aee2-154">URI tam olarak nitelemek gerek kalmadan kullanıcı bir varsayılan sayfa sunmak Web uygulamanız için sırayla çağırın `UseDefaultFiles` uzantısı yönteminden `Startup.Configure` gibi.</span><span class="sxs-lookup"><span data-stu-id="8aee2-154">In order for your Web app to serve a default page without the user having to fully qualify the URI, call the `UseDefaultFiles` extension method from `Startup.Configure` as follows.</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]

> [!NOTE]
> <span data-ttu-id="8aee2-155">`UseDefaultFiles`önce çağrılmalıdır `UseStaticFiles` varsayılan dosyayı sunması için.</span><span class="sxs-lookup"><span data-stu-id="8aee2-155">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="8aee2-156">`UseDefaultFiles`dosyayı gerçekten sunması olmayan bir URL yeniden yazıcı olur.</span><span class="sxs-lookup"><span data-stu-id="8aee2-156">`UseDefaultFiles` is a URL re-writer that doesn't actually serve the file.</span></span> <span data-ttu-id="8aee2-157">Statik dosya ara yazılımlarını etkinleştir (`UseStaticFiles`) dosyayı sunması için.</span><span class="sxs-lookup"><span data-stu-id="8aee2-157">You must enable the static file middleware (`UseStaticFiles`) to serve the file.</span></span>

<span data-ttu-id="8aee2-158">İle `UseDefaultFiles`, bir klasör için istekleri için arama:</span><span class="sxs-lookup"><span data-stu-id="8aee2-158">With `UseDefaultFiles`, requests to a folder will search for:</span></span>

* <span data-ttu-id="8aee2-159">default.htm</span><span class="sxs-lookup"><span data-stu-id="8aee2-159">default.htm</span></span>
* <span data-ttu-id="8aee2-160">default.HTML</span><span class="sxs-lookup"><span data-stu-id="8aee2-160">default.html</span></span>
* <span data-ttu-id="8aee2-161">index.htm</span><span class="sxs-lookup"><span data-stu-id="8aee2-161">index.htm</span></span>
* <span data-ttu-id="8aee2-162">index.HTML</span><span class="sxs-lookup"><span data-stu-id="8aee2-162">index.html</span></span>

<span data-ttu-id="8aee2-163">(Tarayıcı URL İstenen URI göstermeye devam eder ancak) istek tam uygun URI boşmuş gibi ilk listeden bulunan dosya sunulacak.</span><span class="sxs-lookup"><span data-stu-id="8aee2-163">The first file found from the list will be served as if the request was the fully qualified URI (although the browser URL will continue to show the URI requested).</span></span>

<span data-ttu-id="8aee2-164">Aşağıdaki kod için varsayılan dosya adını değiştirmek nasıl gösterir *mydefault.html*.</span><span class="sxs-lookup"><span data-stu-id="8aee2-164">The following code shows how to change the default file name to *mydefault.html*.</span></span>

[!code-csharp[Main](static-files/sample/StartupDefault.cs?name=snippet1)]

## <a name="usefileserver"></a><span data-ttu-id="8aee2-165">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="8aee2-165">UseFileServer</span></span>

<span data-ttu-id="8aee2-166">`UseFileServer`birleştirir `UseStaticFiles`, `UseDefaultFiles`, ve `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="8aee2-166">`UseFileServer` combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="8aee2-167">Aşağıdaki kod statik dosyaları ve sunulması için varsayılan dosya etkinleştirir Dizin tarama izin vermez ancak:</span><span class="sxs-lookup"><span data-stu-id="8aee2-167">The following code enables static files and the default file to be served, but does not allow directory browsing:</span></span>

```csharp
app.UseFileServer();
   ```

<span data-ttu-id="8aee2-168">Aşağıdaki kod, statik dosyalar, varsayılan dosya ve dizin taramayı etkinleştirir:</span><span class="sxs-lookup"><span data-stu-id="8aee2-168">The following code enables static files, default files and  directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
   ```

<span data-ttu-id="8aee2-169">Bkz: [konuları](#considerations) güvenlik gözatma etkinleştirirken riskleri üzerinde.</span><span class="sxs-lookup"><span data-stu-id="8aee2-169">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span> <span data-ttu-id="8aee2-170">Olduğu gibi `UseStaticFiles`, `UseDefaultFiles`, ve `UseDirectoryBrowser`, dışında mevcut dosyaları işleme almak isterseniz `web root`, örneği ve yapılandırma bir `FileServerOptions` bir parametre olarak geçirdiğiniz nesne `UseFileServer`.</span><span class="sxs-lookup"><span data-stu-id="8aee2-170">As with `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`, if you wish to serve files that exist outside the `web root`, you instantiate and configure an `FileServerOptions` object that you pass as a parameter to `UseFileServer`.</span></span> <span data-ttu-id="8aee2-171">Örneğin, aşağıdaki dizin hiyerarşi Web uygulamanızda verilen:</span><span class="sxs-lookup"><span data-stu-id="8aee2-171">For example, given the following directory hierarchy in your Web app:</span></span>

* <span data-ttu-id="8aee2-172">wwwroot</span><span class="sxs-lookup"><span data-stu-id="8aee2-172">wwwroot</span></span>

  * <span data-ttu-id="8aee2-173">CSS</span><span class="sxs-lookup"><span data-stu-id="8aee2-173">css</span></span>

  * <span data-ttu-id="8aee2-174">görüntüler</span><span class="sxs-lookup"><span data-stu-id="8aee2-174">images</span></span>

  * <span data-ttu-id="8aee2-175">...</span><span class="sxs-lookup"><span data-stu-id="8aee2-175">...</span></span>

* <span data-ttu-id="8aee2-176">MyStaticFiles</span><span class="sxs-lookup"><span data-stu-id="8aee2-176">MyStaticFiles</span></span>

  * <span data-ttu-id="8aee2-177">Test.PNG</span><span class="sxs-lookup"><span data-stu-id="8aee2-177">test.png</span></span>

  * <span data-ttu-id="8aee2-178">default.HTML</span><span class="sxs-lookup"><span data-stu-id="8aee2-178">default.html</span></span>

<span data-ttu-id="8aee2-179">Yukarıdaki hiyerarşisi örneği kullanarak, statik dosyalar, varsayılan dosyalar ve için gözatma etkinleştirmek isteyebilirsiniz `MyStaticFiles` dizin.</span><span class="sxs-lookup"><span data-stu-id="8aee2-179">Using the hierarchy example above, you might want to enable static files, default files, and browsing for the `MyStaticFiles` directory.</span></span> <span data-ttu-id="8aee2-180">Aşağıdaki kod parçacığında, gerçekleştirilir tek çağrısıyla `FileServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="8aee2-180">In the following code snippet, that is accomplished with a single call to `FileServerOptions`.</span></span>

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]

<span data-ttu-id="8aee2-181">Varsa `enableDirectoryBrowsing` ayarlanır `true` aramak için gerekli `AddDirectoryBrowser` uzantısı yönteminden `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8aee2-181">If `enableDirectoryBrowsing` is set to `true` you are required to call `AddDirectoryBrowser` extension method from  `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?name=snippet2)]

<span data-ttu-id="8aee2-182">Dosya hiyerarşisi ve yukarıdaki kodu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="8aee2-182">Using the file hierarchy and code above:</span></span>

| <span data-ttu-id="8aee2-183">URI</span><span class="sxs-lookup"><span data-stu-id="8aee2-183">URI</span></span>            |                             <span data-ttu-id="8aee2-184">Yanıt</span><span class="sxs-lookup"><span data-stu-id="8aee2-184">Response</span></span>  |
| ------- | ------|
| `http://<app>/StaticFiles/test.png`    |      <span data-ttu-id="8aee2-185">MyStaticFiles/test.png</span><span class="sxs-lookup"><span data-stu-id="8aee2-185">MyStaticFiles/test.png</span></span> |
| `http://<app>/StaticFiles`              |     <span data-ttu-id="8aee2-186">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="8aee2-186">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="8aee2-187">Dosyaları adlı varsayılan olsalar *MyStaticFiles* dizin, http://\<uygulama > / StaticFiles dizin tıklanabilir bağlantıları ile listeleme döndürür:</span><span class="sxs-lookup"><span data-stu-id="8aee2-187">If no default named files are in the *MyStaticFiles* directory, http://\<app>/StaticFiles returns the directory listing with clickable links:</span></span>

![Statik dosyaların listesi](static-files/_static/db2.PNG)

> [!NOTE]
> <span data-ttu-id="8aee2-189">`UseDefaultFiles`ve `UseDirectoryBrowser` url http:// sürer\<uygulama > / StaticFiles eğik ve neden olmadan bir istemci tarafı yönlendirmek için http://\<uygulama > /StaticFiles/ (eğik ekleyerek).</span><span class="sxs-lookup"><span data-stu-id="8aee2-189">`UseDefaultFiles` and `UseDirectoryBrowser` will take the url http://\<app>/StaticFiles without the trailing slash and cause a client side redirect to http://\<app>/StaticFiles/ (adding the trailing slash).</span></span> <span data-ttu-id="8aee2-190">Sondaki eğik çizgi göreli URL belgelerde olmadan yanlış olabilir.</span><span class="sxs-lookup"><span data-stu-id="8aee2-190">Without the trailing slash relative URLs within the documents would be incorrect.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="8aee2-191">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="8aee2-191">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="8aee2-192">`FileExtensionContentTypeProvider` Sınıfı MIME içerik türleri için dosya uzantıları eşleyen bir koleksiyonu içerir.</span><span class="sxs-lookup"><span data-stu-id="8aee2-192">The `FileExtensionContentTypeProvider` class contains a  collection that maps file extensions to MIME content types.</span></span> <span data-ttu-id="8aee2-193">Aşağıdaki örnekte, bilinen MIME türleri için birkaç dosya uzantılarının kayıtlı olduğundan, ".rtf" değiştirilir ve ".mp4" kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="8aee2-193">In the following sample, several file extensions are registered to known MIME types, the ".rtf" is replaced, and ".mp4" is removed.</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]

<span data-ttu-id="8aee2-194">Bkz: [MIME içerik türleri](http://www.iana.org/assignments/media-types/media-types.xhtml).</span><span class="sxs-lookup"><span data-stu-id="8aee2-194">See   [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="8aee2-195">Standart olmayan içerik türleri</span><span class="sxs-lookup"><span data-stu-id="8aee2-195">Non-standard content types</span></span>

<span data-ttu-id="8aee2-196">ASP.NET statik dosya ara yazılımlarını neredeyse 400 bilinen dosya içerik türleri bilir.</span><span class="sxs-lookup"><span data-stu-id="8aee2-196">The ASP.NET static file middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="8aee2-197">Kullanıcı bir bilinmeyen dosya türü bir dosya istediğinde, statik dosya ara yazılımlarını (bulunamadı) HTTP 404 yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="8aee2-197">If the user requests a file of an unknown file type, the static file middleware returns a HTTP 404 (Not found) response.</span></span> <span data-ttu-id="8aee2-198">Dizin tarama etkin olduğunda dosyaya bir bağlantı görüntülenir, ancak URI HTTP 404 hata döndürür.</span><span class="sxs-lookup"><span data-stu-id="8aee2-198">If directory browsing is enabled, a link to the file will be displayed, but the URI will return an HTTP 404 error.</span></span>

<span data-ttu-id="8aee2-199">Aşağıdaki kod, bilinmeyen tür hizmet veren sağlar ve bilinmeyen dosya bir resim olarak kabul eder.</span><span class="sxs-lookup"><span data-stu-id="8aee2-199">The following code enables serving unknown types and will render the unknown file as an image.</span></span>

[!code-csharp[Main](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]

<span data-ttu-id="8aee2-200">Yukarıdaki kodu ile birlikte bir istek bilinmeyen bir içerik türüne sahip bir dosya için bir resim olarak döndürülür.</span><span class="sxs-lookup"><span data-stu-id="8aee2-200">With the code above, a request for a file with an unknown content type will be returned as an image.</span></span>

>[!WARNING]
> <span data-ttu-id="8aee2-201">Etkinleştirme `ServeUnknownFileTypes` bir güvenlik riski oluşturur ve bunu kullanarak önerilmez.</span><span class="sxs-lookup"><span data-stu-id="8aee2-201">Enabling `ServeUnknownFileTypes` is a security risk and using it is discouraged.</span></span>  <span data-ttu-id="8aee2-202">`FileExtensionContentTypeProvider`(yukarıda açıklanan) standart olmayan uzantılı dosyaları sunma için daha güvenli bir alternatif sunar.</span><span class="sxs-lookup"><span data-stu-id="8aee2-202">`FileExtensionContentTypeProvider`  (explained above) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="8aee2-203">Dikkat Edilecekler</span><span class="sxs-lookup"><span data-stu-id="8aee2-203">Considerations</span></span>

>[!WARNING]
> <span data-ttu-id="8aee2-204">`UseDirectoryBrowser`ve `UseStaticFiles` gizli sızıntısı.</span><span class="sxs-lookup"><span data-stu-id="8aee2-204">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="8aee2-205">Öneririz, **değil** etkinleştir dizin üretimde tarama.</span><span class="sxs-lookup"><span data-stu-id="8aee2-205">We recommend that you **not** enable directory browsing in production.</span></span> <span data-ttu-id="8aee2-206">Dikkatli olun ile etkinleştirme hakkında hangi dizinleri `UseStaticFiles` veya `UseDirectoryBrowser` tüm dizin ve tüm alt dizinler erişilemeyeceği.</span><span class="sxs-lookup"><span data-stu-id="8aee2-206">Be careful about which directories you enable with `UseStaticFiles` or `UseDirectoryBrowser` as the entire directory and all sub-directories will be accessible.</span></span> <span data-ttu-id="8aee2-207">Ortak içeriğe gibi kendi dizininde tutma öneririz  *\<içerik kök > / wwwroot*, uygulama görünümleri, yapılandırma dosyalarını, vb. ayrılmayın.</span><span class="sxs-lookup"><span data-stu-id="8aee2-207">We recommend keeping public content in its own directory such as *\<content root>/wwwroot*, away from application views, configuration files, etc.</span></span>

* <span data-ttu-id="8aee2-208">İle kullanıma sunulan içerik için URL'leri `UseDirectoryBrowser` ve `UseStaticFiles` büyük küçük harfe duyarlılığın ve bunların temel alınan dosya sisteminin karakter kısıtlamaları tabidir.</span><span class="sxs-lookup"><span data-stu-id="8aee2-208">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of their underlying file system.</span></span> <span data-ttu-id="8aee2-209">Örneğin, Windows büyük küçük harfe duyarlı ancak Mac ve Linux değildir.</span><span class="sxs-lookup"><span data-stu-id="8aee2-209">For example, Windows is case insensitive, but Mac and Linux are not.</span></span>

* <span data-ttu-id="8aee2-210">IIS'de barındırılan ASP.NET Core uygulamaları ASP.NET Core modülü statik dosyaları için istekleri dahil olmak üzere uygulama için tüm istekleri iletmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="8aee2-210">ASP.NET Core applications hosted in IIS use the ASP.NET Core Module to forward all requests to the application including requests for static files.</span></span> <span data-ttu-id="8aee2-211">ASP.NET çekirdeği modülü tarafından işlenen önce isteklerini işlemek için bir fırsat açılmıyor çünkü IIS statik dosya işleyici kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="8aee2-211">The IIS static file handler is not used because it doesn't get a chance to handle requests before they are handled by the ASP.NET Core Module.</span></span>

* <span data-ttu-id="8aee2-212">IIS statik dosya işleyici (düzeyinde sunucusunu veya Web sitesi) kaldırmak için:</span><span class="sxs-lookup"><span data-stu-id="8aee2-212">To remove the IIS static file handler (at the server or website level):</span></span>

     * <span data-ttu-id="8aee2-213">Gidin **modülleri** özelliği</span><span class="sxs-lookup"><span data-stu-id="8aee2-213">Navigate to the **Modules** feature</span></span>

     * <span data-ttu-id="8aee2-214">Seçin **StaticFileModule** listesinde</span><span class="sxs-lookup"><span data-stu-id="8aee2-214">Select **StaticFileModule** in the list</span></span>

     * <span data-ttu-id="8aee2-215">Dokunun **kaldırmak** içinde **Eylemler** kenar çubuğu</span><span class="sxs-lookup"><span data-stu-id="8aee2-215">Tap **Remove** in the **Actions** sidebar</span></span>

>[!WARNING]
> <span data-ttu-id="8aee2-216">IIS statik dosya işleyici etkinleştirilirse **ve** ASP.NET Core Modülü (ANCM) düzgün yapılandırılmamış (örneğin, *web.config* dağıtılmamış), statik dosyalar hizmet verilen.</span><span class="sxs-lookup"><span data-stu-id="8aee2-216">If the IIS static file handler is enabled **and** the ASP.NET Core Module (ANCM) is not correctly configured (for example if *web.config* was not deployed), static files will be served.</span></span>

* <span data-ttu-id="8aee2-217">Kod dosyaları (c# ve Razor dahil) uygulama projenin dışında yerleştirilmelidir `web root` (*wwwroot* varsayılan olarak).</span><span class="sxs-lookup"><span data-stu-id="8aee2-217">Code files (including c# and Razor) should be placed outside of the app project's `web root` (*wwwroot* by default).</span></span> <span data-ttu-id="8aee2-218">Bu, uygulamanızın istemci tarafı içeriği ve sunucu tarafı kodu sızmasını engeller sunucu tarafı kaynak kodu arasında temiz bir ayrım oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8aee2-218">This creates a clean separation between your app's client side content and server side source code, which prevents server side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8aee2-219">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="8aee2-219">Additional Resources</span></span>

* [<span data-ttu-id="8aee2-220">Ara yazılım</span><span class="sxs-lookup"><span data-stu-id="8aee2-220">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="8aee2-221">ASP.NET Core giriş</span><span class="sxs-lookup"><span data-stu-id="8aee2-221">Introduction to ASP.NET Core</span></span>](../index.md)
