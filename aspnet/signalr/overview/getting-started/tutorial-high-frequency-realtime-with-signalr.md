---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: "Öğretici: Yüksek sıklıkta gerçek zamanlı SignalR 2 ile | Microsoft Docs"
author: pfletcher
description: "Bu öğretici yüksek sıklıkta Mesajlaşma işlevleri sağlamak için ASP.NET SignalR kullanan bir web uygulamasının nasıl oluşturulacağını gösterir. Yüksek içinde ileti sıklıkta..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ab051b2ab85d1aac1e7179f342f22f470b1d1cc7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-high-frequency-realtime-with-signalr-2"></a><span data-ttu-id="37dc3-104">Öğretici: Yüksek sıklıkta gerçek zamanlı SignalR 2 ile</span><span class="sxs-lookup"><span data-stu-id="37dc3-104">Tutorial: High-Frequency Realtime with SignalR 2</span></span>
====================
<span data-ttu-id="37dc3-105">tarafından [CAN Fletcher'dan](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="37dc3-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="37dc3-106">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="37dc3-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

> <span data-ttu-id="37dc3-107">Bu öğretici yüksek sıklıkta Mesajlaşma işlevleri sağlamak için ASP.NET SignalR 2 kullanan bir web uygulamasının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="37dc3-107">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span> <span data-ttu-id="37dc3-108">Yüksek yoğunlukta Mesajlaşma bu durumda sabit bir oranda gönderilen güncelleştirmeler anlamına gelir; Bu uygulama söz konusu olduğunda, 10'a kadar saniyenin iletileri.</span><span class="sxs-lookup"><span data-stu-id="37dc3-108">High-frequency messaging in this case means updates that are sent at a fixed rate; in the case of this application, up to 10 messages a second.</span></span>
> 
> <span data-ttu-id="37dc3-109">Bu öğreticide oluşturacaksınız uygulama kullanıcıların sürükleyebilirsiniz bir şekli görüntüler.</span><span class="sxs-lookup"><span data-stu-id="37dc3-109">The application you'll create in this tutorial displays a shape that users can drag.</span></span> <span data-ttu-id="37dc3-110">Şeklin konumunu diğer bağlı tarayıcılarda Zamanlanmış güncelleştirmeleri kullanarak sürüklenen şekli konumunu eşleşecek şekilde güncelleştirilecektir.</span><span class="sxs-lookup"><span data-stu-id="37dc3-110">The position of the shape in all other connected browsers will then be updated to match the position of the dragged shape using timed updates.</span></span>
> 
> <span data-ttu-id="37dc3-111">Bu öğreticide sunulan kavramlar gerçek zamanlı oyun uygulamaları ve diğer benzetimi uygulamalar vardır.</span><span class="sxs-lookup"><span data-stu-id="37dc3-111">Concepts introduced in this tutorial have applications in real-time gaming and other simulation applications.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="37dc3-112">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="37dc3-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="37dc3-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="37dc3-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="37dc3-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="37dc3-114">.NET 4.5</span></span>
> - <span data-ttu-id="37dc3-115">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="37dc3-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="37dc3-116">Bu öğretici ile Visual Studio 2012 kullanma</span><span class="sxs-lookup"><span data-stu-id="37dc3-116">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="37dc3-117">Visual Studio 2012 bu öğreticiyle kullanmak için aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="37dc3-117">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="37dc3-118">Güncelleştirme, [Paket Yöneticisi](http://docs.nuget.org/docs/start-here/installing-nuget) en son sürüme.</span><span class="sxs-lookup"><span data-stu-id="37dc3-118">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="37dc3-119">Yükleme [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="37dc3-119">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="37dc3-120">Web Platformu Yükleyicisi'nde arayın ve yükleyin **ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için**.</span><span class="sxs-lookup"><span data-stu-id="37dc3-120">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="37dc3-121">Bu SignalR sınıfları için Visual Studio şablonları gibi yükleyecek **Hub**.</span><span class="sxs-lookup"><span data-stu-id="37dc3-121">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="37dc3-122">Bazı şablonlar (gibi **OWIN başlangıç sınıfı**); kullanılamaz bunlar için bunun yerine bir sınıf dosyası kullanın.</span><span class="sxs-lookup"><span data-stu-id="37dc3-122">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="37dc3-123">Eğitmen sürümleri</span><span class="sxs-lookup"><span data-stu-id="37dc3-123">Tutorial Versions</span></span>
> 
> <span data-ttu-id="37dc3-124">SignalR daha önceki sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="37dc3-124">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="37dc3-125">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="37dc3-125">Questions and comments</span></span>
> 
> <span data-ttu-id="37dc3-126">Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi.</span><span class="sxs-lookup"><span data-stu-id="37dc3-126">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="37dc3-127">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="37dc3-127">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="37dc3-128">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="37dc3-128">Overview</span></span>

<span data-ttu-id="37dc3-129">Bu öğretici bir nesnenin durumunu gerçek zamanlı diğer tarayıcılarda paylaşan bir uygulamasının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="37dc3-129">This tutorial demonstrates how to create an application that shares the state of an object with other browsers in real time.</span></span> <span data-ttu-id="37dc3-130">Oluşturacağız uygulama MoveShape adı verilir.</span><span class="sxs-lookup"><span data-stu-id="37dc3-130">The application we'll create is called MoveShape.</span></span> <span data-ttu-id="37dc3-131">MoveShape sayfa kullanıcı sürükleyebilirsiniz bir HTML Div öğesinin görüntülenir; Kullanıcı Div sürüklendiğinde yeni konumunu ardından eşleştirilecek şeklin konumunu güncelleştirmek için diğer tüm bağlı istemcileri söyleyecektir sunucusuna gönderilir.</span><span class="sxs-lookup"><span data-stu-id="37dc3-131">The MoveShape page will display an HTML Div element that the user can drag; when the user drags the Div, its new position will be sent to the server, which will then tell all other connected clients to update the shape's position to match.</span></span>

![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

<span data-ttu-id="37dc3-133">Bu öğreticide oluşturduğunuz uygulama tarafından Damian Edirneli demo üzerinde temel alır.</span><span class="sxs-lookup"><span data-stu-id="37dc3-133">The application created in this tutorial is based on a demo by Damian Edwards.</span></span> <span data-ttu-id="37dc3-134">Bu Tanıtım içeren bir video görülebilir [burada](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).</span><span class="sxs-lookup"><span data-stu-id="37dc3-134">A video containing this demo can be seen [here](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).</span></span>

<span data-ttu-id="37dc3-135">Öğretici şekli sürüklenen gibi tetiklenen her olay SignalR iletileri göndermek nasıl gösteren tarafından başlatılır.</span><span class="sxs-lookup"><span data-stu-id="37dc3-135">The tutorial will start by demonstrating how to send SignalR messages from each event that fires as the shape is dragged.</span></span> <span data-ttu-id="37dc3-136">Her bağlı istemci sonra her zaman bir ileti alındığında şekli yerel sürümünü konumunu güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="37dc3-136">Each connected client will then update the position of the local version of the shape each time a message is received.</span></span>

<span data-ttu-id="37dc3-137">Uygulama bu yöntemi kullanarak çalışır, ancak olurdu bu yana, böylece istemciler ve sunucu iletileri ile çok sayıda ve performansının düşmesine neden gönderilen iletisi sayısı için üst sınır bu önerilen bir programlama modeli değil .</span><span class="sxs-lookup"><span data-stu-id="37dc3-137">While the application will function using this method, this is not a recommended programming model, since there would be no upper limit to the number of messages getting sent, so the clients and server could get overwhelmed with messages and performance would degrade.</span></span> <span data-ttu-id="37dc3-138">İstemci üzerinde görüntülenen animasyon de kopuk şekli hemen her yöntemi tarafından taşındı yerine her yeni konuma sorunsuz taşıma olacaktır.</span><span class="sxs-lookup"><span data-stu-id="37dc3-138">The displayed animation on the client would also be disjointed, as the shape would be moved instantly by each method, rather than moving smoothly to each new location.</span></span> <span data-ttu-id="37dc3-139">Öğreticinin sonraki bölümlerde iletileri istemci veya sunucu tarafından gönderilme en yüksek hızı kısıtlayan bir zamanlayıcı işlevi oluşturma ve Şekil sorunsuz konumlar arasında taşıma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="37dc3-139">Later sections of the tutorial will demonstrate how to create a timer function that restricts the maximum rate at which messages are sent by either the client or server, and how to move the shape smoothly between locations.</span></span> <span data-ttu-id="37dc3-140">Bu öğreticide oluşturduğunuz uygulamanın son sürümü karşıdan yüklenebilir [kod Galerisi'nden](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).</span><span class="sxs-lookup"><span data-stu-id="37dc3-140">The final version of the application created in this tutorial can be downloaded from [Code Gallery](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).</span></span>

<span data-ttu-id="37dc3-141">Bu öğretici aşağıdaki bölümleri içerir:</span><span class="sxs-lookup"><span data-stu-id="37dc3-141">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="37dc3-142">Önkoşulları</span><span class="sxs-lookup"><span data-stu-id="37dc3-142">Prerequisites</span></span>](#prerequisites)
- [<span data-ttu-id="37dc3-143">Proje oluşturma ve SignalR ve JQuery.UI NuGet paketi ekleme</span><span class="sxs-lookup"><span data-stu-id="37dc3-143">Create the project and add the SignalR and JQuery.UI NuGet package</span></span>](#createtheproject2013)
- [<span data-ttu-id="37dc3-144">Temel uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="37dc3-144">Create the base application</span></span>](#baseapp)
- [<span data-ttu-id="37dc3-145">Uygulama başladığında hub başlatılıyor</span><span class="sxs-lookup"><span data-stu-id="37dc3-145">Starting the hub when the application starts</span></span>](#startup2013)
- [<span data-ttu-id="37dc3-146">İstemci döngü ekleme</span><span class="sxs-lookup"><span data-stu-id="37dc3-146">Add the client loop</span></span>](#clientloop)
- [<span data-ttu-id="37dc3-147">Sunucu döngü ekleme</span><span class="sxs-lookup"><span data-stu-id="37dc3-147">Add the server loop</span></span>](#serverloop)
- [<span data-ttu-id="37dc3-148">İstemcide düzgün animasyon ekleme</span><span class="sxs-lookup"><span data-stu-id="37dc3-148">Add smooth animation on the client</span></span>](#animation)
- [<span data-ttu-id="37dc3-149">Başka bir adım</span><span class="sxs-lookup"><span data-stu-id="37dc3-149">Further Steps</span></span>](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="37dc3-150">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="37dc3-150">Prerequisites</span></span>

<span data-ttu-id="37dc3-151">Bu öğreticide Visual Studio 2013 gerektirir.</span><span class="sxs-lookup"><span data-stu-id="37dc3-151">This tutorial requires Visual Studio 2013.</span></span>

<a id="createtheproject2013"></a>

## <a name="create-the-project-and-add-the-signalr-and-jqueryui-nuget-package"></a><span data-ttu-id="37dc3-152">Proje oluşturma ve SignalR ve JQuery.UI NuGet paketi ekleme</span><span class="sxs-lookup"><span data-stu-id="37dc3-152">Create the project and add the SignalR and JQuery.UI NuGet package</span></span>

<span data-ttu-id="37dc3-153">Bu bölümde, Visual Studio 2013 projesinde oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="37dc3-153">In this section, we'll create the project in Visual Studio 2013.</span></span>

<span data-ttu-id="37dc3-154">ASP.NET boş Web uygulaması oluşturmak ve SignalR ve jQuery.UI kitaplıkları eklemek için Visual Studio 2013 aşağıdaki adımları kullanın:</span><span class="sxs-lookup"><span data-stu-id="37dc3-154">The following steps use Visual Studio 2013 to create an ASP.NET Empty Web Application and add the SignalR and jQuery.UI libraries:</span></span>

1. <span data-ttu-id="37dc3-155">Visual Studio'da ASP.NET Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="37dc3-155">In Visual Studio create an ASP.NET Web Application.</span></span>

    ![Web oluşturulamıyor](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)
2. <span data-ttu-id="37dc3-157">İçinde **yeni ASP.NET projesi** penceresinde, bırakın **boş** 'ı tıklatın ve seçili **proje oluştur**.</span><span class="sxs-lookup"><span data-stu-id="37dc3-157">In the **New ASP.NET Project** window, leave **Empty** selected and click **Create Project**.</span></span>

    ![Boş web oluşturma](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
3. <span data-ttu-id="37dc3-159">İçinde **Çözüm Gezgini**, projeye sağ tıklayın, seçin **Ekle | SignalR hub'ı sınıfı (v2)**.</span><span class="sxs-lookup"><span data-stu-id="37dc3-159">In **Solution Explorer**, right-click the project, select **Add | SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="37dc3-160">Sınıf adını **MoveShapeHub.cs** ve bunu projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="37dc3-160">Name the class **MoveShapeHub.cs** and add it to the project.</span></span> <span data-ttu-id="37dc3-161">Bu adım oluşturur **MoveShapeHub** sınıfı ve bir dizi komut dosyaları ve Signalr'yi destekleyen derleme başvurularını projeye ekler.</span><span class="sxs-lookup"><span data-stu-id="37dc3-161">This step creates the **MoveShapeHub** class and adds to the project a set of script files and assembly references that support SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="37dc3-162">Tıklayarak projeye SignalR ekleyebilirsiniz **Araçlar | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu** ve bir komut çalıştırma:</span><span class="sxs-lookup"><span data-stu-id="37dc3-162">You can also add SignalR to a project by clicking **Tools | Library Package Manager | Package Manager Console** and running a command:</span></span>

    <span data-ttu-id="37dc3-163">`install-package Microsoft.AspNet.SignalR`.</span><span class="sxs-lookup"><span data-stu-id="37dc3-163">`install-package Microsoft.AspNet.SignalR`.</span></span> 

    <span data-ttu-id="37dc3-164">SignalR eklemek için konsol kullanırsanız, SignalR hub'ı sınıf SignalR ekledikten sonra ayrı bir adım olarak oluşturun.</span><span class="sxs-lookup"><span data-stu-id="37dc3-164">If you use the console to add SignalR, create the SignalR hub class as a separate step after you add SignalR.</span></span>
4. <span data-ttu-id="37dc3-165">Tıklatın **Araçlar | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="37dc3-165">Click **Tools | Library Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="37dc3-166">Paket Yöneticisi penceresinde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="37dc3-166">In the package manager window, run the following command:</span></span>

    `Install-Package jQuery.UI.Combined`

    <span data-ttu-id="37dc3-167">Bu şeklin animasyon için kullanacağınız jQuery kullanıcı Arabirimi kitaplığı yükler.</span><span class="sxs-lookup"><span data-stu-id="37dc3-167">This installs the jQuery UI library, which you'll use to animate the shape.</span></span>
5. <span data-ttu-id="37dc3-168">İçinde **Çözüm Gezgini** betikleri düğümünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="37dc3-168">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="37dc3-169">JQuery, jQueryUI ve SignalR için komut dosyası kitaplıkları projede görünür.</span><span class="sxs-lookup"><span data-stu-id="37dc3-169">Script libraries for jQuery, jQueryUI, and SignalR are visible in the project.</span></span>

    ![Komut dosyası Kitaplığı Başvurusu](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="baseapp"></a>

## <a name="create-the-base-application"></a><span data-ttu-id="37dc3-171">Temel uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="37dc3-171">Create the base application</span></span>

<span data-ttu-id="37dc3-172">Bu bölümde, Şekil konumunu sunucuya her fare hareketi olayını sırasında gönderir. bir tarayıcı uygulaması oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="37dc3-172">In this section, we'll create a browser application that sends the location of the shape to the server during each mouse move event.</span></span> <span data-ttu-id="37dc3-173">Alındığında sunucu daha sonra bu bilgileri diğer tüm bağlı istemcileri için yayınlar.</span><span class="sxs-lookup"><span data-stu-id="37dc3-173">The server then broadcasts this information to all other connected clients as it is received.</span></span> <span data-ttu-id="37dc3-174">Sonraki bölümlerde bu uygulamada üzerinde genişletin.</span><span class="sxs-lookup"><span data-stu-id="37dc3-174">We'll expand on this application in later sections.</span></span>

1. <span data-ttu-id="37dc3-175">Zaten MoveShapeHub.cs sınıfı içinde oluşturmadıysanız, **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **Ekle**, **sınıfı...** . Sınıf adını **MoveShapeHub** tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="37dc3-175">If you haven't already created the MoveShapeHub.cs class, in **Solution Explorer**, right-click on the project and select **Add**, **Class...**. Name the class **MoveShapeHub** and click **Add**.</span></span>
2. <span data-ttu-id="37dc3-176">Kod yeni değiştirin **MoveShapeHub** aşağıdaki kodla sınıfı.</span><span class="sxs-lookup"><span data-stu-id="37dc3-176">Replace the code in the new **MoveShapeHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="37dc3-177">`MoveShapeHub` Sınıfı yukarıdaki bir SignalR hub'ının bir uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="37dc3-177">The `MoveShapeHub` class above is an implementation of a SignalR hub.</span></span> <span data-ttu-id="37dc3-178">Olarak [SignalR ile çalışmaya başlama](tutorial-getting-started-with-signalr.md) öğretici, hub istemcileri doğrudan çağıran bir yöntem vardır.</span><span class="sxs-lookup"><span data-stu-id="37dc3-178">As in the [Getting Started with SignalR](tutorial-getting-started-with-signalr.md) tutorial, the hub has a method that the clients will call directly.</span></span> <span data-ttu-id="37dc3-179">Bu durumda, istemci içeren yeni bir nesne gönderir ardından diğer tüm bağlı istemcilere yayımladınız sunucusuna şeklin X ve Y koordinatları.</span><span class="sxs-lookup"><span data-stu-id="37dc3-179">In this case, the client will send an object containing the new X and Y coordinates of the shape to the server, which then gets broadcasted to all other connected clients.</span></span> <span data-ttu-id="37dc3-180">SignalR otomatik olarak JSON kullanarak bu nesneyi seri hale.</span><span class="sxs-lookup"><span data-stu-id="37dc3-180">SignalR will automatically serialize this object using JSON.</span></span>

    <span data-ttu-id="37dc3-181">İstemciye gönderilecek nesne (`ShapeModel`) şeklin konumunu depolamak için üye içeriyor.</span><span class="sxs-lookup"><span data-stu-id="37dc3-181">The object that will be sent to the client (`ShapeModel`) contains members to store the position of the shape.</span></span> <span data-ttu-id="37dc3-182">Belirtilen bir istemci, kendi veri gönderilmez böylece sunucu üzerindeki nesnesi sürümünü hangi istemci verilerinin depolanıyorsa, izlemek için üye de içerir.</span><span class="sxs-lookup"><span data-stu-id="37dc3-182">The version of the object on the server also contains a member to track which client's data is being stored, so that a given client won't be sent their own data.</span></span> <span data-ttu-id="37dc3-183">Bu üyeyi kullanan `JsonIgnore` serileştirilmiş ve istemciye gönderilen tutmak için öznitelik.</span><span class="sxs-lookup"><span data-stu-id="37dc3-183">This member uses the `JsonIgnore` attribute to keep it from being serialized and sent to the client.</span></span>

<a id="startup2013"></a>
## <a name="starting-the-hub-when-the-application-starts"></a><span data-ttu-id="37dc3-184">Uygulama başladığında hub başlatılıyor</span><span class="sxs-lookup"><span data-stu-id="37dc3-184">Starting the hub when the application starts</span></span>

1. <span data-ttu-id="37dc3-185">Uygulama başladığında ardından, hub'ına eşlemesini yaparız.</span><span class="sxs-lookup"><span data-stu-id="37dc3-185">Next, we'll set up mapping to the hub when the application starts.</span></span> <span data-ttu-id="37dc3-186">SignalR 2'de bu çağıracak bir OWIN başlangıç sınıfı ekleyerek yapılır `MapSignalR` zaman başlangıç sınıfı `Configuration` yöntemi OWIN başladığında yürütülebilir.</span><span class="sxs-lookup"><span data-stu-id="37dc3-186">In SignalR 2, this is done by adding an OWIN startup class, which will call `MapSignalR` when the startup class' `Configuration` method is executed when OWIN starts.</span></span> <span data-ttu-id="37dc3-187">Sınıfı için OWIN'in başlangıç eklenir kullanarak işlem `OwinStartup` derleme özniteliği.</span><span class="sxs-lookup"><span data-stu-id="37dc3-187">The class is added to OWIN's startup process using the `OwinStartup` assembly attribute.</span></span>

    <span data-ttu-id="37dc3-188">İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve ardından **Ekle | OWIN başlangıç sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="37dc3-188">In **Solution Explorer**, right-click the project, then click **Add | OWIN Startup Class**.</span></span> <span data-ttu-id="37dc3-189">Sınıf adını *başlangıç* tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="37dc3-189">Name the class *Startup* and click **OK**.</span></span>
2. <span data-ttu-id="37dc3-190">Haline içeriğini aşağıdaki gibi değiştirin:</span><span class="sxs-lookup"><span data-stu-id="37dc3-190">Change the contents of Startup.cs to the following:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<a id="client"></a>
## <a name="adding-the-client"></a><span data-ttu-id="37dc3-191">İstemci ekleme</span><span class="sxs-lookup"><span data-stu-id="37dc3-191">Adding the client</span></span>

1. <span data-ttu-id="37dc3-192">Ardından, istemci ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="37dc3-192">Next, we'll add the client.</span></span> <span data-ttu-id="37dc3-193">İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve ardından **Ekle | Yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="37dc3-193">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="37dc3-194">İçinde **Yeni Öğe Ekle** iletişim kutusunda **Html sayfası**.</span><span class="sxs-lookup"><span data-stu-id="37dc3-194">In the **Add New Item** dialog, select **Html Page**.</span></span> <span data-ttu-id="37dc3-195">Sayfa adı **Default.html** tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="37dc3-195">Name the page **Default.html** and click **Add**.</span></span>
2. <span data-ttu-id="37dc3-196">İçinde **Çözüm Gezgini**, az önce oluşturduğunuz sayfanın sağ tıklatın ve **Başlangıç Sayfası Ayarla**.</span><span class="sxs-lookup"><span data-stu-id="37dc3-196">In **Solution Explorer**, right-click the page you just created and click **Set as Start Page**.</span></span>
3. <span data-ttu-id="37dc3-197">HTML sayfası varsayılan kodda aşağıdaki kod parçacığıyla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="37dc3-197">Replace the default code in the HTML page with the following code snippet.</span></span>

    > [!NOTE]
    > <span data-ttu-id="37dc3-198">Betik betikler klasörüne projenizde eklenen paketler eşleşme başvurduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="37dc3-198">Verify that the script references below match the packages added to your project in the Scripts folder.</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html)]

    <span data-ttu-id="37dc3-199">Yukarıdaki HTML ve JavaScript kodu şekli adlı bir kırmızı Div oluşturur, jQuery kitaplığını kullanarak şeklin sürükleme davranışı etkinleştirir ve şeklin kullanır `drag` şeklin konumu sunucusuna göndermek için olay.</span><span class="sxs-lookup"><span data-stu-id="37dc3-199">The above HTML and JavaScript code creates a red Div called Shape, enables the shape's dragging behavior using the jQuery library, and uses the shape's `drag` event to send the shape's position to the server.</span></span>
4. <span data-ttu-id="37dc3-200">F5 tuşuna basarak uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="37dc3-200">Start the application by pressing F5.</span></span> <span data-ttu-id="37dc3-201">Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="37dc3-201">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="37dc3-202">Şeklin tarayıcı pencerelerini sürükleyin; bir tarayıcı penceresi şeklinde taşımanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="37dc3-202">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span>

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a><span data-ttu-id="37dc3-204">İstemci döngü ekleme</span><span class="sxs-lookup"><span data-stu-id="37dc3-204">Add the client loop</span></span>

<span data-ttu-id="37dc3-205">Şekil her fare hareketi olayını üzerindeki konumunu gönderme gerekli olmayan miktarda ağ trafiği oluşturacak olduğundan, istemciden gelen iletileri kısıtlanan gerekir.</span><span class="sxs-lookup"><span data-stu-id="37dc3-205">Since sending the location of the shape on every mouse move event will create an unneccesary amount of network traffic, the messages from the client need to be throttled.</span></span> <span data-ttu-id="37dc3-206">Javascript kullanacağız `setInterval` sabit bir oranda sunucuya yeni konum bilgilerini gönderir bir döngü ayarlamak için işlevi.</span><span class="sxs-lookup"><span data-stu-id="37dc3-206">We'll use the javascript `setInterval` function to set up a loop that sends new position information to the server at a fixed rate.</span></span> <span data-ttu-id="37dc3-207">Bu döngü, bir "Oyun döngü", oyun veya diğer benzetimi işlevselliğini tüm sürücüleri art arda çağrılan bir işlev çok basit bir gösterimidir.</span><span class="sxs-lookup"><span data-stu-id="37dc3-207">This loop is a very basic representation of a "game loop", a repeatedly called function that drives all of the functionality of a game or other simulation.</span></span>

1. <span data-ttu-id="37dc3-208">Aşağıdaki kod parçacığını eşleşecek şekilde HTML sayfasındaki istemci kodunu güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="37dc3-208">Update the client code in the HTML page to match the following code snippet.</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html)]

    <span data-ttu-id="37dc3-209">Yukarıdaki güncelleştirme ekler `updateServerModel` sabit frekansında çağrılır işlevi.</span><span class="sxs-lookup"><span data-stu-id="37dc3-209">The above update adds the `updateServerModel` function, which gets called on a fixed frequency.</span></span> <span data-ttu-id="37dc3-210">Bu işlev konum verileri sunucuya gönderdiği her `moved` bayrağı göndermek için yeni konum verileri olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="37dc3-210">This function sends the position data to the server whenever the `moved` flag indicates that there is new position data to send.</span></span>
2. <span data-ttu-id="37dc3-211">F5 tuşuna basarak uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="37dc3-211">Start the application by pressing F5.</span></span> <span data-ttu-id="37dc3-212">Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="37dc3-212">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="37dc3-213">Şeklin tarayıcı pencerelerini sürükleyin; bir tarayıcı penceresi şeklinde taşımanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="37dc3-213">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="37dc3-214">Animasyon, sunucuya gönderilen ileti sayısını kısıtlanacak olduğundan, olduğu gibi önceki bölümde kesintisiz olarak görünmez.</span><span class="sxs-lookup"><span data-stu-id="37dc3-214">Since the number of messages that get sent to the server will be throttled, the animation will not appear as smooth as in the previous section.</span></span>

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a><span data-ttu-id="37dc3-216">Sunucu döngü ekleme</span><span class="sxs-lookup"><span data-stu-id="37dc3-216">Add the server loop</span></span>

<span data-ttu-id="37dc3-217">Geçerli uygulamada sunucudan istemciye gönderilen iletileri alınana kadar sık gider.</span><span class="sxs-lookup"><span data-stu-id="37dc3-217">In the current application, messages sent from the server to the client go out as often as they are received.</span></span> <span data-ttu-id="37dc3-218">İstemcide görülen benzer bir sorun gösterir; iletiler daha sık gerekli değildir ve bağlantı sonucunda taşan hale gelebilir gönderilebilir.</span><span class="sxs-lookup"><span data-stu-id="37dc3-218">This presents a similar problem as was seen on the client; messages can be sent more often than they are needed, and the connection could become flooded as a result.</span></span> <span data-ttu-id="37dc3-219">Bu bölümde, giden iletiler oranını kısıtlar bir süreölçer uygulamak için sunucuyu güncelleştirmek açıklar.</span><span class="sxs-lookup"><span data-stu-id="37dc3-219">This section describes how to update the server to implement a timer that throttles the rate of the outgoing messages.</span></span>

1. <span data-ttu-id="37dc3-220">Değiştir `MoveShapeHub.cs` aşağıdaki kod parçacığını ile.</span><span class="sxs-lookup"><span data-stu-id="37dc3-220">Replace the contents of `MoveShapeHub.cs` with the following code snippet.</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    <span data-ttu-id="37dc3-221">Yukarıdaki kod eklemek için istemci genişletir `Broadcaster` giden kısıtlar sınıfı iletileri kullanarak `Timer` .NET framework sınıf.</span><span class="sxs-lookup"><span data-stu-id="37dc3-221">The above code expands the client to add the `Broadcaster` class, which throttles the outgoing messages using the `Timer` class from the .NET framework.</span></span>

    <span data-ttu-id="37dc3-222">Hub geçici olduğundan (gerektiğinde her zaman oluşturulduğu), `Broadcaster` bir singleton olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="37dc3-222">Since the hub itself is transitory (it is created every time it is needed), the `Broadcaster` will be created as a singleton.</span></span> <span data-ttu-id="37dc3-223">Geç Başlatma (.NET 4'te tanıtılan) oluşturma, Zamanlayıcı başlamadan önce ilk hub örneği tamamen oluşturulduğundan emin olduktan gerektiğinde kadar erteleme kullanılır.</span><span class="sxs-lookup"><span data-stu-id="37dc3-223">Lazy initialization (introduced in .NET 4) is used to defer its creation until it is needed, ensuring that the first hub instance is completely created before the timer is started.</span></span>

    <span data-ttu-id="37dc3-224">İstemcilerin çağrısı `UpdateShape` işlevi sonra merkezin dışında taşınmış `UpdateModel` gelen iletileri hemen alınan her onun artık adlı şekilde yöntemi.</span><span class="sxs-lookup"><span data-stu-id="37dc3-224">The call to the clients' `UpdateShape` function is then moved out of the hub's `UpdateModel` method, so that it is no longer called immediately whenever incoming messages are received.</span></span> <span data-ttu-id="37dc3-225">Bunun yerine, iletileri istemcilere saniye başına 25 çağrı hızında gönderilir tarafından yönetilen `_broadcastLoop` Zamanlayıcı içinden `Broadcaster` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="37dc3-225">Instead, the messages to the clients will be sent at a rate of 25 calls per second, managed by the `_broadcastLoop` timer from within the `Broadcaster` class.</span></span>

    <span data-ttu-id="37dc3-226">Son olarak, istemci yöntemi hub'dan doğrudan çağırmak yerine `Broadcaster` sınıf gereken şu anda işletim hub için bir başvuru almak (`_hubContext`) kullanarak `GlobalHost`.</span><span class="sxs-lookup"><span data-stu-id="37dc3-226">Lastly, instead of calling the client method from the hub directly, the `Broadcaster` class needs to obtain a reference to the currently operating hub (`_hubContext`) using the `GlobalHost`.</span></span>
2. <span data-ttu-id="37dc3-227">F5 tuşuna basarak uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="37dc3-227">Start the application by pressing F5.</span></span> <span data-ttu-id="37dc3-228">Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="37dc3-228">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="37dc3-229">Şeklin tarayıcı pencerelerini sürükleyin; bir tarayıcı penceresi şeklinde taşımanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="37dc3-229">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="37dc3-230">Önceki bölümde tarayıcıdan görünür bir fark olmayacak, ancak istemciye gönderilen ileti sayısını kısıtlanacak.</span><span class="sxs-lookup"><span data-stu-id="37dc3-230">There will not be a visible difference in the browser from the previous section, but the number of messages that get sent to the client will be throttled.</span></span>

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a><span data-ttu-id="37dc3-232">İstemcide düzgün animasyon ekleme</span><span class="sxs-lookup"><span data-stu-id="37dc3-232">Add smooth animation on the client</span></span>

<span data-ttu-id="37dc3-233">Uygulama neredeyse tamamlandı, ancak yanıt olarak sunucu ileti taşınırken bir daha fazla geliştirme, istemcide şeklin hareketli vermiyoruz.</span><span class="sxs-lookup"><span data-stu-id="37dc3-233">The application is almost complete, but we could make one more improvement, in the motion of the shape on the client as it is moved in response to server messages.</span></span> <span data-ttu-id="37dc3-234">Sunucu tarafından verilen yeni bir konuma şeklin konumunu ayarlamak yerine JQuery UI kitaplığın kullanacağız `animate` şekli sorunsuz geçerli ve yeni konumunu arasında taşımak için işlevi.</span><span class="sxs-lookup"><span data-stu-id="37dc3-234">Rather than setting the position of the shape to the new location given by the server, we'll use the JQuery UI library's `animate` function to move the shape smoothly between its current and new position.</span></span>

1. <span data-ttu-id="37dc3-235">İstemcinin güncelleştirme `updateShape` aramak için yöntemi aşağıdaki vurgulanmış kodu ister:</span><span class="sxs-lookup"><span data-stu-id="37dc3-235">Update the client's `updateShape` method to look like the highlighted code below:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

    <span data-ttu-id="37dc3-236">Yukarıdaki kod şekli eski konumdan animasyon aralığı (Bu durumda, 100 milisaniyede) seyri içinde sunucu tarafından verilen yeni bir taşır.</span><span class="sxs-lookup"><span data-stu-id="37dc3-236">The above code moves the shape from the old location to the new one given by the server over the course of the animation interval (in this case, 100 milliseconds).</span></span> <span data-ttu-id="37dc3-237">Yeni animasyon başlamadan önce Şekil üzerinde çalışan herhangi bir önceki animasyon temizlenir.</span><span class="sxs-lookup"><span data-stu-id="37dc3-237">Any previous animation running on the shape is cleared before the new animation starts.</span></span>
2. <span data-ttu-id="37dc3-238">F5 tuşuna basarak uygulamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="37dc3-238">Start the application by pressing F5.</span></span> <span data-ttu-id="37dc3-239">Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="37dc3-239">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="37dc3-240">Şeklin tarayıcı pencerelerini sürükleyin; bir tarayıcı penceresi şeklinde taşımanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="37dc3-240">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="37dc3-241">Diğer pencere şeklinde hareketini hareketini üzerinden gelen ileti başına bir kez ayarlanan yerine zaman ilişkilendirilir daha az düzensiz görüntülenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="37dc3-241">The movement of the shape in the other window should appear less jerky as its movement is interpolated over time rather than being set once per incoming message.</span></span>

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image8.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a><span data-ttu-id="37dc3-243">Başka bir adım</span><span class="sxs-lookup"><span data-stu-id="37dc3-243">Further Steps</span></span>

<span data-ttu-id="37dc3-244">Bu öğreticide, istemciler ve sunucular arasında yüksek sıklıkta iletileri gönderen bir SignalR uygulama programı nasıl öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="37dc3-244">In this tutorial, you've learned how to program a SignalR application that sends high-frequency messages between clients and servers.</span></span> <span data-ttu-id="37dc3-245">Bu iletişim çevrimiçi oyunlar ve diğer benzetimleri gibi geliştirmek için yararlı bir örnektir [SignalR ile oluşturulan ShootR oyun](http://shootr.signalr.net).</span><span class="sxs-lookup"><span data-stu-id="37dc3-245">This communication paradigm is useful for developing online games and other simulations, such as [the ShootR game created with SignalR](http://shootr.signalr.net).</span></span>

<span data-ttu-id="37dc3-246">Bu öğreticide oluşturduğunuz tam uygulama yüklenebilir [kod Galerisi'nden](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).</span><span class="sxs-lookup"><span data-stu-id="37dc3-246">The complete application created in this tutorial can be downloaded from [Code Gallery](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).</span></span>

<span data-ttu-id="37dc3-247">SignalR geliştirme kavramları hakkında daha fazla bilgi edinmek için SignalR kaynak kodu ve kaynaklar için aşağıdaki siteleri ziyaret edin:</span><span class="sxs-lookup"><span data-stu-id="37dc3-247">To learn more about SignalR development concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="37dc3-248">SignalR Project</span><span class="sxs-lookup"><span data-stu-id="37dc3-248">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="37dc3-249">SignalR Github ve örnekler</span><span class="sxs-lookup"><span data-stu-id="37dc3-249">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="37dc3-250">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="37dc3-250">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

<span data-ttu-id="37dc3-251">Bir SignalR uygulamayı Azure'a dağıtma hakkında bir kılavuz için bkz: [SignalR kullanarak Azure App Service'te Web uygulamalarını](../deployment/using-signalr-with-azure-web-sites.md).</span><span class="sxs-lookup"><span data-stu-id="37dc3-251">For a walkthrough on how to deploy a SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="37dc3-252">Windows Azure Web sitesi için bir Visual Studio web projesi dağıtma hakkında ayrıntılı bilgi için bkz: [Azure App Service'te bir ASP.NET web uygulaması oluşturma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="37dc3-252">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>
