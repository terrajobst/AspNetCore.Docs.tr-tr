---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: SignalR genişletme Azure Service Bus ile | Microsoft Docs
author: MikeWasson
description: Yazılım sürümleri bu konu Visual Studio 2013 .NET 4.5 SignalR sürümünde bu konuda Bu konu için SignalR 1.x sürümü 2 önceki sürümlerinde kullanılan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: e6d9e4e6ba2040aa2c6e453aacf0ddca38c4a1a9
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "32741546"
---
<a name="signalr-scaleout-with-azure-service-bus"></a><span data-ttu-id="67831-103">Azure Service Bus ile SignalR genişletme</span><span class="sxs-lookup"><span data-stu-id="67831-103">SignalR Scaleout with Azure Service Bus</span></span>
====================
<span data-ttu-id="67831-104">tarafından [CAN Wasson](https://github.com/MikeWasson), [CAN Fletcher'dan](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="67831-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="67831-105">Bu öğreticide, bir Windows Azure Web her rol örneği iletilerini dağıtmak için Service Bus devre kartı kullanarak rolü bir SignalR uygulamasına dağıtırsınız.</span><span class="sxs-lookup"><span data-stu-id="67831-105">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span> <span data-ttu-id="67831-106">(Service Bus devre kartı ile de kullanabileceğiniz [web uygulamaları Azure App Service'te](https://docs.microsoft.com/azure/app-service-web/).)</span><span class="sxs-lookup"><span data-stu-id="67831-106">(You can also use the Service Bus backplane with [web apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="67831-107">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="67831-107">Prerequisites:</span></span>

- <span data-ttu-id="67831-108">Bir Windows Azure hesabı.</span><span class="sxs-lookup"><span data-stu-id="67831-108">A Windows Azure account.</span></span>
- <span data-ttu-id="67831-109">[Windows Azure SDK'sı](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="67831-109">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="67831-110">Visual Studio 2012 veya 2013.</span><span class="sxs-lookup"><span data-stu-id="67831-110">Visual Studio 2012 or 2013.</span></span>

<span data-ttu-id="67831-111">Hizmet veri yolu devre kartı ayrıca ile uyumlu [Windows Server için hizmet veri yolu](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), sürüm 1.1.</span><span class="sxs-lookup"><span data-stu-id="67831-111">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="67831-112">Ancak, Windows Server için hizmet veri yolu 1.0 sürümü ile uyumlu değil.</span><span class="sxs-lookup"><span data-stu-id="67831-112">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="67831-113">Fiyatlandırma</span><span class="sxs-lookup"><span data-stu-id="67831-113">Pricing</span></span>

<span data-ttu-id="67831-114">Hizmet veri yolu devre kartı konuları iletileri göndermek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="67831-114">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="67831-115">En son fiyatlandırma bilgileri için bkz: [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="67831-115">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="67831-116">Bu yazma zaman $1'den için aylık 1.000.000 iletileri gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="67831-116">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="67831-117">Devre kartına her bir SignalR hub yönteminin çağrılması için hizmet veri yolu ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="67831-117">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="67831-118">Bağlantılar, bağlantısının kesilmesi, birleşme veya bırakma grupları ve benzeri için bazı denetim iletileri de vardır.</span><span class="sxs-lookup"><span data-stu-id="67831-118">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="67831-119">Çoğu uygulamada ileti trafiği çoğunluğu hub yöntemi çağrılarına olacaktır.</span><span class="sxs-lookup"><span data-stu-id="67831-119">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="67831-120">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="67831-120">Overview</span></span>

<span data-ttu-id="67831-121">Biz ayrıntılı öğretici ulaşmadan ne yapacağını Hızlı Bakış aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="67831-121">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="67831-122">Yeni bir hizmet veri yolu ad alanı oluşturmak için Windows Azure Portalı'nı kullanın.</span><span class="sxs-lookup"><span data-stu-id="67831-122">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="67831-123">Bu NuGet paketleri uygulamanıza ekleyin:</span><span class="sxs-lookup"><span data-stu-id="67831-123">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="67831-124">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="67831-124">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - <span data-ttu-id="67831-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) veya [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span><span class="sxs-lookup"><span data-stu-id="67831-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) or [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span></span>
3. <span data-ttu-id="67831-126">Bir SignalR uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="67831-126">Create a SignalR application.</span></span>
4. <span data-ttu-id="67831-127">Devre kartına yapılandırmak için haline için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="67831-127">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="67831-128">Bu kod için varsayılan değerlerle devre kartı yapılandırır [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) ve [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="67831-128">This code configures the backplane with the default values for [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="67831-129">Bu değerleri değiştirme hakkında daha fazla bilgi için bkz: [SignalR performans: genişletme ölçümleri](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="67831-129">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

<span data-ttu-id="67831-130">Her uygulama için "Uygulamanızınadı" için farklı bir değer seçin.</span><span class="sxs-lookup"><span data-stu-id="67831-130">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="67831-131">Aynı değeri birden çok uygulama arasında kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="67831-131">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="67831-132">Azure hizmetleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="67831-132">Create the Azure Services</span></span>

<span data-ttu-id="67831-133">Bölümünde açıklandığı gibi bir bulut hizmeti Oluştur [nasıl oluşturulacağı ve bir bulut hizmetinin dağıtılacağı](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="67831-133">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="67831-134">Bölümündeki adımları "nasıl yapılır: hızlı Oluştur kullanarak bir bulut hizmeti oluşturma".</span><span class="sxs-lookup"><span data-stu-id="67831-134">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="67831-135">Bu öğreticide, bir sertifikayı karşıya yüklemek gerekmez.</span><span class="sxs-lookup"><span data-stu-id="67831-135">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="67831-136">Bölümünde açıklandığı gibi yeni bir hizmet veri yolu ad alanı oluşturma [nasıl kullanım Service Bus konuları/abonelikleri](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="67831-136">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="67831-137">"Hizmet Namespace oluşturma" bölümündeki adımları izleyin.</span><span class="sxs-lookup"><span data-stu-id="67831-137">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="67831-138">Bulut hizmeti ve Service Bus ad alanı için aynı bölgede seçtiğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="67831-138">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="67831-139">Visual Studio projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="67831-139">Create the Visual Studio Project</span></span>

<span data-ttu-id="67831-140">Visual Studio'yu başlatın.</span><span class="sxs-lookup"><span data-stu-id="67831-140">Start Visual Studio.</span></span> <span data-ttu-id="67831-141">Gelen **dosya** menüsünde tıklatın **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="67831-141">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="67831-142">İçinde **yeni proje** iletişim kutusunda, genişletin **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="67831-142">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="67831-143">Altında **yüklü şablonlar**seçin **bulut** ve ardından **Windows Azure bulut hizmeti**.</span><span class="sxs-lookup"><span data-stu-id="67831-143">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="67831-144">Varsayılan .NET Framework 4.5 tutun.</span><span class="sxs-lookup"><span data-stu-id="67831-144">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="67831-145">ChatService uygulama adı ve'ı tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="67831-145">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="67831-146">İçinde **yeni Windows Azure bulut hizmeti** iletişim kutusunda, ASP.NET Web rolü seçin.</span><span class="sxs-lookup"><span data-stu-id="67831-146">In the **New Windows Azure Cloud Service** dialog, select ASP.NET Web Role.</span></span> <span data-ttu-id="67831-147">Sağ ok düğmesine tıklayın (**&gt;**) çözümünüze rolü eklemek için.</span><span class="sxs-lookup"><span data-stu-id="67831-147">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="67831-148">Fare yeni rol, bu nedenle vurgulu kalem simgesi görünür.</span><span class="sxs-lookup"><span data-stu-id="67831-148">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="67831-149">Rolü yeniden adlandırmak için bu simgeyi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="67831-149">Click this icon to rename the role.</span></span> <span data-ttu-id="67831-150">"SignalRChat" rol adını verin ve tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="67831-150">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="67831-151">İçinde **yeni ASP.NET projesi** iletişim kutusunda **MVC**, Tamam'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="67831-151">In the **New ASP.NET Project** dialog, select **MVC**, and click OK.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="67831-152">Proje Sihirbazı iki proje oluşturur:</span><span class="sxs-lookup"><span data-stu-id="67831-152">The project wizard creates two projects:</span></span>

- <span data-ttu-id="67831-153">ChatService: Bu proje Windows Azure uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="67831-153">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="67831-154">Azure rol ve diğer yapılandırma seçeneklerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="67831-154">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="67831-155">SignalRChat: Bu, ASP.NET MVC 5 projeniz projesidir.</span><span class="sxs-lookup"><span data-stu-id="67831-155">SignalRChat: This project is your ASP.NET MVC 5 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="67831-156">SignalR sohbet uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="67831-156">Create the SignalR Chat Application</span></span>

<span data-ttu-id="67831-157">Sohbet uygulaması oluşturmak için öğreticide adımları [SignalR ve MVC 5 ile çalışmaya başlama](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="67831-157">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="67831-158">Gerekli kitaplıkları yükleme için NuGet kullanın.</span><span class="sxs-lookup"><span data-stu-id="67831-158">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="67831-159">Gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="67831-159">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="67831-160">İçinde **Paket Yöneticisi Konsolu** penceresinde aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="67831-160">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="67831-161">Kullanım `-ProjectName` Microsoft Azure projesi yerine ASP.NET MVC projesinin paketleri yüklemek için seçeneği.</span><span class="sxs-lookup"><span data-stu-id="67831-161">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="67831-162">Devre kartına yapılandırın</span><span class="sxs-lookup"><span data-stu-id="67831-162">Configure the Backplane</span></span>

<span data-ttu-id="67831-163">Uygulamanızın haline dosyasına aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="67831-163">In your application's Startup.cs file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="67831-164">Artık service bus bağlantı dizenizi almanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="67831-164">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="67831-165">Azure portalında oluşturduğunuz hizmet veri yolu ad alanını seçin ve erişim anahtarı simgesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="67831-165">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

<span data-ttu-id="67831-166">Bağlantı dizesini Pano'ya kopyalayın ve yapıştırın *connectionString* değişkeni.</span><span class="sxs-lookup"><span data-stu-id="67831-166">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="67831-167">Azure’a dağıtma</span><span class="sxs-lookup"><span data-stu-id="67831-167">Deploy to Azure</span></span>

<span data-ttu-id="67831-168">Çözüm Gezgini'nde genişletin **rolleri** ChatService proje klasöründen.</span><span class="sxs-lookup"><span data-stu-id="67831-168">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="67831-169">SignalRChat role sağ tıklayın ve seçin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="67831-169">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="67831-170">Seçin **yapılandırma** sekmesi. Altında **örnekleri** 2'yi seçin.</span><span class="sxs-lookup"><span data-stu-id="67831-170">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="67831-171">De VM boyutu ayarlayabilirsiniz **ek küçük**.</span><span class="sxs-lookup"><span data-stu-id="67831-171">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="67831-172">Değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="67831-172">Save the changes.</span></span>

<span data-ttu-id="67831-173">Çözüm Gezgini'nde ChatService projesine sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="67831-173">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="67831-174">Seçin **yayımlama**.</span><span class="sxs-lookup"><span data-stu-id="67831-174">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="67831-175">Bu, ilk Windows Azure zaman yayımlama ise, kimlik bilgilerinizi indirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="67831-175">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="67831-176">İçinde **Yayımla** Sihirbazı, "kimlik bilgilerini indirmek oturum"'i tıklatın.</span><span class="sxs-lookup"><span data-stu-id="67831-176">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="67831-177">Bu Windows Azure portalında oturum açın ve yayımlama ayarları dosyasını indirme isteyip istemediğinizi sorar.</span><span class="sxs-lookup"><span data-stu-id="67831-177">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="67831-178">Tıklatın **alma** ve indirdiğiniz yayımlama ayarları dosyasını seçin.</span><span class="sxs-lookup"><span data-stu-id="67831-178">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="67831-179">**İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="67831-179">Click **Next**.</span></span> <span data-ttu-id="67831-180">İçinde **yayımlama ayarları** iletişim altında **bulut hizmeti**, daha önce oluşturduğunuz bulut hizmeti seçin.</span><span class="sxs-lookup"><span data-stu-id="67831-180">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="67831-181">Tıklatın **yayımlama**.</span><span class="sxs-lookup"><span data-stu-id="67831-181">Click **Publish**.</span></span> <span data-ttu-id="67831-182">Uygulamayı dağıtmak ve sanal makineleri başlatmak için birkaç dakika sürebilir.</span><span class="sxs-lookup"><span data-stu-id="67831-182">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="67831-183">Şimdi Sohbet uygulamayı çalıştırdığınızda, rol örneklerinin bir Service Bus konu kullanarak Azure Service Bus iletişim kurar.</span><span class="sxs-lookup"><span data-stu-id="67831-183">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="67831-184">Birden çok aboneye sağlayan bir ileti sırası konudur.</span><span class="sxs-lookup"><span data-stu-id="67831-184">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="67831-185">Devre kartına, konu ve abonelik otomatik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="67831-185">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="67831-186">İleti etkinliği ve abonelikleri görmek için Azure portalını açın, hizmet veri yolu ad alanını seçin ve "Konularda"'i tıklatın.</span><span class="sxs-lookup"><span data-stu-id="67831-186">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="67831-187">Anlaması panosunda göstermeyi ileti etkinliği için birkaç dakika sürer.</span><span class="sxs-lookup"><span data-stu-id="67831-187">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

<span data-ttu-id="67831-188">SignalR konu yaşam süresini yönetir.</span><span class="sxs-lookup"><span data-stu-id="67831-188">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="67831-189">Uygulamanızı dağıttınız sürece, el ile konuları silmek veya konuyla ilgili ayarları değiştirmek denemeyin.</span><span class="sxs-lookup"><span data-stu-id="67831-189">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="67831-190">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="67831-190">Troubleshooting</span></span>

<span data-ttu-id="67831-191">**System.InvalidOperationException "Yalnızca desteklenen IsolationLevel 'IsolationLevel.Serializable' değil."**</span><span class="sxs-lookup"><span data-stu-id="67831-191">**System.InvalidOperationException "The only supported IsolationLevel is 'IsolationLevel.Serializable'."**</span></span>

<span data-ttu-id="67831-192">Bir işlem için işlem düzeyi bir şeye dışında ayarlandıysa, bu hata oluşabilir `Serializable`.</span><span class="sxs-lookup"><span data-stu-id="67831-192">This error can occur if the transaction level for an operation is set to something other than `Serializable`.</span></span> <span data-ttu-id="67831-193">Diğer işlem düzeyleriyle hiçbir işlem gerçekleştiriliyor doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="67831-193">Verify that no operations are being performed with other transaction levels.</span></span>
