---
uid: signalr/overview/performance/scaleout-with-redis
title: Redis ile SignalR ölçeğini genişletme | Microsoft Docs
author: MikeWasson
description: Yazılım sürümleri, sürüm 2 önceki sürümleri bu konunun önceki sürümleri hakkında bilgi için bu konu Visual Studio 2013 .NET 4.5 SignalR kullanılan...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: ebb61e4296f78bcd74622b729a10d45b60ebb724
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912793"
---
<a name="signalr-scaleout-with-redis"></a><span data-ttu-id="5ad67-103">Redis ile SignalR ölçeğini genişletme</span><span class="sxs-lookup"><span data-stu-id="5ad67-103">SignalR Scaleout with Redis</span></span>
====================
<span data-ttu-id="5ad67-104">tarafından [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="5ad67-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="5ad67-105">Bu konu başlığında kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="5ad67-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="5ad67-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5ad67-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="5ad67-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="5ad67-107">.NET 4.5</span></span>
> - <span data-ttu-id="5ad67-108">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="5ad67-108">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="5ad67-109">Bu konunun önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="5ad67-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="5ad67-110">SignalR eski sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="5ad67-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="5ad67-111">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="5ad67-111">Questions and comments</span></span>
>
> <span data-ttu-id="5ad67-112">Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın.</span><span class="sxs-lookup"><span data-stu-id="5ad67-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="5ad67-113">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="5ad67-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="5ad67-114">Bu öğreticide kullanacağınız [Redis](http://redis.io/) iletileri iki ayrı IIS örneklerinde dağıtılan bir SignalR uygulamayı dağıtmak üzere.</span><span class="sxs-lookup"><span data-stu-id="5ad67-114">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="5ad67-115">Redis bellek içi anahtar-değer deposudur.</span><span class="sxs-lookup"><span data-stu-id="5ad67-115">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="5ad67-116">Ayrıca, bir Mesajlaşma sistemi ile bir yayımlama/abone olma modelini destekler.</span><span class="sxs-lookup"><span data-stu-id="5ad67-116">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="5ad67-117">SignalR Redis devre kartına ileti başka bir sunucuya iletmek için pub/sub özelliğini kullanır.</span><span class="sxs-lookup"><span data-stu-id="5ad67-117">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="5ad67-118">Bu öğretici için üç sunucu kullanır:</span><span class="sxs-lookup"><span data-stu-id="5ad67-118">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="5ad67-119">Bir SignalR uygulamayı dağıtmak için kullanacağınız Windows çalıştıran iki sunucu.</span><span class="sxs-lookup"><span data-stu-id="5ad67-119">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="5ad67-120">Redis çalışmaya kullanacağı Linux çalıştıran bir sunucu.</span><span class="sxs-lookup"><span data-stu-id="5ad67-120">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="5ad67-121">Bu öğreticideki ekran görüntüleri için Ubuntu 12.04 TLS kullandım.</span><span class="sxs-lookup"><span data-stu-id="5ad67-121">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="5ad67-122">Kullanmak için üç fiziksel sunucu yoksa, Hyper-V VM'ler oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5ad67-122">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="5ad67-123">Azure'da VM'ler oluşturmanızı başka bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="5ad67-123">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="5ad67-124">Bu öğreticide resmi Redis uygulama kullansa da, de mevcuttur bir [, Windows bağlantı noktası Redis](https://github.com/MSOpenTech/redis) MSOpenTech öğesinden.</span><span class="sxs-lookup"><span data-stu-id="5ad67-124">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="5ad67-125">Kurulum ve yapılandırma farklıdır, ancak Aksi halde adımlar aynıdır.</span><span class="sxs-lookup"><span data-stu-id="5ad67-125">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE]
>
> <span data-ttu-id="5ad67-126">Redis ile SignalR ölçeğini genişletme için Redis kümelerini desteklemez.</span><span class="sxs-lookup"><span data-stu-id="5ad67-126">SignalR scaleout with Redis does not support Redis clusters.</span></span>


## <a name="overview"></a><span data-ttu-id="5ad67-127">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="5ad67-127">Overview</span></span>

<span data-ttu-id="5ad67-128">Ayrıntılı Öğreticisine aldığımız önce ne yapacağını, hızlı bir genel bakış aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="5ad67-128">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="5ad67-129">Redis yükleyin ve Redis sunucuyu başlatın.</span><span class="sxs-lookup"><span data-stu-id="5ad67-129">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="5ad67-130">Bu NuGet paketlerini uygulamanıza ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5ad67-130">Add these NuGet packages to your application:</span></span>

    - [<span data-ttu-id="5ad67-131">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="5ad67-131">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="5ad67-132">Microsoft.AspNet.SignalR.Redis</span><span class="sxs-lookup"><span data-stu-id="5ad67-132">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="5ad67-133">Bir SignalR uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5ad67-133">Create a SignalR application.</span></span>
4. <span data-ttu-id="5ad67-134">Devre kartına yapılandırmak için Startup.cs için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5ad67-134">Add the following code to Startup.cs to configure the backplane:</span></span>

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="5ad67-135">Ubuntu üzerinde Hyper-V</span><span class="sxs-lookup"><span data-stu-id="5ad67-135">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="5ad67-136">Windows Hyper-V kullanarak, Windows Server üzerinde bir Ubuntu VM kolayca oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5ad67-136">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="5ad67-137">Ubuntu ISO'dan indirme [ http://www.ubuntu.com ](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="5ad67-137">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="5ad67-138">Hyper-V, yeni bir sanal makine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5ad67-138">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="5ad67-139">İçinde **Sanal Sabit Disk Bağla** adım, select **sanal sabit disk oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="5ad67-139">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="5ad67-140">İçinde **yükleme seçenekleri** adım seçin **görüntü dosyası (.iso)**, tıklayın **Gözat**, Ubuntu yükleme ISO göz atın.</span><span class="sxs-lookup"><span data-stu-id="5ad67-140">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="5ad67-141">Redis'i yükleme</span><span class="sxs-lookup"><span data-stu-id="5ad67-141">Install Redis</span></span>

<span data-ttu-id="5ad67-142">Bölümündeki adımları [ http://redis.io/download ](http://redis.io/download) indirip Redis oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5ad67-142">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="5ad67-143">Bu Redis ikili değerlerini oluşturur `src` dizin.</span><span class="sxs-lookup"><span data-stu-id="5ad67-143">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="5ad67-144">Varsayılan olarak, Redis, parola gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="5ad67-144">By default, Redis does not require a password.</span></span> <span data-ttu-id="5ad67-145">Bir parola ayarlamak için Düzenle `redis.conf` kaynak kodunun kök dizininde bulunan dosya.</span><span class="sxs-lookup"><span data-stu-id="5ad67-145">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="5ad67-146">(Düzenlemeden önce dosyasının yedek bir kopyasını olun!) Eklemek için aşağıdaki yönerge `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="5ad67-146">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="5ad67-147">Redis Sunucu Şimdi Başlat:</span><span class="sxs-lookup"><span data-stu-id="5ad67-147">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="5ad67-148">Redis varsayılan bağlantı noktası olan açık 6379 bağlantı noktasının, üzerinde dinler.</span><span class="sxs-lookup"><span data-stu-id="5ad67-148">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="5ad67-149">(Yapılandırma dosyasındaki bağlantı noktası numarasını değiştirebilirsiniz.)</span><span class="sxs-lookup"><span data-stu-id="5ad67-149">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="5ad67-150">SignalR uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="5ad67-150">Create the SignalR Application</span></span>

<span data-ttu-id="5ad67-151">Bir SignalR uygulaması ya da bu öğreticileri izleyerek oluşturun:</span><span class="sxs-lookup"><span data-stu-id="5ad67-151">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="5ad67-152">SignalR 2.0 ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="5ad67-152">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="5ad67-153">SignalR 2.0 ve MVC 5 kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="5ad67-153">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="5ad67-154">Ardından, biz sohbet uygulaması, Redis ile ölçeğini genişletme destekleyecek şekilde değiştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="5ad67-154">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="5ad67-155">İlk olarak, SignalR.Redis NuGet paketini projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5ad67-155">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="5ad67-156">Visual Studio'da gelen **Araçları** menüsünde **NuGet Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="5ad67-156">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="5ad67-157">Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="5ad67-157">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="5ad67-158">Ardından, Startup.cs dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="5ad67-158">Next, open the Startup.cs file.</span></span> <span data-ttu-id="5ad67-159">Aşağıdaki kodu ekleyin **yapılandırma** yöntemi:</span><span class="sxs-lookup"><span data-stu-id="5ad67-159">Add the following code to the **Configuration** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="5ad67-160">"server" Redis'ı çalıştıran sunucunun adıdır.</span><span class="sxs-lookup"><span data-stu-id="5ad67-160">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="5ad67-161">*bağlantı noktası* bağlantı noktası numarası</span><span class="sxs-lookup"><span data-stu-id="5ad67-161">*port* is the port number</span></span>
- <span data-ttu-id="5ad67-162">"password" redis.conf dosyasında tanımlanan bir paroladır.</span><span class="sxs-lookup"><span data-stu-id="5ad67-162">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="5ad67-163">"AppName" herhangi bir dizedir.</span><span class="sxs-lookup"><span data-stu-id="5ad67-163">"AppName" is any string.</span></span> <span data-ttu-id="5ad67-164">SignalR bu ada sahip bir Redis pub/sub kanalı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5ad67-164">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="5ad67-165">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5ad67-165">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="5ad67-166">Dağıtma ve uygulama çalıştırma</span><span class="sxs-lookup"><span data-stu-id="5ad67-166">Deploy and Run the Application</span></span>

<span data-ttu-id="5ad67-167">SignalR uygulamayı dağıtmak için Windows Server örneklerinizin hazırlayın.</span><span class="sxs-lookup"><span data-stu-id="5ad67-167">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="5ad67-168">IIS rolünü ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5ad67-168">Add the IIS role.</span></span> <span data-ttu-id="5ad67-169">WebSocket Protokolü dahil olmak üzere "Uygulama geliştirme" özellikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="5ad67-169">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="5ad67-170">Yönetim Hizmeti ("Yönetim Araçları" altında listelenen) de içerir.</span><span class="sxs-lookup"><span data-stu-id="5ad67-170">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="5ad67-171">**Yükleme Web dağıtımı 3.0.**</span><span class="sxs-lookup"><span data-stu-id="5ad67-171">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="5ad67-172">IIS Yöneticisi'ni çalıştırın, Microsoft Web Platformu'nu yüklemenizi ister veya yapabilecekleriniz [intstaller indirme](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="5ad67-172">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="5ad67-173">Platform Yükleyicisi'nde, Web dağıtımı için arama ve Web dağıtımı 3.0 yükleyin</span><span class="sxs-lookup"><span data-stu-id="5ad67-173">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="5ad67-174">Web yönetimi hizmeti çalışıp çalışmadığını denetleyin.</span><span class="sxs-lookup"><span data-stu-id="5ad67-174">Check that the Web Management Service is running.</span></span> <span data-ttu-id="5ad67-175">Aksi durumda, hizmeti başlatın.</span><span class="sxs-lookup"><span data-stu-id="5ad67-175">If not, start the service.</span></span> <span data-ttu-id="5ad67-176">(Web yönetimi hizmeti Windows Hizmetleri listede görmüyorsanız, IIS rolü eklendiğinde yönetim Hizmeti'nin yüklü emin olun.)</span><span class="sxs-lookup"><span data-stu-id="5ad67-176">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="5ad67-177">Varsayılan olarak, Web yönetimi hizmeti 8172 numaralı TCP bağlantı noktasını dinler.</span><span class="sxs-lookup"><span data-stu-id="5ad67-177">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="5ad67-178">Windows Güvenlik Duvarı'nda 8172 numaralı bağlantı noktasındaki TCP trafiğine izin veren yeni bir gelen kuralı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5ad67-178">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="5ad67-179">Daha fazla bilgi için [güvenlik duvarı kurallarını yapılandırma](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="5ad67-179">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="5ad67-180">(Azure Vm'lerinde barındırıyorsanız, doğrudan Azure Portalı'nda bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5ad67-180">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="5ad67-181">Bkz: [bir sanal makineye uç noktaları ayarlama](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="5ad67-181">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="5ad67-182">Geliştirme makinenizde Visual Studio projesinden sunucusuna dağıtmak hazırsınız.</span><span class="sxs-lookup"><span data-stu-id="5ad67-182">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="5ad67-183">Çözüm Gezgini'nde çözüme sağ tıklayın ve **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="5ad67-183">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="5ad67-184">Belgeler web dağıtımı hakkında daha fazla ayrıntılı için bkz [Visual Studio ve ASP.NET için Web dağıtımı içerik haritası](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="5ad67-184">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="5ad67-185">İki sunucu için uygulama dağıtıyorsanız, her örnek bir ayrı bir tarayıcı penceresi açın ve her SignalR iletileri diğer aldığınız bakın.</span><span class="sxs-lookup"><span data-stu-id="5ad67-185">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="5ad67-186">(Kuşkusuz, bir üretim ortamında, iki sunucu bir yük dengeleyicinin arkasına sit.)</span><span class="sxs-lookup"><span data-stu-id="5ad67-186">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="5ad67-187">Redis için kullanabileceğiniz gönderilen iletileri görmek merak ediyorsanız **redis-cli** istemcisi, Redis ile yükler.</span><span class="sxs-lookup"><span data-stu-id="5ad67-187">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
