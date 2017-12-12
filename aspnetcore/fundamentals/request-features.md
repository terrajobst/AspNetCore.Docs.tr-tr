---
title: "ASP.NET Core istek özellikleri"
author: ardalis
description: "HTTP istekleri ve yanıtları arabirimlerde ASP.NET Core için tanımlanan ilgili web sunucusu uygulama ayrıntıları hakkında bilgi edinin."
keywords: "ASP.NET Çekirdeği"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d1fbd23c-2ff9-4216-b908-0201ff3afb7c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/request-features
ms.openlocfilehash: b689d82d16c6ef55485691b3474a070765c8144b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
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
> `ISessionFeature`Sunucu özelliği değil, ancak tarafından uygulanan `SessionMiddleware` (bkz [yönetme uygulama durumu](app-state.md)).

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

* [Ara yazılım](middleware.md)

* [.NET (OWIN) için açık Web arabirimi](owin.md)
