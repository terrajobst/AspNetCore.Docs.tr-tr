---
title: ASP.NET core'da statik dosyalar
author: rick-anderson
description: Hizmet ve statik dosyaların güvenliğini sağlamak ve ASP.NET Core web uygulaması ara yazılım davranışları barındırma statik dosya yapılandırma hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
uid: fundamentals/static-files
ms.openlocfilehash: 52c7916b9fc55c875d56acd49c01f76dd2053817
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47861011"
---
# <a name="static-files-in-aspnet-core"></a>ASP.NET core'da statik dosyalar

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Scott Addie](https://twitter.com/Scott_Addie)

Statik dosyaları, HTML, CSS, görüntü ve JavaScript gibi ASP.NET Core uygulaması doğrudan istemcilere hizmet varlıklardır. Bazı yapılandırmalar için bu dosyaların sunma etkinleştirmek için gereklidir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="serve-static-files"></a>Statik dosyaları işleme

Statik dosyaları, projenizin web kök dizininde depolanır. Varsayılan dizin,  *\<content_root > / wwwroot*, ancak aracılığıyla değiştirilebilir [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) yöntemi. Bkz: [içerik kök](xref:fundamentals/index#content-root) ve [Web kök](xref:fundamentals/index#web-root) daha fazla bilgi için.

Uygulamanın web ana bilgisayarı, içerik kök dizini kullanan yapılmalıdır.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

`WebHost.CreateDefaultBuilder` Yöntemini, geçerli dizine içerik kök ayarlar:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

İçerik kök çağırarak geçerli dizine ayarlayın [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) içine `Program.Main`:

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

Statik dosyalar, web kökü göreli bir yol aracılığıyla erişilebilir. Örneğin, **Web uygulaması** proje şablonu içeren birkaç klasörler *wwwroot* klasörü:

* **wwwroot**
  * **CSS**
  * **Görüntüleri**
  * **js**

Bir dosyaya erişmek için URI biçimi *görüntüleri* alt *http://\<server_address > /images/\<image_file_name >*. Örneğin, *http://localhost:9189/images/banner3.svg*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

.NET Framework'ü hedefleyen, ekleme [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) paketini projenize. .NET Core'u hedefleyen, [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) bu paketi içerir.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Ekleme [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) paketini projenize.

---

Yapılandırma [ara yazılım](xref:fundamentals/middleware/index) statik dosyaları sunma sağlar.

### <a name="serve-files-inside-of-web-root"></a>İçinde web kök dosyaları işleme

Çağırma [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) yöntemi içinde `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

Parametresiz `UseStaticFiles` yöntemi aşırı yüklemesini dosyaları web kök servable olarak işaretler. Aşağıdaki biçimlendirme başvuruları *wwwroot/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a>Web kökünün dışında dosyaları işleme

Statik dosyaların sunulmasını web kökünün dışında bulunduğu dizin sıradüzeni göz önünde bulundurun:

* **wwwroot**
  * **CSS**
  * **Görüntüleri**
  * **js**
* **MyStaticFiles**
  * **Görüntüleri**
      * *banner1.SVG*

Bir isteği erişip *banner1.svg* statik dosya ara yazılımlarını şu şekilde yapılandırarak dosyası:

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

Önceki kodda, *MyStaticFiles* dizin hiyerarşisi sunulmuştur herkese açık şekilde aracılığıyla *StaticFiles* URI segmenti. Bir istek *http://\<server_address > /StaticFiles/images/banner1.svg* hizmet *banner1.svg* dosya.

Aşağıdaki biçimlendirme başvuruları *MyStaticFiles/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a>HTTP yanıt üstbilgilerini Ayarla

A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) nesne, HTTP yanıt üst bilgilerini ayarlayacak şekilde kullanılabilir. Statik dosya sunucusu web kök yapılandırmaya ek olarak, aşağıdaki kod kümeleri `Cache-Control` üst bilgi:

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) yöntem bulunmaktadır [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paket.

Dosyalarını geliştirme ortamında 10 dakika (600 saniye) halka açık bir şekilde önbelleğe yapılmıştır:

![Cache-Control üst bilgisi gösteren yanıt üst bilgileri eklendi](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>Statik dosya yetkilendirmesi

Statik dosya ara yazılımlarını yetkilendirme denetimleri sağlamaz. Tüm dosyaları sunulan işlem tarafından altında dahil olmak üzere *wwwroot*, genel olarak erişilebilir. Kimlik tabanlı dosyaları işleme için:

* Bunları dışında Store *wwwroot* ve statik dosya ara yazılımlarını için erişilebilir olan herhangi bir dizinini **ve**
* Bunları, yetkilendirme uygulandığı bir eylem yöntemi işlevi görür. Döndürür bir [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) nesnesi:

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a>Dizin taramayı etkinleştir

Dizin tarama dizin listesini görmek, kullanıcıların web uygulamanızın sağlar ve belirtilen bir dizin içinde dosyaları. Dizin tarama varsayılan olarak devre dışıdır güvenlik nedenleriyle (bkz [konuları](#considerations)). Etkin Dizin çağırarak gözatma [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) yönteminde `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

Gerekli hizmetler çağırarak ekleme [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) yönteminden `Startup.ConfigureServices`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

Yukarıdaki kod, dizin tarama sağlayan *wwwroot/görüntülerinden* URL'yi kullanarak klasör *http://\<server_address > / Myımages*, her dosya ve klasör için bağlantılarla birlikte:

![Dizin tarama](static-files/_static/dir-browse.png)

Bkz: [konuları](#considerations) gözatma etkinleştirilirken güvenlik risklerini üzerinde.

İki Not `UseStaticFiles` aşağıdaki örnekte çağırır. İlk çağrıda statik dosyaları sunma sağlayan *wwwroot* klasör. İkinci çağrı, dizin taramayı etkinleştirir *wwwroot/görüntülerinden* URL'yi kullanarak klasör *http://\<server_address > / Myımages*:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a>Hizmet bir varsayılan belge

Varsayılan giriş sayfası ayarı ziyaretçiler mantıksal bir başlangıç noktası sitenizi ziyaret eden sağlar. Varsayılan sayfa tam URI uygun olmayan kullanıcı hizmet vermek için çağrı [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) yönteminden `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> `UseDefaultFiles` önce çağrılmalıdır `UseStaticFiles` varsayılan dosya yapacak. `UseDefaultFiles` dosyanın gerçekten görmese bir URL rewriter olur. Aracılığıyla statik dosya ara yazılımlarını etkinleştir `UseStaticFiles` dosya yapacak.

İle `UseDefaultFiles`, istekleri için bir klasörü arayın:

* *default.htm*
* *default.HTML*
* *index.htm*
* *index.HTML*

İlk listede bulunan dosya istek gibi davranarak tam uygun URI sunulur. Tarayıcı URL, istenen URI'yi yansıtacak şekilde devam eder.

Varsayılan dosya adına aşağıdaki kod değişiklikleri *mydefault.html*:

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a>UseFileServer

[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) işlevlerini birleştiren `UseStaticFiles`, `UseDefaultFiles`, ve `UseDirectoryBrowser`.

Aşağıdaki kod, statik dosyalar ve varsayılan dosya sunma etkinleştirir. Dizin tarama etkin değil.

```csharp
app.UseFileServer();
```

Aşağıdaki kod, dizin tarama etkinleştirerek parametresiz aşırı yüklemesi sırasında oluşturur:

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

Aşağıdaki dizin hiyerarşi göz önünde bulundurun:

* **wwwroot**
  * **CSS**
  * **Görüntüleri**
  * **js**
* **MyStaticFiles**
  * **Görüntüleri**
      * *banner1.SVG*
  * *default.HTML*

Aşağıdaki kod, statik dosyalar, varsayılan dosya ve Dizin tarama etkinleştirir `MyStaticFiles`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

`AddDirectoryBrowser` ne zaman çağrılmalıdır `EnableDirectoryBrowsing` özellik değeri `true`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

İse dosya hiyerarşisini kullanarak ve önceki kod, URL şu şekilde çözün:

| URI            |                             Yanıt  |
| ------- | ------|
| *http://\<server_address > /StaticFiles/images/banner1.svg*    |      MyStaticFiles/images/banner1.svg |
| *http://\<server_address > / StaticFiles*             |     MyStaticFiles/default.html |

Varsayılan adlı dosya varsa *MyStaticFiles* dizin *http://\<server_address > / StaticFiles* dizin ile tıklanabilir bağlantılar listesi döndürür:

![Statik dosyaları listesi](static-files/_static/db2.png)

> [!NOTE]
> `UseDefaultFiles` ve `UseDirectoryBrowser` URL'sini kullanarak *http://\<server_address > / StaticFiles* istemci tarafı tetiklemek için sondaki eğik çizgi yeniden yönlendirme *http://\<server_address > / StaticFiles /*. Eğik eklenmesini dikkat edin. Belgelerde göreli URL'ler, sonunda bir eğik çizgi geçersiz sayılan.

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

[FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) sınıfı içeren bir `Mappings` MIME içerik türleri için dosya uzantıları eşlemesi olarak hizmet veren özellik. Aşağıdaki örnekte, çeşitli dosya uzantıları bilinen MIME türleri için kaydedilir. *.Rtf* uzantısı değiştirilir ve *.mp4* kaldırılır.

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

Bkz: [MIME içerik türleri](http://www.iana.org/assignments/media-types/media-types.xhtml).

## <a name="non-standard-content-types"></a>Standart olmayan içerik türleri

Statik dosya ara yazılımlarını hemen 400 bilinen dosya içerik türlerinin bilincindedir. Kullanıcı bir bilinmeyen dosya türü ile bir dosya isterse, statik dosya ara yazılımlarını istek ardışık düzende sonraki ara yazılımı geçirir. Hiçbir ara yazılım istek işliyorsa bir *404 Bulunamadı* yanıt döndürülür. Dizin tarama etkin olduğunda, dosyanın bir bağlantısını bir dizin listesinde görüntülenir.

Aşağıdaki kod, bilinmeyen tür tanıtıcısıyla sağlar ve bilinmeyen dosya bir görüntü olarak işler:

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

Yukarıdaki kod ile bir isteği bir dosyayla bilinmeyen bir içerik türü için bir görüntü olarak döndürülür.

> [!WARNING]
> Etkinleştirme [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) bir güvenlik riski oluşturur. Varsayılan olarak devre dışıdır ve kullanımı önerilmez. [FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) standart uzantılı dosyaları sunma için güvenli bir alternatif sunar.

### <a name="considerations"></a>Dikkat Edilecekler

> [!WARNING]
> `UseDirectoryBrowser` ve `UseStaticFiles` gizli dizileri sızabilir. Devre dışı bırakma dizin üretimde tarama önemle tavsiye edilir. Hangi dizinler aracılığıyla etkinleştirilir dikkatle inceleyin `UseStaticFiles` veya `UseDirectoryBrowser`. Tüm dizin ve alt dizinlerinin genel olarak erişilebilir duruma gelir. Store dosyaları genel kullanımı için uygun bir adanmış dizinde gibi  *\<content_root > / wwwroot*. Bu dosyalar, MVC görünümleri, Razor sayfaları (yalnızca 2.x), yapılandırma dosyaları, vb. ayırın.

* İle kullanıma sunulan içerik için URL'leri `UseDirectoryBrowser` ve `UseStaticFiles` büyük küçük harf duyarlılığı ve temel alınan dosya sisteminin karakter kısıtlamaları tabidir. Örneğin, Windows büyük küçük harfe duyarlı&mdash;macOS ve Linux değildir.

* IIS kullanımda barındırılan ASP.NET Core uygulamaları [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) tüm statik dosya istekleri dahil olmak üzere uygulama isteklerini iletmek için. IIS statik dosya işleyicisi kullanılmaz. Modül tarafından işlenen önce isteklerini işlemek için bir şans var.

* Sunucu veya Web sitesi düzeyinde IIS statik dosya işleyicisi kaldırmak için IIS Yöneticisi'nde aşağıdaki adımları tamamlayın:
    1. Gidin **modülleri** özelliği.
    1. Seçin **StaticFileModule** listesinde.
    1. Tıklayın **Kaldır** içinde **eylemleri** kenar çubuğu.

> [!WARNING]
> IIS statik dosya işleyicisi etkinse **ve** statik dosyalar sunulan, ASP.NET Core modülü yanlış yapılandırılmış. Bu, örneğin, olur *web.config* değil dosya dağıtılır.

* Kod dosyaları yerleştirmek (dahil olmak üzere *.cs* ve *.cshtml*) uygulama projenin web kökünün dışında. Mantıksal ayrılığı, bu nedenle uygulamanın istemci-tarafı içerik ve sunucu tabanlı kod arasında oluşturulur. Bu, sunucu tarafı kodu sızmasını engeller.

## <a name="additional-resources"></a>Ek kaynaklar

* [Ara Yazılım](xref:fundamentals/middleware/index)
* [ASP.NET Core'a giriş](xref:index)
