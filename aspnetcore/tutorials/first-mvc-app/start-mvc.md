---
title: "ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama"
author: rick-anderson
description: "ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama"
keywords: ASP.NET Core, MVC
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 6e233a0114967405a67b05365e0125620441efb4
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="e6930-104">ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="e6930-104">Getting started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="e6930-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e6930-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e6930-106">Bu öğretici bir ASP.NET Core MVC web uygulaması kullanılarak oluşturmaya temellerini öğretmek [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="e6930-106">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio 2017](https://www.visualstudio.com/).</span></span> [!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="e6930-107">Bu öğretici 3 sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="e6930-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="e6930-108">macOS: [Mac için Visual Studio ile ASP.NET Core MVC uygulama oluşturma](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e6930-108">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="e6930-109">Windows: [Visual Studio ile ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e6930-109">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="e6930-110">macOS, Linux ve Windows: [Visual Studio Code ile bir ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e6930-110">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

<span data-ttu-id="e6930-111">Visual Studio 2015 sürümünde Bu öğretici için bkz: [ASP.NET Core belgeleri PDF biçimli VS 2015 sürümü](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).</span><span class="sxs-lookup"><span data-stu-id="e6930-111">For the Visual Studio 2015 version of this tutorial, see the [VS 2015 version of ASP.NET Core documentation in PDF format](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="e6930-112">Visual Studio ve .NET Core yükleyin</span><span class="sxs-lookup"><span data-stu-id="e6930-112">Install Visual Studio and .NET Core</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e6930-113">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="e6930-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e6930-114">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="e6930-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e6930-115">Visual Studio Community 2017 yükleyin.</span><span class="sxs-lookup"><span data-stu-id="e6930-115">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="e6930-116">Topluluk indirme seçin.</span><span class="sxs-lookup"><span data-stu-id="e6930-116">Select the Community download.</span></span> <span data-ttu-id="e6930-117">Visual Studio yüklü 2017 varsa bu adımı atlayın.</span><span class="sxs-lookup"><span data-stu-id="e6930-117">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="e6930-118">Visual Studio 2017 giriş sayfası yükleyicisi</span><span class="sxs-lookup"><span data-stu-id="e6930-118">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="e6930-119">Yükleyiciyi çalıştırın ve aşağıdaki iş yüklerini seçin:</span><span class="sxs-lookup"><span data-stu-id="e6930-119">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="e6930-120">**ASP.NET ve web geliştirme** (altında **Web ve bulut**)</span><span class="sxs-lookup"><span data-stu-id="e6930-120">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="e6930-121">**.NET core platformlar arası geliştirme** (altında **diğer Toolsets**)</span><span class="sxs-lookup"><span data-stu-id="e6930-121">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET ve web geliştirme ** (altında ** Web ve bulut **)](start-mvc/_static/web_workload.png)

![* *.NET çekirdek arası arası platfrom geliştirme ** (altında ** diğer Toolsets **)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="e6930-124">Bir web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="e6930-124">Create a web app</span></span>

<span data-ttu-id="e6930-125">Visual Studio'dan seçin **Dosya > Yeni > Proje**.</span><span class="sxs-lookup"><span data-stu-id="e6930-125">From Visual Studio, select  **File > New > Project**.</span></span>

![Dosya > Yeni > Proje](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="e6930-127">Tamamlamak **yeni proje** iletişim:</span><span class="sxs-lookup"><span data-stu-id="e6930-127">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="e6930-128">Sol bölmede, dokunun **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="e6930-128">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="e6930-129">Orta bölmede, dokunun **ASP.NET çekirdek Web uygulaması (.NET çekirdek)**</span><span class="sxs-lookup"><span data-stu-id="e6930-129">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="e6930-130">(Kod kopyaladığınızda, ad alanı eşleşecek şekilde "MvcMovie" proje adı önemlidir.) "MvcMovie" proje adı</span><span class="sxs-lookup"><span data-stu-id="e6930-130">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="e6930-131">Dokunun **Tamam**</span><span class="sxs-lookup"><span data-stu-id="e6930-131">Tap **OK**</span></span>

![<span data-ttu-id="e6930-132">Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web .net core</span><span class="sxs-lookup"><span data-stu-id="e6930-132">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e6930-133">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="e6930-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e6930-134">Tamamlamak **yeni ASP.NET çekirdek Web uygulaması (.NET Core) - MvcMovie** iletişim:</span><span class="sxs-lookup"><span data-stu-id="e6930-134">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="e6930-135">Sürüm Seçici açılan kutusunda **ASP.NET Core 2.-**</span><span class="sxs-lookup"><span data-stu-id="e6930-135">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="e6930-136">Seçin **Application(Model-View-Controller) Web**</span><span class="sxs-lookup"><span data-stu-id="e6930-136">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="e6930-137">Dokunun **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="e6930-137">Tap **OK**.</span></span>

![<span data-ttu-id="e6930-138">Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web .net core</span><span class="sxs-lookup"><span data-stu-id="e6930-138">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e6930-139">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="e6930-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e6930-140">Tamamlamak **yeni ASP.NET çekirdek Web uygulaması (.NET Core) - MvcMovie** iletişim:</span><span class="sxs-lookup"><span data-stu-id="e6930-140">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="e6930-141">Sürüm Seçici açılan kutu dokunun içinde **ASP.NET Core 1.1**</span><span class="sxs-lookup"><span data-stu-id="e6930-141">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="e6930-142">Dokunun **Web uygulaması**</span><span class="sxs-lookup"><span data-stu-id="e6930-142">Tap **Web Application**</span></span>
* <span data-ttu-id="e6930-143">Varsayılan tutmak **doğrulaması yok**</span><span class="sxs-lookup"><span data-stu-id="e6930-143">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="e6930-144">Dokunun **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="e6930-144">Tap **OK**.</span></span>

![Yeni ASP.NET Core web uygulaması](start-mvc/_static/p3.png)

---

<span data-ttu-id="e6930-146">Visual Studio, yeni oluşturduğunuz MVC proje için varsayılan bir şablon kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e6930-146">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="e6930-147">Bir çalışma şu anda bir proje adı girerek ve birkaç seçenek seçerek uygulamanız.</span><span class="sxs-lookup"><span data-stu-id="e6930-147">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="e6930-148">Bu bir basit başlangıç projesi ve başlatmak için uygun bir yerdir,</span><span class="sxs-lookup"><span data-stu-id="e6930-148">This is a simple starter project, and it's a good place to start,</span></span>

<span data-ttu-id="e6930-149">Dokunun **F5** uygulamayı hata ayıklama modunda çalıştırmak için veya **Ctrl-F5** olmayan hata ayıklama modunda.</span><span class="sxs-lookup"><span data-stu-id="e6930-149">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Uygulamayı çalıştırma](start-mvc/_static/1.png)

* <span data-ttu-id="e6930-151">Visual Studio başlatır [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamanızı çalışır.</span><span class="sxs-lookup"><span data-stu-id="e6930-151">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="e6930-152">Adres çubuğunun bildirim `localhost:port#` bir şey yok gibi ve `example.com`.</span><span class="sxs-lookup"><span data-stu-id="e6930-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e6930-153">Çünkü `localhost` , yerel bilgisayarınızın standart barındırıcı adıdır.</span><span class="sxs-lookup"><span data-stu-id="e6930-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e6930-154">Visual Studio web projesini oluşturduğunda, rastgele bir bağlantı noktası web sunucusu için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e6930-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="e6930-155">Yukarıdaki resimde 5000 bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="e6930-155">In the image above, the port number is 5000.</span></span> <span data-ttu-id="e6930-156">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="e6930-156">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="e6930-157">Uygulama başlatma **Ctrl + F5** (olmayan hata ayıklama modu), kod değişiklikleri yapabilir, dosyayı kaydedin, tarayıcıyı yenilemek ve kod değişiklikleri görmek olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="e6930-157">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="e6930-158">Çoğu geliştirici, hızlı bir şekilde uygulamayı başlatın ve değişiklikleri görmek için olmayan hata ayıklama modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="e6930-158">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="e6930-159">Hata ayıklama veya hata ayıklama olmayan modundan uygulamada başlatabilirsiniz **hata ayıklama** menü öğesi:</span><span class="sxs-lookup"><span data-stu-id="e6930-159">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menü hata ayıklama](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="e6930-161">Uygulama dokunarak ayıklayabilirsiniz **IIS Express** düğmesi</span><span class="sxs-lookup"><span data-stu-id="e6930-161">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="e6930-163">Varsayılan şablonu, çalışma sunar **hakkında giriş** ve **kişi** bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="e6930-163">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="e6930-164">Yukarıdaki tarayıcı resimde bu bağlantıları göstermez.</span><span class="sxs-lookup"><span data-stu-id="e6930-164">The browser image above doesn't show these links.</span></span> <span data-ttu-id="e6930-165">Tarayıcınız boyutuna bağlı olarak, bunları göstermek için Gezinti simgesini gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="e6930-165">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![sağ üstteki gezinti simgesi](start-mvc/_static/2.png)

<span data-ttu-id="e6930-167">Hata ayıklama modunda çalışıyormuş dokunun **Shift + F5** hata ayıklamasını durdurmak için.</span><span class="sxs-lookup"><span data-stu-id="e6930-167">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="e6930-168">Bu öğreticinin sonraki bölümünde, biz MVC hakkında bilgi edinin ve biraz kod yazmaya başlamadan.</span><span class="sxs-lookup"><span data-stu-id="e6930-168">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="e6930-169">Sonraki</span><span class="sxs-lookup"><span data-stu-id="e6930-169">Next</span></span>](adding-controller.md)  
