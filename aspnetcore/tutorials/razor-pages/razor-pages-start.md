---
title: ASP.NET Core Razor sayfalarında kullanmaya başlama
author: rick-anderson
description: Bir ASP.NET Core Razor sayfalarının web uygulaması oluşturmanın temel bilgileri bulur. Razor sayfalarının ASP.NET Core web iş yükleri için önerilir.
manager: wpickett
ms.author: riande
ms.date: 5/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: d7cdf7c8fac3b2ac1e526c6eeee8205068964ec9
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34582823"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="86672-104">ASP.NET Core Razor sayfalarında kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="86672-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="86672-105">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="86672-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="86672-106">Bu öğretici ASP.NET Core 2.1 sürümüne izleyin öneririz.</span><span class="sxs-lookup"><span data-stu-id="86672-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="86672-107">Bunun **çok** izleyin daha kolay ve daha fazla özellikleri kapsar.</span><span class="sxs-lookup"><span data-stu-id="86672-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="86672-108">Seçin **ASP.NET Core 2.1** sürüm Seçici içinde.</span><span class="sxs-lookup"><span data-stu-id="86672-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Sürüm Seçici TOC içinde](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="86672-110">Bu öğretici, bir ASP.NET Core Razor sayfalarının web uygulaması oluşturmanın temel öğretir.</span><span class="sxs-lookup"><span data-stu-id="86672-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="86672-111">Razor sayfalarının ASP.NET Core web uygulamaları için kullanıcı Arabirimi oluşturmak için önerilen yöntem olduğu.</span><span class="sxs-lookup"><span data-stu-id="86672-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="86672-112">Bu öğretici için üç sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="86672-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="86672-113">Windows: Bu öğretici</span><span class="sxs-lookup"><span data-stu-id="86672-113">Windows: This tutorial</span></span>
* <span data-ttu-id="86672-114">MacOS: [Razor sayfalarının Visual Studio ile Mac için kullanmaya başlama](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="86672-114">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="86672-115">macOS, Linux ve Windows: [ASP.NET Core Razor sayfalarında, Visual Studio Code ile çalışmaya başlama](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="86672-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="86672-116">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="86672-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="86672-117">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="86672-117">Prerequisites</span></span>

<span data-ttu-id="86672-118">[! [] (~/İncludes/net-core-prereqs-windows.md) içerir [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="86672-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="86672-119">Bir Razor web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="86672-119">Create a Razor web app</span></span>

* <span data-ttu-id="86672-120">Visual Studio'dan **dosya** menüsünde, select **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="86672-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="86672-121">Yeni bir ASP.NET çekirdek Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="86672-121">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="86672-122">Proje adı **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="86672-122">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="86672-123">Proje adı önemlidir *RazorPagesMovie* ad alanları, kopyala/yapıştır kod zaman eşleşecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="86672-123">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="86672-124">![Yeni ASP.NET çekirdek Web uygulaması](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="86672-124">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="86672-125">Seçin **ASP.NET Core 2.1** açılır ve ardından **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="86672-125">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![Yeni ASP.NET çekirdek Web uygulaması](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="86672-127">Visual Studio şablon bir başlangıç projesi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="86672-127">The Visual Studio template creates a starter project:</span></span>

![Çözüm Gezgini](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="86672-129">Tuşuna **F5** uygulamayı hata ayıklama modunda çalıştırmak için veya **Ctrl-F5** hata ayıklayıcı eklemeden çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="86672-129">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="86672-130">Seçin **kabul** İzleme'onayı için.</span><span class="sxs-lookup"><span data-stu-id="86672-130">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="86672-131">Bu uygulamayı, kişisel bilgi izlemez.</span><span class="sxs-lookup"><span data-stu-id="86672-131">This app doesn't track personal information.</span></span> <span data-ttu-id="86672-132">Oluşturulan şablon kodunun karşılamak amacıyla varlıklar içeren [genel veri koruma düzenleme (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="86672-132">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Giriş veya dizin sayfası](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="86672-134">Aşağıdaki resimde, izleme kabul ettikten sonra uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="86672-134">The following image shows the app after accepting tracking:</span></span>

![Giriş veya dizin sayfası](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="86672-136">Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="86672-136">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="86672-137">Adres çubuğunu gösterir `localhost:port#` bir şey yok gibi ve `example.com`.</span><span class="sxs-lookup"><span data-stu-id="86672-137">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="86672-138">Çünkü `localhost` , yerel bilgisayarınızın standart barındırıcı adıdır.</span><span class="sxs-lookup"><span data-stu-id="86672-138">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="86672-139">Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="86672-139">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="86672-140">Visual Studio web projesini oluşturduğunda, rastgele bir bağlantı noktası web sunucusu için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="86672-140">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="86672-141">Önceki görüntüde 5000 bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="86672-141">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="86672-142">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="86672-142">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="86672-143">Uygulama başlatma **Ctrl + F5** (olmayan hata ayıklama modu), kod değişiklikleri yapabilir, dosyayı kaydedin, tarayıcıyı yenilemek ve kod değişiklikleri görmek olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="86672-143">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="86672-144">Çoğu geliştirici, hızlı bir şekilde uygulamayı başlatın ve değişiklikleri görmek için olmayan hata ayıklama modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="86672-144">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="86672-145">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="86672-145">Prerequisites</span></span>

<span data-ttu-id="86672-146">[! [] (~/İncludes/net-core-prereqs-windows.md) içerir [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="86672-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="86672-147">Bir Razor web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="86672-147">Create a Razor web app</span></span>

* <span data-ttu-id="86672-148">Visual Studio'dan **dosya** menüsünde, select **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="86672-148">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="86672-149">Yeni bir ASP.NET çekirdek Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="86672-149">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="86672-150">Proje adı **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="86672-150">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="86672-151">Proje adı önemlidir *RazorPagesMovie* ad alanları, kopyala/yapıştır kod zaman eşleşecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="86672-151">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="86672-152">![Yeni ASP.NET çekirdek Web uygulaması](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="86672-152">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="86672-153">Seçin **ASP.NET Core 2.0** açılır ve ardından **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="86672-153">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="86672-154">Visual Studio şablon bir başlangıç projesi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="86672-154">The Visual Studio template creates a starter project:</span></span>

![Çözüm Gezgini](razor-pages-start/_static/se.png)

<span data-ttu-id="86672-156">Tuşuna **F5** uygulamayı hata ayıklama modunda çalıştırmak için veya **Ctrl-F5** hata ayıklayıcı eklemeden çalıştırmak için</span><span class="sxs-lookup"><span data-stu-id="86672-156">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Giriş veya dizin sayfası](razor-pages-start/_static/home.png)

* <span data-ttu-id="86672-158">Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamanızı çalışır.</span><span class="sxs-lookup"><span data-stu-id="86672-158">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="86672-159">Adres çubuğunu gösterir `localhost:port#` bir şey yok gibi ve `example.com`.</span><span class="sxs-lookup"><span data-stu-id="86672-159">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="86672-160">Çünkü `localhost` , yerel bilgisayarınızın standart barındırıcı adıdır.</span><span class="sxs-lookup"><span data-stu-id="86672-160">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="86672-161">Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="86672-161">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="86672-162">Visual Studio web projesini oluşturduğunda, rastgele bir bağlantı noktası web sunucusu için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="86672-162">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="86672-163">Önceki görüntüde 5000 bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="86672-163">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="86672-164">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="86672-164">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="86672-165">Uygulama başlatma **Ctrl + F5** (olmayan hata ayıklama modu), kod değişiklikleri yapabilir, dosyayı kaydedin, tarayıcıyı yenilemek ve kod değişiklikleri görmek olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="86672-165">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="86672-166">Çoğu geliştirici, hızlı bir şekilde uygulamayı başlatın ve değişiklikleri görmek için olmayan hata ayıklama modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="86672-166">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="86672-167">Sonraki: bir modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="86672-167">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
