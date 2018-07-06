---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Katana'da Windows kimlik doğrulamasını etkinleştirme | Microsoft Docs
author: MikeWasson
description: "Bu makalede katana'da Windows kimlik doğrulamasının nasıl etkinleştirileceği gösterilmektedir. İki senaryoyu da kapsamaktadır: IIS konağa Katana ve Kat barındırma için HttpListener kullanarak..."
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 80bdc3c76c8867dc559e80a794ac8bee84b47646
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826332"
---
<a name="enabling-windows-authentication-in-katana"></a><span data-ttu-id="0c63e-104">Katana'da Windows kimlik doğrulamasını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="0c63e-104">Enabling Windows Authentication in Katana</span></span>
====================
<span data-ttu-id="0c63e-105">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0c63e-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="0c63e-106">Bu makalede katana'da Windows kimlik doğrulamasının nasıl etkinleştirileceği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="0c63e-106">This article shows how to enable Windows Authentication in Katana.</span></span> <span data-ttu-id="0c63e-107">İki senaryoyu da kapsamaktadır: IIS konağa Katana ve Katana özel bir işlemde kendiliğinden barındırmak için HttpListener kullanarak.</span><span class="sxs-lookup"><span data-stu-id="0c63e-107">It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process.</span></span> <span data-ttu-id="0c63e-108">Bu makalede gözden geçirme için teşekkür ederiz Barry Dorrans, David Matson ve Chris Ross.</span><span class="sxs-lookup"><span data-stu-id="0c63e-108">Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.</span></span>


<span data-ttu-id="0c63e-109">Katana Microsoft'un uygulaması olan [OWIN](http://owin.org/), .NET için açık Web arabirimi.</span><span class="sxs-lookup"><span data-stu-id="0c63e-109">Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET.</span></span> <span data-ttu-id="0c63e-110">OWIN ve Katana giriş edinebilirsiniz [burada](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="0c63e-110">You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="0c63e-111">OWIN mimari birkaç katmana sahiptir:</span><span class="sxs-lookup"><span data-stu-id="0c63e-111">The OWIN architecture has several layers:</span></span>

- <span data-ttu-id="0c63e-112">Ana bilgisayarı: OWIN ardışık düzenini çalıştığı işlemin yönetir.</span><span class="sxs-lookup"><span data-stu-id="0c63e-112">Host: Manages the process in which the OWIN pipeline runs.</span></span>
- <span data-ttu-id="0c63e-113">Sunucu: bir ağ yuva açar ve isteklerini dinler.</span><span class="sxs-lookup"><span data-stu-id="0c63e-113">Server: Opens a network socket and listens for requests.</span></span>
- <span data-ttu-id="0c63e-114">Ara yazılım: HTTP istek ve yanıt işler.</span><span class="sxs-lookup"><span data-stu-id="0c63e-114">Middleware: Processes the HTTP request and response.</span></span>

<span data-ttu-id="0c63e-115">Katana şu anda iki sunucu, Windows tümleşik kimlik doğrulaması ikisi için de destek sağlar:</span><span class="sxs-lookup"><span data-stu-id="0c63e-115">Katana currently provides two servers, both of which support Windows Integrated Authentication:</span></span>

- <span data-ttu-id="0c63e-116">**Microsoft.Owin.Host.SystemWeb**.</span><span class="sxs-lookup"><span data-stu-id="0c63e-116">**Microsoft.Owin.Host.SystemWeb**.</span></span> <span data-ttu-id="0c63e-117">IIS ile ASP.NET ardışık düzenini kullanır.</span><span class="sxs-lookup"><span data-stu-id="0c63e-117">Uses IIS with the ASP.NET pipeline.</span></span>
- <span data-ttu-id="0c63e-118">**Microsoft.Owin.Host.HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="0c63e-118">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="0c63e-119">Kullanan [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="0c63e-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span></span> <span data-ttu-id="0c63e-120">Bu sunucu şu anda kendi kendine barındırma olduğunda varsayılan seçenek olan Katana.</span><span class="sxs-lookup"><span data-stu-id="0c63e-120">This server is currently the default option when self-hosting Katana.</span></span>

> [!NOTE]
> <span data-ttu-id="0c63e-121">Bu işlevselliği sunucular kullanılabilir olduğundan Katana şu anda OWIN ara yazılımı için Windows kimlik doğrulaması sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="0c63e-121">Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.</span></span>


## <a name="windows-authentication-in-iis"></a><span data-ttu-id="0c63e-122">IIS Windows kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="0c63e-122">Windows Authentication in IIS</span></span>

<span data-ttu-id="0c63e-123">Microsoft.Owin.Host.SystemWeb kullanarak, yalnızca IIS Windows kimlik doğrulamasını etkinleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0c63e-123">Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.</span></span>

<span data-ttu-id="0c63e-124">"ASP.NET boş Web uygulaması" proje şablonunu kullanarak yeni bir ASP.NET uygulaması oluşturarak başlayalım.</span><span class="sxs-lookup"><span data-stu-id="0c63e-124">Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.</span></span>

![](enabling-windows-authentication-in-katana/_static/image1.png)

<span data-ttu-id="0c63e-125">Ardından, NuGet paketlerini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0c63e-125">Next, add NuGet packages.</span></span> <span data-ttu-id="0c63e-126">Gelen **Araçları** menüsünde **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="0c63e-126">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="0c63e-127">Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="0c63e-127">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

<span data-ttu-id="0c63e-128">Şimdi adlı bir sınıf ekleyin `Startup` aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="0c63e-128">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

<span data-ttu-id="0c63e-129">Tüm OWIN, IIS üzerinde çalışan bir "Hello world" uygulaması oluşturmak için gereken budur.</span><span class="sxs-lookup"><span data-stu-id="0c63e-129">That's all you need to create a "Hello world" application for OWIN, running on IIS.</span></span> <span data-ttu-id="0c63e-130">Uygulamada hata ayıklamak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="0c63e-130">Press F5 to debug the application.</span></span> <span data-ttu-id="0c63e-131">"Hello World!" görmeniz gerekir</span><span class="sxs-lookup"><span data-stu-id="0c63e-131">You should see "Hello World!"</span></span> <span data-ttu-id="0c63e-132">tarayıcı penceresinde.</span><span class="sxs-lookup"><span data-stu-id="0c63e-132">in the browser window.</span></span>

![](enabling-windows-authentication-in-katana/_static/image2.png)

<span data-ttu-id="0c63e-133">Ardından, IIS Express Windows kimlik doğrulaması etkinleştiririz.</span><span class="sxs-lookup"><span data-stu-id="0c63e-133">Next, we'll enable Windows Authentication in IIS Express.</span></span> <span data-ttu-id="0c63e-134">Gelen **görünümü** menüsünde **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="0c63e-134">From the **View** menu, select **Properties**.</span></span> <span data-ttu-id="0c63e-135">Proje özelliklerini görüntülemek için Çözüm Gezgini'nde proje adının üzerine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="0c63e-135">Click on the project name in Solution Explorer to view the project properties.</span></span>

<span data-ttu-id="0c63e-136">İçinde **özellikleri** penceresinde **anonim kimlik doğrulaması** için **devre dışı bırakılmış** ayarlayıp **Windows kimlik doğrulaması** için  **Etkin**.</span><span class="sxs-lookup"><span data-stu-id="0c63e-136">In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.</span></span>

![](enabling-windows-authentication-in-katana/_static/image3.png)

<span data-ttu-id="0c63e-137">Visual Studio'dan uygulamayı çalıştırdığınızda, IIS Express kullanıcının Windows kimlik bilgilerini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="0c63e-137">When you run the application from Visual Studio, IIS Express will require the user's Windows credentials.</span></span> <span data-ttu-id="0c63e-138">Bunu kullanarak görmek [Fiddler](http://fiddler2.com/home) veya başka bir HTTP hata ayıklama aracı.</span><span class="sxs-lookup"><span data-stu-id="0c63e-138">You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool.</span></span> <span data-ttu-id="0c63e-139">HTTP yanıt örneği burada verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="0c63e-139">Here is an example HTTP response:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

<span data-ttu-id="0c63e-140">Bu yanıt WWW-Authenticate üstbilgi sunucu desteklediğini göstermek [anlaş](http://www.ietf.org/rfc/rfc4559.txt) protokolü, Kerberos veya NTLM kullanır.</span><span class="sxs-lookup"><span data-stu-id="0c63e-140">The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.</span></span>

<span data-ttu-id="0c63e-141">Bir sunucuya uygulamayı dağıttığınızda, daha sonra izleyin [adımları](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) o sunucuda IIS Windows kimlik doğrulamasını etkinleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="0c63e-141">Later, when you deploy the application to a server, follow [these steps](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.</span></span>

## <a name="windows-authentication-in-httplistener"></a><span data-ttu-id="0c63e-142">HttpListener Windows kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="0c63e-142">Windows Authentication in HttpListener</span></span>

<span data-ttu-id="0c63e-143">Katana barındırma için Microsoft.Owin.Host.HttpListener kullanıyorsanız, doğrudan Windows kimlik doğrulamasını etkinleştirebilirsiniz **HttpListener** örneği.</span><span class="sxs-lookup"><span data-stu-id="0c63e-143">If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.</span></span>

<span data-ttu-id="0c63e-144">İlk olarak, yeni bir konsol uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0c63e-144">First, create a new console application.</span></span> <span data-ttu-id="0c63e-145">Ardından, NuGet paketlerini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0c63e-145">Next, add NuGet packages.</span></span> <span data-ttu-id="0c63e-146">Gelen **Araçları** menüsünde **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="0c63e-146">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="0c63e-147">Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="0c63e-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

<span data-ttu-id="0c63e-148">Şimdi adlı bir sınıf ekleyin `Startup` aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="0c63e-148">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

<span data-ttu-id="0c63e-149">Bu sınıf önce aynı "Hello world" örnek uygular, ancak ayrıca Windows kimlik doğrulaması kimlik doğrulama düzeni ayarlar.</span><span class="sxs-lookup"><span data-stu-id="0c63e-149">This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.</span></span>

<span data-ttu-id="0c63e-150">İçinde `Main` işlev, OWIN ardışık düzenini başlatın:</span><span class="sxs-lookup"><span data-stu-id="0c63e-150">Inside the `Main` function, start the OWIN pipeline:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

<span data-ttu-id="0c63e-151">Uygulamayı Windows kimlik doğrulaması kullandığını doğrulayın Fiddler'da bir istek gönderebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="0c63e-151">You can send a request in Fiddler to confirm that the application is using Windows Authentication:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a><span data-ttu-id="0c63e-152">İlgili Konular</span><span class="sxs-lookup"><span data-stu-id="0c63e-152">Related Topics</span></span>

[<span data-ttu-id="0c63e-153">Project Katana’ya Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="0c63e-153">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)

[<span data-ttu-id="0c63e-154">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="0c63e-154">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[<span data-ttu-id="0c63e-155">MVC 5 OWIN form kimlik doğrulamasını anlama</span><span class="sxs-lookup"><span data-stu-id="0c63e-155">Understanding OWIN Forms Authentication in MVC 5</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
