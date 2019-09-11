---
title: ASP.NET Core Blazor sunucu-tarafı barındırma ve dağıtma
author: guardrex
description: ASP.NET Core kullanarak Blazor sunucu tarafı uygulamasını nasıl barındırılacağını ve dağıtacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/07/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: fc47dfa1344b74ec7110211e3698217e246ab86d
ms.sourcegitcommit: e7c56e8da5419bbc20b437c2dd531dedf9b0dc6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878491"
---
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="b6f0c-103">Blazor sunucu tarafı barındırma ve dağıtma</span><span class="sxs-lookup"><span data-stu-id="b6f0c-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="b6f0c-104">, [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)ve [Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="b6f0c-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="b6f0c-105">Ana bilgisayar yapılandırma değerleri</span><span class="sxs-lookup"><span data-stu-id="b6f0c-105">Host configuration values</span></span>

<span data-ttu-id="b6f0c-106">[Sunucu tarafı barındırma modelini](xref:blazor/hosting-models#server-side) kullanan sunucu tarafı uygulamalar [genel ana bilgisayar yapılandırma değerlerini](xref:fundamentals/host/generic-host#host-configuration)kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="b6f0c-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="b6f0c-107">Dağıtım</span><span class="sxs-lookup"><span data-stu-id="b6f0c-107">Deployment</span></span>

<span data-ttu-id="b6f0c-108">[Sunucu tarafı barındırma modeliyle](xref:blazor/hosting-models#server-side), Blazor bir ASP.NET Core uygulamasının içinden sunucuda yürütülür.</span><span class="sxs-lookup"><span data-stu-id="b6f0c-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="b6f0c-109">Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrıları bir [SignalR](xref:signalr/introduction) bağlantısı üzerinden işlenir.</span><span class="sxs-lookup"><span data-stu-id="b6f0c-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="b6f0c-110">ASP.NET Core uygulaması barındırabilen bir Web sunucusu gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="b6f0c-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="b6f0c-111">Visual Studio, **Blazor Server uygulaması** proje şablonunu (`blazorserverside` [DotNet New](/dotnet/core/tools/dotnet-new) komutu kullanılırken şablon) içerir.</span><span class="sxs-lookup"><span data-stu-id="b6f0c-111">Visual Studio includes the **Blazor Server App** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

## <a name="scalability"></a><span data-ttu-id="b6f0c-112">Ölçeklenebilirlik</span><span class="sxs-lookup"><span data-stu-id="b6f0c-112">Scalability</span></span>

<span data-ttu-id="b6f0c-113">Bir Blazor sunucusu uygulaması için kullanılabilir altyapıyı en iyi şekilde kullanmasını sağlamak üzere bir dağıtım planlayın.</span><span class="sxs-lookup"><span data-stu-id="b6f0c-113">Plan a deployment to make the best use of the available infrastructure for a Blazor Server app.</span></span> <span data-ttu-id="b6f0c-114">Blazor Server uygulama ölçeklenebilirliğini ele almak için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="b6f0c-114">See the following resources to address Blazor Server app scalability:</span></span>

* [<span data-ttu-id="b6f0c-115">Blazor Server uygulamalarının temelleri</span><span class="sxs-lookup"><span data-stu-id="b6f0c-115">Fundamentals of Blazor Server apps</span></span>](xref:blazor/hosting-models#server-side)
* <xref:security/blazor/server-side>

### <a name="deployment-server"></a><span data-ttu-id="b6f0c-116">Dağıtım sunucusu</span><span class="sxs-lookup"><span data-stu-id="b6f0c-116">Deployment server</span></span>

<span data-ttu-id="b6f0c-117">Tek bir sunucunun ölçeklenebilirliğini değerlendirirken (ölçeği büyütme), bir uygulama için kullanılabilir olan bellek büyük olasılıkla uygulamanın kullanıcı talebi arttıkça arttırabileceği ilk kaynaktır.</span><span class="sxs-lookup"><span data-stu-id="b6f0c-117">When considering the scalability of a single server (scale up), the memory available to an app is likely the first resource that the app will exhaust as user demands increase.</span></span> <span data-ttu-id="b6f0c-118">Sunucudaki kullanılabilir bellek şunları etkiler:</span><span class="sxs-lookup"><span data-stu-id="b6f0c-118">The available memory on the server affects the:</span></span>

* <span data-ttu-id="b6f0c-119">Bir sunucunun destekleyebileceği etkin devre sayısı.</span><span class="sxs-lookup"><span data-stu-id="b6f0c-119">Number of active circuits that a server can support.</span></span>
* <span data-ttu-id="b6f0c-120">İstemcide UI gecikme süresi.</span><span class="sxs-lookup"><span data-stu-id="b6f0c-120">UI latency on the client.</span></span>

<span data-ttu-id="b6f0c-121">Güvenli ve ölçeklenebilir Blazor Server uygulamaları oluşturma hakkında yönergeler için bkz <xref:security/blazor/server-side>.</span><span class="sxs-lookup"><span data-stu-id="b6f0c-121">For guidance on building secure and scalable Blazor server apps, see <xref:security/blazor/server-side>.</span></span>

<span data-ttu-id="b6f0c-122">Her bağlantı hattı en düşük *Merhaba Dünya*stilinde bir uygulama için YAKLAŞıK 250 KB bellek kullanır.</span><span class="sxs-lookup"><span data-stu-id="b6f0c-122">Each circuit uses approximately 250 KB of memory for a minimal *Hello World*-style app.</span></span> <span data-ttu-id="b6f0c-123">Bir devrenin boyutu, uygulamanın koduna ve her bileşenle ilişkili durum bakım gereksinimlerine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="b6f0c-123">The size of a circuit depends on the app's code and the state maintenance requirements associated with each component.</span></span> <span data-ttu-id="b6f0c-124">Uygulamanız ve altyapınız için geliştirme sırasında kaynak taleplerini ölçmenizi öneririz, ancak aşağıdaki taban çizgisi dağıtım hedefini planlarken bir başlangıç noktası olabilir: Uygulamanızı 5.000 eşzamanlı kullanıcı desteğine karşı bekleliyorsanız, en az 1,3 GB sunucu belleğini uygulamaya (veya Kullanıcı başına ~ 273 KB) göre bütçeleme yapmayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="b6f0c-124">We recommend that you measure resource demands during development for your app and infrastructure, but the following baseline can be a starting point in planning your deployment target: If you expect your app to support 5,000 concurrent users, consider budgeting at least 1.3 GB of server memory to the app (or ~273 KB per user).</span></span>

### <a name="signalr-configuration"></a><span data-ttu-id="b6f0c-125">SignalR yapılandırması</span><span class="sxs-lookup"><span data-stu-id="b6f0c-125">SignalR configuration</span></span>

<span data-ttu-id="b6f0c-126">Blazor Server Apps, tarayıcıyla iletişim kurmak için ASP.NET Core SignalR kullanır.</span><span class="sxs-lookup"><span data-stu-id="b6f0c-126">Blazor Server apps use ASP.NET Core SignalR to communicate with the browser.</span></span> <span data-ttu-id="b6f0c-127">[SignalR 'nin barındırma ve ölçeklendirme koşulları](xref:signalr/publish-to-azure-web-app) Blazor Server uygulamaları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="b6f0c-127">[SignalR's hosting and scaling conditions](xref:signalr/publish-to-azure-web-app) apply to Blazor Server apps.</span></span>

<span data-ttu-id="b6f0c-128">Blazor, düşük gecikme süresi, güvenilirlik ve [güvenlik](xref:signalr/security)nedeniyle SignalR taşıması olarak WebSockets kullanırken en iyi şekilde geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="b6f0c-128">Blazor works best when using WebSockets as the SignalR transport due to lower latency, reliability, and [security](xref:signalr/security).</span></span> <span data-ttu-id="b6f0c-129">Uzun yoklama, WebSockets kullanılamadığında veya uygulama açıkça uzun yoklamayı kullanacak şekilde yapılandırıldığında, SignalR tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b6f0c-129">Long Polling is used by SignalR when WebSockets isn't available or when the app is explicitly configured to use Long Polling.</span></span> <span data-ttu-id="b6f0c-130">Azure App Service dağıtım sırasında, uygulamayı hizmetin Azure portal ayarları içinde kullanmak üzere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="b6f0c-130">When deploying to Azure App Service, configure the app to use WebSockets in the Azure portal settings for the service.</span></span> <span data-ttu-id="b6f0c-131">Azure App Service için uygulamayı yapılandırma hakkında ayrıntılı bilgi için bkz. [SignalR yayımlama yönergeleri](xref:signalr/publish-to-azure-web-app).</span><span class="sxs-lookup"><span data-stu-id="b6f0c-131">For details on configuring the app for Azure App Service, see the [SignalR publishing guidelines](xref:signalr/publish-to-azure-web-app).</span></span>

<span data-ttu-id="b6f0c-132">Blazor Server uygulamaları için [Azure SignalR hizmetini](/azure/azure-signalr) kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="b6f0c-132">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="b6f0c-133">Hizmet, Blazor sunucu uygulamasını çok sayıda eşzamanlı SignalR bağlantısına ölçeklendirmeye olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="b6f0c-133">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="b6f0c-134">Ayrıca, SignalR hizmetinin küresel erişim ve yüksek performanslı veri merkezleri Coğrafya nedeniyle gecikme süresini azaltmaya önemli ölçüde yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="b6f0c-134">In addition, the SignalR service's global reach and high-performance data centers significantly aid in reducing latency due to geography.</span></span>

### <a name="measure-network-latency"></a><span data-ttu-id="b6f0c-135">Ölçü ağı gecikmesi</span><span class="sxs-lookup"><span data-stu-id="b6f0c-135">Measure network latency</span></span>

<span data-ttu-id="b6f0c-136">Aşağıdaki örnekte gösterildiği gibi, [js birlikte çalışması](xref:blazor/javascript-interop) ağ gecikmesini ölçmek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="b6f0c-136">[JS interop](xref:blazor/javascript-interop) can be used to measure network latency, as the following example demonstrates:</span></span>

```cshtml
@inject IJSRuntime JS

@if (latency is null)
{
    <span>Calculating...</span>
}
else
{
    <span>@(latency.Value.TotalMilliseconds)ms</span>
}

@code
{
    private DateTime startTime;
    private TimeSpan? latency;

    protected override async Task OnInitializedAsync()
    {
        startTime = DateTime.UtcNow;
        var _ = await JS.InvokeAsync<string>("toString");
        latency = DateTime.UtcNow - startTime;
    }
}
```

<span data-ttu-id="b6f0c-137">Makul bir kullanıcı arabirimi deneyimi için, 250ms veya daha az sayıda sürekli Kullanıcı arabirimi gecikme süresi önerilir.</span><span class="sxs-lookup"><span data-stu-id="b6f0c-137">For a reasonable UI experience, we recommend a sustained UI latency of 250ms or less.</span></span>
