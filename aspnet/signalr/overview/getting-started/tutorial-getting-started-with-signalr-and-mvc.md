---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Öğretici: SignalR 2 ve MVC 5 ile gerçek zamanlı bir sohbet | Microsoft Docs'
author: pfletcher
description: Bu öğreticide, ASP.NET SignalR 2 gerçek zamanlı bir sohbet uygulaması oluşturmak için nasıl kullanılacağını gösterir. Bir MVC 5 uygulaması için SignalR eklersiniz.
ms.author: riande
ms.date: 01/02/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: eb4b7e1403f4070d65702b756bf98c5294c7fb17
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098610"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a><span data-ttu-id="13017-104">Öğretici: SignalR 2 ve MVC 5 ile gerçek zamanlı bir sohbet</span><span class="sxs-lookup"><span data-stu-id="13017-104">Tutorial: Real-time chat with SignalR 2 and MVC 5</span></span>

<span data-ttu-id="13017-105">Bu öğreticide, ASP.NET SignalR 2 gerçek zamanlı bir sohbet uygulaması oluşturmak için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="13017-105">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="13017-106">Bir MVC 5 uygulaması için SignalR eklediğinizde ve gönderin ve iletileri görüntülemek için bir sohbet görünüm oluşturun.</span><span class="sxs-lookup"><span data-stu-id="13017-106">You add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span>

<span data-ttu-id="13017-107">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="13017-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="13017-108">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="13017-108">Set up the project</span></span>
> * <span data-ttu-id="13017-109">Örneği çalıştırma</span><span class="sxs-lookup"><span data-stu-id="13017-109">Run the sample</span></span>
> * <span data-ttu-id="13017-110">Kod İnceleme</span><span class="sxs-lookup"><span data-stu-id="13017-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="13017-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="13017-111">Prerequisites</span></span>

* <span data-ttu-id="13017-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) ile **ASP.NET ve web geliştirme** iş yükü.</span><span class="sxs-lookup"><span data-stu-id="13017-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="13017-113">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="13017-113">Set up the Project</span></span>

<span data-ttu-id="13017-114">Bu bölümde, Visual Studio 2017 ve SignalR 2 boş bir ASP.NET MVC 5 uygulaması oluşturma, SignalR kitaplığa ekleyin ve sohbet uygulaması oluşturmak için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="13017-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="13017-115">Visual Studio'da .NET Framework 4.5 hedefleyen bir C# ASP.NET uygulaması oluşturma, SignalRChat adlandırın ve Tamam'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="13017-115">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![Web oluşturma](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. <span data-ttu-id="13017-117">İçinde **yeni ASP.NET Web uygulaması - SignalRMvcChat**seçin **MVC** seçip **kimlik doğrulamayı Değiştir**.</span><span class="sxs-lookup"><span data-stu-id="13017-117">In **New ASP.NET Web Application - SignalRMvcChat**, select **MVC** and then select **Change Authentication**.</span></span>

1. <span data-ttu-id="13017-118">İçinde **kimlik doğrulamayı Değiştir**seçin **kimlik doğrulaması yok** tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="13017-118">In **Change Authentication**, select **No Authentication** and click **OK**.</span></span>

    ![Kimlik doğrulaması Yok'u seçin](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. <span data-ttu-id="13017-120">İçinde **yeni ASP.NET Web uygulaması - SignalRMvcChat**seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="13017-120">In **New ASP.NET Web Application - SignalRMvcChat**, select **OK**.</span></span>

1. <span data-ttu-id="13017-121">İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="13017-121">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="13017-122">İçinde **Yeni Öğe Ekle - SignalRChat**seçin **yüklü** > **Visual C#**   >  **Web**  >  **SignalR** seçip **SignalR Hub sınıfı (v2)**.</span><span class="sxs-lookup"><span data-stu-id="13017-122">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="13017-123">Sınıf adı *ChatHub* ve projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="13017-123">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="13017-124">Bu adımda oluşturulur *ChatHub.cs* sınıf dosyası ve bir dizi komut dosyaları ve projeye Signalr'yi destekleyen derleme başvurularını ekler.</span><span class="sxs-lookup"><span data-stu-id="13017-124">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="13017-125">Yeni bir kodu *ChatHub.cs* bu kodla sınıf dosyası:</span><span class="sxs-lookup"><span data-stu-id="13017-125">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. <span data-ttu-id="13017-126">İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="13017-126">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="13017-127">Yeni bir sınıf adı *başlangıç* ve projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="13017-127">Name the new class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="13017-128">Değiştirin *Startup.cs* bu kodla sınıf dosyası:</span><span class="sxs-lookup"><span data-stu-id="13017-128">Replace the code in the *Startup.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. <span data-ttu-id="13017-129">İçinde **Çözüm Gezgini**seçin **denetleyicileri** > **HomeController.cs**.</span><span class="sxs-lookup"><span data-stu-id="13017-129">In **Solution Explorer**, select **Controllers** > **HomeController.cs**.</span></span>

1. <span data-ttu-id="13017-130">Bu yönteme ekleme *HomeController.cs*.</span><span class="sxs-lookup"><span data-stu-id="13017-130">Add this method to the *HomeController.cs*.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    <span data-ttu-id="13017-131">Bu yöntem döndürür **sohbet** sonraki adımlardan birinde oluşturduğunuz görünümü.</span><span class="sxs-lookup"><span data-stu-id="13017-131">This method returns the **Chat** view that you create in a later step.</span></span>

1. <span data-ttu-id="13017-132">İçinde **Çözüm Gezgini**, sağ **görünümleri** > **giriş**seçip **Ekle**  >    **Görünüm**.</span><span class="sxs-lookup"><span data-stu-id="13017-132">In **Solution Explorer**, right-click **Views** > **Home**, and select **Add** >  **View**.</span></span>

1. <span data-ttu-id="13017-133">İçinde **Görünüm Ekle**, yeni görünüm adı **sohbet** seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="13017-133">In **Add View**, name the new view **Chat** and select **Add**.</span></span>

1. <span data-ttu-id="13017-134">Öğesinin içeriğini değiştirin **Chat.cshtml** Bu kod ile:</span><span class="sxs-lookup"><span data-stu-id="13017-134">Replace the contents of **Chat.cshtml** with this code:</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. <span data-ttu-id="13017-135">İçinde **Çözüm Gezgini**, genişletme **betikleri**.</span><span class="sxs-lookup"><span data-stu-id="13017-135">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="13017-136">JQuery ve SignalR için betik kitaplıkları projesinde görünür.</span><span class="sxs-lookup"><span data-stu-id="13017-136">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="13017-137">Paket Yöneticisi SignalR betikleri daha sonraki bir sürümünü yüklemiş olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="13017-137">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="13017-138">Kod bloğunun içinde komut dosyası başvuruları projeye komut dosyalarında sürümlerine karşılık geldiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="13017-138">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="13017-139">Özgün kod bloğunun komut dosyası başvuruları:</span><span class="sxs-lookup"><span data-stu-id="13017-139">Script references from the original code block:</span></span>

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. <span data-ttu-id="13017-140">Bunlar eşleşmiyorsa, güncelleştirme *.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="13017-140">If they don't match, update the *.cshtml* file.</span></span>

1. <span data-ttu-id="13017-141">Menü çubuğundan seçin **dosya** > **Tümünü Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="13017-141">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="13017-142">Örneği çalıştırma</span><span class="sxs-lookup"><span data-stu-id="13017-142">Run the Sample</span></span>

1. <span data-ttu-id="13017-143">Araç çubuğunda, açma **betik hata ayıklamasını** ve hata ayıklama modunda örneği çalıştırmak için Yürüt düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="13017-143">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Kullanıcı adı girin](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. <span data-ttu-id="13017-145">Tarayıcı açıldığında, sohbet kimliğinizi için bir ad girin.</span><span class="sxs-lookup"><span data-stu-id="13017-145">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="13017-146">Tarayıcıdan URL'yi kopyalayın, diğer iki tarayıcılar açın ve URL'leri adresi çubukları yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="13017-146">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="13017-147">Her tarayıcıda benzersiz bir ad girin.</span><span class="sxs-lookup"><span data-stu-id="13017-147">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="13017-148">Şimdi bir yorum girip seçin ekleyin **Gönder**.</span><span class="sxs-lookup"><span data-stu-id="13017-148">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="13017-149">Bu, diğer tarayıcılarda yineleyin.</span><span class="sxs-lookup"><span data-stu-id="13017-149">Repeat that in the other browsers.</span></span> <span data-ttu-id="13017-150">Açıklamalar, gerçek zamanlı olarak görünür.</span><span class="sxs-lookup"><span data-stu-id="13017-150">The comments appear in real time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="13017-151">Bu basit sohbet uygulaması sunucusunda tartışma bağlam korumaz.</span><span class="sxs-lookup"><span data-stu-id="13017-151">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="13017-152">Hub'ın tüm geçerli kullanıcılar yorum yayınlar.</span><span class="sxs-lookup"><span data-stu-id="13017-152">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="13017-153">Sohbet daha sonra katılmak kullanıcıların zamandan eklenen iletileri katılmaları görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="13017-153">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="13017-154">Sohbet uygulaması üç farklı tarayıcılarda nasıl çalıştığını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="13017-154">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="13017-155">Tüm tarayıcılar, Tom, Anand ve Susan iletileri gönderirken, gerçek zamanlı olarak güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="13017-155">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Üç tüm tarayıcılar aynı Sohbet geçmişini görüntüleme](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. <span data-ttu-id="13017-157">İçinde **Çözüm Gezgini**, inceleme **betik belgelerini** çalışan uygulama düğümü.</span><span class="sxs-lookup"><span data-stu-id="13017-157">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="13017-158">Adlı bir betik dosyası *hubs* , çalışma zamanında SignalR kitaplığı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="13017-158">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="13017-159">Bu dosya, jQuery betik sunucu tarafı kodu arasında iletişimi yönetir.</span><span class="sxs-lookup"><span data-stu-id="13017-159">This file manages the communication between jQuery script and server-side code.</span></span>

    ![Betik dosyaları düğümüne otomatik olarak oluşturulan hub komut dosyası](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="13017-161">Kod İnceleme</span><span class="sxs-lookup"><span data-stu-id="13017-161">Examine the Code</span></span>

<span data-ttu-id="13017-162">SignalR sohbet uygulaması iki temel SignalR geliştirme görevlerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="13017-162">The SignalR chat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="13017-163">Bir hub'ı oluşturma işlemini gösterir.</span><span class="sxs-lookup"><span data-stu-id="13017-163">It shows you how to create a hub.</span></span> <span data-ttu-id="13017-164">Sunucu ana koordinasyon nesnesi olarak, hub'ı kullanır.</span><span class="sxs-lookup"><span data-stu-id="13017-164">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="13017-165">Hub SignalR jQuery kitaplığı ileti göndermek ve almak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="13017-165">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="13017-166">SignalR hub'ları ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="13017-166">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="13017-167">Kod örneğinde, `ChatHub` sınıf türetilir `Microsoft.AspNet.SignalR.Hub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="13017-167">In the code sample, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="13017-168">Öğesinden türetme `Hub` SignalR uygulama oluşturmak için kullanışlı bir yöntem bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="13017-168">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="13017-169">Hub sınıfınıza genel yöntemleri oluşturun ve bu yöntemler bir web sayfasında komut dosyalarından çağırarak erişin.</span><span class="sxs-lookup"><span data-stu-id="13017-169">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="13017-170">İstemciler sohbet kodda çağrı `ChatHub.Send` yeni bir ileti göndermek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="13017-170">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="13017-171">Hub sırayla ileti tüm istemcilere çağırarak gönderen `Clients.All.addNewMessageToPage`.</span><span class="sxs-lookup"><span data-stu-id="13017-171">The hub in turn sends the message to all clients by calling `Clients.All.addNewMessageToPage`.</span></span>

<span data-ttu-id="13017-172">`Send` Yöntemi birkaç hub kavramları göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="13017-172">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="13017-173">İstemciler, bunları çağırabilirsiniz genel yöntemleri bir hub'da bildirin.</span><span class="sxs-lookup"><span data-stu-id="13017-173">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="13017-174">Kullanım `Microsoft.AspNet.SignalR.Hub.Clients` tüm istemcilerle iletişim kurmak için dinamik özellik bu hub'a bağlı.</span><span class="sxs-lookup"><span data-stu-id="13017-174">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="13017-175">İstemcide bir işlevi çağırmayı (gibi `addNewMessageToPage` işlevi) istemcilerini güncelleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="13017-175">Call a function on the client (like the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a><span data-ttu-id="13017-176">SignalR ve jQuery Chat.cshtml</span><span class="sxs-lookup"><span data-stu-id="13017-176">SignalR and jQuery Chat.cshtml</span></span>

<span data-ttu-id="13017-177">*Chat.cshtml* görünüm dosyası kod örneğinde SignalR jQuery kitaplığının bir SignalR hub'ı ile iletişim kurmak için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="13017-177">The *Chat.cshtml* view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span>  <span data-ttu-id="13017-178">Kod, birçok önemli görevleri gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="13017-178">The code carries out many important tasks.</span></span> <span data-ttu-id="13017-179">Bu, otomatik olarak oluşturulan proxy hub için bir başvuru oluşturur, içeriği istemcilere göndermek için sunucu çağırabilir ve hub'ına ileti göndermek için bir bağlantı başlatır bir işlev bildirir.</span><span class="sxs-lookup"><span data-stu-id="13017-179">It creates a reference to the autogenerated proxy for the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="13017-180">JavaScript'te, sunucu sınıfını ve üyelerini camelCase başvurudur.</span><span class="sxs-lookup"><span data-stu-id="13017-180">In JavaScript, the reference to the server class and its members is in camelCase.</span></span> <span data-ttu-id="13017-181">Kod örneği başvuru C# `ChatHub` JavaScript olarak sınıfında `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="13017-181">The code sample references the C# `ChatHub` class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="13017-182">Bu kod bloğu içinde bir geri çağırma işlevini betiğinde oluşturun.</span><span class="sxs-lookup"><span data-stu-id="13017-182">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="13017-183">Sunucudaki hub sınıfına içerik güncelleştirmeleri her bir istemciye göndermek için bu işlevi çağırır.</span><span class="sxs-lookup"><span data-stu-id="13017-183">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="13017-184">İsteğe bağlı çağrısı `htmlEncode` sayfasında görüntülemeden önce ileti içeriği kodlama HTML şekilde işlev gösterir.</span><span class="sxs-lookup"><span data-stu-id="13017-184">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page.</span></span> <span data-ttu-id="13017-185">Bu kod eklemesini engellemek üzere bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="13017-185">It's a way to prevent script injection.</span></span>

<span data-ttu-id="13017-186">Bu kod hub'ı ile bir bağlantı açar.</span><span class="sxs-lookup"><span data-stu-id="13017-186">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> <span data-ttu-id="13017-187">Bu yaklaşım, olay işleyici yürütülmeden önce bir bağlantı oluşturmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="13017-187">This approach ensures that you establish a connection before the event handler executes.</span></span>

<span data-ttu-id="13017-188">Kod bağlantı başlar ve ardından üzerinde click olayını işlemek için bir işlev geçirir **Gönder** sohbet sayfasında düğme.</span><span class="sxs-lookup"><span data-stu-id="13017-188">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="13017-189">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="13017-189">Additional resources</span></span>

<span data-ttu-id="13017-190">SignalR hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="13017-190">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="13017-191">SignalR projesi</span><span class="sxs-lookup"><span data-stu-id="13017-191">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="13017-192">SignalR GitHub ve örnekleri</span><span class="sxs-lookup"><span data-stu-id="13017-192">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="13017-193">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="13017-193">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="13017-194">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="13017-194">Next steps</span></span>

<span data-ttu-id="13017-195">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="13017-195">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="13017-196">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="13017-196">Set up the project</span></span>
> * <span data-ttu-id="13017-197">Örnek çalıştı</span><span class="sxs-lookup"><span data-stu-id="13017-197">Ran the sample</span></span>
> * <span data-ttu-id="13017-198">Dio</span><span class="sxs-lookup"><span data-stu-id="13017-198">Examined the code</span></span>

<span data-ttu-id="13017-199">Yüksek frekanslı Mesajlaşma işlevleri sağlamak için ASP.NET SignalR 2 kullanan bir web uygulamasının nasıl oluşturulacağını öğrenmek için sonraki makaleye ilerleyin.</span><span class="sxs-lookup"><span data-stu-id="13017-199">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="13017-200">Yüksek frekanslı Mesajlaşma ile web uygulaması</span><span class="sxs-lookup"><span data-stu-id="13017-200">Web app with high-frequency messaging</span></span>](tutorial-high-frequency-realtime-with-signalr.md)