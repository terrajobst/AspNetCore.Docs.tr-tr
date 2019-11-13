---
title: ASP.NET Core tarayıcı bağlantısı
author: ncarandini
description: Tarayıcı bağlantısının, geliştirme ortamını bir veya daha fazla Web tarayıcısına bağlayan bir Visual Studio özelliği olduğunu açıklar.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/12/2019
no-loc:
- SignalR
uid: client-side/using-browserlink
ms.openlocfilehash: b21b698d49e72b559cd9cd3753c48a38c99db24d
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962790"
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="46753-103">ASP.NET Core tarayıcı bağlantısı</span><span class="sxs-lookup"><span data-stu-id="46753-103">Browser Link in ASP.NET Core</span></span>

<span data-ttu-id="46753-104">, [Nicolò Carandini](https://github.com/ncarandini), [Mike, son](https://github.com/MikeWasson)ve [Tom Dykstra](https://github.com/tdykstra) tarafından</span><span class="sxs-lookup"><span data-stu-id="46753-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="46753-105">Tarayıcı bağlantısı, Visual Studio 'da geliştirme ortamı ile bir veya daha fazla Web tarayıcısı arasında bir iletişim kanalı oluşturan bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="46753-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="46753-106">Tarayıcı bağlantısını, Web uygulamanızı birden çok tarayıcıda tek seferde yenilemek için kullanabilirsiniz. Bu, tarayıcılar arası testler için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="46753-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="46753-107">Tarayıcı bağlantısı kurulumu</span><span class="sxs-lookup"><span data-stu-id="46753-107">Browser Link setup</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="46753-108">ASP.NET Core 2,0 projesi ASP.NET Core 2,1 ' e dönüştürülürken ve [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)'e geçiş yaparken, browserlink Işlevselliği için [Microsoft. VisualStudio. Web. browserlink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) paketini yükledikten sonra.</span><span class="sxs-lookup"><span data-stu-id="46753-108">When converting an ASP.NET Core 2.0 project to ASP.NET Core 2.1 and transitioning to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), install the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package for BrowserLink functionality.</span></span> <span data-ttu-id="46753-109">ASP.NET Core 2,1 proje şablonları varsayılan olarak `Microsoft.AspNetCore.App` metapaketini kullanır.</span><span class="sxs-lookup"><span data-stu-id="46753-109">The ASP.NET Core 2.1 project templates use the `Microsoft.AspNetCore.App` metapackage by default.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="46753-110">ASP.NET Core 2,0 **Web uygulaması**, **Empty**ve **Web API** proje şablonları, [Microsoft. VisualStudio. Web. browserlink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)Için bir paket başvurusu içeren [Microsoft. aspnetcore. All meta paketini](xref:fundamentals/metapackage)kullanır.</span><span class="sxs-lookup"><span data-stu-id="46753-110">The ASP.NET Core 2.0 **Web Application**, **Empty**, and **Web API** project templates use the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="46753-111">Bu nedenle, `Microsoft.AspNetCore.All` metapackage 'in kullanılması, tarayıcı bağlantısının kullanılabilir olmasını sağlamak için başka bir eylem gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="46753-111">Therefore, using the `Microsoft.AspNetCore.All` metapackage requires no further action to make Browser Link available for use.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="46753-112">ASP.NET Core 1. x **Web uygulaması** proje şablonunda, [Microsoft. VisualStudio. Web. browserlink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) paketi için bir paket başvurusu vardır.</span><span class="sxs-lookup"><span data-stu-id="46753-112">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="46753-113">**Boş** veya **Web apı** şablonu projeleri `Microsoft.VisualStudio.Web.BrowserLink`için bir paket başvurusu eklemenizi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="46753-113">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="46753-114">Bu bir Visual Studio özelliği olduğundan, paketi **boş** veya **Web API** şablonu projesine eklemenin en kolay yolu, **Paket Yöneticisi konsolu 'nu** açmak ( **diğer Windows** > **Paket Yöneticisi konsolunu**>**görüntüleyin** ) ve şu komutu çalıştırmalıdır:</span><span class="sxs-lookup"><span data-stu-id="46753-114">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="46753-115">Alternatif olarak, **NuGet Paket Yöneticisi**' ni kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46753-115">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="46753-116">**Çözüm Gezgini** ' de proje adına sağ tıklayın ve **NuGet Paketlerini Yönet**' i seçin:</span><span class="sxs-lookup"><span data-stu-id="46753-116">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![NuGet Paket Yöneticisi 'Ni aç](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="46753-118">Paketi bulun ve yükledikten sonra:</span><span class="sxs-lookup"><span data-stu-id="46753-118">Find and install the package:</span></span>

![NuGet Paket Yöneticisi ile paket ekleme](using-browserlink/_static/add-package-with-nuget-package-manager.png)

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="46753-120">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="46753-120">Configuration</span></span>

<span data-ttu-id="46753-121">`Startup.Configure` yönteminde:</span><span class="sxs-lookup"><span data-stu-id="46753-121">In the `Startup.Configure` method:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="46753-122">Genellikle kod, burada gösterildiği gibi yalnızca geliştirme ortamında tarayıcı bağlantısını sağlayan bir `if` bloğunun içindedir:</span><span class="sxs-lookup"><span data-stu-id="46753-122">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="46753-123">Daha fazla bilgi için bkz. [birden çok ortam kullanma](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="46753-123">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="46753-124">Tarayıcı bağlantısı kullanma</span><span class="sxs-lookup"><span data-stu-id="46753-124">How to use Browser Link</span></span>

<span data-ttu-id="46753-125">Bir ASP.NET Core projesi açıkken, Visual Studio **hata ayıklama hedefi** araç çubuğu denetiminin yanında tarayıcı bağlantısı araç çubuğu denetimini gösterir:</span><span class="sxs-lookup"><span data-stu-id="46753-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Tarayıcı bağlantısı açılan menüsü](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="46753-127">Tarayıcı bağlantısı araç çubuğu denetiminden şunları yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="46753-127">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="46753-128">Web uygulamasını aynı anda birkaç tarayıcıda yenileyin.</span><span class="sxs-lookup"><span data-stu-id="46753-128">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="46753-129">**Tarayıcı bağlantısı panosunu**açın.</span><span class="sxs-lookup"><span data-stu-id="46753-129">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="46753-130">**Tarayıcı bağlantısını**etkinleştirin veya devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="46753-130">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="46753-131">Note: tarayıcı bağlantısı, Visual Studio 2017 (15,3) içinde varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="46753-131">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="46753-132">[CSS otomatik eşitlemesini](#enable-or-disable-css-auto-sync)etkinleştirin veya devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="46753-132">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="46753-133">Bazı Visual Studio eklentileri, en önemlisi *Web uzantısı paketi 2015* ve *web uzantısı paketi 2017*, tarayıcı bağlantısı için genişletilmiş işlevsellik sunar, ancak bazı ek özellikler ASP.NET Core projelerle çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="46753-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a><span data-ttu-id="46753-134">Aynı anda birkaç tarayıcıda Web uygulamasını yenileyin</span><span class="sxs-lookup"><span data-stu-id="46753-134">Refresh the web app in several browsers at once</span></span>

<span data-ttu-id="46753-135">Projeyi başlatırken başlatılacak tek bir Web tarayıcısı seçmek için, **Hata Ayıkla hedef** araç çubuğu denetimindeki açılan menüyü kullanın:</span><span class="sxs-lookup"><span data-stu-id="46753-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![F5 açılır menüsü](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="46753-137">Aynı anda birden çok tarayıcı açmak için, aynı açılan listeden **... öğesine gidin** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="46753-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="46753-138">İstediğiniz tarayıcıları seçmek için CTRL tuşunu basılı tutarak, ardından da **Araştır**' a tıklayın:</span><span class="sxs-lookup"><span data-stu-id="46753-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Birçok tarayıcıyı aynı anda aç](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="46753-140">Visual Studio 'Yu Açık Dizin görünümü ve iki açık tarayıcıyla gösteren bir ekran görüntüsü aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="46753-140">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![İki tarayıcıyla Eşitle örneği](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="46753-142">Projeye bağlı tarayıcıları görmek için tarayıcı bağlantısı araç çubuğu denetiminin üzerine gelin:</span><span class="sxs-lookup"><span data-stu-id="46753-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Üzerine gelme İpucu](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="46753-144">Dizin görünümünü değiştirin ve tarayıcı bağlantısı yenileme düğmesine tıkladığınızda tüm bağlı tarayıcılar güncelleştirilir:</span><span class="sxs-lookup"><span data-stu-id="46753-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![tarayıcılar-değişikliklere karşı eşitleme](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="46753-146">Tarayıcı bağlantısı, Visual Studio dışından başlamış ve uygulama URL 'sine gidebileceğiniz tarayıcılarla de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="46753-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="46753-147">Tarayıcı bağlantısı panosu</span><span class="sxs-lookup"><span data-stu-id="46753-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="46753-148">Açık tarayıcılarla bağlantıyı yönetmek için tarayıcı bağlantısı açılan menüsünden tarayıcı bağlantısı panosunu açın:</span><span class="sxs-lookup"><span data-stu-id="46753-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![Açık-browserslink-Pano](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="46753-150">Hiçbir tarayıcı bağlı değilse, *Tarayıcıda görüntüle* bağlantısını seçerek hata ayıklama olmayan bir oturum başlatabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="46753-150">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![browserlink-Pano-bağlantı yok](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="46753-152">Aksi halde, bağlı tarayıcılar her bir tarayıcının gösterdiği sayfanın yoluyla gösterilir:</span><span class="sxs-lookup"><span data-stu-id="46753-152">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-Pano-iki bağlantı](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="46753-154">İsterseniz, bu tek tarayıcıyı yenilemek için listelenen bir tarayıcı adına tıklayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46753-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="46753-155">Tarayıcı bağlantısını etkinleştir veya devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="46753-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="46753-156">Tarayıcı bağlantısını devre dışı bıraktıktan sonra yeniden etkinleştirdiğinizde, bunları yeniden bağlamak için tarayıcıları yenilemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="46753-156">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="46753-157">CSS otomatik eşitlemesini etkinleştir veya devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="46753-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="46753-158">CSS otomatik eşitleme etkinleştirildiğinde, CSS dosyalarında herhangi bir değişiklik yaptığınızda bağlantılı tarayıcılar otomatik olarak yenilenir.</span><span class="sxs-lookup"><span data-stu-id="46753-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="46753-159">Nasıl çalıştığı</span><span class="sxs-lookup"><span data-stu-id="46753-159">How it works</span></span>

<span data-ttu-id="46753-160">Tarayıcı bağlantısı, Visual Studio ile tarayıcı arasında bir iletişim kanalı oluşturmak için SignalR kullanır.</span><span class="sxs-lookup"><span data-stu-id="46753-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="46753-161">Tarayıcı bağlantısı etkinleştirildiğinde, Visual Studio birden çok istemcinin (tarayıcının) bağlanabileceği bir SignalR sunucusu gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="46753-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="46753-162">Tarayıcı bağlantısı ayrıca ASP.NET Core isteği ardışık düzenine bir ara yazılım bileşeni kaydeder.</span><span class="sxs-lookup"><span data-stu-id="46753-162">Browser Link also registers a middleware component in the ASP.NET Core request pipeline.</span></span> <span data-ttu-id="46753-163">Bu bileşen, sunucudan her sayfa isteğine özel `<script>` başvurularını çıkarır.</span><span class="sxs-lookup"><span data-stu-id="46753-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="46753-164">Tarayıcıda **Görünüm kaynağı** ' nı seçerek komut dosyası başvurularını görebilir ve `<body>` Tag içeriğinin sonuna kadar kaydırma yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="46753-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="46753-165">Kaynak dosyalarınız değiştirilmez.</span><span class="sxs-lookup"><span data-stu-id="46753-165">Your source files aren't modified.</span></span> <span data-ttu-id="46753-166">Ara yazılım bileşeni, betik başvurularını dinamik olarak çıkarır.</span><span class="sxs-lookup"><span data-stu-id="46753-166">The middleware component injects the script references dynamically.</span></span>

<span data-ttu-id="46753-167">Tarayıcı tarafı kodu tüm JavaScript olduğundan, tarayıcı eklentisi gerekmeden SignalR desteklediği tüm tarayıcılarda çalışıyor olur.</span><span class="sxs-lookup"><span data-stu-id="46753-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
