---
title: Geliştirme zamanı IIS, ASP.NET Core için Visual Studio desteği
author: guardrex
description: IIS ile Windows Server üzerinde çalışan ASP.NET Core uygulamalarında hata ayıklamaya yönelik destek keşfedin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: d2b2456c7ab6b72f2270b6edc17000695061cc2b
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901046"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Geliştirme zamanı IIS, ASP.NET Core için Visual Studio desteği

Tarafından [Sourabh Shirhatti](https://twitter.com/sshirhatti) ve [Luke Latham](https://github.com/guardrex)

Bu makalede [Visual Studio](https://visualstudio.microsoft.com) IIS ile Windows Server üzerinde çalışan ASP.NET Core uygulamalarında hata ayıklama için destek. Bu konuda bu senaryoyu etkinleştirmek ve bir proje ayarlama gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

* [Windows için Visual Studio](https://visualstudio.microsoft.com/downloads/)
* **ASP.NET ve web geliştirme** iş yükü
* **.NET core çoklu platform geliştirme** iş yükü
* X.509 güvenlik sertifikası (için HTTPS desteği)

## <a name="enable-iis"></a>IIS'yi etkinleştirin

1. Windows içinde gidin **Denetim Masası** > **programlar** > **programlar ve Özellikler** > **Windows Aç özelliklerini aç veya Kapat** (ekranın sol).
1. Seçin **Internet Information Services** onay kutusu. **Tamam**’ı seçin.

IIS yüklemesi, sistemin yeniden başlatılması gerekebilir.

## <a name="configure-iis"></a>IIS yapılandırma

IIS, aşağıdaki ile yapılandırılmış bir Web sitesinde sahip olmanız gerekir:

* **Ana bilgisayar adı** &ndash; genellikle **varsayılan Web sitesi** ile kullanılan bir **ana bilgisayar adı** , `localhost`. Ancak, herhangi bir geçerli IIS Web sitesinde bir benzersiz ana bilgisayar adı ile çalışır.
* **Site bağlama**
  * HTTPS gerektiren uygulamalar için bir sertifika ile 443 numaralı bağlantı noktasına bir bağlama oluşturun. Genellikle, **IIS Express geliştirme sertifikası** kullanılan, ancak herhangi bir geçerli sertifika Works.
  * HTTP kullanan uygulamalar onaylamak için 80 göndermek için bir bağlama varlığını veya yeni bir site için 80 numaralı bağlantı noktasına bir bağlama oluşturun.
  * Tek bir bağlama için HTTP veya HTTPS kullanın. **HTTP ve HTTPS bağlantı noktalarını aynı anda bağlama desteklenmiyor.**

## <a name="enable-development-time-iis-support-in-visual-studio"></a>Visual Studio'da geliştirme zamanı IIS desteğini etkinleştirin

1. Visual Studio Yükleyicisi'ni başlatın.
1. Seçin **Değiştir** IIS geliştirme zamanı desteği için kullanmayı planladığınız Visual Studio yükleme.
1. İçin **ASP.NET ve web geliştirme** iş yükü, bulma ve yükleme **geliştirme zamanı IIS desteğini** bileşeni.

   Bileşen listelenen **isteğe bağlı** bölümüne **geliştirme zamanı IIS desteğini** içinde **Yükleme ayrıntıları** panelinde sağ iş yükleri için. Bileşeni yükler [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module), ASP.NET Core uygulamaları ile IIS çalıştırmak için gereken yerel bir IIS modül olduğu.

## <a name="configure-the-project"></a>Proje yapılandırma

### <a name="https-redirection"></a>HTTPS yeniden yönlendirmesi

HTTPS gerektiren yeni bir proje için onay kutusunu işaretleyin **HTTPS için Yapılandır** içinde **yeni bir ASP.NET Core Web uygulaması oluşturma** penceresi. Onay kutusunu işaretleyerek ekler [HTTPS yeniden yönlendirmesi ve HSTS ara yazılım](xref:security/enforcing-ssl) oluşturulduğunda uygulamaya.

HTTPS gerektiren var olan bir proje için HTTPS yeniden yönlendirmesi ve HSTS Ara yazılımında kullanın `Startup.Configure`. Daha fazla bilgi için bkz. <xref:security/enforcing-ssl>.

HTTP kullanan bir proje için [HTTPS yeniden yönlendirmesi ve HSTS ara yazılım](xref:security/enforcing-ssl) uygulamaya eklenmez. Uygulama yapılandırma gerekmiyor.

### <a name="iis-launch-profile"></a>IIS başlatma profili

Geliştirme zamanı IIS desteği eklemek için yeni bir başlatma profili oluşturun:

::: moniker range=">= aspnetcore-3.0"

1. Projeye sağ **Çözüm Gezgini**. Seçin **özellikleri**. Açık **hata ayıklama** sekmesi.
1. İçin **profili**seçin **yeni** düğmesi. Profili, "IIS" açılan pencerede adlandırın. Seçin **Tamam** profili oluşturmak için.
1. İçin **başlatma** ayarını seçin **IIS** listeden.
1. Onay kutusunu seçin **tarayıcıyı başlatın** ve uç nokta URL'sini sağlayın.

   Uygulama HTTPS gerektirdiğinde, HTTPS uç noktasının kullanın (`https://`). HTTP için bir HTTP kullan (`http://`) uç noktası.

   Aynı konak adı belirtin ve olarak bağlantı noktası [IIS yapılandırması belirtilen önceki kullanımlar](#configure-iis), genellikle `localhost`.

   URL'nin sonunda bir uygulama adı sağlayın.

   Örneğin, `https://localhost/WebApplication1` (HTTPS) ya da `http://localhost/WebApplication1` (HTTP) geçerli uç nokta URL'leri.
1. İçinde **ortam değişkenlerini** bölümünden **Ekle** düğmesi. Bir ortam değişkeni ile sağlayan bir **adı** , `ASPNETCORE_ENVIRONMENT` ve **değer** , `Development`.
1. İçinde **Web sunucusu ayarlarını** alanı ayarlama **uygulama URL'si** için kullanılan aynı değere **tarayıcıyı başlatın** uç nokta URL'si.
1. İçin **barındırma modeli** ayarı Visual Studio 2019 ya da daha sonra Seç **varsayılan** proje tarafından kullanılan barındırma modelini kullanmak için. Proje ayarlarsa `<AspNetCoreHostingModel>` kendi proje dosyası bir özellik, özelliğin değerini (`InProcess` veya `OutOfProcess`) kullanılır. Özelliği yoksa, işlem içi olduğu barındırma modeli uygulamanın varsayılan kullanılır. Uygulamayı, uygulamanın barındırma normal modelden farklı ayarı açık bir barındırma modeli gerektiriyorsa, ayarlama **barındırma modeli** ya da `In Process` veya `Out Of Process` gerektiğinde.
1. Profili kaydedin.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

1. Projeye sağ **Çözüm Gezgini**. Seçin **özellikleri**. Açık **hata ayıklama** sekmesi.
1. İçin **profili**seçin **yeni** düğmesi. Profili, "IIS" açılan pencerede adlandırın. Seçin **Tamam** profili oluşturmak için.
1. İçin **başlatma** ayarını seçin **IIS** listeden.
1. Onay kutusunu seçin **tarayıcıyı başlatın** ve uç nokta URL'sini sağlayın.

   Uygulama HTTPS gerektirdiğinde, HTTPS uç noktasının kullanın (`https://`). HTTP için bir HTTP kullan (`http://`) uç noktası.

   Aynı konak adı belirtin ve olarak bağlantı noktası [IIS yapılandırması belirtilen önceki kullanımlar](#configure-iis), genellikle `localhost`.

   URL'nin sonunda bir uygulama adı sağlayın.

   Örneğin, `https://localhost/WebApplication1` (HTTPS) ya da `http://localhost/WebApplication1` (HTTP) geçerli uç nokta URL'leri.
1. İçinde **ortam değişkenlerini** bölümünden **Ekle** düğmesi. Bir ortam değişkeni ile sağlayan bir **adı** , `ASPNETCORE_ENVIRONMENT` ve **değer** , `Development`.
1. İçinde **Web sunucusu ayarlarını** alanı ayarlama **uygulama URL'si** için kullanılan aynı değere **tarayıcıyı başlatın** uç nokta URL'si.
1. İçin **barındırma modeli** ayarı Visual Studio 2019 ya da daha sonra Seç **varsayılan** proje tarafından kullanılan barındırma modelini kullanmak için. Proje ayarlarsa `<AspNetCoreHostingModel>` kendi proje dosyası bir özellik, özelliğin değerini (`InProcess` veya `OutOfProcess`) kullanılır. Özelliği yoksa, işlem dışı olduğu barındırma modeli uygulamanın varsayılan kullanılır. Uygulamayı, uygulamanın barındırma normal modelden farklı ayarı açık bir barındırma modeli gerektiriyorsa, ayarlama **barındırma modeli** ya da `In Process` veya `Out Of Process` gerektiğinde.
1. Profili kaydedin.

::: moniker-end

Ne zaman Visual Studio kullanmayan el ile eklemek için bir başlatma profili [launchSettings.json](http://json.schemastore.org/launchsettings) dosyası *özellikleri* klasör. Aşağıdaki örnek, HTTPS protokolünü kullanmak için profil yapılandırır:

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

Onaylayın `applicationUrl` ve `launchUrl` uç noktaları eşleşen ve aynı protokolü IIS bağlama yapılandırması, HTTP veya HTTPS kullanın.

## <a name="run-the-project"></a>Projeyi Çalıştır

Visual Studio'yu yönetici olarak çalıştırın:

* Aşağı açılan listede derleme yapılandırmasını değerine ayarlandığını onaylayın **hata ayıklama**.
* Çalıştır düğmesini kümesine **IIS** profil ve uygulamayı başlatmak için düğmesini seçin.

Visual Studio'yu yönetici olarak çalıştırarak değil, yeniden başlatma isteyebilir. İstenirse, Visual Studio'yu yeniden başlatın.

Güvenilmeyen bir geliştirme sertifikası kullanıyorsanız, tarayıcı Güvenilmeyen sertifika için bir özel durum oluşturmak gerekli kılabiliriz.

> [!NOTE]
> Yayın hata ayıklama derleme yapılandırması ile [yalnızca kendi kodum](/visualstudio/debugger/just-my-code) ve düzeyi düşürülmüş bir deneyimle derleyici iyileştirmeleri sonuçları. Örneğin, kesme noktaları isabet değildir.

## <a name="additional-resources"></a>Ek kaynaklar

* [IIS'de IIS Yöneticisi'ni kullanmaya başlama](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [Windows IIS üzerinde ASP.NET Core barındırma](xref:host-and-deploy/iis/index)
* [ASP.NET Core modülü için giriş](xref:host-and-deploy/aspnet-core-module)
* [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module)
* [HTTPS'yi Zorunlu Kılma](xref:security/enforcing-ssl)
