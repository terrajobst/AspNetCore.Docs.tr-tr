---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: $Select kullanarak $expand ve ASP.NET Web API 2 OData $value | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/11/2013
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: d198ecf40155cba36204bc0810f4735aae6b100b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751974"
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a>$Select kullanarak $expand ve ASP.NET Web API 2 OData $value
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

İçin $expand, $select ve OData $value seçeneklerinde web API 2 desteği ekler. Bu seçenekler, bir istemcinin sunucudan geri alır gösterimini kontrol sağlar.

- **$expand** yanıt satır içi olarak ilgili varlıkları neden olur.
- **$select** yanıta dahil edilecek özellik kümesini seçer.
- **$value** ham bir özelliğin değerini alır.

## <a name="example-schema"></a>Şema örneği

Bu makalede üç varlıklar tanımlayan bir OData hizmeti kullanacağım: Ürün, tedarikçi ve kategorisi. Her ürünün bir kategori ve bir tedarikçi vardır.

![](using-select-expand-and-value/_static/image1.png)

Varlık modelleri tanımlayan C# sınıfları şunlardır:

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

Dikkat `Product` sınıfı tanımlar için gezinme özelliklerinin `Supplier` ve `Category`. `Category` Sınıfı, her kategoride ürünleri için bir gezinti özelliği tanımlar.

Bu şema için bir OData uç noktası oluşturmak için Visual Studio 2013 yapı iskelesi açıklandığı kullanın [ASP.NET Web API OData uç noktası oluşturma](odata-v3/creating-an-odata-endpoint.md). Ürün, kategori ve tedarikçi ayrı denetleyicileri ekleyin.

## <a name="enabling-expand-and-select"></a>Etkinleştirme $genişletin ve $select

Visual Studio 2013'te Web API OData yapı iskelesi, otomatik olarak $expand destekler ve $select bir denetleyiciyi oluşturur. Başvuru için işte desteklemeyi $ı gereksinimleri genişletin ve bir denetleyicide $select.

Koleksiyon denetleyiciye ait `Get` yöntemi döndürmelidir bir **Iqueryable**.

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

Tek varlıklar için dönüş bir **SingleResult&lt;T&gt;**, burada T, bir **Iqueryable** , sıfır veya bir varlık içeriyor.

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

Ayrıca, tasarlamanız, `Get` yöntemleriyle **[Queryable]** , önceki kod parçacıklarında gösterildiği gibi öznitelik. Alternatif olarak, çağrı **EnableQuerySupport** üzerinde **HttpConfiguration** başlangıçta nesnesi. (Daha fazla bilgi için [OData sorgu seçenekleri etkinleştirme](supporting-odata-query-options.md#enable).)

## <a name="using-expand"></a>Kullanarak $genişletin

Bir OData varlık veya koleksiyon sorguladığınızda, varsayılan yanıt ilgili varlıkları içermez. Örneğin, varsayılan yanıt kategorileri varlık kümesi için şu şekildedir:

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

Gördüğünüz gibi kategori varlık ürünleri Gezinti bağlantısı olmasına rağmen yanıt herhangi bir ürün içermez. Ancak istemcinin kullanabileceği $ her kategori için ürünlerin listesini almak için genişletin. $Expand seçeneği isteğinin sorgu dizesinde geçer:

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

Artık sunucu ürünleri, satır içi kategorileri ile olmak üzere her kategori için içerir. Yanıt yükünde şu şekildedir:

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

"Value" dizisindeki her giriş için bir ürün listesini içerdiğine dikkat edin.

$ Seçeneği alır bir virgülle ayrılmış listesi genişletilecek Gezinti özelliklerini genişletin. Aşağıdaki isteği kategorisi hem bir ürünün sağlayıcısına genişletir.

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

Yanıt gövdesi şu şekildedir:

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

Gezinme özelliğinin birden fazla düzeyde genişletebilirsiniz. Aşağıdaki örnek, bir kategorideki tüm ürünleri ve her ürün için tedarikçi içerir.

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

Yanıt gövdesi şu şekildedir:

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

Varsayılan olarak, Web API 2 en büyük genişletme derinliği sınırlar. Engelleyen istemci gibi karmaşık istekler göndermesini `$expand=Orders/OrderDetails/Product/Supplier/Region`, sorgu ve büyük yanıtları oluşturmak için yetersiz gösterebilir. Varsayılan geçersiz kılmak için ayarlanmış **MaxExpansionDepth** özelliği **[Queryable]** özniteliği.

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

Seçeneğini genişletin $ hakkında daha fazla bilgi için bkz: [sistem sorgusu ($expand) seçeneği genişletin](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) resmi OData belgelerinde.

## <a name="using-select"></a>$Select kullanma

$Select seçeneği, yanıt gövdesinde dahil edilecek özellik kümesini belirtir. Örneğin, yalnızca adı ve her ürünün fiyatı almak için aşağıdaki sorguyu kullanın:

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

Yanıt gövdesi şu şekildedir:

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

$Select birleştirebilir ve $, aynı sorgu içinde genişletin. $Select seçeneğinde genişletilmiş özelliği eklediğinizden emin olun. Örneğin, aşağıdaki isteği, üretici ve ürün adını alır.

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

Yanıt gövdesi şu şekildedir:

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

Genişletilmiş bir özellik içinde özelliklerini de seçebilirsiniz. Aşağıdaki isteği ürünleri genişletir ve kategori adı artı ürün adını seçer.

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

Yanıt gövdesi şu şekildedir:

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

$Select seçeneği hakkında daha fazla bilgi için bkz. [sistem sorgusu seçeneği seçin ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) resmi OData belgelerinde.

## <a name="getting-individual-properties-of-an-entity-value"></a>Tek bir varlık ($value) özelliklerini alma

Bir varlıktan tek bir özellik almak bir OData istemcisi için iki yolu vardır. İstemci değeri OData biçiminde alın veya özelliğin ham değeri alır.

Aşağıdaki isteği OData biçiminde bir özelliğini alır.

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

JSON biçiminde bir yanıt örneği aşağıdadır:

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

Ham özelliğin değerini almak için $value URİ'ye ekler:

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

Yanıtı aşağıda verilmiştir. İçerik türü "text/plain" JSON olmadığına dikkat edin.

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

OData denetleyiciniz bu sorguları desteklemek için adında bir yöntem ekleyin `GetProperty`burada `Property` özelliğin adıdır. Örneğin, Name özelliği alınacak yöntemin adlı `GetName`. Yöntemi, bu özelliğin değeri döndürmesi gerekir:

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
