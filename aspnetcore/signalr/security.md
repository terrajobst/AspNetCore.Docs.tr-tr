---
title: ASP.NET Core signalr'da güvenlik konuları
author: bradygaster
description: Kimlik doğrulama ve yetkilendirme ASP.NET Core SignalR kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/security
ms.openlocfilehash: 6e9f849ed856cf1cbf989b8b16cab5209c465471
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898646"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a>ASP.NET Core signalr'da güvenlik konuları

Tarafından [Andrew Stanton-Nurse](https://twitter.com/anurse)

Bu makalede, SignalR güvenli hale getirme hakkında bilgi sağlar.

## <a name="cross-origin-resource-sharing"></a>Çıkış noktaları arası kaynak paylaşımı

[Çıkış noktaları arası kaynak paylaşımı (CORS)](https://www.w3.org/TR/cors/) tarayıcıda çıkış noktaları arası SignalR bağlantılara izin vermek için kullanılabilir. JavaScript kodu farklı bir etki alanı, SignalR uygulamadan üzerinde barındırılıyorsa [CORS ara yazılımı](xref:security/cors) SignalR uygulamaya bağlanmak için JavaScript'e izin vermeniz etkinleştirilmesi gerekir. Çıkış noktaları arası istekleri yalnızca güvendiğiniz etki alanları veya denetim sağlar. Örneğin:

* Sitenizi üzerinde barındırılır `http://www.example.com`
* SignalR uygulamanız üzerinde barındırılır `http://signalr.example.com`

CORS SignalR uygulamada yalnızca kaynak izin verecek şekilde yapılandırılmalıdır `www.example.com`.

CORS yapılandırma hakkında daha fazla bilgi için bkz. [etkinleştirme çıkış noktaları arası istekleri (CORS)](xref:security/cors). SignalR **gerektirir** aşağıdaki CORS kuralları:

* Belirli beklenen kaynakları sağlar. Her türlü kaynağa izin verme mümkündür ancak olan **değil** güvenli veya önerilir.
* HTTP yöntemleri `GET` ve `POST` izin verilmesi gerekir.
* Hatta kimlik doğrulama olmadığında kimlik bilgileri etkinleştirilmelidir.

Örneğin, barındırılan bir SignalR tarayıcı istemcisi aşağıdaki CORS ilkesinin sağlar `https://example.com` barındırılan SignalR uygulamaya erişmek için `https://signalr.example.com`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

> [!NOTE]
> SignalR, Azure App Service'te yerleşik CORS özelliği ile uyumlu değil.

## <a name="websocket-origin-restriction"></a>WebSocket kaynak kısıtlama

::: moniker range=">= aspnetcore-2.2"

CORS tarafından sağlanan korumaları WebSockets için geçerli değildir. WebSockets temel kaynak kısıtlama için okuma [WebSockets kaynak kısıtlama](xref:fundamentals/websockets#websocket-origin-restriction).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

CORS tarafından sağlanan korumaları WebSockets için geçerli değildir. Tarayıcılar **değil**:

* CORS uçuş öncesi istekler gerçekleştirin.
* Belirtilen kısıtlamalarını dikkate `Access-Control` WebSocket istekleri yaparken üstbilgileri.

Ancak, tarayıcılar gönderebilirsiniz `Origin` WebSocket istekleri gönderirken, üst bilgisi. Uygulamalar, beklenen kaynaklardan gelen WebSockets izin verildiğinden emin olmak için bu üstbilgileri doğrulamak için yapılandırılmalıdır.

ASP.NET Core 2.1 ve sonraki sürümlerinde, üst bilgisi doğrulama yerleştirilen özel bir ara yazılım kullanarak gerçekleştirilebilir **önce `UseSignalR`ve kimlik doğrulaması ara yazılımı** içinde `Configure`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> `Origin` Üst bilgisi, istemcinin ve gibi denetlenir `Referer` başlık sahte. Bu üst gereken **değil** bir kimlik doğrulama mekanizması kullanılır.

::: moniker-end

## <a name="access-token-logging"></a>Erişim belirteci günlüğü

WebSockets veya Server-Sent olayları kullanırken, tarayıcı istemci erişim belirteci sorgu dizesinde yer gönderir. Sorgu dizesi aracılığıyla erişim belirteci alma standart kullanmak genellikle kadar güvenli `Authorization` başlığı. İstemci ve sunucu arasında güvenli bir uçtan uca bağlantı sağlamak için her zaman HTTPS kullanmanız gerekir. Birçok web sunucusu URL'si sorgu dizesi dahil olmak üzere her istek için oturum açın. URL'leri günlüğü erişim belirteci oturum açabilir. ASP.NET Core her istek için URL sorgu dizesini içeren varsayılan olarak günlüğe kaydeder. Örneğin:

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

Bu veri günlüğü, sunucu günlükleri ile ilgili endişeleriniz varsa, bu günlük tamamen yapılandırarak devre dışı bırakabilirsiniz `Microsoft.AspNetCore.Hosting` için Günlükçü `Warning` düzeyi veya üzeri (Bu iletiler yazıldığı `Info` düzeyi). İlgili belgelere bakın [günlük filtreleme](xref:fundamentals/logging/index#log-filtering) daha fazla bilgi için. Belirli istek bilgileri günlüğe kaydetmek isterseniz, [bir ara yazılım yazma](xref:fundamentals/middleware/write) gerektirir ve filtrelemek verilerini günlüğe kaydetmek `access_token` sorgu dize değeri (varsa).

## <a name="exceptions"></a>Özel Durumlar

Özel durum iletileri istemciye ortaya olmamalıdır hassas verileri genel olarak kabul edilir. Varsayılan olarak, SignalR için istemci bir hub yöntemi tarafından oluşturulan bir özel durum ayrıntılarını göndermez. Bunun yerine, istemci bir hata oluştupunu genel bir ileti alır. Özel durum iletisi teslim istemciye geçersiz kılınabilir (örneğin, geliştirme veya test) ile [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options). Özel durum iletileri, üretim uygulamaları istemciye sunulmamalıdır.

## <a name="buffer-management"></a>Arabellek Yönetimi

SignalR bağlantı başına arabellekler gelen ve giden iletileri yönetmek için kullanır. Varsayılan olarak, bu arabellekleri 32 KB SignalR sınırlar. Bir istemci veya sunucu gönderebilirsiniz en büyük ileti, 32 KB'dir. İletiler için bir bağlantı tarafından kullanılan en fazla bellek 32 KB'tır. İletilerinizi 32 KB'den küçük varsa, her zaman sınırı azaltabilir:

* Bir istemci daha büyük bir ileti göndermek engeller.
* Sunucu, hiçbir zaman ileti kabul etmek için büyük arabellekler ayrılacak gerekir.

İletilerinizi 32 KB'den büyükse, sınırı artırabilirsiniz. Bu sınırı artırmak anlamına gelir:

* İstemci, büyük bellek arabelleği ayrılamadı. sunucu neden olabilir.
* Server ayırma büyük arabellek boyutunu eş zamanlı bağlantı sayısını azaltabilir.

Gelen ve giden iletiler için sınırları vardır, her ikisi de yapılandırılabilir [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) yapılandırılmış nesne `MapHub`:

* `ApplicationMaxBufferSize` istemciden en büyük bayt sayısını temsil eder, sunucu arabellekleri. Bu sınırdan daha büyük bir ileti göndermek istemci çalışırsa, bağlantı kapalı.
* `TransportMaxBufferSize` en büyük sunucu gönderebilirsiniz bayt sayısını temsil eder. Bu sınırdan daha büyük (dahil olmak üzere dönüş değerleri hub yöntemleri) bir ileti göndermek sunucu çalışırsa, bir özel durum oluşturulur.

Sınırı ayarını `0` sınırını devre dışı bırakır. Sınır kaldırma, her boyuttaki bir ileti göndermek bir istemci sağlar. Büyük ileti gönderme kötü amaçlı istemciler, aşırı bellek ayrılmasını neden olabilir. Aşırı bellek kullanımı, eş zamanlı bağlantı sayısını önemli ölçüde azaltabilir.
