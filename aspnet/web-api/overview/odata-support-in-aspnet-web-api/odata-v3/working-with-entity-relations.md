---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Web API 2 OData v3 varlık ilişkilerini destekleme | Microsoft Docs
author: MikeWasson
description: 'Çoğu veri kümesinde varlıklar arasındaki ilişkileri tanımlamak: imajlarını siparişler; Books Yazar; yine de olan istiyor musunuz? Ürünler tedarikçileri sahip. OData kullanarak istemcileri üzerinden gidebilirsiniz...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: 311e84a2beb3ec7661fd650b277f23458bcb0cb2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377483"
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>Web API 2 OData v3 varlık ilişkilerini destekleme
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

[Projeyi yükle](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Çoğu veri kümesinde varlıklar arasındaki ilişkileri tanımlamak: imajlarını siparişler; Books Yazar; yine de olan istiyor musunuz? Ürünler tedarikçileri sahip. İstemciler, OData kullanarak, varlık ilişkileri gidebilirsiniz. Bir ürün göz önünde bulundurulduğunda, tedarikçi bulabilirsiniz. Ayrıca, oluşturmak veya ilişkileri kaldırın. Örneğin, bir ürünün sağlayıcısına ayarlayabilirsiniz.
> 
> Bu öğreticide, ASP.NET Web API işlemlerini desteklemek gösterilir. Öğretici öğreticiyi genişletip yapılar [ile Web API 2 OData v3 uç noktası oluşturma](creating-an-odata-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - Web API 2
> - OData sürüm 3
> - Entity Framework 6


## <a name="add-a-supplier-entity"></a>Tedarikçi varlık ekleme

İlk olarak biz bizim OData akışına yeni bir varlık türü eklemeniz gerekir. Ekleyeceğiz bir `Supplier` sınıfı.

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

Bu sınıf, bir dize için varlık anahtarı kullanır. Uygulamada, bir tamsayı anahtarı kullanmaktan daha az yaygın olabilir. Ancak OData tamsayılar yanı sıra diğer anahtar türleri nasıl işlediğini görmeye değerindedir.

Ardından, bir ilişki ekleyerek oluşturacağız bir `Supplier` özelliğini `Product` sınıfı:

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

Yeni bir **olan DB** için `ProductServiceContext` sınıfının Entity Framework içermesi `Supplier` veritabanındaki tablo.

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

WebApiConfig.cs içinde "Tedarikçileri" varlığı için EDM modeline ekleyin:

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>Gezinti özellikleri

Sağlayıcı için bir ürün almak için istemci bir GET isteği gönderir:

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

Burada "Sağlayıcı" bir gezinti özelliği açıktır `Product` türü. Bu durumda, `Supplier` başvuran tek bir öğe, ancak bir gezinti özelliği ayrıca bir koleksiyon (bire çok veya çoka çok ilişkisi) döndürebilir.

Bu istek desteklemek için aşağıdaki yöntemi ekleyin. `ProductsController` sınıfı:

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

*Anahtar* parametresi, ürün anahtarı. Bu durumda, ilgili varlık & #8212 yöntemi döndürür bir `Supplier` örneği. Parametre adı ve yöntem adı önemli olduğunda. Genel olarak, gezinme özelliğini "X" ise, "GetX" adlı bir yöntemi eklemeniz gerekir. Yöntem adlı bir parametre almalıdır "*anahtar*" üst öğenin anahtar veri türü ile eşleşen.

Eklenmesi önemlidir **[FromOdataUri]** özniteliğini *anahtar* parametresi. Bu öznitelik, istek URI'si anahtarından ayrıştırırken OData sözdizimi kurallarını kullanmak için Web API'si söyler.

## <a name="creating-and-deleting-links"></a>Oluşturma ve bağlantıları silme

OData iki varlıklar arasında ilişki oluşturma veya kaldırmayı destekler. OData terminolojisinde, ilişki bir "." bağlantıdır Her bağlantı form bir URI'ya sahip *varlık*/$links /*varlık*. Örneğin, ürün bağlantıdan tedarikçiye şöyle görünür:

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

Yeni bir bağlantı oluşturmak için istemci bağlantı URI'si için bir POST isteği gönderir. Hedef varlığın URI'si istek gövdesidir. Örneğin, "CTSO" anahtarına sahip bir sağlayıcı yok varsayalım. "Product(1)" "Supplier('CTSO')" için bir bağlantı oluşturmak için istemci aşağıdaki gibi bir istek gönderir:

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

Bir bağlantıyı silmek için istemci bağlantı URI'si için bir silme isteği gönderir.

**Bağlantı oluşturma**

Ürün tedarikçi bağlantılar oluşturmak bir istemci etkinleştirmek için aşağıdaki kodu ekleyin. `ProductsController` sınıfı:

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

Bu yöntem, üç parametreleri alır:

- *anahtar*: üst varlık (ürün) anahtarı
- *navigationProperty*: gezinme özelliğinin adı. Bu örnekte, yalnızca geçerli gezinme özelliğini "Sağlayıcı" dir.
- *Bağlantı*: ilgili varlığın OData URI. Bu değer, istek gövdesinden alınır. Örneğin, bağlantı URI'si olabilir "`http://localhost/odata/Suppliers('CTSO')`, kimliği bir tedarikçi anlamı = 'CTSO'.

Yöntemi, tedarikçi aramak için bağlantıya kullanır. Eşleşen tedarikçi bulunursa, yöntem ayarlar `Product.Supplier` özelliği ve sonuçları veritabanına kaydeder.

Uygulamalarınızdaki Bölümü ' % s'bağlantı URI'si ayrıştırılıyor. Temel olarak, bu URI'ye bir GET isteği gönderme sonucunu benzetimini yapmak gerekir. Aşağıdaki yardımcı yöntemini bunun nasıl yapılacağı gösterilmektedir. Bu yöntem, Web API'si yönlendirme işlemi çağırır ve geri alır bir **ODataPath** ayrıştırılmış OData yolunu temsil ettiği örneği. Bağlantı için URI, parçaların bir varlık anahtarı olmalıdır. (Aksi halde, hatalı bir URI istemciye gönderilen.)

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**Bağlantıları silme**

Bağlantıyı silmek için aşağıdaki kodu ekleyin. `ProductsController` sınıfı:

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

Bu örnekte, tek bir gezinti özelliği olan `Supplier` varlık. Bir koleksiyon gezinme özelliği ise bir bağlantıyı silmek için URI ilgili varlık için bir anahtar içermelidir. Örneğin:

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

Bu istek sırası 1 müşteri 1 ' kaldırır. Bu durumda, DeleteLink yöntemi aşağıdaki imzası olacaktır:

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

*RelatedKey* parametresi ilgili varlık için anahtar sağlar. Dolayısıyla, `DeleteLink` yöntemi, birincil varlık tarafından Ara *anahtarı* parametresi, ilgili varlık tarafından bulma *relatedKey* parametresini kaldırın ve sonra ilişkilendirme. Veri modelinizi bağlı olarak iki sürümü de uygulamanız gerekebilir `DeleteLink`. Web API'si istek URI'SİNDEKİ göre doğru sürümü çağırır.
