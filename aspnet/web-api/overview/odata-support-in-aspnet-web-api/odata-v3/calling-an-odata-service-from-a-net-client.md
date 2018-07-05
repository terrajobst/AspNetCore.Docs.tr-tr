---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Bir .NET istemcisinden (C#) bir OData hizmeti çağırma | Microsoft Docs
author: MikeWasson
description: Bu öğretici, C# istemci uygulamasından bir OData hizmeti çağırma işlemi gösterilmektedir. Öğretici Visual Studio 2013 (çalışır Visual s... kullanılan yazılım sürümleri
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: d987e7fbe737055b3e2b690ef3e8de5ca7e2b937
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369789"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a>Bir OData hizmeti çağırmak bir .NET istemcisinden (C#)
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

[Projeyi yükle](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Bu öğretici, C# istemci uygulamasından bir OData hizmeti çağırma işlemi gösterilmektedir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013'ün](https://www.microsoft.com/visualstudio/eng/2013-downloads) (Visual Studio 2012 ile çalışır)
> - [WCF Veri Hizmetleri İstemci Kitaplığı](https://msdn.microsoft.com/library/cc668772.aspx)
> - Web API 2. (OData hizmeti örnek Web API 2 kullanılarak derlendi ancak istemci uygulaması Web API'ye bağlı değildir.)


Bu öğreticide, ben bir OData hizmetine çağrı yapan bir istemci uygulaması oluşturmada size yardımcı olacağız. Aşağıdaki varlıkların OData hizmeti sunar:

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

Aşağıdaki makaleler, Web API OData hizmeti uygulamak açıklanmaktadır. (Bu öğreticide, ancak anlamak için bunları okumak gerekmez.)

- [Web API 2 OData uç noktası oluşturma](creating-an-odata-endpoint.md)
- [Web API 2 OData varlık ilişkileri](working-with-entity-relations.md)
- [Web API 2’de OData Eylemleri](odata-actions.md)

## <a name="generate-the-service-proxy"></a>Hizmet proxy'si oluştur

İlk adım, bir hizmeti proxy'si oluşturmaktır. Hizmet proxy'si OData hizmetine erişim yöntemleri tanımlayan bir .NET sınıfıdır. Ara sunucu HTTP isteklerinin yöntemi çağrılarını çevirir.

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

OData hizmet proje Visual Studio'da açarak işleme başlayın. IIS Express'te URL'i hizmeti yerel olarak çalıştırmak için CTRL + F5 tuşlarına basın. Visual Studio atadığı bağlantı noktası numarasını da dahil olmak üzere, yerel adres unutmayın. Proxy oluşturduğunuzda bu adresi gerekir.

Ardından, Visual Studio'nun başka bir örneğini açın ve bir konsol uygulama projesi oluşturun. Konsol uygulaması OData istemci uygulamamız olacaktır. (, Ayrıca projeyi hizmet olarak aynı çözüme ekleyebilirsiniz.)

> [!NOTE]
> Konsol projesi kalan adımlara bakın.


Çözüm Gezgini'nde sağ **başvuruları** seçip **hizmet Başvurusu Ekle**.

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

İçinde **hizmet Başvurusu Ekle** iletişim kutusunda, OData hizmeti adresini yazın:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

Burada *bağlantı noktası* bağlantı noktası numarasıdır.

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

İçin **Namespace**, "ProductService" yazın. Bu seçenek, ad alanı proxy sınıfını tanımlar.

Tıklayın **Git**. Visual Studio service varlıklarda bulmak için OData meta veri belgesini okur.

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

Tıklayın **Tamam** proxy sınıfı projenize eklenecek.

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>Hizmeti Proxy sınıfı bir örneğini oluşturun

İçinde `Main` yöntemi, proxy sınıfının yeni bir örneğini şu şekilde oluşturun:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

Hizmet uygulamanızın nerede çalıştığına yeniden gerçek bağlantı noktası numarasını kullanın. Hizmetinizi dağıttığınızda, Canlı hizmeti URI'si kullanır. Proxy güncelleştirmek gerekmez.

Aşağıdaki kod, istek URI konsol penceresine yazdırır bir olay işleyicisi ekler. Bu adım gerekli değildir, ancak her sorgu için bir URI'leri görmek ilginç.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>Hizmet sorgulama

Aşağıdaki kod, OData hizmeti ürünlerin listesini alır.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

HTTP isteği göndermek veya yanıtları ayrıştırmak için herhangi bir kod yazmanız gerekmez dikkat edin. Numaralandırma, proxy sınıfı bu otomatik olarak yapar `Container.Products` koleksiyonda **foreach** döngü.

Uygulamayı çalıştırdığınızda, çıktı aşağıdaki gibi görünmelidir:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

Kimliğe göre varlık almak için kullanmak bir `where` yan tümcesi.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

Bu konunun geri kalanı için ı tamamını gösterilmez `Main` işlev, yalnızca hizmeti çağırmak için gereken kodu.

## <a name="apply-query-options"></a>Geçerli sorgu seçenekleri

OData tanımlar [sorgu seçenekleri](../supporting-odata-query-options.md) kullanılabilen filtre, sıralama, sayfa verileri ve benzeri. Hizmet ara sunucu, çeşitli LINQ ifadelerini kullanarak bu seçenekleri uygulayabilirsiniz.

Bu bölümde, ı kısa örnekler göstereceğiz. Daha fazla bilgi için Ek Yardım konusuna [LINQ konuları (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) MSDN'de.

### <a name="filtering-filter"></a>Filtreleme ($filter)

Filtre uygulamak için kullanmak bir `where` yan tümcesi. Aşağıdaki örnek filtreler ürün kategorisine göre.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

Bu kod, aşağıdaki OData sorgu için karşılık gelir.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

Proxy dönüştürür bildirimi `where` yan tümcesine bir OData `$filter` ifade.

### <a name="sorting-orderby"></a>Sıralama ($orderby)

Sıralamak için kullanmak bir `orderby` yan tümcesi. Aşağıdaki örnek, yüksekten en düşüğe fiyata göre sıralar.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

Karşılık gelen OData isteği aşağıda verilmiştir.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>İstemci tarafı sayfalama ($skip ve $top)

Büyük varlık kümeleri için istemci sonuçları sayısını sınırlamak isteyebilirsiniz. Örneğin, bir istemci aynı anda 10 girişleri gösterebilir. Bu adlandırılır *istemci-tarafı sayfalama*. (Ayrıca [sunucu tarafı sayfalama](../supporting-odata-query-options.md#server-paging), burada sunucu sonuçları sayısını sınırlar.) İstemci tarafı sayfalama gerçekleştirmek için LINQ kullanma **atla** ve **ele** yöntemleri. Aşağıdaki örnek, ilk 40 sonuçları atlar ve sonraki 10 alır.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

Karşılık gelen OData isteği şu şekildedir:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>($Select) seçme ve genişletme ($expand)

İlişkili varlıkları içermesi için kullanın `DataServiceQuery<t>.Expand` yöntemi. Örneğin, dahil etmek için `Supplier` her `Product`:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

Karşılık gelen OData isteği şu şekildedir:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

Yanıtın şeklini değiştirmek için LINQ kullanma **seçin** yan tümcesi. Aşağıdaki örnek, yalnızca başka hiçbir özellik ile her ürün adını alır.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

Karşılık gelen OData isteği şu şekildedir:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Bir select yan tümcesi, ilgili varlıklar içerebilir. Bu durumda, çağırmayın **genişletme**; bu durumda, otomatik olarak içerir genişletme proxy. Aşağıdaki örnek, her ürünün Tedarikçi ve adını alır.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

Karşılık gelen OData isteği aşağıda verilmiştir. İçerdiği bildirimi **$expand** seçeneği.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

$Select ve $expand hakkında daha fazla bilgi için genişletme, bkz: [$select kullanarak $expand ve Web API 2 $value](../using-select-expand-and-value.md).

## <a name="add-a-new-entity"></a>Yeni varlık Ekle

Bir varlık kümesi için yeni bir varlık eklemek için çağrı `AddToEntitySet`burada *EntitySet* varlık kümesinin adı. Örneğin, `AddToProducts` yeni ekler `Product` için `Products` varlık kümesi. Ara sunucu oluşturduğunuzda, WCF veri hizmetleri otomatik olarak bu türü kesin belirlenmiş oluşturur **AddTo** yöntemleri.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

İki varlık arasında bir bağlantı eklemek için **AddLink** ve **SetLink** yöntemleri. Aşağıdaki kod, yeni bir tedarikçi ve yeni bir ürün ekler ve ardından aralarındaki bağlantıları oluşturur.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

Kullanım **AddLink** gezinme özelliği, bir koleksiyon olduğunda. Bu örnekte, bir ürün ekliyoruz `Products` sağlayıcı koleksiyonu.

Kullanım **SetLink** gezinti özelliği tek bir varlık olduğunda. Biz bu örnekte, ayarladığınız `Supplier` ürün özelliği.

## <a name="update--patch"></a>Güncelleştirme / düzeltme eki

Bir varlığı güncelleştirmek için çağrı **UpdateObject** yöntemi.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

Güncelleştirme yapılmadan çağırdığınızda **SaveChanges**. Varsayılan olarak, WCF, bir HTTP birleştirme isteği gönderir. **PatchOnUpdate** seçeneği, bunun yerine bir HTTP PATCH göndermek için WCF söyler.

> [!NOTE]
> Neden birleştirme düzeltme eki? Özgün HTTP 1.1 belirtimini ([RCF 2616](http://tools.ietf.org/html/rfc2616)) herhangi bir HTTP yöntemi "kısmi güncelleştirme" semantiğine sahip tanımlamıyor. Kısmi güncelleştirmeleri desteklemek için OData belirtiminden birleştirme yöntemi olarak tanımlanır. 2010'da, [RFC 5789](http://tools.ietf.org/html/rfc5789) kısmi güncelleştirmeler için PATCH yöntemini tanımlanmış. Bu geçmiş bazıları edinebilirsiniz [blog gönderisi](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) WCF Veri Hizmetleri blogunda. Bugün, düzeltme eki üzerinden birleştirme tercih edilir. Web APİ'si yapı iskelesini tarafından oluşturulan OData denetleyicisi her iki yöntemi destekler.


Tüm varlık (PUT semantiği) değiştirmek istiyorsanız, belirtin **ReplaceOnUpdate** seçeneği. Bu, bir HTTP PUT isteği göndermek WCF neden olur.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>Bir varlığı silme

Bir varlığı silmek için çağrı **NesneSil**.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>Bir OData eylemini çağırma

Odata'da, [eylemleri](odata-actions.md) varlıkları CRUD işlemleri olarak kolayca tanımlanmayan bir sunucu tarafı davranışları eklemek için bir yol sağlar.

OData meta veri belgesi eylemleri açıklar ancak proxy sınıfı kesin türü belirtilmiş yöntemler için bunları oluşturmaz. Genel kullanarak bir OData eylemini yine de çağırabilirsiniz **yürütme** yöntemi. Ancak, parametreler ve dönüş değeri veri türlerini bilmeniz gerekir.

Örneğin, `RateProduct` parametre türü "derecesi" adlı bir eylemde `Int32` ve döndüren bir `double`. Aşağıdaki kod bu eylemi çağırmak nasıl gösterir.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

Daha fazla bilgi için[hizmet işlemlerini çağırma ve eylemleri](https://msdn.microsoft.com/library/hh230677.aspx).

Bir seçenektir genişletmek için **kapsayıcı** eylemi çağırır kesin türü belirtilmiş bir yöntem sağlar sınıfını:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
