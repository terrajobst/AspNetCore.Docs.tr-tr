---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: "Web API 2.2 kullanma kapsama OData v4 içinde | Microsoft Docs"
author: rick-anderson
description: "Bir varlık kümesi içinde kapsüllenmiş, geleneksel olarak, bir varlığın yalnızca erişilebilir. Ancak OData v4 Singleton ve Con olmak üzere iki ek seçenekler sağlar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 7d3c81bf3d2a43faa3e71155637e031f81143782
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="containment-in-odata-v4-using-web-api-22"></a>OData v4 içinde kapsama Web API 2.2 kullanma
====================
tarafından Jinfu Tan

> Bir varlık kümesi içinde kapsüllenmiş, geleneksel olarak, bir varlığın yalnızca erişilebilir. Ancak OData v4 Singleton ve her ikisi de Webapı 2.2 destekleyen kapsama olmak üzere iki ek seçenekler sağlar.


Bu konuda, bir kapsama Webapı 2.2 içinde bir OData uç noktasını tanımlamak gösterilmiştir. Kapsama hakkında daha fazla bilgi için bkz: [kapsama ile OData v4 yakında](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx). Web API'de OData V4 uç noktası oluşturmak için bkz: [bir OData v4 uç nokta kullanarak ASP.NET Web API 2.2 oluşturma](create-an-odata-v4-endpoint.md).

İlk olarak, bir kapsama etki alanı modeli OData hizmeti bu veri modelini kullanarak oluşturacağız:

![Veri modeli](odata-containment-in-web-api-22/_static/image1.png)

Bir hesap birçok PaymentInstruments (PI) içerir, ancak bir varlık için bir PI kümesine tanımlarız yok. Bunun yerine, yönergelerinin yalnızca bir hesap erişilebilir.

Bu konudan kullanılan çözüm indirebilirsiniz [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).

## <a name="defining-the-data-model"></a>Veri modeli tanımlama

1. CLR Türleri tanımlayın.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    `Contained` Özniteliği kapsama Gezinti özellikleri için kullanılır.
2. CLR türlerine göre EDM modeli oluşturur.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    `ODataConventionModelBuilder` Varsa EDM modelini oluşturma işleyecek `Contained` özniteliği, karşılık gelen gezinme özelliğini eklenir. Özelliği bir koleksiyon türü ise bir `GetCount(string NameContains)` işlevi de oluşturulacaktır.

    Oluşturulan meta verileri aşağıdaki gibi görünür:

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    `ContainsTarget` Özniteliği gezinme özelliğinin bir kapsama olduğunu gösterir.

## <a name="define-the-containing-entity-set-controller"></a>İçeren varlık kümesi denetleyicisi tanımlayın

Kapsanan varlıklar kendi denetleyicisi gerekmez; eylemi içeren varlık kümesi denetleyicide tanımlanır. Bu örnekte, bir AccountsController, ancak hiçbir PaymentInstrumentsController yoktur.

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

OData yolu kesimleri 4 veya daha fazla ise, yalnızca Yönlendirme works gibi özniteliği `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` yukarıdaki denetleyicideki. Aksi takdirde, hem öznitelik hem de Geleneksel yönlendirme çalışır: Örneğin, `GetPayInPIs(int key)` eşleşen `GET ~/Accounts(1)/PayinPIs`.

*Bu makalenin özgün içerik için Leo Hu teşekkür ederiz.*
