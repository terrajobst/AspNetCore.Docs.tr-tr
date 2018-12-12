---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: SignalR 1.x projelerini 2 sürümüne yükseltme | Microsoft Docs
author: pfletcher
description: Bu konu, SignalR için mevcut bir SignalR 1.x projesini yükseltmek açıklar 2.x ve yükseltme işlemi sırasında oluşabilecek sorunların nasıl giderileceği...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: 23ea23585b15395cf86bdad13885af32d1b64e79
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53286850"
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a><span data-ttu-id="c800a-103">SignalR 1.x Projelerini 2 sürümüne yükseltme</span><span class="sxs-lookup"><span data-stu-id="c800a-103">Upgrading SignalR 1.x Projects to version 2</span></span>
====================
<span data-ttu-id="c800a-104">tarafından [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="c800a-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="c800a-105">Bu konu, SignalR için mevcut bir SignalR 1.x projesini yükseltmek açıklar 2.x ve yükseltme işlemi sırasında oluşabilecek sorunları nasıl giderilir.</span><span class="sxs-lookup"><span data-stu-id="c800a-105">This topic describes how to upgrade an existing SignalR 1.x project to SignalR 2.x, and how to troubleshoot issues that may arise during the upgrade process.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c800a-106">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="c800a-106">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="c800a-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="c800a-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="c800a-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="c800a-108">.NET 4.5</span></span>
> - <span data-ttu-id="c800a-109">SignalR sürüm 1 ve 2</span><span class="sxs-lookup"><span data-stu-id="c800a-109">SignalR versions 1 and 2</span></span>
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="c800a-110">Bu öğreticide Visual Studio 2012 kullanarak</span><span class="sxs-lookup"><span data-stu-id="c800a-110">Using Visual Studio 2012 with this tutorial</span></span>
>
>
> <span data-ttu-id="c800a-111">Visual Studio 2012 bu öğreticiyle kullanmak için aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="c800a-111">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
>
> - <span data-ttu-id="c800a-112">Güncelleştirme, [Paket Yöneticisi](http://docs.nuget.org/docs/start-here/installing-nuget) en son sürüme.</span><span class="sxs-lookup"><span data-stu-id="c800a-112">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="c800a-113">Yükleme [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="c800a-113">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="c800a-114">Web Platformu Yükleyicisi'nde arama ve yükleme **ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için**.</span><span class="sxs-lookup"><span data-stu-id="c800a-114">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="c800a-115">Bu SignalR sınıflar için Visual Studio şablonları gibi yükleyecek **Hub**.</span><span class="sxs-lookup"><span data-stu-id="c800a-115">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="c800a-116">Bazı şablonlar (gibi **OWIN başlangıç sınıfı**) kullanılabilir; olmayacaktır, bunlar için sınıf dosyası kullanın.</span><span class="sxs-lookup"><span data-stu-id="c800a-116">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="c800a-117">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="c800a-117">Questions and comments</span></span>
>
> <span data-ttu-id="c800a-118">Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın.</span><span class="sxs-lookup"><span data-stu-id="c800a-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="c800a-119">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="c800a-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="c800a-120">SignalR 2 kullanarak sunucu platformları arasında tutarlı bir geliştirme deneyimi sunan [OWIN](http://owin.org).</span><span class="sxs-lookup"><span data-stu-id="c800a-120">SignalR 2 offers a consistent development experience across server platforms using [OWIN](http://owin.org).</span></span> <span data-ttu-id="c800a-121">Bu makalede, bir sürüm 2 için SignalR 1.x uygulamayı güncelleştirmek için gereken birkaç adım açıklanır.</span><span class="sxs-lookup"><span data-stu-id="c800a-121">This article describes the few steps that are needed to update a SignalR 1.x application to version 2.</span></span>

<span data-ttu-id="c800a-122">SignalR 2 uygulamalarını yükseltmek için önerilir, ancak SignalR 1.x devam eder desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="c800a-122">While it is encouraged to upgrade applications to SignalR 2, SignalR 1.x will still be supported.</span></span>

<span data-ttu-id="c800a-123">Bu öğreticide SignalR 2 web barındırılan bir uygulamaya yükseltileceği açıklanır.</span><span class="sxs-lookup"><span data-stu-id="c800a-123">This tutorial describes how to upgrade a web-hosted application to SignalR 2.</span></span> <span data-ttu-id="c800a-124">Şirket içinde barındırılan uygulamaları (Bu bir konsol uygulaması, Windows hizmeti veya başka bir işlem bir sunucu barındırıyor) altında SignalR 2 artık desteklenmektedir.</span><span class="sxs-lookup"><span data-stu-id="c800a-124">Self-hosted applications (those that host a server in a console application, Windows service, or other process) are now supported under SignalR 2.</span></span> <span data-ttu-id="c800a-125">SignalR 2 ile şirket içinde barındırılan bir uygulama oluşturmaya başlama hakkında daha fazla bilgi için bkz: [Öğreticisi: Şirket içinde SignalR barındırma](../deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="c800a-125">For information on how to get started creating a self-hosted application with SignalR 2, see [Tutorial: SignalR Self-Host](../deployment/tutorial-signalr-self-host.md).</span></span>

## <a name="contents"></a><span data-ttu-id="c800a-126">İçindekiler</span><span class="sxs-lookup"><span data-stu-id="c800a-126">Contents</span></span>

<span data-ttu-id="c800a-127">Aşağıdaki bölümlerde, SignalR projeleri ve oluşabilecek sorunları gidermek nasıl yükseltme ile ilgili görevleri açıklar.</span><span class="sxs-lookup"><span data-stu-id="c800a-127">The following sections describe tasks involved with upgrading SignalR projects, and how to troubleshoot issues that may arise.</span></span>

- [<span data-ttu-id="c800a-128">Örnek: Başlarken Öğreticisi SignalR 2'ye yükseltme</span><span class="sxs-lookup"><span data-stu-id="c800a-128">Example: Upgrading the Getting Started tutorial to SignalR 2</span></span>](#example)
- [<span data-ttu-id="c800a-129">Yükseltme sırasında karşılaşılan hataları giderme</span><span class="sxs-lookup"><span data-stu-id="c800a-129">Troubleshooting errors encountered during upgrading</span></span>](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a><span data-ttu-id="c800a-130">Örnek: SignalR 2 Başlarken Öğreticisi uygulamayı yükseltme</span><span class="sxs-lookup"><span data-stu-id="c800a-130">Example: Upgrading the Getting Started tutorial application to SignalR 2</span></span>

<span data-ttu-id="c800a-131">Bu bölümde, oluşturulan uygulama güncelleştireceksiniz [SignalR 1.x sürümü kullanmaya başlama Öğreticisi](../older-versions/index.md) SignalR 2 kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="c800a-131">In this section, you'll update the application created in the [SignalR 1.x version of the Getting Started Tutorial](../older-versions/index.md) to use SignalR 2.</span></span>

1. <span data-ttu-id="c800a-132">Başlarken Öğreticisi bitirdiğinizde, projeye sağ tıklayın ve seçin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="c800a-132">Once you've finished the Getting Started tutorial, right-click on the project, and select **Properties**.</span></span> <span data-ttu-id="c800a-133">Doğrulayın **hedef Framework'ü** ayarlanır **.NET Framework 4.5.**</span><span class="sxs-lookup"><span data-stu-id="c800a-133">Verify that the **Target framework** is set to **.NET Framework 4.5.**</span></span>
2. <span data-ttu-id="c800a-134">Paket Yöneticisi konsolu açın.</span><span class="sxs-lookup"><span data-stu-id="c800a-134">Open the Package Manager Console.</span></span> <span data-ttu-id="c800a-135">SignalR kaldırın aşağıdaki komutu kullanarak proje 1.x:</span><span class="sxs-lookup"><span data-stu-id="c800a-135">Remove SignalR 1.x from the project using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. <span data-ttu-id="c800a-136">SignalR 2 aşağıdaki komutu kullanarak yükleyin:</span><span class="sxs-lookup"><span data-stu-id="c800a-136">Install SignalR 2 using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. <span data-ttu-id="c800a-137">HTML sayfalarında betik başvurusu için SignalR komut dosyası artık projeye dahil sürümüyle eşleşecek şekilde güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="c800a-137">In the HTML page, update the script reference for SignalR to match the version of the script now included in the project.</span></span>

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. <span data-ttu-id="c800a-138">Genel uygulama sınıfı içinde MapHubs çağrısını kaldırın.</span><span class="sxs-lookup"><span data-stu-id="c800a-138">In the global application class, remove the call to MapHubs.</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. <span data-ttu-id="c800a-139">Çözüme sağ tıklayıp seçin **Ekle**, **yeni öğe...** . İletişim kutusunda, seçmek **Owın başlangıç sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="c800a-139">Right-click the solution, and select **Add**, **New Item...**. In the dialog, select **Owin Startup Class**.</span></span> <span data-ttu-id="c800a-140">Yeni bir sınıf adı **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="c800a-140">Name the new class **Startup.cs**.</span></span>

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. <span data-ttu-id="c800a-141">Startup.cs içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="c800a-141">Replace the contents of Startup.cs with the following code:</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    <span data-ttu-id="c800a-142">Derleme özniteliğini yürütür Owın'ın başlatma işlemi için sınıf ekler `Configuration` Owın başlatıldığında yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c800a-142">The assembly attribute adds the class to Owin's startup process, which executes the `Configuration` method when Owin starts up.</span></span> <span data-ttu-id="c800a-143">Bu sırayla çağırır `MapSignalR` yöntemi uygulamada tüm SignalR hub'ları için rotalar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c800a-143">This in turn calls the `MapSignalR` method, which creates routes for all SignalR hubs in the application.</span></span>
8. <span data-ttu-id="c800a-144">Projeyi çalıştırmak ve başka bir ana sayfanın URL'sini kopyalayın tarayıcı veya tarayıcı bölmesinde, önceki olarak.</span><span class="sxs-lookup"><span data-stu-id="c800a-144">Run the project, and copy the URL of the main page into another browser or browser pane, as before.</span></span> <span data-ttu-id="c800a-145">Her sayfa için bir kullanıcı adı sorar ve her sayfadan gönderilen iletileri hem de tarayıcı bölmelerinde görünür olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c800a-145">Each page will ask for a username, and messages sent from each page should be visible in both browser panes.</span></span>

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a><span data-ttu-id="c800a-146">Yükseltme sırasında karşılaşılan hataları giderme</span><span class="sxs-lookup"><span data-stu-id="c800a-146">Troubleshooting errors encountered during upgrading</span></span>

<span data-ttu-id="c800a-147">Bu bölümde, yükseltme sırasında karşılaşabileceğiniz sorunlar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="c800a-147">This section describes issues that may arise during upgrading.</span></span> <span data-ttu-id="c800a-148">Hatalar ve SignalR uygulamayla meydana gelebilecek sorunları daha kapsamlı bir liste için bkz [SignalR sorunlarını giderme](../testing-and-debugging/troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="c800a-148">For a more comprehensive list of errors and issues that may occur with a SignalR application, see [SignalR Troubleshooting](../testing-and-debugging/troubleshooting.md).</span></span>

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a><span data-ttu-id="c800a-149">'Çağrısı aşağıdaki yöntemleri veya özellikleri arasında belirsiz'</span><span class="sxs-lookup"><span data-stu-id="c800a-149">'The call is ambiguous between the following methods or properties'</span></span>

<span data-ttu-id="c800a-150">Bir başvuru değilse bu hata meydana gelir `Microsoft.AspNet.SignalR.Owin` kaldırılmaz.</span><span class="sxs-lookup"><span data-stu-id="c800a-150">This error will occur if a reference to `Microsoft.AspNet.SignalR.Owin` is not removed.</span></span> <span data-ttu-id="c800a-151">Bu paket kullanım dışı; başvuru kaldırılmalıdır ve SelfHost paketini 1.x sürümü kaldırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c800a-151">This package is deprecated; the reference must be removed and the 1.x version of the SelfHost package must be uninstalled.</span></span>

### <a name="hub-methods-fail-silently"></a><span data-ttu-id="c800a-152">Hub yöntemlerinde sessizce başarısız</span><span class="sxs-lookup"><span data-stu-id="c800a-152">Hub methods fail silently</span></span>

<span data-ttu-id="c800a-153">İstemci komut dosyası başvurularını tarih ve, en fazla olduğundan emin olun `OwinStartup` başlangıç sınıfınıza doğru sınıf ve projenizin derleme adları için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="c800a-153">Verify that the script references in your client are up to date, and that the `OwinStartup` attribute for your Startup class has the correct class and assembly names for your project.</span></span> <span data-ttu-id="c800a-154">Ayrıca, tarayıcınızda hubs adresi (/ signalr/hub) açmayı deneyin; görüntülenen herhangi bir hata yanlış neler hakkında daha fazla bilgi sunar.</span><span class="sxs-lookup"><span data-stu-id="c800a-154">Also, try opening up the hubs address (/signalr/hubs) in your browser; any error that appears will offer more information about what's going wrong.</span></span>
