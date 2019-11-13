---
title: ASP.NET Core Blazor sunucusu barındırma ve dağıtma
author: guardrex
description: ASP.NET Core kullanarak Blazor sunucu uygulamasının nasıl barındırılacağını ve dağıtılacağını öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/server
ms.openlocfilehash: ad83c57f55c75d4a49656fa5d7046c32b0f049ff
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963571"
---
# <a name="host-and-deploy-opno-locblazor-server"></a><span data-ttu-id="0aa78-103">Blazor sunucusu barındırma ve dağıtma</span><span class="sxs-lookup"><span data-stu-id="0aa78-103">Host and deploy Blazor Server</span></span>

<span data-ttu-id="0aa78-104">, [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)ve [Daniel Roth](https://github.com/danroth27) tarafından</span><span class="sxs-lookup"><span data-stu-id="0aa78-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="0aa78-105">Ana bilgisayar yapılandırma değerleri</span><span class="sxs-lookup"><span data-stu-id="0aa78-105">Host configuration values</span></span>

<span data-ttu-id="0aa78-106">[Blazor Server uygulamaları](xref:blazor/hosting-models#blazor-server) [genel ana bilgisayar yapılandırma değerlerini](xref:fundamentals/host/generic-host#host-configuration)kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="0aa78-106">[Blazor Server apps](xref:blazor/hosting-models#blazor-server) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="0aa78-107">Dağıtım</span><span class="sxs-lookup"><span data-stu-id="0aa78-107">Deployment</span></span>

<span data-ttu-id="0aa78-108">[Blazor sunucusu barındırma modelini](xref:blazor/hosting-models#blazor-server)kullanarak, Blazor sunucuda bir ASP.NET Core uygulaması içinden yürütülür.</span><span class="sxs-lookup"><span data-stu-id="0aa78-108">Using the [Blazor Server hosting model](xref:blazor/hosting-models#blazor-server), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="0aa78-109">Kullanıcı Arabirimi güncelleştirmeleri, olay işleme ve JavaScript çağrıları [SignalR](xref:signalr/introduction) bir bağlantı üzerinden işlenir.</span><span class="sxs-lookup"><span data-stu-id="0aa78-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="0aa78-110">ASP.NET Core uygulaması barındırabilen bir Web sunucusu gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="0aa78-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="0aa78-111">Visual Studio, [DotNet New](/dotnet/core/tools/dotnet-new) komutu kullanılırken **Blazor Server uygulaması** proje şablonunu (`blazorserverside` şablonu) içerir.</span><span class="sxs-lookup"><span data-stu-id="0aa78-111">Visual Studio includes the **Blazor Server App** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

## <a name="scalability"></a><span data-ttu-id="0aa78-112">Ölçeklenebilirlik</span><span class="sxs-lookup"><span data-stu-id="0aa78-112">Scalability</span></span>

<span data-ttu-id="0aa78-113">Bir Blazor sunucusu uygulaması için kullanılabilir altyapıyı en iyi şekilde kullanmasını sağlamak üzere bir dağıtım planlayın.</span><span class="sxs-lookup"><span data-stu-id="0aa78-113">Plan a deployment to make the best use of the available infrastructure for a Blazor Server app.</span></span> <span data-ttu-id="0aa78-114">Blazor Server uygulama ölçeklenebilirliğini karşılamak için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="0aa78-114">See the following resources to address Blazor Server app scalability:</span></span>

* <span data-ttu-id="0aa78-115">[Blazor Server uygulamalarının temelleri](xref:blazor/hosting-models#blazor-server)</span><span class="sxs-lookup"><span data-stu-id="0aa78-115">[Fundamentals of Blazor Server apps](xref:blazor/hosting-models#blazor-server)</span></span>
* <xref:security/blazor/server>

### <a name="deployment-server"></a><span data-ttu-id="0aa78-116">Dağıtım sunucusu</span><span class="sxs-lookup"><span data-stu-id="0aa78-116">Deployment server</span></span>

<span data-ttu-id="0aa78-117">Tek bir sunucunun ölçeklenebilirliğini değerlendirirken (ölçeği büyütme), bir uygulama için kullanılabilir olan bellek büyük olasılıkla uygulamanın kullanıcı talebi arttıkça arttırabileceği ilk kaynaktır.</span><span class="sxs-lookup"><span data-stu-id="0aa78-117">When considering the scalability of a single server (scale up), the memory available to an app is likely the first resource that the app will exhaust as user demands increase.</span></span> <span data-ttu-id="0aa78-118">Sunucudaki kullanılabilir bellek şunları etkiler:</span><span class="sxs-lookup"><span data-stu-id="0aa78-118">The available memory on the server affects the:</span></span>

* <span data-ttu-id="0aa78-119">Bir sunucunun destekleyebileceği etkin devre sayısı.</span><span class="sxs-lookup"><span data-stu-id="0aa78-119">Number of active circuits that a server can support.</span></span>
* <span data-ttu-id="0aa78-120">İstemcide UI gecikme süresi.</span><span class="sxs-lookup"><span data-stu-id="0aa78-120">UI latency on the client.</span></span>

<span data-ttu-id="0aa78-121">Güvenli ve ölçeklenebilir Blazor sunucu uygulamaları oluşturma hakkında yönergeler için bkz. <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="0aa78-121">For guidance on building secure and scalable Blazor server apps, see <xref:security/blazor/server>.</span></span>

<span data-ttu-id="0aa78-122">Her bağlantı hattı en düşük *Merhaba Dünya*stilinde bir uygulama için YAKLAŞıK 250 KB bellek kullanır.</span><span class="sxs-lookup"><span data-stu-id="0aa78-122">Each circuit uses approximately 250 KB of memory for a minimal *Hello World*-style app.</span></span> <span data-ttu-id="0aa78-123">Bir devrenin boyutu, uygulamanın koduna ve her bileşenle ilişkili durum bakım gereksinimlerine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="0aa78-123">The size of a circuit depends on the app's code and the state maintenance requirements associated with each component.</span></span> <span data-ttu-id="0aa78-124">Uygulama ve altyapınız için geliştirme sırasında kaynak taleplerini ölçmenizi öneririz, ancak aşağıdaki taban çizgisi, dağıtım hedefini planlamada bir başlangıç noktası olabilir: uygulamanızın 5.000 eşzamanlı kullanıcı desteklemesini istiyorsanız, şu adreste bütçeleme yapmayı düşünün: uygulamaya en az 1,3 GB sunucu belleği (veya Kullanıcı başına ~ 273 KB).</span><span class="sxs-lookup"><span data-stu-id="0aa78-124">We recommend that you measure resource demands during development for your app and infrastructure, but the following baseline can be a starting point in planning your deployment target: If you expect your app to support 5,000 concurrent users, consider budgeting at least 1.3 GB of server memory to the app (or ~273 KB per user).</span></span>

### <a name="opno-locsignalr-configuration"></a>SignalR<span data-ttu-id="0aa78-125"> yapılandırması</span><span class="sxs-lookup"><span data-stu-id="0aa78-125"> configuration</span></span>

Blazor<span data-ttu-id="0aa78-126"> sunucu uygulamaları tarayıcıyla iletişim kurmak için ASP.NET Core SignalR kullanır.</span><span class="sxs-lookup"><span data-stu-id="0aa78-126"> Server apps use ASP.NET Core SignalR to communicate with the browser.</span></span> <span data-ttu-id="0aa78-127">[SignalRbarındırma ve ölçeklendirme koşulları](xref:signalr/publish-to-azure-web-app) Blazor sunucu uygulamaları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="0aa78-127">[SignalR's hosting and scaling conditions](xref:signalr/publish-to-azure-web-app) apply to Blazor Server apps.</span></span>

<span data-ttu-id="0aa78-128">daha düşük gecikme süresi, güvenilirlik ve [güvenlik](xref:signalr/security)nedeniyle SignalR taşıma olarak WebSockets kullanırken Blazor en iyi şekilde işe yarar.</span><span class="sxs-lookup"><span data-stu-id="0aa78-128">Blazor works best when using WebSockets as the SignalR transport due to lower latency, reliability, and [security](xref:signalr/security).</span></span> <span data-ttu-id="0aa78-129">Uzun yoklama, WebSockets kullanılamadığında veya uygulama açıkça uzun yoklamayı kullanacak şekilde yapılandırıldığında SignalR tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0aa78-129">Long Polling is used by SignalR when WebSockets isn't available or when the app is explicitly configured to use Long Polling.</span></span> <span data-ttu-id="0aa78-130">Azure App Service dağıtım sırasında, uygulamayı hizmetin Azure portal ayarları içinde kullanmak üzere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="0aa78-130">When deploying to Azure App Service, configure the app to use WebSockets in the Azure portal settings for the service.</span></span> <span data-ttu-id="0aa78-131">Azure App Service için uygulamayı yapılandırma hakkında ayrıntılı bilgi için bkz. [SignalR yayımlama yönergeleri](xref:signalr/publish-to-azure-web-app).</span><span class="sxs-lookup"><span data-stu-id="0aa78-131">For details on configuring the app for Azure App Service, see the [SignalR publishing guidelines](xref:signalr/publish-to-azure-web-app).</span></span>

<span data-ttu-id="0aa78-132">Blazor Server uygulamaları için [Azure SignalR hizmetini](/azure/azure-signalr) kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="0aa78-132">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="0aa78-133">Hizmet, Blazor sunucu uygulamasının ölçeğini çok sayıda eşzamanlı SignalR bağlantı ile ölçeklendirmeye olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="0aa78-133">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="0aa78-134">Ayrıca, SignalR hizmetin küresel erişim ve yüksek performanslı veri merkezleri Coğrafya nedeniyle gecikme süresini azaltmaya önemli ölçüde yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="0aa78-134">In addition, the SignalR service's global reach and high-performance data centers significantly aid in reducing latency due to geography.</span></span> <span data-ttu-id="0aa78-135">Azure SignalR hizmetini bir uygulamayı yapılandırmak (ve isteğe bağlı olarak sağlamak) için:</span><span class="sxs-lookup"><span data-stu-id="0aa78-135">To configure an app (and optionally provision) the Azure SignalR Service:</span></span>

* <span data-ttu-id="0aa78-136">Blazor Server uygulaması için Visual Studio 'da bir Azure Apps yayımlama profili oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0aa78-136">Create an Azure Apps publish profile in Visual Studio for the Blazor Server app.</span></span>
* <span data-ttu-id="0aa78-137">**Azure SignalR hizmeti** bağımlılığını profile ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0aa78-137">Add the **Azure SignalR Service** dependency to the profile.</span></span> <span data-ttu-id="0aa78-138">Azure aboneliğinin uygulamaya atanacak önceden mevcut bir Azure SignalR hizmeti örneği yoksa, yeni bir hizmet örneği sağlamak için **Yeni bir azure SignalR hizmet örneği oluştur** ' u seçin.</span><span class="sxs-lookup"><span data-stu-id="0aa78-138">If the Azure subscription doesn't have a pre-existing Azure SignalR Service instance to assign to the app, select **Create a new Azure SignalR Service instance** to provision a new service instance.</span></span>
* <span data-ttu-id="0aa78-139">Uygulamayı Azure 'da yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="0aa78-139">Publish the app to Azure.</span></span>

### <a name="measure-network-latency"></a><span data-ttu-id="0aa78-140">Ölçü ağı gecikmesi</span><span class="sxs-lookup"><span data-stu-id="0aa78-140">Measure network latency</span></span>

<span data-ttu-id="0aa78-141">Aşağıdaki örnekte gösterildiği gibi, [js birlikte çalışması](xref:blazor/javascript-interop) ağ gecikmesini ölçmek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="0aa78-141">[JS interop](xref:blazor/javascript-interop) can be used to measure network latency, as the following example demonstrates:</span></span>

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

<span data-ttu-id="0aa78-142">Makul bir kullanıcı arabirimi deneyimi için, 250ms veya daha az sayıda sürekli Kullanıcı arabirimi gecikme süresi önerilir.</span><span class="sxs-lookup"><span data-stu-id="0aa78-142">For a reasonable UI experience, we recommend a sustained UI latency of 250ms or less.</span></span>
