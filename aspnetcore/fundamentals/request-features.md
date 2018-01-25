---
title: "ASP.NET Core istek özellikleri"
author: ardalis
description: "HTTP istekleri ve yanıtları arabirimlerde ASP.NET Core için tanımlanan ilgili web sunucusu uygulama ayrıntıları hakkında bilgi edinin."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/request-features
ms.openlocfilehash: f0e371f5ea6c6688ef32adcacf667a412e4625e5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="request-features-in-aspnet-core"></a>ASP.NET Core istek özellikleri

Tarafından [Steve Smith](https://ardalis.com/)

Web sunucusu uygulama ayrıntılarını ilgili HTTP istekleri ve yanıtları arabirimlerde tanımlanır. Bu arabirimleri oluşturma ve uygulama barındırma ardışık düzen değiştirme sunucu uygulamaları ve ara yazılım tarafından kullanılır.

## <a name="feature-interfaces"></a>Özelliği arabirimleri

ASP.NET Core HTTP özelliği arabirimlerde sayısını tanımlar `Microsoft.AspNetCore.Http.Features` hangi sunucuları tarafından destekledikleri özellikleri tanımlamak için kullanılır. Aşağıdaki özellik arabirimleri isteklerini işler ve yanıtları döndürür:

`IHttpRequestFeature`Protokol, yol, sorgu dizesi, üstbilgiler ve gövde dahil olmak üzere bir HTTP isteği yapısını tanımlar.

`IHttpResponseFeature`Durum kodu, üstbilgiler ve yanıtın gövdesini içeren bir HTTP yanıtının yapısını tanımlar.

`IHttpAuthenticationFeature`Temel alarak kullanıcılara tanımlamak için destek tanımlayan bir `ClaimsPrincipal` ve bir kimlik doğrulama işleyicisi belirtme.

`IHttpUpgradeFeature`Desteğini tanımlar [HTTP yükseltmeler](https://tools.ietf.org/html/rfc2616.html#section-14.42), ek, iletişim kurallarını belirtmek istemci olanak sunucu protokolleri geçiş isterse kullanmak istersiniz.

`IHttpBufferingFeature`Arabelleğe alma isteklerini ve/veya yanıtlarını devre dışı bırakmak için yöntemleri tanımlar.

`IHttpConnectionFeature`Yerel ve uzak adresleri ve bağlantı noktaları özelliklerini tanımlar.

`IHttpRequestLifetimeFeature`Bağlantıları durduruluyor ya da bir istek erken, böyle bir istemcinin bağlantısının kesildiği gibi tarafından sonlanıp sonlanmadığını algılama desteği tanımlar.

`IHttpSendFileFeature`Dosyaları zaman uyumsuz olarak göndermek için bir yöntem tanımlar.

`IHttpWebSocketFeature`Bir API web yuvalarını desteklemek için tanımlar.

`IHttpRequestIdentifierFeature`İstekleri benzersiz şekilde tanımlamak için uygulanan bir özellik ekler.

`ISessionFeature`Tanımlar `ISessionFactory` ve `ISession` kullanıcı oturumlarını desteklemek için soyutlamalar.

`ITlsConnectionFeature`İstemci sertifikaları almak için bir API tanımlar.

`ITlsTokenBindingFeature`TLS belirteci bağlama parametreleri ile çalışmak için yöntemleri tanımlar.

> [!NOTE]
> `ISessionFeature`bir sunucu özelliği olmayan, ancak tarafından uygulanan `SessionMiddleware` (bkz [yönetme uygulama durumu](app-state.md)).

## <a name="feature-collections"></a>Özellik koleksiyonları

`Features` Özelliği `HttpContext` geçerli istek için kullanılabilir HTTP özellikleri ayarlama ve alma için bir arabirim sağlar. Özellik koleksiyonu bile bir istek bağlamı içinde değişebilir olduğundan, ara yazılım koleksiyonu değiştirmek ve ek özellikler için destek eklemek için kullanılabilir.

## <a name="middleware-and-request-features"></a>Ara yazılım ve istek özellikleri

Sunucuları özellik koleksiyonu oluşturmaktan sorumlu olsa da, ara yazılımı bu koleksiyona eklemek hem koleksiyondan özellikleri kullanmak olabilir. Örneğin, `StaticFileMiddleware` erişen `IHttpSendFileFeature` özelliği. Özellik varsa, istenen statik dosyanın fiziksel yoldan göndermek için kullanılır. Aksi takdirde, daha yavaş alternatif bir yöntemi, dosya göndermek için kullanılır. Kullanılabilir olduğunda `IHttpSendFileFeature` dosyasını açın ve bir ağ kartı doğrudan çekirdek modu kopyaya gerçekleştirmek işletim sistemi sağlar.

Ayrıca, Ara sunucu tarafından oluşturulmuş özellik koleksiyonu ekleyebilirsiniz. Var olan özellikleri bile sunucunun işlevselliğini genişletmek ara yazılım izin vererek ara yazılımı tarafından değiştirilebilir. Koleksiyona eklenen özellikler, diğer ara yazılımdan veya temel uygulamada kendisini daha sonra isteği ardışık düzeni için hemen kullanılabilir.

Özel sunucu uygulamaları ve belirli Ara geliştirmeler birleştirerek, bir uygulama gerektiren özellikler kesin kümesi oluşturulabilir. Eksik Server'daki bir değişiklik gerektirmeden eklenecek özellikleri ve özellikleri yalnızca en az miktarda açığa, böylece saldırı sınırlaması sağlar böylece yüzey alanını ve performans artırılır.

## <a name="summary"></a>Özet

Özellik arabirimler belirtilen bir isteğin destekleyebilir belirli HTTP özellikleri tanımlar. Koleksiyonlar özelliklerinin ve bu sunucu tarafından desteklenen özellikler başlangıç kümesi sunucuları tanımlamak, ancak ara yazılım, bu özellikleri geliştirmek için kullanılabilir.

## <a name="additional-resources"></a>Ek Kaynaklar

* [Sunucular](servers/index.md)

* [Ara Yazılım](middleware.md)

* [.NET için Açık Web Arabirimi (OWIN)](owin.md)
