---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: İle Web API 2 OData v3 uç noktası oluşturma | Microsoft Docs
author: MikeWasson
description: Açık Veri Protokolü (OData), web için veri erişim protokolüdür. OData veri yapısı, verileri sorgulamak ve verileri işlemek için Tekdüzen bir yol sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: bcf089003cfb4233fd2aa70cf669972de46c27b1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362596"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a>İle Web API 2 OData v3 uç noktası oluşturma
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

[Projeyi yükle](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> [Açık veri Protokolü](http://www.odata.org/) (OData) web için bir veri erişim protokolüdür. OData veri yapısı, verileri sorgulamak ve veri kümesi üzerinden CRUD işlemleri işlemek için Tekdüzen bir yol sağlar (oluşturma, okuma, güncelleştirme ve silme). OData AtomPub (XML) hem de JSON biçimlerini destekler. OData veri hakkındaki meta verileri kullanıma sunmak için bir yol da tanımlar. İstemciler, tür bilgileri ve veri kümesi için ilişkileri keşfetmek için meta verileri kullanabilir.
> 
> ASP.NET Web API OData uç noktası için bir veri kümesi oluşturmak kolaylaştırır. Uç nokta tam olarak hangi OData işlemleri destekleyen denetleyebilirsiniz. OData olmayan uç noktaları yanı sıra birden çok OData uç barındırabilirsiniz. Size, veri modeli, arka uç iş mantığı ve veri katmanı üzerinde tam denetime sahiptir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web API 2
> - OData sürüm 3
> - Entity Framework 6
> - [Fiddler'ı Web Proxy (isteğe bağlı) hata ayıklama](http://www.fiddler2.com)
> 
> Web API OData desteği eklendi [ASP.NET ve Web Araçları 2012.2 güncelleştirme](https://go.microsoft.com/fwlink/?LinkId=282650). Ancak, Bu öğretici, Visual Studio 2013'te eklenmiş olan yapı iskelesi kullanır.


Bu öğreticide, istemciler sorgulayabilir basit bir OData uç noktası oluşturur. Ayrıca bir C# istemci uç noktası için oluşturulur. Bu öğreticiyi tamamladıktan sonra sonraki öğreticiler kümesini Göster varlık ilişkileri, Eylemler dahil olmak üzere daha fazla işlevsellik ekleme ve genişletin $/ $seçin.

- [Visual Studio projesi oluşturma](#create-project)
- [Varlık modeli ekleme](#add-model)
- [Bir OData denetleyicisi Ekle](#add-controller)
- [EDM ve rota Ekle](#edm)
- [(İsteğe bağlı) veritabanının çekirdeğini oluşturma](#seed-db)
- [OData uç noktasını keşfetme](#explore)
- [OData serileştirme biçimleri](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a>Visual Studio projesi oluşturma

Bu öğreticide, temel CRUD işlemleri destekleyen OData uç noktası oluşturur. Uç nokta ürünlerin listesini tek bir kaynak açığa çıkarır. Sonraki öğreticiler daha fazla özellik ekler.

Visual Studio'yu başlatın ve seçin **yeni proje** başlangıç sayfasından. Veya **dosya** menüsünde **yeni** ardından **proje**.

İçinde **şablonları** bölmesinde **yüklü şablonlar** ve Visual C# düğümünü genişletin. Altında **Visual C#** seçin **Web**. Seçin **ASP.NET Web uygulaması** şablonu.

![](creating-an-odata-endpoint/_static/image1.png)

İçinde **yeni ASP.NET projesi** iletişim kutusunda **boş** şablonu. Altında &quot;klasörleri ekleyin ve çekirdek başvuruları... &quot;, kontrol **Web API**. **Tamam**'ı tıklatın.

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a>Varlık modeli ekleme

A *modeli* uygulamanızdaki verileri temsil eden bir nesnedir. Bu öğreticide, bir ürünü temsil eden bir model oluşturmamız gerekir. Model OData varlık türüne karşılık gelir.

Çözüm Gezgini'nde modeller klasörü sağ tıklatın. Bağlam menüsünden seçin **Ekle** seçip **sınıfı**.

![](creating-an-odata-endpoint/_static/image3.png)

İçinde **yeni Ekle** öğesi iletişim, sınıf adını &quot;ürün&quot;.

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> Kural gereği, model sınıfları modelleri klasörüne yerleştirilir. Bu kurala projelerinizi izleyin gerekmez, ancak bu öğretici için kullanacağız.


Product.cs dosyasındaki aşağıdaki sınıf tanımını ekleyin:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

ID özelliği, varlık anahtarı olacaktır. İstemciler ürün kimliğine göre sorgulayabilir. Bu alan ayrıca arka uç veritabanı birincil anahtar olacaktır.

Şimdi, projeyi derleyin. Sonraki adımda, ürün türü bulmak için yansıtma kullanan bazı Visual Studio yapı iskelesi kullanacağız.

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a>Bir OData denetleyicisi Ekle

A *denetleyicisi* HTTP isteklerini işleyen sınıftır. İçinde OData hizmeti ayarladığınız her varlık için bir ayrı denetleyicisinin tanımlarsınız. Bu öğreticide, tek bir denetleyici oluşturacağız.

Çözüm Gezgini'nde denetleyicileri klasörüne sağ tıklayın. Seçin **Ekle** seçip **denetleyicisi**.

![](creating-an-odata-endpoint/_static/image5.png)

İçinde **İskele Ekle** iletişim kutusunda &quot;Web API 2 OData denetleyici Entity Framework kullanarak Eylemler ile&quot;.

![](creating-an-odata-endpoint/_static/image6.png)

İçinde **denetleyici Ekle** iletişim kutusunda, "ProductsController" denetleyicinin adı. Seçin &quot;zaman uyumsuz denetleyici eylemlerini kullanmak&quot; onay kutusu. İçinde **modeli** aşağı açılan listesinde, ürün sınıfı seçin.

![](creating-an-odata-endpoint/_static/image7.png)

Tıklayın **yeni veri bağlamı...**  düğmesi. Veri bağlamı türü için varsayılan adı bırakın ve tıklayın **Ekle**.

![](creating-an-odata-endpoint/_static/image8.png)

Denetleyici Ekle iletişim kutusunda denetleyicisi eklemek için Ekle'ye tıklayın.

![](creating-an-odata-endpoint/_static/image9.png)

Not: bir hata iletisi alırsanız bildiren &quot;türü alınırken bir hata oluştu... &quot;, ürün sınıfı eklendikten sonra Visual Studio projesini yerleşik olduğundan emin olun. Yapı iskelesi sınıfı bulmak için yansıtma kullanır.

![](creating-an-odata-endpoint/_static/image10.png)

Yapı iskelesi iki kod dosyasını projeye ekler:

- OData uç noktasını uygulayan bir Web API denetleyicisi Products.cs tanımlar.
- ProductServiceContext.cs Entity Framework kullanarak temel alınan veritabanını sorgulamak için yöntemler sağlar.

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a>EDM ve rota Ekle

Çözüm Gezgini'nde uygulama genişletin\_başlangıç klasörü ve WebApiConfig.cs adlı dosyayı açın. Bu sınıf, Web API'si için yapılandırma kodu içerir. Bu kodu aşağıdakiyle değiştirin:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

Bu kod, iki şey yapar:

- OData uç noktasını bir varlık veri modeli (EDM) oluşturur.
- Uç nokta için bir rota ekler.

Bir EDM soyut bir veri modelidir. EDM meta verileri belgesi oluşturun ve hizmet için bir URI'leri tanımlamak için kullanılır. **ODataConventionModelBuilder** varsayılan adlandırma kuralları EDM kümesini kullanarak bir EDM oluşturur. Bu yaklaşım, en az kod gerektirir. EDM üzerinde daha fazla denetim istiyorsanız kullanabileceğiniz **ODataModelBuilder** açıkça özellikleri, anahtarları ve gezinti özellikleri ekleyerek EDM oluşturmak için sınıf.

**EntitySet** yöntemi varlık kümesi için EDM ekler:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

"Ürünler" dizesini varlık kümesinin adını tanımlar. Denetleyicinin adı, varlık kümesinin adı eşleşmelidir. Bu öğreticide, varlık kümesini "Ürünler" olarak adlandırılır ve denetleyici adlı `ProductsController`. Adlı varlık "ProductSet" kümesi, denetleyici garip gelse `ProductSetController`. Bir uç nokta birden fazla varlık kümesine sahip olabileceğini unutmayın. Çağrı **EntitySet&lt;T&gt;**  her varlık için ayarlamak ve karşılık gelen bir denetleyici tanımlayabilirsiniz.

**MapODataRoute** yöntemi OData uç noktası için bir rota ekler.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

İlk parametre, rota için kolay bir addır. Hizmetinizin istemciler bu adı görmez. Uç noktası için URI öneki ikinci parametredir. Bu kodu göz önünde bulundurulduğunda, ürünleri varlık kümesi için http:// URI'dir<em>hostname</em>  /odata/ürünleri. Uygulamanız birden fazla OData uç noktası olabilir. Her uç nokta için çağrı <strong>MapODataRoute</strong> ve benzersiz rota adı ve benzersiz bir URI öneki belirtin.

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a>(İsteğe bağlı) veritabanının çekirdeğini oluşturma

Bu adımda, bazı test verileri ile veritabanının çekirdeğini oluşturma için Entity Framework kullanır. Bu adım isteğe bağlıdır, ancak OData uç noktanızı hemen test etmenize olanak tanır.

Gelen **Araçları** menüsünde **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu girin:

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

Bu, geçiş ve Configuration.cs adlı bir kod dosyası adlı bir klasör ekler.

![](creating-an-odata-endpoint/_static/image12.png)

Bu dosyayı açın ve aşağıdaki kodu ekleyin `Configuration.Seed` yöntemi.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

Paket Yöneticisi konsolu penceresinde, aşağıdaki komutları girin:

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

Bu komutlar veritabanı oluşturur ve sonra bu kodu yürüten bir kod oluşturur.

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a>OData uç noktasını keşfetme

Bu bölümde, kullanacağız [Fiddler Web hata ayıklama proxy'si](http://www.fiddler2.com) istekleri uç noktasına göndermesi ve yanıt iletilerini inceleyin. Bu, OData uç noktası yeteneklerini anlamalarına yardımcı olur.

Visual Studio'da hata ayıklamayı başlatmak için F5 tuşuna basın. Varsayılan olarak, Visual Studio için tarayıcınızı açar `http://localhost:*port*`burada *bağlantı noktası* proje ayarlarında yapılandırılan bağlantı noktası numarasıdır.

Proje ayarlarında bağlantı noktası numarasını değiştirebilirsiniz. Çözüm Gezgini'nde projeye sağ tıklayıp seçin **özellikleri**. Özellikler penceresinde seçin **Web**. Altında bağlantı noktası numarasını girin **proje URL'si**.

### <a name="service-document"></a>Hizmet belgesi

*Hizmet belgesi* OData uç noktası için varlık kümeleri listesini içerir. Hizmet belgesi almak için kök URI'sine hizmetinin bir GET isteği gönderin.

Fiddler, kullanarak aşağıdaki URI'de girin **Oluşturucusu** sekmesi: `http://localhost:port/odata/`burada *bağlantı noktası* bağlantı noktası numarasıdır.

![](creating-an-odata-endpoint/_static/image13.png)

Tıklayın **yürütme** düğmesi. Fiddler, uygulamanız için bir HTTP GET isteği gönderir. Yanıt Web oturum listesinde görmeniz gerekir. Her şeyin çalıştığından, durum kodu 200 olacaktır.

![](creating-an-odata-endpoint/_static/image14.png)

Denetçiler sekmesinde yanıt iletisinin ayrıntıları görmek için Web oturumları listesi yanıtta çift tıklayın.

![](creating-an-odata-endpoint/_static/image15.png)

Ham HTTP yanıt iletisi, aşağıdakine benzer görünmelidir:

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

Varsayılan olarak, Web API, hizmet belgesi AtomPub biçiminde döndürür. JSON istek için HTTP isteği aşağıdaki üstbilgi ekleyin:

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

Artık HTTP yanıtının JSON yükü içerir:

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a>Hizmet meta verileri belgesi

*Hizmet meta verileri belgesi* kavramsal şema tanım dili (CSDL) adlı bir XML dili kullanarak hizmeti veri modelini açıklar. Meta veri belgesi hizmette verilerin yapısını gösterir ve istemci kodu oluşturmak için kullanılabilir.

Meta veri belgesi almak için GET isteğini göndermek `http://localhost:port/odata/$metadata`. Bu öğreticide gösterilen uç nokta için meta veriler aşağıdadır.

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a>Varlık kümesi

Ürünleri varlık kümesini almak için bir GET isteğini göndermek `http://localhost:port/odata/Products`.

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a>Varlık

Tek bir ürün almak için bir GET isteği gönderme `http://localhost:port/odata/Products(1)`, "1" Ürün Kimliği

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a>OData serileştirme biçimleri

OData çeşitli serileştirme biçimleri destekler:

- Atom Pub (XML)
- JSON "açık" (OData v3 sürümünde sunulan)
- JSON "ayrıntılı" (OData v2)

Varsayılan olarak, Web API'si AtomPubJSON "açık" biçimini kullanır. 

Accept üst bilgisi AtomPub biçimi almak için "application/atom + xml" ayarlayın. Bir örnek yanıt gövdesi şu şekildedir:

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

Atom biçimi belirgin bir dezavantajı görebilirsiniz: JSON light çok daha ayrıntılıdır. AtomPub anlayan bir istemciniz varsa, ancak istemci, biçimi JSON tercih edebilirsiniz.

Aynı varlık JSON light hali aşağıdadır:

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

JSON light biçimini OData Protokolü 3. sürümü kullanıma sunulmuştur. Geriye dönük uyumluluk için "ayrıntılı" eski JSON biçiminde bir istemcinin isteyebileceği. Ayrıntılı JSON istek için Accept üst bilgisi ayarlamak `application/json;odata=verbose`. Ayrıntılı sürüm şu şekildedir:

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

Bu biçim, önemli ölçüde gider tamamını bir oturum üzerinden ekleyebilirsiniz. yanıt gövdesi içinde daha fazla meta veri sağlar. Ayrıca, "d" adlı bir özelliğe nesne sarmalama tarafından yöneltme düzeyi ekler.

## <a name="next-steps"></a>Sonraki Adımlar

- [Varlık ilişkilerini ekleyin](working-with-entity-relations.md)
- [OData eylemleri ekleme](odata-actions.md)
- [Bir .NET istemcisinden OData hizmetine çağrı](calling-an-odata-service-from-a-net-client.md)
