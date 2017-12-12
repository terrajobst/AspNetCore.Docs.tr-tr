---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: "SignalR performans sayaçlarını bir Azure Web rolü kullanılarak | Microsoft Docs"
author: guardrex
description: "Nasıl yüklemek ve bir Azure Web rolünde SignalR performans sayaçlarını kullanın."
keywords: "ASP.NET,signalr,Performance sayacı, azure web rolü"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2017
ms.topic: article
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 0d2717eb318d282e21e9aa8622a205f556e3a4ee
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="26df8-104">Bir Azure Web rolünde SignalR performans sayaçlarını kullanma</span><span class="sxs-lookup"><span data-stu-id="26df8-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="26df8-105">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="26df8-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="26df8-106">SignalR performans sayaçlarını, bir Azure Web rolünde, uygulamanızın performansını izlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="26df8-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="26df8-107">Sayaçlar, Microsoft Azure tanılama tarafından yakalanır.</span><span class="sxs-lookup"><span data-stu-id="26df8-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="26df8-108">SignalR performans sayaçlarını ile azure'da yüklediğiniz *signalr.exe*, tek başına veya şirket içi uygulamalar için kullanılan aynı aracı.</span><span class="sxs-lookup"><span data-stu-id="26df8-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="26df8-109">Azure rolleri geçici olduğundan, bir uygulama yüklemek ve başlangıç sırasında SignalR performans sayaçlarını kaydetmek için yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="26df8-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="26df8-110">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="26df8-110">Prerequisites</span></span>

* [<span data-ttu-id="26df8-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="26df8-111">Visual Studio 2015</span></span>](https://www.visualstudio.com/vs/visual-studio-express/)
* <span data-ttu-id="26df8-112">[Visual Studio 2015 (VS2015) için Microsoft Azure SDK'sı](https://azure.microsoft.com/downloads/) **Not: SDK'yı yükledikten sonra bilgisayarınızı yeniden başlatın.**</span><span class="sxs-lookup"><span data-stu-id="26df8-112">[Microsoft Azure SDK for Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="26df8-113">Microsoft Azure aboneliği: ücretsiz Azure deneme hesabı için kaydolmak için bkz: [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="26df8-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="26df8-114">SignalR performans sayaçlarını kullanıma sunan bir Azure Web rolü uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="26df8-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="26df8-115">Visual Studio 2015'i açın.</span><span class="sxs-lookup"><span data-stu-id="26df8-115">Open Visual Studio 2015.</span></span>

2. <span data-ttu-id="26df8-116">Visual Studio 2015'te seçin **dosya &gt; yeni &gt; proje**.</span><span class="sxs-lookup"><span data-stu-id="26df8-116">In Visual Studio 2015, select **File &gt; New &gt; Project**.</span></span>

3. <span data-ttu-id="26df8-117">İçinde **şablonları** bölmesinde **yeni proje** penceresinde altında **Visual C#** düğümü, select **bulut** düğümü ve select **Azure bulut hizmeti** şablonu.</span><span class="sxs-lookup"><span data-stu-id="26df8-117">In the **Templates** pane of the **New Project** window under the **Visual C#** node, select the **Cloud** node and select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="26df8-118">Uygulama adı **SignalRPerfCounters** seçip **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="26df8-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![Yeni bulut uygulaması](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. <span data-ttu-id="26df8-120">İçinde **yeni Microsoft Azure bulut hizmeti** iletişim kutusunda **ASP.NET Web rolü** seçip  **&gt;**  projeye rolü eklemek için düğmesi.</span><span class="sxs-lookup"><span data-stu-id="26df8-120">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the **&gt;** button to add the role to the project.</span></span> <span data-ttu-id="26df8-121">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="26df8-121">Select **OK**.</span></span>

   ![ASP.NET Web rolü ekleme](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. <span data-ttu-id="26df8-123">İçinde **yeni ASP.NET Web uygulaması - WebRole1** iletişim kutusunda **MVC** şablonu ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="26df8-123">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![MVC ve Web API ekleme](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. <span data-ttu-id="26df8-125">İçinde **Çözüm Gezgini**, açık *diagnostics.wadcfgx* altında dosya **WebRole1**.</span><span class="sxs-lookup"><span data-stu-id="26df8-125">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![Çözüm Gezgini diagnostics.wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. <span data-ttu-id="26df8-127">Aşağıdaki yapılandırmaya sahip dosyasının içeriğini değiştirin ve dosyayı kaydedin:</span><span class="sxs-lookup"><span data-stu-id="26df8-127">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. <span data-ttu-id="26df8-128">Açık **Paket Yöneticisi Konsolu** gelen **Araçları &gt; NuGet Paket Yöneticisi**.</span><span class="sxs-lookup"><span data-stu-id="26df8-128">Open the **Package Manager Console** from **Tools &gt; NuGet Package Manager**.</span></span> <span data-ttu-id="26df8-129">SignalR ve SignalR yardımcı programları paketi en son sürümünü yüklemek için aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="26df8-129">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. <span data-ttu-id="26df8-130">Uygulama başlatıldığında veya geri dönüştürüldüğünde SignalR performans sayaçlarını rolü örneği yüklemek için yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="26df8-130">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="26df8-131">İçinde **Çözüm Gezgini**, sağ tıklayın **WebRole1** proje ve seçin **Ekle &gt; yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="26df8-131">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add &gt; New Folder**.</span></span> <span data-ttu-id="26df8-132">Yeni bir klasör adı *başlangıç*.</span><span class="sxs-lookup"><span data-stu-id="26df8-132">Name the new folder *Startup*.</span></span>

   ![Başlangıç klasörü Ekle](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. <span data-ttu-id="26df8-134">Kopya *signalr.exe* dosyası (eklendi **Microsoft.AspNet.SignalR.Utils** paket) gelen  **&lt;proje klasörünü&gt;\SignalRPerfCounters\packages\ Microsoft.AspNet.SignalR.Utils. &lt;sürüm&gt;\tools** için *başlangıç* önceki adımda oluşturduğunuz klasör.</span><span class="sxs-lookup"><span data-stu-id="26df8-134">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from **&lt;project folder&gt;\SignalRPerfCounters\packages\Microsoft.AspNet.SignalR.Utils.&lt;version&gt;\tools** to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="26df8-135">İçinde **Çözüm Gezgini**, sağ tıklatın *başlangıç* klasörü ve select **Ekle &gt; varolan öğeyi**.</span><span class="sxs-lookup"><span data-stu-id="26df8-135">In **Solution Explorer**, right-click the *Startup* folder and select **Add &gt; Existing Item**.</span></span> <span data-ttu-id="26df8-136">Görüntülenen iletişim kutusunda, seçin *signalr.exe* seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="26df8-136">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    ![Signalr.exe projeye ekleyin](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. <span data-ttu-id="26df8-138">Sağ *başlangıç* oluşturduğunuz klasör.</span><span class="sxs-lookup"><span data-stu-id="26df8-138">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="26df8-139">Seçin **ekleme &gt; yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="26df8-139">Select **Add &gt; New Item**.</span></span> <span data-ttu-id="26df8-140">Seçin **genel** düğümü, select **metin dosyası**ve yeni öğe adını *SignalRPerfCounterInstall.cmd*.</span><span class="sxs-lookup"><span data-stu-id="26df8-140">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="26df8-141">Bu komut dosyasını web rolünde SignalR performans sayaçlarını yükler.</span><span class="sxs-lookup"><span data-stu-id="26df8-141">This command file will install the SignalR performance counters into the web role.</span></span>

    ![SignalR performans sayacı yükleme toplu iş dosyası oluşturma](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. <span data-ttu-id="26df8-143">Visual Studio oluşturduğunda *SignalRPerfCounterInstall.cmd* dosyası, ana penceresinde otomatik olarak açmak.</span><span class="sxs-lookup"><span data-stu-id="26df8-143">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="26df8-144">Aşağıdaki komut dosyası ile dosyasının içeriğini değiştirin sonra dosyasını kaydedin ve kapatın.</span><span class="sxs-lookup"><span data-stu-id="26df8-144">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="26df8-145">Bu betiği yürüten *signalr.exe*, rol örneği için SignalR performans sayaçlarını ekler.</span><span class="sxs-lookup"><span data-stu-id="26df8-145">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. <span data-ttu-id="26df8-146">Seçin *signalr.exe* dosyasını **Çözüm Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="26df8-146">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="26df8-147">Dosyanın içinde **özellikleri**ayarlayın **çıktı dizinine Kopyala** için **her zaman Kopyala**.</span><span class="sxs-lookup"><span data-stu-id="26df8-147">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![Her zaman kopyalamak için çıktı dizinine Kopyala ayarlayın](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. <span data-ttu-id="26df8-149">İçin önceki adımı yineleyin *SignalRPerfCounterInstall.cmd* dosya.</span><span class="sxs-lookup"><span data-stu-id="26df8-149">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>

    
16. <span data-ttu-id="26df8-150">Sağ *SignalRPerfCounterInstall.cmd* dosya ve seçin **birlikte Aç**.</span><span class="sxs-lookup"><span data-stu-id="26df8-150">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="26df8-151">Görüntülenen iletişim kutusunda, seçin **İkili Düzenleyicisi** seçip **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="26df8-151">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![İkili Düzenleyicisi ile Aç](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. <span data-ttu-id="26df8-153">İkili Düzenleyicisi'nde dosyasındaki tüm önde gelen bayt seçmek ve silebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="26df8-153">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="26df8-154">Dosyayı kaydedin ve kapatın.</span><span class="sxs-lookup"><span data-stu-id="26df8-154">Save and close the file.</span></span>

    ![Önde gelen bayt Sil](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. <span data-ttu-id="26df8-156">Açık *ServiceDefinition.csdef* ve yürüten bir başlangıç görevi *SignalrPerfCounterInstall.cmd* dosya hizmeti başladığında:</span><span class="sxs-lookup"><span data-stu-id="26df8-156">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. <span data-ttu-id="26df8-157">Açık `Views/Shared/_Layout.cshtml` ve jQuery paket komut dosyası sonundan kaldırın.</span><span class="sxs-lookup"><span data-stu-id="26df8-157">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. <span data-ttu-id="26df8-158">Sürekli olarak çağıran bir JavaScript istemcisi ekleme `increment` sunucuda yöntemi.</span><span class="sxs-lookup"><span data-stu-id="26df8-158">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="26df8-159">Açık `Views/Home/Index.cshtml` ve içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="26df8-159">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. <span data-ttu-id="26df8-160">Yeni bir klasör oluşturun **WebRole1** adlı projesi *hub*.</span><span class="sxs-lookup"><span data-stu-id="26df8-160">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="26df8-161">Sağ *hub* klasöründe **Çözüm Gezgini**seçin **Web &gt; SignalR**seçip **SignalR hub'ı sınıfı (v2)**.</span><span class="sxs-lookup"><span data-stu-id="26df8-161">Right-click the *Hubs* folder in **Solution Explorer**, select **Web &gt; SignalR**, and select **SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="26df8-162">Yeni hub'ı adı *MyHub.cs* seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="26df8-162">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![Yeni Öğe Ekle iletişim kutusu hub klasöründe SignalR hub'ı sınıfı ekleme](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="26df8-164">*MyHub.cs* ana penceresinde otomatik olarak açılır.</span><span class="sxs-lookup"><span data-stu-id="26df8-164">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="26df8-165">İçeriğini aşağıdaki kodla değiştirin sonra dosyayı kaydedip kapatın:</span><span class="sxs-lookup"><span data-stu-id="26df8-165">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. <span data-ttu-id="26df8-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)*  olan SignalR codebase ile sağlanan aracını test bağlantısı yoğunluğu.</span><span class="sxs-lookup"><span data-stu-id="26df8-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="26df8-167">Mili kalıcı bir bağlantı gerektirdiğinden, sitenize kullanmak için bir tane sınarken ekleyin.</span><span class="sxs-lookup"><span data-stu-id="26df8-167">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="26df8-168">Yeni bir klasör ekleyin **WebRole1** adlı proje *PersistentConnections*.</span><span class="sxs-lookup"><span data-stu-id="26df8-168">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="26df8-169">Bu klasörü sağ tıklatın ve seçin **Ekle &gt; sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="26df8-169">Right-click this folder and select **Add &gt; Class**.</span></span> <span data-ttu-id="26df8-170">Yeni sınıf dosya adı *MyPersistentConnections.cs* seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="26df8-170">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="26df8-171">Visual Studio açılır *MyPersistentConnections.cs* ana penceresinde dosya.</span><span class="sxs-lookup"><span data-stu-id="26df8-171">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="26df8-172">İçeriğini aşağıdaki kodla değiştirin sonra dosyayı kaydedip kapatın:</span><span class="sxs-lookup"><span data-stu-id="26df8-172">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. <span data-ttu-id="26df8-173">Kullanarak `Startup` sınıfı, SignalR nesneleri Başlat OWIN başladığında.</span><span class="sxs-lookup"><span data-stu-id="26df8-173">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="26df8-174">Açmaya veya oluşturmaya *haline* ve içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="26df8-174">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    <span data-ttu-id="26df8-175">Yukarıdaki kod `OwinStartup` özniteliği OWIN başlatmak için bu sınıf işaretler.</span><span class="sxs-lookup"><span data-stu-id="26df8-175">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="26df8-176">`Configuration` Yöntemi SignalR başlatır.</span><span class="sxs-lookup"><span data-stu-id="26df8-176">The `Configuration` method starts SignalR.</span></span>
    
26. <span data-ttu-id="26df8-177">Tuşlarına basarak uygulamanızı Microsoft Azure öykünücüsünde test **F5**.</span><span class="sxs-lookup"><span data-stu-id="26df8-177">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="26df8-178">Karşılaşırsanız bir **FileLoadException** adresindeki **MapSignalR**, bağlama yeniden yönlendirmeleri içinde değiştirmek *web.config* şu:</span><span class="sxs-lookup"><span data-stu-id="26df8-178">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. <span data-ttu-id="26df8-179">Yaklaşık bir dakika bekleyin.</span><span class="sxs-lookup"><span data-stu-id="26df8-179">Wait about one minute.</span></span> <span data-ttu-id="26df8-180">Visual Studio'da Cloud Explorer araç penceresi açın (**Görünüm &gt; Cloud Explorer**) ve yolun genişletin `(Local)\Storage Accounts\(Development)\Tables`.</span><span class="sxs-lookup"><span data-stu-id="26df8-180">Open the Cloud Explorer tool window in Visual Studio (**View &gt; Cloud Explorer**) and expand the path `(Local)\Storage Accounts\(Development)\Tables`.</span></span> <span data-ttu-id="26df8-181">Çift **WADPerformanceCountersTable**.</span><span class="sxs-lookup"><span data-stu-id="26df8-181">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="26df8-182">Tablo verisi SignalR sayaçları görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="26df8-182">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="26df8-183">Tablo görmüyorsanız, Azure depolama kimlik bilgilerinizi yeniden girmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="26df8-183">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="26df8-184">Seçmeniz gerekebilir **yenileme** tablosunda görmek için düğmesi **Cloud Explorer** veya seçin **yenileme** tablosundaki verileri görmek için tabloyu Aç penceresinde düğmesini.</span><span class="sxs-lookup"><span data-stu-id="26df8-184">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![Visual Studio Cloud Explorer'da WAD performans sayaçları tablo seçme](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![WAD performans sayaçları tabloda toplanan sayaçları gösterme](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. <span data-ttu-id="26df8-187">Bulutta uygulamanızı test etmek için Güncelleştir **ServiceConfiguration.Cloud.cscfg** dosya ve ayarlayın `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` geçerli bir Azure depolama hesabı bağlantı dizesi.</span><span class="sxs-lookup"><span data-stu-id="26df8-187">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="26df8-188">Azure aboneliğiniz uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="26df8-188">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="26df8-189">Azure için bir uygulamanın nasıl dağıtılacağı hakkında daha fazla bilgi için bkz: [nasıl oluşturulacağı ve bir bulut hizmetinin dağıtılacağı](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="26df8-189">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="26df8-190">Birkaç dakika bekleyin.</span><span class="sxs-lookup"><span data-stu-id="26df8-190">Wait a few minutes.</span></span> <span data-ttu-id="26df8-191">İçinde **Cloud Explorer**, yukarıda yapılandırılan depolama hesabını bulun ve bulmak `WADPerformanceCountersTable` bu tabloda.</span><span class="sxs-lookup"><span data-stu-id="26df8-191">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="26df8-192">Tablo verisi SignalR sayaçları görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="26df8-192">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="26df8-193">Tablo görmüyorsanız, Azure depolama kimlik bilgilerinizi yeniden girmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="26df8-193">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="26df8-194">Seçmeniz gerekebilir **yenileme** tablosunda görmek için düğmesi **Cloud Explorer** veya seçin **yenileme** tablosundaki verileri görmek için tabloyu Aç penceresinde düğmesini.</span><span class="sxs-lookup"><span data-stu-id="26df8-194">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="26df8-195">Özel teşekkürler [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) Bu öğreticide kullanılan özgün içerik için.</span><span class="sxs-lookup"><span data-stu-id="26df8-195">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
