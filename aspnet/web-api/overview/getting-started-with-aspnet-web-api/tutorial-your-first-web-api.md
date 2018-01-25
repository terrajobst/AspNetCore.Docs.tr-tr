---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: "ASP.NET Web API 2 (C#) ile çalışmaya başlama"
author: MikeWasson
description: "HTTP web sayfaları yalnızca hizmet vermek için değil. Hizmetler ve veri kullanıma API'ları oluşturmak için de güçlü bir platformdur. HTTP basit, esnek ve ubiq ise..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/28/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 6ff9fd279a03197f761454bba3f180d7428b1b1f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-aspnet-web-api-2-c"></a>ASP.NET Web API 2 (C#) ile çalışmaya başlama
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

HTTP web sayfaları yalnızca hizmet vermek için değil. Ayrıca Hizmetleri ve veriler kullanıma API'ları oluşturmak için güçlü bir platform HTTP'dir. Basit, esnek ve her yerden HTTP'dir. HTTP Hizmetleri çok çeşitli istemciler, tarayıcılar, mobil cihazlar ve geleneksel masaüstü uygulamaları gibi ulaşabilmeniz için bir HTTP kitaplığı düşünebilirsiniz neredeyse herhangi bir platform sahiptir.
 
ASP.NET Web API, web API'leri .NET Framework üzerine oluşturmaya yönelik bir çerçevedir. Bu öğreticide, bir web ürünlerin listesini döndürür API oluşturmak için ASP.NET Web API kullanır.

 
 ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
  
 - [Visual Studio 2017](https://www.visualstudio.com/downloads/)
 - Web API 2

Bkz: [ASP.NET Core ve Windows için Visual Studio ile web API oluşturma](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) Bu öğreticide daha yeni bir sürümü için.

## <a name="create-a-web-api-project"></a>Bir Web API projesi oluşturma

Bu öğreticide, bir web ürünlerin listesini döndürür API oluşturmak için ASP.NET Web API kullanır. Ön uç web sayfası jQuery sonuçları görüntülemek için kullanır.

![](tutorial-your-first-web-api/_static/image1.png)

Visual Studio'yu başlatın ve seçin **yeni proje** gelen **Başlat** sayfası. Veya **dosya** menüsünde, select **yeni** ve ardından **proje**.

İçinde **şablonları** bölmesinde, **yüklü şablonlar** ve genişletin **Visual C#** düğümü. Altında **Visual C#**seçin **Web**. Proje şablonları listesinde seçin **ASP.NET Web uygulaması**. "ProductsApp" adını verin ve projeyi tıklatın **Tamam**.

![](tutorial-your-first-web-api/_static/image2.png)

İçinde **yeni ASP.NET projesi** iletişim kutusunda **boş** şablonu. Altında &quot;klasörler ekleme ve çekirdek için başvurular&quot;, denetleme **Web API**. **Tamam**'ı tıklatın.

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> Kullanarak bir Web API projesi oluşturabilirsiniz &quot;Web API&quot; şablonu. Web API şablon ASP.NET MVC API yardım sayfalarına sağlamak için kullanır. MVC olmadan Web API göstermek istediklerinden Bu öğretici için boş şablonu kullanıyorum. Genel olarak, ASP.NET MVC Web API kullanılacağını bilmeniz gerek yoktur.


## <a name="adding-a-model"></a>Model ekleme

A *modeli* uygulamanızdaki verileri temsil eden bir nesnedir. ASP.NET Web API otomatik olarak modelinize JSON, XML veya başka bir biçime serileştirir ve ardından HTTP yanıt iletisinin gövdesine seri hale getirilmiş veri yazar. Bir istemci seri hale getirme biçimi okuyabilirler sürece, nesneyi seri durumdan. Çoğu istemcileri, XML veya JSON ayrıştıramıyor. Ayrıca, istemci HTTP istek iletisinde Accept üstbilgisi ayarlayarak istediği hangi biçimini belirtebilirsiniz.

Bir ürün temsil eden basit bir modeli oluşturarak başlayalım.

Çözüm Gezgini görünür değilse, **Görünüm** menü ve select **Çözüm Gezgini**. Çözüm Gezgini'nde modeller klasörü sağ tıklatın. Bağlam menüsünden seçin **Ekle** seçip **sınıfı**.

![](tutorial-your-first-web-api/_static/image4.png)

Sınıf adını &quot;ürün&quot;. Aşağıdaki özellikleri ekleyin `Product` sınıfı.

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a>Bir denetleyici ekleme

Web API içinde bir *denetleyicisi* HTTP isteklerini işleyen bir nesnedir. Ürünlerin listesini ya da kimliğine göre belirtilen tek ürün döndürebilir denetleyicisi ekleyeceğiz.

> [!NOTE]
> ASP.NET MVC kullandıysanız, denetleyicileriyle bilginiz. Web API denetleyicilerinin MVC denetleyicileri için benzerdir, ancak devral **ApiController** sınıfına **denetleyicisi** sınıfı.

İçinde **Çözüm Gezgini**, denetleyicileri klasörünü sağ tıklatın. Seçin **Ekle** ve ardından **denetleyicisi**.

![](tutorial-your-first-web-api/_static/image5.png)

İçinde **İskele Ekle** iletişim kutusunda **Web API denetleyici - boş**. **Ekle**'yi tıklatın.

![](tutorial-your-first-web-api/_static/image6.png)

İçinde **denetleyici Ekle** iletişim kutusunda, denetleyici adı &quot;ProductsController&quot;. **Ekle**'yi tıklatın.

![](tutorial-your-first-web-api/_static/image7.png)

Yapı iskelesi denetleyicileri klasöründe ProductsController.cs adlı bir dosya oluşturur.

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> Denetleyicilerinizi denetleyicileri adlı bir klasöre yerleştirin gerek yoktur. Klasör adı, kaynak dosyaları düzenlemek için yalnızca bir yoludur.


Bu dosya zaten açık değilse, açmak için dosyaya çift tıklayın. Bu dosyadaki kod aşağıdakiyle değiştirin:

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

Örneği basit tutmak için ürünleri sabit bir dizi denetleyici sınıfı içinde depolanır. Elbette, gerçek bir uygulamada bunu bir veritabanını sorgulamak veya başka bir dış veri kaynağını kullanabilirsiniz.

Denetleyici ürünleri dönüş iki yöntem tanımlar:

- `GetAllProducts` Yöntemi ürünlerinin tam listesini döndürür bir **IEnumerable&lt;ürün&gt;**  türü.
- `GetProduct` Tek bir ürün, kimliğe göre arar yöntemi

İşte bu kadar! Çalışma web API'si var. Denetleyicisinde her bir yöntemi, bir veya daha fazla URI'ler karşılık gelir:

| Denetleyici yöntemi | URI |
| --- | --- |
| GetAllProducts | / api/ürünleri |
| GetProduct | /api/products/*id* |

İçin `GetProduct` yöntemi, *kimliği* URI'de bir yer tutucudur. Örneğin, 5 kimlikli ürün almak için URI. `api/products/5`.

Nasıl Web API denetleyici yöntemlerine HTTP isteklerini yolları hakkında daha fazla bilgi için bkz: [ASP.NET Web API'de yönlendirme](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="calling-the-web-api-with-javascript-and-jquery"></a>Javascript ve Jquery'de ile Web API çağırma

Bu bölümde, web API'sini çağırmak için AJAX kullanan bir HTML sayfası ekleyeceğiz. JQuery AJAX çağrıları yapma ve sayfa sonuçlarıyla güncelleştirmek için kullanacağız.

Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Ekle**seçeneğini belirleyip **yeni öğe**.

![](tutorial-your-first-web-api/_static/image9.png)

İçinde **Yeni Öğe Ekle** iletişim kutusunda **Web** düğümü altında **Visual C#**ve ardından **HTML sayfası** öğesi. Sayfa adı &quot;index.html&quot;.

![](tutorial-your-first-web-api/_static/image10.png)

Bu dosyadaki her şeyi aşağıdakiyle değiştirin:

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

JQuery almak için birkaç yolu vardır. Bu örnekte, kullandım [Microsoft Ajax CDN](../../../ajax/cdn/overview.md). Ayrıca buradan indirebilirsiniz [http://jquery.com/](http://jquery.com/)ve "Web API'sini" ASP.NET proje şablonu jQuery de içerir.

### <a name="getting-a-list-of-products"></a>Ürünlerin listesini alma

Ürünlerin listesini almak için bir HTTP GET isteği Gönder &quot;/api/ürünleri&quot;.

JQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) işlevi bir AJAX isteği gönderir. Yanıt için JSON nesnelerinin dizisi içerir. `done` İstek başarılı olursa çağrılan bir geri çağırma işlevini belirtiyor. Geri arama, ürün bilgilerle DOM güncelleştiriyoruz.

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a>Ürün Kimliği ile Başlarken

Kimliğe göre bir ürün almak için bir HTTP GET isteği Gönder &quot;/api/ürünler/*kimliği*&quot;, burada *kimliği* ürün kimliğidir.

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

Hala diyoruz `getJSON` AJAX isteği, ancak bu kez göndermeyi biz kimliği URI isteğinde yerleştirin. Bu istek yanıtı tek bir ürün JSON gösterimidir.

## <a name="running-the-application"></a>Uygulamayı Çalıştırma

Uygulama hata ayıklamayı başlatmak için F5 tuşuna basın. Web sayfasını aşağıdaki gibi görünmelidir:

![](tutorial-your-first-web-api/_static/image11.png)

Kimliğe göre bir ürün almak için kimliği girin ve Ara'yı tıklatın:

![](tutorial-your-first-web-api/_static/image12.png)

Geçersiz kimlik girerseniz, sunucunun bir HTTP hata döndürür:

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a>HTTP istek ve yanıt görüntülemek için F12 kullanma

Bir HTTP hizmeti ile çalışırken, HTTP isteği görmek ve iletileri istemek çok kullanışlı olabilir. Internet Explorer 9 ' F12 geliştirici araçlarını kullanarak bunu yapabilirsiniz. Internet Explorer 9 ' basın **F12** araçlarını açmak için. Tıklatın **ağ** sekmesi ve tuşuna **Başlat yakalama**. Şimdi geri dönerek basın ve web sayfası **F5** web sayfasını yeniden yüklemek için. Internet Explorer tarayıcı ile web sunucusu arasındaki HTTP trafiğini yakalar. Özet görünümü bir sayfa için tüm ağ trafiğini gösterir:

![](tutorial-your-first-web-api/_static/image14.png)

İçin göreli URI girişini bulun "API/Ürünler /". Bu girişi seçin ve tıklatın **ayrıntılı görünümüne gidin**. Ayrıntılı görünümde gövdeleri ve istek ve yanıt üst bilgileri görüntülemek için sekmeleri vardır. Örneğin, tıklatırsanız **istek üst** sekmesinde, istemci istenilen görebilirsiniz &quot;uygulama/json&quot; Accept üstbilgisindeki.

![](tutorial-your-first-web-api/_static/image15.png)

Yanıt gövdesi sekmesini tıklatırsanız, ürün listesinin nasıl JSON olarak serileştirilmiş görebilirsiniz. Diğer tarayıcılarda benzer işlevlere sahip. Başka bir yararlı bir araçtır [Fiddler](http://www.fiddler2.com/fiddler2/), bir web proxy hata ayıklama. Fiddler HTTP trafiğini görüntülemek için ve ayrıca HTTP isteklerini oluşturmak için istek HTTP üstbilgilerini üzerinde tam denetim imkanı sağlayan kullanabilirsiniz.

## <a name="see-this-app-running-on-azure"></a>Azure üzerinde çalışan bu uygulamayı bakın

Canlı web uygulaması çalışan tamamlanmış site görmek ister misiniz? Aşağıdaki düğmeyi tıklatarak, uygulama tam sürümü Azure hesabınızda dağıtabilirsiniz.

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

Bu çözüm Azure'a dağıtmak için bir Azure hesabınız olmalıdır. Bir hesap zaten yoksa, aşağıdaki seçenekler vardır:

- [Ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -krediler alırsınız, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.
- [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -MSDN aboneliğiniz size kredi verir Ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.

## <a name="next-steps"></a>Sonraki Adımlar

- POST, PUT ve silme eylemlerini destekler ve bu veritabanına yazan bir HTTP hizmeti daha eksiksiz bir örnek için bkz: [Entity Framework 6 ile Web API 2 kullanarak](../data/using-web-api-with-entity-framework/part-1.md).
- Bir HTTP hizmeti üstünde esnektir, hızlı yanıt web uygulamaları oluşturma hakkında daha fazla bilgi için bkz: [ASP.NET tek sayfa uygulaması](../../../single-page-application/index.md).
- Visual Studio web projesini Azure App Service'e dağıtma hakkında daha fazla bilgi için bkz: [Azure App Service'te bir ASP.NET web uygulaması oluşturma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
