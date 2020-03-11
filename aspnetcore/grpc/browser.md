---
title: Tarayıcı uygulamalarında gRPC kullanma
author: jamesnk
description: ASP.NET Core GRPC hizmetlerini, gRPC-Web kullanan tarayıcı uygulamalarından çağrılabilir olacak şekilde nasıl yapılandıracağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 02/16/2020
uid: grpc/browser
ms.openlocfilehash: 3beeffc26ffd3c2dc85bfc22a46d97d5fd78d3d0
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664201"
---
# <a name="use-grpc-in-browser-apps"></a><span data-ttu-id="df2bc-103">Tarayıcı uygulamalarında gRPC kullanma</span><span class="sxs-lookup"><span data-stu-id="df2bc-103">Use gRPC in browser apps</span></span>

<span data-ttu-id="df2bc-104">, [James bAyKiNg](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="df2bc-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="df2bc-105">**gRPC-.NET 'teki Web desteği deneysel**</span><span class="sxs-lookup"><span data-stu-id="df2bc-105">**gRPC-Web support in .NET is experimental**</span></span>
>
> <span data-ttu-id="df2bc-106">gRPC-.NET için Web, kaydedilmiş bir ürün değil, deneysel bir projem projem.</span><span class="sxs-lookup"><span data-stu-id="df2bc-106">gRPC-Web for .NET is an experimental project, not a committed product.</span></span> <span data-ttu-id="df2bc-107">Şunları yapmak istiyoruz:</span><span class="sxs-lookup"><span data-stu-id="df2bc-107">We want to:</span></span>
>
> * <span data-ttu-id="df2bc-108">GRPC-Web ' i uygulamaya yönelik yaklaşımımızı test edin.</span><span class="sxs-lookup"><span data-stu-id="df2bc-108">Test that our approach to implementing gRPC-Web works.</span></span>
> * <span data-ttu-id="df2bc-109">Bu yaklaşım .NET geliştiricileri için, bir ara sunucu aracılığıyla gRPC-Web ayarlamanın geleneksel bir yolu ile karşılaştırıldığında yararlı olup olmadığını hakkında geri bildirim alın.</span><span class="sxs-lookup"><span data-stu-id="df2bc-109">Get feedback on if this approach is useful to .NET developers compared to the traditional way of setting up gRPC-Web via a proxy.</span></span>
>
> <span data-ttu-id="df2bc-110">Bu geliştiricilerin, ve ile üretken olduğu bir şey oluşturduğumuzun olduğundan emin olmak için [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) lütfen geri bildirimde bulunun.</span><span class="sxs-lookup"><span data-stu-id="df2bc-110">Please leave feedback at [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) to ensure we build something that developers like and are productive with.</span></span>

<span data-ttu-id="df2bc-111">Tarayıcı tabanlı bir uygulamadan HTTP/2 gRPC hizmetini çağırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="df2bc-111">It is not possible to call a HTTP/2 gRPC service from a browser-based app.</span></span> <span data-ttu-id="df2bc-112">[GRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) , tarayıcı JavaScript ve Blazor uygulamalarının GRPC hizmetlerini çağırmasını sağlayan bir protokoldür.</span><span class="sxs-lookup"><span data-stu-id="df2bc-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) is a protocol that allows browser JavaScript and Blazor apps to call gRPC services.</span></span> <span data-ttu-id="df2bc-113">Bu makalede, .NET Core 'da gRPC-Web ' i kullanma açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="df2bc-113">This article explains how to use gRPC-Web in .NET Core.</span></span>

## <a name="configure-grpc-web-in-aspnet-core"></a><span data-ttu-id="df2bc-114">ASP.NET Core 'de gRPC-Web yapılandırma</span><span class="sxs-lookup"><span data-stu-id="df2bc-114">Configure gRPC-Web in ASP.NET Core</span></span>

<span data-ttu-id="df2bc-115">ASP.NET Core 'de barındırılan gRPC Hizmetleri, gRPC-Web ' i HTTP/2 gRPC ile birlikte destekleyecek şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="df2bc-115">gRPC services hosted in ASP.NET Core can be configured to support gRPC-Web alongside HTTP/2 gRPC.</span></span> <span data-ttu-id="df2bc-116">gRPC-Web, hizmetlerde herhangi bir değişiklik yapılmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="df2bc-116">gRPC-Web does not require any changes to services.</span></span> <span data-ttu-id="df2bc-117">Tek değişiklik başlangıç yapılandırması ' dır.</span><span class="sxs-lookup"><span data-stu-id="df2bc-117">The only modification is startup configuration.</span></span>

<span data-ttu-id="df2bc-118">GRPC-Web ' i bir ASP.NET Core gRPC hizmeti ile etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="df2bc-118">To enable gRPC-Web with an ASP.NET Core gRPC service:</span></span>

* <span data-ttu-id="df2bc-119">[GRPC. AspNetCore. Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) paketine bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="df2bc-119">Add a reference to the [Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) package.</span></span>
* <span data-ttu-id="df2bc-120">*Startup.cs*'e `AddGrpcWeb` ve `UseGrpcWeb` ekleyerek, uygulamayı GRPC-Web kullanacak şekilde yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="df2bc-120">Configure the app to use gRPC-Web by adding `AddGrpcWeb` and `UseGrpcWeb` to *Startup.cs*:</span></span>

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=10,14)]

<span data-ttu-id="df2bc-121">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="df2bc-121">The preceding code:</span></span>

* <span data-ttu-id="df2bc-122">Yönlendirme ve bitiş noktalarından önce `UseGrpcWeb`gRPC-Web ara yazılımını ekler.</span><span class="sxs-lookup"><span data-stu-id="df2bc-122">Adds the gRPC-Web middleware, `UseGrpcWeb`, after routing and before endpoints.</span></span>
* <span data-ttu-id="df2bc-123">`endpoints.MapGrpcService<GreeterService>()` yönteminin `EnableGrpcWeb`ile gRPC-Web 'i desteklediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="df2bc-123">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports gRPC-Web with `EnableGrpcWeb`.</span></span> 

<span data-ttu-id="df2bc-124">Alternatif olarak, tüm hizmetleri, bir `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` ConfigureServices 'e ekleyerek gRPC-Web ' i destekleyecek şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="df2bc-124">Alternatively, configure all services to support gRPC-Web by adding `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` to ConfigureServices.</span></span>

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=6,13)]

### <a name="grpc-web-and-cors"></a><span data-ttu-id="df2bc-125">gRPC-Web ve CORS</span><span class="sxs-lookup"><span data-stu-id="df2bc-125">gRPC-Web and CORS</span></span>

<span data-ttu-id="df2bc-126">Tarayıcı güvenliği, bir Web sayfasının Web sayfasını sunduğundan farklı bir etki alanına istek yapmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="df2bc-126">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="df2bc-127">Bu kısıtlama, tarayıcı uygulamalarıyla gRPC Web çağrıları yapmak için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="df2bc-127">This restriction applies to making gRPC-Web calls with browser apps.</span></span> <span data-ttu-id="df2bc-128">Örneğin, `https://www.contoso.com` tarafından sunulan bir tarayıcı uygulamasının, `https://services.contoso.com`barındırılan gRPC-Web hizmetlerinden çağrılması engellenir.</span><span class="sxs-lookup"><span data-stu-id="df2bc-128">For example, a browser app served by `https://www.contoso.com` is blocked from calling gRPC-Web services hosted on `https://services.contoso.com`.</span></span> <span data-ttu-id="df2bc-129">Bu kısıtlamayı rahatmak için çapraz kaynak kaynak paylaşımı (CORS) kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="df2bc-129">Cross Origin Resource Sharing (CORS) can be used to relax this restriction.</span></span>

<span data-ttu-id="df2bc-130">Tarayıcı uygulamanızın çıkış noktaları arası gRPC-Web çağrıları yapmasına izin vermek için [ASP.NET Core 'de CORS](xref:security/cors)'yi ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="df2bc-130">To allow your browser app to make cross-origin gRPC-Web calls, set up [CORS in ASP.NET Core](xref:security/cors).</span></span> <span data-ttu-id="df2bc-131">Yerleşik CORS desteğini kullanın ve <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>ile gRPC 'ye özgü üst bilgileri sunun.</span><span class="sxs-lookup"><span data-stu-id="df2bc-131">Use the built-in CORS support, and expose gRPC-specific headers with <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>.</span></span>

[!code-csharp[](~/grpc/browser/sample/CORS_Startup.cs?name=snippet_1&highlight=5-11,19,24)]

<span data-ttu-id="df2bc-132">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="df2bc-132">The preceding code:</span></span>

* <span data-ttu-id="df2bc-133">CORS hizmetleri eklemek için `AddCors` çağırır ve gRPC 'ye özgü üst bilgileri sunan bir CORS ilkesi yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="df2bc-133">Calls `AddCors` to add CORS services and configures a CORS policy that exposes gRPC-specific headers.</span></span>
* <span data-ttu-id="df2bc-134">, Yönlendirmeden ve bitiş noktalarından önce CORS ara yazılımını eklemek için `UseCors` çağırır.</span><span class="sxs-lookup"><span data-stu-id="df2bc-134">Calls `UseCors` to add the CORS middleware after routing and before endpoints.</span></span>
* <span data-ttu-id="df2bc-135">`endpoints.MapGrpcService<GreeterService>()` yönteminin `RequiresCors`ile CORS 'yi desteklediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="df2bc-135">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports CORS with `RequiresCors`.</span></span>

## <a name="call-grpc-web-from-the-browser"></a><span data-ttu-id="df2bc-136">Tarayıcıdan gRPC-Web 'i çağırma</span><span class="sxs-lookup"><span data-stu-id="df2bc-136">Call gRPC-Web from the browser</span></span>

<span data-ttu-id="df2bc-137">Tarayıcı uygulamaları GRPC hizmetlerini çağırmak için gRPC-Web kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="df2bc-137">Browser apps can use gRPC-Web to call gRPC services.</span></span> <span data-ttu-id="df2bc-138">GRPC hizmetlerini tarayıcıda GRPC-Web ile çağırırken bazı gereksinimler ve sınırlamalar vardır:</span><span class="sxs-lookup"><span data-stu-id="df2bc-138">There are some requirements and limitations when calling gRPC services with gRPC-Web from the browser:</span></span>

* <span data-ttu-id="df2bc-139">Sunucu, gRPC-Web ' i destekleyecek şekilde yapılandırılmış olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="df2bc-139">The server must have been configured to support gRPC-Web.</span></span>
* <span data-ttu-id="df2bc-140">İstemci akışı ve çift yönlü akış çağrıları desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="df2bc-140">Client streaming and bidirectional streaming calls aren't supported.</span></span> <span data-ttu-id="df2bc-141">Sunucu akışı destekleniyor.</span><span class="sxs-lookup"><span data-stu-id="df2bc-141">Server streaming is supported.</span></span>
* <span data-ttu-id="df2bc-142">Farklı bir etki alanında gRPC hizmetlerinin çağrılması için [CORS](xref:security/cors) 'nin sunucuda yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="df2bc-142">Calling gRPC services on a different domain requires [CORS](xref:security/cors) to be configured on the server.</span></span>

### <a name="javascript-grpc-web-client"></a><span data-ttu-id="df2bc-143">JavaScript gRPC-Web istemcisi</span><span class="sxs-lookup"><span data-stu-id="df2bc-143">JavaScript gRPC-Web client</span></span>

<span data-ttu-id="df2bc-144">Bir JavaScript gRPC-Web istemcisi vardır.</span><span class="sxs-lookup"><span data-stu-id="df2bc-144">There is a JavaScript gRPC-Web client.</span></span> <span data-ttu-id="df2bc-145">JavaScript 'ten gRPC-Web kullanma hakkında yönergeler için bkz. [gRPC-web Ile JavaScript istemci kodu yazma](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span><span class="sxs-lookup"><span data-stu-id="df2bc-145">For instructions on how to use gRPC-Web from JavaScript, see [write JavaScript client code with gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span></span>

### <a name="configure-grpc-web-with-the-net-grpc-client"></a><span data-ttu-id="df2bc-146">.NET gRPC istemcisiyle gRPC-Web yapılandırma</span><span class="sxs-lookup"><span data-stu-id="df2bc-146">Configure gRPC-Web with the .NET gRPC client</span></span>

<span data-ttu-id="df2bc-147">.NET gRPC istemcisi, gRPC-Web çağrıları yapmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="df2bc-147">The .NET gRPC client can be configured to make gRPC-Web calls.</span></span> <span data-ttu-id="df2bc-148">Bu, tarayıcıda barındırılan ve JavaScript kodunda aynı HTTP kısıtlamalarına sahip olan [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) uygulamaları için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="df2bc-148">This is useful for [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) apps, which are hosted in the browser and have the same HTTP limitations of JavaScript code.</span></span> <span data-ttu-id="df2bc-149">Bir .NET istemcisiyle gRPC-Web ' i çağırmak [http/2 gRPC ile aynıdır](xref:grpc/client).</span><span class="sxs-lookup"><span data-stu-id="df2bc-149">Calling gRPC-Web with a .NET client is [the same as HTTP/2 gRPC](xref:grpc/client).</span></span> <span data-ttu-id="df2bc-150">Tek değişiklik, kanalın oluşturulma şekli olur.</span><span class="sxs-lookup"><span data-stu-id="df2bc-150">The only modification is how the channel is created.</span></span>

<span data-ttu-id="df2bc-151">GRPC-Web kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="df2bc-151">To use gRPC-Web:</span></span>

* <span data-ttu-id="df2bc-152">[GRPC .net. Client. Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) paketine başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="df2bc-152">Add a reference to the [Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) package.</span></span>
* <span data-ttu-id="df2bc-153">[GRPC .net. Client](https://www.nuget.org/packages/Grpc.Net.Client) paketine yönelik başvurunun 2.27.0 veya daha büyük olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="df2bc-153">Ensure the reference to [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) package is 2.27.0 or greater.</span></span>
* <span data-ttu-id="df2bc-154">Kanalı `GrpcWebHandler`kullanacak şekilde yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="df2bc-154">Configure the channel to use the `GrpcWebHandler`:</span></span>

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

<span data-ttu-id="df2bc-155">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="df2bc-155">The preceding code:</span></span>

* <span data-ttu-id="df2bc-156">GRPC-Web kullanmak için bir kanal yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="df2bc-156">Configures a channel to use gRPC-Web.</span></span>
* <span data-ttu-id="df2bc-157">Bir istemci oluşturur ve kanalı kullanarak bir çağrı yapar.</span><span class="sxs-lookup"><span data-stu-id="df2bc-157">Creates a client and makes a call using the channel.</span></span>

<span data-ttu-id="df2bc-158">`GrpcWebHandler` oluşturulduğunda aşağıdaki yapılandırma seçeneklerine sahiptir:</span><span class="sxs-lookup"><span data-stu-id="df2bc-158">The `GrpcWebHandler` has the following configuration options when created:</span></span>

* <span data-ttu-id="df2bc-159">**InnerHandler**: GRPC http isteğini oluşturan temel <xref:System.Net.Http.HttpMessageHandler>, örneğin, `HttpClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="df2bc-159">**InnerHandler**: The underlying <xref:System.Net.Http.HttpMessageHandler> that makes the gRPC HTTP request, for example, `HttpClientHandler`.</span></span>
* <span data-ttu-id="df2bc-160">**Mode**: GRPC http istek isteğinin `Content-Type` `application/grpc-web` veya `application/grpc-web-text`olup olmadığını belirten bir numaralandırma türü.</span><span class="sxs-lookup"><span data-stu-id="df2bc-160">**Mode**: An enumeration type that specifies whether the gRPC HTTP request request `Content-Type` is `application/grpc-web` or `application/grpc-web-text`.</span></span>
    * <span data-ttu-id="df2bc-161">`GrpcWebMode.GrpcWeb`, içeriği kodlama olmadan gönderilecek şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="df2bc-161">`GrpcWebMode.GrpcWeb` configures content to be sent without encoding.</span></span> <span data-ttu-id="df2bc-162">Varsayılan değer.</span><span class="sxs-lookup"><span data-stu-id="df2bc-162">Default value.</span></span>
    * <span data-ttu-id="df2bc-163">`GrpcWebMode.GrpcWebText`, içeriği Base64 kodlamalı olacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="df2bc-163">`GrpcWebMode.GrpcWebText` configures content to be base64 encoded.</span></span> <span data-ttu-id="df2bc-164">Tarayıcılarda sunucu akış çağrıları için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="df2bc-164">Required for server streaming calls in browsers.</span></span>
* <span data-ttu-id="df2bc-165">**HttpVersion**: temel alınan GRPC http Isteğindeki [HttpRequestMessage. Version](xref:System.Net.Http.HttpRequestMessage.Version) ayarlamak için kullanılan http Protokolü `Version`.</span><span class="sxs-lookup"><span data-stu-id="df2bc-165">**HttpVersion**: HTTP protocol `Version` used to set [HttpRequestMessage.Version](xref:System.Net.Http.HttpRequestMessage.Version) on the underlying gRPC HTTP request.</span></span> <span data-ttu-id="df2bc-166">gRPC-Web belirli bir sürüm gerektirmez ve belirtilmediği takdirde varsayılanı geçersiz kılmaz.</span><span class="sxs-lookup"><span data-stu-id="df2bc-166">gRPC-Web doesn't require a specific version and doesn't override the default unless specified.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="df2bc-167">Oluşturulan gRPC istemcilerinin birli yöntemleri çağırmak için eşitleme ve zaman uyumsuz yöntemleri vardır.</span><span class="sxs-lookup"><span data-stu-id="df2bc-167">Generated gRPC clients have sync and async methods for calling unary methods.</span></span> <span data-ttu-id="df2bc-168">Örneğin, `SayHello` eşitlenir ve `SayHelloAsync` zaman uyumsuz olur.</span><span class="sxs-lookup"><span data-stu-id="df2bc-168">For example, `SayHello` is sync and `SayHelloAsync` is async.</span></span> <span data-ttu-id="df2bc-169">Bir Blazor WebAssembly uygulamasında bir Sync yönteminin çağrılması uygulamanın yanıt vermemeye başlamasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="df2bc-169">Calling a sync method in a Blazor WebAssembly app will cause the app to become unresponsive.</span></span> <span data-ttu-id="df2bc-170">Zaman uyumsuz yöntemlerin her zaman Blazor WebAssembly içinde kullanılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="df2bc-170">Async methods must always be used in Blazor WebAssembly.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="df2bc-171">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="df2bc-171">Additional resources</span></span>

* [<span data-ttu-id="df2bc-172">Web Istemcileri için gRPC GitHub projesi</span><span class="sxs-lookup"><span data-stu-id="df2bc-172">gRPC for Web Clients GitHub project</span></span>](https://github.com/grpc/grpc-web)
* <xref:security/cors>
