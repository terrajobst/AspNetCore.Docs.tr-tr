---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: Sürüm 2 SignalR 1.x projeleri yükseltme | Microsoft Docs
author: pfletcher
description: Bu konu, SignalR için mevcut bir SignalR 1.x projesini yükseltileceğini açıklar 2.x ve yükseltme işlemi sırasında ortaya çıkabilecek sorunları gidermek nasıl...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: e372275ae5dd4bbf354db2d02e4407f8c513b7a3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566028"
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a><span data-ttu-id="91131-103">Sürüm 2 SignalR 1.x projelerini yükseltme</span><span class="sxs-lookup"><span data-stu-id="91131-103">Upgrading SignalR 1.x Projects to version 2</span></span>
====================
<span data-ttu-id="91131-104">tarafından [CAN Fletcher'dan](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="91131-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="91131-105">Bu konu, SignalR için mevcut bir SignalR 1.x projesini yükseltileceğini açıklar 2.x ve yükseltme işlemi sırasında ortaya çıkabilecek sorunları nasıl giderilir.</span><span class="sxs-lookup"><span data-stu-id="91131-105">This topic describes how to upgrade an existing SignalR 1.x project to SignalR 2.x, and how to troubleshoot issues that may arise during the upgrade process.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="91131-106">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="91131-106">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="91131-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="91131-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="91131-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="91131-108">.NET 4.5</span></span>
> - <span data-ttu-id="91131-109">SignalR sürüm 1 ve 2</span><span class="sxs-lookup"><span data-stu-id="91131-109">SignalR versions 1 and 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="91131-110">Bu öğretici ile Visual Studio 2012 kullanma</span><span class="sxs-lookup"><span data-stu-id="91131-110">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="91131-111">Visual Studio 2012 bu öğreticiyle kullanmak için aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="91131-111">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="91131-112">Güncelleştirme, [Paket Yöneticisi](http://docs.nuget.org/docs/start-here/installing-nuget) en son sürüme.</span><span class="sxs-lookup"><span data-stu-id="91131-112">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="91131-113">Yükleme [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="91131-113">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="91131-114">Web Platformu Yükleyicisi'nde arayın ve yükleyin **ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için**.</span><span class="sxs-lookup"><span data-stu-id="91131-114">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="91131-115">Bu SignalR sınıfları için Visual Studio şablonları gibi yükleyecek **Hub**.</span><span class="sxs-lookup"><span data-stu-id="91131-115">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="91131-116">Bazı şablonlar (gibi **OWIN başlangıç sınıfı**); kullanılamaz bunlar için bunun yerine bir sınıf dosyası kullanın.</span><span class="sxs-lookup"><span data-stu-id="91131-116">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="91131-117">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="91131-117">Questions and comments</span></span>
> 
> <span data-ttu-id="91131-118">Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi.</span><span class="sxs-lookup"><span data-stu-id="91131-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="91131-119">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="91131-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="91131-120">SignalR 2 kullanarak sunucu platformlar genelinde tutarlı geliştirme deneyimi sunar [OWIN](http://owin.org).</span><span class="sxs-lookup"><span data-stu-id="91131-120">SignalR 2 offers a consistent development experience across server platforms using [OWIN](http://owin.org).</span></span> <span data-ttu-id="91131-121">Bu makalede, sürüm 2 SignalR 1.x uygulamaya güncelleştirmek için gereken adımlarda açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="91131-121">This article describes the few steps that are needed to update a SignalR 1.x application to version 2.</span></span>

<span data-ttu-id="91131-122">SignalR 2 uygulamalarını yükseltmek için teşvik karşın SignalR 1.x hala desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="91131-122">While it is encouraged to upgrade applications to SignalR 2, SignalR 1.x will still be supported.</span></span>

<span data-ttu-id="91131-123">Bu öğretici, SignalR 2 web barındırılan bir uygulamaya yükseltmek açıklar.</span><span class="sxs-lookup"><span data-stu-id="91131-123">This tutorial describes how to upgrade a web-hosted application to SignalR 2.</span></span> <span data-ttu-id="91131-124">Kendini barındıran uygulamaları (Bu ana bilgisayar sunucusunda bir konsol uygulaması, Windows hizmeti veya başka bir işlemin) altında SignalR 2 artık desteklenmektedir.</span><span class="sxs-lookup"><span data-stu-id="91131-124">Self-hosted applications (those that host a server in a console application, Windows service, or other process) are now supported under SignalR 2.</span></span> <span data-ttu-id="91131-125">SignalR 2 ile kendini barındıran bir uygulama oluşturmaya başlamak hakkında daha fazla bilgi için bkz: [Öğreticisi: SignalR kendini barındırma](../deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="91131-125">For information on how to get started creating a self-hosted application with SignalR 2, see [Tutorial: SignalR Self-Host](../deployment/tutorial-signalr-self-host.md).</span></span>

## <a name="contents"></a><span data-ttu-id="91131-126">İçindekiler</span><span class="sxs-lookup"><span data-stu-id="91131-126">Contents</span></span>

<span data-ttu-id="91131-127">Aşağıdaki bölümlerde SignalR projeleri ve ortaya çıkabilecek sorunları gidermek nasıl yükseltme işlemiyle ilgili görevleri açıklar.</span><span class="sxs-lookup"><span data-stu-id="91131-127">The following sections describe tasks involved with upgrading SignalR projects, and how to troubleshoot issues that may arise.</span></span>

- [<span data-ttu-id="91131-128">Örnek: Başlarken Öğreticisi SignalR 2'ye yükseltme</span><span class="sxs-lookup"><span data-stu-id="91131-128">Example: Upgrading the Getting Started tutorial to SignalR 2</span></span>](#example)
- [<span data-ttu-id="91131-129">Yükseltme sırasında karşılaşılan sorun giderme</span><span class="sxs-lookup"><span data-stu-id="91131-129">Troubleshooting errors encountered during upgrading</span></span>](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a><span data-ttu-id="91131-130">Örnek: SignalR 2 Başlarken Öğreticisi uygulama yükseltme</span><span class="sxs-lookup"><span data-stu-id="91131-130">Example: Upgrading the Getting Started tutorial application to SignalR 2</span></span>

<span data-ttu-id="91131-131">Bu bölümde, oluşturduğunuz uygulama güncelleştireceğim [SignalR 1.x başlangıç Öğreticisi sürümü](../older-versions/index.md) SignalR 2 kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="91131-131">In this section, you'll update the application created in the [SignalR 1.x version of the Getting Started Tutorial](../older-versions/index.md) to use SignalR 2.</span></span>

1. <span data-ttu-id="91131-132">Başlarken Öğreticisi bitirdikten sonra projeye sağ tıklayın ve seçin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="91131-132">Once you've finished the Getting Started tutorial, right-click on the project, and select **Properties**.</span></span> <span data-ttu-id="91131-133">Doğrulayın **hedef framework** ayarlanır **.NET Framework 4.5.**</span><span class="sxs-lookup"><span data-stu-id="91131-133">Verify that the **Target framework** is set to **.NET Framework 4.5.**</span></span>
2. <span data-ttu-id="91131-134">Paket Yöneticisi Konsolu'nu açın.</span><span class="sxs-lookup"><span data-stu-id="91131-134">Open the Package Manager Console.</span></span> <span data-ttu-id="91131-135">SignalR kaldırın aşağıdaki komutu kullanarak projeden 1.x:</span><span class="sxs-lookup"><span data-stu-id="91131-135">Remove SignalR 1.x from the project using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. <span data-ttu-id="91131-136">SignalR aşağıdaki komutu kullanarak 2 yükleyin:</span><span class="sxs-lookup"><span data-stu-id="91131-136">Install SignalR 2 using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. <span data-ttu-id="91131-137">HTML sayfasında şimdi projeye dahil betik sürümüyle eşleşecek şekilde SignalR için komut dosyası başvurusunu güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="91131-137">In the HTML page, update the script reference for SignalR to match the version of the script now included in the project.</span></span>

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. <span data-ttu-id="91131-138">Genel Uygulama sınıfında MapHubs çağrısı kaldırın.</span><span class="sxs-lookup"><span data-stu-id="91131-138">In the global application class, remove the call to MapHubs.</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. <span data-ttu-id="91131-139">Çözüme sağ tıklayın ve seçin **Ekle**, **yeni öğe...** . İletişim kutusunda seçin **Owın başlangıç sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="91131-139">Right-click the solution, and select **Add**, **New Item...**. In the dialog, select **Owin Startup Class**.</span></span> <span data-ttu-id="91131-140">Yeni sınıf **haline**.</span><span class="sxs-lookup"><span data-stu-id="91131-140">Name the new class **Startup.cs**.</span></span>

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. <span data-ttu-id="91131-141">Haline içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="91131-141">Replace the contents of Startup.cs with the following code:</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    <span data-ttu-id="91131-142">Derleme özniteliği yürütür Owın'ın başlatma işlemi için sınıfı ekler `Configuration` Owın başladığında yöntemi.</span><span class="sxs-lookup"><span data-stu-id="91131-142">The assembly attribute adds the class to Owin's startup process, which executes the `Configuration` method when Owin starts up.</span></span> <span data-ttu-id="91131-143">Bu sırayla çağırır `MapSignalR` yollar tüm SignalR hub'ları için uygulamada oluşturur yöntemi.</span><span class="sxs-lookup"><span data-stu-id="91131-143">This in turn calls the `MapSignalR` method, which creates routes for all SignalR hubs in the application.</span></span>
8. <span data-ttu-id="91131-144">Projeyi çalıştırın ve başka bir dosyaya ana sayfanın URL'sini kopyalayın tarayıcı veya tarayıcı bölmesi önce olarak.</span><span class="sxs-lookup"><span data-stu-id="91131-144">Run the project, and copy the URL of the main page into another browser or browser pane, as before.</span></span> <span data-ttu-id="91131-145">Her bir sayfa için bir kullanıcı adı ister ve her sayfasından gönderilen iletiler hem tarayıcı bölmelerinde görünür olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="91131-145">Each page will ask for a username, and messages sent from each page should be visible in both browser panes.</span></span>

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a><span data-ttu-id="91131-146">Yükseltme sırasında karşılaşılan sorun giderme</span><span class="sxs-lookup"><span data-stu-id="91131-146">Troubleshooting errors encountered during upgrading</span></span>

<span data-ttu-id="91131-147">Bu bölümde, yükseltme sırasında ortaya çıkabilecek sorunlar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="91131-147">This section describes issues that may arise during upgrading.</span></span> <span data-ttu-id="91131-148">Hatalar ve bir SignalR uygulaması ile ortaya çıkabilecek sorunlar daha kapsamlı bir listesi için bkz [SignalR sorun giderme](../testing-and-debugging/troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="91131-148">For a more comprehensive list of errors and issues that may occur with a SignalR application, see [SignalR Troubleshooting](../testing-and-debugging/troubleshooting.md).</span></span>

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a><span data-ttu-id="91131-149">'Çağrı aşağıdaki yöntemleri veya özellikler arasında belirsiz'</span><span class="sxs-lookup"><span data-stu-id="91131-149">'The call is ambiguous between the following methods or properties'</span></span>

<span data-ttu-id="91131-150">Bu hata bir başvuru oluşacaktır `Microsoft.AspNet.SignalR.Owin` kaldırılmaz.</span><span class="sxs-lookup"><span data-stu-id="91131-150">This error will occur if a reference to `Microsoft.AspNet.SignalR.Owin` is not removed.</span></span> <span data-ttu-id="91131-151">Bu paket kullanım dışıdır; başvuru kaldırılmalıdır ve SelfHost paketini 1.x sürümünü kaldırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="91131-151">This package is deprecated; the reference must be removed and the 1.x version of the SelfHost package must be uninstalled.</span></span>

### <a name="hub-methods-fail-silently"></a><span data-ttu-id="91131-152">Hub yöntemlerini sessizce başarısız</span><span class="sxs-lookup"><span data-stu-id="91131-152">Hub methods fail silently</span></span>

<span data-ttu-id="91131-153">İstemci komut dosyası başvurularında tarih ve bu kadar doğrulayın `OwinStartup` başlangıç sınıfınızın doğru sınıf ve projeniz için derleme adları için öznitelik.</span><span class="sxs-lookup"><span data-stu-id="91131-153">Verify that the script references in your client are up to date, and that the `OwinStartup` attribute for your Startup class has the correct class and assembly names for your project.</span></span> <span data-ttu-id="91131-154">Ayrıca, tarayıcınızda hub adresini (/ signalr/hub) açmayı deneyin; görünen herhangi bir hata yanlış neler hakkında daha fazla bilgi sunar.</span><span class="sxs-lookup"><span data-stu-id="91131-154">Also, try opening up the hubs address (/signalr/hubs) in your browser; any error that appears will offer more information about what's going wrong.</span></span>
