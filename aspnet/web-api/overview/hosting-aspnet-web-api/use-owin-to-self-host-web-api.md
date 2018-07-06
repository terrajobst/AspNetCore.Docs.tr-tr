---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: OWIN, ASP.NET Web API 2 barındırma kullanmasını | Microsoft Docs
author: rick-anderson
description: Bu öğreticide, ASP.NET Web API OWIN barındırma Web API çerçevesini kullanarak bir konsol uygulamasında barındırmak gösterilir. .NET (OWIN) d için Web arabirimi Aç...
ms.author: aspnetcontent
ms.date: 07/09/2013
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9fba2774e3873d32115a14fa0c84b99466eda04f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830917"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a><span data-ttu-id="6bc59-104">ASP.NET Web API 2 barındırma için OWIN kullanın</span><span class="sxs-lookup"><span data-stu-id="6bc59-104">Use OWIN to Self-Host ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="6bc59-105">tarafından [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span><span class="sxs-lookup"><span data-stu-id="6bc59-105">by [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span></span>

> <span data-ttu-id="6bc59-106">Bu öğreticide, ASP.NET Web API OWIN barındırma Web API çerçevesini kullanarak bir konsol uygulamasında barındırmak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="6bc59-106">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="6bc59-107">[.NET için açık Web arabirimi](http://owin.org) (OWIN) .NET web sunucuları ve web uygulaması arasında bir Özet tanımlar.</span><span class="sxs-lookup"><span data-stu-id="6bc59-107">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="6bc59-108">OWIN OWIN kendi işleminizde IIS dışında bir web uygulaması kendi kendine barındırma için ideal hale getirir sunucu web uygulamasından ayırır.</span><span class="sxs-lookup"><span data-stu-id="6bc59-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6bc59-109">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="6bc59-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6bc59-110">[Visual Studio 2013'ün](https://www.microsoft.com/visualstudio/eng/2013-downloads) (Visual Studio 2012 ile de çalışır)</span><span class="sxs-lookup"><span data-stu-id="6bc59-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (also works with Visual Studio 2012)</span></span>
> - <span data-ttu-id="6bc59-111">Web API 2</span><span class="sxs-lookup"><span data-stu-id="6bc59-111">Web API 2</span></span>


> [!NOTE]
> <span data-ttu-id="6bc59-112">Bu öğreticinin için tam kaynak kodunu bulabilirsiniz [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="6bc59-112">You can find the complete source code for this tutorial at [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="6bc59-113">Bir konsol uygulaması oluşturun</span><span class="sxs-lookup"><span data-stu-id="6bc59-113">Create a Console Application</span></span>

<span data-ttu-id="6bc59-114">Üzerinde **dosya** menüsünde tıklayın **yeni**, ardından **proje**.</span><span class="sxs-lookup"><span data-stu-id="6bc59-114">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="6bc59-115">Gelen **yüklü şablonlar**, Visual C# altında tıklayın **Windows** ve ardından **konsol uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="6bc59-115">From **Installed Templates**, under Visual C#, click **Windows** and then click **Console Application**.</span></span> <span data-ttu-id="6bc59-116">"OwinSelfhostSample" Projeyi adlandırın ve tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="6bc59-116">Name the project "OwinSelfhostSample" and click **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="6bc59-117">Web API ve OWIN paketleri Ekle</span><span class="sxs-lookup"><span data-stu-id="6bc59-117">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="6bc59-118">Gelen **Araçları** menüsünde tıklatın **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="6bc59-118">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span> <span data-ttu-id="6bc59-119">Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="6bc59-119">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="6bc59-120">Bu Webapı OWIN selfhost paketi ve tüm gerekli OWIN paketlerini yükler.</span><span class="sxs-lookup"><span data-stu-id="6bc59-120">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="6bc59-121">Web API'si için yapılandırma barındırma</span><span class="sxs-lookup"><span data-stu-id="6bc59-121">Configure Web API for Self-Host</span></span>

<span data-ttu-id="6bc59-122">Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Ekle** / **sınıfı** yeni bir sınıf eklemek için.</span><span class="sxs-lookup"><span data-stu-id="6bc59-122">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="6bc59-123">Sınıf adını `Startup`.</span><span class="sxs-lookup"><span data-stu-id="6bc59-123">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="6bc59-124">Tüm bu dosya ortak kodun aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="6bc59-124">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="6bc59-125">Web API denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="6bc59-125">Add a Web API Controller</span></span>

<span data-ttu-id="6bc59-126">Ardından, bir Web API denetleyici sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6bc59-126">Next, add a Web API controller class.</span></span> <span data-ttu-id="6bc59-127">Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Ekle** / **sınıfı** yeni bir sınıf eklemek için.</span><span class="sxs-lookup"><span data-stu-id="6bc59-127">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="6bc59-128">Sınıf adını `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="6bc59-128">Name the class `ValuesController`.</span></span>

<span data-ttu-id="6bc59-129">Tüm bu dosya ortak kodun aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="6bc59-129">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a><span data-ttu-id="6bc59-130">OWIN ana bilgisayarı başlatılamıyor ve HttpClient kullanan bir istek oluşturun</span><span class="sxs-lookup"><span data-stu-id="6bc59-130">Start the OWIN Host and Make a Request Using HttpClient</span></span>

<span data-ttu-id="6bc59-131">Tüm ortak kod Program.cs dosyasındaki aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="6bc59-131">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a><span data-ttu-id="6bc59-132">Uygulamayı Çalıştırma</span><span class="sxs-lookup"><span data-stu-id="6bc59-132">Running the Application</span></span>

<span data-ttu-id="6bc59-133">Uygulamayı çalıştırmak için Visual Studio'da F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="6bc59-133">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="6bc59-134">Çıktı aşağıdaki gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="6bc59-134">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="6bc59-135">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6bc59-135">Additional Resources</span></span>

[<span data-ttu-id="6bc59-136">Project Katana’ya Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="6bc59-136">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="6bc59-137">Bir Azure çalışan rolünde ASP.NET Web API barındırma</span><span class="sxs-lookup"><span data-stu-id="6bc59-137">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
