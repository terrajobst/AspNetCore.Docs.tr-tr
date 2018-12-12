---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Bir Azure Web rolünde SignalR performans sayaçlarını kullanarak | Microsoft Docs
author: guardrex
description: Nasıl yükleyin ve bir Azure Web rolünde SignalR performans sayaçları kullanma.
keywords: ASP.NET,signalr,Performance sayacı, azure web rolü
ms.author: riande
ms.date: 10/03/2018
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: bdd875201895c6eaf155b54582d0898c2570d93c
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287722"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="e551f-104">Bir Azure Web rolünde SignalR performans sayaçları kullanma</span><span class="sxs-lookup"><span data-stu-id="e551f-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="e551f-105">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e551f-105">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="e551f-106">SignalR performans sayaçları, Azure Web rolünde uygulamanızın performansını izlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e551f-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="e551f-107">Sayaçlar, Microsoft Azure tanılama tarafından yakalanır.</span><span class="sxs-lookup"><span data-stu-id="e551f-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="e551f-108">Azure ile SignalR performans sayaçları yüklediğiniz *signalr.exe*, tek başına veya şirket içi uygulamalar için kullanılan aynı aracı.</span><span class="sxs-lookup"><span data-stu-id="e551f-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="e551f-109">Azure rolleri geçici olduğundan, bir uygulama yüklemek ve SignalR performans sayaçları başlatma kaydetmek için yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="e551f-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e551f-110">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="e551f-110">Prerequisites</span></span>

* <span data-ttu-id="e551f-111">Visual Studio 2015 veya [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span><span class="sxs-lookup"><span data-stu-id="e551f-111">Visual Studio 2015 or [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span></span>
* <span data-ttu-id="e551f-112">[Visual Studio için Microsoft Azure SDK'sı](https://azure.microsoft.com/downloads/) **Not: SDK'yı yükledikten sonra bilgisayarınızı yeniden başlatın.**</span><span class="sxs-lookup"><span data-stu-id="e551f-112">[Microsoft Azure SDK for Visual Studio](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="e551f-113">Microsoft Azure aboneliği: Ücretsiz Azure deneme hesabı için kaydolmak için bkz: [Azure ücretsiz deneme](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="e551f-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="e551f-114">SignalR performans sayaçları sunan bir Azure Web rolü uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="e551f-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="e551f-115">Visual Studio'yu açın.</span><span class="sxs-lookup"><span data-stu-id="e551f-115">Open Visual Studio.</span></span>

2. <span data-ttu-id="e551f-116">Visual Studio'da **dosya** > **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="e551f-116">In Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="e551f-117">İçinde **yeni proje** iletişim kutusunda **Visual C#** > **bulut** solda, kategori seçip **Azure bulut hizmeti** şablonu.</span><span class="sxs-lookup"><span data-stu-id="e551f-117">In the **New Project** dialog box, select the **Visual C#** > **Cloud** category on the left, and then select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="e551f-118">Uygulamayı adlandırma **SignalRPerfCounters** seçip **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="e551f-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![Yeni bulut uygulaması](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)

   > [!NOTE]
   > <span data-ttu-id="e551f-120">Görmüyorsanız **bulut** şablon kategorisi veya **Azure bulut hizmeti** şablonu, yüklemeniz gereken **Azure geliştirme** Visual Studio 2017 için iş yükü.</span><span class="sxs-lookup"><span data-stu-id="e551f-120">If you don't see the **Cloud** template category or the **Azure Cloud Service** template, you need to install the **Azure development** workload for Visual Studio 2017.</span></span> <span data-ttu-id="e551f-121">Seçin **açın, Visual Studio yükleyicisi** bağlantı sol alt tarafında **yeni proje** Visual Studio Yükleyicisi'ni açmak için iletişim.</span><span class="sxs-lookup"><span data-stu-id="e551f-121">Choose the **Open Visual Studio Installer** link on the bottom-left side of the **New Project** dialog to open Visual Studio Installer.</span></span> <span data-ttu-id="e551f-122">Seçin **Azure geliştirme** iş yükü ve ardından **Değiştir** iş yükü yüklemeye başlamak için.</span><span class="sxs-lookup"><span data-stu-id="e551f-122">Select the **Azure development** workload, and then choose **Modify** to start installing the workload.</span></span>
   >
   > ![Visual Studio Yükleyicisi'deki Azure geliştirme iş yükü](using-signalr-performance-counters-in-an-azure-web-role/_static/azure-development-workload.png)

4. <span data-ttu-id="e551f-124">İçinde **yeni Microsoft Azure bulut hizmeti** iletişim kutusunda **ASP.NET Web rolü** seçin > rolü projeye eklemek için düğme.</span><span class="sxs-lookup"><span data-stu-id="e551f-124">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the > button to add the role to the project.</span></span> <span data-ttu-id="e551f-125">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="e551f-125">Select **OK**.</span></span>

   ![ASP.NET Web rolü ekleme](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)

5. <span data-ttu-id="e551f-127">İçinde **yeni ASP.NET Web uygulaması - WebRole1** iletişim kutusunda **MVC** şablonu ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="e551f-127">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![MVC ve Web API ekleme](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)

6. <span data-ttu-id="e551f-129">İçinde **Çözüm Gezgini**açın *diagnostics.wadcfgx* altında dosya **WebRole1**.</span><span class="sxs-lookup"><span data-stu-id="e551f-129">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![Çözüm Gezgini diagnostics.wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)

7. <span data-ttu-id="e551f-131">Dosyanın içeriğini aşağıdaki yapılandırma ile değiştirin ve dosyayı kaydedin:</span><span class="sxs-lookup"><span data-stu-id="e551f-131">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]

8. <span data-ttu-id="e551f-132">Açık **Paket Yöneticisi Konsolu** gelen **Araçları** > **NuGet Paket Yöneticisi**.</span><span class="sxs-lookup"><span data-stu-id="e551f-132">Open the **Package Manager Console** from **Tools** > **NuGet Package Manager**.</span></span> <span data-ttu-id="e551f-133">SignalR ve SignalR yardımcı programları paketin en son sürümünü yüklemek için aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="e551f-133">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]

9. <span data-ttu-id="e551f-134">Başlatıldığında veya geri dönüştüren, rol örneğine SignalR performans sayaçlarını yüklemek üzere uygulamayı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="e551f-134">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="e551f-135">İçinde **Çözüm Gezgini**, sağ **WebRole1** seçin ve proje **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="e551f-135">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="e551f-136">Yeni klasör adı *başlangıç*.</span><span class="sxs-lookup"><span data-stu-id="e551f-136">Name the new folder *Startup*.</span></span>

   ![Başlangıç klasörü Ekle](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)

10. <span data-ttu-id="e551f-138">Kopyalama *signalr.exe* dosyası (eklenen **Microsoft.AspNet.SignalR.Utils** paket) öğesinden \<proje klasörü > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\< Sürüm > / Araçları *başlangıç* önceki adımda oluşturduğunuz klasör.</span><span class="sxs-lookup"><span data-stu-id="e551f-138">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from \<project folder>/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\<version>/tools to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="e551f-139">İçinde **Çözüm Gezgini**, sağ *başlangıç* klasörü ve select **Ekle** > **varolan öğe**.</span><span class="sxs-lookup"><span data-stu-id="e551f-139">In **Solution Explorer**, right-click the *Startup* folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="e551f-140">Görüntülenen iletişim kutusunda, seçmek *signalr.exe* seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="e551f-140">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    ![Signalr.exe projeye Ekle](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)

12. <span data-ttu-id="e551f-142">Sağ *başlangıç* oluşturduğunuz klasör.</span><span class="sxs-lookup"><span data-stu-id="e551f-142">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="e551f-143">Seçin **ekleme** > **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="e551f-143">Select **Add** > **New Item**.</span></span> <span data-ttu-id="e551f-144">Seçin **genel** düğümünü **metin dosyası**ve yeni öğe adı *SignalRPerfCounterInstall.cmd*.</span><span class="sxs-lookup"><span data-stu-id="e551f-144">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="e551f-145">Bu komut dosyasını web rolünde SignalR performans sayaçları yükler.</span><span class="sxs-lookup"><span data-stu-id="e551f-145">This command file will install the SignalR performance counters into the web role.</span></span>

    ![SignalR performans sayacı yükleme toplu iş dosyasını oluşturun](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)

13. <span data-ttu-id="e551f-147">Visual Studio oluşturduğunda *SignalRPerfCounterInstall.cmd* dosyası ana penceresinde otomatik olarak açmak.</span><span class="sxs-lookup"><span data-stu-id="e551f-147">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="e551f-148">Dosyanın içeriğini aşağıdaki betikle değiştirin, sonra kaydedin ve dosyayı kapatın.</span><span class="sxs-lookup"><span data-stu-id="e551f-148">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="e551f-149">Bu betiği yürüten *signalr.exe*, rol örneği için SignalR performans sayaçları ekler.</span><span class="sxs-lookup"><span data-stu-id="e551f-149">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]

14. <span data-ttu-id="e551f-150">Seçin *signalr.exe* dosyası **Çözüm Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="e551f-150">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="e551f-151">Dosyanın içinde **özellikleri**ayarlayın **çıkış dizinine Kopyala** için **her zaman Kopyala**.</span><span class="sxs-lookup"><span data-stu-id="e551f-151">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![Her zaman Kopyala için çıktı dizinine Kopyala'ya ayarlayın](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)

15. <span data-ttu-id="e551f-153">İçin önceki adımı yineleyin *SignalRPerfCounterInstall.cmd* dosya.</span><span class="sxs-lookup"><span data-stu-id="e551f-153">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>


16. <span data-ttu-id="e551f-154">Sağ *SignalRPerfCounterInstall.cmd* seçin ve dosya **birlikte Aç**.</span><span class="sxs-lookup"><span data-stu-id="e551f-154">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="e551f-155">Görüntülenen iletişim kutusunda, seçmek **ikili düzenleyici** seçip **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="e551f-155">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![İkili Düzenleyicisi'ni Aç](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)

17. <span data-ttu-id="e551f-157">İkili Düzenleyici, tüm önde gelen bayt dosyasını seçin ve silebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e551f-157">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="e551f-158">Dosyayı kaydedin ve kapatın.</span><span class="sxs-lookup"><span data-stu-id="e551f-158">Save and close the file.</span></span>

    ![Önde gelen bayt Sil](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)

18. <span data-ttu-id="e551f-160">Açık *ServiceDefinition.csdef* ve yürüten bir başlangıç görevi *SignalrPerfCounterInstall.cmd* dosya hizmeti başladığında:</span><span class="sxs-lookup"><span data-stu-id="e551f-160">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]

19. <span data-ttu-id="e551f-161">Açık `Views/Shared/_Layout.cshtml` ve jQuery paket betik dosyanın sonundan kaldırın.</span><span class="sxs-lookup"><span data-stu-id="e551f-161">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]

20. <span data-ttu-id="e551f-162">Sürekli olarak çağıran bir JavaScript istemcisi ekleme `increment` sunucuda yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e551f-162">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="e551f-163">Açık `Views/Home/Index.cshtml` ve içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="e551f-163">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]

21. <span data-ttu-id="e551f-164">Yeni bir klasör oluşturun **WebRole1** adlı proje *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="e551f-164">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="e551f-165">Sağ *Hubs* klasöründe **Çözüm Gezgini** seçip **Ekle** > **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="e551f-165">Right-click the *Hubs* folder in **Solution Explorer** and select **Add** > **New Item**.</span></span> <span data-ttu-id="e551f-166">İçinde **Yeni Öğe Ekle** iletişim kutusunda **Web** > **SignalR** kategori tıklayın ve ardından **SignalR Hub sınıfı (v2)** öğe şablonu.</span><span class="sxs-lookup"><span data-stu-id="e551f-166">In the **Add New Item** dialog box, select the **Web** > **SignalR** category, and then select the **SignalR Hub Class (v2)** item template.</span></span> <span data-ttu-id="e551f-167">Yeni hub'ı adı *MyHub.cs* seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="e551f-167">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![Yeni Öğe Ekle iletişim kutusu Hubs klasörüne SignalR Hub sınıfı ekleme](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="e551f-169">*MyHub.cs* ana penceresinde otomatik olarak açılır.</span><span class="sxs-lookup"><span data-stu-id="e551f-169">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="e551f-170">İçeriğini aşağıdaki kodla değiştirin. daha sonra dosyayı kaydedip kapatın:</span><span class="sxs-lookup"><span data-stu-id="e551f-170">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]

23. <span data-ttu-id="e551f-171">*[Crank.exe](signalr-connection-density-testing-with-crank.md)*  olan bir bağlantı yoğunluğu testi aracı SignalR kod temeli ile sağlanan test.</span><span class="sxs-lookup"><span data-stu-id="e551f-171">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="e551f-172">Mili kalıcı bir bağlantı gerektirdiğinden, sitenizde kullanmak için bir test ederken ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e551f-172">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="e551f-173">Yeni bir klasör eklemek **WebRole1** adlı proje *PersistentConnections*.</span><span class="sxs-lookup"><span data-stu-id="e551f-173">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="e551f-174">Bu klasörü sağ tıklayıp **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="e551f-174">Right-click this folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e551f-175">Yeni bir sınıf dosyası adı *MyPersistentConnections.cs* seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="e551f-175">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="e551f-176">Visual Studio açılır *MyPersistentConnections.cs* ana penceresinde dosya.</span><span class="sxs-lookup"><span data-stu-id="e551f-176">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="e551f-177">İçeriğini aşağıdaki kodla değiştirin. daha sonra dosyayı kaydedip kapatın:</span><span class="sxs-lookup"><span data-stu-id="e551f-177">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]

25. <span data-ttu-id="e551f-178">Kullanarak `Startup` sınıfı, SignalR nesneleri Başlat OWIN başlatıldığında.</span><span class="sxs-lookup"><span data-stu-id="e551f-178">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="e551f-179">Oluşturun veya açın *Startup.cs* ve içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="e551f-179">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]

    <span data-ttu-id="e551f-180">Yukarıdaki kodda `OwinStartup` özniteliği OWIN başlatmak için bu sınıfın işaretler.</span><span class="sxs-lookup"><span data-stu-id="e551f-180">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="e551f-181">`Configuration` Yöntemi SignalR başlatır.</span><span class="sxs-lookup"><span data-stu-id="e551f-181">The `Configuration` method starts SignalR.</span></span>

26. <span data-ttu-id="e551f-182">Tuşuna basarak uygulamanızı Microsoft Azure öykünücüsü'nde test **F5**.</span><span class="sxs-lookup"><span data-stu-id="e551f-182">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e551f-183">Karşılaşırsanız bir **FileLoadException** adresindeki **MapSignalR**, bağlama yeniden yönlendirmelerini değiştirmek *web.config* aşağıdaki:</span><span class="sxs-lookup"><span data-stu-id="e551f-183">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]

27. <span data-ttu-id="e551f-184">Yaklaşık bir dakika bekleyin.</span><span class="sxs-lookup"><span data-stu-id="e551f-184">Wait about one minute.</span></span> <span data-ttu-id="e551f-185">Visual Studio'da bulut Gezgini araç penceresi açın (**görünümü** > **Cloud Explorer**) yolunu genişletin `(Local)/Storage Accounts/(Development)/Tables`.</span><span class="sxs-lookup"><span data-stu-id="e551f-185">Open the Cloud Explorer tool window in Visual Studio (**View** > **Cloud Explorer**) and expand the path `(Local)/Storage Accounts/(Development)/Tables`.</span></span> <span data-ttu-id="e551f-186">Çift **WADPerformanceCountersTable**.</span><span class="sxs-lookup"><span data-stu-id="e551f-186">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="e551f-187">Tablo verilerini SignalR sayaçları görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="e551f-187">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="e551f-188">Tablo görmüyorsanız, Azure depolama kimlik bilgilerinizi yeniden girmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="e551f-188">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="e551f-189">Seçmeniz gerekebilir **Yenile** tablosunda görmek için düğmeyi **Cloud Explorer** veya **Yenile** tablosundaki verileri görmek için tabloyu Aç penceresinde düğmesine.</span><span class="sxs-lookup"><span data-stu-id="e551f-189">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![Visual Studio Cloud Explorer'da WAD performans sayaçları tabloyu seçme](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![WAD performans sayaçları tablosunda toplanan gösteren sayaçlar](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)

28. <span data-ttu-id="e551f-192">Uygulamanızı bulutta test etmek için güncelleştirme **ServiceConfiguration.Cloud.cscfg** ayarlayın ve dosya `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` için geçerli bir Azure depolama hesabı bağlantı dizesi.</span><span class="sxs-lookup"><span data-stu-id="e551f-192">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="e551f-193">Azure aboneliğinizde uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="e551f-193">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="e551f-194">Bir uygulamayı azure'a dağıtma hakkında daha fazla ayrıntı için bkz. [bir bulut hizmeti oluşturma ve dağıtma konusunda](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="e551f-194">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="e551f-195">Birkaç dakika bekleyin.</span><span class="sxs-lookup"><span data-stu-id="e551f-195">Wait a few minutes.</span></span> <span data-ttu-id="e551f-196">İçinde **Cloud Explorer**, yukarıda yapılandırdığınız depolama hesabını bulun ve bulma `WADPerformanceCountersTable` da tablo.</span><span class="sxs-lookup"><span data-stu-id="e551f-196">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="e551f-197">Tablo verilerini SignalR sayaçları görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="e551f-197">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="e551f-198">Tablo görmüyorsanız, Azure depolama kimlik bilgilerinizi yeniden girmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="e551f-198">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="e551f-199">Seçmeniz gerekebilir **Yenile** tablosunda görmek için düğmeyi **Cloud Explorer** veya **Yenile** tablosundaki verileri görmek için tabloyu Aç penceresinde düğmesine.</span><span class="sxs-lookup"><span data-stu-id="e551f-199">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="e551f-200">Özel performanstan [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) Bu öğreticide kullanılan özgün içerik için.</span><span class="sxs-lookup"><span data-stu-id="e551f-200">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
