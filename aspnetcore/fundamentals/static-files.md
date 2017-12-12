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
# <a name="working-with-static-files-in-aspnet-core"></a>ASP.NET Core statik dosyaları ile çalışma

<a name="fundamentals-static-files"></a>

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

HTML, CSS, görüntü ve JavaScript gibi statik dosyaları ASP.NET Core uygulama doğrudan istemcilere hizmet verebilir varlıkları içerir.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="serving-static-files"></a>Statik dosyaları sunma

Statik dosyalar genellikle yerleştirilir `web root` (*\<içeriği kök > / wwwroot*) klasör. Bkz: [içerik kök](xref:fundamentals/index#content-root) ve [Web kök](xref:fundamentals/index#web-root) daha fazla bilgi için. Geçerli dizin olarak içerik kök genellikle ayarlamak için projenizin `web root` geliştirme sırasında bulundu.

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]

Statik dosyalar, altında herhangi bir klasörde depolanabilir `web root` ve o kökü için göreli bir yol ile erişilebilir. Örneğin, Visual Studio kullanarak bir varsayılan Web uygulaması projesi oluşturduğunuzda, içinde oluşturulan çeşitli klasörler vardır *wwwroot* klasör - *css*, *görüntüleri*, ve *js*. Görüntüde erişmek için URI *görüntüleri* alt:

* `http://<app>/images/<imageFileName>`
* `http://localhost:9189/images/banner3.svg`

Statik dosyaların sunulması sırayla yapılandırmanız gerekir [ara yazılım](middleware.md) ardışık düzene statik dosyaları eklemek için. Statik dosya ara yazılımlarını bir bağımlılık ekleyerek yapılandırılabilir *Microsoft.AspNetCore.StaticFiles* paketini proje ve ardından arama `UseStaticFiles` uzantısı yönteminden `Startup.Configure`:

[!code-csharp[Main](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]

`app.UseStaticFiles();`dosyaları yapar `web root` (*wwwroot* varsayılan olarak) servable. Daha sonra diğer dizin içeriği ile servable nasıl göstereceğiz `UseStaticFiles`.

"Microsoft.AspNetCore.StaticFiles" NuGet paketini eklemeniz gerekir.

> [!NOTE]
> `web root`Varsayılan olarak *wwwroot* dizin, ancak ayarlayabilirsiniz `web root` ile dizin `UseWebRoot`.

Hizmet istediğiniz statik dosyaların nerede dışında bir proje hiyerarşisi olduğunu varsayalım `web root`. Örneğin:

* wwwroot
  * CSS
  * görüntüler
  * ...
* MyStaticFiles
  * Test.PNG

Bir isteğin erişmek *test.png*, statik dosya ara yazılımı aşağıdaki gibi yapılandırın:

[!code-csharp[Main](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]

Bir istek `http://<app>/StaticFiles/test.png` görecek *test.png* dosya.

`StaticFileOptions()`Yanıt Üstbilgileri ayarlayabilirsiniz. Örneğin, aşağıdaki kodu gelen statik dosya ayarlar *wwwroot* klasörü ve kümelerini `Cache-Control` başlığı 10 dakika (600 saniye olarak) olmalarını genel olarak alınabilir:

[!code-csharp[Main](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]

[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) yöntemi edinilebilir [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paket. Ekleme `using Microsoft.AspNetCore.Http;` için *csharp* yöntemi kullanılamıyorsa, dosya.

![Cache-Control üstbilgisinin gösteren yanıt üstbilgilerini eklendi](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>Statik dosya yetkilendirme

Statik dosya modülü sağlar **hiçbir** yetkilendirme denetimleri. Herhangi bir dosya sunulan işlem tarafından altında dahil olmak üzere *wwwroot* genel olarak kullanılabilir. Dosyalara hizmet üzerinde Yetkilendirme göre:

* Bunları dışında depolamak *wwwroot* ve statik dosya ara yazılımı için erişilebilir olan herhangi bir dizin **ve**

* Döndüren bir denetleyici eylemi hizmet bir `FileResult` yetkilendirme burada uygulanır

## <a name="enabling-directory-browsing"></a>Dizin taramayı etkinleştirme

Dizin tarama dizin ve belirli bir dizindeki dosyaların listesini görmek, web uygulamanızın verir. Dizin tarama varsayılan olarak devre dışıdır güvenlik nedenleriyle (bkz [konuları](#considerations)). Dizin taramayı etkinleştirmek için arama `UseDirectoryBrowser` uzantısı yönteminden `Startup.Configure`:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet1)]

Gerekli hizmetler çağırarak ekleyin `AddDirectoryBrowser` uzantısı yönteminden `Startup.ConfigureServices`:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet2)]

Yukarıdaki kod, dizin tarama verir *wwwroot/görüntüleri* klasörü URL http:// kullanarak\<uygulama > / her dosya ve klasör için bağlantılar ile birlikte MyImages:

![Dizin tarama](static-files/_static/dir-browse.png)

Bkz: [konuları](#considerations) güvenlik gözatma etkinleştirirken riskleri üzerinde.

İki Not `app.UseStaticFiles` çağrıları. Birinci CSS, görüntüler ve JavaScript hizmet vermek için gereken *wwwroot* klasörü ve Dizin tarama için ikinci çağrı *wwwroot/görüntüleri* klasörü URL http:// kullanarak\<uygulama > / MyImages:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]

## <a name="serving-a-default-document"></a>Hizmet veren bir varsayılan belge

Varsayılan giriş sayfası ayarı site ziyaretçilerini sitenizi ziyaret eden başlatmak için bir yer sağlar. URI tam olarak nitelemek gerek kalmadan kullanıcı bir varsayılan sayfa sunmak Web uygulamanız için sırayla çağırın `UseDefaultFiles` uzantısı yönteminden `Startup.Configure` gibi.

[!code-csharp[Main](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]

> [!NOTE]
> `UseDefaultFiles`önce çağrılmalıdır `UseStaticFiles` varsayılan dosyayı sunması için. `UseDefaultFiles`dosyayı gerçekten sunması olmayan bir URL yeniden yazıcı olur. Statik dosya ara yazılımlarını etkinleştir (`UseStaticFiles`) dosyayı sunması için.

İle `UseDefaultFiles`, bir klasör için istekleri için arama:

* default.htm
* default.HTML
* index.htm
* index.HTML

(Tarayıcı URL İstenen URI göstermeye devam eder ancak) istek tam uygun URI boşmuş gibi ilk listeden bulunan dosya sunulacak.

Aşağıdaki kod için varsayılan dosya adını değiştirmek nasıl gösterir *mydefault.html*.

[!code-csharp[Main](static-files/sample/StartupDefault.cs?name=snippet1)]

## <a name="usefileserver"></a>UseFileServer

`UseFileServer`birleştirir `UseStaticFiles`, `UseDefaultFiles`, ve `UseDirectoryBrowser`.

Aşağıdaki kod statik dosyaları ve sunulması için varsayılan dosya etkinleştirir Dizin tarama izin vermez ancak:

```csharp
app.UseFileServer();
   ```

Aşağıdaki kod, statik dosyalar, varsayılan dosya ve dizin taramayı etkinleştirir:

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
   ```

Bkz: [konuları](#considerations) güvenlik gözatma etkinleştirirken riskleri üzerinde. Olduğu gibi `UseStaticFiles`, `UseDefaultFiles`, ve `UseDirectoryBrowser`, dışında mevcut dosyaları işleme almak isterseniz `web root`, örneği ve yapılandırma bir `FileServerOptions` bir parametre olarak geçirdiğiniz nesne `UseFileServer`. Örneğin, aşağıdaki dizin hiyerarşi Web uygulamanızda verilen:

* wwwroot

  * CSS

  * görüntüler

  * ...

* MyStaticFiles

  * Test.PNG

  * default.HTML

Yukarıdaki hiyerarşisi örneği kullanarak, statik dosyalar, varsayılan dosyalar ve için gözatma etkinleştirmek isteyebilirsiniz `MyStaticFiles` dizin. Aşağıdaki kod parçacığında, gerçekleştirilir tek çağrısıyla `FileServerOptions`.

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]

Varsa `enableDirectoryBrowsing` ayarlanır `true` aramak için gerekli `AddDirectoryBrowser` uzantısı yönteminden `Startup.ConfigureServices`:

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?name=snippet2)]

Dosya hiyerarşisi ve yukarıdaki kodu kullanarak:

| URI            |                             Yanıt  |
| ------- | ------|
| `http://<app>/StaticFiles/test.png`    |      MyStaticFiles/test.png |
| `http://<app>/StaticFiles`              |     MyStaticFiles/default.html |

Dosyaları adlı varsayılan olsalar *MyStaticFiles* dizin, http://\<uygulama > / StaticFiles dizin tıklanabilir bağlantıları ile listeleme döndürür:

![Statik dosyaların listesi](static-files/_static/db2.PNG)

> [!NOTE]
> `UseDefaultFiles`ve `UseDirectoryBrowser` url http:// sürer\<uygulama > / StaticFiles eğik ve neden olmadan bir istemci tarafı yönlendirmek için http://\<uygulama > /StaticFiles/ (eğik ekleyerek). Sondaki eğik çizgi göreli URL belgelerde olmadan yanlış olabilir.

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

`FileExtensionContentTypeProvider` Sınıfı MIME içerik türleri için dosya uzantıları eşleyen bir koleksiyonu içerir. Aşağıdaki örnekte, bilinen MIME türleri için birkaç dosya uzantılarının kayıtlı olduğundan, ".rtf" değiştirilir ve ".mp4" kaldırılır.

[!code-csharp[Main](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]

Bkz: [MIME içerik türleri](http://www.iana.org/assignments/media-types/media-types.xhtml).

## <a name="non-standard-content-types"></a>Standart olmayan içerik türleri

ASP.NET statik dosya ara yazılımlarını neredeyse 400 bilinen dosya içerik türleri bilir. Kullanıcı bir bilinmeyen dosya türü bir dosya istediğinde, statik dosya ara yazılımlarını (bulunamadı) HTTP 404 yanıtı döndürür. Dizin tarama etkin olduğunda dosyaya bir bağlantı görüntülenir, ancak URI HTTP 404 hata döndürür.

Aşağıdaki kod, bilinmeyen tür hizmet veren sağlar ve bilinmeyen dosya bir resim olarak kabul eder.

[!code-csharp[Main](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]

Yukarıdaki kodu ile birlikte bir istek bilinmeyen bir içerik türüne sahip bir dosya için bir resim olarak döndürülür.

>[!WARNING]
> Etkinleştirme `ServeUnknownFileTypes` bir güvenlik riski oluşturur ve bunu kullanarak önerilmez.  `FileExtensionContentTypeProvider`(yukarıda açıklanan) standart olmayan uzantılı dosyaları sunma için daha güvenli bir alternatif sunar.

### <a name="considerations"></a>Dikkat Edilecekler

>[!WARNING]
> `UseDirectoryBrowser`ve `UseStaticFiles` gizli sızıntısı. Öneririz, **değil** etkinleştir dizin üretimde tarama. Dikkatli olun ile etkinleştirme hakkında hangi dizinleri `UseStaticFiles` veya `UseDirectoryBrowser` tüm dizin ve tüm alt dizinler erişilemeyeceği. Ortak içeriğe gibi kendi dizininde tutma öneririz  *\<içerik kök > / wwwroot*, uygulama görünümleri, yapılandırma dosyalarını, vb. ayrılmayın.

* İle kullanıma sunulan içerik için URL'leri `UseDirectoryBrowser` ve `UseStaticFiles` büyük küçük harfe duyarlılığın ve bunların temel alınan dosya sisteminin karakter kısıtlamaları tabidir. Örneğin, Windows büyük küçük harfe duyarlı ancak Mac ve Linux değildir.

* IIS'de barındırılan ASP.NET Core uygulamaları ASP.NET Core modülü statik dosyaları için istekleri dahil olmak üzere uygulama için tüm istekleri iletmek için kullanın. ASP.NET çekirdeği modülü tarafından işlenen önce isteklerini işlemek için bir fırsat açılmıyor çünkü IIS statik dosya işleyici kullanılmaz.

* IIS statik dosya işleyici (düzeyinde sunucusunu veya Web sitesi) kaldırmak için:

     * Gidin **modülleri** özelliği

     * Seçin **StaticFileModule** listesinde

     * Dokunun **kaldırmak** içinde **Eylemler** kenar çubuğu

>[!WARNING]
> IIS statik dosya işleyici etkinleştirilirse **ve** ASP.NET Core Modülü (ANCM) düzgün yapılandırılmamış (örneğin, *web.config* dağıtılmamış), statik dosyalar hizmet verilen.

* Kod dosyaları (c# ve Razor dahil) uygulama projenin dışında yerleştirilmelidir `web root` (*wwwroot* varsayılan olarak). Bu, uygulamanızın istemci tarafı içeriği ve sunucu tarafı kodu sızmasını engeller sunucu tarafı kaynak kodu arasında temiz bir ayrım oluşturur.

## <a name="additional-resources"></a>Ek Kaynaklar

* [Ara yazılım](middleware.md)

* [ASP.NET Core giriş](../index.md)
