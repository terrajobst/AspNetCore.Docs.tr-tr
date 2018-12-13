---
title: ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama
author: rick-anderson
description: ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama hakkında bilgi edinin.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: dfc9a864cf10db5fc44d5631dcbb2f73325d9e14
ms.sourcegitcommit: a16352c1c88a71770ab3922200a8cd148fb278a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53335331"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="90638-103">ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="90638-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="90638-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="90638-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="90638-105">Bu öğreticinin 3 sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="90638-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="90638-106">macOS: [Mac için Visual Studio ile bir ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="90638-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="90638-107">Windows: [Visual Studio ile bir ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="90638-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="90638-108">macOS, Linux ve Windows için: [Visual Studio kodu ile bir ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="90638-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="90638-109">Visual Studio ve .NET Core yükleyin</span><span class="sxs-lookup"><span data-stu-id="90638-109">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="90638-110">Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="90638-110">Create a web app</span></span>

<span data-ttu-id="90638-111">Visual Studio'dan seçin **Dosya > Yeni > Proje**.</span><span class="sxs-lookup"><span data-stu-id="90638-111">From Visual Studio, select  **File > New > Project**.</span></span>

![Dosya > Yeni > Proje](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="90638-113">Tamamlamak **yeni proje** iletişim:</span><span class="sxs-lookup"><span data-stu-id="90638-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="90638-114">Sol bölmede, dokunun **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="90638-114">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="90638-115">Orta bölmede dokunun **ASP.NET Core Web uygulaması (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="90638-115">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="90638-116">(Kod kopyaladığınızda, ad alanı eşleşecek şekilde "MvcMovie" proje adı önemlidir.) "MvcMovie" proje adı</span><span class="sxs-lookup"><span data-stu-id="90638-116">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="90638-117">Dokunun **Tamam**</span><span class="sxs-lookup"><span data-stu-id="90638-117">Tap **OK**</span></span>

![<span data-ttu-id="90638-118">Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web'de .net core</span><span class="sxs-lookup"><span data-stu-id="90638-118">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="90638-119">Tamamlamak **yeni ASP.NET Core Web uygulaması (.NET Core) - MvcMovie** iletişim:</span><span class="sxs-lookup"><span data-stu-id="90638-119">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="90638-120">Sürüm Seçici açılan kutusunda seçin **ASP.NET Core 2.1**</span><span class="sxs-lookup"><span data-stu-id="90638-120">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="90638-121">Seçin **Web uygulaması (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="90638-121">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="90638-122">Dokunun **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="90638-122">Tap **OK**.</span></span>

![<span data-ttu-id="90638-123">Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web'de .net core</span><span class="sxs-lookup"><span data-stu-id="90638-123">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="90638-124">Visual Studio, yeni oluşturduğunuz MVC projesi için varsayılan bir şablon kullanılır.</span><span class="sxs-lookup"><span data-stu-id="90638-124">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="90638-125">Çalışan bir uygulamayı şu anda bir proje adı girerek ve bazı Seçenekler'i seçerek sizde.</span><span class="sxs-lookup"><span data-stu-id="90638-125">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="90638-126">Bu temel başlangıç projesini ve başlatmak için iyi bir yerdir.</span><span class="sxs-lookup"><span data-stu-id="90638-126">This is a basic starter project, and it's a good place to start.</span></span>

<span data-ttu-id="90638-127">Dokunun **F5** uygulamayı hata ayıklama modunda çalıştırmak için veya **Ctrl-F5** olmayan hata ayıklama modunda.</span><span class="sxs-lookup"><span data-stu-id="90638-127">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="90638-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Uygulamayı çalıştırma](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="90638-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="90638-129">Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamanızı çalışır.</span><span class="sxs-lookup"><span data-stu-id="90638-129">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="90638-130">Adres çubuğuna gösteren uyarı `localhost:port#` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="90638-130">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="90638-131">Çünkü `localhost` standart, yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="90638-131">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="90638-132">Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="90638-132">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="90638-133">Yukarıdaki görüntüde, bağlantı noktası numarasını 5000'dir.</span><span class="sxs-lookup"><span data-stu-id="90638-133">In the image above, the port number is 5000.</span></span> <span data-ttu-id="90638-134">Tarayıcı gösterir URL'de `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="90638-134">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="90638-135">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="90638-135">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="90638-136">Uygulamayı başlatma **Ctrl + F5** (hata ayıklama olmayan mod), kod değişiklikleri yapabilir, dosyayı kaydetmek, tarayıcıyı yenileyin ve kod değişikliklerini görebilirsiniz olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="90638-136">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="90638-137">Geliştiricilerin çoğu, hızlı bir şekilde uygulamayı başlatın ve değişiklikleri görmek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="90638-137">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="90638-138">Uygulamada hata ayıklama veya hata ayıklama olmayan moddan başlatabilirsiniz **hata ayıklama** menü öğesi:</span><span class="sxs-lookup"><span data-stu-id="90638-138">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menü hata ayıklama](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="90638-140">Dokunarak uygulamayı hata ayıklaması yapabilirsiniz **IIS Express** düğmesi</span><span class="sxs-lookup"><span data-stu-id="90638-140">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="90638-142">Varsayılan şablonu size çalışma **hakkında giriş** ve **kişi** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="90638-142">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="90638-143">Yukarıdaki tarayıcı resimde, bu bağlantıları göstermez.</span><span class="sxs-lookup"><span data-stu-id="90638-143">The browser image above doesn't show these links.</span></span> <span data-ttu-id="90638-144">Tarayıcınız boyutuna bağlı olarak, gösterileceğinin Gezinti simgesine tıklamanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="90638-144">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![sağ üstteki gezinti simgesi](start-mvc/_static/2.png)

<span data-ttu-id="90638-146">Hata ayıklama modunda çalışıyormuş dokunun **Shift + F5** hata ayıklamayı durdurmak için.</span><span class="sxs-lookup"><span data-stu-id="90638-146">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="90638-147">Bu öğreticinin sonraki bölümünde, biz MVC konusunda bilgi ve biraz kod yazmaya.</span><span class="sxs-lookup"><span data-stu-id="90638-147">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="90638-148">Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="90638-148">Create a web app</span></span>

<span data-ttu-id="90638-149">Visual Studio'dan seçin **Dosya > Yeni > Proje**.</span><span class="sxs-lookup"><span data-stu-id="90638-149">From Visual Studio, select  **File > New > Project**.</span></span>

![Dosya > Yeni > Proje](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="90638-151">Tamamlamak **yeni proje** iletişim:</span><span class="sxs-lookup"><span data-stu-id="90638-151">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="90638-152">Sol bölmede, dokunun **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="90638-152">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="90638-153">Orta bölmede dokunun **ASP.NET Core Web uygulaması (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="90638-153">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="90638-154">(Kod kopyaladığınızda, ad alanı eşleşecek şekilde "MvcMovie" proje adı önemlidir.) "MvcMovie" proje adı</span><span class="sxs-lookup"><span data-stu-id="90638-154">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="90638-155">Dokunun **Tamam**</span><span class="sxs-lookup"><span data-stu-id="90638-155">Tap **OK**</span></span>

![<span data-ttu-id="90638-156">Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web'de .net core</span><span class="sxs-lookup"><span data-stu-id="90638-156">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

<span data-ttu-id="90638-157">Tamamlamak **yeni ASP.NET Core Web uygulaması (.NET Core) - MvcMovie** iletişim:</span><span class="sxs-lookup"><span data-stu-id="90638-157">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="90638-158">Sürüm Seçici açılan kutusunda seçin **ASP.NET Core 2.-**</span><span class="sxs-lookup"><span data-stu-id="90638-158">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="90638-159">Seçin **Application(Model-View-Controller) Web**</span><span class="sxs-lookup"><span data-stu-id="90638-159">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="90638-160">Dokunun **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="90638-160">Tap **OK**.</span></span>

![<span data-ttu-id="90638-161">Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web'de .net core</span><span class="sxs-lookup"><span data-stu-id="90638-161">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

<span data-ttu-id="90638-162">Visual Studio, yeni oluşturduğunuz MVC projesi için varsayılan bir şablon kullanılır.</span><span class="sxs-lookup"><span data-stu-id="90638-162">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="90638-163">Çalışan bir uygulamayı şu anda bir proje adı girerek ve bazı Seçenekler'i seçerek sizde.</span><span class="sxs-lookup"><span data-stu-id="90638-163">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="90638-164">Bu temel başlangıç projesini ve başlatmak için iyi bir yerdir,</span><span class="sxs-lookup"><span data-stu-id="90638-164">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="90638-165">Dokunun **F5** uygulamayı hata ayıklama modunda çalıştırmak için veya **Ctrl-F5** olmayan hata ayıklama modunda.</span><span class="sxs-lookup"><span data-stu-id="90638-165">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="90638-166"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Uygulamayı çalıştırma](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="90638-166"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="90638-167">Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamanızı çalışır.</span><span class="sxs-lookup"><span data-stu-id="90638-167">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="90638-168">Adres çubuğuna gösteren uyarı `localhost:port#` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="90638-168">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="90638-169">Çünkü `localhost` standart, yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="90638-169">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="90638-170">Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="90638-170">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="90638-171">Yukarıdaki görüntüde, bağlantı noktası numarasını 5000'dir.</span><span class="sxs-lookup"><span data-stu-id="90638-171">In the image above, the port number is 5000.</span></span> <span data-ttu-id="90638-172">Tarayıcı gösterir URL'de `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="90638-172">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="90638-173">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="90638-173">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="90638-174">Uygulamayı başlatma **Ctrl + F5** (hata ayıklama olmayan mod), kod değişiklikleri yapabilir, dosyayı kaydetmek, tarayıcıyı yenileyin ve kod değişikliklerini görebilirsiniz olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="90638-174">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="90638-175">Geliştiricilerin çoğu, hızlı bir şekilde uygulamayı başlatın ve değişiklikleri görmek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="90638-175">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="90638-176">Uygulamada hata ayıklama veya hata ayıklama olmayan moddan başlatabilirsiniz **hata ayıklama** menü öğesi:</span><span class="sxs-lookup"><span data-stu-id="90638-176">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menü hata ayıklama](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="90638-178">Dokunarak uygulamayı hata ayıklaması yapabilirsiniz **IIS Express** düğmesi</span><span class="sxs-lookup"><span data-stu-id="90638-178">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="90638-180">Varsayılan şablonu size çalışma **hakkında giriş** ve **kişi** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="90638-180">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="90638-181">Yukarıdaki tarayıcı resimde, bu bağlantıları göstermez.</span><span class="sxs-lookup"><span data-stu-id="90638-181">The browser image above doesn't show these links.</span></span> <span data-ttu-id="90638-182">Tarayıcınız boyutuna bağlı olarak, gösterileceğinin Gezinti simgesine tıklamanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="90638-182">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![sağ üstteki gezinti simgesi](start-mvc/_static/2.png)

<span data-ttu-id="90638-184">Hata ayıklama modunda çalışıyormuş dokunun **Shift + F5** hata ayıklamayı durdurmak için.</span><span class="sxs-lookup"><span data-stu-id="90638-184">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="90638-185">Bu öğreticinin sonraki bölümünde, biz MVC konusunda bilgi ve biraz kod yazmaya.</span><span class="sxs-lookup"><span data-stu-id="90638-185">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="90638-186">Next</span><span class="sxs-lookup"><span data-stu-id="90638-186">Next</span></span>](adding-controller.md)  
