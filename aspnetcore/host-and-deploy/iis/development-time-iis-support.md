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
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Geliştirme zamanı IIS, ASP.NET Core için Visual Studio desteği

Tarafından [Sourabh Shirhatti](https://twitter.com/sshirhatti) ve [Luke Latham](https://github.com/guardrex)

Bu makalede [Visual Studio](https://www.visualstudio.com/vs/) IIS Windows Server üzerinde çalışan ASP.NET Core uygulamaları hata ayıklama için destek. Bu konu başlığı altında bu özelliği etkinleştirmek ve bir proje ayarlama gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

* [Windows için Visual Studio](https://www.microsoft.com/net/download/windows)
* **ASP.NET ve web geliştirme** iş yükü
* **.NET core çoklu platform geliştirme** iş yükü
* X.509 güvenlik sertifikası

## <a name="enable-iis"></a>IIS'yi etkinleştirin

1. Gidin **Denetim Masası** > **programlar** > **programlar ve Özellikler** > **kapatma Windows özellikleri hakkında ya da kapalı** (ekranın sol).
1. Seçin **Internet Information Services** onay kutusu.

![Windows özelliklerini gösteren Internet Information Services onay kutusunu işaretli IIS özelliklerinden bazıları etkinleştirildiğini belirten bir siyah bir kare olarak (bir onay işareti)](development-time-iis-support/_static/enable_iis.png)

IIS yüklemesi, sistemin yeniden başlatılması gerekebilir.

## <a name="configure-iis"></a>IIS yapılandırma

IIS, aşağıdaki ile yapılandırılmış bir Web sitesinde sahip olmanız gerekir:

* Uygulama başlatma profili URL'si ana bilgisayar adıyla eşleşen bir konak adı.
* Atanan bir sertifika ile 443 numaralı bağlantı noktası için bağlama.

Örneğin, **ana bilgisayar adı** eklenen bir Web sitesi, "localhost" (başlatma profili de kullanır "localhost" Bu konunun ilerleyen bölümlerinde) olarak ayarlayın. Bağlantı noktası "443" (HTTPS) ayarlanır. **IIS Express geliştirme sertifikası** çalışır, Web sitesi, ancak herhangi bir geçerli sertifika atanır:

![Bağlama kümesi localhost için atanan bir sertifika ile bağlantı noktası 443 üzerinde gösteren bir IIS Web sitesi penceresini ekleyin.](development-time-iis-support/_static/add-website-window.png)

IIS yüklemesi zaten varsa, bir **varsayılan Web sitesi** uygulamanın başlatma profili URL'si ana bilgisayar adıyla eşleşen bir ana bilgisayar adına sahip:

* Bağlantı noktası 443 (HTTPS) için bir bağlantı noktası bağlaması ekleyin.
* Geçerli bir sertifika Web sitesine atayın.

## <a name="enable-development-time-iis-support-in-visual-studio"></a>Visual Studio'da geliştirme zamanı IIS desteğini etkinleştirin

1. Visual Studio Yükleyicisi'ni başlatın.
1. Seçin **geliştirme zamanı IIS desteğini** bileşeni. Bileşen isteğe bağlı olarak, listelenen **özeti** panelinde **ASP.NET ve web geliştirme** iş yükü. Bileşeni yükler [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module), ASP.NET Core uygulamaları ile IIS çalıştırmak için gereken yerel bir IIS modül olduğu.

![Visual Studio özellikleri değiştirerek: iş yüklerini sekmesi seçilmiştir. Web ve bulut bölümünde, ASP.NET ve web geliştirme paneli seçilir. Özet panelini isteğe bağlı alanında sağ tarafta, geliştirme zamanı IIS desteğini için bir onay kutusu yok.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Proje yapılandırma

### <a name="https-redirection"></a>HTTPS yeniden yönlendirmesi

Yeni bir proje için onay kutusunu işaretleyin **HTTPS için Yapılandır** içinde **yeni ASP.NET Core Web uygulaması** penceresi:

![Yeni ASP.NET Core Web uygulaması penceresi için HTTPS onay kutusu seçili yapılandırma ile.](development-time-iis-support/_static/new-app.png)

Varolan bir projede, HTTPS yeniden yönlendirmesi Ara yazılımında kullanın `Startup.Configure` çağırarak [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) genişletme yöntemi:

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

### <a name="iis-launch-profile"></a>IIS başlatma profili

Geliştirme zamanı IIS desteği eklemek için yeni bir başlatma profili oluşturun:

1. İçin **profili**seçin **yeni** düğmesi. Profili, "IIS" açılan pencerede adlandırın. Seçin **Tamam** profili oluşturmak için.
1. İçin **başlatma** ayarını seçin **IIS** listeden.
1. Onay kutusunu seçin **tarayıcıyı başlatın** ve uç nokta URL'sini sağlayın. HTTPS protokolünü kullanır. Bu örnekte `https://localhost/WebApplication1`.
1. İçinde **ortam değişkenlerini** bölümünden **Ekle** düğmesi. Bir anahtar ile bir ortam değişkeni sağlamak `ASPNETCORE_ENVIRONMENT` değerini `Development`.
1. İçinde **Web sunucusu ayarlarını** alanı ayarlama **uygulama URL'si**. Bu örnekte `https://localhost/WebApplication1`.
1. Profili kaydedin.

![Hata ayıklama sekmesi seçili Proje Özellikleri penceresi. Profil ve başlatma ayarları, IIS için ayarlanır. Başlatma tarayıcı özelliği bir adresi ile etkin https://localhost/WebApplication1. Aynı adres alanının Web sunucusu ayarları uygulama URL alanını de sağlanır.](development-time-iis-support/_static/project_properties.png)

Alternatif olarak, el ile eklemek için bir başlatma profili [launchSettings.json](http://json.schemastore.org/launchsettings) uygulama dosyasında:

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

## <a name="run-the-project"></a>Projeyi Çalıştır

Visual Studio'da:

* Aşağı açılan listede derleme yapılandırmasını değerine ayarlandığını onaylayın **hata ayıklama**.
* Çalıştır düğmesini kümesine **IIS** profil ve uygulamayı başlatmak için düğmesini seçin.

![VS araç çubuğundaki Çalıştır düğmesini IIS profiline yayın olarak ayarlanmış derleme yapılandırması açılan liste ile ayarlanır.](development-time-iis-support/_static/toolbar.png)

Visual Studio'yu yönetici olarak çalıştırarak değil, yeniden başlatma isteyebilir. İstenirse, Visual Studio'yu yeniden başlatın.

Güvenilmeyen bir geliştirme sertifikası kullanıyorsanız, tarayıcı Güvenilmeyen sertifika için bir özel durum oluşturmak gerekli kılabiliriz.

> [!NOTE]
> Yayın hata ayıklama derleme yapılandırması ile [yalnızca kendi kodum](/visualstudio/debugger/just-my-code) ve düzeyi düşürülmüş bir deneyimle derleyici iyileştirmeleri sonuçları. Örneğin, kesme noktaları isabet değildir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Windows IIS üzerinde ASP.NET Core barındırma](xref:host-and-deploy/iis/index)
* [ASP.NET Core modülü için giriş](xref:fundamentals/servers/aspnet-core-module)
* [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module)
* [HTTPS'yi Zorunlu Kılma](xref:security/enforcing-ssl)
