---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: "Varlık İlişkileriyle ile Web API 2 OData v3 destekleme | Microsoft Docs"
author: MikeWasson
description: "Varlıkları arasındaki ilişkileri çoğu veri kümelerini tanımlayın: müşterilerin sahip siparişleri; Books yazarlar; yine de sahip istiyor musunuz? Ürünler Üreticiler vardır. OData kullanarak, istemciler üzerinde gidebilirsiniz..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: dec7e10e59cc2441c967afe062df227b105106a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>Varlık İlişkileriyle ile Web API 2 OData v3 destekleme
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Varlıkları arasındaki ilişkileri çoğu veri kümelerini tanımlayın: müşterilerin sahip siparişleri; Books yazarlar; yine de sahip istiyor musunuz? Ürünler Üreticiler vardır. OData'ı kullanarak istemcileri varlık ilişkileriyle gidin. Bir ürün verildiğinde, tedarikçi bulabilirsiniz. Ayrıca, oluşturabilir veya ilişkileri kaldırın. Örneğin, bir ürün için tedarikçiye ayarlayabilirsiniz.
> 
> Bu öğreticide, ASP.NET Web API işlemlerini desteklemek gösterilmiştir. Öğretici öğreticiyi derlemeler [ile Web API 2 OData v3 uç noktası oluşturma](creating-an-odata-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - Web API 2
> - OData sürüm 3
> - Varlık Çerçevesi 6


## <a name="add-a-supplier-entity"></a>Bir sağlayıcı varlık ekleme

İlk olarak biz bizim OData akışı için yeni bir varlık türü eklemeniz gerekir. Ekleyeceğiz bir `Supplier` sınıfı.

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

Bu sınıf, bir dize için varlık anahtarı kullanır. Uygulamada, bir tamsayı anahtarı kullanmaktan daha az görülen olabilir. Ancak OData tamsayılar yanı sıra anahtar diğer türleri nasıl işlediğini görmesini değerindedir.

Ardından, bir ilişki ekleyerek oluşturacağız bir `Supplier` özelliğine `Product` sınıfı:

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

Yeni bir ekleme **DbSet** için `ProductServiceContext` Entity Framework içerecek şekilde, sınıfının `Supplier` veritabanındaki tablo.

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

WebApiConfig.cs içinde "Üreticiler" varlığı için EDM modeline ekleyin:

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>Gezinti özellikleri

Sağlayıcı bir ürün almak için istemci bir GET isteği gönderir:

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

Burada "Sağlayıcı" bir gezinti özelliği açıktır `Product` türü. Bu durumda, `Supplier` başvuruyor tek bir öğe, ancak bir gezinti özelliği bir koleksiyon (bir çok veya çok-çok ilişkisi) de geri dönebilirsiniz.

Bu istek desteklemek için aşağıdaki yöntemi ekleyin `ProductsController` sınıfı:

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

*Anahtar* parametresi, ürün anahtarıdır. Bu durumda, ilgili varlığın &#8212;yöntemi döndürür bir `Supplier` örneği. Yöntem adı parametre adı hem de önemli olması. Genel olarak, gezinme özelliğini "X" olarak adlandırılmışsa "GetX" adlı bir yöntem eklemeniz gerekir. Yöntem adlı bir parametre almalıdır "*anahtar*" üst öğenin anahtarın veri türü ile eşleşen.

Eklenmesi önemlidir **[FromOdataUri]** özniteliğini *anahtar* parametresi. Bu öznitelik, istek URI'si anahtarından ayrıştırırken OData sözdizimi kurallarına kullanmak için Web API söyler.

## <a name="creating-and-deleting-links"></a>Oluşturma ve bağlantıları silme

OData iki varlık arasında oluşturma veya kaldırma ilişkileri destekler. OData terminolojisinde ilişki "." bağlantıdır Her bir bağlantının form ile bir URI'ya sahip *varlık*/$links /*varlık*. Örneğin, ürün bağlantıdan tedarikçiye şöyle görünür:

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

Yeni bir bağlantı oluşturmak için istemci bağlantısını URI bir POST isteği gönderir. İstek gövdesini hedef varlığın URI'si değil. Örneğin, "CTSO" anahtarına sahip bir sağlayıcı yok varsayalım. "Supplier('CTSO')" "Product(1)" öğesinden bir bağlantı oluşturmak için istemci aşağıdaki gibi bir isteği gönderir:

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

Bir bağlantıyı silmek için istemci bağlantısını URI silme isteği gönderir.

**Bağlantı oluşturma**

Ürün tedarikçi bağlantılar oluşturmak bir istemci etkinleştirmek için aşağıdaki kodu ekleyin `ProductsController` sınıfı:

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

Bu yöntem üç parametreleri alır:

- *anahtar*: üst varlık (ürün) anahtarı
- *navigationProperty*: Gezinti özelliğinin adı. Bu örnekte, "Sağlayıcı" yalnızca geçerli bir gezinti özelliğidir.
- *Bağlantı*: ilgili varlığın OData URI. Bu değer, istek gövdesinden alınır. Örneğin, bağlantı URI olabilir "`http://localhost/odata/Suppliers('CTSO')`, tedarikçi kimlikli anlamına = 'CTSO'.

Yöntemi bağlantı tedarikçi aramak için kullanır. Eşleşen tedarikçi bulunursa, yöntem ayarlar `Product.Supplier` özelliği ve sonucu veritabanına kaydeder.

En zor bölümü bağlantı URI ayrıştırma. Temel olarak, bu URI bir GET isteği göndererek sonucunu benzetimini gerekir. Aşağıdaki yardımcı yöntemi bunun nasıl yapılacağı gösterilmektedir. Bu yöntem Web API yönlendirme işlemi çağırır ve geri alır bir **ODataPath** ayrıştırılmış OData yolunu temsil eden örneği. Bağlantı için URI, kesimleri birini Varlık anahtarı olmalıdır. (Aksi durumda, istemci geçersiz bir URI gönderilen.)

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**Bağlantıları silme**

Bir bağlantıyı silmek için aşağıdaki kodu ekleyin `ProductsController` sınıfı:

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

Bu örnekte, tek bir gezinti özelliği olduğundan `Supplier` varlık. Bir koleksiyon gezinti özelliği olduğundan, bir bağlantıyı silmek için URI ilgili varlık için bir anahtar içermelidir. Örneğin:

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

Bu istek sırası 1 müşteri 1 ' kaldırır. Bu durumda, DeleteLink yöntemi aşağıdaki imzası olacaktır:

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

*RelatedKey* parametresi ilgili varlık için anahtar sağlar. Bunu içinde `DeleteLink` yöntemi, birincil varlık tarafından Ara *anahtar* parametresi, ilgili varlık tarafından Bul *relatedKey* parametre ve ardından Kaldır ilişkilendirme. Veri modeline bağlı olarak, her iki sürümünün de uygulamanız gerekebilir `DeleteLink`. Web API istek URI'SİNDEKİ doğru sürümü çağırır.
