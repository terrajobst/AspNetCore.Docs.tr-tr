---
title: Geliştirme zamanı IIS Visual Studio'da ASP.NET Core için desteği
author: shirhatti
description: ASP.NET Core uygulamaları IIS Windows Server üzerinde çalışırken hata ayıklama desteği bulur.
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2018
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: eb8b4369d6d5434adbac187f59b18d7a2b80055c
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277660"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="92534-103">Geliştirme zamanı IIS Visual Studio'da ASP.NET Core için desteği</span><span class="sxs-lookup"><span data-stu-id="92534-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="92534-104">Tarafından [Sourabh Shirhatti](https://twitter.com/sshirhatti) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="92534-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="92534-105">Bu makalede [Visual Studio](https://www.visualstudio.com/vs/) hata ayıklama IIS Windows Server'da çalışan ASP.NET Core uygulamaları için destek.</span><span class="sxs-lookup"><span data-stu-id="92534-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="92534-106">Bu konu, bu özelliği etkinleştirmek ve bir projeyi ayarını size yol göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="92534-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="92534-107">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="92534-107">Prerequisites</span></span>

* [<span data-ttu-id="92534-108">Windows için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="92534-108">Visual Studio for Windows</span></span>](https://www.microsoft.com/net/download/windows)
* <span data-ttu-id="92534-109">**ASP.NET ve web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="92534-109">**ASP.NET and web development** workload</span></span>
* <span data-ttu-id="92534-110">**.NET core platformlar arası geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="92534-110">**.NET Core cross-platform development** workload</span></span>
* <span data-ttu-id="92534-111">X.509 güvenlik sertifikası</span><span class="sxs-lookup"><span data-stu-id="92534-111">X.509 security certificate</span></span>

## <a name="enable-iis"></a><span data-ttu-id="92534-112">IIS etkinleştir</span><span class="sxs-lookup"><span data-stu-id="92534-112">Enable IIS</span></span>

1. <span data-ttu-id="92534-113">Gidin **Denetim Masası** > **programları** > **programlar ve Özellikler** > **kapatma Windows özellikleri ya da kapalı** (sol tarafında ekranı).</span><span class="sxs-lookup"><span data-stu-id="92534-113">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="92534-114">Seçin **Internet Information Services** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="92534-114">Select the **Internet Information Services** check box.</span></span>

![Windows özelliklerini gösteren Internet Information Services onay kutusunu işaretli IIS özelliklerinden bazıları etkinleştirildiğini belirten bir siyah bir kare olarak (bir onay işareti)](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="92534-116">IIS yükleme sistemin yeniden başlatılması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="92534-116">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="92534-117">IIS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="92534-117">Configure IIS</span></span>

<span data-ttu-id="92534-118">IIS aşağıdaki ile yapılandırılmış bir Web sitesinde yüklü olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="92534-118">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="92534-119">Uygulamanın başlatma profil URL'si ana bilgisayar adıyla eşleşen bir ana bilgisayar adı.</span><span class="sxs-lookup"><span data-stu-id="92534-119">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="92534-120">Atanmış bir sertifikayla 443 numaralı bağlantı noktası için bağlama.</span><span class="sxs-lookup"><span data-stu-id="92534-120">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="92534-121">Örneğin, **ana bilgisayar adı** eklenen bir Web sitesi, "localhost (başlatma profil de kullanacak"localhost"bölümüne bakın)" olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="92534-121">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="92534-122">Bağlantı noktası "443" (HTTPS) ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="92534-122">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="92534-123">**IIS Express geliştirme sertifikası** Web sitesi, ancak herhangi bir geçerli sertifika works atanır:</span><span class="sxs-lookup"><span data-stu-id="92534-123">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![Bağlama kümesi localhost için bağlantı noktası 443 üzerinde atanmış bir sertifikayla gösteren IIS Web sitesi penceresi ekleyin.](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="92534-125">IIS yüklemesi zaten varsa bir **varsayılan Web sitesi** uygulamanın başlatma profil URL'si ana bilgisayar adıyla eşleşen bir ana bilgisayar adı ile:</span><span class="sxs-lookup"><span data-stu-id="92534-125">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="92534-126">Bağlantı noktası 443 (HTTPS) için bir bağlantı noktası bağlama ekleyin.</span><span class="sxs-lookup"><span data-stu-id="92534-126">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="92534-127">Geçerli bir sertifika Web sitesine atayın.</span><span class="sxs-lookup"><span data-stu-id="92534-127">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="92534-128">Visual Studio geliştirme zamanı IIS desteğini etkinleştir</span><span class="sxs-lookup"><span data-stu-id="92534-128">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="92534-129">Visual Studio yükleyicisi başlatın.</span><span class="sxs-lookup"><span data-stu-id="92534-129">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="92534-130">Seçin **IIS desteği geliştirme zamanı** bileşeni.</span><span class="sxs-lookup"><span data-stu-id="92534-130">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="92534-131">Bileşen, isteğe bağlı olarak listelenen **Özet** için panel **ASP.NET ve web geliştirme** iş yükü.</span><span class="sxs-lookup"><span data-stu-id="92534-131">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="92534-132">Bileşeni yükler [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module), yerel bir IIS modül ASP.NET Core uygulamaları IIS arkasındaki bir ters proxy yapılandırması çalıştırmak için gerekli olduğu.</span><span class="sxs-lookup"><span data-stu-id="92534-132">The component installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps behind IIS in a reverse proxy configuration.</span></span>

![Visual Studio özellikleri değiştirme: iş yükleri sekmesi seçilmiştir.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="92534-136">Projeyi Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="92534-136">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="92534-137">HTTPS yeniden yönlendirmesi</span><span class="sxs-lookup"><span data-stu-id="92534-137">HTTPS redirection</span></span>

<span data-ttu-id="92534-138">Yeni bir proje için onay kutusunu işaretleyin **HTTPS için Yapılandır** içinde **çekirdek yeni bir ASP.NET Web uygulaması** penceresi:</span><span class="sxs-lookup"><span data-stu-id="92534-138">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![Yeni ASP.NET çekirdek Web uygulaması penceresiyle HTTPS onay kutusu seçili yapılandırma.](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="92534-140">Varolan bir projede HTTPS yeniden yönlendirmesi Ara yazılımında kullanmak `Startup.Configure` çağırarak [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="92534-140">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

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

### <a name="iis-launch-profile"></a><span data-ttu-id="92534-141">Profil IIS Başlat</span><span class="sxs-lookup"><span data-stu-id="92534-141">IIS launch profile</span></span>

<span data-ttu-id="92534-142">Geliştirme zamanı IIS desteği eklemek için yeni bir başlatma profili oluşturun:</span><span class="sxs-lookup"><span data-stu-id="92534-142">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="92534-143">İçin **profil**seçin **yeni** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="92534-143">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="92534-144">Profil açılır penceresinde "IIS" adı.</span><span class="sxs-lookup"><span data-stu-id="92534-144">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="92534-145">Seçin **Tamam** profili oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="92534-145">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="92534-146">İçin **başlatma** ayarını seçin **IIS** listeden.</span><span class="sxs-lookup"><span data-stu-id="92534-146">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="92534-147">Onay kutusunu seçin **başlatma tarayıcı** ve uç nokta URL'sini belirtin.</span><span class="sxs-lookup"><span data-stu-id="92534-147">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="92534-148">HTTPS protokolünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="92534-148">Use the HTTPS protocol.</span></span> <span data-ttu-id="92534-149">Bu örnekte `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="92534-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="92534-150">İçinde **ortam değişkenleri** bölümünde, select **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="92534-150">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="92534-151">Bir ortam değişkeni bir anahtar sağlamak `ASPNETCORE_ENVIRONMENT` ve değerini `Development`.</span><span class="sxs-lookup"><span data-stu-id="92534-151">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="92534-152">İçinde **Web sunucusu ayarları** alanı ayarlamak **uygulama URL'si**.</span><span class="sxs-lookup"><span data-stu-id="92534-152">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="92534-153">Bu örnekte `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="92534-153">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="92534-154">Profil kaydedin.</span><span class="sxs-lookup"><span data-stu-id="92534-154">Save the profile.</span></span>

![Hata ayıklama sekmesi seçili proje Özellikler penceresi.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="92534-159">Alternatif olarak, bir başlatma profili el ile eklemeniz [launchSettings.json](http://json.schemastore.org/launchsettings) uygulama dosyasında:</span><span class="sxs-lookup"><span data-stu-id="92534-159">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

## <a name="run-the-project"></a><span data-ttu-id="92534-160">Projeyi çalıştırın</span><span class="sxs-lookup"><span data-stu-id="92534-160">Run the project</span></span>

<span data-ttu-id="92534-161">Çalıştır düğmesini VS Arabiriminde ayarlanırsa **IIS** profil ve uygulamayı başlatmak için düğmesini seçin:</span><span class="sxs-lookup"><span data-stu-id="92534-161">In the VS UI, set the Run button to the **IIS** profile and select the button to start the app:</span></span>

!["IIS" profili kümesi VS araç çubuğu düğmesi çalıştırın.](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="92534-163">Visual Studio, bir yönetici olarak çalışmıyor, yeniden başlatma isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="92534-163">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="92534-164">İstenirse, Visual Studio'yu yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="92534-164">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="92534-165">Güvenilmeyen geliştirme sertifikası kullanılırsa, tarayıcı, güvenilir olmayan bir sertifika için bir özel durum oluşturmak gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="92534-165">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="92534-166">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="92534-166">Additional resources</span></span>

* [<span data-ttu-id="92534-167">IIS ile Windows ana ASP.NET Çekirdeği</span><span class="sxs-lookup"><span data-stu-id="92534-167">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="92534-168">ASP.NET çekirdeği modülü için giriş</span><span class="sxs-lookup"><span data-stu-id="92534-168">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="92534-169">ASP.NET Core Module yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="92534-169">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="92534-170">HTTPS'yi Zorunlu Kılma</span><span class="sxs-lookup"><span data-stu-id="92534-170">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
