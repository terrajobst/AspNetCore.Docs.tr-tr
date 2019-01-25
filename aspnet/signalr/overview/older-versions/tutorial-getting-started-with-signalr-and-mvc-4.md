---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Öğretici: SignalR ile çalışmaya başlama 1.x ve MVC 4 | Microsoft Docs'
author: bradygaster
description: Bir gerçek zamanlı bir sohbet uygulaması oluşturmak için ASP.NET SignalR ve ASP.NET MVC 4 kullanın.
ms.author: bradyg
ms.date: 03/29/2013
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: dd55ca22004b7e3899f6a8789494c842b984787f
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837799"
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a><span data-ttu-id="98e5e-103">Öğretici: SignalR 1.x ve MVC 4 ile Çalışmaya Başlama</span><span class="sxs-lookup"><span data-stu-id="98e5e-103">Tutorial: Getting Started with SignalR 1.x and MVC 4</span></span>
====================
<span data-ttu-id="98e5e-104">tarafından [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="98e5e-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="98e5e-105">Bu öğreticide, ASP.NET Signalr'yi gerçek zamanlı bir sohbet uygulaması oluşturmak için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="98e5e-105">This tutorial shows how to use ASP.NET SignalR to create a real-time chat application.</span></span> <span data-ttu-id="98e5e-106">SignalR MVC 4 uygulama ekleme ve gönderin ve iletileri görüntülemek için bir sohbet görünümü oluşturmak.</span><span class="sxs-lookup"><span data-stu-id="98e5e-106">You will add SignalR to an MVC 4 application and create a chat view to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="98e5e-107">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="98e5e-107">Overview</span></span>

<span data-ttu-id="98e5e-108">Bu öğreticide, ASP.NET SignalR ve ASP.NET MVC 4 ile gerçek zamanlı bir web uygulaması geliştirme tanıtır.</span><span class="sxs-lookup"><span data-stu-id="98e5e-108">This tutorial introduces you to real-time web application development with ASP.NET SignalR and ASP.NET MVC 4.</span></span> <span data-ttu-id="98e5e-109">Öğreticide aynı sohbet uygulama kodunu [SignalR Başlarken Öğreticisi](tutorial-getting-started-with-signalr.md), ancak Internet şablonu temel alan bir MVC 4 uygulamaya ekleme işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="98e5e-109">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 4 application based on the Internet template.</span></span>

<span data-ttu-id="98e5e-110">Bu konuda aşağıdaki SignalR geliştirme görevleri öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="98e5e-110">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="98e5e-111">SignalR kitaplığı, MVC 4 uygulamaya ekleme.</span><span class="sxs-lookup"><span data-stu-id="98e5e-111">Adding the SignalR library to an MVC 4 application.</span></span>
- <span data-ttu-id="98e5e-112">İçeriği istemcilere göndermek için bir hub sınıf oluşturuluyor.</span><span class="sxs-lookup"><span data-stu-id="98e5e-112">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="98e5e-113">İleti göndermek ve hub'ından güncelleştirmeleri görüntülemek için bir web sayfasında SignalR jQuery kitaplığı kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="98e5e-113">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="98e5e-114">Aşağıdaki ekran görüntüsünde, bir tarayıcıda çalışan tamamlanmış sohbet uygulaması gösterir.</span><span class="sxs-lookup"><span data-stu-id="98e5e-114">The following screen shot shows the completed chat application running in a browser.</span></span>

![Sohbet örnekleri](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

<span data-ttu-id="98e5e-116">Bölümler:</span><span class="sxs-lookup"><span data-stu-id="98e5e-116">Sections:</span></span>

- [<span data-ttu-id="98e5e-117">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="98e5e-117">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="98e5e-118">Örneği çalıştırma</span><span class="sxs-lookup"><span data-stu-id="98e5e-118">Run the Sample</span></span>](#run)
- [<span data-ttu-id="98e5e-119">Kod İnceleme</span><span class="sxs-lookup"><span data-stu-id="98e5e-119">Examine the Code</span></span>](#code)
- [<span data-ttu-id="98e5e-120">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="98e5e-120">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="98e5e-121">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="98e5e-121">Set up the Project</span></span>

<span data-ttu-id="98e5e-122">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="98e5e-122">Prerequisites:</span></span>

- <span data-ttu-id="98e5e-123">Visual Studio 2010 SP1, Visual Studio 2012 veya Visual Studio 2012 Express.</span><span class="sxs-lookup"><span data-stu-id="98e5e-123">Visual Studio 2010 SP1, Visual Studio 2012, or Visual Studio 2012 Express.</span></span> <span data-ttu-id="98e5e-124">Visual Studio yoksa bkz [ASP.NET indirir](https://www.asp.net/downloads) ücretsiz Visual Studio 2012 Express geliştirme aracı alınamıyor.</span><span class="sxs-lookup"><span data-stu-id="98e5e-124">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="98e5e-125">Visual Studio 2010 için yükleme [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span><span class="sxs-lookup"><span data-stu-id="98e5e-125">For Visual Studio 2010, install [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span></span>

<span data-ttu-id="98e5e-126">Bu bölümde, bir ASP.NET MVC 4 uygulama oluşturmak, SignalR kitaplığa ekleyin ve sohbet uygulaması oluşturma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="98e5e-126">This section shows how to create an ASP.NET MVC 4 application, add the SignalR library, and create the chat application.</span></span>

1. 1. <span data-ttu-id="98e5e-127">Visual Studio'da ASP.NET MVC 4 uygulama oluşturma, SignalRChat adlandırın ve Tamam'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="98e5e-127">In Visual Studio create an ASP.NET MVC 4 application, name it SignalRChat, and click OK.</span></span>

        > [!NOTE]
        > <span data-ttu-id="98e5e-128">VS 2010'da seçin **.NET Framework 4** Framework sürüm dropdown denetimi.</span><span class="sxs-lookup"><span data-stu-id="98e5e-128">In VS 2010, select **.NET Framework 4** in the Framework version dropdown control.</span></span> <span data-ttu-id="98e5e-129">SignalR kod, .NET Framework sürüm 4 ve 4.5 üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="98e5e-129">SignalR code runs on .NET Framework versions 4 and 4.5.</span></span>

        ![MVC web oluşturma](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. <span data-ttu-id="98e5e-131">Internet uygulaması şablonu seçin, seçeneğini temizleyin **birim testi projesi oluşturma**, Tamam'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="98e5e-131">Select the Internet Application template, clear the option to **Create a unit test project**, and click OK.</span></span>

         ![MVC internet sitesi oluşturma](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. <span data-ttu-id="98e5e-133">Açık **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu** ve aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="98e5e-133">Open the **Tools > NuGet Package Manager > Package Manager Console** and run the following command.</span></span> <span data-ttu-id="98e5e-134">Bu adım, bir dizi komut dosyaları ve SignalR işlevselliğini etkinleştirmek derleme başvuruları projeye ekler.</span><span class="sxs-lookup"><span data-stu-id="98e5e-134">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. <span data-ttu-id="98e5e-135">İçinde **Çözüm Gezgini** betikleri klasörünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="98e5e-135">In **Solution Explorer** expand the Scripts folder.</span></span> <span data-ttu-id="98e5e-136">SignalR için betik kitaplıkları projeye eklendiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="98e5e-136">Note that script libraries for SignalR have been added to the project.</span></span>

         ![Kitaplık başvuruları](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. <span data-ttu-id="98e5e-138">İçinde **Çözüm Gezgini**, projeye sağ tıklayın, **Ekle | Yeni klasör**, adlı yeni bir klasör ekleyin **Hubs**.</span><span class="sxs-lookup"><span data-stu-id="98e5e-138">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
      6. <span data-ttu-id="98e5e-139">Sağ **Hubs** klasörü tıklatın **Ekle | Sınıf**ve adlı yeni bir C# sınıfı oluşturma **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="98e5e-139">Right-click the **Hubs** folder, click **Add | Class**, and create a new C# class named **ChatHub.cs**.</span></span> <span data-ttu-id="98e5e-140">Bu sınıf, tüm istemciler için iletileri gönderen bir SignalR sunucu hub olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="98e5e-140">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

> [!NOTE]
> <span data-ttu-id="98e5e-141">Visual Studio 2012 kullanın ve yüklüyse [ASP.NET ve Web Araçları 2012.2 güncelleştirme](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), hub sınıfı oluşturmak için yeni SignalR öğe şablonu kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="98e5e-141">If you use Visual Studio 2012 and have installed the [ASP.NET and Web Tools 2012.2 update](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), you can use the new SignalR item template to create the hub class.</span></span> <span data-ttu-id="98e5e-142">Bunu yapmak için sağ **Hubs** klasörü tıklatın **Ekle | Yeni öğe**seçin **SignalR Hub sınıfı (v1)** ve sınıf adını **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="98e5e-142">To do that, right-click the **Hubs** folder, click **Add | New Item**, select **SignalR Hub Class (v1)**, and name the class **ChatHub.cs**.</span></span>


1. <span data-ttu-id="98e5e-143">Değiştirin **ChatHub** aşağıdaki kodla sınıfı.</span><span class="sxs-lookup"><span data-stu-id="98e5e-143">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. <span data-ttu-id="98e5e-144">Açık **Global.asax** proje için dosya ve yöntemine bir çağrı ekleyin `RouteTable.Routes.MapHubs();` kod ilk satırı olarak `Application_Start` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="98e5e-144">Open the **Global.asax** file for the project, and add a call to the method `RouteTable.Routes.MapHubs();` as the first line of code in the `Application_Start` method.</span></span> <span data-ttu-id="98e5e-145">Bu kod, SignalR hub'ları için varsayılan rota kaydeder ve başka bir yolun kaydetmeden önce çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="98e5e-145">This code registers the default route for SignalR hubs and must be called before you register any other routes.</span></span> <span data-ttu-id="98e5e-146">Tamamlanan `Application_Start` yöntemi aşağıdaki örnekteki gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="98e5e-146">The completed `Application_Start` method looks like the following example.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. <span data-ttu-id="98e5e-147">Düzen `HomeController` sınıfı bulundu **Controllers/HomeController.cs** ve sınıfına aşağıdaki yöntemi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="98e5e-147">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="98e5e-148">Bu yöntem döndürür **sohbet** daha sonraki bir adımda oluşturacağınız görünümü.</span><span class="sxs-lookup"><span data-stu-id="98e5e-148">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. <span data-ttu-id="98e5e-149">İçinde sağ `Chat` az önce oluşturduğunuz ve tıklayın yöntemi **Görünüm Ekle** yeni bir görünüm dosyası oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="98e5e-149">Right-click within the `Chat` method you just created, and click **Add View** to create a new view file.</span></span>
5. <span data-ttu-id="98e5e-150">İçinde **Görünüm Ekle** iletişim kutusunda emin olun onay kutusunun seçili için **düzeni veya ana sayfayı kullan** (diğer onay kutularını temizleyin) ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="98e5e-150">In the **Add View** dialog, make sure the check box is selected to **Use a layout or master page** (clear the other check boxes), and then click **Add**.</span></span>

    ![Görünüm ekleme](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. <span data-ttu-id="98e5e-152">Adlı yeni görünüm dosyası Düzenle **Chat.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="98e5e-152">Edit the new view file named **Chat.cshtml**.</span></span> <span data-ttu-id="98e5e-153">Sonra &lt;h2&gt; etiketinde, aşağıdakini yapıştırın &lt;div&gt; bölümü ve `@section scripts` sayfasına kod bloğu.</span><span class="sxs-lookup"><span data-stu-id="98e5e-153">After the &lt;h2&gt; tag, paste the following &lt;div&gt; section and `@section scripts` code block into the page.</span></span> <span data-ttu-id="98e5e-154">Bu betik, sohbet iletileri gönderme ve sunucudaki iletileri görüntülemek sayfanın sağlar.</span><span class="sxs-lookup"><span data-stu-id="98e5e-154">This script enables the page to send chat messages and display messages from the server.</span></span> <span data-ttu-id="98e5e-155">Sohbet görünümü için tam kod, aşağıdaki kod bloğu içinde görünür.</span><span class="sxs-lookup"><span data-stu-id="98e5e-155">The complete code for the chat view appears in the following code block.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="98e5e-156">Visual Studio projenize SignalR ve diğer betik kitaplıkları eklemek, Paket Yöneticisi betikleriyle bu konuda gösterilen sürümlerden daha yeni sürümlerini yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="98e5e-156">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install versions of the scripts that are more recent than the versions shown in this topic.</span></span> <span data-ttu-id="98e5e-157">Kodunuzda komut dosyası başvuruları projenize yüklü betik kitaplıkları sürümleri eşleştiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="98e5e-157">Make sure that the script references in your code match the versions of the script libraries installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. <span data-ttu-id="98e5e-158">**Tümünü Kaydet** projesi için.</span><span class="sxs-lookup"><span data-stu-id="98e5e-158">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="98e5e-159">Örneği çalıştırma</span><span class="sxs-lookup"><span data-stu-id="98e5e-159">Run the Sample</span></span>

1. <span data-ttu-id="98e5e-160">Projeyi hata ayıklama modunda çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="98e5e-160">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="98e5e-161">Tarayıcının adres satırındaki ekleme **/home/sohbet** proje için varsayılan sayfasının URL'si.</span><span class="sxs-lookup"><span data-stu-id="98e5e-161">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="98e5e-162">Bir tarayıcı örneğinde ve bir kullanıcı adı için istemleri sohbet sayfayı yüklemez.</span><span class="sxs-lookup"><span data-stu-id="98e5e-162">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![Kullanıcı adı girin](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. <span data-ttu-id="98e5e-164">Bir kullanıcı adı girin.</span><span class="sxs-lookup"><span data-stu-id="98e5e-164">Enter a user name.</span></span>
4. <span data-ttu-id="98e5e-165">Tarayıcının adres satırından URL'sini kopyalayın ve iki daha fazla tarayıcı örneği açmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="98e5e-165">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="98e5e-166">Her bir tarayıcı örneğinde benzersiz bir kullanıcı adı girin.</span><span class="sxs-lookup"><span data-stu-id="98e5e-166">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="98e5e-167">Her bir tarayıcı örneğinde bir açıklama ekleyin ve **Gönder**.</span><span class="sxs-lookup"><span data-stu-id="98e5e-167">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="98e5e-168">Açıklamalar, hepsinin tarayıcı görüntülemelidir.</span><span class="sxs-lookup"><span data-stu-id="98e5e-168">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="98e5e-169">Bu basit sohbet uygulaması sunucusunda tartışma bağlam korumaz.</span><span class="sxs-lookup"><span data-stu-id="98e5e-169">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="98e5e-170">Hub'ın tüm geçerli kullanıcılar yorum yayınlar.</span><span class="sxs-lookup"><span data-stu-id="98e5e-170">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="98e5e-171">Sohbet daha sonra katılmak kullanıcıların zamandan eklenen iletileri katılmaları görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="98e5e-171">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="98e5e-172">Aşağıdaki ekran görüntüsünde, bir tarayıcıda çalışan sohbet uygulaması gösterir.</span><span class="sxs-lookup"><span data-stu-id="98e5e-172">The following screen shot shows the chat application running in a browser.</span></span>

    ![Sohbet tarayıcılar](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. <span data-ttu-id="98e5e-174">İçinde **Çözüm Gezgini**, inceleme **betik belgelerini** çalışan uygulama düğümü.</span><span class="sxs-lookup"><span data-stu-id="98e5e-174">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="98e5e-175">Internet Explorer tarayıcı olarak kullanıyorsanız bu hata ayıklama modunda görünür bir düğümdür.</span><span class="sxs-lookup"><span data-stu-id="98e5e-175">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="98e5e-176">Adlı bir betik dosyası **hubs** , SignalR kitaplık çalışma zamanında dinamik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="98e5e-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="98e5e-177">Bu dosya, jQuery betik sunucu tarafı kodu arasında iletişimi yönetir.</span><span class="sxs-lookup"><span data-stu-id="98e5e-177">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="98e5e-178">Internet Explorer dışındaki bir tarayıcı kullanıyorsanız, dinamik da erişebilirsiniz **hubs** atarak ona doğrudan, örneğin dosya http://mywebsite/signalr/hubs.</span><span class="sxs-lookup"><span data-stu-id="98e5e-178">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

    ![Oluşturulan hub betiği](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="98e5e-180">Kod İnceleme</span><span class="sxs-lookup"><span data-stu-id="98e5e-180">Examine the Code</span></span>

<span data-ttu-id="98e5e-181">SignalR sohbet uygulaması iki temel SignalR geliştirme görevleri gösterir: sunucunun ana koordinasyon nesne olarak bir hub'ı oluşturma ve ileti göndermek ve almak için SignalR jQuery kitaplığı kullanma.</span><span class="sxs-lookup"><span data-stu-id="98e5e-181">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="98e5e-182">SignalR Hubs</span><span class="sxs-lookup"><span data-stu-id="98e5e-182">SignalR Hubs</span></span>

<span data-ttu-id="98e5e-183">Kod örneğinde **ChatHub** sınıf türetilir **Microsoft.AspNet.SignalR.Hub** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="98e5e-183">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="98e5e-184">Öğesinden türetme **Hub** SignalR uygulama oluşturmak için kullanışlı bir yöntem bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="98e5e-184">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="98e5e-185">Hub sınıfınıza genel yöntemleri oluşturun ve bu yöntemler bir web sayfasında jQuery betiklerden çağırarak erişin.</span><span class="sxs-lookup"><span data-stu-id="98e5e-185">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="98e5e-186">İstemciler sohbet kodda çağrı **ChatHub.Send** yeni bir ileti göndermek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="98e5e-186">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="98e5e-187">Hub sırayla ileti tüm istemcilere çağırarak gönderen **Clients.All.addNewMessageToPage**.</span><span class="sxs-lookup"><span data-stu-id="98e5e-187">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="98e5e-188">**Gönder** yöntemi birkaç hub kavramları göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="98e5e-188">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="98e5e-189">İstemciler, bunları çağırabilirsiniz genel yöntemleri bir hub'da bildirin.</span><span class="sxs-lookup"><span data-stu-id="98e5e-189">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="98e5e-190">Kullanım **Microsoft.AspNet.SignalR.Hub.Clients** tüm istemcilere erişmek için özelliği bu hub'a bağlı.</span><span class="sxs-lookup"><span data-stu-id="98e5e-190">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="98e5e-191">İstemcide jQuery işlev çağırma (gibi `addNewMessageToPage` işlevi) istemcilerini güncelleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="98e5e-191">Call a jQuery function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="98e5e-192">SignalR ve jQuery</span><span class="sxs-lookup"><span data-stu-id="98e5e-192">SignalR and jQuery</span></span>

<span data-ttu-id="98e5e-193">**Chat.cshtml** görünüm dosyası kod örneğinde SignalR jQuery kitaplığının bir SignalR hub'ı ile iletişim kurmak için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="98e5e-193">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="98e5e-194">Önemli görevleri kod hub'ına ileti göndermek için sunucu istemcilere anında iletme içeriğe çağırabilen bir işlevi bildirmek ve bağlantı başlatma otomatik olarak oluşturulan proxy hub'ına yönelik bir başvuru oluşturuyor.</span><span class="sxs-lookup"><span data-stu-id="98e5e-194">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="98e5e-195">Aşağıdaki kod, bir hub için bir proxy bildirir.</span><span class="sxs-lookup"><span data-stu-id="98e5e-195">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="98e5e-196">JQuery içinde sunucu sınıfını ve üyelerini ortası büyük harf başvurudur.</span><span class="sxs-lookup"><span data-stu-id="98e5e-196">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="98e5e-197">Kod örneği C# başvuran **ChatHub** jQuery sınıfında **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="98e5e-197">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span> <span data-ttu-id="98e5e-198">Başvuru istiyorsanız `ChatHub` sınıfı ile geleneksel Pascal jQuery içinde C# dilinde olduğu gibi büyük/küçük harfleri ChatHub.cs sınıf dosyasını düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="98e5e-198">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="98e5e-199">Ekleme bir `using` başvurmak için deyimi `Microsoft.AspNet.SignalR.Hubs` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="98e5e-199">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="98e5e-200">Ardından Ekle `HubName` özniteliğini `ChatHub` sınıfından, örneğin `[HubName("ChatHub")]`.</span><span class="sxs-lookup"><span data-stu-id="98e5e-200">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="98e5e-201">Son olarak, jQuery başvuru güncelleştirme `ChatHub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="98e5e-201">Finally, update your jQuery reference to the `ChatHub` class.</span></span>


<span data-ttu-id="98e5e-202">Aşağıdaki kod, komut dosyasında bir geri çağırma işlevini oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="98e5e-202">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="98e5e-203">Sunucudaki hub sınıfına içerik güncelleştirmeleri her bir istemciye göndermek için bu işlevi çağırır.</span><span class="sxs-lookup"><span data-stu-id="98e5e-203">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="98e5e-204">İsteğe bağlı çağrısı `htmlEncode` kod eklemesini engellemek için bir yol olarak sayfasını görüntülemeden önce ileti içeriği kodlama HTML şekilde işlev gösterir.</span><span class="sxs-lookup"><span data-stu-id="98e5e-204">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

<span data-ttu-id="98e5e-205">Aşağıdaki kod hub'ı ile bir bağlantı açmak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="98e5e-205">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="98e5e-206">Kod bağlantı başlar ve ardından üzerinde click olayını işlemek için bir işlev geçirir **Gönder** sohbet sayfasında düğme.</span><span class="sxs-lookup"><span data-stu-id="98e5e-206">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="98e5e-207">Bu yaklaşım, olay işleyici yürütülmeden önce bağlantı kurulur sağlar.</span><span class="sxs-lookup"><span data-stu-id="98e5e-207">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="98e5e-208">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="98e5e-208">Next Steps</span></span>

<span data-ttu-id="98e5e-209">SignalR gerçek zamanlı web uygulamaları oluşturmaya yönelik bir çerçeve olduğunu öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="98e5e-209">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="98e5e-210">Ayrıca birkaç SignalR geliştirme görevlerini öğrendiniz: bir ASP.NET uygulaması için SignalR ekleme, hub sınıfı oluşturma ve hub'ından iletiler alan nasıl.</span><span class="sxs-lookup"><span data-stu-id="98e5e-210">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="98e5e-211">Daha gelişmiş SignalR gelişmeleri kavramları hakkında bilgi için SignalR kaynak kodu ve kaynaklar için aşağıdaki siteleri ziyaret edin:</span><span class="sxs-lookup"><span data-stu-id="98e5e-211">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="98e5e-212">SignalR projesi</span><span class="sxs-lookup"><span data-stu-id="98e5e-212">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="98e5e-213">SignalR Github ve örnekleri</span><span class="sxs-lookup"><span data-stu-id="98e5e-213">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="98e5e-214">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="98e5e-214">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
