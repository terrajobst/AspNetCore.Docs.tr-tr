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
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Geliştirme zamanı IIS Visual Studio'da ASP.NET Core için desteği

Göre: [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Bu makalede [Visual Studio](https://www.visualstudio.com/vs/) destekleyen IIS Windows Server'da çalışan ASP.NET Core uygulamalarında hata ayıklama için. Bu konu, bu özelliği etkinleştirmek ve projenizi ayarı aracılığıyla açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

* Visual Studio (2017/15.3 veya sonraki bir sürümü)
* ASP.NET ve web geliştirme iş yükü *veya* .NET Core platformlar arası geliştirme iş yükü

## <a name="enable-iis"></a>IIS etkinleştir

IIS, sisteminizde etkinleştirin. Gidin **Denetim Masası** > **programları** > **programlar ve Özellikler** > **kapatma Windows özellikleri ya da kapalı** (sol tarafında ekranı). Seçin **Internet Information Services** onay kutusu.

![Windows Internet Information Services onay kutusunu siyah kare (bir onay işareti) IIS özelliklerinden bazıları etkinleştirildiğini belirten bir olarak işaretli gösteren özellikleri](development-time-iis-support/_static/enable_iis.png)

IIS yüklemenizi bir yeniden başlatma gerektirirse, sisteminizi yeniden başlatın.

## <a name="enable-development-time-iis-support"></a>Geliştirme zamanı IIS desteğini etkinleştir

IIS'yi yükledikten sonra var olan Visual Studio yüklemenizin değiştirmek için Visual Studio yükleyicisi başlatın. Yükleyicisi'nde seçin **IIS desteği geliştirme zamanı** bileşeni. Bileşen isteğe bağlı bir bileşen olarak listelenen **Özet** için panel **ASP.NET ve web geliştirme** iş yükü. Bu yükler [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module), ASP.NET Core uygulamaları çalıştırmak için gereken bir yerel IIS modül olduğu.

![Visual Studio özellikleri değiştirme: iş yükleri sekmesi seçilmiştir. Web ve bulut bölümünde, ASP.NET ve web geliştirme panelinde seçilir. İsteğe bağlı alan Özet bölmenin sağ tarafta IIS desteği geliştirme zamanı için bir onay kutusu yok.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Projeyi Yapılandırma

Geliştirme zamanı IIS desteği eklemek için yeni bir başlatma profili oluşturun. Visual Studio'nun içinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **özellikleri**. Seçin **hata ayıklama** sekmesi. Seçin **IIS** gelen **başlatma** açılır. Onaylayın **başlatma tarayıcı** özelliği doğru URL'nin etkin.

![Hata ayıklama sekmesi seçili proje Özellikler penceresi. Profil ve başlatma ayarlarını IIS ayarlanır. Başlatma tarayıcı özelliği bir http://localhost/WebApplication2 adresiyle etkindir. Aynı adresi, etkinleştirme anonim kimlik doğrulaması etkin Web sunucusu ayarları alanının uygulama URL'si alanına de sağlanır.](development-time-iis-support/_static/project_properties.png)

Alternatif olarak, el ile başlatma profiline ekleyebilirsiniz, [launchSettings.json](http://json.schemastore.org/launchsettings) uygulama dosyasında:

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

Yönetici olarak çalışıyorsa doğru Visual Studio yeniden başlatmanız istenebilir. İstenirse, Visual Studio'yu yeniden başlatın.

Tebrikler! Bu noktada, projenizi geliştirme zamanı IIS desteği için yapılandırılır. 

## <a name="additional-resources"></a>Ek kaynaklar

* [IIS ile Windows ana ASP.NET Çekirdeği](xref:publishing/iis)
* [ASP.NET çekirdeği modülü için giriş](xref:fundamentals/servers/aspnet-core-module)
* [ASP.NET çekirdeği modülü yapılandırma başvurusu](xref:hosting/aspnet-core-module)
