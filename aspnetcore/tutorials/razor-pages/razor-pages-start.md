---
title: ASP.NET Core Razor sayfaları kullanmaya başlama
author: rick-anderson
description: Bir ASP.NET Core Razor sayfaları web uygulaması oluşturmaya ilişkin temel bilgileri öğrenin. Razor sayfaları, ASP.NET Core web iş yükleri için önerilir.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 7148f2d944bd1978b1a83278dfed9051f192e4dd
ms.sourcegitcommit: 08f1a9baa97060da5168840b332c9c0805b5f901
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37144943"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="cdfb7-104">ASP.NET Core Razor sayfaları kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="cdfb7-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="cdfb7-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cdfb7-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="cdfb7-106">Bu öğreticide ASP.NET Core 2.1 sürümünü izleyin öneririz.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="cdfb7-107">Sahip **çok** izleyin daha kolay ve daha fazla özellikleri kapsar.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="cdfb7-108">Seçin **ASP.NET Core 2.1** sürüm Seçici içinde.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Sürüm Seçici İçindekiler](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="cdfb7-110">Bu öğreticide bir ASP.NET Core Razor sayfaları web uygulaması oluşturmaya ilişkin temel bilgileri size öğretir.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="cdfb7-111">Razor sayfaları, ASP.NET Core web uygulamaları için kullanıcı arabirimini derlemek için önerilen yoludur.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="cdfb7-112">Bu öğretici üç sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="cdfb7-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="cdfb7-113">Windows: Bu öğretici</span><span class="sxs-lookup"><span data-stu-id="cdfb7-113">Windows: This tutorial</span></span>
* <span data-ttu-id="cdfb7-114">MacOS: [Mac için Visual Studio ile Razor sayfaları ile başlama](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="cdfb7-114">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="cdfb7-115">macOS, Linux ve Windows: [Visual Studio code'da ASP.NET Core Razor sayfaları kullanmaya başlama](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="cdfb7-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="cdfb7-116">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cdfb7-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="cdfb7-117">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="cdfb7-117">Prerequisites</span></span>

<span data-ttu-id="cdfb7-118">[! [] (~/İncludes/net-core-prereqs-windows.md) içerir [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="cdfb7-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="cdfb7-119">Razor web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="cdfb7-119">Create a Razor web app</span></span>

* <span data-ttu-id="cdfb7-120">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="cdfb7-121">Yeni bir ASP.NET Core Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-121">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="cdfb7-122">Projeyi adlandırın **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-122">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="cdfb7-123">Projeyi adlandırın önemlidir *RazorPagesMovie* ad alanları, kopyala/yapıştır kod olduğunda eşleşecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-123">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="cdfb7-124">![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="cdfb7-124">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="cdfb7-125">Seçin **ASP.NET Core 2.1** açılır ve ardından **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-125">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="cdfb7-127">Visual Studio şablonu bir başlangıç projesi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="cdfb7-127">The Visual Studio template creates a starter project:</span></span>

![Çözüm Gezgini](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="cdfb7-129">Tuşuna **F5** uygulamayı hata ayıklama modunda çalıştırmak için veya **Ctrl-F5** Haya ayıklayıcı eklemeden çalıştırılacak.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-129">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="cdfb7-130">Seçin **kabul** izleme için onay verme.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-130">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="cdfb7-131">Bu uygulama, kişisel bilgi izlemez.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-131">This app doesn't track personal information.</span></span> <span data-ttu-id="cdfb7-132">Oluşturulan şablon kodunun karşılamanıza yardımcı olmak üzere varlıkları içeren [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="cdfb7-132">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Giriş ya da dizin sayfası](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="cdfb7-134">Aşağıdaki görüntüde, izleme kabul ettikten sonra uygulama gösterilir:</span><span class="sxs-lookup"><span data-stu-id="cdfb7-134">The following image shows the app after accepting tracking:</span></span>

![Giriş ya da dizin sayfası](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="cdfb7-136">Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-136">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="cdfb7-137">Adres çubuğu gösterir `localhost:port#` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-137">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="cdfb7-138">Çünkü `localhost` standart, yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-138">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="cdfb7-139">Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-139">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="cdfb7-140">Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-140">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="cdfb7-141">Yukarıdaki görüntüde, bağlantı noktası numarasını 5000'dir.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-141">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="cdfb7-142">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-142">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="cdfb7-143">Uygulamayı başlatma **Ctrl + F5** (hata ayıklama olmayan mod), kod değişiklikleri yapabilir, dosyayı kaydetmek, tarayıcıyı yenileyin ve kod değişikliklerini görebilirsiniz olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-143">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="cdfb7-144">Geliştiricilerin çoğu, hızlı bir şekilde uygulamayı başlatın ve değişiklikleri görmek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-144">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="cdfb7-145">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="cdfb7-145">Prerequisites</span></span>

<span data-ttu-id="cdfb7-146">[! [] (~/İncludes/net-core-prereqs-windows.md) içerir [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="cdfb7-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="cdfb7-147">Razor web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="cdfb7-147">Create a Razor web app</span></span>

* <span data-ttu-id="cdfb7-148">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-148">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="cdfb7-149">Yeni bir ASP.NET Core Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-149">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="cdfb7-150">Projeyi adlandırın **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-150">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="cdfb7-151">Projeyi adlandırın önemlidir *RazorPagesMovie* ad alanları, kopyala/yapıştır kod olduğunda eşleşecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-151">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="cdfb7-152">![Yeni ASP.NET Core Web uygulaması](../../razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="cdfb7-152">![new ASP.NET Core Web Application](../../razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="cdfb7-153">Seçin **ASP.NET Core 2.0** açılır ve ardından **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-153">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="cdfb7-154">Visual Studio şablonu bir başlangıç projesi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="cdfb7-154">The Visual Studio template creates a starter project:</span></span>

![Çözüm Gezgini](razor-pages-start/_static/se.png)

<span data-ttu-id="cdfb7-156">Tuşuna **F5** uygulamayı hata ayıklama modunda çalıştırmak için veya **Ctrl-F5** Haya ayıklayıcı eklemeden çalıştırmak için</span><span class="sxs-lookup"><span data-stu-id="cdfb7-156">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Giriş ya da dizin sayfası](razor-pages-start/_static/home.png)

* <span data-ttu-id="cdfb7-158">Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamanızı çalışır.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-158">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="cdfb7-159">Adres çubuğu gösterir `localhost:port#` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-159">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="cdfb7-160">Çünkü `localhost` standart, yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-160">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="cdfb7-161">Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-161">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="cdfb7-162">Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-162">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="cdfb7-163">Yukarıdaki görüntüde, bağlantı noktası numarasını 5000'dir.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-163">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="cdfb7-164">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-164">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="cdfb7-165">Uygulamayı başlatma **Ctrl + F5** (hata ayıklama olmayan mod), kod değişiklikleri yapabilir, dosyayı kaydetmek, tarayıcıyı yenileyin ve kod değişikliklerini görebilirsiniz olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-165">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="cdfb7-166">Geliştiricilerin çoğu, hızlı bir şekilde uygulamayı başlatın ve değişiklikleri görmek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="cdfb7-166">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="cdfb7-167">Sonraki: model ekleme</span><span class="sxs-lookup"><span data-stu-id="cdfb7-167">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
