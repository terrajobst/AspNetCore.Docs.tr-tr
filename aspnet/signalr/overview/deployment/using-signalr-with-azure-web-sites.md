---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Azure App Service'te Web Apps ile SignalR kullanma | Microsoft Docs
author: pfletcher
description: Bu belge, Microsoft Azure üzerinde çalışan bir SignalR uygulamasını yapılandırmak açıklar. Yazılım sürümleri, Visual Studio 2013 veya Vis. öğreticide kullandığınız...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: dabf0f6cfed401e10d2c1134c260022d94c3ab92
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389581"
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a><span data-ttu-id="d8745-104">Azure App Service'te Web Apps ile SignalR kullanma</span><span class="sxs-lookup"><span data-stu-id="d8745-104">Using SignalR with Web Apps in Azure App Service</span></span>
====================
<span data-ttu-id="d8745-105">tarafından [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="d8745-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="d8745-106">Bu belge, Microsoft Azure üzerinde çalışan bir SignalR uygulamasını yapılandırmak açıklar.</span><span class="sxs-lookup"><span data-stu-id="d8745-106">This document describes how to configure a SignalR application that runs on Microsoft Azure.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d8745-107">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="d8745-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="d8745-108">[Visual Studio 2013'ün](https://www.microsoft.com/visualstudio/eng/2013-downloads) veya Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="d8745-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) or Visual Studio 2012</span></span>
> - <span data-ttu-id="d8745-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="d8745-109">.NET 4.5</span></span>
> - <span data-ttu-id="d8745-110">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="d8745-110">SignalR version 2</span></span>
> - <span data-ttu-id="d8745-111">Visual Studio 2013 veya 2012 için Azure SDK 2.3</span><span class="sxs-lookup"><span data-stu-id="d8745-111">Azure SDK 2.3 for Visual Studio 2013 or 2012</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="d8745-112">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="d8745-112">Questions and comments</span></span>
> 
> <span data-ttu-id="d8745-113">Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın.</span><span class="sxs-lookup"><span data-stu-id="d8745-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="d8745-114">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), veya [Microsoft Azure forumları](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span><span class="sxs-lookup"><span data-stu-id="d8745-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), or the [Microsoft Azure forums](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span></span>


## <a name="table-of-contents"></a><span data-ttu-id="d8745-115">İçindekiler tablosu</span><span class="sxs-lookup"><span data-stu-id="d8745-115">Table of Contents</span></span>

- [<span data-ttu-id="d8745-116">Giriş</span><span class="sxs-lookup"><span data-stu-id="d8745-116">Introduction</span></span>](#introduction)
- [<span data-ttu-id="d8745-117">SignalR Web uygulamasını Azure App Service'e dağıtma</span><span class="sxs-lookup"><span data-stu-id="d8745-117">Deploying a SignalR Web App to Azure App Service</span></span>](#deploying)
- [<span data-ttu-id="d8745-118">Azure App Service'te WebSockets etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="d8745-118">Enabling WebSockets on Azure App Service</span></span>](#websocket)
- [<span data-ttu-id="d8745-119">Azure Redis Cache devre kartına kullanma</span><span class="sxs-lookup"><span data-stu-id="d8745-119">Using the Azure Redis Cache Backplane</span></span>](#backplane)
- [<span data-ttu-id="d8745-120">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="d8745-120">Next Steps</span></span>](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a><span data-ttu-id="d8745-121">Giriş</span><span class="sxs-lookup"><span data-stu-id="d8745-121">Introduction</span></span>

<span data-ttu-id="d8745-122">ASP.NET SignalR, yeni bir düzeye sunucuları ve web veya .NET istemcileri arasındaki etkileşim için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d8745-122">ASP.NET SignalR can be used to bring a new level of interactivity between servers and web or .NET clients.</span></span> <span data-ttu-id="d8745-123">Azure'da barındırılan, SignalR uygulamalarını yüksek oranda kullanılabilir, ölçeklenebilir yararlanabilir ve bulutta çalışan yüksek performanslı ortamı sağlar.</span><span class="sxs-lookup"><span data-stu-id="d8745-123">When hosted in Azure, SignalR applications can take advantage of the highly available, scalable, and performant environment that running in the cloud provides.</span></span>

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a><span data-ttu-id="d8745-124">SignalR Web uygulamasını Azure App Service'e dağıtma</span><span class="sxs-lookup"><span data-stu-id="d8745-124">Deploying a SignalR Web App to Azure App Service</span></span>

<span data-ttu-id="d8745-125">SignalR herhangi belirli bir zorluk, şirket içi bir sunucuya dağıtma ve bir uygulamayı azure'a dağıtmak için eklemez.</span><span class="sxs-lookup"><span data-stu-id="d8745-125">SignalR doesn't add any particular complications to deploying an application to Azure versus deploying to an on-premises server.</span></span> <span data-ttu-id="d8745-126">SignalR kullanan bir uygulama yapılandırma ya da diğer ayarları herhangi bir değişiklik yapmadan Azure'da barındırılabilir (ancak WebSockets desteği için bkz. [etkinleştirme WebSockets Azure App Service'te](#websocket) aşağıda.) Bu öğreticide, oluşturulan uygulamayı dağıtacaksınız [başlangıç Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md) azure'a.</span><span class="sxs-lookup"><span data-stu-id="d8745-126">An application that uses SignalR can be hosted in Azure without any changes in configuration or other settings (though for WebSockets support, see [Enabling WebSockets on Azure App Service](#websocket) below.) For this tutorial, you'll deploy the application created in the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) to Azure.</span></span>

<span data-ttu-id="d8745-127">**Önkoşullar**</span><span class="sxs-lookup"><span data-stu-id="d8745-127">**Prerequisites**</span></span>

- <span data-ttu-id="d8745-128">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="d8745-128">Visual Studio 2013.</span></span> <span data-ttu-id="d8745-129">Visual Studio 2013 Express Web için Visual Studio sahip değilseniz Azure SDK'yı yükleme, dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="d8745-129">If you don't have Visual Studio, Visual Studio 2013 Express for Web is included in the install of the Azure SDK.</span></span>
- <span data-ttu-id="d8745-130">[Visual Studio 2013 için Azure SDK 2.3](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) veya [Visual Studio 2012 için Azure SDK 2.3](https://go.microsoft.com/fwlink/p/?linkid=323511).</span><span class="sxs-lookup"><span data-stu-id="d8745-130">[Azure SDK 2.3 for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) or [Azure SDK 2.3 for Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span></span>
- <span data-ttu-id="d8745-131">Bu öğreticiyi tamamlamak için bir Azure aboneliği gerekir.</span><span class="sxs-lookup"><span data-stu-id="d8745-131">To complete this tutorial, you will need an Azure subscription.</span></span> <span data-ttu-id="d8745-132">Yapabilecekleriniz [MSDN abone Avantajlarınızı etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), veya [bir deneme aboneliği için kaydolun](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d8745-132">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), or [sign up for a trial subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

### <a name="deploying-a-signalr-web-app-to-azure"></a><span data-ttu-id="d8745-133">SignalR web uygulamasını Azure'a dağıtma</span><span class="sxs-lookup"><span data-stu-id="d8745-133">Deploying a SignalR web app to Azure</span></span>

1. <span data-ttu-id="d8745-134">Tamamlamak [başlangıç Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md), veya tamamlanmış projesinden indirmeniz [kod Galerisi](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="d8745-134">Complete the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the finished project from [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
2. <span data-ttu-id="d8745-135">Visual Studio'da **derleme**, **yayımlama SignalR sohbet**.</span><span class="sxs-lookup"><span data-stu-id="d8745-135">In Visual Studio, select **Build**, **Publish SignalR Chat**.</span></span>
3. <span data-ttu-id="d8745-136">"Web'de Yayımla" iletişim kutusunda, "Windows Azure Web siteleri" seçin.</span><span class="sxs-lookup"><span data-stu-id="d8745-136">In the "Publish Web" dialog, select "Windows Azure Web Sites".</span></span>

    ![Azure Web siteleri seçin](using-signalr-with-azure-web-sites/_static/image1.png)
4. <span data-ttu-id="d8745-138">Microsoft hesabınızda oturum açmadıysanız, tıklayın **oturum aç...**  "mevcut Web sitesi seçin" iletişim ve oturum açın.</span><span class="sxs-lookup"><span data-stu-id="d8745-138">If you aren't signed in to your Microsoft account, click **Sign In...** in the "Select Existing Web Site" dialog, and sign in.</span></span>

    ![Mevcut Web sitesini seçin](using-signalr-with-azure-web-sites/_static/image2.png)    ![Azure'da oturum açın](using-signalr-with-azure-web-sites/_static/image3.png)
5. <span data-ttu-id="d8745-141">"Mevcut Web sitesi seçin" iletişim kutusunda tıklatın **yeni**.</span><span class="sxs-lookup"><span data-stu-id="d8745-141">In the "Select Existing Web Site" dialog, click **New**.</span></span>

    ![Yeni Web sitesi](using-signalr-with-azure-web-sites/_static/image4.png)
6. <span data-ttu-id="d8745-143">"Windows azure'da site oluşturma" iletişim kutusunda, benzersiz bir uygulama adı girin.</span><span class="sxs-lookup"><span data-stu-id="d8745-143">In the "Create site on Windows Azure" dialog, enter a unique app name.</span></span> <span data-ttu-id="d8745-144">Bölge açılır menüde size en yakın bölgeyi seçin.</span><span class="sxs-lookup"><span data-stu-id="d8745-144">Select the region closest to you in the Region dropdown.</span></span> <span data-ttu-id="d8745-145">**Oluştur**'u tıklatın.</span><span class="sxs-lookup"><span data-stu-id="d8745-145">Click **Create**.</span></span>

    ![Azure'da site oluşturma](using-signalr-with-azure-web-sites/_static/image5.png)
7. <span data-ttu-id="d8745-147">"Web'de Yayımla" iletişim kutusunda tıklatın **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="d8745-147">In the "Publish Web" dialog, click **Publish**.</span></span>

    ![Site yayımlama](using-signalr-with-azure-web-sites/_static/image6.png)
8. <span data-ttu-id="d8745-149">Uygulama yayımlama tamamlandıktan sonra Azure App Service Web Apps'te barındırılan SignalR sohbet uygulaması tarayıcıda açılır.</span><span class="sxs-lookup"><span data-stu-id="d8745-149">When the app has completed publishing, the SignalR Chat application hosted in Azure App Service Web Apps will open in a browser.</span></span>

    ![Site bir tarayıcıda açma](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a><span data-ttu-id="d8745-151">Azure App Service Web apps'te WebSockets etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="d8745-151">Enabling WebSockets on Azure App Service Web Apps</span></span>

<span data-ttu-id="d8745-152">WebSockets, web uygulamanızı SignalR uygulamada kullanılmak için açıkça etkinleştirilmesi gerekiyor; Aksi takdirde, diğer protokolleri kullanılır (bkz [aktarım ve geri dönüşler](../getting-started/introduction-to-signalr.md#transports) Ayrıntılar için).</span><span class="sxs-lookup"><span data-stu-id="d8745-152">WebSockets needs to be explicitly enabled in your web app to be used in a SignalR application; otherwise, other protocols will be used (See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for details).</span></span>

<span data-ttu-id="d8745-153">Azure App Service Web Apps üzerinde WebSockets kullanmak için web uygulaması yapılandırma bölümünde etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="d8745-153">In order to use WebSockets on Azure App Service Web Apps, enable it in the configuration section of the web app.</span></span> <span data-ttu-id="d8745-154">Bunu yapmak için web uygulamanızı açın [Azure Yönetim Portalı](https://manage.windowsazure.com/), Yapılandır'ı seçin.</span><span class="sxs-lookup"><span data-stu-id="d8745-154">To do this, open your web app in the [Azure Management Portal](https://manage.windowsazure.com/), and select Configure.</span></span>

![Yapılandır sekmesi](using-signalr-with-azure-web-sites/_static/image8.png)

<span data-ttu-id="d8745-156">Yapılandırma sayfasında üst kısmında, .NET 4.5 web uygulamanız için kullanıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="d8745-156">At the top of the configuration page, ensure that .NET 4.5 is used for your web app.</span></span>

![.NET framework 4.5 sürümünü ayarlama](using-signalr-with-azure-web-sites/_static/image9.png)

<span data-ttu-id="d8745-158">Yapılandırma sayfasında, **WebSockets** ayarını seçin **üzerinde**.</span><span class="sxs-lookup"><span data-stu-id="d8745-158">On the configuration page, in the **WebSockets** setting, select **On**.</span></span>

![WebSockets ayarı: üzerinde](using-signalr-with-azure-web-sites/_static/image10.png)

<span data-ttu-id="d8745-160">Yapılandırma sayfasında sonunda seçin **Kaydet** yaptığınız değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="d8745-160">At the bottom of the Configuration page, select **Save** to save your changes.</span></span>

![Ayarları Kaydet](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a><span data-ttu-id="d8745-162">Azure Redis Cache devre kartına kullanma</span><span class="sxs-lookup"><span data-stu-id="d8745-162">Using the Azure Redis Cache Backplane</span></span>

<span data-ttu-id="d8745-163">Web uygulamanız için birden çok örneği kullanın ve bu örneklere kullanıcılarının (örneğin, bir örneğinde oluşturulan sohbet iletileri diğer örneklerine bağlı olan kullanıcıların erişebilmesi için) birbiriyle etkileşim gerekiyor, [Azure Redis Cache devre kartına](../performance/scaleout-with-redis.md) uygulamanızda uygulanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d8745-163">If you use multiple instances for your web app, and the users of those instances need to interact with one another (so that, for instance, chat messages created in one instance can reach the users connected to other instances), the [Azure Redis Cache backplane](../performance/scaleout-with-redis.md) must be implemented in your application.</span></span>

<a id="nextsteps"></a>
## <a name="next-steps"></a><span data-ttu-id="d8745-164">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="d8745-164">Next Steps</span></span>

<span data-ttu-id="d8745-165">Azure App service'taki Web Apps hakkında daha fazla bilgi için bkz. [Web Apps'e genel bakış](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span><span class="sxs-lookup"><span data-stu-id="d8745-165">For more information on Web Apps in Azure App Service, see [Web Apps overview](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span></span>
