---
title: ASP.NET Core signalr'da güvenlik konuları
author: rachelappel
description: Kimlik doğrulama ve yetkilendirme ASP.NET Core SignalR kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/security
ms.openlocfilehash: eff4542b88f24dd6c1c0675f56874e368d441fdd
ms.sourcegitcommit: 32626efaa7316c9b283c96be6516e637d548c5e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39028526"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="f7740-103">ASP.NET Core signalr'da güvenlik konuları</span><span class="sxs-lookup"><span data-stu-id="f7740-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="f7740-104">Tarafından [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="f7740-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

## <a name="overview"></a><span data-ttu-id="f7740-105">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="f7740-105">Overview</span></span>

<span data-ttu-id="f7740-106">SignalR, varsayılan olarak birkaç güvenlik koruması sağlar.</span><span class="sxs-lookup"><span data-stu-id="f7740-106">SignalR provides a number of security protections by default.</span></span> <span data-ttu-id="f7740-107">Bu korumalar yapılandırma anlamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="f7740-107">It's important to understand how to configure these protections.</span></span>

### <a name="cross-origin-resource-sharing"></a><span data-ttu-id="f7740-108">Çıkış noktaları arası kaynak paylaşımı</span><span class="sxs-lookup"><span data-stu-id="f7740-108">Cross-origin resource sharing</span></span>

<span data-ttu-id="f7740-109">[Çıkış noktaları arası kaynak paylaşımı (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) tarayıcıda çıkış noktaları arası SignalR bağlantılara izin vermek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f7740-109">[Cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="f7740-110">JavaScript kodunuza, SignalR uygulamanızı farklı bir etki alanı adı üzerinde barındırılıyorsa, etkinleştirmek için sahip [ASP.NET Core CORS ara yazılımı](xref:security/cors) bağlantılara izin vermek için.</span><span class="sxs-lookup"><span data-stu-id="f7740-110">If your JavaScript code is hosted on a different domain name from your SignalR app, you have to enable the [ASP.NET Core CORS middleware](xref:security/cors) in order to allow the connection.</span></span> <span data-ttu-id="f7740-111">Genel olarak, çıkış noktaları arası istekleri yalnızca denetim etki alanlarından izin verir.</span><span class="sxs-lookup"><span data-stu-id="f7740-111">In general, allow cross-origin requests only from domains you control.</span></span> <span data-ttu-id="f7740-112">Örneğin, siteniz üzerinde barındırılıyorsa `http://www.example.com` ve SignalR uygulamanız üzerinde barındırıldığı `http://signalr.example.com`, CORS SignalR uygulamanızı yalnızca kaynak izin verecek şekilde yapılandırmanız gerekir `www.example.com`.</span><span class="sxs-lookup"><span data-stu-id="f7740-112">For example, if your site is hosted on `http://www.example.com` and your SignalR app is hosted on `http://signalr.example.com`, you should configure CORS in your SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="f7740-113">CORS yapılandırma hakkında daha fazla bilgi için bkz. [ASP.NET Core CORS belgelerine](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="f7740-113">For more information on configuring CORS, see [the documentation on ASP.NET Core CORS](xref:security/cors).</span></span> <span data-ttu-id="f7740-114">SignalR düzgün çalışması için aşağıdaki CORS ilkelerini gerektirir:</span><span class="sxs-lookup"><span data-stu-id="f7740-114">SignalR requires the following CORS policies in order to operate correctly:</span></span>

* <span data-ttu-id="f7740-115">İlkeyi belirli kaynakları beklemez veya izin ver (önerilmez) her türlü kaynağa izin vermeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="f7740-115">The policy must allow the specific origins you expect, or allow any origin (not recommended).</span></span>
* <span data-ttu-id="f7740-116">HTTP yöntemleri `GET` ve `POST` izin verilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="f7740-116">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="f7740-117">Bile kimlik doğrulaması kullanmadığınızda, kimlik bilgileri etkinleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="f7740-117">Credentials must be enabled, even when you aren't using authentication.</span></span>

<span data-ttu-id="f7740-118">Örneğin, barındırılan bir SignalR tarayıcı istemcisi aşağıdaki CORS ilkesinin sağlar `http://example.com` SignalR uygulamanıza erişmek için:</span><span class="sxs-lookup"><span data-stu-id="f7740-118">For example, the following CORS policy allows a SignalR browser client hosted on `http://example.com` to access your SignalR app:</span></span>

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
> <span data-ttu-id="f7740-119">SignalR, Azure App Service'te yerleşik CORS özelliği ile uyumlu değil.</span><span class="sxs-lookup"><span data-stu-id="f7740-119">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

### <a name="access-token-logging"></a><span data-ttu-id="f7740-120">Erişim belirteci günlüğü</span><span class="sxs-lookup"><span data-stu-id="f7740-120">Access token logging</span></span>

<span data-ttu-id="f7740-121">WebSockets veya Server-Sent olayları kullanırken, tarayıcı istemci erişim belirteci sorgu dizesinde yer gönderir.</span><span class="sxs-lookup"><span data-stu-id="f7740-121">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="f7740-122">Bu standart kullanmak genellikle güvenlidir `Authorization` başlığı, birçok web sunucusu URL'si her istek için oturum ancak sorgu dizesi dahil olmak üzere.</span><span class="sxs-lookup"><span data-stu-id="f7740-122">This is generally as secure as using the standard `Authorization` header, however many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="f7740-123">Başka bir deyişle, erişim belirtecini günlüklerinde eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="f7740-123">This means the access token may be included in logs.</span></span> <span data-ttu-id="f7740-124">Bu bilgi günlük kaydı önlemek için web sunucusunun günlüğe kaydetme ayarlarını gözden geçirme göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="f7740-124">Consider reviewing the web server's logging settings to avoid logging this information.</span></span>

### <a name="exceptions"></a><span data-ttu-id="f7740-125">Özel Durumlar</span><span class="sxs-lookup"><span data-stu-id="f7740-125">Exceptions</span></span>

<span data-ttu-id="f7740-126">Özel durum iletileri istemciye ortaya olmamalıdır hassas verileri genel olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="f7740-126">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="f7740-127">Varsayılan olarak, SignalR için istemci bir hub yöntemi tarafından oluşturulan bir özel durum ayrıntılarını göndermez.</span><span class="sxs-lookup"><span data-stu-id="f7740-127">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="f7740-128">Bunun yerine, istemci bir hata oluştupunu genel bir ileti alır.</span><span class="sxs-lookup"><span data-stu-id="f7740-128">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="f7740-129">Ayarlayarak bu davranışı geçersiz kılabilirsiniz [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options) ayarı.</span><span class="sxs-lookup"><span data-stu-id="f7740-129">You can override this behavior by setting the [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options) setting.</span></span>

### <a name="buffer-management"></a><span data-ttu-id="f7740-130">Arabellek Yönetimi</span><span class="sxs-lookup"><span data-stu-id="f7740-130">Buffer management</span></span>

<span data-ttu-id="f7740-131">SignalR bağlantı başına arabellekler gelen ve giden iletileri yönetmek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="f7740-131">SignalR uses per-connection buffers in order to manage incoming and outgoing messages.</span></span> <span data-ttu-id="f7740-132">Varsayılan olarak, bu arabellekleri 32 KB SignalR sınırlar.</span><span class="sxs-lookup"><span data-stu-id="f7740-132">By default, SignalR limits these buffers to 32KB.</span></span> <span data-ttu-id="f7740-133">Başka bir deyişle, bir istemci veya sunucu gönderebilirsiniz olası en büyük ileti 32 KB'tır.</span><span class="sxs-lookup"><span data-stu-id="f7740-133">This means the largest possible message a client or server can send is 32KB.</span></span> <span data-ttu-id="f7740-134">Bu ayrıca en fazla ileti için bir bağlantı tarafından kullanılan bellek miktarı 32 KB'dir anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="f7740-134">This also means the maximum amount of memory consumed by a connection for messages is 32KB.</span></span> <span data-ttu-id="f7740-135">İletilerinizi her zaman bu boyuttan küçük olduğunu biliyorsanız, bir istemci daha büyük bir ileti gönderin ve kabul etmek için bellek tahsis zorlayın engellemek için bu boyutunu küçültebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f7740-135">If you know your messages are always smaller than this limit, you can reduce this size to prevent a client from being able to send a larger message and force the server to allocate memory to accept it.</span></span> <span data-ttu-id="f7740-136">Benzer şekilde, iletilerinizi bu sınırdan büyük biliyorsanız, bu artırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f7740-136">Similarly, if you know your messages are larger than this limit, you can increase it.</span></span> <span data-ttu-id="f7740-137">Ancak, bu sınırı artırmak istemciye ek bellek ayırmak sunucunun neden olanağına sahip ve uygulamanızı işleyebileceği eş zamanlı bağlantı sayısını azaltabilir anlamına unutmayın.</span><span class="sxs-lookup"><span data-stu-id="f7740-137">However, be aware that increasing this limit means that the client is able to cause the server to allocate additional memory and may reduce the number of concurrent connections your app can handle.</span></span>

<span data-ttu-id="f7740-138">Gelen ve giden iletiler için ayrı sınırları vardır, her ikisi de yapılandırılabilir [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) yapılandırılmış nesne `MapHub`:</span><span class="sxs-lookup"><span data-stu-id="f7740-138">There are separate limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="f7740-139">`ApplicationMaxBufferSize` istemciden en büyük bayt sayısını temsil eder, sunucu arabellekleri.</span><span class="sxs-lookup"><span data-stu-id="f7740-139">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="f7740-140">Bu sınırdan daha büyük bir ileti göndermek istemci çalışırsa, bağlantı kapalı.</span><span class="sxs-lookup"><span data-stu-id="f7740-140">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="f7740-141">`TransportMaxBufferSize` en büyük sunucu gönderebilirsiniz bayt sayısını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="f7740-141">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="f7740-142">Sunucu bir ileti göndermeye çalışırsa (hub yöntemleri dönüş değerleri dahil) bu sınırı daha büyük bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f7740-142">If the server attempts to send a message (includes return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="f7740-143">Sınırı ayarını `0` sınırı tamamen devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="f7740-143">Setting the limit to `0` disables the limit entirely.</span></span> <span data-ttu-id="f7740-144">Ancak, bu çok dikkatli yapılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f7740-144">However, this should be done with extreme caution.</span></span> <span data-ttu-id="f7740-145">Sınır kaldırma, her boyuttaki bir ileti göndermek bir istemci sağlar.</span><span class="sxs-lookup"><span data-stu-id="f7740-145">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="f7740-146">Bu, uygulamanızın destekleyebilir eş zamanlı bağlantı sayısını önemli ölçüde azaltabilir ayrılacak, aşırı bellek neden kötü amaçlı bir istemci tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f7740-146">This could be used by a malicious client to cause excess memory to be allocated, which could dramatically reduce the number of concurrent connections your app can support.</span></span>
