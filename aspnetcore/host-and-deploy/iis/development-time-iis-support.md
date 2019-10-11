---
title: ASP.NET Core için Visual Studio 'da geliştirme zamanı IIS desteği
author: guardrex
description: Windows Server 'da IIS ile çalışırken ASP.NET Core uygulamalarda hata ayıklama desteğini bulur.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2019
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: e5da4f7202bf31e65c366d6f7de54212f5b0fed7
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259804"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="17e0a-103">ASP.NET Core için Visual Studio 'da geliştirme zamanı IIS desteği</span><span class="sxs-lookup"><span data-stu-id="17e0a-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="17e0a-104">, [Sourabh Shirhatti](https://twitter.com/sshirhatti) ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="17e0a-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="17e0a-105">Bu makalede, Windows Server 'da IIS ile çalışan ASP.NET Core hata ayıklama için [Visual Studio](https://visualstudio.microsoft.com) desteği açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="17e0a-105">This article describes [Visual Studio](https://visualstudio.microsoft.com) support for debugging ASP.NET Core apps running with IIS on Windows Server.</span></span> <span data-ttu-id="17e0a-106">Bu konu başlığı altında, bu senaryonun etkinleştirilmesi ve bir projenin kurulması anlatılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="17e0a-106">This topic walks through enabling this scenario and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="17e0a-107">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="17e0a-107">Prerequisites</span></span>

* [<span data-ttu-id="17e0a-108">Windows için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="17e0a-108">Visual Studio for Windows</span></span>](https://visualstudio.microsoft.com/downloads/)
* <span data-ttu-id="17e0a-109">**ASP.net ve Web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="17e0a-109">**ASP.NET and web development** workload</span></span>
* <span data-ttu-id="17e0a-110">**.NET Core platformlar arası geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="17e0a-110">**.NET Core cross-platform development** workload</span></span>
* <span data-ttu-id="17e0a-111">X. 509.440 güvenlik sertifikası (HTTPS desteği için)</span><span class="sxs-lookup"><span data-stu-id="17e0a-111">X.509 security certificate (for HTTPS support)</span></span>

## <a name="enable-iis"></a><span data-ttu-id="17e0a-112">IIS 'yi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="17e0a-112">Enable IIS</span></span>

1. <span data-ttu-id="17e0a-113">Windows 'da, **Denetim masası** > **Programlar** > **Programlar ve Özellikler** > **Windows özelliklerini aç veya kapat** (ekranın sol tarafında) ' ya gidin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-113">In Windows, navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="17e0a-114">**Internet Information Services** onay kutusunu seçin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-114">Select the **Internet Information Services** check box.</span></span> <span data-ttu-id="17e0a-115">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-115">Select **OK**.</span></span>

<span data-ttu-id="17e0a-116">IIS yüklemesi için sistemin yeniden başlatılması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="17e0a-116">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="17e0a-117">IIS 'yi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="17e0a-117">Configure IIS</span></span>

<span data-ttu-id="17e0a-118">IIS 'nin aşağıdaki ile yapılandırılmış bir Web sitesine sahip olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="17e0a-118">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="17e0a-119">**Ana bilgisayar adı** &ndash; genellikle, **varsayılan Web sitesi** `localhost` **ana bilgisayar adıyla** kullanılır.</span><span class="sxs-lookup"><span data-stu-id="17e0a-119">**Host name** &ndash; Typically, the **Default Web Site** is used with a **Host name** of `localhost`.</span></span> <span data-ttu-id="17e0a-120">Ancak, benzersiz bir ana bilgisayar adına sahip geçerli bir IIS Web sitesi çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="17e0a-120">However, any valid IIS website with a unique host name works.</span></span>
* <span data-ttu-id="17e0a-121">**Site bağlama**</span><span class="sxs-lookup"><span data-stu-id="17e0a-121">**Site Binding**</span></span>
  * <span data-ttu-id="17e0a-122">HTTPS gerektiren uygulamalar için, sertifika ile 443 numaralı bağlantı noktasına bir bağlama oluşturun.</span><span class="sxs-lookup"><span data-stu-id="17e0a-122">For apps that require HTTPS, create a binding to port 443 with a certificate.</span></span> <span data-ttu-id="17e0a-123">Genellikle **IIS Express geliştirme sertifikası** kullanılır, ancak geçerli bir sertifika çalışıyor olur.</span><span class="sxs-lookup"><span data-stu-id="17e0a-123">Typically, the **IIS Express Development Certificate** is used, but any valid certificate works.</span></span>
  * <span data-ttu-id="17e0a-124">HTTP kullanan uygulamalar için, 80 veya yeni bir site için bağlantı noktası 80 ' e bir bağlama oluşturmak için bir bağlamanın mevcut olduğunu onaylayın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-124">For apps that use HTTP, confirm the existence of a binding to post 80 or create a binding to port 80 for a new site.</span></span>
  * <span data-ttu-id="17e0a-125">HTTP veya HTTPS için tek bir bağlama kullanın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-125">Use a single binding for either HTTP or HTTPS.</span></span> <span data-ttu-id="17e0a-126">**Aynı anda hem HTTP hem de HTTPS bağlantı noktalarına bağlama desteklenmez.**</span><span class="sxs-lookup"><span data-stu-id="17e0a-126">**Binding to both HTTP and HTTPS ports simultaneously isn't supported.**</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="17e0a-127">Visual Studio 'da geliştirme zamanı IIS desteğini etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="17e0a-127">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="17e0a-128">Visual Studio yükleyicisi 'ni başlatın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-128">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="17e0a-129">IIS geliştirme zamanı desteği için kullanmayı planladığınız Visual Studio yüklemesi için **Değiştir** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-129">Select **Modify** for the Visual Studio installation that you plan to use for IIS development-time support.</span></span>
1. <span data-ttu-id="17e0a-130">**ASP.net ve Web geliştirme** iş yükü için **geliştirme zamanı IIS destek** bileşeni ' ni bulun ve yükler.</span><span class="sxs-lookup"><span data-stu-id="17e0a-130">For the **ASP.NET and web development** workload, locate and install the **Development time IIS support** component.</span></span>

   <span data-ttu-id="17e0a-131">Bileşen, iş yüklerinin sağındaki **Yükleme ayrıntıları** panelinde, **geliştirme zamanı IIS desteği** altındaki **isteğe bağlı** bölümde listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="17e0a-131">The component is listed in the **Optional** section under **Development time IIS support** in the **Installation details** panel to the right of the workloads.</span></span> <span data-ttu-id="17e0a-132">Bileşen, IIS ile ASP.NET Core uygulamaları çalıştırmak için gerekli yerel bir IIS modülü olan [ASP.NET Core modülünü](xref:host-and-deploy/aspnet-core-module)yüklüyor.</span><span class="sxs-lookup"><span data-stu-id="17e0a-132">The component installs the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps with IIS.</span></span>

## <a name="configure-the-project"></a><span data-ttu-id="17e0a-133">Projeyi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="17e0a-133">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="17e0a-134">HTTPS yönlendirmesi</span><span class="sxs-lookup"><span data-stu-id="17e0a-134">HTTPS redirection</span></span>

<span data-ttu-id="17e0a-135">HTTPS gerektiren yeni bir proje için **yeni ASP.NET Core Web uygulaması oluşturma** penceresinde **https için yapılandırmak** üzere onay kutusunu seçin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-135">For a new project that requires HTTPS, select the check box to **Configure for HTTPS** in the **Create a new ASP.NET Core Web Application** window.</span></span> <span data-ttu-id="17e0a-136">Onay kutusunun belirlenmesi, uygulama oluşturulduğunda [https yeniden yönlendirme ve HSTS ara yazılımı](xref:security/enforcing-ssl) ekler.</span><span class="sxs-lookup"><span data-stu-id="17e0a-136">Selecting the check box adds [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) to the app when it's created.</span></span>

<span data-ttu-id="17e0a-137">HTTPS gerektiren mevcut bir proje için, `Startup.Configure` ' da HTTPS yeniden yönlendirme ve HSTS ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-137">For an existing project that requires HTTPS, use HTTPS Redirection and HSTS Middleware in `Startup.Configure`.</span></span> <span data-ttu-id="17e0a-138">Daha fazla bilgi için bkz. <xref:security/enforcing-ssl>.</span><span class="sxs-lookup"><span data-stu-id="17e0a-138">For more information, see <xref:security/enforcing-ssl>.</span></span>

<span data-ttu-id="17e0a-139">HTTP kullanan bir proje için, [https yeniden yönlendirme ve HSTS ara yazılımı](xref:security/enforcing-ssl) uygulamaya eklenmez.</span><span class="sxs-lookup"><span data-stu-id="17e0a-139">For a project that uses HTTP, [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) aren't added to the app.</span></span> <span data-ttu-id="17e0a-140">Uygulama yapılandırması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="17e0a-140">No app configuration is required.</span></span>

### <a name="iis-launch-profile"></a><span data-ttu-id="17e0a-141">IIS başlatma profili</span><span class="sxs-lookup"><span data-stu-id="17e0a-141">IIS launch profile</span></span>

<span data-ttu-id="17e0a-142">Geliştirme zamanı IIS desteği eklemek için yeni bir başlatma profili oluşturun:</span><span class="sxs-lookup"><span data-stu-id="17e0a-142">Create a new launch profile to add development-time IIS support:</span></span>

::: moniker range=">= aspnetcore-3.0"

1. <span data-ttu-id="17e0a-143">**Çözüm Gezgini**projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-143">Right-click the project in **Solution Explorer**.</span></span> <span data-ttu-id="17e0a-144">**Özellikler**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-144">Select **Properties**.</span></span> <span data-ttu-id="17e0a-145">**Hata Ayıkla** sekmesini açın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-145">Open the **Debug** tab.</span></span>
1. <span data-ttu-id="17e0a-146">**Profil**için **Yeni** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-146">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="17e0a-147">Profili, açılan pencerede "IIS" olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-147">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="17e0a-148">Profili oluşturmak için **Tamam ' ı** seçin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-148">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="17e0a-149">**Başlatma** ayarı Için listeden **IIS** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-149">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="17e0a-150">**Başlat tarayıcısı** onay kutusunu seçin ve uç nokta URL 'sini sağlayın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-150">Select the check box for **Launch browser** and provide the endpoint URL.</span></span>

   <span data-ttu-id="17e0a-151">Uygulama HTTPS gerektirdiğinde, bir HTTPS uç noktası (`https://`) kullanın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-151">When the app requires HTTPS, use an HTTPS endpoint (`https://`).</span></span> <span data-ttu-id="17e0a-152">HTTP için bir HTTP (`http://`) uç noktası kullanın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-152">For HTTP, use an HTTP (`http://`) endpoint.</span></span>

   <span data-ttu-id="17e0a-153">[Daha önce belirtilen IIS yapılandırmasıyla](#configure-iis)aynı ana bilgisayar adını ve bağlantı noktasını, genellikle `localhost` olarak belirtin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-153">Provide the same host name and port as the [IIS configuration specified earlier uses](#configure-iis), typically `localhost`.</span></span>

   <span data-ttu-id="17e0a-154">URL 'nin sonundaki uygulamanın adını belirtin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-154">Provide the name of the app at the end of the URL.</span></span>

   <span data-ttu-id="17e0a-155">Örneğin, `https://localhost/WebApplication1` (HTTPS) veya `http://localhost/WebApplication1` (HTTP) geçerli uç nokta URL 'Lardır.</span><span class="sxs-lookup"><span data-stu-id="17e0a-155">For example, `https://localhost/WebApplication1` (HTTPS) or `http://localhost/WebApplication1` (HTTP) are valid endpoint URLs.</span></span>
1. <span data-ttu-id="17e0a-156">**Ortam değişkenleri** bölümünde **Ekle** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-156">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="17e0a-157">@No__t-1 **adı** ve `Development` **değeri** olan bir ortam değişkeni sağlayın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-157">Provide an environment variable with a **Name** of `ASPNETCORE_ENVIRONMENT` and a **Value** of `Development`.</span></span>
1. <span data-ttu-id="17e0a-158">**Web sunucusu ayarları** alanında, **Uygulama URL** 'sini **başlatma tarayıcısı** uç noktası URL 'si için kullanılan aynı değere ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-158">In the **Web Server Settings** area, set the **App URL** to the same value used for the **Launch browser** endpoint URL.</span></span>
1. <span data-ttu-id="17e0a-159">Visual Studio 2019 veya sonraki sürümlerde **barındırma modeli** ayarı için, proje tarafından kullanılan barındırma modelini kullanmak üzere **varsayılan** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-159">For the **Hosting Model** setting in Visual Studio 2019 or later, select **Default** to use the hosting model used by the project.</span></span> <span data-ttu-id="17e0a-160">Proje, proje dosyasında `<AspNetCoreHostingModel>` özelliğini ayarlarsa, özelliğin değeri (`InProcess` veya `OutOfProcess`) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="17e0a-160">If the project sets the `<AspNetCoreHostingModel>` property in its project file, the value of the property (`InProcess` or `OutOfProcess`) is used.</span></span> <span data-ttu-id="17e0a-161">Özellik mevcut değilse, uygulamanın varsayılan barındırma modeli, işlem içi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="17e0a-161">If the property isn't present, the default hosting model of the app is used, which is in-process.</span></span> <span data-ttu-id="17e0a-162">Uygulama, uygulamanın normal barındırma modelinden farklı bir açık barındırma modeli ayarı gerektiriyorsa, **barındırma modelini** gerektiği gibi `In Process` veya `Out Of Process` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-162">If the app requires an explicit hosting model setting different from the app's normal hosting model, set the **Hosting Model** to either `In Process` or `Out Of Process` as needed.</span></span>
1. <span data-ttu-id="17e0a-163">Profili kaydedin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-163">Save the profile.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

1. <span data-ttu-id="17e0a-164">**Çözüm Gezgini**projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-164">Right-click the project in **Solution Explorer**.</span></span> <span data-ttu-id="17e0a-165">**Özellikler**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-165">Select **Properties**.</span></span> <span data-ttu-id="17e0a-166">**Hata Ayıkla** sekmesini açın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-166">Open the **Debug** tab.</span></span>
1. <span data-ttu-id="17e0a-167">**Profil**için **Yeni** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-167">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="17e0a-168">Profili, açılan pencerede "IIS" olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-168">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="17e0a-169">Profili oluşturmak için **Tamam ' ı** seçin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-169">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="17e0a-170">**Başlatma** ayarı Için listeden **IIS** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-170">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="17e0a-171">**Başlat tarayıcısı** onay kutusunu seçin ve uç nokta URL 'sini sağlayın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-171">Select the check box for **Launch browser** and provide the endpoint URL.</span></span>

   <span data-ttu-id="17e0a-172">Uygulama HTTPS gerektirdiğinde, bir HTTPS uç noktası (`https://`) kullanın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-172">When the app requires HTTPS, use an HTTPS endpoint (`https://`).</span></span> <span data-ttu-id="17e0a-173">HTTP için bir HTTP (`http://`) uç noktası kullanın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-173">For HTTP, use an HTTP (`http://`) endpoint.</span></span>

   <span data-ttu-id="17e0a-174">[Daha önce belirtilen IIS yapılandırmasıyla](#configure-iis)aynı ana bilgisayar adını ve bağlantı noktasını, genellikle `localhost` olarak belirtin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-174">Provide the same host name and port as the [IIS configuration specified earlier uses](#configure-iis), typically `localhost`.</span></span>

   <span data-ttu-id="17e0a-175">URL 'nin sonundaki uygulamanın adını belirtin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-175">Provide the name of the app at the end of the URL.</span></span>

   <span data-ttu-id="17e0a-176">Örneğin, `https://localhost/WebApplication1` (HTTPS) veya `http://localhost/WebApplication1` (HTTP) geçerli uç nokta URL 'Lardır.</span><span class="sxs-lookup"><span data-stu-id="17e0a-176">For example, `https://localhost/WebApplication1` (HTTPS) or `http://localhost/WebApplication1` (HTTP) are valid endpoint URLs.</span></span>
1. <span data-ttu-id="17e0a-177">**Ortam değişkenleri** bölümünde **Ekle** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-177">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="17e0a-178">@No__t-1 **adı** ve `Development` **değeri** olan bir ortam değişkeni sağlayın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-178">Provide an environment variable with a **Name** of `ASPNETCORE_ENVIRONMENT` and a **Value** of `Development`.</span></span>
1. <span data-ttu-id="17e0a-179">**Web sunucusu ayarları** alanında, **Uygulama URL** 'sini **başlatma tarayıcısı** uç noktası URL 'si için kullanılan aynı değere ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-179">In the **Web Server Settings** area, set the **App URL** to the same value used for the **Launch browser** endpoint URL.</span></span>
1. <span data-ttu-id="17e0a-180">Visual Studio 2019 veya sonraki sürümlerde **barındırma modeli** ayarı için, proje tarafından kullanılan barındırma modelini kullanmak üzere **varsayılan** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-180">For the **Hosting Model** setting in Visual Studio 2019 or later, select **Default** to use the hosting model used by the project.</span></span> <span data-ttu-id="17e0a-181">Proje, proje dosyasında `<AspNetCoreHostingModel>` özelliğini ayarlarsa, özelliğin değeri (`InProcess` veya `OutOfProcess`) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="17e0a-181">If the project sets the `<AspNetCoreHostingModel>` property in its project file, the value of the property (`InProcess` or `OutOfProcess`) is used.</span></span> <span data-ttu-id="17e0a-182">Özellik mevcut değilse, uygulamanın varsayılan barındırma modeli kullanılır ve bu işlem, işlem dışı olur.</span><span class="sxs-lookup"><span data-stu-id="17e0a-182">If the property isn't present, the default hosting model of the app is used, which is out-of-process.</span></span> <span data-ttu-id="17e0a-183">Uygulama, uygulamanın normal barındırma modelinden farklı bir açık barındırma modeli ayarı gerektiriyorsa, **barındırma modelini** gerektiği gibi `In Process` veya `Out Of Process` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-183">If the app requires an explicit hosting model setting different from the app's normal hosting model, set the **Hosting Model** to either `In Process` or `Out Of Process` as needed.</span></span>
1. <span data-ttu-id="17e0a-184">Profili kaydedin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-184">Save the profile.</span></span>

::: moniker-end

<span data-ttu-id="17e0a-185">Visual Studio kullanmadığınız durumlarda, *Özellikler* klasöründeki [launchsettings. JSON](https://json.schemastore.org/launchsettings) dosyasına el ile bir başlatma profili ekleyin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-185">When not using Visual Studio, manually add a launch profile to the [launchSettings.json](https://json.schemastore.org/launchsettings) file in the *Properties* folder.</span></span> <span data-ttu-id="17e0a-186">Aşağıdaki örnek, HTTPS protokolünü kullanmak için profili yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="17e0a-186">The following example configures the profile to use the HTTPS protocol:</span></span>

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

<span data-ttu-id="17e0a-187">@No__t-0 ve `launchUrl` bitiş noktalarının eşleştiğini ve IIS bağlama yapılandırmasıyla aynı Protokolü (HTTP veya HTTPS) kullandığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-187">Confirm that the `applicationUrl` and `launchUrl` endpoints match and use the same protocol as the IIS binding configuration, either HTTP or HTTPS.</span></span>

## <a name="run-the-project"></a><span data-ttu-id="17e0a-188">Projeyi çalıştırma</span><span class="sxs-lookup"><span data-stu-id="17e0a-188">Run the project</span></span>

<span data-ttu-id="17e0a-189">Visual Studio 'Yu yönetici olarak çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="17e0a-189">Run Visual Studio as an administrator:</span></span>

* <span data-ttu-id="17e0a-190">Derleme yapılandırması açılır listesinin **hata ayıklama**olarak ayarlandığını onaylayın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-190">Confirm that the build configuration drop-down list is set to **Debug**.</span></span>
* <span data-ttu-id="17e0a-191">[Hata ayıklamayı Başlat düğmesini](/visualstudio/debugger/debugger-feature-tour) **IIS** profiline ayarlayın ve uygulamayı başlatmak için düğmeyi seçin.</span><span class="sxs-lookup"><span data-stu-id="17e0a-191">Set the [Start Debugging button](/visualstudio/debugger/debugger-feature-tour) to the **IIS** profile and select the button to start the app.</span></span>

<span data-ttu-id="17e0a-192">Visual Studio, yönetici olarak çalışmıyorsa bir yeniden başlatma isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="17e0a-192">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="17e0a-193">İstenirse, Visual Studio 'Yu yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="17e0a-193">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="17e0a-194">Güvenilmeyen bir geliştirme sertifikası kullanılırsa, tarayıcı güvenilmeyen sertifika için bir özel durum oluşturmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="17e0a-194">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="17e0a-195">[Yalnızca kendi kodum](/visualstudio/debugger/just-my-code) ve derleyici Iyileştirmeleriyle yayın derleme yapılandırması hata ayıklaması, düzeyi düşürülmüş bir deneyimle sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="17e0a-195">Debugging a Release build configuration with [Just My Code](/visualstudio/debugger/just-my-code) and compiler optimizations results in a degraded experience.</span></span> <span data-ttu-id="17e0a-196">Örneğin, kesme noktaları isabet edilmez.</span><span class="sxs-lookup"><span data-stu-id="17e0a-196">For example, break points aren't hit.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="17e0a-197">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="17e0a-197">Additional resources</span></span>

* [<span data-ttu-id="17e0a-198">IIS 'de IIS Yöneticisi 'Ni kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="17e0a-198">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [<span data-ttu-id="17e0a-199">IIS ile Windows üzerinde ASP.NET Core barındırma</span><span class="sxs-lookup"><span data-stu-id="17e0a-199">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="17e0a-200">ASP.NET Core modüle giriş</span><span class="sxs-lookup"><span data-stu-id="17e0a-200">Introduction to ASP.NET Core Module</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="17e0a-201">ASP.NET Core modülü yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="17e0a-201">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="17e0a-202">HTTPS zorlama</span><span class="sxs-lookup"><span data-stu-id="17e0a-202">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
