---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: Azure Service Bus ile SignalR ölçeğini genişletme | Microsoft Docs
author: bradygaster
description: Yazılım sürümleri bu konu Visual Studio 2013 .NET 4.5 SignalR sürümünde 2 önceki sürümleri bu konu başlığı altında bu konu için SignalR 1.x sürümünde kullanılan...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 9f6188ff5f716c20d759f73975d6a8ad522834d8
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836824"
---
<a name="signalr-scaleout-with-azure-service-bus"></a><span data-ttu-id="ed50d-103">Azure Service Bus ile SignalR Ölçeğini Genişletme</span><span class="sxs-lookup"><span data-stu-id="ed50d-103">SignalR Scaleout with Azure Service Bus</span></span>
====================
<span data-ttu-id="ed50d-104">tarafından [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="ed50d-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="ed50d-105">Bu öğreticide, bir Windows Azure Web her rol örneği iletilerini dağıtmak için Service Bus devre kartına kullanarak rol bir SignalR uygulamayı dağıtacaksınız.</span><span class="sxs-lookup"><span data-stu-id="ed50d-105">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span> <span data-ttu-id="ed50d-106">(Service Bus devre kartı ile de kullanabilirsiniz [web uygulamaları Azure App Service'te](https://docs.microsoft.com/azure/app-service-web/).)</span><span class="sxs-lookup"><span data-stu-id="ed50d-106">(You can also use the Service Bus backplane with [web apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="ed50d-107">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="ed50d-107">Prerequisites:</span></span>

- <span data-ttu-id="ed50d-108">Bir Windows Azure hesabı.</span><span class="sxs-lookup"><span data-stu-id="ed50d-108">A Windows Azure account.</span></span>
- <span data-ttu-id="ed50d-109">[Windows Azure SDK'sı](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="ed50d-109">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="ed50d-110">Visual Studio 2012 veya 2013.</span><span class="sxs-lookup"><span data-stu-id="ed50d-110">Visual Studio 2012 or 2013.</span></span>

<span data-ttu-id="ed50d-111">Service bus devre kartı da uyumlu [Windows Server için hizmet veri yolu](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), sürüm 1.1.</span><span class="sxs-lookup"><span data-stu-id="ed50d-111">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="ed50d-112">Ancak, Windows Server için hizmet veri yolu 1.0 sürümü ile uyumlu değil.</span><span class="sxs-lookup"><span data-stu-id="ed50d-112">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="ed50d-113">Fiyatlandırma</span><span class="sxs-lookup"><span data-stu-id="ed50d-113">Pricing</span></span>

<span data-ttu-id="ed50d-114">Service Bus devre kartına ileti göndermek için konulara kullanır.</span><span class="sxs-lookup"><span data-stu-id="ed50d-114">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="ed50d-115">En son fiyatlandırma bilgileri için bkz. [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="ed50d-115">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="ed50d-116">Bu makalenin yazıldığı sırada, kısa $1 için ayda 1.000.000 iletileri gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="ed50d-116">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="ed50d-117">Devre kartına bir SignalR hub'yöntemini her çalıştırılışı için hizmet veri yolu ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="ed50d-117">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="ed50d-118">Bağlantı, bağlantı kesilmesi, katılma veya bırakma grupları ve benzeri için bazı denetim iletileri de vardır.</span><span class="sxs-lookup"><span data-stu-id="ed50d-118">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="ed50d-119">Çoğu uygulamada ileti trafiği çoğunu hub yöntemi çağrılarına olacaktır.</span><span class="sxs-lookup"><span data-stu-id="ed50d-119">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="ed50d-120">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="ed50d-120">Overview</span></span>

<span data-ttu-id="ed50d-121">Ayrıntılı Öğreticisine aldığımız önce ne yapacağını, hızlı bir genel bakış aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ed50d-121">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="ed50d-122">Yeni bir Service Bus ad alanı oluşturmak için Windows Azure portal'ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="ed50d-122">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="ed50d-123">Bu NuGet paketlerini uygulamanıza ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ed50d-123">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="ed50d-124">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="ed50d-124">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - <span data-ttu-id="ed50d-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) veya [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span><span class="sxs-lookup"><span data-stu-id="ed50d-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) or [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span></span>
3. <span data-ttu-id="ed50d-126">Bir SignalR uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ed50d-126">Create a SignalR application.</span></span>
4. <span data-ttu-id="ed50d-127">Devre kartına yapılandırmak için Startup.cs için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ed50d-127">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="ed50d-128">Bu kod için varsayılan değerlerle devre kartına yapılandırır [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) ve [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="ed50d-128">This code configures the backplane with the default values for [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="ed50d-129">Bu değerleri değiştirme hakkında daha fazla bilgi için bkz: [SignalR performansı: Genişletme ölçümleri](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="ed50d-129">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

<span data-ttu-id="ed50d-130">Her uygulama için "Uygulamanızınadı" için farklı bir değer seçin.</span><span class="sxs-lookup"><span data-stu-id="ed50d-130">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="ed50d-131">Aynı değeri, birden çok uygulamada kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="ed50d-131">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="ed50d-132">Azure hizmetleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="ed50d-132">Create the Azure Services</span></span>

<span data-ttu-id="ed50d-133">Bölümünde anlatıldığı gibi bir bulut hizmeti oluşturma [bir bulut hizmeti oluşturma ve dağıtma konusunda](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="ed50d-133">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="ed50d-134">Bölümündeki adımları "nasıl yapılır: Hızlı oluşturma yöntemini kullanarak bir bulut hizmeti oluşturma".</span><span class="sxs-lookup"><span data-stu-id="ed50d-134">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="ed50d-135">Bu öğreticide, bir sertifikayı karşıya yüklemek gerekmez.</span><span class="sxs-lookup"><span data-stu-id="ed50d-135">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="ed50d-136">Açıklanan şekilde yeni bir Service Bus ad alanı oluşturma [nasıl kullanım hizmet veri yolu konuları/abonelikleri için](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="ed50d-136">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="ed50d-137">"Hizmet Namespace oluşturma" bölümündeki adımları izleyin.</span><span class="sxs-lookup"><span data-stu-id="ed50d-137">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="ed50d-138">Bulut hizmeti ve Service Bus ad alanı için aynı bölge seçtiğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="ed50d-138">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="ed50d-139">Visual Studio projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="ed50d-139">Create the Visual Studio Project</span></span>

<span data-ttu-id="ed50d-140">Visual Studio’yu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ed50d-140">Start Visual Studio.</span></span> <span data-ttu-id="ed50d-141">Gelen **dosya** menüsünü tıklatın **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="ed50d-141">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="ed50d-142">İçinde **yeni proje** iletişim kutusunda **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="ed50d-142">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="ed50d-143">Altında **yüklü şablonlar**seçin **bulut** seçip **Windows Azure bulut hizmeti**.</span><span class="sxs-lookup"><span data-stu-id="ed50d-143">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="ed50d-144">Varsayılan .NET Framework 4.5 tutun.</span><span class="sxs-lookup"><span data-stu-id="ed50d-144">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="ed50d-145">ChatService uygulamaya bir ad ve tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="ed50d-145">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="ed50d-146">İçinde **yeni Windows Azure bulut hizmeti** iletişim kutusunda, ASP.NET Web rolü seçin.</span><span class="sxs-lookup"><span data-stu-id="ed50d-146">In the **New Windows Azure Cloud Service** dialog, select ASP.NET Web Role.</span></span> <span data-ttu-id="ed50d-147">Sağ ok düğmesine tıklayın (**&gt;**) rolünü çözümünüze ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ed50d-147">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="ed50d-148">Fareyi yeni rol, bu nedenle gelin Kurşun Kalem simgesi görünür.</span><span class="sxs-lookup"><span data-stu-id="ed50d-148">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="ed50d-149">Rolü yeniden adlandırmak için bu simgeye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="ed50d-149">Click this icon to rename the role.</span></span> <span data-ttu-id="ed50d-150">Rol "SignalRChat" adını verin ve tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="ed50d-150">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="ed50d-151">İçinde **yeni ASP.NET projesi** iletişim kutusunda **MVC**, Tamam'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="ed50d-151">In the **New ASP.NET Project** dialog, select **MVC**, and click OK.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="ed50d-152">Proje Sihirbazı, iki proje oluşturur:</span><span class="sxs-lookup"><span data-stu-id="ed50d-152">The project wizard creates two projects:</span></span>

- <span data-ttu-id="ed50d-153">ChatService: Bu proje, Windows Azure uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="ed50d-153">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="ed50d-154">Bu, Azure rolleri ve diğer yapılandırma seçenekleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="ed50d-154">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="ed50d-155">SignalRChat: ASP.NET MVC 5 projeniz projesidir.</span><span class="sxs-lookup"><span data-stu-id="ed50d-155">SignalRChat: This project is your ASP.NET MVC 5 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="ed50d-156">SignalR sohbet uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="ed50d-156">Create the SignalR Chat Application</span></span>

<span data-ttu-id="ed50d-157">Sohbet uygulaması oluşturmak için öğreticinin adımları [SignalR ve MVC 5 ile çalışmaya başlama](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="ed50d-157">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="ed50d-158">Gerekli kitaplıkları yükleme için NuGet kullanın.</span><span class="sxs-lookup"><span data-stu-id="ed50d-158">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="ed50d-159">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="ed50d-159">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="ed50d-160">İçinde **Paket Yöneticisi Konsolu** penceresinde aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="ed50d-160">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="ed50d-161">Kullanım `-ProjectName` Windows Azure projesi yerine ASP.NET MVC projesi için paketleri yüklemek için seçeneği.</span><span class="sxs-lookup"><span data-stu-id="ed50d-161">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="ed50d-162">Devre kartına yapılandırın</span><span class="sxs-lookup"><span data-stu-id="ed50d-162">Configure the Backplane</span></span>

<span data-ttu-id="ed50d-163">Uygulamanızın Startup.cs dosyasına aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ed50d-163">In your application's Startup.cs file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="ed50d-164">Artık service bus bağlantı dizenizi almanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="ed50d-164">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="ed50d-165">Azure portalında, oluşturduğunuz hizmet veri yolu ad alanını seçin ve erişim anahtarı simgesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="ed50d-165">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

<span data-ttu-id="ed50d-166">Bağlantı dizesini panoya kopyalayın ve yapıştırın *connectionString* değişkeni.</span><span class="sxs-lookup"><span data-stu-id="ed50d-166">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="ed50d-167">Azure’a dağıtma</span><span class="sxs-lookup"><span data-stu-id="ed50d-167">Deploy to Azure</span></span>

<span data-ttu-id="ed50d-168">Çözüm Gezgini'nde **rolleri** ChatService proje klasöründen.</span><span class="sxs-lookup"><span data-stu-id="ed50d-168">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="ed50d-169">SignalRChat role sağ tıklayıp **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="ed50d-169">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="ed50d-170">Seçin **yapılandırma** sekmesi. Altında **örnekleri** 2'yi seçin.</span><span class="sxs-lookup"><span data-stu-id="ed50d-170">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="ed50d-171">Ayrıca VM boyutu ayarlayabileceğiniz **çok küçük**.</span><span class="sxs-lookup"><span data-stu-id="ed50d-171">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="ed50d-172">Değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="ed50d-172">Save the changes.</span></span>

<span data-ttu-id="ed50d-173">Çözüm Gezgini'nde ChatService projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="ed50d-173">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="ed50d-174">**Yayımla**’yı seçin.</span><span class="sxs-lookup"><span data-stu-id="ed50d-174">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="ed50d-175">Bu, ilk saat yayımlama Windows azure'a ise, kimlik bilgilerinizi indirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="ed50d-175">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="ed50d-176">İçinde **Yayımla** Sihirbazı, "kimlik bilgilerini indirmek oturum"'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="ed50d-176">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="ed50d-177">Bu, Windows Azure portalında oturum açıp bir yayımlama ayarları dosyasını indirin ister.</span><span class="sxs-lookup"><span data-stu-id="ed50d-177">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="ed50d-178">Tıklayın **alma** ve indirdiğiniz yayımlama ayarları dosyası seçin.</span><span class="sxs-lookup"><span data-stu-id="ed50d-178">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="ed50d-179">**İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="ed50d-179">Click **Next**.</span></span> <span data-ttu-id="ed50d-180">İçinde **yayımlama ayarları** iletişim altında **bulut hizmeti**, daha önce oluşturduğunuz bir bulut hizmeti seçin.</span><span class="sxs-lookup"><span data-stu-id="ed50d-180">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="ed50d-181">Tıklayın **yayımlama**.</span><span class="sxs-lookup"><span data-stu-id="ed50d-181">Click **Publish**.</span></span> <span data-ttu-id="ed50d-182">Uygulamanın, uygulamayı dağıtmak ve sanal makinelerin başlamak için birkaç dakika sürebilir.</span><span class="sxs-lookup"><span data-stu-id="ed50d-182">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="ed50d-183">Şimdi Sohbet uygulamayı çalıştırdığınızda, rol örneklerini bir Service Bus konusu kullanarak Azure Service Bus ile iletişim kurar.</span><span class="sxs-lookup"><span data-stu-id="ed50d-183">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="ed50d-184">Birden çok aboneyi izin veren bir ileti kuyruğu bir konudur.</span><span class="sxs-lookup"><span data-stu-id="ed50d-184">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="ed50d-185">Devre kartına, konu ve abonelik otomatik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ed50d-185">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="ed50d-186">Abonelikler ve ileti etkinliği görmek için Azure portalını açın, Service Bus ad alanını seçin ve "Konularda"'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="ed50d-186">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="ed50d-187">Bunu yapmak Panoda gösterilecek ileti etkinliği için birkaç dakika sürer.</span><span class="sxs-lookup"><span data-stu-id="ed50d-187">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

<span data-ttu-id="ed50d-188">SignalR konu yaşam süresini yönetir.</span><span class="sxs-lookup"><span data-stu-id="ed50d-188">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="ed50d-189">Uygulamanız dağıtıldığında olduğu sürece, konu ayarlarını el ile konuları silin veya çalışmayın.</span><span class="sxs-lookup"><span data-stu-id="ed50d-189">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="ed50d-190">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="ed50d-190">Troubleshooting</span></span>

<span data-ttu-id="ed50d-191">**System.InvalidOperationException "Yalnızca desteklenen IsolationLevel 'IsolationLevel.Serializable' sağlıyor."**</span><span class="sxs-lookup"><span data-stu-id="ed50d-191">**System.InvalidOperationException "The only supported IsolationLevel is 'IsolationLevel.Serializable'."**</span></span>

<span data-ttu-id="ed50d-192">İşlem düzeyi bir işlem için bir şeye dışında ayarlanırsa, bu hata oluşabilir `Serializable`.</span><span class="sxs-lookup"><span data-stu-id="ed50d-192">This error can occur if the transaction level for an operation is set to something other than `Serializable`.</span></span> <span data-ttu-id="ed50d-193">Diğer işlem düzeylerini ile bir işlem gerçekleştiriliyor doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="ed50d-193">Verify that no operations are being performed with other transaction levels.</span></span>
