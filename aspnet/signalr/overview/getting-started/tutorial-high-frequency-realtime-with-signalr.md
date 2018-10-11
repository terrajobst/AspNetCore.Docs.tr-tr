---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Öğretici: SignalR 2 ile yüksek sıklıkta gerçek | Microsoft Docs'
author: pfletcher
description: Bu öğretici, ASP.NET SignalR, yüksek frekanslı Mesajlaşma işlevleri sağlamak için kullanan bir web uygulaması oluşturma işlemi gösterilmektedir. Yüksek frekanslı..., Mesajlaşma
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 23dc9cc7fd469e934ed9915922a3baa772d9e1ab
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912043"
---
<a name="tutorial-high-frequency-realtime-with-signalr-2"></a><span data-ttu-id="c86ec-104">Öğretici: SignalR 2 ile yüksek sıklıkta gerçek</span><span class="sxs-lookup"><span data-stu-id="c86ec-104">Tutorial: High-Frequency Realtime with SignalR 2</span></span>
====================
<span data-ttu-id="c86ec-105">tarafından [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="c86ec-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="c86ec-106">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="c86ec-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

> <span data-ttu-id="c86ec-107">Bu öğreticide, yüksek frekanslı Mesajlaşma işlevleri sağlamak için ASP.NET SignalR 2 kullanan bir web uygulaması oluşturma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="c86ec-107">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span> <span data-ttu-id="c86ec-108">Yüksek frekanslı Mesajlaşma bu durumda sabit bir fiyat karşılığında gönderilen güncelleştirmeleri anlamına gelir; Bu uygulama söz konusu olduğunda, en fazla 10 saniyenin iletileri.</span><span class="sxs-lookup"><span data-stu-id="c86ec-108">High-frequency messaging in this case means updates that are sent at a fixed rate; in the case of this application, up to 10 messages a second.</span></span>
>
> <span data-ttu-id="c86ec-109">Bu öğreticide oluşturacağınız uygulama kullanıcıları sürükleyebilirsiniz bir şekil görüntüler.</span><span class="sxs-lookup"><span data-stu-id="c86ec-109">The application you'll create in this tutorial displays a shape that users can drag.</span></span> <span data-ttu-id="c86ec-110">Diğer tüm bağlı tarayıcıların şekil konumunu Zamanlanmış güncelleştirmeleri kullanarak sürüklenen şekli konumunu eşleşecek şekilde güncelleştirilecektir.</span><span class="sxs-lookup"><span data-stu-id="c86ec-110">The position of the shape in all other connected browsers will then be updated to match the position of the dragged shape using timed updates.</span></span>
>
> <span data-ttu-id="c86ec-111">Bu öğreticide tanıtılan kavramları, gerçek zamanlı oyun uygulamaları ve diğer benzetimi uygulamaları vardır.</span><span class="sxs-lookup"><span data-stu-id="c86ec-111">Concepts introduced in this tutorial have applications in real-time gaming and other simulation applications.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c86ec-112">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="c86ec-112">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="c86ec-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="c86ec-113">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="c86ec-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="c86ec-114">.NET 4.5</span></span>
> - <span data-ttu-id="c86ec-115">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="c86ec-115">SignalR version 2</span></span>
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="c86ec-116">Bu öğreticide Visual Studio 2012 kullanarak</span><span class="sxs-lookup"><span data-stu-id="c86ec-116">Using Visual Studio 2012 with this tutorial</span></span>
>
>
> <span data-ttu-id="c86ec-117">Visual Studio 2012 bu öğreticiyle kullanmak için aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="c86ec-117">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
>
> - <span data-ttu-id="c86ec-118">Güncelleştirme, [Paket Yöneticisi](http://docs.nuget.org/docs/start-here/installing-nuget) en son sürüme.</span><span class="sxs-lookup"><span data-stu-id="c86ec-118">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="c86ec-119">Yükleme [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="c86ec-119">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="c86ec-120">Web Platformu Yükleyicisi'nde arama ve yükleme **ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için**.</span><span class="sxs-lookup"><span data-stu-id="c86ec-120">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="c86ec-121">Bu SignalR sınıflar için Visual Studio şablonları gibi yükleyecek **Hub**.</span><span class="sxs-lookup"><span data-stu-id="c86ec-121">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="c86ec-122">Bazı şablonlar (gibi **OWIN başlangıç sınıfı**) kullanılabilir; olmayacaktır, bunlar için sınıf dosyası kullanın.</span><span class="sxs-lookup"><span data-stu-id="c86ec-122">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="c86ec-123">Öğretici sürümleri</span><span class="sxs-lookup"><span data-stu-id="c86ec-123">Tutorial Versions</span></span>
>
> <span data-ttu-id="c86ec-124">SignalR eski sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="c86ec-124">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="c86ec-125">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="c86ec-125">Questions and comments</span></span>
>
> <span data-ttu-id="c86ec-126">Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın.</span><span class="sxs-lookup"><span data-stu-id="c86ec-126">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="c86ec-127">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="c86ec-127">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="c86ec-128">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="c86ec-128">Overview</span></span>

<span data-ttu-id="c86ec-129">Bu öğreticide, diğer tarayıcılarda gerçek zamanlı olarak bir nesnenin durumu paylaşan bir uygulamanın nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="c86ec-129">This tutorial demonstrates how to create an application that shares the state of an object with other browsers in real time.</span></span> <span data-ttu-id="c86ec-130">Uygulama oluşturacağız MoveShape çağrılır.</span><span class="sxs-lookup"><span data-stu-id="c86ec-130">The application we'll create is called MoveShape.</span></span> <span data-ttu-id="c86ec-131">Kullanıcı sürükleyebilirsiniz bir HTML Div öğesi MoveShape sayfası görüntüler; Kullanıcı Div sürüklediğinde, yeni konumuna ardından eşleştirilecek şeklin konum güncelleştirmek için diğer tüm bağlı istemcileri söyleyecektir sunucuya gönderilir.</span><span class="sxs-lookup"><span data-stu-id="c86ec-131">The MoveShape page will display an HTML Div element that the user can drag; when the user drags the Div, its new position will be sent to the server, which will then tell all other connected clients to update the shape's position to match.</span></span>

![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

<span data-ttu-id="c86ec-133">Bu öğreticide oluşturulan uygulama tarafından Damian Edwards bir tanıtım temel alır.</span><span class="sxs-lookup"><span data-stu-id="c86ec-133">The application created in this tutorial is based on a demo by Damian Edwards.</span></span> <span data-ttu-id="c86ec-134">Bu Tanıtım içeren bir video görülebilir [burada](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).</span><span class="sxs-lookup"><span data-stu-id="c86ec-134">A video containing this demo can be seen [here](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).</span></span>

<span data-ttu-id="c86ec-135">Öğreticiyi şekli sürüklediğiniz olarak çalıştırılan her olaydan SignalR iletileri göndermek nasıl yazılacağını gösteren tarafından başlatılır.</span><span class="sxs-lookup"><span data-stu-id="c86ec-135">The tutorial will start by demonstrating how to send SignalR messages from each event that fires as the shape is dragged.</span></span> <span data-ttu-id="c86ec-136">Her bağlı istemci, her zaman bir ileti alındığında sonra yerel sürüm şeklin konumunu güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="c86ec-136">Each connected client will then update the position of the local version of the shape each time a message is received.</span></span>

<span data-ttu-id="c86ec-137">Bu yöntemi kullanarak uygulama çalışır durumdayken olacaktır, böylece istemciler ve sunucu iletileri ile dolmasını ve performans düşmesine neden gönderilen ileti sayısı için üst sınır bu yana bu önerilen bir programlama modeli değil .</span><span class="sxs-lookup"><span data-stu-id="c86ec-137">While the application will function using this method, this is not a recommended programming model, since there would be no upper limit to the number of messages getting sent, so the clients and server could get overwhelmed with messages and performance would degrade.</span></span> <span data-ttu-id="c86ec-138">İstemci üzerinde görüntülenen animasyonu da şekli anında her yöntemle taşındı ayrık yerine her yeni konuma sorunsuz taşıma olacaktır.</span><span class="sxs-lookup"><span data-stu-id="c86ec-138">The displayed animation on the client would also be disjointed, as the shape would be moved instantly by each method, rather than moving smoothly to each new location.</span></span> <span data-ttu-id="c86ec-139">Öğreticinin sonraki bölümlerde, iletileri istemci veya sunucu tarafından gönderilen en yüksek hızı kısıtlayan bir zamanlayıcı işlevinin nasıl oluşturulacağı ve şekli sorunsuz konumlar arasında taşıma gösterilecektir.</span><span class="sxs-lookup"><span data-stu-id="c86ec-139">Later sections of the tutorial will demonstrate how to create a timer function that restricts the maximum rate at which messages are sent by either the client or server, and how to move the shape smoothly between locations.</span></span> <span data-ttu-id="c86ec-140">Bu öğreticide oluşturduğunuz uygulamanın son sürümü yüklenebilir [kod Galerisi](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).</span><span class="sxs-lookup"><span data-stu-id="c86ec-140">The final version of the application created in this tutorial can be downloaded from [Code Gallery](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).</span></span>

<span data-ttu-id="c86ec-141">Bu öğreticide, aşağıdaki bölümleri içerir:</span><span class="sxs-lookup"><span data-stu-id="c86ec-141">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="c86ec-142">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="c86ec-142">Prerequisites</span></span>](#prerequisites)
- [<span data-ttu-id="c86ec-143">Projeyi oluşturmak ve SignalR ve JQuery.UI NuGet paketi ekleme</span><span class="sxs-lookup"><span data-stu-id="c86ec-143">Create the project and add the SignalR and JQuery.UI NuGet package</span></span>](#createtheproject2013)
- [<span data-ttu-id="c86ec-144">Temel uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="c86ec-144">Create the base application</span></span>](#baseapp)
- [<span data-ttu-id="c86ec-145">Uygulama başladığında hub'ı başlatma</span><span class="sxs-lookup"><span data-stu-id="c86ec-145">Starting the hub when the application starts</span></span>](#startup2013)
- [<span data-ttu-id="c86ec-146">İstemci döngü Ekle</span><span class="sxs-lookup"><span data-stu-id="c86ec-146">Add the client loop</span></span>](#clientloop)
- [<span data-ttu-id="c86ec-147">Sunucu döngü Ekle</span><span class="sxs-lookup"><span data-stu-id="c86ec-147">Add the server loop</span></span>](#serverloop)
- [<span data-ttu-id="c86ec-148">İstemcide düzgün animasyon ekleme</span><span class="sxs-lookup"><span data-stu-id="c86ec-148">Add smooth animation on the client</span></span>](#animation)
- [<span data-ttu-id="c86ec-149">Başka bir adım</span><span class="sxs-lookup"><span data-stu-id="c86ec-149">Further Steps</span></span>](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="c86ec-150">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="c86ec-150">Prerequisites</span></span>

<span data-ttu-id="c86ec-151">Bu öğretici, Visual Studio 2013 gerektirir.</span><span class="sxs-lookup"><span data-stu-id="c86ec-151">This tutorial requires Visual Studio 2013.</span></span>

<a id="createtheproject2013"></a>

## <a name="create-the-project-and-add-the-signalr-and-jqueryui-nuget-package"></a><span data-ttu-id="c86ec-152">Projeyi oluşturmak ve SignalR ve JQuery.UI NuGet paketi ekleme</span><span class="sxs-lookup"><span data-stu-id="c86ec-152">Create the project and add the SignalR and JQuery.UI NuGet package</span></span>

<span data-ttu-id="c86ec-153">Bu bölümde, projenin Visual Studio 2013'te oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="c86ec-153">In this section, we'll create the project in Visual Studio 2013.</span></span>

<span data-ttu-id="c86ec-154">ASP.NET boş Web uygulaması oluşturma ve SignalR ve jQuery.UI kitaplıkları eklemek için Visual Studio 2013'ün aşağıdaki adımları kullanın:</span><span class="sxs-lookup"><span data-stu-id="c86ec-154">The following steps use Visual Studio 2013 to create an ASP.NET Empty Web Application and add the SignalR and jQuery.UI libraries:</span></span>

1. <span data-ttu-id="c86ec-155">Visual Studio'da ASP.NET Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c86ec-155">In Visual Studio create an ASP.NET Web Application.</span></span>

    ![Web oluşturma](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)
2. <span data-ttu-id="c86ec-157">İçinde **yeni ASP.NET projesi** penceresinde bırakın **boş** tıklatın ve seçili **proje oluştur**.</span><span class="sxs-lookup"><span data-stu-id="c86ec-157">In the **New ASP.NET Project** window, leave **Empty** selected and click **Create Project**.</span></span>

    ![Boş web oluşturma](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
3. <span data-ttu-id="c86ec-159">İçinde **Çözüm Gezgini**, projeye sağ tıklayın, **Ekle | SignalR Hub sınıfı (v2)**.</span><span class="sxs-lookup"><span data-stu-id="c86ec-159">In **Solution Explorer**, right-click the project, select **Add | SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="c86ec-160">Sınıf adı **MoveShapeHub.cs** ve projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c86ec-160">Name the class **MoveShapeHub.cs** and add it to the project.</span></span> <span data-ttu-id="c86ec-161">Bu adımda oluşturulur **MoveShapeHub** sınıfı ve bir dizi komut dosyaları ve Signalr'yi destekleyen bir bütünleştirilmiş kod başvuruları projeye ekler.</span><span class="sxs-lookup"><span data-stu-id="c86ec-161">This step creates the **MoveShapeHub** class and adds to the project a set of script files and assembly references that support SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c86ec-162">Tıklayarak bir projeye SignalR ekleyebilirsiniz **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu** ve bir komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="c86ec-162">You can also add SignalR to a project by clicking **Tools > NuGet Package Manager > Package Manager Console** and running a command:</span></span>

    <span data-ttu-id="c86ec-163">`install-package Microsoft.AspNet.SignalR`.</span><span class="sxs-lookup"><span data-stu-id="c86ec-163">`install-package Microsoft.AspNet.SignalR`.</span></span>

    <span data-ttu-id="c86ec-164">SignalR eklemek için konsolunu kullanırsanız, SignalR hub sınıfı SignalR ekledikten sonra ayrı bir adım olarak oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c86ec-164">If you use the console to add SignalR, create the SignalR hub class as a separate step after you add SignalR.</span></span>
4. <span data-ttu-id="c86ec-165">Tıklayın **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="c86ec-165">Click **Tools > NuGet Package Manager > Package Manager Console**.</span></span> <span data-ttu-id="c86ec-166">Paket Yöneticisi penceresinde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="c86ec-166">In the package manager window, run the following command:</span></span>

    `Install-Package jQuery.UI.Combined`

    <span data-ttu-id="c86ec-167">Bu şeklin animasyon uygulamak için kullanacağınız jQuery kullanıcı Arabirimi kitaplığı yükler.</span><span class="sxs-lookup"><span data-stu-id="c86ec-167">This installs the jQuery UI library, which you'll use to animate the shape.</span></span>
5. <span data-ttu-id="c86ec-168">İçinde **Çözüm Gezgini** betikleri düğümünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="c86ec-168">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="c86ec-169">Betik kitaplıkları için jQuery jQueryUI ve SignalR projesinde görünür.</span><span class="sxs-lookup"><span data-stu-id="c86ec-169">Script libraries for jQuery, jQueryUI, and SignalR are visible in the project.</span></span>

    ![Komut dosyası kitaplık başvuruları](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="baseapp"></a>

## <a name="create-the-base-application"></a><span data-ttu-id="c86ec-171">Temel uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="c86ec-171">Create the base application</span></span>

<span data-ttu-id="c86ec-172">Bu bölümde, Şekil konumunu sunucuya her fare hareketi olayını sırasında gönderir. bir tarayıcı uygulaması oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="c86ec-172">In this section, we'll create a browser application that sends the location of the shape to the server during each mouse move event.</span></span> <span data-ttu-id="c86ec-173">Alınan sunucu daha sonra diğer tüm bağlı istemcileri bu bilgileri yayınlar.</span><span class="sxs-lookup"><span data-stu-id="c86ec-173">The server then broadcasts this information to all other connected clients as it is received.</span></span> <span data-ttu-id="c86ec-174">Sonraki bölümlerde bu uygulamada şirket genişletin.</span><span class="sxs-lookup"><span data-stu-id="c86ec-174">We'll expand on this application in later sections.</span></span>

1. <span data-ttu-id="c86ec-175">Zaten MoveShapeHub.cs sınıf oluşturduğunuz yapmadıysanız, **Çözüm Gezgini**, projeye sağ tıklayıp **Ekle**, **sınıfı...** . Sınıf adı **MoveShapeHub** tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="c86ec-175">If you haven't already created the MoveShapeHub.cs class, in **Solution Explorer**, right-click on the project and select **Add**, **Class...**. Name the class **MoveShapeHub** and click **Add**.</span></span>
2. <span data-ttu-id="c86ec-176">Yeni bir kodu **MoveShapeHub** aşağıdaki kodla sınıfı.</span><span class="sxs-lookup"><span data-stu-id="c86ec-176">Replace the code in the new **MoveShapeHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="c86ec-177">`MoveShapeHub` Yukarıdaki sınıfı, bir SignalR hub'ı uygulaması.</span><span class="sxs-lookup"><span data-stu-id="c86ec-177">The `MoveShapeHub` class above is an implementation of a SignalR hub.</span></span> <span data-ttu-id="c86ec-178">Olarak [SignalR ile çalışmaya başlama](tutorial-getting-started-with-signalr.md) Öğreticisi, hub'ına istemcileri doğrudan çağıran bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="c86ec-178">As in the [Getting Started with SignalR](tutorial-getting-started-with-signalr.md) tutorial, the hub has a method that the clients will call directly.</span></span> <span data-ttu-id="c86ec-179">Bu durumda, istemci içeren yeni bir nesne gönderir sonra diğer tüm bağlı istemcileri yayımladınız sunucunun şekle X ve Y koordinatları.</span><span class="sxs-lookup"><span data-stu-id="c86ec-179">In this case, the client will send an object containing the new X and Y coordinates of the shape to the server, which then gets broadcasted to all other connected clients.</span></span> <span data-ttu-id="c86ec-180">SignalR, otomatik olarak JSON'ı kullanarak bu nesneyi serileştirmek.</span><span class="sxs-lookup"><span data-stu-id="c86ec-180">SignalR will automatically serialize this object using JSON.</span></span>

    <span data-ttu-id="c86ec-181">İstemciye gönderilecek nesne (`ShapeModel`) şekil konumunu saklamak için üyeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="c86ec-181">The object that will be sent to the client (`ShapeModel`) contains members to store the position of the shape.</span></span> <span data-ttu-id="c86ec-182">Belirli bir istemci, kendi veri gönderilmez, böylece nesnenin sunucuda hangi istemci verilerinin depolandığını, izlemek için bir üyesi de içerir.</span><span class="sxs-lookup"><span data-stu-id="c86ec-182">The version of the object on the server also contains a member to track which client's data is being stored, so that a given client won't be sent their own data.</span></span> <span data-ttu-id="c86ec-183">Bu üyeyi kullanan `JsonIgnore` , serileştirilmiş ve istemciye gönderilen tutmak için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="c86ec-183">This member uses the `JsonIgnore` attribute to keep it from being serialized and sent to the client.</span></span>

<a id="startup2013"></a>
## <a name="starting-the-hub-when-the-application-starts"></a><span data-ttu-id="c86ec-184">Uygulama başladığında hub'ı başlatma</span><span class="sxs-lookup"><span data-stu-id="c86ec-184">Starting the hub when the application starts</span></span>

1. <span data-ttu-id="c86ec-185">Uygulama başladığında ardından, biz hub'ına eşleme ayarlarsınız.</span><span class="sxs-lookup"><span data-stu-id="c86ec-185">Next, we'll set up mapping to the hub when the application starts.</span></span> <span data-ttu-id="c86ec-186">SignalR 2'de bu çağıracak bir OWIN başlangıç sınıfı ekleyerek yapılır `MapSignalR` olduğunda Başlangıç sınıfı `Configuration` yöntemi OWIN başladığında yürütülür.</span><span class="sxs-lookup"><span data-stu-id="c86ec-186">In SignalR 2, this is done by adding an OWIN startup class, which will call `MapSignalR` when the startup class' `Configuration` method is executed when OWIN starts.</span></span> <span data-ttu-id="c86ec-187">Sınıf için OWIN'ın başlangıç eklenir oluşturabilirsiniz.computercube `OwinStartup` derleme özniteliği.</span><span class="sxs-lookup"><span data-stu-id="c86ec-187">The class is added to OWIN's startup process using the `OwinStartup` assembly attribute.</span></span>

    <span data-ttu-id="c86ec-188">İçinde **Çözüm Gezgini**, projeyi sağ tıklatın ve ardından **Ekle | OWIN başlangıç sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="c86ec-188">In **Solution Explorer**, right-click the project, then click **Add | OWIN Startup Class**.</span></span> <span data-ttu-id="c86ec-189">Sınıf adı *başlangıç* tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="c86ec-189">Name the class *Startup* and click **OK**.</span></span>
2. <span data-ttu-id="c86ec-190">Startup.cs içeriğini aşağıdaki gibi değiştirin:</span><span class="sxs-lookup"><span data-stu-id="c86ec-190">Change the contents of Startup.cs to the following:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<a id="client"></a>
## <a name="adding-the-client"></a><span data-ttu-id="c86ec-191">İstemci ekleme</span><span class="sxs-lookup"><span data-stu-id="c86ec-191">Adding the client</span></span>

1. <span data-ttu-id="c86ec-192">Ardından, istemci ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="c86ec-192">Next, we'll add the client.</span></span> <span data-ttu-id="c86ec-193">İçinde **Çözüm Gezgini**, projeyi sağ tıklatın ve ardından **Ekle | Yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="c86ec-193">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="c86ec-194">İçinde **Yeni Öğe Ekle** iletişim kutusunda **Html sayfası**.</span><span class="sxs-lookup"><span data-stu-id="c86ec-194">In the **Add New Item** dialog, select **Html Page**.</span></span> <span data-ttu-id="c86ec-195">Sayfayı adlandırın **Default.html** tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="c86ec-195">Name the page **Default.html** and click **Add**.</span></span>
2. <span data-ttu-id="c86ec-196">İçinde **Çözüm Gezgini**, yeni oluşturduğunuz sayfanın sağ tıklatıp **Başlangıç Sayfası Ayarla**.</span><span class="sxs-lookup"><span data-stu-id="c86ec-196">In **Solution Explorer**, right-click the page you just created and click **Set as Start Page**.</span></span>
3. <span data-ttu-id="c86ec-197">Varsayılan HTML sayfasını aşağıdaki kod parçacığı ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="c86ec-197">Replace the default code in the HTML page with the following code snippet.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c86ec-198">Komut dosyası projenize betikler klasörüne eklenen paketler eşleşme başvurduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="c86ec-198">Verify that the script references below match the packages added to your project in the Scripts folder.</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html)]

    <span data-ttu-id="c86ec-199">Yukarıdaki HTML ve JavaScript kodunu adlı şekli kırmızı bir Div oluşturur, jQuery kitaplığını kullanarak şeklin sürükleyerek davranışını etkinleştirir ve şeklin `drag` şeklin konum sunucuya göndermek için olay.</span><span class="sxs-lookup"><span data-stu-id="c86ec-199">The above HTML and JavaScript code creates a red Div called Shape, enables the shape's dragging behavior using the jQuery library, and uses the shape's `drag` event to send the shape's position to the server.</span></span>
4. <span data-ttu-id="c86ec-200">F5 tuşuna basarak uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="c86ec-200">Start the application by pressing F5.</span></span> <span data-ttu-id="c86ec-201">Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="c86ec-201">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="c86ec-202">Şekil tarayıcı pencerelerini birinde sürükleyin. Şekil bir tarayıcı penceresinde taşımanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c86ec-202">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span>

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a><span data-ttu-id="c86ec-204">İstemci döngü Ekle</span><span class="sxs-lookup"><span data-stu-id="c86ec-204">Add the client loop</span></span>

<span data-ttu-id="c86ec-205">Her fare hareketi olayını şeklinizde konumunu gönderme gerekli olmayan miktarda ağ trafiği oluşturur, istemciden gelen iletileri kısıtlanabilir gerekir.</span><span class="sxs-lookup"><span data-stu-id="c86ec-205">Since sending the location of the shape on every mouse move event will create an unneccesary amount of network traffic, the messages from the client need to be throttled.</span></span> <span data-ttu-id="c86ec-206">Javascript kullanacağız `setInterval` sabit bir hızda sunucuya yeni konum bilgileri gönderen bir döngü ayarlamak için işlevi.</span><span class="sxs-lookup"><span data-stu-id="c86ec-206">We'll use the javascript `setInterval` function to set up a loop that sends new position information to the server at a fixed rate.</span></span> <span data-ttu-id="c86ec-207">Bu döngü bir "Oyun döngüsü", tüm işlevlerin bir oyun veya diğer benzetimi sürücüleri tekrar tekrar çağrılan bir işlevin çok basit bir gösterimidir.</span><span class="sxs-lookup"><span data-stu-id="c86ec-207">This loop is a very basic representation of a "game loop", a repeatedly called function that drives all of the functionality of a game or other simulation.</span></span>

1. <span data-ttu-id="c86ec-208">İstemci kodu HTML sayfasındaki, aşağıdaki kod parçacığı eşleşecek şekilde güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="c86ec-208">Update the client code in the HTML page to match the following code snippet.</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html)]

    <span data-ttu-id="c86ec-209">Yukarıdaki güncelleştirme ekler `updateServerModel` işlevi çağrılan bir sabit sıklığı temel.</span><span class="sxs-lookup"><span data-stu-id="c86ec-209">The above update adds the `updateServerModel` function, which gets called on a fixed frequency.</span></span> <span data-ttu-id="c86ec-210">Bu işlev, sunucuya konum verileri gönderir her `moved` bayrağı, yeni konum verileri göndermek için olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="c86ec-210">This function sends the position data to the server whenever the `moved` flag indicates that there is new position data to send.</span></span>
2. <span data-ttu-id="c86ec-211">F5 tuşuna basarak uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="c86ec-211">Start the application by pressing F5.</span></span> <span data-ttu-id="c86ec-212">Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="c86ec-212">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="c86ec-213">Şekil tarayıcı pencerelerini birinde sürükleyin. Şekil bir tarayıcı penceresinde taşımanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c86ec-213">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="c86ec-214">Animasyon, sunucuya gönderilen ileti sayısını kısıtlanacak olduğundan, önceki bölümdeki kesintisiz olarak görünmez.</span><span class="sxs-lookup"><span data-stu-id="c86ec-214">Since the number of messages that get sent to the server will be throttled, the animation will not appear as smooth as in the previous section.</span></span>

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a><span data-ttu-id="c86ec-216">Sunucu döngü Ekle</span><span class="sxs-lookup"><span data-stu-id="c86ec-216">Add the server loop</span></span>

<span data-ttu-id="c86ec-217">Geçerli uygulamada, sunucudan istemciye gönderilen iletileri alındıkları sıklıkta gönderilir.</span><span class="sxs-lookup"><span data-stu-id="c86ec-217">In the current application, messages sent from the server to the client go out as often as they are received.</span></span> <span data-ttu-id="c86ec-218">İstemcide görülen benzer bir sorun gösterir; iletiler daha sık gerekli olan ve bağlantı sonucunda yayılmamış olabilir gönderilebilir.</span><span class="sxs-lookup"><span data-stu-id="c86ec-218">This presents a similar problem as was seen on the client; messages can be sent more often than they are needed, and the connection could become flooded as a result.</span></span> <span data-ttu-id="c86ec-219">Bu bölümde, giden iletiler oranını kısıtlar bir zamanlayıcı uygulamak için sunucu güncelleştirme işlemi açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="c86ec-219">This section describes how to update the server to implement a timer that throttles the rate of the outgoing messages.</span></span>

1. <span data-ttu-id="c86ec-220">Öğesinin içeriğini değiştirin `MoveShapeHub.cs` aşağıdaki kod parçacığı ile.</span><span class="sxs-lookup"><span data-stu-id="c86ec-220">Replace the contents of `MoveShapeHub.cs` with the following code snippet.</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    <span data-ttu-id="c86ec-221">Yukarıdaki kod eklemek için istemci genişletir `Broadcaster` giden kısıtlar sınıfı iletileri kullanarak `Timer` .NET Framework sınıfı.</span><span class="sxs-lookup"><span data-stu-id="c86ec-221">The above code expands the client to add the `Broadcaster` class, which throttles the outgoing messages using the `Timer` class from the .NET framework.</span></span>

    <span data-ttu-id="c86ec-222">Hub geçici olduğundan (her zaman gerekli oluşturulduktan), `Broadcaster` tekil olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c86ec-222">Since the hub itself is transitory (it is created every time it is needed), the `Broadcaster` will be created as a singleton.</span></span> <span data-ttu-id="c86ec-223">Bu, Zamanlayıcıyı başlatılmadan önce ilk hub örneği tamamen oluşturulduktan gerekli kadar oluşturulduktan erteleneceği yavaş başlatma (.NET 4'te sunulmuştur) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c86ec-223">Lazy initialization (introduced in .NET 4) is used to defer its creation until it is needed, ensuring that the first hub instance is completely created before the timer is started.</span></span>

    <span data-ttu-id="c86ec-224">İstemcilerin çağrısı `UpdateShape` işlevi ardından hub dışında taşınmış `UpdateModel` yöntemi, böylece onun artık hemen gelen mesajların alındığı zaman çağrılır.</span><span class="sxs-lookup"><span data-stu-id="c86ec-224">The call to the clients' `UpdateShape` function is then moved out of the hub's `UpdateModel` method, so that it is no longer called immediately whenever incoming messages are received.</span></span> <span data-ttu-id="c86ec-225">Bunun yerine, istemciler için iletileri, saniye başına 25 çağrılarının bir hızda gönderilecek tarafından yönetilen `_broadcastLoop` içinden Zamanlayıcı `Broadcaster` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="c86ec-225">Instead, the messages to the clients will be sent at a rate of 25 calls per second, managed by the `_broadcastLoop` timer from within the `Broadcaster` class.</span></span>

    <span data-ttu-id="c86ec-226">Son olarak, istemci yöntemi hub'dan doğrudan çağırmak yerine `Broadcaster` sınıfı gerekiyor şu anda işletim hub'ına bir başvuru almak (`_hubContext`) kullanarak `GlobalHost`.</span><span class="sxs-lookup"><span data-stu-id="c86ec-226">Lastly, instead of calling the client method from the hub directly, the `Broadcaster` class needs to obtain a reference to the currently operating hub (`_hubContext`) using the `GlobalHost`.</span></span>
2. <span data-ttu-id="c86ec-227">F5 tuşuna basarak uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="c86ec-227">Start the application by pressing F5.</span></span> <span data-ttu-id="c86ec-228">Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="c86ec-228">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="c86ec-229">Şekil tarayıcı pencerelerini birinde sürükleyin. Şekil bir tarayıcı penceresinde taşımanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c86ec-229">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="c86ec-230">Önceki bölümde tarayıcınızda görünür bir farkı olmayacak, ancak istemciye gönderilen ileti sayısını kısıtlanır.</span><span class="sxs-lookup"><span data-stu-id="c86ec-230">There will not be a visible difference in the browser from the previous section, but the number of messages that get sent to the client will be throttled.</span></span>

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a><span data-ttu-id="c86ec-232">İstemcide düzgün animasyon ekleme</span><span class="sxs-lookup"><span data-stu-id="c86ec-232">Add smooth animation on the client</span></span>

<span data-ttu-id="c86ec-233">Uygulama neredeyse tamamlandı, ancak biz yanıt olarak sunucu ileti taşınırken bir daha fazla geliştirme istemcide şeklin halindeki yapabilir.</span><span class="sxs-lookup"><span data-stu-id="c86ec-233">The application is almost complete, but we could make one more improvement, in the motion of the shape on the client as it is moved in response to server messages.</span></span> <span data-ttu-id="c86ec-234">Sunucu tarafından verilen yeni bir konuma şekil konumunu ayarlamak yerine JQuery kullanıcı Arabirimi kitaplığın kullanacağız `animate` şekli sorunsuz geçerli ve yeni konumu arasında taşımak için işlevi.</span><span class="sxs-lookup"><span data-stu-id="c86ec-234">Rather than setting the position of the shape to the new location given by the server, we'll use the JQuery UI library's `animate` function to move the shape smoothly between its current and new position.</span></span>

1. <span data-ttu-id="c86ec-235">İstemci güncelleştirme `updateShape` yöntemi aramak için aşağıdaki vurgulanmış kodu ister:</span><span class="sxs-lookup"><span data-stu-id="c86ec-235">Update the client's `updateShape` method to look like the highlighted code below:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

    <span data-ttu-id="c86ec-236">Yukarıdaki kod şekli eski konumundan (Bu durumda, 100 milisaniye) animasyon aralığı boyunca sunucu tarafından verilen yeni bir tane taşır.</span><span class="sxs-lookup"><span data-stu-id="c86ec-236">The above code moves the shape from the old location to the new one given by the server over the course of the animation interval (in this case, 100 milliseconds).</span></span> <span data-ttu-id="c86ec-237">Yeni animasyon başlatılmadan önce şekli üzerinde çalışan herhangi bir önceki animasyon temizlenir.</span><span class="sxs-lookup"><span data-stu-id="c86ec-237">Any previous animation running on the shape is cleared before the new animation starts.</span></span>
2. <span data-ttu-id="c86ec-238">F5 tuşuna basarak uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="c86ec-238">Start the application by pressing F5.</span></span> <span data-ttu-id="c86ec-239">Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="c86ec-239">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="c86ec-240">Şekil tarayıcı pencerelerini birinde sürükleyin. Şekil bir tarayıcı penceresinde taşımanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c86ec-240">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="c86ec-241">Diğer pencere şeklinde hareketini hareketini gelen ileti başına bir kez ayarlanan yerine zaman içinde ilişkilendirilmiş daha az düzensiz görüntülenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="c86ec-241">The movement of the shape in the other window should appear less jerky as its movement is interpolated over time rather than being set once per incoming message.</span></span>

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image8.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a><span data-ttu-id="c86ec-243">Başka bir adım</span><span class="sxs-lookup"><span data-stu-id="c86ec-243">Further Steps</span></span>

<span data-ttu-id="c86ec-244">Bu öğreticide, istemciler ve sunucular arasında yüksek frekanslı iletiler gönderen bir SignalR uygulama programı öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="c86ec-244">In this tutorial, you've learned how to program a SignalR application that sends high-frequency messages between clients and servers.</span></span> <span data-ttu-id="c86ec-245">Bu iletişim paradigma çevrimiçi oyunlar ve diğer simülasyonları gibi geliştirmek için kullanışlıdır [SignalR ile oluşturulan ShootR game](https://shootr.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="c86ec-245">This communication paradigm is useful for developing online games and other simulations, such as [the ShootR game created with SignalR](https://shootr.azurewebsites.net/).</span></span>

<span data-ttu-id="c86ec-246">Bu öğreticide oluşturduğunuz uygulamanın indirilebileceğini [kod Galerisi](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).</span><span class="sxs-lookup"><span data-stu-id="c86ec-246">The complete application created in this tutorial can be downloaded from [Code Gallery](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).</span></span>

<span data-ttu-id="c86ec-247">SignalR geliştirme kavramları hakkında daha fazla bilgi edinmek için SignalR kaynak kodu ve kaynaklar için aşağıdaki siteleri ziyaret edin:</span><span class="sxs-lookup"><span data-stu-id="c86ec-247">To learn more about SignalR development concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="c86ec-248">SignalR projesi</span><span class="sxs-lookup"><span data-stu-id="c86ec-248">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="c86ec-249">SignalR Github ve örnekleri</span><span class="sxs-lookup"><span data-stu-id="c86ec-249">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="c86ec-250">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="c86ec-250">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

<span data-ttu-id="c86ec-251">Bir SignalR uygulamayı azure'a dağıtma hakkında kılavuz için bkz. [Azure App service'taki Web Apps ile SignalR kullanarak](../deployment/using-signalr-with-azure-web-sites.md).</span><span class="sxs-lookup"><span data-stu-id="c86ec-251">For a walkthrough on how to deploy a SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="c86ec-252">Visual Studio web projesini bir Windows Azure Web sitesine dağıtma hakkında ayrıntılı bilgi için bkz. [Azure App Service'te ASP.NET web uygulaması oluşturma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="c86ec-252">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>
