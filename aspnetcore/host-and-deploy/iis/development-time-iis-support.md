---
title: "Geliştirme zamanı IIS Visual Studio'da ASP.NET Core için desteği"
author: shirhatti
description: "ASP.NET Core uygulamaları IIS Windows Server üzerinde çalışırken hata ayıklama desteği bulur."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: a8bdf4c0c0399c62666e6e61e70c0298a42c2c12
ms.sourcegitcommit: 9f758b1550fcae88ab1eb284798a89e6320548a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Geliştirme zamanı IIS Visual Studio'da ASP.NET Core için desteği

Göre: [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Bu makalede [Visual Studio](https://www.visualstudio.com/vs/) hata ayıklama IIS Windows Server'da çalışan ASP.NET Core uygulamaları için destek. Bu konu, bu özelliği etkinleştirmek ve bir projeyi ayarını size yol göstermektedir.

## <a name="prerequisites"></a>Önkoşullar

* Visual Studio (2017/15.3 veya sonraki bir sürümü)
* ASP.NET ve web geliştirme iş yükü *veya* .NET Core platformlar arası geliştirme iş yükü

## <a name="enable-iis"></a>IIS etkinleştir

IIS etkinleştirin. Gidin **Denetim Masası** > **programları** > **programlar ve Özellikler** > **kapatma Windows özellikleri ya da kapalı** (sol tarafında ekranı). Seçin **Internet Information Services** onay kutusu.

![Windows Internet Information Services onay kutusunu siyah kare (bir onay işareti) IIS özelliklerinden bazıları etkinleştirildiğini belirten bir olarak işaretli gösteren özellikleri](development-time-iis-support/_static/enable_iis.png)

IIS yüklemesi bir yeniden başlatma gerektirirse, sistemi yeniden başlatın.

## <a name="enable-development-time-iis-support"></a>Geliştirme zamanı IIS desteğini etkinleştir

Visual Studio yükleyicisi başlatın. Seçin **IIS desteği geliştirme zamanı** bileşeni. Bileşen, isteğe bağlı olarak listelenen **Özet** için panel **ASP.NET ve web geliştirme** iş yükü. Bu yükler [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module), yerel bir IIS modül ASP.NET Core uygulamaları çalıştırmak için gerekli olduğu.

![Visual Studio özellikleri değiştirme: iş yükleri sekmesi seçilmiştir. Web ve bulut bölümünde, ASP.NET ve web geliştirme panelinde seçilir. İsteğe bağlı alan Özet bölmenin sağ tarafta IIS desteği geliştirme zamanı için bir onay kutusu yok.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Projeyi Yapılandırma

Geliştirme zamanı IIS desteği eklemek için yeni bir başlatma profili oluşturun. Visual Studio'nun içinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **özellikleri**. Seçin **hata ayıklama** sekmesi. Seçin **IIS** gelen **başlatma** açılır. Onaylayın **başlatma tarayıcı** özelliği doğru URL'nin etkin.

![Hata ayıklama sekmesi seçili proje Özellikler penceresi. Profil ve başlatma ayarlarını IIS ayarlanır. Başlatma tarayıcı özelliği bir http://localhost/WebApplication2 adresiyle etkindir. Aynı adresi, etkinleştirme anonim kimlik doğrulaması etkin Web sunucusu ayarları alanının uygulama URL'si alanına de sağlanır.](development-time-iis-support/_static/project_properties.png)

Alternatif olarak, bir başlatma profili el ile eklemeniz [launchSettings.json](http://json.schemastore.org/launchsettings) uygulama dosyasında:

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

Visual Studio, bir yönetici olarak çalışmıyor, yeniden başlatma isteyebilir. İstenirse, Visual Studio'yu yeniden başlatın.

## <a name="additional-resources"></a>Ek kaynaklar

* [IIS ile Windows ana ASP.NET Çekirdeği](xref:host-and-deploy/iis/index)
* [ASP.NET çekirdeği modülü için giriş](xref:fundamentals/servers/aspnet-core-module)
* [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module)
