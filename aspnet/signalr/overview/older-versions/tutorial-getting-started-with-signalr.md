---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Öğretici: SignalR ile çalışmaya başlama 1.x | Microsoft Docs'
author: pfletcher
description: Bir HTML sayfasında bir gerçek zamanlı sohbet uygulaması oluşturmak için ASP.NET SignalR kullanın.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ce4953a0abf64af28ef4dbc5a62bb2d989343d99
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044740"
---
<a name="tutorial-getting-started-with-signalr-1x"></a><span data-ttu-id="666ae-103">Öğretici: SignalR ile çalışmaya başlama 1.x</span><span class="sxs-lookup"><span data-stu-id="666ae-103">Tutorial: Getting Started with SignalR 1.x</span></span>
====================
<span data-ttu-id="666ae-104">tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="666ae-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

> <span data-ttu-id="666ae-105">Bu öğreticide SignalR kullanarak gerçek zamanlı bir sohbet uygulaması oluşturma işlemi gösterilir.</span><span class="sxs-lookup"><span data-stu-id="666ae-105">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="666ae-106">Boş bir ASP.NET web uygulamasına SignalR ekleyebilir ve göndermek ve iletileri görüntülemek için bir HTML sayfası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="666ae-106">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="666ae-107">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="666ae-107">Overview</span></span>

<span data-ttu-id="666ae-108">Bu öğretici, bir basit tarayıcı tabanlı sohbet uygulaması oluşturmak nasıl yapıldığını göstererek SignalR geliştirme sunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="666ae-108">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="666ae-109">Boş bir ASP.NET web uygulamasına SignalR Kitaplığı eklemek, istemcilere ileti göndermek için bir hub sınıf oluşturun ve sohbet ileti gönderme ve alma kullanıcıların olanak sağlayan bir HTML sayfası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="666ae-109">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="666ae-110">MVC görünümü kullanarak MVC 4'te bir sohbet uygulaması oluşturmak nasıl oluşturulduğunu gösteren benzer bir öğretici için bkz: [SignalR ve MVC 4 ile çalışmaya başlama](index.md).</span><span class="sxs-lookup"><span data-stu-id="666ae-110">For a similar tutorial that shows how to create a chat application in MVC 4 using an MVC view, see [Getting Started with SignalR and MVC 4](index.md).</span></span>

> [!NOTE]
> <span data-ttu-id="666ae-111">Bu öğretici (1.x) sürümünü SignalR kullanır.</span><span class="sxs-lookup"><span data-stu-id="666ae-111">This tutorial uses the release (1.x) version of SignalR.</span></span> <span data-ttu-id="666ae-112">SignalR arasındaki değişiklikleri hakkında ayrıntılı bilgi için bkz: 1.x ve 2.0 [yükseltme SignalR 1.x projeleri](../releases/upgrading-signalr-1x-projects-to-20.md).</span><span class="sxs-lookup"><span data-stu-id="666ae-112">For details on changes between SignalR 1.x and 2.0, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<span data-ttu-id="666ae-113">SignalR, Canlı kullanıcı etkileşimi veya gerçek zamanlı veri güncelleştirmeleri gerektiren web uygulamaları oluşturmak için bir açık kaynak .NET kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="666ae-113">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="666ae-114">Sosyal uygulamalar, çok kullanıcılı oyunlar, iş işbirliği ve haberler, hava durumu veya finansal güncelleştirme uygulamaları örnek olarak verilebilir.</span><span class="sxs-lookup"><span data-stu-id="666ae-114">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="666ae-115">Bunlar genellikle gerçek zamanlı uygulamaları olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="666ae-115">These are often called real-time applications.</span></span>

<span data-ttu-id="666ae-116">SignalR gerçek zamanlı uygulamaları oluşturma işlemini basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="666ae-116">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="666ae-117">Bir ASP.NET server kitaplığı ve istemci-sunucu bağlantıları yönetmek ve içerik güncelleştirmelerini istemcilere anında kolaylaştırmak için JavaScript istemci kitaplığını içerir.</span><span class="sxs-lookup"><span data-stu-id="666ae-117">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="666ae-118">Gerçek zamanlı işlevselliği sağlamak için mevcut bir ASP.NET uygulamasını için SignalR kitaplığı ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="666ae-118">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="666ae-119">Öğretici aşağıdaki SignalR geliştirme görevleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="666ae-119">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="666ae-120">SignalR kitaplığı, bir ASP.NET web uygulamasına ekleniyor.</span><span class="sxs-lookup"><span data-stu-id="666ae-120">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="666ae-121">İçeriği istemcilere göndermek için bir hub sınıfı oluşturma.</span><span class="sxs-lookup"><span data-stu-id="666ae-121">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="666ae-122">İleti gönderme ve hub güncelleştirmelerini görüntülemek için bir web sayfasında SignalR jQuery kitaplığı kullanma.</span><span class="sxs-lookup"><span data-stu-id="666ae-122">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="666ae-123">Aşağıdaki ekran görüntüsünde bir tarayıcıda çalışan sohbet uygulamayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="666ae-123">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="666ae-124">Her yeni kullanıcı yorumları gönderin ve kullanıcı sohbet katıldıktan sonra eklenen açıklamalar bakın.</span><span class="sxs-lookup"><span data-stu-id="666ae-124">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Sohbet örnekleri](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="666ae-126">Bölümler:</span><span class="sxs-lookup"><span data-stu-id="666ae-126">Sections:</span></span>

- [<span data-ttu-id="666ae-127">Projesi ayarlayın</span><span class="sxs-lookup"><span data-stu-id="666ae-127">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="666ae-128">Örnek çalıştırın</span><span class="sxs-lookup"><span data-stu-id="666ae-128">Run the Sample</span></span>](#run)
- [<span data-ttu-id="666ae-129">Kodu inceleyin</span><span class="sxs-lookup"><span data-stu-id="666ae-129">Examine the Code</span></span>](#code)
- [<span data-ttu-id="666ae-130">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="666ae-130">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="666ae-131">Projesi ayarlayın</span><span class="sxs-lookup"><span data-stu-id="666ae-131">Set up the Project</span></span>

<span data-ttu-id="666ae-132">Bu bölümde, boş bir ASP.NET web uygulaması oluşturmak gösterilmektedir, SignalR ekleyin ve sohbet uygulaması oluşturma.</span><span class="sxs-lookup"><span data-stu-id="666ae-132">This section shows how to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="666ae-133">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="666ae-133">Prerequisites:</span></span>

- <span data-ttu-id="666ae-134">Visual Studio 2010 SP1 veya 2012.</span><span class="sxs-lookup"><span data-stu-id="666ae-134">Visual Studio 2010 SP1 or 2012.</span></span> <span data-ttu-id="666ae-135">Visual Studio yoksa bkz [ASP.NET indirmeleri](https://www.asp.net/downloads) ücretsiz Visual Studio 2012 Express geliştirme aracı alınamıyor.</span><span class="sxs-lookup"><span data-stu-id="666ae-135">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="666ae-136">[Microsoft ASP.NET ve Web Araçları 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span><span class="sxs-lookup"><span data-stu-id="666ae-136">[Microsoft ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span></span> <span data-ttu-id="666ae-137">Visual Studio 2012 için bu yükleyiciyi Visual Studio için SignalR şablonları dahil olmak üzere yeni ASP.NET özellikleri ekler.</span><span class="sxs-lookup"><span data-stu-id="666ae-137">For Visual Studio 2012, this installer adds new ASP.NET features including SignalR templates to Visual Studio.</span></span> <span data-ttu-id="666ae-138">Visual Studio 2010 SP1 için bir yükleyici kullanılabilir değildir ancak Kurulum adımlarında açıklandığı gibi SignalR NuGet paketini yükleyerek öğretici tamamlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="666ae-138">For Visual Studio 2010 SP1, an installer is not available but you can complete the tutorial by installing the SignalR NuGet package as described in the setup steps.</span></span>

<span data-ttu-id="666ae-139">ASP.NET boş Web uygulaması oluşturmak ve SignalR Kitaplığı eklemek için Visual Studio 2012 aşağıdaki adımları kullanın:</span><span class="sxs-lookup"><span data-stu-id="666ae-139">The following steps use Visual Studio 2012 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="666ae-140">Visual Studio'da ASP.NET boş Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="666ae-140">In Visual Studio create an ASP.NET Empty Web Application.</span></span>

    ![Boş web oluşturma](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="666ae-142">Açık **Paket Yöneticisi Konsolu** seçerek **Araçlar | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="666ae-142">Open the **Package Manager Console** by selecting **Tools | Library Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="666ae-143">Konsol penceresine aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="666ae-143">Enter the following command into the console window:</span></span>

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    <span data-ttu-id="666ae-144">Bu komut SignalR en son sürümünü yükler 1.x.</span><span class="sxs-lookup"><span data-stu-id="666ae-144">This command installs the latest version of SignalR 1.x.</span></span>
3. <span data-ttu-id="666ae-145">İçinde **Çözüm Gezgini**, projeye sağ tıklayın, seçin **Ekle | Sınıf**.</span><span class="sxs-lookup"><span data-stu-id="666ae-145">In **Solution Explorer**, right-click the project, select **Add | Class**.</span></span> <span data-ttu-id="666ae-146">Yeni sınıf **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="666ae-146">Name the new class **ChatHub**.</span></span>
4. <span data-ttu-id="666ae-147">İçinde **Çözüm Gezgini** betikleri düğümünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="666ae-147">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="666ae-148">JQuery ve SignalR için komut dosyası kitaplıkları projede görünür.</span><span class="sxs-lookup"><span data-stu-id="666ae-148">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    ![Kitaplık Başvurusu](tutorial-getting-started-with-signalr/_static/image3.png)
5. <span data-ttu-id="666ae-150">Kodla **ChatHub** aşağıdaki kodla sınıfı.</span><span class="sxs-lookup"><span data-stu-id="666ae-150">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="666ae-151">İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve ardından **Ekle | Yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="666ae-151">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="666ae-152">İçinde **Yeni Öğe Ekle** iletişim kutusunda **genel uygulama sınıfı** tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="666ae-152">In the **Add New Item** dialog, select **Global Application Class** and click **Add**.</span></span>

    ![Genel ekleme](tutorial-getting-started-with-signalr/_static/image4.png)
7. <span data-ttu-id="666ae-154">Aşağıdakileri ekleyin `using` sağlanan sonra deyimleri `using` Global.asax.cs sınıfı deyimlerinde.</span><span class="sxs-lookup"><span data-stu-id="666ae-154">Add the following `using` statements after the provided `using` statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="666ae-155">Aşağıdaki kod satırını ekleyin `Application_Start` SignalR hub'ları için varsayılan yol kaydetmek için genel sınıfının yöntemi.</span><span class="sxs-lookup"><span data-stu-id="666ae-155">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR hubs.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. <span data-ttu-id="666ae-156">İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve ardından **Ekle | Yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="666ae-156">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="666ae-157">İçinde **Yeni Öğe Ekle** iletişim kutusu, select Html sayfası ve tıklatın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="666ae-157">In the **Add New Item** dialog, select Html Page and click **Add**.</span></span>
10. <span data-ttu-id="666ae-158">İçinde **Çözüm Gezgini**, az önce oluşturduğunuz HTML sayfası sağ tıklatın ve **Başlangıç Sayfası Ayarla**.</span><span class="sxs-lookup"><span data-stu-id="666ae-158">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
11. <span data-ttu-id="666ae-159">HTML sayfası varsayılan kodu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="666ae-159">Replace the default code in the HTML page with the following code.</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. <span data-ttu-id="666ae-160">**Tümünü Kaydet** projesi için.</span><span class="sxs-lookup"><span data-stu-id="666ae-160">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="666ae-161">Örnek çalıştırın</span><span class="sxs-lookup"><span data-stu-id="666ae-161">Run the Sample</span></span>

1. <span data-ttu-id="666ae-162">Projeyi hata ayıklama modunda çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="666ae-162">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="666ae-163">HTML sayfası bir tarayıcı örneği ve bir kullanıcı adı için ister yükler.</span><span class="sxs-lookup"><span data-stu-id="666ae-163">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Kullanıcı adı girin](tutorial-getting-started-with-signalr/_static/image5.png)
2. <span data-ttu-id="666ae-165">Bir kullanıcı adı girin.</span><span class="sxs-lookup"><span data-stu-id="666ae-165">Enter a user name.</span></span>
3. <span data-ttu-id="666ae-166">Tarayıcının adres satırından URL'yi kopyalayın ve iki daha fazla tarayıcı örnekleri açmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="666ae-166">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="666ae-167">Her tarayıcı örnek benzersiz bir kullanıcı adı girin.</span><span class="sxs-lookup"><span data-stu-id="666ae-167">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="666ae-168">Her tarayıcı örnek bir açıklama ekleyin ve **Gönder**.</span><span class="sxs-lookup"><span data-stu-id="666ae-168">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="666ae-169">Açıklamaları tüm tarayıcı örnekleri görüntülemelidir.</span><span class="sxs-lookup"><span data-stu-id="666ae-169">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="666ae-170">Bu basit sohbet uygulama sunucusunda tartışma bağlam korumaz.</span><span class="sxs-lookup"><span data-stu-id="666ae-170">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="666ae-171">Hub'ın tüm geçerli kullanıcı yorumları yayınlar.</span><span class="sxs-lookup"><span data-stu-id="666ae-171">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="666ae-172">Sohbet daha sonra katılmak kullanıcılar zamandan eklenen iletilerini bunlar katılma görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="666ae-172">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="666ae-173">Aşağıdaki ekran görüntüsünde bir örnek ileti gönderdiğinde, güncelleştirilen üç tarayıcı durumlarda çalışan sohbet uygulama gösterir:</span><span class="sxs-lookup"><span data-stu-id="666ae-173">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Sohbet tarayıcılar](tutorial-getting-started-with-signalr/_static/image6.png)
5. <span data-ttu-id="666ae-175">İçinde **Çözüm Gezgini**, inceleme **betik belgelerini** çalışan uygulama için düğüm.</span><span class="sxs-lookup"><span data-stu-id="666ae-175">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="666ae-176">Adlı bir komut dosyası **hub** , SignalR kitaplık çalışma zamanında dinamik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="666ae-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="666ae-177">Bu dosya, jQuery betik sunucu tarafı kodu arasında iletişim yönetir.</span><span class="sxs-lookup"><span data-stu-id="666ae-177">This file manages the communication between jQuery script and server-side code.</span></span>

    ![Oluşturulan hub komut dosyası](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="666ae-179">Kodu inceleyin</span><span class="sxs-lookup"><span data-stu-id="666ae-179">Examine the Code</span></span>

<span data-ttu-id="666ae-180">SignalR sohbet uygulaması iki temel SignalR geliştirme görevlerini gösterir: sunucu üzerindeki ana koordinasyon nesnesi olarak bir hub'ı oluşturma ve ileti gönderme ve alma için SignalR jQuery kitaplığı kullanma.</span><span class="sxs-lookup"><span data-stu-id="666ae-180">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="666ae-181">SignalR hub'ları</span><span class="sxs-lookup"><span data-stu-id="666ae-181">SignalR Hubs</span></span>

<span data-ttu-id="666ae-182">Kod örneğinde **ChatHub** sınıfı türer **Microsoft.AspNet.SignalR.Hub** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="666ae-182">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="666ae-183">Türetme **Hub** bir SignalR uygulaması oluşturmak için kullanışlı bir yol bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="666ae-183">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="666ae-184">Genel yöntemler hub sınıfınız oluşturun ve sonra bu yöntemler bir web sayfasında jQuery komut dosyalarından çağırarak erişim.</span><span class="sxs-lookup"><span data-stu-id="666ae-184">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="666ae-185">İstemciler sohbet kodda çağrısı **ChatHub.Send** yeni bir ileti göndermek için yöntem.</span><span class="sxs-lookup"><span data-stu-id="666ae-185">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="666ae-186">Hub sırayla iletiyi tüm istemcilere çağırarak gönderir **Clients.All.broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="666ae-186">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="666ae-187">**Gönder** yöntemi birkaç hub kavramları göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="666ae-187">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="666ae-188">Böylece istemciler bunları çağırabilirsiniz genel yöntemler bir hub'ına bildirin.</span><span class="sxs-lookup"><span data-stu-id="666ae-188">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="666ae-189">Kullanım **Microsoft.AspNet.SignalR.Hub.Clients** tüm istemciler erişmek için dinamik özellik bu hub'ına bağlı.</span><span class="sxs-lookup"><span data-stu-id="666ae-189">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="666ae-190">İstemcide bir jQuery işlevi çağırma (aşağıdaki gibi `broadcastMessage` işlevi) istemcilerini güncelleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="666ae-190">Call a jQuery function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="666ae-191">SignalR ve jQuery</span><span class="sxs-lookup"><span data-stu-id="666ae-191">SignalR and jQuery</span></span>

<span data-ttu-id="666ae-192">Kod örneğinde HTML sayfası SignalR jQuery kitaplığı bir SignalR hub ile iletişim kurmak için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="666ae-192">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="666ae-193">Kodda önemli görevleri istemcilere anında içeriği için sunucu çağırabilirsiniz işlevi bildirme ve hub'ına iletileri göndermek için bir bağlantı başlatma hub başvurmak için bir proxy bildirdiğiniz.</span><span class="sxs-lookup"><span data-stu-id="666ae-193">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="666ae-194">Aşağıdaki kod, bir hub için bir proxy bildirir.</span><span class="sxs-lookup"><span data-stu-id="666ae-194">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="666ae-195">JQuery içinde sunucu sınıfı ve üyelerini içinde ortası büyük başvurudur.</span><span class="sxs-lookup"><span data-stu-id="666ae-195">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="666ae-196">Kod örneği C# başvuran **ChatHub** jQuery sınıfında **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="666ae-196">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span>


<span data-ttu-id="666ae-197">Aşağıdaki komut dosyasında bir geri çağırma işlevini oluşturma kodudur.</span><span class="sxs-lookup"><span data-stu-id="666ae-197">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="666ae-198">Sunucudaki hub sınıfı içerik güncelleştirmeleri her bir istemciye göndermek için bu işlevi çağırır.</span><span class="sxs-lookup"><span data-stu-id="666ae-198">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="666ae-199">HTML kodlama, içeriği görüntülemeden önce iki satır isteğe bağlıdır ve kod eklemesini önlemeye yönelik basit bir yol gösterir.</span><span class="sxs-lookup"><span data-stu-id="666ae-199">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

<span data-ttu-id="666ae-200">Aşağıdaki kod, hub ile bir bağlantı açmak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="666ae-200">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="666ae-201">Kod bağlantı başlar ve üzerinde click olayını işlemek için bir işlev geçirir **Gönder** HTML sayfasının düğmesini.</span><span class="sxs-lookup"><span data-stu-id="666ae-201">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="666ae-202">Bu yaklaşım, olay işleyici yürütülmeden önce bağlantı kurulana oluşturmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="666ae-202">This approach insures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="666ae-203">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="666ae-203">Next Steps</span></span>

<span data-ttu-id="666ae-204">SignalR gerçek zamanlı web uygulamaları oluşturmak için bir çerçeve olduğunu öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="666ae-204">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="666ae-205">Birkaç SignalR geliştirme görevleri de öğrenilen: bir ASP.NET uygulaması için SignalR ekleme, hub sınıfın nasıl oluşturulacağı ve hub'dan ileti alıp göndermek nasıl.</span><span class="sxs-lookup"><span data-stu-id="666ae-205">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="666ae-206">Örnek uygulamayı Bu öğretici veya diğer SignalR uygulamalarını Internet üzerinden bir barındırma sağlayıcısına dağıtarak sunabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="666ae-206">You can make the sample application in this tutorial or other SignalR applications available over the Internet by deploying them to a hosting provider.</span></span> <span data-ttu-id="666ae-207">Microsoft, 10 web siteleri için ücretsiz bir ücretsiz web barındırma sunar [Windows Azure deneme sürümü hesabı](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="666ae-207">Microsoft offers free web hosting for up to 10 web sites in a free [Windows Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="666ae-208">Örnek SignalR uygulama dağıtma hakkında bir kılavuz için bkz: [SignalR alma başlatıldı örnek olarak bir Windows Azure Web sitesi yayımlama](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span><span class="sxs-lookup"><span data-stu-id="666ae-208">For a walkthrough on how to deploy the sample SignalR application, see [Publish the SignalR Getting Started Sample as a Windows Azure Web Site](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span></span> <span data-ttu-id="666ae-209">Windows Azure Web sitesi için bir Visual Studio web projesi dağıtma hakkında ayrıntılı bilgi için bkz: [bir ASP.NET uygulaması için bir Windows Azure Web sitesi dağıtma](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="666ae-209">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Deploying an ASP.NET Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span> <span data-ttu-id="666ae-210">(Not: WebSocket aktarımı şu anda Windows Azure Web siteleri için desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="666ae-210">(Note: The WebSocket transport is not currently supported for Windows Azure Web Sites.</span></span> <span data-ttu-id="666ae-211">Zaman WebSocket taşıma kullanılabilir değil, SignalR kullanan diğer kullanılabilir taşımalar taşımaları bölümünde açıklandığı gibi [SignalR konuya giriş](index.md).)</span><span class="sxs-lookup"><span data-stu-id="666ae-211">When WebSocket transport is not available, SignalR uses the other available transports as described in the Transports section of the [Introduction to SignalR topic](index.md).)</span></span>

<span data-ttu-id="666ae-212">Daha gelişmiş SignalR gelişmeler kavramları hakkında bilgi için SignalR kaynak kodu ve kaynaklar için aşağıdaki siteleri ziyaret edin:</span><span class="sxs-lookup"><span data-stu-id="666ae-212">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="666ae-213">SignalR Project</span><span class="sxs-lookup"><span data-stu-id="666ae-213">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="666ae-214">SignalR Github ve örnekler</span><span class="sxs-lookup"><span data-stu-id="666ae-214">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="666ae-215">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="666ae-215">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
