---
title: ASP.NET core'da istek özellikleri
author: ardalis
description: HTTP isteklerini ve yanıtlarını arabirimlerde, ASP.NET Core için tanımlanan ilgili web sunucusu uygulaması ayrıntıları hakkında bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: fundamentals/request-features
ms.openlocfilehash: d0f3ae521d1f314dd04cb581d9a921da4719273d
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087027"
---
# <a name="request-features-in-aspnet-core"></a>ASP.NET core'da istek özellikleri

Tarafından [Steve Smith](https://ardalis.com/)

Web sunucusu uygulaması ayrıntıları HTTP istekleriyle ilgili ve yanıtları arabirimlerde tanımlanır. Bu arabirimler, uygulamanın barındırma işlem hattı oluşturup için sunucu uygulamaları ve ara yazılım tarafından kullanılır.

## <a name="feature-interfaces"></a>Özelliği arabirimleri

ASP.NET Core tanımlayan bir dizi HTTP özelliği arabirimlerde `Microsoft.AspNetCore.Http.Features` destekledikleri özellikleri tanımlamak için sunucuları tarafından kullanılır. Aşağıdaki özellik arabirimleri isteklerini işlemek ve yanıtları döndürür:

`IHttpRequestFeature` Protokol, yol, sorgu dizesi, üstbilgi ve gövde içeren bir HTTP isteği yapısını tanımlar.

`IHttpResponseFeature` Bir HTTP yanıtının durum kodu, üst bilgiler ve yanıt gövdesinin gibi yapısını tanımlar.

`IHttpAuthenticationFeature` Temel kullanıcı tanımlamaya yönelik desteği tanımlar bir `ClaimsPrincipal` belirterek bir kimlik doğrulama işleyicisi.

`IHttpUpgradeFeature` Desteğini tanımlar [HTTP yükseltmeleri](https://tools.ietf.org/html/rfc2616.html#section-14.42), istemci, ek protokoller belirtmek izin veren sunucu protokolleri geçmek istiyorsa kullanmak istiyorsunuz.

`IHttpBufferingFeature` İstek ve/veya yanıtlarını arabelleğe almayı devre dışı bırakmak için yöntemleri tanımlar.

`IHttpConnectionFeature` Yerel ve uzak adresler ve bağlantı noktaları için özellikleri tanımlar.

`IHttpRequestLifetimeFeature` Bağlantıları durduruluyor ya da bir isteği beklenenden önce gibi olarak bir istemci bağlantıyı kesme tarafından sonlanıp sonlanmadığını algılama desteği tanımlar.

`IHttpSendFileFeature` Dosyaları zaman uyumsuz olarak gönderme yöntemi tanımlar.

`IHttpWebSocketFeature` Web yuvaları desteklemek için bir API tanımlar.

`IHttpRequestIdentifierFeature` İstekleri benzersiz olarak tanımlanabilmesi için uygulanan bir özellik ekler.

`ISessionFeature` Tanımlar `ISessionFactory` ve `ISession` soyutlama kullanıcı oturumlarını destekleme.

`ITlsConnectionFeature` İstemci sertifikaları almak için bir API tanımlar.

`ITlsTokenBindingFeature` TLS belirteç bağlama parametreleri ile çalışmak için yöntemleri tanımlar.

> [!NOTE]
> `ISessionFeature` Sunucu özelliği olmayan, ancak tarafından uygulanan `SessionMiddleware` (bkz [yönetme uygulama durumu](app-state.md)).

## <a name="feature-collections"></a>Özellik koleksiyonları

`Features` Özelliği `HttpContext` alma ve ayarlama geçerli istek için kullanılabilir HTTP özellikleri için bir arabirim sağlar. Özellik koleksiyonu bile bir istek bağlamı içinde değişebilir olduğundan, ara yazılım koleksiyonu değiştirmek ve ek özellikleri için destek eklemek için kullanılabilir.

## <a name="middleware-and-request-features"></a>Ara yazılım ve istek özellikleri

Sunucuları özellik koleksiyonu oluşturmaktan sorumlu olsa da, ara yazılım bu koleksiyona eklemek hem koleksiyonunun özelliklerini kullanmak olabilir. Örneğin, `StaticFileMiddleware` erişen `IHttpSendFileFeature` özelliği. Özellik zaten varsa, fiziksel yolundan istenen statik dosya göndermek için kullanılır. Aksi takdirde, daha yavaş bir alternatif yöntem dosyayı göndermek için kullanılır. Kullanılabilir olduğunda, `IHttpSendFileFeature` dosyasını açın ve bir ağ kartına doğrudan çekirdek modu kopyalama işlemini gerçekleştirmek işletim sistemi sağlar.

Ayrıca, Ara sunucu tarafından oluşturulan özellik koleksiyonu ekleyebilirsiniz. Mevcut özellikler bile Ara sunucu işlevlerini genişletmek izin verme ara yazılımı tarafından değiştirilebilir. Koleksiyona eklenen özellikler, diğer ara yazılımdan veya temel uygulamada kendisini daha sonra istek ardışık düzenini için hemen kullanılabilir.

Özel sunucu uygulamaları ve belirli bir ara yazılım geliştirmeler birleştirerek özellikleri gerektiren bir uygulama hassas dizi oluşturulabilir. Eksik sunucu değişikliğe gerek kalmadan eklenecek özellikleri ve yalnızca en az miktarda özellikler sunulur, böylece saldırı sınırlama sağlar böylece yüzey alanını ve performansını iyileştirme.

## <a name="summary"></a>Özet

Özellik arabirimler, belirtilen bir isteğin destekleyebilir belirli HTTP özellikleri tanımlar. Özellikleri koleksiyonları ve sunucu tarafından desteklenen özellikler ilk dizi sunucuları tanımlamak, ancak ara yazılım, bu özellikleri geliştirmek için kullanılabilir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Sunucular](xref:fundamentals/servers/index)
* [Ara Yazılım](xref:fundamentals/middleware/index)
* [.NET için Açık Web Arabirimi (OWIN)](xref:fundamentals/owin)
