---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: ASP.NET Web API ile OData v4 sürümünde karmaşık tür devralma | Microsoft Docs
author: microsoft
description: OData v4 belirtimine göre bir karmaşık tür başka bir karmaşık türden devralabilir. (Bir karmaşık türü bir yapılandırılmış bir anahtar olmadan türdür.) Web API...
ms.author: aspnetcontent
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: d295a6ae20f5771ae1f4f28166f7e651b6ec5c58
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824036"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>ASP.NET Web API ile OData v4 sürümünde karmaşık tür devralma
====================
tarafından [Microsoft](https://github.com/microsoft)

> OData v4 göre [belirtimi](http://www.odata.org/documentation/odata-version-4-0/), karmaşık bir tür, başka bir karmaşık türden devralabilir. (A *karmaşık* türü olan bir anahtar olmadan yapılandırılmış bir tür.) Web API OData 5.3, karmaşık tür devralma destekler.
> 
> Bu konu, bir varlık veri modeli (EDM) derleme ile karmaşık devralma türleri nasıl gösterir. Tam kaynak kodunu görmek [OData karmaşık tür devralma örnek](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - Web API OData 5.3
> - OData v4


## <a name="model-hierarchy"></a>Modeli hiyerarşisi

Karmaşık Tür devralma göstermek için aşağıdaki sınıf hiyerarşisi kullanacağız.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape` bir soyut karmaşık bir türdür. `Rectangle`, `Triangle`, ve `Circle` öğesinden türetilen karmaşık türler `Shape`, ve `RoundRectangle` türetildiği `Rectangle`. `Window` bir varlık türü olduğu ve içeren bir `Shape` örneği.

Bu tür tanımlama CLR sınıfları şunlardır.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>EDM modeli oluşturun

EDM oluşturmak için kullanabileceğiniz **ODataConventionModelBuilder**, kalıtım ilişkileri CLR türünden çıkarır.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

Ayrıca EDM açıkça kullanarak oluşturabilirsiniz **ODataModelBuilder**. Bu, daha fazla kod sürer, ancak EDM üzerinde daha fazla denetim verir.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

Bu iki örnek aynı EDM şema oluşturun.

## <a name="metadata-document"></a>Meta veri belgesi

Karmaşık Tür devralma gösteren OData meta veri belgesi aşağıda verilmiştir.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

Meta veri belgeden görebilirsiniz:

- `Shape` Karmaşık türü Özet.
- `Rectangle`, `Triangle`, Ve `Circle` karmaşık türün temel türünü sahip `Shape`.
- `RoundRectangle` Türünde taban türü `Rectangle`.

## <a name="casting-complex-types"></a>Karmaşık türler atama

Karmaşık türlerde atama artık desteklenmektedir. Örneğin, aşağıdaki atamalardan sorgu bir `Shape` için bir `Rectangle`.

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

Yanıt yükünde şu şekildedir:

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
