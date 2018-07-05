---
uid: web-api/overview/advanced/httpclient-message-handlers
title: ASP.NET Web API'de HttpClient ileti işleyicileri | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2012
ms.topic: article
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 1712f190c5a313c79b7c91b671214dd8972cb3c9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402947"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a>ASP.NET Web API'de HttpClient ileti işleyicileri
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

A *ileti işleyicisi* HTTP isteği aldığında ve bir HTTP yanıtı döndüren bir sınıftır.

Genellikle, bir dizi ileti işleyicileri zincirleme birlikte. İlk işleyici HTTP isteği aldığında, işlem yapar ve bir sonraki işleyici istek sağlar. Belirli bir noktada yanıt oluşturulur ve zincirinde döner. Bu düzen adı verilen bir *temsilci olarak görevlendirme* işleyici.

![](httpclient-message-handlers/_static/image1.png)

İstemci tarafında **HttpClient** sınıfını isteklerini işlemek için bir ileti işleyicisi kullanır. Varsayılan işleyici **HttpClientHandler**, ağ üzerinden isteği gönderir ve sunucudan yanıtı alır. Özel ileti işleyicileri istemci ardışık düzende ekleyebilirsiniz:

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> ASP.NET Web API, sunucu tarafında ileti işleyicileri de kullanır. Daha fazla bilgi için [HTTP ileti işleyicileri](http-message-handlers.md).


## <a name="custom-message-handlers"></a>Özel ileti işleyicileri

Özel ileti işleyicisi yazmak için türetilen **System.Net.Http.DelegatingHandler** ve geçersiz kılma **SendAsync** yöntemi. Aşağıda, yöntem imzası verilmiştir:

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

Yöntem alır bir **HttpRequestMessage** giriş ve zaman uyumsuz olarak döndürür olarak bir **HttpResponseMessage**. Tipik bir uygulaması şunları yapar:

1. İşlem istek iletisi.
2. Çağrı `base.SendAsync` iç işleyici için istek gönderebilirsiniz.
3. İç işleyici, bir yanıt iletisi döndürür. (Bu adım, zaman uyumsuz.)
4. Yanıta işlemek ve çağırana döndürmesi.

Aşağıdaki örnek, bir giden istek için özel bir başlık ekler bir ileti işleyicisini gösterir:

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

Çağrı `base.SendAsync` zaman uyumsuzdur. İşleyici, bu çağrıdan sonra herhangi bir iş varsa, kullanmak **await** yöntemi tamamlandıktan sonra yürütülmesine devam etmek için anahtar sözcüğü. Aşağıdaki örnek, hata kodları günlükleri bir işleyici gösterir. Günlük çok ilginç değil, ancak örnek işleyici içinde yanıt alın nasıl gösterir.

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>İstemci işlem hattına ileti işleyicileri ekleme

Özel işleyicileri eklemek için **HttpClient**, kullanın **HttpClientFactory.Create** yöntemi:

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

İleti işleyicileri içine geçirdiğiniz sırayla çağrılır **Oluştur** yöntemi. Yanıt iletisi işleyicileri nedeniyle, diğer yönde hareket eder. Diğer bir deyişle, son ilk yanıt iletisi işleyicidir.
