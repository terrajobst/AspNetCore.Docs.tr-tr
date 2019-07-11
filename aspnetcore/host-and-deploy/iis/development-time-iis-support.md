---
title: Geliştirme zamanı IIS, ASP.NET Core için Visual Studio desteği
author: guardrex
description: IIS ile Windows Server üzerinde çalışan ASP.NET Core uygulamalarında hata ayıklamaya yönelik destek keşfedin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: f2d5dbbdc80eec035616ddea234ee5d3343eeae8
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815187"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="e3986-103">Geliştirme zamanı IIS, ASP.NET Core için Visual Studio desteği</span><span class="sxs-lookup"><span data-stu-id="e3986-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="e3986-104">Tarafından [Sourabh Shirhatti](https://twitter.com/sshirhatti) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e3986-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e3986-105">Bu makalede [Visual Studio](https://visualstudio.microsoft.com) IIS ile Windows Server üzerinde çalışan ASP.NET Core uygulamalarında hata ayıklama için destek.</span><span class="sxs-lookup"><span data-stu-id="e3986-105">This article describes [Visual Studio](https://visualstudio.microsoft.com) support for debugging ASP.NET Core apps running with IIS on Windows Server.</span></span> <span data-ttu-id="e3986-106">Bu konuda bu senaryoyu etkinleştirmek ve bir proje ayarlama gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="e3986-106">This topic walks through enabling this scenario and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e3986-107">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="e3986-107">Prerequisites</span></span>

* [<span data-ttu-id="e3986-108">Windows için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3986-108">Visual Studio for Windows</span></span>](https://visualstudio.microsoft.com/downloads/)
* <span data-ttu-id="e3986-109">**ASP.NET ve web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="e3986-109">**ASP.NET and web development** workload</span></span>
* <span data-ttu-id="e3986-110">**.NET core çoklu platform geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="e3986-110">**.NET Core cross-platform development** workload</span></span>
* <span data-ttu-id="e3986-111">X.509 güvenlik sertifikası (için HTTPS desteği)</span><span class="sxs-lookup"><span data-stu-id="e3986-111">X.509 security certificate (for HTTPS support)</span></span>

## <a name="enable-iis"></a><span data-ttu-id="e3986-112">IIS'yi etkinleştirin</span><span class="sxs-lookup"><span data-stu-id="e3986-112">Enable IIS</span></span>

1. <span data-ttu-id="e3986-113">Windows içinde gidin **Denetim Masası** > **programlar** > **programlar ve Özellikler** > **Windows Aç özelliklerini aç veya Kapat** (ekranın sol).</span><span class="sxs-lookup"><span data-stu-id="e3986-113">In Windows, navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="e3986-114">Seçin **Internet Information Services** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="e3986-114">Select the **Internet Information Services** check box.</span></span> <span data-ttu-id="e3986-115">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="e3986-115">Select **OK**.</span></span>

<span data-ttu-id="e3986-116">IIS yüklemesi, sistemin yeniden başlatılması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="e3986-116">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="e3986-117">IIS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="e3986-117">Configure IIS</span></span>

<span data-ttu-id="e3986-118">IIS, aşağıdaki ile yapılandırılmış bir Web sitesinde sahip olmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="e3986-118">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="e3986-119">**Ana bilgisayar adı** &ndash; genellikle **varsayılan Web sitesi** ile kullanılan bir **ana bilgisayar adı** , `localhost`.</span><span class="sxs-lookup"><span data-stu-id="e3986-119">**Host name** &ndash; Typically, the **Default Web Site** is used with a **Host name** of `localhost`.</span></span> <span data-ttu-id="e3986-120">Ancak, herhangi bir geçerli IIS Web sitesinde bir benzersiz ana bilgisayar adı ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="e3986-120">However, any valid IIS website with a unique host name works.</span></span>
* <span data-ttu-id="e3986-121">**Site bağlama**</span><span class="sxs-lookup"><span data-stu-id="e3986-121">**Site Binding**</span></span>
  * <span data-ttu-id="e3986-122">HTTPS gerektiren uygulamalar için bir sertifika ile 443 numaralı bağlantı noktasına bir bağlama oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e3986-122">For apps that require HTTPS, create a binding to port 443 with a certificate.</span></span> <span data-ttu-id="e3986-123">Genellikle, **IIS Express geliştirme sertifikası** kullanılan, ancak herhangi bir geçerli sertifika Works.</span><span class="sxs-lookup"><span data-stu-id="e3986-123">Typically, the **IIS Express Development Certificate** is used, but any valid certificate works.</span></span>
  * <span data-ttu-id="e3986-124">HTTP kullanan uygulamalar onaylamak için 80 göndermek için bir bağlama varlığını veya yeni bir site için 80 numaralı bağlantı noktasına bir bağlama oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e3986-124">For apps that use HTTP, confirm the existence of a binding to post 80 or create a binding to port 80 for a new site.</span></span>
  * <span data-ttu-id="e3986-125">Tek bir bağlama için HTTP veya HTTPS kullanın.</span><span class="sxs-lookup"><span data-stu-id="e3986-125">Use a single binding for either HTTP or HTTPS.</span></span> <span data-ttu-id="e3986-126">**HTTP ve HTTPS bağlantı noktalarını aynı anda bağlama desteklenmiyor.**</span><span class="sxs-lookup"><span data-stu-id="e3986-126">**Binding to both HTTP and HTTPS ports simultaneously isn't supported.**</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="e3986-127">Visual Studio'da geliştirme zamanı IIS desteğini etkinleştirin</span><span class="sxs-lookup"><span data-stu-id="e3986-127">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="e3986-128">Visual Studio Yükleyicisi'ni başlatın.</span><span class="sxs-lookup"><span data-stu-id="e3986-128">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="e3986-129">Seçin **Değiştir** IIS geliştirme zamanı desteği için kullanmayı planladığınız Visual Studio yükleme.</span><span class="sxs-lookup"><span data-stu-id="e3986-129">Select **Modify** for the Visual Studio installation that you plan to use for IIS development-time support.</span></span>
1. <span data-ttu-id="e3986-130">İçin **ASP.NET ve web geliştirme** iş yükü, bulma ve yükleme **geliştirme zamanı IIS desteğini** bileşeni.</span><span class="sxs-lookup"><span data-stu-id="e3986-130">For the **ASP.NET and web development** workload, locate and install the **Development time IIS support** component.</span></span>

   <span data-ttu-id="e3986-131">Bileşen listelenen **isteğe bağlı** bölümüne **geliştirme zamanı IIS desteğini** içinde **Yükleme ayrıntıları** panelinde sağ iş yükleri için.</span><span class="sxs-lookup"><span data-stu-id="e3986-131">The component is listed in the **Optional** section under **Development time IIS support** in the **Installation details** panel to the right of the workloads.</span></span> <span data-ttu-id="e3986-132">Bileşeni yükler [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module), ASP.NET Core uygulamaları ile IIS çalıştırmak için gereken yerel bir IIS modül olduğu.</span><span class="sxs-lookup"><span data-stu-id="e3986-132">The component installs the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps with IIS.</span></span>

## <a name="configure-the-project"></a><span data-ttu-id="e3986-133">Proje yapılandırma</span><span class="sxs-lookup"><span data-stu-id="e3986-133">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="e3986-134">HTTPS yeniden yönlendirmesi</span><span class="sxs-lookup"><span data-stu-id="e3986-134">HTTPS redirection</span></span>

<span data-ttu-id="e3986-135">HTTPS gerektiren yeni bir proje için onay kutusunu işaretleyin **HTTPS için Yapılandır** içinde **yeni bir ASP.NET Core Web uygulaması oluşturma** penceresi.</span><span class="sxs-lookup"><span data-stu-id="e3986-135">For a new project that requires HTTPS, select the check box to **Configure for HTTPS** in the **Create a new ASP.NET Core Web Application** window.</span></span> <span data-ttu-id="e3986-136">Onay kutusunu işaretleyerek ekler [HTTPS yeniden yönlendirmesi ve HSTS ara yazılım](xref:security/enforcing-ssl) oluşturulduğunda uygulamaya.</span><span class="sxs-lookup"><span data-stu-id="e3986-136">Selecting the check box adds [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) to the app when it's created.</span></span>

<span data-ttu-id="e3986-137">HTTPS gerektiren var olan bir proje için HTTPS yeniden yönlendirmesi ve HSTS Ara yazılımında kullanın `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="e3986-137">For an existing project that requires HTTPS, use HTTPS Redirection and HSTS Middleware in `Startup.Configure`.</span></span> <span data-ttu-id="e3986-138">Daha fazla bilgi için bkz. <xref:security/enforcing-ssl>.</span><span class="sxs-lookup"><span data-stu-id="e3986-138">For more information, see <xref:security/enforcing-ssl>.</span></span>

<span data-ttu-id="e3986-139">HTTP kullanan bir proje için [HTTPS yeniden yönlendirmesi ve HSTS ara yazılım](xref:security/enforcing-ssl) uygulamaya eklenmez.</span><span class="sxs-lookup"><span data-stu-id="e3986-139">For a project that uses HTTP, [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) aren't added to the app.</span></span> <span data-ttu-id="e3986-140">Uygulama yapılandırma gerekmiyor.</span><span class="sxs-lookup"><span data-stu-id="e3986-140">No app configuration is required.</span></span>

### <a name="iis-launch-profile"></a><span data-ttu-id="e3986-141">IIS başlatma profili</span><span class="sxs-lookup"><span data-stu-id="e3986-141">IIS launch profile</span></span>

<span data-ttu-id="e3986-142">Geliştirme zamanı IIS desteği eklemek için yeni bir başlatma profili oluşturun:</span><span class="sxs-lookup"><span data-stu-id="e3986-142">Create a new launch profile to add development-time IIS support:</span></span>

::: moniker range=">= aspnetcore-3.0"

1. <span data-ttu-id="e3986-143">Projeye sağ **Çözüm Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="e3986-143">Right-click the project in **Solution Explorer**.</span></span> <span data-ttu-id="e3986-144">Seçin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="e3986-144">Select **Properties**.</span></span> <span data-ttu-id="e3986-145">Açık **hata ayıklama** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="e3986-145">Open the **Debug** tab.</span></span>
1. <span data-ttu-id="e3986-146">İçin **profili**seçin **yeni** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="e3986-146">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="e3986-147">Profili, "IIS" açılan pencerede adlandırın.</span><span class="sxs-lookup"><span data-stu-id="e3986-147">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="e3986-148">Seçin **Tamam** profili oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="e3986-148">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="e3986-149">İçin **başlatma** ayarını seçin **IIS** listeden.</span><span class="sxs-lookup"><span data-stu-id="e3986-149">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="e3986-150">Onay kutusunu seçin **tarayıcıyı başlatın** ve uç nokta URL'sini sağlayın.</span><span class="sxs-lookup"><span data-stu-id="e3986-150">Select the check box for **Launch browser** and provide the endpoint URL.</span></span>

   <span data-ttu-id="e3986-151">Uygulama HTTPS gerektirdiğinde, HTTPS uç noktasının kullanın (`https://`).</span><span class="sxs-lookup"><span data-stu-id="e3986-151">When the app requires HTTPS, use an HTTPS endpoint (`https://`).</span></span> <span data-ttu-id="e3986-152">HTTP için bir HTTP kullan (`http://`) uç noktası.</span><span class="sxs-lookup"><span data-stu-id="e3986-152">For HTTP, use an HTTP (`http://`) endpoint.</span></span>

   <span data-ttu-id="e3986-153">Aynı konak adı belirtin ve olarak bağlantı noktası [IIS yapılandırması belirtilen önceki kullanımlar](#configure-iis), genellikle `localhost`.</span><span class="sxs-lookup"><span data-stu-id="e3986-153">Provide the same host name and port as the [IIS configuration specified earlier uses](#configure-iis), typically `localhost`.</span></span>

   <span data-ttu-id="e3986-154">URL'nin sonunda bir uygulama adı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="e3986-154">Provide the name of the app at the end of the URL.</span></span>

   <span data-ttu-id="e3986-155">Örneğin, `https://localhost/WebApplication1` (HTTPS) ya da `http://localhost/WebApplication1` (HTTP) geçerli uç nokta URL'leri.</span><span class="sxs-lookup"><span data-stu-id="e3986-155">For example, `https://localhost/WebApplication1` (HTTPS) or `http://localhost/WebApplication1` (HTTP) are valid endpoint URLs.</span></span>
1. <span data-ttu-id="e3986-156">İçinde **ortam değişkenlerini** bölümünden **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="e3986-156">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="e3986-157">Bir ortam değişkeni ile sağlayan bir **adı** , `ASPNETCORE_ENVIRONMENT` ve **değer** , `Development`.</span><span class="sxs-lookup"><span data-stu-id="e3986-157">Provide an environment variable with a **Name** of `ASPNETCORE_ENVIRONMENT` and a **Value** of `Development`.</span></span>
1. <span data-ttu-id="e3986-158">İçinde **Web sunucusu ayarlarını** alanı ayarlama **uygulama URL'si** için kullanılan aynı değere **tarayıcıyı başlatın** uç nokta URL'si.</span><span class="sxs-lookup"><span data-stu-id="e3986-158">In the **Web Server Settings** area, set the **App URL** to the same value used for the **Launch browser** endpoint URL.</span></span>
1. <span data-ttu-id="e3986-159">İçin **barındırma modeli** ayarı Visual Studio 2019 ya da daha sonra Seç **varsayılan** proje tarafından kullanılan barındırma modelini kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="e3986-159">For the **Hosting Model** setting in Visual Studio 2019 or later, select **Default** to use the hosting model used by the project.</span></span> <span data-ttu-id="e3986-160">Proje ayarlarsa `<AspNetCoreHostingModel>` kendi proje dosyası bir özellik, özelliğin değerini (`InProcess` veya `OutOfProcess`) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e3986-160">If the project sets the `<AspNetCoreHostingModel>` property in its project file, the value of the property (`InProcess` or `OutOfProcess`) is used.</span></span> <span data-ttu-id="e3986-161">Özelliği yoksa, işlem içi olduğu barındırma modeli uygulamanın varsayılan kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e3986-161">If the property isn't present, the default hosting model of the app is used, which is in-process.</span></span> <span data-ttu-id="e3986-162">Uygulamayı, uygulamanın barındırma normal modelden farklı ayarı açık bir barındırma modeli gerektiriyorsa, ayarlama **barındırma modeli** ya da `In Process` veya `Out Of Process` gerektiğinde.</span><span class="sxs-lookup"><span data-stu-id="e3986-162">If the app requires an explicit hosting model setting different from the app's normal hosting model, set the **Hosting Model** to either `In Process` or `Out Of Process` as needed.</span></span>
1. <span data-ttu-id="e3986-163">Profili kaydedin.</span><span class="sxs-lookup"><span data-stu-id="e3986-163">Save the profile.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

1. <span data-ttu-id="e3986-164">Projeye sağ **Çözüm Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="e3986-164">Right-click the project in **Solution Explorer**.</span></span> <span data-ttu-id="e3986-165">Seçin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="e3986-165">Select **Properties**.</span></span> <span data-ttu-id="e3986-166">Açık **hata ayıklama** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="e3986-166">Open the **Debug** tab.</span></span>
1. <span data-ttu-id="e3986-167">İçin **profili**seçin **yeni** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="e3986-167">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="e3986-168">Profili, "IIS" açılan pencerede adlandırın.</span><span class="sxs-lookup"><span data-stu-id="e3986-168">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="e3986-169">Seçin **Tamam** profili oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="e3986-169">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="e3986-170">İçin **başlatma** ayarını seçin **IIS** listeden.</span><span class="sxs-lookup"><span data-stu-id="e3986-170">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="e3986-171">Onay kutusunu seçin **tarayıcıyı başlatın** ve uç nokta URL'sini sağlayın.</span><span class="sxs-lookup"><span data-stu-id="e3986-171">Select the check box for **Launch browser** and provide the endpoint URL.</span></span>

   <span data-ttu-id="e3986-172">Uygulama HTTPS gerektirdiğinde, HTTPS uç noktasının kullanın (`https://`).</span><span class="sxs-lookup"><span data-stu-id="e3986-172">When the app requires HTTPS, use an HTTPS endpoint (`https://`).</span></span> <span data-ttu-id="e3986-173">HTTP için bir HTTP kullan (`http://`) uç noktası.</span><span class="sxs-lookup"><span data-stu-id="e3986-173">For HTTP, use an HTTP (`http://`) endpoint.</span></span>

   <span data-ttu-id="e3986-174">Aynı konak adı belirtin ve olarak bağlantı noktası [IIS yapılandırması belirtilen önceki kullanımlar](#configure-iis), genellikle `localhost`.</span><span class="sxs-lookup"><span data-stu-id="e3986-174">Provide the same host name and port as the [IIS configuration specified earlier uses](#configure-iis), typically `localhost`.</span></span>

   <span data-ttu-id="e3986-175">URL'nin sonunda bir uygulama adı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="e3986-175">Provide the name of the app at the end of the URL.</span></span>

   <span data-ttu-id="e3986-176">Örneğin, `https://localhost/WebApplication1` (HTTPS) ya da `http://localhost/WebApplication1` (HTTP) geçerli uç nokta URL'leri.</span><span class="sxs-lookup"><span data-stu-id="e3986-176">For example, `https://localhost/WebApplication1` (HTTPS) or `http://localhost/WebApplication1` (HTTP) are valid endpoint URLs.</span></span>
1. <span data-ttu-id="e3986-177">İçinde **ortam değişkenlerini** bölümünden **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="e3986-177">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="e3986-178">Bir ortam değişkeni ile sağlayan bir **adı** , `ASPNETCORE_ENVIRONMENT` ve **değer** , `Development`.</span><span class="sxs-lookup"><span data-stu-id="e3986-178">Provide an environment variable with a **Name** of `ASPNETCORE_ENVIRONMENT` and a **Value** of `Development`.</span></span>
1. <span data-ttu-id="e3986-179">İçinde **Web sunucusu ayarlarını** alanı ayarlama **uygulama URL'si** için kullanılan aynı değere **tarayıcıyı başlatın** uç nokta URL'si.</span><span class="sxs-lookup"><span data-stu-id="e3986-179">In the **Web Server Settings** area, set the **App URL** to the same value used for the **Launch browser** endpoint URL.</span></span>
1. <span data-ttu-id="e3986-180">İçin **barındırma modeli** ayarı Visual Studio 2019 ya da daha sonra Seç **varsayılan** proje tarafından kullanılan barındırma modelini kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="e3986-180">For the **Hosting Model** setting in Visual Studio 2019 or later, select **Default** to use the hosting model used by the project.</span></span> <span data-ttu-id="e3986-181">Proje ayarlarsa `<AspNetCoreHostingModel>` kendi proje dosyası bir özellik, özelliğin değerini (`InProcess` veya `OutOfProcess`) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e3986-181">If the project sets the `<AspNetCoreHostingModel>` property in its project file, the value of the property (`InProcess` or `OutOfProcess`) is used.</span></span> <span data-ttu-id="e3986-182">Özelliği yoksa, işlem dışı olduğu barındırma modeli uygulamanın varsayılan kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e3986-182">If the property isn't present, the default hosting model of the app is used, which is out-of-process.</span></span> <span data-ttu-id="e3986-183">Uygulamayı, uygulamanın barındırma normal modelden farklı ayarı açık bir barındırma modeli gerektiriyorsa, ayarlama **barındırma modeli** ya da `In Process` veya `Out Of Process` gerektiğinde.</span><span class="sxs-lookup"><span data-stu-id="e3986-183">If the app requires an explicit hosting model setting different from the app's normal hosting model, set the **Hosting Model** to either `In Process` or `Out Of Process` as needed.</span></span>
1. <span data-ttu-id="e3986-184">Profili kaydedin.</span><span class="sxs-lookup"><span data-stu-id="e3986-184">Save the profile.</span></span>

::: moniker-end

<span data-ttu-id="e3986-185">Ne zaman Visual Studio kullanmayan el ile eklemek için bir başlatma profili [launchSettings.json](https://json.schemastore.org/launchsettings) dosyası *özellikleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="e3986-185">When not using Visual Studio, manually add a launch profile to the [launchSettings.json](https://json.schemastore.org/launchsettings) file in the *Properties* folder.</span></span> <span data-ttu-id="e3986-186">Aşağıdaki örnek, HTTPS protokolünü kullanmak için profil yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="e3986-186">The following example configures the profile to use the HTTPS protocol:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iis": {
      "applicationUrl": "https://localhost/WebApplication1",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS": {
      "commandName": "IIS",
      "launchBrowser": true,
      "launchUrl": "https://localhost/WebApplication1",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

<span data-ttu-id="e3986-187">Onaylayın `applicationUrl` ve `launchUrl` uç noktaları eşleşen ve aynı protokolü IIS bağlama yapılandırması, HTTP veya HTTPS kullanın.</span><span class="sxs-lookup"><span data-stu-id="e3986-187">Confirm that the `applicationUrl` and `launchUrl` endpoints match and use the same protocol as the IIS binding configuration, either HTTP or HTTPS.</span></span>

## <a name="run-the-project"></a><span data-ttu-id="e3986-188">Projeyi Çalıştır</span><span class="sxs-lookup"><span data-stu-id="e3986-188">Run the project</span></span>

<span data-ttu-id="e3986-189">Visual Studio'yu yönetici olarak çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e3986-189">Run Visual Studio as an administrator:</span></span>

* <span data-ttu-id="e3986-190">Aşağı açılan listede derleme yapılandırmasını değerine ayarlandığını onaylayın **hata ayıklama**.</span><span class="sxs-lookup"><span data-stu-id="e3986-190">Confirm that the build configuration drop-down list is set to **Debug**.</span></span>
* <span data-ttu-id="e3986-191">Çalıştır düğmesini kümesine **IIS** profil ve uygulamayı başlatmak için düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="e3986-191">Set the Run button to the **IIS** profile and select the button to start the app.</span></span>

<span data-ttu-id="e3986-192">Visual Studio'yu yönetici olarak çalıştırarak değil, yeniden başlatma isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="e3986-192">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="e3986-193">İstenirse, Visual Studio'yu yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="e3986-193">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="e3986-194">Güvenilmeyen bir geliştirme sertifikası kullanıyorsanız, tarayıcı Güvenilmeyen sertifika için bir özel durum oluşturmak gerekli kılabiliriz.</span><span class="sxs-lookup"><span data-stu-id="e3986-194">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="e3986-195">Yayın hata ayıklama derleme yapılandırması ile [yalnızca kendi kodum](/visualstudio/debugger/just-my-code) ve düzeyi düşürülmüş bir deneyimle derleyici iyileştirmeleri sonuçları.</span><span class="sxs-lookup"><span data-stu-id="e3986-195">Debugging a Release build configuration with [Just My Code](/visualstudio/debugger/just-my-code) and compiler optimizations results in a degraded experience.</span></span> <span data-ttu-id="e3986-196">Örneğin, kesme noktaları isabet değildir.</span><span class="sxs-lookup"><span data-stu-id="e3986-196">For example, break points aren't hit.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e3986-197">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="e3986-197">Additional resources</span></span>

* [<span data-ttu-id="e3986-198">IIS'de IIS Yöneticisi'ni kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="e3986-198">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [<span data-ttu-id="e3986-199">Windows IIS üzerinde ASP.NET Core barındırma</span><span class="sxs-lookup"><span data-stu-id="e3986-199">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="e3986-200">ASP.NET Core modülü için giriş</span><span class="sxs-lookup"><span data-stu-id="e3986-200">Introduction to ASP.NET Core Module</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="e3986-201">ASP.NET Core Module yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="e3986-201">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="e3986-202">HTTPS'yi Zorunlu Kılma</span><span class="sxs-lookup"><span data-stu-id="e3986-202">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
