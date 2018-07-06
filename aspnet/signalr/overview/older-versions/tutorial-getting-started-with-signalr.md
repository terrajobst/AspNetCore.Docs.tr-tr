---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Öğretici: SignalR ile çalışmaya başlama 1.x | Microsoft Docs'
author: pfletcher
description: ASP.NET SignalR, bir HTML sayfasında bir gerçek zamanlı bir sohbet uygulaması oluşturmak için kullanın.
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 13d33ff7e3cfff996a9849cfccfcc43754c8234e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838633"
---
<a name="tutorial-getting-started-with-signalr-1x"></a><span data-ttu-id="acf5b-103">Öğretici: SignalR ile çalışmaya başlama 1.x</span><span class="sxs-lookup"><span data-stu-id="acf5b-103">Tutorial: Getting Started with SignalR 1.x</span></span>
====================
<span data-ttu-id="acf5b-104">tarafından [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="acf5b-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

> <span data-ttu-id="acf5b-105">Bu öğreticide SignalR kullanarak gerçek zamanlı bir sohbet uygulaması oluşturma işlemi gösterilir.</span><span class="sxs-lookup"><span data-stu-id="acf5b-105">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="acf5b-106">SignalR için boş bir ASP.NET web uygulamasına ekleme ve gönderin ve iletileri görüntülemek için bir HTML sayfası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="acf5b-106">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="acf5b-107">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="acf5b-107">Overview</span></span>

<span data-ttu-id="acf5b-108">Bu öğretici, bir basit tarayıcı tabanlı sohbet uygulaması oluşturmak nasıl yapıldığını göstererek SignalR geliştirme tanıtır.</span><span class="sxs-lookup"><span data-stu-id="acf5b-108">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="acf5b-109">SignalR kitaplık için boş bir ASP.NET web uygulamasına eklemek, istemcilere ileti göndermek için hub sınıfı oluşturun ve sohbet iletileri gönderip kullanıcıların olanak sağlayan bir HTML sayfası oluşturmak.</span><span class="sxs-lookup"><span data-stu-id="acf5b-109">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="acf5b-110">Bir MVC görünümü kullanarak MVC 4'te bir sohbet uygulaması oluşturma işlemini gösteren benzer bir öğretici için bkz. [SignalR ve MVC 4 ile çalışmaya başlama](index.md).</span><span class="sxs-lookup"><span data-stu-id="acf5b-110">For a similar tutorial that shows how to create a chat application in MVC 4 using an MVC view, see [Getting Started with SignalR and MVC 4](index.md).</span></span>

> [!NOTE]
> <span data-ttu-id="acf5b-111">Bu öğreticide SignalR sürüm (1.x) sürümünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="acf5b-111">This tutorial uses the release (1.x) version of SignalR.</span></span> <span data-ttu-id="acf5b-112">SignalR arasındaki değişiklikleri hakkında ayrıntılı bilgi için bkz: 1.x ve 2.0 [yükseltme SignalR 1.x projelerini](../releases/upgrading-signalr-1x-projects-to-20.md).</span><span class="sxs-lookup"><span data-stu-id="acf5b-112">For details on changes between SignalR 1.x and 2.0, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<span data-ttu-id="acf5b-113">SignalR Canlı kullanıcı etkileşimi veya gerçek zamanlı veri güncelleştirmeleri gerektiren web uygulamaları oluşturmaya yönelik bir açık kaynak .NET Kitaplığı ' dir.</span><span class="sxs-lookup"><span data-stu-id="acf5b-113">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="acf5b-114">Sosyal uygulamalar, çok kullanıcılı bir oyun, iş işbirliği ve haberler, hava durumu veya finansal güncelleştirme uygulamaları verilebilir.</span><span class="sxs-lookup"><span data-stu-id="acf5b-114">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="acf5b-115">Bunlar genellikle gerçek zamanlı uygulamalar olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="acf5b-115">These are often called real-time applications.</span></span>

<span data-ttu-id="acf5b-116">SignalR, gerçek zamanlı uygulamalar oluşturma işlemini basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="acf5b-116">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="acf5b-117">Bu, bir ASP.NET sunucu kitaplığı ve istemci-sunucu bağlantılarını yönetme ve içerik güncelleştirmelerini istemcilere göndermek daha kolay hale getirmek için bir JavaScript istemci kitaplığı içerir.</span><span class="sxs-lookup"><span data-stu-id="acf5b-117">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="acf5b-118">Gerçek zamanlı işlevsellik sağlamak için mevcut bir ASP.NET uygulamasını için SignalR kitaplığa ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="acf5b-118">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="acf5b-119">Öğretici aşağıdaki SignalR geliştirme görevleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="acf5b-119">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="acf5b-120">SignalR kitaplığı, bir ASP.NET web uygulamasına ekleniyor.</span><span class="sxs-lookup"><span data-stu-id="acf5b-120">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="acf5b-121">İçeriği istemcilere göndermek için bir hub sınıf oluşturuluyor.</span><span class="sxs-lookup"><span data-stu-id="acf5b-121">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="acf5b-122">İleti göndermek ve hub'ından güncelleştirmeleri görüntülemek için bir web sayfasında SignalR jQuery kitaplığı kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="acf5b-122">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="acf5b-123">Aşağıdaki ekran görüntüsünde, bir tarayıcıda çalışan sohbet uygulaması gösterir.</span><span class="sxs-lookup"><span data-stu-id="acf5b-123">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="acf5b-124">Her yeni kullanıcı, yorumlarınızı ve kullanıcı sohbet katıldıktan sonra eklenen yorumlara bakın.</span><span class="sxs-lookup"><span data-stu-id="acf5b-124">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Sohbet örnekleri](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="acf5b-126">Bölümler:</span><span class="sxs-lookup"><span data-stu-id="acf5b-126">Sections:</span></span>

- [<span data-ttu-id="acf5b-127">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="acf5b-127">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="acf5b-128">Örneği çalıştırma</span><span class="sxs-lookup"><span data-stu-id="acf5b-128">Run the Sample</span></span>](#run)
- [<span data-ttu-id="acf5b-129">Kod İnceleme</span><span class="sxs-lookup"><span data-stu-id="acf5b-129">Examine the Code</span></span>](#code)
- [<span data-ttu-id="acf5b-130">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="acf5b-130">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="acf5b-131">Projesi kurun</span><span class="sxs-lookup"><span data-stu-id="acf5b-131">Set up the Project</span></span>

<span data-ttu-id="acf5b-132">Bu bölümde, boş bir ASP.NET web uygulaması oluşturma işlemi gösterilmektedir, SignalR ekleyin ve sohbet uygulaması oluşturma.</span><span class="sxs-lookup"><span data-stu-id="acf5b-132">This section shows how to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="acf5b-133">Önkoşullar:</span><span class="sxs-lookup"><span data-stu-id="acf5b-133">Prerequisites:</span></span>

- <span data-ttu-id="acf5b-134">Visual Studio 2010 SP1 veya 2012.</span><span class="sxs-lookup"><span data-stu-id="acf5b-134">Visual Studio 2010 SP1 or 2012.</span></span> <span data-ttu-id="acf5b-135">Visual Studio yoksa bkz [ASP.NET indirir](https://www.asp.net/downloads) ücretsiz Visual Studio 2012 Express geliştirme aracı alınamıyor.</span><span class="sxs-lookup"><span data-stu-id="acf5b-135">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="acf5b-136">[Microsoft ASP.NET ve Web Araçları 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span><span class="sxs-lookup"><span data-stu-id="acf5b-136">[Microsoft ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span></span> <span data-ttu-id="acf5b-137">Visual Studio 2012 için Visual Studio için SignalR şablonları dahil olmak üzere yeni ASP.NET özellikleri bu yükleyiciyi ekler.</span><span class="sxs-lookup"><span data-stu-id="acf5b-137">For Visual Studio 2012, this installer adds new ASP.NET features including SignalR templates to Visual Studio.</span></span> <span data-ttu-id="acf5b-138">Visual Studio 2010 SP1 için bir yükleyici kullanılamaz ancak SignalR NuGet paketini yükleyerek Kurulum adımlarında açıklandığı gibi öğreticiyi tamamlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="acf5b-138">For Visual Studio 2010 SP1, an installer is not available but you can complete the tutorial by installing the SignalR NuGet package as described in the setup steps.</span></span>

<span data-ttu-id="acf5b-139">ASP.NET boş Web uygulaması oluşturma ve SignalR Kitaplığı eklemek için Visual Studio 2012 aşağıdaki adımları kullanın:</span><span class="sxs-lookup"><span data-stu-id="acf5b-139">The following steps use Visual Studio 2012 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="acf5b-140">Visual Studio'da ASP.NET boş Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="acf5b-140">In Visual Studio create an ASP.NET Empty Web Application.</span></span>

    ![Boş web oluşturma](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="acf5b-142">Açık **Paket Yöneticisi Konsolu** seçerek **araçları | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="acf5b-142">Open the **Package Manager Console** by selecting **Tools | Library Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="acf5b-143">Konsol penceresine aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="acf5b-143">Enter the following command into the console window:</span></span>

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    <span data-ttu-id="acf5b-144">Bu komut SignalR en son sürümünü yükler 1.x.</span><span class="sxs-lookup"><span data-stu-id="acf5b-144">This command installs the latest version of SignalR 1.x.</span></span>
3. <span data-ttu-id="acf5b-145">İçinde **Çözüm Gezgini**, projeye sağ tıklayın, **Ekle | Sınıf**.</span><span class="sxs-lookup"><span data-stu-id="acf5b-145">In **Solution Explorer**, right-click the project, select **Add | Class**.</span></span> <span data-ttu-id="acf5b-146">Yeni bir sınıf adı **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="acf5b-146">Name the new class **ChatHub**.</span></span>
4. <span data-ttu-id="acf5b-147">İçinde **Çözüm Gezgini** betikleri düğümünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="acf5b-147">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="acf5b-148">JQuery ve SignalR için betik kitaplıkları projesinde görünür.</span><span class="sxs-lookup"><span data-stu-id="acf5b-148">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    ![Kitaplık başvuruları](tutorial-getting-started-with-signalr/_static/image3.png)
5. <span data-ttu-id="acf5b-150">Değiştirin **ChatHub** aşağıdaki kodla sınıfı.</span><span class="sxs-lookup"><span data-stu-id="acf5b-150">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="acf5b-151">İçinde **Çözüm Gezgini**, projeyi sağ tıklatın ve ardından **Ekle | Yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="acf5b-151">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="acf5b-152">İçinde **Yeni Öğe Ekle** iletişim kutusunda **genel uygulama sınıfı** tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="acf5b-152">In the **Add New Item** dialog, select **Global Application Class** and click **Add**.</span></span>

    ![Genel ekleme](tutorial-getting-started-with-signalr/_static/image4.png)
7. <span data-ttu-id="acf5b-154">Aşağıdaki `using` deyimleri sağlanan sonra `using` deyimlerinde Global.asax.cs sınıfı.</span><span class="sxs-lookup"><span data-stu-id="acf5b-154">Add the following `using` statements after the provided `using` statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="acf5b-155">Aşağıdaki kod satırını ekleyin `Application_Start` SignalR hub'ları için varsayılan yolu kaydetmek için genel sınıfının yöntemi.</span><span class="sxs-lookup"><span data-stu-id="acf5b-155">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR hubs.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. <span data-ttu-id="acf5b-156">İçinde **Çözüm Gezgini**, projeyi sağ tıklatın ve ardından **Ekle | Yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="acf5b-156">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="acf5b-157">İçinde **Yeni Öğe Ekle** iletişim, select Html sayfası ve tıklatın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="acf5b-157">In the **Add New Item** dialog, select Html Page and click **Add**.</span></span>
10. <span data-ttu-id="acf5b-158">İçinde **Çözüm Gezgini**, yeni oluşturduğunuz HTML sayfasının sağ tıklatıp **Başlangıç Sayfası Ayarla**.</span><span class="sxs-lookup"><span data-stu-id="acf5b-158">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
11. <span data-ttu-id="acf5b-159">HTML sayfasındaki varsayılan kodu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="acf5b-159">Replace the default code in the HTML page with the following code.</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. <span data-ttu-id="acf5b-160">**Tümünü Kaydet** projesi için.</span><span class="sxs-lookup"><span data-stu-id="acf5b-160">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="acf5b-161">Örneği çalıştırma</span><span class="sxs-lookup"><span data-stu-id="acf5b-161">Run the Sample</span></span>

1. <span data-ttu-id="acf5b-162">Projeyi hata ayıklama modunda çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="acf5b-162">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="acf5b-163">Bir tarayıcı örneğinde ve bir kullanıcı adı için istemleri HTML sayfasını yükler.</span><span class="sxs-lookup"><span data-stu-id="acf5b-163">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Kullanıcı adı girin](tutorial-getting-started-with-signalr/_static/image5.png)
2. <span data-ttu-id="acf5b-165">Bir kullanıcı adı girin.</span><span class="sxs-lookup"><span data-stu-id="acf5b-165">Enter a user name.</span></span>
3. <span data-ttu-id="acf5b-166">Tarayıcının adres satırından URL'sini kopyalayın ve iki daha fazla tarayıcı örneği açmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="acf5b-166">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="acf5b-167">Her bir tarayıcı örneğinde benzersiz bir kullanıcı adı girin.</span><span class="sxs-lookup"><span data-stu-id="acf5b-167">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="acf5b-168">Her bir tarayıcı örneğinde bir açıklama ekleyin ve **Gönder**.</span><span class="sxs-lookup"><span data-stu-id="acf5b-168">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="acf5b-169">Açıklamalar, hepsinin tarayıcı görüntülemelidir.</span><span class="sxs-lookup"><span data-stu-id="acf5b-169">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="acf5b-170">Bu basit sohbet uygulaması sunucusunda tartışma bağlam korumaz.</span><span class="sxs-lookup"><span data-stu-id="acf5b-170">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="acf5b-171">Hub'ın tüm geçerli kullanıcılar yorum yayınlar.</span><span class="sxs-lookup"><span data-stu-id="acf5b-171">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="acf5b-172">Sohbet daha sonra katılmak kullanıcıların zamandan eklenen iletileri katılmaları görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="acf5b-172">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="acf5b-173">Aşağıdaki ekran görüntüsünde, çalışan bir örnek ileti gönderdiğinde tümü güncelleştirilir üç tarayıcı durumlarda sohbet uygulaması gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="acf5b-173">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Sohbet tarayıcılar](tutorial-getting-started-with-signalr/_static/image6.png)
5. <span data-ttu-id="acf5b-175">İçinde **Çözüm Gezgini**, inceleme **betik belgelerini** çalışan uygulama düğümü.</span><span class="sxs-lookup"><span data-stu-id="acf5b-175">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="acf5b-176">Adlı bir betik dosyası **hubs** , SignalR kitaplık çalışma zamanında dinamik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="acf5b-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="acf5b-177">Bu dosya, jQuery betik sunucu tarafı kodu arasında iletişimi yönetir.</span><span class="sxs-lookup"><span data-stu-id="acf5b-177">This file manages the communication between jQuery script and server-side code.</span></span>

    ![Oluşturulan hub betiği](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="acf5b-179">Kod İnceleme</span><span class="sxs-lookup"><span data-stu-id="acf5b-179">Examine the Code</span></span>

<span data-ttu-id="acf5b-180">SignalR sohbet uygulaması iki temel SignalR geliştirme görevleri gösterir: sunucunun ana koordinasyon nesne olarak bir hub'ı oluşturma ve ileti göndermek ve almak için SignalR jQuery kitaplığı kullanma.</span><span class="sxs-lookup"><span data-stu-id="acf5b-180">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="acf5b-181">SignalR hub'ları</span><span class="sxs-lookup"><span data-stu-id="acf5b-181">SignalR Hubs</span></span>

<span data-ttu-id="acf5b-182">Kod örneğinde **ChatHub** sınıf türetilir **Microsoft.AspNet.SignalR.Hub** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="acf5b-182">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="acf5b-183">Öğesinden türetme **Hub** SignalR uygulama oluşturmak için kullanışlı bir yöntem bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="acf5b-183">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="acf5b-184">Hub sınıfınıza genel yöntemleri oluşturun ve bu yöntemler bir web sayfasında jQuery betiklerden çağırarak erişin.</span><span class="sxs-lookup"><span data-stu-id="acf5b-184">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="acf5b-185">İstemciler sohbet kodda çağrı **ChatHub.Send** yeni bir ileti göndermek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="acf5b-185">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="acf5b-186">Hub sırayla ileti tüm istemcilere çağırarak gönderen **Clients.All.broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="acf5b-186">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="acf5b-187">**Gönder** yöntemi birkaç hub kavramları göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="acf5b-187">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="acf5b-188">İstemciler, bunları çağırabilirsiniz genel yöntemleri bir hub'da bildirin.</span><span class="sxs-lookup"><span data-stu-id="acf5b-188">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="acf5b-189">Kullanım **Microsoft.AspNet.SignalR.Hub.Clients** tüm istemcilere erişmek için dinamik özellik bu hub'a bağlı.</span><span class="sxs-lookup"><span data-stu-id="acf5b-189">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="acf5b-190">İstemcide jQuery işlev çağırma (gibi `broadcastMessage` işlevi) istemcilerini güncelleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="acf5b-190">Call a jQuery function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="acf5b-191">SignalR ve jQuery</span><span class="sxs-lookup"><span data-stu-id="acf5b-191">SignalR and jQuery</span></span>

<span data-ttu-id="acf5b-192">Kod örneği HTML sayfasındaki bir SignalR hub'ı ile iletişim kurmak için SignalR jQuery kitaplığı kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="acf5b-192">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="acf5b-193">Kodda önemli görevleri istemcilere anında içeriği için sunucu çağırabilen bir işlevi bildirmek ve hub'ına ileti göndermek için bir bağlantı başlatma hub başvurmak için bir proxy bildirdiğiniz.</span><span class="sxs-lookup"><span data-stu-id="acf5b-193">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="acf5b-194">Aşağıdaki kod, bir hub için bir proxy bildirir.</span><span class="sxs-lookup"><span data-stu-id="acf5b-194">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="acf5b-195">JQuery içinde sunucu sınıfını ve üyelerini ortası büyük harf başvurudur.</span><span class="sxs-lookup"><span data-stu-id="acf5b-195">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="acf5b-196">Kod örneği C# başvuran **ChatHub** jQuery sınıfında **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="acf5b-196">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span>


<span data-ttu-id="acf5b-197">Aşağıdaki betikte bir geri çağırma işlevini nasıl oluşturacağınız kodudur.</span><span class="sxs-lookup"><span data-stu-id="acf5b-197">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="acf5b-198">Sunucudaki hub sınıfına içerik güncelleştirmeleri her bir istemciye göndermek için bu işlevi çağırır.</span><span class="sxs-lookup"><span data-stu-id="acf5b-198">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="acf5b-199">HTML kodlama, içerik görüntülemeden önce aşağıdaki iki satırı isteğe bağlıdır ve kod eklemesini engellemek için basit bir yol gösterir.</span><span class="sxs-lookup"><span data-stu-id="acf5b-199">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

<span data-ttu-id="acf5b-200">Aşağıdaki kod hub'ı ile bir bağlantı açmak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="acf5b-200">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="acf5b-201">Kod bağlantı başlar ve ardından üzerinde click olayını işlemek için bir işlev geçirir **Gönder** HTML sayfasındaki düğmesi.</span><span class="sxs-lookup"><span data-stu-id="acf5b-201">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="acf5b-202">Bu yaklaşım, olay işleyici yürütülmeden önce bağlantı kurulur oluşturmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="acf5b-202">This approach insures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="acf5b-203">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="acf5b-203">Next Steps</span></span>

<span data-ttu-id="acf5b-204">SignalR gerçek zamanlı web uygulamaları oluşturmaya yönelik bir çerçeve olduğunu öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="acf5b-204">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="acf5b-205">Ayrıca birkaç SignalR geliştirme görevlerini öğrendiniz: bir ASP.NET uygulaması için SignalR ekleme, hub sınıfı oluşturma ve hub'ından iletiler alan nasıl.</span><span class="sxs-lookup"><span data-stu-id="acf5b-205">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="acf5b-206">Örnek uygulamayı Bu öğretici veya diğer SignalR uygulamalarında kullanılabilir Internet üzerinden bir barındırma sağlayıcısına dağıtarak yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="acf5b-206">You can make the sample application in this tutorial or other SignalR applications available over the Internet by deploying them to a hosting provider.</span></span> <span data-ttu-id="acf5b-207">Microsoft'un sunduğu ücretsiz olarak 10 adede kadar web siteleri için ücretsiz bir web barındırma [Windows Azure deneme hesabı](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="acf5b-207">Microsoft offers free web hosting for up to 10 web sites in a free [Windows Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="acf5b-208">Örnek SignalR uygulama dağıtma hakkında kılavuz için bkz. [SignalR alma çalışmaya örnek olarak bir Windows Azure Web sitesi yayımlama](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span><span class="sxs-lookup"><span data-stu-id="acf5b-208">For a walkthrough on how to deploy the sample SignalR application, see [Publish the SignalR Getting Started Sample as a Windows Azure Web Site](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span></span> <span data-ttu-id="acf5b-209">Visual Studio web projesini bir Windows Azure Web sitesine dağıtma hakkında ayrıntılı bilgi için bkz. [bir ASP.NET uygulaması için bir Windows Azure Web sitesi dağıtma](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="acf5b-209">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Deploying an ASP.NET Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span> <span data-ttu-id="acf5b-210">(Not: WebSocket taşıma için Windows Azure Web siteleri şu anda desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="acf5b-210">(Note: The WebSocket transport is not currently supported for Windows Azure Web Sites.</span></span> <span data-ttu-id="acf5b-211">Zaman WebSocket aktarım bulunmuyorsa, SignalR taşımalar bölümünde açıklandığı gibi diğer kullanılabilir aktarımları kullanır [SignalR konusuna giriş](index.md).)</span><span class="sxs-lookup"><span data-stu-id="acf5b-211">When WebSocket transport is not available, SignalR uses the other available transports as described in the Transports section of the [Introduction to SignalR topic](index.md).)</span></span>

<span data-ttu-id="acf5b-212">Daha gelişmiş SignalR gelişmeleri kavramları hakkında bilgi için SignalR kaynak kodu ve kaynaklar için aşağıdaki siteleri ziyaret edin:</span><span class="sxs-lookup"><span data-stu-id="acf5b-212">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="acf5b-213">SignalR projesi</span><span class="sxs-lookup"><span data-stu-id="acf5b-213">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="acf5b-214">SignalR Github ve örnekleri</span><span class="sxs-lookup"><span data-stu-id="acf5b-214">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="acf5b-215">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="acf5b-215">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
