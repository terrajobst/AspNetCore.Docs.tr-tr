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
ms.openlocfilehash: aeff8cd7da0637290d4edffaf183fc3c4f56f7f4
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34555488"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Geliştirme zamanı IIS Visual Studio'da ASP.NET Core için desteği

Tarafından [Sourabh Shirhatti](https://twitter.com/sshirhatti) ve [Luke Latham](https://github.com/guardrex)

Bu makalede [Visual Studio](https://www.visualstudio.com/vs/) hata ayıklama IIS Windows Server'da çalışan ASP.NET Core uygulamaları için destek. Bu konu, bu özelliği etkinleştirmek ve bir projeyi ayarını size yol göstermektedir.

## <a name="prerequisites"></a>Önkoşullar

* [Windows için Visual Studio](https://www.microsoft.com/net/download/windows)
* **ASP.NET ve web geliştirme** iş yükü
* **.NET core platformlar arası geliştirme** iş yükü
* X.509 güvenlik sertifikası

## <a name="enable-iis"></a>IIS etkinleştir

1. Gidin **Denetim Masası** > **programları** > **programlar ve Özellikler** > **kapatma Windows özellikleri ya da kapalı** (sol tarafında ekranı).
1. Seçin **Internet Information Services** onay kutusu.

![Windows özelliklerini gösteren Internet Information Services onay kutusunu işaretli IIS özelliklerinden bazıları etkinleştirildiğini belirten bir siyah bir kare olarak (bir onay işareti)](development-time-iis-support/_static/enable_iis.png)

IIS yükleme sistemin yeniden başlatılması gerekebilir.

## <a name="configure-iis"></a>IIS yapılandırma

IIS aşağıdaki ile yapılandırılmış bir Web sitesinde yüklü olmalıdır:

* Uygulamanın başlatma profil URL'si ana bilgisayar adıyla eşleşen bir ana bilgisayar adı.
* Atanmış bir sertifikayla 443 numaralı bağlantı noktası için bağlama.

Örneğin, **ana bilgisayar adı** eklenen bir Web sitesi, "localhost (başlatma profil de kullanacak"localhost"bölümüne bakın)" olarak ayarlayın. Bağlantı noktası "443" (HTTPS) ayarlanır. **IIS Express geliştirme sertifikası** Web sitesi, ancak herhangi bir geçerli sertifika works atanır:

![Bağlama kümesi localhost için bağlantı noktası 443 üzerinde atanmış bir sertifikayla gösteren IIS Web sitesi penceresi ekleyin.](development-time-iis-support/_static/add-website-window.png)

IIS yüklemesi zaten varsa bir **varsayılan Web sitesi** uygulamanın başlatma profil URL'si ana bilgisayar adıyla eşleşen bir ana bilgisayar adı ile:

* Bağlantı noktası 443 (HTTPS) için bir bağlantı noktası bağlama ekleyin.
* Geçerli bir sertifika Web sitesine atayın.

## <a name="enable-development-time-iis-support-in-visual-studio"></a>Visual Studio geliştirme zamanı IIS desteğini etkinleştir

1. Visual Studio yükleyicisi başlatın.
1. Seçin **IIS desteği geliştirme zamanı** bileşeni. Bileşen, isteğe bağlı olarak listelenen **Özet** için panel **ASP.NET ve web geliştirme** iş yükü. Bileşeni yükler [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module), yerel bir IIS modül ASP.NET Core uygulamaları IIS arkasındaki bir ters proxy yapılandırması çalıştırmak için gerekli olduğu.

![Visual Studio özellikleri değiştirme: iş yükleri sekmesi seçilmiştir. Web ve bulut bölümünde, ASP.NET ve web geliştirme panelinde seçilir. İsteğe bağlı alan Özet bölmenin sağ tarafta IIS desteği geliştirme zamanı için bir onay kutusu yok.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Projeyi Yapılandırma

### <a name="https-redirection"></a>HTTPS yeniden yönlendirmesi

Yeni bir proje için onay kutusunu işaretleyin **HTTPS için Yapılandır** içinde **çekirdek yeni bir ASP.NET Web uygulaması** penceresi:

![Yeni ASP.NET çekirdek Web uygulaması penceresiyle HTTPS onay kutusu seçili yapılandırma.](development-time-iis-support/_static/new-app.png)

Varolan bir projede HTTPS yeniden yönlendirmesi Ara yazılımında kullanmak `Startup.Configure` çağırarak [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) genişletme yöntemi:

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

### <a name="iis-launch-profile"></a>Profil IIS Başlat

Geliştirme zamanı IIS desteği eklemek için yeni bir başlatma profili oluşturun:

1. İçin **profil**seçin **yeni** düğmesi. Profil açılır penceresinde "IIS" adı. Seçin **Tamam** profili oluşturmak için.
1. İçin **başlatma** ayarını seçin **IIS** listeden.
1. Onay kutusunu seçin **başlatma tarayıcı** ve uç nokta URL'sini belirtin. HTTPS protokolünü kullanır. Bu örnekte `https://localhost/WebApplication1`.
1. İçinde **ortam değişkenleri** bölümünde, select **Ekle** düğmesi. Bir ortam değişkeni bir anahtar sağlamak `ASPNETCORE_ENVIRONMENT` ve değerini `Development`.
1. İçinde **Web sunucusu ayarları** alanı ayarlamak **uygulama URL'si**. Bu örnekte `https://localhost/WebApplication1`.
1. Profil kaydedin.

![Hata ayıklama sekmesi seçili proje Özellikler penceresi. Profil ve başlatma ayarlarını IIS ayarlanır. Bir adresi ile başlatma tarayıcı özelliği etkinleştirilmişse https://localhost/WebApplication1. Aynı adresi, Web sunucusu ayarları alan uygulama URL'si alanına de sağlanır.](development-time-iis-support/_static/project_properties.png)

Alternatif olarak, bir başlatma profili el ile eklemeniz [launchSettings.json](http://json.schemastore.org/launchsettings) uygulama dosyasında:

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

## <a name="run-the-project"></a>Projeyi çalıştırın

Çalıştır düğmesini VS Arabiriminde ayarlanırsa **IIS** profil ve uygulamayı başlatmak için düğmesini seçin:

!["IIS" profili kümesi VS araç çubuğu düğmesi çalıştırın.](development-time-iis-support/_static/toolbar.png)

Visual Studio, bir yönetici olarak çalışmıyor, yeniden başlatma isteyebilir. İstenirse, Visual Studio'yu yeniden başlatın.

Güvenilmeyen geliştirme sertifikası kullanılırsa, tarayıcı, güvenilir olmayan bir sertifika için bir özel durum oluşturmak gerektirebilir.

## <a name="additional-resources"></a>Ek kaynaklar

* [IIS ile Windows ana ASP.NET Çekirdeği](xref:host-and-deploy/iis/index)
* [ASP.NET çekirdeği modülü için giriş](xref:fundamentals/servers/aspnet-core-module)
* [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module)
* [HTTPS'yi Zorunlu Kılma](xref:security/enforcing-ssl)
