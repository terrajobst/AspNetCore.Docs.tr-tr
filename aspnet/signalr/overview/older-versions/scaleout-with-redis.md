---
uid: signalr/overview/older-versions/scaleout-with-redis
title: Redis ile SignalR genişletme (SignalR 1.x) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 8376c6537d693841a621158358cc8f69cda0a1d6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28035745"
---
<a name="signalr-scaleout-with-redis-signalr-1x"></a><span data-ttu-id="3474b-102">Redis ile SignalR genişletme (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="3474b-102">SignalR Scaleout with Redis (SignalR 1.x)</span></span>
====================
<span data-ttu-id="3474b-103">tarafından [CAN Wasson](https://github.com/MikeWasson), [CAN Fletcher'dan](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="3474b-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="3474b-104">Bu öğreticide, kullanacağınız [Redis](http://redis.io/) iki ayrı IIS örneklerinde dağıtılan bir SignalR uygulaması üzerinden iletilerini dağıtmak için.</span><span class="sxs-lookup"><span data-stu-id="3474b-104">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="3474b-105">Redis bir bellek içi anahtar-değer deposudur.</span><span class="sxs-lookup"><span data-stu-id="3474b-105">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="3474b-106">Bir ileti sistemi Yayımla ve abone modeli de destekler.</span><span class="sxs-lookup"><span data-stu-id="3474b-106">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="3474b-107">SignalR Redis devre kartı iletileri diğer sunuculara pub/alt özelliğini kullanır.</span><span class="sxs-lookup"><span data-stu-id="3474b-107">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="3474b-108">Bu öğretici için üç sunucu kullanır:</span><span class="sxs-lookup"><span data-stu-id="3474b-108">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="3474b-109">Bir SignalR uygulama dağıtmak için kullanacağınız Windows çalıştıran iki sunucu.</span><span class="sxs-lookup"><span data-stu-id="3474b-109">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="3474b-110">Redis çalıştırmak için kullanacağınız Linux çalıştıran bir sunucu.</span><span class="sxs-lookup"><span data-stu-id="3474b-110">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="3474b-111">Bu öğreticide ekran ı Ubuntu 12.04 TLS kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3474b-111">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="3474b-112">Üç fiziksel sunucuları kullanmak üzere yoksa, Hyper-V sanal makineleri oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3474b-112">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="3474b-113">Başka bir seçenek, Azure üzerinde VM'ler oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="3474b-113">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="3474b-114">Bu öğretici resmi Redis uygulama kullansa da, ayrıca vardır bir [, Windows bağlantı noktası Redis](https://github.com/MSOpenTech/redis) MSOpenTech gelen.</span><span class="sxs-lookup"><span data-stu-id="3474b-114">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="3474b-115">Kurulum ve yapılandırma farklı ancak Aksi halde adımları aynıdır.</span><span class="sxs-lookup"><span data-stu-id="3474b-115">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="3474b-116">SignalR genişletme ile Redis, Redis kümeleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="3474b-116">SignalR scaleout with Redis does not support Redis clusters.</span></span>


## <a name="overview"></a><span data-ttu-id="3474b-117">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="3474b-117">Overview</span></span>

<span data-ttu-id="3474b-118">Biz ayrıntılı öğretici ulaşmadan ne yapacağını Hızlı Bakış aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="3474b-118">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="3474b-119">Redis yükleyin ve Redis sunucu başlatın.</span><span class="sxs-lookup"><span data-stu-id="3474b-119">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="3474b-120">Bu NuGet paketleri uygulamanıza ekleyin:</span><span class="sxs-lookup"><span data-stu-id="3474b-120">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="3474b-121">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="3474b-121">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="3474b-122">Microsoft.AspNet.SignalR.Redis</span><span class="sxs-lookup"><span data-stu-id="3474b-122">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="3474b-123">Bir SignalR uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3474b-123">Create a SignalR application.</span></span>
4. <span data-ttu-id="3474b-124">Devre kartına yapılandırmak için Global.asax için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="3474b-124">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="3474b-125">Ubuntu on Hyper-V</span><span class="sxs-lookup"><span data-stu-id="3474b-125">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="3474b-126">Windows Hyper-V kullanarak, Windows Server'da bir Ubuntu VM kolayca oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3474b-126">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="3474b-127">Ubuntu ISO'yu makineden karşıdan [http://www.ubuntu.com](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="3474b-127">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="3474b-128">Hyper-V, yeni bir VM'e ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3474b-128">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="3474b-129">İçinde **sanal Hard diske Bağlan** adım, select **bir sanal sabit disk oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="3474b-129">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="3474b-130">İçinde **yükleme seçenekleri** adım, select **görüntü dosyası (.iso)**, tıklatın **Gözat**ve Ubuntu yükleme ISO göz atın.</span><span class="sxs-lookup"><span data-stu-id="3474b-130">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="3474b-131">Redis yükleyin</span><span class="sxs-lookup"><span data-stu-id="3474b-131">Install Redis</span></span>

<span data-ttu-id="3474b-132">Bölümündeki adımları izleyin [http://redis.io/download](http://redis.io/download) indirmek ve Redis oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="3474b-132">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="3474b-133">Bu Redis ikili dosyaları derlemeler `src` dizin.</span><span class="sxs-lookup"><span data-stu-id="3474b-133">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="3474b-134">Varsayılan olarak, Redis bir parola gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="3474b-134">By default, Redis does not require a password.</span></span> <span data-ttu-id="3474b-135">Bir parola ayarlamak için düzenleme `redis.conf` kaynak kodun kök dizininde bulunan dosya.</span><span class="sxs-lookup"><span data-stu-id="3474b-135">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="3474b-136">(Düzenlemeden önce dosyayı yedek bir kopyasını olun!) Aşağıdaki yönergesi eklemek `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="3474b-136">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="3474b-137">Redis Sunucu Şimdi Başlat:</span><span class="sxs-lookup"><span data-stu-id="3474b-137">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="3474b-138">Redis varsayılan bağlantı noktası açık bağlantı 6379, üzerinde dinler.</span><span class="sxs-lookup"><span data-stu-id="3474b-138">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="3474b-139">(Yapılandırma dosyasındaki bağlantı noktası numarasını değiştirebilirsiniz.)</span><span class="sxs-lookup"><span data-stu-id="3474b-139">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="3474b-140">SignalR uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="3474b-140">Create the SignalR Application</span></span>

<span data-ttu-id="3474b-141">Bir SignalR uygulaması ya da bu öğreticileri izleyerek oluşturun:</span><span class="sxs-lookup"><span data-stu-id="3474b-141">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="3474b-142">SignalR ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="3474b-142">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="3474b-143">SignalR ve MVC 4 ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="3474b-143">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="3474b-144">Ardından, biz genişletme Redis ile desteklemek için Sohbet uygulaması değiştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="3474b-144">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="3474b-145">İlk olarak, SignalR.Redis NuGet paketini projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3474b-145">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="3474b-146">Visual Studio'da gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="3474b-146">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="3474b-147">Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="3474b-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="3474b-148">Ardından, Global.asax dosyası açın.</span><span class="sxs-lookup"><span data-stu-id="3474b-148">Next, open the Global.asax file.</span></span> <span data-ttu-id="3474b-149">Aşağıdaki kodu ekleyin **uygulama\_Başlat** yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3474b-149">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="3474b-150">"server" Redis çalıştıran sunucunun adıdır.</span><span class="sxs-lookup"><span data-stu-id="3474b-150">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="3474b-151">*bağlantı noktası* bağlantı noktası numarası</span><span class="sxs-lookup"><span data-stu-id="3474b-151">*port* is the port number</span></span>
- <span data-ttu-id="3474b-152">"parola" redis.conf dosyasında tanımlanan paroladır.</span><span class="sxs-lookup"><span data-stu-id="3474b-152">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="3474b-153">"AppName" herhangi bir dize değil.</span><span class="sxs-lookup"><span data-stu-id="3474b-153">"AppName" is any string.</span></span> <span data-ttu-id="3474b-154">SignalR bu ada sahip bir Redis pub/alt kanalı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3474b-154">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="3474b-155">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="3474b-155">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="3474b-156">Dağıtma ve uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="3474b-156">Deploy and Run the Application</span></span>

<span data-ttu-id="3474b-157">SignalR uygulamayı dağıtmak için Windows Server örneklerinizi hazırlayın.</span><span class="sxs-lookup"><span data-stu-id="3474b-157">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="3474b-158">IIS rolü ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3474b-158">Add the IIS role.</span></span> <span data-ttu-id="3474b-159">WebSocket Protokolü dahil olmak üzere, "Uygulama geliştirme" özellikleri bulunur.</span><span class="sxs-lookup"><span data-stu-id="3474b-159">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="3474b-160">Ayrıca Yönetim Hizmeti ("Yönetim Araçları" altında listelenen) içerir.</span><span class="sxs-lookup"><span data-stu-id="3474b-160">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="3474b-161">**Yükleme Web dağıtımı 3.0.**</span><span class="sxs-lookup"><span data-stu-id="3474b-161">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="3474b-162">IIS Yöneticisi'ni çalıştırdığınızda, Microsoft Web Platformu yüklemenizi ister veya yapabilecekleriniz [intstaller karşıdan](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="3474b-162">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="3474b-163">Platformu yükleyicisi, Web dağıtımı için arama ve Web dağıtımı 3.0 yükleyin</span><span class="sxs-lookup"><span data-stu-id="3474b-163">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="3474b-164">Web yönetimi hizmeti çalışıp çalışmadığını denetleyin.</span><span class="sxs-lookup"><span data-stu-id="3474b-164">Check that the Web Management Service is running.</span></span> <span data-ttu-id="3474b-165">Aksi durumda, hizmeti başlatın.</span><span class="sxs-lookup"><span data-stu-id="3474b-165">If not, start the service.</span></span> <span data-ttu-id="3474b-166">(Web yönetimi hizmeti Windows Hizmetleri listede görmüyorsanız, IIS rolü eklendiğinde yönetim Hizmeti'nin yüklü emin olun.)</span><span class="sxs-lookup"><span data-stu-id="3474b-166">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="3474b-167">Varsayılan olarak, Web yönetimi hizmeti 8172 numaralı TCP bağlantı noktasını dinler.</span><span class="sxs-lookup"><span data-stu-id="3474b-167">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="3474b-168">Windows Güvenlik Duvarı'nda 8172 numaralı bağlantı noktasındaki TCP trafiğine izin veren yeni bir gelen kuralı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3474b-168">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="3474b-169">Daha fazla bilgi için bkz: [güvenlik duvarı kurallarını yapılandırma](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="3474b-169">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="3474b-170">(Azure Vm'lerinde barındırıyorsanız, doğrudan Azure Portalı'nda bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3474b-170">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="3474b-171">Bkz: [nasıl bir sanal makineye uç noktaları ayarlamak için](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="3474b-171">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="3474b-172">Artık geliştirme makinenizden Visual Studio projesi sunucuya dağıtmaya hazır olursunuz.</span><span class="sxs-lookup"><span data-stu-id="3474b-172">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="3474b-173">Çözüm Gezgini'nde çözüme sağ tıklayın ve **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="3474b-173">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="3474b-174">Daha ayrıntılı web dağıtımı ile ilgili belgeler için bkz: [Visual Studio ve ASP.NET için Web dağıtımı içerik haritası](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="3474b-174">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="3474b-175">İki sunucu için uygulama dağıtıyorsanız, her örneği ayrı bir tarayıcı penceresinde açın ve her SignalR iletileri diğer aldığınız bakın.</span><span class="sxs-lookup"><span data-stu-id="3474b-175">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="3474b-176">(Elbette, bir üretim ortamında, iki sunucu bir yük dengeleyicinin arkasına sit.)</span><span class="sxs-lookup"><span data-stu-id="3474b-176">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="3474b-177">Redis için kullanabileceğiniz gönderilen iletileri görmek merak ediyorsanız **redis CLI** Redis ile yükleyen istemci.</span><span class="sxs-lookup"><span data-stu-id="3474b-177">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
