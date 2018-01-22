---
title: "ASP.NET Core tarayıcı bağlantısı"
author: ncarandini
description: "Tarayıcı bağlantısı bir veya daha fazla web tarayıcıları ile geliştirme ortamı bağlanan bir Visual Studio özelliği ne olduğunu açıklar."
ms.author: riande
manager: wpickett
ms.date: 09/22/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-browserlink
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d5db65c268923e96c45b034639437fc3496ccac1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="04fe9-103">ASP.NET Core tarayıcı bağlantısı</span><span class="sxs-lookup"><span data-stu-id="04fe9-103">Browser Link in ASP.NET Core</span></span> 

<span data-ttu-id="04fe9-104">Tarafından [Nicolò Carandini](https://github.com/ncarandini), [CAN Wasson](https://github.com/MikeWasson), ve [zel Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="04fe9-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="04fe9-105">Tarayıcı bağlantısı Visual Studio'da geliştirme ortamı ve bir veya daha fazla web tarayıcıları arasında bir iletişim kanalı oluşturan bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="04fe9-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="04fe9-106">Tarayıcılar arası test etmek için faydalı olan bazı tarayıcılar, web uygulamanızda aynı anda yenilemek için tarayıcı bağlantısı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="04fe9-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="04fe9-107">Tarayıcı bağlantı Kur</span><span class="sxs-lookup"><span data-stu-id="04fe9-107">Browser Link setup</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="04fe9-108">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="04fe9-108">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="04fe9-109">ASP.NET Core 2.x **Web uygulaması**, **boş**, ve **Web API** şablon projeleri kullanım [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) için bir paket başvuru içeren meta-package [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span><span class="sxs-lookup"><span data-stu-id="04fe9-109">The ASP.NET Core 2.x **Web Application**, **Empty**, and **Web API** template projects use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) meta-package, which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="04fe9-110">Bu nedenle, kullanarak `Microsoft.AspNetCore.All` meta paketi tarayıcı bağlantısı kullanmak için kullanılabilir hale getirmek için başka bir eylem gerektirir.</span><span class="sxs-lookup"><span data-stu-id="04fe9-110">Therefore, using the `Microsoft.AspNetCore.All` meta-package requires no further action to make Browser Link available for use.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="04fe9-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="04fe9-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="04fe9-112">ASP.NET Core 1.x **Web uygulaması** proje şablonu için bir paket başvuru sahip [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) paket.</span><span class="sxs-lookup"><span data-stu-id="04fe9-112">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="04fe9-113">**Boş** veya **Web API** şablon projelerini paket başvuru eklemek gerekli `Microsoft.VisualStudio.Web.BrowserLink`.</span><span class="sxs-lookup"><span data-stu-id="04fe9-113">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="04fe9-114">Bu Visual Studio, en kolay yolu pakete eklemek için bir özelliktir bu yana bir **boş** veya **Web API** şablon proje mi açmak için **Paket Yöneticisi Konsolu** (**Görünüm** > **diğer pencereler** > **Paket Yöneticisi Konsolu**) ve aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="04fe9-114">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="04fe9-115">Alternatif olarak, kullanabileceğiniz **NuGet Paket Yöneticisi**.</span><span class="sxs-lookup"><span data-stu-id="04fe9-115">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="04fe9-116">Proje adına sağ tıklayın **Çözüm Gezgini** ve **NuGet paketlerini Yönet**:</span><span class="sxs-lookup"><span data-stu-id="04fe9-116">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![Açık NuGet Paket Yöneticisi](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="04fe9-118">Bul ve paketi yükleyin:</span><span class="sxs-lookup"><span data-stu-id="04fe9-118">Find and install the package:</span></span>

![Paket ile NuGet Paket Yöneticisi ekleme](using-browserlink/_static/add-package-with-nuget-package-manager.png)

---

### <a name="configuration"></a><span data-ttu-id="04fe9-120">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="04fe9-120">Configuration</span></span>

<span data-ttu-id="04fe9-121">İçinde `Configure` yöntemi *haline* dosyası:</span><span class="sxs-lookup"><span data-stu-id="04fe9-121">In the `Configure` method of the *Startup.cs* file:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="04fe9-122">Kod içindedir genellikle bir `if` aşağıda gösterildiği gibi tarayıcı bağlantısı geliştirme ortamında yalnızca etkinleştirir blok:</span><span class="sxs-lookup"><span data-stu-id="04fe9-122">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="04fe9-123">Daha fazla bilgi için bkz: [birden çok ortamlarıyla çalışma](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="04fe9-123">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="04fe9-124">Tarayıcı bağlantısı kullanma</span><span class="sxs-lookup"><span data-stu-id="04fe9-124">How to use Browser Link</span></span>

<span data-ttu-id="04fe9-125">Açık bir ASP.NET Core projeniz varsa, Visual Studio tarayıcı bağlantısı araç çubuğu denetimi yanına gösterir **hata ayıklama hedefi** araç çubuğu denetimi:</span><span class="sxs-lookup"><span data-stu-id="04fe9-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Tarayıcı bağlantısı açılır menü](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="04fe9-127">Tarayıcı bağlantısı araç denetiminden şunları yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="04fe9-127">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="04fe9-128">Aynı anda web uygulaması birkaç tarayıcılarda yenileyin.</span><span class="sxs-lookup"><span data-stu-id="04fe9-128">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="04fe9-129">Açık **tarayıcı bağlantı Pano**.</span><span class="sxs-lookup"><span data-stu-id="04fe9-129">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="04fe9-130">Etkinleştirmek veya devre dışı **tarayıcı bağlantısı**.</span><span class="sxs-lookup"><span data-stu-id="04fe9-130">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="04fe9-131">Not: Tarayıcı bağlantısı Visual Studio 2017 (15.3) varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="04fe9-131">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="04fe9-132">Etkinleştirmek veya devre dışı [CSS otomatik eşitleme](#enable-or-disable-css-auto-sync).</span><span class="sxs-lookup"><span data-stu-id="04fe9-132">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="04fe9-133">Bazı Visual Studio eklentiler, özellikle *Web uzantısı paketi 2015* ve *Web uzantısı paketi 2017*, tarayıcı bağlantısı için genişletilmiş işlevselliği sunar, ancak bazı ek özelliklerini ASP ile çalışmaz. NET çekirdek projeleri.</span><span class="sxs-lookup"><span data-stu-id="04fe9-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a><span data-ttu-id="04fe9-134">Aynı anda web uygulaması birkaç tarayıcılarda Yenile</span><span class="sxs-lookup"><span data-stu-id="04fe9-134">Refresh the web application in several browsers at once</span></span>

<span data-ttu-id="04fe9-135">Proje başlatma sırasında başlatmak için bir tek bir web tarayıcısı seçmek için açılır menüde kullanmak **hata ayıklama hedefi** araç çubuğu denetimi:</span><span class="sxs-lookup"><span data-stu-id="04fe9-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![F5 açılır menü](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="04fe9-137">Aynı anda birden çok tarayıcı açmak için **ile Gözat...**  gelen aynı açılır.</span><span class="sxs-lookup"><span data-stu-id="04fe9-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="04fe9-138">İstediğiniz tarayıcılar seçmek için CTRL tuşunu basılı tutun ve ardından **Gözat**:</span><span class="sxs-lookup"><span data-stu-id="04fe9-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Aynı anda birçok tarayıcısı açın](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="04fe9-140">Visual Studio dizin görünümünün açık gösteren ekran görüntüsü ve iki açık tarayıcılar şöyledir:</span><span class="sxs-lookup"><span data-stu-id="04fe9-140">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![İki tarayıcılar örnek ile eşitleme](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="04fe9-142">Projeye bağlı tarayıcılarda görmek için tarayıcı bağlantısı araç çubuğu denetimi üzerine getirin:</span><span class="sxs-lookup"><span data-stu-id="04fe9-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![İpucu getirin](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="04fe9-144">Dizin görünümünün değiştirmek ve tarayıcı bağlantısı Yenile düğmesini tıklatın, tüm bağlı tarayıcılar güncelleştirilir:</span><span class="sxs-lookup"><span data-stu-id="04fe9-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![değişiklik tarayıcılar eşitleme](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="04fe9-146">Tarayıcı bağlantısı dış Visual Studio'dan başlatın ve uygulamayı URL'ye tarayıcıları ile de çalışır.</span><span class="sxs-lookup"><span data-stu-id="04fe9-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="04fe9-147">Tarayıcı bağlantısı Panosu</span><span class="sxs-lookup"><span data-stu-id="04fe9-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="04fe9-148">Tarayıcı bağlantısı açılan menüden bağlantı açık tarayıcılar ile yönetmek için tarayıcı bağlantı panosunu açın:</span><span class="sxs-lookup"><span data-stu-id="04fe9-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![open-browserslink-dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="04fe9-150">Hiçbir tarayıcı bağlıysa, seçerek hata ayıklama olmayan bir oturum başlatabilirsiniz *tarayıcıda görüntüle* bağlantı:</span><span class="sxs-lookup"><span data-stu-id="04fe9-150">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![browserlink-dashboard-no-connections](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="04fe9-152">Aksi takdirde, her tarayıcı gösteren sayfasının yolu ile bağlı tarayıcılarda gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="04fe9-152">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-dashboard-two-connections](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="04fe9-154">İsterseniz, bu tek tarayıcıyı yenilemek için listelenen tarayıcı adına tıklayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="04fe9-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="04fe9-155">Etkinleştirmek veya devre dışı tarayıcı bağlantısı</span><span class="sxs-lookup"><span data-stu-id="04fe9-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="04fe9-156">Tarayıcı bağlantısı devre dışı bıraktıktan sonra yeniden etkinleştirdiğinizde, bunları yeniden bağlanmayı tarayıcılar yenilemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="04fe9-156">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="04fe9-157">Etkinleştirmek veya CSS otomatik eşitleme devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="04fe9-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="04fe9-158">CSS otomatik eşitleme etkinleştirildiğinde, CSS dosyaları herhangi bir değişiklik yaptığınızda bağlı tarayıcılar otomatik olarak yenilenir.</span><span class="sxs-lookup"><span data-stu-id="04fe9-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-does-it-work"></a><span data-ttu-id="04fe9-159">Nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="04fe9-159">How does it work?</span></span>

<span data-ttu-id="04fe9-160">Tarayıcı bağlantısı SignalR Visual Studio ve tarayıcı arasındaki iletişim kanalını oluşturmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="04fe9-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="04fe9-161">Tarayıcı bağlantısı etkin olduğunda, Visual Studio'nun birden çok istemci (tarayıcı) bağlanabileceği bir SignalR sunucusu olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="04fe9-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="04fe9-162">Tarayıcı bağlantısı, bir ara yazılım bileşeni ASP.NET istek ardışık düzeninde de kaydeder.</span><span class="sxs-lookup"><span data-stu-id="04fe9-162">Browser Link also registers a middleware component in the ASP.NET request pipeline.</span></span> <span data-ttu-id="04fe9-163">Bu bileşen özel yerleştirir `<script>` sunucudan her sayfa isteği içine başvuruları.</span><span class="sxs-lookup"><span data-stu-id="04fe9-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="04fe9-164">Betik başvuruların seçerek gördüğünüz **kaynağı görüntüle** tarayıcı ve sonuna kaydırma `<body>` etiketi içerik:</span><span class="sxs-lookup"><span data-stu-id="04fe9-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="04fe9-165">Kaynak dosyalarınız değiştiren değil.</span><span class="sxs-lookup"><span data-stu-id="04fe9-165">Your source files aren't modified.</span></span> <span data-ttu-id="04fe9-166">Ara yazılım bileşeni komut dosyası başvuruları dinamik olarak yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="04fe9-166">The middleware component injects the script references dynamically.</span></span> 

<span data-ttu-id="04fe9-167">Gözatıcı kod tüm JavaScript olduğundan, bir tarayıcı eklentisi gerek kalmadan SignalR destekleyen tüm tarayıcılar üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="04fe9-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
