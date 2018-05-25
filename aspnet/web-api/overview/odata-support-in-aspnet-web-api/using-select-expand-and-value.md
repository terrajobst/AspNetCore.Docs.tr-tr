---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: $Select kullanarak $genişletin ve ASP.NET Web API 2 OData $value | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/11/2013
ms.topic: article
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: f229cdbd8850a787dd3585e0640e8e66f6109331
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a>$Select kullanarak $genişletin ve ASP.NET Web API 2 OData $value
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

İçin $expand, $select ve OData $value seçeneklerinde web API 2 desteği ekler. Bu seçenekler sunucudan geri alır gösterimi denetlemek bir istemci sağlar.

- **$expand** yanıta dahil satır içi olarak ilgili varlıklar neden olur.
- **$select** yanıta dahil edilecek özellik kümesini seçer.
- **$value** özelliğinin ham değeri alır.

## <a name="example-schema"></a>Şema örneği

Bu makalede, üç varlık tanımlayan OData hizmetine kullanmam: Ürün, üretici ve kategori. Her ürünün bir kategori ve bir sağlayıcı vardır.

![](using-select-expand-and-value/_static/image1.png)

Varlık modelleri tanımlayan C# sınıfları şunlardır:

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

Dikkat `Product` sınıfı tanımlayan Gezinti özellikleri için `Supplier` ve `Category`. `Category` Sınıfı, her kategoride ürünlere yönelik bir gezinti özelliği tanımlar.

Bu şema için bir OData uç nokta oluşturmak için Visual Studio 2013 yapı iskelesi açıklandığı gibi kullanın [ASP.NET Web API'de OData uç noktası oluşturma](odata-v3/creating-an-odata-endpoint.md). Ürün, kategori ve sağlayıcı için ayrı denetleyicileri ekleyebilirsiniz.

## <a name="enabling-expand-and-select"></a>Etkinleştirme $genişletin ve $select

Visual Studio 2013'te Web API OData yapı iskelesi bu otomatik olarak $expand destekler ve $select bir denetleyiciyi oluşturur. Başvuru için burada açıklanmıştır desteklemeyi $ gereksinimleri genişletin ve bir denetleyicide $select.

Koleksiyonlar için denetleyici 's `Get` yöntemi döndürmelidir bir **Iqueryable**.

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

Tek varlıklar için dönüş bir **SingleResult&lt;T&gt;**, T değerinin olduğu bir **Iqueryable** sıfır veya bir varlık içeriyor.

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

Ayrıca, tasarlamanız, `Get` yöntemleriyle **[Queryable]** önceki kod parçacıkları gösterildiği gibi öznitelik. Alternatif olarak, arama **EnableQuerySupport** üzerinde **HttpConfiguration** başlangıçta nesnesi. (Daha fazla bilgi için bkz: [etkinleştirme OData sorgu seçeneklerini](supporting-odata-query-options.md#enable).)

## <a name="using-expand"></a>Kullanarak $Genişlet

Bir OData varlık veya koleksiyon sorguladığınızda, varsayılan yanıt ilgili varlıklar içermez. Örneğin, kategoriler varlık kümesi için varsayılan yanıt şöyledir:

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

Gördüğünüz gibi ürünler Gezinti bağlantısı kategori varlık sahip olsa bile yanıt herhangi bir ürünü içermez. Ancak istemcinin kullanabileceği $ her kategori için ürünlerin listesini almak için genişletin. $Expand seçeneği isteğinin sorgu dizesinde geçer:

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

Artık sunucu ürünleri kategorileri satır içi her kategori için dahil edilir. Yanıt yükü şöyledir:

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

"Value" dizisindeki her giriş ürünlerin listesini içerdiğine dikkat edin.

$Expand genişletilecek Gezinti özelliklerini seçeneği alır virgülle ayrılmış listesi. Aşağıdaki isteği, kategori ve bir ürün için tedarikçiye genişletir.

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

Yanıt gövdesi şöyledir:

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

Gezinti özelliğinin birden fazla düzey genişletebilirsiniz. Aşağıdaki örnek, bir kategorideki tüm ürünleri ve ayrıca her ürünün sağlayıcısına içerir.

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

Yanıt gövdesi şöyledir:

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

Varsayılan olarak, Web API 2 en büyük genişletme derinliğini sınırlar. Engelleyen istemci gibi karmaşık istekleri göndermesinin `$expand=Orders/OrderDetails/Product/Supplier/Region`, hangi olabilir sorgulamak ve geniş yanıtları oluşturmak için yetersiz. Varsayılan yerine geçecek şekilde ayarlanmış **MaxExpansionDepth** özelliği **[Queryable]** özniteliği.

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

Seçeneğini genişletin $ hakkında daha fazla bilgi için bkz: [sistem sorgulama ($expand) seçeneğini genişletin](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) resmi OData belgelerinde.

## <a name="using-select"></a>$Select kullanma

$Select seçeneği yanıt gövdesinde dahil edilecek özellik kümesini belirtir. Örneğin, yalnızca adı ve her ürünün fiyatı almak için aşağıdaki sorguyu kullanın:

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

Yanıt gövdesi şöyledir:

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

$Select birleştirebilirsiniz ve $ aynı sorguda genişletin. $Select seçeneğinde genişletilmiş özellik eklediğinizden emin olun. Örneğin, aşağıdaki isteği üretici ve ürün adını alır.

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

Yanıt gövdesi şöyledir:

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

Özellikleri genişletilmiş bir özellik içinde de seçebilirsiniz. Aşağıdaki isteği ürünleri genişletir ve kategori adı ve ürün adı seçer.

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

Yanıt gövdesi şöyledir:

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

$Select seçeneği hakkında daha fazla bilgi için bkz: [seçin sistem sorgusu seçeneği ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) resmi OData belgelerinde.

## <a name="getting-individual-properties-of-an-entity-value"></a>Tek bir varlık ($value) özelliklerini alma

Tek bir özellik bir varlıktan almak bir OData istemcisi için iki yolu vardır. İstemci değeri OData biçiminde almak veya özelliğin ham değeri alır.

Aşağıdaki isteği OData biçiminde bir özelliğini alır.

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

JSON biçiminde bir örnek yanıt şöyledir:

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

Özelliğin ham değeri almak için $value uri'ye ekler:

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

Burada, yanıt verilmiştir. İçerik türü "metin/düz" JSON olmadığına dikkat edin.

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

Bu sorgular, OData denetleyicisi desteklemek için adlı bir yöntem eklemek `GetProperty`, burada `Property` özelliği adıdır. Örneğin, Name özelliği almak için yöntemi adlı `GetName`. Yöntemi, o özelliğin değerini döndürmesi gerekir:

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
