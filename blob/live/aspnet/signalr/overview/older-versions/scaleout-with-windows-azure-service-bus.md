---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: "Azure Service Bus ile SignalR genişletme (SignalR 1.x) | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 0dd245b597ebd4b58b60a53276d7808b6e2377e7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a><span data-ttu-id="a53d7-102">Azure Service Bus ile SignalR genişletme (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="a53d7-102">SignalR Scaleout with Azure Service Bus (SignalR 1.x)</span></span>
====================
<span data-ttu-id="a53d7-103">tarafından [CAN Wasson](https://github.com/MikeWasson), [CAN Fletcher'dan](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="a53d7-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="a53d7-104">Bu öğreticide, bir Windows Azure Web her rol örneği iletilerini dağıtmak için Service Bus devre kartı kullanarak rolü bir SignalR uygulamasına dağıtırsınız.</span><span class="sxs-lookup"><span data-stu-id="a53d7-104">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="a53d7-105">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="a53d7-105">Prerequisites:</span></span>

- <span data-ttu-id="a53d7-106">Bir Windows Azure hesabı.</span><span class="sxs-lookup"><span data-stu-id="a53d7-106">A Windows Azure account.</span></span>
- <span data-ttu-id="a53d7-107">[Windows Azure SDK'sı](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="a53d7-107">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="a53d7-108">Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="a53d7-108">Visual Studio 2012.</span></span>

<span data-ttu-id="a53d7-109">Hizmet veri yolu devre kartı ayrıca ile uyumlu [Windows Server için hizmet veri yolu](https://msdn.microsoft.com/en-us/library/windowsazure/dn282144.aspx), sürüm 1.1.</span><span class="sxs-lookup"><span data-stu-id="a53d7-109">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/en-us/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="a53d7-110">Ancak, Windows Server için hizmet veri yolu 1.0 sürümü ile uyumlu değil.</span><span class="sxs-lookup"><span data-stu-id="a53d7-110">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="a53d7-111">Fiyatlandırma</span><span class="sxs-lookup"><span data-stu-id="a53d7-111">Pricing</span></span>

<span data-ttu-id="a53d7-112">Hizmet veri yolu devre kartı konuları iletileri göndermek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="a53d7-112">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="a53d7-113">En son fiyatlandırma bilgileri için bkz: [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="a53d7-113">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="a53d7-114">Bu yazma zaman $1'den için aylık 1.000.000 iletileri gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="a53d7-114">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="a53d7-115">Devre kartına her bir SignalR hub yönteminin çağrılması için hizmet veri yolu ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="a53d7-115">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="a53d7-116">Bağlantılar, bağlantısının kesilmesi, birleşme veya bırakma grupları ve benzeri için bazı denetim iletileri de vardır.</span><span class="sxs-lookup"><span data-stu-id="a53d7-116">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="a53d7-117">Çoğu uygulamada ileti trafiği çoğunluğu hub yöntemi çağrılarına olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a53d7-117">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="a53d7-118">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="a53d7-118">Overview</span></span>

<span data-ttu-id="a53d7-119">Biz ayrıntılı öğretici ulaşmadan ne yapacağını Hızlı Bakış aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a53d7-119">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="a53d7-120">Yeni bir hizmet veri yolu ad alanı oluşturmak için Windows Azure Portalı'nı kullanın.</span><span class="sxs-lookup"><span data-stu-id="a53d7-120">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="a53d7-121">Bu NuGet paketleri uygulamanıza ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a53d7-121">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="a53d7-122">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="a53d7-122">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="a53d7-123">Microsoft.AspNet.SignalR.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="a53d7-123">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="a53d7-124">Bir SignalR uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a53d7-124">Create a SignalR application.</span></span>
4. <span data-ttu-id="a53d7-125">Devre kartına yapılandırmak için Global.asax için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a53d7-125">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="a53d7-126">Her uygulama için "Uygulamanızınadı" için farklı bir değer seçin.</span><span class="sxs-lookup"><span data-stu-id="a53d7-126">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="a53d7-127">Aynı değeri birden çok uygulama arasında kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="a53d7-127">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="a53d7-128">Azure hizmetleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="a53d7-128">Create the Azure Services</span></span>

<span data-ttu-id="a53d7-129">Bölümünde açıklandığı gibi bir bulut hizmeti Oluştur [nasıl oluşturulacağı ve bir bulut hizmetinin dağıtılacağı](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="a53d7-129">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="a53d7-130">Bölümündeki adımları "nasıl yapılır: hızlı Oluştur kullanarak bir bulut hizmeti oluşturma".</span><span class="sxs-lookup"><span data-stu-id="a53d7-130">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="a53d7-131">Bu öğreticide, bir sertifikayı karşıya yüklemek gerekmez.</span><span class="sxs-lookup"><span data-stu-id="a53d7-131">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="a53d7-132">Bölümünde açıklandığı gibi yeni bir hizmet veri yolu ad alanı oluşturma [nasıl kullanım Service Bus konuları/abonelikleri](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="a53d7-132">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="a53d7-133">"Hizmet Namespace oluşturma" bölümündeki adımları izleyin.</span><span class="sxs-lookup"><span data-stu-id="a53d7-133">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="a53d7-134">Bulut hizmeti ve Service Bus ad alanı için aynı bölgede seçtiğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="a53d7-134">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="a53d7-135">Visual Studio projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="a53d7-135">Create the Visual Studio Project</span></span>

<span data-ttu-id="a53d7-136">Visual Studio'yu başlatın.</span><span class="sxs-lookup"><span data-stu-id="a53d7-136">Start Visual Studio.</span></span> <span data-ttu-id="a53d7-137">Gelen **dosya** menüsünde tıklatın **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="a53d7-137">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="a53d7-138">İçinde **yeni proje** iletişim kutusunda, genişletin **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="a53d7-138">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="a53d7-139">Altında **yüklü şablonlar**seçin **bulut** ve ardından **Windows Azure bulut hizmeti**.</span><span class="sxs-lookup"><span data-stu-id="a53d7-139">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="a53d7-140">Varsayılan .NET Framework 4.5 tutun.</span><span class="sxs-lookup"><span data-stu-id="a53d7-140">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="a53d7-141">ChatService uygulama adı ve'ı tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="a53d7-141">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="a53d7-142">İçinde **yeni Windows Azure bulut hizmeti** iletişim kutusunda, select ASP.NET MVC 4 Web rolü.</span><span class="sxs-lookup"><span data-stu-id="a53d7-142">In the **New Windows Azure Cloud Service** dialog, select ASP.NET MVC 4 Web Role.</span></span> <span data-ttu-id="a53d7-143">Sağ ok düğmesine tıklayın (**&gt;**) çözümünüze rolü eklemek için.</span><span class="sxs-lookup"><span data-stu-id="a53d7-143">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="a53d7-144">Fare yeni rol, bu nedenle vurgulu kalem simgesi görünür.</span><span class="sxs-lookup"><span data-stu-id="a53d7-144">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="a53d7-145">Rolü yeniden adlandırmak için bu simgeyi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="a53d7-145">Click this icon to rename the role.</span></span> <span data-ttu-id="a53d7-146">"SignalRChat" rol adını verin ve tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="a53d7-146">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="a53d7-147">İçinde **yeni ASP.NET MVC 4 proje** seçin **Internet uygulama**.</span><span class="sxs-lookup"><span data-stu-id="a53d7-147">In the **New ASP.NET MVC 4 Project** wizard, select **Internet Application**.</span></span> <span data-ttu-id="a53d7-148">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="a53d7-148">Click **OK**.</span></span> <span data-ttu-id="a53d7-149">Proje Sihirbazı iki proje oluşturur:</span><span class="sxs-lookup"><span data-stu-id="a53d7-149">The project wizard creates two projects:</span></span>

- <span data-ttu-id="a53d7-150">ChatService: Bu proje Windows Azure uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="a53d7-150">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="a53d7-151">Azure rol ve diğer yapılandırma seçeneklerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="a53d7-151">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="a53d7-152">SignalRChat: Bu, ASP.NET MVC 4 Proje projesidir.</span><span class="sxs-lookup"><span data-stu-id="a53d7-152">SignalRChat: This project is your ASP.NET MVC 4 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="a53d7-153">SignalR sohbet uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="a53d7-153">Create the SignalR Chat Application</span></span>

<span data-ttu-id="a53d7-154">Sohbet uygulaması oluşturmak için öğreticide adımları [SignalR ve MVC 4 ile çalışmaya başlama](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="a53d7-154">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="a53d7-155">Gerekli kitaplıkları yükleme için NuGet kullanın.</span><span class="sxs-lookup"><span data-stu-id="a53d7-155">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="a53d7-156">Gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="a53d7-156">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="a53d7-157">İçinde **Paket Yöneticisi Konsolu** penceresinde aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="a53d7-157">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="a53d7-158">Kullanım `-ProjectName` Microsoft Azure projesi yerine ASP.NET MVC projesinin paketleri yüklemek için seçeneği.</span><span class="sxs-lookup"><span data-stu-id="a53d7-158">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="a53d7-159">Devre kartına yapılandırın</span><span class="sxs-lookup"><span data-stu-id="a53d7-159">Configure the Backplane</span></span>

<span data-ttu-id="a53d7-160">Uygulamanızın Global.asax dosyasında aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a53d7-160">In your application's Global.asax file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="a53d7-161">Artık service bus bağlantı dizenizi almanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a53d7-161">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="a53d7-162">Azure portalında oluşturduğunuz hizmet veri yolu ad alanını seçin ve erişim anahtarı simgesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="a53d7-162">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="a53d7-163">Bağlantı dizesini Pano'ya kopyalayın ve yapıştırın *connectionString* değişkeni.</span><span class="sxs-lookup"><span data-stu-id="a53d7-163">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="a53d7-164">Azure'a dağıtma</span><span class="sxs-lookup"><span data-stu-id="a53d7-164">Deploy to Azure</span></span>

<span data-ttu-id="a53d7-165">Çözüm Gezgini'nde genişletin **rolleri** ChatService proje klasöründen.</span><span class="sxs-lookup"><span data-stu-id="a53d7-165">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

<span data-ttu-id="a53d7-166">SignalRChat role sağ tıklayın ve seçin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="a53d7-166">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="a53d7-167">Seçin **yapılandırma** sekmesi. Altında **örnekleri** 2'yi seçin.</span><span class="sxs-lookup"><span data-stu-id="a53d7-167">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="a53d7-168">De VM boyutu ayarlayabilirsiniz **ek küçük**.</span><span class="sxs-lookup"><span data-stu-id="a53d7-168">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="a53d7-169">Değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="a53d7-169">Save the changes.</span></span>

<span data-ttu-id="a53d7-170">Çözüm Gezgini'nde ChatService projesine sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="a53d7-170">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="a53d7-171">Seçin **yayımlama**.</span><span class="sxs-lookup"><span data-stu-id="a53d7-171">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="a53d7-172">Bu, ilk Windows Azure zaman yayımlama ise, kimlik bilgilerinizi indirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a53d7-172">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="a53d7-173">İçinde **Yayımla** Sihirbazı, "kimlik bilgilerini indirmek oturum"'i tıklatın.</span><span class="sxs-lookup"><span data-stu-id="a53d7-173">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="a53d7-174">Bu Windows Azure portalında oturum açın ve yayımlama ayarları dosyasını indirme isteyip istemediğinizi sorar.</span><span class="sxs-lookup"><span data-stu-id="a53d7-174">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="a53d7-175">Tıklatın **alma** ve indirdiğiniz yayımlama ayarları dosyasını seçin.</span><span class="sxs-lookup"><span data-stu-id="a53d7-175">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="a53d7-176">
              **İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="a53d7-176">Click **Next**.</span></span> <span data-ttu-id="a53d7-177">İçinde **yayımlama ayarları** iletişim altında **bulut hizmeti**, daha önce oluşturduğunuz bulut hizmeti seçin.</span><span class="sxs-lookup"><span data-stu-id="a53d7-177">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="a53d7-178">Tıklatın **yayımlama**.</span><span class="sxs-lookup"><span data-stu-id="a53d7-178">Click **Publish**.</span></span> <span data-ttu-id="a53d7-179">Uygulamayı dağıtmak ve sanal makineleri başlatmak için birkaç dakika sürebilir.</span><span class="sxs-lookup"><span data-stu-id="a53d7-179">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="a53d7-180">Şimdi Sohbet uygulamayı çalıştırdığınızda, rol örneklerinin bir Service Bus konu kullanarak Azure Service Bus iletişim kurar.</span><span class="sxs-lookup"><span data-stu-id="a53d7-180">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="a53d7-181">Birden çok aboneye sağlayan bir ileti sırası konudur.</span><span class="sxs-lookup"><span data-stu-id="a53d7-181">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="a53d7-182">Devre kartına, konu ve abonelik otomatik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a53d7-182">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="a53d7-183">İleti etkinliği ve abonelikleri görmek için Azure portalını açın, hizmet veri yolu ad alanını seçin ve "Konularda"'i tıklatın.</span><span class="sxs-lookup"><span data-stu-id="a53d7-183">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="a53d7-184">Anlaması panosunda göstermeyi ileti etkinliği için birkaç dakika sürer.</span><span class="sxs-lookup"><span data-stu-id="a53d7-184">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="a53d7-185">SignalR konu yaşam süresini yönetir.</span><span class="sxs-lookup"><span data-stu-id="a53d7-185">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="a53d7-186">Uygulamanızı dağıttınız sürece, el ile konuları silmek veya konuyla ilgili ayarları değiştirmek denemeyin.</span><span class="sxs-lookup"><span data-stu-id="a53d7-186">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>
