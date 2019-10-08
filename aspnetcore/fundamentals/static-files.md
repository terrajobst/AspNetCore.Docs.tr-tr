---
title: ASP.NET Core statik dosyalar
author: rick-anderson
description: Statik dosyaları sunma ve güvenli hale getirme ve bir ASP.NET Core Web uygulamasındaki statik dosya barındırma ara yazılım davranışlarını yapılandırma hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: fundamentals/static-files
ms.openlocfilehash: 2f153551a86860616469200862723528e4a8cc1c
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007320"
---
# <a name="static-files-in-aspnet-core"></a><span data-ttu-id="bcae8-103">ASP.NET Core statik dosyalar</span><span class="sxs-lookup"><span data-stu-id="bcae8-103">Static files in ASP.NET Core</span></span>

<span data-ttu-id="bcae8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Scott Ade](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="bcae8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="bcae8-105">HTML, CSS, resim ve JavaScript gibi statik dosyalar, ASP.NET Core bir uygulamanın doğrudan istemcilere hizmet verdiği varlıklardır.</span><span class="sxs-lookup"><span data-stu-id="bcae8-105">Static files, such as HTML, CSS, images, and JavaScript, are assets an ASP.NET Core app serves directly to clients.</span></span> <span data-ttu-id="bcae8-106">Bu dosyalara hizmet sunma özelliğini etkinleştirmek için bazı yapılandırmalar gerekir.</span><span class="sxs-lookup"><span data-stu-id="bcae8-106">Some configuration is required to enable serving of these files.</span></span>

<span data-ttu-id="bcae8-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bcae8-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="bcae8-108">Statik dosyaları sunma</span><span class="sxs-lookup"><span data-stu-id="bcae8-108">Serve static files</span></span>

<span data-ttu-id="bcae8-109">Statik dosyalar projenin [Web kök](xref:fundamentals/index#web-root) dizininde depolanır.</span><span class="sxs-lookup"><span data-stu-id="bcae8-109">Static files are stored within the project's [web root](xref:fundamentals/index#web-root) directory.</span></span> <span data-ttu-id="bcae8-110">Varsayılan dizin *{Content root}/Wwwroot*, ancak [usewebroot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) yöntemi aracılığıyla değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="bcae8-110">The default directory is *{content root}/wwwroot*, but it can be changed via the [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) method.</span></span> <span data-ttu-id="bcae8-111">Daha fazla bilgi için bkz. [içerik kökü](xref:fundamentals/index#content-root) ve [Web kök](xref:fundamentals/index#web-root) .</span><span class="sxs-lookup"><span data-stu-id="bcae8-111">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span>

<span data-ttu-id="bcae8-112">Uygulamanın Web ana bilgisayarı, içerik kök dizininden haberdar olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bcae8-112">The app's web host must be made aware of the content root directory.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="bcae8-113">@No__t-0 yöntemi, içerik kökünü geçerli dizine ayarlar:</span><span class="sxs-lookup"><span data-stu-id="bcae8-113">The `WebHost.CreateDefaultBuilder` method sets the content root to the current directory:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="bcae8-114">@No__t-1 ' in içinde [Usecontentroot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) 'yi çağırarak içerik kökünü geçerli dizine ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="bcae8-114">Set the content root to the current directory by invoking [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) inside of `Program.Main`:</span></span>

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

::: moniker-end

<span data-ttu-id="bcae8-115">Statik dosyalara, [Web köküne](xref:fundamentals/index#web-root)göre bir yol aracılığıyla erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="bcae8-115">Static files are accessible via a path relative to the [web root](xref:fundamentals/index#web-root).</span></span> <span data-ttu-id="bcae8-116">Örneğin, **Web uygulaması** proje şablonu *Wwwroot* klasörü içinde birkaç klasör içerir:</span><span class="sxs-lookup"><span data-stu-id="bcae8-116">For example, the **Web Application** project template contains several folders within the *wwwroot* folder:</span></span>

* <span data-ttu-id="bcae8-117">**Wwwroot**</span><span class="sxs-lookup"><span data-stu-id="bcae8-117">**wwwroot**</span></span>
  * <span data-ttu-id="bcae8-118">**Self**</span><span class="sxs-lookup"><span data-stu-id="bcae8-118">**css**</span></span>
  * <span data-ttu-id="bcae8-119">**yansımasını**</span><span class="sxs-lookup"><span data-stu-id="bcae8-119">**images**</span></span>
  * <span data-ttu-id="bcae8-120">**js**</span><span class="sxs-lookup"><span data-stu-id="bcae8-120">**js**</span></span>

<span data-ttu-id="bcae8-121">*Images* alt klasöründeki bir dosyaya erışmek için urı biçimi *http://\<server_address >/images/\<ımage_file_adı >* .</span><span class="sxs-lookup"><span data-stu-id="bcae8-121">The URI format to access a file in the *images* subfolder is *http://\<server_address>/images/\<image_file_name>*.</span></span> <span data-ttu-id="bcae8-122">Örneğin, *http://localhost:9189/images/banner3.svg* .</span><span class="sxs-lookup"><span data-stu-id="bcae8-122">For example, *http://localhost:9189/images/banner3.svg*.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bcae8-123">.NET Framework hedefliyorsanız, [Microsoft. AspNetCore. StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) paketini projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bcae8-123">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to the project.</span></span> <span data-ttu-id="bcae8-124">.NET Core hedefleniyorsa, [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) bu paketi içerir.</span><span class="sxs-lookup"><span data-stu-id="bcae8-124">If targeting .NET Core, the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) includes this package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="bcae8-125">.NET Framework hedefliyorsanız, [Microsoft. AspNetCore. StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) paketini projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bcae8-125">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to the project.</span></span> <span data-ttu-id="bcae8-126">.NET Core hedefleniyorsa, [Microsoft. AspNetCore. All metapackage](xref:fundamentals/metapackage) bu paketi içerir.</span><span class="sxs-lookup"><span data-stu-id="bcae8-126">If targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) includes this package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="bcae8-127">Projeye [Microsoft. AspNetCore. StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) paketini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bcae8-127">Add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to the project.</span></span>

::: moniker-end

<span data-ttu-id="bcae8-128">Statik dosyaları sunmaya izin veren [ara yazılımı](xref:fundamentals/middleware/index) yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="bcae8-128">Configure the [middleware](xref:fundamentals/middleware/index) which enables the serving of static files.</span></span>

### <a name="serve-files-inside-of-web-root"></a><span data-ttu-id="bcae8-129">Web kökünün içindeki dosyaları sunma</span><span class="sxs-lookup"><span data-stu-id="bcae8-129">Serve files inside of web root</span></span>

<span data-ttu-id="bcae8-130">@No__t-1 içinde [Usestaticfiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) metodunu çağırın:</span><span class="sxs-lookup"><span data-stu-id="bcae8-130">Invoke the [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method within `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

<span data-ttu-id="bcae8-131">Parametresiz `UseStaticFiles` yöntemi aşırı yüklemesi, [Web kökündeki](xref:fundamentals/index#web-root) dosyaları servable olarak işaretler.</span><span class="sxs-lookup"><span data-stu-id="bcae8-131">The parameterless `UseStaticFiles` method overload marks the files in [web root](xref:fundamentals/index#web-root) as servable.</span></span> <span data-ttu-id="bcae8-132">Aşağıdaki biçimlendirme *Wwwroot/Images/banner1. SVG*öğesine başvuruyor:</span><span class="sxs-lookup"><span data-stu-id="bcae8-132">The following markup references *wwwroot/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

<span data-ttu-id="bcae8-133">Yukarıdaki kodda, `~/` olan tilde karakteri [Web köküne](xref:fundamentals/index#web-root)işaret eder.</span><span class="sxs-lookup"><span data-stu-id="bcae8-133">In the preceding code, the tilde character `~/` points to the [web root](xref:fundamentals/index#web-root).</span></span>

### <a name="serve-files-outside-of-web-root"></a><span data-ttu-id="bcae8-134">Dosyaları Web kökünün dışında sunma</span><span class="sxs-lookup"><span data-stu-id="bcae8-134">Serve files outside of web root</span></span>

<span data-ttu-id="bcae8-135">Sunulacak statik dosyaların [Web kökünün](xref:fundamentals/index#web-root)dışında yer aldığı bir dizin hiyerarşisini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="bcae8-135">Consider a directory hierarchy in which the static files to be served reside outside of the [web root](xref:fundamentals/index#web-root):</span></span>

* <span data-ttu-id="bcae8-136">**Wwwroot**</span><span class="sxs-lookup"><span data-stu-id="bcae8-136">**wwwroot**</span></span>
  * <span data-ttu-id="bcae8-137">**Self**</span><span class="sxs-lookup"><span data-stu-id="bcae8-137">**css**</span></span>
  * <span data-ttu-id="bcae8-138">**yansımasını**</span><span class="sxs-lookup"><span data-stu-id="bcae8-138">**images**</span></span>
  * <span data-ttu-id="bcae8-139">**js**</span><span class="sxs-lookup"><span data-stu-id="bcae8-139">**js**</span></span>
* <span data-ttu-id="bcae8-140">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="bcae8-140">**MyStaticFiles**</span></span>
  * <span data-ttu-id="bcae8-141">**yansımasını**</span><span class="sxs-lookup"><span data-stu-id="bcae8-141">**images**</span></span>
    * <span data-ttu-id="bcae8-142">*banner1. SVG*</span><span class="sxs-lookup"><span data-stu-id="bcae8-142">*banner1.svg*</span></span>

<span data-ttu-id="bcae8-143">Bir istek statik dosya ara yazılımını aşağıdaki şekilde yapılandırarak *banner1. SVG* dosyasına erişebilir:</span><span class="sxs-lookup"><span data-stu-id="bcae8-143">A request can access the *banner1.svg* file by configuring the Static File Middleware as follows:</span></span>

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

<span data-ttu-id="bcae8-144">Yukarıdaki kodda, *mystaticfiles* dizin hiyerarşisi, *staticfiles* URI segmenti aracılığıyla herkese açıktır.</span><span class="sxs-lookup"><span data-stu-id="bcae8-144">In the preceding code, the *MyStaticFiles* directory hierarchy is exposed publicly via the *StaticFiles* URI segment.</span></span> <span data-ttu-id="bcae8-145">*Http://\<server_address >/StaticFiles/images/banner1.SVG* 'e yönelik bir istek *banner1. SVG* dosyasını hizmet eder.</span><span class="sxs-lookup"><span data-stu-id="bcae8-145">A request to *http://\<server_address>/StaticFiles/images/banner1.svg* serves the *banner1.svg* file.</span></span>

<span data-ttu-id="bcae8-146">Aşağıdaki biçimlendirme *Mystaticfiles/Images/banner1. SVG*' ye başvurur:</span><span class="sxs-lookup"><span data-stu-id="bcae8-146">The following markup references *MyStaticFiles/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a><span data-ttu-id="bcae8-147">HTTP yanıt üstbilgilerini ayarla</span><span class="sxs-lookup"><span data-stu-id="bcae8-147">Set HTTP response headers</span></span>

<span data-ttu-id="bcae8-148">[Staticfileoptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) NESNESI, http yanıt üst bilgilerini ayarlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bcae8-148">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) object can be used to set HTTP response headers.</span></span> <span data-ttu-id="bcae8-149">[Web kökünden](xref:fundamentals/index#web-root)statik dosya sunma yapılandırmasına ek olarak, aşağıdaki kod `Cache-Control` üst bilgisini ayarlar:</span><span class="sxs-lookup"><span data-stu-id="bcae8-149">In addition to configuring static file serving from the [web root](xref:fundamentals/index#web-root), the following code sets the `Cache-Control` header:</span></span>

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="bcae8-150">[Headerdictionaryextensions. Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) yöntemi, [Microsoft. Aspnetcore. http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paketinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="bcae8-150">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method exists in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

<span data-ttu-id="bcae8-151">Dosyalar, geliştirme ortamında 10 dakika (600 saniye) için genel olarak önbelleklenebilir hale getirilir:</span><span class="sxs-lookup"><span data-stu-id="bcae8-151">The files have been made publicly cacheable for 10 minutes (600 seconds) in the Development environment:</span></span>

![Cache-Control üst bilgisini gösteren yanıt üstbilgileri eklendi](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="bcae8-153">Statik dosya yetkilendirmesi</span><span class="sxs-lookup"><span data-stu-id="bcae8-153">Static file authorization</span></span>

<span data-ttu-id="bcae8-154">Statik dosya ara yazılımı yetkilendirme denetimleri sağlamıyor.</span><span class="sxs-lookup"><span data-stu-id="bcae8-154">The Static File Middleware doesn't provide authorization checks.</span></span> <span data-ttu-id="bcae8-155">*Wwwroot*altındakiler de dahil olmak üzere hizmet tarafından sunulan tüm dosyalar herkese açık olarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="bcae8-155">Any files served by it, including those under *wwwroot*, are publicly accessible.</span></span> <span data-ttu-id="bcae8-156">Dosyalara yetkilendirme temelinde hizmeti sağlamak için:</span><span class="sxs-lookup"><span data-stu-id="bcae8-156">To serve files based on authorization:</span></span>

* <span data-ttu-id="bcae8-157">Onları *Wwwroot* dışında ve statik dosya ara yazılımı tarafından erişilebilen herhangi bir dizinle saklayın.</span><span class="sxs-lookup"><span data-stu-id="bcae8-157">Store them outside of *wwwroot* and any directory accessible to the Static File Middleware.</span></span>
* <span data-ttu-id="bcae8-158">Yetkilendirmeyi uygulanan bir eylem yöntemi aracılığıyla onlara sunar.</span><span class="sxs-lookup"><span data-stu-id="bcae8-158">Serve them via an action method to which authorization is applied.</span></span> <span data-ttu-id="bcae8-159">Bir [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) nesnesi döndürür:</span><span class="sxs-lookup"><span data-stu-id="bcae8-159">Return a [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) object:</span></span>

  [!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a><span data-ttu-id="bcae8-160">Dizin taramayı etkinleştir</span><span class="sxs-lookup"><span data-stu-id="bcae8-160">Enable directory browsing</span></span>

<span data-ttu-id="bcae8-161">Dizin tarama, Web uygulamanızın kullanıcılarına belirtilen bir dizin içindeki bir dizin listesini ve dosyalarını görmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="bcae8-161">Directory browsing allows users of your web app to see a directory listing and files within a specified directory.</span></span> <span data-ttu-id="bcae8-162">Dizin tarama, güvenlik nedenleriyle varsayılan olarak devre dışıdır (bkz. [hususlar](#considerations)).</span><span class="sxs-lookup"><span data-stu-id="bcae8-162">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="bcae8-163">@No__t-1 ' de [Usedirectorybrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) metodunu çağırarak dizin taramayı etkinleştirin:</span><span class="sxs-lookup"><span data-stu-id="bcae8-163">Enable directory browsing by invoking the [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) method in `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

<span data-ttu-id="bcae8-164">@No__t-1 ' den [Adddirectorybrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) yöntemini çağırarak gerekli hizmetleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="bcae8-164">Add required services by invoking the [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

<span data-ttu-id="bcae8-165">Yukarıdaki kod, her bir dosya ve klasörün bağlantılarıyla birlikte *http://\<server_address >/myImages*URL 'sini kullanarak *Wwwroot/görüntüler* klasöründe Dizin taramasına izin verir:</span><span class="sxs-lookup"><span data-stu-id="bcae8-165">The preceding code allows directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*, with links to each file and folder:</span></span>

![Dizin tarama](static-files/_static/dir-browse.png)

<span data-ttu-id="bcae8-167">Göz atmayı etkinleştirirken güvenlik riskleri hakkındaki [noktalara](#considerations) göz atın.</span><span class="sxs-lookup"><span data-stu-id="bcae8-167">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="bcae8-168">Aşağıdaki örnekteki iki `UseStaticFiles` çağrısı olduğunu aklınızda edin.</span><span class="sxs-lookup"><span data-stu-id="bcae8-168">Note the two `UseStaticFiles` calls in the following example.</span></span> <span data-ttu-id="bcae8-169">İlk çağrı *Wwwroot* klasöründeki statik dosyaları sunmaya izin veriyor.</span><span class="sxs-lookup"><span data-stu-id="bcae8-169">The first call enables the serving of static files in the *wwwroot* folder.</span></span> <span data-ttu-id="bcae8-170">İkinci çağrı, *http://\<server_address >/myImages*URL 'sini kullanarak *Wwwroot/görüntüler* klasöründe dizin taramayı mümkün bir şekilde sunar:</span><span class="sxs-lookup"><span data-stu-id="bcae8-170">The second call enables directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a><span data-ttu-id="bcae8-171">Varsayılan bir belge sunar</span><span class="sxs-lookup"><span data-stu-id="bcae8-171">Serve a default document</span></span>

<span data-ttu-id="bcae8-172">Varsayılan ana sayfanın ayarlanması, ziyaretçi sitenizi ziyaret ederken mantıksal bir başlangıç noktası sağlar.</span><span class="sxs-lookup"><span data-stu-id="bcae8-172">Setting a default home page provides visitors a logical starting point when visiting your site.</span></span> <span data-ttu-id="bcae8-173">Kullanıcı URI 'yi tamamen nitelemeden varsayılan bir sayfaya hizmeti sağlamak için `Startup.Configure` ' den [Usedefaultfiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) metodunu çağırın:</span><span class="sxs-lookup"><span data-stu-id="bcae8-173">To serve a default page without the user fully qualifying the URI, call the [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method from `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> <span data-ttu-id="bcae8-174">Varsayılan dosyaya hizmeti sağlamak için `UseDefaultFiles` ' dan önce `UseStaticFiles` çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bcae8-174">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="bcae8-175">`UseDefaultFiles`, dosyayı gerçekten sunan bir URL yeniden yazar.</span><span class="sxs-lookup"><span data-stu-id="bcae8-175">`UseDefaultFiles` is a URL rewriter that doesn't actually serve the file.</span></span> <span data-ttu-id="bcae8-176">Dosyayı karşılamak için `UseStaticFiles` aracılığıyla statik dosya ara yazılımı etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="bcae8-176">Enable Static File Middleware via `UseStaticFiles` to serve the file.</span></span>

<span data-ttu-id="bcae8-177">@No__t-0 ile, bir klasör arama istekleri:</span><span class="sxs-lookup"><span data-stu-id="bcae8-177">With `UseDefaultFiles`, requests to a folder search for:</span></span>

* <span data-ttu-id="bcae8-178">*default. htm*</span><span class="sxs-lookup"><span data-stu-id="bcae8-178">*default.htm*</span></span>
* <span data-ttu-id="bcae8-179">*default. html*</span><span class="sxs-lookup"><span data-stu-id="bcae8-179">*default.html*</span></span>
* <span data-ttu-id="bcae8-180">*index. htm*</span><span class="sxs-lookup"><span data-stu-id="bcae8-180">*index.htm*</span></span>
* <span data-ttu-id="bcae8-181">*index. html*</span><span class="sxs-lookup"><span data-stu-id="bcae8-181">*index.html*</span></span>

<span data-ttu-id="bcae8-182">Listedeki ilk dosya, istek tam URI olmasına rağmen olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="bcae8-182">The first file found from the list is served as though the request were the fully qualified URI.</span></span> <span data-ttu-id="bcae8-183">Tarayıcı URL 'SI, istenen URI 'yi yansıtacak şekilde devam ediyor.</span><span class="sxs-lookup"><span data-stu-id="bcae8-183">The browser URL continues to reflect the URI requested.</span></span>

<span data-ttu-id="bcae8-184">Aşağıdaki kod varsayılan dosya adını *mydefault. html*olarak değiştirir:</span><span class="sxs-lookup"><span data-stu-id="bcae8-184">The following code changes the default file name to *mydefault.html*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a><span data-ttu-id="bcae8-185">Usedosya sunucusu</span><span class="sxs-lookup"><span data-stu-id="bcae8-185">UseFileServer</span></span>

<span data-ttu-id="bcae8-186">[Usedosyasunucusu](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) , `UseStaticFiles`, `UseDefaultFiles` ve `UseDirectoryBrowser` işlevlerini birleştirir.</span><span class="sxs-lookup"><span data-stu-id="bcae8-186">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="bcae8-187">Aşağıdaki kod, statik dosyaların ve varsayılan dosyanın kullanılmasına izin veriyor.</span><span class="sxs-lookup"><span data-stu-id="bcae8-187">The following code enables the serving of static files and the default file.</span></span> <span data-ttu-id="bcae8-188">Dizin tarama etkin değil.</span><span class="sxs-lookup"><span data-stu-id="bcae8-188">Directory browsing isn't enabled.</span></span>

```csharp
app.UseFileServer();
```

<span data-ttu-id="bcae8-189">Aşağıdaki kod, dizin taramayı etkinleştirerek Parametresiz aşırı yüklemeden sonra oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="bcae8-189">The following code builds upon the parameterless overload by enabling directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

<span data-ttu-id="bcae8-190">Aşağıdaki dizin hiyerarşisini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="bcae8-190">Consider the following directory hierarchy:</span></span>

* <span data-ttu-id="bcae8-191">**Wwwroot**</span><span class="sxs-lookup"><span data-stu-id="bcae8-191">**wwwroot**</span></span>
  * <span data-ttu-id="bcae8-192">**Self**</span><span class="sxs-lookup"><span data-stu-id="bcae8-192">**css**</span></span>
  * <span data-ttu-id="bcae8-193">**yansımasını**</span><span class="sxs-lookup"><span data-stu-id="bcae8-193">**images**</span></span>
  * <span data-ttu-id="bcae8-194">**js**</span><span class="sxs-lookup"><span data-stu-id="bcae8-194">**js**</span></span>
* <span data-ttu-id="bcae8-195">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="bcae8-195">**MyStaticFiles**</span></span>
  * <span data-ttu-id="bcae8-196">**yansımasını**</span><span class="sxs-lookup"><span data-stu-id="bcae8-196">**images**</span></span>
    * <span data-ttu-id="bcae8-197">*banner1. SVG*</span><span class="sxs-lookup"><span data-stu-id="bcae8-197">*banner1.svg*</span></span>
  * <span data-ttu-id="bcae8-198">*default. html*</span><span class="sxs-lookup"><span data-stu-id="bcae8-198">*default.html*</span></span>

<span data-ttu-id="bcae8-199">Aşağıdaki kod, statik dosyaları, varsayılan dosyaları ve dizin taramayı `MyStaticFiles` ' a sunar:</span><span class="sxs-lookup"><span data-stu-id="bcae8-199">The following code enables static files, default files, and directory browsing of `MyStaticFiles`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

<span data-ttu-id="bcae8-200">`EnableDirectoryBrowsing` Özellik değeri `true` olduğunda `AddDirectoryBrowser` çağrılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="bcae8-200">`AddDirectoryBrowser` must be called when the `EnableDirectoryBrowsing` property value is `true`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

<span data-ttu-id="bcae8-201">Dosya hiyerarşisini ve önceki kodu kullanarak, URL 'Ler aşağıdaki şekilde çözümlenir:</span><span class="sxs-lookup"><span data-stu-id="bcae8-201">Using the file hierarchy and preceding code, URLs resolve as follows:</span></span>

| <span data-ttu-id="bcae8-202">URI</span><span class="sxs-lookup"><span data-stu-id="bcae8-202">URI</span></span>            |                             <span data-ttu-id="bcae8-203">Yanıt</span><span class="sxs-lookup"><span data-stu-id="bcae8-203">Response</span></span>  |
| ------- | ------|
| <span data-ttu-id="bcae8-204">*http://\<sunucu_adresi >/StaticFiles/images/banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="bcae8-204">*http://\<server_address>/StaticFiles/images/banner1.svg*</span></span>    |      <span data-ttu-id="bcae8-205">MyStaticFiles/Images/banner1. SVG</span><span class="sxs-lookup"><span data-stu-id="bcae8-205">MyStaticFiles/images/banner1.svg</span></span> |
| <span data-ttu-id="bcae8-206">*http://\<sunucu_adresi >/StaticFiles*</span><span class="sxs-lookup"><span data-stu-id="bcae8-206">*http://\<server_address>/StaticFiles*</span></span>             |     <span data-ttu-id="bcae8-207">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="bcae8-207">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="bcae8-208">*Mystaticfiles* dizininde varsayılan adlı dosya yoksa, *http://\<server_address >/staticfiles* , tıklatılabilir bağlantılarla dizin listesini döndürür:</span><span class="sxs-lookup"><span data-stu-id="bcae8-208">If no default-named file exists in the *MyStaticFiles* directory, *http://\<server_address>/StaticFiles* returns the directory listing with clickable links:</span></span>

![Statik dosyalar listesi](static-files/_static/db2.png)

> [!NOTE]
> <span data-ttu-id="bcae8-210"><xref:Microsoft.AspNetCore.Builder.DefaultFilesExtensions.UseDefaultFiles*> ve <xref:Microsoft.AspNetCore.Builder.DirectoryBrowserExtensions.UseDirectoryBrowser*> `http://{SERVER ADDRESS}/StaticFiles` ' den (sondaki eğik çizgi olmadan) `http://{SERVER ADDRESS}/StaticFiles/` ' e (sondaki eğik çizgiyle) istemci tarafı yönlendirmesi gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="bcae8-210"><xref:Microsoft.AspNetCore.Builder.DefaultFilesExtensions.UseDefaultFiles*> and <xref:Microsoft.AspNetCore.Builder.DirectoryBrowserExtensions.UseDirectoryBrowser*> perform a client-side redirect from `http://{SERVER ADDRESS}/StaticFiles` (without a trailing slash) to `http://{SERVER ADDRESS}/StaticFiles/` (with a trailing slash).</span></span> <span data-ttu-id="bcae8-211">*Staticfiles* dizinindeki göreli URL 'ler, sondaki eğik çizgi olmadan geçersizdir.</span><span class="sxs-lookup"><span data-stu-id="bcae8-211">Relative URLs within the *StaticFiles* directory are invalid without a trailing slash.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="bcae8-212">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="bcae8-212">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="bcae8-213">[Fileextensioncontenttypeprovider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) sınıfı, MIME içerik türlerine dosya uzantılarının eşlemesi olarak hizmet veren bir `Mappings` özelliği içerir.</span><span class="sxs-lookup"><span data-stu-id="bcae8-213">The [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) class contains a `Mappings` property serving as a mapping of file extensions to MIME content types.</span></span> <span data-ttu-id="bcae8-214">Aşağıdaki örnekte, bazı dosya uzantıları bilinen MIME türlerine kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="bcae8-214">In the following sample, several file extensions are registered to known MIME types.</span></span> <span data-ttu-id="bcae8-215">*. Rtf* uzantısı değiştirilmiştir ve *. mp4* kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="bcae8-215">The *.rtf* extension is replaced, and *.mp4* is removed.</span></span>

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

<span data-ttu-id="bcae8-216">Bkz. [MIME içerik türleri](https://www.iana.org/assignments/media-types/media-types.xhtml).</span><span class="sxs-lookup"><span data-stu-id="bcae8-216">See [MIME content types](https://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="bcae8-217">Standart olmayan içerik türleri</span><span class="sxs-lookup"><span data-stu-id="bcae8-217">Non-standard content types</span></span>

<span data-ttu-id="bcae8-218">Statik dosya ara yazılımı, neredeyse 400 bilinen dosya içerik türlerini anlamıştır.</span><span class="sxs-lookup"><span data-stu-id="bcae8-218">Static File Middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="bcae8-219">Kullanıcı bilinmeyen bir dosya türüne sahip bir dosya isterse, statik dosya ara yazılımı isteği ardışık düzendeki bir sonraki ara yazılıma geçirir.</span><span class="sxs-lookup"><span data-stu-id="bcae8-219">If the user requests a file with an unknown file type, Static File Middleware passes the request to the next middleware in the pipeline.</span></span> <span data-ttu-id="bcae8-220">Bir ara yazılım, isteği işlediğinde, bir *404 bulunamadı* yanıtı döndürülür.</span><span class="sxs-lookup"><span data-stu-id="bcae8-220">If no middleware handles the request, a *404 Not Found* response is returned.</span></span> <span data-ttu-id="bcae8-221">Dizin tarama etkinse, bir dizin listesinde dosyaya bir bağlantı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="bcae8-221">If directory browsing is enabled, a link to the file is displayed in a directory listing.</span></span>

<span data-ttu-id="bcae8-222">Aşağıdaki kod, bilinmeyen türlere hizmet olarak bilinmeyen türler sunar ve bilinmeyen dosyayı görüntü olarak işler:</span><span class="sxs-lookup"><span data-stu-id="bcae8-222">The following code enables serving unknown types and renders the unknown file as an image:</span></span>

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="bcae8-223">Yukarıdaki kodla, bilinmeyen içerik türüne sahip bir dosya isteği görüntü olarak döndürülür.</span><span class="sxs-lookup"><span data-stu-id="bcae8-223">With the preceding code, a request for a file with an unknown content type is returned as an image.</span></span>

> [!WARNING]
> <span data-ttu-id="bcae8-224">[ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) etkinleştirme bir güvenlik riskidir.</span><span class="sxs-lookup"><span data-stu-id="bcae8-224">Enabling [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) is a security risk.</span></span> <span data-ttu-id="bcae8-225">Varsayılan olarak devre dışıdır ve kullanımı önerilmez.</span><span class="sxs-lookup"><span data-stu-id="bcae8-225">It's disabled by default, and its use is discouraged.</span></span> <span data-ttu-id="bcae8-226">[Fileextensioncontenttypeprovider](#fileextensioncontenttypeprovider) standart olmayan uzantılara sahip dosyalara hizmet vermeye yönelik daha güvenli bir alternatif sağlar.</span><span class="sxs-lookup"><span data-stu-id="bcae8-226">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="bcae8-227">Dikkat edilmesi gerekenler</span><span class="sxs-lookup"><span data-stu-id="bcae8-227">Considerations</span></span>

> [!WARNING]
> <span data-ttu-id="bcae8-228">`UseDirectoryBrowser` ve `UseStaticFiles`, gizli dizileri sızdırabilir.</span><span class="sxs-lookup"><span data-stu-id="bcae8-228">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="bcae8-229">Üretimde dizin taramayı devre dışı bırakmak önemle önerilir.</span><span class="sxs-lookup"><span data-stu-id="bcae8-229">Disabling directory browsing in production is highly recommended.</span></span> <span data-ttu-id="bcae8-230">@No__t-0 veya `UseDirectoryBrowser` aracılığıyla hangi dizinlerin etkinleştirildiğini dikkatle gözden geçirin.</span><span class="sxs-lookup"><span data-stu-id="bcae8-230">Carefully review which directories are enabled via `UseStaticFiles` or `UseDirectoryBrowser`.</span></span> <span data-ttu-id="bcae8-231">Tüm dizin ve alt dizinleri herkese açık şekilde erişilebilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="bcae8-231">The entire directory and its sub-directories become publicly accessible.</span></span> <span data-ttu-id="bcae8-232">*@No__t-1content_root >/Wwwroot*gibi özel bir dizinde herkese sunma için uygun dosyaları depolayın.</span><span class="sxs-lookup"><span data-stu-id="bcae8-232">Store files suitable for serving to the public in a dedicated directory, such as *\<content_root>/wwwroot*.</span></span> <span data-ttu-id="bcae8-233">Bu dosyaları MVC görünümlerinden ayırın, Razor Pages (yalnızca 2. x), yapılandırma dosyaları vb.</span><span class="sxs-lookup"><span data-stu-id="bcae8-233">Separate these files from MVC views, Razor Pages (2.x only), configuration files, etc.</span></span>

* <span data-ttu-id="bcae8-234">@No__t-0 ve `UseStaticFiles` ' i içeren içerik URL 'Leri, temel dosya sisteminin büyük/küçük harfe duyarlılık ve karakter kısıtlamalarına tabidir.</span><span class="sxs-lookup"><span data-stu-id="bcae8-234">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of the underlying file system.</span></span> <span data-ttu-id="bcae8-235">Örneğin, Windows büyük/küçük harfe duyarsız @ no__t-0macOS ve Linux değildir.</span><span class="sxs-lookup"><span data-stu-id="bcae8-235">For example, Windows is case insensitive&mdash;macOS and Linux aren't.</span></span>

* <span data-ttu-id="bcae8-236">IIS 'de barındırılan ASP.NET Core uygulamalar, statik dosya istekleri de dahil olmak üzere tüm istekleri uygulamaya iletmek için [ASP.NET Core modülünü](xref:host-and-deploy/aspnet-core-module) kullanır.</span><span class="sxs-lookup"><span data-stu-id="bcae8-236">ASP.NET Core apps hosted in IIS use the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to forward all requests to the app, including static file requests.</span></span> <span data-ttu-id="bcae8-237">IIS statik dosya işleyicisi kullanılmıyor.</span><span class="sxs-lookup"><span data-stu-id="bcae8-237">The IIS static file handler isn't used.</span></span> <span data-ttu-id="bcae8-238">Modül tarafından işlenmek üzere istekleri işleme şansı yoktur.</span><span class="sxs-lookup"><span data-stu-id="bcae8-238">It has no chance to handle requests before they're handled by the module.</span></span>

* <span data-ttu-id="bcae8-239">Sunucu veya Web sitesi düzeyinde IIS statik dosya işleyicisini kaldırmak için IIS Yöneticisi ' nde aşağıdaki adımları uygulayın:</span><span class="sxs-lookup"><span data-stu-id="bcae8-239">Complete the following steps in IIS Manager to remove the IIS static file handler at the server or website level:</span></span>
    1. <span data-ttu-id="bcae8-240">**Modüller** özelliğine gidin.</span><span class="sxs-lookup"><span data-stu-id="bcae8-240">Navigate to the **Modules** feature.</span></span>
    1. <span data-ttu-id="bcae8-241">Listeden **StaticFileModule ' ü** seçin.</span><span class="sxs-lookup"><span data-stu-id="bcae8-241">Select **StaticFileModule** in the list.</span></span>
    1. <span data-ttu-id="bcae8-242">**Eylemler** kenar çubuğunda **Kaldır** ' a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="bcae8-242">Click **Remove** in the **Actions** sidebar.</span></span>

> [!WARNING]
> <span data-ttu-id="bcae8-243">IIS statik dosya işleyicisi etkinse **ve** ASP.NET Core modülü yanlış yapılandırılmışsa, statik dosyalar sunulur.</span><span class="sxs-lookup"><span data-stu-id="bcae8-243">If the IIS static file handler is enabled **and** the ASP.NET Core Module is configured incorrectly, static files are served.</span></span> <span data-ttu-id="bcae8-244">Bu, örneğin, *Web. config* dosyası dağıtılmamışsa oluşur.</span><span class="sxs-lookup"><span data-stu-id="bcae8-244">This happens, for example, if the *web.config* file isn't deployed.</span></span>

* <span data-ttu-id="bcae8-245">Kod dosyalarını ( *. cs* ve *. cshtml*dahil) uygulama projesinin [Web kökünün](xref:fundamentals/index#web-root)dışına yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="bcae8-245">Place code files (including *.cs* and *.cshtml*) outside of the app project's [web root](xref:fundamentals/index#web-root).</span></span> <span data-ttu-id="bcae8-246">Bu nedenle, uygulamanın istemci tarafı içeriği ile sunucu tabanlı kod arasında bir mantıksal ayrım oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="bcae8-246">A logical separation is therefore created between the app's client-side content and server-based code.</span></span> <span data-ttu-id="bcae8-247">Bu, sunucu tarafı kodun sızmasını önler.</span><span class="sxs-lookup"><span data-stu-id="bcae8-247">This prevents server-side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bcae8-248">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="bcae8-248">Additional resources</span></span>

* [<span data-ttu-id="bcae8-249">Ara Yazılım</span><span class="sxs-lookup"><span data-stu-id="bcae8-249">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="bcae8-250">ASP.NET Core'a giriş</span><span class="sxs-lookup"><span data-stu-id="bcae8-250">Introduction to ASP.NET Core</span></span>](xref:index)
