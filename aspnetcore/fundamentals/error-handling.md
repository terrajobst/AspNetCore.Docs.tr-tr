---
title: "ASP.NET Core işleme hatası"
author: ardalis
description: "ASP.NET Core uygulamaları hataların nasıl işleneceğini bulur."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/error-handling
ms.openlocfilehash: 5b0cda7b79b8a9523d1ba6a9b321d22d3ccc753a
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a>Hata ASP.NET çekirdek işleme giriş

Tarafından [Steve Smith](https://ardalis.com/) ve [zel Dykstra](https://github.com/tdykstra/)

Bu makalede, ASP.NET Core uygulamaları hataları işlemek için ortak appoaches yer almaktadır.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="the-developer-exception-page"></a>Geliştirici özel durumu sayfası

Özel durumlar hakkında ayrıntılı bilgiler gösterilmektedir bir sayfasını görüntülemek için uygulamayı yapılandırmak için yükleme `Microsoft.AspNetCore.Diagnostics` NuGet paketini ve bir satırı ekleyin [başlangıç sınıfında yöntemini yapılandırma](startup.md):

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

PUT `UseDeveloperExceptionPage` Ara yazılımların, istediğiniz gibi özel durumları yakalamak için önce `app.UseMvc`.

>[!WARNING]
> Geliştirici özel durum sayfasını etkinleştir **yalnızca uygulama geliştirme ortamında çalışırken**. Uygulama üretimde çalıştırıldığında ayrıntılı özel durum bilgilerini herkese açık şekilde paylaşma istemezsiniz. [Ortamlarını yapılandırma hakkında daha fazla bilgi](environments.md).

Geliştirici özel durum sayfasını görmek için ayarlanan ortam ile örnek uygulamayı çalıştırın `Development`ve ekleme `?throw=true` uygulamanın temel URL. Sayfa özel durumu ve istek hakkında bilgi içeren birkaç sekme içerir. İlk sekme yığın izlemesi içerir. 

![Yığın izleme](error-handling/_static/developer-exception-page.png)

Sonraki sekme sorgu dizesi parametreleri varsa gösterir.

![Sorgu dizesi parametreleri](error-handling/_static/developer-exception-page-query.png)

Bu istek tanımlama bilgilerine sahip oldu, ancak olsaydı, üzerinde görünecekleri **tanımlama bilgilerini** sekmesi. Son sekmede geçirilmiş üstbilgileri görebilirsiniz.

![Üstbilgileri](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>Sayfa işleme özel bir durum yapılandırma

Uygulama çalışmadığında kullanmak için bir özel durum işleyici sayfasını yapılandırmak için iyi bir fikirdir `Development` ortamı.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

Bir MVC uygulamasında açıkça HTTP yöntemi öznitelikleriyle hata işleyici eylem yöntemi gibi tasarlamanız yok `HttpGet`. Açık fiillerini kullanarak bazı istekleri yöntemi ulaşmasını engellemeniz.

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a>Yapılandırma durumu kod sayfaları

Varsayılan olarak, uygulamanız için HTTP durum kodları 500 (Dahili Sunucu hatası) veya 404 (bulunamadı) gibi bir zengin durum kod sayfası sağlamayacak. Yapılandırabileceğiniz `StatusCodePagesMiddleware` bir satıra ekleyerek `Configure` yöntemi:

```csharp
app.UseStatusCodePages();
```

Varsayılan olarak, bu ara yazılımın 404 gibi ortak durum kodları için basit, yalnızca metin işleyiciler ekler:

![404 sayfası](error-handling/_static/default-404-status-code.png)

Ara yazılım birkaç farklı genişletme yöntemleri destekler. Lambda ifadesi alır, başka bir içerik türü ve biçimi dizesini alır.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

Yeniden yönlendirme uzantı yöntemleri vardır. Bir 302 durum kodunu istemciye gönderir ve bir istemciye özgün durum kodunu döndüren ancak işleyicinin yeniden yönlendirme URL'si de yürütür.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

Durum kod sayfaları belirli istekleri için devre dışı bırakmanız gerekirse, bunu yapabilirsiniz:

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a>Özel durum işleme kodu

Özel durum işleme sayfaları kodda istisnalar atabilirsiniz. Genellikle, yalnızca statik içeriği oluşması üretim hata sayfaları için iyi bir fikir olabilir.

Ayrıca, yanıt üstbilgileri gönderildikten sonra yanıtın durum kodu değişiklik yapamaz veya herhangi bir özel durum sayfaları veya işleyicileri çalıştırabilirsiniz dikkat edin. Yanıt doldurulmalıdır veya bağlantı durduruldu.

## <a name="server-exception-handling"></a>Sunucu özel durum işleme

Özel durum işleme mantığı, uygulamanızda yanı sıra [server](servers/index.md) uygulamanızı barındırma bazı özel durum işleme gerçekleştirir. Üstbilgileri gönderilmeden önce sunucunun bir özel durum yakalar, sunucunun hiçbir gövde ile 500 İç sunucu hatası yanıt gönderir. Üstbilgileri gönderildikten sonra sunucu bir özel durum yakalar, sunucu bağlantıyı kapatır. Uygulamanız tarafından işlenmeyen isteği sunucu tarafından işlenir. Sunucunun bir özel durum nedeniyle oluşan herhangi bir özel durum işlenmiş işleme. Herhangi bir özel hata sayfaları yapılandırılabilir veya özel durum işleme ara yazılımı veya filtreler bu davranışını etkilemez.

## <a name="startup-exception-handling"></a>Başlangıç özel durum işleme

Yalnızca barındırma katmanı uygulama başlatma sırasında gerçekleşmesi özel durumlar işleyebilir. Yapabilecekleriniz [nasıl konak hatalarına yanıt olarak başlatma sırasında davranacağını yapılandırmak](hosting.md#detailed-errors) kullanarak `captureStartupErrors` ve `detailedErrors` anahtarı.

Ana bilgisayar adresini/bağlantı noktası sonra bağlama hatası oluşursa Barındırma yakalanan başlatma hatası için bir hata sayfası yalnızca gösterebilir. Hiçbir bağlama herhangi bir nedenle başarısız olursa, barındırma katman kritik bir özel durumla, dotnet işlem çökmesi günlüğe kaydeder ve hiçbir hata sayfası görüntülenir.

## <a name="aspnet-mvc-error-handling"></a>ASP.NET MVC hata işleme

[MVC](xref:mvc/overview) uygulamalara sahip özel durum filtreleri yapılandırma ve model doğrulama gerçekleştirme gibi hataları işlemek için bazı ek seçenekleri.

### <a name="exception-filters"></a>Özel durum filtreleri

Özel durum filtreleri, genel olarak veya bir MVC uygulamasında her denetleyici veya eylem başına temelinde yapılandırılabilir. Bu filtreler bir denetleyici eylemi veya başka bir filtre yürütülmesi sırasında oluşan tüm işlenmeyen bir özel durum işleme ve aksi durumda adlı değil. Özel durum filtreleri hakkında daha fazla bilgi [filtreleri](../mvc/controllers/filters.md).

>[!TIP]
> Özel durum filtreleri içinde MVC Eylemler oluşan özel durumlarını yakalama için iyi, ancak bunlar hata ara yazılım işleme kadar esnek değildir. Ara yazılımı genel örneği için tercih ettiğiniz ve hata işleme yapmak için yalnızca ihtiyaç duyacağınız filtreleri kullanın *farklı* MVC eylemi seçildi tabanlı.

### <a name="handling-model-state-errors"></a>İşleme Model durumu hataları

[Model doğrulama](../mvc/models/validation.md) her denetleyici eylemi çağırma öncesi oluşur ve incelemek için eylem yönteminin sorumluluğu olan `ModelState.IsValid` ve uygun şekilde tepki.

Model doğrulama hataları, ilgilenmek için standart bir kural, bu durumda izlemeniz gereken bazı uygulamalar seçecektir bir [filtre](../mvc/controllers/filters.md) böyle bir ilke uygulamak için uygun bir yerdir olabilir. Geçersiz model durumlarıyla eylemlerinizi nasıl davranacağını test etmeniz gerekir. Daha fazla bilgi edinin [denetleyicisi mantığının test edilmesi](../mvc/controllers/testing.md).



