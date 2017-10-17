---
title: "Geliştirme zamanı IIS Visual Studio'da ASP.NET Core için desteği"
author: shirhatti
description: "ASP.NET Core uygulamaları IIS Windows Server üzerinde çalışırken hata ayıklama desteği bulur."
keywords: "ASP.NET Core, Internet Information services, IIS, windows server,asp.net çekirdek modülü, hata ayıklama"
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.assetid: 83d98477-9d10-4a78-a54a-f325ad67d13b
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/development-time-iis-support
ms.openlocfilehash: a35a6fd9896c4c110d1b6680b6aaf718d29a18ab
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2017
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="08b2d-104">Geliştirme zamanı IIS Visual Studio'da ASP.NET Core için desteği</span><span class="sxs-lookup"><span data-stu-id="08b2d-104">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="08b2d-105">Göre: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="08b2d-105">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="08b2d-106">Bu makalede [Visual Studio](https://www.visualstudio.com/vs/) destekleyen IIS Windows Server'da çalışan ASP.NET Core uygulamalarında hata ayıklama için.</span><span class="sxs-lookup"><span data-stu-id="08b2d-106">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core applications running behind IIS on Windows Server.</span></span> <span data-ttu-id="08b2d-107">Bu konu, bu özelliği etkinleştirmek ve projenizi ayarı aracılığıyla açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="08b2d-107">This topic walks you through enabling this feature and setting up your project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="08b2d-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="08b2d-108">Prerequisites</span></span>

* <span data-ttu-id="08b2d-109">Visual Studio (2017/15.3 veya sonraki bir sürümü)</span><span class="sxs-lookup"><span data-stu-id="08b2d-109">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="08b2d-110">ASP.NET ve web geliştirme iş yükü *veya* .NET Core platformlar arası geliştirme iş yükü</span><span class="sxs-lookup"><span data-stu-id="08b2d-110">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="08b2d-111">IIS etkinleştir</span><span class="sxs-lookup"><span data-stu-id="08b2d-111">Enable IIS</span></span>

<span data-ttu-id="08b2d-112">IIS, sisteminizde etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="08b2d-112">Enable IIS on your system.</span></span> <span data-ttu-id="08b2d-113">Gidin **Denetim Masası** > **programları** > **programlar ve Özellikler** > **kapatma Windows özellikleri ya da kapalı** (sol tarafında ekranı).</span><span class="sxs-lookup"><span data-stu-id="08b2d-113">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="08b2d-114">Seçin **Internet Information Services** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="08b2d-114">Select the **Internet Information Services** checkbox.</span></span>

![Windows Internet Information Services onay kutusunu siyah kare (bir onay işareti) IIS özelliklerinden bazıları etkinleştirildiğini belirten bir olarak işaretli gösteren özellikleri](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="08b2d-116">IIS yüklemenizi bir yeniden başlatma gerektirirse, sisteminizi yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="08b2d-116">If your IIS installation requires a reboot, reboot your system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="08b2d-117">Geliştirme zamanı IIS desteğini etkinleştir</span><span class="sxs-lookup"><span data-stu-id="08b2d-117">Enable development-time IIS support</span></span>

<span data-ttu-id="08b2d-118">IIS'yi yükledikten sonra var olan Visual Studio yüklemenizin değiştirmek için Visual Studio yükleyicisi başlatın.</span><span class="sxs-lookup"><span data-stu-id="08b2d-118">Once you've installed IIS, launch the Visual Studio installer to modify your existing Visual Studio installation.</span></span> <span data-ttu-id="08b2d-119">Yükleyicisi'nde seçin **IIS desteği geliştirme zamanı** bileşeni.</span><span class="sxs-lookup"><span data-stu-id="08b2d-119">In the installer, select the **Development time IIS support** component.</span></span> <span data-ttu-id="08b2d-120">Bileşen isteğe bağlı bir bileşen olarak listelenen **Özet** için panel **ASP.NET ve web geliştirme** iş yükü.</span><span class="sxs-lookup"><span data-stu-id="08b2d-120">The component is listed as an optional component in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="08b2d-121">Bu yükler [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module), ASP.NET Core uygulamaları çalıştırmak için gereken bir yerel IIS modül olduğu.</span><span class="sxs-lookup"><span data-stu-id="08b2d-121">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core applications.</span></span>

![Visual Studio özellikleri değiştirme: iş yükleri sekmesi seçilmiştir.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="08b2d-125">Projeyi Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="08b2d-125">Configure the project</span></span>

<span data-ttu-id="08b2d-126">Geliştirme zamanı IIS desteği eklemek için yeni bir başlatma profili oluşturun.</span><span class="sxs-lookup"><span data-stu-id="08b2d-126">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="08b2d-127">Visual Studio'nun içinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="08b2d-127">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="08b2d-128">Seçin **hata ayıklama** sekmesi. Seçin **IIS** gelen **başlatma** açılır.</span><span class="sxs-lookup"><span data-stu-id="08b2d-128">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="08b2d-129">Onaylayın **başlatma tarayıcı** özelliği doğru URL'nin etkin.</span><span class="sxs-lookup"><span data-stu-id="08b2d-129">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Hata ayıklama sekmesi seçili proje Özellikler penceresi.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="08b2d-134">Alternatif olarak, el ile başlatma profiline ekleyebilirsiniz, [launchSettings.json](http://json.schemastore.org/launchsettings) uygulama dosyasında:</span><span class="sxs-lookup"><span data-stu-id="08b2d-134">Alternatively, you can manually add a launch profile to your [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="08b2d-135">Yönetici olarak çalışıyorsa doğru Visual Studio yeniden başlatmanız istenebilir.</span><span class="sxs-lookup"><span data-stu-id="08b2d-135">You may be prompted to restart Visual Studio if you weren't running as an administrator.</span></span> <span data-ttu-id="08b2d-136">İstenirse, Visual Studio'yu yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="08b2d-136">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="08b2d-137">Tebrikler!</span><span class="sxs-lookup"><span data-stu-id="08b2d-137">Congratulations!</span></span> <span data-ttu-id="08b2d-138">Bu noktada, projenizi geliştirme zamanı IIS desteği için yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="08b2d-138">At this point, your project is configured for development-time IIS support.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="08b2d-139">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="08b2d-139">Additional resources</span></span>

* [<span data-ttu-id="08b2d-140">IIS ile Windows ana ASP.NET Çekirdeği</span><span class="sxs-lookup"><span data-stu-id="08b2d-140">Host ASP.NET Core on Windows with IIS</span></span>](xref:publishing/iis)
* [<span data-ttu-id="08b2d-141">ASP.NET çekirdeği modülü için giriş</span><span class="sxs-lookup"><span data-stu-id="08b2d-141">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="08b2d-142">ASP.NET çekirdeği modülü yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="08b2d-142">ASP.NET Core Module configuration reference</span></span>](xref:hosting/aspnet-core-module)
