---
title: ASP.NET Core statik dosyalar
author: rick-anderson
description: Statik dosyaları sunma ve güvenli hale getirme ve bir ASP.NET Core Web uygulamasındaki statik dosya barındırma ara yazılım davranışlarını yapılandırma hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 07/8/2019
uid: fundamentals/static-files
ms.openlocfilehash: 1c665d1206e984fe41e9f57bb5356839c354dde2
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308188"
---
# <a name="static-files-in-aspnet-core"></a><span data-ttu-id="d942b-103">ASP.NET Core statik dosyalar</span><span class="sxs-lookup"><span data-stu-id="d942b-103">Static files in ASP.NET Core</span></span>

<span data-ttu-id="d942b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Scott Ade](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="d942b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="d942b-105">HTML, CSS, resim ve JavaScript gibi statik dosyalar, ASP.NET Core bir uygulamanın doğrudan istemcilere hizmet verdiği varlıklardır.</span><span class="sxs-lookup"><span data-stu-id="d942b-105">Static files, such as HTML, CSS, images, and JavaScript, are assets an ASP.NET Core app serves directly to clients.</span></span> <span data-ttu-id="d942b-106">Bu dosyalara hizmet sunma özelliğini etkinleştirmek için bazı yapılandırmalar gerekir.</span><span class="sxs-lookup"><span data-stu-id="d942b-106">Some configuration is required to enable serving of these files.</span></span>

<span data-ttu-id="d942b-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d942b-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="d942b-108">Statik dosyaları sunma</span><span class="sxs-lookup"><span data-stu-id="d942b-108">Serve static files</span></span>

<span data-ttu-id="d942b-109">Statik dosyalar projenin Web kök dizininde depolanır.</span><span class="sxs-lookup"><span data-stu-id="d942b-109">Static files are stored within the project's web root directory.</span></span> <span data-ttu-id="d942b-110">Varsayılan dizin,  *\<content_root >/Wwwroot*olur, ancak [usewebroot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) yöntemi aracılığıyla değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="d942b-110">The default directory is *\<content_root>/wwwroot*, but it can be changed via the [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) method.</span></span> <span data-ttu-id="d942b-111">Daha fazla bilgi için bkz. [içerik kökü](xref:fundamentals/index#content-root) ve [Web kök](xref:fundamentals/index#web-root) .</span><span class="sxs-lookup"><span data-stu-id="d942b-111">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span>

<span data-ttu-id="d942b-112">Uygulamanın Web ana bilgisayarı, içerik kök dizininden haberdar olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d942b-112">The app's web host must be made aware of the content root directory.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d942b-113">`WebHost.CreateDefaultBuilder` Yöntemi, içerik kökünü geçerli dizine ayarlar:</span><span class="sxs-lookup"><span data-stu-id="d942b-113">The `WebHost.CreateDefaultBuilder` method sets the content root to the current directory:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d942b-114">Içinde`Program.Main` [usecontentroot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) 'yi çağırarak içerik kökünü geçerli dizine ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="d942b-114">Set the content root to the current directory by invoking [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) inside of `Program.Main`:</span></span>

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

::: moniker-end

<span data-ttu-id="d942b-115">Statik dosyalara, Web köküne göre bir yol aracılığıyla erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="d942b-115">Static files are accessible via a path relative to the web root.</span></span> <span data-ttu-id="d942b-116">Örneğin, **Web uygulaması** proje şablonu *Wwwroot* klasörü içinde birkaç klasör içerir:</span><span class="sxs-lookup"><span data-stu-id="d942b-116">For example, the **Web Application** project template contains several folders within the *wwwroot* folder:</span></span>

* <span data-ttu-id="d942b-117">**Wwwroot**</span><span class="sxs-lookup"><span data-stu-id="d942b-117">**wwwroot**</span></span>
  * <span data-ttu-id="d942b-118">**Self**</span><span class="sxs-lookup"><span data-stu-id="d942b-118">**css**</span></span>
  * <span data-ttu-id="d942b-119">**yansımasını**</span><span class="sxs-lookup"><span data-stu-id="d942b-119">**images**</span></span>
  * <span data-ttu-id="d942b-120">**js**</span><span class="sxs-lookup"><span data-stu-id="d942b-120">**js**</span></span>

<span data-ttu-id="d942b-121">*Images* alt klasöründeki bir dosyaya erişmek için URI biçimi *\<http://server_address >\</images/image_file_name >* .</span><span class="sxs-lookup"><span data-stu-id="d942b-121">The URI format to access a file in the *images* subfolder is *http://\<server_address>/images/\<image_file_name>*.</span></span> <span data-ttu-id="d942b-122">Örneğin, *http://localhost:9189/images/banner3.svg* .</span><span class="sxs-lookup"><span data-stu-id="d942b-122">For example, *http://localhost:9189/images/banner3.svg*.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d942b-123">.NET Framework hedefliyorsanız, [Microsoft. AspNetCore. StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) paketini projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d942b-123">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to the project.</span></span> <span data-ttu-id="d942b-124">.NET Core hedefleniyorsa, [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) bu paketi içerir.</span><span class="sxs-lookup"><span data-stu-id="d942b-124">If targeting .NET Core, the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) includes this package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="d942b-125">.NET Framework hedefliyorsanız, [Microsoft. AspNetCore. StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) paketini projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d942b-125">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to the project.</span></span> <span data-ttu-id="d942b-126">.NET Core hedefleniyorsa, [Microsoft. AspNetCore. All metapackage](xref:fundamentals/metapackage) bu paketi içerir.</span><span class="sxs-lookup"><span data-stu-id="d942b-126">If targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) includes this package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d942b-127">Projeye [Microsoft. AspNetCore. StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) paketini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d942b-127">Add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to the project.</span></span>

::: moniker-end

<span data-ttu-id="d942b-128">Statik dosyaları sunmaya izin veren [ara yazılımı](xref:fundamentals/middleware/index) yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="d942b-128">Configure the [middleware](xref:fundamentals/middleware/index) which enables the serving of static files.</span></span>

### <a name="serve-files-inside-of-web-root"></a><span data-ttu-id="d942b-129">Web kökünün içindeki dosyaları sunma</span><span class="sxs-lookup"><span data-stu-id="d942b-129">Serve files inside of web root</span></span>

<span data-ttu-id="d942b-130">Içinde`Startup.Configure` [usestaticfiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) yöntemini çağır:</span><span class="sxs-lookup"><span data-stu-id="d942b-130">Invoke the [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method within `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

<span data-ttu-id="d942b-131">Parametresiz `UseStaticFiles` yöntemi aşırı yüklemesi, Web kökündeki dosyaları servable olarak işaretler.</span><span class="sxs-lookup"><span data-stu-id="d942b-131">The parameterless `UseStaticFiles` method overload marks the files in web root as servable.</span></span> <span data-ttu-id="d942b-132">Aşağıdaki biçimlendirme *Wwwroot/Images/banner1. SVG*öğesine başvuruyor:</span><span class="sxs-lookup"><span data-stu-id="d942b-132">The following markup references *wwwroot/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

<span data-ttu-id="d942b-133">Yukarıdaki kodda, tilde karakteri `~/` Webroot öğesine işaret eder.</span><span class="sxs-lookup"><span data-stu-id="d942b-133">In the preceding code, the tilde character `~/` points to webroot.</span></span> <span data-ttu-id="d942b-134">Daha fazla bilgi için bkz. [Web root](xref:fundamentals/index#web-root).</span><span class="sxs-lookup"><span data-stu-id="d942b-134">For more information, see [Web root](xref:fundamentals/index#web-root).</span></span>

### <a name="serve-files-outside-of-web-root"></a><span data-ttu-id="d942b-135">Dosyaları Web kökünün dışında sunma</span><span class="sxs-lookup"><span data-stu-id="d942b-135">Serve files outside of web root</span></span>

<span data-ttu-id="d942b-136">Sunulacak statik dosyaların Web kökünün dışında yer aldığı bir dizin hiyerarşisini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="d942b-136">Consider a directory hierarchy in which the static files to be served reside outside of the web root:</span></span>

* <span data-ttu-id="d942b-137">**Wwwroot**</span><span class="sxs-lookup"><span data-stu-id="d942b-137">**wwwroot**</span></span>
  * <span data-ttu-id="d942b-138">**Self**</span><span class="sxs-lookup"><span data-stu-id="d942b-138">**css**</span></span>
  * <span data-ttu-id="d942b-139">**yansımasını**</span><span class="sxs-lookup"><span data-stu-id="d942b-139">**images**</span></span>
  * <span data-ttu-id="d942b-140">**js**</span><span class="sxs-lookup"><span data-stu-id="d942b-140">**js**</span></span>
* <span data-ttu-id="d942b-141">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="d942b-141">**MyStaticFiles**</span></span>
  * <span data-ttu-id="d942b-142">**yansımasını**</span><span class="sxs-lookup"><span data-stu-id="d942b-142">**images**</span></span>
    * <span data-ttu-id="d942b-143">*banner1. SVG*</span><span class="sxs-lookup"><span data-stu-id="d942b-143">*banner1.svg*</span></span>

<span data-ttu-id="d942b-144">Bir istek statik dosya ara yazılımını aşağıdaki şekilde yapılandırarak *banner1. SVG* dosyasına erişebilir:</span><span class="sxs-lookup"><span data-stu-id="d942b-144">A request can access the *banner1.svg* file by configuring the Static File Middleware as follows:</span></span>

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

<span data-ttu-id="d942b-145">Yukarıdaki kodda, *mystaticfiles* dizin hiyerarşisi, *staticfiles* URI segmenti aracılığıyla herkese açıktır.</span><span class="sxs-lookup"><span data-stu-id="d942b-145">In the preceding code, the *MyStaticFiles* directory hierarchy is exposed publicly via the *StaticFiles* URI segment.</span></span> <span data-ttu-id="d942b-146">*Http://\<server_address >/StaticFiles/images/banner1.SVG* için bir istek *banner1. SVG* dosyasına hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="d942b-146">A request to *http://\<server_address>/StaticFiles/images/banner1.svg* serves the *banner1.svg* file.</span></span>

<span data-ttu-id="d942b-147">Aşağıdaki biçimlendirme *Mystaticfiles/Images/banner1. SVG*' ye başvurur:</span><span class="sxs-lookup"><span data-stu-id="d942b-147">The following markup references *MyStaticFiles/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a><span data-ttu-id="d942b-148">HTTP yanıt üstbilgilerini ayarla</span><span class="sxs-lookup"><span data-stu-id="d942b-148">Set HTTP response headers</span></span>

<span data-ttu-id="d942b-149">[Staticfileoptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) NESNESI, http yanıt üst bilgilerini ayarlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d942b-149">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) object can be used to set HTTP response headers.</span></span> <span data-ttu-id="d942b-150">Web kökünden statik dosya sunma yapılandırmasına ek olarak, aşağıdaki kod `Cache-Control` üst bilgisini ayarlar:</span><span class="sxs-lookup"><span data-stu-id="d942b-150">In addition to configuring static file serving from the web root, the following code sets the `Cache-Control` header:</span></span>

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="d942b-151">[Headerdictionaryextensions. Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) yöntemi, [Microsoft. Aspnetcore. http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paketinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="d942b-151">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method exists in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

<span data-ttu-id="d942b-152">Dosyalar, geliştirme ortamında 10 dakika (600 saniye) için genel olarak önbelleklenebilir hale getirilir:</span><span class="sxs-lookup"><span data-stu-id="d942b-152">The files have been made publicly cacheable for 10 minutes (600 seconds) in the Development environment:</span></span>

![Cache-Control üst bilgisini gösteren yanıt üstbilgileri eklendi](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="d942b-154">Statik dosya yetkilendirmesi</span><span class="sxs-lookup"><span data-stu-id="d942b-154">Static file authorization</span></span>

<span data-ttu-id="d942b-155">Statik dosya ara yazılımı yetkilendirme denetimleri sağlamıyor.</span><span class="sxs-lookup"><span data-stu-id="d942b-155">The Static File Middleware doesn't provide authorization checks.</span></span> <span data-ttu-id="d942b-156">*Wwwroot*altındakiler de dahil olmak üzere hizmet tarafından sunulan tüm dosyalar herkese açık olarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="d942b-156">Any files served by it, including those under *wwwroot*, are publicly accessible.</span></span> <span data-ttu-id="d942b-157">Dosyalara yetkilendirme temelinde hizmeti sağlamak için:</span><span class="sxs-lookup"><span data-stu-id="d942b-157">To serve files based on authorization:</span></span>

* <span data-ttu-id="d942b-158">Onları *Wwwroot* dışında ve statik dosya ara yazılımı tarafından erişilebilen herhangi bir dizinle saklayın.</span><span class="sxs-lookup"><span data-stu-id="d942b-158">Store them outside of *wwwroot* and any directory accessible to the Static File Middleware.</span></span>
* <span data-ttu-id="d942b-159">Yetkilendirmeyi uygulanan bir eylem yöntemi aracılığıyla onlara sunar.</span><span class="sxs-lookup"><span data-stu-id="d942b-159">Serve them via an action method to which authorization is applied.</span></span> <span data-ttu-id="d942b-160">Bir [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) nesnesi döndürür:</span><span class="sxs-lookup"><span data-stu-id="d942b-160">Return a [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) object:</span></span>

  [!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a><span data-ttu-id="d942b-161">Dizin taramayı etkinleştir</span><span class="sxs-lookup"><span data-stu-id="d942b-161">Enable directory browsing</span></span>

<span data-ttu-id="d942b-162">Dizin tarama, Web uygulamanızın kullanıcılarına belirtilen bir dizin içindeki bir dizin listesini ve dosyalarını görmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="d942b-162">Directory browsing allows users of your web app to see a directory listing and files within a specified directory.</span></span> <span data-ttu-id="d942b-163">Dizin tarama, güvenlik nedenleriyle varsayılan olarak devre dışıdır (bkz. [hususlar](#considerations)).</span><span class="sxs-lookup"><span data-stu-id="d942b-163">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="d942b-164">Içinde`Startup.Configure` [usedirectorybrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) metodunu çağırarak dizin taramayı etkinleştirin:</span><span class="sxs-lookup"><span data-stu-id="d942b-164">Enable directory browsing by invoking the [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) method in `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

<span data-ttu-id="d942b-165">Adresinden`Startup.ConfigureServices` [adddirectorybrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) yöntemini çağırarak gerekli hizmetleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d942b-165">Add required services by invoking the [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

<span data-ttu-id="d942b-166">Yukarıdaki kod, her bir dosya ve klasörün bağlantılarıyla birlikte *http://\<server_address >/myImages*URL 'sini kullanarak *Wwwroot/görüntüler* klasöründe Dizin taramasına izin verir:</span><span class="sxs-lookup"><span data-stu-id="d942b-166">The preceding code allows directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*, with links to each file and folder:</span></span>

![Dizin tarama](static-files/_static/dir-browse.png)

<span data-ttu-id="d942b-168">Göz atmayı etkinleştirirken güvenlik riskleri hakkındaki [noktalara](#considerations) göz atın.</span><span class="sxs-lookup"><span data-stu-id="d942b-168">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="d942b-169">Aşağıdaki örnekteki iki `UseStaticFiles` çağrının olduğunu aklınızda edin.</span><span class="sxs-lookup"><span data-stu-id="d942b-169">Note the two `UseStaticFiles` calls in the following example.</span></span> <span data-ttu-id="d942b-170">İlk çağrı *Wwwroot* klasöründeki statik dosyaları sunmaya izin veriyor.</span><span class="sxs-lookup"><span data-stu-id="d942b-170">The first call enables the serving of static files in the *wwwroot* folder.</span></span> <span data-ttu-id="d942b-171">İkinci çağrı, *http://\<server_address >/myImages*URL 'sini kullanarak *Wwwroot/görüntüler* klasöründe dizin taramayı mümkün bir şekilde sunar:</span><span class="sxs-lookup"><span data-stu-id="d942b-171">The second call enables directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a><span data-ttu-id="d942b-172">Varsayılan bir belge sunar</span><span class="sxs-lookup"><span data-stu-id="d942b-172">Serve a default document</span></span>

<span data-ttu-id="d942b-173">Varsayılan ana sayfanın ayarlanması, ziyaretçi sitenizi ziyaret ederken mantıksal bir başlangıç noktası sağlar.</span><span class="sxs-lookup"><span data-stu-id="d942b-173">Setting a default home page provides visitors a logical starting point when visiting your site.</span></span> <span data-ttu-id="d942b-174">Kullanıcı URI 'yi tamamen nitelemeden varsayılan bir sayfaya hizmeti sağlamak için, şu `Startup.Configure`kaynaktan [usedefaultfiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) metodunu çağırın:</span><span class="sxs-lookup"><span data-stu-id="d942b-174">To serve a default page without the user fully qualifying the URI, call the [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method from `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> <span data-ttu-id="d942b-175">`UseDefaultFiles`Varsayılan dosyaya kullanılmadan önce `UseStaticFiles` çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d942b-175">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="d942b-176">`UseDefaultFiles`, dosyayı gerçekten sunan bir URL yeniden yazar.</span><span class="sxs-lookup"><span data-stu-id="d942b-176">`UseDefaultFiles` is a URL rewriter that doesn't actually serve the file.</span></span> <span data-ttu-id="d942b-177">Dosya hizmeti `UseStaticFiles` sağlamak için statik dosya ara yazılımını etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="d942b-177">Enable Static File Middleware via `UseStaticFiles` to serve the file.</span></span>

<span data-ttu-id="d942b-178">İle `UseDefaultFiles`, bir klasör için arama istekleri:</span><span class="sxs-lookup"><span data-stu-id="d942b-178">With `UseDefaultFiles`, requests to a folder search for:</span></span>

* <span data-ttu-id="d942b-179">*default. htm*</span><span class="sxs-lookup"><span data-stu-id="d942b-179">*default.htm*</span></span>
* <span data-ttu-id="d942b-180">*default. html*</span><span class="sxs-lookup"><span data-stu-id="d942b-180">*default.html*</span></span>
* <span data-ttu-id="d942b-181">*index. htm*</span><span class="sxs-lookup"><span data-stu-id="d942b-181">*index.htm*</span></span>
* <span data-ttu-id="d942b-182">*index. html*</span><span class="sxs-lookup"><span data-stu-id="d942b-182">*index.html*</span></span>

<span data-ttu-id="d942b-183">Listedeki ilk dosya, istek tam URI olmasına rağmen olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="d942b-183">The first file found from the list is served as though the request were the fully qualified URI.</span></span> <span data-ttu-id="d942b-184">Tarayıcı URL 'SI, istenen URI 'yi yansıtacak şekilde devam ediyor.</span><span class="sxs-lookup"><span data-stu-id="d942b-184">The browser URL continues to reflect the URI requested.</span></span>

<span data-ttu-id="d942b-185">Aşağıdaki kod varsayılan dosya adını *mydefault. html*olarak değiştirir:</span><span class="sxs-lookup"><span data-stu-id="d942b-185">The following code changes the default file name to *mydefault.html*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a><span data-ttu-id="d942b-186">Usedosya sunucusu</span><span class="sxs-lookup"><span data-stu-id="d942b-186">UseFileServer</span></span>

<span data-ttu-id="d942b-187">[Usedosyasunucusu](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) `UseStaticFiles`, `UseDefaultFiles`, ve `UseDirectoryBrowser`işlevlerini birleştirir.</span><span class="sxs-lookup"><span data-stu-id="d942b-187">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="d942b-188">Aşağıdaki kod, statik dosyaların ve varsayılan dosyanın kullanılmasına izin veriyor.</span><span class="sxs-lookup"><span data-stu-id="d942b-188">The following code enables the serving of static files and the default file.</span></span> <span data-ttu-id="d942b-189">Dizin tarama etkin değil.</span><span class="sxs-lookup"><span data-stu-id="d942b-189">Directory browsing isn't enabled.</span></span>

```csharp
app.UseFileServer();
```

<span data-ttu-id="d942b-190">Aşağıdaki kod, dizin taramayı etkinleştirerek Parametresiz aşırı yüklemeden sonra oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="d942b-190">The following code builds upon the parameterless overload by enabling directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

<span data-ttu-id="d942b-191">Aşağıdaki dizin hiyerarşisini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="d942b-191">Consider the following directory hierarchy:</span></span>

* <span data-ttu-id="d942b-192">**Wwwroot**</span><span class="sxs-lookup"><span data-stu-id="d942b-192">**wwwroot**</span></span>
  * <span data-ttu-id="d942b-193">**Self**</span><span class="sxs-lookup"><span data-stu-id="d942b-193">**css**</span></span>
  * <span data-ttu-id="d942b-194">**yansımasını**</span><span class="sxs-lookup"><span data-stu-id="d942b-194">**images**</span></span>
  * <span data-ttu-id="d942b-195">**js**</span><span class="sxs-lookup"><span data-stu-id="d942b-195">**js**</span></span>
* <span data-ttu-id="d942b-196">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="d942b-196">**MyStaticFiles**</span></span>
  * <span data-ttu-id="d942b-197">**yansımasını**</span><span class="sxs-lookup"><span data-stu-id="d942b-197">**images**</span></span>
    * <span data-ttu-id="d942b-198">*banner1. SVG*</span><span class="sxs-lookup"><span data-stu-id="d942b-198">*banner1.svg*</span></span>
  * <span data-ttu-id="d942b-199">*default. html*</span><span class="sxs-lookup"><span data-stu-id="d942b-199">*default.html*</span></span>

<span data-ttu-id="d942b-200">Aşağıdaki kod statik dosyaları, varsayılan dosyaları ve dizin taramayı mümkün bir `MyStaticFiles`şekilde sunar:</span><span class="sxs-lookup"><span data-stu-id="d942b-200">The following code enables static files, default files, and directory browsing of `MyStaticFiles`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

<span data-ttu-id="d942b-201">`AddDirectoryBrowser``EnableDirectoryBrowsing` Özellik değeri şu `true`olduğunda çağrılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="d942b-201">`AddDirectoryBrowser` must be called when the `EnableDirectoryBrowsing` property value is `true`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

<span data-ttu-id="d942b-202">Dosya hiyerarşisini ve önceki kodu kullanarak, URL 'Ler aşağıdaki şekilde çözümlenir:</span><span class="sxs-lookup"><span data-stu-id="d942b-202">Using the file hierarchy and preceding code, URLs resolve as follows:</span></span>

| <span data-ttu-id="d942b-203">URI</span><span class="sxs-lookup"><span data-stu-id="d942b-203">URI</span></span>            |                             <span data-ttu-id="d942b-204">Yanıt</span><span class="sxs-lookup"><span data-stu-id="d942b-204">Response</span></span>  |
| ------- | ------|
| <span data-ttu-id="d942b-205">*http://\<server_address >/StaticFiles/images/banner1.SVG*</span><span class="sxs-lookup"><span data-stu-id="d942b-205">*http://\<server_address>/StaticFiles/images/banner1.svg*</span></span>    |      <span data-ttu-id="d942b-206">MyStaticFiles/Images/banner1. SVG</span><span class="sxs-lookup"><span data-stu-id="d942b-206">MyStaticFiles/images/banner1.svg</span></span> |
| <span data-ttu-id="d942b-207">*http://\<server_address >/staticfiles*</span><span class="sxs-lookup"><span data-stu-id="d942b-207">*http://\<server_address>/StaticFiles*</span></span>             |     <span data-ttu-id="d942b-208">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="d942b-208">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="d942b-209">*Mystaticfiles* dizininde varsayılan adlı dosya yoksa, *\<http://server_address >/staticfiles* , tıklatılabilir bağlantılarla dizin listesini döndürür:</span><span class="sxs-lookup"><span data-stu-id="d942b-209">If no default-named file exists in the *MyStaticFiles* directory, *http://\<server_address>/StaticFiles* returns the directory listing with clickable links:</span></span>

![Statik dosyalar listesi](static-files/_static/db2.png)

> [!NOTE]
> <span data-ttu-id="d942b-211"><xref:Microsoft.AspNetCore.Builder.DefaultFilesExtensions.UseDefaultFiles*>ve <xref:Microsoft.AspNetCore.Builder.DirectoryBrowserExtensions.UseDirectoryBrowser*> `http://{SERVER ADDRESS}/StaticFiles` (sonunda eğik çizgi olmadan) `http://{SERVER ADDRESS}/StaticFiles/` ile (sonunda eğik çizgiyle) bir istemci tarafı yeniden yönlendirmesi gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="d942b-211"><xref:Microsoft.AspNetCore.Builder.DefaultFilesExtensions.UseDefaultFiles*> and <xref:Microsoft.AspNetCore.Builder.DirectoryBrowserExtensions.UseDirectoryBrowser*> perform a client-side redirect from `http://{SERVER ADDRESS}/StaticFiles` (without a trailing slash) to `http://{SERVER ADDRESS}/StaticFiles/` (with a trailing slash).</span></span> <span data-ttu-id="d942b-212">*Staticfiles* dizinindeki göreli URL 'ler, sondaki eğik çizgi olmadan geçersizdir.</span><span class="sxs-lookup"><span data-stu-id="d942b-212">Relative URLs within the *StaticFiles* directory are invalid without a trailing slash.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="d942b-213">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="d942b-213">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="d942b-214">[Fileextensioncontenttypeprovider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) sınıfı, MIME içerik `Mappings` türlerine dosya uzantılarının eşlemesi olarak hizmet veren bir özellik içerir.</span><span class="sxs-lookup"><span data-stu-id="d942b-214">The [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) class contains a `Mappings` property serving as a mapping of file extensions to MIME content types.</span></span> <span data-ttu-id="d942b-215">Aşağıdaki örnekte, bazı dosya uzantıları bilinen MIME türlerine kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="d942b-215">In the following sample, several file extensions are registered to known MIME types.</span></span> <span data-ttu-id="d942b-216">*. Rtf* uzantısı değiştirilmiştir ve *. mp4* kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="d942b-216">The *.rtf* extension is replaced, and *.mp4* is removed.</span></span>

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

<span data-ttu-id="d942b-217">Bkz. [MIME içerik türleri](https://www.iana.org/assignments/media-types/media-types.xhtml).</span><span class="sxs-lookup"><span data-stu-id="d942b-217">See [MIME content types](https://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="d942b-218">Standart olmayan içerik türleri</span><span class="sxs-lookup"><span data-stu-id="d942b-218">Non-standard content types</span></span>

<span data-ttu-id="d942b-219">Statik dosya ara yazılımı, neredeyse 400 bilinen dosya içerik türlerini anlamıştır.</span><span class="sxs-lookup"><span data-stu-id="d942b-219">Static File Middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="d942b-220">Kullanıcı bilinmeyen bir dosya türüne sahip bir dosya isterse, statik dosya ara yazılımı isteği ardışık düzendeki bir sonraki ara yazılıma geçirir.</span><span class="sxs-lookup"><span data-stu-id="d942b-220">If the user requests a file with an unknown file type, Static File Middleware passes the request to the next middleware in the pipeline.</span></span> <span data-ttu-id="d942b-221">Bir ara yazılım, isteği işlediğinde, bir *404 bulunamadı* yanıtı döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d942b-221">If no middleware handles the request, a *404 Not Found* response is returned.</span></span> <span data-ttu-id="d942b-222">Dizin tarama etkinse, bir dizin listesinde dosyaya bir bağlantı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d942b-222">If directory browsing is enabled, a link to the file is displayed in a directory listing.</span></span>

<span data-ttu-id="d942b-223">Aşağıdaki kod, bilinmeyen türlere hizmet olarak bilinmeyen türler sunar ve bilinmeyen dosyayı görüntü olarak işler:</span><span class="sxs-lookup"><span data-stu-id="d942b-223">The following code enables serving unknown types and renders the unknown file as an image:</span></span>

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="d942b-224">Yukarıdaki kodla, bilinmeyen içerik türüne sahip bir dosya isteği görüntü olarak döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d942b-224">With the preceding code, a request for a file with an unknown content type is returned as an image.</span></span>

> [!WARNING]
> <span data-ttu-id="d942b-225">[ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) etkinleştirme bir güvenlik riskidir.</span><span class="sxs-lookup"><span data-stu-id="d942b-225">Enabling [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) is a security risk.</span></span> <span data-ttu-id="d942b-226">Varsayılan olarak devre dışıdır ve kullanımı önerilmez.</span><span class="sxs-lookup"><span data-stu-id="d942b-226">It's disabled by default, and its use is discouraged.</span></span> <span data-ttu-id="d942b-227">[Fileextensioncontenttypeprovider](#fileextensioncontenttypeprovider) standart olmayan uzantılara sahip dosyalara hizmet vermeye yönelik daha güvenli bir alternatif sağlar.</span><span class="sxs-lookup"><span data-stu-id="d942b-227">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="d942b-228">Dikkat Edilecekler</span><span class="sxs-lookup"><span data-stu-id="d942b-228">Considerations</span></span>

> [!WARNING]
> <span data-ttu-id="d942b-229">`UseDirectoryBrowser`ve `UseStaticFiles` gizli dizileri sızdırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d942b-229">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="d942b-230">Üretimde dizin taramayı devre dışı bırakmak önemle önerilir.</span><span class="sxs-lookup"><span data-stu-id="d942b-230">Disabling directory browsing in production is highly recommended.</span></span> <span data-ttu-id="d942b-231">`UseStaticFiles` Veya`UseDirectoryBrowser`ile hangi dizinlerin etkinleştirildiğini dikkatle gözden geçirin.</span><span class="sxs-lookup"><span data-stu-id="d942b-231">Carefully review which directories are enabled via `UseStaticFiles` or `UseDirectoryBrowser`.</span></span> <span data-ttu-id="d942b-232">Tüm dizin ve alt dizinleri herkese açık şekilde erişilebilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="d942b-232">The entire directory and its sub-directories become publicly accessible.</span></span> <span data-ttu-id="d942b-233">Content_root >/Wwwroot gibi özel bir dizinde  *\<* herkese sunma için uygun dosyaları depolayın.</span><span class="sxs-lookup"><span data-stu-id="d942b-233">Store files suitable for serving to the public in a dedicated directory, such as *\<content_root>/wwwroot*.</span></span> <span data-ttu-id="d942b-234">Bu dosyaları MVC görünümlerinden ayırın, Razor Pages (yalnızca 2. x), yapılandırma dosyaları vb.</span><span class="sxs-lookup"><span data-stu-id="d942b-234">Separate these files from MVC views, Razor Pages (2.x only), configuration files, etc.</span></span>

* <span data-ttu-id="d942b-235">`UseDirectoryBrowser` Ve`UseStaticFiles` ile sunulan içerik URL 'leri, temel dosya sisteminin büyük küçük harf duyarlılığı ve karakter kısıtlamalarına tabidir.</span><span class="sxs-lookup"><span data-stu-id="d942b-235">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of the underlying file system.</span></span> <span data-ttu-id="d942b-236">Örneğin, Windows büyük/küçük harfe&mdash;duyarsız MacOS ve Linux değildir.</span><span class="sxs-lookup"><span data-stu-id="d942b-236">For example, Windows is case insensitive&mdash;macOS and Linux aren't.</span></span>

* <span data-ttu-id="d942b-237">IIS 'de barındırılan ASP.NET Core uygulamalar, statik dosya istekleri de dahil olmak üzere tüm istekleri uygulamaya iletmek için [ASP.NET Core modülünü](xref:host-and-deploy/aspnet-core-module) kullanır.</span><span class="sxs-lookup"><span data-stu-id="d942b-237">ASP.NET Core apps hosted in IIS use the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to forward all requests to the app, including static file requests.</span></span> <span data-ttu-id="d942b-238">IIS statik dosya işleyicisi kullanılmıyor.</span><span class="sxs-lookup"><span data-stu-id="d942b-238">The IIS static file handler isn't used.</span></span> <span data-ttu-id="d942b-239">Modül tarafından işlenmek üzere istekleri işleme şansı yoktur.</span><span class="sxs-lookup"><span data-stu-id="d942b-239">It has no chance to handle requests before they're handled by the module.</span></span>

* <span data-ttu-id="d942b-240">Sunucu veya Web sitesi düzeyinde IIS statik dosya işleyicisini kaldırmak için IIS Yöneticisi ' nde aşağıdaki adımları uygulayın:</span><span class="sxs-lookup"><span data-stu-id="d942b-240">Complete the following steps in IIS Manager to remove the IIS static file handler at the server or website level:</span></span>
    1. <span data-ttu-id="d942b-241">**Modüller** özelliğine gidin.</span><span class="sxs-lookup"><span data-stu-id="d942b-241">Navigate to the **Modules** feature.</span></span>
    1. <span data-ttu-id="d942b-242">Listeden **StaticFileModule ' ü** seçin.</span><span class="sxs-lookup"><span data-stu-id="d942b-242">Select **StaticFileModule** in the list.</span></span>
    1. <span data-ttu-id="d942b-243">**Eylemler** kenar çubuğunda **Kaldır** ' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d942b-243">Click **Remove** in the **Actions** sidebar.</span></span>

> [!WARNING]
> <span data-ttu-id="d942b-244">IIS statik dosya işleyicisi etkinse **ve** ASP.NET Core modülü yanlış yapılandırılmışsa, statik dosyalar sunulur.</span><span class="sxs-lookup"><span data-stu-id="d942b-244">If the IIS static file handler is enabled **and** the ASP.NET Core Module is configured incorrectly, static files are served.</span></span> <span data-ttu-id="d942b-245">Bu, örneğin, *Web. config* dosyası dağıtılmamışsa oluşur.</span><span class="sxs-lookup"><span data-stu-id="d942b-245">This happens, for example, if the *web.config* file isn't deployed.</span></span>

* <span data-ttu-id="d942b-246">Kod dosyalarını ( *. cs* ve *. cshtml*dahil) uygulama projesinin Web kökünün dışına yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="d942b-246">Place code files (including *.cs* and *.cshtml*) outside of the app project's web root.</span></span> <span data-ttu-id="d942b-247">Bu nedenle, uygulamanın istemci tarafı içeriği ile sunucu tabanlı kod arasında bir mantıksal ayrım oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d942b-247">A logical separation is therefore created between the app's client-side content and server-based code.</span></span> <span data-ttu-id="d942b-248">Bu, sunucu tarafı kodun sızmasını önler.</span><span class="sxs-lookup"><span data-stu-id="d942b-248">This prevents server-side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d942b-249">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d942b-249">Additional resources</span></span>

* [<span data-ttu-id="d942b-250">Ara Yazılım</span><span class="sxs-lookup"><span data-stu-id="d942b-250">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="d942b-251">ASP.NET Core'a giriş</span><span class="sxs-lookup"><span data-stu-id="d942b-251">Introduction to ASP.NET Core</span></span>](xref:index)
