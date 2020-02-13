---
title: Tarayıcı uygulamalarında gRPC kullanma
author: jamesnk
description: ASP.NET Core GRPC hizmetlerini, gRPC-Web kullanan tarayıcı uygulamalarından çağrılabilir olacak şekilde nasıl yapılandıracağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 02/10/2020
uid: grpc/browser
ms.openlocfilehash: 333fc8c4277bbac47042d4904c276e963186914a
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172273"
---
# <a name="use-grpc-in-browser-apps"></a><span data-ttu-id="77a06-103">Tarayıcı uygulamalarında gRPC kullanma</span><span class="sxs-lookup"><span data-stu-id="77a06-103">Use gRPC in browser apps</span></span>

<span data-ttu-id="77a06-104">, [James bAyKiNg](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="77a06-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="77a06-105">**gRPC-.NET 'teki Web desteği deneysel**</span><span class="sxs-lookup"><span data-stu-id="77a06-105">**gRPC-Web support in .NET is experimental**</span></span>
>
> <span data-ttu-id="77a06-106">gRPC-.NET için Web, kaydedilmiş bir ürün değil, deneysel bir projem projem.</span><span class="sxs-lookup"><span data-stu-id="77a06-106">gRPC-Web for .NET is an experimental project, not a committed product.</span></span> <span data-ttu-id="77a06-107">Şunları yapmak istiyoruz:</span><span class="sxs-lookup"><span data-stu-id="77a06-107">We want to:</span></span>
>
> * <span data-ttu-id="77a06-108">GRPC-Web ' i uygulamaya yönelik yaklaşımımızı test edin.</span><span class="sxs-lookup"><span data-stu-id="77a06-108">Test that our approach to implementing gRPC-Web works.</span></span>
> * <span data-ttu-id="77a06-109">Bu yaklaşım .NET geliştiricileri için, bir ara sunucu aracılığıyla gRPC-Web ayarlamanın geleneksel bir yolu ile karşılaştırıldığında yararlı olup olmadığını hakkında geri bildirim alın.</span><span class="sxs-lookup"><span data-stu-id="77a06-109">Get feedback on if this approach is useful to .NET developers compared to the traditional way of setting up gRPC-Web via a proxy.</span></span>
>
> <span data-ttu-id="77a06-110">Bu geliştiricilerin, ve ile üretken olduğu bir şey oluşturduğumuzun olduğundan emin olmak için [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) lütfen geri bildirimde bulunun.</span><span class="sxs-lookup"><span data-stu-id="77a06-110">Please leave feedback at [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) to ensure we build something that developers like and are productive with.</span></span>

<span data-ttu-id="77a06-111">Tarayıcı tabanlı bir uygulamadan HTTP/2 gRPC hizmetini çağırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="77a06-111">It is not possible to call a HTTP/2 gRPC service from a browser-based app.</span></span> <span data-ttu-id="77a06-112">[GRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) , tarayıcı JavaScript ve Blazor uygulamalarının GRPC hizmetlerini çağırmasını sağlayan bir protokoldür.</span><span class="sxs-lookup"><span data-stu-id="77a06-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) is a protocol that allows browser JavaScript and Blazor apps to call gRPC services.</span></span> <span data-ttu-id="77a06-113">Bu makalede, .NET Core 'da gRPC-Web ' i kullanma açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="77a06-113">This article explains how to use gRPC-Web in .NET Core.</span></span>

## <a name="configure-grpc-web-in-aspnet-core"></a><span data-ttu-id="77a06-114">ASP.NET Core 'de gRPC-Web yapılandırma</span><span class="sxs-lookup"><span data-stu-id="77a06-114">Configure gRPC-Web in ASP.NET Core</span></span>

<span data-ttu-id="77a06-115">ASP.NET Core 'de barındırılan gRPC Hizmetleri, gRPC-Web ' i HTTP/2 gRPC ile birlikte destekleyecek şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="77a06-115">gRPC services hosted in ASP.NET Core can be configured to support gRPC-Web alongside HTTP/2 gRPC.</span></span> <span data-ttu-id="77a06-116">gRPC-Web, hizmetlerde herhangi bir değişiklik yapılmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="77a06-116">gRPC-Web does not require any changes to services.</span></span> <span data-ttu-id="77a06-117">Tek değişiklik başlangıç yapılandırması ' dır.</span><span class="sxs-lookup"><span data-stu-id="77a06-117">The only modification is startup configuration.</span></span>

<span data-ttu-id="77a06-118">GRPC-Web ' i bir ASP.NET Core gRPC hizmeti ile etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="77a06-118">To enable gRPC-Web with an ASP.NET Core gRPC service:</span></span>

* <span data-ttu-id="77a06-119">[GRPC. AspNetCore. Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) paketine bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="77a06-119">Add a reference to the [Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) package.</span></span>
* <span data-ttu-id="77a06-120">*Startup.cs*'e `AddGrpcWeb` ve `UseGrpcWeb` ekleyerek, uygulamayı GRPC-Web kullanacak şekilde yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="77a06-120">Configure the app to use gRPC-Web by adding `AddGrpcWeb` and `UseGrpcWeb` to *Startup.cs*:</span></span>

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=10,14)]

<span data-ttu-id="77a06-121">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="77a06-121">The preceding code:</span></span>

* <span data-ttu-id="77a06-122">Yönlendirme ve bitiş noktalarından önce `UseGrpcWeb`gRPC-Web ara yazılımını ekler.</span><span class="sxs-lookup"><span data-stu-id="77a06-122">Adds the gRPC-Web middleware, `UseGrpcWeb`, after routing and before endpoints.</span></span>
* <span data-ttu-id="77a06-123">`endpoints.MapGrpcService<GreeterService>()` yönteminin `EnableGrpcWeb`ile gRPC-Web 'i desteklediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="77a06-123">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports gRPC-Web with `EnableGrpcWeb`.</span></span> 

<span data-ttu-id="77a06-124">Alternatif olarak, tüm hizmetleri, bir `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` ConfigureServices 'e ekleyerek gRPC-Web ' i destekleyecek şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="77a06-124">Alternatively, configure all services to support gRPC-Web by adding `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` to ConfigureServices.</span></span>

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=6,13)]

<span data-ttu-id="77a06-125">ASP.NET Core CORS 'yi destekleyecek şekilde yapılandırmak gibi, tarayıcıdan gRPC-Web ' i çağırmak için bazı ek yapılandırmalar gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="77a06-125">Some additional configuration may be required to call gRPC-Web from the browser, such as configuring ASP.NET Core to support CORS.</span></span> <span data-ttu-id="77a06-126">Daha fazla bilgi için bkz. [cors desteği](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="77a06-126">For more information, see [support CORS](xref:security/cors).</span></span>

## <a name="call-grpc-web-from-the-browser"></a><span data-ttu-id="77a06-127">Tarayıcıdan gRPC-Web 'i çağırma</span><span class="sxs-lookup"><span data-stu-id="77a06-127">Call gRPC-Web from the browser</span></span>

<span data-ttu-id="77a06-128">Tarayıcı uygulamaları GRPC hizmetlerini çağırmak için gRPC-Web kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="77a06-128">Browser apps can use gRPC-Web to call gRPC services.</span></span> <span data-ttu-id="77a06-129">GRPC hizmetlerini tarayıcıda GRPC-Web ile çağırırken bazı gereksinimler ve sınırlamalar vardır:</span><span class="sxs-lookup"><span data-stu-id="77a06-129">There are some requirements and limitations when calling gRPC services with gRPC-Web from the browser:</span></span>

* <span data-ttu-id="77a06-130">Sunucu, gRPC-Web ' i destekleyecek şekilde yapılandırılmış olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="77a06-130">The server must have been configured to support gRPC-Web.</span></span>
* <span data-ttu-id="77a06-131">İstemci akışı ve çift yönlü akış çağrıları desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="77a06-131">Client streaming and bidirectional streaming calls aren't supported.</span></span> <span data-ttu-id="77a06-132">Sunucu akışı destekleniyor.</span><span class="sxs-lookup"><span data-stu-id="77a06-132">Server streaming is supported.</span></span>
* <span data-ttu-id="77a06-133">Farklı bir etki alanında gRPC hizmetlerinin çağrılması için [CORS](xref:security/cors) 'nin sunucuda yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="77a06-133">Calling gRPC services on a different domain requires [CORS](xref:security/cors) to be configured on the server.</span></span>

### <a name="javascript-grpc-web-client"></a><span data-ttu-id="77a06-134">JavaScript gRPC-Web istemcisi</span><span class="sxs-lookup"><span data-stu-id="77a06-134">JavaScript gRPC-Web client</span></span>

<span data-ttu-id="77a06-135">Bir JavaScript gRPC-Web istemcisi vardır.</span><span class="sxs-lookup"><span data-stu-id="77a06-135">There is a JavaScript gRPC-Web client.</span></span> <span data-ttu-id="77a06-136">JavaScript 'ten gRPC-Web kullanma hakkında yönergeler için bkz. [gRPC-web Ile JavaScript istemci kodu yazma](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span><span class="sxs-lookup"><span data-stu-id="77a06-136">For instructions on how to use gRPC-Web from JavaScript, see [write JavaScript client code with gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span></span>

### <a name="configure-grpc-web-with-the-net-grpc-client"></a><span data-ttu-id="77a06-137">.NET gRPC istemcisiyle gRPC-Web yapılandırma</span><span class="sxs-lookup"><span data-stu-id="77a06-137">Configure gRPC-Web with the .NET gRPC client</span></span>

<span data-ttu-id="77a06-138">.NET gRPC istemcisi, gRPC-Web çağrıları yapmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="77a06-138">The .NET gRPC client can be configured to make gRPC-Web calls.</span></span> <span data-ttu-id="77a06-139">Bu, tarayıcıda barındırılan ve JavaScript kodunda aynı HTTP kısıtlamalarına sahip olan [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) uygulamaları için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="77a06-139">This is useful for [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) apps, which are hosted in the browser and have the same HTTP limitations of JavaScript code.</span></span> <span data-ttu-id="77a06-140">Bir .NET istemcisiyle gRPC-Web ' i çağırmak [http/2 gRPC ile aynıdır](xref:grpc/client).</span><span class="sxs-lookup"><span data-stu-id="77a06-140">Calling gRPC-Web with a .NET client is [the same as HTTP/2 gRPC](xref:grpc/client).</span></span> <span data-ttu-id="77a06-141">Tek değişiklik, kanalın oluşturulma şekli olur.</span><span class="sxs-lookup"><span data-stu-id="77a06-141">The only modification is how the channel is created.</span></span>

<span data-ttu-id="77a06-142">GRPC-Web kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="77a06-142">To use gRPC-Web:</span></span>

* <span data-ttu-id="77a06-143">[GRPC .net. Client. Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) paketine başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="77a06-143">Add a reference to the [Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) package.</span></span>
* <span data-ttu-id="77a06-144">[GRPC .net. Client](https://www.nuget.org/packages/Grpc.Net.Client) paketine yönelik başvurunun 2.27.0 veya daha büyük olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="77a06-144">Ensure the reference to [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) package is 2.27.0 or greater.</span></span>
* <span data-ttu-id="77a06-145">Kanalı `GrpcWebHandler`kullanacak şekilde yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="77a06-145">Configure the channel to use the `GrpcWebHandler`:</span></span>

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

<span data-ttu-id="77a06-146">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="77a06-146">The preceding code:</span></span>

* <span data-ttu-id="77a06-147">GRPC-Web kullanmak için bir kanal yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="77a06-147">Configures a channel to use gRPC-Web.</span></span>
* <span data-ttu-id="77a06-148">Bir istemci oluşturur ve kanalı kullanarak bir çağrı yapar.</span><span class="sxs-lookup"><span data-stu-id="77a06-148">Creates a client and makes a call using the channel.</span></span>

<span data-ttu-id="77a06-149">`GrpcWebHandler` oluşturulduğunda aşağıdaki yapılandırma seçeneklerine sahiptir:</span><span class="sxs-lookup"><span data-stu-id="77a06-149">The `GrpcWebHandler` has the following configuration options when created:</span></span>

* <span data-ttu-id="77a06-150">**InnerHandler**: GRPC http isteğini oluşturan temel <xref:System.Net.Http.HttpMessageHandler>, örneğin, `HttpClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="77a06-150">**InnerHandler**: The underlying <xref:System.Net.Http.HttpMessageHandler> that makes the gRPC HTTP request, for example, `HttpClientHandler`.</span></span>
* <span data-ttu-id="77a06-151">**Mode**: GRPC http istek isteğinin `Content-Type` `application/grpc-web` veya `application/grpc-web-text`olup olmadığını belirten bir numaralandırma türü.</span><span class="sxs-lookup"><span data-stu-id="77a06-151">**Mode**: An enumeration type that specifies whether the gRPC HTTP request request `Content-Type` is `application/grpc-web` or `application/grpc-web-text`.</span></span>
    * <span data-ttu-id="77a06-152">`GrpcWebMode.GrpcWeb`, içeriği kodlama olmadan gönderilecek şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="77a06-152">`GrpcWebMode.GrpcWeb` configures content to be sent without encoding.</span></span> <span data-ttu-id="77a06-153">Varsayılan değer.</span><span class="sxs-lookup"><span data-stu-id="77a06-153">Default value.</span></span>
    * <span data-ttu-id="77a06-154">`GrpcWebMode.GrpcWebText`, içeriği Base64 kodlamalı olacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="77a06-154">`GrpcWebMode.GrpcWebText` configures content to be base64 encoded.</span></span> <span data-ttu-id="77a06-155">Tarayıcılarda sunucu akış çağrıları için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="77a06-155">Required for server streaming calls in browsers.</span></span>
* <span data-ttu-id="77a06-156">**HttpVersion**: temel alınan GRPC http Isteğindeki [HttpRequestMessage. Version](xref:System.Net.Http.HttpRequestMessage.Version) ayarlamak için kullanılan http Protokolü `Version`.</span><span class="sxs-lookup"><span data-stu-id="77a06-156">**HttpVersion**: HTTP protocol `Version` used to set [HttpRequestMessage.Version](xref:System.Net.Http.HttpRequestMessage.Version) on the underlying gRPC HTTP request.</span></span> <span data-ttu-id="77a06-157">gRPC-Web belirli bir sürüm gerektirmez ve belirtilmediği takdirde varsayılanı geçersiz kılmaz.</span><span class="sxs-lookup"><span data-stu-id="77a06-157">gRPC-Web doesn't require a specific version and doesn't override the default unless specified.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="77a06-158">Oluşturulan gRPC istemcilerinin birli yöntemleri çağırmak için eşitleme ve zaman uyumsuz yöntemleri vardır.</span><span class="sxs-lookup"><span data-stu-id="77a06-158">Generated gRPC clients have sync and async methods for calling unary methods.</span></span> <span data-ttu-id="77a06-159">Örneğin, `SayHello` eşitlenir ve `SayHelloAsync` zaman uyumsuz olur.</span><span class="sxs-lookup"><span data-stu-id="77a06-159">For example, `SayHello` is sync and `SayHelloAsync` is async.</span></span> <span data-ttu-id="77a06-160">Bir Blazor WebAssembly uygulamasında bir Sync yönteminin çağrılması uygulamanın yanıt vermemeye başlamasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="77a06-160">Calling a sync method in a Blazor WebAssembly app will cause the app to become unresponsive.</span></span> <span data-ttu-id="77a06-161">Zaman uyumsuz yöntemlerin her zaman Blazor WebAssembly içinde kullanılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="77a06-161">Async methods must always be used in Blazor WebAssembly.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="77a06-162">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="77a06-162">Additional resources</span></span>

* [<span data-ttu-id="77a06-163">Web Istemcileri için gRPC GitHub projesi</span><span class="sxs-lookup"><span data-stu-id="77a06-163">gRPC for Web Clients GitHub project</span></span>](https://github.com/grpc/grpc-web)
* <xref:security/cors>
