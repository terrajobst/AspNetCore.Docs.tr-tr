---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: ASP.NET Web API ile OData v4 karmaşık tür devralma | Microsoft Docs
author: microsoft
description: OData v4 belirtimine göre başka bir karmaşık türü bir karmaşık tür devralabilirsiniz. (Bir karmaşık türü bir anahtar olmadan yapılandırılmış bir tür olur.) Web API...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: be2dbfa82b99b6c48928e4e767716852c14a463b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566832"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>ASP.NET Web API ile OData v4 karmaşık tür devralma
====================
tarafından [Microsoft](https://github.com/microsoft)

> OData v4 göre [belirtimi](http://www.odata.org/documentation/odata-version-4-0/), başka bir karmaşık türü bir karmaşık tür devralabilirsiniz. (A *karmaşık* türü olan bir anahtar olmadan yapılandırılmış bir tür.) Web API OData 5.3 karmaşık tür devralma destekler.
> 
> Bu konu, bir varlık veri modeli (EDM) karmaşık devralma türleriyle nasıl oluşturulacağını gösterir. Tam kaynak kodunu bkz [OData karmaşık tür devralma örnek](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - Web API OData 5.3
> - OData v4


## <a name="model-hierarchy"></a>Modeli hiyerarşisi

Karmaşık Tür devralma göstermek için aşağıdaki sınıf hiyerarşisi kullanacağız.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape`soyut bir karmaşık tür ' dir. `Rectangle`, `Triangle`, ve `Circle` karmaşık türler türetilmiş `Shape`, ve `RoundRectangle` türetilen `Rectangle`. `Window`bir varlık türü olduğu ve içeren bir `Shape` örneği.

Burada, bu tür tanımlamak CLR sınıflarını bulunmaktadır.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>EDM modeli oluşturma

EDM oluşturmak için kullanabileceğiniz **ODataConventionModelBuilder**, CLR türünden devralma ilişkisi oluşturur.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

Ayrıca EDM açıkça kullanarak oluşturabilirsiniz **ODataModelBuilder**. Bu, daha fazla kod alır, ancak EDM üzerinde daha fazla denetim sağlar.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

Bu iki örnek aynı EDM şeması oluşturun.

## <a name="metadata-document"></a>Meta veri belgesi

Karmaşık Tür devralma gösteren OData meta veri belgesi aşağıdadır.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

Meta veri belgeden görebilirsiniz:

- `Shape` Karmaşık türü Özet.
- `Rectangle`, `Triangle`, Ve `Circle` karmaşık türün temel türünü sahip `Shape`.
- `RoundRectangle` Türüne sahip temel türü `Rectangle`.

## <a name="casting-complex-types"></a>Karmaşık türler atama

Karmaşık türler üzerinde atama artık desteklenmektedir. Örneğin, aşağıdaki atamalar sorgu bir `Shape` için bir `Rectangle`.

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

Yanıt yükü şöyledir:

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
