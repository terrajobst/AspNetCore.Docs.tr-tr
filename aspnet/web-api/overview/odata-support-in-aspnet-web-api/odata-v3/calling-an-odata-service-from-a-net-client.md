---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: "Bir .NET İstemci'den (C#) bir OData hizmeti çağırma | Microsoft Docs"
author: MikeWasson
description: "Bu öğretici, C# istemci uygulamasından OData hizmetine çağrı gösterilmektedir. Eğitmen Visual Studio 2013 (Visual S. çalışır.. kullanılan yazılım sürümleri"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 497102cfa98680f2156a56ff9e36d84b7c820020
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="calling-an-odata-service-from-a-net-client-c"></a>Bir OData hizmeti çağrılırken bir .NET İstemci'den (C#)
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Bu öğretici, C# istemci uygulamasından OData hizmetine çağrı gösterilmektedir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (Visual Studio 2012 ile çalışır)
> - [WCF Veri Hizmetleri İstemci Kitaplığı](https://msdn.microsoft.com/library/cc668772.aspx)
> - Web API 2. (OData hizmeti örnek Web API 2 kullanılarak oluşturulur, ancak istemci uygulamasının Web API bağlı değildir.)


Bu öğreticide, ı OData hizmetine çağıran bir istemci uygulaması oluşturmada size yol. OData hizmeti aşağıdaki varlıklar sunar:

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

Aşağıdaki makaleler Web API'de OData hizmeti uygulama açıklar. (Bu öğretici, ancak anlamak için bunları okuyun gerekmez.)

- [Web API 2 OData uç noktası oluşturma](creating-an-odata-endpoint.md)
- [Web API 2 OData varlık ilişkileri](working-with-entity-relations.md)
- [Web API 2’de OData Eylemleri](odata-actions.md)

## <a name="generate-the-service-proxy"></a>Hizmet proxy'si oluştur

İlk adım, bir hizmeti proxy'si oluşturmaktır. Hizmet proxy'si OData hizmetine erişmek için yöntemler tanımlayan bir .NET sınıfıdır. Proxy HTTP istek yöntemi çağrılarını çevirir.

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

Visual Studio'da OData hizmeti projesi açarak başlayın. IIS Express'te hizmeti yerel olarak çalıştırmak için CTRL + F5 tuşuna basın. Visual Studio atar bağlantı noktası numarasını da dahil olmak üzere yerel adres unutmayın. Proxy oluşturduğunuzda bu adresi gerekir.

Ardından, Visual Studio başka bir örneği açın ve konsol uygulama projesi oluşturun. Konsol uygulaması OData istemci uygulamamız olacaktır. (Ayrıca projenin hizmet olarak aynı çözüme ekleyebileceğiniz.)

> [!NOTE]
> Geri kalan adımları konsol projesi bakın.


Çözüm Gezgini'nde sağ **başvuruları** seçip **hizmet Başvurusu Ekle**.

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

İçinde **hizmet Başvurusu Ekle** iletişim kutusunda, OData hizmeti adresini yazın:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

Burada *bağlantı noktası* bağlantı noktası numarasıdır.

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

İçin **Namespace**, "ProductService" yazın. Bu seçenek proxy sınıfının ad alanını tanımlar.

tıklatın **Git**. Visual Studio hizmet varlıkları bulmak için OData meta veri belgesini okur.

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

Tıklatın **Tamam** proxy sınıfı projenize eklemek için.

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>Hizmeti Proxy sınıfı örneği oluşturun

İçinde `Main` yöntemi, proxy sınıfının yeni bir örneğini gibi oluşturun:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

Yeniden hizmetinizi çalıştığı gerçek bağlantı noktası numarasını kullanın. Hizmetinizi dağıttığınızda, Canlı hizmet URI'sini kullanır. Proxy güncelleştirme gerek yoktur.

Aşağıdaki kod, istek URI konsol penceresine yazdırır bir olay işleyicisi ekler. Bu adım gerekli değildir, ancak her sorgu için URI görmek ilginç olacaktır.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>Hizmet sorgulama

Aşağıdaki kod OData hizmetinden ürünlerin listesini alır.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

HTTP isteği göndermek veya yanıtı ayrıştırılamadı için herhangi bir kodun yazılması gerekmez dikkat edin. Ne zaman, listeleme proxy sınıfı bu otomatik olarak mu `Container.Products` koleksiyonunda **foreach** döngü.

Uygulamayı çalıştırdığınızda, çıktı aşağıdaki gibi görünmelidir:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

Bir varlığı Kimliğe göre almak için bir `where` yan tümcesi.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

Bu konunun geri kalanı için t tüm göstermeyecektir `Main` işlev, hizmeti çağırmak için gerekli olan kodu.

## <a name="apply-query-options"></a>Sorgu Seçenekleri Uygula

OData tanımlar [sorgu seçenekleri](../supporting-odata-query-options.md) kullanılabilecek filtre, sıralama, sayfa verilerine ve benzeri. Hizmet proxy'si çeşitli LINQ ifadeleri kullanarak bu seçenekleri uygulayabilirsiniz.

Bu bölümde, ı kısa örnekler göstereceğiz. Daha fazla ayrıntı için Ek Yardım konusuna [LINQ konuları (WCF Veri Hizmetleri)](https://msdn.microsoft.com/library/ee622463.aspx) MSDN'de.

### <a name="filtering-filter"></a>Filtreleme ($filter)

Filtrelemek için kullanmak bir `where` yan tümcesi. Aşağıdaki örnek filtreleri ürün kategorisine göre.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

Bu kod, aşağıdaki OData sorgu karşılık gelir.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

Proxy dönüştürür bildirimi `where` yan tümcesine bir OData `$filter` ifade.

### <a name="sorting-orderby"></a>Sıralama ($orderby)

Sıralamak için kullanmak bir `orderby` yan tümcesi. Aşağıdaki örnek yüksekten en düşüğe fiyat göre sıralar.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

Burada, karşılık gelen OData isteği verilmiştir.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>İstemci-tarafı disk belleği ($skip ve $top)

Büyük varlık kümeleri için istemci sonuç sayısını sınırlamak isteyebilirsiniz. Örneğin, bir istemci bir kerede 10 girişleri gösterebilir. Bu adlı *istemci-tarafı disk belleği*. (Ayrıca [sunucu tarafı disk belleği](../supporting-odata-query-options.md#server-paging), burada sunucu sonuçları sayısını sınırlar.) İstemci-tarafı disk belleği gerçekleştirmek için LINQ kullanın **atla** ve **ele** yöntemleri. Aşağıdaki örnek ilk 40 sonuçları atlar ve sonraki 10 alır.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

Karşılık gelen OData isteği şöyledir:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>($Select) seçin ve genişletin ($expand)

İlgili varlık eklemek için kullanın `DataServiceQuery<t>.Expand` yöntemi. Örneğin, dahil etmek için `Supplier` her `Product`:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

Karşılık gelen OData isteği şöyledir:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

Yanıt şeklini değiştirmek için LINQ kullanın **seçin** yan tümcesi. Aşağıdaki örnekte, yalnızca başka hiçbir özellik ile her ürünün adını alır.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

Karşılık gelen OData isteği şöyledir:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Select yan tümcesi ilgili varlık içerebilir. Bu durumda, çağırmayın **genişletme**; bu durumda, otomatik olarak içeren genişletme proxy. Aşağıdaki örnekte, her ürünün sağlayıcısına ve adını alır.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

Burada, karşılık gelen OData isteği verilmiştir. İçerdiği bildirimi **$expand** seçeneği.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

$Select ve $expand hakkında daha fazla bilgi için ' ni genişletin, bkz: [$select kullanarak $genişletin ve Web API 2'deki $value](../using-select-expand-and-value.md).

## <a name="add-a-new-entity"></a>Yeni bir varlık ekleme

Bir varlık kümesine yeni bir varlık eklemek için arama `AddToEntitySet`, burada *EntitySet* varlık kümesinin adı. Örneğin, `AddToProducts` yeni ekler `Product` için `Products` varlık kümesi. Proxy oluşturduğunuzda, WCF veri hizmetleri otomatik olarak bu kesin türü belirtilmiş oluşturur **AddTo** yöntemleri.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

İki varlık arasında bir bağlantı eklemek için kullanın **AddLink** ve **SetLink** yöntemleri. Aşağıdaki kod yeni bir sağlayıcının ve yeni bir ürün ekler ve ardından bunları arasındaki bağlantıları oluşturur.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

Kullanım **AddLink** gezinti özelliği bir koleksiyon olduğunda. Bu örnekte, bir ürün ekliyoruz `Products` sağlayıcı koleksiyonu.

Kullanım **SetLink** gezinti özelliği tek bir varlık olduğunda. Bu örnekte, biz ayarlıyorsanız `Supplier` ürün özelliği.

## <a name="update--patch"></a>Güncelleştirme / düzeltme eki

Bir varlığı güncelleştirmek için çağrı **UpdateObject** yöntemi.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

Çağırdığınızda güncelleştirme yapılmadan **SaveChanges**. Varsayılan olarak, WCF HTTP birleştirme isteği gönderir. **PatchOnUpdate** seçenek bir HTTP PATCH yerine göndermek için WCF söyler.

> [!NOTE]
> Neden karşı birleştirme düzeltme eki? Özgün HTTP 1.1 belirtimini ([RCF 2616](http://tools.ietf.org/html/rfc2616)) "kısmi güncelleştirme" semantiği ile herhangi bir HTTP yöntemini tanımlamıyor. Kısmi güncelleştirmeler desteklemek için birleştirme yöntemi OData belirtimi tanımlı. 2010'da, [RFC 5789](http://tools.ietf.org/html/rfc5789) kısmi güncelleştirmeler için düzeltme eki yöntemi tanımlı. Bu geçmiş bazıları okuyabilirsiniz [blog gönderisi](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) WCF Veri Hizmetleri blogunda. Bugün, düzeltme eki üzerinden birleştirme tercih edilir. Web API yapı iskelesi tarafından oluşturulan OData denetleyicisi her iki yöntemi destekler.


Tüm varlık (PUT semantiği) değiştirmek istiyorsanız, belirtin **ReplaceOnUpdate** seçeneği. Bu, bir HTTP PUT isteği göndermek WCF neden olur.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>Bir varlığı silme

Bir varlığı silmek için çağrı **DeleteObject**.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>Bir OData eylemini çağırma

Odata'da, [Eylemler](odata-actions.md) kolayca CRUD işlemleri varlıklar üzerinde olarak tanımlanmamış olan sunucu tarafı davranışları eklemek için bir yoldur.

OData meta veri belgesi eylemleri açıklansa da, proxy sınıfı herhangi kesin türü belirtilmiş bir yöntem bunları oluşturmaz. Genel kullanarak bir OData eylemini hala çağırabileceği **yürütme** yöntemi. Ancak, parametreler ve dönüş değeri veri türlerini bilmeniz gerekir.

Örneğin, `RateProduct` eylemde türü "derecesi" adlı parametre `Int32` ve döndüren bir `double`. Aşağıdaki kod bu eylemi çağırmak nasıl gösterir.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

Daha fazla bilgi için bkz:[hizmet işlemlerini çağırma ve eylemleri](https://msdn.microsoft.com/library/hh230677.aspx).

Bir seçenektir genişletmek için **kapsayıcı** sınıfı eylemi çağırır kesin türü belirtilmiş bir yöntem sunar:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
