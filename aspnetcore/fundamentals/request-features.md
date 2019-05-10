---
title: ASP.NET core'da istek özellikleri
author: ardalis
description: HTTP isteklerini ve yanıtlarını arabirimlerde, ASP.NET Core için tanımlanan ilgili web sunucusu uygulaması ayrıntıları hakkında bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: fundamentals/request-features
ms.openlocfilehash: d0f3ae521d1f314dd04cb581d9a921da4719273d
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087027"
---
# <a name="request-features-in-aspnet-core"></a><span data-ttu-id="16d15-103">ASP.NET core'da istek özellikleri</span><span class="sxs-lookup"><span data-stu-id="16d15-103">Request Features in ASP.NET Core</span></span>

<span data-ttu-id="16d15-104">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="16d15-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="16d15-105">Web sunucusu uygulaması ayrıntıları HTTP istekleriyle ilgili ve yanıtları arabirimlerde tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="16d15-105">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="16d15-106">Bu arabirimler, uygulamanın barındırma işlem hattı oluşturup için sunucu uygulamaları ve ara yazılım tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="16d15-106">These interfaces are used by server implementations and middleware to create and modify the application's hosting pipeline.</span></span>

## <a name="feature-interfaces"></a><span data-ttu-id="16d15-107">Özelliği arabirimleri</span><span class="sxs-lookup"><span data-stu-id="16d15-107">Feature interfaces</span></span>

<span data-ttu-id="16d15-108">ASP.NET Core tanımlayan bir dizi HTTP özelliği arabirimlerde `Microsoft.AspNetCore.Http.Features` destekledikleri özellikleri tanımlamak için sunucuları tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="16d15-108">ASP.NET Core defines a number of HTTP feature interfaces in `Microsoft.AspNetCore.Http.Features` which are used by servers to identify the features they support.</span></span> <span data-ttu-id="16d15-109">Aşağıdaki özellik arabirimleri isteklerini işlemek ve yanıtları döndürür:</span><span class="sxs-lookup"><span data-stu-id="16d15-109">The following feature interfaces handle requests and return responses:</span></span>

<span data-ttu-id="16d15-110">`IHttpRequestFeature` Protokol, yol, sorgu dizesi, üstbilgi ve gövde içeren bir HTTP isteği yapısını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="16d15-110">`IHttpRequestFeature` Defines the structure of an HTTP request, including the protocol, path, query string, headers, and body.</span></span>

<span data-ttu-id="16d15-111">`IHttpResponseFeature` Bir HTTP yanıtının durum kodu, üst bilgiler ve yanıt gövdesinin gibi yapısını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="16d15-111">`IHttpResponseFeature` Defines the structure of an HTTP response, including the status code, headers, and body of the response.</span></span>

<span data-ttu-id="16d15-112">`IHttpAuthenticationFeature` Temel kullanıcı tanımlamaya yönelik desteği tanımlar bir `ClaimsPrincipal` belirterek bir kimlik doğrulama işleyicisi.</span><span class="sxs-lookup"><span data-stu-id="16d15-112">`IHttpAuthenticationFeature` Defines support for identifying users based on a `ClaimsPrincipal` and specifying an authentication handler.</span></span>

<span data-ttu-id="16d15-113">`IHttpUpgradeFeature` Desteğini tanımlar [HTTP yükseltmeleri](https://tools.ietf.org/html/rfc2616.html#section-14.42), istemci, ek protokoller belirtmek izin veren sunucu protokolleri geçmek istiyorsa kullanmak istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="16d15-113">`IHttpUpgradeFeature` Defines support for [HTTP Upgrades](https://tools.ietf.org/html/rfc2616.html#section-14.42), which allow the client to specify which additional protocols it would like to use if the server wishes to switch protocols.</span></span>

<span data-ttu-id="16d15-114">`IHttpBufferingFeature` İstek ve/veya yanıtlarını arabelleğe almayı devre dışı bırakmak için yöntemleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="16d15-114">`IHttpBufferingFeature` Defines methods for disabling buffering of requests and/or responses.</span></span>

<span data-ttu-id="16d15-115">`IHttpConnectionFeature` Yerel ve uzak adresler ve bağlantı noktaları için özellikleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="16d15-115">`IHttpConnectionFeature` Defines properties for local and remote addresses and ports.</span></span>

<span data-ttu-id="16d15-116">`IHttpRequestLifetimeFeature` Bağlantıları durduruluyor ya da bir isteği beklenenden önce gibi olarak bir istemci bağlantıyı kesme tarafından sonlanıp sonlanmadığını algılama desteği tanımlar.</span><span class="sxs-lookup"><span data-stu-id="16d15-116">`IHttpRequestLifetimeFeature` Defines support for aborting connections, or detecting if a request has been terminated prematurely, such as by a client disconnect.</span></span>

<span data-ttu-id="16d15-117">`IHttpSendFileFeature` Dosyaları zaman uyumsuz olarak gönderme yöntemi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="16d15-117">`IHttpSendFileFeature` Defines a method for sending files asynchronously.</span></span>

<span data-ttu-id="16d15-118">`IHttpWebSocketFeature` Web yuvaları desteklemek için bir API tanımlar.</span><span class="sxs-lookup"><span data-stu-id="16d15-118">`IHttpWebSocketFeature` Defines an API for supporting web sockets.</span></span>

<span data-ttu-id="16d15-119">`IHttpRequestIdentifierFeature` İstekleri benzersiz olarak tanımlanabilmesi için uygulanan bir özellik ekler.</span><span class="sxs-lookup"><span data-stu-id="16d15-119">`IHttpRequestIdentifierFeature` Adds a property that can be implemented to uniquely identify requests.</span></span>

<span data-ttu-id="16d15-120">`ISessionFeature` Tanımlar `ISessionFactory` ve `ISession` soyutlama kullanıcı oturumlarını destekleme.</span><span class="sxs-lookup"><span data-stu-id="16d15-120">`ISessionFeature` Defines `ISessionFactory` and `ISession` abstractions for supporting user sessions.</span></span>

<span data-ttu-id="16d15-121">`ITlsConnectionFeature` İstemci sertifikaları almak için bir API tanımlar.</span><span class="sxs-lookup"><span data-stu-id="16d15-121">`ITlsConnectionFeature` Defines an API for retrieving client certificates.</span></span>

<span data-ttu-id="16d15-122">`ITlsTokenBindingFeature` TLS belirteç bağlama parametreleri ile çalışmak için yöntemleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="16d15-122">`ITlsTokenBindingFeature` Defines methods for working with TLS token binding parameters.</span></span>

> [!NOTE]
> <span data-ttu-id="16d15-123">`ISessionFeature` Sunucu özelliği olmayan, ancak tarafından uygulanan `SessionMiddleware` (bkz [yönetme uygulama durumu](app-state.md)).</span><span class="sxs-lookup"><span data-stu-id="16d15-123">`ISessionFeature` isn't a server feature, but is implemented by the `SessionMiddleware` (see [Managing Application State](app-state.md)).</span></span>

## <a name="feature-collections"></a><span data-ttu-id="16d15-124">Özellik koleksiyonları</span><span class="sxs-lookup"><span data-stu-id="16d15-124">Feature collections</span></span>

<span data-ttu-id="16d15-125">`Features` Özelliği `HttpContext` alma ve ayarlama geçerli istek için kullanılabilir HTTP özellikleri için bir arabirim sağlar.</span><span class="sxs-lookup"><span data-stu-id="16d15-125">The `Features` property of `HttpContext` provides an interface for getting and setting the available HTTP features for the current request.</span></span> <span data-ttu-id="16d15-126">Özellik koleksiyonu bile bir istek bağlamı içinde değişebilir olduğundan, ara yazılım koleksiyonu değiştirmek ve ek özellikleri için destek eklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="16d15-126">Since the feature collection is mutable even within the context of a request, middleware can be used to modify the collection and add support for additional features.</span></span>

## <a name="middleware-and-request-features"></a><span data-ttu-id="16d15-127">Ara yazılım ve istek özellikleri</span><span class="sxs-lookup"><span data-stu-id="16d15-127">Middleware and request features</span></span>

<span data-ttu-id="16d15-128">Sunucuları özellik koleksiyonu oluşturmaktan sorumlu olsa da, ara yazılım bu koleksiyona eklemek hem koleksiyonunun özelliklerini kullanmak olabilir.</span><span class="sxs-lookup"><span data-stu-id="16d15-128">While servers are responsible for creating the feature collection, middleware can both add to this collection and consume features from the collection.</span></span> <span data-ttu-id="16d15-129">Örneğin, `StaticFileMiddleware` erişen `IHttpSendFileFeature` özelliği.</span><span class="sxs-lookup"><span data-stu-id="16d15-129">For example, the `StaticFileMiddleware` accesses the `IHttpSendFileFeature` feature.</span></span> <span data-ttu-id="16d15-130">Özellik zaten varsa, fiziksel yolundan istenen statik dosya göndermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="16d15-130">If the feature exists, it's used to send the requested static file from its physical path.</span></span> <span data-ttu-id="16d15-131">Aksi takdirde, daha yavaş bir alternatif yöntem dosyayı göndermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="16d15-131">Otherwise, a slower alternative method is used to send the file.</span></span> <span data-ttu-id="16d15-132">Kullanılabilir olduğunda, `IHttpSendFileFeature` dosyasını açın ve bir ağ kartına doğrudan çekirdek modu kopyalama işlemini gerçekleştirmek işletim sistemi sağlar.</span><span class="sxs-lookup"><span data-stu-id="16d15-132">When available, the `IHttpSendFileFeature` allows the operating system to open the file and perform a direct kernel mode copy to the network card.</span></span>

<span data-ttu-id="16d15-133">Ayrıca, Ara sunucu tarafından oluşturulan özellik koleksiyonu ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="16d15-133">Additionally, middleware can add to the feature collection established by the server.</span></span> <span data-ttu-id="16d15-134">Mevcut özellikler bile Ara sunucu işlevlerini genişletmek izin verme ara yazılımı tarafından değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="16d15-134">Existing features can even be replaced by middleware, allowing the middleware to augment the functionality of the server.</span></span> <span data-ttu-id="16d15-135">Koleksiyona eklenen özellikler, diğer ara yazılımdan veya temel uygulamada kendisini daha sonra istek ardışık düzenini için hemen kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="16d15-135">Features added to the collection are available immediately to other middleware or the underlying application itself later in the request pipeline.</span></span>

<span data-ttu-id="16d15-136">Özel sunucu uygulamaları ve belirli bir ara yazılım geliştirmeler birleştirerek özellikleri gerektiren bir uygulama hassas dizi oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="16d15-136">By combining custom server implementations and specific middleware enhancements, the precise set of features an application requires can be constructed.</span></span> <span data-ttu-id="16d15-137">Eksik sunucu değişikliğe gerek kalmadan eklenecek özellikleri ve yalnızca en az miktarda özellikler sunulur, böylece saldırı sınırlama sağlar böylece yüzey alanını ve performansını iyileştirme.</span><span class="sxs-lookup"><span data-stu-id="16d15-137">This allows missing features to be added without requiring a change in server, and ensures only the minimal amount of features are exposed, thus limiting attack surface area and improving performance.</span></span>

## <a name="summary"></a><span data-ttu-id="16d15-138">Özet</span><span class="sxs-lookup"><span data-stu-id="16d15-138">Summary</span></span>

<span data-ttu-id="16d15-139">Özellik arabirimler, belirtilen bir isteğin destekleyebilir belirli HTTP özellikleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="16d15-139">Feature interfaces define specific HTTP features that a given request may support.</span></span> <span data-ttu-id="16d15-140">Özellikleri koleksiyonları ve sunucu tarafından desteklenen özellikler ilk dizi sunucuları tanımlamak, ancak ara yazılım, bu özellikleri geliştirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="16d15-140">Servers define collections of features, and the initial set of features supported by that server, but middleware can be used to enhance these features.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="16d15-141">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="16d15-141">Additional resources</span></span>

* [<span data-ttu-id="16d15-142">Sunucular</span><span class="sxs-lookup"><span data-stu-id="16d15-142">Servers</span></span>](xref:fundamentals/servers/index)
* [<span data-ttu-id="16d15-143">Ara Yazılım</span><span class="sxs-lookup"><span data-stu-id="16d15-143">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="16d15-144">.NET için Açık Web Arabirimi (OWIN)</span><span class="sxs-lookup"><span data-stu-id="16d15-144">Open Web Interface for .NET (OWIN)</span></span>](xref:fundamentals/owin)
