---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: "Öğretici: SignalR kendini barındırma | Microsoft Docs"
author: pfletcher
description: "Bu öğretici, kendi kendini barındıran bir SignalR 2 sunucunun nasıl oluşturulacağını ve JavaScript istemci ile bağlanmak nasıl gösterir. V öğreticide kullanılan yazılım sürümleri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 997756ff8d48e41da981491d6154f3107ec7a051
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-signalr-self-host"></a><span data-ttu-id="796f9-104">Öğretici: SignalR kendini barındırma</span><span class="sxs-lookup"><span data-stu-id="796f9-104">Tutorial: SignalR Self-Host</span></span>
====================
<span data-ttu-id="796f9-105">tarafından [CAN Fletcher'dan](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="796f9-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="796f9-106">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="796f9-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> <span data-ttu-id="796f9-107">Bu öğretici, kendi kendini barındıran bir SignalR 2 sunucunun nasıl oluşturulacağını ve JavaScript istemci ile bağlanmak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="796f9-107">This tutorial shows how to create a self-hosted SignalR 2 server, and how to connect to it with a JavaScript client.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="796f9-108">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="796f9-108">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="796f9-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="796f9-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="796f9-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="796f9-110">.NET 4.5</span></span>
> - <span data-ttu-id="796f9-111">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="796f9-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="796f9-112">Bu öğretici ile Visual Studio 2012 kullanma</span><span class="sxs-lookup"><span data-stu-id="796f9-112">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="796f9-113">Visual Studio 2012 bu öğreticiyle kullanmak için aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="796f9-113">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="796f9-114">Güncelleştirme, [Paket Yöneticisi](http://docs.nuget.org/docs/start-here/installing-nuget) en son sürüme.</span><span class="sxs-lookup"><span data-stu-id="796f9-114">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="796f9-115">Yükleme [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="796f9-115">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="796f9-116">Web Platformu Yükleyicisi'nde arayın ve yükleyin **ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için**.</span><span class="sxs-lookup"><span data-stu-id="796f9-116">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="796f9-117">Bu SignalR sınıfları için Visual Studio şablonları gibi yükleyecek **Hub**.</span><span class="sxs-lookup"><span data-stu-id="796f9-117">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="796f9-118">Bazı şablonlar (gibi **OWIN başlangıç sınıfı**); kullanılamaz bunlar için bunun yerine bir sınıf dosyası kullanın.</span><span class="sxs-lookup"><span data-stu-id="796f9-118">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="796f9-119">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="796f9-119">Questions and comments</span></span>
> 
> <span data-ttu-id="796f9-120">Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi.</span><span class="sxs-lookup"><span data-stu-id="796f9-120">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="796f9-121">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="796f9-121">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="796f9-122">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="796f9-122">Overview</span></span>

<span data-ttu-id="796f9-123">Bir SignalR sunucusu genellikle IIS'de bir ASP.NET uygulamasında barındırılan, ancak Ayrıca kendi kendini barındıran olabilir (örn. bir konsol uygulaması veya Windows hizmeti) kendi kendini barındıran kitaplığı kullanılarak.</span><span class="sxs-lookup"><span data-stu-id="796f9-123">A SignalR server is usually hosted in an ASP.NET application in IIS, but it can also be self-hosted (such as in a console application or Windows service) using the self-host library.</span></span> <span data-ttu-id="796f9-124">Tüm SignalR 2 gibi bu kitaplığı OWIN üzerinde oluşturulmuştur ([.NET açık Web arabirimi](http://owin.org)).</span><span class="sxs-lookup"><span data-stu-id="796f9-124">This library, like all of SignalR 2, is built on OWIN ([Open Web Interface for .NET](http://owin.org)).</span></span> <span data-ttu-id="796f9-125">OWIN .NET web sunucuları ve web uygulamaları arasındaki bir Özet tanımlar.</span><span class="sxs-lookup"><span data-stu-id="796f9-125">OWIN defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="796f9-126">OWIN OWIN web uygulamasını IIS dışında kendi işleminde kendi kendine barındırma için ideal hale getirir sunucunun web uygulamasından ayrıştırır.</span><span class="sxs-lookup"><span data-stu-id="796f9-126">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>

<span data-ttu-id="796f9-127">IIS'de barındırmayan nedenleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="796f9-127">Reasons for not hosting in IIS include:</span></span>

- <span data-ttu-id="796f9-128">IIS kullanılabilir veya varolan bir sunucu grubuna IIS olmadan gibi istenmediğinde olduğu ortamlarda.</span><span class="sxs-lookup"><span data-stu-id="796f9-128">Environments where IIS is not available or desirable, such as an existing server farm without IIS.</span></span>
- <span data-ttu-id="796f9-129">IIS performans yükü kaçınılması gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="796f9-129">The performance overhead of IIS needs to be avoided.</span></span>
- <span data-ttu-id="796f9-130">SignalR bir Windows hizmeti, Azure çalışan rolü veya başka bir işlemin çalışır exising uygulamaya eklenecek bir işlevdir.</span><span class="sxs-lookup"><span data-stu-id="796f9-130">SignalR functionality is to be added to an exising application that runs in a Windows Service, Azure worker role, or other process.</span></span>

<span data-ttu-id="796f9-131">Bir çözüm olarak kendini barındırma performans nedenleriyle geliştirilmekte olduğundan, bu da testine performans avantajı belirlemek için IIS barındırılan uygulama önerilir.</span><span class="sxs-lookup"><span data-stu-id="796f9-131">If a solution is being developed as self-host for performance reasons, it's recommended to also test the application hosted in IIS to determine the performance benefit.</span></span>

<span data-ttu-id="796f9-132">Bu öğretici aşağıdaki bölümleri içerir:</span><span class="sxs-lookup"><span data-stu-id="796f9-132">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="796f9-133">Sunucu oluşturma</span><span class="sxs-lookup"><span data-stu-id="796f9-133">Creating the server</span></span>](#server)
- [<span data-ttu-id="796f9-134">Sunucu bir JavaScript istemci ile erişme</span><span class="sxs-lookup"><span data-stu-id="796f9-134">Accessing the server with a JavaScript client</span></span>](#js)

<a id="server"></a>

## <a name="creating-the-server"></a><span data-ttu-id="796f9-135">Sunucu oluşturma</span><span class="sxs-lookup"><span data-stu-id="796f9-135">Creating the server</span></span>

<span data-ttu-id="796f9-136">Bu öğreticide, barındırılan bir sunucu bir konsol uygulaması oluşturacaksınız, ancak sunucu işlemi, bir Windows hizmeti veya Azure çalışan rolü gibi herhangi bir tür olarak barındırılabilir.</span><span class="sxs-lookup"><span data-stu-id="796f9-136">In this tutorial, you'll create a server that's hosted in a console application, but the server can be hosted in any sort of process, such as a Windows service or Azure worker role.</span></span> <span data-ttu-id="796f9-137">Bir Windows hizmetinde bir SignalR sunucusunu barındırmak için örnek kod için bkz: [Self-Hosting SignalR bir Windows hizmetinde](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span><span class="sxs-lookup"><span data-stu-id="796f9-137">For sample code for hosting a SignalR server in a Windows Service, see [Self-Hosting SignalR in a Windows Service](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span></span>

1. <span data-ttu-id="796f9-138">Visual Studio 2013 yönetici ayrıcalıklarıyla açın.</span><span class="sxs-lookup"><span data-stu-id="796f9-138">Open Visual Studio 2013 with administrator privileges.</span></span> <span data-ttu-id="796f9-139">Seçin **dosya**, **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="796f9-139">Select **File**, **New Project**.</span></span> <span data-ttu-id="796f9-140">Seçin **Windows** altında **Visual C#** düğümünde **şablonları** bölmesinde ve select **konsol uygulaması** şablonu.</span><span class="sxs-lookup"><span data-stu-id="796f9-140">Select **Windows** under the **Visual C#** node in the **Templates** pane, and select the **Console Application** template.</span></span> <span data-ttu-id="796f9-141">Yeni Proje "SignalRSelfHost" olarak adlandırın ve tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="796f9-141">Name the new project "SignalRSelfHost" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image1.png)
2. <span data-ttu-id="796f9-142">Kitaplık Paket Yöneticisi konsolu seçerek açın **Araçları**, **kitaplık Paket Yöneticisi**, **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="796f9-142">Open the library package manager console by selecting **Tools**, **Library Package Manager**, **Package Manager Console**.</span></span>
3. <span data-ttu-id="796f9-143">Paket Yöneticisi Konsolu aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="796f9-143">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    <span data-ttu-id="796f9-144">Bu komut, SignalR 2 Self-Host kitaplıkları projeye ekler.</span><span class="sxs-lookup"><span data-stu-id="796f9-144">This command adds the SignalR 2 Self-Host libraries to the project.</span></span>
4. <span data-ttu-id="796f9-145">Paket Yöneticisi Konsolu aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="796f9-145">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    <span data-ttu-id="796f9-146">Bu komut Microsoft.Owin.Cors kitaplığı projeye ekler.</span><span class="sxs-lookup"><span data-stu-id="796f9-146">This command adds the Microsoft.Owin.Cors library to the project.</span></span> <span data-ttu-id="796f9-147">Bu kitaplık, SignalR ve bir web sayfası istemci farklı etki alanlarında barındıran uygulamalar için gerekli olan etki alanları arası desteği için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="796f9-147">This library will be used for cross-domain support, which is required for applications that host SignalR and a web page client in different domains.</span></span> <span data-ttu-id="796f9-148">SignalR sunucusu ile farklı bağlantı noktaları üzerinde web istemcisi barındırma olduğundan, bu, bu bileşenler arasındaki iletişim için o etki alanları arası etkinleştirilmelidir anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="796f9-148">Since you'll be hosting the SignalR server and the web client on different ports, this means that cross-domain must be enabled for communication between these components.</span></span>
5. <span data-ttu-id="796f9-149">Program.cs içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="796f9-149">Replace the contents of Program.cs with the following code.</span></span>

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    <span data-ttu-id="796f9-150">Yukarıdaki kod üç sınıfları içerir:</span><span class="sxs-lookup"><span data-stu-id="796f9-150">The above code includes three classes:</span></span>

    - <span data-ttu-id="796f9-151">**Program**dahil **ana** yürütme birincil yolu tanımlama yöntemi.</span><span class="sxs-lookup"><span data-stu-id="796f9-151">**Program**, including the **Main** method defining the primary path of execution.</span></span> <span data-ttu-id="796f9-152">Bu yöntemde, bir web uygulaması türünün **başlangıç** belirtilen URL'de başlatıldı (`http://localhost:8080`).</span><span class="sxs-lookup"><span data-stu-id="796f9-152">In this method, a web application of type **Startup** is started at the specified URL (`http://localhost:8080`).</span></span> <span data-ttu-id="796f9-153">SSL güvenlik noktadaki gerekirse uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="796f9-153">If security is required on the endpoint, SSL can be implemented.</span></span> <span data-ttu-id="796f9-154">Bkz: [nasıl yapılır: bir SSL sertifikası ile bir bağlantı noktası yapılandırın](https://msdn.microsoft.com/en-us/library/ms733791.aspx) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="796f9-154">See [How to: Configure a Port with an SSL Certificate](https://msdn.microsoft.com/en-us/library/ms733791.aspx) for more information.</span></span>
    - <span data-ttu-id="796f9-155">**Başlangıç**, SignalR sunucu yapılandırmasını içeren sınıf (Bu öğretici kullanır yalnızca çağrısı yapılandırmadır `UseCors`) ve çağrısı `MapSignalR`, projede yollar için herhangi bir Hub nesne oluşturur.</span><span class="sxs-lookup"><span data-stu-id="796f9-155">**Startup**, the class containing the configuration for the SignalR server (the only configuration this tutorial uses is the call to `UseCors`), and the call to `MapSignalR`, which creates routes for any Hub objects in the project.</span></span>
    - <span data-ttu-id="796f9-156">**MyHub**, istemcilere uygulama sağlayacak SignalR hub'ı sınıfı.</span><span class="sxs-lookup"><span data-stu-id="796f9-156">**MyHub**, the SignalR Hub class that the application will provide to clients.</span></span> <span data-ttu-id="796f9-157">Bu sınıfın tek bir yönteme sahip **Gönder**, istemcilerin diğer tüm bağlı istemcilere bir ileti yayınlamak için çağırır.</span><span class="sxs-lookup"><span data-stu-id="796f9-157">This class has a single method, **Send**, that clients will call to broadcast a message to all other connected clients.</span></span>
6. <span data-ttu-id="796f9-158">Derleme ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="796f9-158">Compile and run the application.</span></span> <span data-ttu-id="796f9-159">Çalıştıran bir sunucuda adresi konsol penceresinde göstermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="796f9-159">The address that the server is running should show in a console window.</span></span>

    ![](tutorial-signalr-self-host/_static/image2.png)
7. <span data-ttu-id="796f9-160">Yürütme özel durumla başarısız olursa `System.Reflection.TargetInvocationException was unhandled`, Visual Studio'nun yönetici ayrıcalıklarıyla yeniden başlatmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="796f9-160">If execution fails with the exception `System.Reflection.TargetInvocationException was unhandled`, you will need to restart Visual Studio with administrator privileges.</span></span>
8. <span data-ttu-id="796f9-161">Sonraki bölüme devam etmeden önce uygulamayı durdurun.</span><span class="sxs-lookup"><span data-stu-id="796f9-161">Stop the application before proceeding to the next section.</span></span>

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a><span data-ttu-id="796f9-162">Sunucu bir JavaScript istemci ile erişme</span><span class="sxs-lookup"><span data-stu-id="796f9-162">Accessing the server with a JavaScript client</span></span>

<span data-ttu-id="796f9-163">Bu bölümde, aynı JavaScript istemciden kullanacağınız [Başlarken Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="796f9-163">In this section, you'll use the same JavaScript client from the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="796f9-164">Biz yalnızca hub URL'sini açıkça tanımlamaktır istemci yapılan bir değişikliği yapacağız.</span><span class="sxs-lookup"><span data-stu-id="796f9-164">We'll only make one modification to the client, which is to explicitly define the hub URL.</span></span> <span data-ttu-id="796f9-165">Kendini barındıran bir uygulamayla sunucu mutlaka bağlantı URL'si (nedeniyle, ters proxy ve yük Dengeleyiciler) ile aynı adresindeki şekilde URL açıkça tanımlanmış olması gerekiyor olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="796f9-165">With a self-hosted application, the server may not necessarily be at the same address as the connection URL (due to reverse proxies and load balancers), so the URL needs to be defined explicitly.</span></span>

1. <span data-ttu-id="796f9-166">İçinde **Çözüm Gezgini**, çözüm üzerinde sağ tıklatın ve seçin **Ekle**, **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="796f9-166">In **Solution Explorer**, right-click on the solution and select **Add**, **New Project**.</span></span> <span data-ttu-id="796f9-167">Seçin **Web** düğümü ve select **ASP.NET Web uygulaması** şablonu.</span><span class="sxs-lookup"><span data-stu-id="796f9-167">Select the **Web** node, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="796f9-168">"JavascriptClient" adını verin ve projeyi tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="796f9-168">Name the project "JavascriptClient" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image3.png)
2. <span data-ttu-id="796f9-169">Seçin **boş** şablonu ve geri kalan seçenekler seçilmemiş olarak bırakın.</span><span class="sxs-lookup"><span data-stu-id="796f9-169">Select the **Empty** template, and leave the remaining options unselected.</span></span> <span data-ttu-id="796f9-170">Seçin **projesi oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="796f9-170">Select **Create Project**.</span></span>

    ![](tutorial-signalr-self-host/_static/image4.png)
3. <span data-ttu-id="796f9-171">Paket Yöneticisi Konsolu "JavascriptClient" projesinde seçin **varsayılan proje** aşağı açılır ve aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="796f9-171">In the package manager console, select the "JavascriptClient" project in the **Default project** drop-down, and execute the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    <span data-ttu-id="796f9-172">Bu komut istemcisinde ihtiyacınız vardır SignalR ve JQuery kitaplıklarını yükler.</span><span class="sxs-lookup"><span data-stu-id="796f9-172">This command installs the SignalR and JQuery libraries that you'll need in the client.</span></span>
4. <span data-ttu-id="796f9-173">Seçin ve proje üzerinde sağ **Ekle**, **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="796f9-173">Right-click on your project and select **Add**, **New Item**.</span></span> <span data-ttu-id="796f9-174">Seçin **Web** düğümü ve select HTML sayfası.</span><span class="sxs-lookup"><span data-stu-id="796f9-174">Select the **Web** node, and select HTML Page.</span></span> <span data-ttu-id="796f9-175">Sayfa adı **Default.html**.</span><span class="sxs-lookup"><span data-stu-id="796f9-175">Name the page **Default.html**.</span></span>

    ![](tutorial-signalr-self-host/_static/image5.png)
5. <span data-ttu-id="796f9-176">Yeni HTML sayfası içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="796f9-176">Replace the contents of the new HTML page with the following code.</span></span> <span data-ttu-id="796f9-177">Komut dosyası başvuruları burada proje betikleri klasöründe bulunan komut dosyaları eşleştiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="796f9-177">Verify that the script references here match the scripts in the Scripts folder of the project.</span></span>

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    <span data-ttu-id="796f9-178">(Yukarıdaki kod örneğinde vurgulanan) aşağıdaki kodu (ek olarak kod SignalR sürüm 2 beta sürümünden yükseltme) alma Stared öğreticide kullanılan istemci yaptığınız ektir.</span><span class="sxs-lookup"><span data-stu-id="796f9-178">The following code (highlighted in the code sample above) is the addition that you've made to the client used in the Getting Stared tutorial (in addition to upgrading the code to SignalR version 2 beta).</span></span> <span data-ttu-id="796f9-179">Bu kod satırı temel bağlantı URL'si sunucuda SignalR için açık olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="796f9-179">This line of code explicitly sets the base connection URL for SignalR on the server.</span></span>

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. <span data-ttu-id="796f9-180">Çözüm üzerinde sağ tıklatın ve seçin **başlangıç projelerini Ayarla...** . Seçin **birden fazla başlangıç projesi** radyo düğmesinin ve her iki proje ayarlayın **eylem** için **Başlat**.</span><span class="sxs-lookup"><span data-stu-id="796f9-180">Right-click on the solution, and select **Set Startup Projects...**. Select the **Multiple startup projects** radio button, and set both projects' **Action** to **Start**.</span></span>

    ![](tutorial-signalr-self-host/_static/image6.png)
7. <span data-ttu-id="796f9-181">"Default.html üzerinde" sağ tıklatıp **başlangıç sayfası olarak ayarla**.</span><span class="sxs-lookup"><span data-stu-id="796f9-181">Right-click on "Default.html" and select **Set As Start Page**.</span></span>
8. <span data-ttu-id="796f9-182">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="796f9-182">Run the application.</span></span> <span data-ttu-id="796f9-183">Sunucu ve sayfa başlatır.</span><span class="sxs-lookup"><span data-stu-id="796f9-183">The server and page will launch.</span></span> <span data-ttu-id="796f9-184">Web sayfasını yeniden yüklemeniz gerekebilir (veya seçin **devam** hata ayıklayıcı) server başlatılmadan önce sayfa yüklerse.</span><span class="sxs-lookup"><span data-stu-id="796f9-184">You may need to reload the web page (or select **Continue** in the debugger) if the page loads before the server is started.</span></span>
9. <span data-ttu-id="796f9-185">Tarayıcıda, istendiğinde bir kullanıcı adını belirtin.</span><span class="sxs-lookup"><span data-stu-id="796f9-185">In the browser, provide a username when prompted.</span></span> <span data-ttu-id="796f9-186">Başka bir tarayıcı sekmesinde veya penceresinde sayfanın URL'sini kopyalayın ve farklı bir kullanıcı adı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="796f9-186">Copy the page's URL into another browser tab or window and provide a different username.</span></span> <span data-ttu-id="796f9-187">Diğer, Başlarken Öğreticisi olduğu gibi bir tarayıcı bölmesinden iletileri göndermek kuramaz.</span><span class="sxs-lookup"><span data-stu-id="796f9-187">You will be able to send messages from one browser pane to the other, as in the Getting Started tutorial.</span></span>
