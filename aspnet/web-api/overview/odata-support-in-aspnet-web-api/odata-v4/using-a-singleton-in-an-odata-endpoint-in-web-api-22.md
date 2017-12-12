---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: "Web API 2.2 kullanarak OData v4 Teklide oluşturma | Microsoft Docs"
author: rick-anderson
description: "Bu konuda, tek bir OData uç Web API'si 2.2 tanımlamak gösterilmiştir."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 92c5056548b1e39defb28ac36f83b001dd108f5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>Web API 2.2 kullanarak OData v4 Teklide oluşturma
====================
Zoe Luo tarafından

> Bir varlık kümesi içinde kapsüllenmiş, geleneksel olarak, bir varlığın yalnızca erişilebilir. Ancak OData v4 Singleton ve her ikisi de Webapı 2.2 destekleyen kapsama olmak üzere iki ek seçenekler sağlar.


Bu makalede, tek bir OData uç Web API'si 2.2 tanımlamak gösterilmiştir. Hangi bir tek ve bunu kullanarak nasıl yardımcı olabileceğini hakkında daha fazla bilgi için bkz: [özel varlığınız tanımlamak için bir singleton kullanarak](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx). Web API'de OData V4 uç noktası oluşturmak için bkz: [bir OData v4 uç nokta kullanarak ASP.NET Web API 2.2 oluşturma](create-an-odata-v4-endpoint.md). 

Aşağıdaki veri modelini kullanarak Web API projesinde tek oluşturacağız:

![Veri modeli](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

Adlı bir singleton `Umbrella` türüne bağlı olarak tanımlanacak `Company`ve bir varlık adlandırılmış kümesi `Employees` türüne bağlı olarak tanımlanacak `Employee`.

Bu öğreticide kullanılan çözüm yüklenebilir [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).

## <a name="define-the-data-model"></a>Veri modelini tanımlama

1. CLR Türleri tanımlayın.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. CLR türlerine göre EDM modeli oluşturur.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    Burada, `builder.Singleton<Company>("Umbrella")` adlı bir singleton oluşturmak için model Oluşturucu söyler `Umbrella` EDM modeli.

    Oluşturulan meta verileri aşağıdaki gibi görünür:

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    Meta verileri, görebiliriz gezinti özelliği `Company` içinde `Employees` varlık kümesi için singleton bağlı `Umbrella`. Bağlama tarafından otomatik olarak yapılır `ODataConventionModelBuilder`, bu yana yalnızca `Umbrella` sahip `Company` türü. Modelde bir belirsizlik ise kullanabilirsiniz `HasSingletonBinding` açıkça bir tekliye; bir gezinti özelliği bağlamak için `HasSingletonBinding` kullanarak aynı etkiye sahip `Singleton` CLR türü tanımı özniteliğinde:

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>Singleton denetleyicisi tanımlayın

Singleton denetleyicisi devraldığı EntitySet denetleyicisi gibi `ODataController`, ve singleton Denetleyici adı olmalıdır `[singletonName]Controller`.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

Farklı türde isteklerini işlemek için Eylemler denetleyicide önceden tanımlanmış olması gerekir. **Öznitelik yönlendirme** Webapı 2.2 varsayılan olarak etkindir. Örneğin, sorgulama işlemek için bir eylem tanımlamak için `Revenue` gelen `Company` özniteliği yönlendirme, kullanımı aşağıdaki:

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

Her eylem öznitelik tanımlamak istekli değilse, aşağıdaki eylemleri tanımlamak [OData yönlendirme kuralları](../odata-routing-conventions.md). Bir anahtar tek sorgulamak için gerekli olmadığından, singleton denetleyicide tanımlanan Eylemler entityset denetleyicide tanımlanan Eylemler biraz farklıdır.

Başvuru için her tek denetleyici eylem tanımında yöntemi imzaları aşağıda listelenmiştir.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

Temel olarak, tüm hizmet tarafında yapmanız gereken budur. [Örnek proje](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) tüm çözüm ve tekli kullanmayı gösterir OData istemci kodunu içerir. İçindeki adımları izleyerek istemci yerleşik [bir OData v4 istemci uygulaması oluşturma](create-an-odata-v4-client-app.md).

biçimindeki telefon numarasıdır. 

*Bu makalenin özgün içerik için Leo Hu teşekkür ederiz.*
