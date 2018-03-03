---
title: "ASP.NET çekirdeği Modülü"
author: tdykstra
description: "ASP.NET çekirdeği modülü Kestrel web sunucusuna IIS veya IIS Express ters proxy sunucusu olarak kullanmak nasıl olanak tanıdığını öğrenin."
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: e2170014f1a8fc89ec7e0a02d19c943b88e005fb
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-core-module"></a><span data-ttu-id="68528-103">ASP.NET çekirdeği Modülü</span><span class="sxs-lookup"><span data-stu-id="68528-103">ASP.NET Core Module</span></span>

<span data-ttu-id="68528-104">Tarafından [zel Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), ve [Chris fillerin](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="68528-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span> 

<span data-ttu-id="68528-105">ASP.NET çekirdeği modülü ASP.NET Core uygulamaları IIS bir ters proxy yapılandırması çalıştırılacak şekilde sağlar.</span><span class="sxs-lookup"><span data-stu-id="68528-105">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="68528-106">IIS, Gelişmiş web uygulaması güvenlik ve yönetilebilirlik özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="68528-106">IIS provides advanced web app security and manageability features.</span></span>

<span data-ttu-id="68528-107">Desteklenen Windows sürümlerine:</span><span class="sxs-lookup"><span data-stu-id="68528-107">Supported Windows versions:</span></span>

* <span data-ttu-id="68528-108">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="68528-108">Windows 7 or later</span></span>
* <span data-ttu-id="68528-109">Windows Server 2008 R2 veya daha sonra &#8224;</span><span class="sxs-lookup"><span data-stu-id="68528-109">Windows Server 2008 R2 or later&#8224;</span></span>

<span data-ttu-id="68528-110">&#8224; Kavramsal olarak, bu belgede açıklanan IIS ASP.NET Core modülüyle kullanımını da Nano Server IIS üzerinde ASP.NET Core uygulamaları barındırmak için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="68528-110">&#8224;Conceptually, the use of the ASP.NET Core Module with IIS described in this document also applies to hosting ASP.NET Core apps on Nano Server IIS.</span></span> <span data-ttu-id="68528-111">Nano Server için özel yönergeler için bkz: [Nano Server IIS ile ASP.NET Core](xref:tutorials/nano-server) Öğreticisi.</span><span class="sxs-lookup"><span data-stu-id="68528-111">For instructions specific to Nano Server, see the [ASP.NET Core with IIS on Nano Server](xref:tutorials/nano-server) tutorial.</span></span>

<span data-ttu-id="68528-112">ASP.NET çekirdeği modülü yalnızca Kestrel ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="68528-112">The ASP.NET Core Module only works with Kestrel.</span></span> <span data-ttu-id="68528-113">Modül uyumlu değil. [HTTP.sys](xref:fundamentals/servers/httpsys) (eski adıysa [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="68528-113">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

## <a name="aspnet-core-module-description"></a><span data-ttu-id="68528-114">ASP.NET Core modül açıklaması</span><span class="sxs-lookup"><span data-stu-id="68528-114">ASP.NET Core Module description</span></span>

<span data-ttu-id="68528-115">ASP.NET çekirdeği modülü web istekleri arka ucuna yönlendirmek için IIS ardışık düzen halinde ASP.NET Core uygulamaları takılan yerel bir IIS modüldür.</span><span class="sxs-lookup"><span data-stu-id="68528-115">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to redirect web requests to backend ASP.NET Core apps.</span></span> <span data-ttu-id="68528-116">Windows kimlik doğrulaması gibi birçok yerel modül etkin kalır.</span><span class="sxs-lookup"><span data-stu-id="68528-116">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="68528-117">IIS modülleri etkin modülüyle hakkında daha fazla bilgi için bkz: [kullanarak IIS modüllerini](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="68528-117">To learn more about IIS modules active with the module, see [Using IIS modules](xref:host-and-deploy/iis/modules).</span></span>

<span data-ttu-id="68528-118">ASP.NET Core uygulamaları bir işlem olarak çalıştırmak için IIS çalışan işlemini ayrı olduğundan, modül işlem yönetimi da işler.</span><span class="sxs-lookup"><span data-stu-id="68528-118">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="68528-119">İlk istek ulaştığında ve onu çökerse uygulama yeniden başlatmalarını modülü ASP.NET Core uygulama işlemini başlatır.</span><span class="sxs-lookup"><span data-stu-id="68528-119">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="68528-120">Bu temelde aynı işlem içinde çalıştırma ASP.NET 4.x uygulamalarla görüldüğü gibi davranıştır tarafından yönetilen IIS'de [Windows İşlem Etkinleştirme Hizmeti (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="68528-120">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="68528-121">Aşağıdaki diyagram, IIS, ASP.NET Core modülü ve ASP.NET Core uygulamaları arasındaki ilişkiyi göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="68528-121">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and ASP.NET Core apps:</span></span>

![ASP.NET çekirdeği Modülü](aspnet-core-module/_static/ancm.png)

<span data-ttu-id="68528-123">İstekleri için çekirdek modu HTTP.sys sürücüsünü Web'den ulaşır.</span><span class="sxs-lookup"><span data-stu-id="68528-123">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="68528-124">Sürücü istekleri IIS Web sitesinin yapılandırılan bağlantı noktası, genellikle 80 (HTTP) veya 443 (HTTPS) üzerinde yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="68528-124">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="68528-125">Modül Kestrel bağlantı noktası değil uygulama için rastgele bir bağlantı noktası isteklerini iletir 80/443'tür.</span><span class="sxs-lookup"><span data-stu-id="68528-125">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="68528-126">Modülü başlatma sırasında bir ortam değişkeni aracılığıyla bağlantı noktasını belirtir ve IIS tümleştirme Ara sunucu üzerinde dinleme yapılandırır `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="68528-126">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="68528-127">Ek denetimleri yapılır ve modülünden kökenli olmayan istekler reddedilir.</span><span class="sxs-lookup"><span data-stu-id="68528-127">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="68528-128">İstekleri, IIS tarafından HTTPS üzerinde alınan olsa bile HTTP üzerinden iletilir böylece modülü HTTPS iletme desteklemiyor.</span><span class="sxs-lookup"><span data-stu-id="68528-128">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="68528-129">Modül isteğinden Kestrel seçer sonra isteği ASP.NET Core ara yazılım ardışık düzenine gönderilir.</span><span class="sxs-lookup"><span data-stu-id="68528-129">After Kestrel picks up a request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="68528-130">Ara yazılım ardışık düzenini isteği işler ve olarak geçirir bir `HttpContext` örnek uygulamanın mantığı.</span><span class="sxs-lookup"><span data-stu-id="68528-130">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="68528-131">Uygulamanın yanıt geri IIS, geri istek başlatılan HTTP istemcisi hangi iter geçirilir.</span><span class="sxs-lookup"><span data-stu-id="68528-131">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="68528-132">ASP.NET çekirdeği modülü birkaç diğer işlevleri vardır.</span><span class="sxs-lookup"><span data-stu-id="68528-132">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="68528-133">Modül yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="68528-133">The module can:</span></span>

* <span data-ttu-id="68528-134">Çalışan işlemi için ortam değişkenleri ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="68528-134">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="68528-135">Günlük `stdout` başlatma sorunlarını gidermek için dosya depolama alanına çıktı.</span><span class="sxs-lookup"><span data-stu-id="68528-135">Log `stdout` output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="68528-136">Windows kimlik doğrulama belirteçleri iletin.</span><span class="sxs-lookup"><span data-stu-id="68528-136">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="68528-137">Yükleme ve ASP.NET Core modülü kullanın</span><span class="sxs-lookup"><span data-stu-id="68528-137">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="68528-138">Yükleme ve ASP.NET Core Modülü'nü kullanma konusunda ayrıntılı yönergeler için bkz: [IIS ile Windows konakta](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="68528-138">For detailed instructions on how to install and use the ASP.NET Core Module, see [Host on Windows with IIS](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="68528-139">Modül yapılandırma hakkında daha fazla bilgi için bkz: [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="68528-139">For information on configuring the module, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="68528-140">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="68528-140">Additional resources</span></span>

* [<span data-ttu-id="68528-141">IIS ile Windows’da barındırma</span><span class="sxs-lookup"><span data-stu-id="68528-141">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="68528-142">ASP.NET Core Module yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="68528-142">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="68528-143">ASP.NET çekirdeği modülü GitHub deposunu (kaynak kodu)</span><span class="sxs-lookup"><span data-stu-id="68528-143">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
