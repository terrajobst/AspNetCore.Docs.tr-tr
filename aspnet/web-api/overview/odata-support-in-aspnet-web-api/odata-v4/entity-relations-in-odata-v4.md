---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: OData v4 sürümünde varlık ilişkilerini kullanarak ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: 'Çoğu veri kümesinde varlıklar arasındaki ilişkileri tanımlamak: imajlarını siparişler; Books Yazar; yine de olan istiyor musunuz? Ürünler tedarikçileri sahip. OData kullanarak istemcileri üzerinden gidebilirsiniz...'
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 80173519f1c8abd77b4138b7d29f780ffc60a188
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753840"
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a>OData v4 sürümünde varlık ilişkilerini kullanarak ASP.NET Web API 2.2
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

> Çoğu veri kümesinde varlıklar arasındaki ilişkileri tanımlamak: imajlarını siparişler; Books Yazar; yine de olan istiyor musunuz? Ürünler tedarikçileri sahip. İstemciler, OData kullanarak, varlık ilişkileri gidebilirsiniz. Bir ürün göz önünde bulundurulduğunda, tedarikçi bulabilirsiniz. Ayrıca, oluşturmak veya ilişkileri kaldırın. Örneğin, bir ürünün sağlayıcısına ayarlayabilirsiniz.
> 
> Bu öğreticide, ASP.NET Web API'sini kullanarak bu işlemleri OData v4 sürümünde destekleyen gösterilmektedir. Öğretici öğreticiyi genişletip yapılar [OData v4 uç noktası kullanarak ASP.NET Web API 2 oluşturma](create-an-odata-v4-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - Web API 2.1
> - OData v4
> - [Visual Studio 2013 Güncelleştirme 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Öğretici sürümleri
> 
> OData sürüm 3 için bkz: [OData v3 sürümünde varlık ilişkileri'ı destekleyen](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).


## <a name="add-a-supplier-entity"></a>Tedarikçi varlık ekleme

> [!NOTE]
> Öğretici öğreticiyi genişletip yapılar [OData v4 uç noktası kullanarak ASP.NET Web API 2 oluşturma](create-an-odata-v4-endpoint.md).


İlk olarak, ilgili varlık gerekir. Adlı bir sınıf ekleyin `Supplier` modeller klasörü içinde.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

Bir gezinme özelliği için ekleme `Product` sınıfı:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

Yeni bir **olan DB** için `ProductsContext` Entity Framework Sağlayıcısı tablo veritabanında içermesi sınıfı.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

WebApiConfig.cs içinde ekleme bir &quot;tedarikçileri&quot; varlık kümesi için varlık veri modeli:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a>Üreticiler denetleyici ekleme

Ekleme bir `SuppliersController` denetleyicileri klasörüne sınıfı.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

Ben CRUD işlemleri Bu denetleyici ekleme gösterilmez. Adımları ürünleri denetleyicisi aynıdır (bkz [bir OData v4 uç noktası oluşturma](create-an-odata-v4-endpoint.md)).

## <a name="getting-related-entities"></a>Başlangıç ilgili varlıklar

Sağlayıcı için bir ürün almak için istemci bir GET isteği gönderir:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

Bu istek desteklemek için aşağıdaki yöntemi ekleyin. `ProductsController` sınıfı:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

Bu yöntem varsayılan bir adlandırma kuralını kullanır.

- Yöntem adı: GetX, burada X, gezinme özelliği.
- Parametre adı: *anahtarı*

Bu adlandırma kuralını izler, Web API denetleyicisi yöntemi HTTP isteği otomatik olarak eşler.

Örnek HTTP istek:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

Örnek HTTP yanıtı:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a>İlgili koleksiyonu alma

Önceki örnekte, bir ürün bir tedarikçi sahiptir. Bir gezinme özelliği, bir koleksiyon da döndürebilir. Aşağıdaki kod ürünleri için bir sağlayıcı alır:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

Bu durumda, yöntem döndürür bir **Iqueryable** yerine bir **SingleResult&lt;T&gt;**

Örnek HTTP istek:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

Örnek HTTP yanıtı:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a>Varlıklar arasında ilişki oluşturma

OData oluşturmaya veya var olan iki varlık arasında ilişkiler destekler. OData v4 terminolojisinde ilişkidir bir &quot;başvuru&quot;. (OData v3 sürümünde ilişki çağrıldı bir *bağlantı*. Protokol farklar Bu öğretici için önemli değildir.)

Başvuru formunu kendi URI'sine sahip `/Entity/NavigationProperty/$ref`. Örneğin, bir ürün, tedarikçi arasındaki başvuru adres için URI şu şekildedir:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

İstemci bir ilişki eklemek için bu adrese bir POST veya PUT İsteği gönderir.

- Gezinti özelliği tek bir varlık gibi ise PUT `Product.Supplier`.
- Bir koleksiyon gezinme özelliği olduğu gibi istiyorsanız `Supplier.Products`.

İstek gövdesi, başka bir varlık ilişkisi URI'sini içerir. Bir örnek İstek şu şekildedir:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

Bu örnekte, istemci bir PUT İsteği gönderir. `/Products(6)/Supplier/$ref`, $ref URI'sini olduğu `Supplier` = 6 ürünün Kimliğine sahip. İstek başarılı olursa sunucu 204 (içerik yok) yanıt gönderir:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

Bir ilişki eklemek için denetleyici yöntemi İşte bir `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

*NavigationProperty* parametre ayarlamak için hangi ilişki belirtir. (Varlık üzerinde birden fazla gezinti özelliği varsa, daha fazla ekleyebilirsiniz `case` deyimleri.)

*Bağlantı* parametresi tedarikçi URI'sini içerir. Web API'si, otomatik olarak bu parametre için değer almak için istek gövdesini ayrıştırır.

Tedarikçi bakmak için kimliği (veya anahtar) ihtiyacımız parçası olduğu *bağlantı* parametresi. Bunu yapmak için aşağıdaki yardımcı yöntemini kullanın:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

Temel olarak, bu yöntem, URI yolu parçalara bölmek için anahtarı içeren kesim bulup anahtarı doğru türe dönüştürmeniz OData kitaplığı kullanır.

## <a name="deleting-a-relationship-between-entities"></a>Varlıklar arasında ilişki siliniyor

Bir ilişkiyi silmek için istemci için $ref URI HTTP DELETE isteği gönderir:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

Bir ürün ve tedarikçi arasındaki ilişkiyi silmek için denetleyici yöntemi aşağıda verilmiştir:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

Bu durumda, `Product.Supplier` olduğu &quot;1&quot; yalnızca ayarlayarak ilişki kaldırabilmeniz için 1-çok ilişkisi sonuna `Product.Supplier` için `null`.

İçinde &quot;birçok&quot; istemci bir ilişki sonu, varlık kaldırmak için ilgili belirtmeniz gerekir. Bunu yapmak için istemci isteğinin sorgu dizesinde ilgili varlığın URI'si gönderir. Örneğin, "Ürün 1" kaldırmak için "1" sağlayıcısı:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

Web API'de Bunu desteklemek için ek bir parametre içerecek şekilde ihtiyacımız `DeleteRef` yöntemi. İşte bir üründen kaldırmak için denetleyici yöntemi `Supplier.Products` ilişkisi.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

*Anahtarı* tedarikçi, anahtarı bir parametredir ve *relatedKey* parametredir kaldırmak ürün anahtarı `Products` ilişki. Web API Sorgu dizesinden otomatik olarak anahtarı alır unutmayın.
