---
uid: signalr/overview/performance/scaleout-with-redis
title: "SignalR genişletme Redis ile | Microsoft Docs"
author: MikeWasson
description: "Yazılım sürümleri bu konuda Visual Studio 2013 .NET 4.5 SignalR önceki sürümleri hakkında bilgi için bu konuda sürüm 2 önceki sürümlerinde kullanılan..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 965c32a4e2f2c9c4bd457d0c13ae99c1378c22c9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-redis"></a><span data-ttu-id="a6eeb-103">Redis ile SignalR genişletme</span><span class="sxs-lookup"><span data-stu-id="a6eeb-103">SignalR Scaleout with Redis</span></span>
====================
<span data-ttu-id="a6eeb-104">tarafından [CAN Wasson](https://github.com/MikeWasson), [CAN Fletcher'dan](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="a6eeb-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="a6eeb-105">Bu konuda kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="a6eeb-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="a6eeb-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="a6eeb-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="a6eeb-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="a6eeb-107">.NET 4.5</span></span>
> - <span data-ttu-id="a6eeb-108">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="a6eeb-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="a6eeb-109">Bu konunun önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="a6eeb-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="a6eeb-110">SignalR daha önceki sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="a6eeb-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="a6eeb-111">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="a6eeb-111">Questions and comments</span></span>
> 
> <span data-ttu-id="a6eeb-112">Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="a6eeb-113">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="a6eeb-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="a6eeb-114">Bu öğreticide, kullanacağınız [Redis](http://redis.io/) iki ayrı IIS örneklerinde dağıtılan bir SignalR uygulaması üzerinden iletilerini dağıtmak için.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-114">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="a6eeb-115">Redis bir bellek içi anahtar-değer deposudur.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-115">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="a6eeb-116">Bir ileti sistemi Yayımla ve abone modeli de destekler.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-116">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="a6eeb-117">SignalR Redis devre kartı iletileri diğer sunuculara pub/alt özelliğini kullanır.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-117">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="a6eeb-118">Bu öğretici için üç sunucu kullanır:</span><span class="sxs-lookup"><span data-stu-id="a6eeb-118">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="a6eeb-119">Bir SignalR uygulama dağıtmak için kullanacağınız Windows çalıştıran iki sunucu.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-119">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="a6eeb-120">Redis çalıştırmak için kullanacağınız Linux çalıştıran bir sunucu.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-120">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="a6eeb-121">Bu öğreticide ekran ı Ubuntu 12.04 TLS kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-121">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="a6eeb-122">Üç fiziksel sunucuları kullanmak üzere yoksa, Hyper-V sanal makineleri oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-122">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="a6eeb-123">Başka bir seçenek, Azure üzerinde VM'ler oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-123">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="a6eeb-124">Bu öğretici resmi Redis uygulama kullansa da, ayrıca vardır bir [, Windows bağlantı noktası Redis](https://github.com/MSOpenTech/redis) MSOpenTech gelen.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-124">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="a6eeb-125">Kurulum ve yapılandırma farklı ancak Aksi halde adımları aynıdır.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-125">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a6eeb-126">SignalR genişletme ile Redis, Redis kümeleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-126">SignalR scaleout with Redis does not support Redis clusters.</span></span>


## <a name="overview"></a><span data-ttu-id="a6eeb-127">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="a6eeb-127">Overview</span></span>

<span data-ttu-id="a6eeb-128">Biz ayrıntılı öğretici ulaşmadan ne yapacağını Hızlı Bakış aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-128">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="a6eeb-129">Redis yükleyin ve Redis sunucu başlatın.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-129">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="a6eeb-130">Bu NuGet paketleri uygulamanıza ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a6eeb-130">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="a6eeb-131">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="a6eeb-131">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="a6eeb-132">Microsoft.AspNet.SignalR.Redis</span><span class="sxs-lookup"><span data-stu-id="a6eeb-132">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="a6eeb-133">Bir SignalR uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-133">Create a SignalR application.</span></span>
4. <span data-ttu-id="a6eeb-134">Devre kartına yapılandırmak için haline için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a6eeb-134">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="a6eeb-135">Hyper-V'de ubuntu</span><span class="sxs-lookup"><span data-stu-id="a6eeb-135">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="a6eeb-136">Windows Hyper-V kullanarak, Windows Server'da bir Ubuntu VM kolayca oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-136">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="a6eeb-137">Ubuntu ISO'yu makineden karşıdan [http://www.ubuntu.com](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="a6eeb-137">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="a6eeb-138">Hyper-V, yeni bir VM'e ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-138">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="a6eeb-139">İçinde **sanal Hard diske Bağlan** adım, select **bir sanal sabit disk oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-139">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="a6eeb-140">İçinde **yükleme seçenekleri** adım, select **görüntü dosyası (.iso)**, tıklatın **Gözat**ve Ubuntu yükleme ISO göz atın.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-140">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="a6eeb-141">Redis yükleyin</span><span class="sxs-lookup"><span data-stu-id="a6eeb-141">Install Redis</span></span>

<span data-ttu-id="a6eeb-142">Bölümündeki adımları izleyin [http://redis.io/download](http://redis.io/download) indirmek ve Redis oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-142">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="a6eeb-143">Bu Redis ikili dosyaları derlemeler `src` dizin.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-143">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="a6eeb-144">Varsayılan olarak, Redis bir parola gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-144">By default, Redis does not require a password.</span></span> <span data-ttu-id="a6eeb-145">Bir parola ayarlamak için düzenleme `redis.conf` kaynak kodun kök dizininde bulunan dosya.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-145">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="a6eeb-146">(Düzenlemeden önce dosyayı yedek bir kopyasını olun!) Aşağıdaki yönergesi eklemek `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="a6eeb-146">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="a6eeb-147">Redis Sunucu Şimdi Başlat:</span><span class="sxs-lookup"><span data-stu-id="a6eeb-147">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="a6eeb-148">Redis varsayılan bağlantı noktası açık bağlantı 6379, üzerinde dinler.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-148">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="a6eeb-149">(Yapılandırma dosyasındaki bağlantı noktası numarasını değiştirebilirsiniz.)</span><span class="sxs-lookup"><span data-stu-id="a6eeb-149">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="a6eeb-150">SignalR uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="a6eeb-150">Create the SignalR Application</span></span>

<span data-ttu-id="a6eeb-151">Bir SignalR uygulaması ya da bu öğreticileri izleyerek oluşturun:</span><span class="sxs-lookup"><span data-stu-id="a6eeb-151">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="a6eeb-152">SignalR 2.0 ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="a6eeb-152">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="a6eeb-153">SignalR 2.0 ve MVC 5 ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="a6eeb-153">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="a6eeb-154">Ardından, biz genişletme Redis ile desteklemek için Sohbet uygulaması değiştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-154">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="a6eeb-155">İlk olarak, SignalR.Redis NuGet paketini projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-155">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="a6eeb-156">Visual Studio'da gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-156">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="a6eeb-157">Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="a6eeb-157">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="a6eeb-158">Ardından, haline dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-158">Next, open the Startup.cs file.</span></span> <span data-ttu-id="a6eeb-159">Aşağıdaki kodu ekleyin **yapılandırma** yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a6eeb-159">Add the following code to the **Configuration** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="a6eeb-160">"server" Redis çalıştıran sunucunun adıdır.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-160">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="a6eeb-161">*bağlantı noktası* bağlantı noktası numarası</span><span class="sxs-lookup"><span data-stu-id="a6eeb-161">*port* is the port number</span></span>
- <span data-ttu-id="a6eeb-162">"parola" redis.conf dosyasında tanımlanan paroladır.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-162">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="a6eeb-163">"AppName" herhangi bir dize değil.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-163">"AppName" is any string.</span></span> <span data-ttu-id="a6eeb-164">SignalR bu ada sahip bir Redis pub/alt kanalı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-164">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="a6eeb-165">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="a6eeb-165">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="a6eeb-166">Dağıtma ve uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="a6eeb-166">Deploy and Run the Application</span></span>

<span data-ttu-id="a6eeb-167">SignalR uygulamayı dağıtmak için Windows Server örneklerinizi hazırlayın.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-167">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="a6eeb-168">IIS rolü ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-168">Add the IIS role.</span></span> <span data-ttu-id="a6eeb-169">WebSocket Protokolü dahil olmak üzere, "Uygulama geliştirme" özellikleri bulunur.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-169">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="a6eeb-170">Ayrıca Yönetim Hizmeti ("Yönetim Araçları" altında listelenen) içerir.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-170">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="a6eeb-171">**Yükleme Web dağıtımı 3.0.**</span><span class="sxs-lookup"><span data-stu-id="a6eeb-171">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="a6eeb-172">IIS Yöneticisi'ni çalıştırdığınızda, Microsoft Web Platformu yüklemenizi ister veya yapabilecekleriniz [intstaller karşıdan](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="a6eeb-172">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="a6eeb-173">Platformu yükleyicisi, Web dağıtımı için arama ve Web dağıtımı 3.0 yükleyin</span><span class="sxs-lookup"><span data-stu-id="a6eeb-173">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="a6eeb-174">Web yönetimi hizmeti çalışıp çalışmadığını denetleyin.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-174">Check that the Web Management Service is running.</span></span> <span data-ttu-id="a6eeb-175">Aksi durumda, hizmeti başlatın.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-175">If not, start the service.</span></span> <span data-ttu-id="a6eeb-176">(Web yönetimi hizmeti Windows Hizmetleri listede görmüyorsanız, IIS rolü eklendiğinde yönetim Hizmeti'nin yüklü emin olun.)</span><span class="sxs-lookup"><span data-stu-id="a6eeb-176">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="a6eeb-177">Varsayılan olarak, Web yönetimi hizmeti 8172 numaralı TCP bağlantı noktasını dinler.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-177">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="a6eeb-178">Windows Güvenlik Duvarı'nda 8172 numaralı bağlantı noktasındaki TCP trafiğine izin veren yeni bir gelen kuralı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-178">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="a6eeb-179">Daha fazla bilgi için bkz: [güvenlik duvarı kurallarını yapılandırma](https://technet.microsoft.com/en-us/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="a6eeb-179">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/en-us/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="a6eeb-180">(Azure Vm'lerinde barındırıyorsanız, doğrudan Azure Portalı'nda bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-180">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="a6eeb-181">Bkz: [nasıl bir sanal makineye uç noktaları ayarlamak için](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="a6eeb-181">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="a6eeb-182">Artık geliştirme makinenizden Visual Studio projesi sunucuya dağıtmaya hazır olursunuz.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-182">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="a6eeb-183">Çözüm Gezgini'nde çözüme sağ tıklayın ve **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-183">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="a6eeb-184">Daha ayrıntılı web dağıtımı ile ilgili belgeler için bkz: [Visual Studio ve ASP.NET için Web dağıtımı içerik haritası](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="a6eeb-184">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="a6eeb-185">İki sunucu için uygulama dağıtıyorsanız, her örneği ayrı bir tarayıcı penceresinde açın ve her SignalR iletileri diğer aldığınız bakın.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-185">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="a6eeb-186">(Elbette, bir üretim ortamında, iki sunucu bir yük dengeleyicinin arkasına sit.)</span><span class="sxs-lookup"><span data-stu-id="a6eeb-186">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="a6eeb-187">Redis için kullanabileceğiniz gönderilen iletileri görmek merak ediyorsanız **redis CLI** Redis ile yükleyen istemci.</span><span class="sxs-lookup"><span data-stu-id="a6eeb-187">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
