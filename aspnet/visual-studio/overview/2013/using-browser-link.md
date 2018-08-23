---
uid: visual-studio/overview/2013/using-browser-link
title: Visual Studio 2013'te tarayıcı bağlantısı kullanma | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: f470aa7e425d16aec3f67d2a0ebb664a3e7eac41
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755115"
---
<a name="using-browser-link-in-visual-studio-2013"></a><span data-ttu-id="0c172-102">Visual Studio 2013'te tarayıcı bağlantısı kullanma</span><span class="sxs-lookup"><span data-stu-id="0c172-102">Using Browser Link in Visual Studio 2013</span></span>
====================
<span data-ttu-id="0c172-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0c172-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="0c172-104">Tarayıcı bağlantısı, geliştirme ortamı ve bir veya daha fazla web tarayıcıları arasında bir iletişim kanalı oluşturan Visual Studio 2013'te yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="0c172-104">Browser Link is a new feature in Visual Studio 2013 that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="0c172-105">Çapraz tarayıcı test etmek için faydalı olan bazı tarayıcılarda, web uygulamanızın tek bir seferde yenilemek için tarayıcı bağlantısını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0c172-105">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

- [<span data-ttu-id="0c172-106">Tarayıcı Yenile</span><span class="sxs-lookup"><span data-stu-id="0c172-106">Browser Refresh</span></span>](#browser-refresh)
- [<span data-ttu-id="0c172-107">Tarayıcı bağlantı Panosu görüntüleme</span><span class="sxs-lookup"><span data-stu-id="0c172-107">Viewing the Browser Link Dashboard</span></span>](#dashboard)
- [<span data-ttu-id="0c172-108">Statik HTML dosyaları için tarayıcı bağlantısını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="0c172-108">Enabling Browser Link for Static HTML Files</span></span>](#static-html)
- [<span data-ttu-id="0c172-109">Tarayıcı bağlantısı devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="0c172-109">Disabling Browser Link</span></span>](#disabling)
- [<span data-ttu-id="0c172-110">Nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="0c172-110">How Does It Work?</span></span>](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a><span data-ttu-id="0c172-111">Tarayıcı Yenile</span><span class="sxs-lookup"><span data-stu-id="0c172-111">Browser Refresh</span></span>

<span data-ttu-id="0c172-112">Tarayıcıyı yenileyin ile Visual Studio tarayıcı bağlantısı aracılığıyla bağlı birden çok tarayıcı yenileyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0c172-112">With Browser Refresh, you can refresh multiple browsers that are connected to Visual Studio through Browser Link.</span></span>

<span data-ttu-id="0c172-113">Tarayıcıyı yenileyin kullanmak için ilk proje şablonlarını kullanarak bir ASP.NET uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0c172-113">To use Browser Refresh, first create an ASP.NET application, using any of the project templates.</span></span> <span data-ttu-id="0c172-114">Uygulama, F5 tuşuna basarak veya araç çubuğunda ok simgesine tıklayarak hata ayıklama:</span><span class="sxs-lookup"><span data-stu-id="0c172-114">Debug the application by pressing F5 or clicking the arrow icon in the toolbar:</span></span>

![](using-browser-link/_static/image1.png)

<span data-ttu-id="0c172-115">Hata ayıklama için belirli bir tarayıcı seçmek için açılan listeyi de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0c172-115">You can also use the dropdown to select a specific browser for debugging.</span></span>

![](using-browser-link/_static/image2.png)

<span data-ttu-id="0c172-116">Birden çok tarayıcı ile hata ayıklamak için seçin **şununla Gözat**.</span><span class="sxs-lookup"><span data-stu-id="0c172-116">To debug with multiple browsers, select **Browse With**.</span></span> <span data-ttu-id="0c172-117">İçinde **şununla Gözat** iletişim kutusunda birden fazla tarayıcı seçmek için CTRL tuşunu basılı tutun.</span><span class="sxs-lookup"><span data-stu-id="0c172-117">In the **Browse With** dialog, hold down the CTRL key to select more than one browser.</span></span> <span data-ttu-id="0c172-118">Tıklayın **Gözat** seçili tarayıcıları ile hata ayıklamak için.</span><span class="sxs-lookup"><span data-stu-id="0c172-118">Click **Browse** to debug with the selected browsers.</span></span> <span data-ttu-id="0c172-119">Visual Studio dışında bir tarayıcıdan başlatın ve uygulama URL'sine gidin, tarayıcı bağlantısı da çalışır.</span><span class="sxs-lookup"><span data-stu-id="0c172-119">Browser Link also works if you launch a browser from outside Visual Studio and navigate to the application URL.</span></span>

![](using-browser-link/_static/image3.png)

<span data-ttu-id="0c172-120">Tarayıcı bağlantısı denetimleri yuvarlak ok simgesi açılan listesi bulunur.</span><span class="sxs-lookup"><span data-stu-id="0c172-120">The Browser Link controls are located in the dropdown with the circular arrow icon.</span></span> <span data-ttu-id="0c172-121">Ok simgesi **Yenile** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="0c172-121">The arrow icon is the **Refresh** button.</span></span>

![](using-browser-link/_static/image4.png)

<span data-ttu-id="0c172-122">Hangi tarayıcılar bağlı olduğunu görmek için fareyi üzerine **Yenile** hata ayıklama sırasında düğmesi.</span><span class="sxs-lookup"><span data-stu-id="0c172-122">To see which browsers are connected, hover the mouse over the **Refresh** button while debugging.</span></span> <span data-ttu-id="0c172-123">Bağlı tarayıcıların bir araç ipucu penceresi gösterilir.</span><span class="sxs-lookup"><span data-stu-id="0c172-123">The connected browsers are shown in a ToolTip window.</span></span>

![](using-browser-link/_static/image5.png)

<span data-ttu-id="0c172-124">Bağlı tarayıcıyı yenilemek için şuna tıklayın **Yenile** düğmesine veya CTRL + ALT + ENTER tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="0c172-124">To refresh the connected browsers, click the **Refresh** button or press CTRL+ALT+ENTER.</span></span> <span data-ttu-id="0c172-125">Örneğin, aşağıdaki ekran görüntüsünde MVC 5 proje şablonunu kullanarak oluşturduğum bir ASP.NET projesi gösterir.</span><span class="sxs-lookup"><span data-stu-id="0c172-125">For example, the following screenshot shows an ASP.NET project, which I created using the MVC 5 project template.</span></span> <span data-ttu-id="0c172-126">Üstteki iki tarayıcılarında çalışan uygulama görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0c172-126">You can see the application running in two browsers at the top.</span></span> <span data-ttu-id="0c172-127">Altındaki Proje Visual Studio'da açın.</span><span class="sxs-lookup"><span data-stu-id="0c172-127">At the bottom, the project is open in Visual Studio.</span></span>

![](using-browser-link/_static/image6.png)

<span data-ttu-id="0c172-128">Visual Studio'da değiştirdim &lt;h1&gt; giriş sayfası başlığı:</span><span class="sxs-lookup"><span data-stu-id="0c172-128">In Visual Studio, I changed the &lt;h1&gt; heading for the home page:</span></span>

![](using-browser-link/_static/image7.png)

<span data-ttu-id="0c172-129">Ne zaman tıkladım **Yenile** düğmesi, değişikliği hem de Tarayıcı pencerelerinde görüntülenmektedir:</span><span class="sxs-lookup"><span data-stu-id="0c172-129">When I clicked the **Refresh** button, the change appeared in both browser windows:</span></span>

![](using-browser-link/_static/image8.png)

<span data-ttu-id="0c172-130">**Notlar**</span><span class="sxs-lookup"><span data-stu-id="0c172-130">**Notes**</span></span>

- <span data-ttu-id="0c172-131">Tarayıcı bağlantısını etkinleştirmek için ayarlanmış `debug=true` içinde [ &lt;derleme&gt; ](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) projenin Web.config dosyasında öğe.</span><span class="sxs-lookup"><span data-stu-id="0c172-131">To enable Browser Link, set `debug=true` in the [&lt;compilation&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) element in the project's Web.config file.</span></span>
- <span data-ttu-id="0c172-132">Uygulamayı, komut localhost üzerinde çalıştırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="0c172-132">The application must be running on localhost.</span></span>
- <span data-ttu-id="0c172-133">Uygulama, .NET 4.0 veya üzerini hedeflemelidir.</span><span class="sxs-lookup"><span data-stu-id="0c172-133">The application must target .NET 4.0 or later.</span></span>

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a><span data-ttu-id="0c172-134">Tarayıcı bağlantı Panosu görüntüleme</span><span class="sxs-lookup"><span data-stu-id="0c172-134">Viewing the Browser Link Dashboard</span></span>

<span data-ttu-id="0c172-135">Tarayıcı bağlantı Panosu, tarayıcı bağlantısı bağlantıları hakkındaki bilgileri gösterir.</span><span class="sxs-lookup"><span data-stu-id="0c172-135">The Browser Link dashboard shows information about the Browser Link connections.</span></span> <span data-ttu-id="0c172-136">Panoyu görüntülemek için tarayıcı bağlantısı açılan menüyü seçin (yanındaki küçük ok **Yenile** düğmesi).</span><span class="sxs-lookup"><span data-stu-id="0c172-136">To view the dashboard, select the Browser Link dropdown menu (the small arrow next to the **Refresh** button).</span></span> <span data-ttu-id="0c172-137">Ardından **tarayıcı bağlantı Panosu**.</span><span class="sxs-lookup"><span data-stu-id="0c172-137">Then click **Browser Link Dashboard**.</span></span>

![](using-browser-link/_static/image9.png)

<span data-ttu-id="0c172-138">Pano, bağlı tarayıcıların ve istediğiniz her tarayıcıda gezinen URL listeler.</span><span class="sxs-lookup"><span data-stu-id="0c172-138">The dashboard lists the connected Browsers and the URL to which each browser has navigated.</span></span>

![](using-browser-link/_static/image10.png)

<span data-ttu-id="0c172-139">**Önkoşulları** bölümünde o proje için tarayıcı bağlantısını etkinleştirmek için gerekli tüm adımları gösterilir.</span><span class="sxs-lookup"><span data-stu-id="0c172-139">The **Prerequisites** section shows any steps needed to enable Browser Link for that project.</span></span> <span data-ttu-id="0c172-140">Örneğin, aşağıdaki ekran görüntüsünde, "debug" false olarak Web.config dosyasında ayarlandığı bir proje gösterir.</span><span class="sxs-lookup"><span data-stu-id="0c172-140">For example, the following screenshot shows a project where "debug" is set to false in the Web.config file.</span></span>

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a><span data-ttu-id="0c172-141">Statik HTML dosyaları için tarayıcı bağlantısını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="0c172-141">Enabling Browser Link for Static HTML Files</span></span>

<span data-ttu-id="0c172-142">Statik HTML dosyaları için tarayıcı bağlantısını etkinleştirmek için Web.config dosyasına aşağıdakileri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0c172-142">To enable Browser Link for static HTML files, add the following to your Web.config file.</span></span>

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

<span data-ttu-id="0c172-143">Projenizi yayımladığınızda performansla ilgili nedenlerden dolayı bu ayarı kaldırın.</span><span class="sxs-lookup"><span data-stu-id="0c172-143">For performance reasons, remove this setting when you publish your project.</span></span>

<a id="disabling"></a>
## <a name="disabling-browser-link"></a><span data-ttu-id="0c172-144">Tarayıcı bağlantısı devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="0c172-144">Disabling Browser Link</span></span>

<span data-ttu-id="0c172-145">Tarayıcı bağlantısı varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="0c172-145">Browser Link is enabled by default.</span></span> <span data-ttu-id="0c172-146">Devre dışı bırakmak için birkaç yol vardır:</span><span class="sxs-lookup"><span data-stu-id="0c172-146">There are several ways to disable it:</span></span>

- <span data-ttu-id="0c172-147">Tarayıcı bağlantısı açılan menüde'seçeneğinin işaretini kaldırın **tarayıcı bağlantısını etkinleştir**.</span><span class="sxs-lookup"><span data-stu-id="0c172-147">In the Browser Link dropdown menu, uncheck **Enable Browser Link**.</span></span> 

    ![](using-browser-link/_static/image12.png)
- <span data-ttu-id="0c172-148">Web.config dosyasında "vs: EnableBrowserLink" değer "false" ile appSettings bölümündeki adlı bir anahtar ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0c172-148">In the Web.config file, add a key named "vs:EnableBrowserLink" with the value "false" in the appSettings section.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- <span data-ttu-id="0c172-149">Web.config dosyasında hata ayıklama false olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="0c172-149">In the Web.config file, set debug to false.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a><span data-ttu-id="0c172-150">Nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="0c172-150">How Does It Work?</span></span>

<span data-ttu-id="0c172-151">Tarayıcı bağlantısı kullanan [SignalR](../../../signalr/index.md) Visual Studio ile tarayıcı arasında bir iletişim kanalı oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="0c172-151">Browser Link uses [SignalR](../../../signalr/index.md) to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="0c172-152">Tarayıcı bağlantısı etkin olduğunda, Visual Studio için birden çok istemci (tarayıcı) bağlanabilen bir SignalR sunucusu olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="0c172-152">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="0c172-153">Tarayıcı bağlantısı, ASP.NET ile bir HTTP modülü de kaydeder.</span><span class="sxs-lookup"><span data-stu-id="0c172-153">Browser Link also registers an HTTP module with ASP.NET.</span></span> <span data-ttu-id="0c172-154">Bu modül özel eklediği &lt;betik&gt; sunucudan her sayfa isteği halinde başvuruları.</span><span class="sxs-lookup"><span data-stu-id="0c172-154">This module injects special &lt;script&gt; references into every page request from the server.</span></span> <span data-ttu-id="0c172-155">Komut dosyası başvuruları, tarayıcıda "Kaynağı görüntüle" seçeneğini belirleyerek görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0c172-155">You can see the script references by selecting "View source" in the browser.</span></span>

![](using-browser-link/_static/image13.png)

<span data-ttu-id="0c172-156">Kaynak dosyalarını değiştirilmez.</span><span class="sxs-lookup"><span data-stu-id="0c172-156">Your source files are not modified.</span></span> <span data-ttu-id="0c172-157">HTTP modülü, komut dosyası başvuruları dinamik olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="0c172-157">The HTTP module injects the script references dynamically.</span></span>

<span data-ttu-id="0c172-158">Tarayıcı tarafı kod, tüm JavaScript olduğundan, tüm tarayıcılarla çalışır, [SignalR destekler](../../../signalr/overview/getting-started/supported-platforms.md), herhangi bir tarayıcı eklentisini gerektirmeden.</span><span class="sxs-lookup"><span data-stu-id="0c172-158">Because the browser-side code is all JavaScript, it works on all browsers that [SignalR supports](../../../signalr/overview/getting-started/supported-platforms.md), without requiring any browser plug-in.</span></span>
