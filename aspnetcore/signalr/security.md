---
title: ASP.NET Core signalr'da güvenlik konuları
author: tdykstra
description: Kimlik doğrulama ve yetkilendirme ASP.NET Core SignalR kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 10/17/2018
uid: signalr/security
ms.openlocfilehash: 1adf762cd6de4f0cf62e31c0ec6e595a32ed56f8
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477546"
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

Örneğin, barındırılan bir SignalR tarayıcı istemcisi aşağıdaki CORS ilkesinin sağlar `http://example.com` barındırılan SignalR uygulamaya erişmek için `http://signalr.example.com`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

> [!NOTE]
> SignalR, Azure App Service'te yerleşik CORS özelliği ile uyumlu değil.

## <a name="websocket-origin-restriction"></a>WebSocket kaynak kısıtlama

CORS tarafından sağlanan korumaları WebSockets için geçerli değildir. Tarayıcılar **değil**:

* CORS uçuş öncesi istekler gerçekleştirin.
* Belirtilen kısıtlamalarını dikkate `Access-Control` WebSocket istekleri yaparken üstbilgileri.

Ancak, tarayıcılar gönderebilirsiniz `Origin` WebSocket istekleri gönderirken, üst bilgisi. Uygulamalar, beklenen kaynaklardan gelen WebSockets izin verildiğinden emin olmak için bu üstbilgileri doğrulamak için yapılandırılmalıdır.

ASP.NET Core 2.1 ve sonraki sürümlerinde, üst bilgisi doğrulama yerleştirilen özel bir ara yazılım kullanarak gerçekleştirilebilir **önce `UseSignalR`ve kimlik doğrulaması ara yazılımı** içinde `Configure`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> `Origin` Üst bilgisi, istemcinin ve gibi denetlenir `Referer` başlık sahte. Bu üst gereken **değil** bir kimlik doğrulama mekanizması kullanılır.

## <a name="access-token-logging"></a>Erişim belirteci günlüğü

WebSockets veya Server-Sent olayları kullanırken, tarayıcı istemci erişim belirteci sorgu dizesinde yer gönderir. Sorgu dizesi aracılığıyla erişim belirteci alma standart kullanmak genellikle kadar güvenli `Authorization` başlığı. Ancak, birçok web sunucusu URL'si sorgu dizesi dahil olmak üzere her istek için oturum açın. URL'leri günlüğü erişim belirteci oturum açabilir. Web günlüğü erişim belirteçleri önlemek için sunucunun günlüğe kaydetme ayarlarını en iyi bir uygulamadır.

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
