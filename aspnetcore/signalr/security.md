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
ms.sourcegitcommit: b3894b65e313570e97a2ab78b8addd22f427cac8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56743793"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="c5b73-103">ASP.NET Core signalr'da güvenlik konuları</span><span class="sxs-lookup"><span data-stu-id="c5b73-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="c5b73-104">Tarafından [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="c5b73-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="c5b73-105">Bu makalede, SignalR güvenli hale getirme hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="c5b73-105">This article provides information on securing SignalR.</span></span>

## <a name="cross-origin-resource-sharing"></a><span data-ttu-id="c5b73-106">Çıkış noktaları arası kaynak paylaşımı</span><span class="sxs-lookup"><span data-stu-id="c5b73-106">Cross-origin resource sharing</span></span>

<span data-ttu-id="c5b73-107">[Çıkış noktaları arası kaynak paylaşımı (CORS)](https://www.w3.org/TR/cors/) tarayıcıda çıkış noktaları arası SignalR bağlantılara izin vermek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c5b73-107">[Cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="c5b73-108">JavaScript kodu farklı bir etki alanı, SignalR uygulamadan üzerinde barındırılıyorsa [CORS ara yazılımı](xref:security/cors) SignalR uygulamaya bağlanmak için JavaScript'e izin vermeniz etkinleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="c5b73-108">If JavaScript code is hosted on a different domain from the SignalR app, [CORS middleware](xref:security/cors) must be enabled to allow the JavaScript to connect to the SignalR app.</span></span> <span data-ttu-id="c5b73-109">Çıkış noktaları arası istekleri yalnızca güvendiğiniz etki alanları veya denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="c5b73-109">Allow cross-origin requests only from domains you trust or control.</span></span> <span data-ttu-id="c5b73-110">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="c5b73-110">For example:</span></span>

* <span data-ttu-id="c5b73-111">Sitenizi üzerinde barındırılır `http://www.example.com`</span><span class="sxs-lookup"><span data-stu-id="c5b73-111">Your site is hosted on `http://www.example.com`</span></span>
* <span data-ttu-id="c5b73-112">SignalR uygulamanız üzerinde barındırılır `http://signalr.example.com`</span><span class="sxs-lookup"><span data-stu-id="c5b73-112">Your SignalR app is hosted on `http://signalr.example.com`</span></span>

<span data-ttu-id="c5b73-113">CORS SignalR uygulamada yalnızca kaynak izin verecek şekilde yapılandırılmalıdır `www.example.com`.</span><span class="sxs-lookup"><span data-stu-id="c5b73-113">CORS should be configured in the SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="c5b73-114">CORS yapılandırma hakkında daha fazla bilgi için bkz. [etkinleştirme çıkış noktaları arası istekleri (CORS)](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="c5b73-114">For more information on configuring CORS, see [Enable Cross-Origin Requests (CORS)](xref:security/cors).</span></span> <span data-ttu-id="c5b73-115">SignalR **gerektirir** aşağıdaki CORS kuralları:</span><span class="sxs-lookup"><span data-stu-id="c5b73-115">SignalR **requires** the following CORS policies:</span></span>

* <span data-ttu-id="c5b73-116">Belirli beklenen kaynakları sağlar.</span><span class="sxs-lookup"><span data-stu-id="c5b73-116">Allow the specific expected origins.</span></span> <span data-ttu-id="c5b73-117">Her türlü kaynağa izin verme mümkündür ancak olan **değil** güvenli veya önerilir.</span><span class="sxs-lookup"><span data-stu-id="c5b73-117">Allowing any origin is possible but is **not** secure or recommended.</span></span>
* <span data-ttu-id="c5b73-118">HTTP yöntemleri `GET` ve `POST` izin verilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="c5b73-118">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="c5b73-119">Hatta kimlik doğrulama olmadığında kimlik bilgileri etkinleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="c5b73-119">Credentials must be enabled, even when authentication is not used.</span></span>

<span data-ttu-id="c5b73-120">Örneğin, barındırılan bir SignalR tarayıcı istemcisi aşağıdaki CORS ilkesinin sağlar `https://example.com` barındırılan SignalR uygulamaya erişmek için `https://signalr.example.com`:</span><span class="sxs-lookup"><span data-stu-id="c5b73-120">For example, the following CORS policy allows a SignalR browser client hosted on `https://example.com` to access the SignalR app hosted on `https://signalr.example.com`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

> [!NOTE]
> <span data-ttu-id="c5b73-121">SignalR, Azure App Service'te yerleşik CORS özelliği ile uyumlu değil.</span><span class="sxs-lookup"><span data-stu-id="c5b73-121">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="c5b73-122">WebSocket kaynak kısıtlama</span><span class="sxs-lookup"><span data-stu-id="c5b73-122">WebSocket Origin Restriction</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="c5b73-123">CORS tarafından sağlanan korumaları WebSockets için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="c5b73-123">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="c5b73-124">WebSockets temel kaynak kısıtlama için okuma [WebSockets kaynak kısıtlama](xref:fundamentals/websockets#websocket-origin-restriction).</span><span class="sxs-lookup"><span data-stu-id="c5b73-124">For origin restriction on WebSockets, read [WebSockets origin restriction](xref:fundamentals/websockets#websocket-origin-restriction).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="c5b73-125">CORS tarafından sağlanan korumaları WebSockets için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="c5b73-125">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="c5b73-126">Tarayıcılar **değil**:</span><span class="sxs-lookup"><span data-stu-id="c5b73-126">Browsers do **not**:</span></span>

* <span data-ttu-id="c5b73-127">CORS uçuş öncesi istekler gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="c5b73-127">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="c5b73-128">Belirtilen kısıtlamalarını dikkate `Access-Control` WebSocket istekleri yaparken üstbilgileri.</span><span class="sxs-lookup"><span data-stu-id="c5b73-128">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="c5b73-129">Ancak, tarayıcılar gönderebilirsiniz `Origin` WebSocket istekleri gönderirken, üst bilgisi.</span><span class="sxs-lookup"><span data-stu-id="c5b73-129">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="c5b73-130">Uygulamalar, beklenen kaynaklardan gelen WebSockets izin verildiğinden emin olmak için bu üstbilgileri doğrulamak için yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c5b73-130">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="c5b73-131">ASP.NET Core 2.1 ve sonraki sürümlerinde, üst bilgisi doğrulama yerleştirilen özel bir ara yazılım kullanarak gerçekleştirilebilir **önce `UseSignalR`ve kimlik doğrulaması ara yazılımı** içinde `Configure`:</span><span class="sxs-lookup"><span data-stu-id="c5b73-131">In ASP.NET Core 2.1 and later, header validation can be achieved using a custom middleware placed **before `UseSignalR`, and authentication middleware** in `Configure`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="c5b73-132">`Origin` Üst bilgisi, istemcinin ve gibi denetlenir `Referer` başlık sahte.</span><span class="sxs-lookup"><span data-stu-id="c5b73-132">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="c5b73-133">Bu üst gereken **değil** bir kimlik doğrulama mekanizması kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c5b73-133">These headers should **not** be used as an authentication mechanism.</span></span>

::: moniker-end

## <a name="access-token-logging"></a><span data-ttu-id="c5b73-134">Erişim belirteci günlüğü</span><span class="sxs-lookup"><span data-stu-id="c5b73-134">Access token logging</span></span>

<span data-ttu-id="c5b73-135">WebSockets veya Server-Sent olayları kullanırken, tarayıcı istemci erişim belirteci sorgu dizesinde yer gönderir.</span><span class="sxs-lookup"><span data-stu-id="c5b73-135">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="c5b73-136">Sorgu dizesi aracılığıyla erişim belirteci alma standart kullanmak genellikle kadar güvenli `Authorization` başlığı.</span><span class="sxs-lookup"><span data-stu-id="c5b73-136">Receiving the access token via query string is generally as secure as using the standard `Authorization` header.</span></span> <span data-ttu-id="c5b73-137">İstemci ve sunucu arasında güvenli bir uçtan uca bağlantı sağlamak için her zaman HTTPS kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c5b73-137">You should always use HTTPS to ensure a secure end-to-end connection between the client and the server.</span></span> <span data-ttu-id="c5b73-138">Birçok web sunucusu URL'si sorgu dizesi dahil olmak üzere her istek için oturum açın.</span><span class="sxs-lookup"><span data-stu-id="c5b73-138">Many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="c5b73-139">URL'leri günlüğü erişim belirteci oturum açabilir.</span><span class="sxs-lookup"><span data-stu-id="c5b73-139">Logging the URLs may log the access token.</span></span> <span data-ttu-id="c5b73-140">ASP.NET Core her istek için URL sorgu dizesini içeren varsayılan olarak günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="c5b73-140">ASP.NET Core logs the URL for each request by default, which will include the query string.</span></span> <span data-ttu-id="c5b73-141">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="c5b73-141">For example:</span></span>

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

<span data-ttu-id="c5b73-142">Bu veri günlüğü, sunucu günlükleri ile ilgili endişeleriniz varsa, bu günlük tamamen yapılandırarak devre dışı bırakabilirsiniz `Microsoft.AspNetCore.Hosting` için Günlükçü `Warning` düzeyi veya üzeri (Bu iletiler yazıldığı `Info` düzeyi).</span><span class="sxs-lookup"><span data-stu-id="c5b73-142">If you have concerns about logging this data with your server logs, you can disable this logging entirely by configuring the `Microsoft.AspNetCore.Hosting` logger to the `Warning` level or above (these messages are written at `Info` level).</span></span> <span data-ttu-id="c5b73-143">İlgili belgelere bakın [günlük filtreleme](xref:fundamentals/logging/index#log-filtering) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="c5b73-143">See the documentation on [Log Filtering](xref:fundamentals/logging/index#log-filtering) for more information.</span></span> <span data-ttu-id="c5b73-144">Belirli istek bilgileri günlüğe kaydetmek isterseniz, [bir ara yazılım yazma](xref:fundamentals/middleware/write) gerektirir ve filtrelemek verilerini günlüğe kaydetmek `access_token` sorgu dize değeri (varsa).</span><span class="sxs-lookup"><span data-stu-id="c5b73-144">If you still want to log certain request information, you can [write a middleware](xref:fundamentals/middleware/write) to log the data you require and filter out the `access_token` query string value (if present).</span></span>

## <a name="exceptions"></a><span data-ttu-id="c5b73-145">Özel Durumlar</span><span class="sxs-lookup"><span data-stu-id="c5b73-145">Exceptions</span></span>

<span data-ttu-id="c5b73-146">Özel durum iletileri istemciye ortaya olmamalıdır hassas verileri genel olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="c5b73-146">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="c5b73-147">Varsayılan olarak, SignalR için istemci bir hub yöntemi tarafından oluşturulan bir özel durum ayrıntılarını göndermez.</span><span class="sxs-lookup"><span data-stu-id="c5b73-147">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="c5b73-148">Bunun yerine, istemci bir hata oluştupunu genel bir ileti alır.</span><span class="sxs-lookup"><span data-stu-id="c5b73-148">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="c5b73-149">Özel durum iletisi teslim istemciye geçersiz kılınabilir (örneğin, geliştirme veya test) ile [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options).</span><span class="sxs-lookup"><span data-stu-id="c5b73-149">Exception message delivery to the client can be overridden (for example in development or test) with [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options).</span></span> <span data-ttu-id="c5b73-150">Özel durum iletileri, üretim uygulamaları istemciye sunulmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="c5b73-150">Exception messages should not be exposed to the client in production apps.</span></span>

## <a name="buffer-management"></a><span data-ttu-id="c5b73-151">Arabellek Yönetimi</span><span class="sxs-lookup"><span data-stu-id="c5b73-151">Buffer management</span></span>

<span data-ttu-id="c5b73-152">SignalR bağlantı başına arabellekler gelen ve giden iletileri yönetmek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="c5b73-152">SignalR uses per-connection buffers to manage incoming and outgoing messages.</span></span> <span data-ttu-id="c5b73-153">Varsayılan olarak, bu arabellekleri 32 KB SignalR sınırlar.</span><span class="sxs-lookup"><span data-stu-id="c5b73-153">By default, SignalR limits these buffers to 32 KB.</span></span> <span data-ttu-id="c5b73-154">Bir istemci veya sunucu gönderebilirsiniz en büyük ileti, 32 KB'dir.</span><span class="sxs-lookup"><span data-stu-id="c5b73-154">The largest message a client or server can send is 32 KB.</span></span> <span data-ttu-id="c5b73-155">İletiler için bir bağlantı tarafından kullanılan en fazla bellek 32 KB'tır.</span><span class="sxs-lookup"><span data-stu-id="c5b73-155">The maximum memory consumed by a connection for messages is 32 KB.</span></span> <span data-ttu-id="c5b73-156">İletilerinizi 32 KB'den küçük varsa, her zaman sınırı azaltabilir:</span><span class="sxs-lookup"><span data-stu-id="c5b73-156">If your messages are always smaller than 32 KB, you can reduce the limit, which:</span></span>

* <span data-ttu-id="c5b73-157">Bir istemci daha büyük bir ileti göndermek engeller.</span><span class="sxs-lookup"><span data-stu-id="c5b73-157">Prevents a client from being able to send a larger message.</span></span>
* <span data-ttu-id="c5b73-158">Sunucu, hiçbir zaman ileti kabul etmek için büyük arabellekler ayrılacak gerekir.</span><span class="sxs-lookup"><span data-stu-id="c5b73-158">The server will never need to allocate large buffers to accept messages.</span></span>

<span data-ttu-id="c5b73-159">İletilerinizi 32 KB'den büyükse, sınırı artırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c5b73-159">If your messages are larger than 32 KB, you can increase the limit.</span></span> <span data-ttu-id="c5b73-160">Bu sınırı artırmak anlamına gelir:</span><span class="sxs-lookup"><span data-stu-id="c5b73-160">Increasing this limit means:</span></span>

* <span data-ttu-id="c5b73-161">İstemci, büyük bellek arabelleği ayrılamadı. sunucu neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="c5b73-161">The client can cause the server to allocate large memory buffers.</span></span>
* <span data-ttu-id="c5b73-162">Server ayırma büyük arabellek boyutunu eş zamanlı bağlantı sayısını azaltabilir.</span><span class="sxs-lookup"><span data-stu-id="c5b73-162">Server allocation of large buffers may reduce the number of concurrent connections.</span></span>

<span data-ttu-id="c5b73-163">Gelen ve giden iletiler için sınırları vardır, her ikisi de yapılandırılabilir [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) yapılandırılmış nesne `MapHub`:</span><span class="sxs-lookup"><span data-stu-id="c5b73-163">There are limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="c5b73-164">`ApplicationMaxBufferSize` istemciden en büyük bayt sayısını temsil eder, sunucu arabellekleri.</span><span class="sxs-lookup"><span data-stu-id="c5b73-164">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="c5b73-165">Bu sınırdan daha büyük bir ileti göndermek istemci çalışırsa, bağlantı kapalı.</span><span class="sxs-lookup"><span data-stu-id="c5b73-165">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="c5b73-166">`TransportMaxBufferSize` en büyük sunucu gönderebilirsiniz bayt sayısını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="c5b73-166">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="c5b73-167">Bu sınırdan daha büyük (dahil olmak üzere dönüş değerleri hub yöntemleri) bir ileti göndermek sunucu çalışırsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c5b73-167">If the server attempts to send a message (including return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="c5b73-168">Sınırı ayarını `0` sınırını devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="c5b73-168">Setting the limit to `0` disables the limit.</span></span> <span data-ttu-id="c5b73-169">Sınır kaldırma, her boyuttaki bir ileti göndermek bir istemci sağlar.</span><span class="sxs-lookup"><span data-stu-id="c5b73-169">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="c5b73-170">Büyük ileti gönderme kötü amaçlı istemciler, aşırı bellek ayrılmasını neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="c5b73-170">Malicious clients sending large messages can cause excess memory to be allocated.</span></span> <span data-ttu-id="c5b73-171">Aşırı bellek kullanımı, eş zamanlı bağlantı sayısını önemli ölçüde azaltabilir.</span><span class="sxs-lookup"><span data-stu-id="c5b73-171">Excess memory usage can significantly reduce the number of concurrent connections.</span></span>
