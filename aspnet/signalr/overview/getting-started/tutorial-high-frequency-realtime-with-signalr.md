---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Öğretici: SignalR 2 ile yüksek sıklıkta gerçek zamanlı uygulama oluşturma | Microsoft Docs'
author: bradygaster
description: Bu öğretici, ASP.NET SignalR, yüksek frekanslı Mesajlaşma işlevleri sağlamak için kullanan bir web uygulaması oluşturma işlemi gösterilmektedir.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 44aaa2b0c059de310e963f642fa56c2f00a7e443
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836733"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a><span data-ttu-id="550df-103">Öğretici: SignalR 2 ile yüksek sıklıkta gerçek zamanlı uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="550df-103">Tutorial: Create high-frequency real-time app with SignalR 2</span></span>

<span data-ttu-id="550df-104">Bu öğreticide, yüksek frekanslı Mesajlaşma işlevleri sağlamak için ASP.NET SignalR 2 kullanan bir web uygulaması oluşturma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="550df-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span> <span data-ttu-id="550df-105">Bu durumda, "yüksek sıklıkta ileti" sunucu sabit bir fiyat karşılığında güncelleştirmeler gönderir anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="550df-105">In this case, "high-frequency messaging" means the server sends updates at a fixed rate.</span></span> <span data-ttu-id="550df-106">Saniyede en fazla 10 iletileri gönderir.</span><span class="sxs-lookup"><span data-stu-id="550df-106">You send up to 10 messages a second.</span></span>

<span data-ttu-id="550df-107">Oluşturduğunuz uygulama, kullanıcıların sürükleyebilirsiniz bir şekil görüntüler.</span><span class="sxs-lookup"><span data-stu-id="550df-107">The application you create displays a shape that users can drag.</span></span> <span data-ttu-id="550df-108">Sunucu, Zamanlanmış güncelleştirmeleri kullanarak sürüklenen şekli konumunu eşleşecek şekilde tüm bağlı tarayıcıların şekil konumunu güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="550df-108">The server updates the position of the shape in all connected browsers to match the position of the dragged shape using timed updates.</span></span>

<span data-ttu-id="550df-109">Bu öğreticide tanıtılan kavramları, gerçek zamanlı oyun uygulamaları ve diğer benzetimi uygulamaları vardır.</span><span class="sxs-lookup"><span data-stu-id="550df-109">Concepts introduced in this tutorial have applications in real-time gaming and other simulation applications.</span></span>

<span data-ttu-id="550df-110">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="550df-110">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="550df-111">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="550df-111">Set up the project</span></span>
> * <span data-ttu-id="550df-112">Temel uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="550df-112">Create the base application</span></span>
> * <span data-ttu-id="550df-113">Uygulama başlatıldığında hub'ına eşleme</span><span class="sxs-lookup"><span data-stu-id="550df-113">Map to the hub when app starts</span></span>
> * <span data-ttu-id="550df-114">İstemci Ekle</span><span class="sxs-lookup"><span data-stu-id="550df-114">Add the client</span></span>
> * <span data-ttu-id="550df-115">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="550df-115">Run the app</span></span>
> * <span data-ttu-id="550df-116">İstemci döngü Ekle</span><span class="sxs-lookup"><span data-stu-id="550df-116">Add the client loop</span></span>
> * <span data-ttu-id="550df-117">Sunucu döngü Ekle</span><span class="sxs-lookup"><span data-stu-id="550df-117">Add the server loop</span></span>
> * <span data-ttu-id="550df-118">Kesintisiz animasyon ekleme</span><span class="sxs-lookup"><span data-stu-id="550df-118">Add smooth animation</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="550df-119">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="550df-119">Prerequisites</span></span>

* <span data-ttu-id="550df-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) ile **ASP.NET ve web geliştirme** iş yükü.</span><span class="sxs-lookup"><span data-stu-id="550df-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="550df-121">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="550df-121">Set up the project</span></span>

<span data-ttu-id="550df-122">Bu bölümde, Visual Studio 2017'de bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="550df-122">In this section, you create the project in Visual Studio 2017.</span></span>

<span data-ttu-id="550df-123">Bu bölümde, boş bir ASP.NET Web uygulaması oluşturma ve SignalR ve jQuery.UI kitaplıkları eklemek için Visual Studio 2017'yi kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="550df-123">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application and add the SignalR and jQuery.UI libraries.</span></span>

1. <span data-ttu-id="550df-124">Visual Studio kullanarak ASP.NET Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="550df-124">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Web oluşturma](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. <span data-ttu-id="550df-126">İçinde **yeni ASP.NET Web uygulaması - MoveShapeDemo** penceresinde bırakın **boş** seçin ve seçilen **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="550df-126">In the **New ASP.NET Web Application - MoveShapeDemo** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="550df-127">İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="550df-127">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="550df-128">İçinde **Yeni Öğe Ekle - MoveShapeDemo**seçin **yüklü** > **Visual C#**   >  **Web**  >  **SignalR** seçip **SignalR Hub sınıfı (v2)**.</span><span class="sxs-lookup"><span data-stu-id="550df-128">In **Add New Item - MoveShapeDemo**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="550df-129">Sınıf adı *MoveShapeHub* ve projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="550df-129">Name the class *MoveShapeHub* and add it to the project.</span></span>

    <span data-ttu-id="550df-130">Bu adımda oluşturulur *MoveShapeHub.cs* sınıf dosyası.</span><span class="sxs-lookup"><span data-stu-id="550df-130">This step creates the *MoveShapeHub.cs* class file.</span></span> <span data-ttu-id="550df-131">Aynı anda bir dizi komut dosyaları ve projeye Signalr'yi destekleyen derleme başvurularını ekler.</span><span class="sxs-lookup"><span data-stu-id="550df-131">Simultaneously, it adds  a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="550df-132">Seçin **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="550df-132">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

1. <span data-ttu-id="550df-133">İçinde **Paket Yöneticisi Konsolu**, şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="550df-133">In **Package Manager Console**, run this command:</span></span>

    ```console
    Install-Package jQuery.UI.Combined
    ```

    <span data-ttu-id="550df-134">Komut, jQuery kullanıcı Arabirimi kitaplığı yükler.</span><span class="sxs-lookup"><span data-stu-id="550df-134">The command installs the jQuery UI library.</span></span> <span data-ttu-id="550df-135">Şekil animasyon uygulamak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="550df-135">You use it to animate the shape.</span></span>

1. <span data-ttu-id="550df-136">İçinde **Çözüm Gezgini**, betikleri düğümünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="550df-136">In **Solution Explorer**, expand the Scripts node.</span></span>

    ![Komut dosyası kitaplık başvuruları](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    <span data-ttu-id="550df-138">Betik kitaplıkları için jQuery jQueryUI ve SignalR projesinde görünür.</span><span class="sxs-lookup"><span data-stu-id="550df-138">Script libraries for jQuery, jQueryUI, and SignalR are visible in the project.</span></span>

## <a name="create-the-base-application"></a><span data-ttu-id="550df-139">Temel uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="550df-139">Create the base application</span></span>

<span data-ttu-id="550df-140">Bu bölümde, bir tarayıcı uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="550df-140">In this section, you create a browser application.</span></span> <span data-ttu-id="550df-141">Uygulama, her fare hareketi olayını sırasında şekil konumunu sunucusuna gönderir.</span><span class="sxs-lookup"><span data-stu-id="550df-141">The app sends the location of the shape to the server during each mouse move event.</span></span> <span data-ttu-id="550df-142">Sunucu, bu bilgiler diğer bağlı istemcilere gerçek zamanlı olarak yayınlar.</span><span class="sxs-lookup"><span data-stu-id="550df-142">The server broadcasts this information to all other connected clients in real time.</span></span> <span data-ttu-id="550df-143">Sonraki bölümlerde bu uygulama hakkında daha fazla bilgi.</span><span class="sxs-lookup"><span data-stu-id="550df-143">You learn more about this application in later sections.</span></span>

1. <span data-ttu-id="550df-144">Açık *MoveShapeHub.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="550df-144">Open the *MoveShapeHub.cs* file.</span></span>

1. <span data-ttu-id="550df-145">Değiştirin *MoveShapeHub.cs* Bu kod dosyası:</span><span class="sxs-lookup"><span data-stu-id="550df-145">Replace the code in the *MoveShapeHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="550df-146">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="550df-146">Save the file.</span></span>

<span data-ttu-id="550df-147">`MoveShapeHub` Uygulaması bir SignalR hub'ın bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="550df-147">The `MoveShapeHub` class is an implementation of a SignalR hub.</span></span> <span data-ttu-id="550df-148">Olarak [SignalR ile çalışmaya başlama](tutorial-getting-started-with-signalr.md) Öğreticisi, hub'ına istemcileri doğrudan çağıran bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="550df-148">As in the [Getting Started with SignalR](tutorial-getting-started-with-signalr.md) tutorial, the hub has a method that the clients call directly.</span></span> <span data-ttu-id="550df-149">Bu durumda, bir nesne yeni X ve Y koordinatları sunucuya şeklin istemci gönderir.</span><span class="sxs-lookup"><span data-stu-id="550df-149">In this case, the client sends an object with the new X and Y coordinates of the shape to the server.</span></span> <span data-ttu-id="550df-150">Bu koordinatları, diğer tüm bağlı istemcileri yayımladınız.</span><span class="sxs-lookup"><span data-stu-id="550df-150">Those coordinates get broadcasted to all other connected clients.</span></span> <span data-ttu-id="550df-151">SignalR otomatik olarak JSON'ı kullanarak bu nesneyi serileştirir.</span><span class="sxs-lookup"><span data-stu-id="550df-151">SignalR automatically serializes this object using JSON.</span></span>

<span data-ttu-id="550df-152">Uygulama gönderen `ShapeModel` istemciye nesne.</span><span class="sxs-lookup"><span data-stu-id="550df-152">The app sends the `ShapeModel` object to the client.</span></span> <span data-ttu-id="550df-153">Şeklin konum depolamak için üyeleri var.</span><span class="sxs-lookup"><span data-stu-id="550df-153">It has members to store the position of the shape.</span></span> <span data-ttu-id="550df-154">Sunucuda nesnenin hangi istemci verilerinin depolandığını izlemek için bir üyesi de sahiptir.</span><span class="sxs-lookup"><span data-stu-id="550df-154">The version of the object on the server also has a member to track which client's data is being stored.</span></span> <span data-ttu-id="550df-155">Bu nesne, sunucunun geri kendisine bir istemcinin veri göndermesini engeller.</span><span class="sxs-lookup"><span data-stu-id="550df-155">This object prevents the server from sending a client's data back to itself.</span></span> <span data-ttu-id="550df-156">Bu üyeyi kullanan `JsonIgnore` uygulama verileri seri hale getirme ve istemciye geri göndermeden tutmak için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="550df-156">This member uses the `JsonIgnore` attribute to keep the application from serializing the data and sending it back to the client.</span></span>

## <a name="map-to-the-hub-when-app-starts"></a><span data-ttu-id="550df-157">Uygulama başlatıldığında hub'ına eşleme</span><span class="sxs-lookup"><span data-stu-id="550df-157">Map to the hub when app starts</span></span>

<span data-ttu-id="550df-158">Ardından, uygulama başlatıldığında hub'ına eşleme ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="550df-158">Next, you set up mapping to the hub when the application starts.</span></span> <span data-ttu-id="550df-159">SignalR 2'de bir OWIN başlangıç sınıfı ekleme eşleme oluşturur.</span><span class="sxs-lookup"><span data-stu-id="550df-159">In SignalR 2, adding an OWIN startup class creates the mapping.</span></span>

1. <span data-ttu-id="550df-160">İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="550df-160">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="550df-161">İçinde **Yeni Öğe Ekle - MoveShapeDemo** seçin **yüklü** > **Visual C#**   >  **Web** ve ardından seçin **OWIN başlangıç sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="550df-161">In **Add New Item - MoveShapeDemo** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="550df-162">Sınıf adı *başlangıç* seçip **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="550df-162">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="550df-163">Varsayılan kodda değiştirin *Startup.cs* Bu kod dosyası:</span><span class="sxs-lookup"><span data-stu-id="550df-163">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<span data-ttu-id="550df-164">OWIN başlangıç sınıfı çağrıları `MapSignalR` zaman uygulama yürütür `Configuration` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="550df-164">The OWIN startup class calls `MapSignalR` when the app executes the `Configuration` method.</span></span> <span data-ttu-id="550df-165">OWIN'ın başlangıç sınıfa işlemi kullanarak uygulamanın eklediği `OwinStartup` derleme özniteliği.</span><span class="sxs-lookup"><span data-stu-id="550df-165">The app adds the class to OWIN's startup process using the `OwinStartup` assembly attribute.</span></span>

## <a name="add-the-client"></a><span data-ttu-id="550df-166">İstemci Ekle</span><span class="sxs-lookup"><span data-stu-id="550df-166">Add the client</span></span>

<span data-ttu-id="550df-167">HTML sayfası için istemci ekleyin.</span><span class="sxs-lookup"><span data-stu-id="550df-167">Add the HTML page for the client.</span></span>

1. <span data-ttu-id="550df-168">İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **HTML sayfası**.</span><span class="sxs-lookup"><span data-stu-id="550df-168">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="550df-169">Sayfayı adlandırın **varsayılan** seçip **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="550df-169">Name the page **Default** and select **OK**.</span></span>

1. <span data-ttu-id="550df-170">İçinde **Çözüm Gezgini**, sağ *Default.html* seçip **Başlangıç Sayfası Ayarla**.</span><span class="sxs-lookup"><span data-stu-id="550df-170">In **Solution Explorer**, right-click *Default.html* and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="550df-171">Varsayılan kodda değiştirin *Default.html* Bu kod dosyası:</span><span class="sxs-lookup"><span data-stu-id="550df-171">Replace the default code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. <span data-ttu-id="550df-172">İçinde **Çözüm Gezgini**, genişletme **betikleri**.</span><span class="sxs-lookup"><span data-stu-id="550df-172">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="550df-173">JQuery ve SignalR için betik kitaplıkları projesinde görünür.</span><span class="sxs-lookup"><span data-stu-id="550df-173">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="550df-174">Paket Yöneticisi SignalR betikleri daha sonraki bir sürümünü yükler.</span><span class="sxs-lookup"><span data-stu-id="550df-174">The package manager installs a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="550df-175">Komut dosyalarını projedeki sürümleri değerine karşılık gelen kod bloğundaki komut dosyası başvuruları güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="550df-175">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

<span data-ttu-id="550df-176">Bu HTML ve JavaScript kodu kırmızı oluşturur `div` adlı `shape`.</span><span class="sxs-lookup"><span data-stu-id="550df-176">This HTML and JavaScript code creates a red `div` called `shape`.</span></span> <span data-ttu-id="550df-177">JQuery kitaplığını kullanarak şeklin sürükleyerek davranışını etkinleştirir ve kullandığı `drag` şeklin konum sunucuya göndermek için olay.</span><span class="sxs-lookup"><span data-stu-id="550df-177">It enables the shape's dragging behavior using the jQuery library and uses the `drag` event to send the shape's position to the server.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="550df-178">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="550df-178">Run the app</span></span>

<span data-ttu-id="550df-179">Uygulamayı çalıştırdığınız için se'e çalışabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="550df-179">You can run the app to se\`e it work.</span></span> <span data-ttu-id="550df-180">Bir tarayıcı penceresi şekli sürüklediğinizde, Şekil diğer tarayıcılarda çok taşır.</span><span class="sxs-lookup"><span data-stu-id="550df-180">When you drag the shape around a browser window, the shape moves in the other browsers too.</span></span>

1. <span data-ttu-id="550df-181">Araç çubuğunda, açma **betik hata ayıklamasını** ve ardından uygulamayı hata ayıklama modunda çalıştırmak için Yürüt düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="550df-181">In the toolbar, turn on **Script Debugging** and then select the play button to run the application in Debug mode.</span></span>

    ![Hata ayıklama modu ve Çalıştır'ı seçerek kullanıcı görüntüsü.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    <span data-ttu-id="550df-183">Sağ üst köşesindeki kırmızı şeklinde bir tarayıcı penceresi açılır.</span><span class="sxs-lookup"><span data-stu-id="550df-183">A browser window opens with the red shape in the upper-right corner.</span></span>

1. <span data-ttu-id="550df-184">Sayfanın URL'sini kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="550df-184">Copy the page's URL.</span></span>

1. <span data-ttu-id="550df-185">Başka bir tarayıcı açın ve adres çubuğuna URL'yi yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="550df-185">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="550df-186">Şekil tarayıcı pencerelerini birinde sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="550df-186">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="550df-187">Şekil bir tarayıcı penceresinde izler.</span><span class="sxs-lookup"><span data-stu-id="550df-187">The shape in the other browser window follows.</span></span>

<span data-ttu-id="550df-188">Uygulama çalışırken İşlevler, bu yöntemi kullanarak, önerilen bir programlama modeli değil.</span><span class="sxs-lookup"><span data-stu-id="550df-188">While the application functions using this method, it's not a recommended programming model.</span></span> <span data-ttu-id="550df-189">Gönderilen ileti sayısı üst sınırı yoktur.</span><span class="sxs-lookup"><span data-stu-id="550df-189">There's no upper limit to the number of messages getting sent.</span></span> <span data-ttu-id="550df-190">Sonuç olarak, istemciler ve sunucu iletileri ile dolmasını ve performansı düşürür.</span><span class="sxs-lookup"><span data-stu-id="550df-190">As a result, the clients and server get overwhelmed with messages and performance degrades.</span></span> <span data-ttu-id="550df-191">Ayrıca, uygulama istemcide kopuk bir animasyon görüntüler.</span><span class="sxs-lookup"><span data-stu-id="550df-191">Also, the app displays a disjointed animation on the client.</span></span> <span data-ttu-id="550df-192">Şekil anında her yöntemle taşıdığı düzensiz animasyon gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="550df-192">This jerky animation happens because the shape moves instantly by each method.</span></span> <span data-ttu-id="550df-193">Şekil sorunsuz her yeni bir konuma taşınırsa, daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="550df-193">It's better if the shape moves smoothly to each new location.</span></span> <span data-ttu-id="550df-194">Ardından, bu sorunları gidermek nasıl öğrenin.</span><span class="sxs-lookup"><span data-stu-id="550df-194">Next, you learn how to fix those issues.</span></span>

## <a name="add-the-client-loop"></a><span data-ttu-id="550df-195">İstemci döngü Ekle</span><span class="sxs-lookup"><span data-stu-id="550df-195">Add the client loop</span></span>

<span data-ttu-id="550df-196">Her fare hareketi olayını şeklinizde konumunu gönderme gereksiz miktarda ağ trafiği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="550df-196">Sending the location of the shape on every mouse move event creates an unnecessary amount of network traffic.</span></span> <span data-ttu-id="550df-197">Uygulama, istemciden gelen iletileri kısıtlama gerekir.</span><span class="sxs-lookup"><span data-stu-id="550df-197">The app needs to throttle the messages from the client.</span></span>

<span data-ttu-id="550df-198">Javascript kullanan `setInterval` sabit bir hızda sunucuya yeni konum bilgileri gönderen bir döngü ayarlamak için işlevi.</span><span class="sxs-lookup"><span data-stu-id="550df-198">Use the javascript `setInterval` function to set up a loop that sends new position information to the server at a fixed rate.</span></span> <span data-ttu-id="550df-199">Bu döngü bir "Oyun döngüsü." temel gösterimidir</span><span class="sxs-lookup"><span data-stu-id="550df-199">This loop is a basic representation of a "game loop."</span></span> <span data-ttu-id="550df-200">Oyun işlevselliğini tüm sürücüleri tekrar tekrar çağrılan bir işlevdir.</span><span class="sxs-lookup"><span data-stu-id="550df-200">It's a repeatedly called function that drives all the functionality of a game.</span></span>

1. <span data-ttu-id="550df-201">İstemci kodu değiştirin *Default.html* Bu kod dosyası:</span><span class="sxs-lookup"><span data-stu-id="550df-201">Replace the client code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > <span data-ttu-id="550df-202">Komut dosyası başvuruları yeniden değiştirmek zorunda.</span><span class="sxs-lookup"><span data-stu-id="550df-202">You have to replace the script references again.</span></span> <span data-ttu-id="550df-203">Bunlar, projeyi betiklerde sürümleri eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="550df-203">They must match the versions of the scripts in the project.</span></span>

    <span data-ttu-id="550df-204">Bu yeni bir kod ekler `updateServerModel` işlevi.</span><span class="sxs-lookup"><span data-stu-id="550df-204">This new code adds the `updateServerModel` function.</span></span> <span data-ttu-id="550df-205">Bir sabit sıklığına adlı.</span><span class="sxs-lookup"><span data-stu-id="550df-205">It gets called on a fixed frequency.</span></span> <span data-ttu-id="550df-206">İşlev konum verileri sunucusuna gönderir. her `moved` bayrağı, yeni konum verileri göndermek için olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="550df-206">The function sends the position data to the server whenever the `moved` flag indicates that there's new position data to send.</span></span>

1. <span data-ttu-id="550df-207">Uygulamayı başlatmak için Yürüt düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="550df-207">Select the play button to start the application</span></span>

1. <span data-ttu-id="550df-208">Sayfanın URL'sini kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="550df-208">Copy the page's URL.</span></span>

1. <span data-ttu-id="550df-209">Başka bir tarayıcı açın ve adres çubuğuna URL'yi yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="550df-209">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="550df-210">Şekil tarayıcı pencerelerini birinde sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="550df-210">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="550df-211">Şekil bir tarayıcı penceresinde izler.</span><span class="sxs-lookup"><span data-stu-id="550df-211">The shape in the other browser window follows.</span></span>

<span data-ttu-id="550df-212">Uygulama animasyon kesintisiz olarak görünmez sunucuya gönderilen ileti sayısını kısıtlar bu yana ilk başta vermedi.</span><span class="sxs-lookup"><span data-stu-id="550df-212">Since the app throttles the number of messages that get sent to the server, the animation won't appear as smooth did at first.</span></span>

## <a name="add-the-server-loop"></a><span data-ttu-id="550df-213">Sunucu döngü Ekle</span><span class="sxs-lookup"><span data-stu-id="550df-213">Add the server loop</span></span>

<span data-ttu-id="550df-214">Geçerli uygulamada, sunucudan istemciye gönderilen iletileri alındıklarında sıklıkta gönderilir.</span><span class="sxs-lookup"><span data-stu-id="550df-214">In the current application, messages sent from the server to the client go out as often as they're received.</span></span> <span data-ttu-id="550df-215">İstemcide görüyoruz gibi benzer bir sorun bu ağ trafiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="550df-215">This network traffic presents a similar problem as we see on the client.</span></span>

<span data-ttu-id="550df-216">Uygulamayı, ihtiyaç duyulan daha sık ileti gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="550df-216">The app can send messages more often than they're needed.</span></span> <span data-ttu-id="550df-217">Sonuç olarak bağlantı yayılmamış olabilir.</span><span class="sxs-lookup"><span data-stu-id="550df-217">The connection can become flooded as a result.</span></span> <span data-ttu-id="550df-218">Bu bölümde, giden iletiler oranını kısıtlar Zamanlayıcı eklemek için sunucu güncelleştirme işlemi açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="550df-218">This section describes how to update the server to add a timer that throttles the rate of the outgoing messages.</span></span>

1. <span data-ttu-id="550df-219">Öğesinin içeriğini değiştirin `MoveShapeHub.cs` Bu kod ile:</span><span class="sxs-lookup"><span data-stu-id="550df-219">Replace the contents of `MoveShapeHub.cs` with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. <span data-ttu-id="550df-220">Uygulamayı başlatmak için Yürüt düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="550df-220">Select the play button to start the application.</span></span>

1. <span data-ttu-id="550df-221">Sayfanın URL'sini kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="550df-221">Copy the page's URL.</span></span>

1. <span data-ttu-id="550df-222">Başka bir tarayıcı açın ve adres çubuğuna URL'yi yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="550df-222">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="550df-223">Şekil tarayıcı pencerelerini birinde sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="550df-223">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="550df-224">Bu kod eklemek için istemci genişletir `Broadcaster` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="550df-224">This code expands the client to add the `Broadcaster` class.</span></span> <span data-ttu-id="550df-225">Yeni bir sınıf kullanarak giden iletiler kısıtlar `Timer` .NET Framework sınıfı.</span><span class="sxs-lookup"><span data-stu-id="550df-225">The new class throttles the outgoing messages using the `Timer` class from the .NET framework.</span></span>

<span data-ttu-id="550df-226">Hub geçici olduğunu öğrenmek uygundur.</span><span class="sxs-lookup"><span data-stu-id="550df-226">It's good to learn that the hub itself is transitory.</span></span> <span data-ttu-id="550df-227">Gereken her zaman oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="550df-227">It's created every time it's needed.</span></span> <span data-ttu-id="550df-228">Uygulamayı oluşturur `Broadcaster` singleton olarak.</span><span class="sxs-lookup"><span data-stu-id="550df-228">So the app creates the `Broadcaster` as a singleton.</span></span> <span data-ttu-id="550df-229">Yavaş başlatma erteleneceği kullanan `Broadcaster`'s, gerekli olana kadar oluşturma.</span><span class="sxs-lookup"><span data-stu-id="550df-229">It uses lazy initialization to defer the `Broadcaster`'s creation until it's needed.</span></span> <span data-ttu-id="550df-230">Bu, uygulamanın ilk hub örneği tamamen Zamanlayıcı başlatmadan önce oluşturduğu garanti eder.</span><span class="sxs-lookup"><span data-stu-id="550df-230">That guarantees that the app creates the first hub instance completely before starting the timer.</span></span>

<span data-ttu-id="550df-231">İstemcilerin çağrısı `UpdateShape` işlevi ardından hub dışında taşınmış `UpdateModel` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="550df-231">The call to the clients' `UpdateShape` function is then moved out of the hub's `UpdateModel` method.</span></span> <span data-ttu-id="550df-232">Artık her hemen uygulama gelen iletiler aldığında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="550df-232">It's no longer called immediately whenever the app receives incoming messages.</span></span> <span data-ttu-id="550df-233">Bunun yerine, uygulama, saniye başına 25 çağrılarının bir hızda istemcilere iletileri gönderir.</span><span class="sxs-lookup"><span data-stu-id="550df-233">Instead, the app sends the messages to the clients at a rate of 25 calls per second.</span></span> <span data-ttu-id="550df-234">İşlem tarafından yönetilen `_broadcastLoop` içinden Zamanlayıcı `Broadcaster` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="550df-234">The process is  managed by the `_broadcastLoop` timer from within the `Broadcaster` class.</span></span>

<span data-ttu-id="550df-235">Son olarak, istemci yöntemi hub'dan doğrudan çağırmak yerine `Broadcaster` sınıfı şu anda çalışan bir başvuru almak için gereksinim duyduğu `_hubContext` hub.</span><span class="sxs-lookup"><span data-stu-id="550df-235">Lastly, instead of calling the client method from the hub directly, the `Broadcaster` class needs to get a reference to the currently operating `_hubContext` hub.</span></span> <span data-ttu-id="550df-236">Başvuru ile alır `GlobalHost`.</span><span class="sxs-lookup"><span data-stu-id="550df-236">It gets the reference with the `GlobalHost`.</span></span>

## <a name="add-smooth-animation"></a><span data-ttu-id="550df-237">Kesintisiz animasyon ekleme</span><span class="sxs-lookup"><span data-stu-id="550df-237">Add smooth animation</span></span>

<span data-ttu-id="550df-238">Neredeyse tamamlanmış bir uygulamadır, ancak biz tek daha fazla iyileştirme yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="550df-238">The application is almost finished, but we could make one more improvement.</span></span> <span data-ttu-id="550df-239">Uygulama sunucusu iletilere yanıt olarak istemcide şekli taşır.</span><span class="sxs-lookup"><span data-stu-id="550df-239">The app moves the shape on the client in response to server messages.</span></span> <span data-ttu-id="550df-240">JQuery kullanıcı Arabirimi kitaplık sunucusu tarafından verilen yeni bir konuma şekil konumunu ayarlamak yerine kullanın `animate` işlevi.</span><span class="sxs-lookup"><span data-stu-id="550df-240">Instead of setting the position of the shape to the new location given by the server, use the JQuery UI library's `animate` function.</span></span> <span data-ttu-id="550df-241">Bu şeklin sorunsuz geçerli ve yeni konumu arasında taşıyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="550df-241">It can move the shape smoothly between its current and new position.</span></span>

1. <span data-ttu-id="550df-242">İstemci güncelleştirme `updateShape` yönteminde *Default.html* dosyasının vurgulanan kod gibi:</span><span class="sxs-lookup"><span data-stu-id="550df-242">Update the client's `updateShape` method in the *Default.html* file to look like the highlighted code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. <span data-ttu-id="550df-243">Uygulamayı başlatmak için Yürüt düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="550df-243">Select the play button to start the application.</span></span>

1. <span data-ttu-id="550df-244">Sayfanın URL'sini kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="550df-244">Copy the page's URL.</span></span>

1. <span data-ttu-id="550df-245">Başka bir tarayıcı açın ve adres çubuğuna URL'yi yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="550df-245">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="550df-246">Şekil tarayıcı pencerelerini birinde sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="550df-246">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="550df-247">Diğer pencere şeklinde hareketini daha az düzensiz görünür.</span><span class="sxs-lookup"><span data-stu-id="550df-247">The movement of the shape in the other window appears less jerky.</span></span> <span data-ttu-id="550df-248">Uygulama hareketi gelen ileti başına bir kez ayarlanan yerine zaman içinde ilişkilendirir.</span><span class="sxs-lookup"><span data-stu-id="550df-248">The app interpolates its movement over time rather than being set once per incoming message.</span></span>

<span data-ttu-id="550df-249">Bu kod, yeni bir tane eski konumdan şekli taşır.</span><span class="sxs-lookup"><span data-stu-id="550df-249">This code moves the shape from the old location to the new one.</span></span> <span data-ttu-id="550df-250">Sunucu, animasyon zaman aralığı boyunca şekil konumunu sağlar.</span><span class="sxs-lookup"><span data-stu-id="550df-250">The server gives the position of the shape over the course of the animation interval.</span></span> <span data-ttu-id="550df-251">Bu durumda, 100 milisaniyeden kısadır.</span><span class="sxs-lookup"><span data-stu-id="550df-251">In this case, that's 100 milliseconds.</span></span> <span data-ttu-id="550df-252">Uygulama, yeni animasyon başlatılmadan önce şekli üzerinde çalışan herhangi bir önceki animasyon temizler.</span><span class="sxs-lookup"><span data-stu-id="550df-252">The app clears any previous animation running on the shape before the new animation starts.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="550df-253">Kodu alma</span><span class="sxs-lookup"><span data-stu-id="550df-253">Get the code</span></span>

[<span data-ttu-id="550df-254">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="550df-254">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

## <a name="additional-resources"></a><span data-ttu-id="550df-255">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="550df-255">Additional resources</span></span>

<span data-ttu-id="550df-256">Yalnızca öğrendiğiniz hakkında iletişim paradigma çevrimiçi oyunlar ve diğer simülasyonları gibi geliştirmek için kullanışlıdır [SignalR ile oluşturulan ShootR game](https://shootr.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="550df-256">The communication paradigm you just learned about is useful for developing online games and other simulations, like [the ShootR game created with SignalR](https://shootr.azurewebsites.net/).</span></span>

<span data-ttu-id="550df-257">SignalR hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="550df-257">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="550df-258">SignalR projesi</span><span class="sxs-lookup"><span data-stu-id="550df-258">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="550df-259">SignalR GitHub ve örnekleri</span><span class="sxs-lookup"><span data-stu-id="550df-259">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="550df-260">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="550df-260">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="550df-261">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="550df-261">Next steps</span></span>

<span data-ttu-id="550df-262">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="550df-262">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="550df-263">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="550df-263">Set up the project</span></span>
> * <span data-ttu-id="550df-264">Temel bir uygulama oluşturdu</span><span class="sxs-lookup"><span data-stu-id="550df-264">Created the base application</span></span>
> * <span data-ttu-id="550df-265">Uygulama başlatıldığında hub'ına eşlendi</span><span class="sxs-lookup"><span data-stu-id="550df-265">Mapped to the hub when app starts</span></span>
> * <span data-ttu-id="550df-266">İstemci eklendi</span><span class="sxs-lookup"><span data-stu-id="550df-266">Added the client</span></span>
> * <span data-ttu-id="550df-267">Bir uygulamayı çalıştırdınız</span><span class="sxs-lookup"><span data-stu-id="550df-267">Ran the app</span></span>
> * <span data-ttu-id="550df-268">İstemci döngüsü eklendi</span><span class="sxs-lookup"><span data-stu-id="550df-268">Added the client loop</span></span>
> * <span data-ttu-id="550df-269">Sunucu döngü eklendi</span><span class="sxs-lookup"><span data-stu-id="550df-269">Added the server loop</span></span>
> * <span data-ttu-id="550df-270">Eklenen kesintisiz animasyon</span><span class="sxs-lookup"><span data-stu-id="550df-270">Added smooth animation</span></span>

<span data-ttu-id="550df-271">ASP.NET SignalR 2 sunucu yayın işlevselliği sağlamak için kullandığı bir web uygulamasının nasıl oluşturulacağını öğrenmek için sonraki makaleye ilerleyin.</span><span class="sxs-lookup"><span data-stu-id="550df-271">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="550df-272">SignalR 2 ile sunucu yayını</span><span class="sxs-lookup"><span data-stu-id="550df-272">SignalR 2 and server broadcasting</span></span>](tutorial-server-broadcast-with-signalr.md)