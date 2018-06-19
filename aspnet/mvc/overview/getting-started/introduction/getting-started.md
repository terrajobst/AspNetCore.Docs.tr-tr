---
uid: mvc/overview/getting-started/introduction/getting-started
title: ASP.NET MVC 5 ile çalışmaya başlama | Microsoft Docs
author: Rick-Anderson
description: 'Not: Burada Visual Studio 2015 kullanarak bu öğreticinin güncelleştirilmiş bir sürümü kullanılabilir. Yeni öğretici birçok improvem sağlar ASP.NET Core MVC 6 kullanır...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 0f1fd2026691d3bc0e81b20a9731879d7a6041bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868327"
---
<a name="getting-started-with-aspnet-mvc-5"></a><span data-ttu-id="8598a-104">.NET MVC 5 ile Çalışmaya Başlama</span><span class="sxs-lookup"><span data-stu-id="8598a-104">Getting Started with ASP.NET MVC 5</span></span>
====================
<span data-ttu-id="8598a-105">tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="8598a-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 <span data-ttu-id="8598a-106">Bu öğretici bir ASP.NET MVC 5 web uygulaması kullanılarak oluşturmaya temellerini öğretmek [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="8598a-106">This tutorial will teach you the basics of building an ASP.NET MVC 5 web app using [Visual Studio 2017](https://www.visualstudio.com/).</span></span> <span data-ttu-id="8598a-107">Öğretici için son kaynak bulunan [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span><span class="sxs-lookup"><span data-stu-id="8598a-107">Final Source for tutorial located on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span></span>


 <span data-ttu-id="8598a-108">Bu öğretici tarafından yazıldı [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , ve [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )</span><span class="sxs-lookup"><span data-stu-id="8598a-108">This tutorial was written by [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[@scottgu](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](https://twitter.com/shanselman) ), and [Rick Anderson](https://twitter.com/RickAndMSFT) ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )</span></span>

 <span data-ttu-id="8598a-109">Bu uygulamayı Azure'a dağıtmak için bir Azure hesabı gerekir:</span><span class="sxs-lookup"><span data-stu-id="8598a-109">You need an Azure account to deploy this app to Azure:</span></span>

 - <span data-ttu-id="8598a-110">Yapabilecekleriniz [ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -krediler alırsınız, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8598a-110">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
 - <span data-ttu-id="8598a-111">Yapabilecekleriniz [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -MSDN aboneliğiniz size kredi verir Ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.</span><span class="sxs-lookup"><span data-stu-id="8598a-111">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>


## <a name="getting-started"></a><span data-ttu-id="8598a-112">Başlarken</span><span class="sxs-lookup"><span data-stu-id="8598a-112">Getting Started</span></span>

<span data-ttu-id="8598a-113">Başlangıç yüklenmesi ve çalıştırılması [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="8598a-113">Start by installing and running [Visual Studio 2017](https://www.visualstudio.com/).</span></span>

<span data-ttu-id="8598a-114">Visual Studio, bir IDE veya tümleşik geliştirme ortamını değil.</span><span class="sxs-lookup"><span data-stu-id="8598a-114">Visual Studio is an IDE, or integrated development environment.</span></span> <span data-ttu-id="8598a-115">Belgeleri yazmak için Microsoft Word kullanma gibi uygulamaları oluşturmak için bir IDE kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="8598a-115">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="8598a-116">Visual Studio'da bir listesi için çeşitli seçenekleri gösteren alt boyunca yoktur.</span><span class="sxs-lookup"><span data-stu-id="8598a-116">In Visual Studio there's a list along the bottom showing various options available to you.</span></span> <span data-ttu-id="8598a-117">IDE içinde görevleri gerçekleştirmek için başka bir yol sağlayan bir menüsünü yoktur.</span><span class="sxs-lookup"><span data-stu-id="8598a-117">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="8598a-118">(Örneğin, seçmek yerine **yeni proje** gelen **Başlat** sayfasında, menüsünü kullanın ve seçin **dosya** &gt; **yeni proje**.)</span><span class="sxs-lookup"><span data-stu-id="8598a-118">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a><span data-ttu-id="8598a-119">İlk uygulamanızı oluşturma</span><span class="sxs-lookup"><span data-stu-id="8598a-119">Creating Your First Application</span></span>

<span data-ttu-id="8598a-120">Tıklatın **yeni proje**, ardından Visual C# sol tarafta, ardından **Web** ve ardından **ASP.NET Web uygulaması (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="8598a-120">Click **New Project**, then select Visual C# on the left, then **Web** and then select **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="8598a-121">Projeniz "MvcMovie" olarak adlandırın ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="8598a-121">Name your project "MvcMovie" and then click **OK**.</span></span>

![](getting-started/_static/image2.png)

<span data-ttu-id="8598a-122">İçinde **yeni ASP.NET projesi** iletişim kutusunda, tıklatın **MVC** ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="8598a-122">In the **New ASP.NET Project** dialog, click **MVC** and then click **OK**.</span></span>

![](getting-started/_static/image3.png)

<span data-ttu-id="8598a-123">Visual Studio varsayılan bir şablon, yeni oluşturduğunuz, ASP.NET MVC proje için kullanılan çalışan bir uygulama şu anda herhangi bir şey yapmadan elinizde!</span><span class="sxs-lookup"><span data-stu-id="8598a-123">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="8598a-124">Bu bir basit "Hello World!" olur</span><span class="sxs-lookup"><span data-stu-id="8598a-124">This is a simple "Hello World!"</span></span> <span data-ttu-id="8598a-125">Proje ve kullanıcının uygulamanızı başlatmak için uygun bir yerdir.</span><span class="sxs-lookup"><span data-stu-id="8598a-125">project, and it's a good place to start your application.</span></span>

![](getting-started/_static/image4.png)

<span data-ttu-id="8598a-126">Hata ayıklamayı başlatmak için F5'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="8598a-126">Click F5 to start debugging.</span></span> <span data-ttu-id="8598a-127">F5 neden başlatmak Visual Studio [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) ve web uygulamanızı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8598a-127">F5 causes Visual Studio to start [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) and run your web app.</span></span> <span data-ttu-id="8598a-128">Visual Studio bir tarayıcı başlatılır ve uygulamanın giriş sayfasını açar.</span><span class="sxs-lookup"><span data-stu-id="8598a-128">Visual Studio then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="8598a-129">Tarayıcının adres çubuğunda diyor bildirimi `localhost:port#` bir şey yok gibi ve `example.com`.</span><span class="sxs-lookup"><span data-stu-id="8598a-129">Notice that the address bar of the browser says `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="8598a-130">Çünkü `localhost` her zaman, bu durumda yalnızca yerleşik uygulama çalışan kendi yerel bilgisayara gösterir.</span><span class="sxs-lookup"><span data-stu-id="8598a-130">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="8598a-131">Visual Studio web projesini çalıştığında, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8598a-131">When Visual Studio runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="8598a-132">Aşağıdaki resimde 1234 bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="8598a-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="8598a-133">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="8598a-133">When you run the application, you'll see a different port number.</span></span>

![](getting-started/_static/image5.png)

<span data-ttu-id="8598a-134">Bu varsayılan şablonu kullanıma hazır giriş, kişi ve ilgili sayfaları sunar.</span><span class="sxs-lookup"><span data-stu-id="8598a-134">Right out of the box this default template gives you Home, Contact and About pages.</span></span> <span data-ttu-id="8598a-135">Yukarıdaki resimde göstermiyor **giriş**, **hakkında** ve **kişi** bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="8598a-135">The image above doesn't show the **Home**, **About** and **Contact** links.</span></span> <span data-ttu-id="8598a-136">Tarayıcı pencerenizin boyutuna bağlı olarak, bu bağlantıları görmek için Gezinti simgesini gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="8598a-136">Depending on the size of your browser window, you might need to click the navigation icon to see these links.</span></span>

![](getting-started/_static/image6.png)  

<span data-ttu-id="8598a-137">Uygulama kayıt ve oturum açmak için destek de sağlar.</span><span class="sxs-lookup"><span data-stu-id="8598a-137">The application also provides support to register and log in.</span></span> <span data-ttu-id="8598a-138">Bu uygulama şeklini değiştirmek ve biraz ASP.NET MVC hakkında bilgi almak için sonraki adımdır bakın.</span><span class="sxs-lookup"><span data-stu-id="8598a-138">The next step is to change how this application works and learn a little bit about ASP.NET MVC.</span></span> <span data-ttu-id="8598a-139">ASP.NET MVC uygulamasını kapatın ve biraz kod değiştirelim.</span><span class="sxs-lookup"><span data-stu-id="8598a-139">Close the ASP.NET MVC application and let's change some code.</span></span>

<span data-ttu-id="8598a-140">Geçerli öğreticilerin bir listesi için bkz: [makaleleri önerilen MVC](../mvc-learning-sequence.md).</span><span class="sxs-lookup"><span data-stu-id="8598a-140">For a list of current tutorials, see [MVC recommended articles](../mvc-learning-sequence.md).</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="8598a-141">Azure üzerinde çalışan bu uygulamayı bakın</span><span class="sxs-lookup"><span data-stu-id="8598a-141">See this App Running on Azure</span></span>

<span data-ttu-id="8598a-142">Canlı web uygulaması çalışan tamamlanmış site görmek ister misiniz?</span><span class="sxs-lookup"><span data-stu-id="8598a-142">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="8598a-143">Aşağıdaki düğmeyi tıklatarak, uygulama tam sürümü Azure hesabınızda dağıtabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8598a-143">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

<span data-ttu-id="8598a-144">Bu çözüm Azure'a dağıtmak için bir Azure hesabınız olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8598a-144">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="8598a-145">Bir hesap zaten yoksa, aşağıdaki seçenekler vardır:</span><span class="sxs-lookup"><span data-stu-id="8598a-145">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="8598a-146">[Ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -krediler alırsınız, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8598a-146">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="8598a-147">[MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -MSDN aboneliğiniz size kredi verir Ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.</span><span class="sxs-lookup"><span data-stu-id="8598a-147">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8598a-148">Next</span><span class="sxs-lookup"><span data-stu-id="8598a-148">Next</span></span>](adding-a-controller.md)
