---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: ASP.NET Web API 2 (C#) ile çalışmaya başlama
author: MikeWasson
description: HTTP web sayfaları yalnızca hizmet vermek için değil. Veri ve hizmetlerinizi kullanıma API'leri oluşturmak için de güçlü bir platformdur. Basit, esnek ve ubiq HTTP'dir...
ms.author: aspnetcontent
ms.date: 11/28/2017
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 170e361c46631e7ecdbe026061c181158dcf803f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823426"
---
<a name="get-started-with-aspnet-web-api-2-c"></a>ASP.NET Web API 2 (C#) ile çalışmaya başlama
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

[Projeyi yükle](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

HTTP web sayfaları yalnızca hizmet vermek için değil. Ayrıca veri ve hizmetlerinizi kullanıma API'leri oluşturmaya yönelik güçlü bir platform HTTP'dir. Basit, esnek ve her yerde karşılaşılan HTTP'dir. İstemciler, tarayıcılar, mobil cihazları ve geleneksel masaüstü uygulamaları gibi çok geniş bir yelpazede HTTP Hizmetleri ulaşabilmeniz için bir HTTP kitaplığı kadar aklınıza gelebilecek neredeyse herhangi bir platform sahiptir.
 
ASP.NET Web API'si, .NET Framework üzerinde web API'leri oluşturmaya yönelik bir çerçevedir. Bu öğreticide, bir web ürünlerin listesini döndüren API oluşturmak için ASP.NET Web API kullanır.

 
 ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
  
 - [Visual Studio 2017](https://www.visualstudio.com/downloads/)
 - Web API 2

Bkz: [ASP.NET Core ve Windows için Visual Studio ile web API'si oluşturma](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) Bu öğreticinin daha yeni bir sürümü için.

## <a name="create-a-web-api-project"></a>Bir Web API projesi oluşturma

Bu öğreticide, bir web ürünlerin listesini döndüren API oluşturmak için ASP.NET Web API kullanır. Ön uç web sayfası, sonuçları görüntülemek için jQuery kullanır.

![](tutorial-your-first-web-api/_static/image1.png)

Visual Studio'yu başlatın ve seçin **yeni proje** gelen **Başlat** sayfası. Veya **dosya** menüsünde **yeni** ardından **proje**.

İçinde **şablonları** bölmesinde **yüklü şablonlar** genişletin **Visual C#** düğümü. Altında **Visual C#** seçin **Web**. Proje şablonları listesinde seçin **ASP.NET Web uygulaması**. "ProductsApp" Projeyi adlandırın ve tıklayın **Tamam**.

![](tutorial-your-first-web-api/_static/image2.png)

İçinde **yeni ASP.NET projesi** iletişim kutusunda **boş** şablonu. Altında &quot;klasörleri ekleyin ve çekirdek başvuruları&quot;, kontrol **Web API**. **Tamam**'ı tıklatın.

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> Kullanarak bir Web API projesi oluşturabilirsiniz &quot;Web API&quot; şablonu. API Yardım sayfaları sağlamak için ASP.NET MVC Web API şablonu kullanır. MVC olmadan Web API gösterilecek istediğinden Bu öğretici için boş şablonu kullanıyorum. Genel olarak, ASP.NET MVC, Web API'sini kullanmak için bilmeniz gerekmez.


## <a name="adding-a-model"></a>Model ekleme

A *modeli* uygulamanızdaki verileri temsil eden bir nesnedir. ASP.NET Web API'si, JSON, XML veya başka bir biçime modelinizi otomatik serileştir ve HTTP yanıt iletisinin gövdesine serileştirilmiş veriler yazın. Bir istemci serileştirme biçimi okuyabilirsiniz sürece, nesne seri durumdan çıkarabiliyorsa. Çoğu istemci, XML veya JSON ayrıştırabilirsiniz. Ayrıca, istemci HTTP isteğine bir Accept üst bilgisi ayarlayarak istediği hangi biçimi belirtebilirsiniz.

Bir ürünü temsil eden basit bir modeli oluşturarak başlayalım.

Çözüm Gezgini görünür değilse, **görünümü** menü ve select **Çözüm Gezgini**. Çözüm Gezgini'nde modeller klasörü sağ tıklatın. Bağlam menüsünden seçin **Ekle** seçip **sınıfı**.

![](tutorial-your-first-web-api/_static/image4.png)

Sınıf adı &quot;ürün&quot;. Aşağıdaki özellikleri `Product` sınıfı.

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a>Denetleyici ekleme

Web API'de bir *denetleyicisi* HTTP isteklerini işleyen bir nesnedir. Ürünlerin listesini ya da kimliği ile belirtilen tek bir ürün döndürebilen bir denetleyici ekleyeceğiz.

> [!NOTE]
> ASP.NET MVC kullandıysanız denetleyicileriyle bilginiz. Web API denetleyicisi, MVC denetleyicileri için benzerdir, ancak devralınan **ApiController** sınıfı yerine **denetleyicisi** sınıfı.

İçinde **Çözüm Gezgini**, denetleyicileri klasörü sağ tıklatın. Seçin **Ekle** seçip **denetleyicisi**.

![](tutorial-your-first-web-api/_static/image5.png)

İçinde **İskele Ekle** iletişim kutusunda **Web API denetleyicisi – boş**. **Ekle**'yi tıklatın.

![](tutorial-your-first-web-api/_static/image6.png)

İçinde **denetleyici Ekle** iletişim kutusunda, denetleyici adı &quot;ProductsController&quot;. **Ekle**'yi tıklatın.

![](tutorial-your-first-web-api/_static/image7.png)

Yapı iskelesinde denetleyicileri klasöründeki ProductsController.cs adlı bir dosya oluşturur.

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> Denetleyicilerinizi denetleyicileri adlı klasöre koyun gerek yoktur. Klasör adı, kaynak dosyalarınızı düzenlemek için yalnızca bir yoludur.


Bu dosya zaten açık değilse, açmak için dosyaya çift tıklayın. Bu dosyadaki kodu aşağıdakiyle değiştirin:

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

Örneği basit tutmak için ürünleri sabit bir dizi denetleyici sınıfı içinde depolanır. Elbette, gerçek bir uygulamada, veritabanını sorgulama veya başka bir dış veri kaynağı kullanın.

Denetleyici ürünleri döndüren iki yöntemi tanımlar:

- `GetAllProducts` Yöntemi ürünlerin tam listesini döndüren bir **IEnumerable&lt;ürün&gt;**  türü.
- `GetProduct` Kendi kimliği tarafından tek bir ürün şuna yöntemi

İşte bu kadar! Sahip olduğunuz çalışma web API'si. Denetleyicisindeki her yöntem için en az bir URI'leri karşılık gelmektedir:

| Denetleyici yöntemi | URI |
| --- | --- |
| GetAllProducts | / api/ürünleri |
| GetProduct | /API'si/ürünler/*kimliği* |

İçin `GetProduct` yöntemi *kimliği* URI'de bir yer tutucudur. Örneğin, 5 Kimliğine sahip bir ürün almak için URI değil `api/products/5`.

Web API HTTP istekleri için denetleyici yöntemleri nasıl yönlendirdiğini hakkında daha fazla bilgi için bkz. [ASP.NET Web API'de yönlendirme](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="calling-the-web-api-with-javascript-and-jquery"></a>Javascript ve jQuery ile Web API çağırma

Bu bölümde, web API'sini çağırmak için AJAX kullanan bir HTML sayfasına ekleyeceğiz. AJAX çağrılarını ve sayfa sonuçları ile güncelleştirmek için jQuery kullanacağız.

Çözüm Gezgini'nde projeye sağ tıklayıp seçin **Ekle**, ardından **yeni öğe**.

![](tutorial-your-first-web-api/_static/image9.png)

İçinde **Yeni Öğe Ekle** iletişim kutusunda **Web** düğümünde **Visual C#** ve ardından **HTML sayfası** öğesi. Sayfayı adlandırın &quot;index.html&quot;.

![](tutorial-your-first-web-api/_static/image10.png)

Bu dosyadaki her şeyi aşağıdakiyle değiştirin:

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

JQuery almak için birkaç yolu vardır. Bu örnekte, kullandım [Microsoft Ajax CDN](../../../ajax/cdn/overview.md). Nden de indirebilirsiniz [ http://jquery.com/ ](http://jquery.com/)ve "Web API'si" ASP.NET proje şablonu, jQuery de içerir.

### <a name="getting-a-list-of-products"></a>Ürünlerin listesini alma

Ürünlerin listesini almak için bir HTTP GET isteği gönderme &quot;/api/ürünleri&quot;.

JQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) işlevi bir AJAX isteği gönderir. Yanıt için JSON nesne dizisi içerir. `done` İşlevi istek başarılı olursa çağrılan bir geri çağırma işlemini belirtir. Geri çağırma içinde ürün bilgileri DOM güncelleştiriyoruz.

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a>Bir ürün Kimliğine göre alma

Kimliğe göre bir ürün almak için bir HTTP GET isteği gönderme &quot;/API'si/ürünler/*kimliği*&quot;burada *kimliği* ürün kimliğidir.

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

Hala diyoruz `getJSON` AJAX isteği, ancak bu kez göndermek için biz kimliği URI isteğinde yerleştirin. Bu istek yanıtı tek bir ürün JSON gösterimidir.

## <a name="running-the-application"></a>Uygulamayı Çalıştırma

Uygulama hata ayıklamayı başlatmak için F5 tuşuna basın. Web sayfasını aşağıdaki gibi görünmelidir:

![](tutorial-your-first-web-api/_static/image11.png)

Kimliğe göre bir ürün almak için kimliği girin ve ara:

![](tutorial-your-first-web-api/_static/image12.png)

Geçersiz kimlik girerseniz, sunucunun bir HTTP hata döndürür:

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a>HTTP istek ve yanıt görüntülemek için F12'ı kullanma

Bir HTTP hizmeti ile çalışırken, HTTP isteği görmek ve iletileri istek çok kullanışlı olabilir. Internet Explorer 9'da F12 geliştirici araçlarını kullanarak bunu yapabilirsiniz. Internet Explorer 9'dan basın **F12** Araçları'nı açın. Tıklayın **ağ** sekmesi ve ENTER tuşuna **Yakalamayı Başlat**. Artık a basın ve web sayfasına dönün **F5** web sayfasını yeniden yüklemek için. Internet Explorer tarayıcı ve web sunucusu arasında HTTP trafiğini yakalar. Özet görünümü, bir sayfa için tüm ağ trafiğini gösterir:

![](tutorial-your-first-web-api/_static/image14.png)

Göreli URI'si girişini bulun "API/Ürünler /". Bu girdiyi seçin ve tıklayın **ayrıntılı görünümüne gidin**. Ayrıntılı görünümde gövdeleri ve istek ve yanıt üst bilgileri görüntülemek için sekme bulunur. Örneğin, tıklarsanız **istek üst** sekmesinde istemci istenen görebilirsiniz &quot;application/json&quot; Accept üstbilgisindeki.

![](tutorial-your-first-web-api/_static/image15.png)

Yanıt gövdesi sekmesini tıklatırsanız, nasıl ürün listesi JSON için seri duruma görebilirsiniz. Diğer tarayıcılarda benzer işlevselliğe sahiptir. Başka bir kullanışlı bir araçtır [Fiddler](http://www.fiddler2.com/fiddler2/), bir web proxy hata ayıklama. Fiddler, HTTP trafiğini görüntülemek için ve ayrıca HTTP istekleri oluşturmak için istek HTTP üstbilgilerini üzerinde tam denetim imkanı sağlayan kullanabilirsiniz.

## <a name="see-this-app-running-on-azure"></a>Azure üzerinde çalışan bu uygulamayı bakın

Canlı web uygulaması olarak çalışan tamamlanmış site görmek ister misiniz? Aşağıdaki düğmeye tıklayarak Azure hesabınızda bir tam sürümü uygulama dağıtabilirsiniz.

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

Bu çözüm, Azure'a dağıtmak için bir Azure hesabına ihtiyacınız var. Bir hesap zaten yoksa, aşağıdaki seçenekleriniz:

- [Ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -KREDİLERİ edinin, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.
- [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -MSDN aboneliğiniz size kredi verir, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.

## <a name="next-steps"></a>Sonraki Adımlar

- POST, PUT ve DELETE işlemleri destekleyen ve bir veritabanına yazar HTTP hizmetine daha eksiksiz bir örnek için bkz: [Entity Framework 6 ile Web API 2 kullanarak](../data/using-web-api-with-entity-framework/part-1.md).
- Bir HTTP hizmetine üzerine esnektir, hızlı yanıt veren web uygulamaları oluşturma hakkında daha fazla bilgi için bkz. [ASP.NET tek sayfalık uygulaması](../../../single-page-application/index.md).
- Visual Studio web projesini Azure App Service'e dağıtma hakkında daha fazla bilgi için bkz: [Azure App Service'te ASP.NET web uygulaması oluşturma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
