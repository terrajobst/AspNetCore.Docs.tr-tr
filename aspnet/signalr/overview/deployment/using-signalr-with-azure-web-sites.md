---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Azure App Service'te Web uygulamalarını SignalR kullanarak | Microsoft Docs
author: pfletcher
description: Bu belgede, Microsoft Azure üzerinde çalışan bir SignalR uygulamasının nasıl yapılandırılacağını açıklar. Yazılım sürümleri, Visual Studio 2013 veya Vis. öğreticide kullanılan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 8386441690a3fb479ffb941ebd7c0b2f83870781
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28043213"
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a><span data-ttu-id="ad104-104">Azure App Service'te Web uygulamalarını SignalR kullanma</span><span class="sxs-lookup"><span data-stu-id="ad104-104">Using SignalR with Web Apps in Azure App Service</span></span>
====================
<span data-ttu-id="ad104-105">tarafından [CAN Fletcher'dan](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="ad104-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="ad104-106">Bu belgede, Microsoft Azure üzerinde çalışan bir SignalR uygulamasının nasıl yapılandırılacağını açıklar.</span><span class="sxs-lookup"><span data-stu-id="ad104-106">This document describes how to configure a SignalR application that runs on Microsoft Azure.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ad104-107">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="ad104-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ad104-108">[Visual Studio 2013'ün](https://www.microsoft.com/visualstudio/eng/2013-downloads) veya Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="ad104-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) or Visual Studio 2012</span></span>
> - <span data-ttu-id="ad104-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="ad104-109">.NET 4.5</span></span>
> - <span data-ttu-id="ad104-110">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="ad104-110">SignalR version 2</span></span>
> - <span data-ttu-id="ad104-111">Visual Studio 2013 veya 2012 için Azure SDK 2.3</span><span class="sxs-lookup"><span data-stu-id="ad104-111">Azure SDK 2.3 for Visual Studio 2013 or 2012</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="ad104-112">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="ad104-112">Questions and comments</span></span>
> 
> <span data-ttu-id="ad104-113">Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi.</span><span class="sxs-lookup"><span data-stu-id="ad104-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="ad104-114">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), veya [Microsoft Azure forumları](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span><span class="sxs-lookup"><span data-stu-id="ad104-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), or the [Microsoft Azure forums](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span></span>


## <a name="table-of-contents"></a><span data-ttu-id="ad104-115">İçindekiler tablosu</span><span class="sxs-lookup"><span data-stu-id="ad104-115">Table of Contents</span></span>

- [<span data-ttu-id="ad104-116">Giriş</span><span class="sxs-lookup"><span data-stu-id="ad104-116">Introduction</span></span>](#introduction)
- [<span data-ttu-id="ad104-117">Bir SignalR Web uygulamasını Azure App Service'e dağıtma</span><span class="sxs-lookup"><span data-stu-id="ad104-117">Deploying a SignalR Web App to Azure App Service</span></span>](#deploying)
- [<span data-ttu-id="ad104-118">Azure uygulama Hizmeti'nde WebSockets etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="ad104-118">Enabling WebSockets on Azure App Service</span></span>](#websocket)
- [<span data-ttu-id="ad104-119">Azure Redis önbelleği devre kartı kullanma</span><span class="sxs-lookup"><span data-stu-id="ad104-119">Using the Azure Redis Cache Backplane</span></span>](#backplane)
- [<span data-ttu-id="ad104-120">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="ad104-120">Next Steps</span></span>](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a><span data-ttu-id="ad104-121">Giriş</span><span class="sxs-lookup"><span data-stu-id="ad104-121">Introduction</span></span>

<span data-ttu-id="ad104-122">ASP.NET SignalR, sunucuları ve web veya .NET istemcileri arasındaki etkileşim yeni bir düzeyine getirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ad104-122">ASP.NET SignalR can be used to bring a new level of interactivity between servers and web or .NET clients.</span></span> <span data-ttu-id="ad104-123">Azure üzerinde barındırılan, SignalR uygulamalarını yüksek oranda kullanılabilir, ölçeklenebilir yararlanabilir ve bulutta çalışan kullanıcı ortamı sağlar.</span><span class="sxs-lookup"><span data-stu-id="ad104-123">When hosted in Azure, SignalR applications can take advantage of the highly available, scalable, and performant environment that running in the cloud provides.</span></span>

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a><span data-ttu-id="ad104-124">Bir SignalR Web uygulamasını Azure App Service'e dağıtma</span><span class="sxs-lookup"><span data-stu-id="ad104-124">Deploying a SignalR Web App to Azure App Service</span></span>

<span data-ttu-id="ad104-125">SignalR, bir şirket içi sunucusuna dağıtma karşı Azure bir uygulamayı dağıtmak için belirli bir zorluk eklemez.</span><span class="sxs-lookup"><span data-stu-id="ad104-125">SignalR doesn't add any particular complications to deploying an application to Azure versus deploying to an on-premises server.</span></span> <span data-ttu-id="ad104-126">SignalR kullanan bir uygulama yapılandırma ya da diğer ayarları herhangi bir değişiklik yapılmadan Azure'da barındırılabilir (ancak WebSockets desteği için bkz: [etkinleştirme WebSockets Azure App Service'te](#websocket) aşağıda.) Bu öğretici için oluşturduğunuz uygulama dağıtacaksınız [başlangıç Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md) Azure.</span><span class="sxs-lookup"><span data-stu-id="ad104-126">An application that uses SignalR can be hosted in Azure without any changes in configuration or other settings (though for WebSockets support, see [Enabling WebSockets on Azure App Service](#websocket) below.) For this tutorial, you'll deploy the application created in the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) to Azure.</span></span>

<span data-ttu-id="ad104-127">**Önkoşullar**</span><span class="sxs-lookup"><span data-stu-id="ad104-127">**Prerequisites**</span></span>

- <span data-ttu-id="ad104-128">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="ad104-128">Visual Studio 2013.</span></span> <span data-ttu-id="ad104-129">Visual Studio yoksa, Visual Studio 2013 Express Web için Azure SDK'sını yükleme dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="ad104-129">If you don't have Visual Studio, Visual Studio 2013 Express for Web is included in the install of the Azure SDK.</span></span>
- <span data-ttu-id="ad104-130">[Visual Studio 2013 için Azure SDK 2.3](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) veya [Visual Studio 2012 için Azure SDK 2.3](https://go.microsoft.com/fwlink/p/?linkid=323511).</span><span class="sxs-lookup"><span data-stu-id="ad104-130">[Azure SDK 2.3 for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) or [Azure SDK 2.3 for Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span></span>
- <span data-ttu-id="ad104-131">Bu öğreticiyi tamamlamak için bir Azure aboneliği gerekir.</span><span class="sxs-lookup"><span data-stu-id="ad104-131">To complete this tutorial, you will need an Azure subscription.</span></span> <span data-ttu-id="ad104-132">Yapabilecekleriniz [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), veya [deneme aboneliği için kaydolun](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ad104-132">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), or [sign up for a trial subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

### <a name="deploying-a-signalr-web-app-to-azure"></a><span data-ttu-id="ad104-133">Bir SignalR web uygulaması Azure'a dağıtma</span><span class="sxs-lookup"><span data-stu-id="ad104-133">Deploying a SignalR web app to Azure</span></span>

1. <span data-ttu-id="ad104-134">Tamamlamak [başlangıç Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md), veya tamamlanmış projeden indirme [kod Galerisi'nden](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="ad104-134">Complete the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the finished project from [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
2. <span data-ttu-id="ad104-135">Visual Studio'da seçin **yapı**, **yayımlama SignalR sohbet**.</span><span class="sxs-lookup"><span data-stu-id="ad104-135">In Visual Studio, select **Build**, **Publish SignalR Chat**.</span></span>
3. <span data-ttu-id="ad104-136">"Web'de Yayımla" iletişim kutusunda, "Windows Azure Web siteleri" seçin.</span><span class="sxs-lookup"><span data-stu-id="ad104-136">In the "Publish Web" dialog, select "Windows Azure Web Sites".</span></span>

    ![Azure Web sitelerini seçin](using-signalr-with-azure-web-sites/_static/image1.png)
4. <span data-ttu-id="ad104-138">Microsoft hesabınıza imzalanmadığını tıklatmak **oturum aç...**  "varolan Web sitesi seçin" iletişim kutusu ve oturum açın.</span><span class="sxs-lookup"><span data-stu-id="ad104-138">If you aren't signed in to your Microsoft account, click **Sign In...** in the "Select Existing Web Site" dialog, and sign in.</span></span>

    ![Varolan bir Web sitesini seçin](using-signalr-with-azure-web-sites/_static/image2.png)    ![Azure'da oturum aç](using-signalr-with-azure-web-sites/_static/image3.png)
5. <span data-ttu-id="ad104-141">"Var olan Web sitesi seçin" iletişim kutusundan tıklatın **yeni**.</span><span class="sxs-lookup"><span data-stu-id="ad104-141">In the "Select Existing Web Site" dialog, click **New**.</span></span>

    ![Yeni Web sitesi](using-signalr-with-azure-web-sites/_static/image4.png)
6. <span data-ttu-id="ad104-143">"Windows azure'da site oluşturma" iletişim kutusunda, bir benzersiz uygulama adı girin.</span><span class="sxs-lookup"><span data-stu-id="ad104-143">In the "Create site on Windows Azure" dialog, enter a unique app name.</span></span> <span data-ttu-id="ad104-144">Size en yakın bölgeyi bölge açılır menüde seçin.</span><span class="sxs-lookup"><span data-stu-id="ad104-144">Select the region closest to you in the Region dropdown.</span></span> <span data-ttu-id="ad104-145">**Oluştur**'u tıklatın.</span><span class="sxs-lookup"><span data-stu-id="ad104-145">Click **Create**.</span></span>

    ![Azure'da site oluşturma](using-signalr-with-azure-web-sites/_static/image5.png)
7. <span data-ttu-id="ad104-147">"Web'de Yayımla" iletişim kutusundan tıklatın **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="ad104-147">In the "Publish Web" dialog, click **Publish**.</span></span>

    ![Site yayımlama](using-signalr-with-azure-web-sites/_static/image6.png)
8. <span data-ttu-id="ad104-149">Uygulama yayımlama tamamlandığında, Azure App Service Web Apps içinde barındırılan SignalR sohbet uygulaması bir tarayıcıda açın.</span><span class="sxs-lookup"><span data-stu-id="ad104-149">When the app has completed publishing, the SignalR Chat application hosted in Azure App Service Web Apps will open in a browser.</span></span>

    ![Bir tarayıcıda açmadan site](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a><span data-ttu-id="ad104-151">Azure uygulama hizmeti Web uygulamalarını WebSockets etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="ad104-151">Enabling WebSockets on Azure App Service Web Apps</span></span>

<span data-ttu-id="ad104-152">WebSockets, web uygulamanızı SignalR uygulamada kullanılacak açıkça etkinleştirilmesi gerekir; Aksi takdirde, diğer protokolleri kullanılır (bkz [aktarımları ve geri dönüşler](../getting-started/introduction-to-signalr.md#transports) Ayrıntılar için).</span><span class="sxs-lookup"><span data-stu-id="ad104-152">WebSockets needs to be explicitly enabled in your web app to be used in a SignalR application; otherwise, other protocols will be used (See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for details).</span></span>

<span data-ttu-id="ad104-153">Azure App Service Web Apps'de WebSockets kullanmak için web uygulamasının yapılandırma bölümünde etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="ad104-153">In order to use WebSockets on Azure App Service Web Apps, enable it in the configuration section of the web app.</span></span> <span data-ttu-id="ad104-154">Bunu yapmak için web uygulamanızı açın [Azure Yönetim Portalı](https://manage.windowsazure.com/), yapılandırma seçin.</span><span class="sxs-lookup"><span data-stu-id="ad104-154">To do this, open your web app in the [Azure Management Portal](https://manage.windowsazure.com/), and select Configure.</span></span>

![Yapılandır sekmesi](using-signalr-with-azure-web-sites/_static/image8.png)

<span data-ttu-id="ad104-156">Yapılandırma sayfanın en üstünde, .NET 4.5 web uygulamanız için kullanıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="ad104-156">At the top of the configuration page, ensure that .NET 4.5 is used for your web app.</span></span>

![.NET framework sürüm 4.5 ayarı](using-signalr-with-azure-web-sites/_static/image9.png)

<span data-ttu-id="ad104-158">Yapılandırma sayfasında, **WebSockets** ayarını seçin **üzerinde**.</span><span class="sxs-lookup"><span data-stu-id="ad104-158">On the configuration page, in the **WebSockets** setting, select **On**.</span></span>

![WebSockets ayarı: hakkında](using-signalr-with-azure-web-sites/_static/image10.png)

<span data-ttu-id="ad104-160">Yapılandırma sayfasının en altında seçin **kaydetmek** yaptığınız değişiklikleri kaydetmek için.</span><span class="sxs-lookup"><span data-stu-id="ad104-160">At the bottom of the Configuration page, select **Save** to save your changes.</span></span>

![Ayarları Kaydet](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a><span data-ttu-id="ad104-162">Azure Redis önbelleği devre kartı kullanma</span><span class="sxs-lookup"><span data-stu-id="ad104-162">Using the Azure Redis Cache Backplane</span></span>

<span data-ttu-id="ad104-163">Web uygulamanız için birden çok örneği kullanın ve kullanıcıların bu örnekleri (örneğin, bir örneğinde oluşturulan sohbet iletiler diğer örneklerine bağlı kullanıcıları erişebilmesi için) birbiriyle etkileşim gerekiyorsa, [Azure Redis önbelleği devre kartına](../performance/scaleout-with-redis.md) uygulamanızda uygulanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ad104-163">If you use multiple instances for your web app, and the users of those instances need to interact with one another (so that, for instance, chat messages created in one instance can reach the users connected to other instances), the [Azure Redis Cache backplane](../performance/scaleout-with-redis.md) must be implemented in your application.</span></span>

<a id="nextsteps"></a>
## <a name="next-steps"></a><span data-ttu-id="ad104-164">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="ad104-164">Next Steps</span></span>

<span data-ttu-id="ad104-165">Azure App Service'te Web Apps hakkında daha fazla bilgi için bkz: [Web Apps'e genel bakış](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span><span class="sxs-lookup"><span data-stu-id="ad104-165">For more information on Web Apps in Azure App Service, see [Web Apps overview](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span></span>
