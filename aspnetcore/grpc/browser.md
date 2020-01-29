---
title: tarayıcı uygulamalarında gRPC
author: jamesnk
description: ASP.NET Core GRPC hizmetlerini, gRPC-Web kullanan tarayıcı uygulamalarından çağrılabilir olacak şekilde nasıl yapılandıracağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 01/24/2020
uid: grpc/browser
ms.openlocfilehash: 6359c3b76b3cb1ba2b6d9f9a989f64cbf4c4379d
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76830662"
---
# <a name="grpc-in-browser-apps"></a><span data-ttu-id="0c644-103">tarayıcı uygulamalarında gRPC</span><span class="sxs-lookup"><span data-stu-id="0c644-103">gRPC in browser apps</span></span>

<span data-ttu-id="0c644-104">, [James bAyKiNg](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="0c644-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0c644-105">**gRPC-.NET 'teki Web desteği deneysel**</span><span class="sxs-lookup"><span data-stu-id="0c644-105">**gRPC-Web support in .NET is experimental**</span></span>
>
> <span data-ttu-id="0c644-106">gRPC-.NET için Web, kaydedilmiş bir ürün değil, deneysel bir projem projem.</span><span class="sxs-lookup"><span data-stu-id="0c644-106">gRPC-Web for .NET is an experimental project, not a committed product.</span></span> <span data-ttu-id="0c644-107">Şunları yapmak istiyoruz:</span><span class="sxs-lookup"><span data-stu-id="0c644-107">We want to:</span></span>
>
> * <span data-ttu-id="0c644-108">GRPC-Web ' i uygulamaya yönelik yaklaşımımızı test edin.</span><span class="sxs-lookup"><span data-stu-id="0c644-108">Test that our approach to implementing gRPC-Web works.</span></span>
> * <span data-ttu-id="0c644-109">Bu yaklaşım .NET geliştiricileri için, bir ara sunucu aracılığıyla gRPC-Web ayarlamanın geleneksel bir yolu ile karşılaştırıldığında yararlı olup olmadığını hakkında geri bildirim alın.</span><span class="sxs-lookup"><span data-stu-id="0c644-109">Get feedback on if this approach is useful to .NET developers compared to the traditional way of setting up gRPC-Web via a proxy.</span></span>
>
> <span data-ttu-id="0c644-110">Bu geliştiricilerin, ve ile üretken olduğu bir şey oluşturduğumuzun olduğundan emin olmak için [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) lütfen geri bildirimde bulunun.</span><span class="sxs-lookup"><span data-stu-id="0c644-110">Please leave feedback at [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) to ensure we build something that developers like and are productive with.</span></span>

<span data-ttu-id="0c644-111">Tarayıcı tabanlı bir uygulamadan HTTP/2 gRPC hizmetini çağırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="0c644-111">It is not possible to call a HTTP/2 gRPC service from a browser-based app.</span></span> <span data-ttu-id="0c644-112">[GRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) , tarayıcı JavaScript ve Blazor uygulamalarının GRPC hizmetlerini çağırmasını sağlayan bir protokoldür.</span><span class="sxs-lookup"><span data-stu-id="0c644-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) is a protocol that allows browser JavaScript and Blazor apps to call gRPC services.</span></span> <span data-ttu-id="0c644-113">Bu makalede, .NET Core 'da gRPC-Web ' i kullanma açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="0c644-113">This article explains how to use gRPC-Web in .NET Core.</span></span>

## <a name="configure-grpc-web-in-aspnet-core"></a><span data-ttu-id="0c644-114">ASP.NET Core 'de gRPC-Web yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0c644-114">Configure gRPC-Web in ASP.NET Core</span></span>

<span data-ttu-id="0c644-115">ASP.NET Core 'de barındırılan gRPC Hizmetleri, gRPC-Web ' i HTTP/2 gRPC ile birlikte destekleyecek şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="0c644-115">gRPC services hosted in ASP.NET Core can be configured to support gRPC-Web alongside HTTP/2 gRPC.</span></span> <span data-ttu-id="0c644-116">gRPC-Web, hizmetlerde herhangi bir değişiklik yapılmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="0c644-116">gRPC-Web does not require any changes to services.</span></span> <span data-ttu-id="0c644-117">Tek değişiklik başlangıç yapılandırması ' dır.</span><span class="sxs-lookup"><span data-stu-id="0c644-117">The only modification is startup configuration.</span></span>

<span data-ttu-id="0c644-118">GRPC-Web ' i bir ASP.NET Core gRPC hizmeti ile etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="0c644-118">To enable gRPC-Web with an ASP.NET Core gRPC service:</span></span>

* <span data-ttu-id="0c644-119">[GRPC. AspNetCore. Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) paketine bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0c644-119">Add a reference to the [Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) package.</span></span>
* <span data-ttu-id="0c644-120">*Startup.cs*'e `AddGrpcWeb` ve `UseGrpcWeb` ekleyerek, uygulamayı GRPC-Web kullanacak şekilde yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="0c644-120">Configure the app to use gRPC-Web by adding `AddGrpcWeb` and `UseGrpcWeb` to *Startup.cs*:</span></span>

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=3,10,14)]

<span data-ttu-id="0c644-121">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="0c644-121">The preceding code:</span></span>

* <span data-ttu-id="0c644-122">Yönlendirme ve bitiş noktalarından önce `UseGrpcWeb`gRPC-Web ara yazılımını ekler.</span><span class="sxs-lookup"><span data-stu-id="0c644-122">Adds the gRPC-Web middleware, `UseGrpcWeb`, after routing and before endpoints.</span></span>
* <span data-ttu-id="0c644-123">`endpoints.MapGrpcService<GreeterService>()` yönteminin `EnableGrpcWeb`ile gRPC-Web 'i desteklediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="0c644-123">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports gRPC-Web with `EnableGrpcWeb`.</span></span> 

<span data-ttu-id="0c644-124">Alternatif olarak, tüm hizmetleri, bir `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` ConfigureServices 'e ekleyerek gRPC-Web ' i destekleyecek şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="0c644-124">Alternatively, configure all services to support gRPC-Web by adding `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` to ConfigureServices.</span></span>

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=5,12,16)]

<span data-ttu-id="0c644-125">ASP.NET Core CORS 'yi destekleyecek şekilde yapılandırmak gibi, tarayıcıdan gRPC-Web ' i çağırmak için bazı ek yapılandırmalar gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="0c644-125">Some additional configuration may be required to call gRPC-Web from the browser, such as configuring ASP.NET Core to support CORS.</span></span> <span data-ttu-id="0c644-126">Daha fazla bilgi için bkz. [cors desteği](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="0c644-126">For more information, see [support CORS](xref:security/cors).</span></span>

## <a name="call-grpc-web-from-the-browser"></a><span data-ttu-id="0c644-127">Tarayıcıdan gRPC-Web 'i çağırma</span><span class="sxs-lookup"><span data-stu-id="0c644-127">Call gRPC-Web from the browser</span></span>

<span data-ttu-id="0c644-128">Tarayıcı uygulamaları GRPC hizmetlerini çağırmak için gRPC-Web kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="0c644-128">Browser apps can use gRPC-Web to call gRPC services.</span></span> <span data-ttu-id="0c644-129">GRPC hizmetlerini tarayıcıda GRPC-Web ile çağırırken bazı gereksinimler ve sınırlamalar vardır:</span><span class="sxs-lookup"><span data-stu-id="0c644-129">There are some requirements and limitations when calling gRPC services with gRPC-Web from the browser:</span></span>

* <span data-ttu-id="0c644-130">Sunucu, gRPC-Web ' i destekleyecek şekilde yapılandırılmış olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0c644-130">The server must have been configured to support gRPC-Web.</span></span>
* <span data-ttu-id="0c644-131">İstemci akışı ve çift yönlü akış çağrıları desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="0c644-131">Client streaming and bidirectional streaming calls aren't supported.</span></span> <span data-ttu-id="0c644-132">Sunucu akışı destekleniyor.</span><span class="sxs-lookup"><span data-stu-id="0c644-132">Server streaming is supported.</span></span>
* <span data-ttu-id="0c644-133">Farklı bir etki alanında gRPC hizmetlerinin çağrılması için [CORS](xref:security/cors) 'nin sunucuda yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="0c644-133">Calling gRPC services on a different domain requires [CORS](xref:security/cors) to be configured on the server.</span></span>

### <a name="javascript-grpc-web-client"></a><span data-ttu-id="0c644-134">JavaScript gRPC-Web istemcisi</span><span class="sxs-lookup"><span data-stu-id="0c644-134">JavaScript gRPC-Web client</span></span>

<span data-ttu-id="0c644-135">Bir JavaScript gRPC-Web istemcisi vardır.</span><span class="sxs-lookup"><span data-stu-id="0c644-135">There is a JavaScript gRPC-Web client.</span></span> <span data-ttu-id="0c644-136">JavaScript 'ten gRPC-Web kullanma hakkında yönergeler için bkz. [gRPC-web Ile JavaScript istemci kodu yazma](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span><span class="sxs-lookup"><span data-stu-id="0c644-136">For instructions on how to use gRPC-Web from JavaScript, see [write JavaScript client code with gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span></span>

### <a name="configure-grpc-web-with-the-net-grpc-client"></a><span data-ttu-id="0c644-137">.NET gRPC istemcisiyle gRPC-Web yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0c644-137">Configure gRPC-Web with the .NET gRPC client</span></span>

<span data-ttu-id="0c644-138">.NET gRPC istemcisi, gRPC-Web çağrıları yapmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="0c644-138">The .NET gRPC client can be configured to make gRPC-Web calls.</span></span> <span data-ttu-id="0c644-139">Bu, tarayıcıda barındırılan ve JavaScript kodunda aynı HTTP kısıtlamalarına sahip olan [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) uygulamaları için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="0c644-139">This is useful for [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) apps, which are hosted in the browser and have the same HTTP limitations of JavaScript code.</span></span> <span data-ttu-id="0c644-140">Bir .NET istemcisiyle gRPC-Web ' i çağırmak [http/2 gRPC ile aynıdır](xref:grpc/client).</span><span class="sxs-lookup"><span data-stu-id="0c644-140">Calling gRPC-Web with a .NET client is [the same as HTTP/2 gRPC](xref:grpc/client).</span></span> <span data-ttu-id="0c644-141">Tek değişiklik, kanalın oluşturulma şekli olur.</span><span class="sxs-lookup"><span data-stu-id="0c644-141">The only modification is how the channel is created.</span></span>

<span data-ttu-id="0c644-142">GRPC-Web kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="0c644-142">To use gRPC-Web:</span></span>

* <span data-ttu-id="0c644-143">[GRPC .net. Client. Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) paketine başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0c644-143">Add a reference to the [Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) package.</span></span>
* <span data-ttu-id="0c644-144">Kanalı `GrpcWebHandler`kullanacak şekilde yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="0c644-144">Configure the channel to use the `GrpcWebHandler`:</span></span>

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

<span data-ttu-id="0c644-145">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="0c644-145">The preceding code:</span></span>

* <span data-ttu-id="0c644-146">GRPC-Web kullanmak için bir kanal yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="0c644-146">Configures a channel to use gRPC-Web.</span></span>
* <span data-ttu-id="0c644-147">Bir istemci oluşturur ve kanalı kullanarak bir çağrı yapar.</span><span class="sxs-lookup"><span data-stu-id="0c644-147">Creates a client and makes a call using the channel.</span></span>

<span data-ttu-id="0c644-148">`GrpcWebHandler` oluşturulduğunda aşağıdaki yapılandırma seçeneklerine sahiptir:</span><span class="sxs-lookup"><span data-stu-id="0c644-148">The `GrpcWebHandler` has the following configuration options when created:</span></span>

* <span data-ttu-id="0c644-149">**InnerHandler**: http çağrısını yapan temel <xref:System.Net.Http.HttpMessageHandler>, örneğin, `HttpClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="0c644-149">**InnerHandler**: The underlying <xref:System.Net.Http.HttpMessageHandler> that makes the HTTP call, for example, `HttpClientHandler`.</span></span>
* <span data-ttu-id="0c644-150">**Mod**: enum `GrpcWebMode`.</span><span class="sxs-lookup"><span data-stu-id="0c644-150">**Mode**: `GrpcWebMode` enum.</span></span> <span data-ttu-id="0c644-151">`GrpcWebMode.GrpcWebText`, içeriği Base64 kodlamalı olacak şekilde yapılandırır, bu da sunucu akış çağrılarını desteklemek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="0c644-151">`GrpcWebMode.GrpcWebText` configures content to be base64 encoded, which is required to support server streaming calls.</span></span>
* <span data-ttu-id="0c644-152">**HttpVersion**: HTTP protokol `Version`.</span><span class="sxs-lookup"><span data-stu-id="0c644-152">**HttpVersion**: HTTP protocol `Version`.</span></span> <span data-ttu-id="0c644-153">gRPC-Web belirli bir protokol gerektirmez ve yapılandırılmadığı takdirde bir istek yapıldığında bir tane belirtmez.</span><span class="sxs-lookup"><span data-stu-id="0c644-153">gRPC-Web doesn't require a specific protocol and won't specify one when making a request unless configured.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0c644-154">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="0c644-154">Additional resources</span></span>

* [<span data-ttu-id="0c644-155">Web Istemcileri için gRPC GitHub projesi</span><span class="sxs-lookup"><span data-stu-id="0c644-155">gRPC for Web Clients GitHub project</span></span>](https://github.com/grpc/grpc-web)
* <xref:security/cors>
