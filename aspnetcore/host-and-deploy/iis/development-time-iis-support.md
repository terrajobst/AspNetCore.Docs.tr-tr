---
title: "Geliştirme zamanı IIS Visual Studio'da ASP.NET Core için desteği"
author: shirhatti
description: "ASP.NET Core uygulamaları IIS Windows Server üzerinde çalışırken hata ayıklama desteği bulur."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 09/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 264be49e8aff72f913c22508150e424541d49fa0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="26308-103">Geliştirme zamanı IIS Visual Studio'da ASP.NET Core için desteği</span><span class="sxs-lookup"><span data-stu-id="26308-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="26308-104">Göre: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="26308-104">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="26308-105">Bu makalede [Visual Studio](https://www.visualstudio.com/vs/) destekleyen IIS Windows Server'da çalışan ASP.NET Core uygulamalarında hata ayıklama için.</span><span class="sxs-lookup"><span data-stu-id="26308-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core applications running behind IIS on Windows Server.</span></span> <span data-ttu-id="26308-106">Bu konu, bu özelliği etkinleştirmek ve bir projeyi ayarını size yol göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="26308-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="26308-107">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="26308-107">Prerequisites</span></span>

* <span data-ttu-id="26308-108">Visual Studio (2017/15.3 veya sonraki bir sürümü)</span><span class="sxs-lookup"><span data-stu-id="26308-108">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="26308-109">ASP.NET ve web geliştirme iş yükü *veya* .NET Core platformlar arası geliştirme iş yükü</span><span class="sxs-lookup"><span data-stu-id="26308-109">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="26308-110">IIS etkinleştir</span><span class="sxs-lookup"><span data-stu-id="26308-110">Enable IIS</span></span>

<span data-ttu-id="26308-111">IIS etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="26308-111">Enable IIS.</span></span> <span data-ttu-id="26308-112">Gidin **Denetim Masası** > **programları** > **programlar ve Özellikler** > **kapatma Windows özellikleri ya da kapalı** (sol tarafında ekranı).</span><span class="sxs-lookup"><span data-stu-id="26308-112">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="26308-113">Seçin **Internet Information Services** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="26308-113">Select the **Internet Information Services** checkbox.</span></span>

![Windows Internet Information Services onay kutusunu siyah kare (bir onay işareti) IIS özelliklerinden bazıları etkinleştirildiğini belirten bir olarak işaretli gösteren özellikleri](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="26308-115">IIS yüklemesi bir yeniden başlatma gerektirirse, sistemi yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="26308-115">If the IIS installation requires a reboot, reboot the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="26308-116">Geliştirme zamanı IIS desteğini etkinleştir</span><span class="sxs-lookup"><span data-stu-id="26308-116">Enable development-time IIS support</span></span>

<span data-ttu-id="26308-117">IIS yüklendikten sonra Visual Studio yükleyicisi, var olan Visual Studio yüklemeyi değiştirmek için başlatın.</span><span class="sxs-lookup"><span data-stu-id="26308-117">Once IIS is installed, launch the Visual Studio installer to modify the existing Visual Studio installation.</span></span> <span data-ttu-id="26308-118">Yükleyicisi'nde seçin **IIS desteği geliştirme zamanı** bileşeni.</span><span class="sxs-lookup"><span data-stu-id="26308-118">In the installer, select the **Development time IIS support** component.</span></span> <span data-ttu-id="26308-119">Bileşen isteğe bağlı bir bileşen olarak listelenen **Özet** için panel **ASP.NET ve web geliştirme** iş yükü.</span><span class="sxs-lookup"><span data-stu-id="26308-119">The component is listed as an optional component in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="26308-120">Bu yükler [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module), ASP.NET Core uygulamaları çalıştırmak için gereken bir yerel IIS modül olduğu.</span><span class="sxs-lookup"><span data-stu-id="26308-120">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core applications.</span></span>

![Visual Studio özellikleri değiştirme: iş yükleri sekmesi seçilmiştir.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="26308-124">Projeyi Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="26308-124">Configure the project</span></span>

<span data-ttu-id="26308-125">Geliştirme zamanı IIS desteği eklemek için yeni bir başlatma profili oluşturun.</span><span class="sxs-lookup"><span data-stu-id="26308-125">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="26308-126">Visual Studio'nun içinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="26308-126">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="26308-127">Seçin **hata ayıklama** sekmesi. Seçin **IIS** gelen **başlatma** açılır.</span><span class="sxs-lookup"><span data-stu-id="26308-127">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="26308-128">Onaylayın **başlatma tarayıcı** özelliği doğru URL'nin etkin.</span><span class="sxs-lookup"><span data-stu-id="26308-128">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Hata ayıklama sekmesi seçili proje Özellikler penceresi.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="26308-133">Alternatif olarak, bir başlatma profili el ile eklemeniz [launchSettings.json](http://json.schemastore.org/launchsettings) uygulama dosyasında:</span><span class="sxs-lookup"><span data-stu-id="26308-133">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

```json
{
    "iisSettings": {
        "windowsAuthentication": false,
        "anonymousAuthentication": true,
        "iis": {
            "applicationUrl": "http://localhost/WebApplication2",
            "sslPort": 0
        }
    },
    "profiles": {
        "IIS": {
            "commandName": "IIS",
            "launchBrowser": "true",
            "launchUrl": "http://localhost/WebApplication2",
            "environmentVariables": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    }
}
```

<span data-ttu-id="26308-134">Visual Studio, bir yönetici olarak çalışmıyor, yeniden başlatma isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="26308-134">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="26308-135">İstenirse, Visual Studio'yu yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="26308-135">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="26308-136">Tebrikler!</span><span class="sxs-lookup"><span data-stu-id="26308-136">Congratulations!</span></span> <span data-ttu-id="26308-137">Bu noktada, geliştirme zamanı IIS desteği için proje yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="26308-137">At this point, the project is configured for development-time IIS support.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="26308-138">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="26308-138">Additional resources</span></span>

* [<span data-ttu-id="26308-139">IIS ile Windows ana ASP.NET Çekirdeği</span><span class="sxs-lookup"><span data-stu-id="26308-139">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="26308-140">ASP.NET çekirdeği modülü için giriş</span><span class="sxs-lookup"><span data-stu-id="26308-140">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="26308-141">ASP.NET Core Module yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="26308-141">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
