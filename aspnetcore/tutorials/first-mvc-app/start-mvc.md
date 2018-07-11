---
title: ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama
author: rick-anderson
description: ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama hakkında bilgi edinin.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 1fb3947023843341403f4355c6ae1e61d7e4f6b1
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38217984"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="ee3c7-103">ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="ee3c7-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="ee3c7-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ee3c7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="ee3c7-105">Bu öğreticinin 3 sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="ee3c7-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="ee3c7-106">macOS: [Mac için Visual Studio ile ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="ee3c7-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="ee3c7-107">Windows: [Visual Studio ile bir ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="ee3c7-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="ee3c7-108">macOS, Linux ve Windows: [Visual Studio Code ile ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="ee3c7-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="ee3c7-109">Visual Studio ve .NET Core yükleyin</span><span class="sxs-lookup"><span data-stu-id="ee3c7-109">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ee3c7-110">[! [] (~/İncludes/net-core-prereqs-windows.md) içerir [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="ee3c7-110">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="ee3c7-111">Bir web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="ee3c7-111">Create a web app</span></span>

<span data-ttu-id="ee3c7-112">Visual Studio'dan seçin **Dosya > Yeni > Proje**.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-112">From Visual Studio, select  **File > New > Project**.</span></span>

![Dosya > Yeni > Proje](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="ee3c7-114">Tamamlamak **yeni proje** iletişim:</span><span class="sxs-lookup"><span data-stu-id="ee3c7-114">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="ee3c7-115">Sol bölmede, dokunun **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="ee3c7-115">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="ee3c7-116">Orta bölmede dokunun **ASP.NET Core Web uygulaması (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="ee3c7-116">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="ee3c7-117">(Kod kopyaladığınızda, ad alanı eşleşecek şekilde "MvcMovie" proje adı önemlidir.) "MvcMovie" proje adı</span><span class="sxs-lookup"><span data-stu-id="ee3c7-117">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="ee3c7-118">Dokunun **Tamam**</span><span class="sxs-lookup"><span data-stu-id="ee3c7-118">Tap **OK**</span></span>

![<span data-ttu-id="ee3c7-119">Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web'de .net core</span><span class="sxs-lookup"><span data-stu-id="ee3c7-119">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="ee3c7-120">Tamamlamak **yeni ASP.NET Core Web uygulaması (.NET Core) - MvcMovie** iletişim:</span><span class="sxs-lookup"><span data-stu-id="ee3c7-120">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="ee3c7-121">Sürüm Seçici açılan kutusunda seçin **ASP.NET Core 2.1**</span><span class="sxs-lookup"><span data-stu-id="ee3c7-121">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="ee3c7-122">Seçin **Application(Model-View-Controller) Web**</span><span class="sxs-lookup"><span data-stu-id="ee3c7-122">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="ee3c7-123">Dokunun **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-123">Tap **OK**.</span></span>

![<span data-ttu-id="ee3c7-124">Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web'de .net core</span><span class="sxs-lookup"><span data-stu-id="ee3c7-124">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="ee3c7-125">Visual Studio, yeni oluşturduğunuz MVC projesi için varsayılan bir şablon kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-125">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="ee3c7-126">Çalışan bir uygulamayı şu anda bir proje adı girerek ve bazı Seçenekler'i seçerek sizde.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-126">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="ee3c7-127">Bu temel başlangıç projesini ve başlatmak için iyi bir yerdir,</span><span class="sxs-lookup"><span data-stu-id="ee3c7-127">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="ee3c7-128">Dokunun **F5** uygulamayı hata ayıklama modunda çalıştırmak için veya **Ctrl-F5** olmayan hata ayıklama modunda.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-128">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="ee3c7-129"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Uygulamayı çalıştırma](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="ee3c7-129"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="ee3c7-130">Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamanızı çalışır.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-130">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="ee3c7-131">Adres çubuğuna gösteren uyarı `localhost:port#` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-131">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ee3c7-132">Çünkü `localhost` standart, yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-132">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ee3c7-133">Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-133">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="ee3c7-134">Yukarıdaki görüntüde, bağlantı noktası numarasını 5000'dir.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-134">In the image above, the port number is 5000.</span></span> <span data-ttu-id="ee3c7-135">Tarayıcı gösterir URL'de `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-135">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="ee3c7-136">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-136">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="ee3c7-137">Uygulamayı başlatma **Ctrl + F5** (hata ayıklama olmayan mod), kod değişiklikleri yapabilir, dosyayı kaydetmek, tarayıcıyı yenileyin ve kod değişikliklerini görebilirsiniz olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-137">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ee3c7-138">Geliştiricilerin çoğu, hızlı bir şekilde uygulamayı başlatın ve değişiklikleri görmek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-138">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="ee3c7-139">Uygulamada hata ayıklama veya hata ayıklama olmayan moddan başlatabilirsiniz **hata ayıklama** menü öğesi:</span><span class="sxs-lookup"><span data-stu-id="ee3c7-139">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menü hata ayıklama](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="ee3c7-141">Dokunarak uygulamayı hata ayıklaması yapabilirsiniz **IIS Express** düğmesi</span><span class="sxs-lookup"><span data-stu-id="ee3c7-141">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="ee3c7-143">Varsayılan şablonu size çalışma **hakkında giriş** ve **kişi** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-143">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="ee3c7-144">Yukarıdaki tarayıcı resimde, bu bağlantıları göstermez.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-144">The browser image above doesn't show these links.</span></span> <span data-ttu-id="ee3c7-145">Tarayıcınız boyutuna bağlı olarak, gösterileceğinin Gezinti simgesine tıklamanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-145">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![sağ üstteki gezinti simgesi](start-mvc/_static/2.png)

<span data-ttu-id="ee3c7-147">Hata ayıklama modunda çalışıyormuş dokunun **Shift + F5** hata ayıklamayı durdurmak için.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-147">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="ee3c7-148">Bu öğreticinin sonraki bölümünde, biz MVC konusunda bilgi ve biraz kod yazmaya.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-148">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ee3c7-149">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ee3c7-149">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="ee3c7-150">[! [] (~/İncludes/net-core-prereqs.md) içerir [](~/includes/net-core-prereqs.md)]</span><span class="sxs-lookup"><span data-stu-id="ee3c7-150">[!INCLUDE [](~/includes/net-core-prereqs.md) [](~/includes/net-core-prereqs.md)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ee3c7-151">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ee3c7-151">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="ee3c7-152">Visual Studio Community 2017'yi yükleyin.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-152">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="ee3c7-153">Topluluk indir'i seçin.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-153">Select the Community download.</span></span> <span data-ttu-id="ee3c7-154">Visual Studio 2017 varsa bu adımı atlayın.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-154">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="ee3c7-155">Visual Studio 2017 giriş sayfası yükleyicisi</span><span class="sxs-lookup"><span data-stu-id="ee3c7-155">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="ee3c7-156">Yükleyiciyi çalıştırın ve aşağıdaki iş yükleri seçin:</span><span class="sxs-lookup"><span data-stu-id="ee3c7-156">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="ee3c7-157">**ASP.NET ve web geliştirme** (altında **Web ve bulut**)</span><span class="sxs-lookup"><span data-stu-id="ee3c7-157">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="ee3c7-158">**.NET core çoklu platform geliştirme** (altında **diğer araç takımları**)</span><span class="sxs-lookup"><span data-stu-id="ee3c7-158">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET ve web geliştirme ** (altında ** Web ve bulut **)](start-mvc/_static/web_workload.png)

![* *.NET çekirdek arası arası platfrom geliştirme ** (altında ** diğer araç takımları **)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="ee3c7-161">Bir web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="ee3c7-161">Create a web app</span></span>

<span data-ttu-id="ee3c7-162">Visual Studio'dan seçin **Dosya > Yeni > Proje**.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-162">From Visual Studio, select  **File > New > Project**.</span></span>

![Dosya > Yeni > Proje](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="ee3c7-164">Tamamlamak **yeni proje** iletişim:</span><span class="sxs-lookup"><span data-stu-id="ee3c7-164">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="ee3c7-165">Sol bölmede, dokunun **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="ee3c7-165">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="ee3c7-166">Orta bölmede dokunun **ASP.NET Core Web uygulaması (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="ee3c7-166">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="ee3c7-167">(Kod kopyaladığınızda, ad alanı eşleşecek şekilde "MvcMovie" proje adı önemlidir.) "MvcMovie" proje adı</span><span class="sxs-lookup"><span data-stu-id="ee3c7-167">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="ee3c7-168">Dokunun **Tamam**</span><span class="sxs-lookup"><span data-stu-id="ee3c7-168">Tap **OK**</span></span>

![<span data-ttu-id="ee3c7-169">Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web'de .net core</span><span class="sxs-lookup"><span data-stu-id="ee3c7-169">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ee3c7-170">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ee3c7-170">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ee3c7-171">Tamamlamak **yeni ASP.NET Core Web uygulaması (.NET Core) - MvcMovie** iletişim:</span><span class="sxs-lookup"><span data-stu-id="ee3c7-171">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="ee3c7-172">Sürüm Seçici açılan kutusunda seçin **ASP.NET Core 2.-**</span><span class="sxs-lookup"><span data-stu-id="ee3c7-172">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="ee3c7-173">Seçin **Application(Model-View-Controller) Web**</span><span class="sxs-lookup"><span data-stu-id="ee3c7-173">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="ee3c7-174">Dokunun **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-174">Tap **OK**.</span></span>

![<span data-ttu-id="ee3c7-175">Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web'de .net core</span><span class="sxs-lookup"><span data-stu-id="ee3c7-175">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ee3c7-176">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ee3c7-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ee3c7-177">Tamamlamak **yeni ASP.NET Core Web uygulaması (.NET Core) - MvcMovie** iletişim:</span><span class="sxs-lookup"><span data-stu-id="ee3c7-177">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="ee3c7-178">Sürüm Seçici açılan kutusunda tap içindeki **ASP.NET Core 1.1**</span><span class="sxs-lookup"><span data-stu-id="ee3c7-178">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="ee3c7-179">Dokunun **Web uygulaması**</span><span class="sxs-lookup"><span data-stu-id="ee3c7-179">Tap **Web Application**</span></span>
* <span data-ttu-id="ee3c7-180">Varsayılan tutun **kimlik doğrulaması yok**</span><span class="sxs-lookup"><span data-stu-id="ee3c7-180">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="ee3c7-181">Dokunun **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-181">Tap **OK**.</span></span>

![Yeni ASP.NET Core web uygulaması](start-mvc/_static/p3.png)

---

<span data-ttu-id="ee3c7-183">Visual Studio, yeni oluşturduğunuz MVC projesi için varsayılan bir şablon kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-183">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="ee3c7-184">Çalışan bir uygulamayı şu anda bir proje adı girerek ve bazı Seçenekler'i seçerek sizde.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-184">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="ee3c7-185">Bu temel başlangıç projesini ve başlatmak için iyi bir yerdir,</span><span class="sxs-lookup"><span data-stu-id="ee3c7-185">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="ee3c7-186">Dokunun **F5** uygulamayı hata ayıklama modunda çalıştırmak için veya **Ctrl-F5** olmayan hata ayıklama modunda.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-186">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="ee3c7-187"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Uygulamayı çalıştırma](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="ee3c7-187"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="ee3c7-188">Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamanızı çalışır.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-188">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="ee3c7-189">Adres çubuğuna gösteren uyarı `localhost:port#` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-189">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ee3c7-190">Çünkü `localhost` standart, yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-190">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ee3c7-191">Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-191">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="ee3c7-192">Yukarıdaki görüntüde, bağlantı noktası numarasını 5000'dir.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-192">In the image above, the port number is 5000.</span></span> <span data-ttu-id="ee3c7-193">Tarayıcı gösterir URL'de `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-193">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="ee3c7-194">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-194">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="ee3c7-195">Uygulamayı başlatma **Ctrl + F5** (hata ayıklama olmayan mod), kod değişiklikleri yapabilir, dosyayı kaydetmek, tarayıcıyı yenileyin ve kod değişikliklerini görebilirsiniz olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-195">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ee3c7-196">Geliştiricilerin çoğu, hızlı bir şekilde uygulamayı başlatın ve değişiklikleri görmek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-196">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="ee3c7-197">Uygulamada hata ayıklama veya hata ayıklama olmayan moddan başlatabilirsiniz **hata ayıklama** menü öğesi:</span><span class="sxs-lookup"><span data-stu-id="ee3c7-197">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menü hata ayıklama](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="ee3c7-199">Dokunarak uygulamayı hata ayıklaması yapabilirsiniz **IIS Express** düğmesi</span><span class="sxs-lookup"><span data-stu-id="ee3c7-199">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="ee3c7-201">Varsayılan şablonu size çalışma **hakkında giriş** ve **kişi** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-201">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="ee3c7-202">Yukarıdaki tarayıcı resimde, bu bağlantıları göstermez.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-202">The browser image above doesn't show these links.</span></span> <span data-ttu-id="ee3c7-203">Tarayıcınız boyutuna bağlı olarak, gösterileceğinin Gezinti simgesine tıklamanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-203">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![sağ üstteki gezinti simgesi](start-mvc/_static/2.png)

<span data-ttu-id="ee3c7-205">Hata ayıklama modunda çalışıyormuş dokunun **Shift + F5** hata ayıklamayı durdurmak için.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-205">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="ee3c7-206">Bu öğreticinin sonraki bölümünde, biz MVC konusunda bilgi ve biraz kod yazmaya.</span><span class="sxs-lookup"><span data-stu-id="ee3c7-206">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end
> [!div class="step-by-step"]
> [<span data-ttu-id="ee3c7-207">Next</span><span class="sxs-lookup"><span data-stu-id="ee3c7-207">Next</span></span>](adding-controller.md)  
