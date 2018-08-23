---
title: ASP.NET core'da tarayıcı bağlantısı
author: ncarandini
description: Tarayıcı bağlantısı bir veya daha fazla web tarayıcıları geliştirme ortamıyla bağlanan bir Visual Studio özelliği ne olduğunu açıklar.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
uid: client-side/using-browserlink
ms.openlocfilehash: 452ba5149563c186750466f471c7b950f0017614
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754264"
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="6f707-103">ASP.NET core'da tarayıcı bağlantısı</span><span class="sxs-lookup"><span data-stu-id="6f707-103">Browser Link in ASP.NET Core</span></span>

<span data-ttu-id="6f707-104">Tarafından [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), ve [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="6f707-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="6f707-105">Tarayıcı bağlantısı, Visual Studio'da geliştirme ortamı ve bir veya daha fazla web tarayıcıları arasında bir iletişim kanalı oluşturan bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="6f707-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="6f707-106">Çapraz tarayıcı test etmek için faydalı olan bazı tarayıcılarda, web uygulamanızın tek bir seferde yenilemek için tarayıcı bağlantısını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6f707-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="6f707-107">Tarayıcı bağlantısı Kurulum</span><span class="sxs-lookup"><span data-stu-id="6f707-107">Browser Link setup</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6f707-108">ASP.NET Core 2.1 ve geçiş için bir ASP.NET Core 2.0 proje dönüştürülürken [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), yükleme [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) için paket BrowserLink işlevselliği.</span><span class="sxs-lookup"><span data-stu-id="6f707-108">When converting an ASP.NET Core 2.0 project to ASP.NET Core 2.1 and transitioning to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), install the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package for BrowserLink functionality.</span></span> <span data-ttu-id="6f707-109">ASP.NET Core 2.1 proje şablonlarını kullanma `Microsoft.AspNetCore.App` metapackage varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="6f707-109">The ASP.NET Core 2.1 project templates use the `Microsoft.AspNetCore.App` metapackage by default.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6f707-110">ASP.NET Core 2.0 **Web uygulaması**, **boş**, ve **Web API** proje şablonları kullanın [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) , bir paket başvurusu için içeren [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span><span class="sxs-lookup"><span data-stu-id="6f707-110">The ASP.NET Core 2.0 **Web Application**, **Empty**, and **Web API** project templates use the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="6f707-111">Bu nedenle, kullanarak `Microsoft.AspNetCore.All` metapackage tarayıcı bağlantısı kullanmak için kullanılabilir hale getirmek için başka bir işlem gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6f707-111">Therefore, using the `Microsoft.AspNetCore.All` metapackage requires no further action to make Browser Link available for use.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="6f707-112">ASP.NET Core 1.x **Web uygulaması** proje şablonu için bir paket başvurusu var. [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) paket.</span><span class="sxs-lookup"><span data-stu-id="6f707-112">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="6f707-113">**Boş** veya **Web API** şablonu projeleri için bir paket başvurusu ekleme gerektirir `Microsoft.VisualStudio.Web.BrowserLink`.</span><span class="sxs-lookup"><span data-stu-id="6f707-113">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="6f707-114">Bu, bir Visual Studio özellik en kolay yolu pakete eklenecek olduğundan bir **boş** veya **Web API** şablon projedir açmak için **Paket Yöneticisi Konsolu** (**Görünümü** > **diğer Windows** > **Paket Yöneticisi Konsolu**) ve aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6f707-114">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="6f707-115">Alternatif olarak, **NuGet Paket Yöneticisi**.</span><span class="sxs-lookup"><span data-stu-id="6f707-115">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="6f707-116">İçinde proje adınıza sağ **Çözüm Gezgini** ve **NuGet paketlerini Yönet**:</span><span class="sxs-lookup"><span data-stu-id="6f707-116">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![Açık NuGet Paket Yöneticisi](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="6f707-118">Bul ve paket yükleyin:</span><span class="sxs-lookup"><span data-stu-id="6f707-118">Find and install the package:</span></span>

![Paket ile NuGet Paket Yöneticisi ekleme](using-browserlink/_static/add-package-with-nuget-package-manager.png)

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="6f707-120">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6f707-120">Configuration</span></span>

<span data-ttu-id="6f707-121">İçinde `Startup.Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="6f707-121">In the `Startup.Configure` method:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="6f707-122">Kodu içindedir genellikle bir `if` yalnızca tarayıcı bağlantısı burada gösterildiği gibi geliştirme ortamında sağlayan bloğu:</span><span class="sxs-lookup"><span data-stu-id="6f707-122">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="6f707-123">Daha fazla bilgi için [birden fazla ortam kullanayım](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="6f707-123">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="6f707-124">Tarayıcı bağlantısı kullanma</span><span class="sxs-lookup"><span data-stu-id="6f707-124">How to use Browser Link</span></span>

<span data-ttu-id="6f707-125">Bir ASP.NET Core projesi açık olduğunda, Visual Studio tarayıcı bağlantısı araç çubuğu denetimi yanındaki gösteren **hata ayıklama hedefi** araç çubuğu denetimi:</span><span class="sxs-lookup"><span data-stu-id="6f707-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Tarayıcı bağlantısı aşağı açılan menüsü](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="6f707-127">Tarayıcı bağlantısı araç denetiminden şunları yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="6f707-127">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="6f707-128">Web uygulamasını aynı anda birkaç tarayıcılarda yenileyin.</span><span class="sxs-lookup"><span data-stu-id="6f707-128">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="6f707-129">Açık **tarayıcı bağlantı Panosu**.</span><span class="sxs-lookup"><span data-stu-id="6f707-129">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="6f707-130">Etkinleştirmek veya devre dışı **tarayıcı bağlantısı**.</span><span class="sxs-lookup"><span data-stu-id="6f707-130">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="6f707-131">Not: Tarayıcı bağlantısı Visual Studio 2017 (15.3) varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="6f707-131">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="6f707-132">Etkinleştirmek veya devre dışı [CSS otomatik eşitleme](#enable-or-disable-css-auto-sync).</span><span class="sxs-lookup"><span data-stu-id="6f707-132">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="6f707-133">Bazı Visual Studio eklentileri, özellikle *Web uzantı paketi 2015* ve *Web uzantı paketi 2017*genişletilmiş işlevselliği için tarayıcı bağlantısı sunar, ancak bazı ek özellikleri ASP ile çalışmaz. NET Core projeleri.</span><span class="sxs-lookup"><span data-stu-id="6f707-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a><span data-ttu-id="6f707-134">Çeşitli tarayıcılarda web uygulamasını aynı anda Yenile</span><span class="sxs-lookup"><span data-stu-id="6f707-134">Refresh the web app in several browsers at once</span></span>

<span data-ttu-id="6f707-135">Proje başlatılırken başlatmak için tek bir web tarayıcısı seçmek için aşağı açılan menüden kullanın **hata ayıklama hedefi** araç çubuğu denetimi:</span><span class="sxs-lookup"><span data-stu-id="6f707-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![F5 açılan menüsü](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="6f707-137">Aynı anda birden çok tarayıcı açmak için seçin **birlikte Gözat...**  aynı açılır listeden.</span><span class="sxs-lookup"><span data-stu-id="6f707-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="6f707-138">İstediğiniz tarayıcıları seçmek için CTRL tuşunu basılı tutun ve ardından **Gözat**:</span><span class="sxs-lookup"><span data-stu-id="6f707-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Tek seferde birçok tarayıcıda aç](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="6f707-140">Visual Studio açık şekilde dizin görünümünün gösteren bir ekran ve iki açık tarayıcılar aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="6f707-140">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![İki tarayıcılar örnek ile eşitleme](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="6f707-142">Tarayıcı bağlantısı araç çubuğu denetimi projesine bağlı tarayıcıları görmek için üzerine:</span><span class="sxs-lookup"><span data-stu-id="6f707-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Vurgulu İpucu](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="6f707-144">Index görünümünü değiştirin ve tarayıcı bağlantısı yenile düğmesine tıkladığınızda, tüm bağlı tarayıcıların güncelleştirilir:</span><span class="sxs-lookup"><span data-stu-id="6f707-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![değişiklik tarayıcılar eşitleme](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="6f707-146">Tarayıcı bağlantısı da dış Visual Studio'dan başlatmak ve uygulama URL'sine gidin tarayıcılarla çalışır.</span><span class="sxs-lookup"><span data-stu-id="6f707-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="6f707-147">Tarayıcı bağlantı Panosu</span><span class="sxs-lookup"><span data-stu-id="6f707-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="6f707-148">Tarayıcı bağlantısı açılan menüden açık tarayıcı bağlantısı yönetmek için tarayıcı bağlantı Panosu açın:</span><span class="sxs-lookup"><span data-stu-id="6f707-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![Açık browserslink Panosu](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="6f707-150">Hiçbir tarayıcı bağlıysa, seçerek hata ayıklama olmayan bir oturum başlatabilirsiniz *tarayıcıda görüntüle* bağlantı:</span><span class="sxs-lookup"><span data-stu-id="6f707-150">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![Pano no bağlantıları browserlink](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="6f707-152">Aksi takdirde, bağlı tarayıcıların her tarayıcıda gösterildiğini sayfasının yolu ile gösterilir:</span><span class="sxs-lookup"><span data-stu-id="6f707-152">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink Panosu iki bağlantı](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="6f707-154">İsterseniz, tek bir tarayıcıyı yenilemek için listelenen tarayıcı adına tıklayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6f707-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="6f707-155">Etkinleştirmek veya devre dışı tarayıcı bağlantısı</span><span class="sxs-lookup"><span data-stu-id="6f707-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="6f707-156">Tarayıcı bağlantısı devre dışı bırakıldıktan sonra yeniden etkinleştirdiğinizde, bunları yeniden bağlanmayı tarayıcılar yenilemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="6f707-156">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="6f707-157">CSS otomatik eşitleme devre dışı bırakma veya etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="6f707-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="6f707-158">CSS otomatik eşitleme etkinleştirildiğinde, CSS dosyaları için herhangi bir değişiklik yaptığınızda bağlı tarayıcıları otomatik olarak yenilenir.</span><span class="sxs-lookup"><span data-stu-id="6f707-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="6f707-159">Nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="6f707-159">How it works</span></span>

<span data-ttu-id="6f707-160">Tarayıcı bağlantısı, Visual Studio ile tarayıcı arasında bir iletişim kanalı oluşturmak için SignalR kullanır.</span><span class="sxs-lookup"><span data-stu-id="6f707-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="6f707-161">Tarayıcı bağlantısı etkin olduğunda, Visual Studio için birden çok istemci (tarayıcı) bağlanabilen bir SignalR sunucusu olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="6f707-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="6f707-162">Tarayıcı bağlantısı, ASP.NET Core istek işlem hattı, bir ara yazılım bileşeni de kaydeder.</span><span class="sxs-lookup"><span data-stu-id="6f707-162">Browser Link also registers a middleware component in the ASP.NET Core request pipeline.</span></span> <span data-ttu-id="6f707-163">Bu bileşen özel eklediği `<script>` sunucudan her sayfa isteği halinde başvuruları.</span><span class="sxs-lookup"><span data-stu-id="6f707-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="6f707-164">Komut dosyası başvuruları seçerek gördüğünüz **kaynağı görüntüle** tarayıcı ve sonuna kadar kaydırma `<body>` etiketi içeriği:</span><span class="sxs-lookup"><span data-stu-id="6f707-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="6f707-165">Kaynak dosyalarını değiştiren değildir.</span><span class="sxs-lookup"><span data-stu-id="6f707-165">Your source files aren't modified.</span></span> <span data-ttu-id="6f707-166">Ara yazılım bileşeni dinamik olarak komut dosyası başvuruları ekler.</span><span class="sxs-lookup"><span data-stu-id="6f707-166">The middleware component injects the script references dynamically.</span></span>

<span data-ttu-id="6f707-167">Tarayıcı tarafı kod, tüm JavaScript olduğundan, bir tarayıcı eklentisini gerek kalmadan SignalR destekleyen tüm tarayıcılarda çalışır.</span><span class="sxs-lookup"><span data-stu-id="6f707-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
