---
title: ASP.NET Core içindeki istek özellikleri
author: ardalis
description: ASP.NET Core arabirimlerde tanımlanan HTTP istekleri ve yanıtları ile ilgili Web sunucusu uygulama ayrıntıları hakkında bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: fundamentals/request-features
ms.openlocfilehash: d0f3ae521d1f314dd04cb581d9a921da4719273d
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2020
ms.locfileid: "79416228"
---
# <a name="request-features-in-aspnet-core"></a>ASP.NET Core içindeki istek özellikleri

[Steve Smith](https://ardalis.com/) tarafından

HTTP istekleri ve yanıtları ile ilgili Web sunucusu uygulama ayrıntıları arabirimlerde tanımlanmıştır. Bu arabirimler, uygulamanın barındırma işlem hattını oluşturmak ve değiştirmek için sunucu uygulamaları ve ara yazılım tarafından kullanılır.

## <a name="feature-interfaces"></a>Özellik arabirimleri

ASP.NET Core, desteklediği özellikleri belirlemek için sunucular tarafından kullanılan `Microsoft.AspNetCore.Http.Features` bir dizi HTTP özelliği arabirimini tanımlar. Aşağıdaki özellik arabirimleri istekleri ve dönüş yanıtlarını işler:

`IHttpRequestFeature` protokol, yol, sorgu dizesi, üst bilgiler ve gövde dahil olmak üzere bir HTTP isteğinin yapısını tanımlar.

`IHttpResponseFeature`, yanıtın durum kodu, üstbilgileri ve gövdesi dahil olmak üzere bir HTTP yanıtının yapısını tanımlar.

`IHttpAuthenticationFeature`, bir `ClaimsPrincipal` göre kullanıcıları tanımlama ve kimlik doğrulama işleyicisi belirleme desteğini tanımlar.

`IHttpUpgradeFeature`, istemci, sunucu protokolleri değiştirmek isterse, istemcinin kullanmak istediğiniz ek protokolleri belirtmesini sağlayan [http yükseltmeleri](https://tools.ietf.org/html/rfc2616.html#section-14.42)Için desteği tanımlar.

`IHttpBufferingFeature` istek ve/veya yanıtların arabelleğe almayı devre dışı bırakma yöntemlerini tanımlar.

`IHttpConnectionFeature`, yerel ve uzak adresler ve bağlantı noktaları için özellikleri tanımlar.

`IHttpRequestLifetimeFeature` bağlantıları iptal etme desteğini tanımlar veya bir isteğin erken sonlandırılıp sonlandırılmayacağını (örneğin, bir istemci bağlantısı kesildiğini) belirler.

`IHttpSendFileFeature`, zaman uyumsuz olarak dosya göndermek için bir yöntem tanımlar.

`IHttpWebSocketFeature`, Web yuvalarını desteklemek için bir API tanımlar.

`IHttpRequestIdentifierFeature`, istekleri benzersiz şekilde tanımlamak için uygulanabilecek bir özellik ekler.

`ISessionFeature`, kullanıcı oturumlarını desteklemek için `ISessionFactory` ve `ISession` soyutlamalarını tanımlar.

`ITlsConnectionFeature` istemci sertifikalarını almak için bir API tanımlar.

`ITlsTokenBindingFeature`, TLS belirteci bağlama parametreleriyle çalışmaya yönelik yöntemleri tanımlar.

> [!NOTE]
> `ISessionFeature` bir sunucu özelliği değildir, ancak `SessionMiddleware` tarafından uygulanır (bkz. [uygulama durumunu yönetme](app-state.md)).

## <a name="feature-collections"></a>Özellik koleksiyonları

`HttpContext` `Features` özelliği, geçerli istek için kullanılabilir HTTP özelliklerini almak ve ayarlamak için bir arabirim sağlar. Özellik koleksiyonu bir istek bağlamı içinde bile değişebilir olduğundan, ara yazılım, koleksiyonu değiştirmek ve ek özellikler için destek eklemek üzere kullanılabilir.

## <a name="middleware-and-request-features"></a>Ara yazılım ve istek özellikleri

Sunucular Özellik koleksiyonu oluşturmaktan sorumludur, ancak ara yazılım bu koleksiyona ekleyebilir ve koleksiyondaki özellikleri kullanabilir. Örneğin, `StaticFileMiddleware` `IHttpSendFileFeature` özelliğine erişir. Özellik varsa, istenen statik dosyayı fiziksel yolundan göndermek için kullanılır. Aksi takdirde, dosyayı göndermek için daha yavaş bir alternatif yöntem kullanılır. `IHttpSendFileFeature`, işletim sisteminin dosyayı açmasına ve ağ kartına doğrudan bir çekirdek modu kopyalaması gerçekleştirmesine izin verir.

Ayrıca, ara yazılım, sunucu tarafından belirlenen özellik koleksiyonuna eklenebilir. Mevcut özellikler, ara yazılım tarafından da değiştirilebilir ve bu da ara yazılım, sunucunun işlevselliğini artırabilir. Koleksiyona eklenen özellikler, daha sonra istek ardışık düzeninde bulunan diğer ara yazılım veya temeldeki uygulamanın kendisi için de kullanılabilir.

Özel sunucu uygulamalarını ve belirli ara yazılım geliştirmelerini birleştirerek, bir uygulamanın gerektirdiği kesin özellikler kümesi oluşturulabilir. Bu, sunucuda değişiklik gerektirmeden eksik özelliklerin eklenmesine izin verir ve yalnızca en düşük miktarda özelliğin gösterilmesini sağlar, böylece saldırı yüzeyi alanını kısıtlar ve performansı geliştirir.

## <a name="summary"></a>Özet

Özellik arabirimleri, belirli bir isteğin destekleyebileceği belirli HTTP özelliklerini tanımlar. Sunucular, özellik koleksiyonlarını ve bu sunucu tarafından desteklenen ilk özellik kümesini tanımlar, ancak bu özellikleri geliştirmek için ara yazılım kullanılabilir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Sunucular](xref:fundamentals/servers/index)
* [Ara Yazılım](xref:fundamentals/middleware/index)
* [.NET için Açık Web Arabirimi (OWIN)](xref:fundamentals/owin)
