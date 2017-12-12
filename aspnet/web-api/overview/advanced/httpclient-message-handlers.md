---
uid: web-api/overview/advanced/httpclient-message-handlers
title: "ASP.NET Web API'de HttpClient ileti işleyicileri | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2012
ms.topic: article
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 805741b0ac682b7479ce82127df48b1b9a49a427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a>ASP.NET Web API'de HttpClient ileti işleyicileri
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

A *ileti işleyicisi* bir HTTP isteğini alır ve bir HTTP yanıtı döndüren bir sınıftır.

Genellikle, bir dizi ileti işleyicileri zincirleme birlikte. İlk işleyici bir HTTP isteği aldığında, bir işlem yapar ve sonraki işleyicisi isteği verir. Belirli bir noktada yanıtı oluşturulur ve tekrar zincirinde geçer. Bu desen adlı bir *temsilci* işleyicisi.

![](httpclient-message-handlers/_static/image1.png)

İstemci tarafında **HttpClient** sınıfı isteklerini işlemek için bir ileti işleyicisini kullanır. Varsayılan işleyici **HttpClientHandler**, ağ üzerinden isteği gönderir ve sunucudan yanıtı alır. Özel ileti işleyicileri istemci ardışık düzenine ekleyebilirsiniz:

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> ASP.NET Web API, sunucu tarafında ileti işleyicileri de kullanır. Daha fazla bilgi için bkz: [HTTP ileti işleyicileri](http-message-handlers.md).


## <a name="custom-message-handlers"></a>Özel ileti işleyicileri

Özel ileti işleyicisi yazma için türetilen **System.Net.Http.DelegatingHandler** ve geçersiz kılma **SendAsync** yöntemi. Yöntem imzası şöyledir:

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

Yöntem alır bir **HttpRequestMessage** giriş ve zaman uyumsuz olarak döndürür olarak bir **httpresponsemessage öğesini**. Tipik bir uygulama şunları yapar:

1. İşlem istek iletisi.
2. Çağrı `base.SendAsync` iç işleyici isteği göndermek için.
3. İç işleyici bir yanıt iletisi döndürür. (Bu adım, zaman uyumsuz bağlıdır.)
4. Yanıtı işlemek ve çağırana döndürür.

Aşağıdaki örnek, bir özel üst bilgi giden isteğe ekler bir ileti işleyicisini gösterir:

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

Çağrı `base.SendAsync` zaman uyumsuz olarak çağrılır. İşleyici bu çağrısından sonra herhangi bir iş varsa, kullanmak **await** yöntemi tamamlandıktan sonra yürütme devam etmek için anahtar sözcüğü. Aşağıdaki örnek hata kodları günlüklerini bir işleyici gösterir. Günlük çok ilginç değildir, ancak örnek işleyici içinde yanıt alın gösterilmektedir.

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>İstemci ardışık düzene ileti işleyicileri ekleme

Özel işleyicileri eklemek için **HttpClient**, kullanın **HttpClientFactory.Create** yöntemi:

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

İleti işleyicileri bunlara geçirdiğiniz sırayla çağrılır **oluşturma** yöntemi. Yanıt iletisi işleyicileri iç içe geçmiş olduğundan, diğer yönde hareket eder. Diğer bir deyişle, son yanıt iletisi ilk işleyicidir.
