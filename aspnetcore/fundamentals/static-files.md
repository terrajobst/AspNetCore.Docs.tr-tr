---
title: ASP.NET Core statik dosyalarıyla çalışma
author: rick-anderson
description: Hizmet ve statik dosyalar güvenli ve statik dosya ara yazılımı davranışları bir ASP.NET Core web uygulamasında barındırma yapılandırma hakkında bilgi edinin.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: 46e868910661024ea3b950e78ced02a095896be1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="work-with-static-files-in-aspnet-core"></a>ASP.NET Core statik dosyalarıyla çalışma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Scott Addie](https://twitter.com/Scott_Addie)

HTML, CSS, görüntüler ve JavaScript gibi statik dosyaları ASP.NET Core uygulama doğrudan istemcilere hizmet varlıkları içerir. Bazı yapılandırmalar için bu dosyaların sunma etkinleştirmek için gereklidir.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="serve-static-files"></a>Statik dosyaları işleme

Statik dosyaları projenizin web kök dizininde depolanır. Varsayılan dizin  *\<content_root > / wwwroot*, ancak aracılığıyla değiştirilebilir [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) yöntemi. Bkz: [içerik kök](xref:fundamentals/index#content-root) ve [Web kök](xref:fundamentals/index#web-root) daha fazla bilgi için.

Uygulamanızın web ana içerik kök dizininin haberdar olmanız gerekir.

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
`WebHost.CreateDefaultBuilder` Yöntemi geçerli dizine içerik kök ayarlar:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
Çağırarak set geçerli dizine içerik root [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) içine `Program.Main`:

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

* * *
Statik dosyalar web kök göreli bir yol üzerinden erişilebilir. Örneğin, **Web uygulaması** proje şablonu içeren birkaç klasörlere *wwwroot* klasörü:

* **wwwroot**
  * **CSS**
  * **Görüntüleri**
  * **js**

Bir dosyaya erişmek için URI biçimi *görüntüleri* alt *http://\<server_address > /images/\<image_file_name >*. Örneğin, *http://localhost:9189/images/banner3.svg*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

.NET Framework'ü hedefleme varsa ekleyin [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) projenize paket. .NET Core hedefleme varsa [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) bu paketi içerir.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Ekleme [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) projenize paket.

---

Yapılandırma [ara yazılım](xref:fundamentals/middleware/index) statik dosyaları sunma sağlar.

### <a name="serve-files-inside-of-web-root"></a>Web kök içinde dosyaları sunar

Çağırma [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) yöntemi içinde `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

Parametresiz `UseStaticFiles` yöntemi aşırı yüklemesini dosyaları web kök servable olarak işaretler. Aşağıdaki biçimlendirme başvuru *wwwroot/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a>Web kök dışında dosyaları sunar

Sunulacak statik dosyaları dışında web kök bulunduğu bir dizin hiyerarşisi göz önünde bulundurun:

* **wwwroot**
  * **CSS**
  * **Görüntüleri**
  * **js**
* **MyStaticFiles**
  * **Görüntüleri**
      * *banner1.svg*

Bir istek erişebilirsiniz *banner1.svg* statik dosya ara yazılımlarını şu şekilde yapılandırarak dosyası:

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

Önceki kod *MyStaticFiles* dizin hiyerarşisi gösterilir herkese açık şekilde aracılığıyla *StaticFiles* URI kesimi. Bir istek *http://\<server_address > /StaticFiles/images/banner1.svg* hizmet *banner1.svg* dosya.

Aşağıdaki biçimlendirme başvuru *MyStaticFiles/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a>HTTP yanıt üstbilgilerini Ayarla

A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) nesnesi, HTTP yanıt üstbilgilerini Ayarla için kullanılabilir. Statik dosya sunucusu web kök yapılandırmaya ek olarak, aşağıdaki kod kümeleri `Cache-Control` üstbilgisi:

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) yöntemi mevcut [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paket.

Dosyaları 10 dakika (600 saniye olarak) için genel olarak alınabilir yapılmıştır:

![Cache-Control üstbilgisinin gösteren yanıt üstbilgilerini eklendi](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>Statik dosya yetkilendirme

Statik dosya ara yazılımlarını yetkilendirme denetimleri sağlamaz. Herhangi bir dosya sunulan işlem tarafından altında dahil olmak üzere *wwwroot*, genel olarak erişilebilir. Dosyalara hizmet üzerinde Yetkilendirme göre:

* Bunları dışında depolamak *wwwroot* ve statik dosya ara yazılımı için erişilebilir olan herhangi bir dizin **ve**
* Bunları yetkilendirme uygulandığı bir eylem yöntemiyle hizmet. Dönüş bir [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) nesnesi:

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a>Dizin taramayı etkinleştir

Dizin tarama dizin listesini görmek, web uygulamanızın kullanıcılara ve belirtilen bir dizin içinde dosyaları. Dizin tarama varsayılan olarak devre dışıdır güvenlik nedenleriyle (bkz [konuları](#considerations)). Etkinleştirme dizin harekete geçirerek tarama [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) yönteminde `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

Gerekli hizmetler çağırarak eklemek [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) yönteminden `Startup.ConfigureServices`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

Önceki kod, dizin tarama verir *wwwroot/görüntüleri* URL'yi kullanarak klasör *http://\<server_address > / MyImages*, her dosya ve klasör için bağlantılar ile birlikte:

![Dizin tarama](static-files/_static/dir-browse.png)

Bkz: [konuları](#considerations) güvenlik gözatma etkinleştirirken riskleri üzerinde.

İki Not `UseStaticFiles` aşağıdaki örnekte çağırır. İlk çağrıda statik dosyaları sunma etkinleştirir *wwwroot* klasör. İkinci çağrı, dizin taramayı etkinleştirir *wwwroot/görüntüleri* URL'yi kullanarak klasör *http://\<server_address > / MyImages*:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a>Varsayılan bir belge sunacak

Varsayılan giriş sayfası ayarı ziyaretçileri mantıksal bir başlangıç noktası, sitenizi ziyaret eden sağlar. Varsayılan sayfa tam URI uygun kullanıcı hizmet vermek için arama [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) yönteminden `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> `UseDefaultFiles` önce çağrılmalıdır `UseStaticFiles` varsayılan dosyayı sunması için. `UseDefaultFiles` dosyayı gerçekten sunması olmayan bir URL yeniden yazan olur. Aracılığıyla statik dosya ara yazılımlarını etkinleştir `UseStaticFiles` dosyayı sunması için.

İle `UseDefaultFiles`, bir klasör arama istekleri:

* *default.htm*
* *default.html*
* *index.htm*
* *index.html*

İstek gibi davranarak tam uygun URI ilk listeden bulunan dosya sunulur. Tarayıcı URL İstenen URI yansıtacak şekilde devam eder.

Varsayılan dosya adı aşağıdaki kod değişiklikleri *mydefault.html*:

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a>UseFileServer

[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) birleştirir `UseStaticFiles`, `UseDefaultFiles`, ve `UseDirectoryBrowser`.

Aşağıdaki kod, hizmet varsayılan dosya ve statik dosyaların etkinleştirir. Dizin tarama etkin değil.

```csharp
app.UseFileServer();
```

Aşağıdaki kod, dizin tarama etkinleştirerek parametresiz aşırı oluşturur:

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
      * *banner1.svg*
  * *default.html*

Aşağıdaki kod statik dosyalar, varsayılan dosya ve Dizin tarama etkinleştirir `MyStaticFiles`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

`AddDirectoryBrowser` ne zaman çağrılmalıdır `EnableDirectoryBrowsing` özellik değeri `true`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

Dosya hiyerarşisi kullanarak ve önceki kod, URL şu şekilde çözün:

| URI            |                             Yanıt  |
| ------- | ------|
| *http://\<server_address>/StaticFiles/images/banner1.svg*    |      MyStaticFiles/images/banner1.svg |
| *http://\<server_address>/StaticFiles*             |     MyStaticFiles/default.html |

Varsayılan adlı dosya varsa *MyStaticFiles* dizin *http://\<server_address > / StaticFiles* dizin tıklanabilir bağlantıları ile listeleme döndürür:

![Statik dosyaların listesi](static-files/_static/db2.png)

> [!NOTE]
> `UseDefaultFiles` ve `UseDirectoryBrowser` URL'sini kullanarak *http://\<server_address > / StaticFiles* istemci tarafı tetiklemek için eğik yeniden yönlendirme *http://\<server_address > / StaticFiles /*. Eğik eklenmesi dikkat edin. Belgelerde göreli URL eğik geçersiz olarak kabul edilen.

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

[FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) sınıfı içeren bir `Mappings` MIME içerik türleri için dosya uzantıları eşlemesi olarak hizmet veren özelliği. Aşağıdaki örnekte, çeşitli dosya uzantılarını bilinen MIME türleri için kayıtlı. *.Rtf* uzantısı değiştirilir ve *.mp4* kaldırılır.

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

Bkz: [MIME içerik türleri](http://www.iana.org/assignments/media-types/media-types.xhtml).

## <a name="non-standard-content-types"></a>Standart olmayan içerik türleri

Statik dosya ara yazılımlarını neredeyse 400 bilinen dosya içerik türleri bilir. Kullanıcı bir bilinmeyen dosya türü bir dosya istediğinde, statik dosya ara yazılımlarını bir HTTP 404 (bulunamadı) yanıtı döndürür. Dizin tarama etkin değilse, dosyaya bir bağlantı görüntülenir. URI HTTP 404 hatası döndürür.

Aşağıdaki kod bilinmeyen tür hizmet veren sağlar ve bir görüntü olarak bilinmeyen dosya işler:

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

Önceki kod ile bilinmeyen bir içerik türüne sahip bir dosya için bir istek bir görüntü olarak döndürülür.

> [!WARNING]
> Etkinleştirme [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) bir güvenlik riski oluşturur. Varsayılan olarak devre dışıdır ve kullanımı önerilmez. [FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) standart olmayan uzantılı dosyaları sunma için daha güvenli bir alternatif sunar.

### <a name="considerations"></a>Dikkat Edilecekler

> [!WARNING]
> `UseDirectoryBrowser` ve `UseStaticFiles` gizli sızıntısı. Devre dışı bırakma dizin üretimde tarama kullanmamanız önerilir. Hangi dizinler aracılığıyla etkinleştirilir dikkatlice inceleyin `UseStaticFiles` veya `UseDirectoryBrowser`. Tüm dizin ve alt dizinleri genel olarak erişilebilir duruma gelir. Depolama dosyaları genel hizmet için uygun adanmış bir dizinde gibi  *\<content_root > / wwwroot*. Bu dosyalar MVC görünümleri, Razor sayfalarının (yalnızca 2.x), yapılandırma dosyalarını, vb. ayırın.

* İle kullanıma sunulan içerik için URL'leri `UseDirectoryBrowser` ve `UseStaticFiles` büyük küçük harfe duyarlılığın ve temeldeki dosya sistemi karakter kısıtlamalarını tabidir. Örneğin, Windows büyük küçük harfe duyarlı&mdash;macOS ve Linux değil.

* IIS kullanımda barındırılan ASP.NET Core uygulamaları [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) tüm statik dosya istekleri dahil olmak üzere uygulama isteklerini iletmek için. IIS statik dosya işleyici kullanılmaz. Bir modül tarafından işlenen önce isteklerini işlemek için bir fırsat sahiptir.

* Sunucu veya Web sitesi düzeyinde IIS statik dosya işleyici kaldırmak için IIS Yöneticisi'nde aşağıdaki adımları tamamlayın:
    1. Gidin **modülleri** özelliği.
    1. Seçin **StaticFileModule** listesinde.
    1. Tıklatın **kaldırmak** içinde **Eylemler** kenar.

> [!WARNING]
> IIS statik dosya işleyici etkinleştirilirse **ve** ASP.NET Core modülü yanlış yapılandırılmış, statik dosyalar sunulduğunu. Bu, örneğin, olur *web.config* değil dosya dağıtılır.

* Kod dosyaları yerleştirmek (de dahil olmak üzere *.cs* ve *.cshtml*) uygulama projenin web kökü dışında. Mantıksal ayırma, bu nedenle uygulamanın istemci-tarafı içerik ve sunucu tabanlı kod arasında oluşturulur. Bu, sunucu tarafı kodu sızmasını önler.

## <a name="additional-resources"></a>Ek kaynaklar

* [Ara Yazılım](xref:fundamentals/middleware/index)
* [ASP.NET Core giriş](xref:index)
