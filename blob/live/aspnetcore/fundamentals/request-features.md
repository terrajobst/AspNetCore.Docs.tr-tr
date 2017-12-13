---
title: "ASP.NET Core istek özellikleri"
author: ardalis
description: "HTTP istekleri ve yanıtları arabirimlerde ASP.NET Core için tanımlanan ilgili web sunucusu uygulama ayrıntıları hakkında bilgi edinin."
keywords: "ASP.NET Çekirdeği"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d1fbd23c-2ff9-4216-b908-0201ff3afb7c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/request-features
ms.openlocfilehash: b689d82d16c6ef55485691b3474a070765c8144b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="request-features-in-aspnet-core"></a><span data-ttu-id="33f51-104">ASP.NET Core istek özellikleri</span><span class="sxs-lookup"><span data-stu-id="33f51-104">Request Features in ASP.NET Core</span></span>

<span data-ttu-id="33f51-105">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="33f51-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="33f51-106">Web sunucusu uygulama ayrıntılarını ilgili HTTP istekleri ve yanıtları arabirimlerde tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="33f51-106">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="33f51-107">Bu arabirimleri oluşturma ve uygulama barındırma ardışık düzen değiştirme sunucu uygulamaları ve ara yazılım tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="33f51-107">These interfaces are used by server implementations and middleware to create and modify the application's hosting pipeline.</span></span>

## <a name="feature-interfaces"></a><span data-ttu-id="33f51-108">Özelliği arabirimleri</span><span class="sxs-lookup"><span data-stu-id="33f51-108">Feature interfaces</span></span>

<span data-ttu-id="33f51-109">ASP.NET Core HTTP özelliği arabirimlerde sayısını tanımlar `Microsoft.AspNetCore.Http.Features` hangi sunucuları tarafından destekledikleri özellikleri tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="33f51-109">ASP.NET Core defines a number of HTTP feature interfaces in `Microsoft.AspNetCore.Http.Features` which are used by servers to identify the features they support.</span></span> <span data-ttu-id="33f51-110">Aşağıdaki özellik arabirimleri isteklerini işler ve yanıtları döndürür:</span><span class="sxs-lookup"><span data-stu-id="33f51-110">The following feature interfaces handle requests and return responses:</span></span>

<span data-ttu-id="33f51-111">`IHttpRequestFeature`Protokol, yol, sorgu dizesi, üstbilgiler ve gövde dahil olmak üzere bir HTTP isteği yapısını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="33f51-111">`IHttpRequestFeature` Defines the structure of an HTTP request, including the protocol, path, query string, headers, and body.</span></span>

<span data-ttu-id="33f51-112">`IHttpResponseFeature`Durum kodu, üstbilgiler ve yanıtın gövdesini içeren bir HTTP yanıtının yapısını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="33f51-112">`IHttpResponseFeature` Defines the structure of an HTTP response, including the status code, headers, and body of the response.</span></span>

<span data-ttu-id="33f51-113">`IHttpAuthenticationFeature`Temel alarak kullanıcılara tanımlamak için destek tanımlayan bir `ClaimsPrincipal` ve bir kimlik doğrulama işleyicisi belirtme.</span><span class="sxs-lookup"><span data-stu-id="33f51-113">`IHttpAuthenticationFeature` Defines support for identifying users based on a `ClaimsPrincipal` and specifying an authentication handler.</span></span>

<span data-ttu-id="33f51-114">`IHttpUpgradeFeature`Desteğini tanımlar [HTTP yükseltmeler](https://tools.ietf.org/html/rfc2616.html#section-14.42), ek, iletişim kurallarını belirtmek istemci olanak sunucu protokolleri geçiş isterse kullanmak istersiniz.</span><span class="sxs-lookup"><span data-stu-id="33f51-114">`IHttpUpgradeFeature` Defines support for [HTTP Upgrades](https://tools.ietf.org/html/rfc2616.html#section-14.42), which allow the client to specify which additional protocols it would like to use if the server wishes to switch protocols.</span></span>

<span data-ttu-id="33f51-115">`IHttpBufferingFeature`Arabelleğe alma isteklerini ve/veya yanıtlarını devre dışı bırakmak için yöntemleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="33f51-115">`IHttpBufferingFeature` Defines methods for disabling buffering of requests and/or responses.</span></span>

<span data-ttu-id="33f51-116">`IHttpConnectionFeature`Yerel ve uzak adresleri ve bağlantı noktaları özelliklerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="33f51-116">`IHttpConnectionFeature` Defines properties for local and remote addresses and ports.</span></span>

<span data-ttu-id="33f51-117">`IHttpRequestLifetimeFeature`Bağlantıları durduruluyor ya da bir istek erken, böyle bir istemcinin bağlantısının kesildiği gibi tarafından sonlanıp sonlanmadığını algılama desteği tanımlar.</span><span class="sxs-lookup"><span data-stu-id="33f51-117">`IHttpRequestLifetimeFeature` Defines support for aborting connections, or detecting if a request has been terminated prematurely, such as by a client disconnect.</span></span>

<span data-ttu-id="33f51-118">`IHttpSendFileFeature`Dosyaları zaman uyumsuz olarak göndermek için bir yöntem tanımlar.</span><span class="sxs-lookup"><span data-stu-id="33f51-118">`IHttpSendFileFeature` Defines a method for sending files asynchronously.</span></span>

<span data-ttu-id="33f51-119">`IHttpWebSocketFeature`Bir API web yuvalarını desteklemek için tanımlar.</span><span class="sxs-lookup"><span data-stu-id="33f51-119">`IHttpWebSocketFeature` Defines an API for supporting web sockets.</span></span>

<span data-ttu-id="33f51-120">`IHttpRequestIdentifierFeature`İstekleri benzersiz şekilde tanımlamak için uygulanan bir özellik ekler.</span><span class="sxs-lookup"><span data-stu-id="33f51-120">`IHttpRequestIdentifierFeature` Adds a property that can be implemented to uniquely identify requests.</span></span>

<span data-ttu-id="33f51-121">`ISessionFeature`Tanımlar `ISessionFactory` ve `ISession` kullanıcı oturumlarını desteklemek için soyutlamalar.</span><span class="sxs-lookup"><span data-stu-id="33f51-121">`ISessionFeature` Defines `ISessionFactory` and `ISession` abstractions for supporting user sessions.</span></span>

<span data-ttu-id="33f51-122">`ITlsConnectionFeature`İstemci sertifikaları almak için bir API tanımlar.</span><span class="sxs-lookup"><span data-stu-id="33f51-122">`ITlsConnectionFeature` Defines an API for retrieving client certificates.</span></span>

<span data-ttu-id="33f51-123">`ITlsTokenBindingFeature`TLS belirteci bağlama parametreleri ile çalışmak için yöntemleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="33f51-123">`ITlsTokenBindingFeature` Defines methods for working with TLS token binding parameters.</span></span>

> [!NOTE]
> <span data-ttu-id="33f51-124">`ISessionFeature`Sunucu özelliği değil, ancak tarafından uygulanan `SessionMiddleware` (bkz [yönetme uygulama durumu](app-state.md)).</span><span class="sxs-lookup"><span data-stu-id="33f51-124">`ISessionFeature` is not a server feature, but is implemented by the `SessionMiddleware` (see [Managing Application State](app-state.md)).</span></span>

## <a name="feature-collections"></a><span data-ttu-id="33f51-125">Özellik koleksiyonları</span><span class="sxs-lookup"><span data-stu-id="33f51-125">Feature collections</span></span>

<span data-ttu-id="33f51-126">`Features` Özelliği `HttpContext` geçerli istek için kullanılabilir HTTP özellikleri ayarlama ve alma için bir arabirim sağlar.</span><span class="sxs-lookup"><span data-stu-id="33f51-126">The `Features` property of `HttpContext` provides an interface for getting and setting the available HTTP features for the current request.</span></span> <span data-ttu-id="33f51-127">Özellik koleksiyonu bile bir istek bağlamı içinde değişebilir olduğundan, ara yazılım koleksiyonu değiştirmek ve ek özellikler için destek eklemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="33f51-127">Since the feature collection is mutable even within the context of a request, middleware can be used to modify the collection and add support for additional features.</span></span>

## <a name="middleware-and-request-features"></a><span data-ttu-id="33f51-128">Ara yazılım ve istek özellikleri</span><span class="sxs-lookup"><span data-stu-id="33f51-128">Middleware and request features</span></span>

<span data-ttu-id="33f51-129">Sunucuları özellik koleksiyonu oluşturmaktan sorumlu olsa da, ara yazılımı bu koleksiyona eklemek hem koleksiyondan özellikleri kullanmak olabilir.</span><span class="sxs-lookup"><span data-stu-id="33f51-129">While servers are responsible for creating the feature collection, middleware can both add to this collection and consume features from the collection.</span></span> <span data-ttu-id="33f51-130">Örneğin, `StaticFileMiddleware` erişen `IHttpSendFileFeature` özelliği.</span><span class="sxs-lookup"><span data-stu-id="33f51-130">For example, the `StaticFileMiddleware` accesses the `IHttpSendFileFeature` feature.</span></span> <span data-ttu-id="33f51-131">Özellik varsa, istenen statik dosyanın fiziksel yoldan göndermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="33f51-131">If the feature exists, it is used to send the requested static file from its physical path.</span></span> <span data-ttu-id="33f51-132">Aksi takdirde, daha yavaş alternatif bir yöntemi, dosya göndermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="33f51-132">Otherwise, a slower alternative method is used to send the file.</span></span> <span data-ttu-id="33f51-133">Kullanılabilir olduğunda `IHttpSendFileFeature` dosyasını açın ve bir ağ kartı doğrudan çekirdek modu kopyaya gerçekleştirmek işletim sistemi sağlar.</span><span class="sxs-lookup"><span data-stu-id="33f51-133">When available, the `IHttpSendFileFeature` allows the operating system to open the file and perform a direct kernel mode copy to the network card.</span></span>

<span data-ttu-id="33f51-134">Ayrıca, Ara sunucu tarafından oluşturulmuş özellik koleksiyonu ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33f51-134">Additionally, middleware can add to the feature collection established by the server.</span></span> <span data-ttu-id="33f51-135">Var olan özellikleri bile sunucunun işlevselliğini genişletmek ara yazılım izin vererek ara yazılımı tarafından değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="33f51-135">Existing features can even be replaced by middleware, allowing the middleware to augment the functionality of the server.</span></span> <span data-ttu-id="33f51-136">Koleksiyona eklenen özellikler, diğer ara yazılımdan veya temel uygulamada kendisini daha sonra isteği ardışık düzeni için hemen kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="33f51-136">Features added to the collection are available immediately to other middleware or the underlying application itself later in the request pipeline.</span></span>

<span data-ttu-id="33f51-137">Özel sunucu uygulamaları ve belirli Ara geliştirmeler birleştirerek, bir uygulama gerektiren özellikler kesin kümesi oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="33f51-137">By combining custom server implementations and specific middleware enhancements, the precise set of features an application requires can be constructed.</span></span> <span data-ttu-id="33f51-138">Eksik Server'daki bir değişiklik gerektirmeden eklenecek özellikleri ve özellikleri yalnızca en az miktarda açığa, böylece saldırı sınırlaması sağlar böylece yüzey alanını ve performans artırılır.</span><span class="sxs-lookup"><span data-stu-id="33f51-138">This allows missing features to be added without requiring a change in server, and ensures only the minimal amount of features are exposed, thus limiting attack surface area and improving performance.</span></span>

## <a name="summary"></a><span data-ttu-id="33f51-139">Özet</span><span class="sxs-lookup"><span data-stu-id="33f51-139">Summary</span></span>

<span data-ttu-id="33f51-140">Özellik arabirimler belirtilen bir isteğin destekleyebilir belirli HTTP özellikleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="33f51-140">Feature interfaces define specific HTTP features that a given request may support.</span></span> <span data-ttu-id="33f51-141">Koleksiyonlar özelliklerinin ve bu sunucu tarafından desteklenen özellikler başlangıç kümesi sunucuları tanımlamak, ancak ara yazılım, bu özellikleri geliştirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="33f51-141">Servers define collections of features, and the initial set of features supported by that server, but middleware can be used to enhance these features.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="33f51-142">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="33f51-142">Additional Resources</span></span>

* [<span data-ttu-id="33f51-143">Sunucular</span><span class="sxs-lookup"><span data-stu-id="33f51-143">Servers</span></span>](servers/index.md)

* [<span data-ttu-id="33f51-144">Ara yazılım</span><span class="sxs-lookup"><span data-stu-id="33f51-144">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="33f51-145">.NET (OWIN) için açık Web arabirimi</span><span class="sxs-lookup"><span data-stu-id="33f51-145">Open Web Interface for .NET (OWIN)</span></span>](owin.md)
