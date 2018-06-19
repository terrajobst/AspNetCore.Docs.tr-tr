---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: SQL Server ile SignalR genişletme (SignalR 1.x) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 7a589f5c6a5196444c3c616b39f501f3a7512471
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26565989"
---
<a name="signalr-scaleout-with-sql-server-signalr-1x"></a><span data-ttu-id="96249-102">SQL Server ile SignalR genişletme (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="96249-102">SignalR Scaleout with SQL Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="96249-103">tarafından [CAN Wasson](https://github.com/MikeWasson), [CAN Fletcher'dan](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="96249-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="96249-104">Bu öğreticide, iki ayrı IIS durumlarda dağıtılan bir SignalR uygulaması üzerinden iletilerini dağıtmak için SQL Server kullanır.</span><span class="sxs-lookup"><span data-stu-id="96249-104">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="96249-105">Bu öğretici bir tek test makinesi üzerinde de çalıştırabilirsiniz, ancak tam etkisi almak için iki veya daha fazla sunucu SignalR uygulamayı dağıtmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="96249-105">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="96249-106">SQL Server sunuculardan birinde ya da ayrılmış ayrı bir sunucuya yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="96249-106">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="96249-107">Sanal makineleri Azure üzerinde kullanarak öğreticisini çalıştırma başka bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="96249-107">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="96249-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="96249-108">Prerequisites</span></span>

<span data-ttu-id="96249-109">Microsoft SQL Server 2005 veya üzeri.</span><span class="sxs-lookup"><span data-stu-id="96249-109">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="96249-110">Devre kartına SQL Server'ın masaüstü ve sunucu sürümleri destekler.</span><span class="sxs-lookup"><span data-stu-id="96249-110">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="96249-111">SQL Server Compact Edition veya Azure SQL Database desteklemez.</span><span class="sxs-lookup"><span data-stu-id="96249-111">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="96249-112">(Uygulamanızın Azure üzerinde barındırılan, bunun yerine hizmet veri yolu devre kartı düşünün.)</span><span class="sxs-lookup"><span data-stu-id="96249-112">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="96249-113">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="96249-113">Overview</span></span>

<span data-ttu-id="96249-114">Biz ayrıntılı öğretici ulaşmadan ne yapacağını Hızlı Bakış aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="96249-114">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="96249-115">Yeni bir boş veritabanı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="96249-115">Create a new empty database.</span></span> <span data-ttu-id="96249-116">Devre kartına bu veritabanında gerekli tabloları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="96249-116">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="96249-117">Bu NuGet paketleri uygulamanıza ekleyin:</span><span class="sxs-lookup"><span data-stu-id="96249-117">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="96249-118">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="96249-118">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="96249-119">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="96249-119">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="96249-120">Bir SignalR uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="96249-120">Create a SignalR application.</span></span>
4. <span data-ttu-id="96249-121">Devre kartına yapılandırmak için Global.asax için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="96249-121">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a><span data-ttu-id="96249-122">Veritabanını yapılandırın</span><span class="sxs-lookup"><span data-stu-id="96249-122">Configure the Database</span></span>

<span data-ttu-id="96249-123">Uygulamayı Windows kimlik doğrulaması veya SQL Server kimlik doğrulaması veritabanına erişmek için kullanıp kullanmayacağını karar verin.</span><span class="sxs-lookup"><span data-stu-id="96249-123">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="96249-124">Ne olursa olsun, veritabanı kullanıcı oturum açma, şemaları oluşturma ve tabloları oluşturma izni olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="96249-124">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="96249-125">Kullanılacak devre kartı için yeni bir veritabanı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="96249-125">Create a new database for the backplane to use.</span></span> <span data-ttu-id="96249-126">Veritabanı adı verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="96249-126">You can give the database any name.</span></span> <span data-ttu-id="96249-127">Tüm tabloları veritabanında oluşturmanız gerekmez; devre kartına gerekli tabloları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="96249-127">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="96249-128">Hizmet Aracısı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="96249-128">Enable Service Broker</span></span>

<span data-ttu-id="96249-129">Hizmet Aracısı devre kartı veritabanı için etkinleştirilmesi önerilir.</span><span class="sxs-lookup"><span data-stu-id="96249-129">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="96249-130">Hizmet Aracısı ileti ve daha verimli bir şekilde güncelleştirmelerini devre kartı sağlayan, SQL Server'da sıraya alma için yerel destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="96249-130">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="96249-131">(Ancak devre kartı Ayrıca hizmet aracısı çalışır.)</span><span class="sxs-lookup"><span data-stu-id="96249-131">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="96249-132">Service Broker etkin olup olmadığını denetlemek için sorgu **olan\_Aracısı\_etkin** sütununda **sys.databases** Katalog görünümü.</span><span class="sxs-lookup"><span data-stu-id="96249-132">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="96249-133">Hizmet Aracısı etkinleştirmek için aşağıdaki SQL sorgusunu kullanın:</span><span class="sxs-lookup"><span data-stu-id="96249-133">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="96249-134">Bu sorgu görünürse kilitlenme, emin olmak için Veritabanına bağlı uygulama yok.</span><span class="sxs-lookup"><span data-stu-id="96249-134">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>


<span data-ttu-id="96249-135">İzleme etkinleştirilirse, izlemeleri Ayrıca hizmet Aracısı etkin olup olmadığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="96249-135">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="96249-136">Bir SignalR uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="96249-136">Create a SignalR Application</span></span>

<span data-ttu-id="96249-137">Bir SignalR uygulaması ya da bu öğreticileri izleyerek oluşturun:</span><span class="sxs-lookup"><span data-stu-id="96249-137">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="96249-138">SignalR ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="96249-138">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="96249-139">SignalR ve MVC 4 ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="96249-139">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="96249-140">Ardından, biz SQL Server ile genişletme desteklemek için Sohbet uygulaması değiştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="96249-140">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="96249-141">İlk olarak, SignalR.SqlServer NuGet paketini projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="96249-141">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="96249-142">Visual Studio'da gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="96249-142">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="96249-143">Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="96249-143">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="96249-144">Ardından, Global.asax dosyası açın.</span><span class="sxs-lookup"><span data-stu-id="96249-144">Next, open the Global.asax file.</span></span> <span data-ttu-id="96249-145">Aşağıdaki kodu ekleyin **uygulama\_Başlat** yöntemi:</span><span class="sxs-lookup"><span data-stu-id="96249-145">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="96249-146">Dağıtma ve uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="96249-146">Deploy and Run the Application</span></span>

<span data-ttu-id="96249-147">SignalR uygulamayı dağıtmak için Windows Server örneklerinizi hazırlayın.</span><span class="sxs-lookup"><span data-stu-id="96249-147">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="96249-148">IIS rolü ekleyin.</span><span class="sxs-lookup"><span data-stu-id="96249-148">Add the IIS role.</span></span> <span data-ttu-id="96249-149">WebSocket Protokolü dahil olmak üzere, "Uygulama geliştirme" özellikleri bulunur.</span><span class="sxs-lookup"><span data-stu-id="96249-149">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="96249-150">Ayrıca Yönetim Hizmeti ("Yönetim Araçları" altında listelenen) içerir.</span><span class="sxs-lookup"><span data-stu-id="96249-150">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="96249-151">**Yükleme Web dağıtımı 3.0.**</span><span class="sxs-lookup"><span data-stu-id="96249-151">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="96249-152">IIS Yöneticisi'ni çalıştırdığınızda, Microsoft Web Platformu yüklemenizi ister veya yapabilecekleriniz [intstaller karşıdan](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="96249-152">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="96249-153">Platformu yükleyicisi, Web dağıtımı için arama ve Web dağıtımı 3.0 yükleyin</span><span class="sxs-lookup"><span data-stu-id="96249-153">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="96249-154">Web yönetimi hizmeti çalışıp çalışmadığını denetleyin.</span><span class="sxs-lookup"><span data-stu-id="96249-154">Check that the Web Management Service is running.</span></span> <span data-ttu-id="96249-155">Aksi durumda, hizmeti başlatın.</span><span class="sxs-lookup"><span data-stu-id="96249-155">If not, start the service.</span></span> <span data-ttu-id="96249-156">(Web yönetimi hizmeti Windows Hizmetleri listede görmüyorsanız, IIS rolü eklendiğinde yönetim Hizmeti'nin yüklü emin olun.)</span><span class="sxs-lookup"><span data-stu-id="96249-156">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="96249-157">Son olarak, TCP bağlantı noktası 8172 açın.</span><span class="sxs-lookup"><span data-stu-id="96249-157">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="96249-158">Bu Web Dağıtım Aracı'nı kullanır bağlantı noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="96249-158">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="96249-159">Artık geliştirme makinenizden Visual Studio projesi sunucuya dağıtmaya hazır olursunuz.</span><span class="sxs-lookup"><span data-stu-id="96249-159">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="96249-160">Çözüm Gezgini'nde çözüme sağ tıklayın ve **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="96249-160">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="96249-161">Daha ayrıntılı web dağıtımı ile ilgili belgeler için bkz: [Visual Studio ve ASP.NET için Web dağıtımı içerik haritası](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="96249-161">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="96249-162">İki sunucu için uygulama dağıtıyorsanız, her örneği ayrı bir tarayıcı penceresinde açın ve her SignalR iletileri diğer aldığınız bakın.</span><span class="sxs-lookup"><span data-stu-id="96249-162">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="96249-163">(Elbette, bir üretim ortamında, iki sunucu bir yük dengeleyicinin arkasına sit.)</span><span class="sxs-lookup"><span data-stu-id="96249-163">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="96249-164">Uygulamayı çalıştırdıktan sonra veritabanında tablolarda, SignalR otomatik olarak oluşturdu görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="96249-164">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="96249-165">SignalR tabloları yönetir.</span><span class="sxs-lookup"><span data-stu-id="96249-165">SignalR manages the tables.</span></span> <span data-ttu-id="96249-166">Uygulamanızı dağıttınız sürece yok satırları silmek, tabloyu değiştirerek ve benzeri.</span><span class="sxs-lookup"><span data-stu-id="96249-166">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
