---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Model doğrulama ASP.NET Web API'de | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 409a91eceb8baa48a7dded1b850d59a27cec2c60
ms.sourcegitcommit: 5ae0c125ee3bbd324edef3818d1d160f4dd84602
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34224732"
---
<a name="model-validation-in-aspnet-web-api"></a>ASP.NET Web API'de model doğrulama
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

Bir istemci web API'nize veri gönderdiğinde, genellikle verileri herhangi bir işlem yapmadan önce doğrulamak istediğiniz. Bu makalede, Modellerinizi açıklama, veri doğrulama için ek açıklamalar kullanmak ve web API doğrulama hataları işlemek gösterilmektedir.

## <a name="data-annotations"></a>Veri ek açıklamaları

ASP.NET Web API'de özniteliklerini kullanabilirsiniz [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) modelinize göre özellikleri için doğrulama kuralları ayarlamak için ad alanı. Aşağıdaki model göz önünde bulundurun:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

ASP.NET MVC model doğrulama kullandıysanız, bu tanıdık gelecektir. **Gerekli** özniteliği diyor `Name` özelliği null olmamalıdır. **Aralığı** özniteliği diyor `Weight` sıfır ile 999 arasında olmalıdır.

Bir istemci bir POST isteği aşağıdaki JSON gösterimi ile gönderir varsayın:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

İstemci içermediği görebilirsiniz `Name` olarak işaretlenmiş özelliği gerekli. Web API dönüştürür JSON içinde ne zaman bir `Product` örneği doğruladığı `Product` karşı doğrulama öznitelikleri. Denetleyici eyleminizi modelin geçerli olup olmadığını kontrol edin:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

Model doğrulama istemci verilerini güvenli olduğu garanti etmez. Ek doğrulama diğer uygulama katmanında gerekli. (Örneğin, veri katmanı yabancı anahtar kısıtlamaları zorlayabilir.) Öğretici [Entity Framework ile Web API kullanarak](../data/using-web-api-with-entity-framework/part-1.md) bu sorunlardan bazıları araştırır.

**"Eksik nakil"**: eksik nakil istemci bazı özellikleri ayrıldığında olur. Örneğin, istemci aşağıdaki gönderir varsayın:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

Burada, istemci için değerler belirtmedi `Price` veya `Weight`. JSON formatter sıfır eksik özellikleri için varsayılan değeri atar.

![](model-validation-in-aspnet-web-api/_static/image1.png)

Bu özellikler için geçerli bir değer sıfır olduğundan model durumu geçerli değil. Bu bir sorun olup olmadığını, senaryoya bağlıdır. Örneğin, bir güncelleştirme işleminde, "sıfır" ve "ayarlı değil." ayırt etmek isteyebilirsiniz Bir değer ayarlamak için istemcileri zorlamak üzere özelliği boş değer atanabilir ayarlanmış yapıp **gerekli** özniteliği:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**"Aşırı gönderim"**: bir istemci de gönderebilirsiniz *daha fazla* , beklenen veri. Örneğin:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

Burada, JSON bulunmayan bir özelliği ("renk") içeren `Product` modeli. Bu durumda, JSON biçimlendirici yalnızca bu değer yok sayar. (Aynı XML biçimlendiricisi desteklemez.) Model salt okunur olarak kullanılması özelliklere sahipse atlayarak nakil sorunlara neden olur. Örneğin:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

Güncelleştirmek için kullanıcıların istemediğiniz `IsAdmin` özelliği ve kendilerini yöneticilere! Güvenli stratejisi istemci ne göndermesine izin verildiğinden tam olarak eşleşen bir model sınıfı kullanmaktır:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> Brad Wilson'ın blog gönderisi "[giriş doğrulama vs. ASP.NET MVC doğrulama model](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"eksik nakil ve atlayarak nakil iyi bir tartışma vardır. Post ASP.NET MVC 2 hakkında olsa da, Web API için hala ilgili bir sorunlardır.


## <a name="handling-validation-errors"></a>Doğrulama hataları işleme

Doğrulama başarısız olduğunda web API otomatik olarak bir hata istemciye döndürmez. Model durumunu denetleyin ve uygun şekilde yanıt vermek için en fazla denetleyici eylemi var.

Denetleyici eylemini çağrılmadan önce model durumunu denetlemek için bir eylem filtresi oluşturabilirsiniz. Aşağıdaki kod örneği gösterir:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

Model doğrulama başarısız olursa, bu filtre doğrulama hatalarını içeren bir HTTP yanıtı döndürür. Bu durumda, denetleyici eylem çağrılır.

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

Tüm Web API denetleyicilerinin bu filtre uygulamak için filtre örneğini ekleme **HttpConfiguration.Filters** yapılandırma sırasında koleksiyonu:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

Başka bir seçenek filtresi öznitelik olarak tek tek denetleyicileri veya denetleyici eylemleri ayarlamaktır:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
