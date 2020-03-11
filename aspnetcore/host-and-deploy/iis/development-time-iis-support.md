---
title: ASP.NET Core için Visual Studio 'da geliştirme zamanı IIS desteği
author: rick-anderson
description: Windows Server 'da IIS ile çalışırken ASP.NET Core uygulamalarda hata ayıklama desteğini bulur.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: f87a1d8cf41248f14932908c0633f98a7198853f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664047"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="58ef2-103">ASP.NET Core için Visual Studio 'da geliştirme zamanı IIS desteği</span><span class="sxs-lookup"><span data-stu-id="58ef2-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="58ef2-104">, [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="58ef2-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="58ef2-105">Bu makalede, Windows Server 'da IIS ile çalışan ASP.NET Core hata ayıklama için [Visual Studio](https://visualstudio.microsoft.com) desteği açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="58ef2-105">This article describes [Visual Studio](https://visualstudio.microsoft.com) support for debugging ASP.NET Core apps running with IIS on Windows Server.</span></span> <span data-ttu-id="58ef2-106">Bu konu başlığı altında, bu senaryonun etkinleştirilmesi ve bir projenin kurulması anlatılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="58ef2-106">This topic walks through enabling this scenario and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="58ef2-107">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="58ef2-107">Prerequisites</span></span>

* [<span data-ttu-id="58ef2-108">Windows için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="58ef2-108">Visual Studio for Windows</span></span>](https://visualstudio.microsoft.com/downloads/)
* <span data-ttu-id="58ef2-109">**ASP.net ve Web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="58ef2-109">**ASP.NET and web development** workload</span></span>
* <span data-ttu-id="58ef2-110">**.NET Core platformlar arası geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="58ef2-110">**.NET Core cross-platform development** workload</span></span>
* <span data-ttu-id="58ef2-111">X. 509.440 güvenlik sertifikası (HTTPS desteği için)</span><span class="sxs-lookup"><span data-stu-id="58ef2-111">X.509 security certificate (for HTTPS support)</span></span>

## <a name="enable-iis"></a><span data-ttu-id="58ef2-112">IIS 'yi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="58ef2-112">Enable IIS</span></span>

1. <span data-ttu-id="58ef2-113">Windows 'da, **Programlar ve özellikler** **> programlar** ve Özellikler ' **> e gidin** > **Windows özelliklerini açın veya kapatın** (ekranın sol tarafında).</span><span class="sxs-lookup"><span data-stu-id="58ef2-113">In Windows, navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="58ef2-114">**Internet Information Services** onay kutusunu seçin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-114">Select the **Internet Information Services** check box.</span></span> <span data-ttu-id="58ef2-115">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-115">Select **OK**.</span></span>

<span data-ttu-id="58ef2-116">IIS yüklemesi için sistemin yeniden başlatılması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="58ef2-116">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="58ef2-117">IIS 'yi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="58ef2-117">Configure IIS</span></span>

<span data-ttu-id="58ef2-118">IIS 'nin aşağıdaki ile yapılandırılmış bir Web sitesine sahip olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="58ef2-118">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="58ef2-119">**Ana bilgisayar adı** &ndash; genellikle **varsayılan Web sitesi** `localhost`**ana bilgisayar adıyla** kullanılır.</span><span class="sxs-lookup"><span data-stu-id="58ef2-119">**Host name** &ndash; Typically, the **Default Web Site** is used with a **Host name** of `localhost`.</span></span> <span data-ttu-id="58ef2-120">Ancak, benzersiz bir ana bilgisayar adına sahip geçerli bir IIS Web sitesi çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="58ef2-120">However, any valid IIS website with a unique host name works.</span></span>
* <span data-ttu-id="58ef2-121">**Site bağlama**</span><span class="sxs-lookup"><span data-stu-id="58ef2-121">**Site Binding**</span></span>
  * <span data-ttu-id="58ef2-122">HTTPS gerektiren uygulamalar için, sertifika ile 443 numaralı bağlantı noktasına bir bağlama oluşturun.</span><span class="sxs-lookup"><span data-stu-id="58ef2-122">For apps that require HTTPS, create a binding to port 443 with a certificate.</span></span> <span data-ttu-id="58ef2-123">Genellikle **IIS Express geliştirme sertifikası** kullanılır, ancak geçerli bir sertifika çalışıyor olur.</span><span class="sxs-lookup"><span data-stu-id="58ef2-123">Typically, the **IIS Express Development Certificate** is used, but any valid certificate works.</span></span>
  * <span data-ttu-id="58ef2-124">HTTP kullanan uygulamalar için, 80 veya yeni bir site için bağlantı noktası 80 ' e bir bağlama oluşturmak için bir bağlamanın mevcut olduğunu onaylayın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-124">For apps that use HTTP, confirm the existence of a binding to post 80 or create a binding to port 80 for a new site.</span></span>
  * <span data-ttu-id="58ef2-125">HTTP veya HTTPS için tek bir bağlama kullanın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-125">Use a single binding for either HTTP or HTTPS.</span></span> <span data-ttu-id="58ef2-126">**Aynı anda hem HTTP hem de HTTPS bağlantı noktalarına bağlama desteklenmez.**</span><span class="sxs-lookup"><span data-stu-id="58ef2-126">**Binding to both HTTP and HTTPS ports simultaneously isn't supported.**</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="58ef2-127">Visual Studio 'da geliştirme zamanı IIS desteğini etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="58ef2-127">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="58ef2-128">Visual Studio yükleyicisi 'ni başlatın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-128">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="58ef2-129">IIS geliştirme zamanı desteği için kullanmayı planladığınız Visual Studio yüklemesi için **Değiştir** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-129">Select **Modify** for the Visual Studio installation that you plan to use for IIS development-time support.</span></span>
1. <span data-ttu-id="58ef2-130">**ASP.net ve Web geliştirme** iş yükü için **geliştirme zamanı IIS destek** bileşeni ' ni bulun ve yükler.</span><span class="sxs-lookup"><span data-stu-id="58ef2-130">For the **ASP.NET and web development** workload, locate and install the **Development time IIS support** component.</span></span>

   <span data-ttu-id="58ef2-131">Bileşen, iş yüklerinin sağındaki **Yükleme ayrıntıları** panelinde, **geliştirme zamanı IIS desteği** altındaki **isteğe bağlı** bölümde listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="58ef2-131">The component is listed in the **Optional** section under **Development time IIS support** in the **Installation details** panel to the right of the workloads.</span></span> <span data-ttu-id="58ef2-132">Bileşen, IIS ile ASP.NET Core uygulamaları çalıştırmak için gerekli yerel bir IIS modülü olan [ASP.NET Core modülünü](xref:host-and-deploy/aspnet-core-module)yüklüyor.</span><span class="sxs-lookup"><span data-stu-id="58ef2-132">The component installs the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps with IIS.</span></span>

## <a name="configure-the-project"></a><span data-ttu-id="58ef2-133">Projeyi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="58ef2-133">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="58ef2-134">HTTPS yönlendirmesi</span><span class="sxs-lookup"><span data-stu-id="58ef2-134">HTTPS redirection</span></span>

<span data-ttu-id="58ef2-135">HTTPS gerektiren yeni bir proje için **yeni ASP.NET Core Web uygulaması oluşturma** penceresinde **https için yapılandırmak** üzere onay kutusunu seçin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-135">For a new project that requires HTTPS, select the check box to **Configure for HTTPS** in the **Create a new ASP.NET Core Web Application** window.</span></span> <span data-ttu-id="58ef2-136">Onay kutusunun belirlenmesi, uygulama oluşturulduğunda [https yeniden yönlendirme ve HSTS ara yazılımı](xref:security/enforcing-ssl) ekler.</span><span class="sxs-lookup"><span data-stu-id="58ef2-136">Selecting the check box adds [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) to the app when it's created.</span></span>

<span data-ttu-id="58ef2-137">HTTPS gerektiren mevcut bir proje için, `Startup.Configure`'de HTTPS yeniden yönlendirme ve HSTS ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-137">For an existing project that requires HTTPS, use HTTPS Redirection and HSTS Middleware in `Startup.Configure`.</span></span> <span data-ttu-id="58ef2-138">Daha fazla bilgi için bkz. <xref:security/enforcing-ssl>.</span><span class="sxs-lookup"><span data-stu-id="58ef2-138">For more information, see <xref:security/enforcing-ssl>.</span></span>

<span data-ttu-id="58ef2-139">HTTP kullanan bir proje için, [https yeniden yönlendirme ve HSTS ara yazılımı](xref:security/enforcing-ssl) uygulamaya eklenmez.</span><span class="sxs-lookup"><span data-stu-id="58ef2-139">For a project that uses HTTP, [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) aren't added to the app.</span></span> <span data-ttu-id="58ef2-140">Uygulama yapılandırması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="58ef2-140">No app configuration is required.</span></span>

### <a name="iis-launch-profile"></a><span data-ttu-id="58ef2-141">IIS başlatma profili</span><span class="sxs-lookup"><span data-stu-id="58ef2-141">IIS launch profile</span></span>

<span data-ttu-id="58ef2-142">Geliştirme zamanı IIS desteği eklemek için yeni bir başlatma profili oluşturun:</span><span class="sxs-lookup"><span data-stu-id="58ef2-142">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="58ef2-143">**Çözüm Gezgini**projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-143">Right-click the project in **Solution Explorer**.</span></span> <span data-ttu-id="58ef2-144">**Özellikler**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-144">Select **Properties**.</span></span> <span data-ttu-id="58ef2-145">**Hata Ayıkla** sekmesini açın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-145">Open the **Debug** tab.</span></span>
1. <span data-ttu-id="58ef2-146">**Profil**için **Yeni** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-146">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="58ef2-147">Profili, açılan pencerede "IIS" olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-147">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="58ef2-148">Profili oluşturmak için **Tamam ' ı** seçin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-148">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="58ef2-149">**Başlatma** ayarı Için listeden **IIS** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-149">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="58ef2-150">**Başlat tarayıcısı** onay kutusunu seçin ve uç nokta URL 'sini sağlayın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-150">Select the check box for **Launch browser** and provide the endpoint URL.</span></span>

   <span data-ttu-id="58ef2-151">Uygulama HTTPS gerektirdiğinde, bir HTTPS uç noktası (`https://`) kullanın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-151">When the app requires HTTPS, use an HTTPS endpoint (`https://`).</span></span> <span data-ttu-id="58ef2-152">HTTP için bir HTTP (`http://`) uç noktası kullanın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-152">For HTTP, use an HTTP (`http://`) endpoint.</span></span>

   <span data-ttu-id="58ef2-153">[Daha önce belirtilen IIS yapılandırmasıyla](#configure-iis)aynı ana bilgisayar adını ve bağlantı noktasını, genellikle `localhost`sağlayın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-153">Provide the same host name and port as the [IIS configuration specified earlier uses](#configure-iis), typically `localhost`.</span></span>

   <span data-ttu-id="58ef2-154">URL 'nin sonundaki uygulamanın adını belirtin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-154">Provide the name of the app at the end of the URL.</span></span>

   <span data-ttu-id="58ef2-155">Örneğin, `https://localhost/WebApplication1` (HTTPS) veya `http://localhost/WebApplication1` (HTTP) geçerli uç nokta URL 'Lardır.</span><span class="sxs-lookup"><span data-stu-id="58ef2-155">For example, `https://localhost/WebApplication1` (HTTPS) or `http://localhost/WebApplication1` (HTTP) are valid endpoint URLs.</span></span>
1. <span data-ttu-id="58ef2-156">**Ortam değişkenleri** bölümünde **Ekle** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-156">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="58ef2-157">`ASPNETCORE_ENVIRONMENT` **adı** ve `Development`**değeri** olan bir ortam değişkeni sağlayın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-157">Provide an environment variable with a **Name** of `ASPNETCORE_ENVIRONMENT` and a **Value** of `Development`.</span></span>
1. <span data-ttu-id="58ef2-158">**Web sunucusu ayarları** alanında, **Uygulama URL** 'sini **başlatma tarayıcısı** uç noktası URL 'si için kullanılan aynı değere ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-158">In the **Web Server Settings** area, set the **App URL** to the same value used for the **Launch browser** endpoint URL.</span></span>
1. <span data-ttu-id="58ef2-159">Visual Studio 2019 veya sonraki sürümlerde **barındırma modeli** ayarı için, proje tarafından kullanılan barındırma modelini kullanmak üzere **varsayılan** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-159">For the **Hosting Model** setting in Visual Studio 2019 or later, select **Default** to use the hosting model used by the project.</span></span> <span data-ttu-id="58ef2-160">Proje, proje dosyasında `<AspNetCoreHostingModel>` özelliğini ayarlarsa, özelliğin değeri (`InProcess` veya `OutOfProcess`) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="58ef2-160">If the project sets the `<AspNetCoreHostingModel>` property in its project file, the value of the property (`InProcess` or `OutOfProcess`) is used.</span></span> <span data-ttu-id="58ef2-161">Özellik mevcut değilse, uygulamanın varsayılan barındırma modeli, işlem içi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="58ef2-161">If the property isn't present, the default hosting model of the app is used, which is in-process.</span></span> <span data-ttu-id="58ef2-162">Uygulama, uygulamanın normal barındırma modelinden farklı bir açık barındırma modeli ayarı gerektiriyorsa, **barındırma modelini** `In Process` veya `Out Of Process` gerektiği şekilde ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-162">If the app requires an explicit hosting model setting different from the app's normal hosting model, set the **Hosting Model** to either `In Process` or `Out Of Process` as needed.</span></span>
1. <span data-ttu-id="58ef2-163">Profili kaydedin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-163">Save the profile.</span></span>

<span data-ttu-id="58ef2-164">Visual Studio kullanmadığınız durumlarda, *Özellikler* klasöründeki [launchsettings. JSON](https://json.schemastore.org/launchsettings) dosyasına el ile bir başlatma profili ekleyin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-164">When not using Visual Studio, manually add a launch profile to the [launchSettings.json](https://json.schemastore.org/launchsettings) file in the *Properties* folder.</span></span> <span data-ttu-id="58ef2-165">Aşağıdaki örnek, HTTPS protokolünü kullanmak için profili yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="58ef2-165">The following example configures the profile to use the HTTPS protocol:</span></span>

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

<span data-ttu-id="58ef2-166">`applicationUrl` ve `launchUrl` uç noktalarının eşleştiğini ve IIS bağlama yapılandırmasıyla aynı Protokolü (HTTP veya HTTPS) kullandığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-166">Confirm that the `applicationUrl` and `launchUrl` endpoints match and use the same protocol as the IIS binding configuration, either HTTP or HTTPS.</span></span>

## <a name="run-the-project"></a><span data-ttu-id="58ef2-167">Projeyi çalıştırma</span><span class="sxs-lookup"><span data-stu-id="58ef2-167">Run the project</span></span>

<span data-ttu-id="58ef2-168">Visual Studio 'Yu yönetici olarak çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="58ef2-168">Run Visual Studio as an administrator:</span></span>

* <span data-ttu-id="58ef2-169">Derleme yapılandırması açılır listesinin **hata ayıklama**olarak ayarlandığını onaylayın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-169">Confirm that the build configuration drop-down list is set to **Debug**.</span></span>
* <span data-ttu-id="58ef2-170">[Hata ayıklamayı Başlat düğmesini](/visualstudio/debugger/debugger-feature-tour) **IIS** profiline ayarlayın ve uygulamayı başlatmak için düğmeyi seçin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-170">Set the [Start Debugging button](/visualstudio/debugger/debugger-feature-tour) to the **IIS** profile and select the button to start the app.</span></span>

<span data-ttu-id="58ef2-171">Visual Studio, yönetici olarak çalışmıyorsa bir yeniden başlatma isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="58ef2-171">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="58ef2-172">İstenirse, Visual Studio 'Yu yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-172">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="58ef2-173">Güvenilmeyen bir geliştirme sertifikası kullanılırsa, tarayıcı güvenilmeyen sertifika için bir özel durum oluşturmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="58ef2-173">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="58ef2-174">[Yalnızca kendi kodum](/visualstudio/debugger/just-my-code) ve derleyici Iyileştirmeleriyle yayın derleme yapılandırması hata ayıklaması, düzeyi düşürülmüş bir deneyimle sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="58ef2-174">Debugging a Release build configuration with [Just My Code](/visualstudio/debugger/just-my-code) and compiler optimizations results in a degraded experience.</span></span> <span data-ttu-id="58ef2-175">Örneğin, kesme noktaları isabet edilmez.</span><span class="sxs-lookup"><span data-stu-id="58ef2-175">For example, break points aren't hit.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="58ef2-176">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="58ef2-176">Additional resources</span></span>

* [<span data-ttu-id="58ef2-177">IIS 'de IIS Yöneticisi 'Ni kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="58ef2-177">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* <xref:security/enforcing-ssl>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="58ef2-178">Bu makalede, Windows Server 'da IIS ile çalışan ASP.NET Core hata ayıklama için [Visual Studio](https://visualstudio.microsoft.com) desteği açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="58ef2-178">This article describes [Visual Studio](https://visualstudio.microsoft.com) support for debugging ASP.NET Core apps running with IIS on Windows Server.</span></span> <span data-ttu-id="58ef2-179">Bu konu başlığı altında, bu senaryonun etkinleştirilmesi ve bir projenin kurulması anlatılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="58ef2-179">This topic walks through enabling this scenario and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="58ef2-180">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="58ef2-180">Prerequisites</span></span>

* [<span data-ttu-id="58ef2-181">Windows için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="58ef2-181">Visual Studio for Windows</span></span>](https://visualstudio.microsoft.com/downloads/)
* <span data-ttu-id="58ef2-182">**ASP.net ve Web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="58ef2-182">**ASP.NET and web development** workload</span></span>
* <span data-ttu-id="58ef2-183">**.NET Core platformlar arası geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="58ef2-183">**.NET Core cross-platform development** workload</span></span>
* <span data-ttu-id="58ef2-184">X. 509.440 güvenlik sertifikası (HTTPS desteği için)</span><span class="sxs-lookup"><span data-stu-id="58ef2-184">X.509 security certificate (for HTTPS support)</span></span>

## <a name="enable-iis"></a><span data-ttu-id="58ef2-185">IIS 'yi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="58ef2-185">Enable IIS</span></span>

1. <span data-ttu-id="58ef2-186">Windows 'da, **Programlar ve özellikler** **> programlar** ve Özellikler ' **> e gidin** > **Windows özelliklerini açın veya kapatın** (ekranın sol tarafında).</span><span class="sxs-lookup"><span data-stu-id="58ef2-186">In Windows, navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="58ef2-187">**Internet Information Services** onay kutusunu seçin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-187">Select the **Internet Information Services** check box.</span></span> <span data-ttu-id="58ef2-188">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-188">Select **OK**.</span></span>

<span data-ttu-id="58ef2-189">IIS yüklemesi için sistemin yeniden başlatılması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="58ef2-189">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="58ef2-190">IIS 'yi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="58ef2-190">Configure IIS</span></span>

<span data-ttu-id="58ef2-191">IIS 'nin aşağıdaki ile yapılandırılmış bir Web sitesine sahip olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="58ef2-191">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="58ef2-192">**Ana bilgisayar adı** &ndash; genellikle **varsayılan Web sitesi** `localhost`**ana bilgisayar adıyla** kullanılır.</span><span class="sxs-lookup"><span data-stu-id="58ef2-192">**Host name** &ndash; Typically, the **Default Web Site** is used with a **Host name** of `localhost`.</span></span> <span data-ttu-id="58ef2-193">Ancak, benzersiz bir ana bilgisayar adına sahip geçerli bir IIS Web sitesi çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="58ef2-193">However, any valid IIS website with a unique host name works.</span></span>
* <span data-ttu-id="58ef2-194">**Site bağlama**</span><span class="sxs-lookup"><span data-stu-id="58ef2-194">**Site Binding**</span></span>
  * <span data-ttu-id="58ef2-195">HTTPS gerektiren uygulamalar için, sertifika ile 443 numaralı bağlantı noktasına bir bağlama oluşturun.</span><span class="sxs-lookup"><span data-stu-id="58ef2-195">For apps that require HTTPS, create a binding to port 443 with a certificate.</span></span> <span data-ttu-id="58ef2-196">Genellikle **IIS Express geliştirme sertifikası** kullanılır, ancak geçerli bir sertifika çalışıyor olur.</span><span class="sxs-lookup"><span data-stu-id="58ef2-196">Typically, the **IIS Express Development Certificate** is used, but any valid certificate works.</span></span>
  * <span data-ttu-id="58ef2-197">HTTP kullanan uygulamalar için, 80 veya yeni bir site için bağlantı noktası 80 ' e bir bağlama oluşturmak için bir bağlamanın mevcut olduğunu onaylayın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-197">For apps that use HTTP, confirm the existence of a binding to post 80 or create a binding to port 80 for a new site.</span></span>
  * <span data-ttu-id="58ef2-198">HTTP veya HTTPS için tek bir bağlama kullanın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-198">Use a single binding for either HTTP or HTTPS.</span></span> <span data-ttu-id="58ef2-199">**Aynı anda hem HTTP hem de HTTPS bağlantı noktalarına bağlama desteklenmez.**</span><span class="sxs-lookup"><span data-stu-id="58ef2-199">**Binding to both HTTP and HTTPS ports simultaneously isn't supported.**</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="58ef2-200">Visual Studio 'da geliştirme zamanı IIS desteğini etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="58ef2-200">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="58ef2-201">Visual Studio yükleyicisi 'ni başlatın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-201">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="58ef2-202">IIS geliştirme zamanı desteği için kullanmayı planladığınız Visual Studio yüklemesi için **Değiştir** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-202">Select **Modify** for the Visual Studio installation that you plan to use for IIS development-time support.</span></span>
1. <span data-ttu-id="58ef2-203">**ASP.net ve Web geliştirme** iş yükü için **geliştirme zamanı IIS destek** bileşeni ' ni bulun ve yükler.</span><span class="sxs-lookup"><span data-stu-id="58ef2-203">For the **ASP.NET and web development** workload, locate and install the **Development time IIS support** component.</span></span>

   <span data-ttu-id="58ef2-204">Bileşen, iş yüklerinin sağındaki **Yükleme ayrıntıları** panelinde, **geliştirme zamanı IIS desteği** altındaki **isteğe bağlı** bölümde listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="58ef2-204">The component is listed in the **Optional** section under **Development time IIS support** in the **Installation details** panel to the right of the workloads.</span></span> <span data-ttu-id="58ef2-205">Bileşen, IIS ile ASP.NET Core uygulamaları çalıştırmak için gerekli yerel bir IIS modülü olan [ASP.NET Core modülünü](xref:host-and-deploy/aspnet-core-module)yüklüyor.</span><span class="sxs-lookup"><span data-stu-id="58ef2-205">The component installs the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps with IIS.</span></span>

## <a name="configure-the-project"></a><span data-ttu-id="58ef2-206">Projeyi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="58ef2-206">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="58ef2-207">HTTPS yönlendirmesi</span><span class="sxs-lookup"><span data-stu-id="58ef2-207">HTTPS redirection</span></span>

<span data-ttu-id="58ef2-208">HTTPS gerektiren yeni bir proje için **yeni ASP.NET Core Web uygulaması oluşturma** penceresinde **https için yapılandırmak** üzere onay kutusunu seçin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-208">For a new project that requires HTTPS, select the check box to **Configure for HTTPS** in the **Create a new ASP.NET Core Web Application** window.</span></span> <span data-ttu-id="58ef2-209">Onay kutusunun belirlenmesi, uygulama oluşturulduğunda [https yeniden yönlendirme ve HSTS ara yazılımı](xref:security/enforcing-ssl) ekler.</span><span class="sxs-lookup"><span data-stu-id="58ef2-209">Selecting the check box adds [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) to the app when it's created.</span></span>

<span data-ttu-id="58ef2-210">HTTPS gerektiren mevcut bir proje için, `Startup.Configure`'de HTTPS yeniden yönlendirme ve HSTS ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-210">For an existing project that requires HTTPS, use HTTPS Redirection and HSTS Middleware in `Startup.Configure`.</span></span> <span data-ttu-id="58ef2-211">Daha fazla bilgi için bkz. <xref:security/enforcing-ssl>.</span><span class="sxs-lookup"><span data-stu-id="58ef2-211">For more information, see <xref:security/enforcing-ssl>.</span></span>

<span data-ttu-id="58ef2-212">HTTP kullanan bir proje için, [https yeniden yönlendirme ve HSTS ara yazılımı](xref:security/enforcing-ssl) uygulamaya eklenmez.</span><span class="sxs-lookup"><span data-stu-id="58ef2-212">For a project that uses HTTP, [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) aren't added to the app.</span></span> <span data-ttu-id="58ef2-213">Uygulama yapılandırması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="58ef2-213">No app configuration is required.</span></span>

### <a name="iis-launch-profile"></a><span data-ttu-id="58ef2-214">IIS başlatma profili</span><span class="sxs-lookup"><span data-stu-id="58ef2-214">IIS launch profile</span></span>

<span data-ttu-id="58ef2-215">Geliştirme zamanı IIS desteği eklemek için yeni bir başlatma profili oluşturun:</span><span class="sxs-lookup"><span data-stu-id="58ef2-215">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="58ef2-216">**Çözüm Gezgini**projeye sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-216">Right-click the project in **Solution Explorer**.</span></span> <span data-ttu-id="58ef2-217">**Özellikler**’i seçin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-217">Select **Properties**.</span></span> <span data-ttu-id="58ef2-218">**Hata Ayıkla** sekmesini açın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-218">Open the **Debug** tab.</span></span>
1. <span data-ttu-id="58ef2-219">**Profil**için **Yeni** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-219">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="58ef2-220">Profili, açılan pencerede "IIS" olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-220">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="58ef2-221">Profili oluşturmak için **Tamam ' ı** seçin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-221">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="58ef2-222">**Başlatma** ayarı Için listeden **IIS** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-222">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="58ef2-223">**Başlat tarayıcısı** onay kutusunu seçin ve uç nokta URL 'sini sağlayın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-223">Select the check box for **Launch browser** and provide the endpoint URL.</span></span>

   <span data-ttu-id="58ef2-224">Uygulama HTTPS gerektirdiğinde, bir HTTPS uç noktası (`https://`) kullanın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-224">When the app requires HTTPS, use an HTTPS endpoint (`https://`).</span></span> <span data-ttu-id="58ef2-225">HTTP için bir HTTP (`http://`) uç noktası kullanın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-225">For HTTP, use an HTTP (`http://`) endpoint.</span></span>

   <span data-ttu-id="58ef2-226">[Daha önce belirtilen IIS yapılandırmasıyla](#configure-iis)aynı ana bilgisayar adını ve bağlantı noktasını, genellikle `localhost`sağlayın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-226">Provide the same host name and port as the [IIS configuration specified earlier uses](#configure-iis), typically `localhost`.</span></span>

   <span data-ttu-id="58ef2-227">URL 'nin sonundaki uygulamanın adını belirtin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-227">Provide the name of the app at the end of the URL.</span></span>

   <span data-ttu-id="58ef2-228">Örneğin, `https://localhost/WebApplication1` (HTTPS) veya `http://localhost/WebApplication1` (HTTP) geçerli uç nokta URL 'Lardır.</span><span class="sxs-lookup"><span data-stu-id="58ef2-228">For example, `https://localhost/WebApplication1` (HTTPS) or `http://localhost/WebApplication1` (HTTP) are valid endpoint URLs.</span></span>
1. <span data-ttu-id="58ef2-229">**Ortam değişkenleri** bölümünde **Ekle** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-229">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="58ef2-230">`ASPNETCORE_ENVIRONMENT` **adı** ve `Development`**değeri** olan bir ortam değişkeni sağlayın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-230">Provide an environment variable with a **Name** of `ASPNETCORE_ENVIRONMENT` and a **Value** of `Development`.</span></span>
1. <span data-ttu-id="58ef2-231">**Web sunucusu ayarları** alanında, **Uygulama URL** 'sini **başlatma tarayıcısı** uç noktası URL 'si için kullanılan aynı değere ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-231">In the **Web Server Settings** area, set the **App URL** to the same value used for the **Launch browser** endpoint URL.</span></span>
1. <span data-ttu-id="58ef2-232">Visual Studio 2019 veya sonraki sürümlerde **barındırma modeli** ayarı için, proje tarafından kullanılan barındırma modelini kullanmak üzere **varsayılan** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-232">For the **Hosting Model** setting in Visual Studio 2019 or later, select **Default** to use the hosting model used by the project.</span></span> <span data-ttu-id="58ef2-233">Proje, proje dosyasında `<AspNetCoreHostingModel>` özelliğini ayarlarsa, özelliğin değeri (`InProcess` veya `OutOfProcess`) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="58ef2-233">If the project sets the `<AspNetCoreHostingModel>` property in its project file, the value of the property (`InProcess` or `OutOfProcess`) is used.</span></span> <span data-ttu-id="58ef2-234">Özellik mevcut değilse, uygulamanın varsayılan barındırma modeli kullanılır ve bu işlem, işlem dışı olur.</span><span class="sxs-lookup"><span data-stu-id="58ef2-234">If the property isn't present, the default hosting model of the app is used, which is out-of-process.</span></span> <span data-ttu-id="58ef2-235">Uygulama, uygulamanın normal barındırma modelinden farklı bir açık barındırma modeli ayarı gerektiriyorsa, **barındırma modelini** `In Process` veya `Out Of Process` gerektiği şekilde ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-235">If the app requires an explicit hosting model setting different from the app's normal hosting model, set the **Hosting Model** to either `In Process` or `Out Of Process` as needed.</span></span>
1. <span data-ttu-id="58ef2-236">Profili kaydedin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-236">Save the profile.</span></span>

<span data-ttu-id="58ef2-237">Visual Studio kullanmadığınız durumlarda, *Özellikler* klasöründeki [launchsettings. JSON](https://json.schemastore.org/launchsettings) dosyasına el ile bir başlatma profili ekleyin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-237">When not using Visual Studio, manually add a launch profile to the [launchSettings.json](https://json.schemastore.org/launchsettings) file in the *Properties* folder.</span></span> <span data-ttu-id="58ef2-238">Aşağıdaki örnek, HTTPS protokolünü kullanmak için profili yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="58ef2-238">The following example configures the profile to use the HTTPS protocol:</span></span>

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

<span data-ttu-id="58ef2-239">`applicationUrl` ve `launchUrl` uç noktalarının eşleştiğini ve IIS bağlama yapılandırmasıyla aynı Protokolü (HTTP veya HTTPS) kullandığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-239">Confirm that the `applicationUrl` and `launchUrl` endpoints match and use the same protocol as the IIS binding configuration, either HTTP or HTTPS.</span></span>

## <a name="run-the-project"></a><span data-ttu-id="58ef2-240">Projeyi çalıştırma</span><span class="sxs-lookup"><span data-stu-id="58ef2-240">Run the project</span></span>

<span data-ttu-id="58ef2-241">Visual Studio 'Yu yönetici olarak çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="58ef2-241">Run Visual Studio as an administrator:</span></span>

* <span data-ttu-id="58ef2-242">Derleme yapılandırması açılır listesinin **hata ayıklama**olarak ayarlandığını onaylayın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-242">Confirm that the build configuration drop-down list is set to **Debug**.</span></span>
* <span data-ttu-id="58ef2-243">[Hata ayıklamayı Başlat düğmesini](/visualstudio/debugger/debugger-feature-tour) **IIS** profiline ayarlayın ve uygulamayı başlatmak için düğmeyi seçin.</span><span class="sxs-lookup"><span data-stu-id="58ef2-243">Set the [Start Debugging button](/visualstudio/debugger/debugger-feature-tour) to the **IIS** profile and select the button to start the app.</span></span>

<span data-ttu-id="58ef2-244">Visual Studio, yönetici olarak çalışmıyorsa bir yeniden başlatma isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="58ef2-244">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="58ef2-245">İstenirse, Visual Studio 'Yu yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="58ef2-245">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="58ef2-246">Güvenilmeyen bir geliştirme sertifikası kullanılırsa, tarayıcı güvenilmeyen sertifika için bir özel durum oluşturmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="58ef2-246">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="58ef2-247">[Yalnızca kendi kodum](/visualstudio/debugger/just-my-code) ve derleyici Iyileştirmeleriyle yayın derleme yapılandırması hata ayıklaması, düzeyi düşürülmüş bir deneyimle sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="58ef2-247">Debugging a Release build configuration with [Just My Code](/visualstudio/debugger/just-my-code) and compiler optimizations results in a degraded experience.</span></span> <span data-ttu-id="58ef2-248">Örneğin, kesme noktaları isabet edilmez.</span><span class="sxs-lookup"><span data-stu-id="58ef2-248">For example, break points aren't hit.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="58ef2-249">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="58ef2-249">Additional resources</span></span>

* [<span data-ttu-id="58ef2-250">IIS 'de IIS Yöneticisi 'Ni kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="58ef2-250">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* <xref:security/enforcing-ssl>

::: moniker-end
