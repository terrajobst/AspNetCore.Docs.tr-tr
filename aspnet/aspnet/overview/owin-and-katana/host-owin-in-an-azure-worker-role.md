---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: "Konağı Azure çalışan rolünde OWIN | Microsoft Docs"
author: MikeWasson
description: "Bu öğretici, bir Microsoft Azure çalışan rolünde kendini OWIN barındırma gösterilmektedir. Açık Web arabirimi için .NET (OWIN) .NET web sunucusu arasında bir Özet tanımlar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/11/2014
ms.topic: article
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 8c0fdfdf60ff3bde34b6869adf3f8693b4d9615d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="host-owin-in-an-azure-worker-role"></a><span data-ttu-id="f8c32-104">Konak bir Azure çalışan rolünde OWIN</span><span class="sxs-lookup"><span data-stu-id="f8c32-104">Host OWIN in an Azure Worker Role</span></span>
====================
<span data-ttu-id="f8c32-105">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f8c32-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="f8c32-106">Bu öğretici, bir Microsoft Azure çalışan rolünde kendini OWIN barındırma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="f8c32-106">This tutorial shows how to self-host OWIN in a Microsoft Azure worker role.</span></span>
> 
> <span data-ttu-id="f8c32-107">[.NET için Web Arabirimi'ni açmak](http://owin.org/) (OWIN) .NET web sunucuları ve web uygulamaları arasındaki bir Özet tanımlar.</span><span class="sxs-lookup"><span data-stu-id="f8c32-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="f8c32-108">OWIN ayrıştırır OWIN IIS dışında kendi işleminde, bir web uygulaması kendi kendine barındırma için ideal hale getirir sunucunun web uygulamasından – Örneğin, Azure çalışan rolüne içinde.</span><span class="sxs-lookup"><span data-stu-id="f8c32-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
> 
> <span data-ttu-id="f8c32-109">Bu öğreticide, bir Microsoft Azure çalışan rolü OWIN uygulamalarında kendini barındırma öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="f8c32-109">In this tutorial, you'll learn how to self-host an OWIN applications inside a Microsoft Azure worker role.</span></span> <span data-ttu-id="f8c32-110">Çalışan rolleri hakkında daha fazla bilgi için bkz: [Azure yürütme modelleri](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span><span class="sxs-lookup"><span data-stu-id="f8c32-110">To learn more about worker roles, see [Azure Execution Models](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f8c32-111">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="f8c32-111">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="f8c32-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f8c32-112">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [<span data-ttu-id="f8c32-113">2.3 .NET için Azure SDK</span><span class="sxs-lookup"><span data-stu-id="f8c32-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)
> - [<span data-ttu-id="f8c32-114">Microsoft.Owin.Selfhost 2.1.0</span><span class="sxs-lookup"><span data-stu-id="f8c32-114">Microsoft.Owin.Selfhost 2.1.0</span></span>](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="f8c32-115">Bir Microsoft Azure projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="f8c32-115">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="f8c32-116">Visual Studio'yu yönetici ayrıcalıklarıyla çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f8c32-116">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="f8c32-117">Azure işlem öykünücüsü kullanarak uygulama yerel olarak hata ayıklama için yönetici ayrıcalıkları gerekir.</span><span class="sxs-lookup"><span data-stu-id="f8c32-117">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="f8c32-118">Üzerinde **dosya** menüsünde tıklatın **yeni**, ardından **proje**.</span><span class="sxs-lookup"><span data-stu-id="f8c32-118">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="f8c32-119">Gelen **yüklü şablonlar**, Visual C# altında tıklatın **bulut** ve ardından **Windows Azure bulut hizmeti**.</span><span class="sxs-lookup"><span data-stu-id="f8c32-119">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="f8c32-120">"AzureApp" adını verin ve projeyi tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="f8c32-120">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="f8c32-121">İçinde **yeni Windows Azure bulut hizmeti** iletişim kutusunda, çift **çalışan rolü**.</span><span class="sxs-lookup"><span data-stu-id="f8c32-121">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="f8c32-122">Varsayılan adı ("WorkerRole1") bırakın.</span><span class="sxs-lookup"><span data-stu-id="f8c32-122">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="f8c32-123">Bu adım, bir çalışan rolü çözüme ekler.</span><span class="sxs-lookup"><span data-stu-id="f8c32-123">This step adds a worker role to the solution.</span></span> <span data-ttu-id="f8c32-124">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="f8c32-124">Click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="f8c32-125">Oluşturulan Visual Studio çözümü iki proje içerir:</span><span class="sxs-lookup"><span data-stu-id="f8c32-125">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="f8c32-126">&quot;AzureApp&quot; Azure uygulaması için yapılandırma ve rolleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="f8c32-126">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="f8c32-127">&quot;WorkerRole1&quot; çalışan rolü kodunu içerir.</span><span class="sxs-lookup"><span data-stu-id="f8c32-127">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="f8c32-128">Genel olarak, tek bir rol kullansa da Bu öğretici bir Azure uygulama birden çok rol içerebilir.</span><span class="sxs-lookup"><span data-stu-id="f8c32-128">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a><span data-ttu-id="f8c32-129">Kendini barındırma OWIN paketleri ekleme</span><span class="sxs-lookup"><span data-stu-id="f8c32-129">Add the OWIN Self-Host Packages</span></span>

<span data-ttu-id="f8c32-130">Gelen **Araçları** menüsünde tıklatın **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="f8c32-130">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="f8c32-131">Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="f8c32-131">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="f8c32-132">Bir HTTP uç nokta ekleyin</span><span class="sxs-lookup"><span data-stu-id="f8c32-132">Add an HTTP Endpoint</span></span>

<span data-ttu-id="f8c32-133">Çözüm Gezgini'nde AzureApp projenizi genişletin.</span><span class="sxs-lookup"><span data-stu-id="f8c32-133">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="f8c32-134">Rolleri düğümünü genişletin, WorkerRole1 sağ tıklatın ve seçin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="f8c32-134">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="f8c32-135">Tıklatın **uç noktaları**ve ardından **uç nokta Ekle**.</span><span class="sxs-lookup"><span data-stu-id="f8c32-135">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="f8c32-136">İçinde **Protokolü** açılır listeden seçin "http".</span><span class="sxs-lookup"><span data-stu-id="f8c32-136">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="f8c32-137">İçinde **genel bağlantı noktası** ve **özel bağlantı noktası**, 80 yazın.</span><span class="sxs-lookup"><span data-stu-id="f8c32-137">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="f8c32-138">Bu bağlantı noktası numaralarını farklı olabilir.</span><span class="sxs-lookup"><span data-stu-id="f8c32-138">These port numbers can be different.</span></span> <span data-ttu-id="f8c32-139">Role bir isteği gönderdiğinizde hangi istemcilerin kullandığı genel bağlantı noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="f8c32-139">The public port is what clients use when they send a request to the role.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a><span data-ttu-id="f8c32-140">OWIN başlangıç sınıfı oluşturma</span><span class="sxs-lookup"><span data-stu-id="f8c32-140">Create the OWIN Startup Class</span></span>

<span data-ttu-id="f8c32-141">Çözüm Gezgini'nde, WorkerRole1 projeye sağ tıklayın ve seçin **Ekle** / **sınıfı** yeni bir sınıf eklemek için.</span><span class="sxs-lookup"><span data-stu-id="f8c32-141">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="f8c32-142">Sınıf adını `Startup`.</span><span class="sxs-lookup"><span data-stu-id="f8c32-142">Name the class `Startup`.</span></span>

<span data-ttu-id="f8c32-143">Tüm Demirbaş kod aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="f8c32-143">Replace all of the boilerplate code with the following:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

<span data-ttu-id="f8c32-144">`UseWelcomePage` Genişletme yöntemi, uygulamanızı site çalıştığını doğrulamak için basit bir HTML sayfası ekler.</span><span class="sxs-lookup"><span data-stu-id="f8c32-144">The `UseWelcomePage` extension method adds a simple HTML page to your application, to verify the site is working.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="f8c32-145">OWIN konak Başlat</span><span class="sxs-lookup"><span data-stu-id="f8c32-145">Start the OWIN Host</span></span>

<span data-ttu-id="f8c32-146">WorkerRole.cs dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="f8c32-146">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="f8c32-147">Bu sınıf, çalışan rolü başlatıldığında ve durdurulduğunda, çalışan bir kod tanımlar.</span><span class="sxs-lookup"><span data-stu-id="f8c32-147">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="f8c32-148">Aşağıdaki ekleme deyimini kullanarak:</span><span class="sxs-lookup"><span data-stu-id="f8c32-148">Add the following using statement:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="f8c32-149">Ekleme bir **IDisposable** üyesine `WorkerRole` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="f8c32-149">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="f8c32-150">İçinde `OnStart` yöntemi, konağı başlatmak için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="f8c32-150">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

<span data-ttu-id="f8c32-151">**WebApp.Start** yöntemi OWIN ana bilgisayarı başlatır.</span><span class="sxs-lookup"><span data-stu-id="f8c32-151">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="f8c32-152">Adını `Startup` yöntemi için bir tür parametresi bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="f8c32-152">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="f8c32-153">Kurala göre konak çağıracak `Configure` bu sınıfın yöntemi.</span><span class="sxs-lookup"><span data-stu-id="f8c32-153">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="f8c32-154">Geçersiz kılma `OnStop` elden için  *\_uygulama* örneği:</span><span class="sxs-lookup"><span data-stu-id="f8c32-154">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

<span data-ttu-id="f8c32-155">WorkerRole.cs için tam kod aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="f8c32-155">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="f8c32-156">Çözümü derlemek ve Azure işlem öykünücüsü uygulamayı yerel olarak çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="f8c32-156">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="f8c32-157">Güvenlik Duvarı ayarlarınızı bağlı olarak, güvenlik duvarı üzerinden öykünücüsü izin gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="f8c32-157">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

<span data-ttu-id="f8c32-158">İşlem öykünücüsü uç noktası için bir yerel IP adresi atar.</span><span class="sxs-lookup"><span data-stu-id="f8c32-158">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="f8c32-159">IP adresi işlem öykünücüsü kullanıcı Arabiriminde görüntüleyerek bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f8c32-159">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="f8c32-160">Görev çubuğu bildirim alanında bulunan öykünücü simgesine sağ tıklatın ve seçin **Göster işlem öykünücüsü kullanıcı Arabiriminde**.</span><span class="sxs-lookup"><span data-stu-id="f8c32-160">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="f8c32-161">IP adresi hizmet dağıtımları, dağıtım [kimlik], hizmet ayrıntıları altında bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f8c32-161">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="f8c32-162">Bir web tarayıcısı açın ve http:// gidin*adresi*, burada *adresi* işlem öykünücüsü tarafından; atanan IP adresi gibi `http://127.0.0.1:80`.</span><span class="sxs-lookup"><span data-stu-id="f8c32-162">Open a web browser and navigate to http://*address*, where *address* is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80`.</span></span> <span data-ttu-id="f8c32-163">OWIN Karşılama sayfasını görmeniz gerekir:</span><span class="sxs-lookup"><span data-stu-id="f8c32-163">You should see the OWIN welcome page:</span></span>

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="f8c32-164">Azure'a dağıtma</span><span class="sxs-lookup"><span data-stu-id="f8c32-164">Deploy to Azure</span></span>

<span data-ttu-id="f8c32-165">Bu adım için bir Azure hesabınızın olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="f8c32-165">For this step, you must have an Azure account.</span></span> <span data-ttu-id="f8c32-166">Zaten yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f8c32-166">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="f8c32-167">Ayrıntılar için bkz [Microsoft Azure ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="f8c32-167">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="f8c32-168">Çözüm Gezgini'nde AzureApp projesine sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="f8c32-168">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="f8c32-169">Seçin **yayımlama**.</span><span class="sxs-lookup"><span data-stu-id="f8c32-169">Select **Publish**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image12.png)

<span data-ttu-id="f8c32-170">Azure hesabınızda açmadınız tıklatmak **oturum**.</span><span class="sxs-lookup"><span data-stu-id="f8c32-170">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="f8c32-171">Oturumunuz açıldıktan sonra bir abonelik seçin ve'ı tıklatın **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="f8c32-171">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

<span data-ttu-id="f8c32-172">Bulut hizmeti için bir ad girin ve bir bölge seçin.</span><span class="sxs-lookup"><span data-stu-id="f8c32-172">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="f8c32-173">**Oluştur**'u tıklatın.</span><span class="sxs-lookup"><span data-stu-id="f8c32-173">Click **Create**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image17.png)

<span data-ttu-id="f8c32-174">Tıklatın **yayımlama**.</span><span class="sxs-lookup"><span data-stu-id="f8c32-174">Click **Publish**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="f8c32-175">Azure etkinlik günlüğü penceresi dağıtımının ilerlemesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="f8c32-175">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="f8c32-176">Uygulama dağıtıldığında, Gözat `http://appname.cloudapp.net/`, burada *appname* bulut hizmetiniz adıdır.</span><span class="sxs-lookup"><span data-stu-id="f8c32-176">When the app is deployed, browse to `http://appname.cloudapp.net/`, where *appname* is the name of your cloud service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f8c32-177">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="f8c32-177">Additional Resources</span></span>

- [<span data-ttu-id="f8c32-178">Project Katana’ya Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="f8c32-178">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)
- [<span data-ttu-id="f8c32-179">Github'da Katana proje</span><span class="sxs-lookup"><span data-stu-id="f8c32-179">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana/)
