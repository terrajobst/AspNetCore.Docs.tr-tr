---
uid: visual-studio/overview/2013/using-browser-link
title: "Visual Studio 2013'te tarayıcı bağlantısı kullanarak | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/04/2013
ms.topic: article
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: e5a13405a303580ec8c1d4cdacafc26c6f8ff34a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="using-browser-link-in-visual-studio-2013"></a><span data-ttu-id="8268a-102">Visual Studio 2013'te tarayıcı bağlantısı kullanma</span><span class="sxs-lookup"><span data-stu-id="8268a-102">Using Browser Link in Visual Studio 2013</span></span>
====================
<span data-ttu-id="8268a-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8268a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="8268a-104">Tarayıcı bağlantısı, geliştirme ortamı ve bir veya daha fazla web tarayıcıları arasında bir iletişim kanalı oluşturur Visual Studio 2013'te yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="8268a-104">Browser Link is a new feature in Visual Studio 2013 that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="8268a-105">Tarayıcılar arası test etmek için faydalı olan bazı tarayıcılar, web uygulamanızda aynı anda yenilemek için tarayıcı bağlantısı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8268a-105">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

- [<span data-ttu-id="8268a-106">Tarayıcı Yenile</span><span class="sxs-lookup"><span data-stu-id="8268a-106">Browser Refresh</span></span>](#browser-refresh)
- [<span data-ttu-id="8268a-107">Tarayıcı bağlantısı panoyu görüntüleme</span><span class="sxs-lookup"><span data-stu-id="8268a-107">Viewing the Browser Link Dashboard</span></span>](#dashboard)
- [<span data-ttu-id="8268a-108">Tarayıcı bağlantısı statik HTML dosyaları için etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="8268a-108">Enabling Browser Link for Static HTML Files</span></span>](#static-html)
- [<span data-ttu-id="8268a-109">Disabling Browser Link</span><span class="sxs-lookup"><span data-stu-id="8268a-109">Disabling Browser Link</span></span>](#disabling)
- [<span data-ttu-id="8268a-110">Nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="8268a-110">How Does It Work?</span></span>](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a><span data-ttu-id="8268a-111">Tarayıcı Yenile</span><span class="sxs-lookup"><span data-stu-id="8268a-111">Browser Refresh</span></span>

<span data-ttu-id="8268a-112">Tarayıcıyı yenilemek ile Visual Studio tarayıcı bağlantısı üzerinden bağlanan birden çok tarayıcı yenileyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8268a-112">With Browser Refresh, you can refresh multiple browsers that are connected to Visual Studio through Browser Link.</span></span>

<span data-ttu-id="8268a-113">Tarayıcıyı yenilemek kullanmak için ilk proje şablonları birini kullanarak bir ASP.NET uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8268a-113">To use Browser Refresh, first create an ASP.NET application, using any of the project templates.</span></span> <span data-ttu-id="8268a-114">Uygulama hata ayıklama F5 tuşuna basarak veya araç çubuğunda ok simgesini tıklatarak:</span><span class="sxs-lookup"><span data-stu-id="8268a-114">Debug the application by pressing F5 or clicking the arrow icon in the toolbar:</span></span>

![](using-browser-link/_static/image1.png)

<span data-ttu-id="8268a-115">Hata ayıklama için belirli bir tarayıcı seçmek için açılır de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8268a-115">You can also use the dropdown to select a specific browser for debugging.</span></span>

![](using-browser-link/_static/image2.png)

<span data-ttu-id="8268a-116">Birden çok tarayıcı ile hata ayıklamak için seçin **Gözat ile**.</span><span class="sxs-lookup"><span data-stu-id="8268a-116">To debug with multiple browsers, select **Browse With**.</span></span> <span data-ttu-id="8268a-117">İçinde **Gözat ile** iletişim kutusunda, birden fazla tarayıcı seçmek için CTRL tuşunu basılı tutun.</span><span class="sxs-lookup"><span data-stu-id="8268a-117">In the **Browse With** dialog, hold down the CTRL key to select more than one browser.</span></span> <span data-ttu-id="8268a-118">Tıklatın **Gözat** seçili tarayıcılarla hata ayıklamak için.</span><span class="sxs-lookup"><span data-stu-id="8268a-118">Click **Browse** to debug with the selected browsers.</span></span> <span data-ttu-id="8268a-119">Dış Visual Studio'dan bir tarayıcıyı başlatın ve uygulamayı URL'ye gidin tarayıcı bağlantısı de çalışır.</span><span class="sxs-lookup"><span data-stu-id="8268a-119">Browser Link also works if you launch a browser from outside Visual Studio and navigate to the application URL.</span></span>

![](using-browser-link/_static/image3.png)

<span data-ttu-id="8268a-120">Tarayıcı bağlantısı denetimleri döngüsel oku simgesiyle açılır bulunur.</span><span class="sxs-lookup"><span data-stu-id="8268a-120">The Browser Link controls are located in the dropdown with the circular arrow icon.</span></span> <span data-ttu-id="8268a-121">Ok simgesi **yenileme** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="8268a-121">The arrow icon is the **Refresh** button.</span></span>

![](using-browser-link/_static/image4.png)

<span data-ttu-id="8268a-122">Hangi tarayıcılar bağlı olduğunu görmek için fare üzerine gelerek **yenileme** hata ayıklama sırasında düğmesi.</span><span class="sxs-lookup"><span data-stu-id="8268a-122">To see which browsers are connected, hover the mouse over the **Refresh** button while debugging.</span></span> <span data-ttu-id="8268a-123">Bağlı tarayıcıları bir araç ipucu penceresinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8268a-123">The connected browsers are shown in a ToolTip window.</span></span>

![](using-browser-link/_static/image5.png)

<span data-ttu-id="8268a-124">Bağlı tarayıcı yenilemek için tıklatın **yenileme** düğmesini tıklatın veya CTRL + ALT + ENTER tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="8268a-124">To refresh the connected browsers, click the **Refresh** button or press CTRL+ALT+ENTER.</span></span> <span data-ttu-id="8268a-125">Örneğin, aşağıdaki ekran görüntüsünde ı MVC 5 proje şablonu kullanılarak oluşturulan bir ASP.NET projesi gösterir.</span><span class="sxs-lookup"><span data-stu-id="8268a-125">For example, the following screenshot shows an ASP.NET project, which I created using the MVC 5 project template.</span></span> <span data-ttu-id="8268a-126">Üst iki tarayıcılarında çalışan uygulama görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8268a-126">You can see the application running in two browsers at the top.</span></span> <span data-ttu-id="8268a-127">Alt kısmındaki projeyi Visual Studio'da açın.</span><span class="sxs-lookup"><span data-stu-id="8268a-127">At the bottom, the project is open in Visual Studio.</span></span>

![](using-browser-link/_static/image6.png)

<span data-ttu-id="8268a-128">Visual Studio'da değiştirdim &lt;h1&gt; başlık için giriş sayfası:</span><span class="sxs-lookup"><span data-stu-id="8268a-128">In Visual Studio, I changed the &lt;h1&gt; heading for the home page:</span></span>

![](using-browser-link/_static/image7.png)

<span data-ttu-id="8268a-129">I tıklandığında **yenileme** düğmesi, her iki tarayıcı pencerelerini değişikliği görünen:</span><span class="sxs-lookup"><span data-stu-id="8268a-129">When I clicked the **Refresh** button, the change appeared in both browser windows:</span></span>

![](using-browser-link/_static/image8.png)

<span data-ttu-id="8268a-130">**Notlar**</span><span class="sxs-lookup"><span data-stu-id="8268a-130">**Notes**</span></span>

- <span data-ttu-id="8268a-131">Tarayıcı bağlantısını etkinleştirmek için ayarlanmış `debug=true` içinde [ &lt;derleme&gt; ](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) projenin Web.config dosyasında öğesi.</span><span class="sxs-lookup"><span data-stu-id="8268a-131">To enable Browser Link, set `debug=true` in the [&lt;compilation&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) element in the project's Web.config file.</span></span>
- <span data-ttu-id="8268a-132">Uygulama localhost üzerinde çalışmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8268a-132">The application must be running on localhost.</span></span>
- <span data-ttu-id="8268a-133">Uygulama, .NET 4.0 veya üzeri hedeflemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="8268a-133">The application must target .NET 4.0 or later.</span></span>

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a><span data-ttu-id="8268a-134">Tarayıcı bağlantısı panoyu görüntüleme</span><span class="sxs-lookup"><span data-stu-id="8268a-134">Viewing the Browser Link Dashboard</span></span>

<span data-ttu-id="8268a-135">Tarayıcı bağlantısı Panosu tarayıcı bağlantısı bağlantılar hakkında bilgi gösterir.</span><span class="sxs-lookup"><span data-stu-id="8268a-135">The Browser Link dashboard shows information about the Browser Link connections.</span></span> <span data-ttu-id="8268a-136">Panoyu görüntülemek için tarayıcı bağlantısı açılır menüsünü seçin (yanında küçük oka **yenileme** düğmesi).</span><span class="sxs-lookup"><span data-stu-id="8268a-136">To view the dashboard, select the Browser Link dropdown menu (the small arrow next to the **Refresh** button).</span></span> <span data-ttu-id="8268a-137">Ardından **tarayıcı bağlantı Pano**.</span><span class="sxs-lookup"><span data-stu-id="8268a-137">Then click **Browser Link Dashboard**.</span></span>

![](using-browser-link/_static/image9.png)

<span data-ttu-id="8268a-138">Pano bağlı tarayıcılar ve her tarayıcı gittiğinizde URL listeler.</span><span class="sxs-lookup"><span data-stu-id="8268a-138">The dashboard lists the connected Browsers and the URL to which each browser has navigated.</span></span>

![](using-browser-link/_static/image10.png)

<span data-ttu-id="8268a-139">**Önkoşullar** bölümünde bu proje için tarayıcı bağlantısını etkinleştirmek için gereken adımlar gösterilir.</span><span class="sxs-lookup"><span data-stu-id="8268a-139">The **Prerequisites** section shows any steps needed to enable Browser Link for that project.</span></span> <span data-ttu-id="8268a-140">Örneğin, aşağıdaki ekran görüntüsünde, "hata ayıklama" false olarak Web.config dosyasında ayarlandığı bir proje gösterir.</span><span class="sxs-lookup"><span data-stu-id="8268a-140">For example, the following screenshot shows a project where "debug" is set to false in the Web.config file.</span></span>

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a><span data-ttu-id="8268a-141">Tarayıcı bağlantısı statik HTML dosyaları için etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="8268a-141">Enabling Browser Link for Static HTML Files</span></span>

<span data-ttu-id="8268a-142">Statik HTML dosyaları için tarayıcı bağlantısını etkinleştirmek için aşağıdaki Web.config dosyanıza ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8268a-142">To enable Browser Link for static HTML files, add the following to your Web.config file.</span></span>

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

<span data-ttu-id="8268a-143">Projenizi yayımladığınızda, performansı artırmak için bu ayarı kaldırın.</span><span class="sxs-lookup"><span data-stu-id="8268a-143">For performance reasons, remove this setting when you publish your project.</span></span>

<a id="disabling"></a>
## <a name="disabling-browser-link"></a><span data-ttu-id="8268a-144">Tarayıcı bağlantısı devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="8268a-144">Disabling Browser Link</span></span>

<span data-ttu-id="8268a-145">Tarayıcı bağlantısı varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="8268a-145">Browser Link is enabled by default.</span></span> <span data-ttu-id="8268a-146">Devre dışı bırakmak birkaç yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="8268a-146">There are several ways to disable it:</span></span>

- <span data-ttu-id="8268a-147">Tarayıcı bağlantısı açılır menüde işaretini **tarayıcı bağlantısını etkinleştirmek**.</span><span class="sxs-lookup"><span data-stu-id="8268a-147">In the Browser Link dropdown menu, uncheck **Enable Browser Link**.</span></span> 

    ![](using-browser-link/_static/image12.png)
- <span data-ttu-id="8268a-148">Web.config dosyasında "vs: EnableBrowserLink" "false" değeri ile appSettings bölümünde adlı bir anahtar ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8268a-148">In the Web.config file, add a key named "vs:EnableBrowserLink" with the value "false" in the appSettings section.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- <span data-ttu-id="8268a-149">Web.config dosyasında hata ayıklama false olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="8268a-149">In the Web.config file, set debug to false.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a><span data-ttu-id="8268a-150">Nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="8268a-150">How Does It Work?</span></span>

<span data-ttu-id="8268a-151">Tarayıcı bağlantısını kullanan [SignalR](../../../signalr/index.md) Visual Studio ve tarayıcı arasındaki iletişim kanalını oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="8268a-151">Browser Link uses [SignalR](../../../signalr/index.md) to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="8268a-152">Tarayıcı bağlantısı etkin olduğunda, Visual Studio'nun birden çok istemci (tarayıcı) bağlanabileceği bir SignalR sunucusu olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="8268a-152">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="8268a-153">Tarayıcı bağlantısı, bir HTTP modülü ASP.NET ile de kaydeder.</span><span class="sxs-lookup"><span data-stu-id="8268a-153">Browser Link also registers an HTTP module with ASP.NET.</span></span> <span data-ttu-id="8268a-154">Bu modül özel yerleştirir &lt;betik&gt; sunucudan her sayfa isteği içine başvuruları.</span><span class="sxs-lookup"><span data-stu-id="8268a-154">This module injects special &lt;script&gt; references into every page request from the server.</span></span> <span data-ttu-id="8268a-155">Komut dosyası başvuruları tarayıcıda "Kaynağı görüntüle" seçerek görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8268a-155">You can see the script references by selecting "View source" in the browser.</span></span>

![](using-browser-link/_static/image13.png)

<span data-ttu-id="8268a-156">Kaynak dosyalarınız değiştirilmez.</span><span class="sxs-lookup"><span data-stu-id="8268a-156">Your source files are not modified.</span></span> <span data-ttu-id="8268a-157">HTTP modülü, komut dosyası başvuruları dinamik olarak yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="8268a-157">The HTTP module injects the script references dynamically.</span></span>

<span data-ttu-id="8268a-158">Gözatıcı kod tüm JavaScript olduğundan, tüm tarayıcılarla çalışır, [SignalR destekleyen](../../../signalr/overview/getting-started/supported-platforms.md), tüm tarayıcı eklentisi gerektirmeden.</span><span class="sxs-lookup"><span data-stu-id="8268a-158">Because the browser-side code is all JavaScript, it works on all browsers that [SignalR supports](../../../signalr/overview/getting-started/supported-platforms.md), without requiring any browser plug-in.</span></span>
