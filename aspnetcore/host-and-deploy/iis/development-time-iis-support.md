---
title: Geliştirme zamanı IIS Visual Studio'da ASP.NET Core için desteği
author: shirhatti
description: ASP.NET Core uygulamaları IIS Windows Server üzerinde çalışırken hata ayıklama desteği bulur.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 0bf4585d44e61c5e7e5b89ce9d8dfdfa10d5460e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="70307-103">Geliştirme zamanı IIS Visual Studio'da ASP.NET Core için desteği</span><span class="sxs-lookup"><span data-stu-id="70307-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="70307-104">Tarafından [Sourabh Shirhatti](https://twitter.com/sshirhatti) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="70307-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="70307-105">Bu makalede [Visual Studio](https://www.visualstudio.com/vs/) hata ayıklama IIS Windows Server'da çalışan ASP.NET Core uygulamaları için destek.</span><span class="sxs-lookup"><span data-stu-id="70307-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="70307-106">Bu konu, bu özelliği etkinleştirmek ve bir projeyi ayarını size yol göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="70307-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="70307-107">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="70307-107">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="70307-108">IIS etkinleştir</span><span class="sxs-lookup"><span data-stu-id="70307-108">Enable IIS</span></span>

1. <span data-ttu-id="70307-109">Gidin **Denetim Masası** > **programları** > **programlar ve Özellikler** > **kapatma Windows özellikleri ya da kapalı** (sol tarafında ekranı).</span><span class="sxs-lookup"><span data-stu-id="70307-109">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="70307-110">Seçin **Internet Information Services** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="70307-110">Select the **Internet Information Services** check box.</span></span>

![Windows özelliklerini gösteren Internet Information Services onay kutusunu işaretli IIS özelliklerinden bazıları etkinleştirildiğini belirten bir siyah bir kare olarak (bir onay işareti)](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="70307-112">IIS yükleme sistemin yeniden başlatılması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="70307-112">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="70307-113">IIS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="70307-113">Configure IIS</span></span>

<span data-ttu-id="70307-114">IIS aşağıdaki ile yapılandırılmış bir Web sitesinde yüklü olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="70307-114">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="70307-115">Uygulamanın başlatma profil URL'si ana bilgisayar adıyla eşleşen bir ana bilgisayar adı.</span><span class="sxs-lookup"><span data-stu-id="70307-115">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="70307-116">Atanmış bir sertifikayla 443 numaralı bağlantı noktası için bağlama.</span><span class="sxs-lookup"><span data-stu-id="70307-116">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="70307-117">Örneğin, **ana bilgisayar adı** eklenen bir Web sitesi, "localhost (başlatma profil de kullanacak"localhost"bölümüne bakın)" olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="70307-117">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="70307-118">Bağlantı noktası "443" (HTTPS) ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="70307-118">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="70307-119">**IIS Express geliştirme sertifikası** Web sitesi, ancak herhangi bir geçerli sertifika works atanır:</span><span class="sxs-lookup"><span data-stu-id="70307-119">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![Bağlama kümesi localhost için bağlantı noktası 443 üzerinde atanmış bir sertifikayla gösteren IIS Web sitesi penceresi ekleyin.](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="70307-121">IIS yüklemesi zaten varsa bir **varsayılan Web sitesi** uygulamanın başlatma profil URL'si ana bilgisayar adıyla eşleşen bir ana bilgisayar adı ile:</span><span class="sxs-lookup"><span data-stu-id="70307-121">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="70307-122">Bağlantı noktası 443 (HTTPS) için bir bağlantı noktası bağlama ekleyin.</span><span class="sxs-lookup"><span data-stu-id="70307-122">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="70307-123">Geçerli bir sertifika Web sitesine atayın.</span><span class="sxs-lookup"><span data-stu-id="70307-123">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="70307-124">Visual Studio geliştirme zamanı IIS desteğini etkinleştir</span><span class="sxs-lookup"><span data-stu-id="70307-124">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="70307-125">Visual Studio yükleyicisi başlatın.</span><span class="sxs-lookup"><span data-stu-id="70307-125">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="70307-126">Seçin **IIS desteği geliştirme zamanı** bileşeni.</span><span class="sxs-lookup"><span data-stu-id="70307-126">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="70307-127">Bileşen, isteğe bağlı olarak listelenen **Özet** için panel **ASP.NET ve web geliştirme** iş yükü.</span><span class="sxs-lookup"><span data-stu-id="70307-127">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="70307-128">Bileşeni yükler [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module), yerel bir IIS modül ASP.NET Core uygulamaları IIS arkasındaki bir ters proxy yapılandırması çalıştırmak için gerekli olduğu.</span><span class="sxs-lookup"><span data-stu-id="70307-128">The component installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps behind IIS in a reverse proxy configuration.</span></span>

![Visual Studio özellikleri değiştirme: iş yükleri sekmesi seçilmiştir.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="70307-132">Projeyi Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="70307-132">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="70307-133">HTTPS yeniden yönlendirmesi</span><span class="sxs-lookup"><span data-stu-id="70307-133">HTTPS redirection</span></span>

<span data-ttu-id="70307-134">Yeni bir proje için onay kutusunu işaretleyin **HTTPS için Yapılandır** içinde **çekirdek yeni bir ASP.NET Web uygulaması** penceresi:</span><span class="sxs-lookup"><span data-stu-id="70307-134">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![Yeni ASP.NET çekirdek Web uygulaması penceresiyle HTTPS onay kutusu seçili yapılandırma.](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="70307-136">Varolan bir projede HTTPS yeniden yönlendirmesi Ara yazılımında kullanmak `Startup.Configure` çağırarak [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="70307-136">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

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

### <a name="iis-launch-profile"></a><span data-ttu-id="70307-137">Profil IIS Başlat</span><span class="sxs-lookup"><span data-stu-id="70307-137">IIS launch profile</span></span>

<span data-ttu-id="70307-138">Geliştirme zamanı IIS desteği eklemek için yeni bir başlatma profili oluşturun:</span><span class="sxs-lookup"><span data-stu-id="70307-138">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="70307-139">İçin **profil**seçin **yeni** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="70307-139">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="70307-140">Profil açılır penceresinde "IIS" adı.</span><span class="sxs-lookup"><span data-stu-id="70307-140">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="70307-141">Seçin **Tamam** profili oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="70307-141">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="70307-142">İçin **başlatma** ayarını seçin **IIS** listeden.</span><span class="sxs-lookup"><span data-stu-id="70307-142">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="70307-143">Onay kutusunu seçin **başlatma tarayıcı** ve uç nokta URL'sini belirtin.</span><span class="sxs-lookup"><span data-stu-id="70307-143">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="70307-144">HTTPS protokolünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="70307-144">Use the HTTPS protocol.</span></span> <span data-ttu-id="70307-145">Bu örnekte `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="70307-145">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="70307-146">İçinde **ortam değişkenleri** bölümünde, select **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="70307-146">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="70307-147">Bir ortam değişkeni bir anahtar sağlamak `ASPNETCORE_ENVIRONMENT` ve değerini `Development`.</span><span class="sxs-lookup"><span data-stu-id="70307-147">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="70307-148">İçinde **Web sunucusu ayarları** alanı ayarlamak **uygulama URL'si**.</span><span class="sxs-lookup"><span data-stu-id="70307-148">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="70307-149">Bu örnekte `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="70307-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="70307-150">Profil kaydedin.</span><span class="sxs-lookup"><span data-stu-id="70307-150">Save the profile.</span></span>

![Hata ayıklama sekmesi seçili proje Özellikler penceresi.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="70307-155">Alternatif olarak, bir başlatma profili el ile eklemeniz [launchSettings.json](http://json.schemastore.org/launchsettings) uygulama dosyasında:</span><span class="sxs-lookup"><span data-stu-id="70307-155">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

## <a name="run-the-project"></a><span data-ttu-id="70307-156">Projeyi çalıştırın</span><span class="sxs-lookup"><span data-stu-id="70307-156">Run the project</span></span>

<span data-ttu-id="70307-157">Çalıştır düğmesini VS Arabiriminde ayarlanırsa **IIS** profil ve uygulamayı başlatmak için düğmesini seçin:</span><span class="sxs-lookup"><span data-stu-id="70307-157">In the VS UI, set the Run button to the **IIS** profile and select the button to start the app:</span></span>

!["IIS" profili kümesi VS araç çubuğu düğmesi çalıştırın.](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="70307-159">Visual Studio, bir yönetici olarak çalışmıyor, yeniden başlatma isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="70307-159">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="70307-160">İstenirse, Visual Studio'yu yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="70307-160">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="70307-161">Güvenilmeyen geliştirme sertifikası kullanılırsa, tarayıcı, güvenilir olmayan bir sertifika için bir özel durum oluşturmak gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="70307-161">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="70307-162">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="70307-162">Additional resources</span></span>

* [<span data-ttu-id="70307-163">IIS ile Windows ana ASP.NET Çekirdeği</span><span class="sxs-lookup"><span data-stu-id="70307-163">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="70307-164">ASP.NET çekirdeği modülü için giriş</span><span class="sxs-lookup"><span data-stu-id="70307-164">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="70307-165">ASP.NET Core Module yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="70307-165">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="70307-166">HTTPS'yi Zorunlu Kılma</span><span class="sxs-lookup"><span data-stu-id="70307-166">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
