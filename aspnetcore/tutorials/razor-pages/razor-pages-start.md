---
title: ASP.NET Core Razor sayfaları kullanmaya başlama
author: rick-anderson
description: Bu öğretici serisinde, ASP.NET Core Razor sayfaları kullanma işlemi gösterilmektedir. Model oluşturma, Razor sayfaları için kod oluşturmak, veri erişimi için Entity Framework Core ve SQL Server kullanmak, arama işlevi eklemek, giriş doğrulaması eklemek ve modeli güncelleştirmek için geçişleri kullanma hakkında bilgi edinin.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: ca5b134aaa0a9218bc3b0466c3448bd41e8cef8b
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450742"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="e6bca-104">Öğretici: ASP.NET Core Razor sayfaları kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="e6bca-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="e6bca-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e6bca-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="e6bca-106">Bu öğreticide ASP.NET Core 2.1 sürümünü izleyin öneririz.</span><span class="sxs-lookup"><span data-stu-id="e6bca-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="e6bca-107">Sahip **çok** izleyin daha kolay ve daha fazla özellikleri kapsar.</span><span class="sxs-lookup"><span data-stu-id="e6bca-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="e6bca-108">Seçin **ASP.NET Core 2.1** sürüm Seçici içinde.</span><span class="sxs-lookup"><span data-stu-id="e6bca-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Sürüm Seçici İçindekiler](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="e6bca-110">Bu öğreticide bir ASP.NET Core Razor sayfaları web uygulaması oluşturmaya ilişkin temel bilgileri size öğretir.</span><span class="sxs-lookup"><span data-stu-id="e6bca-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="e6bca-111">Razor sayfaları, ASP.NET Core web uygulamaları için kullanıcı arabirimini derlemek için önerilen yoludur.</span><span class="sxs-lookup"><span data-stu-id="e6bca-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="e6bca-112">Bu öğretici üç sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="e6bca-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="e6bca-113">Windows: Bu öğretici</span><span class="sxs-lookup"><span data-stu-id="e6bca-113">Windows: This tutorial</span></span>
* <span data-ttu-id="e6bca-114">MacOS: [Mac için Visual Studio ile Razor sayfaları ile başlama](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="e6bca-114">macOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="e6bca-115">macOS, Linux ve Windows: [Visual Studio code'da ASP.NET Core Razor sayfaları kullanmaya başlama](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="e6bca-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="e6bca-116">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e6bca-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

> [!NOTE]
> <span data-ttu-id="e6bca-117">ASP.NET Core içindekiler tablosuna yönelik önerilmiş olan yeni bir yapının kullanılabilirliğini test ediyoruz.</span><span class="sxs-lookup"><span data-stu-id="e6bca-117">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="e6bca-118">Geçerli veya önerilen içindekiler tablosunda 7 farklı konuyu bulmaya ilişkin alıştırmayı denemek için vaktiniz varsa lütfen [çalışmaya katılmak için buraya tıklayın](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span><span class="sxs-lookup"><span data-stu-id="e6bca-118">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="e6bca-119">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="e6bca-119">Prerequisites</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="e6bca-120">Razor web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="e6bca-120">Create a Razor web app</span></span>

* <span data-ttu-id="e6bca-121">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="e6bca-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e6bca-122">Yeni bir ASP.NET Core Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e6bca-122">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="e6bca-123">Projeyi adlandırın **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="e6bca-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="e6bca-124">Projeyi adlandırın önemlidir *RazorPagesMovie* ad alanları, kopyala/yapıştır kod olduğunda eşleşecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="e6bca-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="e6bca-125">![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="e6bca-125">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="e6bca-126">Seçin **ASP.NET Core 2.1** açılır ve ardından **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="e6bca-126">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![Yeni ASP.NET Core Web uygulaması](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="e6bca-128">Visual Studio şablonu bir başlangıç projesi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="e6bca-128">The Visual Studio template creates a starter project:</span></span>

![Çözüm Gezgini](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="e6bca-130">Tuşuna **F5** uygulamayı hata ayıklama modunda çalıştırmak için veya **Ctrl-F5** Haya ayıklayıcı eklemeden çalıştırılacak.</span><span class="sxs-lookup"><span data-stu-id="e6bca-130">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="e6bca-131">Seçin **kabul** izleme için onay verme.</span><span class="sxs-lookup"><span data-stu-id="e6bca-131">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="e6bca-132">Bu uygulama, kişisel bilgi izlemez.</span><span class="sxs-lookup"><span data-stu-id="e6bca-132">This app doesn't track personal information.</span></span> <span data-ttu-id="e6bca-133">Oluşturulan şablon kodunun karşılamanıza yardımcı olmak üzere varlıkları içeren [genel veri koruma yönetmeliği (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="e6bca-133">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Giriş ya da dizin sayfası](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="e6bca-135">Aşağıdaki görüntüde, izleme kabul ettikten sonra uygulama gösterilir:</span><span class="sxs-lookup"><span data-stu-id="e6bca-135">The following image shows the app after accepting tracking:</span></span>

![Giriş ya da dizin sayfası](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="e6bca-137">Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="e6bca-137">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="e6bca-138">Adres çubuğu gösterir `localhost:port#` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="e6bca-138">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e6bca-139">Çünkü `localhost` standart, yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="e6bca-139">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e6bca-140">Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="e6bca-140">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="e6bca-141">Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e6bca-141">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="e6bca-142">Yukarıdaki görüntüde, 5001 bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="e6bca-142">In the preceding image, the port number is 5001.</span></span> <span data-ttu-id="e6bca-143">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="e6bca-143">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="e6bca-144">Uygulamayı başlatma **Ctrl + F5** (hata ayıklama olmayan mod), kod değişiklikleri yapabilir, dosyayı kaydetmek, tarayıcıyı yenileyin ve kod değişikliklerini görebilirsiniz olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="e6bca-144">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="e6bca-145">Geliştiricilerin çoğu, hızlı bir şekilde uygulamayı başlatın ve değişiklikleri görmek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="e6bca-145">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

[!INCLUDE [F7](~/includes/RP/F7.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="e6bca-146">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="e6bca-146">Prerequisites</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="e6bca-147">Razor web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="e6bca-147">Create a Razor web app</span></span>

* <span data-ttu-id="e6bca-148">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="e6bca-148">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e6bca-149">Yeni bir ASP.NET Core Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e6bca-149">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="e6bca-150">Projeyi adlandırın **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="e6bca-150">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="e6bca-151">Projeyi adlandırın önemlidir *RazorPagesMovie* ad alanları, kopyala/yapıştır kod olduğunda eşleşecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="e6bca-151">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="e6bca-152">![Yeni ASP.NET Core Web uygulaması](../../razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="e6bca-152">![new ASP.NET Core Web Application](../../razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="e6bca-153">Seçin **ASP.NET Core 2.0** açılır ve ardından **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="e6bca-153">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="e6bca-154">Visual Studio şablonu bir başlangıç projesi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="e6bca-154">The Visual Studio template creates a starter project:</span></span>

![Çözüm Gezgini](razor-pages-start/_static/se.png)

<span data-ttu-id="e6bca-156">Tuşuna **F5** uygulamayı hata ayıklama modunda çalıştırmak için veya **Ctrl-F5** Haya ayıklayıcı eklemeden çalıştırmak için</span><span class="sxs-lookup"><span data-stu-id="e6bca-156">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Giriş ya da dizin sayfası](razor-pages-start/_static/home.png)

* <span data-ttu-id="e6bca-158">Visual Studio başlatır [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamanızı çalışır.</span><span class="sxs-lookup"><span data-stu-id="e6bca-158">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="e6bca-159">Adres çubuğu gösterir `localhost:port#` gibi bir şey `example.com`.</span><span class="sxs-lookup"><span data-stu-id="e6bca-159">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e6bca-160">Çünkü `localhost` standart, yerel bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="e6bca-160">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e6bca-161">Localhost yalnızca yerel bilgisayara gelen web isteklerini işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="e6bca-161">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="e6bca-162">Visual Studio, bir web projesi oluşturduğunda, web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e6bca-162">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="e6bca-163">Yukarıdaki görüntüde, bağlantı noktası numarasını 5000'dir.</span><span class="sxs-lookup"><span data-stu-id="e6bca-163">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="e6bca-164">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="e6bca-164">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="e6bca-165">Uygulamayı başlatma **Ctrl + F5** (hata ayıklama olmayan mod), kod değişiklikleri yapabilir, dosyayı kaydetmek, tarayıcıyı yenileyin ve kod değişikliklerini görebilirsiniz olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="e6bca-165">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="e6bca-166">Geliştiricilerin çoğu, hızlı bir şekilde uygulamayı başlatın ve değişiklikleri görmek için hata ayıklama olmayan modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="e6bca-166">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="e6bca-167">Sonraki: model ekleme</span><span class="sxs-lookup"><span data-stu-id="e6bca-167">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
