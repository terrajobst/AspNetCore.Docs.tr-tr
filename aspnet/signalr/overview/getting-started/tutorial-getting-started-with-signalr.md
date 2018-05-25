---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Öğretici: 2 SignalR ile çalışmaya başlama | Microsoft Docs'
author: pfletcher
description: Bu öğreticide SignalR kullanarak gerçek zamanlı bir sohbet uygulaması oluşturma işlemi gösterilir. Boş bir ASP.NET web uygulamasına SignalR ekleyebilir ve bir HTML pa oluştur...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 8be851f5a2b1cca39f5f8f284ff1c002c486d7e8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-2"></a><span data-ttu-id="79ed2-104">Öğretici: 2 SignalR ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="79ed2-104">Tutorial: Getting Started with SignalR 2</span></span>
====================
<span data-ttu-id="79ed2-105">tarafından [CAN Fletcher'dan](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="79ed2-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="79ed2-106">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="79ed2-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> <span data-ttu-id="79ed2-107">Bu öğreticide SignalR kullanarak gerçek zamanlı bir sohbet uygulaması oluşturma işlemi gösterilir.</span><span class="sxs-lookup"><span data-stu-id="79ed2-107">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="79ed2-108">Boş bir ASP.NET web uygulamasına SignalR ekleyebilir ve göndermek ve iletileri görüntülemek için bir HTML sayfası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="79ed2-108">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span> 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="79ed2-109">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="79ed2-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="79ed2-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="79ed2-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="79ed2-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="79ed2-111">.NET 4.5</span></span>
> - <span data-ttu-id="79ed2-112">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="79ed2-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="79ed2-113">Bu öğretici ile Visual Studio 2012 kullanma</span><span class="sxs-lookup"><span data-stu-id="79ed2-113">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="79ed2-114">Visual Studio 2012 bu öğreticiyle kullanmak için aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="79ed2-114">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="79ed2-115">Güncelleştirme, [Paket Yöneticisi](http://docs.nuget.org/docs/start-here/installing-nuget) en son sürüme.</span><span class="sxs-lookup"><span data-stu-id="79ed2-115">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="79ed2-116">Yükleme [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="79ed2-116">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="79ed2-117">Web Platformu Yükleyicisi'nde arayın ve yükleyin **ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için**.</span><span class="sxs-lookup"><span data-stu-id="79ed2-117">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="79ed2-118">Bu SignalR sınıfları için Visual Studio şablonları gibi yükleyecek **Hub**.</span><span class="sxs-lookup"><span data-stu-id="79ed2-118">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="79ed2-119">Bazı şablonlar (gibi **OWIN başlangıç sınıfı**); kullanılamaz bunlar için bunun yerine bir sınıf dosyası kullanın.</span><span class="sxs-lookup"><span data-stu-id="79ed2-119">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="79ed2-120">Eğitmen sürümleri</span><span class="sxs-lookup"><span data-stu-id="79ed2-120">Tutorial versions</span></span>
> 
> <span data-ttu-id="79ed2-121">SignalR daha önceki sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="79ed2-121">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="79ed2-122">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="79ed2-122">Questions and comments</span></span>
> 
> <span data-ttu-id="79ed2-123">Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi.</span><span class="sxs-lookup"><span data-stu-id="79ed2-123">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="79ed2-124">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="79ed2-124">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="79ed2-125">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="79ed2-125">Overview</span></span>

<span data-ttu-id="79ed2-126">Bu öğretici, bir basit tarayıcı tabanlı sohbet uygulaması oluşturmak nasıl yapıldığını göstererek SignalR geliştirme sunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="79ed2-126">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="79ed2-127">Boş bir ASP.NET web uygulamasına SignalR Kitaplığı eklemek, istemcilere ileti göndermek için bir hub sınıf oluşturun ve sohbet ileti gönderme ve alma kullanıcıların olanak sağlayan bir HTML sayfası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="79ed2-127">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="79ed2-128">MVC 5'te bir sohbet uygulaması oluşturmak MVC görünümü kullanarak gösteren benzer bir öğretici için bkz: [SignalR 2 ve MVC 5 ile çalışmaya başlama](tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="79ed2-128">For a similar tutorial that shows how to create a chat application in MVC 5 using an MVC view, see [Getting Started with SignalR 2 and MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

> [!NOTE]
> <span data-ttu-id="79ed2-129">Bu öğretici SignalR uygulamalarının sürüm 2 nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="79ed2-129">This tutorial demonstrates how to create SignalR applications in version 2.</span></span> <span data-ttu-id="79ed2-130">SignalR arasındaki değişiklikleri hakkında ayrıntılı bilgi için bkz: 1.x ve 2, [yükseltme SignalR 1.x projeleri](../releases/upgrading-signalr-1x-projects-to-20.md) ve [Visual Studio 2013 sürüm notları](../../../visual-studio/overview/2013/release-notes.md#TOC13).</span><span class="sxs-lookup"><span data-stu-id="79ed2-130">For details on changes between SignalR 1.x and 2, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md) and [Visual Studio 2013 Release Notes](../../../visual-studio/overview/2013/release-notes.md#TOC13).</span></span>

<span data-ttu-id="79ed2-131">SignalR, Canlı kullanıcı etkileşimi veya gerçek zamanlı veri güncelleştirmeleri gerektiren web uygulamaları oluşturmak için bir açık kaynak .NET kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="79ed2-131">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="79ed2-132">Sosyal uygulamalar, çok kullanıcılı oyunlar, iş işbirliği ve haberler, hava durumu veya finansal güncelleştirme uygulamaları örnek olarak verilebilir.</span><span class="sxs-lookup"><span data-stu-id="79ed2-132">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="79ed2-133">Bunlar genellikle gerçek zamanlı uygulamaları olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="79ed2-133">These are often called real-time applications.</span></span>

<span data-ttu-id="79ed2-134">SignalR gerçek zamanlı uygulamaları oluşturma işlemini basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="79ed2-134">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="79ed2-135">Bir ASP.NET server kitaplığı ve istemci-sunucu bağlantıları yönetmek ve içerik güncelleştirmelerini istemcilere anında kolaylaştırmak için JavaScript istemci kitaplığını içerir.</span><span class="sxs-lookup"><span data-stu-id="79ed2-135">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="79ed2-136">Gerçek zamanlı işlevselliği sağlamak için mevcut bir ASP.NET uygulamasını için SignalR kitaplığı ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="79ed2-136">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="79ed2-137">Öğretici aşağıdaki SignalR geliştirme görevleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="79ed2-137">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="79ed2-138">SignalR kitaplığı, bir ASP.NET web uygulamasına ekleniyor.</span><span class="sxs-lookup"><span data-stu-id="79ed2-138">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="79ed2-139">İçeriği istemcilere göndermek için bir hub sınıfı oluşturma.</span><span class="sxs-lookup"><span data-stu-id="79ed2-139">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="79ed2-140">Uygulamayı yapılandırmak için OWIN başlangıç sınıfı oluşturma.</span><span class="sxs-lookup"><span data-stu-id="79ed2-140">Creating an OWIN startup class to configure the application.</span></span>
- <span data-ttu-id="79ed2-141">İleti gönderme ve hub güncelleştirmelerini görüntülemek için bir web sayfasında SignalR jQuery kitaplığı kullanma.</span><span class="sxs-lookup"><span data-stu-id="79ed2-141">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="79ed2-142">Aşağıdaki ekran görüntüsünde bir tarayıcıda çalışan sohbet uygulamayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="79ed2-142">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="79ed2-143">Her yeni kullanıcı yorumları gönderin ve kullanıcı sohbet katıldıktan sonra eklenen açıklamalar bakın.</span><span class="sxs-lookup"><span data-stu-id="79ed2-143">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Sohbet örnekleri](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="79ed2-145">Bölümler:</span><span class="sxs-lookup"><span data-stu-id="79ed2-145">Sections:</span></span>

- [<span data-ttu-id="79ed2-146">Projesi ayarlayın</span><span class="sxs-lookup"><span data-stu-id="79ed2-146">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="79ed2-147">Örnek çalıştırın</span><span class="sxs-lookup"><span data-stu-id="79ed2-147">Run the Sample</span></span>](#run)
- [<span data-ttu-id="79ed2-148">Kodu inceleyin</span><span class="sxs-lookup"><span data-stu-id="79ed2-148">Examine the Code</span></span>](#code)
- [<span data-ttu-id="79ed2-149">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="79ed2-149">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="79ed2-150">Projesi ayarlayın</span><span class="sxs-lookup"><span data-stu-id="79ed2-150">Set up the Project</span></span>

<span data-ttu-id="79ed2-151">Bu bölümde Visual Studio 2013 ve SignalR sürüm 2 boş bir ASP.NET web uygulaması oluşturmak için nasıl kullanılacağı gösterilmiştir SignalR ekleyin ve sohbet uygulaması oluşturma.</span><span class="sxs-lookup"><span data-stu-id="79ed2-151">This section shows how to use Visual Studio 2013 and SignalR version 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="79ed2-152">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="79ed2-152">Prerequisites:</span></span>

- <span data-ttu-id="79ed2-153">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="79ed2-153">Visual Studio 2013.</span></span> <span data-ttu-id="79ed2-154">Visual Studio yoksa bkz [ASP.NET indirmeleri](https://www.asp.net/downloads) ücretsiz Visual Studio 2013 Express geliştirme aracı alınamıyor.</span><span class="sxs-lookup"><span data-stu-id="79ed2-154">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express Development Tool.</span></span>

<span data-ttu-id="79ed2-155">ASP.NET boş Web uygulaması oluşturmak ve SignalR Kitaplığı eklemek için Visual Studio 2013 aşağıdaki adımları kullanın:</span><span class="sxs-lookup"><span data-stu-id="79ed2-155">The following steps use Visual Studio 2013 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="79ed2-156">Visual Studio'da ASP.NET Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="79ed2-156">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Web oluşturulamıyor](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="79ed2-158">İçinde **yeni ASP.NET projesi** penceresinde, bırakın **boş** 'ı tıklatın ve seçili **proje oluştur**.</span><span class="sxs-lookup"><span data-stu-id="79ed2-158">In the **New ASP.NET Project** window, leave **Empty** selected and click **Create Project**.</span></span>

    ![Boş web oluşturma](tutorial-getting-started-with-signalr/_static/image3.png)
3. <span data-ttu-id="79ed2-160">İçinde **Çözüm Gezgini**, projeye sağ tıklayın, seçin **Ekle | SignalR hub'ı sınıfı (v2)**.</span><span class="sxs-lookup"><span data-stu-id="79ed2-160">In **Solution Explorer**, right-click the project, select **Add | SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="79ed2-161">Sınıf adını **ChatHub.cs** ve bunu projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="79ed2-161">Name the class **ChatHub.cs** and add it to the project.</span></span> <span data-ttu-id="79ed2-162">Bu adım oluşturur **ChatHub** sınıfı ve bir dizi komut dosyaları ve Signalr'yi destekleyen derleme başvurularını projeye ekler.</span><span class="sxs-lookup"><span data-stu-id="79ed2-162">This step creates the **ChatHub** class and adds to the project a set of script files and assembly references that support SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="79ed2-163">Açarak projeye SignalR ekleyebilirsiniz **Araçlar | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu** ve bir komut çalıştırma:</span><span class="sxs-lookup"><span data-stu-id="79ed2-163">You can also add SignalR to a project by opening the **Tools | Library Package Manager | Package Manager Console** and running a command:</span></span>

    `install-package Microsoft.AspNet.SignalR`

    <span data-ttu-id="79ed2-164">SignalR eklemek için konsol kullanırsanız, SignalR hub'ı sınıf SignalR ekledikten sonra ayrı bir adım olarak oluşturun.</span><span class="sxs-lookup"><span data-stu-id="79ed2-164">If you use the console to add SignalR, create the SignalR hub class as a separate step after you add SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="79ed2-165">Visual Studio 2012 kullanıyorsanız **SignalR hub'ı sınıfı (v2)** şablonu kullanılabilir olmaz.</span><span class="sxs-lookup"><span data-stu-id="79ed2-165">If you are using Visual Studio 2012, the **SignalR Hub Class (v2)** template will not be available.</span></span> <span data-ttu-id="79ed2-166">Bir düz ekleyebilirsiniz **sınıfı** adlı `ChatHub` yerine.</span><span class="sxs-lookup"><span data-stu-id="79ed2-166">You can add a plain **Class** called `ChatHub` instead.</span></span>
4. <span data-ttu-id="79ed2-167">İçinde **Çözüm Gezgini**, komut dosyaları düğümünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="79ed2-167">In **Solution Explorer**, expand the Scripts node.</span></span> <span data-ttu-id="79ed2-168">JQuery ve SignalR için komut dosyası kitaplıkları projede görünür.</span><span class="sxs-lookup"><span data-stu-id="79ed2-168">Script libraries for jQuery and SignalR are visible in the project.</span></span>
5. <span data-ttu-id="79ed2-169">Kod yeni değiştirin **ChatHub** aşağıdaki kodla sınıfı.</span><span class="sxs-lookup"><span data-stu-id="79ed2-169">Replace the code in the new **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="79ed2-170">İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve ardından **Ekle | OWIN başlangıç sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="79ed2-170">In **Solution Explorer**, right-click the project, then click **Add | OWIN Startup Class**.</span></span> <span data-ttu-id="79ed2-171">Yeni sınıf `Startup` ve Tamam'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="79ed2-171">Name the new class `Startup` and click OK.</span></span>

    > [!NOTE]
    > <span data-ttu-id="79ed2-172">Visual Studio 2012 kullanıyorsanız **OWIN başlangıç sınıfı** şablonu kullanılabilir olmaz.</span><span class="sxs-lookup"><span data-stu-id="79ed2-172">If you are using Visual Studio 2012, the **OWIN Startup Class** template will not be available.</span></span> <span data-ttu-id="79ed2-173">Bir düz ekleyebilirsiniz **sınıfı** adlı `Startup` yerine.</span><span class="sxs-lookup"><span data-stu-id="79ed2-173">You can add a plain **Class** called `Startup` instead.</span></span>
7. <span data-ttu-id="79ed2-174">Yeni başlangıç sınıfı içeriğini aşağıdaki gibi değiştirin.</span><span class="sxs-lookup"><span data-stu-id="79ed2-174">Change the contents of the new Startup class to the following.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="79ed2-175">İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve ardından **Ekle | HTML sayfası**.</span><span class="sxs-lookup"><span data-stu-id="79ed2-175">In **Solution Explorer**, right-click the project, then click **Add | HTML Page**.</span></span> <span data-ttu-id="79ed2-176">Yeni sayfa adı `index.html`.</span><span class="sxs-lookup"><span data-stu-id="79ed2-176">Name the new page `index.html`.</span></span>
    >[!NOTE]
    ><span data-ttu-id="79ed2-177">Sürüm numaraları JQuery ve SignalR kitaplıklarına başvurular için değiştirmeniz gerekebilir</span><span class="sxs-lookup"><span data-stu-id="79ed2-177">You might need to change the version numbers for the references to JQuery and SignalR libraries</span></span>
9. <span data-ttu-id="79ed2-178">İçinde **Çözüm Gezgini**, az önce oluşturduğunuz HTML sayfası sağ tıklatın ve **Başlangıç Sayfası Ayarla**.</span><span class="sxs-lookup"><span data-stu-id="79ed2-178">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
10. <span data-ttu-id="79ed2-179">HTML sayfası varsayılan kodu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="79ed2-179">Replace the default code in the HTML page with the following code.</span></span>

    > [!NOTE]
    > <span data-ttu-id="79ed2-180">SignalR betikler daha sonraki bir sürümü Paket Yöneticisi tarafından yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="79ed2-180">A later version of the SignalR scripts may be installed by the package manager.</span></span> <span data-ttu-id="79ed2-181">Aşağıdaki komut dosyası başvuruları (bunlar bir hub'ı eklemek yerine NuGet kullanarak SignalR eklediyseniz farklı olacaktır.) proje komut dosyalarında sürümlerine karşılık geldiğinden emin olun</span><span class="sxs-lookup"><span data-stu-id="79ed2-181">Verify that the script references below correspond to the versions of the script files in the project (they will be different if you added SignalR using NuGet rather than adding a hub.)</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. <span data-ttu-id="79ed2-182">**Tümünü Kaydet** projesi için.</span><span class="sxs-lookup"><span data-stu-id="79ed2-182">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="79ed2-183">Örnek çalıştırın</span><span class="sxs-lookup"><span data-stu-id="79ed2-183">Run the Sample</span></span>

1. <span data-ttu-id="79ed2-184">Projeyi hata ayıklama modunda çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="79ed2-184">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="79ed2-185">HTML sayfası bir tarayıcı örneği ve bir kullanıcı adı için ister yükler.</span><span class="sxs-lookup"><span data-stu-id="79ed2-185">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Kullanıcı adı girin](tutorial-getting-started-with-signalr/_static/image4.png)
2. <span data-ttu-id="79ed2-187">Bir kullanıcı adı girin.</span><span class="sxs-lookup"><span data-stu-id="79ed2-187">Enter a user name.</span></span>
3. <span data-ttu-id="79ed2-188">Tarayıcının adres satırından URL'yi kopyalayın ve iki daha fazla tarayıcı örnekleri açmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="79ed2-188">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="79ed2-189">Her tarayıcı örnek benzersiz bir kullanıcı adı girin.</span><span class="sxs-lookup"><span data-stu-id="79ed2-189">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="79ed2-190">Her tarayıcı örnek bir açıklama ekleyin ve **Gönder**.</span><span class="sxs-lookup"><span data-stu-id="79ed2-190">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="79ed2-191">Açıklamaları tüm tarayıcı örnekleri görüntülemelidir.</span><span class="sxs-lookup"><span data-stu-id="79ed2-191">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="79ed2-192">Bu basit sohbet uygulama sunucusunda tartışma bağlam korumaz.</span><span class="sxs-lookup"><span data-stu-id="79ed2-192">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="79ed2-193">Hub'ın tüm geçerli kullanıcı yorumları yayınlar.</span><span class="sxs-lookup"><span data-stu-id="79ed2-193">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="79ed2-194">Sohbet daha sonra katılmak kullanıcılar zamandan eklenen iletilerini bunlar katılma görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="79ed2-194">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="79ed2-195">Aşağıdaki ekran görüntüsünde bir örnek ileti gönderdiğinde, güncelleştirilen üç tarayıcı durumlarda çalışan sohbet uygulama gösterir:</span><span class="sxs-lookup"><span data-stu-id="79ed2-195">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Sohbet tarayıcılar](tutorial-getting-started-with-signalr/_static/image5.png)
5. <span data-ttu-id="79ed2-197">İçinde **Çözüm Gezgini**, inceleme **betik belgelerini** çalışan uygulama için düğüm.</span><span class="sxs-lookup"><span data-stu-id="79ed2-197">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="79ed2-198">Adlı bir komut dosyası **hub** , SignalR kitaplık çalışma zamanında dinamik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="79ed2-198">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="79ed2-199">Bu dosya, jQuery betik sunucu tarafı kodu arasında iletişim yönetir.</span><span class="sxs-lookup"><span data-stu-id="79ed2-199">This file manages the communication between jQuery script and server-side code.</span></span>

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="79ed2-200">Kodu inceleyin</span><span class="sxs-lookup"><span data-stu-id="79ed2-200">Examine the Code</span></span>

<span data-ttu-id="79ed2-201">SignalR sohbet uygulaması iki temel SignalR geliştirme görevlerini gösterir: sunucu üzerindeki ana koordinasyon nesnesi olarak bir hub'ı oluşturma ve ileti gönderme ve alma için SignalR jQuery kitaplığı kullanma.</span><span class="sxs-lookup"><span data-stu-id="79ed2-201">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="79ed2-202">SignalR hub'ları</span><span class="sxs-lookup"><span data-stu-id="79ed2-202">SignalR Hubs</span></span>

<span data-ttu-id="79ed2-203">Kod örneğinde **ChatHub** sınıfı türer **Microsoft.AspNet.SignalR.Hub** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="79ed2-203">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="79ed2-204">Türetme **Hub** bir SignalR uygulaması oluşturmak için kullanışlı bir yol bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="79ed2-204">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="79ed2-205">Genel yöntemler hub sınıfınız oluşturun ve sonra bu yöntemler bir web sayfasında komut dosyalarından çağırarak erişim.</span><span class="sxs-lookup"><span data-stu-id="79ed2-205">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="79ed2-206">İstemciler sohbet kodda çağrısı **ChatHub.Send** yeni bir ileti göndermek için yöntem.</span><span class="sxs-lookup"><span data-stu-id="79ed2-206">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="79ed2-207">Hub sırayla iletiyi tüm istemcilere çağırarak gönderir **Clients.All.broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="79ed2-207">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="79ed2-208">**Gönder** yöntemi birkaç hub kavramları göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="79ed2-208">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="79ed2-209">Böylece istemciler bunları çağırabilirsiniz genel yöntemler bir hub'ına bildirin.</span><span class="sxs-lookup"><span data-stu-id="79ed2-209">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="79ed2-210">Kullanım **Microsoft.AspNet.SignalR.Hub.Clients** tüm istemciler erişmek için dinamik özellik bu hub'ına bağlı.</span><span class="sxs-lookup"><span data-stu-id="79ed2-210">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="79ed2-211">İstemcide bir işlevi çağırmak (gibi `broadcastMessage` işlevi) istemcilerini güncelleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="79ed2-211">Call a function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="79ed2-212">SignalR ve jQuery</span><span class="sxs-lookup"><span data-stu-id="79ed2-212">SignalR and jQuery</span></span>

<span data-ttu-id="79ed2-213">Kod örneğinde HTML sayfası SignalR jQuery kitaplığı bir SignalR hub ile iletişim kurmak için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="79ed2-213">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="79ed2-214">Kodda önemli görevleri istemcilere anında içeriği için sunucu çağırabilirsiniz işlevi bildirme ve hub'ına iletileri göndermek için bir bağlantı başlatma hub başvurmak için bir proxy bildirdiğiniz.</span><span class="sxs-lookup"><span data-stu-id="79ed2-214">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="79ed2-215">Aşağıdaki kod, bir hub proxy için bir başvuru bildirir.</span><span class="sxs-lookup"><span data-stu-id="79ed2-215">The following code declares a reference to a hub proxy.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="79ed2-216">JavaScript'te sunucu sınıfı ve üyelerini içinde ortası büyük başvurudur.</span><span class="sxs-lookup"><span data-stu-id="79ed2-216">In JavaScript the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="79ed2-217">Kod örneği C# başvuran **ChatHub** JavaScript sınıfında **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="79ed2-217">The code sample references the C# **ChatHub** class in JavaScript as **chatHub**.</span></span>


<span data-ttu-id="79ed2-218">Aşağıdaki komut dosyasında bir geri çağırma işlevini oluşturma kodudur.</span><span class="sxs-lookup"><span data-stu-id="79ed2-218">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="79ed2-219">Sunucudaki hub sınıfı içerik güncelleştirmeleri her bir istemciye göndermek için bu işlevi çağırır.</span><span class="sxs-lookup"><span data-stu-id="79ed2-219">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="79ed2-220">HTML kodlama, içeriği görüntülemeden önce iki satır isteğe bağlıdır ve kod eklemesini önlemeye yönelik basit bir yol gösterir.</span><span class="sxs-lookup"><span data-stu-id="79ed2-220">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="79ed2-221">Aşağıdaki kod, hub ile bir bağlantı açmak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="79ed2-221">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="79ed2-222">Kod bağlantı başlar ve üzerinde click olayını işlemek için bir işlev geçirir **Gönder** HTML sayfasının düğmesini.</span><span class="sxs-lookup"><span data-stu-id="79ed2-222">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="79ed2-223">Bu yaklaşım, olay işleyici yürütülmeden önce bağlantı kurulana sağlar.</span><span class="sxs-lookup"><span data-stu-id="79ed2-223">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="79ed2-224">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="79ed2-224">Next Steps</span></span>

<span data-ttu-id="79ed2-225">SignalR gerçek zamanlı web uygulamaları oluşturmak için bir çerçeve olduğunu öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="79ed2-225">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="79ed2-226">Birkaç SignalR geliştirme görevleri de öğrenilen: bir ASP.NET uygulaması için SignalR ekleme, hub sınıfın nasıl oluşturulacağı ve hub'dan ileti alıp göndermek nasıl.</span><span class="sxs-lookup"><span data-stu-id="79ed2-226">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="79ed2-227">Örnek SignalR uygulamayı Azure'a dağıtma hakkında bir kılavuz için bkz: [SignalR kullanarak Azure App Service'te Web uygulamalarını](../deployment/using-signalr-with-azure-web-sites.md).</span><span class="sxs-lookup"><span data-stu-id="79ed2-227">For a walkthrough on how to deploy the sample SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="79ed2-228">Windows Azure Web sitesi için bir Visual Studio web projesi dağıtma hakkında ayrıntılı bilgi için bkz: [Azure App Service'te bir ASP.NET web uygulaması oluşturma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="79ed2-228">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<span data-ttu-id="79ed2-229">Daha gelişmiş SignalR gelişmeler kavramları hakkında bilgi için SignalR kaynak kodu ve kaynaklar için aşağıdaki siteleri ziyaret edin:</span><span class="sxs-lookup"><span data-stu-id="79ed2-229">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="79ed2-230">SignalR Project</span><span class="sxs-lookup"><span data-stu-id="79ed2-230">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="79ed2-231">SignalR Github ve örnekler</span><span class="sxs-lookup"><span data-stu-id="79ed2-231">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="79ed2-232">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="79ed2-232">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
