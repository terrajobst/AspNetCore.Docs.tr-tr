---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Azure Service Bus ile SignalR ölçeğini genişletme (SignalR 1.x) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 05/01/2013
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: d597eebc958815b1b1b9fdffc256c4453efce6b3
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910960"
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a><span data-ttu-id="4aa14-102">Azure Service Bus ile SignalR ölçeğini genişletme (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="4aa14-102">SignalR Scaleout with Azure Service Bus (SignalR 1.x)</span></span>
====================
<span data-ttu-id="4aa14-103">tarafından [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="4aa14-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="4aa14-104">Bu öğreticide, bir Windows Azure Web her rol örneği iletilerini dağıtmak için Service Bus devre kartına kullanarak rol bir SignalR uygulamayı dağıtacaksınız.</span><span class="sxs-lookup"><span data-stu-id="4aa14-104">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="4aa14-105">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="4aa14-105">Prerequisites:</span></span>

- <span data-ttu-id="4aa14-106">Bir Windows Azure hesabı.</span><span class="sxs-lookup"><span data-stu-id="4aa14-106">A Windows Azure account.</span></span>
- <span data-ttu-id="4aa14-107">[Windows Azure SDK'sı](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="4aa14-107">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="4aa14-108">Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="4aa14-108">Visual Studio 2012.</span></span>

<span data-ttu-id="4aa14-109">Service bus devre kartı da uyumlu [Windows Server için hizmet veri yolu](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), sürüm 1.1.</span><span class="sxs-lookup"><span data-stu-id="4aa14-109">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="4aa14-110">Ancak, Windows Server için hizmet veri yolu 1.0 sürümü ile uyumlu değil.</span><span class="sxs-lookup"><span data-stu-id="4aa14-110">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="4aa14-111">Fiyatlandırma</span><span class="sxs-lookup"><span data-stu-id="4aa14-111">Pricing</span></span>

<span data-ttu-id="4aa14-112">Service Bus devre kartına ileti göndermek için konulara kullanır.</span><span class="sxs-lookup"><span data-stu-id="4aa14-112">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="4aa14-113">En son fiyatlandırma bilgileri için bkz. [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="4aa14-113">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="4aa14-114">Bu makalenin yazıldığı sırada, kısa $1 için ayda 1.000.000 iletileri gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="4aa14-114">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="4aa14-115">Devre kartına bir SignalR hub'yöntemini her çalıştırılışı için hizmet veri yolu ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="4aa14-115">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="4aa14-116">Bağlantı, bağlantı kesilmesi, katılma veya bırakma grupları ve benzeri için bazı denetim iletileri de vardır.</span><span class="sxs-lookup"><span data-stu-id="4aa14-116">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="4aa14-117">Çoğu uygulamada ileti trafiği çoğunu hub yöntemi çağrılarına olacaktır.</span><span class="sxs-lookup"><span data-stu-id="4aa14-117">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="4aa14-118">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="4aa14-118">Overview</span></span>

<span data-ttu-id="4aa14-119">Ayrıntılı Öğreticisine aldığımız önce ne yapacağını, hızlı bir genel bakış aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="4aa14-119">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="4aa14-120">Yeni bir Service Bus ad alanı oluşturmak için Windows Azure portal'ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="4aa14-120">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="4aa14-121">Bu NuGet paketlerini uygulamanıza ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4aa14-121">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="4aa14-122">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="4aa14-122">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="4aa14-123">Microsoft.AspNet.SignalR.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="4aa14-123">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="4aa14-124">Bir SignalR uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4aa14-124">Create a SignalR application.</span></span>
4. <span data-ttu-id="4aa14-125">Devre kartına yapılandırmak için Global.asax için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4aa14-125">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="4aa14-126">Her uygulama için "Uygulamanızınadı" için farklı bir değer seçin.</span><span class="sxs-lookup"><span data-stu-id="4aa14-126">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="4aa14-127">Aynı değeri, birden çok uygulamada kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="4aa14-127">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="4aa14-128">Azure hizmetleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="4aa14-128">Create the Azure Services</span></span>

<span data-ttu-id="4aa14-129">Bölümünde anlatıldığı gibi bir bulut hizmeti oluşturma [bir bulut hizmeti oluşturma ve dağıtma konusunda](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="4aa14-129">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="4aa14-130">Bölümündeki adımları "nasıl yapılır: hızlı Oluştur kullanarak bir bulut hizmeti oluşturma".</span><span class="sxs-lookup"><span data-stu-id="4aa14-130">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="4aa14-131">Bu öğreticide, bir sertifikayı karşıya yüklemek gerekmez.</span><span class="sxs-lookup"><span data-stu-id="4aa14-131">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="4aa14-132">Açıklanan şekilde yeni bir Service Bus ad alanı oluşturma [nasıl kullanım hizmet veri yolu konuları/abonelikleri için](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="4aa14-132">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="4aa14-133">"Hizmet Namespace oluşturma" bölümündeki adımları izleyin.</span><span class="sxs-lookup"><span data-stu-id="4aa14-133">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="4aa14-134">Bulut hizmeti ve Service Bus ad alanı için aynı bölge seçtiğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="4aa14-134">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="4aa14-135">Visual Studio projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="4aa14-135">Create the Visual Studio Project</span></span>

<span data-ttu-id="4aa14-136">Visual Studio'yu başlatın.</span><span class="sxs-lookup"><span data-stu-id="4aa14-136">Start Visual Studio.</span></span> <span data-ttu-id="4aa14-137">Gelen **dosya** menüsünü tıklatın **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="4aa14-137">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="4aa14-138">İçinde **yeni proje** iletişim kutusunda **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="4aa14-138">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="4aa14-139">Altında **yüklü şablonlar**seçin **bulut** seçip **Windows Azure bulut hizmeti**.</span><span class="sxs-lookup"><span data-stu-id="4aa14-139">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="4aa14-140">Varsayılan .NET Framework 4.5 tutun.</span><span class="sxs-lookup"><span data-stu-id="4aa14-140">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="4aa14-141">ChatService uygulamaya bir ad ve tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="4aa14-141">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="4aa14-142">İçinde **yeni Windows Azure bulut hizmeti** iletişim kutusunda seçim ASP.NET MVC 4 Web rolü.</span><span class="sxs-lookup"><span data-stu-id="4aa14-142">In the **New Windows Azure Cloud Service** dialog, select ASP.NET MVC 4 Web Role.</span></span> <span data-ttu-id="4aa14-143">Sağ ok düğmesine tıklayın (**&gt;**) rolünü çözümünüze ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4aa14-143">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="4aa14-144">Fareyi yeni rol, bu nedenle gelin Kurşun Kalem simgesi görünür.</span><span class="sxs-lookup"><span data-stu-id="4aa14-144">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="4aa14-145">Rolü yeniden adlandırmak için bu simgeye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4aa14-145">Click this icon to rename the role.</span></span> <span data-ttu-id="4aa14-146">Rol "SignalRChat" adını verin ve tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="4aa14-146">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="4aa14-147">İçinde **yeni ASP.NET MVC 4 proje** seçin **Internet uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="4aa14-147">In the **New ASP.NET MVC 4 Project** wizard, select **Internet Application**.</span></span> <span data-ttu-id="4aa14-148">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="4aa14-148">Click **OK**.</span></span> <span data-ttu-id="4aa14-149">Proje Sihirbazı, iki proje oluşturur:</span><span class="sxs-lookup"><span data-stu-id="4aa14-149">The project wizard creates two projects:</span></span>

- <span data-ttu-id="4aa14-150">ChatService: Bu proje, Windows Azure uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="4aa14-150">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="4aa14-151">Bu, Azure rolleri ve diğer yapılandırma seçenekleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="4aa14-151">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="4aa14-152">SignalRChat: Bu, ASP.NET MVC 4 Proje projesidir.</span><span class="sxs-lookup"><span data-stu-id="4aa14-152">SignalRChat: This project is your ASP.NET MVC 4 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="4aa14-153">SignalR sohbet uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="4aa14-153">Create the SignalR Chat Application</span></span>

<span data-ttu-id="4aa14-154">Sohbet uygulaması oluşturmak için öğreticinin adımları [SignalR ve MVC 4 ile çalışmaya başlama](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="4aa14-154">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="4aa14-155">Gerekli kitaplıkları yükleme için NuGet kullanın.</span><span class="sxs-lookup"><span data-stu-id="4aa14-155">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="4aa14-156">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="4aa14-156">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="4aa14-157">İçinde **Paket Yöneticisi Konsolu** penceresinde aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="4aa14-157">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="4aa14-158">Kullanım `-ProjectName` Windows Azure projesi yerine ASP.NET MVC projesi için paketleri yüklemek için seçeneği.</span><span class="sxs-lookup"><span data-stu-id="4aa14-158">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="4aa14-159">Devre kartına yapılandırın</span><span class="sxs-lookup"><span data-stu-id="4aa14-159">Configure the Backplane</span></span>

<span data-ttu-id="4aa14-160">Uygulamanızın Global.asax dosyasında aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4aa14-160">In your application's Global.asax file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="4aa14-161">Artık service bus bağlantı dizenizi almanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="4aa14-161">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="4aa14-162">Azure portalında, oluşturduğunuz hizmet veri yolu ad alanını seçin ve erişim anahtarı simgesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4aa14-162">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="4aa14-163">Bağlantı dizesini panoya kopyalayın ve yapıştırın *connectionString* değişkeni.</span><span class="sxs-lookup"><span data-stu-id="4aa14-163">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="4aa14-164">Azure’a dağıtma</span><span class="sxs-lookup"><span data-stu-id="4aa14-164">Deploy to Azure</span></span>

<span data-ttu-id="4aa14-165">Çözüm Gezgini'nde **rolleri** ChatService proje klasöründen.</span><span class="sxs-lookup"><span data-stu-id="4aa14-165">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

<span data-ttu-id="4aa14-166">SignalRChat role sağ tıklayıp **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="4aa14-166">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="4aa14-167">Seçin **yapılandırma** sekmesi. Altında **örnekleri** 2'yi seçin.</span><span class="sxs-lookup"><span data-stu-id="4aa14-167">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="4aa14-168">Ayrıca VM boyutu ayarlayabileceğiniz **çok küçük**.</span><span class="sxs-lookup"><span data-stu-id="4aa14-168">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="4aa14-169">Değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="4aa14-169">Save the changes.</span></span>

<span data-ttu-id="4aa14-170">Çözüm Gezgini'nde ChatService projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4aa14-170">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="4aa14-171">Seçin **yayımlama**.</span><span class="sxs-lookup"><span data-stu-id="4aa14-171">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="4aa14-172">Bu, ilk saat yayımlama Windows azure'a ise, kimlik bilgilerinizi indirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="4aa14-172">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="4aa14-173">İçinde **Yayımla** Sihirbazı, "kimlik bilgilerini indirmek oturum"'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4aa14-173">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="4aa14-174">Bu, Windows Azure portalında oturum açıp bir yayımlama ayarları dosyasını indirin ister.</span><span class="sxs-lookup"><span data-stu-id="4aa14-174">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="4aa14-175">Tıklayın **alma** ve indirdiğiniz yayımlama ayarları dosyası seçin.</span><span class="sxs-lookup"><span data-stu-id="4aa14-175">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="4aa14-176">**İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4aa14-176">Click **Next**.</span></span> <span data-ttu-id="4aa14-177">İçinde **yayımlama ayarları** iletişim altında **bulut hizmeti**, daha önce oluşturduğunuz bir bulut hizmeti seçin.</span><span class="sxs-lookup"><span data-stu-id="4aa14-177">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="4aa14-178">Tıklayın **yayımlama**.</span><span class="sxs-lookup"><span data-stu-id="4aa14-178">Click **Publish**.</span></span> <span data-ttu-id="4aa14-179">Uygulamanın, uygulamayı dağıtmak ve sanal makinelerin başlamak için birkaç dakika sürebilir.</span><span class="sxs-lookup"><span data-stu-id="4aa14-179">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="4aa14-180">Şimdi Sohbet uygulamayı çalıştırdığınızda, rol örneklerini bir Service Bus konusu kullanarak Azure Service Bus ile iletişim kurar.</span><span class="sxs-lookup"><span data-stu-id="4aa14-180">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="4aa14-181">Birden çok aboneyi izin veren bir ileti kuyruğu bir konudur.</span><span class="sxs-lookup"><span data-stu-id="4aa14-181">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="4aa14-182">Devre kartına, konu ve abonelik otomatik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4aa14-182">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="4aa14-183">Abonelikler ve ileti etkinliği görmek için Azure portalını açın, Service Bus ad alanını seçin ve "Konularda"'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4aa14-183">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="4aa14-184">Bunu yapmak Panoda gösterilecek ileti etkinliği için birkaç dakika sürer.</span><span class="sxs-lookup"><span data-stu-id="4aa14-184">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="4aa14-185">SignalR konu yaşam süresini yönetir.</span><span class="sxs-lookup"><span data-stu-id="4aa14-185">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="4aa14-186">Uygulamanız dağıtıldığında olduğu sürece, konu ayarlarını el ile konuları silin veya çalışmayın.</span><span class="sxs-lookup"><span data-stu-id="4aa14-186">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>
