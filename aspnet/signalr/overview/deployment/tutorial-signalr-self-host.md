---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Öğretici: Şirket içinde SignalR barındırma | Microsoft Docs'
author: pfletcher
description: Bu öğretici, şirket içinde barındırılan bir SignalR 2 sunucunun nasıl oluşturulacağını ve bir JavaScript istemcisi ile bağlanmak nasıl gösterir. V öğreticide kullanılan yazılım sürümleri...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 48cb3d4d71c33ac3382b2b35b5a19fa1c4958874
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287409"
---
<a name="tutorial-signalr-self-host"></a><span data-ttu-id="3c2bc-104">Öğretici: Şirket içinde SignalR barındırma</span><span class="sxs-lookup"><span data-stu-id="3c2bc-104">Tutorial: SignalR Self-Host</span></span>
====================
<span data-ttu-id="3c2bc-105">tarafından [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="3c2bc-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[<span data-ttu-id="3c2bc-106">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="3c2bc-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> <span data-ttu-id="3c2bc-107">Bu öğretici, şirket içinde barındırılan bir SignalR 2 sunucunun nasıl oluşturulacağını ve bir JavaScript istemcisi ile bağlanmak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-107">This tutorial shows how to create a self-hosted SignalR 2 server, and how to connect to it with a JavaScript client.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3c2bc-108">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="3c2bc-108">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="3c2bc-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3c2bc-109">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="3c2bc-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="3c2bc-110">.NET 4.5</span></span>
> - <span data-ttu-id="3c2bc-111">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="3c2bc-111">SignalR version 2</span></span>
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="3c2bc-112">Bu öğreticide Visual Studio 2012 kullanarak</span><span class="sxs-lookup"><span data-stu-id="3c2bc-112">Using Visual Studio 2012 with this tutorial</span></span>
>
>
> <span data-ttu-id="3c2bc-113">Visual Studio 2012 bu öğreticiyle kullanmak için aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="3c2bc-113">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
>
> - <span data-ttu-id="3c2bc-114">Güncelleştirme, [Paket Yöneticisi](http://docs.nuget.org/docs/start-here/installing-nuget) en son sürüme.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-114">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="3c2bc-115">Yükleme [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="3c2bc-115">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="3c2bc-116">Web Platformu Yükleyicisi'nde arama ve yükleme **ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için**.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-116">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="3c2bc-117">Bu SignalR sınıflar için Visual Studio şablonları gibi yükleyecek **Hub**.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-117">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="3c2bc-118">Bazı şablonlar (gibi **OWIN başlangıç sınıfı**) kullanılabilir; olmayacaktır, bunlar için sınıf dosyası kullanın.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-118">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="3c2bc-119">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="3c2bc-119">Questions and comments</span></span>
>
> <span data-ttu-id="3c2bc-120">Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-120">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="3c2bc-121">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="3c2bc-121">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="3c2bc-122">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="3c2bc-122">Overview</span></span>

<span data-ttu-id="3c2bc-123">Bir SignalR sunucusunu genellikle IIS'de bir ASP.NET uygulamasında barındırılır ancak aynı zamanda şirket içinde barındırılan olabilir (örn. bir konsol uygulaması veya Windows hizmeti) barındırılan kitaplığını kullanarak.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-123">A SignalR server is usually hosted in an ASP.NET application in IIS, but it can also be self-hosted (such as in a console application or Windows service) using the self-host library.</span></span> <span data-ttu-id="3c2bc-124">Tüm SignalR 2'de, bu kitaplık OWIN oluşturulmuştur ([.NET için açık Web arabirimi](http://owin.org)).</span><span class="sxs-lookup"><span data-stu-id="3c2bc-124">This library, like all of SignalR 2, is built on OWIN ([Open Web Interface for .NET](http://owin.org)).</span></span> <span data-ttu-id="3c2bc-125">OWIN .NET web sunucuları ve web uygulaması arasında bir Özet tanımlar.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-125">OWIN defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="3c2bc-126">OWIN OWIN kendi işleminizde IIS dışında bir web uygulaması kendi kendine barındırma için ideal hale getirir sunucu web uygulamasından ayırır.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-126">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>

<span data-ttu-id="3c2bc-127">IIS'de barındırmayan nedenleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="3c2bc-127">Reasons for not hosting in IIS include:</span></span>

- <span data-ttu-id="3c2bc-128">Ortamlar nerede IIS veya IIS olmadan var olan bir sunucu grubu gibi istenen kullanılabilir değil.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-128">Environments where IIS is not available or desirable, such as an existing server farm without IIS.</span></span>
- <span data-ttu-id="3c2bc-129">IIS performans yükü kaçınılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-129">The performance overhead of IIS needs to be avoided.</span></span>
- <span data-ttu-id="3c2bc-130">SignalR bir Windows hizmeti, Azure çalışan rolü veya başka bir işlem çalıştırır geçireceğinizi uygulamaya eklenecek bir işlevdir.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-130">SignalR functionality is to be added to an exising application that runs in a Windows Service, Azure worker role, or other process.</span></span>

<span data-ttu-id="3c2bc-131">Bir çözüm olarak barındırılan performans nedenleriyle geliştirilmekte olduğundan, performans avantajı belirlemek için IIS'de barındırılan uygulamayı test için de önerilir.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-131">If a solution is being developed as self-host for performance reasons, it's recommended to also test the application hosted in IIS to determine the performance benefit.</span></span>

<span data-ttu-id="3c2bc-132">Bu öğreticide, aşağıdaki bölümleri içerir:</span><span class="sxs-lookup"><span data-stu-id="3c2bc-132">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="3c2bc-133">Sunucu oluşturma</span><span class="sxs-lookup"><span data-stu-id="3c2bc-133">Creating the server</span></span>](#server)
- [<span data-ttu-id="3c2bc-134">Server JavaScript istemcisi ile erişme</span><span class="sxs-lookup"><span data-stu-id="3c2bc-134">Accessing the server with a JavaScript client</span></span>](#js)

<a id="server"></a>

## <a name="creating-the-server"></a><span data-ttu-id="3c2bc-135">Sunucu oluşturma</span><span class="sxs-lookup"><span data-stu-id="3c2bc-135">Creating the server</span></span>

<span data-ttu-id="3c2bc-136">Bu öğreticide, bir konsol uygulamasında barındırılan bir sunucu oluşturacaksınız ancak sunucu, işlem, bir Windows hizmeti ya da Azure çalışan rolü gibi herhangi bir tür içinde barındırılabilir.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-136">In this tutorial, you'll create a server that's hosted in a console application, but the server can be hosted in any sort of process, such as a Windows service or Azure worker role.</span></span> <span data-ttu-id="3c2bc-137">Bir Windows hizmetinde bir SignalR sunucusunu barındırmak için örnek kod için bkz: [Self-Hosting SignalR bir Windows hizmetinde](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span><span class="sxs-lookup"><span data-stu-id="3c2bc-137">For sample code for hosting a SignalR server in a Windows Service, see [Self-Hosting SignalR in a Windows Service](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span></span>

1. <span data-ttu-id="3c2bc-138">Visual Studio 2013, yönetici ayrıcalıklarıyla açın.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-138">Open Visual Studio 2013 with administrator privileges.</span></span> <span data-ttu-id="3c2bc-139">Seçin **dosya**, **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-139">Select **File**, **New Project**.</span></span> <span data-ttu-id="3c2bc-140">Seçin **Windows** altında **Visual C#** düğümünde **şablonları** bölmesi ve select **konsol uygulaması** şablonu.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-140">Select **Windows** under the **Visual C#** node in the **Templates** pane, and select the **Console Application** template.</span></span> <span data-ttu-id="3c2bc-141">Yeni Proje "SignalRSelfHost" adını verin ve tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-141">Name the new project "SignalRSelfHost" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image1.png)
2. <span data-ttu-id="3c2bc-142">NuGet Paket Yöneticisi konsolu seçerek açın **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-142">Open the NuGet package manager console by selecting **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>
3. <span data-ttu-id="3c2bc-143">Paket Yöneticisi konsolunda aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="3c2bc-143">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    <span data-ttu-id="3c2bc-144">Bu komut, SignalR 2 Self-Host kitaplıkları projeye ekler.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-144">This command adds the SignalR 2 Self-Host libraries to the project.</span></span>
4. <span data-ttu-id="3c2bc-145">Paket Yöneticisi konsolunda aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="3c2bc-145">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    <span data-ttu-id="3c2bc-146">Bu komut Microsoft.Owin.Cors kitaplığı projeye ekler.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-146">This command adds the Microsoft.Owin.Cors library to the project.</span></span> <span data-ttu-id="3c2bc-147">Bu kitaplık, SignalR ve web sayfasının istemci farklı etki alanlarında barındıran uygulamalar için gerekli olan etki alanları arası desteği için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-147">This library will be used for cross-domain support, which is required for applications that host SignalR and a web page client in different domains.</span></span> <span data-ttu-id="3c2bc-148">SignalR sunucusunu ve web istemcisi farklı noktalarında barındırma olduğundan, bu, etki alanları arası bu bileşenleri arasındaki iletişim için etkinleştirilmelidir anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-148">Since you'll be hosting the SignalR server and the web client on different ports, this means that cross-domain must be enabled for communication between these components.</span></span>
5. <span data-ttu-id="3c2bc-149">Program.cs içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-149">Replace the contents of Program.cs with the following code.</span></span>

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    <span data-ttu-id="3c2bc-150">Yukarıdaki kod, üç sınıfları içerir:</span><span class="sxs-lookup"><span data-stu-id="3c2bc-150">The above code includes three classes:</span></span>

    - <span data-ttu-id="3c2bc-151">**Program**de dahil olmak üzere **ana** yürütme birincil yolu tanımlama yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-151">**Program**, including the **Main** method defining the primary path of execution.</span></span> <span data-ttu-id="3c2bc-152">Bu yöntemde, bir web uygulaması türünün **başlangıç** belirtilen URL'deki başlatılır (`http://localhost:8080`).</span><span class="sxs-lookup"><span data-stu-id="3c2bc-152">In this method, a web application of type **Startup** is started at the specified URL (`http://localhost:8080`).</span></span> <span data-ttu-id="3c2bc-153">SSL güvenlik uç noktada gerekiyorsa uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-153">If security is required on the endpoint, SSL can be implemented.</span></span> <span data-ttu-id="3c2bc-154">Bkz: [nasıl yapılır: Bir SSL sertifikası ile bir bağlantı noktası yapılandırma](https://msdn.microsoft.com/library/ms733791.aspx) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-154">See [How to: Configure a Port with an SSL Certificate](https://msdn.microsoft.com/library/ms733791.aspx) for more information.</span></span>
    - <span data-ttu-id="3c2bc-155">**Başlangıç**, SignalR sunucusu yapılandırmasını içeren sınıf (Bu öğreticide yalnızca yapılandırma çağrıdır `UseCors`) ve çağrısı `MapSignalR`, projede herhangi bir Hub nesne için rotalar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-155">**Startup**, the class containing the configuration for the SignalR server (the only configuration this tutorial uses is the call to `UseCors`), and the call to `MapSignalR`, which creates routes for any Hub objects in the project.</span></span>
    - <span data-ttu-id="3c2bc-156">**MyHub**, istemcilere uygulama sağlayacak SignalR Hub sınıfı.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-156">**MyHub**, the SignalR Hub class that the application will provide to clients.</span></span> <span data-ttu-id="3c2bc-157">Bu sınıfın tek bir yöntemi vardır **Gönder**, istemcilerin diğer tüm bağlı istemcileri için bir ileti yayınlamak için çağırır.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-157">This class has a single method, **Send**, that clients will call to broadcast a message to all other connected clients.</span></span>
6. <span data-ttu-id="3c2bc-158">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-158">Compile and run the application.</span></span> <span data-ttu-id="3c2bc-159">Konsol penceresinde çalıştıran bir sunucuda adresini göstermelidir.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-159">The address that the server is running should show in a console window.</span></span>

    ![](tutorial-signalr-self-host/_static/image2.png)
7. <span data-ttu-id="3c2bc-160">Yürütme özel durumla başarısız olursa `System.Reflection.TargetInvocationException was unhandled`, yönetici ayrıcalıklarıyla Visual Studio'yu yeniden başlatmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-160">If execution fails with the exception `System.Reflection.TargetInvocationException was unhandled`, you will need to restart Visual Studio with administrator privileges.</span></span>
8. <span data-ttu-id="3c2bc-161">Sonraki bölüme devam etmeden önce uygulamayı durdurun.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-161">Stop the application before proceeding to the next section.</span></span>

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a><span data-ttu-id="3c2bc-162">Server JavaScript istemcisi ile erişme</span><span class="sxs-lookup"><span data-stu-id="3c2bc-162">Accessing the server with a JavaScript client</span></span>

<span data-ttu-id="3c2bc-163">Bu bölümde, aynı JavaScript istemciden kullanacağınız [Başlarken Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="3c2bc-163">In this section, you'll use the same JavaScript client from the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="3c2bc-164">Hub'ı URL'si açıkça tanımlamak için olan istemci, bir değişiklik yalnızca oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-164">We'll only make one modification to the client, which is to explicitly define the hub URL.</span></span> <span data-ttu-id="3c2bc-165">Şirket içinde barındırılan bir uygulamayla sunucu mutlaka aynı adresten (ters proxy ve yük Dengeleyiciler) nedeniyle bağlantı URL'si olarak bu nedenle açıkça tanımlanması gerekir olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-165">With a self-hosted application, the server may not necessarily be at the same address as the connection URL (due to reverse proxies and load balancers), so the URL needs to be defined explicitly.</span></span>

1. <span data-ttu-id="3c2bc-166">İçinde **Çözüm Gezgini**, çözüme sağ tıklayın ve seçin **Ekle**, **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-166">In **Solution Explorer**, right-click on the solution and select **Add**, **New Project**.</span></span> <span data-ttu-id="3c2bc-167">Seçin **Web** düğüm ve select **ASP.NET Web uygulaması** şablonu.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-167">Select the **Web** node, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="3c2bc-168">"JavascriptClient" Projeyi adlandırın ve tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-168">Name the project "JavascriptClient" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image3.png)
2. <span data-ttu-id="3c2bc-169">Seçin **boş** şablonu ve diğer seçenekleri seçilmemiş olarak bırakın.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-169">Select the **Empty** template, and leave the remaining options unselected.</span></span> <span data-ttu-id="3c2bc-170">Seçin **projesi oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-170">Select **Create Project**.</span></span>

    ![](tutorial-signalr-self-host/_static/image4.png)
3. <span data-ttu-id="3c2bc-171">Paket Yöneticisi Konsolu'nda "JavascriptClient" projenizi **varsayılan proje** açılır ve aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="3c2bc-171">In the package manager console, select the "JavascriptClient" project in the **Default project** drop-down, and execute the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    <span data-ttu-id="3c2bc-172">Bu komut istemcisinde ihtiyacınız olacak SignalR ve JQuery kitaplıklarını yükler.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-172">This command installs the SignalR and JQuery libraries that you'll need in the client.</span></span>
4. <span data-ttu-id="3c2bc-173">Seçin ve proje üzerinde sağ **Ekle**, **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-173">Right-click on your project and select **Add**, **New Item**.</span></span> <span data-ttu-id="3c2bc-174">Seçin **Web** düğüm ve HTML sayfasını seçin.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-174">Select the **Web** node, and select HTML Page.</span></span> <span data-ttu-id="3c2bc-175">Sayfayı adlandırın **Default.html**.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-175">Name the page **Default.html**.</span></span>

    ![](tutorial-signalr-self-host/_static/image5.png)
5. <span data-ttu-id="3c2bc-176">Yeni HTML sayfasının içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-176">Replace the contents of the new HTML page with the following code.</span></span> <span data-ttu-id="3c2bc-177">Komut dosyası başvuruları burada projenin Scripts klasörü betiklerde eşleştiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-177">Verify that the script references here match the scripts in the Scripts folder of the project.</span></span>

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    <span data-ttu-id="3c2bc-178">Aşağıdaki kodu (Yukarıdaki kod örneğinde vurgulanan) (ek olarak kodu SignalR sürüm 2 beta yükseltme) alma Stared öğreticide kullanılan istemci yaptığınız ektir.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-178">The following code (highlighted in the code sample above) is the addition that you've made to the client used in the Getting Stared tutorial (in addition to upgrading the code to SignalR version 2 beta).</span></span> <span data-ttu-id="3c2bc-179">Bu kod satırı, sunucuda SignalR için temel bağlantı URL'si açıkça ayarlar.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-179">This line of code explicitly sets the base connection URL for SignalR on the server.</span></span>

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. <span data-ttu-id="3c2bc-180">Çözüme sağ tıklayın ve seçin **başlangıç projelerini Ayarla...** . Seçin **birden fazla başlangıç projesi** radyo düğmesini ve her iki projeleri ayarlama **eylem** için **Başlat**.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-180">Right-click on the solution, and select **Set Startup Projects...**. Select the **Multiple startup projects** radio button, and set both projects' **Action** to **Start**.</span></span>

    ![](tutorial-signalr-self-host/_static/image6.png)
7. <span data-ttu-id="3c2bc-181">"Default.html üzerinde" sağ tıklayıp **başlangıç sayfası olarak ayarla**.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-181">Right-click on "Default.html" and select **Set As Start Page**.</span></span>
8. <span data-ttu-id="3c2bc-182">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-182">Run the application.</span></span> <span data-ttu-id="3c2bc-183">Sayfa ve sunucu başlatılır.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-183">The server and page will launch.</span></span> <span data-ttu-id="3c2bc-184">Web sayfasını yeniden yüklemeniz gerekebilir (veya **devam** hata ayıklayıcısı) server başlatılmadan önce sayfa yüklenirse.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-184">You may need to reload the web page (or select **Continue** in the debugger) if the page loads before the server is started.</span></span>
9. <span data-ttu-id="3c2bc-185">Tarayıcıda, istendiğinde bir kullanıcı adını belirtin.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-185">In the browser, provide a username when prompted.</span></span> <span data-ttu-id="3c2bc-186">Başka bir tarayıcı sekmesinde veya penceresinde sayfanın URL'sini kopyalayın ve farklı bir kullanıcı adını belirtin.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-186">Copy the page's URL into another browser tab or window and provide a different username.</span></span> <span data-ttu-id="3c2bc-187">Diğer kullanmaya başlama Öğreticisi olduğu gibi bir tarayıcı bölmesinden mesajları gönderebilir olacaktır.</span><span class="sxs-lookup"><span data-stu-id="3c2bc-187">You will be able to send messages from one browser pane to the other, as in the Getting Started tutorial.</span></span>
