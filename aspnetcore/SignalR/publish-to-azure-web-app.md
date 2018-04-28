---
title: Bir ASP.NET Core yayımlama SignalR uygulaması Azure Web uygulaması
author: rachelappel
description: Bir ASP.NET Core yayımlama SignalR uygulaması Azure Web uygulaması
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/20/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: fd7d38ad47d9004db2ae7b5858dc22609943f601
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a><span data-ttu-id="781f8-103">Bir ASP.NET Core yayımlama SignalR uygulaması bir Azure Web uygulaması</span><span class="sxs-lookup"><span data-stu-id="781f8-103">Publish an ASP.NET Core SignalR app to an Azure Web App</span></span>

<span data-ttu-id="781f8-104">[Azure Web uygulaması](/azure/app-service/app-service-web-overview) olan bir [Microsoft bulut bilgi işlem](https://azure.microsoft.com/) ASP.NET Core dahil web uygulamalarını barındırmak için hizmet platform.</span><span class="sxs-lookup"><span data-stu-id="781f8-104">[Azure Web App](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="781f8-105">Uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="781f8-105">Publish the app</span></span>

<span data-ttu-id="781f8-106">Visual Studio için Azure Web uygulaması yayımlama için yerleşik araçlar sağlar.</span><span class="sxs-lookup"><span data-stu-id="781f8-106">Visual Studio provides built-in tools for publishing to an Azure Web App.</span></span> <span data-ttu-id="781f8-107">Visual Studio Code kullanıcının kullanabileceği [Azure CLI](/cli/azure) uygulamaları için Azure yayımlama için komutları.</span><span class="sxs-lookup"><span data-stu-id="781f8-107">Visual Studio Code user can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="781f8-108">Bu makalede, Visual Studio'da yayımlama araçları kullanarak kapsar.</span><span class="sxs-lookup"><span data-stu-id="781f8-108">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="781f8-109">Azure CLI kullanarak bir uygulamayı yayımlamak için bkz: [ASP.NET Core uygulama için Azure komut satırı araçları ile yayımlama](xref:tutorials/publish-to-azure-webapp-using-cli).</span><span class="sxs-lookup"><span data-stu-id="781f8-109">To publish an app using Azure CLI, see [Publish an ASP.NET Core app to Azure with command line tools](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

<span data-ttu-id="781f8-110">Projeye sağ tıklayın **Çözüm Gezgini** seçip **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="781f8-110">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span> <span data-ttu-id="781f8-111">Onaylayın **Yeni Oluştur** iade **yayımlama hedefi çekme** iletişim ve select **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="781f8-111">Confirm that **Create new** is checked in the **Pick a publish target** dialog, and select **Publish**.</span></span>

![Çekme yayımlama hedefi](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

<span data-ttu-id="781f8-113">Aşağıdaki bilgileri girin **App Service Oluştur** iletişim ve select **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="781f8-113">Enter the following information in the **Create App Service** dialog and select **Create**.</span></span>

| <span data-ttu-id="781f8-114">Öğe</span><span class="sxs-lookup"><span data-stu-id="781f8-114">Item</span></span> | <span data-ttu-id="781f8-115">Açıklama</span><span class="sxs-lookup"><span data-stu-id="781f8-115">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="781f8-116">**Uygulama adı**</span><span class="sxs-lookup"><span data-stu-id="781f8-116">**App name**</span></span> | <span data-ttu-id="781f8-117">Uygulamanın benzersiz bir ad.</span><span class="sxs-lookup"><span data-stu-id="781f8-117">A unique name of the app.</span></span> |
| <span data-ttu-id="781f8-118">**Abonelik**</span><span class="sxs-lookup"><span data-stu-id="781f8-118">**Subscription**</span></span> | <span data-ttu-id="781f8-119">Uygulamanın kullandığı Azure aboneliği.</span><span class="sxs-lookup"><span data-stu-id="781f8-119">The Azure subscription that the app uses.</span></span> |
| <span data-ttu-id="781f8-120">**Kaynak grubu**</span><span class="sxs-lookup"><span data-stu-id="781f8-120">**Resource Group**</span></span> | <span data-ttu-id="781f8-121">İlgili kaynaklar uygulamanın ait olduğu grubu.</span><span class="sxs-lookup"><span data-stu-id="781f8-121">The group of related resources to which the app belongs.</span></span>  |
| <span data-ttu-id="781f8-122">**Barındırma planı**</span><span class="sxs-lookup"><span data-stu-id="781f8-122">**Hosting Plan**</span></span> | <span data-ttu-id="781f8-123">Web uygulaması için fiyatlandırma planı.</span><span class="sxs-lookup"><span data-stu-id="781f8-123">The pricing plan for the web app.</span></span> |

![Uygulama hizmeti oluşturma](publish-to-azure-web-app/_static/create-app-service-dialog.png)

<span data-ttu-id="781f8-125">Visual Studio aşağıdaki görevleri tamamlar:</span><span class="sxs-lookup"><span data-stu-id="781f8-125">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="781f8-126">Bir yayımlama profili oluşturur içeren yayımlama ayarları.</span><span class="sxs-lookup"><span data-stu-id="781f8-126">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="781f8-127">Oluşturur veya mevcut bir kullanan *Azure Web uygulaması* sağlanan ayrıntılarla.</span><span class="sxs-lookup"><span data-stu-id="781f8-127">Creates or uses an existing *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="781f8-128">Uygulama yayımlar.</span><span class="sxs-lookup"><span data-stu-id="781f8-128">Publishes the app.</span></span>
* <span data-ttu-id="781f8-129">Bir tarayıcı yüklenen yayımlanan web uygulaması ile başlatır.</span><span class="sxs-lookup"><span data-stu-id="781f8-129">Launches a browser, with the published web app loaded.</span></span>

<span data-ttu-id="781f8-130">Uygulama için URL biçimi fark *{uygulama adı} .azurewebsites .net*.</span><span class="sxs-lookup"><span data-stu-id="781f8-130">Notice the format of the URL for the app is *{app name}.azurewebsites.net*.</span></span> <span data-ttu-id="781f8-131">Örneğin, adlı bir uygulama `SignalRChattR` benzer bir URL'ye sahip `https://signalrchattr.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="781f8-131">For example, an app named `SignalRChattR` has a URL that looks like `https://signalrchattr.azurewebsites.net`.</span></span>

<span data-ttu-id="781f8-132">Bir HTTP 502.2 hata oluşursa, bakınız [Azure App Service'e dağıtma ASP.NET Core Önizleme sürümü](xref:host-and-deploy/azure-apps/index) bunu çözmek için.</span><span class="sxs-lookup"><span data-stu-id="781f8-132">If an HTTP 502.2 error occurs, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index) to resolve it.</span></span>

## <a name="configure-signalr-web-app"></a><span data-ttu-id="781f8-133">SignalR web uygulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="781f8-133">Configure SignalR web app</span></span>

<span data-ttu-id="781f8-134">Bir Azure Web uygulaması olarak yayımlanan ASP.NET Core SignalR uygulamalara [ARR benzeşim](https://en.wikipedia.org/wiki/Application_Request_Routing) etkin.</span><span class="sxs-lookup"><span data-stu-id="781f8-134">ASP.NET Core SignalR apps that are published as an Azure Web App must have [ARR Affinity](https://en.wikipedia.org/wiki/Application_Request_Routing) enabled.</span></span> <span data-ttu-id="781f8-135">[WebSockets](xref:fundamentals/websockets) , işlevi WebSockets aktarıma izin vermek için etkinleştirilmiş olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="781f8-135">[WebSockets](xref:fundamentals/websockets) should be enabled, to allow the WebSockets transport to function.</span></span>

<span data-ttu-id="781f8-136">Azure portalında gidin **uygulama ayarları** web uygulamanız için.</span><span class="sxs-lookup"><span data-stu-id="781f8-136">In the Azure portal, navigate to **App Settings** for your web app.</span></span> <span data-ttu-id="781f8-137">Ayarlama **WebSockets** için **üzerinde**ve doğrulama **ARR benzeşim** olan **üzerinde**.</span><span class="sxs-lookup"><span data-stu-id="781f8-137">Set **WebSockets** to **On**, and verify **ARR Affinity** is **On**.</span></span>

![Azure portalında Azure Web uygulaması ayarları](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 <span data-ttu-id="781f8-139">WebSockets ve diğer taşımaları [App Service planı üzerinde temel sınırlıdır](/azure/azure-subscription-service-limits#app-service-limits).</span><span class="sxs-lookup"><span data-stu-id="781f8-139">WebSockets and other transports [are limited based on the App Service Plan](/azure/azure-subscription-service-limits#app-service-limits).</span></span>

## <a name="related-resources"></a><span data-ttu-id="781f8-140">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="781f8-140">Related resources</span></span>

* [<span data-ttu-id="781f8-141">ASP.NET Core uygulama için Azure komut satırı araçları ile yayımlama</span><span class="sxs-lookup"><span data-stu-id="781f8-141">Publish an ASP.NET Core app to Azure with command line tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [<span data-ttu-id="781f8-142">Visual Studio ile Azure ASP.NET Core uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="781f8-142">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)
* [<span data-ttu-id="781f8-143">Ana bilgisayar ve Azure üzerinde ASP.NET Core Önizleme uygulamaları dağıtma</span><span class="sxs-lookup"><span data-stu-id="781f8-143">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
