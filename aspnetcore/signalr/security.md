---
title: ASP.NET Core signalr'da güvenlik konuları
author: tdykstra
description: Kimlik doğrulama ve yetkilendirme ASP.NET Core SignalR kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/security
ms.openlocfilehash: 98b5eb7be87920aacf7a941f76ff652ae7905303
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391264"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a>ASP.NET Core signalr'da güvenlik konuları

Tarafından [Andrew Stanton-Nurse](https://twitter.com/anurse)

## <a name="overview"></a>Genel Bakış

SignalR, varsayılan olarak birkaç güvenlik koruması sağlar. Bu korumalar yapılandırma anlamak önemlidir.

### <a name="cross-origin-resource-sharing"></a>Çıkış noktaları arası kaynak paylaşımı

[Çıkış noktaları arası kaynak paylaşımı (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) tarayıcıda çıkış noktaları arası SignalR bağlantılara izin vermek için kullanılabilir. JavaScript kodunuza, SignalR uygulamanızı farklı bir etki alanı adı üzerinde barındırılıyorsa, etkinleştirmek için sahip [ASP.NET Core CORS ara yazılımı](xref:security/cors) bağlantılara izin vermek için. Genel olarak, çıkış noktaları arası istekleri yalnızca denetim etki alanlarından izin verir. Örneğin, siteniz üzerinde barındırılıyorsa `http://www.example.com` ve SignalR uygulamanız üzerinde barındırıldığı `http://signalr.example.com`, CORS SignalR uygulamanızı yalnızca kaynak izin verecek şekilde yapılandırmanız gerekir `www.example.com`.

CORS yapılandırma hakkında daha fazla bilgi için bkz. [ASP.NET Core CORS belgelerine](xref:security/cors). SignalR düzgün çalışması için aşağıdaki CORS ilkelerini gerektirir:

* İlkeyi belirli kaynakları beklemez veya izin ver (önerilmez) her türlü kaynağa izin vermeniz gerekir.
* HTTP yöntemleri `GET` ve `POST` izin verilmesi gerekir.
* Bile kimlik doğrulaması kullanmadığınızda, kimlik bilgileri etkinleştirilmelidir.

Örneğin, barındırılan bir SignalR tarayıcı istemcisi aşağıdaki CORS ilkesinin sağlar `http://example.com` SignalR uygulamanıza erişmek için:

```csharp
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder => {
        builder.WithOrigins("http://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> SignalR, Azure App Service'te yerleşik CORS özelliği ile uyumlu değil.

### <a name="websocket-origin-restriction"></a>WebSocket kaynak kısıtlama

CORS tarafından sağlanan korumaları WebSockets için geçerli değildir. Tarayıcılar CORS uçuş öncesi isteklerini gerçekleştirme ya da bunlar belirtilen kısıtlamalarını dikkate `Access-Control` WebSocket istekleri yaparken üstbilgileri. Ancak, tarayıcılar gönderebilirsiniz `Origin` WebSocket istekleri gönderirken, üst bilgisi. İzin verilen beklediğiniz kaynaklardan gelen yalnızca WebSockets emin olmak için bu üstbilgileri doğrulamayı uygulamanızı yapılandırmanız gerekir.

ASP.NET Core 2.1 içinde bu yerleştirebileceğiniz özel bir ara yazılım kullanarak gerçekleştirilebilir **yukarıda `UseSignalR`ve herhangi bir kimlik doğrulaması ara yazılımı** içinde `Configure` yöntemi:

```csharp
// In your Startup class, add a static field listing the allowed Origin values:
private static readonly HashSet<string> _allowedOrigins = new HashSet<string>()
{
    // Add allowed origins here. For example:
    "http://www.mysite.com",
    "http://mysite.com",
};

// In your Configure method:
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Validate Origin header on WebSocket requests to prevent unexpected cross-site WebSocket requests
    app.Use((context, next) =>
    {
        // Check for a WebSocket request.
        if(string.Equals(context.Request.Headers["Upgrade"], "websocket"))
        {
            var origin = context.Request.Headers["Origin"];

            // If there is no origin header, or if the origin header doesn't match an allowed value:
            if(string.IsNullOrEmpty(origin) && !_allowedOrigins.Contains(origin))
            {
                // The origin is not allowed, reject the request
                context.Response.StatusCode = StatusCodes.Status400BadRequest;
                return Task.CompletedTask;
            }
        }

        // The request is not a WebSocket request or is a valid Origin, so let it continue
        return next();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> `Origin` Başlığı istemcinin ve gibi tamamen kontrol `Referer` başlık sahte. Bu üstbilgileri, bir kimlik doğrulama mekanizması hiçbir zaman kullanılmamalıdır.

### <a name="access-token-logging"></a>Erişim belirteci günlüğü

WebSockets veya Server-Sent olayları kullanırken, tarayıcı istemci erişim belirteci sorgu dizesinde yer gönderir. Bu standart kullanmak genellikle güvenlidir `Authorization` başlığı, birçok web sunucusu URL'si her istek için oturum ancak sorgu dizesi dahil olmak üzere. Başka bir deyişle, erişim belirtecini günlüklerinde eklenebilir. Bu bilgi günlük kaydı önlemek için web sunucusunun günlüğe kaydetme ayarlarını gözden geçirme göz önünde bulundurun.

### <a name="exceptions"></a>Özel Durumlar

Özel durum iletileri istemciye ortaya olmamalıdır hassas verileri genel olarak kabul edilir. Varsayılan olarak, SignalR için istemci bir hub yöntemi tarafından oluşturulan bir özel durum ayrıntılarını göndermez. Bunun yerine, istemci bir hata oluştupunu genel bir ileti alır. Ayarlayarak bu davranışı geçersiz kılabilirsiniz [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options) ayarı.

### <a name="buffer-management"></a>Arabellek Yönetimi

SignalR bağlantı başına arabellekler gelen ve giden iletileri yönetmek için kullanır. Varsayılan olarak, bu arabellekleri 32 KB SignalR sınırlar. Başka bir deyişle, bir istemci veya sunucu gönderebilirsiniz olası en büyük ileti 32 KB'tır. Bu ayrıca en fazla ileti için bir bağlantı tarafından kullanılan bellek miktarı 32 KB'dir anlamına gelir. İletilerinizi her zaman bu boyuttan küçük olduğunu biliyorsanız, bir istemci daha büyük bir ileti gönderin ve kabul etmek için bellek tahsis zorlayın engellemek için bu boyutunu küçültebilirsiniz. Benzer şekilde, iletilerinizi bu sınırdan büyük biliyorsanız, bu artırabilirsiniz. Ancak, bu sınırı artırmak istemciye ek bellek ayırmak sunucunun neden olanağına sahip ve uygulamanızı işleyebileceği eş zamanlı bağlantı sayısını azaltabilir anlamına unutmayın.

Gelen ve giden iletiler için ayrı sınırları vardır, her ikisi de yapılandırılabilir [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) yapılandırılmış nesne `MapHub`:

* `ApplicationMaxBufferSize` istemciden en büyük bayt sayısını temsil eder, sunucu arabellekleri. Bu sınırdan daha büyük bir ileti göndermek istemci çalışırsa, bağlantı kapalı.
* `TransportMaxBufferSize` en büyük sunucu gönderebilirsiniz bayt sayısını temsil eder. Sunucu bir ileti göndermeye çalışırsa (hub yöntemleri dönüş değerleri dahil) bu sınırı daha büyük bir özel durum oluşturulur.

Sınırı ayarını `0` sınırı tamamen devre dışı bırakır. Ancak, bu çok dikkatli yapılmalıdır. Sınır kaldırma, her boyuttaki bir ileti göndermek bir istemci sağlar. Bu, uygulamanızın destekleyebilir eş zamanlı bağlantı sayısını önemli ölçüde azaltabilir ayrılacak, aşırı bellek neden kötü amaçlı bir istemci tarafından kullanılabilir.
