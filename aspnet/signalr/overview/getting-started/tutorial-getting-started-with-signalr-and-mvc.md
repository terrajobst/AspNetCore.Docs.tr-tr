---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: "Öğretici: SignalR 2 ve MVC 5 ile çalışmaya başlama | Microsoft Docs"
author: pfletcher
description: "Bu öğretici ASP.NET SignalR 2 gerçek zamanlı sohbet uygulaması oluşturmak için nasıl kullanılacağını gösterir. Bir MVC 5 uygulamaya SignalR ekleyebilir ve sohbet Görünüm Oluştur..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: 96a6f6446b26d96b2bcffe4354375ab9c444bbbb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a><span data-ttu-id="b4980-104">Öğretici: SignalR 2 ve MVC 5 ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="b4980-104">Tutorial: Getting Started with SignalR 2 and MVC 5</span></span>
====================
<span data-ttu-id="b4980-105">tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="b4980-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[<span data-ttu-id="b4980-106">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="b4980-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> <span data-ttu-id="b4980-107">Bu öğretici ASP.NET SignalR 2 gerçek zamanlı sohbet uygulaması oluşturmak için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="b4980-107">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="b4980-108">Bir MVC 5 uygulamaya SignalR ekleyebilir ve göndermek ve iletileri görüntülemek için bir sohbet görünüm oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b4980-108">You will add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span> 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b4980-109">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="b4980-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="b4980-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b4980-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="b4980-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="b4980-111">.NET 4.5</span></span>
> - <span data-ttu-id="b4980-112">MVC 5</span><span class="sxs-lookup"><span data-stu-id="b4980-112">MVC 5</span></span>
> - <span data-ttu-id="b4980-113">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="b4980-113">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="b4980-114">Bu öğretici ile Visual Studio 2012 kullanma</span><span class="sxs-lookup"><span data-stu-id="b4980-114">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="b4980-115">Visual Studio 2012 bu öğreticiyle kullanmak için aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="b4980-115">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="b4980-116">Güncelleştirme, [Paket Yöneticisi](http://docs.nuget.org/docs/start-here/installing-nuget) en son sürüme.</span><span class="sxs-lookup"><span data-stu-id="b4980-116">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="b4980-117">Yükleme [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="b4980-117">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="b4980-118">Web Platformu Yükleyicisi'nde arayın ve yükleyin **ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için**.</span><span class="sxs-lookup"><span data-stu-id="b4980-118">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="b4980-119">Bu SignalR sınıfları için Visual Studio şablonları gibi yükleyecek **Hub**.</span><span class="sxs-lookup"><span data-stu-id="b4980-119">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="b4980-120">Bazı şablonlar (gibi **OWIN başlangıç sınıfı**); kullanılamaz bunlar için bunun yerine bir sınıf dosyası kullanın.</span><span class="sxs-lookup"><span data-stu-id="b4980-120">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="b4980-121">Eğitmen sürümleri</span><span class="sxs-lookup"><span data-stu-id="b4980-121">Tutorial Versions</span></span>
> 
> <span data-ttu-id="b4980-122">SignalR daha önceki sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="b4980-122">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="b4980-123">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="b4980-123">Questions and comments</span></span>
> 
> <span data-ttu-id="b4980-124">Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi.</span><span class="sxs-lookup"><span data-stu-id="b4980-124">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="b4980-125">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="b4980-125">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="b4980-126">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="b4980-126">Overview</span></span>

<span data-ttu-id="b4980-127">Bu öğreticide, ASP.NET SignalR 2 ve ASP.NET MVC 5 ile gerçek zamanlı web uygulaması geliştirme tanıtır.</span><span class="sxs-lookup"><span data-stu-id="b4980-127">This tutorial introduces you to real-time web application development with ASP.NET SignalR 2 and ASP.NET MVC 5.</span></span> <span data-ttu-id="b4980-128">Öğretici aynı sohbet uygulama kodunu kullanır [SignalR Başlarken Öğreticisi](tutorial-getting-started-with-signalr.md), ancak bir MVC 5 uygulama eklemek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="b4980-128">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 5 application.</span></span>

<span data-ttu-id="b4980-129">Bu konuda aşağıdaki SignalR geliştirme görevleri öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="b4980-129">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="b4980-130">SignalR kitaplığı MVC 5 uygulamaya ekleme.</span><span class="sxs-lookup"><span data-stu-id="b4980-130">Adding the SignalR library to an MVC 5 application.</span></span>
- <span data-ttu-id="b4980-131">Hub ve içeriği istemcilere göndermek için OWIN başlangıç sınıfları oluşturma.</span><span class="sxs-lookup"><span data-stu-id="b4980-131">Creating hub and OWIN startup classes to push content to clients.</span></span>
- <span data-ttu-id="b4980-132">İleti gönderme ve hub güncelleştirmelerini görüntülemek için bir web sayfasında SignalR jQuery kitaplığı kullanma.</span><span class="sxs-lookup"><span data-stu-id="b4980-132">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="b4980-133">Aşağıdaki ekran görüntüsünde bir tarayıcıda çalışan tamamlanmış sohbet uygulamayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="b4980-133">The following screen shot shows the completed chat application running in a browser.</span></span>

![Sohbet örnekleri](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

<span data-ttu-id="b4980-135">Bölümler:</span><span class="sxs-lookup"><span data-stu-id="b4980-135">Sections:</span></span>

- [<span data-ttu-id="b4980-136">Projesi ayarlayın</span><span class="sxs-lookup"><span data-stu-id="b4980-136">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="b4980-137">Örnek çalıştırın</span><span class="sxs-lookup"><span data-stu-id="b4980-137">Run the Sample</span></span>](#run)
- [<span data-ttu-id="b4980-138">Kodu inceleyin</span><span class="sxs-lookup"><span data-stu-id="b4980-138">Examine the Code</span></span>](#code)
- [<span data-ttu-id="b4980-139">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="b4980-139">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="b4980-140">Projesi ayarlayın</span><span class="sxs-lookup"><span data-stu-id="b4980-140">Set up the Project</span></span>

<span data-ttu-id="b4980-141">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="b4980-141">Prerequisites:</span></span>

- <span data-ttu-id="b4980-142">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="b4980-142">Visual Studio 2013.</span></span> <span data-ttu-id="b4980-143">Visual Studio yoksa bkz [ASP.NET indirmeleri](https://www.asp.net/downloads) ücretsiz Visual Studio 2013 Express geliştirme aracı alınamıyor.</span><span class="sxs-lookup"><span data-stu-id="b4980-143">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express Development Tool.</span></span>

<span data-ttu-id="b4980-144">Bu bölümde, bir ASP.NET MVC 5 uygulaması oluşturmak, SignalR Kitaplığı eklemek ve sohbet uygulaması oluşturma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="b4980-144">This section shows how to create an ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="b4980-145">Visual Studio'da .NET Framework 4.5 hedefleyen bir C# ASP.NET uygulaması oluşturun, SignalRChat adlandırın ve Tamam'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="b4980-145">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![Web oluşturulamıyor](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. <span data-ttu-id="b4980-147">İçinde `New ASP.NET Project` iletişim ve select **MVC**, tıklatıp **kimlik doğrulamayı Değiştir**.</span><span class="sxs-lookup"><span data-stu-id="b4980-147">In the `New ASP.NET Project` dialog, and select **MVC**, and click **Change Authentication**.</span></span>

    ![Web oluşturulamıyor](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. <span data-ttu-id="b4980-149">Seçin **doğrulaması yok** içinde **kimlik doğrulamayı Değiştir** iletişim ve tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="b4980-149">Select **No Authentication** in the **Change Authentication** dialog, and click **OK**.</span></span>

    ![Hiçbir kimlik doğrulama yöntemini seçin](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > <span data-ttu-id="b4980-151">Uygulamanız için farklı kimlik doğrulama sağlayıcısı seçerseniz bir `Startup.cs` sınıfı sizin için oluşturulacaktır; kendi oluşturmanız gerekmez `Startup.cs` 10 aşağıdaki adımda sınıfı.</span><span class="sxs-lookup"><span data-stu-id="b4980-151">If you select a different authentication provider for your application, a `Startup.cs` class will be created for you; you will not need to create your own `Startup.cs` class in step 10 below.</span></span>
4. <span data-ttu-id="b4980-152">Tıklatın **Tamam** içinde **yeni ASP.NET projesi** iletişim.</span><span class="sxs-lookup"><span data-stu-id="b4980-152">Click **OK** in the **New ASP.NET Project** dialog.</span></span>
5. <span data-ttu-id="b4980-153">Açık **Araçlar | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu** ve aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b4980-153">Open the **Tools | Library Package Manager | Package Manager Console** and run the following command.</span></span> <span data-ttu-id="b4980-154">Bu adım, bir dizi komut dosyaları ve SignalR işlevselliğini etkinleştirmek derleme başvurularını projeye ekler.</span><span class="sxs-lookup"><span data-stu-id="b4980-154">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

    `install-package Microsoft.AspNet.SignalR`
6. <span data-ttu-id="b4980-155">İçinde **Çözüm Gezgini**, komut dosyaları klasörünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="b4980-155">In **Solution Explorer**, expand the Scripts folder.</span></span> <span data-ttu-id="b4980-156">SignalR için komut dosyası kitaplıkları projeye eklendiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="b4980-156">Note that script libraries for SignalR have been added to the project.</span></span>

    ![Betik klasörü](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. <span data-ttu-id="b4980-158">İçinde **Çözüm Gezgini**, projeye sağ tıklayın, seçin **Ekle | Yeni klasör**, ve adlı yeni bir klasör ekleyin **hub**.</span><span class="sxs-lookup"><span data-stu-id="b4980-158">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
8. <span data-ttu-id="b4980-159">Sağ **hub** klasörünü tıklatın **Ekle | Yeni öğe**seçin **Visual C# | Web | SignalR** düğümünde **yüklü** bölmesinde, **SignalR hub'ı sınıfı (v2)** center bölmesinden ve adlı yeni bir hub'ı oluşturma **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="b4980-159">Right-click the **Hubs** folder, click **Add | New Item**, select the **Visual C# | Web | SignalR** node in the **Installed** pane, select **SignalR Hub Class (v2)** from the center pane, and create a new hub named **ChatHub.cs**.</span></span> <span data-ttu-id="b4980-160">Bu sınıf, tüm istemcilere iletileri gönderen bir SignalR sunucu hub olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="b4980-160">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

    ![Yeni hub'ı Oluştur](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. <span data-ttu-id="b4980-162">Kodla **ChatHub** aşağıdaki kodla sınıfı.</span><span class="sxs-lookup"><span data-stu-id="b4980-162">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. <span data-ttu-id="b4980-163">Haline adlı yeni bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b4980-163">Create a new class called Startup.cs.</span></span> <span data-ttu-id="b4980-164">Dosyanın içeriği aşağıdaki gibi değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b4980-164">Change the contents of the file to the following.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. <span data-ttu-id="b4980-165">Düzen `HomeController` sınıfı bulunan **Controllers/HomeController.cs** ve sınıfına aşağıdaki yöntemi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b4980-165">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="b4980-166">Bu yöntem **sohbet** bir sonraki adımda oluşturacağınız görünümü.</span><span class="sxs-lookup"><span data-stu-id="b4980-166">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. <span data-ttu-id="b4980-167">Sağ **görünümler/giriş** klasörü ve select **Ekle... | Görünüm**.</span><span class="sxs-lookup"><span data-stu-id="b4980-167">Right-click the **Views/Home** folder, and select **Add... | View**.</span></span>
13. <span data-ttu-id="b4980-168">İçinde **Görünüm Ekle** iletişim kutusunda, yeni görünüm adı **sohbet**.</span><span class="sxs-lookup"><span data-stu-id="b4980-168">In the **Add View** dialog, name the new view **Chat**.</span></span>

    ![Bir görünüm ekleyin](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. <span data-ttu-id="b4980-170">Değiştir **Chat.cshtml** aşağıdaki kod ile.</span><span class="sxs-lookup"><span data-stu-id="b4980-170">Replace the contents of **Chat.cshtml** with the following code.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="b4980-171">Visual Studio projenizi SignalR ve diğer betik kitaplıkları eklediğinizde, Paket Yöneticisi SignalR komut dosyasının bu konuda gösterilen sürümden daha yeni bir sürümünü yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b4980-171">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install a version of the SignalR script file that is more recent than the version shown in this topic.</span></span> <span data-ttu-id="b4980-172">Komut başvurusu, kodunuzda projenizde yüklü kod kitaplığı sürümü eşleştiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="b4980-172">Make sure that the script reference in your code matches the version of the script library installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. <span data-ttu-id="b4980-173">**Tümünü Kaydet** projesi için.</span><span class="sxs-lookup"><span data-stu-id="b4980-173">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="b4980-174">Örnek çalıştırın</span><span class="sxs-lookup"><span data-stu-id="b4980-174">Run the Sample</span></span>

1. <span data-ttu-id="b4980-175">Projeyi hata ayıklama modunda çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="b4980-175">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="b4980-176">Tarayıcının adres satırındaki append **/home/sohbet** proje için varsayılan sayfasının URL'si için.</span><span class="sxs-lookup"><span data-stu-id="b4980-176">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="b4980-177">Bir tarayıcı örneği ve bir kullanıcı adı için ister sohbet sayfa yükler.</span><span class="sxs-lookup"><span data-stu-id="b4980-177">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![Kullanıcı adı girin](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. <span data-ttu-id="b4980-179">Bir kullanıcı adı girin.</span><span class="sxs-lookup"><span data-stu-id="b4980-179">Enter a user name.</span></span>
4. <span data-ttu-id="b4980-180">Tarayıcının adres satırından URL'yi kopyalayın ve iki daha fazla tarayıcı örnekleri açmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="b4980-180">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="b4980-181">Her tarayıcı örnek benzersiz bir kullanıcı adı girin.</span><span class="sxs-lookup"><span data-stu-id="b4980-181">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="b4980-182">Her tarayıcı örnek bir açıklama ekleyin ve **Gönder**.</span><span class="sxs-lookup"><span data-stu-id="b4980-182">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="b4980-183">Açıklamaları tüm tarayıcı örnekleri görüntülemelidir.</span><span class="sxs-lookup"><span data-stu-id="b4980-183">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b4980-184">Bu basit sohbet uygulama sunucusunda tartışma bağlam korumaz.</span><span class="sxs-lookup"><span data-stu-id="b4980-184">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="b4980-185">Hub'ın tüm geçerli kullanıcı yorumları yayınlar.</span><span class="sxs-lookup"><span data-stu-id="b4980-185">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="b4980-186">Sohbet daha sonra katılmak kullanıcılar zamandan eklenen iletilerini bunlar katılma görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="b4980-186">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="b4980-187">Aşağıdaki ekran görüntüsünde bir tarayıcıda çalışan sohbet uygulamayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="b4980-187">The following screen shot shows the chat application running in a browser.</span></span>

    ![Sohbet tarayıcılar](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. <span data-ttu-id="b4980-189">İçinde **Çözüm Gezgini**, inceleme **betik belgelerini** çalışan uygulama için düğüm.</span><span class="sxs-lookup"><span data-stu-id="b4980-189">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="b4980-190">Tarayıcı olarak Internet Explorer kullanıyorsanız, bu hata ayıklama modunda görünür bir düğümdür.</span><span class="sxs-lookup"><span data-stu-id="b4980-190">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="b4980-191">Adlı bir komut dosyası **hub** , SignalR kitaplık çalışma zamanında dinamik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b4980-191">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="b4980-192">Bu dosya, jQuery betik sunucu tarafı kodu arasında iletişim yönetir.</span><span class="sxs-lookup"><span data-stu-id="b4980-192">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="b4980-193">Internet Explorer dışında bir tarayıcı kullanıyorsanız, dinamik da erişebilirsiniz **hub** kendisine doğrudan, örneğin http://mywebsite/signalr/hubs giderek dosya.</span><span class="sxs-lookup"><span data-stu-id="b4980-193">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="b4980-194">Kodu inceleyin</span><span class="sxs-lookup"><span data-stu-id="b4980-194">Examine the Code</span></span>

<span data-ttu-id="b4980-195">SignalR sohbet uygulaması iki temel SignalR geliştirme görevlerini gösterir: sunucu üzerindeki ana koordinasyon nesnesi olarak bir hub'ı oluşturma ve ileti gönderme ve alma için SignalR jQuery kitaplığı kullanma.</span><span class="sxs-lookup"><span data-stu-id="b4980-195">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="b4980-196">SignalR hub'ları</span><span class="sxs-lookup"><span data-stu-id="b4980-196">SignalR Hubs</span></span>

<span data-ttu-id="b4980-197">Kod örneğinde **ChatHub** sınıfı türer **Microsoft.AspNet.SignalR.Hub** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="b4980-197">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="b4980-198">Türetme **Hub** bir SignalR uygulaması oluşturmak için kullanışlı bir yol bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="b4980-198">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="b4980-199">Genel yöntemler hub sınıfınız oluşturun ve sonra bu yöntemler bir web sayfasında komut dosyalarından çağırarak erişim.</span><span class="sxs-lookup"><span data-stu-id="b4980-199">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="b4980-200">İstemciler sohbet kodda çağrısı **ChatHub.Send** yeni bir ileti göndermek için yöntem.</span><span class="sxs-lookup"><span data-stu-id="b4980-200">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="b4980-201">Hub sırayla iletiyi tüm istemcilere çağırarak gönderir **Clients.All.addNewMessageToPage**.</span><span class="sxs-lookup"><span data-stu-id="b4980-201">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="b4980-202">**Gönder** yöntemi birkaç hub kavramları göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="b4980-202">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="b4980-203">Böylece istemciler bunları çağırabilirsiniz genel yöntemler bir hub'ına bildirin.</span><span class="sxs-lookup"><span data-stu-id="b4980-203">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="b4980-204">Kullanım **Microsoft.AspNet.SignalR.Hub.Clients** tüm istemciler erişmek için özelliğini bu hub'ına bağlı.</span><span class="sxs-lookup"><span data-stu-id="b4980-204">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="b4980-205">İstemcide bir işlevi çağırmak (gibi `addNewMessageToPage` işlevi) istemcilerini güncelleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="b4980-205">Call a function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="b4980-206">SignalR ve jQuery</span><span class="sxs-lookup"><span data-stu-id="b4980-206">SignalR and jQuery</span></span>

<span data-ttu-id="b4980-207">**Chat.cshtml** görünüm dosyası kod örneğinde SignalR jQuery kitaplığı bir SignalR hub ile iletişim kurmak için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="b4980-207">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="b4980-208">Kodda önemli görevleri hub'ına iletileri göndermek için sunucunun istemcilere anında içeriği için çağırabilirsiniz işlevi bildirme ve bağlantı başlatma otomatik olarak oluşturulan Proxy hub'ına yönelik bir başvuru oluşturuyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="b4980-208">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="b4980-209">Aşağıdaki kod, bir hub proxy için bir başvuru bildirir.</span><span class="sxs-lookup"><span data-stu-id="b4980-209">The following code declares a reference to a hub proxy.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="b4980-210">JavaScript'te sunucu sınıfı ve üyelerini içinde ortası büyük başvurudur.</span><span class="sxs-lookup"><span data-stu-id="b4980-210">In JavaScript the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="b4980-211">Kod örneği C# başvuran **ChatHub** JavaScript sınıfında **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="b4980-211">The code sample references the C# **ChatHub** class in JavaScript as **chatHub**.</span></span> <span data-ttu-id="b4980-212">Başvuru istiyorsanız `ChatHub` sınıfı jQuery geleneksel Pascal ile C# ' ta yaptığınız gibi büyük/küçük harfleri ChatHub.cs sınıfı dosyasını düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="b4980-212">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="b4980-213">Ekleme bir `using` başvurmak için deyimi `Microsoft.AspNet.SignalR.Hubs` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="b4980-213">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="b4980-214">Ardından ekleyin `HubName` özniteliğini `ChatHub` sınıfı, örneğin `[HubName("ChatHub")]`.</span><span class="sxs-lookup"><span data-stu-id="b4980-214">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="b4980-215">Son olarak, jQuery referansı güncelleştirme `ChatHub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="b4980-215">Finally, update your jQuery reference to the `ChatHub` class.</span></span>


<span data-ttu-id="b4980-216">Aşağıdaki kod komut dosyasındaki bir geri çağırma işlevini oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="b4980-216">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="b4980-217">Sunucudaki hub sınıfı içerik güncelleştirmeleri her bir istemciye göndermek için bu işlevi çağırır.</span><span class="sxs-lookup"><span data-stu-id="b4980-217">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="b4980-218">İsteğe bağlı arama `htmlEncode` kod eklemesini önlemeye yönelik bir yolu olarak sayfasını görüntülemeden önce ileti içeriği kodlamak HTML şekilde işlev gösterir.</span><span class="sxs-lookup"><span data-stu-id="b4980-218">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="b4980-219">Aşağıdaki kod, hub ile bir bağlantı açmak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="b4980-219">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="b4980-220">Kod bağlantı başlar ve üzerinde click olayını işlemek için bir işlev geçirir **Gönder** sohbet sayfasının düğmesini.</span><span class="sxs-lookup"><span data-stu-id="b4980-220">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="b4980-221">Bu yaklaşım, olay işleyici yürütülmeden önce bağlantı kurulana sağlar.</span><span class="sxs-lookup"><span data-stu-id="b4980-221">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="b4980-222">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="b4980-222">Next Steps</span></span>

<span data-ttu-id="b4980-223">SignalR gerçek zamanlı web uygulamaları oluşturmak için bir çerçeve olduğunu öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="b4980-223">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="b4980-224">Birkaç SignalR geliştirme görevleri de öğrenilen: bir ASP.NET uygulaması için SignalR ekleme, hub sınıfın nasıl oluşturulacağı ve hub'dan ileti alıp göndermek nasıl.</span><span class="sxs-lookup"><span data-stu-id="b4980-224">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="b4980-225">Örnek SignalR uygulamayı Azure'a dağıtma hakkında bir kılavuz için bkz: [SignalR kullanarak Azure App Service'te Web uygulamalarını](../deployment/using-signalr-with-azure-web-sites.md).</span><span class="sxs-lookup"><span data-stu-id="b4980-225">For a walkthrough on how to deploy the sample SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="b4980-226">Windows Azure Web sitesi için bir Visual Studio web projesi dağıtma hakkında ayrıntılı bilgi için bkz: [Azure App Service'te bir ASP.NET web uygulaması oluşturma](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="b4980-226">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<span data-ttu-id="b4980-227">Daha gelişmiş SignalR gelişmeler kavramları hakkında bilgi için SignalR kaynak kodu ve kaynaklar için aşağıdaki siteleri ziyaret edin:</span><span class="sxs-lookup"><span data-stu-id="b4980-227">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="b4980-228">SignalR projesi</span><span class="sxs-lookup"><span data-stu-id="b4980-228">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="b4980-229">SignalR Github ve örnekler</span><span class="sxs-lookup"><span data-stu-id="b4980-229">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="b4980-230">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="b4980-230">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
