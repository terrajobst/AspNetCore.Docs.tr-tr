---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: OWIN ve Katana Başlarken | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/27/2013
ms.topic: article
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: ac0302ef1a786f6b1eef8119b3134a965f01c533
ms.sourcegitcommit: 5ab5c5f4bfdb0150f42ba84c2770eadf540cae48
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
<a name="getting-started-with-owin-and-katana"></a><span data-ttu-id="cdd2e-102">OWIN ve Katana ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="cdd2e-102">Getting Started with OWIN and Katana</span></span>
====================
<span data-ttu-id="cdd2e-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cdd2e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="cdd2e-104">[.NET (OWIN) için Web Arabirimi'ni açmak](http://owin.org/) .NET web sunucuları ve web uygulamaları arasındaki bir Özet tanımlar.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-104">[Open Web Interface for .NET (OWIN)](http://owin.org/) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="cdd2e-105">Uygulama web sunucusundan ayrıştırarak OWIN ara yazılımı .NET web geliştirme için oluşturmak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-105">By decoupling the web server from the application, OWIN makes it easier to create middleware for .NET web development.</span></span> <span data-ttu-id="cdd2e-106">Ayrıca, OWIN, bağlantı noktası web uygulamalarının diğer Konaklara kolaylaştırır&#8212;Örneğin, bir Windows hizmeti veya başka bir işlem kendi kendine barındırma.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-106">Also, OWIN makes it easier to port web applications to other hosts&#8212;for example, self-hosting in a Windows service or other process.</span></span>

<span data-ttu-id="cdd2e-107">OWIN bir topluluk ait olmayan bir uygulama tanımıdır.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-107">OWIN is a community-owned specification, not an implementation.</span></span> <span data-ttu-id="cdd2e-108">Katana proje, Microsoft tarafından geliştirilen açık kaynak OWIN bileşenleri kümesidir.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-108">The Katana project is a set of open-source OWIN components developed by Microsoft.</span></span> <span data-ttu-id="cdd2e-109">OWIN ve Katana genel bir bakış için bkz: [bir genel bakış, proje Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="cdd2e-109">For a general overview of both OWIN and Katana, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="cdd2e-110">Bu makalede, ı başlamak koda sağ atlayacaktır.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-110">In this article, I will jump right into code to get started.</span></span>

<span data-ttu-id="cdd2e-111">Bu öğretici kullanır [Visual Studio 2013 Sürüm Adayı](https://go.microsoft.com/fwlink/?LinkId=306566), ancak Visual Studio 2012 de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-111">This tutorial uses [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), but you can also use Visual Studio 2012.</span></span> <span data-ttu-id="cdd2e-112">Adımları bazılarını ı aşağıda Not Visual Studio 2012'de farklıdır.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-112">A few of the steps are different in Visual Studio 2012, which I note below.</span></span>

## <a name="host-owin-in-iis"></a><span data-ttu-id="cdd2e-113">Ana bilgisayar IIS'de OWIN</span><span class="sxs-lookup"><span data-stu-id="cdd2e-113">Host OWIN in IIS</span></span>

<span data-ttu-id="cdd2e-114">Bu bölümde, biz IIS'de OWIN ana bilgisayar.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-114">In this section, we'll host OWIN in IIS.</span></span> <span data-ttu-id="cdd2e-115">Bu seçenek bir OWIN ardışık IIS olgun özellik kümesini birlikte composability ve esneklik sağlar.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-115">This option gives you the flexibility and composability of an OWIN pipeline together with the mature feature set of IIS.</span></span> <span data-ttu-id="cdd2e-116">Bu seçenek kullanılarak, ASP.NET istek ardışık düzeninde OWIN uygulaması çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-116">Using this option, the OWIN application runs in the ASP.NET request pipeline.</span></span>

<span data-ttu-id="cdd2e-117">İlk olarak, yeni bir ASP.NET Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-117">First, create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="cdd2e-118">(Visual Studio 2012'de ASP.NET boş Web uygulaması proje türü kullanın.)</span><span class="sxs-lookup"><span data-stu-id="cdd2e-118">(In Visual Studio 2012, use the ASP.NET Empty Web Application project type.)</span></span>

![](getting-started-with-owin-and-katana/_static/image1.png)

<span data-ttu-id="cdd2e-119">İçinde **yeni ASP.NET projesi** iletişim kutusunda **boş** şablonu.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-119">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span>

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a><span data-ttu-id="cdd2e-120">NuGet paketleri ekleme</span><span class="sxs-lookup"><span data-stu-id="cdd2e-120">Add NuGet Packages</span></span>

<span data-ttu-id="cdd2e-121">Ardından, gerekli NuGet paketleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-121">Next, add the required NuGet packages.</span></span> <span data-ttu-id="cdd2e-122">Gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-122">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="cdd2e-123">Paket Yöneticisi konsolu penceresinde aşağıdaki komutu yazın:</span><span class="sxs-lookup"><span data-stu-id="cdd2e-123">In the Package Manager Console window, type the following command:</span></span>

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a><span data-ttu-id="cdd2e-124">Başlangıç sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="cdd2e-124">Add a Startup Class</span></span>

<span data-ttu-id="cdd2e-125">Ardından, OWIN başlangıç sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-125">Next, add an OWIN startup class.</span></span> <span data-ttu-id="cdd2e-126">Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Ekle**seçeneğini belirleyip **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-126">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span> <span data-ttu-id="cdd2e-127">İçinde **Yeni Öğe Ekle** iletişim kutusunda **Owın başlangıç sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-127">In the **Add New Item** dialog, select **Owin Startup class**.</span></span> <span data-ttu-id="cdd2e-128">Başlangıç sınıfı yapılandırma hakkında daha fazla bilgi için bkz: [OWIN başlangıç sınıfı algılama](owin-startup-class-detection.md).</span><span class="sxs-lookup"><span data-stu-id="cdd2e-128">For more info on configuring the startup class, see [OWIN Startup Class Detection](owin-startup-class-detection.md).</span></span>

![](getting-started-with-owin-and-katana/_static/image4.png)

<span data-ttu-id="cdd2e-129">Aşağıdaki kodu ekleyin `Startup1.Configuration` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="cdd2e-129">Add the following code to the `Startup1.Configuration` method:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

<span data-ttu-id="cdd2e-130">Bu kod basit bir ara yazılım alan, bir işlevi olarak uygulanan OWIN ardışık düzenine ekler bir **Microsoft.Owin.IOwinContext** örneği.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-130">This code adds a simple piece of middleware to the OWIN pipeline, implemented as a function that receives a **Microsoft.Owin.IOwinContext** instance.</span></span> <span data-ttu-id="cdd2e-131">Sunucu bir HTTP isteği aldığında, OWIN ardışık düzenini ara yazılım çağırır.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-131">When the server receives an HTTP request, the OWIN pipeline invokes the middleware.</span></span> <span data-ttu-id="cdd2e-132">Ara yazılımı için yanıt içerik türünü ayarlar ve yanıt gövdesi yazar.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-132">The middleware sets the content type for the response and writes the response body.</span></span>

> [!NOTE]
> <span data-ttu-id="cdd2e-133">OWIN başlangıç sınıfı şablon Visual Studio 2013'te kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-133">The OWIN Startup class template is available in Visual Studio 2013.</span></span> <span data-ttu-id="cdd2e-134">Visual Studio 2012 kullanıyorsanız, adlı yeni bir boş sınıf eklemeniz yeterlidir `Startup1`, aşağıdaki kodu yapıştırın:</span><span class="sxs-lookup"><span data-stu-id="cdd2e-134">If you are using Visual Studio 2012, just add a new empty class named `Startup1`, and paste in the following code:</span></span>


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a><span data-ttu-id="cdd2e-135">Uygulamayı çalıştırın</span><span class="sxs-lookup"><span data-stu-id="cdd2e-135">Run the Application</span></span>

<span data-ttu-id="cdd2e-136">Hata ayıklama başlatmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-136">Press F5 to begin debugging.</span></span> <span data-ttu-id="cdd2e-137">Visual Studio, bir tarayıcı penceresi açılır `http://localhost:*port*/`.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-137">Visual Studio will open a browser window to `http://localhost:*port*/`.</span></span> <span data-ttu-id="cdd2e-138">Sayfasında, aşağıdaki gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="cdd2e-138">The page should look like the following:</span></span>

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a><span data-ttu-id="cdd2e-139">Bir konsol uygulamasında OWIN kendini barındırma</span><span class="sxs-lookup"><span data-stu-id="cdd2e-139">Self-Host OWIN in a Console Application</span></span>

<span data-ttu-id="cdd2e-140">Bu uygulama, özel bir işlem olarak kendi kendine barındırma için IIS barındırma Dönüştür kolaydır.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-140">It's easy to convert this application from IIS hosting to self-hosting in a custom process.</span></span> <span data-ttu-id="cdd2e-141">IIS barındırma ile IIS hizmeti barındıran işleme ve her iki HTTP sunucusu olarak işlev görür.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-141">With IIS hosting, IIS acts as both the HTTP server and as the process that hosts the service.</span></span> <span data-ttu-id="cdd2e-142">Kendi kendine barındırma, uygulamanızın işlem oluşturur ve kullanır **HttpListener** sınıf HTTP sunucusu olarak.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-142">With self-hosting, your application creates the process and uses the **HttpListener** class as the HTTP server.</span></span>

<span data-ttu-id="cdd2e-143">Visual Studio'da yeni bir konsol uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-143">In Visual Studio, create a new console application.</span></span> <span data-ttu-id="cdd2e-144">Paket Yöneticisi konsolu penceresinde aşağıdaki komutu yazın:</span><span class="sxs-lookup"><span data-stu-id="cdd2e-144">In the Package Manager Console window, type the following command:</span></span>

`Install-Package Microsoft.Owin.SelfHost -Pre`

<span data-ttu-id="cdd2e-145">Ekleme bir `Startup1` projeye bu öğreticinin 1 bölümünden sınıfı.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-145">Add a `Startup1` class from part 1 of this tutorial to the project.</span></span> <span data-ttu-id="cdd2e-146">Bu sınıf değişiklik gerekmez.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-146">You don't need to modify this class.</span></span>

<span data-ttu-id="cdd2e-147">Uygulamanın uygulama `Main` yöntemini aşağıdaki şekilde.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-147">Implement the application's `Main` method as follows.</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

<span data-ttu-id="cdd2e-148">Konsol uygulamasını çalıştırdığınızda, sunucu başlatıldığında dinleme `http://localhost:9000`.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-148">When you run the console application, the server starts listening to `http://localhost:9000`.</span></span> <span data-ttu-id="cdd2e-149">Bir web tarayıcısında bu adrese gidin "Hello world" sayfasını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-149">If you navigate to this address in a web browser, you will see the "Hello world" page.</span></span>

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a><span data-ttu-id="cdd2e-150">OWIN Diagnostics ekleme</span><span class="sxs-lookup"><span data-stu-id="cdd2e-150">Add OWIN Diagnostics</span></span>

<span data-ttu-id="cdd2e-151">Microsoft.Owin.Diagnostics paketi işlenmeyen özel durumları yakalar ve hata ayrıntıları içeren bir HTML sayfası görüntüleyen ara yazılım içerir.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-151">The Microsoft.Owin.Diagnostics package contains middleware that catches unhandled exceptions and displays an HTML page with error details.</span></span> <span data-ttu-id="cdd2e-152">Bu sayfa işlevlerini daha çok benzer şekilde adlandırılır ASP.NET hata sayfası "[sarı renkli kilitlenme ekranı](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span><span class="sxs-lookup"><span data-stu-id="cdd2e-152">This page functions much like the ASP.NET error page that is sometimes called the "[yellow screen of death](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span></span> <span data-ttu-id="cdd2e-153">YSOD gibi Katana hata sayfası geliştirme sırasında yararlıdır, ancak üretim modunda devre dışı bırakmak için iyi bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-153">Like the YSOD, the Katana error page is useful during development, but it's a good practice to disable it in production mode.</span></span>

<span data-ttu-id="cdd2e-154">Projenizde Tanılama paketi yüklemek için Paket Yöneticisi konsol penceresinde aşağıdaki komutu yazın:</span><span class="sxs-lookup"><span data-stu-id="cdd2e-154">To install the Diagnostics package in your project, type the following command in the Package Manager Console window:</span></span>

`install-package Microsoft.Owin.Diagnostics –Pre`

<span data-ttu-id="cdd2e-155">Kodda değişiklik, `Startup1.Configuration` yöntemini aşağıdaki şekilde:</span><span class="sxs-lookup"><span data-stu-id="cdd2e-155">Change the code in your `Startup1.Configuration` method as follows:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

<span data-ttu-id="cdd2e-156">Böylece Visual Studio bir özel durum nedeniyle kesecektir değil şimdi hata ayıklama olmadan, uygulamayı çalıştırmak için CTRL + F5'nı kullanın.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-156">Now use CTRL+F5 to run the application without debugging, so that Visual Studio will not break on the exception.</span></span> <span data-ttu-id="cdd2e-157">Gitmeniz kadar uygulama aynı önceki gibi davranır `http://localhost/fail`, bu noktada uygulama özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-157">The application behaves the same as before, until you navigate to `http://localhost/fail`, at which point the application throws the exception.</span></span> <span data-ttu-id="cdd2e-158">Hata sayfası ara yazılımını catch özel durum ve hata hakkında bilgi içeren bir HTML sayfası görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-158">The error page middleware will catch the exception and display an HTML page with information about the error.</span></span> <span data-ttu-id="cdd2e-159">Yığın, sorgu dizesi, tanımlama bilgileri, istek üstbilgisi ve OWIN ortam değişkenleri görmek için sekmeleri tıklatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cdd2e-159">You can click the tabs to see the stack, query string, cookies, request header, and OWIN environment variables.</span></span>

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a><span data-ttu-id="cdd2e-160">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="cdd2e-160">Next Steps</span></span>

- [<span data-ttu-id="cdd2e-161">OWIN Başlangıç Sınıfı Algılama</span><span class="sxs-lookup"><span data-stu-id="cdd2e-161">OWIN Startup Class Detection</span></span>](owin-startup-class-detection.md)
- [<span data-ttu-id="cdd2e-162">ASP.NET Web API Self barındırmak için OWIN kullanın</span><span class="sxs-lookup"><span data-stu-id="cdd2e-162">Use OWIN to Self-Host ASP.NET Web API</span></span>](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [<span data-ttu-id="cdd2e-163">SignalR kendi kendine barındırmak için OWIN kullanın</span><span class="sxs-lookup"><span data-stu-id="cdd2e-163">Use OWIN to Self-Host SignalR</span></span>](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
