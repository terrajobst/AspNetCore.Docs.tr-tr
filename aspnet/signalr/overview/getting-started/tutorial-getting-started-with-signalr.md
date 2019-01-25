---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Öğretici: SignalR 2 ile gerçek zamanlı bir sohbet | Microsoft Docs'
author: bradygaster
description: Bu öğreticide SignalR kullanarak gerçek zamanlı bir sohbet uygulaması oluşturma işlemi gösterilir. SignalR için boş bir ASP.NET web uygulamasına ekleyin.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 90f2c03fbda522e3a46200bc0132cc74100ce70f
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836798"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a><span data-ttu-id="403fc-104">Öğretici: SignalR 2 ile gerçek zamanlı sohbet</span><span class="sxs-lookup"><span data-stu-id="403fc-104">Tutorial: Real-time chat with SignalR 2</span></span>

<span data-ttu-id="403fc-105">Bu öğreticide SignalR gerçek zamanlı bir sohbet uygulaması oluşturmak için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="403fc-105">This tutorial shows you how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="403fc-106">Boş bir ASP.NET web uygulaması için SignalR eklediğinizde ve gönderin ve iletileri görüntülemek için bir HTML sayfası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="403fc-106">You add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>

<span data-ttu-id="403fc-107">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="403fc-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="403fc-108">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="403fc-108">Set up the project</span></span>
> * <span data-ttu-id="403fc-109">Örneği çalıştırma</span><span class="sxs-lookup"><span data-stu-id="403fc-109">Run the sample</span></span>
> * <span data-ttu-id="403fc-110">Kod İnceleme</span><span class="sxs-lookup"><span data-stu-id="403fc-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="403fc-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="403fc-111">Prerequisites</span></span>

* <span data-ttu-id="403fc-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) ile **ASP.NET ve web geliştirme** iş yükü.</span><span class="sxs-lookup"><span data-stu-id="403fc-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="403fc-113">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="403fc-113">Set up the Project</span></span>

<span data-ttu-id="403fc-114">Bu bölümde Visual Studio 2017 ve SignalR 2 boş bir ASP.NET web uygulaması oluşturmak için nasıl kullanılacağını gösterir, SignalR ekleyin ve sohbet uygulaması oluşturma.</span><span class="sxs-lookup"><span data-stu-id="403fc-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

1. <span data-ttu-id="403fc-115">Visual Studio kullanarak ASP.NET Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="403fc-115">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Web oluşturma](tutorial-getting-started-with-signalr/_static/image2.png)

1. <span data-ttu-id="403fc-117">İçinde **yeni ASP.NET projesi - SignalRChat** penceresinde bırakın **boş** seçin ve seçilen **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="403fc-117">In the **New ASP.NET Project - SignalRChat** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="403fc-118">İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="403fc-118">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="403fc-119">İçinde **Yeni Öğe Ekle - SignalRChat**seçin **yüklü** > **Visual C#**   >  **Web**  >  **SignalR** seçip **SignalR Hub sınıfı (v2)**.</span><span class="sxs-lookup"><span data-stu-id="403fc-119">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="403fc-120">Sınıf adı *ChatHub* ve projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="403fc-120">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="403fc-121">Bu adımda oluşturulur *ChatHub.cs* sınıf dosyası ve bir dizi komut dosyaları ve projeye Signalr'yi destekleyen derleme başvurularını ekler.</span><span class="sxs-lookup"><span data-stu-id="403fc-121">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="403fc-122">Yeni bir kodu *ChatHub.cs* bu kodla sınıf dosyası:</span><span class="sxs-lookup"><span data-stu-id="403fc-122">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="403fc-123">İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="403fc-123">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="403fc-124">İçinde **Yeni Öğe Ekle - SignalRChat** seçin **yüklü** > **Visual C#**   >  **Web** ve ardından seçin **OWIN başlangıç sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="403fc-124">In **Add New Item - SignalRChat** select **Installed** > **Visual C#** > **Web**  and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="403fc-125">Sınıf adı *başlangıç* ve projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="403fc-125">Name the class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="403fc-126">İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **HTML sayfası**.</span><span class="sxs-lookup"><span data-stu-id="403fc-126">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="403fc-127">Yeni sayfa adı *dizin* seçip **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="403fc-127">Name the new page *index* and select **OK**.</span></span>

1. <span data-ttu-id="403fc-128">İçinde **Çözüm Gezgini**, oluşturduğunuz HTML sayfasının sağ tıklayıp **Başlangıç Sayfası Ayarla**.</span><span class="sxs-lookup"><span data-stu-id="403fc-128">In **Solution Explorer**, right-click the HTML page you created and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="403fc-129">HTML sayfasındaki varsayılan kodu şu kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="403fc-129">Replace the default code in the HTML page with this code:</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. <span data-ttu-id="403fc-130">İçinde **Çözüm Gezgini**, genişletme **betikleri**.</span><span class="sxs-lookup"><span data-stu-id="403fc-130">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="403fc-131">JQuery ve SignalR için betik kitaplıkları projesinde görünür.</span><span class="sxs-lookup"><span data-stu-id="403fc-131">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="403fc-132">Paket Yöneticisi SignalR betikleri daha sonraki bir sürümünü yüklemiş olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="403fc-132">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="403fc-133">Kod bloğunun içinde komut dosyası başvuruları projeye komut dosyalarında sürümlerine karşılık geldiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="403fc-133">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="403fc-134">Özgün kod bloğunun komut dosyası başvuruları:</span><span class="sxs-lookup"><span data-stu-id="403fc-134">Script references from the original code block:</span></span>
    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. <span data-ttu-id="403fc-135">Bunlar eşleşmiyorsa, güncelleştirme *.html* dosya.</span><span class="sxs-lookup"><span data-stu-id="403fc-135">If they don't match, update the *.html* file.</span></span>

1. <span data-ttu-id="403fc-136">Menü çubuğundan seçin **dosya** > **Tümünü Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="403fc-136">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="403fc-137">Örneği çalıştırma</span><span class="sxs-lookup"><span data-stu-id="403fc-137">Run the Sample</span></span>

1. <span data-ttu-id="403fc-138">Araç çubuğunda, açma **betik hata ayıklamasını** ve hata ayıklama modunda örneği çalıştırmak için Yürüt düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="403fc-138">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Kullanıcı adı girin](tutorial-getting-started-with-signalr/_static/image3.png)

1. <span data-ttu-id="403fc-140">Tarayıcı açıldığında, sohbet kimliğinizi için bir ad girin.</span><span class="sxs-lookup"><span data-stu-id="403fc-140">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="403fc-141">Tarayıcıdan URL'yi kopyalayın, diğer iki tarayıcılar açın ve URL'leri adresi çubukları yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="403fc-141">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="403fc-142">Her tarayıcıda benzersiz bir ad girin.</span><span class="sxs-lookup"><span data-stu-id="403fc-142">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="403fc-143">Şimdi bir yorum girip seçin ekleyin **Gönder**.</span><span class="sxs-lookup"><span data-stu-id="403fc-143">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="403fc-144">Bu, diğer tarayıcılarda yineleyin.</span><span class="sxs-lookup"><span data-stu-id="403fc-144">Repeat that in the other browsers.</span></span> <span data-ttu-id="403fc-145">Açıklamalar, gerçek zamanlı olarak görünür.</span><span class="sxs-lookup"><span data-stu-id="403fc-145">The comments appear in real-time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="403fc-146">Bu basit sohbet uygulaması sunucusunda tartışma bağlam korumaz.</span><span class="sxs-lookup"><span data-stu-id="403fc-146">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="403fc-147">Hub'ın tüm geçerli kullanıcılar yorum yayınlar.</span><span class="sxs-lookup"><span data-stu-id="403fc-147">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="403fc-148">Sohbet daha sonra katılmak kullanıcıların zamandan eklenen iletileri katılmaları görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="403fc-148">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="403fc-149">Sohbet uygulaması üç farklı tarayıcılarda nasıl çalıştığını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="403fc-149">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="403fc-150">Tüm tarayıcılar, Tom, Anand ve Susan iletileri gönderirken, gerçek zamanlı olarak güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="403fc-150">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Üç tüm tarayıcılar aynı Sohbet geçmişini görüntüleme](tutorial-getting-started-with-signalr/_static/image4.png)

1. <span data-ttu-id="403fc-152">İçinde **Çözüm Gezgini**, inceleme **betik belgelerini** çalışan uygulama düğümü.</span><span class="sxs-lookup"><span data-stu-id="403fc-152">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="403fc-153">Adlı bir betik dosyası *hubs* , çalışma zamanında SignalR kitaplığı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="403fc-153">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="403fc-154">Bu dosya, jQuery betik sunucu tarafı kodu arasında iletişimi yönetir.</span><span class="sxs-lookup"><span data-stu-id="403fc-154">This file manages the communication between jQuery script and server-side code.</span></span>

    ![Betik dosyaları düğümüne otomatik olarak oluşturulan hub komut dosyası](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="403fc-156">Kod İnceleme</span><span class="sxs-lookup"><span data-stu-id="403fc-156">Examine the Code</span></span>

<span data-ttu-id="403fc-157">SignalRChat uygulaması, iki temel SignalR geliştirme görevlerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="403fc-157">The SignalRChat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="403fc-158">Bir hub'ı oluşturma işlemini gösterir.</span><span class="sxs-lookup"><span data-stu-id="403fc-158">It shows you how to create a hub.</span></span> <span data-ttu-id="403fc-159">Sunucu ana koordinasyon nesnesi olarak, hub'ı kullanır.</span><span class="sxs-lookup"><span data-stu-id="403fc-159">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="403fc-160">Hub SignalR jQuery kitaplığı ileti göndermek ve almak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="403fc-160">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="403fc-161">SignalR hub'ları ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="403fc-161">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="403fc-162">Yukarıdaki kod örneğinde `ChatHub` sınıf türetilir `Microsoft.AspNet.SignalR.Hub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="403fc-162">In the code sample above, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="403fc-163">Öğesinden türetme `Hub` SignalR uygulama oluşturmak için kullanışlı bir yöntem bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="403fc-163">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="403fc-164">Genel yöntemler hub sınıfınıza oluşturabilir ve sonra bir web sayfasında komut dosyalarından çağırarak bu yöntemi kullanın.</span><span class="sxs-lookup"><span data-stu-id="403fc-164">You can create public methods on your hub class and then use those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="403fc-165">İstemciler sohbet kodda çağrı `ChatHub.Send` yeni bir ileti göndermek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="403fc-165">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="403fc-166">Hub'ın ardından iletiyi tüm istemcilere çağırarak gönderir `Clients.All.broadcastMessage`.</span><span class="sxs-lookup"><span data-stu-id="403fc-166">The hub then sends the message to all clients by calling `Clients.All.broadcastMessage`.</span></span>

<span data-ttu-id="403fc-167">`Send` Yöntemi birkaç hub kavramları göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="403fc-167">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="403fc-168">İstemciler, bunları çağırabilirsiniz genel yöntemleri bir hub'da bildirin.</span><span class="sxs-lookup"><span data-stu-id="403fc-168">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="403fc-169">Kullanım `Microsoft.AspNet.SignalR.Hub.Clients` tüm istemcilerle iletişim kurmak için dinamik özellik bu hub'a bağlı.</span><span class="sxs-lookup"><span data-stu-id="403fc-169">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="403fc-170">İstemcide bir işlevi çağırmayı (gibi `broadcastMessage` işlevi) istemcilerini güncelleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="403fc-170">Call a function on the client (like the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a><span data-ttu-id="403fc-171">SignalR ve jQuery index.html içinde</span><span class="sxs-lookup"><span data-stu-id="403fc-171">SignalR and jQuery in the index.html</span></span>

<span data-ttu-id="403fc-172">*İndex.html* sayfası kod örneğinde SignalR jQuery kitaplığının bir SignalR hub'ı ile iletişim kurmak için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="403fc-172">The *index.html* page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="403fc-173">Kod, birçok önemli görevleri gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="403fc-173">The code carries out many important tasks.</span></span> <span data-ttu-id="403fc-174">Bu hub başvurmak için bir proxy bildirir, sunucunun anında içeriğin istemciler için çağrı yapabilirsiniz ve hub'ına ileti göndermek için bir bağlantı başlatır bir işlev bildirir.</span><span class="sxs-lookup"><span data-stu-id="403fc-174">It declares a proxy to reference the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="403fc-175">JavaScript'te, sunucu sınıfını ve üyelerini başvuru camelCase olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="403fc-175">In JavaScript the reference to the server class and its members has to be camelCase.</span></span> <span data-ttu-id="403fc-176">Kod örneği başvuru C# *ChatHub* JavaScript olarak sınıfında `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="403fc-176">The code sample references the C# *ChatHub* class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="403fc-177">Bu kod bloğu içinde bir geri çağırma işlevini betiğinde oluşturun.</span><span class="sxs-lookup"><span data-stu-id="403fc-177">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="403fc-178">Sunucudaki hub sınıfına içerik güncelleştirmeleri her bir istemciye göndermek için bu işlevi çağırır.</span><span class="sxs-lookup"><span data-stu-id="403fc-178">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="403fc-179">İki satırları, bu HTML içeriğini görüntülemeden önce isteğe bağlıdır ve kod eklemesini engellemek için en iyi yolu Göster kodlayın.</span><span class="sxs-lookup"><span data-stu-id="403fc-179">The two lines that HTML-encode the content before displaying it are optional and show a good way to prevent script injection.</span></span>

<span data-ttu-id="403fc-180">Bu kod hub'ı ile bir bağlantı açar.</span><span class="sxs-lookup"><span data-stu-id="403fc-180">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> <span data-ttu-id="403fc-181">Bu yaklaşım, olay işleyici yürütülmeden önce kodu bir bağlantı kurar sağlar.</span><span class="sxs-lookup"><span data-stu-id="403fc-181">This approach ensures that the code establishes a connection before the event handler executes.</span></span>

<span data-ttu-id="403fc-182">Kod bağlantı başlar ve ardından üzerinde click olayını işlemek için bir işlev geçirir **Gönder** HTML sayfasındaki düğmesi.</span><span class="sxs-lookup"><span data-stu-id="403fc-182">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="403fc-183">Kodu alma</span><span class="sxs-lookup"><span data-stu-id="403fc-183">Get the code</span></span>

[<span data-ttu-id="403fc-184">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="403fc-184">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

## <a name="additional-resources"></a><span data-ttu-id="403fc-185">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="403fc-185">Additional resources</span></span>

<span data-ttu-id="403fc-186">SignalR hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="403fc-186">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="403fc-187">SignalR projesi</span><span class="sxs-lookup"><span data-stu-id="403fc-187">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="403fc-188">SignalR Github ve örnekleri</span><span class="sxs-lookup"><span data-stu-id="403fc-188">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="403fc-189">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="403fc-189">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="403fc-190">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="403fc-190">Next steps</span></span>

<span data-ttu-id="403fc-191">Bu öğreticide:</span><span class="sxs-lookup"><span data-stu-id="403fc-191">In this tutorial you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="403fc-192">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="403fc-192">Set up the project</span></span>
> * <span data-ttu-id="403fc-193">Örnek çalıştı</span><span class="sxs-lookup"><span data-stu-id="403fc-193">Ran the sample</span></span>
> * <span data-ttu-id="403fc-194">Dio</span><span class="sxs-lookup"><span data-stu-id="403fc-194">Examined the code</span></span>

<span data-ttu-id="403fc-195">SignalR ve MVC 5 nasıl kullanılacağını öğrenmek için sonraki makaleye ilerleyin.</span><span class="sxs-lookup"><span data-stu-id="403fc-195">Advance to the next article to learn how to use SignalR and MVC 5.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="403fc-196">SignalR 2 ve MVC 5</span><span class="sxs-lookup"><span data-stu-id="403fc-196">SignalR 2 and MVC 5</span></span>](tutorial-getting-started-with-signalr-and-mvc.md)