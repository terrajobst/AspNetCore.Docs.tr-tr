---
title: ASP.NET Core SignalR güvenlik konuları
author: bradygaster
description: ASP.NET Core SignalRkimlik doğrulama ve yetkilendirmeyi nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- SignalR
uid: signalr/security
ms.openlocfilehash: f443fe0fbaaa1facd09edc0878c048772895ecff
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881178"
---
# <a name="security-considerations-in-aspnet-core-opno-locsignalr"></a>ASP.NET Core SignalR güvenlik konuları

, [Andrew Stanton-nurte](https://twitter.com/anurse)

Bu makalede SignalRgüvenliğini sağlama hakkında bilgi sağlanır.

## <a name="cross-origin-resource-sharing"></a>Çıkış noktaları arası kaynak paylaşma

[Çapraz kaynak kaynak paylaşımı (CORS)](https://www.w3.org/TR/cors/) , tarayıcıda çapraz kaynak SignalR bağlantılara izin vermek için kullanılabilir. JavaScript kodu SignalR uygulamasından farklı bir etki alanında barındırılıyorsa, JavaScript 'In SignalR uygulamasına bağlanmasına izin vermek için [CORS ara yazılımı](xref:security/cors) etkinleştirilmelidir. Yalnızca güvendiğiniz veya denetlediğiniz etki alanlarından çıkış noktaları arası isteklere izin verin. Örneğin:

* Siteniz `http://www.example.com` barındırılıyor
* SignalR uygulamanız `http://signalr.example.com` üzerinde barındırılıyor

CORS, SignalR uygulamasında yalnızca kaynak `www.example.com`izin verecek şekilde yapılandırılmalıdır.

CORS 'yi yapılandırma hakkında daha fazla bilgi için bkz. [çıkış noktaları arası istekleri (CORS) etkinleştirme](xref:security/cors). SignalR aşağıdaki CORS ilkelerini **gerektirir** :

* Beklenen belirli kaynaklardan izin verin. Herhangi bir kaynağa izin verilmesi mümkün olsa **da güvenli veya** önerilmez.
* HTTP yöntemlerine `GET` ve `POST` izin verilmelidir.
* Kimlik doğrulaması kullanılmasa bile kimlik bilgilerinin etkinleştirilmesi gerekir.

Örneğin, aşağıdaki CORS ilkesi, `https://example.com` barındırılan SignalR tarayıcısı istemcisinin `https://signalr.example.com`üzerinde barındırılan SignalR uygulamasına erişmesine izin verir:

::: moniker range=">= aspnetcore-3.0"

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder =>
    {
        builder.WithOrigins("https://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<ChatHub>("/chatHub");
    });

    // ... other middleware ...
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

::: moniker-end

> [!NOTE]
> SignalR, Azure App Service yerleşik CORS özelliğiyle uyumlu değildir.

## <a name="websocket-origin-restriction"></a>WebSocket kaynak kısıtlaması

::: moniker range=">= aspnetcore-2.2"

CORS tarafından sunulan korumalar WebSockets için geçerlidir. WebSockets üzerindeki kaynak kısıtlaması için [WebSockets kaynak kısıtlaması](xref:fundamentals/websockets#websocket-origin-restriction)' nı okuyun.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

CORS tarafından sunulan korumalar WebSockets için geçerlidir. Tarayıcılar şunları **desteklemez**:

* CORS ön uçuş istekleri gerçekleştirin.
* WebSocket istekleri yaparken `Access-Control` üst bilgilerinde belirtilen kısıtlamalara saygı.

Ancak, tarayıcılar WebSocket istekleri verirken `Origin` üst bilgisini gönderir. Yalnızca beklenen kaynaklardan gelen WebSockets izin verildiğinden emin olmak için uygulamalar bu üstbilgileri doğrulamak üzere yapılandırılmalıdır.

ASP.NET Core 2,1 ve üzeri sürümlerde, üst bilgi doğrulaması, **`UseSignalR`ve `Configure`kimlik doğrulama ara yazılımı** aracılığıyla bulunan özel bir ara yazılım kullanılarak elde edilebilir:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> `Origin` üstbilgisi istemci tarafından denetlenir ve `Referer` üst bilgisi gibi erişilebilir. Bu üst bilgiler bir kimlik doğrulama mekanizması **olarak kullanılmamalıdır.**

::: moniker-end

## <a name="access-token-logging"></a>Erişim belirteci günlüğe kaydetme

WebSockets veya sunucu tarafından gönderilen olaylar kullanılırken, tarayıcı istemcisi erişim belirtecini sorgu dizesinde gönderir. Sorgu dizesi aracılığıyla erişim belirtecinin alınması genellikle standart `Authorization` üst bilgisi kullanılarak güvenlidir. İstemci ve sunucu arasında güvenli bir uçtan uca bağlantı sağlamak için her zaman HTTPS kullanmanız gerekir. Birçok Web sunucusu, sorgu dizesi dahil olmak üzere her bir isteğin URL 'sini günlüğe kaydeder. URL 'Leri günlüğe kaydetme erişim belirtecini günlüğe alabilir. ASP.NET Core, her isteğin URL 'sini varsayılan olarak günlüğe kaydeder ve bu sorgu dizesini içerir. Örneğin:

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

Bu verileri sunucu günlüklerinize kaydetme hakkında endişeleriniz varsa, `Microsoft.AspNetCore.Hosting` günlükçüsü `Warning` düzeyine veya üstüne yapılandırarak (Bu iletiler `Info` düzeyinde yazılır) bu günlüğü tamamen devre dışı bırakabilirsiniz. Daha fazla bilgi için [günlük filtrelemeye](xref:fundamentals/logging/index#log-filtering) yönelik belgelere bakın. Hala belirli istek bilgilerini günlüğe kaydetmek istiyorsanız, ihtiyacınız olan verileri günlüğe kaydetmek için [bir ara yazılım yazabilir](xref:fundamentals/middleware/write) ve `access_token` sorgu dizesi değerini (varsa) filtreleyebilirsiniz.

## <a name="exceptions"></a>Özel Durumlar

Özel durum iletileri genellikle bir istemciye görüntülenmemelidir gizli veriler olarak değerlendirilir. Varsayılan olarak, SignalR bir hub yöntemi tarafından oluşturulan bir özel durumun ayrıntılarını istemciye göndermez. Bunun yerine, istemci bir hata oluştuğunu belirten genel bir ileti alır. İstemciye özel durum iletisi teslimi, [Enabledetailederrors](xref:signalr/configuration#configure-server-options)ile geçersiz kılınabilir (örneğin, geliştirme veya test). Özel durum iletileri, üretim uygulamalarındaki istemciye gösterilmemelidir.

## <a name="buffer-management"></a>Arabellek Yönetimi

SignalR gelen ve giden iletileri yönetmek için bağlantı başına arabellekleri kullanır. Varsayılan olarak, SignalR bu arabellekleri 32 KB ile sınırlandırır. Bir istemcinin veya sunucunun gönderecan en büyük ileti 32 KB 'tır. İleti bağlantısı tarafından tüketilen maksimum bellek 32 KB 'tır. İletileriniz her zaman 32 KB 'den küçükse, sınırı azaltabilirsiniz ve şunları yapabilirsiniz:

* Bir istemcinin daha büyük bir ileti gönderebilmesini engeller.
* Sunucu, iletileri kabul etmek için hiçbir şekilde büyük arabellekler ayırmayı gerektirmez.

İletileriniz 32 KB 'tan büyükse, limiti artırabilirsiniz. Bu sınırı artırmak şu anlama gelir:

* İstemci, sunucunun büyük bellek arabellekleri ayırmasına neden olabilir.
* Büyük arabelleklerin sunucu ayırması, eşzamanlı bağlantı sayısını azaltabilir.

Gelen ve giden iletiler için her ikisi de `MapHub`' de yapılandırılan [Httpconnectiondispatcheroptions](xref:signalr/configuration#configure-server-options) nesnesinde yapılandırılabilir:

* `ApplicationMaxBufferSize`, istemciden sunucunun arabelleğe aldığı en fazla bayt sayısını temsil eder. İstemci bu sınırdan daha büyük bir ileti göndermemeyi denerse bağlantı kapatılabilir.
* `TransportMaxBufferSize`, sunucunun gönderemediği en fazla bayt sayısını temsil eder. Sunucu, bu sınırdan daha büyük bir ileti (hub metotlarından dönüş değerleri dahil) gönderilmeye çalışırsa, bir özel durum oluşturulur.

Sınırın `0` olarak ayarlanması sınırı devre dışı bırakır. Sınırı kaldırmak, bir istemcinin herhangi bir boyutta ileti göndermesini sağlar. Büyük iletiler gönderen kötü amaçlı istemciler fazla belleğin ayrılmasına neden olabilir. Aşırı bellek kullanımı, eşzamanlı bağlantı sayısını önemli ölçüde azaltabilir.
