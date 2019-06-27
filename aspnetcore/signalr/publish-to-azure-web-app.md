---
title: Bir ASP.NET Core yayımlama SignalR uygulamasını Azure App Service'e
author: bradygaster
description: Azure App Service'e bir ASP.NET Core SignalR uygulama yayımlama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/26/2019
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 87a9c93add373b24e3c473912cdbfcc00bbebf7e
ms.sourcegitcommit: 9bb29f9ba6f0645ee8b9cabda07e3a5aa52cd659
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67406106"
---
# <a name="publish-an-aspnet-core-signalr-app-to-azure-app-service"></a><span data-ttu-id="37ba9-103">Bir ASP.NET Core yayımlama SignalR uygulamasını Azure App Service'e</span><span class="sxs-lookup"><span data-stu-id="37ba9-103">Publish an ASP.NET Core SignalR app to Azure App Service</span></span>

<span data-ttu-id="37ba9-104">Tarafından [Brady Gaster](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="37ba9-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="37ba9-105">[Azure App Service](/azure/app-service/app-service-web-overview) olduğu bir [Microsoft bulut bilgi işlem](https://azure.microsoft.com/) ASP.NET Core web uygulamaları barındırmak için bir hizmet.</span><span class="sxs-lookup"><span data-stu-id="37ba9-105">[Azure App Service](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="37ba9-106">Bu makalede, bir ASP.NET Core SignalR uygulamayı Visual Studio'dan yayımlama için ifade eder.</span><span class="sxs-lookup"><span data-stu-id="37ba9-106">This article refers to publishing an ASP.NET Core SignalR app from Visual Studio.</span></span> <span data-ttu-id="37ba9-107">Daha fazla bilgi için [Azure SignalR hizmeti](https://azure.microsoft.com/services/signalr-service).</span><span class="sxs-lookup"><span data-stu-id="37ba9-107">For more information, see [SignalR service for Azure](https://azure.microsoft.com/services/signalr-service).</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="37ba9-108">Uygulamayı yayımlama</span><span class="sxs-lookup"><span data-stu-id="37ba9-108">Publish the app</span></span>

<span data-ttu-id="37ba9-109">Bu makale, Visual Studio'da yayımlama araçları kullanarak kapsar.</span><span class="sxs-lookup"><span data-stu-id="37ba9-109">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="37ba9-110">Visual Studio Code kullanıcılar [Azure CLI](/cli/azure) komutlarını uygulamalarını Azure'da yayımlayabilir.</span><span class="sxs-lookup"><span data-stu-id="37ba9-110">Visual Studio Code users can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="37ba9-111">Daha fazla bilgi için [komut satırı araçları ile Azure'da ASP.NET Core uygulaması yayımlama](/azure/app-service/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="37ba9-111">For more information, see [Publish an ASP.NET Core app to Azure with command line tools](/azure/app-service/app-service-web-get-started-dotnet).</span></span>

1. <span data-ttu-id="37ba9-112">Projeye sağ tıklayarak **Çözüm Gezgini** seçip **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="37ba9-112">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span>

1. <span data-ttu-id="37ba9-113">Onaylayın **App Service** ve **Yeni Oluştur** seçili **yayımlama hedefi seçin** iletişim.</span><span class="sxs-lookup"><span data-stu-id="37ba9-113">Confirm that **App Service** and **Create new** are selected in the **Pick a publish target** dialog.</span></span>

1. <span data-ttu-id="37ba9-114">Seçin **profili oluştur** gelen **Yayımla** aşağı açılır düğme.</span><span class="sxs-lookup"><span data-stu-id="37ba9-114">Select **Create Profile** from the **Publish** button drop down.</span></span>

   <span data-ttu-id="37ba9-115">Aşağıdaki tabloda açıklanan bilgileri girin **App Service Oluştur** iletişim ve select **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="37ba9-115">Enter the information described in the following table in the **Create App Service** dialog and select **Create**.</span></span>

   | <span data-ttu-id="37ba9-116">Öğe</span><span class="sxs-lookup"><span data-stu-id="37ba9-116">Item</span></span>               | <span data-ttu-id="37ba9-117">Açıklama</span><span class="sxs-lookup"><span data-stu-id="37ba9-117">Description</span></span> |
   | ------------------ | ----------- |
   | <span data-ttu-id="37ba9-118">**Ad**</span><span class="sxs-lookup"><span data-stu-id="37ba9-118">**Name**</span></span>           | <span data-ttu-id="37ba9-119">Uygulamanın benzersiz adı.</span><span class="sxs-lookup"><span data-stu-id="37ba9-119">Unique name of the app.</span></span> |
   | <span data-ttu-id="37ba9-120">**Abonelik**</span><span class="sxs-lookup"><span data-stu-id="37ba9-120">**Subscription**</span></span>   | <span data-ttu-id="37ba9-121">Uygulamanın kullandığı azure aboneliği.</span><span class="sxs-lookup"><span data-stu-id="37ba9-121">Azure subscription that the app uses.</span></span> |
   | <span data-ttu-id="37ba9-122">**Kaynak Grubu**</span><span class="sxs-lookup"><span data-stu-id="37ba9-122">**Resource Group**</span></span> | <span data-ttu-id="37ba9-123">İlgili kaynakları uygulamanın ait olduğu grubu.</span><span class="sxs-lookup"><span data-stu-id="37ba9-123">Group of related resources to which the app belongs.</span></span> |
   | <span data-ttu-id="37ba9-124">**Barındırma planı**</span><span class="sxs-lookup"><span data-stu-id="37ba9-124">**Hosting Plan**</span></span>   | <span data-ttu-id="37ba9-125">Web uygulamanız için fiyatlandırma planı.</span><span class="sxs-lookup"><span data-stu-id="37ba9-125">Pricing plan for the web app.</span></span> |

1. <span data-ttu-id="37ba9-126">Seçin **Azure SignalR hizmeti** içinde **bağımlılıkları** > **Ekle** aşağı açılan listesi:</span><span class="sxs-lookup"><span data-stu-id="37ba9-126">Select the **Azure SignalR Service** in the **Dependencies** > **Add** drop-down list:</span></span>

   ![Azure SignalR hizmeti seçimini Ekle aşağı açılan listede gösteren bağımlılıkları alanı](publish-to-azure-web-app/_static/signalr-service-dependency.png)

1. <span data-ttu-id="37ba9-128">İçinde **Azure SignalR hizmeti** iletişim kutusunda **yeni bir Azure SignalR hizmeti örneği oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="37ba9-128">In the **Azure SignalR Service** dialog, select **Create a new Azure SignalR Service instance**.</span></span>

1. <span data-ttu-id="37ba9-129">Sağlayan bir **adı**, **kaynak grubu**, ve **konumu**.</span><span class="sxs-lookup"><span data-stu-id="37ba9-129">Provide a **Name**, **Resource Group**, and **Location**.</span></span> <span data-ttu-id="37ba9-130">Geri dönüp **Azure SignalR hizmeti** iletişim ve select **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="37ba9-130">Return to the **Azure SignalR Service** dialog and select **Add**.</span></span>

<span data-ttu-id="37ba9-131">Visual Studio aşağıdaki görevleri tamamlar:</span><span class="sxs-lookup"><span data-stu-id="37ba9-131">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="37ba9-132">Bir yayımlama profili oluşturur içeren yayımlama ayarları.</span><span class="sxs-lookup"><span data-stu-id="37ba9-132">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="37ba9-133">Oluşturur bir *Azure Web uygulaması* sağlanan ayrıntılara sahip.</span><span class="sxs-lookup"><span data-stu-id="37ba9-133">Creates an *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="37ba9-134">Uygulamanın yayınlar.</span><span class="sxs-lookup"><span data-stu-id="37ba9-134">Publishes the app.</span></span>
* <span data-ttu-id="37ba9-135">Web uygulamasını yükleyen bir tarayıcı başlatır.</span><span class="sxs-lookup"><span data-stu-id="37ba9-135">Launches a browser, which loads the web app.</span></span>

<span data-ttu-id="37ba9-136">Uygulamanın URL'si biçimi `{APP SERVICE NAME}.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="37ba9-136">The format of the app's URL is `{APP SERVICE NAME}.azurewebsites.net`.</span></span> <span data-ttu-id="37ba9-137">Örneğin, adlı bir uygulama `SignalRChatApp` , bir URL'ye sahip `https://signalrchatapp.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="37ba9-137">For example, an app named `SignalRChatApp` has a URL of `https://signalrchatapp.azurewebsites.net`.</span></span>

<span data-ttu-id="37ba9-138">Bir HTTP, *502.2 - bozuk ağ geçidi* .NET Core yayın önizlemesini hedefleyen bir uygulama dağıtımı hatası oluşur, bkz: [Azure App Service'e dağıtma ASP.NET Core Önizleme sürümü](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) bu sorunu çözmek için.</span><span class="sxs-lookup"><span data-stu-id="37ba9-138">If an HTTP *502.2 - Bad Gateway* error occurs when deploying an app that targets a preview .NET Core release, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) to resolve it.</span></span>

## <a name="configure-the-app-in-azure-app-service"></a><span data-ttu-id="37ba9-139">Azure App Service'te uygulama yapılandırma</span><span class="sxs-lookup"><span data-stu-id="37ba9-139">Configure the app in Azure App Service</span></span>

> [!NOTE]
> <span data-ttu-id="37ba9-140">*Bu bölüm, yalnızca Azure SignalR hizmeti kullanmayan uygulamalar için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="37ba9-140">*This section only applies to apps not using the Azure SignalR Service.*</span></span>
>
> <span data-ttu-id="37ba9-141">Azure SignalR hizmeti uygulama kullanıyorsa, App Service uygulama isteği yönlendirme (ARR) benzeşimi ve Web yuvaları Bu bölümde açıklanan yapılandırma gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="37ba9-141">If the app uses the Azure SignalR Service, the App Service doesn't require the configuration of Application Request Routing (ARR) Affinity and Web Sockets described in this section.</span></span> <span data-ttu-id="37ba9-142">İstemciler kendi Web yuvalarını Azure SignalR hizmeti, uygulama için doğrudan bağlanır.</span><span class="sxs-lookup"><span data-stu-id="37ba9-142">Clients connect their Web Sockets to the Azure SignalR Service, not directly to the app.</span></span>

<span data-ttu-id="37ba9-143">Azure SignalR hizmeti olmadan barındırılan uygulamalar için etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="37ba9-143">For apps hosted without the Azure SignalR Service, enable:</span></span>

* <span data-ttu-id="37ba9-144">[ARR benzeşimi](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) aynı App Service örneğine geri bir kullanıcıdan gelen istekleri.</span><span class="sxs-lookup"><span data-stu-id="37ba9-144">[ARR Affinity](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) to route requests from a user back to the same App Service instance.</span></span> <span data-ttu-id="37ba9-145">Varsayılan ayar **üzerinde**.</span><span class="sxs-lookup"><span data-stu-id="37ba9-145">The default setting is **On**.</span></span>
* <span data-ttu-id="37ba9-146">[Web yuvaları](xref:fundamentals/websockets) işlevi Web yuvalarını aktarıma izin vermek için.</span><span class="sxs-lookup"><span data-stu-id="37ba9-146">[Web Sockets](xref:fundamentals/websockets) to allow the Web Sockets transport to function.</span></span> <span data-ttu-id="37ba9-147">Varsayılan ayar **kapalı**.</span><span class="sxs-lookup"><span data-stu-id="37ba9-147">The default setting is **Off**.</span></span>

1. <span data-ttu-id="37ba9-148">Azure portalında web uygulamasında gidin **uygulama hizmetleri**.</span><span class="sxs-lookup"><span data-stu-id="37ba9-148">In the Azure portal, navigate to the web app in **App Services**.</span></span>
1. <span data-ttu-id="37ba9-149">Açık **yapılandırma** > **genel ayarlar**.</span><span class="sxs-lookup"><span data-stu-id="37ba9-149">Open **Configuration** > **General settings**.</span></span>
1. <span data-ttu-id="37ba9-150">Ayarlama **Web yuvaları** için **üzerinde**.</span><span class="sxs-lookup"><span data-stu-id="37ba9-150">Set **Web sockets** to **On**.</span></span>
1. <span data-ttu-id="37ba9-151">Doğrulayın **ARR benzeşimi** ayarlanır **üzerinde**.</span><span class="sxs-lookup"><span data-stu-id="37ba9-151">Verify that **ARR affinity** is set to **On**.</span></span>

## <a name="app-service-plan-limits"></a><span data-ttu-id="37ba9-152">App Service planı sınırları</span><span class="sxs-lookup"><span data-stu-id="37ba9-152">App Service Plan limits</span></span>

<span data-ttu-id="37ba9-153">Web yuvaları ve diğer taşımaları, uygulama hizmeti seçili planınıza göre sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="37ba9-153">Web Sockets and other transports are limited based on the App Service Plan selected.</span></span> <span data-ttu-id="37ba9-154">Daha fazla bilgi için *Azure Cloud Services sınırlar* ve *App Service limitleri* bölümlerini [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](/azure/azure-subscription-service-limits#app-service-limits) makale.</span><span class="sxs-lookup"><span data-stu-id="37ba9-154">For more information, see the *Azure Cloud Services limits* and *App Service limits* sections of the [Azure subscription and service limits, quotas, and constraints](/azure/azure-subscription-service-limits#app-service-limits) article.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="37ba9-155">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="37ba9-155">Additional resources</span></span>

* [<span data-ttu-id="37ba9-156">Azure SignalR hizmeti nedir?</span><span class="sxs-lookup"><span data-stu-id="37ba9-156">What is Azure SignalR Service?</span></span>](/azure/azure-signalr/signalr-overview)
* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="37ba9-157">Komut satırı araçları ile Azure'da ASP.NET Core uygulaması yayımlama</span><span class="sxs-lookup"><span data-stu-id="37ba9-157">Publish an ASP.NET Core app to Azure with command line tools</span></span>](/azure/app-service/app-service-web-get-started-dotnet)
* [<span data-ttu-id="37ba9-158">Barındırma ve azure'da ASP.NET Core Önizleme uygulamaları dağıtma</span><span class="sxs-lookup"><span data-stu-id="37ba9-158">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
