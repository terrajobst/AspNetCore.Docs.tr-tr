---
title: Geliştirme zamanı IIS, ASP.NET Core için Visual Studio desteği
author: shirhatti
description: IIS Windows Server'da çalışan ASP.NET Core uygulamalarında hata ayıklamaya yönelik destek keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 11/30/2018
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 51375e6a6bb25a469d467ca97a151abd305c1ece
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862388"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="cf4a0-103">Geliştirme zamanı IIS, ASP.NET Core için Visual Studio desteği</span><span class="sxs-lookup"><span data-stu-id="cf4a0-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="cf4a0-104">Tarafından [Sourabh Shirhatti](https://twitter.com/sshirhatti) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="cf4a0-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="cf4a0-105">Bu makalede [Visual Studio](https://www.visualstudio.com/vs/) IIS Windows Server üzerinde çalışan ASP.NET Core uygulamaları hata ayıklama için destek.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="cf4a0-106">Bu konu başlığı altında bu özelliği etkinleştirmek ve bir proje ayarlama gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cf4a0-107">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="cf4a0-107">Prerequisites</span></span>

* [<span data-ttu-id="cf4a0-108">Windows için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cf4a0-108">Visual Studio for Windows</span></span>](https://www.microsoft.com/net/download/windows)
* <span data-ttu-id="cf4a0-109">**ASP.NET ve web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="cf4a0-109">**ASP.NET and web development** workload</span></span>
* <span data-ttu-id="cf4a0-110">**.NET core çoklu platform geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="cf4a0-110">**.NET Core cross-platform development** workload</span></span>
* <span data-ttu-id="cf4a0-111">X.509 güvenlik sertifikası</span><span class="sxs-lookup"><span data-stu-id="cf4a0-111">X.509 security certificate</span></span>

## <a name="enable-iis"></a><span data-ttu-id="cf4a0-112">IIS'yi etkinleştirin</span><span class="sxs-lookup"><span data-stu-id="cf4a0-112">Enable IIS</span></span>

1. <span data-ttu-id="cf4a0-113">Gidin **Denetim Masası** > **programlar** > **programlar ve Özellikler** > **kapatma Windows özellikleri hakkında ya da kapalı** (ekranın sol).</span><span class="sxs-lookup"><span data-stu-id="cf4a0-113">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="cf4a0-114">Seçin **Internet Information Services** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-114">Select the **Internet Information Services** check box.</span></span>

![Windows özelliklerini gösteren Internet Information Services onay kutusunu işaretli IIS özelliklerinden bazıları etkinleştirildiğini belirten bir siyah bir kare olarak (bir onay işareti)](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="cf4a0-116">IIS yüklemesi, sistemin yeniden başlatılması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-116">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="cf4a0-117">IIS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="cf4a0-117">Configure IIS</span></span>

<span data-ttu-id="cf4a0-118">IIS, aşağıdaki ile yapılandırılmış bir Web sitesinde sahip olmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="cf4a0-118">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="cf4a0-119">Uygulama başlatma profili URL'si ana bilgisayar adıyla eşleşen bir konak adı.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-119">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="cf4a0-120">Atanan bir sertifika ile 443 numaralı bağlantı noktası için bağlama.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-120">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="cf4a0-121">Örneğin, **ana bilgisayar adı** eklenen bir Web sitesi, "localhost" (başlatma profili de kullanır "localhost" Bu konunun ilerleyen bölümlerinde) olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-121">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="cf4a0-122">Bağlantı noktası "443" (HTTPS) ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-122">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="cf4a0-123">**IIS Express geliştirme sertifikası** çalışır, Web sitesi, ancak herhangi bir geçerli sertifika atanır:</span><span class="sxs-lookup"><span data-stu-id="cf4a0-123">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![Bağlama kümesi localhost için atanan bir sertifika ile bağlantı noktası 443 üzerinde gösteren bir IIS Web sitesi penceresini ekleyin.](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="cf4a0-125">IIS yüklemesi zaten varsa, bir **varsayılan Web sitesi** uygulamanın başlatma profili URL'si ana bilgisayar adıyla eşleşen bir ana bilgisayar adına sahip:</span><span class="sxs-lookup"><span data-stu-id="cf4a0-125">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="cf4a0-126">Bağlantı noktası 443 (HTTPS) için bir bağlantı noktası bağlaması ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-126">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="cf4a0-127">Geçerli bir sertifika Web sitesine atayın.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-127">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="cf4a0-128">Visual Studio'da geliştirme zamanı IIS desteğini etkinleştirin</span><span class="sxs-lookup"><span data-stu-id="cf4a0-128">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="cf4a0-129">Visual Studio Yükleyicisi'ni başlatın.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-129">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="cf4a0-130">Seçin **geliştirme zamanı IIS desteğini** bileşeni.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-130">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="cf4a0-131">Bileşen isteğe bağlı olarak, listelenen **özeti** panelinde **ASP.NET ve web geliştirme** iş yükü.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-131">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="cf4a0-132">Bileşeni yükler [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module), ASP.NET Core uygulamaları ile IIS çalıştırmak için gereken yerel bir IIS modül olduğu.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-132">The component installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps with IIS.</span></span>

![Visual Studio özellikleri değiştirerek: iş yüklerini sekmesi seçilmiştir.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="cf4a0-136">Proje yapılandırma</span><span class="sxs-lookup"><span data-stu-id="cf4a0-136">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="cf4a0-137">HTTPS yeniden yönlendirmesi</span><span class="sxs-lookup"><span data-stu-id="cf4a0-137">HTTPS redirection</span></span>

<span data-ttu-id="cf4a0-138">Yeni bir proje için onay kutusunu işaretleyin **HTTPS için Yapılandır** içinde **yeni ASP.NET Core Web uygulaması** penceresi:</span><span class="sxs-lookup"><span data-stu-id="cf4a0-138">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![Yeni ASP.NET Core Web uygulaması penceresi için HTTPS onay kutusu seçili yapılandırma ile.](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="cf4a0-140">Varolan bir projede, HTTPS yeniden yönlendirmesi Ara yazılımında kullanın `Startup.Configure` çağırarak [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="cf4a0-140">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();

    app.UseMvc();
}
```

### <a name="iis-launch-profile"></a><span data-ttu-id="cf4a0-141">IIS başlatma profili</span><span class="sxs-lookup"><span data-stu-id="cf4a0-141">IIS launch profile</span></span>

<span data-ttu-id="cf4a0-142">Geliştirme zamanı IIS desteği eklemek için yeni bir başlatma profili oluşturun:</span><span class="sxs-lookup"><span data-stu-id="cf4a0-142">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="cf4a0-143">İçin **profili**seçin **yeni** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-143">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="cf4a0-144">Profili, "IIS" açılan pencerede adlandırın.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-144">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="cf4a0-145">Seçin **Tamam** profili oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-145">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="cf4a0-146">İçin **başlatma** ayarını seçin **IIS** listeden.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-146">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="cf4a0-147">Onay kutusunu seçin **tarayıcıyı başlatın** ve uç nokta URL'sini sağlayın.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-147">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="cf4a0-148">HTTPS protokolünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-148">Use the HTTPS protocol.</span></span> <span data-ttu-id="cf4a0-149">Bu örnekte `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="cf4a0-150">İçinde **ortam değişkenlerini** bölümünden **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-150">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="cf4a0-151">Bir anahtar ile bir ortam değişkeni sağlamak `ASPNETCORE_ENVIRONMENT` değerini `Development`.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-151">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="cf4a0-152">İçinde **Web sunucusu ayarlarını** alanı ayarlama **uygulama URL'si**.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-152">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="cf4a0-153">Bu örnekte `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-153">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="cf4a0-154">Profili kaydedin.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-154">Save the profile.</span></span>

![Hata ayıklama sekmesi seçili Proje Özellikleri penceresi.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="cf4a0-159">Alternatif olarak, el ile eklemek için bir başlatma profili [launchSettings.json](http://json.schemastore.org/launchsettings) uygulama dosyasında:</span><span class="sxs-lookup"><span data-stu-id="cf4a0-159">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

## <a name="run-the-project"></a><span data-ttu-id="cf4a0-160">Projeyi Çalıştır</span><span class="sxs-lookup"><span data-stu-id="cf4a0-160">Run the project</span></span>

<span data-ttu-id="cf4a0-161">Visual Studio'da:</span><span class="sxs-lookup"><span data-stu-id="cf4a0-161">In Visual Studio:</span></span>

* <span data-ttu-id="cf4a0-162">Aşağı açılan listede derleme yapılandırmasını değerine ayarlandığını onaylayın **hata ayıklama**.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-162">Confirm that the build configuration drop-down list is set to **Debug**.</span></span>
* <span data-ttu-id="cf4a0-163">Çalıştır düğmesini kümesine **IIS** profil ve uygulamayı başlatmak için düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-163">Set the Run button to the **IIS** profile and select the button to start the app.</span></span>

![VS araç çubuğundaki Çalıştır düğmesini IIS profiline yayın olarak ayarlanmış derleme yapılandırması açılan liste ile ayarlanır.](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="cf4a0-165">Visual Studio'yu yönetici olarak çalıştırarak değil, yeniden başlatma isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-165">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="cf4a0-166">İstenirse, Visual Studio'yu yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-166">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="cf4a0-167">Güvenilmeyen bir geliştirme sertifikası kullanıyorsanız, tarayıcı Güvenilmeyen sertifika için bir özel durum oluşturmak gerekli kılabiliriz.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-167">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="cf4a0-168">Yayın hata ayıklama derleme yapılandırması ile [yalnızca kendi kodum](/visualstudio/debugger/just-my-code) ve düzeyi düşürülmüş bir deneyimle derleyici iyileştirmeleri sonuçları.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-168">Debugging a Release build configuration with [Just My Code](/visualstudio/debugger/just-my-code) and compiler optimizations results in a degraded experience.</span></span> <span data-ttu-id="cf4a0-169">Örneğin, kesme noktaları isabet değildir.</span><span class="sxs-lookup"><span data-stu-id="cf4a0-169">For example, break points aren't hit.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cf4a0-170">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="cf4a0-170">Additional resources</span></span>

* [<span data-ttu-id="cf4a0-171">Windows IIS üzerinde ASP.NET Core barındırma</span><span class="sxs-lookup"><span data-stu-id="cf4a0-171">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="cf4a0-172">ASP.NET Core modülü için giriş</span><span class="sxs-lookup"><span data-stu-id="cf4a0-172">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="cf4a0-173">ASP.NET Core Module yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="cf4a0-173">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="cf4a0-174">HTTPS'yi Zorunlu Kılma</span><span class="sxs-lookup"><span data-stu-id="cf4a0-174">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
