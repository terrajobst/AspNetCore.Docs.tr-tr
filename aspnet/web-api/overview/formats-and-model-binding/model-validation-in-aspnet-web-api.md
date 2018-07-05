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
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 75474d5597ba11e09f788269b9a2a278559c63a9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393102"
---
<a name="model-validation-in-aspnet-web-api"></a>ASP.NET Web API'de model doğrulama
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

İstemci web API'nize veri gönderdiğinde, genellikle herhangi bir işlem gerçekleştirmeden önce verileri doğrulamak istediğiniz. Bu makalede, web API'NİZİN doğrulama hataları işlemek modellerinize eklemek ve veri doğrulama için ek açıklamalarını kullanma gösterilmektedir.

## <a name="data-annotations"></a>Veri ek açıklamaları

ASP.NET Web API'de öznitelikleri kullanabilirsiniz [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) modelinize göre özellikler için doğrulama kurallarını ayarlamak için ad alanı. Şu model göz önünde bulundurun:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

ASP.NET MVC'de model doğrulama kullandıysanız, bu tanıdık gelecektir. **Gerekli** öznitelik yazan `Name` özelliği null olmamalıdır. **Aralığı** öznitelik yazan `Weight` sıfır ile 999 arasında olmalıdır.

Bir istemci aşağıdaki JSON gösterimine sahip bir POST isteği gönderir varsayalım:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

İstemci içermediği gördüğünüz `Name` olarak işaretlenmiş özelliği gerekli. Web API dönüştürür JSON'a ne zaman bir `Product` örneği, nasıl doğruladığı `Product` karşı doğrulama öznitelikleri. Denetleyici eyleminizi model geçerli olup olmadığını kontrol edebilirsiniz:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

Model doğrulaması istemci verilerini güvenli olduğu garanti etmez. Ek doğrulama diğer uygulama katmanında gerekli. (Örneğin, veri katmanı yabancı anahtar kısıtlamalarını zorlayabilir.) Öğreticiyi [Entity Framework ile Web API kullanarak](../data/using-web-api-with-entity-framework/part-1.md) bu sorunlardan bazıları keşfediyor.

**"Eksik posta"**: eksik posta istemci bazı özellikleri dışına çıktığında gerçekleşir. Örneğin, aşağıdaki istemcinin gönderdiği varsayalım:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

Burada, istemci için değerleri belirtmediniz `Price` veya `Weight`. JSON biçimlendirici sıfır eksik özellikleri için varsayılan değeri atar.

![](model-validation-in-aspnet-web-api/_static/image1.png)

Bu özellikler için geçerli bir değer sıfır olduğundan model durumu geçerli değil. Bu bir sorun olup olmadığını, senaryoya bağlıdır. Örneğin, bir update işleminde "sıfır" ve "ayarlı değil." ayırt etmek isteyebilirsiniz Bir değer ayarlamak için istemcileri zorlamak için özelliği boş değer atanabilir ve ayarlanmış olun **gerekli** özniteliği:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**"Aşırı gönderme"**: bir istemci de gönderebilirsiniz *daha fazla* beklediğinizden daha veri. Örneğin:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

Burada, JSON yok ("rengi") bir özellik içeren `Product` modeli. Bu durumda, JSON biçimlendirici, yalnızca bu değeri yoksayar. (Aynı XML biçimlendiricisi desteklemez.) Model salt okunur olmasını istediğiniz özellikler varsa fazla posta sorunlara neden olur. Örneğin:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

Güncelleştirilecek kullanıcı istemediğiniz `IsAdmin` özelliği ve kendileri için yöneticileri Yükselt! Güvenli stratejisi, istemcinin göndermek için izin verilenden tam olarak eşleşen bir model sınıfı kullanmaktır:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> Brad Wilson'ın blog gönderisi "[giriş doğrulaması vs. Model doğrulama ASP.NET mvc'de](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"eksik posta ve fazla posta iyi bir tartışma sahiptir. ASP.NET MVC 2 hakkında posta olsa da, sorunları Web API'sine yine de ilgilidir.


## <a name="handling-validation-errors"></a>Doğrulama hataları işleme

Doğrulama başarısız olduğunda web API'si otomatik olarak bir hata istemciye döndürmez. Model durumu denetlemek ve uygun şekilde yanıtlanması için en fazla denetleyici eylemi var.

Denetleyici eylemini çağrılmadan önce model durumunu denetlemek için bir eyleme eylem filtresi de oluşturabilirsiniz. Aşağıdaki kod, bir örnek göstermektedir:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

Bu filtre, model doğrulama başarısız olursa doğrulama hatalarını içeren bir HTTP yanıtı döndürür. Bu durumda, denetleyici eylem çağrılmaz.

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

Tüm Web APİ'si denetleyicilerinin için bu filtre uygulamak için filtre bir örneğini ekleme **HttpConfiguration.Filters** yapılandırma sırasında koleksiyonu:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

Filtre bir özniteliği olarak tek tek denetleyicileri veya denetleyici eylemlerini ayarlamak başka bir seçenektir:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
