---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: OWIN ve Katana ile çalışmaya başlama | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 9920861da0e67d9304a944cacfb8ff8685267cd6
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913183"
---
<a name="getting-started-with-owin-and-katana"></a><span data-ttu-id="3f235-102">OWIN ve Katana ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="3f235-102">Getting Started with OWIN and Katana</span></span>
====================
<span data-ttu-id="3f235-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3f235-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="3f235-104">[.NET (OWIN) için açık Web arabirimi](http://owin.org/) .NET web sunucuları ve web uygulaması arasında bir Özet tanımlar.</span><span class="sxs-lookup"><span data-stu-id="3f235-104">[Open Web Interface for .NET (OWIN)](http://owin.org/) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="3f235-105">Uygulamayı web sunucusundan ayrıştırarak OWIN ara yazılımı için .NET web geliştirme oluşturmak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="3f235-105">By decoupling the web server from the application, OWIN makes it easier to create middleware for .NET web development.</span></span> <span data-ttu-id="3f235-106">Ayrıca, OWIN diğer Konaklara bağlantı noktası web uygulamaları kolaylaştırır&#8212;Örneğin, bir Windows hizmeti veya başka bir işlem kendi kendine barındırma.</span><span class="sxs-lookup"><span data-stu-id="3f235-106">Also, OWIN makes it easier to port web applications to other hosts&#8212;for example, self-hosting in a Windows service or other process.</span></span>

<span data-ttu-id="3f235-107">OWIN uygulaması değil bir topluluk ait belirtimi ' dir.</span><span class="sxs-lookup"><span data-stu-id="3f235-107">OWIN is a community-owned specification, not an implementation.</span></span> <span data-ttu-id="3f235-108">Katana proje, Microsoft tarafından geliştirilen bir açık kaynak OWIN bileşenleri kümesidir.</span><span class="sxs-lookup"><span data-stu-id="3f235-108">The Katana project is a set of open-source OWIN components developed by Microsoft.</span></span> <span data-ttu-id="3f235-109">OWIN ve Katana hem genel bir bakış için bkz. [bir genel bakış, Project Katana'ya](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="3f235-109">For a general overview of both OWIN and Katana, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="3f235-110">Bu makalede, ı kullanmaya başlamak için kod sağ atlayacaktır.</span><span class="sxs-lookup"><span data-stu-id="3f235-110">In this article, I will jump right into code to get started.</span></span>

<span data-ttu-id="3f235-111">Bu öğreticide [Visual Studio 2013 Sürüm Adayı](https://go.microsoft.com/fwlink/?LinkId=306566), ancak Visual Studio 2012'de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3f235-111">This tutorial uses [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), but you can also use Visual Studio 2012.</span></span> <span data-ttu-id="3f235-112">Birkaç adımın ı aşağıda unutmayın Visual Studio 2012'de farklıdır.</span><span class="sxs-lookup"><span data-stu-id="3f235-112">A few of the steps are different in Visual Studio 2012, which I note below.</span></span>

## <a name="host-owin-in-iis"></a><span data-ttu-id="3f235-113">IIS'de OWIN barındırma</span><span class="sxs-lookup"><span data-stu-id="3f235-113">Host OWIN in IIS</span></span>

<span data-ttu-id="3f235-114">Bu bölümde, biz IIS'de OWIN barındırılacak.</span><span class="sxs-lookup"><span data-stu-id="3f235-114">In this section, we'll host OWIN in IIS.</span></span> <span data-ttu-id="3f235-115">Bu seçenek IIS'in olgun özellik kümesi ile birlikte bir OWIN Ardışık düzenin composability ve esneklik sağlar.</span><span class="sxs-lookup"><span data-stu-id="3f235-115">This option gives you the flexibility and composability of an OWIN pipeline together with the mature feature set of IIS.</span></span> <span data-ttu-id="3f235-116">Bu seçeneği kullanarak ASP.NET isteği işlem hattında OWIN uygulaması çalışır.</span><span class="sxs-lookup"><span data-stu-id="3f235-116">Using this option, the OWIN application runs in the ASP.NET request pipeline.</span></span>

<span data-ttu-id="3f235-117">İlk olarak, yeni bir ASP.NET Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3f235-117">First, create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="3f235-118">(Visual Studio 2012'de ASP.NET boş Web uygulaması proje türünü kullanın.)</span><span class="sxs-lookup"><span data-stu-id="3f235-118">(In Visual Studio 2012, use the ASP.NET Empty Web Application project type.)</span></span>

![](getting-started-with-owin-and-katana/_static/image1.png)

<span data-ttu-id="3f235-119">İçinde **yeni ASP.NET projesi** iletişim kutusunda **boş** şablonu.</span><span class="sxs-lookup"><span data-stu-id="3f235-119">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span>

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a><span data-ttu-id="3f235-120">NuGet paketleri Ekle</span><span class="sxs-lookup"><span data-stu-id="3f235-120">Add NuGet Packages</span></span>

<span data-ttu-id="3f235-121">Ardından, gerekli NuGet paketlerini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3f235-121">Next, add the required NuGet packages.</span></span> <span data-ttu-id="3f235-122">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="3f235-122">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="3f235-123">Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu yazın:</span><span class="sxs-lookup"><span data-stu-id="3f235-123">In the Package Manager Console window, type the following command:</span></span>

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a><span data-ttu-id="3f235-124">Başlangıç sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="3f235-124">Add a Startup Class</span></span>

<span data-ttu-id="3f235-125">Ardından, OWIN başlangıç sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3f235-125">Next, add an OWIN startup class.</span></span> <span data-ttu-id="3f235-126">Çözüm Gezgini'nde projeye sağ tıklayıp seçin **Ekle**, ardından **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="3f235-126">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span> <span data-ttu-id="3f235-127">İçinde **Yeni Öğe Ekle** iletişim kutusunda **Owın başlangıç sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="3f235-127">In the **Add New Item** dialog, select **Owin Startup class**.</span></span> <span data-ttu-id="3f235-128">Başlangıç sınıfı yapılandırma hakkında daha fazla bilgi için bkz. [OWIN başlangıç sınıfı algılama](owin-startup-class-detection.md).</span><span class="sxs-lookup"><span data-stu-id="3f235-128">For more info on configuring the startup class, see [OWIN Startup Class Detection](owin-startup-class-detection.md).</span></span>

![](getting-started-with-owin-and-katana/_static/image4.png)

<span data-ttu-id="3f235-129">Aşağıdaki kodu ekleyin `Startup1.Configuration` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3f235-129">Add the following code to the `Startup1.Configuration` method:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

<span data-ttu-id="3f235-130">Bu kod basit bir ara yazılım alan bir işlev uygulanan OWIN ardışık düzenine ekler. bir **Microsoft.Owin.IOwinContext** örneği.</span><span class="sxs-lookup"><span data-stu-id="3f235-130">This code adds a simple piece of middleware to the OWIN pipeline, implemented as a function that receives a **Microsoft.Owin.IOwinContext** instance.</span></span> <span data-ttu-id="3f235-131">Sunucu HTTP isteği aldığında, OWIN ardışık düzenine bir ara yazılım çağırır.</span><span class="sxs-lookup"><span data-stu-id="3f235-131">When the server receives an HTTP request, the OWIN pipeline invokes the middleware.</span></span> <span data-ttu-id="3f235-132">Ara yazılım, içerik türü yanıtı için ayarlar ve yanıt gövdesi olarak yazar.</span><span class="sxs-lookup"><span data-stu-id="3f235-132">The middleware sets the content type for the response and writes the response body.</span></span>

> [!NOTE]
> <span data-ttu-id="3f235-133">OWIN başlangıç sınıfı şablon, Visual Studio 2013'te kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3f235-133">The OWIN Startup class template is available in Visual Studio 2013.</span></span> <span data-ttu-id="3f235-134">Visual Studio 2012 kullanıyorsanız, yalnızca adında yeni bir boş Sınıf Ekle `Startup1`, aşağıdaki kodu yapıştırın:</span><span class="sxs-lookup"><span data-stu-id="3f235-134">If you are using Visual Studio 2012, just add a new empty class named `Startup1`, and paste in the following code:</span></span>


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a><span data-ttu-id="3f235-135">Uygulamayı çalıştırın</span><span class="sxs-lookup"><span data-stu-id="3f235-135">Run the Application</span></span>

<span data-ttu-id="3f235-136">Hata ayıklamayı başlatmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="3f235-136">Press F5 to begin debugging.</span></span> <span data-ttu-id="3f235-137">Visual Studio için bir tarayıcı penceresi açılır `http://localhost:*port*/`.</span><span class="sxs-lookup"><span data-stu-id="3f235-137">Visual Studio will open a browser window to `http://localhost:*port*/`.</span></span> <span data-ttu-id="3f235-138">Sayfa aşağıdaki gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="3f235-138">The page should look like the following:</span></span>

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a><span data-ttu-id="3f235-139">Bir konsol uygulamasında OWIN barındırma</span><span class="sxs-lookup"><span data-stu-id="3f235-139">Self-Host OWIN in a Console Application</span></span>

<span data-ttu-id="3f235-140">Bu uygulama, özel bir işlemde kendi kendine barındırma için IIS barındırma dönüştürmek kolay bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="3f235-140">It's easy to convert this application from IIS hosting to self-hosting in a custom process.</span></span> <span data-ttu-id="3f235-141">IIS barındırma ile IIS hem HTTP sunucusunu ve hizmetini barındıran işlemi olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="3f235-141">With IIS hosting, IIS acts as both the HTTP server and as the process that hosts the service.</span></span> <span data-ttu-id="3f235-142">Kendi kendine barındırma ile uygulamanızı bir işlem oluşturur ve kullanır **HttpListener** HTTP sunucusuyla sınıfı.</span><span class="sxs-lookup"><span data-stu-id="3f235-142">With self-hosting, your application creates the process and uses the **HttpListener** class as the HTTP server.</span></span>

<span data-ttu-id="3f235-143">Visual Studio'da yeni bir konsol uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3f235-143">In Visual Studio, create a new console application.</span></span> <span data-ttu-id="3f235-144">Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu yazın:</span><span class="sxs-lookup"><span data-stu-id="3f235-144">In the Package Manager Console window, type the following command:</span></span>

`Install-Package Microsoft.Owin.SelfHost -Pre`

<span data-ttu-id="3f235-145">Ekleme bir `Startup1` projeye bu öğreticinin 1 bölümünden sınıfı.</span><span class="sxs-lookup"><span data-stu-id="3f235-145">Add a `Startup1` class from part 1 of this tutorial to the project.</span></span> <span data-ttu-id="3f235-146">Bu sınıf değiştirmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="3f235-146">You don't need to modify this class.</span></span>

<span data-ttu-id="3f235-147">Uygulamanın uygulama `Main` yöntemini aşağıdaki şekilde.</span><span class="sxs-lookup"><span data-stu-id="3f235-147">Implement the application's `Main` method as follows.</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

<span data-ttu-id="3f235-148">Konsol uygulamasını çalıştırdığınızda, sunucuyu başlatır dinleme `http://localhost:9000`.</span><span class="sxs-lookup"><span data-stu-id="3f235-148">When you run the console application, the server starts listening to `http://localhost:9000`.</span></span> <span data-ttu-id="3f235-149">Bir web tarayıcısında bu adrese gidin, "Hello world" sayfası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="3f235-149">If you navigate to this address in a web browser, you will see the "Hello world" page.</span></span>

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a><span data-ttu-id="3f235-150">OWIN tanılama Ekle</span><span class="sxs-lookup"><span data-stu-id="3f235-150">Add OWIN Diagnostics</span></span>

<span data-ttu-id="3f235-151">İşlenmeyen özel durumları yakalar ve hata ayrıntılarını içeren bir HTML sayfası görüntüleyen bir ara yazılım Microsoft.Owin.Diagnostics paket içerir.</span><span class="sxs-lookup"><span data-stu-id="3f235-151">The Microsoft.Owin.Diagnostics package contains middleware that catches unhandled exceptions and displays an HTML page with error details.</span></span> <span data-ttu-id="3f235-152">Bu sayfa işlevlerini benzer şekilde adlandırılır ASP.NET hata sayfası "[Ölüm sarı ekran](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span><span class="sxs-lookup"><span data-stu-id="3f235-152">This page functions much like the ASP.NET error page that is sometimes called the "[yellow screen of death](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span></span> <span data-ttu-id="3f235-153">YSOD gibi geliştirme sırasında Katana hata sayfası yararlıdır, ancak üretim modunda devre dışı bırakmak için iyi bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="3f235-153">Like the YSOD, the Katana error page is useful during development, but it's a good practice to disable it in production mode.</span></span>

<span data-ttu-id="3f235-154">Tanılama paketini projenize yüklemek için Paket Yöneticisi konsolu penceresinde aşağıdaki komutu yazın:</span><span class="sxs-lookup"><span data-stu-id="3f235-154">To install the Diagnostics package in your project, type the following command in the Package Manager Console window:</span></span>

`install-package Microsoft.Owin.Diagnostics –Pre`

<span data-ttu-id="3f235-155">Kodda değişiklik, `Startup1.Configuration` yöntemini aşağıdaki şekilde:</span><span class="sxs-lookup"><span data-stu-id="3f235-155">Change the code in your `Startup1.Configuration` method as follows:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

<span data-ttu-id="3f235-156">Visual Studio özel durumla ilgili kesmemesi böylece artık uygulamayı hata ayıklama olmadan çalıştırmak için CTRL + F5'nı kullanın.</span><span class="sxs-lookup"><span data-stu-id="3f235-156">Now use CTRL+F5 to run the application without debugging, so that Visual Studio will not break on the exception.</span></span> <span data-ttu-id="3f235-157">Gittikten kadar uygulama önceki gibi davranır `http://localhost/fail`, bu noktada uygulama özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3f235-157">The application behaves the same as before, until you navigate to `http://localhost/fail`, at which point the application throws the exception.</span></span> <span data-ttu-id="3f235-158">Hata sayfası ara yazılımını özel durumu yakalar ve hata hakkında bilgi içeren bir HTML sayfası görüntüler.</span><span class="sxs-lookup"><span data-stu-id="3f235-158">The error page middleware will catch the exception and display an HTML page with information about the error.</span></span> <span data-ttu-id="3f235-159">Yığın, sorgu dizesi, tanımlama bilgileri, istek üst bilgisi ve OWIN ortam değişkenleri görmek için sekmeler tıklayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3f235-159">You can click the tabs to see the stack, query string, cookies, request header, and OWIN environment variables.</span></span>

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a><span data-ttu-id="3f235-160">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="3f235-160">Next Steps</span></span>

- [<span data-ttu-id="3f235-161">OWIN Başlangıç Sınıfı Algılama</span><span class="sxs-lookup"><span data-stu-id="3f235-161">OWIN Startup Class Detection</span></span>](owin-startup-class-detection.md)
- [<span data-ttu-id="3f235-162">Barındırılan ASP.NET Web API'si için OWIN kullanın</span><span class="sxs-lookup"><span data-stu-id="3f235-162">Use OWIN to Self-Host ASP.NET Web API</span></span>](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [<span data-ttu-id="3f235-163">SignalR barındırma için OWIN kullanın</span><span class="sxs-lookup"><span data-stu-id="3f235-163">Use OWIN to Self-Host SignalR</span></span>](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
