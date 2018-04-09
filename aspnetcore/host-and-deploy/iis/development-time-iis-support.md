---
title: Geliştirme zamanı IIS Visual Studio'da ASP.NET Core için desteği
author: shirhatti
description: ASP.NET Core uygulamaları IIS Windows Server üzerinde çalışırken hata ayıklama desteği bulur.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 218bb2653b92cd7b1cf2c6726b2d4bedbf307a62
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="6925e-103">Geliştirme zamanı IIS Visual Studio'da ASP.NET Core için desteği</span><span class="sxs-lookup"><span data-stu-id="6925e-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="6925e-104">Tarafından [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="6925e-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="6925e-105">Bu makalede [Visual Studio](https://www.visualstudio.com/vs/) hata ayıklama IIS Windows Server'da çalışan ASP.NET Core uygulamaları için destek.</span><span class="sxs-lookup"><span data-stu-id="6925e-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="6925e-106">Bu konu, bu özelliği etkinleştirmek ve bir projeyi ayarını size yol göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="6925e-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6925e-107">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="6925e-107">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="6925e-108">IIS etkinleştir</span><span class="sxs-lookup"><span data-stu-id="6925e-108">Enable IIS</span></span>

<span data-ttu-id="6925e-109">IIS etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="6925e-109">Enable IIS.</span></span> <span data-ttu-id="6925e-110">Gidin **Denetim Masası** > **programları** > **programlar ve Özellikler** > **kapatma Windows özellikleri ya da kapalı** (sol tarafında ekranı).</span><span class="sxs-lookup"><span data-stu-id="6925e-110">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="6925e-111">Seçin **Internet Information Services** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="6925e-111">Select the **Internet Information Services** checkbox.</span></span>

![Windows Internet Information Services onay kutusunu siyah kare (bir onay işareti) IIS özelliklerinden bazıları etkinleştirildiğini belirten bir olarak işaretli gösteren özellikleri](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="6925e-113">IIS yüklemesi bir yeniden başlatma gerektirirse, sistemi yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="6925e-113">If the IIS installation requires a restart, restart the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="6925e-114">Geliştirme zamanı IIS desteğini etkinleştir</span><span class="sxs-lookup"><span data-stu-id="6925e-114">Enable development-time IIS support</span></span>

<span data-ttu-id="6925e-115">Visual Studio yükleyicisi başlatın.</span><span class="sxs-lookup"><span data-stu-id="6925e-115">Launch the Visual Studio installer.</span></span> <span data-ttu-id="6925e-116">Seçin **IIS desteği geliştirme zamanı** bileşeni.</span><span class="sxs-lookup"><span data-stu-id="6925e-116">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="6925e-117">Bileşen, isteğe bağlı olarak listelenen **Özet** için panel **ASP.NET ve web geliştirme** iş yükü.</span><span class="sxs-lookup"><span data-stu-id="6925e-117">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="6925e-118">Bu yükler [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module), yerel bir IIS modül ASP.NET Core uygulamaları çalıştırmak için gerekli olduğu.</span><span class="sxs-lookup"><span data-stu-id="6925e-118">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps.</span></span>

![Visual Studio özellikleri değiştirme: iş yükleri sekmesi seçilmiştir.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="6925e-122">Projeyi Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6925e-122">Configure the project</span></span>

<span data-ttu-id="6925e-123">Geliştirme zamanı IIS desteği eklemek için yeni bir başlatma profili oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6925e-123">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="6925e-124">Visual Studio'nun içinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="6925e-124">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="6925e-125">Seçin **hata ayıklama** sekmesi. Seçin **IIS** gelen **başlatma** açılır.</span><span class="sxs-lookup"><span data-stu-id="6925e-125">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="6925e-126">Onaylayın **başlatma tarayıcı** özelliği doğru URL'nin etkin.</span><span class="sxs-lookup"><span data-stu-id="6925e-126">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Hata ayıklama sekmesi seçili proje Özellikler penceresi.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="6925e-131">Alternatif olarak, bir başlatma profili el ile eklemeniz [launchSettings.json](http://json.schemastore.org/launchsettings) uygulama dosyasında:</span><span class="sxs-lookup"><span data-stu-id="6925e-131">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="6925e-132">Visual Studio, bir yönetici olarak çalışmıyor, yeniden başlatma isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="6925e-132">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="6925e-133">İstenirse, Visual Studio'yu yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="6925e-133">If prompted, restart Visual Studio.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6925e-134">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6925e-134">Additional resources</span></span>

* [<span data-ttu-id="6925e-135">IIS ile Windows ana ASP.NET Çekirdeği</span><span class="sxs-lookup"><span data-stu-id="6925e-135">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="6925e-136">ASP.NET çekirdeği modülü için giriş</span><span class="sxs-lookup"><span data-stu-id="6925e-136">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="6925e-137">ASP.NET Core Module yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="6925e-137">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
