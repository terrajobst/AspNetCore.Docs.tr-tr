---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Web API 2.2 kullanma OData v4 sürümünde Singleton oluşturma | Microsoft Docs
author: rick-anderson
description: Bu konuda, tek bir OData uç noktası, Web API 2.2 tanımlamak gösterilmektedir.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 3ae9b23fae356e387a011a190119d760dc46d022
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390933"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>Web API 2.2 kullanma OData v4 sürümünde Singleton oluşturma
====================
Zoe Luo tarafından

> Geleneksel olarak, bir varlık kümesi içinde kapsüllenmiş, bir varlığın yalnızca erişilemedi. Ancak, OData v4 tekil ve kapsama, ikisi için de Webapı 2.2 destekleyen iki ek seçenekler sağlar.


Bu makalede, tek bir OData uç noktası, Web API 2.2 tanımlamak gösterilmektedir. Hangi bir tek ve onu kullanarak kazanabileceğinizi hakkında daha fazla bilgi için bkz. [, özel bir varlık tanımlamak için singleton kullanarak](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx). Web API'de OData V4 uç nokta oluşturmak için bkz [OData v4 uç noktası kullanarak ASP.NET Web API 2.2 oluşturma](create-an-odata-v4-endpoint.md). 

Aşağıdaki veri modelini kullanarak Web API projesinde bir singleton oluşturacağız:

![Veri modeli](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

Adlı bir singleton `Umbrella` türüne göre tanımlanır `Company`ve bir varlık adlandırılmış kümesi `Employees` türüne göre tanımlanır `Employee`.

Bu öğreticide kullanılan çözüm indirilebileceğini [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).

## <a name="define-the-data-model"></a>Veri modelini tanımlama

1. CLR Türleri tanımlar.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. CLR türlerine göre EDM modelini oluşturun.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    Burada, `builder.Singleton<Company>("Umbrella")` adlı bir singleton oluşturmak için model Oluşturucu bildirir `Umbrella` EDM modeli.

    Oluşturulan meta veriler aşağıdaki gibi görünür:

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    Meta verileri görebiliriz gezinme özelliğini `Company` içinde `Employees` varlık kümesi için tekli bağlı `Umbrella`. Bağlama tarafından otomatik olarak yapılır `ODataConventionModelBuilder`, bu yana yalnızca `Umbrella` sahip `Company` türü. Modelde belirsizlik varsa, kullanabileceğiniz `HasSingletonBinding` açıkça bir gezinti özelliği için singleton; bağlamak için `HasSingletonBinding` kullanmakla aynı etkiye sahip `Singleton` CLR tür tanımında öznitelik:

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>Singleton denetleyicisi tanımlayın

Singleton denetleyicisi devraldığı EntitySet denetleyicisi gibi `ODataController`, ve tekil Denetleyici adı olması gereken `[singletonName]Controller`.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

Farklı türde istekleri işlemek için Eylemler denetleyicide önceden tanımlanmış olması gerekir. **Öznitelik yönlendirme** Webapı 2.2 içinde varsayılan olarak etkindir. Örneğin, sorgulama işlemek için bir eylem tanımlamak için `Revenue` gelen `Company` öznitelik yönlendirme, kullanarak aşağıdaki kullanın:

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

Her eylem için öznitelikleri tanımlamak iradeye sahip değilse, aşağıdaki eylemleri tanımlamak [OData yönlendirme kuralları](../odata-routing-conventions.md). Bir anahtarı bir singleton sorgulamak için gerekli olmadığından, tekil denetleyicide tanımlanan biraz farklı eylemler entityset denetleyicide tanımlanan eylemlerdir.

Başvuru için her eylem tanımı tekil denetleyicideki için yöntem imzaları aşağıda listelenmiştir.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

Temel olarak, tüm hizmet tarafında yapmanız gereken budur. [Kodunuzla](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) tüm çözüm ve tekli kullanmayı gösterir OData istemci kodu içerir. İçindeki adımları izleyerek istemci yerleşik [bir OData v4 istemci uygulaması oluşturma](create-an-odata-v4-client-app.md).

biçimindeki telefon numarasıdır. 

*Bu makalede, özgün içerik sayesinde Hu satılmıştır.*
