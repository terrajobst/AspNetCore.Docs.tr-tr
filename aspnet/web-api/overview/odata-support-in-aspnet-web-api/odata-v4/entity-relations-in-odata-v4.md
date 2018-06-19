---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: ASP.NET Web API 2.2 kullanarak varlık ilişkileriyle OData v4 içinde | Microsoft Docs
author: MikeWasson
description: 'Varlıkları arasındaki ilişkileri çoğu veri kümelerini tanımlayın: müşterilerin sahip siparişleri; Books yazarlar; yine de sahip istiyor musunuz? Ürünler Üreticiler vardır. OData kullanarak, istemciler üzerinde gidebilirsiniz...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 4127fb2fa83f513a4faeb222068ca99f234ece22
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566736"
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a>OData v4 varlık ilişkilerine ASP.NET Web API 2.2 kullanma
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

> Varlıkları arasındaki ilişkileri çoğu veri kümelerini tanımlayın: müşterilerin sahip siparişleri; Books yazarlar; yine de sahip istiyor musunuz? Ürünler Üreticiler vardır. OData'ı kullanarak istemcileri varlık ilişkileriyle gidin. Bir ürün verildiğinde, tedarikçi bulabilirsiniz. Ayrıca, oluşturabilir veya ilişkileri kaldırın. Örneğin, bir ürün için tedarikçiye ayarlayabilirsiniz.
> 
> Bu öğreticide, ASP.NET Web API kullanarak OData v4 bu işlemleri desteklemek gösterilmiştir. Öğretici öğreticiyi derlemeler [bir OData v4 uç nokta kullanarak ASP.NET Web API 2 oluşturma](create-an-odata-v4-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - Web API 2.1
> - OData v4
> - [Visual Studio 2013 Güncelleştirme 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Varlık Çerçevesi 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Eğitmen sürümleri
> 
> OData sürüm 3 için bkz: [varlık ilişkilerine destekleyen OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).


## <a name="add-a-supplier-entity"></a>Bir sağlayıcı varlık ekleme

> [!NOTE]
> Öğretici öğreticiyi derlemeler [bir OData v4 uç nokta kullanarak ASP.NET Web API 2 oluşturma](create-an-odata-v4-endpoint.md).


İlk olarak, ilgili varlığın gerekir. Adlı bir sınıf ekleyin `Supplier` modeller klasörü içinde.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

Bir gezinti özelliği Ekle `Product` sınıfı:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

Yeni bir ekleme **DbSet** için `ProductsContext` sınıf böylece Entity Framework sağlayıcı tablo veritabanında dahil edilir.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

WebApiConfig.cs içinde eklemek bir &quot;Üreticiler&quot; varlık için varlık veri modeli ayarlayın:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a>Üreticiler denetleyici ekleme

Ekleme bir `SuppliersController` denetleyicileri klasörüne sınıfı.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

Bu denetleyici için CRUD işlemleri ekleme Göster olmaz. Adımları ürünleri denetleyicisi aynıdır (bkz [bir OData v4 uç noktası oluşturma](create-an-odata-v4-endpoint.md)).

## <a name="getting-related-entities"></a>Alma ilgili varlıklar

Sağlayıcı bir ürün almak için istemci bir GET isteği gönderir:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

Bu istek desteklemek için aşağıdaki yöntemi ekleyin `ProductsController` sınıfı:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

Bu yöntem varsayılan adlandırma kuralını kullanır

- Yöntem adı: GetX, burada X, gezinme özelliği.
- Parametre adı: *anahtarı*

Bu adlandırma kuralını izlerseniz, Web API denetleyicisi yöntemi HTTP isteği otomatik olarak eşlenir.

Örnek HTTP isteği:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

Örnek HTTP yanıtı:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a>İlişkili bir koleksiyon alma

Önceki örnekte, bir ürünün bir sağlayıcı vardır. Bir gezinme özelliği de bir koleksiyonu döndürebilir. Aşağıdaki kod ürünleri için bir sağlayıcının alır:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

Bu durumda, yöntem bir **Iqueryable** yerine bir **SingleResult&lt;T&gt;**

Örnek HTTP isteği:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

Örnek HTTP yanıtı:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a>Varlıklar arasında bir ilişki oluşturma

OData oluşturma veya var olan iki varlık arasındaki ilişkileri kaldırma destekler. OData v4 terminolojisinde ilişkidir bir &quot;başvuru&quot;. (OData v3 ilişki adlı bir *bağlantı*. Protokol farklar Bu öğretici için önemi yoktur.)

Form ile kendi URI başvuruyor `/Entity/NavigationProperty/$ref`. Örneğin, bir ürünü ve onun tedarikçi arasındaki referansı adres için URI şudur:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

İstemci bir ilişki eklemek için bu adrese bir POST veya PUT İsteği gönderir.

- Gezinti özelliği tek bir varlık gibi ise PUT `Product.Supplier`.
- Gezinti özelliği bir koleksiyon gibi ise sonrası `Supplier.Products`.

İstek gövdesi bir ilişki varlığındaki URI'sini içeriyor. Bir örnek isteği şöyledir:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

Bu örnekte, istemci bir PUT İsteği gönderir. `/Products(6)/Supplier/$ref`, $ref URI'sini olduğu `Supplier` = 6 ürün kimliği. İstek başarılı olursa, sunucunun 204 (No içerik) yanıt gönderir:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

Bir ilişki eklemek için denetleyici yöntemi İşte bir `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

*NavigationProperty* parametresi ayarlamak için hangi ilişki belirtir. (Varlık üzerinde birden fazla gezinti özelliği varsa, daha fazla ekleyebilirsiniz `case` deyimleri.)

*Bağlantı* parametresi tedarikçi URI'sini içeriyor. Web API otomatik olarak bu parametre için değer almak için istek gövdesini ayrıştırır.

Tedarikçi aramak için kimliği (veya anahtar) ihtiyacımız parçası olduğu *bağlantı* parametresi. Bunu yapmak için aşağıdaki yardımcı yöntemi kullanın:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

Temel olarak, bu yöntem, URI yolu parçalara bölmek, anahtarı içeren segmenti bulun ve anahtarı doğru türüne dönüştürün OData kitaplığı kullanır.

## <a name="deleting-a-relationship-between-entities"></a>Varlıklar arasında bir ilişki silme

İstemci bir ilişkiyi silmek için $ref URI HTTP DELETE isteği gönderir:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

Bir ürün ve tedarikçi arasındaki ilişkiyi silmek için denetleyici yöntemi şöyledir:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

Bu durumda, `Product.Supplier` olan &quot;1&quot; yalnızca ayarlayarak ilişki kaldırabilmeniz için 1-çok ilişkisi, end `Product.Supplier` için `null`.

İçinde &quot;birçok&quot; ilişkinin bir ucundaki, istemci, ilgili kaldırmak için varlık belirtmeniz gerekir. Bunu yapmak için istemci ilgili varlığın URI'si isteğin sorgu dizesi gönderir. Örneğin, "Ürün 1" kaldırmak için "1" Tedarikçi:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

Bu Web API'si desteklemek için ek bir parametre eklemek ihtiyacımız `DeleteRef` yöntemi. Bir ürün kaldırmak için denetleyici yöntemi işte `Supplier.Products` ilişkisi.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

*Anahtar* anahtar sağlayıcı için bir parametredir ve *relatedKey* parametredir kaldırmak ürün anahtarı `Products` ilişki. Web API Sorgu dizesinden otomatik olarak anahtarı alır unutmayın.
