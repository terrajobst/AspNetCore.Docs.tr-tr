---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: "Öğretici: SignalR ile çalışmaya başlama 1.x ve MVC 4 | Microsoft Docs"
author: pfletcher
description: "ASP.NET SignalR ve ASP.NET MVC 4 gerçek zamanlı sohbet uygulaması oluşturmak için kullanın."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/29/2013
ms.topic: article
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: e678c85520613fea2a8d00de60aca04d895d6307
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a><span data-ttu-id="7c852-103">Öğretici: SignalR ile çalışmaya başlama 1.x ve MVC 4</span><span class="sxs-lookup"><span data-stu-id="7c852-103">Tutorial: Getting Started with SignalR 1.x and MVC 4</span></span>
====================
<span data-ttu-id="7c852-104">tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="7c852-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

> <span data-ttu-id="7c852-105">Bu öğreticide, ASP.NET SignalR gerçek zamanlı sohbet uygulaması oluşturmak için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="7c852-105">This tutorial shows how to use ASP.NET SignalR to create a real-time chat application.</span></span> <span data-ttu-id="7c852-106">MVC 4 uygulama SignalR eklemek ve göndermek ve iletileri görüntülemek için bir sohbet görünüm oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7c852-106">You will add SignalR to an MVC 4 application and create a chat view to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="7c852-107">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="7c852-107">Overview</span></span>

<span data-ttu-id="7c852-108">Bu öğreticide, ASP.NET SignalR ve ASP.NET MVC 4 ile gerçek zamanlı web uygulaması geliştirme tanıtır.</span><span class="sxs-lookup"><span data-stu-id="7c852-108">This tutorial introduces you to real-time web application development with ASP.NET SignalR and ASP.NET MVC 4.</span></span> <span data-ttu-id="7c852-109">Öğretici aynı sohbet uygulama kodunu kullanır [SignalR Başlarken Öğreticisi](tutorial-getting-started-with-signalr.md), ancak Internet şablona dayalı bir MVC 4 uygulama eklemek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="7c852-109">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 4 application based on the Internet template.</span></span>

<span data-ttu-id="7c852-110">Bu konuda aşağıdaki SignalR geliştirme görevleri öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="7c852-110">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="7c852-111">SignalR kitaplığı MVC 4 uygulamaya ekleme.</span><span class="sxs-lookup"><span data-stu-id="7c852-111">Adding the SignalR library to an MVC 4 application.</span></span>
- <span data-ttu-id="7c852-112">İçeriği istemcilere göndermek için bir hub sınıfı oluşturma.</span><span class="sxs-lookup"><span data-stu-id="7c852-112">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="7c852-113">İleti gönderme ve hub güncelleştirmelerini görüntülemek için bir web sayfasında SignalR jQuery kitaplığı kullanma.</span><span class="sxs-lookup"><span data-stu-id="7c852-113">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="7c852-114">Aşağıdaki ekran görüntüsünde bir tarayıcıda çalışan tamamlanmış sohbet uygulamayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="7c852-114">The following screen shot shows the completed chat application running in a browser.</span></span>

![Sohbet örnekleri](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

<span data-ttu-id="7c852-116">Bölümler:</span><span class="sxs-lookup"><span data-stu-id="7c852-116">Sections:</span></span>

- [<span data-ttu-id="7c852-117">Projesi ayarlayın</span><span class="sxs-lookup"><span data-stu-id="7c852-117">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="7c852-118">Örnek çalıştırın</span><span class="sxs-lookup"><span data-stu-id="7c852-118">Run the Sample</span></span>](#run)
- [<span data-ttu-id="7c852-119">Kodu inceleyin</span><span class="sxs-lookup"><span data-stu-id="7c852-119">Examine the Code</span></span>](#code)
- [<span data-ttu-id="7c852-120">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="7c852-120">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="7c852-121">Projesi ayarlayın</span><span class="sxs-lookup"><span data-stu-id="7c852-121">Set up the Project</span></span>

<span data-ttu-id="7c852-122">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="7c852-122">Prerequisites:</span></span>

- <span data-ttu-id="7c852-123">Visual Studio 2010 SP1, Visual Studio 2012 veya Visual Studio 2012 Express.</span><span class="sxs-lookup"><span data-stu-id="7c852-123">Visual Studio 2010 SP1, Visual Studio 2012, or Visual Studio 2012 Express.</span></span> <span data-ttu-id="7c852-124">Visual Studio yoksa bkz [ASP.NET indirmeleri](https://www.asp.net/downloads) ücretsiz Visual Studio 2012 Express geliştirme aracı alınamıyor.</span><span class="sxs-lookup"><span data-stu-id="7c852-124">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="7c852-125">Visual Studio 2010 için yükleme [ASP.NET MVC 4](https://www.microsoft.com/en-us/download/details.aspx?id=30683).</span><span class="sxs-lookup"><span data-stu-id="7c852-125">For Visual Studio 2010, install [ASP.NET MVC 4](https://www.microsoft.com/en-us/download/details.aspx?id=30683).</span></span>

<span data-ttu-id="7c852-126">Bu bölümde, bir ASP.NET MVC 4 uygulama oluşturmak, SignalR Kitaplığı eklemek ve sohbet uygulaması oluşturma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="7c852-126">This section shows how to create an ASP.NET MVC 4 application, add the SignalR library, and create the chat application.</span></span>

1. 1. <span data-ttu-id="7c852-127">Visual Studio'da ASP.NET MVC 4 uygulama oluşturma, SignalRChat adlandırın ve Tamam'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="7c852-127">In Visual Studio create an ASP.NET MVC 4 application, name it SignalRChat, and click OK.</span></span>

        > [!NOTE]
        > <span data-ttu-id="7c852-128">VS 2010'da seçin **.NET Framework 4** Framework sürümü açılır denetimindeki.</span><span class="sxs-lookup"><span data-stu-id="7c852-128">In VS 2010, select **.NET Framework 4** in the Framework version dropdown control.</span></span> <span data-ttu-id="7c852-129">SignalR kodu .NET Framework sürüm 4 ve 4.5 çalışır.</span><span class="sxs-lookup"><span data-stu-id="7c852-129">SignalR code runs on .NET Framework versions 4 and 4.5.</span></span>

        ![MVC web oluşturma](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
    2. <span data-ttu-id="7c852-131">Internet uygulaması şablonunu seçin, seçeneğini temizleyin **birim testi projesi oluşturma**, Tamam'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="7c852-131">Select the Internet Application template, clear the option to **Create a unit test project**, and click OK.</span></span>

        ![MVC internet sitesi oluştur](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
    3. <span data-ttu-id="7c852-133">Açık **Araçlar | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu** ve aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7c852-133">Open the **Tools | Library Package Manager | Package Manager Console** and run the following command.</span></span> <span data-ttu-id="7c852-134">Bu adım, bir dizi komut dosyaları ve SignalR işlevselliğini etkinleştirmek derleme başvurularını projeye ekler.</span><span class="sxs-lookup"><span data-stu-id="7c852-134">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

        `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
    4. <span data-ttu-id="7c852-135">İçinde **Çözüm Gezgini** komut dosyaları klasörünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="7c852-135">In **Solution Explorer** expand the Scripts folder.</span></span> <span data-ttu-id="7c852-136">SignalR için komut dosyası kitaplıkları projeye eklendiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="7c852-136">Note that script libraries for SignalR have been added to the project.</span></span>

        ![Kitaplık Başvurusu](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
    5. <span data-ttu-id="7c852-138">İçinde **Çözüm Gezgini**, projeye sağ tıklayın, seçin **Ekle | Yeni klasör**, ve adlı yeni bir klasör ekleyin **hub**.</span><span class="sxs-lookup"><span data-stu-id="7c852-138">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
    6. <span data-ttu-id="7c852-139">Sağ **hub** klasörünü tıklatın **Ekle | Sınıf**ve adlı yeni bir C# sınıfı oluşturmanız **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="7c852-139">Right-click the **Hubs** folder, click **Add | Class**, and create a new C# class named **ChatHub.cs**.</span></span> <span data-ttu-id="7c852-140">Bu sınıf, tüm istemcilere iletileri gönderen bir SignalR sunucu hub olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="7c852-140">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

> [!NOTE]
> <span data-ttu-id="7c852-141">Visual Studio 2012 kullanın ve yüklediyseniz [ASP.NET ve Web Araçları 2012.2 güncelleştirme](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), hub sınıfı oluşturmak için yeni SignalR öğe şablonu kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7c852-141">If you use Visual Studio 2012 and have installed the [ASP.NET and Web Tools 2012.2 update](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), you can use the new SignalR item template to create the hub class.</span></span> <span data-ttu-id="7c852-142">Bunu yapmak için sağ **hub** klasörünü tıklatın **Ekle | Yeni öğe**seçin **SignalR hub'ı sınıfı (v1)**ve sınıf adını **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="7c852-142">To do that, right-click the **Hubs** folder, click **Add | New Item**, select **SignalR Hub Class (v1)**, and name the class **ChatHub.cs**.</span></span>


1. <span data-ttu-id="7c852-143">Kodla **ChatHub** aşağıdaki kodla sınıfı.</span><span class="sxs-lookup"><span data-stu-id="7c852-143">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. <span data-ttu-id="7c852-144">Açık **Global.asax** proje için dosya ve yöntemine bir çağrı ekleyin `RouteTable.Routes.MapHubs();` kod ilk satırı olarak `Application_Start` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7c852-144">Open the **Global.asax** file for the project, and add a call to the method `RouteTable.Routes.MapHubs();` as the first line of code in the `Application_Start` method.</span></span> <span data-ttu-id="7c852-145">Bu kod, SignalR hub'ları için varsayılan rota kaydeder ve diğer yollar kaydetmeden önce çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="7c852-145">This code registers the default route for SignalR hubs and must be called before you register any other routes.</span></span> <span data-ttu-id="7c852-146">Tamamlanan `Application_Start` yöntemi aşağıdaki gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="7c852-146">The completed `Application_Start` method looks like the following example.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. <span data-ttu-id="7c852-147">Düzen `HomeController` sınıfı bulunan **Controllers/HomeController.cs** ve sınıfına aşağıdaki yöntemi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7c852-147">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="7c852-148">Bu yöntem **sohbet** bir sonraki adımda oluşturacağınız görünümü.</span><span class="sxs-lookup"><span data-stu-id="7c852-148">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. <span data-ttu-id="7c852-149">İçinde sağ `Chat` yalnızca oluşturulur ve tıklatın yöntemi **Görünüm Ekle** yeni bir görünüm dosyası oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="7c852-149">Right-click within the `Chat` method you just created, and click **Add View** to create a new view file.</span></span>
5. <span data-ttu-id="7c852-150">İçinde **Görünüm Ekle** iletişim kutusunda, emin olun onay kutusunun seçili için **düzen veya ana sayfa kullanmak** (diğer onay kutularını temizleyin) ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="7c852-150">In the **Add View** dialog, make sure the check box is selected to **Use a layout or master page** (clear the other check boxes), and then click **Add**.</span></span>

    ![Bir görünüm ekleyin](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. <span data-ttu-id="7c852-152">Adlı yeni görünüm dosya düzenleme **Chat.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="7c852-152">Edit the new view file named **Chat.cshtml**.</span></span> <span data-ttu-id="7c852-153">Sonra &lt;h2&gt; etiketi, aşağıdaki yapıştırın &lt;div&gt; bölüm ve `@section scripts` sayfasına kod bloğu.</span><span class="sxs-lookup"><span data-stu-id="7c852-153">After the &lt;h2&gt; tag, paste the following &lt;div&gt; section and `@section scripts` code block into the page.</span></span> <span data-ttu-id="7c852-154">Bu betiği, Sohbet iletilerini göndermek ve sunucu alınan iletileri görüntülemek için sayfanın sağlar.</span><span class="sxs-lookup"><span data-stu-id="7c852-154">This script enables the page to send chat messages and display messages from the server.</span></span> <span data-ttu-id="7c852-155">Sohbet görünüm için tam kod aşağıdaki kod bloğunda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="7c852-155">The complete code for the chat view appears in the following code block.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="7c852-156">Visual Studio projenizi SignalR ve diğer betik kitaplıkları eklediğinizde, Paket Yöneticisi bu konuda gösterilen sürümlerinden daha yeni olan komutlar sürümlerini yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7c852-156">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install versions of the scripts that are more recent than the versions shown in this topic.</span></span> <span data-ttu-id="7c852-157">Komut dosyası başvuruları kodunuzda projenizde yüklü betik kitaplıkları sürümleri eşleştiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="7c852-157">Make sure that the script references in your code match the versions of the script libraries installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. <span data-ttu-id="7c852-158">**Tümünü Kaydet** projesi için.</span><span class="sxs-lookup"><span data-stu-id="7c852-158">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="7c852-159">Örnek çalıştırın</span><span class="sxs-lookup"><span data-stu-id="7c852-159">Run the Sample</span></span>

1. <span data-ttu-id="7c852-160">Projeyi hata ayıklama modunda çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="7c852-160">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="7c852-161">Tarayıcının adres satırındaki append **/home/sohbet** proje için varsayılan sayfasının URL'si için.</span><span class="sxs-lookup"><span data-stu-id="7c852-161">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="7c852-162">Bir tarayıcı örneği ve bir kullanıcı adı için ister sohbet sayfa yükler.</span><span class="sxs-lookup"><span data-stu-id="7c852-162">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![Kullanıcı adı girin](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. <span data-ttu-id="7c852-164">Bir kullanıcı adı girin.</span><span class="sxs-lookup"><span data-stu-id="7c852-164">Enter a user name.</span></span>
4. <span data-ttu-id="7c852-165">Tarayıcının adres satırından URL'yi kopyalayın ve iki daha fazla tarayıcı örnekleri açmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="7c852-165">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="7c852-166">Her tarayıcı örnek benzersiz bir kullanıcı adı girin.</span><span class="sxs-lookup"><span data-stu-id="7c852-166">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="7c852-167">Her tarayıcı örnek bir açıklama ekleyin ve **Gönder**.</span><span class="sxs-lookup"><span data-stu-id="7c852-167">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="7c852-168">Açıklamaları tüm tarayıcı örnekleri görüntülemelidir.</span><span class="sxs-lookup"><span data-stu-id="7c852-168">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7c852-169">Bu basit sohbet uygulama sunucusunda tartışma bağlam korumaz.</span><span class="sxs-lookup"><span data-stu-id="7c852-169">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="7c852-170">Hub'ın tüm geçerli kullanıcı yorumları yayınlar.</span><span class="sxs-lookup"><span data-stu-id="7c852-170">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="7c852-171">Sohbet daha sonra katılmak kullanıcılar zamandan eklenen iletilerini bunlar katılma görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="7c852-171">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="7c852-172">Aşağıdaki ekran görüntüsünde bir tarayıcıda çalışan sohbet uygulamayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="7c852-172">The following screen shot shows the chat application running in a browser.</span></span>

    ![Sohbet tarayıcılar](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. <span data-ttu-id="7c852-174">İçinde **Çözüm Gezgini**, inceleme **betik belgelerini** çalışan uygulama için düğüm.</span><span class="sxs-lookup"><span data-stu-id="7c852-174">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="7c852-175">Tarayıcı olarak Internet Explorer kullanıyorsanız, bu hata ayıklama modunda görünür bir düğümdür.</span><span class="sxs-lookup"><span data-stu-id="7c852-175">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="7c852-176">Adlı bir komut dosyası **hub** , SignalR kitaplık çalışma zamanında dinamik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7c852-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="7c852-177">Bu dosya, jQuery betik sunucu tarafı kodu arasında iletişim yönetir.</span><span class="sxs-lookup"><span data-stu-id="7c852-177">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="7c852-178">Internet Explorer dışında bir tarayıcı kullanıyorsanız, dinamik da erişebilirsiniz **hub** kendisine doğrudan, örneğin http://mywebsite/signalr/hubs giderek dosya.</span><span class="sxs-lookup"><span data-stu-id="7c852-178">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

    ![Oluşturulan hub komut dosyası](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="7c852-180">Kodu inceleyin</span><span class="sxs-lookup"><span data-stu-id="7c852-180">Examine the Code</span></span>

<span data-ttu-id="7c852-181">SignalR sohbet uygulaması iki temel SignalR geliştirme görevlerini gösterir: sunucu üzerindeki ana koordinasyon nesnesi olarak bir hub'ı oluşturma ve ileti gönderme ve alma için SignalR jQuery kitaplığı kullanma.</span><span class="sxs-lookup"><span data-stu-id="7c852-181">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="7c852-182">SignalR hub'ları</span><span class="sxs-lookup"><span data-stu-id="7c852-182">SignalR Hubs</span></span>

<span data-ttu-id="7c852-183">Kod örneğinde **ChatHub** sınıfı türer **Microsoft.AspNet.SignalR.Hub** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="7c852-183">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="7c852-184">Türetme **Hub** bir SignalR uygulaması oluşturmak için kullanışlı bir yol bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="7c852-184">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="7c852-185">Genel yöntemler hub sınıfınız oluşturun ve sonra bu yöntemler bir web sayfasında jQuery komut dosyalarından çağırarak erişim.</span><span class="sxs-lookup"><span data-stu-id="7c852-185">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="7c852-186">İstemciler sohbet kodda çağrısı **ChatHub.Send** yeni bir ileti göndermek için yöntem.</span><span class="sxs-lookup"><span data-stu-id="7c852-186">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="7c852-187">Hub sırayla iletiyi tüm istemcilere çağırarak gönderir **Clients.All.addNewMessageToPage**.</span><span class="sxs-lookup"><span data-stu-id="7c852-187">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="7c852-188">**Gönder** yöntemi birkaç hub kavramları göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="7c852-188">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="7c852-189">Böylece istemciler bunları çağırabilirsiniz genel yöntemler bir hub'ına bildirin.</span><span class="sxs-lookup"><span data-stu-id="7c852-189">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="7c852-190">Kullanım **Microsoft.AspNet.SignalR.Hub.Clients** tüm istemciler erişmek için özelliğini bu hub'ına bağlı.</span><span class="sxs-lookup"><span data-stu-id="7c852-190">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="7c852-191">İstemcide bir jQuery işlevi çağırma (aşağıdaki gibi `addNewMessageToPage` işlevi) istemcilerini güncelleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="7c852-191">Call a jQuery function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="7c852-192">SignalR ve jQuery</span><span class="sxs-lookup"><span data-stu-id="7c852-192">SignalR and jQuery</span></span>

<span data-ttu-id="7c852-193">**Chat.cshtml** görünüm dosyası kod örneğinde SignalR jQuery kitaplığı bir SignalR hub ile iletişim kurmak için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="7c852-193">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="7c852-194">Kodda önemli görevleri hub'ına iletileri göndermek için sunucunun istemcilere anında içeriği için çağırabilirsiniz işlevi bildirme ve bağlantı başlatma otomatik olarak oluşturulan Proxy hub'ına yönelik bir başvuru oluşturuyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="7c852-194">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="7c852-195">Aşağıdaki kod, bir hub için bir proxy bildirir.</span><span class="sxs-lookup"><span data-stu-id="7c852-195">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="7c852-196">JQuery içinde sunucu sınıfı ve üyelerini içinde ortası büyük başvurudur.</span><span class="sxs-lookup"><span data-stu-id="7c852-196">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="7c852-197">Kod örneği C# başvuran **ChatHub** jQuery sınıfında **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="7c852-197">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span> <span data-ttu-id="7c852-198">Başvuru istiyorsanız `ChatHub` sınıfı jQuery geleneksel Pascal ile C# ' ta yaptığınız gibi büyük/küçük harfleri ChatHub.cs sınıfı dosyasını düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="7c852-198">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="7c852-199">Ekleme bir `using` başvurmak için deyimi `Microsoft.AspNet.SignalR.Hubs` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="7c852-199">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="7c852-200">Ardından ekleyin `HubName` özniteliğini `ChatHub` sınıfı, örneğin `[HubName("ChatHub")]`.</span><span class="sxs-lookup"><span data-stu-id="7c852-200">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="7c852-201">Son olarak, jQuery referansı güncelleştirme `ChatHub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="7c852-201">Finally, update your jQuery reference to the `ChatHub` class.</span></span>


<span data-ttu-id="7c852-202">Aşağıdaki kod komut dosyasındaki bir geri çağırma işlevini oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="7c852-202">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="7c852-203">Sunucudaki hub sınıfı içerik güncelleştirmeleri her bir istemciye göndermek için bu işlevi çağırır.</span><span class="sxs-lookup"><span data-stu-id="7c852-203">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="7c852-204">İsteğe bağlı arama `htmlEncode` kod eklemesini önlemeye yönelik bir yolu olarak sayfasını görüntülemeden önce ileti içeriği kodlamak HTML şekilde işlev gösterir.</span><span class="sxs-lookup"><span data-stu-id="7c852-204">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

<span data-ttu-id="7c852-205">Aşağıdaki kod, hub ile bir bağlantı açmak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="7c852-205">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="7c852-206">Kod bağlantı başlar ve üzerinde click olayını işlemek için bir işlev geçirir **Gönder** sohbet sayfasının düğmesini.</span><span class="sxs-lookup"><span data-stu-id="7c852-206">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="7c852-207">Bu yaklaşım, olay işleyici yürütülmeden önce bağlantı kurulana sağlar.</span><span class="sxs-lookup"><span data-stu-id="7c852-207">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="7c852-208">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="7c852-208">Next Steps</span></span>

<span data-ttu-id="7c852-209">SignalR gerçek zamanlı web uygulamaları oluşturmak için bir çerçeve olduğunu öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="7c852-209">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="7c852-210">Birkaç SignalR geliştirme görevleri de öğrenilen: bir ASP.NET uygulaması için SignalR ekleme, hub sınıfın nasıl oluşturulacağı ve hub'dan ileti alıp göndermek nasıl.</span><span class="sxs-lookup"><span data-stu-id="7c852-210">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="7c852-211">Daha gelişmiş SignalR gelişmeler kavramları hakkında bilgi için SignalR kaynak kodu ve kaynaklar için aşağıdaki siteleri ziyaret edin:</span><span class="sxs-lookup"><span data-stu-id="7c852-211">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="7c852-212">SignalR projesi</span><span class="sxs-lookup"><span data-stu-id="7c852-212">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="7c852-213">SignalR Github ve örnekler</span><span class="sxs-lookup"><span data-stu-id="7c852-213">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="7c852-214">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="7c852-214">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
