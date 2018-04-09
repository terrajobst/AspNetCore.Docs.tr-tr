---
uid: signalr/overview/performance/scaleout-with-sql-server
title: SQL Server ile SignalR genişletme | Microsoft Docs
author: MikeWasson
description: Yazılım sürümleri bu konuda Visual Studio 2013 .NET 4.5 SignalR önceki sürümleri hakkında bilgi için bu konuda sürüm 2 önceki sürümlerinde kullanılan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: b3189c36fc076333c0c6007bd039b12e03d63bc8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="signalr-scaleout-with-sql-server"></a><span data-ttu-id="8c093-103">SQL Server ile SignalR genişletme</span><span class="sxs-lookup"><span data-stu-id="8c093-103">SignalR Scaleout with SQL Server</span></span>
====================
<span data-ttu-id="8c093-104">tarafından [CAN Wasson](https://github.com/MikeWasson), [CAN Fletcher'dan](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="8c093-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="8c093-105">Bu konuda kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="8c093-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="8c093-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="8c093-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="8c093-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="8c093-107">.NET 4.5</span></span>
> - <span data-ttu-id="8c093-108">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="8c093-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="8c093-109">Bu konunun önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="8c093-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="8c093-110">SignalR daha önceki sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="8c093-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="8c093-111">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="8c093-111">Questions and comments</span></span>
> 
> <span data-ttu-id="8c093-112">Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi.</span><span class="sxs-lookup"><span data-stu-id="8c093-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="8c093-113">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="8c093-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="8c093-114">Bu öğreticide, iki ayrı IIS durumlarda dağıtılan bir SignalR uygulaması üzerinden iletilerini dağıtmak için SQL Server kullanır.</span><span class="sxs-lookup"><span data-stu-id="8c093-114">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="8c093-115">Bu öğretici bir tek test makinesi üzerinde de çalıştırabilirsiniz, ancak tam etkisi almak için iki veya daha fazla sunucu SignalR uygulamayı dağıtmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="8c093-115">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="8c093-116">SQL Server sunuculardan birinde ya da ayrılmış ayrı bir sunucuya yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="8c093-116">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="8c093-117">Sanal makineleri Azure üzerinde kullanarak öğreticisini çalıştırma başka bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="8c093-117">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="8c093-118">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="8c093-118">Prerequisites</span></span>

<span data-ttu-id="8c093-119">Microsoft SQL Server 2005 veya üzeri.</span><span class="sxs-lookup"><span data-stu-id="8c093-119">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="8c093-120">Devre kartına SQL Server'ın masaüstü ve sunucu sürümleri destekler.</span><span class="sxs-lookup"><span data-stu-id="8c093-120">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="8c093-121">SQL Server Compact Edition veya Azure SQL Database desteklemez.</span><span class="sxs-lookup"><span data-stu-id="8c093-121">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="8c093-122">(Uygulamanızın Azure üzerinde barındırılan, bunun yerine hizmet veri yolu devre kartı düşünün.)</span><span class="sxs-lookup"><span data-stu-id="8c093-122">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="8c093-123">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="8c093-123">Overview</span></span>

<span data-ttu-id="8c093-124">Biz ayrıntılı öğretici ulaşmadan ne yapacağını Hızlı Bakış aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="8c093-124">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="8c093-125">Yeni bir boş veritabanı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8c093-125">Create a new empty database.</span></span> <span data-ttu-id="8c093-126">Devre kartına bu veritabanında gerekli tabloları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8c093-126">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="8c093-127">Bu NuGet paketleri uygulamanıza ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8c093-127">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="8c093-128">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="8c093-128">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="8c093-129">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="8c093-129">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="8c093-130">Bir SignalR uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8c093-130">Create a SignalR application.</span></span>
4. <span data-ttu-id="8c093-131">Devre kartına yapılandırmak için haline için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8c093-131">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   <span data-ttu-id="8c093-132">Bu kod için varsayılan değerlerle devre kartı yapılandırır [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) ve [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="8c093-132">This code configures the backplane with the default values for [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="8c093-133">Bu değerleri değiştirme hakkında daha fazla bilgi için bkz: [SignalR performans: genişletme ölçümleri](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="8c093-133">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span> 

## <a name="configure-the-database"></a><span data-ttu-id="8c093-134">Veritabanını yapılandırın</span><span class="sxs-lookup"><span data-stu-id="8c093-134">Configure the Database</span></span>

<span data-ttu-id="8c093-135">Uygulamayı Windows kimlik doğrulaması veya SQL Server kimlik doğrulaması veritabanına erişmek için kullanıp kullanmayacağını karar verin.</span><span class="sxs-lookup"><span data-stu-id="8c093-135">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="8c093-136">Ne olursa olsun, veritabanı kullanıcı oturum açma, şemaları oluşturma ve tabloları oluşturma izni olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="8c093-136">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="8c093-137">Kullanılacak devre kartı için yeni bir veritabanı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8c093-137">Create a new database for the backplane to use.</span></span> <span data-ttu-id="8c093-138">Veritabanı adı verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8c093-138">You can give the database any name.</span></span> <span data-ttu-id="8c093-139">Tüm tabloları veritabanında oluşturmanız gerekmez; devre kartına gerekli tabloları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8c093-139">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="8c093-140">Hizmet Aracısı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="8c093-140">Enable Service Broker</span></span>

<span data-ttu-id="8c093-141">Hizmet Aracısı devre kartı veritabanı için etkinleştirilmesi önerilir.</span><span class="sxs-lookup"><span data-stu-id="8c093-141">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="8c093-142">Hizmet Aracısı ileti ve daha verimli bir şekilde güncelleştirmelerini devre kartı sağlayan, SQL Server'da sıraya alma için yerel destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="8c093-142">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="8c093-143">(Ancak devre kartı Ayrıca hizmet aracısı çalışır.)</span><span class="sxs-lookup"><span data-stu-id="8c093-143">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="8c093-144">Service Broker etkin olup olmadığını denetlemek için sorgu **olan\_Aracısı\_etkin** sütununda **sys.databases** Katalog görünümü.</span><span class="sxs-lookup"><span data-stu-id="8c093-144">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="8c093-145">Hizmet Aracısı etkinleştirmek için aşağıdaki SQL sorgusunu kullanın:</span><span class="sxs-lookup"><span data-stu-id="8c093-145">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="8c093-146">Bu sorgu görünürse kilitlenme, emin olmak için Veritabanına bağlı uygulama yok.</span><span class="sxs-lookup"><span data-stu-id="8c093-146">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>


<span data-ttu-id="8c093-147">İzleme etkinleştirilirse, izlemeleri Ayrıca hizmet Aracısı etkin olup olmadığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="8c093-147">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="8c093-148">Bir SignalR uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="8c093-148">Create a SignalR Application</span></span>

<span data-ttu-id="8c093-149">Bir SignalR uygulaması ya da bu öğreticileri izleyerek oluşturun:</span><span class="sxs-lookup"><span data-stu-id="8c093-149">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="8c093-150">SignalR 2.0 ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="8c093-150">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="8c093-151">SignalR 2.0 ve MVC 5 ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="8c093-151">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="8c093-152">Ardından, biz SQL Server ile genişletme desteklemek için Sohbet uygulaması değiştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="8c093-152">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="8c093-153">İlk olarak, SignalR.SqlServer NuGet paketini projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8c093-153">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="8c093-154">Visual Studio'da gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="8c093-154">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="8c093-155">Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="8c093-155">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="8c093-156">Ardından, haline dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="8c093-156">Next, open the Startup.cs file.</span></span> <span data-ttu-id="8c093-157">Aşağıdaki kodu ekleyin **yapılandırma** yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8c093-157">Add the following code to the **Configure** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="8c093-158">Dağıtma ve uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="8c093-158">Deploy and Run the Application</span></span>

<span data-ttu-id="8c093-159">SignalR uygulamayı dağıtmak için Windows Server örneklerinizi hazırlayın.</span><span class="sxs-lookup"><span data-stu-id="8c093-159">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="8c093-160">IIS rolü ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8c093-160">Add the IIS role.</span></span> <span data-ttu-id="8c093-161">WebSocket Protokolü dahil olmak üzere, "Uygulama geliştirme" özellikleri bulunur.</span><span class="sxs-lookup"><span data-stu-id="8c093-161">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="8c093-162">Ayrıca Yönetim Hizmeti ("Yönetim Araçları" altında listelenen) içerir.</span><span class="sxs-lookup"><span data-stu-id="8c093-162">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="8c093-163">**Yükleme Web dağıtımı 3.0.**</span><span class="sxs-lookup"><span data-stu-id="8c093-163">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="8c093-164">IIS Yöneticisi'ni çalıştırdığınızda, Microsoft Web Platformu yüklemenizi ister veya yapabilecekleriniz [intstaller karşıdan](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="8c093-164">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="8c093-165">Platformu yükleyicisi, Web dağıtımı için arama ve Web dağıtımı 3.0 yükleyin</span><span class="sxs-lookup"><span data-stu-id="8c093-165">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="8c093-166">Web yönetimi hizmeti çalışıp çalışmadığını denetleyin.</span><span class="sxs-lookup"><span data-stu-id="8c093-166">Check that the Web Management Service is running.</span></span> <span data-ttu-id="8c093-167">Aksi durumda, hizmeti başlatın.</span><span class="sxs-lookup"><span data-stu-id="8c093-167">If not, start the service.</span></span> <span data-ttu-id="8c093-168">(Web yönetimi hizmeti Windows Hizmetleri listede görmüyorsanız, IIS rolü eklendiğinde yönetim Hizmeti'nin yüklü emin olun.)</span><span class="sxs-lookup"><span data-stu-id="8c093-168">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="8c093-169">Son olarak, TCP bağlantı noktası 8172 açın.</span><span class="sxs-lookup"><span data-stu-id="8c093-169">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="8c093-170">Bu Web Dağıtım Aracı'nı kullanır bağlantı noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="8c093-170">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="8c093-171">Artık geliştirme makinenizden Visual Studio projesi sunucuya dağıtmaya hazır olursunuz.</span><span class="sxs-lookup"><span data-stu-id="8c093-171">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="8c093-172">Çözüm Gezgini'nde çözüme sağ tıklayın ve **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="8c093-172">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="8c093-173">Daha ayrıntılı web dağıtımı ile ilgili belgeler için bkz: [Visual Studio ve ASP.NET için Web dağıtımı içerik haritası](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="8c093-173">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="8c093-174">İki sunucu için uygulama dağıtıyorsanız, her örneği ayrı bir tarayıcı penceresinde açın ve her SignalR iletileri diğer aldığınız bakın.</span><span class="sxs-lookup"><span data-stu-id="8c093-174">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="8c093-175">(Elbette, bir üretim ortamında, iki sunucu bir yük dengeleyicinin arkasına sit.)</span><span class="sxs-lookup"><span data-stu-id="8c093-175">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="8c093-176">Uygulamayı çalıştırdıktan sonra veritabanında tablolarda, SignalR otomatik olarak oluşturdu görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="8c093-176">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="8c093-177">SignalR tabloları yönetir.</span><span class="sxs-lookup"><span data-stu-id="8c093-177">SignalR manages the tables.</span></span> <span data-ttu-id="8c093-178">Uygulamanızı dağıttınız sürece yok satırları silmek, tabloyu değiştirerek ve benzeri.</span><span class="sxs-lookup"><span data-stu-id="8c093-178">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
