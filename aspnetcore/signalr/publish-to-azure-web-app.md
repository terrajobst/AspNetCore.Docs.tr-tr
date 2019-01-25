---
title: Bir ASP.NET Core yayımlama SignalR uygulamasına Azure Web uygulaması
author: bradygaster
description: Bir ASP.NET Core yayımlama SignalR uygulamasına Azure Web uygulaması
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 66fa855590c49c4284e4b42cae57f3d4d81dd0fc
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837682"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a><span data-ttu-id="9e48a-103">Bir ASP.NET Core yayımlama SignalR uygulama için bir Azure Web uygulaması</span><span class="sxs-lookup"><span data-stu-id="9e48a-103">Publish an ASP.NET Core SignalR app to an Azure Web App</span></span>

<span data-ttu-id="9e48a-104">[Azure Web uygulaması](/azure/app-service/app-service-web-overview) olduğu bir [Microsoft bulut bilgi işlem](https://azure.microsoft.com/) ASP.NET Core web uygulamaları barındırmak için bir hizmet.</span><span class="sxs-lookup"><span data-stu-id="9e48a-104">[Azure Web App](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="9e48a-105">Bu makalede, bir ASP.NET Core SignalR uygulamayı Visual Studio'dan yayımlama için ifade eder.</span><span class="sxs-lookup"><span data-stu-id="9e48a-105">This article refers to publishing an ASP.NET Core SignalR app from Visual Studio.</span></span> <span data-ttu-id="9e48a-106">Ziyaret [Azure SignalR hizmeti](https://azure.microsoft.com/en-gb/services/signalr-service?) Azure'da SignalR kullanma hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="9e48a-106">Visit [SignalR service for Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) for more information about using SignalR on Azure.</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="9e48a-107">Uygulamayı yayımlama</span><span class="sxs-lookup"><span data-stu-id="9e48a-107">Publish the app</span></span>

<span data-ttu-id="9e48a-108">Visual Studio, bir Azure Web uygulaması için yayımlama için yerleşik araçlar sağlar.</span><span class="sxs-lookup"><span data-stu-id="9e48a-108">Visual Studio provides built-in tools for publishing to an Azure Web App.</span></span> <span data-ttu-id="9e48a-109">Visual Studio Code kullanıcı kullanabileceğiniz [Azure CLI](/cli/azure) komutlarını uygulamalarını Azure'da yayımlayabilir.</span><span class="sxs-lookup"><span data-stu-id="9e48a-109">Visual Studio Code user can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="9e48a-110">Bu makale, Visual Studio'da yayımlama araçları kullanarak kapsar.</span><span class="sxs-lookup"><span data-stu-id="9e48a-110">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="9e48a-111">Azure CLI kullanarak bir uygulamayı yayımlamak için bkz: [komut satırı araçları ile Azure'da ASP.NET Core uygulaması yayımlama](/azure/app-service/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="9e48a-111">To publish an app using Azure CLI, see [Publish an ASP.NET Core app to Azure with command line tools](/azure/app-service/app-service-web-get-started-dotnet).</span></span>

<span data-ttu-id="9e48a-112">Projeye sağ tıklayarak **Çözüm Gezgini** seçip **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="9e48a-112">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span> <span data-ttu-id="9e48a-113">Onaylayın **Yeni Oluştur** iade **yayımlama hedefi seçin** iletişim ve select **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="9e48a-113">Confirm that **Create new** is checked in the **Pick a publish target** dialog, and select **Publish**.</span></span>

![Yayımlama hedefi seçme](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

<span data-ttu-id="9e48a-115">Aşağıdaki bilgileri girin **App Service Oluştur** iletişim ve select **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="9e48a-115">Enter the following information in the **Create App Service** dialog and select **Create**.</span></span>

| <span data-ttu-id="9e48a-116">Öğe</span><span class="sxs-lookup"><span data-stu-id="9e48a-116">Item</span></span> | <span data-ttu-id="9e48a-117">Açıklama</span><span class="sxs-lookup"><span data-stu-id="9e48a-117">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="9e48a-118">**Uygulama adı**</span><span class="sxs-lookup"><span data-stu-id="9e48a-118">**App name**</span></span> | <span data-ttu-id="9e48a-119">Uygulamanın benzersiz bir ad.</span><span class="sxs-lookup"><span data-stu-id="9e48a-119">A unique name of the app.</span></span> |
| <span data-ttu-id="9e48a-120">**Abonelik**</span><span class="sxs-lookup"><span data-stu-id="9e48a-120">**Subscription**</span></span> | <span data-ttu-id="9e48a-121">Uygulamanın kullandığı Azure aboneliği.</span><span class="sxs-lookup"><span data-stu-id="9e48a-121">The Azure subscription that the app uses.</span></span> |
| <span data-ttu-id="9e48a-122">**Kaynak Grubu**</span><span class="sxs-lookup"><span data-stu-id="9e48a-122">**Resource Group**</span></span> | <span data-ttu-id="9e48a-123">İlgili kaynakları uygulamanın ait olduğu grubu.</span><span class="sxs-lookup"><span data-stu-id="9e48a-123">The group of related resources to which the app belongs.</span></span>  |
| <span data-ttu-id="9e48a-124">**Barındırma planı**</span><span class="sxs-lookup"><span data-stu-id="9e48a-124">**Hosting Plan**</span></span> | <span data-ttu-id="9e48a-125">Web uygulaması için fiyatlandırma planı.</span><span class="sxs-lookup"><span data-stu-id="9e48a-125">The pricing plan for the web app.</span></span> |

![App service oluştur](publish-to-azure-web-app/_static/create-app-service-dialog.png)

<span data-ttu-id="9e48a-127">Visual Studio aşağıdaki görevleri tamamlar:</span><span class="sxs-lookup"><span data-stu-id="9e48a-127">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="9e48a-128">Bir yayımlama profili oluşturur içeren yayımlama ayarları.</span><span class="sxs-lookup"><span data-stu-id="9e48a-128">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="9e48a-129">Oluşturur veya mevcut bir kullanan *Azure Web uygulaması* sağlanan ayrıntılara sahip.</span><span class="sxs-lookup"><span data-stu-id="9e48a-129">Creates or uses an existing *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="9e48a-130">Uygulamanın yayınlar.</span><span class="sxs-lookup"><span data-stu-id="9e48a-130">Publishes the app.</span></span>
* <span data-ttu-id="9e48a-131">Bir tarayıcı ile yüklenen yayımlanan web uygulaması başlatır.</span><span class="sxs-lookup"><span data-stu-id="9e48a-131">Launches a browser, with the published web app loaded.</span></span>

<span data-ttu-id="9e48a-132">Uygulama için URL biçimi fark *{uygulamanızın adı} .azurewebsites .net*.</span><span class="sxs-lookup"><span data-stu-id="9e48a-132">Notice the format of the URL for the app is *{app name}.azurewebsites.net*.</span></span> <span data-ttu-id="9e48a-133">Örneğin, adlı bir uygulama `SignalRChattR` şuna benzer bir URL'ye sahip `https://signalrchattr.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="9e48a-133">For example, an app named `SignalRChattR` has a URL that looks like `https://signalrchattr.azurewebsites.net`.</span></span>

<span data-ttu-id="9e48a-134">Bir HTTP 502.2 hata oluşursa bkz [Azure App Service'e dağıtma ASP.NET Core Önizleme sürümü](xref:host-and-deploy/azure-apps/index) bu sorunu çözmek için.</span><span class="sxs-lookup"><span data-stu-id="9e48a-134">If an HTTP 502.2 error occurs, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index) to resolve it.</span></span>

## <a name="configure-signalr-web-app"></a><span data-ttu-id="9e48a-135">SignalR web uygulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="9e48a-135">Configure SignalR web app</span></span>

<span data-ttu-id="9e48a-136">Bir Azure Web uygulaması olarak yayımlanan ASP.NET Core SignalR uygulamalara [ARR benzeşimi](https://en.wikipedia.org/wiki/Application_Request_Routing) etkin.</span><span class="sxs-lookup"><span data-stu-id="9e48a-136">ASP.NET Core SignalR apps that are published as an Azure Web App must have [ARR Affinity](https://en.wikipedia.org/wiki/Application_Request_Routing) enabled.</span></span> <span data-ttu-id="9e48a-137">[WebSockets](xref:fundamentals/websockets) , işlev WebSockets aktarıma izin vermek için etkinleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="9e48a-137">[WebSockets](xref:fundamentals/websockets) should be enabled, to allow the WebSockets transport to function.</span></span>

<span data-ttu-id="9e48a-138">Azure portalında gidin **uygulama ayarları** web uygulamanız için.</span><span class="sxs-lookup"><span data-stu-id="9e48a-138">In the Azure portal, navigate to **App Settings** for your web app.</span></span> <span data-ttu-id="9e48a-139">Ayarlama **WebSockets** için **üzerinde**ve doğrulama **ARR benzeşimi** olduğu **üzerinde**.</span><span class="sxs-lookup"><span data-stu-id="9e48a-139">Set **WebSockets** to **On**, and verify **ARR Affinity** is **On**.</span></span>

![Azure portalında Azure Web uygulaması ayarları](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 <span data-ttu-id="9e48a-141">WebSockets ve diğer taşımaları [App Service planı üzerinde temel sınırlıdır](/azure/azure-subscription-service-limits#app-service-limits).</span><span class="sxs-lookup"><span data-stu-id="9e48a-141">WebSockets and other transports [are limited based on the App Service Plan](/azure/azure-subscription-service-limits#app-service-limits).</span></span>

## <a name="related-resources"></a><span data-ttu-id="9e48a-142">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="9e48a-142">Related resources</span></span>

* [<span data-ttu-id="9e48a-143">Komut satırı araçları ile Azure'da ASP.NET Core uygulaması yayımlama</span><span class="sxs-lookup"><span data-stu-id="9e48a-143">Publish an ASP.NET Core app to Azure with command line tools</span></span>](/azure/app-service/app-service-web-get-started-dotnet)
* [<span data-ttu-id="9e48a-144">Visual Studio ile Azure'a bir ASP.NET Core uygulaması yayımlama</span><span class="sxs-lookup"><span data-stu-id="9e48a-144">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)
* [<span data-ttu-id="9e48a-145">Barındırma ve azure'da ASP.NET Core Önizleme uygulamaları dağıtma</span><span class="sxs-lookup"><span data-stu-id="9e48a-145">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
