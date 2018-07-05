---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Öğretici: SignalR 2 ile çalışmaya başlama | Microsoft Docs'
author: pfletcher
description: Bu öğreticide SignalR kullanarak gerçek zamanlı bir sohbet uygulaması oluşturma işlemi gösterilir. SignalR için boş bir ASP.NET web uygulamasına ekleme ve bir HTML pa oluştur...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: fcd00419de77a380e004cbe306eb46910655a355
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398190"
---
<a name="tutorial-getting-started-with-signalr-2"></a><span data-ttu-id="959ac-104">Öğretici: SignalR 2 ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="959ac-104">Tutorial: Getting Started with SignalR 2</span></span>
====================
<span data-ttu-id="959ac-105">tarafından [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="959ac-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="959ac-106">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="959ac-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> <span data-ttu-id="959ac-107">Bu öğreticide SignalR kullanarak gerçek zamanlı bir sohbet uygulaması oluşturma işlemi gösterilir.</span><span class="sxs-lookup"><span data-stu-id="959ac-107">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="959ac-108">SignalR için boş bir ASP.NET web uygulamasına ekleme ve gönderin ve iletileri görüntülemek için bir HTML sayfası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="959ac-108">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span> 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="959ac-109">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="959ac-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="959ac-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="959ac-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="959ac-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="959ac-111">.NET 4.5</span></span>
> - <span data-ttu-id="959ac-112">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="959ac-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="959ac-113">Bu öğreticide Visual Studio 2012 kullanarak</span><span class="sxs-lookup"><span data-stu-id="959ac-113">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="959ac-114">Visual Studio 2012 bu öğreticiyle kullanmak için aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="959ac-114">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="959ac-115">Güncelleştirme, [Paket Yöneticisi](http://docs.nuget.org/docs/start-here/installing-nuget) en son sürüme.</span><span class="sxs-lookup"><span data-stu-id="959ac-115">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="959ac-116">Yükleme [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="959ac-116">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="959ac-117">Web Platformu Yükleyicisi'nde arama ve yükleme **ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için**.</span><span class="sxs-lookup"><span data-stu-id="959ac-117">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="959ac-118">Bu SignalR sınıflar için Visual Studio şablonları gibi yükleyecek **Hub**.</span><span class="sxs-lookup"><span data-stu-id="959ac-118">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="959ac-119">Bazı şablonlar (gibi **OWIN başlangıç sınıfı**) kullanılabilir; olmayacaktır, bunlar için sınıf dosyası kullanın.</span><span class="sxs-lookup"><span data-stu-id="959ac-119">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="959ac-120">Öğretici sürümleri</span><span class="sxs-lookup"><span data-stu-id="959ac-120">Tutorial versions</span></span>
> 
> <span data-ttu-id="959ac-121">SignalR eski sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="959ac-121">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="959ac-122">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="959ac-122">Questions and comments</span></span>
> 
> <span data-ttu-id="959ac-123">Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın.</span><span class="sxs-lookup"><span data-stu-id="959ac-123">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="959ac-124">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="959ac-124">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="959ac-125">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="959ac-125">Overview</span></span>

<span data-ttu-id="959ac-126">Bu öğretici, bir basit tarayıcı tabanlı sohbet uygulaması oluşturmak nasıl yapıldığını göstererek SignalR geliştirme tanıtır.</span><span class="sxs-lookup"><span data-stu-id="959ac-126">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="959ac-127">SignalR kitaplık için boş bir ASP.NET web uygulamasına eklemek, istemcilere ileti göndermek için hub sınıfı oluşturun ve sohbet iletileri gönderip kullanıcıların olanak sağlayan bir HTML sayfası oluşturmak.</span><span class="sxs-lookup"><span data-stu-id="959ac-127">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="959ac-128">MVC 5'te bir sohbet uygulaması oluşturma işlemini kullanarak bir MVC görünümü gösteren benzer bir öğretici için bkz. [SignalR 2 ve MVC 5 ile çalışmaya başlama](tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="959ac-128">For a similar tutorial that shows how to create a chat application in MVC 5 using an MVC view, see [Getting Started with SignalR 2 and MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

> [!NOTE]
> <span data-ttu-id="959ac-129">Bu öğreticide, sürüm 2 SignalR uygulamalarını oluşturma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="959ac-129">This tutorial demonstrates how to create SignalR applications in version 2.</span></span> <span data-ttu-id="959ac-130">SignalR arasındaki değişiklikleri hakkında ayrıntılı bilgi için 1.x ve 2 ' nin bkz [yükseltme SignalR 1.x projelerini](../releases/upgrading-signalr-1x-projects-to-20.md) ve [Visual Studio 2013 sürüm notları](../../../visual-studio/overview/2013/release-notes.md#TOC13).</span><span class="sxs-lookup"><span data-stu-id="959ac-130">For details on changes between SignalR 1.x and 2, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md) and [Visual Studio 2013 Release Notes](../../../visual-studio/overview/2013/release-notes.md#TOC13).</span></span>

<span data-ttu-id="959ac-131">SignalR Canlı kullanıcı etkileşimi veya gerçek zamanlı veri güncelleştirmeleri gerektiren web uygulamaları oluşturmaya yönelik bir açık kaynak .NET Kitaplığı ' dir.</span><span class="sxs-lookup"><span data-stu-id="959ac-131">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="959ac-132">Sosyal uygulamalar, çok kullanıcılı bir oyun, iş işbirliği ve haberler, hava durumu veya finansal güncelleştirme uygulamaları verilebilir.</span><span class="sxs-lookup"><span data-stu-id="959ac-132">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="959ac-133">Bunlar genellikle gerçek zamanlı uygulamalar olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="959ac-133">These are often called real-time applications.</span></span>

<span data-ttu-id="959ac-134">SignalR, gerçek zamanlı uygulamalar oluşturma işlemini basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="959ac-134">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="959ac-135">Bu, bir ASP.NET sunucu kitaplığı ve istemci-sunucu bağlantılarını yönetme ve içerik güncelleştirmelerini istemcilere göndermek daha kolay hale getirmek için bir JavaScript istemci kitaplığı içerir.</span><span class="sxs-lookup"><span data-stu-id="959ac-135">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="959ac-136">Gerçek zamanlı işlevsellik sağlamak için mevcut bir ASP.NET uygulamasını için SignalR kitaplığa ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="959ac-136">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="959ac-137">Öğretici aşağıdaki SignalR geliştirme görevleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="959ac-137">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="959ac-138">SignalR kitaplığı, bir ASP.NET web uygulamasına ekleniyor.</span><span class="sxs-lookup"><span data-stu-id="959ac-138">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="959ac-139">İçeriği istemcilere göndermek için bir hub sınıf oluşturuluyor.</span><span class="sxs-lookup"><span data-stu-id="959ac-139">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="959ac-140">Uygulamayı yapılandırmak için OWIN başlangıç sınıfı oluşturuluyor.</span><span class="sxs-lookup"><span data-stu-id="959ac-140">Creating an OWIN startup class to configure the application.</span></span>
- <span data-ttu-id="959ac-141">İleti göndermek ve hub'ından güncelleştirmeleri görüntülemek için bir web sayfasında SignalR jQuery kitaplığı kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="959ac-141">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="959ac-142">Aşağıdaki ekran görüntüsünde, bir tarayıcıda çalışan sohbet uygulaması gösterir.</span><span class="sxs-lookup"><span data-stu-id="959ac-142">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="959ac-143">Her yeni kullanıcı, yorumlarınızı ve kullanıcı sohbet katıldıktan sonra eklenen yorumlara bakın.</span><span class="sxs-lookup"><span data-stu-id="959ac-143">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Sohbet örnekleri](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="959ac-145">Bölümler:</span><span class="sxs-lookup"><span data-stu-id="959ac-145">Sections:</span></span>

- [<span data-ttu-id="959ac-146">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="959ac-146">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="959ac-147">Örneği çalıştırma</span><span class="sxs-lookup"><span data-stu-id="959ac-147">Run the Sample</span></span>](#run)
- [<span data-ttu-id="959ac-148">Kod İnceleme</span><span class="sxs-lookup"><span data-stu-id="959ac-148">Examine the Code</span></span>](#code)
- [<span data-ttu-id="959ac-149">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="959ac-149">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="959ac-150">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="959ac-150">Set up the Project</span></span>

<span data-ttu-id="959ac-151">Bu bölümde Visual Studio 2013 ve SignalR sürüm 2 boş bir ASP.NET web uygulaması oluşturmak için nasıl kullanılacağını gösterir, SignalR ekleyin ve sohbet uygulaması oluşturma.</span><span class="sxs-lookup"><span data-stu-id="959ac-151">This section shows how to use Visual Studio 2013 and SignalR version 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="959ac-152">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="959ac-152">Prerequisites:</span></span>

- <span data-ttu-id="959ac-153">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="959ac-153">Visual Studio 2013.</span></span> <span data-ttu-id="959ac-154">Visual Studio yoksa bkz [ASP.NET indirir](https://www.asp.net/downloads) ücretsiz Visual Studio 2013 Express geliştirme aracı alınamıyor.</span><span class="sxs-lookup"><span data-stu-id="959ac-154">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express Development Tool.</span></span>

<span data-ttu-id="959ac-155">ASP.NET boş Web uygulaması oluşturma ve SignalR Kitaplığı eklemek için Visual Studio 2013'ün aşağıdaki adımları kullanın:</span><span class="sxs-lookup"><span data-stu-id="959ac-155">The following steps use Visual Studio 2013 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="959ac-156">Visual Studio kullanarak ASP.NET Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="959ac-156">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Web oluşturma](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="959ac-158">İçinde **yeni ASP.NET projesi** penceresinde bırakın **boş** tıklatın ve seçili **proje oluştur**.</span><span class="sxs-lookup"><span data-stu-id="959ac-158">In the **New ASP.NET Project** window, leave **Empty** selected and click **Create Project**.</span></span>

    ![Boş web oluşturma](tutorial-getting-started-with-signalr/_static/image3.png)
3. <span data-ttu-id="959ac-160">İçinde **Çözüm Gezgini**, projeye sağ tıklayın, **Ekle | SignalR Hub sınıfı (v2)**.</span><span class="sxs-lookup"><span data-stu-id="959ac-160">In **Solution Explorer**, right-click the project, select **Add | SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="959ac-161">Sınıf adı **ChatHub.cs** ve projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="959ac-161">Name the class **ChatHub.cs** and add it to the project.</span></span> <span data-ttu-id="959ac-162">Bu adımda oluşturulur **ChatHub** sınıfı ve bir dizi komut dosyaları ve Signalr'yi destekleyen bir bütünleştirilmiş kod başvuruları projeye ekler.</span><span class="sxs-lookup"><span data-stu-id="959ac-162">This step creates the **ChatHub** class and adds to the project a set of script files and assembly references that support SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="959ac-163">Açarak bir projeye SignalR ekleyebilirsiniz **araçları | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu** ve bir komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="959ac-163">You can also add SignalR to a project by opening the **Tools | Library Package Manager | Package Manager Console** and running a command:</span></span>

    `install-package Microsoft.AspNet.SignalR`

    <span data-ttu-id="959ac-164">SignalR eklemek için konsolunu kullanırsanız, SignalR hub sınıfı SignalR ekledikten sonra ayrı bir adım olarak oluşturun.</span><span class="sxs-lookup"><span data-stu-id="959ac-164">If you use the console to add SignalR, create the SignalR hub class as a separate step after you add SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="959ac-165">Visual Studio 2012 kullanıyorsanız **SignalR Hub sınıfı (v2)** şablonu kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="959ac-165">If you are using Visual Studio 2012, the **SignalR Hub Class (v2)** template will not be available.</span></span> <span data-ttu-id="959ac-166">Bir düz ekleyebilirsiniz **sınıfı** adlı `ChatHub` yerine.</span><span class="sxs-lookup"><span data-stu-id="959ac-166">You can add a plain **Class** called `ChatHub` instead.</span></span>
4. <span data-ttu-id="959ac-167">İçinde **Çözüm Gezgini**, betikleri düğümünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="959ac-167">In **Solution Explorer**, expand the Scripts node.</span></span> <span data-ttu-id="959ac-168">JQuery ve SignalR için betik kitaplıkları projesinde görünür.</span><span class="sxs-lookup"><span data-stu-id="959ac-168">Script libraries for jQuery and SignalR are visible in the project.</span></span>
5. <span data-ttu-id="959ac-169">Yeni bir kodu **ChatHub** aşağıdaki kodla sınıfı.</span><span class="sxs-lookup"><span data-stu-id="959ac-169">Replace the code in the new **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="959ac-170">İçinde **Çözüm Gezgini**, projeyi sağ tıklatın ve ardından **Ekle | OWIN başlangıç sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="959ac-170">In **Solution Explorer**, right-click the project, then click **Add | OWIN Startup Class**.</span></span> <span data-ttu-id="959ac-171">Yeni bir sınıf adı `Startup` ve Tamam'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="959ac-171">Name the new class `Startup` and click OK.</span></span>

    > [!NOTE]
    > <span data-ttu-id="959ac-172">Visual Studio 2012 kullanıyorsanız **OWIN başlangıç sınıfı** şablonu kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="959ac-172">If you are using Visual Studio 2012, the **OWIN Startup Class** template will not be available.</span></span> <span data-ttu-id="959ac-173">Bir düz ekleyebilirsiniz **sınıfı** adlı `Startup` yerine.</span><span class="sxs-lookup"><span data-stu-id="959ac-173">You can add a plain **Class** called `Startup` instead.</span></span>
7. <span data-ttu-id="959ac-174">Yeni başlangıç sınıfı içeriğini şu şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="959ac-174">Change the contents of the new Startup class to the following.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="959ac-175">İçinde **Çözüm Gezgini**, projeyi sağ tıklatın ve ardından **Ekle | HTML sayfası**.</span><span class="sxs-lookup"><span data-stu-id="959ac-175">In **Solution Explorer**, right-click the project, then click **Add | HTML Page**.</span></span> <span data-ttu-id="959ac-176">Yeni sayfa adı `index.html`.</span><span class="sxs-lookup"><span data-stu-id="959ac-176">Name the new page `index.html`.</span></span>
    >[!NOTE]
    ><span data-ttu-id="959ac-177">JQuery ve SignalR kitaplıklarına başvurular için sürüm numaraları değiştirmeniz gerekebilir</span><span class="sxs-lookup"><span data-stu-id="959ac-177">You might need to change the version numbers for the references to JQuery and SignalR libraries</span></span>
9. <span data-ttu-id="959ac-178">İçinde **Çözüm Gezgini**, yeni oluşturduğunuz HTML sayfasının sağ tıklatıp **Başlangıç Sayfası Ayarla**.</span><span class="sxs-lookup"><span data-stu-id="959ac-178">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
10. <span data-ttu-id="959ac-179">HTML sayfasındaki varsayılan kodu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="959ac-179">Replace the default code in the HTML page with the following code.</span></span>

    > [!NOTE]
    > <span data-ttu-id="959ac-180">SignalR betikleri daha sonraki bir sürümünü Paket Yöneticisi tarafından yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="959ac-180">A later version of the SignalR scripts may be installed by the package manager.</span></span> <span data-ttu-id="959ac-181">Aşağıdaki betik başvurularını komut dosyalarını (bunlar SignalR hub'ı eklemek yerine NuGet kullanarak eklediyseniz farklı olacaktır.) projede sürümlerine karşılık geldiğini doğrulayın</span><span class="sxs-lookup"><span data-stu-id="959ac-181">Verify that the script references below correspond to the versions of the script files in the project (they will be different if you added SignalR using NuGet rather than adding a hub.)</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. <span data-ttu-id="959ac-182">**Tümünü Kaydet** projesi için.</span><span class="sxs-lookup"><span data-stu-id="959ac-182">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="959ac-183">Örneği çalıştırma</span><span class="sxs-lookup"><span data-stu-id="959ac-183">Run the Sample</span></span>

1. <span data-ttu-id="959ac-184">Projeyi hata ayıklama modunda çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="959ac-184">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="959ac-185">Bir tarayıcı örneğinde ve bir kullanıcı adı için istemleri HTML sayfasını yükler.</span><span class="sxs-lookup"><span data-stu-id="959ac-185">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Kullanıcı adı girin](tutorial-getting-started-with-signalr/_static/image4.png)
2. <span data-ttu-id="959ac-187">Bir kullanıcı adı girin.</span><span class="sxs-lookup"><span data-stu-id="959ac-187">Enter a user name.</span></span>
3. <span data-ttu-id="959ac-188">Tarayıcının adres satırından URL'sini kopyalayın ve iki daha fazla tarayıcı örneği açmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="959ac-188">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="959ac-189">Her bir tarayıcı örneğinde benzersiz bir kullanıcı adı girin.</span><span class="sxs-lookup"><span data-stu-id="959ac-189">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="959ac-190">Her bir tarayıcı örneğinde bir açıklama ekleyin ve **Gönder**.</span><span class="sxs-lookup"><span data-stu-id="959ac-190">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="959ac-191">Açıklamalar, hepsinin tarayıcı görüntülemelidir.</span><span class="sxs-lookup"><span data-stu-id="959ac-191">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="959ac-192">Bu basit sohbet uygulaması sunucusunda tartışma bağlam korumaz.</span><span class="sxs-lookup"><span data-stu-id="959ac-192">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="959ac-193">Hub'ın tüm geçerli kullanıcılar yorum yayınlar.</span><span class="sxs-lookup"><span data-stu-id="959ac-193">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="959ac-194">Sohbet daha sonra katılmak kullanıcıların zamandan eklenen iletileri katılmaları görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="959ac-194">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="959ac-195">Aşağıdaki ekran görüntüsünde, çalışan bir örnek ileti gönderdiğinde tümü güncelleştirilir üç tarayıcı durumlarda sohbet uygulaması gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="959ac-195">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Sohbet tarayıcılar](tutorial-getting-started-with-signalr/_static/image5.png)
5. <span data-ttu-id="959ac-197">İçinde **Çözüm Gezgini**, inceleme **betik belgelerini** çalışan uygulama düğümü.</span><span class="sxs-lookup"><span data-stu-id="959ac-197">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="959ac-198">Adlı bir betik dosyası **hubs** , SignalR kitaplık çalışma zamanında dinamik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="959ac-198">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="959ac-199">Bu dosya, jQuery betik sunucu tarafı kodu arasında iletişimi yönetir.</span><span class="sxs-lookup"><span data-stu-id="959ac-199">This file manages the communication between jQuery script and server-side code.</span></span>

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="959ac-200">Kod İnceleme</span><span class="sxs-lookup"><span data-stu-id="959ac-200">Examine the Code</span></span>

<span data-ttu-id="959ac-201">SignalR sohbet uygulaması iki temel SignalR geliştirme görevleri gösterir: sunucunun ana koordinasyon nesne olarak bir hub'ı oluşturma ve ileti göndermek ve almak için SignalR jQuery kitaplığı kullanma.</span><span class="sxs-lookup"><span data-stu-id="959ac-201">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="959ac-202">SignalR hub'ları</span><span class="sxs-lookup"><span data-stu-id="959ac-202">SignalR Hubs</span></span>

<span data-ttu-id="959ac-203">Kod örneğinde **ChatHub** sınıf türetilir **Microsoft.AspNet.SignalR.Hub** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="959ac-203">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="959ac-204">Öğesinden türetme **Hub** SignalR uygulama oluşturmak için kullanışlı bir yöntem bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="959ac-204">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="959ac-205">Hub sınıfınıza genel yöntemleri oluşturun ve bu yöntemler bir web sayfasında komut dosyalarından çağırarak erişin.</span><span class="sxs-lookup"><span data-stu-id="959ac-205">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="959ac-206">İstemciler sohbet kodda çağrı **ChatHub.Send** yeni bir ileti göndermek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="959ac-206">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="959ac-207">Hub sırayla ileti tüm istemcilere çağırarak gönderen **Clients.All.broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="959ac-207">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="959ac-208">**Gönder** yöntemi birkaç hub kavramları göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="959ac-208">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="959ac-209">İstemciler, bunları çağırabilirsiniz genel yöntemleri bir hub'da bildirin.</span><span class="sxs-lookup"><span data-stu-id="959ac-209">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="959ac-210">Kullanım **Microsoft.AspNet.SignalR.Hub.Clients** tüm istemcilere erişmek için dinamik özellik bu hub'a bağlı.</span><span class="sxs-lookup"><span data-stu-id="959ac-210">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="959ac-211">İstemcide bir işlevi çağırmayı (gibi `broadcastMessage` işlevi) istemcilerini güncelleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="959ac-211">Call a function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="959ac-212">SignalR ve jQuery</span><span class="sxs-lookup"><span data-stu-id="959ac-212">SignalR and jQuery</span></span>

<span data-ttu-id="959ac-213">Kod örneği HTML sayfasındaki bir SignalR hub'ı ile iletişim kurmak için SignalR jQuery kitaplığı kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="959ac-213">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="959ac-214">Kodda önemli görevleri istemcilere anında içeriği için sunucu çağırabilen bir işlevi bildirmek ve hub'ına ileti göndermek için bir bağlantı başlatma hub başvurmak için bir proxy bildirdiğiniz.</span><span class="sxs-lookup"><span data-stu-id="959ac-214">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="959ac-215">Aşağıdaki kod, bir hub proxy için bir başvuru bildirir.</span><span class="sxs-lookup"><span data-stu-id="959ac-215">The following code declares a reference to a hub proxy.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="959ac-216">JavaScript'te, sunucu sınıfını ve üyelerini ortası büyük harf başvurudur.</span><span class="sxs-lookup"><span data-stu-id="959ac-216">In JavaScript the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="959ac-217">Kod örneği C# başvuran **ChatHub** JavaScript olarak sınıfında **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="959ac-217">The code sample references the C# **ChatHub** class in JavaScript as **chatHub**.</span></span>


<span data-ttu-id="959ac-218">Aşağıdaki betikte bir geri çağırma işlevini nasıl oluşturacağınız kodudur.</span><span class="sxs-lookup"><span data-stu-id="959ac-218">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="959ac-219">Sunucudaki hub sınıfına içerik güncelleştirmeleri her bir istemciye göndermek için bu işlevi çağırır.</span><span class="sxs-lookup"><span data-stu-id="959ac-219">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="959ac-220">HTML kodlama, içerik görüntülemeden önce aşağıdaki iki satırı isteğe bağlıdır ve kod eklemesini engellemek için basit bir yol gösterir.</span><span class="sxs-lookup"><span data-stu-id="959ac-220">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="959ac-221">Aşağıdaki kod hub'ı ile bir bağlantı açmak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="959ac-221">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="959ac-222">Kod bağlantı başlar ve ardından üzerinde click olayını işlemek için bir işlev geçirir **Gönder** HTML sayfasındaki düğmesi.</span><span class="sxs-lookup"><span data-stu-id="959ac-222">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="959ac-223">Bu yaklaşım, olay işleyici yürütülmeden önce bağlantı kurulur sağlar.</span><span class="sxs-lookup"><span data-stu-id="959ac-223">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="959ac-224">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="959ac-224">Next Steps</span></span>

<span data-ttu-id="959ac-225">SignalR gerçek zamanlı web uygulamaları oluşturmaya yönelik bir çerçeve olduğunu öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="959ac-225">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="959ac-226">Ayrıca birkaç SignalR geliştirme görevlerini öğrendiniz: bir ASP.NET uygulaması için SignalR ekleme, hub sınıfı oluşturma ve hub'ından iletiler alan nasıl.</span><span class="sxs-lookup"><span data-stu-id="959ac-226">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="959ac-227">Örnek SignalR uygulamayı Azure'a dağıtma hakkında kılavuz için bkz. [Azure App service'taki Web Apps ile SignalR kullanarak](../deployment/using-signalr-with-azure-web-sites.md).</span><span class="sxs-lookup"><span data-stu-id="959ac-227">For a walkthrough on how to deploy the sample SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="959ac-228">Visual Studio web projesini bir Windows Azure Web sitesine dağıtma hakkında ayrıntılı bilgi için bkz. [Azure App Service'te ASP.NET web uygulaması oluşturma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="959ac-228">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<span data-ttu-id="959ac-229">Daha gelişmiş SignalR gelişmeleri kavramları hakkında bilgi için SignalR kaynak kodu ve kaynaklar için aşağıdaki siteleri ziyaret edin:</span><span class="sxs-lookup"><span data-stu-id="959ac-229">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="959ac-230">SignalR projesi</span><span class="sxs-lookup"><span data-stu-id="959ac-230">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="959ac-231">SignalR Github ve örnekleri</span><span class="sxs-lookup"><span data-stu-id="959ac-231">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="959ac-232">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="959ac-232">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
