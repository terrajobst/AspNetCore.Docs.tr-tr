---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Web API 2 OData v3 uç noktası oluşturma | Microsoft Docs
author: MikeWasson
description: Açık Veri Protokolü (OData), web için veri erişim protokolüdür. OData veri yapısı, verileri sorgulamak ve verileri işlemek için Tekdüzen bir yol sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 227faacd3f42731e08a4cd2b71075776309961b6
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "30874635"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a>Web API 2 OData v3 uç noktası oluşturma
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> [Açık veri Protokolü](http://www.odata.org/) (OData) web için bir veri erişim protokolüdür. OData veri yapısı, verileri sorgulamak ve CRUD işlemleri üzerinden veri kümesi işlemek için Tekdüzen bir yol sağlar (oluşturma, okuma, güncelleştirme ve silme). OData AtomPub (XML) ve JSON biçimleri destekler. OData meta veriler hakkında kullanıma sunmak için bir yol da tanımlar. İstemciler, veri kümesi için ilişkileri ve tür bilgileri bulmak için meta verileri kullanabilir.
> 
> ASP.NET Web API veri kümesi için bir OData uç nokta oluşturmak kolay hale getirir. Uç nokta tam olarak hangi OData işlemlerini destekleyen kontrol edebilirsiniz. Birden fazla OData uç noktası, OData olmayan uç noktaları yanında barındırabilir. Veri modeli, arka uç iş mantığı ve veri katmanı üzerinde tam denetime sahiptir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web API 2
> - OData sürüm 3
> - Entity Framework 6
> - [Fiddler Web Proxy (isteğe bağlı) hata ayıklama](http://www.fiddler2.com)
> 
> Web API OData desteği eklendi [ASP.NET ve Web Araçları 2012.2 güncelleştirmesi](https://go.microsoft.com/fwlink/?LinkId=282650). Ancak, bu öğreticide Visual Studio 2013'te eklendi yapı iskelesi kullanır.


Bu öğreticide, istemciler sorgulayabilir basit bir OData uç noktası oluşturur. Ayrıca bir C# istemci uç noktası için oluşturulur. Bu öğreticiyi tamamladıktan sonra sonraki öğreticileri kümesini Göster varlık ilişkileriyle, Eylemler, dahil olmak üzere daha fazla işlevsellik ekleme ve genişletin $/ $seçin.

- [Visual Studio projesi oluşturma](#create-project)
- [Bir varlık modeli ekleme](#add-model)
- [Bir OData denetleyicisi ekleme](#add-controller)
- [EDM ve rota ekleme](#edm)
- [Çekirdek veritabanı (isteğe bağlı)](#seed-db)
- [OData uç noktasını keşfetme](#explore)
- [OData serileştirme biçimleri](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a>Visual Studio projesi oluşturma

Bu öğreticide, temel CRUD işlemleri destekleyen OData uç noktası oluşturur. Uç nokta ürünlerin listesini tek bir kaynak açığa çıkarır. Sonraki öğreticileri daha fazla özellik ekler.

Visual Studio'yu başlatın ve seçin **yeni proje** başlangıç sayfasından. Veya **dosya** menüsünde, select **yeni** ve ardından **proje**.

İçinde **şablonları** bölmesinde, **yüklü şablonlar** ve Visual C# düğümünü genişletin. Altında **Visual C#** seçin **Web**. Seçin **ASP.NET Web uygulaması** şablonu.

![](creating-an-odata-endpoint/_static/image1.png)

İçinde **yeni ASP.NET projesi** iletişim kutusunda **boş** şablonu. Altında &quot;klasörler ekleme ve çekirdek başvurular için... &quot;, denetleme **Web API**. **Tamam**'ı tıklatın.

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a>Bir varlık modeli ekleme

A *modeli* uygulamanızdaki verileri temsil eden bir nesnedir. Bu öğreticide, bir ürün temsil eden bir modeli gerekir. Model bizim OData varlık türüne karşılık gelir.

Çözüm Gezgini'nde modeller klasörü sağ tıklatın. Bağlam menüsünden seçin **Ekle** seçip **sınıfı**.

![](creating-an-odata-endpoint/_static/image3.png)

İçinde **yeni Ekle** öğesi iletişim, sınıf adını &quot;ürün&quot;.

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> Kurala göre modeli sınıfları modelleri klasörüne yerleştirilir. Bu kural, kendi projelerine izleyin gerekmez, ancak bu öğretici için kullanacağız.


Product.cs dosyasındaki şu sınıf tanımını ekleyin:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

ID özelliği, varlık anahtarı olacaktır. İstemcileri ürünleri kimliğe göre sorgulama yapabilirsiniz Bu alan, aynı zamanda arka uç veritabanı birincil anahtarda olacaktır.

Projeyi şimdi oluşturun. Sonraki adımda, yansıma ürün türü bulmak için kullandığı bazı Visual Studio yapı iskelesi kullanacağız.

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a>Bir OData denetleyicisi ekleme

A *denetleyicisi* HTTP isteklerini işleyen sınıftır. Her varlık size OData hizmeti kümesi için ayrı bir denetleyici tanımlayın. Bu öğreticide, tek bir denetleyici oluşturacağız.

Çözüm Gezgini'nde denetleyicileri klasörü sağ tıklatın. Seçin **Ekle** ve ardından **denetleyicisi**.

![](creating-an-odata-endpoint/_static/image5.png)

İçinde **İskele Ekle** iletişim kutusunda &quot;Web API 2 OData denetleyici Entity Framework kullanarak Eylemler ile&quot;.

![](creating-an-odata-endpoint/_static/image6.png)

İçinde **denetleyici Ekle** iletişim kutusunda, adı "ProductsController" denetleyici. Seçin &quot;zaman uyumsuz denetleyici eylemlerini kullanmak&quot; onay kutusu. İçinde **modeli** aşağı açılan listesinde, ürün sınıfı seçin.

![](creating-an-odata-endpoint/_static/image7.png)

Tıklatın **yeni veri bağlamı...**  düğmesi. Veri içerik türü için varsayılan adı bırakın ve tıklayın **Ekle**.

![](creating-an-odata-endpoint/_static/image8.png)

Denetleyicisi eklemek için denetleyici Ekle iletişim kutusunda Ekle'yi tıklatın.

![](creating-an-odata-endpoint/_static/image9.png)

Not: bir hata iletisi alırsanız bildiren &quot;türü alınırken bir hata oluştu... &quot;, ürün sınıfı eklendikten sonra Visual Studio projesi yerleşik emin olun. Yapı iskelesi yansıma olan sınıfı bulmak için kullanır.

![](creating-an-odata-endpoint/_static/image10.png)

Yapı iskelesi iki kod dosyaları projeye ekler:

- OData uç noktasını uygulayan Web API denetleyicisi Products.cs tanımlar.
- ProductServiceContext.cs Entity Framework kullanarak temel veritabanını sorgulamak için yöntemler sağlar.

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a>EDM ve rota ekleme

Çözüm Gezgini'nde, uygulama genişletin\_başlangıç klasörü ve WebApiConfig.cs adlı dosyayı açın. Bu sınıf için Web API yapılandırma kodunu içerir. Bu kodu aşağıdakilerle değiştirin:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

Bu kod iki işlemi yapar:

- OData uç noktası için varlık veri modeli (EDM) oluşturur.
- Uç noktası için bir yol ekler.

Bir EDM verilerin soyut bir modelidir. EDM meta veri belgesi oluşturmak ve hizmet için URI tanımlamak için kullanılır. **ODataConventionModelBuilder** varsayılan adlandırma kuralları EDM kümesi kullanarak bir EDM oluşturur. Bu yaklaşım, en az kod gerektirir. EDM üzerinde daha fazla denetim isterseniz, kullanabileceğiniz **ODataModelBuilder** açıkça özellikleri, anahtarları ve gezinti özellikleri ekleyerek EDM oluşturmasını sağlar.

**EntitySet** yöntemi, bir varlık için EDM kümesine ekler:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

"Ürünler" dizesini varlık kümesinin adını tanımlar. Denetleyicinin adını varlık kümesinin adı eşleşmelidir. Bu öğreticide, varlık kümesini "Ürünler" olarak adlandırılır ve denetleyici adlı `ProductsController`. Adlı "ProductSet" varlık kümesi, denetleyici adı `ProductSetController`. Bir uç nokta birden çok varlık kümesine sahip olduğunu unutmayın. Çağrı **EntitySet&lt;T&gt;**  her varlık için ayarlamak ve karşılık gelen bir denetleyici tanımlayın.

**MapODataRoute** yöntemi OData uç noktası için bir yol ekler.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

İlk parametre, yol için bir kolay addır. Hizmetinizin istemciler bu adı görmezsiniz. İkinci parametre uç noktası için URI öneki ' dir. Bu kod verildiğinde, ürünleri varlık kümesi için http:// URI'dir<em>ana bilgisayar adı</em>  /odata/ürünler. Uygulamanız birden fazla OData uç noktası olabilir. Her uç noktası için çağrı <strong>MapODataRoute</strong> ve benzersiz rota adı ve benzersiz bir URI öneki belirtin.

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a>Çekirdek veritabanı (isteğe bağlı)

Bu adımda, bazı test verilerini ile veritabanı oluşturmak için Entity Framework kullanır. Bu adım isteğe bağlıdır, ancak OData uç noktanızı hemen test etmenizi sağlar.

Gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

Geçişler ve Configuration.cs adlı bir kod dosyası adlı bir klasör ekler.

![](creating-an-odata-endpoint/_static/image12.png)

Bu dosyayı açın ve aşağıdaki kodu ekleyin `Configuration.Seed` yöntemi.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

Paket Yöneticisi konsol penceresinde aşağıdaki komutları girin:

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

Bu komutlar veritabanını oluşturur ve ardından bu kodu yürütür kodu oluşturur.

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a>OData uç noktasını keşfetme

Bu bölümde, kullanacağız [Fiddler Web hata ayıklama Proxy](http://www.fiddler2.com) uç noktasına istekleri göndermek ve yanıt iletilerini inceleyin. Bu, bir OData uç noktası özelliklerini anlamanıza yardımcı olur.

Visual Studio'da hata ayıklamayı başlatmak için F5 tuşuna basın. Varsayılan olarak, Visual Studio için tarayıcınızın açılır `http://localhost:*port*`, burada *bağlantı noktası* proje ayarlarında yapılandırılan bağlantı noktası numarasıdır.

Bağlantı noktası numarası proje ayarları değiştirebilirsiniz. Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **özellikleri**. Özellikler penceresinde seçin **Web**. Altında bağlantı noktası numarası girin **proje URL'sini**.

### <a name="service-document"></a>Hizmet belgesi

*Hizmet belgesini* OData uç noktası için varlık kümeleri listesini içerir. Hizmet belgesini almak için hizmet URI'sini köküne GET isteği gönderin.

Fiddler, kullanarak girin aşağıdaki URI'de **Oluşturucu** sekmesi: `http://localhost:port/odata/`, burada *bağlantı noktası* bağlantı noktası numarasıdır.

![](creating-an-odata-endpoint/_static/image13.png)

Tıklatın **yürütme** düğmesi. Fiddler uygulamanız için bir HTTP GET isteği gönderir. Yanıt Web oturum listesinde görmeniz gerekir. Her şeyi çalışıyorsa, durum kodu 200 olacaktır.

![](creating-an-odata-endpoint/_static/image14.png)

Denetçiler sekmesindeki yanıt iletisi ayrıntılarını görmek için Web oturumlarının listesini yanıtta çift tıklayın.

![](creating-an-odata-endpoint/_static/image15.png)

Ham HTTP yanıt iletisi aşağıdakine benzer görünmelidir:

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

Varsayılan olarak, Web API hizmet belgesini AtomPub biçiminde döndürür. JSON İstek HTTP isteği aşağıdaki üstbilgi ekleyin:

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

Artık HTTP yanıtının JSON yükü içerir:

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a>Hizmet meta veri belgesi

*Hizmeti meta veri belgesi* kavramsal şema tanım dili (CSDL) adlı bir XML dili kullanarak hizmeti veri modelini açıklar. Meta veri belgesi hizmetinde verilerin yapısını gösterir ve istemci kodu oluşturmak için kullanılabilir.

Meta veri belgesi almak için bir GET isteği Gönder `http://localhost:port/odata/$metadata`. Bu öğreticide gösterilen uç nokta için meta verileri aşağıda verilmiştir.

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a>Varlık kümesi

Ürünler varlık kümesini almak için bir GET isteği Gönder `http://localhost:port/odata/Products`.

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a>Varlık

Ayrı bir ürün almak için bir GET isteği Gönder `http://localhost:port/odata/Products(1)`, "1" Ürün Kimliği

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a>OData serileştirme biçimleri

OData birkaç seri hale getirme biçimi destekler:

- Atom Pub (XML)
- JSON "ışık (OData v3 sunulan)"
- JSON "ayrıntılı" (OData v2)

Varsayılan olarak, Web API AtomPubJSON "açık" biçimini kullanır. 

Accept üstbilgisi AtomPub biçimi almak için "application/atom + xml için" ayarlayın. Bir örnek yanıt gövdesi şöyledir:

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

Atom biçimi belirgin bir dezavantajı görebilirsiniz: JSON light çok daha ayrıntılıdır. AtomPub özelliğini algılayan bir istemciniz varsa, ancak, istemci bu biçimi JSON tercih edebilirsiniz.

Aynı varlık JSON light sürümü şöyledir:

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

JSON light biçimi sürüm OData Protokolü 3 sunulmuştur. Geriye dönük uyumluluk için bir istemci eski "ayrıntılı" JSON biçimi isteyebilir. Ayrıntılı JSON istek için Accept üstbilgisi ayarlamak `application/json;odata=verbose`. Ayrıntılı sürüm şöyledir:

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

Bu biçim önemli ölçüde gider tüm oturumu üzerinden ekleyebilirsiniz yanıt gövdesi içinde daha fazla meta veri iletir. Ayrıca, "d" adlı bir özellik nesne kaydırma tarafından yöneltme düzeyini yükseltir.

## <a name="next-steps"></a>Sonraki Adımlar

- [Varlık İlişkileriyle ekleme](working-with-entity-relations.md)
- [OData eylem ekleyin](odata-actions.md)
- [OData hizmeti bir .NET istemciden çağırın](calling-an-odata-service-from-a-net-client.md)
