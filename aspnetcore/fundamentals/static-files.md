---
title: "ASP.NET Core statik dosyalarıyla çalışma"
author: rick-anderson
description: "Hizmet ve statik dosyalar güvenli ve statik dosya ara yazılımı davranışları bir ASP.NET Core web uygulamasında barındırma yapılandırma hakkında bilgi edinin."
keywords: "ASP.NET Core, statik dosyalar, statik varlıklar, HTML, CSS, JavaScript"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: 912923860939a1d1dd91ccc79862e23f9095d161
ms.sourcegitcommit: a3e88639a6bcf8fb4d634036dac93130c464a097
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="work-with-static-files-in-aspnet-core"></a><span data-ttu-id="8a9df-104">ASP.NET Core statik dosyalarıyla çalışma</span><span class="sxs-lookup"><span data-stu-id="8a9df-104">Work with static files in ASP.NET Core</span></span>

<span data-ttu-id="8a9df-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="8a9df-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="8a9df-106">HTML, CSS, görüntüler ve JavaScript gibi statik dosyaları ASP.NET Core uygulama doğrudan istemcilere hizmet varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="8a9df-106">Static files, such as HTML, CSS, images, and JavaScript, are assets an ASP.NET Core app serves directly to clients.</span></span> <span data-ttu-id="8a9df-107">Bazı yapılandırmalar için bu dosyaların sunma etkinleştirmek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="8a9df-107">Some configuration is required to enable to serving of these files.</span></span>

<span data-ttu-id="8a9df-108">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8a9df-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="8a9df-109">Statik dosyaları işleme</span><span class="sxs-lookup"><span data-stu-id="8a9df-109">Serve static files</span></span>

<span data-ttu-id="8a9df-110">Statik dosyaları projenizin web kök dizininde depolanır.</span><span class="sxs-lookup"><span data-stu-id="8a9df-110">Static files are stored within your project's web root directory.</span></span> <span data-ttu-id="8a9df-111">Varsayılan dizin  *\<content_root > / wwwroot*, ancak aracılığıyla değiştirilebilir [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8a9df-111">The default directory is *\<content_root>/wwwroot*, but it can be changed via the [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) method.</span></span> <span data-ttu-id="8a9df-112">Bkz: [içerik kök](xref:fundamentals/index#content-root) ve [Web kök](xref:fundamentals/index#web-root) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="8a9df-112">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span>

<span data-ttu-id="8a9df-113">Uygulamanızın web ana içerik kök dizininin haberdar olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="8a9df-113">The app's web host must be made aware of the content root directory.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8a9df-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8a9df-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8a9df-115">`WebHost.CreateDefaultBuilder` Yöntemi geçerli dizine içerik kök ayarlar:</span><span class="sxs-lookup"><span data-stu-id="8a9df-115">The `WebHost.CreateDefaultBuilder` method sets the content root to the current directory:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8a9df-116">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8a9df-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8a9df-117">Çağırarak set geçerli dizine içerik root [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) içine `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="8a9df-117">Set the content root to the current directory by invoking [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) inside of `Program.Main`:</span></span>

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

<span data-ttu-id="8a9df-118">Statik dosyalar web kök göreli bir yol üzerinden erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="8a9df-118">Static files are accessible via a path relative to the web root.</span></span> <span data-ttu-id="8a9df-119">Örneğin, **Web uygulaması** proje şablonu içeren birkaç klasörlere *wwwroot* klasörü:</span><span class="sxs-lookup"><span data-stu-id="8a9df-119">For example, the **Web Application** project template contains several folders within the *wwwroot* folder:</span></span>

* <span data-ttu-id="8a9df-120">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="8a9df-120">**wwwroot**</span></span>
  * <span data-ttu-id="8a9df-121">**CSS**</span><span class="sxs-lookup"><span data-stu-id="8a9df-121">**css**</span></span>
  * <span data-ttu-id="8a9df-122">**görüntüleri**</span><span class="sxs-lookup"><span data-stu-id="8a9df-122">**images**</span></span>
  * <span data-ttu-id="8a9df-123">**js**</span><span class="sxs-lookup"><span data-stu-id="8a9df-123">**js**</span></span>

<span data-ttu-id="8a9df-124">Bir dosyaya erişmek için URI biçimi *görüntüleri* alt *http://\<server_address > /images/\<image_file_name >*.</span><span class="sxs-lookup"><span data-stu-id="8a9df-124">The URI format to access a file in the *images* subfolder is *http://\<server_address>/images/\<image_file_name>*.</span></span> <span data-ttu-id="8a9df-125">Örneğin, *http://localhost:9189/images/banner3.svg*.</span><span class="sxs-lookup"><span data-stu-id="8a9df-125">For example, *http://localhost:9189/images/banner3.svg*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8a9df-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8a9df-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8a9df-127">.NET Framework'ü hedefleme varsa ekleyin [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) projenize paket.</span><span class="sxs-lookup"><span data-stu-id="8a9df-127">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span> <span data-ttu-id="8a9df-128">.NET Core hedefleme varsa [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) bu paketi içerir.</span><span class="sxs-lookup"><span data-stu-id="8a9df-128">If targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) includes this package.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8a9df-129">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8a9df-129">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8a9df-130">Ekleme [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) projenize paket.</span><span class="sxs-lookup"><span data-stu-id="8a9df-130">Add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span>

---

<span data-ttu-id="8a9df-131">Yapılandırma [ara yazılım](xref:fundamentals/middleware) statik dosyaları sunma sağlar.</span><span class="sxs-lookup"><span data-stu-id="8a9df-131">Configure the [middleware](xref:fundamentals/middleware) which enables the serving of static files.</span></span>

### <a name="serve-files-inside-of-web-root"></a><span data-ttu-id="8a9df-132">Web kök içinde dosyaları sunar</span><span class="sxs-lookup"><span data-stu-id="8a9df-132">Serve files inside of web root</span></span>

<span data-ttu-id="8a9df-133">Çağırma [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) yöntemi içinde `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="8a9df-133">Invoke the [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method within `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

<span data-ttu-id="8a9df-134">Parametresiz `UseStaticFiles` yöntemi aşırı yüklemesini dosyaları web kök servable olarak işaretler.</span><span class="sxs-lookup"><span data-stu-id="8a9df-134">The parameterless `UseStaticFiles` method overload marks the files in web root as servable.</span></span> <span data-ttu-id="8a9df-135">Aşağıdaki biçimlendirme başvuru *wwwroot/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="8a9df-135">The following markup references *wwwroot/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a><span data-ttu-id="8a9df-136">Web kök dışında dosyaları sunar</span><span class="sxs-lookup"><span data-stu-id="8a9df-136">Serve files outside of web root</span></span>

<span data-ttu-id="8a9df-137">Sunulacak statik dosyaları dışında web kök bulunduğu bir dizin hiyerarşisi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="8a9df-137">Consider a directory hierarchy in which the static files to be served reside outside of the web root:</span></span>

* <span data-ttu-id="8a9df-138">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="8a9df-138">**wwwroot**</span></span>
  * <span data-ttu-id="8a9df-139">**CSS**</span><span class="sxs-lookup"><span data-stu-id="8a9df-139">**css**</span></span>
  * <span data-ttu-id="8a9df-140">**görüntüleri**</span><span class="sxs-lookup"><span data-stu-id="8a9df-140">**images**</span></span>
  * <span data-ttu-id="8a9df-141">**js**</span><span class="sxs-lookup"><span data-stu-id="8a9df-141">**js**</span></span>
* <span data-ttu-id="8a9df-142">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="8a9df-142">**MyStaticFiles**</span></span>
  * <span data-ttu-id="8a9df-143">**görüntüleri**</span><span class="sxs-lookup"><span data-stu-id="8a9df-143">**images**</span></span>
      * <span data-ttu-id="8a9df-144">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="8a9df-144">*banner1.svg*</span></span>

<span data-ttu-id="8a9df-145">Bir istek erişebilirsiniz *banner1.svg* statik dosya ara yazılımlarını şu şekilde yapılandırarak dosyası:</span><span class="sxs-lookup"><span data-stu-id="8a9df-145">A request can access the *banner1.svg* file by configuring the static file middleware as follows:</span></span>

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

<span data-ttu-id="8a9df-146">Önceki kod *MyStaticFiles* dizin hiyerarşisi gösterilir herkese açık şekilde aracılığıyla *StaticFiles* URI kesimi.</span><span class="sxs-lookup"><span data-stu-id="8a9df-146">In the preceding code, the *MyStaticFiles* directory hierarchy is exposed publicly via the *StaticFiles* URI segment.</span></span> <span data-ttu-id="8a9df-147">Bir istek *http://\<server_address > /StaticFiles/images/banner1.svg* hizmet *banner1.svg* dosya.</span><span class="sxs-lookup"><span data-stu-id="8a9df-147">A request to *http://\<server_address>/StaticFiles/images/banner1.svg* serves the *banner1.svg* file.</span></span>

<span data-ttu-id="8a9df-148">Aşağıdaki biçimlendirme başvuru *MyStaticFiles/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="8a9df-148">The following markup references *MyStaticFiles/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a><span data-ttu-id="8a9df-149">HTTP yanıt üstbilgilerini Ayarla</span><span class="sxs-lookup"><span data-stu-id="8a9df-149">Set HTTP response headers</span></span>

<span data-ttu-id="8a9df-150">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) nesnesi, HTTP yanıt üstbilgilerini Ayarla için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8a9df-150">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) object can be used to set HTTP response headers.</span></span> <span data-ttu-id="8a9df-151">Statik dosya sunucusu web kök yapılandırmaya ek olarak, aşağıdaki kod kümeleri `Cache-Control` üstbilgisi:</span><span class="sxs-lookup"><span data-stu-id="8a9df-151">In addition to configuring static file serving from the web root, the following code sets the `Cache-Control` header:</span></span>

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="8a9df-152">[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) yöntemi mevcut [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paket.</span><span class="sxs-lookup"><span data-stu-id="8a9df-152">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method exists in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

<span data-ttu-id="8a9df-153">Dosyaları 10 dakika (600 saniye olarak) için genel olarak alınabilir yapılmıştır:</span><span class="sxs-lookup"><span data-stu-id="8a9df-153">The files have been made publicly cacheable for 10 minutes (600 seconds):</span></span>

![Cache-Control üstbilgisinin gösteren yanıt üstbilgilerini eklendi](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="8a9df-155">Statik dosya yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="8a9df-155">Static file authorization</span></span>

<span data-ttu-id="8a9df-156">Statik dosya ara yazılımlarını yetkilendirme denetimleri sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="8a9df-156">The static file middleware doesn't provide authorization checks.</span></span> <span data-ttu-id="8a9df-157">Herhangi bir dosya sunulan işlem tarafından altında dahil olmak üzere *wwwroot*, genel olarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="8a9df-157">Any files served by it, including those under *wwwroot*, are publicly accessible.</span></span> <span data-ttu-id="8a9df-158">Dosyalara hizmet üzerinde Yetkilendirme göre:</span><span class="sxs-lookup"><span data-stu-id="8a9df-158">To serve files based on authorization:</span></span>

* <span data-ttu-id="8a9df-159">Bunları dışında depolamak *wwwroot* ve statik dosya ara yazılımı için erişilebilir olan herhangi bir dizin **ve**</span><span class="sxs-lookup"><span data-stu-id="8a9df-159">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>
* <span data-ttu-id="8a9df-160">Bunları yetkilendirme uygulandığı bir eylem yöntemiyle hizmet.</span><span class="sxs-lookup"><span data-stu-id="8a9df-160">Serve them via an action method to which authorization is applied.</span></span> <span data-ttu-id="8a9df-161">Dönüş bir [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) nesnesi:</span><span class="sxs-lookup"><span data-stu-id="8a9df-161">Return a [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) object:</span></span>

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a><span data-ttu-id="8a9df-162">Dizin taramayı etkinleştir</span><span class="sxs-lookup"><span data-stu-id="8a9df-162">Enable directory browsing</span></span>

<span data-ttu-id="8a9df-163">Dizin tarama dizin listesini görmek, web uygulamanızın kullanıcılara ve belirtilen bir dizin içinde dosyaları.</span><span class="sxs-lookup"><span data-stu-id="8a9df-163">Directory browsing allows users of your web app to see a directory listing and files within a specified directory.</span></span> <span data-ttu-id="8a9df-164">Dizin tarama varsayılan olarak devre dışıdır güvenlik nedenleriyle (bkz [konuları](#considerations)).</span><span class="sxs-lookup"><span data-stu-id="8a9df-164">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="8a9df-165">Etkinleştirme dizin harekete geçirerek tarama [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) yönteminde `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="8a9df-165">Enable directory browsing by invoking the [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) method in `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

<span data-ttu-id="8a9df-166">Gerekli hizmetler çağırarak eklemek [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) yönteminden `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8a9df-166">Add required services by invoking the [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

<span data-ttu-id="8a9df-167">Önceki kod, dizin tarama verir *wwwroot/görüntüleri* URL'yi kullanarak klasör *http://\<server_address > / MyImages*, her dosya ve klasör için bağlantılar ile birlikte:</span><span class="sxs-lookup"><span data-stu-id="8a9df-167">The preceding code allows directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*, with links to each file and folder:</span></span>

![Dizin tarama](static-files/_static/dir-browse.png)

<span data-ttu-id="8a9df-169">Bkz: [konuları](#considerations) güvenlik gözatma etkinleştirirken riskleri üzerinde.</span><span class="sxs-lookup"><span data-stu-id="8a9df-169">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="8a9df-170">İki Not `UseStaticFiles` aşağıdaki örnekte çağırır.</span><span class="sxs-lookup"><span data-stu-id="8a9df-170">Note the two `UseStaticFiles` calls in the following example.</span></span> <span data-ttu-id="8a9df-171">İlk çağrıda statik dosyaları sunma etkinleştirir *wwwroot* klasör.</span><span class="sxs-lookup"><span data-stu-id="8a9df-171">The first call enables the serving of static files in the *wwwroot* folder.</span></span> <span data-ttu-id="8a9df-172">İkinci çağrı, dizin taramayı etkinleştirir *wwwroot/görüntüleri* URL'yi kullanarak klasör *http://\<server_address > / MyImages*:</span><span class="sxs-lookup"><span data-stu-id="8a9df-172">The second call enables directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a><span data-ttu-id="8a9df-173">Varsayılan bir belge sunacak</span><span class="sxs-lookup"><span data-stu-id="8a9df-173">Serve a default document</span></span>

<span data-ttu-id="8a9df-174">Varsayılan giriş sayfası ayarı ziyaretçileri mantıksal bir başlangıç noktası, sitenizi ziyaret eden sağlar.</span><span class="sxs-lookup"><span data-stu-id="8a9df-174">Setting a default home page provides visitors a logical starting point when visiting your site.</span></span> <span data-ttu-id="8a9df-175">Varsayılan sayfa tam URI uygun kullanıcı hizmet vermek için arama [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) yönteminden `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="8a9df-175">To serve a default page without the user fully qualifying the URI, call the [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method from `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> <span data-ttu-id="8a9df-176">`UseDefaultFiles`önce çağrılmalıdır `UseStaticFiles` varsayılan dosyayı sunması için.</span><span class="sxs-lookup"><span data-stu-id="8a9df-176">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="8a9df-177">`UseDefaultFiles`dosyayı gerçekten sunması olmayan bir URL yeniden yazan olur.</span><span class="sxs-lookup"><span data-stu-id="8a9df-177">`UseDefaultFiles` is a URL rewriter that doesn't actually serve the file.</span></span> <span data-ttu-id="8a9df-178">Aracılığıyla statik dosya ara yazılımlarını etkinleştir `UseStaticFiles` dosyayı sunması için.</span><span class="sxs-lookup"><span data-stu-id="8a9df-178">Enable the static file middleware via `UseStaticFiles` to serve the file.</span></span>

<span data-ttu-id="8a9df-179">İle `UseDefaultFiles`, bir klasör arama istekleri:</span><span class="sxs-lookup"><span data-stu-id="8a9df-179">With `UseDefaultFiles`, requests to a folder search for:</span></span>

* <span data-ttu-id="8a9df-180">*default.htm*</span><span class="sxs-lookup"><span data-stu-id="8a9df-180">*default.htm*</span></span>
* <span data-ttu-id="8a9df-181">*default.html*</span><span class="sxs-lookup"><span data-stu-id="8a9df-181">*default.html*</span></span>
* <span data-ttu-id="8a9df-182">*index.htm*</span><span class="sxs-lookup"><span data-stu-id="8a9df-182">*index.htm*</span></span>
* <span data-ttu-id="8a9df-183">*index.html*</span><span class="sxs-lookup"><span data-stu-id="8a9df-183">*index.html*</span></span>

<span data-ttu-id="8a9df-184">İstek gibi davranarak tam uygun URI ilk listeden bulunan dosya sunulur.</span><span class="sxs-lookup"><span data-stu-id="8a9df-184">The first file found from the list is served as though the request were the fully qualified URI.</span></span> <span data-ttu-id="8a9df-185">Tarayıcı URL İstenen URI yansıtacak şekilde devam eder.</span><span class="sxs-lookup"><span data-stu-id="8a9df-185">The browser URL continues to reflect the URI requested.</span></span>

<span data-ttu-id="8a9df-186">Varsayılan dosya adı aşağıdaki kod değişiklikleri *mydefault.html*:</span><span class="sxs-lookup"><span data-stu-id="8a9df-186">The following code changes the default file name to *mydefault.html*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a><span data-ttu-id="8a9df-187">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="8a9df-187">UseFileServer</span></span>

<span data-ttu-id="8a9df-188">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) birleştirir `UseStaticFiles`, `UseDefaultFiles`, ve `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="8a9df-188">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="8a9df-189">Aşağıdaki kod, hizmet varsayılan dosya ve statik dosyaların etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="8a9df-189">The following code enables the serving of static files and the default file.</span></span> <span data-ttu-id="8a9df-190">Dizin tarama etkin değil.</span><span class="sxs-lookup"><span data-stu-id="8a9df-190">Directory browsing isn't enabled.</span></span>

```csharp
app.UseFileServer();
```

<span data-ttu-id="8a9df-191">Aşağıdaki kod, dizin tarama etkinleştirerek parametresiz aşırı oluşturur:</span><span class="sxs-lookup"><span data-stu-id="8a9df-191">The following code builds upon the parameterless overload by enabling directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

<span data-ttu-id="8a9df-192">Aşağıdaki dizin hiyerarşi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="8a9df-192">Consider the following directory hierarchy:</span></span>

* <span data-ttu-id="8a9df-193">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="8a9df-193">**wwwroot**</span></span>
  * <span data-ttu-id="8a9df-194">**CSS**</span><span class="sxs-lookup"><span data-stu-id="8a9df-194">**css**</span></span>
  * <span data-ttu-id="8a9df-195">**görüntüleri**</span><span class="sxs-lookup"><span data-stu-id="8a9df-195">**images**</span></span>
  * <span data-ttu-id="8a9df-196">**js**</span><span class="sxs-lookup"><span data-stu-id="8a9df-196">**js**</span></span>
* <span data-ttu-id="8a9df-197">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="8a9df-197">**MyStaticFiles**</span></span>
  * <span data-ttu-id="8a9df-198">**görüntüleri**</span><span class="sxs-lookup"><span data-stu-id="8a9df-198">**images**</span></span>
      * <span data-ttu-id="8a9df-199">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="8a9df-199">*banner1.svg*</span></span>
  * <span data-ttu-id="8a9df-200">*default.html*</span><span class="sxs-lookup"><span data-stu-id="8a9df-200">*default.html*</span></span>

<span data-ttu-id="8a9df-201">Aşağıdaki kod statik dosyalar, varsayılan dosya ve Dizin tarama etkinleştirir `MyStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="8a9df-201">The following code enables static files, default files, and directory browsing of `MyStaticFiles`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

<span data-ttu-id="8a9df-202">`AddDirectoryBrowser`ne zaman çağrılmalıdır `EnableDirectoryBrowsing` özellik değeri `true`:</span><span class="sxs-lookup"><span data-stu-id="8a9df-202">`AddDirectoryBrowser` must be called when the `EnableDirectoryBrowsing` property value is `true`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

<span data-ttu-id="8a9df-203">Dosya hiyerarşisi kullanarak ve önceki kod, URL şu şekilde çözün:</span><span class="sxs-lookup"><span data-stu-id="8a9df-203">Using the file hierarchy and preceding code, URLs resolve as follows:</span></span>

| <span data-ttu-id="8a9df-204">URI</span><span class="sxs-lookup"><span data-stu-id="8a9df-204">URI</span></span>            |                             <span data-ttu-id="8a9df-205">Yanıt</span><span class="sxs-lookup"><span data-stu-id="8a9df-205">Response</span></span>  |
| ------- | ------|
| <span data-ttu-id="8a9df-206">*http://\<server_address>/StaticFiles/images/banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="8a9df-206">*http://\<server_address>/StaticFiles/images/banner1.svg*</span></span>    |      <span data-ttu-id="8a9df-207">MyStaticFiles/images/banner1.svg</span><span class="sxs-lookup"><span data-stu-id="8a9df-207">MyStaticFiles/images/banner1.svg</span></span> |
| <span data-ttu-id="8a9df-208">*http://\<server_address>/StaticFiles*</span><span class="sxs-lookup"><span data-stu-id="8a9df-208">*http://\<server_address>/StaticFiles*</span></span>             |     <span data-ttu-id="8a9df-209">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="8a9df-209">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="8a9df-210">Varsayılan adlı dosya varsa *MyStaticFiles* dizin *http://\<server_address > / StaticFiles* dizin tıklanabilir bağlantıları ile listeleme döndürür:</span><span class="sxs-lookup"><span data-stu-id="8a9df-210">If no default-named file exists in the *MyStaticFiles* directory, *http://\<server_address>/StaticFiles* returns the directory listing with clickable links:</span></span>

![Statik dosyaların listesi](static-files/_static/db2.png)

> [!NOTE]
> <span data-ttu-id="8a9df-212">`UseDefaultFiles`ve `UseDirectoryBrowser` URL'sini kullanarak *http://\<server_address > / StaticFiles* istemci tarafı tetiklemek için eğik yeniden yönlendirme *http://\<server_address > / StaticFiles /*.</span><span class="sxs-lookup"><span data-stu-id="8a9df-212">`UseDefaultFiles` and `UseDirectoryBrowser` use the URL *http://\<server_address>/StaticFiles* without the trailing slash to trigger a client-side redirect to *http://\<server_address>/StaticFiles/*.</span></span> <span data-ttu-id="8a9df-213">Eğik eklenmesi dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="8a9df-213">Notice the addition of the trailing slash.</span></span> <span data-ttu-id="8a9df-214">Belgelerde göreli URL eğik geçersiz olarak kabul edilen.</span><span class="sxs-lookup"><span data-stu-id="8a9df-214">Relative URLs within the documents are deemed invalid without a trailing slash.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="8a9df-215">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="8a9df-215">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="8a9df-216">[FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) sınıfı içeren bir `Mappings` MIME içerik türleri için dosya uzantıları eşlemesi olarak hizmet veren özelliği.</span><span class="sxs-lookup"><span data-stu-id="8a9df-216">The [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) class contains a `Mappings` property serving as a mapping of file extensions to MIME content types.</span></span> <span data-ttu-id="8a9df-217">Aşağıdaki örnekte, çeşitli dosya uzantılarını bilinen MIME türleri için kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="8a9df-217">In the following sample, several file extensions are registered to known MIME types.</span></span> <span data-ttu-id="8a9df-218">*.Rtf* uzantısı değiştirilir ve *.mp4* kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="8a9df-218">The *.rtf* extension is replaced, and *.mp4* is removed.</span></span>

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

<span data-ttu-id="8a9df-219">Bkz: [MIME içerik türleri](http://www.iana.org/assignments/media-types/media-types.xhtml).</span><span class="sxs-lookup"><span data-stu-id="8a9df-219">See [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="8a9df-220">Standart olmayan içerik türleri</span><span class="sxs-lookup"><span data-stu-id="8a9df-220">Non-standard content types</span></span>

<span data-ttu-id="8a9df-221">Statik dosya ara yazılımlarını neredeyse 400 bilinen dosya içerik türleri bilir.</span><span class="sxs-lookup"><span data-stu-id="8a9df-221">The static file middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="8a9df-222">Kullanıcı bir bilinmeyen dosya türü bir dosya istediğinde, statik dosya ara yazılımlarını bir HTTP 404 (bulunamadı) yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="8a9df-222">If the user requests a file of an unknown file type, the static file middleware returns a HTTP 404 (Not Found) response.</span></span> <span data-ttu-id="8a9df-223">Dizin tarama etkin değilse, dosyaya bir bağlantı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8a9df-223">If directory browsing is enabled, a link to the file is displayed.</span></span> <span data-ttu-id="8a9df-224">URI HTTP 404 hatası döndürür.</span><span class="sxs-lookup"><span data-stu-id="8a9df-224">The URI returns an HTTP 404 error.</span></span>

<span data-ttu-id="8a9df-225">Aşağıdaki kod bilinmeyen tür hizmet veren sağlar ve bir görüntü olarak bilinmeyen dosya işler:</span><span class="sxs-lookup"><span data-stu-id="8a9df-225">The following code enables serving unknown types and renders the unknown file as an image:</span></span>

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="8a9df-226">Önceki kod ile bilinmeyen bir içerik türüne sahip bir dosya için bir istek bir görüntü olarak döndürülür.</span><span class="sxs-lookup"><span data-stu-id="8a9df-226">With the preceding code, a request for a file with an unknown content type is returned as an image.</span></span>

> [!WARNING]
> <span data-ttu-id="8a9df-227">Etkinleştirme [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) bir güvenlik riski oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8a9df-227">Enabling [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) is a security risk.</span></span> <span data-ttu-id="8a9df-228">Varsayılan olarak devre dışıdır ve kullanımı önerilmez.</span><span class="sxs-lookup"><span data-stu-id="8a9df-228">It's disabled by default, and its use is discouraged.</span></span> <span data-ttu-id="8a9df-229">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) standart olmayan uzantılı dosyaları sunma için daha güvenli bir alternatif sunar.</span><span class="sxs-lookup"><span data-stu-id="8a9df-229">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="8a9df-230">Dikkat Edilecekler</span><span class="sxs-lookup"><span data-stu-id="8a9df-230">Considerations</span></span>

> [!WARNING]
> <span data-ttu-id="8a9df-231">`UseDirectoryBrowser`ve `UseStaticFiles` gizli sızıntısı.</span><span class="sxs-lookup"><span data-stu-id="8a9df-231">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="8a9df-232">Devre dışı bırakma dizin üretimde tarama kullanmamanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="8a9df-232">Disabling directory browsing in production is highly recommended.</span></span> <span data-ttu-id="8a9df-233">Hangi dizinler aracılığıyla etkinleştirilir dikkatlice inceleyin `UseStaticFiles` veya `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="8a9df-233">Carefully review which directories are enabled via `UseStaticFiles` or `UseDirectoryBrowser`.</span></span> <span data-ttu-id="8a9df-234">Tüm dizin ve alt dizinleri genel olarak erişilebilir duruma gelir.</span><span class="sxs-lookup"><span data-stu-id="8a9df-234">The entire directory and its sub-directories become publicly accessible.</span></span> <span data-ttu-id="8a9df-235">Depolama dosyaları genel hizmet için uygun adanmış bir dizinde gibi  *\<content_root > / wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="8a9df-235">Store files suitable for serving to the public in a dedicated directory, such as *\<content_root>/wwwroot*.</span></span> <span data-ttu-id="8a9df-236">Bu dosyalar MVC görünümleri, Razor sayfalarının (yalnızca 2.x), yapılandırma dosyalarını, vb. ayırın.</span><span class="sxs-lookup"><span data-stu-id="8a9df-236">Separate these files from MVC views, Razor Pages (2.x only), configuration files, etc.</span></span>

* <span data-ttu-id="8a9df-237">İle kullanıma sunulan içerik için URL'leri `UseDirectoryBrowser` ve `UseStaticFiles` büyük küçük harfe duyarlılığın ve temeldeki dosya sistemi karakter kısıtlamalarını tabidir.</span><span class="sxs-lookup"><span data-stu-id="8a9df-237">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of the underlying file system.</span></span> <span data-ttu-id="8a9df-238">Örneğin, Windows büyük küçük harfe duyarlı&mdash;Mac ve Linux değil.</span><span class="sxs-lookup"><span data-stu-id="8a9df-238">For example, Windows is case insensitive&mdash;Mac and Linux aren't.</span></span>

* <span data-ttu-id="8a9df-239">IIS kullanımda barındırılan ASP.NET Core uygulamaları [ASP.NET Core Modülü (ANCM)](xref:fundamentals/servers/aspnet-core-module) tüm statik dosya istekleri dahil olmak üzere uygulama isteklerini iletmek için.</span><span class="sxs-lookup"><span data-stu-id="8a9df-239">ASP.NET Core apps hosted in IIS use the [ASP.NET Core Module (ANCM)](xref:fundamentals/servers/aspnet-core-module) to forward all requests to the app, including static file requests.</span></span> <span data-ttu-id="8a9df-240">IIS statik dosya işleyici kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="8a9df-240">The IIS static file handler isn't used.</span></span> <span data-ttu-id="8a9df-241">ANCM tarafından işlenen önce isteklerini işlemek için hiçbir olasılığı vardır.</span><span class="sxs-lookup"><span data-stu-id="8a9df-241">It has no chance to handle requests before they're handled by the ANCM.</span></span>

* <span data-ttu-id="8a9df-242">Sunucu veya Web sitesi düzeyinde IIS statik dosya işleyici kaldırmak için IIS Yöneticisi'nde aşağıdaki adımları tamamlayın:</span><span class="sxs-lookup"><span data-stu-id="8a9df-242">Complete the following steps in IIS Manager to remove the IIS static file handler at the server or website level:</span></span>
    1. <span data-ttu-id="8a9df-243">Gidin **modülleri** özelliği.</span><span class="sxs-lookup"><span data-stu-id="8a9df-243">Navigate to the **Modules** feature.</span></span>
    1. <span data-ttu-id="8a9df-244">Seçin **StaticFileModule** listesinde.</span><span class="sxs-lookup"><span data-stu-id="8a9df-244">Select **StaticFileModule** in the list.</span></span>
    1. <span data-ttu-id="8a9df-245">Tıklatın **kaldırmak** içinde **Eylemler** kenar.</span><span class="sxs-lookup"><span data-stu-id="8a9df-245">Click **Remove** in the **Actions** sidebar.</span></span>

> [!WARNING]
> <span data-ttu-id="8a9df-246">IIS statik dosya işleyici etkinleştirilirse **ve** ANCM yanlış yapılandırılmış, statik dosyalar sunulduğunu.</span><span class="sxs-lookup"><span data-stu-id="8a9df-246">If the IIS static file handler is enabled **and** the ANCM is configured incorrectly, static files are served.</span></span> <span data-ttu-id="8a9df-247">Bu, örneğin, olur *web.config* değil dosya dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="8a9df-247">This happens, for example, if the *web.config* file isn't deployed.</span></span>

* <span data-ttu-id="8a9df-248">Kod dosyaları yerleştirmek (de dahil olmak üzere *.cs* ve *.cshtml*) uygulama projenin web kökü dışında.</span><span class="sxs-lookup"><span data-stu-id="8a9df-248">Place code files (including *.cs* and *.cshtml*) outside of the app project's web root.</span></span> <span data-ttu-id="8a9df-249">Mantıksal ayırma, bu nedenle uygulamanın istemci-tarafı içerik ve sunucu tabanlı kod arasında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8a9df-249">A logical separation is therefore created between the app's client-side content and server-based code.</span></span> <span data-ttu-id="8a9df-250">Bu, sunucu tarafı kodu sızmasını önler.</span><span class="sxs-lookup"><span data-stu-id="8a9df-250">This prevents server-side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8a9df-251">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="8a9df-251">Additional resources</span></span>

* [<span data-ttu-id="8a9df-252">Ara Yazılım</span><span class="sxs-lookup"><span data-stu-id="8a9df-252">Middleware</span></span>](xref:fundamentals/middleware)

* [<span data-ttu-id="8a9df-253">ASP.NET Core giriş</span><span class="sxs-lookup"><span data-stu-id="8a9df-253">Introduction to ASP.NET Core</span></span>](xref:index)