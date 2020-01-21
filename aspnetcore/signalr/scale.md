---
title: ASP.NET Core SignalR üretim barındırma ve ölçeklendirme
author: bradygaster
description: ASP.NET Core SignalRkullanan uygulamalardaki performans ve ölçeklendirme sorunlarının nasıl önleneceğini öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 01/17/2020
no-loc:
- SignalR
uid: signalr/scale
ms.openlocfilehash: 2ffafd452af46b635f4ebbdf74561ad043158808
ms.sourcegitcommit: f259889044d1fc0f0c7e3882df0008157ced4915
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294729"
---
# <a name="aspnet-core-opno-locsignalr-hosting-and-scaling"></a><span data-ttu-id="9df96-103">ASP.NET Core SignalR barındırma ve ölçeklendirme</span><span class="sxs-lookup"><span data-stu-id="9df96-103">ASP.NET Core SignalR hosting and scaling</span></span>

<span data-ttu-id="9df96-104">, [Andrew Stanton-nuri](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster)ve [Tom Dykstra](https://github.com/tdykstra),</span><span class="sxs-lookup"><span data-stu-id="9df96-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), and [Tom Dykstra](https://github.com/tdykstra),</span></span>

<span data-ttu-id="9df96-105">Bu makalede, ASP.NET Core SignalRkullanan yüksek trafik uygulamalarına yönelik barındırma ve ölçeklendirme konuları açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="9df96-105">This article explains hosting and scaling considerations for high-traffic apps that use ASP.NET Core SignalR.</span></span>

## <a name="sticky-sessions"></a><span data-ttu-id="9df96-106">Yapışkan oturumlar</span><span class="sxs-lookup"><span data-stu-id="9df96-106">Sticky Sessions</span></span>

SignalR<span data-ttu-id="9df96-107">, belirli bir bağlantı için tüm HTTP isteklerinin aynı sunucu işlemi tarafından işlenmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="9df96-107"> requires that all HTTP requests for a specific connection be handled by the same server process.</span></span> <span data-ttu-id="9df96-108">Sunucu grubunda (birden çok sunucu) SignalR çalıştığında, "yapışkan oturumlar" kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9df96-108">When SignalR is running on a server farm (multiple servers), "sticky sessions" must be used.</span></span> <span data-ttu-id="9df96-109">"Yapışkan oturumlar", bazı yük dengeleyiciler tarafından oturum benzeşimi olarak da adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="9df96-109">"Sticky sessions" are also called session affinity by some load balancers.</span></span> <span data-ttu-id="9df96-110">Azure App Service istekleri yönlendirmek için [uygulama Isteği yönlendirme](https://docs.microsoft.com/iis/extensions/planning-for-arr/application-request-routing-version-2-overview) (ARR) kullanır.</span><span class="sxs-lookup"><span data-stu-id="9df96-110">Azure App Service uses [Application Request Routing](https://docs.microsoft.com/iis/extensions/planning-for-arr/application-request-routing-version-2-overview) (ARR) to route requests.</span></span> <span data-ttu-id="9df96-111">Azure App Service "ARR benzeşimi" ayarının etkinleştirilmesi "yapışkan oturumlar" sağlayacaktır.</span><span class="sxs-lookup"><span data-stu-id="9df96-111">Enabling the "ARR Affinity" setting in your Azure App Service will enable "sticky sessions".</span></span> <span data-ttu-id="9df96-112">Yapışkan oturumların gerekmediği tek koşullar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="9df96-112">The only circumstances in which sticky sessions are not required are:</span></span>

1. <span data-ttu-id="9df96-113">Tek bir sunucuda barındırırken, tek bir işlemde.</span><span class="sxs-lookup"><span data-stu-id="9df96-113">When hosting on a single server, in a single process.</span></span>
1. <span data-ttu-id="9df96-114">Azure SignalR hizmetini kullanırken.</span><span class="sxs-lookup"><span data-stu-id="9df96-114">When using the Azure SignalR Service.</span></span>
1. <span data-ttu-id="9df96-115">Tüm istemciler **yalnızca** WebSockets kullanacak şekilde yapılandırıldığında **ve** istemci yapılandırmasında [skipanlaşma ayarı](xref:signalr/configuration#configure-additional-options) etkinse.</span><span class="sxs-lookup"><span data-stu-id="9df96-115">When all clients are configured to **only** use WebSockets, **and** the [SkipNegotiation setting](xref:signalr/configuration#configure-additional-options) is enabled in the client configuration.</span></span>

<span data-ttu-id="9df96-116">Tüm diğer koşullarda (Redsıs geri düzlemi kullanıldığında dahil), sunucu ortamının yapışkan oturumlar için yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="9df96-116">In all other circumstances (including when the Redis backplane is used), the server environment must be configured for sticky sessions.</span></span>

<span data-ttu-id="9df96-117">SignalRiçin Azure App Service yapılandırma hakkında yönergeler için bkz. <xref:signalr/publish-to-azure-web-app>.</span><span class="sxs-lookup"><span data-stu-id="9df96-117">For guidance on configuring Azure App Service for SignalR, see <xref:signalr/publish-to-azure-web-app>.</span></span>

## <a name="tcp-connection-resources"></a><span data-ttu-id="9df96-118">TCP bağlantı kaynakları</span><span class="sxs-lookup"><span data-stu-id="9df96-118">TCP connection resources</span></span>

<span data-ttu-id="9df96-119">Bir Web sunucusunun destekleyebileceği eşzamanlı TCP bağlantısı sayısı sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="9df96-119">The number of concurrent TCP connections that a web server can support is limited.</span></span> <span data-ttu-id="9df96-120">Standart HTTP istemcileri, *kısa ömürlü* bağlantıları kullanır.</span><span class="sxs-lookup"><span data-stu-id="9df96-120">Standard HTTP clients use *ephemeral* connections.</span></span> <span data-ttu-id="9df96-121">Bu bağlantılar, istemci boşta kaldığında ve daha sonra yeniden açıldığı zaman kapatılabilir.</span><span class="sxs-lookup"><span data-stu-id="9df96-121">These connections can be closed when the client goes idle and reopened later.</span></span> <span data-ttu-id="9df96-122">Öte yandan, SignalR bir bağlantı *kalıcıdır*.</span><span class="sxs-lookup"><span data-stu-id="9df96-122">On the other hand, a SignalR connection is *persistent*.</span></span> SignalR<span data-ttu-id="9df96-123"> bağlantılar, istemci boşta kaldığında bile açık kalır.</span><span class="sxs-lookup"><span data-stu-id="9df96-123"> connections stay open even when the client goes idle.</span></span> <span data-ttu-id="9df96-124">Çok sayıda istemciye hizmet veren yüksek trafikli bir uygulamada, bu kalıcı bağlantılar sunucuların en fazla bağlantı sayısına ulaşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="9df96-124">In a high-traffic app that serves many clients, these persistent connections can cause servers to hit their maximum number of connections.</span></span>

<span data-ttu-id="9df96-125">Kalıcı bağlantılar, her bağlantıyı izlemek için ek bellek de tüketir.</span><span class="sxs-lookup"><span data-stu-id="9df96-125">Persistent connections also consume some additional memory, to track each connection.</span></span>

<span data-ttu-id="9df96-126">SignalR göre bağlantıyla ilgili kaynakların ağır kullanımı, aynı sunucuda barındırılan diğer Web uygulamalarını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="9df96-126">The heavy use of connection-related resources by SignalR can affect other web apps that are hosted on the same server.</span></span> <span data-ttu-id="9df96-127">SignalR açılıp en son kullanılabilir TCP bağlantılarını tutuyorsa, aynı sunucudaki diğer Web uygulamalarına da daha fazla bağlantı bulunmaz.</span><span class="sxs-lookup"><span data-stu-id="9df96-127">When SignalR opens and holds the last available TCP connections, other web apps on the same server also have no more connections available to them.</span></span>

<span data-ttu-id="9df96-128">Sunucuda bağlantı biterse rastgele yuva hataları ve bağlantı sıfırlama hataları görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="9df96-128">If a server runs out of connections, you'll see random socket errors and connection reset errors.</span></span> <span data-ttu-id="9df96-129">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="9df96-129">For example:</span></span>

```
An attempt was made to access a socket in a way forbidden by its access permissions...
```

<span data-ttu-id="9df96-130">SignalR kaynak kullanımını diğer Web uygulamalarındaki hatalara neden olacak şekilde korumak için, diğer Web uygulamalarınızdan farklı sunucularda SignalR çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9df96-130">To keep SignalR resource usage from causing errors in other web apps, run SignalR on different servers than your other web apps.</span></span>

<span data-ttu-id="9df96-131">SignalR kaynak kullanımını SignalR uygulamasında hatalara neden olacak şekilde korumak için, bir sunucunun işleyeceği bağlantı sayısını sınırlamak üzere ölçeği ölçeklendirin.</span><span class="sxs-lookup"><span data-stu-id="9df96-131">To keep SignalR resource usage from causing errors in a SignalR app, scale out to limit the number of connections a server has to handle.</span></span>

## <a name="scale-out"></a><span data-ttu-id="9df96-132">uygulamasını ölçeklendirme</span><span class="sxs-lookup"><span data-stu-id="9df96-132">Scale out</span></span>

<span data-ttu-id="9df96-133">SignalR kullanan bir uygulamanın, bir sunucu grubu için sorunlar oluşturan tüm bağlantılarını izlemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="9df96-133">An app that uses SignalR needs to keep track of all its connections, which creates problems for a server farm.</span></span> <span data-ttu-id="9df96-134">Bir sunucu ekleyin ve diğer sunucuların hakkında bilgi sahibi olmadığı yeni bağlantıları alır.</span><span class="sxs-lookup"><span data-stu-id="9df96-134">Add a server, and it gets new connections that the other servers don't know about.</span></span> <span data-ttu-id="9df96-135">Örneğin, aşağıdaki diyagramdaki her bir sunucuda SignalR diğer sunuculardaki bağlantılardan haberdar değildir.</span><span class="sxs-lookup"><span data-stu-id="9df96-135">For example, SignalR on each server in the following diagram is unaware of the connections on the other servers.</span></span> <span data-ttu-id="9df96-136">Sunuculardan birindeki SignalR tüm istemcilere ileti göndermek istediğinde, ileti yalnızca bu sunucuya bağlı olan istemcilere gider.</span><span class="sxs-lookup"><span data-stu-id="9df96-136">When SignalR on one of the servers wants to send a message to all clients, the message only goes to the clients connected to that server.</span></span>

![Ölçeklendirme [! Üs. NO-LOC (SignalR)] geri düzlemi olmadan](scale/_static/scale-no-backplane.png)

<span data-ttu-id="9df96-138">Bu sorunu çözmeye yönelik seçenekler [Azure SignalR hizmetidir](#azure-signalr-service) ve [redsıs arkadüzledir](#redis-backplane).</span><span class="sxs-lookup"><span data-stu-id="9df96-138">The options for solving this problem are the [Azure SignalR Service](#azure-signalr-service) and [Redis backplane](#redis-backplane).</span></span>

## <a name="azure-opno-locsignalr-service"></a><span data-ttu-id="9df96-139">Azure SignalR hizmeti</span><span class="sxs-lookup"><span data-stu-id="9df96-139">Azure SignalR Service</span></span>

<span data-ttu-id="9df96-140">Azure SignalR hizmeti, arka düzlemi yerine bir ara sunucu.</span><span class="sxs-lookup"><span data-stu-id="9df96-140">The Azure SignalR Service is a proxy rather than a backplane.</span></span> <span data-ttu-id="9df96-141">İstemci sunucuya bir bağlantı başlattığında, istemci hizmete bağlanmak için yeniden yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="9df96-141">Each time a client initiates a connection to the server, the client is redirected to connect to the service.</span></span> <span data-ttu-id="9df96-142">Bu işlem aşağıdaki diyagramda gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="9df96-142">That process is illustrated in the following diagram:</span></span>

![Azure 'a bağlantı kurma [! Üs. NO-LOC (SignalR)] hizmeti](scale/_static/azure-signalr-service-one-connection.png)

<span data-ttu-id="9df96-144">Sonuç olarak, hizmetin tüm istemci bağlantılarını yönetmesi, her sunucunun aşağıdaki diyagramda gösterildiği gibi yalnızca küçük bir sabit bağlantı sayısına ihtiyacı vardır:</span><span class="sxs-lookup"><span data-stu-id="9df96-144">The result is that the service manages all of the client connections, while each server needs only a small constant number of connections to the service, as shown in the following diagram:</span></span>

![Hizmete bağlı istemciler, hizmete bağlı sunucular](scale/_static/azure-signalr-service-multiple-connections.png)

<span data-ttu-id="9df96-146">Bu genişleme yaklaşımına yönelik bu yaklaşım, Redsıs geri düzlemi 'nin diğer avantajlarından biridir:</span><span class="sxs-lookup"><span data-stu-id="9df96-146">This approach to scale-out has several advantages over the Redis backplane alternative:</span></span>

* <span data-ttu-id="9df96-147">[İstemci benzeşimi](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity)olarak da bilinen yapışkan oturumlar gerekli değildir, çünkü istemciler, bağlandıklarında Azure SignalR hizmetine anında yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="9df96-147">Sticky sessions, also known as [client affinity](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity), is not required, because clients are immediately redirected to the Azure SignalR Service when they connect.</span></span>
* <span data-ttu-id="9df96-148">SignalR bir uygulama, gönderilen ileti sayısına göre ölçeklenebilir, ancak Azure SignalR hizmeti herhangi bir sayıda bağlantıyı işleyecek şekilde otomatik olarak ölçeklendirilir.</span><span class="sxs-lookup"><span data-stu-id="9df96-148">A SignalR app can scale out based on the number of messages sent, while the Azure SignalR Service automatically scales to handle any number of connections.</span></span> <span data-ttu-id="9df96-149">Örneğin, binlerce istemci olabilir, ancak saniye başına yalnızca birkaç ileti gönderiliyorsa, SignalR uygulamanın yalnızca bağlantıları işlemek için birden çok sunucuya ölçeklendirilmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="9df96-149">For example, there could be thousands of clients, but if only a few messages per second are sent, the SignalR app won't need to scale out to multiple servers just to handle the connections themselves.</span></span>
* <span data-ttu-id="9df96-150">SignalR bir uygulama, SignalRolmayan bir Web uygulamasından daha fazla bağlantı kaynağı kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="9df96-150">A SignalR app won't use significantly more connection resources than a web app without SignalR.</span></span>

<span data-ttu-id="9df96-151">Bu nedenlerden dolayı, App Service, VM 'Ler ve kapsayıcılar dahil olmak üzere Azure üzerinde barındırılan tüm ASP.NET Core SignalR uygulamaları için Azure SignalR hizmetini öneririz.</span><span class="sxs-lookup"><span data-stu-id="9df96-151">For these reasons, we recommend the Azure SignalR Service for all ASP.NET Core SignalR apps hosted on Azure, including App Service, VMs, and containers.</span></span>

<span data-ttu-id="9df96-152">Daha fazla bilgi için bkz. [Azure SignalR hizmeti belgeleri](/azure/azure-signalr/signalr-overview).</span><span class="sxs-lookup"><span data-stu-id="9df96-152">For more information see the [Azure SignalR Service documentation](/azure/azure-signalr/signalr-overview).</span></span>

## <a name="redis-backplane"></a><span data-ttu-id="9df96-153">Redis kartı</span><span class="sxs-lookup"><span data-stu-id="9df96-153">Redis backplane</span></span>

<span data-ttu-id="9df96-154">[Redsıs](https://redis.io/) , bir yayımlama/abonelik modeliyle bir mesajlaşma sistemini destekleyen bellek içi anahtar-değer deposudur.</span><span class="sxs-lookup"><span data-stu-id="9df96-154">[Redis](https://redis.io/) is an in-memory key-value store that supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="9df96-155">SignalR redin backdüzlemi, iletileri diğer sunuculara iletmek için pub/Sub özelliğini kullanır.</span><span class="sxs-lookup"><span data-stu-id="9df96-155">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span> <span data-ttu-id="9df96-156">İstemci bir bağlantı yaptığında, bağlantı bilgileri geri düzleme geçirilir.</span><span class="sxs-lookup"><span data-stu-id="9df96-156">When a client makes a connection, the connection information is passed to the backplane.</span></span> <span data-ttu-id="9df96-157">Bir sunucu tüm istemcilere ileti göndermek istediğinde, bu geri düzlemi gönderir.</span><span class="sxs-lookup"><span data-stu-id="9df96-157">When a server wants to send a message to all clients, it sends to the backplane.</span></span> <span data-ttu-id="9df96-158">Biriktirme listesi, tüm bağlı istemcileri ve bunların hangi sunuculara olduğunu bilir.</span><span class="sxs-lookup"><span data-stu-id="9df96-158">The backplane knows all connected clients and which servers they're on.</span></span> <span data-ttu-id="9df96-159">İletiyi ilgili sunucuları aracılığıyla tüm istemcilere gönderir.</span><span class="sxs-lookup"><span data-stu-id="9df96-159">It sends the message to all clients via their respective servers.</span></span> <span data-ttu-id="9df96-160">Bu işlem aşağıdaki diyagramda gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="9df96-160">This process is illustrated in the following diagram:</span></span>

![Redsıs geri düzlemi, bir sunucudan tüm istemcilere gönderilen ileti](scale/_static/redis-backplane.png)

<span data-ttu-id="9df96-162">Redsıs geri düzlemi, kendi altyapınızda barındırılan uygulamalar için önerilen genişleme yaklaşımıdır.</span><span class="sxs-lookup"><span data-stu-id="9df96-162">The Redis backplane is the recommended scale-out approach for apps hosted on your own infrastructure.</span></span> <span data-ttu-id="9df96-163">Veri merkezinizde bir Azure veri merkezi arasında önemli bir bağlantı gecikmesi varsa, Azure SignalR hizmeti düşük gecikme süresi veya yüksek performans gereksinimlerine sahip şirket içi uygulamalar için pratik bir seçenek olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="9df96-163">If there is significant connection latency between your data center and an Azure data center, Azure SignalR Service may not be a practical option for on-premises apps with low latency or high throughput requirements.</span></span>

<span data-ttu-id="9df96-164">Daha önce bahsedilen Azure SignalR hizmet avantajları Redsıs geri düzlemi için dezavantajlardır:</span><span class="sxs-lookup"><span data-stu-id="9df96-164">The Azure SignalR Service advantages noted earlier are disadvantages for the Redis backplane:</span></span>

* <span data-ttu-id="9df96-165">[İstemci benzeşimi](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity)olarak da bilinen yapışkan oturumlar, aşağıdakilerin **her ikisi** de doğru olduğunda gereklidir:</span><span class="sxs-lookup"><span data-stu-id="9df96-165">Sticky sessions, also known as [client affinity](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity), is required, except when **both** of the following are true:</span></span>
  * <span data-ttu-id="9df96-166">Tüm istemciler **yalnızca** WebSockets kullanacak şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="9df96-166">All clients are configured to **only** use WebSockets.</span></span>
  * <span data-ttu-id="9df96-167">[Skipanlaşma ayarı](xref:signalr/configuration#configure-additional-options) istemci yapılandırmasında etkindir.</span><span class="sxs-lookup"><span data-stu-id="9df96-167">The [SkipNegotiation setting](xref:signalr/configuration#configure-additional-options) is enabled in the client configuration.</span></span> 
   <span data-ttu-id="9df96-168">Sunucuda bir bağlantı başlatıldıktan sonra bağlantı o sunucuda kalmaya devam etmek zorunda kalır.</span><span class="sxs-lookup"><span data-stu-id="9df96-168">Once a connection is initiated on a server, the connection has to stay on that server.</span></span>
* <span data-ttu-id="9df96-169">Birkaç ileti gönderilse bile SignalR bir uygulama, istemci sayısına göre ölçeklendirmelidir.</span><span class="sxs-lookup"><span data-stu-id="9df96-169">A SignalR app must scale out based on number of clients even if few messages are being sent.</span></span>
* <span data-ttu-id="9df96-170">SignalR uygulaması, SignalRolmayan bir Web uygulamasından çok daha fazla bağlantı kaynağı kullanır.</span><span class="sxs-lookup"><span data-stu-id="9df96-170">A SignalR app uses significantly more connection resources than a web app without SignalR.</span></span>

## <a name="iis-limitations-on-windows-client-os"></a><span data-ttu-id="9df96-171">Windows istemci işletim sisteminde IIS sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="9df96-171">IIS limitations on Windows client OS</span></span>

<span data-ttu-id="9df96-172">Windows 10 ve Windows 8. x istemci işletim sistemleridir.</span><span class="sxs-lookup"><span data-stu-id="9df96-172">Windows 10 and Windows 8.x are client operating systems.</span></span> <span data-ttu-id="9df96-173">İstemci işletim sistemlerindeki IIS, 10 eşzamanlı bağlantı sınırına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="9df96-173">IIS on client operating systems has a limit of 10 concurrent connections.</span></span> SignalR<span data-ttu-id="9df96-174">bağlantıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="9df96-174">'s connections are:</span></span>

* <span data-ttu-id="9df96-175">Geçici ve sık yeniden oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="9df96-175">Transient and frequently re-established.</span></span>
* <span data-ttu-id="9df96-176">Artık **kullanılmadıysa** hemen atılamaz.</span><span class="sxs-lookup"><span data-stu-id="9df96-176">**Not** disposed immediately when no longer used.</span></span>

<span data-ttu-id="9df96-177">Yukarıdaki koşullar, istemci işletim sistemi üzerindeki 10 bağlantı sınırına ulaşmaları olasılığını ortadan alır.</span><span class="sxs-lookup"><span data-stu-id="9df96-177">The preceding conditions make it likely to hit the 10 connection limit on a client OS.</span></span> <span data-ttu-id="9df96-178">Geliştirme için bir istemci işletim sistemi kullanıldığında şunları yapmanızı öneririz:</span><span class="sxs-lookup"><span data-stu-id="9df96-178">When a client OS is used for development, we recommend:</span></span>

* <span data-ttu-id="9df96-179">IIS kullanmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="9df96-179">Avoid IIS.</span></span>
* <span data-ttu-id="9df96-180">Dağıtım hedefleri olarak Kestrel veya IIS Express kullanın.</span><span class="sxs-lookup"><span data-stu-id="9df96-180">Use Kestrel or IIS Express as deployment targets.</span></span>

## <a name="linux-with-nginx"></a><span data-ttu-id="9df96-181">Nginx ile Linux</span><span class="sxs-lookup"><span data-stu-id="9df96-181">Linux with Nginx</span></span>

<span data-ttu-id="9df96-182">Proxy 'nin `Connection` ve `Upgrade` üst bilgilerini SignalR WebSockets için aşağıdaki şekilde ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="9df96-182">Set the proxy's `Connection` and `Upgrade` headers to the following for SignalR WebSockets:</span></span>

```
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection $connection_upgrade;
```

<span data-ttu-id="9df96-183">Daha fazla bilgi için bkz. [WebSocket proxy 'si olarak NGINX](https://www.nginx.com/blog/websocket-nginx/).</span><span class="sxs-lookup"><span data-stu-id="9df96-183">For more information, see [NGINX as a WebSocket Proxy](https://www.nginx.com/blog/websocket-nginx/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9df96-184">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="9df96-184">Next steps</span></span>

<span data-ttu-id="9df96-185">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="9df96-185">For more information, see the following resources:</span></span>

* <span data-ttu-id="9df96-186">[Azure SignalR hizmeti belgeleri](/azure/azure-signalr/signalr-overview)</span><span class="sxs-lookup"><span data-stu-id="9df96-186">[Azure SignalR Service documentation](/azure/azure-signalr/signalr-overview)</span></span>
* [<span data-ttu-id="9df96-187">Redsıs geri düzlemi ayarlama</span><span class="sxs-lookup"><span data-stu-id="9df96-187">Set up a Redis backplane</span></span>](xref:signalr/redis-backplane)
