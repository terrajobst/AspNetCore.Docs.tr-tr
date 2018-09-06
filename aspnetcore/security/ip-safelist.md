---
title: İstemci IP Güvenli ASP.NET Core için liste
author: damienbod
description: Uzak IP adresleri, onaylanan IP adreslerinden oluşan bir liste karşı doğrulamak için bir ara yazılım ya da eylem filtreleri yazmayı öğrenin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: 40fe7b67359efd1692490099c3fb529ba4a6148f
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040125"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="26248-103">İstemci IP Güvenli ASP.NET Core için liste</span><span class="sxs-lookup"><span data-stu-id="26248-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="26248-104">Tarafından [Damien Bowden](https://twitter.com/damien_bod) ve [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="26248-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="26248-105">Bu makalede, bir IP güvenli liste (beyaz liste olarak da bilinir) uygulamak için iki yolunu gösterir:</span><span class="sxs-lookup"><span data-stu-id="26248-105">This article shows two ways to implement an IP safelist (also known as a whitelist):</span></span>

* <span data-ttu-id="26248-106">Uzak IP adresi her isteğin denetlemek için ASP.NET Core ara yazılımı kullanarak.</span><span class="sxs-lookup"><span data-stu-id="26248-106">By using ASP.NET Core middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="26248-107">Belirli bir eylem yöntemleri için isteklerin uzak IP adresi denetlemek için ASP.NET Core eylem filtrelerini kullanarak.</span><span class="sxs-lookup"><span data-stu-id="26248-107">By using ASP.NET Core action filters to check the remote IP address of requests for specific action methods.</span></span>

<span data-ttu-id="26248-108">Örnek uygulamayı her iki yaklaşımı da gösterir.</span><span class="sxs-lookup"><span data-stu-id="26248-108">The sample app illustrates both approaches.</span></span> <span data-ttu-id="26248-109">Her durumda, bir uygulama ayarı onaylı istemci IP adreslerini içeren bir dize olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="26248-109">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="26248-110">Ara yazılım veya filtre bir listeye dizeyi ayrıştırır ve uzak IP listesinde olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="26248-110">The middleware or filter parses the string into a list and  checks if the remote IP is in the list.</span></span> <span data-ttu-id="26248-111">Aksi durumda, bir HTTP 403 Yasak durum kodu döndürülür.</span><span class="sxs-lookup"><span data-stu-id="26248-111">If not, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="26248-112">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="26248-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-safelist"></a><span data-ttu-id="26248-113">Güvenli liste</span><span class="sxs-lookup"><span data-stu-id="26248-113">The safelist</span></span>

<span data-ttu-id="26248-114">Listenin yapılandırılan *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="26248-114">The list is configured in the *appsettings.json* file.</span></span> <span data-ttu-id="26248-115">Bu, noktalı virgülle ayrılmış listesidir ve IPv4 ve IPv6 adreslerini içermelidir.</span><span class="sxs-lookup"><span data-stu-id="26248-115">It's a semicolon-delimited list and can contain IPv4 and IPv6 addresses.</span></span>

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a><span data-ttu-id="26248-116">Ara yazılım</span><span class="sxs-lookup"><span data-stu-id="26248-116">Middleware</span></span>

<span data-ttu-id="26248-117">`Configure` Yöntemi ara yazılımı ekler ve güvenli liste dize bir oluşturucu parametresi geçirilir.</span><span class="sxs-lookup"><span data-stu-id="26248-117">The `Configure` method adds the middleware and passes the safelist string to it in a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=7)]

<span data-ttu-id="26248-118">Ara yazılım bir diziye dizeyi ayrıştırır ve dizideki uzak IP adresi arar.</span><span class="sxs-lookup"><span data-stu-id="26248-118">The middleware parses the string into an array and looks for the remote IP address in the array.</span></span> <span data-ttu-id="26248-119">Uzak IP adresi bulunmazsa, ara yazılım 401 HTTP Yasak döndürür.</span><span class="sxs-lookup"><span data-stu-id="26248-119">If the remote IP address is not found, the middleware returns HTTP 401 Forbidden.</span></span> <span data-ttu-id="26248-120">Bu doğrulama işlemi, HTTP Get isteklerini atlanır.</span><span class="sxs-lookup"><span data-stu-id="26248-120">This validation process is bypassed for HTTP Get requests.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="26248-121">Eylem filtresi</span><span class="sxs-lookup"><span data-stu-id="26248-121">Action filter</span></span>

<span data-ttu-id="26248-122">Yalnızca belirli denetleyicileri veya eylem yöntemleri için bir güvenli liste istiyorsanız, bir eyleme eylem filtresi kullanın.</span><span class="sxs-lookup"><span data-stu-id="26248-122">If you want a safelist only for specific controllers or action methods, use an action filter.</span></span> <span data-ttu-id="26248-123">Örnek buradadır:</span><span class="sxs-lookup"><span data-stu-id="26248-123">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

<span data-ttu-id="26248-124">Eylem filtresi Hizmetleri kapsayıcıya eklenir.</span><span class="sxs-lookup"><span data-stu-id="26248-124">The action filter is added to the services container.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="26248-125">Filtre, bir denetleyici veya eylem yöntemi kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="26248-125">The filter can then be used on a controller or action method.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

<span data-ttu-id="26248-126">Filtrenin uygulandığı örnek uygulamada, `Get` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="26248-126">In the sample app, the filter is applied to the `Get` method.</span></span> <span data-ttu-id="26248-127">Bunu göndererek uygulamayı test ettiğinizde bir `Get` API istek, öznitelik istemci IP adresini doğrulama.</span><span class="sxs-lookup"><span data-stu-id="26248-127">So when you test the app by sending a `Get` API request, the attribute is validating the client IP address.</span></span> <span data-ttu-id="26248-128">Ara yazılım, herhangi bir HTTP yöntemi ile API çağrısı yaparak test ettiğinizde, istemci IP'sini doğruluyor.</span><span class="sxs-lookup"><span data-stu-id="26248-128">When you test by calling the API with any other HTTP method, the middleware is validating the client IP.</span></span>

## <a name="razor-pages-filter"></a><span data-ttu-id="26248-129">Razor sayfaları Filtrele</span><span class="sxs-lookup"><span data-stu-id="26248-129">Razor Pages filter</span></span> 

<span data-ttu-id="26248-130">Bir Razor sayfaları uygulaması için bir güvenli liste istiyorsanız, bir Razor sayfaları filtresini kullanın.</span><span class="sxs-lookup"><span data-stu-id="26248-130">If you want a safelist for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="26248-131">Örnek buradadır:</span><span class="sxs-lookup"><span data-stu-id="26248-131">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

<span data-ttu-id="26248-132">MVC filtreleri koleksiyon ekleyerek bu filtre etkindir.</span><span class="sxs-lookup"><span data-stu-id="26248-132">This filter is enabled by adding it to the MVC Filters collection.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

<span data-ttu-id="26248-133">Uygulamayı çalıştırın ve bir Razor sayfası istek istemci IP'sini Razor sayfaları filtre doğruluyor.</span><span class="sxs-lookup"><span data-stu-id="26248-133">When you run the app and request a Razor page, the Razor Pages filter is validating the client IP.</span></span>

## <a name="next-steps"></a><span data-ttu-id="26248-134">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="26248-134">Next steps</span></span>

<span data-ttu-id="26248-135">[ASP.NET Core ara yazılım hakkında daha fazla bilgi](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="26248-135">[Learn more about ASP.NET Core Middleware](xref:fundamentals/middleware/index).</span></span>
