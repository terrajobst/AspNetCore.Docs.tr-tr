---
title: ASP.NET Core SignalR 'de güvenlik konuları
author: bradygaster
description: ASP.NET Core SignalR 'de kimlik doğrulama ve yetkilendirmeyi nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/security
ms.openlocfilehash: a52db2ff51c55f7299d63aa3c7398f99727e0694
ms.sourcegitcommit: 387cf29f5d5addef2cbc70670a11d612806b36b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70746553"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="2b4f9-103">ASP.NET Core SignalR 'de güvenlik konuları</span><span class="sxs-lookup"><span data-stu-id="2b4f9-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="2b4f9-104">, [Andrew Stanton-nurte](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="2b4f9-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="2b4f9-105">Bu makalede SignalR güvenliğini sağlama hakkında bilgi sağlanır.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-105">This article provides information on securing SignalR.</span></span>

## <a name="cross-origin-resource-sharing"></a><span data-ttu-id="2b4f9-106">Çıkış noktaları arası kaynak paylaşımı</span><span class="sxs-lookup"><span data-stu-id="2b4f9-106">Cross-origin resource sharing</span></span>

<span data-ttu-id="2b4f9-107">[Çapraz kaynak kaynak paylaşımı (CORS)](https://www.w3.org/TR/cors/) , tarayıcıda çapraz kaynak SignalR bağlantılarına izin vermek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-107">[Cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="2b4f9-108">JavaScript kodu, SignalR uygulamasından farklı bir etki alanında barındırılıyorsa, JavaScript 'In SignalR uygulamasına bağlanmasına izin vermek için [CORS ara yazılımı](xref:security/cors) etkinleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-108">If JavaScript code is hosted on a different domain from the SignalR app, [CORS middleware](xref:security/cors) must be enabled to allow the JavaScript to connect to the SignalR app.</span></span> <span data-ttu-id="2b4f9-109">Yalnızca güvendiğiniz veya denetlediğiniz etki alanlarından çıkış noktaları arası isteklere izin verin.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-109">Allow cross-origin requests only from domains you trust or control.</span></span> <span data-ttu-id="2b4f9-110">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2b4f9-110">For example:</span></span>

* <span data-ttu-id="2b4f9-111">Siteniz üzerinde barındırılıyor`http://www.example.com`</span><span class="sxs-lookup"><span data-stu-id="2b4f9-111">Your site is hosted on `http://www.example.com`</span></span>
* <span data-ttu-id="2b4f9-112">SignalR uygulamanız üzerinde barındırılıyor`http://signalr.example.com`</span><span class="sxs-lookup"><span data-stu-id="2b4f9-112">Your SignalR app is hosted on `http://signalr.example.com`</span></span>

<span data-ttu-id="2b4f9-113">CORS, SignalR uygulamasında yalnızca kaynağa `www.example.com`izin verecek şekilde yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-113">CORS should be configured in the SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="2b4f9-114">CORS 'yi yapılandırma hakkında daha fazla bilgi için bkz. [çıkış noktaları arası istekleri (CORS) etkinleştirme](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="2b4f9-114">For more information on configuring CORS, see [Enable Cross-Origin Requests (CORS)](xref:security/cors).</span></span> <span data-ttu-id="2b4f9-115">SignalR aşağıdaki CORS ilkelerini **gerektirir** :</span><span class="sxs-lookup"><span data-stu-id="2b4f9-115">SignalR **requires** the following CORS policies:</span></span>

* <span data-ttu-id="2b4f9-116">Beklenen belirli kaynaklardan izin verin.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-116">Allow the specific expected origins.</span></span> <span data-ttu-id="2b4f9-117">Herhangi bir kaynağa izin verilmesi mümkün olsa **da güvenli veya** önerilmez.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-117">Allowing any origin is possible but is **not** secure or recommended.</span></span>
* <span data-ttu-id="2b4f9-118">HTTP yöntemlerine `GET` ve `POST` izin verilmelidir.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-118">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="2b4f9-119">Kimlik doğrulaması kullanılmasa bile kimlik bilgilerinin etkinleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-119">Credentials must be enabled, even when authentication is not used.</span></span>

<span data-ttu-id="2b4f9-120">Örneğin, aşağıdaki CORS ilkesi üzerinde barındırılan bir SignalR tarayıcı istemcisinin `https://example.com` , üzerinde `https://signalr.example.com`barındırılan SignalR uygulamasına erişmesine izin verir:</span><span class="sxs-lookup"><span data-stu-id="2b4f9-120">For example, the following CORS policy allows a SignalR browser client hosted on `https://example.com` to access the SignalR app hosted on `https://signalr.example.com`:</span></span>

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
> <span data-ttu-id="2b4f9-121">SignalR, Azure App Service yerleşik CORS özelliğiyle uyumlu değildir.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-121">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="2b4f9-122">WebSocket kaynak kısıtlaması</span><span class="sxs-lookup"><span data-stu-id="2b4f9-122">WebSocket Origin Restriction</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2b4f9-123">CORS tarafından sunulan korumalar WebSockets için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-123">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="2b4f9-124">WebSockets üzerindeki kaynak kısıtlaması için [WebSockets kaynak kısıtlaması](xref:fundamentals/websockets#websocket-origin-restriction)' nı okuyun.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-124">For origin restriction on WebSockets, read [WebSockets origin restriction](xref:fundamentals/websockets#websocket-origin-restriction).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="2b4f9-125">CORS tarafından sunulan korumalar WebSockets için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-125">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="2b4f9-126">Tarayıcılar şunları **desteklemez**:</span><span class="sxs-lookup"><span data-stu-id="2b4f9-126">Browsers do **not**:</span></span>

* <span data-ttu-id="2b4f9-127">CORS ön uçuş istekleri gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-127">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="2b4f9-128">WebSocket istekleri yapılırken `Access-Control` üst bilgilerde belirtilen kısıtlamalara saygı.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-128">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="2b4f9-129">Ancak, tarayıcılar WebSocket istekleri verirken `Origin` üstbilgiyi gönderir.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-129">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="2b4f9-130">Yalnızca beklenen kaynaklardan gelen WebSockets izin verildiğinden emin olmak için uygulamalar bu üstbilgileri doğrulamak üzere yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-130">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="2b4f9-131">ASP.NET Core 2,1 ve üzeri sürümlerde, daha önce `Configure`  **`UseSignalR`** yerleştirilmiş özel bir ara yazılım ve içindeki kimlik doğrulama ara yazılımı kullanılarak üst bilgi doğrulaması elde edilebilir:</span><span class="sxs-lookup"><span data-stu-id="2b4f9-131">In ASP.NET Core 2.1 and later, header validation can be achieved using a custom middleware placed **before `UseSignalR`, and authentication middleware** in `Configure`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="2b4f9-132">Üst bilgi istemci tarafından denetlenir ve `Referer` üst bilgi gibi erişilebilir. `Origin`</span><span class="sxs-lookup"><span data-stu-id="2b4f9-132">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="2b4f9-133">Bu üst bilgiler bir kimlik doğrulama mekanizması **olarak kullanılmamalıdır.**</span><span class="sxs-lookup"><span data-stu-id="2b4f9-133">These headers should **not** be used as an authentication mechanism.</span></span>

::: moniker-end

## <a name="access-token-logging"></a><span data-ttu-id="2b4f9-134">Erişim belirteci günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="2b4f9-134">Access token logging</span></span>

<span data-ttu-id="2b4f9-135">WebSockets veya sunucu tarafından gönderilen olaylar kullanılırken, tarayıcı istemcisi erişim belirtecini sorgu dizesinde gönderir.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-135">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="2b4f9-136">Sorgu dizesi aracılığıyla erişim belirtecinin alınması genellikle standart `Authorization` üst bilgisi kullanılarak güvenlidir.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-136">Receiving the access token via query string is generally as secure as using the standard `Authorization` header.</span></span> <span data-ttu-id="2b4f9-137">İstemci ve sunucu arasında güvenli bir uçtan uca bağlantı sağlamak için her zaman HTTPS kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-137">You should always use HTTPS to ensure a secure end-to-end connection between the client and the server.</span></span> <span data-ttu-id="2b4f9-138">Birçok Web sunucusu, sorgu dizesi dahil olmak üzere her bir isteğin URL 'sini günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-138">Many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="2b4f9-139">URL 'Leri günlüğe kaydetme erişim belirtecini günlüğe alabilir.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-139">Logging the URLs may log the access token.</span></span> <span data-ttu-id="2b4f9-140">ASP.NET Core, her isteğin URL 'sini varsayılan olarak günlüğe kaydeder ve bu sorgu dizesini içerir.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-140">ASP.NET Core logs the URL for each request by default, which will include the query string.</span></span> <span data-ttu-id="2b4f9-141">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2b4f9-141">For example:</span></span>

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

<span data-ttu-id="2b4f9-142">Bu verilerin sunucu günlüklerinizi günlüğe kaydetmekle ilgili endişeleriniz varsa, `Microsoft.AspNetCore.Hosting` günlükçü `Warning` 'yi düzeye veya yukarıya yapılandırarak ( `Info` bu iletiler düzeyinde yazılır) bu günlüğü tamamen devre dışı bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-142">If you have concerns about logging this data with your server logs, you can disable this logging entirely by configuring the `Microsoft.AspNetCore.Hosting` logger to the `Warning` level or above (these messages are written at `Info` level).</span></span> <span data-ttu-id="2b4f9-143">Daha fazla bilgi için [günlük filtrelemeye](xref:fundamentals/logging/index#log-filtering) yönelik belgelere bakın.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-143">See the documentation on [Log Filtering](xref:fundamentals/logging/index#log-filtering) for more information.</span></span> <span data-ttu-id="2b4f9-144">Hala belirli istek bilgilerini günlüğe kaydetmek istiyorsanız, ihtiyacınız olan verileri günlüğe kaydetmek için [bir ara yazılım yazabilir](xref:fundamentals/middleware/write) ve `access_token` sorgu dizesi değerini (varsa) filtreleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-144">If you still want to log certain request information, you can [write a middleware](xref:fundamentals/middleware/write) to log the data you require and filter out the `access_token` query string value (if present).</span></span>

## <a name="exceptions"></a><span data-ttu-id="2b4f9-145">Özel Durumlar</span><span class="sxs-lookup"><span data-stu-id="2b4f9-145">Exceptions</span></span>

<span data-ttu-id="2b4f9-146">Özel durum iletileri genellikle bir istemciye görüntülenmemelidir gizli veriler olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-146">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="2b4f9-147">Varsayılan olarak, SignalR bir hub yöntemi tarafından oluşturulan bir özel durumun ayrıntılarını istemciye göndermez.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-147">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="2b4f9-148">Bunun yerine, istemci bir hata oluştuğunu belirten genel bir ileti alır.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-148">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="2b4f9-149">İstemciye özel durum iletisi teslimi (örneğin, geliştirme veya test) ile [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options)geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-149">Exception message delivery to the client can be overridden (for example in development or test) with [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options).</span></span> <span data-ttu-id="2b4f9-150">Özel durum iletileri, üretim uygulamalarındaki istemciye gösterilmemelidir.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-150">Exception messages should not be exposed to the client in production apps.</span></span>

## <a name="buffer-management"></a><span data-ttu-id="2b4f9-151">Arabellek Yönetimi</span><span class="sxs-lookup"><span data-stu-id="2b4f9-151">Buffer management</span></span>

<span data-ttu-id="2b4f9-152">SignalR gelen ve giden iletileri yönetmek için bağlantı başına arabellekleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-152">SignalR uses per-connection buffers to manage incoming and outgoing messages.</span></span> <span data-ttu-id="2b4f9-153">SignalR, varsayılan olarak bu arabellekleri 32 KB ile sınırlandırır.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-153">By default, SignalR limits these buffers to 32 KB.</span></span> <span data-ttu-id="2b4f9-154">Bir istemcinin veya sunucunun gönderecan en büyük ileti 32 KB 'tır.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-154">The largest message a client or server can send is 32 KB.</span></span> <span data-ttu-id="2b4f9-155">İleti bağlantısı tarafından tüketilen maksimum bellek 32 KB 'tır.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-155">The maximum memory consumed by a connection for messages is 32 KB.</span></span> <span data-ttu-id="2b4f9-156">İletileriniz her zaman 32 KB 'den küçükse, sınırı azaltabilirsiniz ve şunları yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="2b4f9-156">If your messages are always smaller than 32 KB, you can reduce the limit, which:</span></span>

* <span data-ttu-id="2b4f9-157">Bir istemcinin daha büyük bir ileti gönderebilmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-157">Prevents a client from being able to send a larger message.</span></span>
* <span data-ttu-id="2b4f9-158">Sunucu, iletileri kabul etmek için hiçbir şekilde büyük arabellekler ayırmayı gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-158">The server will never need to allocate large buffers to accept messages.</span></span>

<span data-ttu-id="2b4f9-159">İletileriniz 32 KB 'tan büyükse, limiti artırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-159">If your messages are larger than 32 KB, you can increase the limit.</span></span> <span data-ttu-id="2b4f9-160">Bu sınırı artırmak şu anlama gelir:</span><span class="sxs-lookup"><span data-stu-id="2b4f9-160">Increasing this limit means:</span></span>

* <span data-ttu-id="2b4f9-161">İstemci, sunucunun büyük bellek arabellekleri ayırmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-161">The client can cause the server to allocate large memory buffers.</span></span>
* <span data-ttu-id="2b4f9-162">Büyük arabelleklerin sunucu ayırması, eşzamanlı bağlantı sayısını azaltabilir.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-162">Server allocation of large buffers may reduce the number of concurrent connections.</span></span>

<span data-ttu-id="2b4f9-163">Gelen ve giden iletiler için sınırlar vardır, her ikisi de ' de [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) `MapHub`yapılandırılan nesne üzerinde yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="2b4f9-163">There are limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="2b4f9-164">`ApplicationMaxBufferSize`istemciden sunucunun arabelleğe aldığı en fazla bayt sayısını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-164">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="2b4f9-165">İstemci bu sınırdan daha büyük bir ileti göndermemeyi denerse bağlantı kapatılabilir.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-165">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="2b4f9-166">`TransportMaxBufferSize`sunucunun gönderemediği en fazla bayt sayısını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-166">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="2b4f9-167">Sunucu, bu sınırdan daha büyük bir ileti (hub metotlarından dönüş değerleri dahil) gönderilmeye çalışırsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-167">If the server attempts to send a message (including return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="2b4f9-168">Limit ayarı sınırı `0` devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-168">Setting the limit to `0` disables the limit.</span></span> <span data-ttu-id="2b4f9-169">Sınırı kaldırmak, bir istemcinin herhangi bir boyutta ileti göndermesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-169">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="2b4f9-170">Büyük iletiler gönderen kötü amaçlı istemciler fazla belleğin ayrılmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-170">Malicious clients sending large messages can cause excess memory to be allocated.</span></span> <span data-ttu-id="2b4f9-171">Aşırı bellek kullanımı, eşzamanlı bağlantı sayısını önemli ölçüde azaltabilir.</span><span class="sxs-lookup"><span data-stu-id="2b4f9-171">Excess memory usage can significantly reduce the number of concurrent connections.</span></span>
