---
title: ASP.NET çekirdeği Modülü
author: tdykstra
description: ASP.NET çekirdeği modülü Kestrel web sunucusuna IIS veya IIS Express ters proxy sunucusu olarak kullanmak nasıl olanak tanıdığını öğrenin.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 4e842544f861c3704ba7798fa693b36435d54731
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2018
---
# <a name="aspnet-core-module"></a><span data-ttu-id="30bfe-103">ASP.NET çekirdeği Modülü</span><span class="sxs-lookup"><span data-stu-id="30bfe-103">ASP.NET Core Module</span></span>

<span data-ttu-id="30bfe-104">Tarafından [zel Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), ve [Chris fillerin](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="30bfe-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span> 

<span data-ttu-id="30bfe-105">ASP.NET çekirdeği modülü ASP.NET Core uygulamaları IIS bir ters proxy yapılandırması çalıştırılacak şekilde sağlar.</span><span class="sxs-lookup"><span data-stu-id="30bfe-105">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="30bfe-106">IIS, Gelişmiş web uygulaması güvenlik ve yönetilebilirlik özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="30bfe-106">IIS provides advanced web app security and manageability features.</span></span>

<span data-ttu-id="30bfe-107">Desteklenen Windows sürümlerine:</span><span class="sxs-lookup"><span data-stu-id="30bfe-107">Supported Windows versions:</span></span>

* <span data-ttu-id="30bfe-108">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="30bfe-108">Windows 7 or later</span></span>
* <span data-ttu-id="30bfe-109">Windows Server 2008 R2 veya sonraki sürümü</span><span class="sxs-lookup"><span data-stu-id="30bfe-109">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="30bfe-110">ASP.NET çekirdeği modülü yalnızca Kestrel ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="30bfe-110">The ASP.NET Core Module only works with Kestrel.</span></span> <span data-ttu-id="30bfe-111">Modül uyumlu değil. [HTTP.sys](xref:fundamentals/servers/httpsys) (eski adıysa [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="30bfe-111">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

## <a name="aspnet-core-module-description"></a><span data-ttu-id="30bfe-112">ASP.NET Core modül açıklaması</span><span class="sxs-lookup"><span data-stu-id="30bfe-112">ASP.NET Core Module description</span></span>

<span data-ttu-id="30bfe-113">ASP.NET çekirdeği modülü web istekleri arka ucuna yönlendirmek için IIS ardışık düzen halinde ASP.NET Core uygulamaları takılan yerel bir IIS modüldür.</span><span class="sxs-lookup"><span data-stu-id="30bfe-113">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to redirect web requests to backend ASP.NET Core apps.</span></span> <span data-ttu-id="30bfe-114">Windows kimlik doğrulaması gibi birçok yerel modül etkin kalır.</span><span class="sxs-lookup"><span data-stu-id="30bfe-114">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="30bfe-115">IIS modülleri etkin modülüyle hakkında daha fazla bilgi için bkz: [IIS modüllerini](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="30bfe-115">To learn more about IIS modules active with the module, see [IIS modules](xref:host-and-deploy/iis/modules).</span></span>

<span data-ttu-id="30bfe-116">ASP.NET Core uygulamaları bir işlem olarak çalıştırmak için IIS çalışan işlemini ayrı olduğundan, modül işlem yönetimi da işler.</span><span class="sxs-lookup"><span data-stu-id="30bfe-116">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="30bfe-117">İlk istek ulaştığında ve onu çökerse uygulama yeniden başlatmalarını modülü ASP.NET Core uygulama işlemini başlatır.</span><span class="sxs-lookup"><span data-stu-id="30bfe-117">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="30bfe-118">Bu temelde aynı işlem içinde çalıştırma ASP.NET 4.x uygulamalarla görüldüğü gibi davranıştır tarafından yönetilen IIS'de [Windows İşlem Etkinleştirme Hizmeti (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="30bfe-118">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="30bfe-119">Aşağıdaki diyagram, IIS, ASP.NET Core modülü ve ASP.NET Core uygulamaları arasındaki ilişkiyi göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="30bfe-119">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and ASP.NET Core apps:</span></span>

![ASP.NET çekirdeği Modülü](aspnet-core-module/_static/ancm.png)

<span data-ttu-id="30bfe-121">İstekleri için çekirdek modu HTTP.sys sürücüsünü Web'den ulaşır.</span><span class="sxs-lookup"><span data-stu-id="30bfe-121">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="30bfe-122">Sürücü istekleri IIS Web sitesinin yapılandırılan bağlantı noktası, genellikle 80 (HTTP) veya 443 (HTTPS) üzerinde yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="30bfe-122">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="30bfe-123">Modül Kestrel bağlantı noktası değil uygulama için rastgele bir bağlantı noktası isteklerini iletir 80/443'tür.</span><span class="sxs-lookup"><span data-stu-id="30bfe-123">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="30bfe-124">Modülü başlatma sırasında bir ortam değişkeni aracılığıyla bağlantı noktasını belirtir ve IIS tümleştirme Ara sunucu üzerinde dinleme yapılandırır `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="30bfe-124">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="30bfe-125">Ek denetimleri yapılır ve modülünden kökenli olmayan istekler reddedilir.</span><span class="sxs-lookup"><span data-stu-id="30bfe-125">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="30bfe-126">İstekleri, IIS tarafından HTTPS üzerinde alınan olsa bile HTTP üzerinden iletilir böylece modülü HTTPS iletme desteklemiyor.</span><span class="sxs-lookup"><span data-stu-id="30bfe-126">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="30bfe-127">Modül isteğinden Kestrel seçer sonra isteği ASP.NET Core ara yazılım ardışık düzenine gönderilir.</span><span class="sxs-lookup"><span data-stu-id="30bfe-127">After Kestrel picks up a request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="30bfe-128">Ara yazılım ardışık düzenini isteği işler ve olarak geçirir bir `HttpContext` örnek uygulamanın mantığı.</span><span class="sxs-lookup"><span data-stu-id="30bfe-128">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="30bfe-129">Uygulamanın yanıt geri IIS, geri istek başlatılan HTTP istemcisi hangi iter geçirilir.</span><span class="sxs-lookup"><span data-stu-id="30bfe-129">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="30bfe-130">ASP.NET çekirdeği modülü birkaç diğer işlevleri vardır.</span><span class="sxs-lookup"><span data-stu-id="30bfe-130">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="30bfe-131">Modül yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="30bfe-131">The module can:</span></span>

* <span data-ttu-id="30bfe-132">Çalışan işlemi için ortam değişkenleri ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="30bfe-132">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="30bfe-133">Stdout çıkış başlatma sorunlarını gidermek için dosya depolama için oturum açın.</span><span class="sxs-lookup"><span data-stu-id="30bfe-133">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="30bfe-134">Windows kimlik doğrulama belirteçleri iletin.</span><span class="sxs-lookup"><span data-stu-id="30bfe-134">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="30bfe-135">Yükleme ve ASP.NET Core modülü kullanın</span><span class="sxs-lookup"><span data-stu-id="30bfe-135">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="30bfe-136">Yükleme ve ASP.NET Core Modülü'nü kullanma konusunda ayrıntılı yönergeler için bkz: [IIS ile Windows konakta](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="30bfe-136">For detailed instructions on how to install and use the ASP.NET Core Module, see [Host on Windows with IIS](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="30bfe-137">Modül yapılandırma hakkında daha fazla bilgi için bkz: [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="30bfe-137">For information on configuring the module, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="30bfe-138">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="30bfe-138">Additional resources</span></span>

* [<span data-ttu-id="30bfe-139">IIS ile Windows’da barındırma</span><span class="sxs-lookup"><span data-stu-id="30bfe-139">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="30bfe-140">ASP.NET Core Module yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="30bfe-140">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="30bfe-141">ASP.NET çekirdeği modülü GitHub deposunu (kaynak kodu)</span><span class="sxs-lookup"><span data-stu-id="30bfe-141">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
