---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: OData v4 sürümünde kapsama kullanarak Web API 2.2 | Microsoft Docs
author: rick-anderson
description: Geleneksel olarak, bir varlık kümesi içinde kapsüllenmiş, bir varlığın yalnızca erişilemedi. Ancak, OData v4 tekil ve Con olmak üzere iki ek seçenekler sağlar...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: aca263a04df25ca241bc0b9798b3a0b588d4cae8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756127"
---
<a name="containment-in-odata-v4-using-web-api-22"></a>OData v4 sürümünde kapsama Web API 2.2 kullanma
====================
tarafından Jinfu Tan

> Geleneksel olarak, bir varlık kümesi içinde kapsüllenmiş, bir varlığın yalnızca erişilemedi. Ancak, OData v4 tekil ve kapsama, ikisi için de Webapı 2.2 destekleyen iki ek seçenekler sağlar.


Bu konuda, bir kapsama Webapı 2.2 içinde OData uç noktası tanımlamak gösterilmektedir. Kapsama hakkında daha fazla bilgi için bkz: [kapsama ile OData v4 yakında](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx). Web API'de OData V4 uç nokta oluşturmak için bkz [OData v4 uç noktası kullanarak ASP.NET Web API 2.2 oluşturma](create-an-odata-v4-endpoint.md).

İlk olarak, bir kapsama etki alanı modeli OData hizmeti kullanarak bu veri modeli oluşturacağız:

![Veri modeli](odata-containment-in-web-api-22/_static/image1.png)

Birçok PaymentInstruments (PI) bir hesap içeriyor, ancak bir varlık kümesini PI sayısı için tanımlarız yok. Bunun yerine, yönergelerinin yalnızca bir hesap erişilebilir.

Bu konu başlığı altında kullanılan çözüm indirebileceğiniz [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).

## <a name="defining-the-data-model"></a>Veri modelini tanımlama

1. CLR Türleri tanımlar.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    `Contained` Özniteliği kapsama Gezinti özellikleri için kullanılır.
2. CLR türlerine göre EDM modelini oluşturun.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    `ODataConventionModelBuilder` EDM modelini derlenerek işleyecek `Contained` özniteliği için karşılık gelen gezinme özelliğini eklenir. Koleksiyon türü, özellik ise bir `GetCount(string NameContains)` işlevi de oluşturulacaktır.

    Oluşturulan meta veriler aşağıdaki gibi görünür:

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    `ContainsTarget` Özniteliği gezinme özelliğinin bir kapsama olduğunu gösterir.

## <a name="define-the-containing-entity-set-controller"></a>İçeren varlık kümesi denetleyicisi tanımlayın

Kapsanan varlıkları kendi denetleyici gerekmez; eylemi içeren varlık kümesi denetleyicide tanımlanır. Bu örnekte, bir AccountsController, ancak hiçbir PaymentInstrumentsController yoktur.

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

OData yolu kesimleri 4 veya daha fazla ise, yalnızca yönlendirmenin çalıştığını gibi öznitelik `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` yukarıdaki denetleyicisindeki. Aksi takdirde, hem öznitelik hem de Geleneksel yönlendirme çalışır: Örneğin, `GetPayInPIs(int key)` eşleşen `GET ~/Accounts(1)/PayinPIs`.

*Bu makalede, özgün içerik sayesinde Hu satılmıştır.*
