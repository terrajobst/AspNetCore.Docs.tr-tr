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
ms.openlocfilehash: 1bdb8b10a24c65735f49f04285e4129cb77eb3fb
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828951"
---
# <a name="security-considerations-in-aspnet-core-opno-locsignalr"></a><span data-ttu-id="9f367-103">ASP.NET Core SignalR güvenlik konuları</span><span class="sxs-lookup"><span data-stu-id="9f367-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="9f367-104">, [Andrew Stanton-nurte](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="9f367-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="9f367-105">Bu makalede SignalRgüvenliğini sağlama hakkında bilgi sağlanır.</span><span class="sxs-lookup"><span data-stu-id="9f367-105">This article provides information on securing SignalR.</span></span>

## <a name="cross-origin-resource-sharing"></a><span data-ttu-id="9f367-106">Çıkış noktaları arası kaynak paylaşma</span><span class="sxs-lookup"><span data-stu-id="9f367-106">Cross-origin resource sharing</span></span>

<span data-ttu-id="9f367-107">[Çapraz kaynak kaynak paylaşımı (CORS)](https://www.w3.org/TR/cors/) , tarayıcıda çapraz kaynak SignalR bağlantılara izin vermek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9f367-107">[Cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="9f367-108">JavaScript kodu SignalR uygulamasından farklı bir etki alanında barındırılıyorsa, JavaScript 'In SignalR uygulamasına bağlanmasına izin vermek için [CORS ara yazılımı](xref:security/cors) etkinleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="9f367-108">If JavaScript code is hosted on a different domain from the SignalR app, [CORS middleware](xref:security/cors) must be enabled to allow the JavaScript to connect to the SignalR app.</span></span> <span data-ttu-id="9f367-109">Yalnızca güvendiğiniz veya denetlediğiniz etki alanlarından çıkış noktaları arası isteklere izin verin.</span><span class="sxs-lookup"><span data-stu-id="9f367-109">Allow cross-origin requests only from domains you trust or control.</span></span> <span data-ttu-id="9f367-110">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="9f367-110">For example:</span></span>

* <span data-ttu-id="9f367-111">Siteniz `http://www.example.com` barındırılıyor</span><span class="sxs-lookup"><span data-stu-id="9f367-111">Your site is hosted on `http://www.example.com`</span></span>
* <span data-ttu-id="9f367-112">SignalR uygulamanız `http://signalr.example.com` üzerinde barındırılıyor</span><span class="sxs-lookup"><span data-stu-id="9f367-112">Your SignalR app is hosted on `http://signalr.example.com`</span></span>

<span data-ttu-id="9f367-113">CORS, SignalR uygulamasında yalnızca kaynak `www.example.com`izin verecek şekilde yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9f367-113">CORS should be configured in the SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="9f367-114">CORS 'yi yapılandırma hakkında daha fazla bilgi için bkz. [çıkış noktaları arası istekleri (CORS) etkinleştirme](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="9f367-114">For more information on configuring CORS, see [Enable Cross-Origin Requests (CORS)](xref:security/cors).</span></span> SignalR<span data-ttu-id="9f367-115"> aşağıdaki CORS ilkelerini **gerektirir** :</span><span class="sxs-lookup"><span data-stu-id="9f367-115"> **requires** the following CORS policies:</span></span>

* <span data-ttu-id="9f367-116">Beklenen belirli kaynaklardan izin verin.</span><span class="sxs-lookup"><span data-stu-id="9f367-116">Allow the specific expected origins.</span></span> <span data-ttu-id="9f367-117">Herhangi bir kaynağa izin verilmesi mümkün olsa **da güvenli veya** önerilmez.</span><span class="sxs-lookup"><span data-stu-id="9f367-117">Allowing any origin is possible but is **not** secure or recommended.</span></span>
* <span data-ttu-id="9f367-118">HTTP yöntemlerine `GET` ve `POST` izin verilmelidir.</span><span class="sxs-lookup"><span data-stu-id="9f367-118">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="9f367-119">Tanımlama bilgisi tabanlı yapışkan oturumların düzgün çalışması için kimlik bilgilerine izin verilmelidir.</span><span class="sxs-lookup"><span data-stu-id="9f367-119">Credentials must be allowed in order for cookie-based sticky sessions to work correctly.</span></span> <span data-ttu-id="9f367-120">Kimlik doğrulaması kullanılmasa bile etkinleştirilmeleri gerekir.</span><span class="sxs-lookup"><span data-stu-id="9f367-120">They must be enabled even when authentication is not used.</span></span>

<!--
::: moniker range=">= aspnetcore-5.0"  // Moniker here just to make sure this doesn't get missed in the 5.0 version update.
However, in 5.0 we have provided an option in the TypeScript client to not use credentials.
The not to use credentials option should only be used when you know 100% that credentials like Cookies are not needed in your app (cookies are used by azure app service when using multiple servers)

For more info, see https://github.com/aspnet/AspNetCore.Docs/issues/16003
.-->

<span data-ttu-id="9f367-121">Örneğin, aşağıdaki CORS ilkesi, `https://example.com` barındırılan SignalR tarayıcısı istemcisinin `https://signalr.example.com`üzerinde barındırılan SignalR uygulamasına erişmesine izin verir:</span><span class="sxs-lookup"><span data-stu-id="9f367-121">For example, the following CORS policy allows a SignalR browser client hosted on `https://example.com` to access the SignalR app hosted on `https://signalr.example.com`:</span></span>

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
> SignalR<span data-ttu-id="9f367-122">, Azure App Service yerleşik CORS özelliğiyle uyumlu değildir.</span><span class="sxs-lookup"><span data-stu-id="9f367-122"> is not compatible with the built-in CORS feature in Azure App Service.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="9f367-123">WebSocket kaynak kısıtlaması</span><span class="sxs-lookup"><span data-stu-id="9f367-123">WebSocket Origin Restriction</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="9f367-124">CORS tarafından sunulan korumalar WebSockets için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="9f367-124">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="9f367-125">WebSockets üzerindeki kaynak kısıtlaması için [WebSockets kaynak kısıtlaması](xref:fundamentals/websockets#websocket-origin-restriction)' nı okuyun.</span><span class="sxs-lookup"><span data-stu-id="9f367-125">For origin restriction on WebSockets, read [WebSockets origin restriction](xref:fundamentals/websockets#websocket-origin-restriction).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="9f367-126">CORS tarafından sunulan korumalar WebSockets için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="9f367-126">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="9f367-127">Tarayıcılar şunları **desteklemez**:</span><span class="sxs-lookup"><span data-stu-id="9f367-127">Browsers do **not**:</span></span>

* <span data-ttu-id="9f367-128">CORS ön uçuş istekleri gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="9f367-128">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="9f367-129">WebSocket istekleri yaparken `Access-Control` üst bilgilerinde belirtilen kısıtlamalara saygı.</span><span class="sxs-lookup"><span data-stu-id="9f367-129">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="9f367-130">Ancak, tarayıcılar WebSocket istekleri verirken `Origin` üst bilgisini gönderir.</span><span class="sxs-lookup"><span data-stu-id="9f367-130">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="9f367-131">Yalnızca beklenen kaynaklardan gelen WebSockets izin verildiğinden emin olmak için uygulamalar bu üstbilgileri doğrulamak üzere yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9f367-131">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="9f367-132">ASP.NET Core 2,1 ve üzeri sürümlerde, üst bilgi doğrulaması, **`UseSignalR`ve `Configure`kimlik doğrulama ara yazılımı** aracılığıyla bulunan özel bir ara yazılım kullanılarak elde edilebilir:</span><span class="sxs-lookup"><span data-stu-id="9f367-132">In ASP.NET Core 2.1 and later, header validation can be achieved using a custom middleware placed **before `UseSignalR`, and authentication middleware** in `Configure`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="9f367-133">`Origin` üstbilgisi istemci tarafından denetlenir ve `Referer` üst bilgisi gibi erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="9f367-133">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="9f367-134">Bu üst bilgiler bir kimlik doğrulama mekanizması **olarak kullanılmamalıdır.**</span><span class="sxs-lookup"><span data-stu-id="9f367-134">These headers should **not** be used as an authentication mechanism.</span></span>

::: moniker-end

## <a name="access-token-logging"></a><span data-ttu-id="9f367-135">Erişim belirteci günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="9f367-135">Access token logging</span></span>

<span data-ttu-id="9f367-136">WebSockets veya sunucu tarafından gönderilen olaylar kullanılırken, tarayıcı istemcisi erişim belirtecini sorgu dizesinde gönderir.</span><span class="sxs-lookup"><span data-stu-id="9f367-136">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="9f367-137">Sorgu dizesi aracılığıyla erişim belirtecinin alınması genellikle standart `Authorization` üst bilgisi kullanılarak güvenlidir.</span><span class="sxs-lookup"><span data-stu-id="9f367-137">Receiving the access token via query string is generally as secure as using the standard `Authorization` header.</span></span> <span data-ttu-id="9f367-138">İstemci ve sunucu arasında güvenli bir uçtan uca bağlantı sağlamak için her zaman HTTPS kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="9f367-138">You should always use HTTPS to ensure a secure end-to-end connection between the client and the server.</span></span> <span data-ttu-id="9f367-139">Birçok Web sunucusu, sorgu dizesi dahil olmak üzere her bir isteğin URL 'sini günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="9f367-139">Many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="9f367-140">URL 'Leri günlüğe kaydetme erişim belirtecini günlüğe alabilir.</span><span class="sxs-lookup"><span data-stu-id="9f367-140">Logging the URLs may log the access token.</span></span> <span data-ttu-id="9f367-141">ASP.NET Core, her isteğin URL 'sini varsayılan olarak günlüğe kaydeder ve bu sorgu dizesini içerir.</span><span class="sxs-lookup"><span data-stu-id="9f367-141">ASP.NET Core logs the URL for each request by default, which will include the query string.</span></span> <span data-ttu-id="9f367-142">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="9f367-142">For example:</span></span>

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

<span data-ttu-id="9f367-143">Bu verileri sunucu günlüklerinize kaydetme hakkında endişeleriniz varsa, `Microsoft.AspNetCore.Hosting` günlükçüsü `Warning` düzeyine veya üstüne yapılandırarak (Bu iletiler `Info` düzeyinde yazılır) bu günlüğü tamamen devre dışı bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9f367-143">If you have concerns about logging this data with your server logs, you can disable this logging entirely by configuring the `Microsoft.AspNetCore.Hosting` logger to the `Warning` level or above (these messages are written at `Info` level).</span></span> <span data-ttu-id="9f367-144">Daha fazla bilgi için [günlük filtrelemeye](xref:fundamentals/logging/index#log-filtering) yönelik belgelere bakın.</span><span class="sxs-lookup"><span data-stu-id="9f367-144">See the documentation on [Log Filtering](xref:fundamentals/logging/index#log-filtering) for more information.</span></span> <span data-ttu-id="9f367-145">Hala belirli istek bilgilerini günlüğe kaydetmek istiyorsanız, ihtiyacınız olan verileri günlüğe kaydetmek için [bir ara yazılım yazabilir](xref:fundamentals/middleware/write) ve `access_token` sorgu dizesi değerini (varsa) filtreleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9f367-145">If you still want to log certain request information, you can [write a middleware](xref:fundamentals/middleware/write) to log the data you require and filter out the `access_token` query string value (if present).</span></span>

## <a name="exceptions"></a><span data-ttu-id="9f367-146">Özel Durumlar</span><span class="sxs-lookup"><span data-stu-id="9f367-146">Exceptions</span></span>

<span data-ttu-id="9f367-147">Özel durum iletileri genellikle bir istemciye görüntülenmemelidir gizli veriler olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="9f367-147">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="9f367-148">Varsayılan olarak, SignalR bir hub yöntemi tarafından oluşturulan bir özel durumun ayrıntılarını istemciye göndermez.</span><span class="sxs-lookup"><span data-stu-id="9f367-148">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="9f367-149">Bunun yerine, istemci bir hata oluştuğunu belirten genel bir ileti alır.</span><span class="sxs-lookup"><span data-stu-id="9f367-149">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="9f367-150">İstemciye özel durum iletisi teslimi, [Enabledetailederrors](xref:signalr/configuration#configure-server-options)ile geçersiz kılınabilir (örneğin, geliştirme veya test).</span><span class="sxs-lookup"><span data-stu-id="9f367-150">Exception message delivery to the client can be overridden (for example in development or test) with [EnableDetailedErrors](xref:signalr/configuration#configure-server-options).</span></span> <span data-ttu-id="9f367-151">Özel durum iletileri, üretim uygulamalarındaki istemciye gösterilmemelidir.</span><span class="sxs-lookup"><span data-stu-id="9f367-151">Exception messages should not be exposed to the client in production apps.</span></span>

## <a name="buffer-management"></a><span data-ttu-id="9f367-152">Arabellek Yönetimi</span><span class="sxs-lookup"><span data-stu-id="9f367-152">Buffer management</span></span>

SignalR<span data-ttu-id="9f367-153"> gelen ve giden iletileri yönetmek için bağlantı başına arabellekleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="9f367-153"> uses per-connection buffers to manage incoming and outgoing messages.</span></span> <span data-ttu-id="9f367-154">Varsayılan olarak, SignalR bu arabellekleri 32 KB ile sınırlandırır.</span><span class="sxs-lookup"><span data-stu-id="9f367-154">By default, SignalR limits these buffers to 32 KB.</span></span> <span data-ttu-id="9f367-155">Bir istemcinin veya sunucunun gönderecan en büyük ileti 32 KB 'tır.</span><span class="sxs-lookup"><span data-stu-id="9f367-155">The largest message a client or server can send is 32 KB.</span></span> <span data-ttu-id="9f367-156">İleti bağlantısı tarafından tüketilen maksimum bellek 32 KB 'tır.</span><span class="sxs-lookup"><span data-stu-id="9f367-156">The maximum memory consumed by a connection for messages is 32 KB.</span></span> <span data-ttu-id="9f367-157">İletileriniz her zaman 32 KB 'den küçükse, sınırı azaltabilirsiniz ve şunları yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="9f367-157">If your messages are always smaller than 32 KB, you can reduce the limit, which:</span></span>

* <span data-ttu-id="9f367-158">Bir istemcinin daha büyük bir ileti gönderebilmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="9f367-158">Prevents a client from being able to send a larger message.</span></span>
* <span data-ttu-id="9f367-159">Sunucu, iletileri kabul etmek için hiçbir şekilde büyük arabellekler ayırmayı gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="9f367-159">The server will never need to allocate large buffers to accept messages.</span></span>

<span data-ttu-id="9f367-160">İletileriniz 32 KB 'tan büyükse, limiti artırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9f367-160">If your messages are larger than 32 KB, you can increase the limit.</span></span> <span data-ttu-id="9f367-161">Bu sınırı artırmak şu anlama gelir:</span><span class="sxs-lookup"><span data-stu-id="9f367-161">Increasing this limit means:</span></span>

* <span data-ttu-id="9f367-162">İstemci, sunucunun büyük bellek arabellekleri ayırmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="9f367-162">The client can cause the server to allocate large memory buffers.</span></span>
* <span data-ttu-id="9f367-163">Büyük arabelleklerin sunucu ayırması, eşzamanlı bağlantı sayısını azaltabilir.</span><span class="sxs-lookup"><span data-stu-id="9f367-163">Server allocation of large buffers may reduce the number of concurrent connections.</span></span>

<span data-ttu-id="9f367-164">Gelen ve giden iletiler için her ikisi de `MapHub`' de yapılandırılan [Httpconnectiondispatcheroptions](xref:signalr/configuration#configure-server-options) nesnesinde yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="9f367-164">There are limits for incoming and outgoing messages, both can be configured on the [HttpConnectionDispatcherOptions](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="9f367-165">`ApplicationMaxBufferSize`, istemciden sunucunun arabelleğe aldığı en fazla bayt sayısını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="9f367-165">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="9f367-166">İstemci bu sınırdan daha büyük bir ileti göndermemeyi denerse bağlantı kapatılabilir.</span><span class="sxs-lookup"><span data-stu-id="9f367-166">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="9f367-167">`TransportMaxBufferSize`, sunucunun gönderemediği en fazla bayt sayısını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="9f367-167">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="9f367-168">Sunucu, bu sınırdan daha büyük bir ileti (hub metotlarından dönüş değerleri dahil) gönderilmeye çalışırsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9f367-168">If the server attempts to send a message (including return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="9f367-169">Sınırın `0` olarak ayarlanması sınırı devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="9f367-169">Setting the limit to `0` disables the limit.</span></span> <span data-ttu-id="9f367-170">Sınırı kaldırmak, bir istemcinin herhangi bir boyutta ileti göndermesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="9f367-170">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="9f367-171">Büyük iletiler gönderen kötü amaçlı istemciler fazla belleğin ayrılmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="9f367-171">Malicious clients sending large messages can cause excess memory to be allocated.</span></span> <span data-ttu-id="9f367-172">Aşırı bellek kullanımı, eşzamanlı bağlantı sayısını önemli ölçüde azaltabilir.</span><span class="sxs-lookup"><span data-stu-id="9f367-172">Excess memory usage can significantly reduce the number of concurrent connections.</span></span>
