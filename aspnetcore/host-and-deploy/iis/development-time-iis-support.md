---
title: ASP.NET Core için Visual Studio 'da geliştirme zamanı IIS desteği
author: guardrex
description: Windows Server 'da IIS ile çalışırken ASP.NET Core uygulamalarda hata ayıklama desteğini bulur.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 704a8dae9da904e4bbdfae0754a6fcdabee6dc82
ms.sourcegitcommit: 2388c2a7334ce66b6be3ffbab06dd7923df18f60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75952029"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>ASP.NET Core için Visual Studio 'da geliştirme zamanı IIS desteği

, [Sourabh Shirhatti](https://twitter.com/sshirhatti) ve [Luke Latham](https://github.com/guardrex) tarafından

Bu makalede, Windows Server 'da IIS ile çalışan ASP.NET Core hata ayıklama için [Visual Studio](https://visualstudio.microsoft.com) desteği açıklanmaktadır. Bu konu başlığı altında, bu senaryonun etkinleştirilmesi ve bir projenin kurulması anlatılmaktadır.

## <a name="prerequisites"></a>Prerequisites

* [Windows için Visual Studio](https://visualstudio.microsoft.com/downloads/)
* **ASP.net ve Web geliştirme** iş yükü
* **.NET Core platformlar arası geliştirme** iş yükü
* X. 509.440 güvenlik sertifikası (HTTPS desteği için)

## <a name="enable-iis"></a>IIS 'yi etkinleştirme

1. Windows 'da, **Programlar ve özellikler** ** > programlar** ve Özellikler ' ** > e gidin** > **Windows özelliklerini açın veya kapatın** (ekranın sol tarafında).
1. **Internet Information Services** onay kutusunu seçin. Seçin **Tamam**.

IIS yüklemesi için sistemin yeniden başlatılması gerekebilir.

## <a name="configure-iis"></a>IIS hizmetini yapılandırma

IIS 'nin aşağıdaki ile yapılandırılmış bir Web sitesine sahip olması gerekir:

* **Ana bilgisayar adı** &ndash; genellikle **varsayılan Web sitesi** `localhost`**ana bilgisayar adıyla** kullanılır. Ancak, benzersiz bir ana bilgisayar adına sahip geçerli bir IIS Web sitesi çalışmaktadır.
* **Site bağlama**
  * HTTPS gerektiren uygulamalar için, sertifika ile 443 numaralı bağlantı noktasına bir bağlama oluşturun. Genellikle **IIS Express geliştirme sertifikası** kullanılır, ancak geçerli bir sertifika çalışıyor olur.
  * HTTP kullanan uygulamalar için, 80 veya yeni bir site için bağlantı noktası 80 ' e bir bağlama oluşturmak için bir bağlamanın mevcut olduğunu onaylayın.
  * HTTP veya HTTPS için tek bir bağlama kullanın. **Aynı anda hem HTTP hem de HTTPS bağlantı noktalarına bağlama desteklenmez.**

## <a name="enable-development-time-iis-support-in-visual-studio"></a>Visual Studio 'da geliştirme zamanı IIS desteğini etkinleştirme

1. Visual Studio yükleyicisi 'ni başlatın.
1. IIS geliştirme zamanı desteği için kullanmayı planladığınız Visual Studio yüklemesi için **Değiştir** ' i seçin.
1. **ASP.net ve Web geliştirme** iş yükü için **geliştirme zamanı IIS destek** bileşeni ' ni bulun ve yükler.

   Bileşen, iş yüklerinin sağındaki **Yükleme ayrıntıları** panelinde, **geliştirme zamanı IIS desteği** altındaki **isteğe bağlı** bölümde listelenmiştir. Bileşen, IIS ile ASP.NET Core uygulamaları çalıştırmak için gerekli yerel bir IIS modülü olan [ASP.NET Core modülünü](xref:host-and-deploy/aspnet-core-module)yüklüyor.

## <a name="configure-the-project"></a>Projeyi yapılandırma

### <a name="https-redirection"></a>HTTPS yönlendirmesi

HTTPS gerektiren yeni bir proje için **yeni ASP.NET Core Web uygulaması oluşturma** penceresinde **https için yapılandırmak** üzere onay kutusunu seçin. Onay kutusunun belirlenmesi, uygulama oluşturulduğunda [https yeniden yönlendirme ve HSTS ara yazılımı](xref:security/enforcing-ssl) ekler.

HTTPS gerektiren mevcut bir proje için, `Startup.Configure`'de HTTPS yeniden yönlendirme ve HSTS ara yazılımı kullanın. Daha fazla bilgi için bkz. <xref:security/enforcing-ssl>.

HTTP kullanan bir proje için, [https yeniden yönlendirme ve HSTS ara yazılımı](xref:security/enforcing-ssl) uygulamaya eklenmez. Uygulama yapılandırması gerekli değildir.

### <a name="iis-launch-profile"></a>IIS başlatma profili

Geliştirme zamanı IIS desteği eklemek için yeni bir başlatma profili oluşturun:

::: moniker range=">= aspnetcore-3.0"

1. **Çözüm Gezgini**projeye sağ tıklayın. Seçin **özellikleri**. **Hata Ayıkla** sekmesini açın.
1. **Profil**için **Yeni** düğmesini seçin. Profili, açılan pencerede "IIS" olarak adlandırın. Profili oluşturmak için **Tamam ' ı** seçin.
1. **Başlatma** ayarı Için listeden **IIS** ' yi seçin.
1. **Başlat tarayıcısı** onay kutusunu seçin ve uç nokta URL 'sini sağlayın.

   Uygulama HTTPS gerektirdiğinde, bir HTTPS uç noktası (`https://`) kullanın. HTTP için bir HTTP (`http://`) uç noktası kullanın.

   [Daha önce belirtilen IIS yapılandırmasıyla](#configure-iis)aynı ana bilgisayar adını ve bağlantı noktasını, genellikle `localhost`sağlayın.

   URL 'nin sonundaki uygulamanın adını belirtin.

   Örneğin, `https://localhost/WebApplication1` (HTTPS) veya `http://localhost/WebApplication1` (HTTP) geçerli uç nokta URL 'Lardır.
1. **Ortam değişkenleri** bölümünde **Ekle** düğmesini seçin. `ASPNETCORE_ENVIRONMENT` **adı** ve `Development`**değeri** olan bir ortam değişkeni sağlayın.
1. **Web sunucusu ayarları** alanında, **Uygulama URL** 'sini **başlatma tarayıcısı** uç noktası URL 'si için kullanılan aynı değere ayarlayın.
1. Visual Studio 2019 veya sonraki sürümlerde **barındırma modeli** ayarı için, proje tarafından kullanılan barındırma modelini kullanmak üzere **varsayılan** ' ı seçin. Proje, proje dosyasında `<AspNetCoreHostingModel>` özelliğini ayarlarsa, özelliğin değeri (`InProcess` veya `OutOfProcess`) kullanılır. Özellik mevcut değilse, uygulamanın varsayılan barındırma modeli, işlem içi kullanılır. Uygulama, uygulamanın normal barındırma modelinden farklı bir açık barındırma modeli ayarı gerektiriyorsa, **barındırma modelini** `In Process` veya `Out Of Process` gerektiği şekilde ayarlayın.
1. Profili kaydedin.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

1. **Çözüm Gezgini**projeye sağ tıklayın. Seçin **özellikleri**. **Hata Ayıkla** sekmesini açın.
1. **Profil**için **Yeni** düğmesini seçin. Profili, açılan pencerede "IIS" olarak adlandırın. Profili oluşturmak için **Tamam ' ı** seçin.
1. **Başlatma** ayarı Için listeden **IIS** ' yi seçin.
1. **Başlat tarayıcısı** onay kutusunu seçin ve uç nokta URL 'sini sağlayın.

   Uygulama HTTPS gerektirdiğinde, bir HTTPS uç noktası (`https://`) kullanın. HTTP için bir HTTP (`http://`) uç noktası kullanın.

   [Daha önce belirtilen IIS yapılandırmasıyla](#configure-iis)aynı ana bilgisayar adını ve bağlantı noktasını, genellikle `localhost`sağlayın.

   URL 'nin sonundaki uygulamanın adını belirtin.

   Örneğin, `https://localhost/WebApplication1` (HTTPS) veya `http://localhost/WebApplication1` (HTTP) geçerli uç nokta URL 'Lardır.
1. **Ortam değişkenleri** bölümünde **Ekle** düğmesini seçin. `ASPNETCORE_ENVIRONMENT` **adı** ve `Development`**değeri** olan bir ortam değişkeni sağlayın.
1. **Web sunucusu ayarları** alanında, **Uygulama URL** 'sini **başlatma tarayıcısı** uç noktası URL 'si için kullanılan aynı değere ayarlayın.
1. Visual Studio 2019 veya sonraki sürümlerde **barındırma modeli** ayarı için, proje tarafından kullanılan barındırma modelini kullanmak üzere **varsayılan** ' ı seçin. Proje, proje dosyasında `<AspNetCoreHostingModel>` özelliğini ayarlarsa, özelliğin değeri (`InProcess` veya `OutOfProcess`) kullanılır. Özellik mevcut değilse, uygulamanın varsayılan barındırma modeli kullanılır ve bu işlem, işlem dışı olur. Uygulama, uygulamanın normal barındırma modelinden farklı bir açık barındırma modeli ayarı gerektiriyorsa, **barındırma modelini** `In Process` veya `Out Of Process` gerektiği şekilde ayarlayın.
1. Profili kaydedin.

::: moniker-end

Visual Studio kullanmadığınız durumlarda, *Özellikler* klasöründeki [launchsettings. JSON](https://json.schemastore.org/launchsettings) dosyasına el ile bir başlatma profili ekleyin. Aşağıdaki örnek, HTTPS protokolünü kullanmak için profili yapılandırır:

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

`applicationUrl` ve `launchUrl` uç noktalarının eşleştiğini ve IIS bağlama yapılandırmasıyla aynı Protokolü (HTTP veya HTTPS) kullandığını doğrulayın.

## <a name="run-the-project"></a>Projeyi çalıştırma

Visual Studio 'Yu yönetici olarak çalıştırın:

* Derleme yapılandırması açılır listesinin **hata ayıklama**olarak ayarlandığını onaylayın.
* [Hata ayıklamayı Başlat düğmesini](/visualstudio/debugger/debugger-feature-tour) **IIS** profiline ayarlayın ve uygulamayı başlatmak için düğmeyi seçin.

Visual Studio, yönetici olarak çalışmıyorsa bir yeniden başlatma isteyebilir. İstenirse, Visual Studio 'Yu yeniden başlatın.

Güvenilmeyen bir geliştirme sertifikası kullanılırsa, tarayıcı güvenilmeyen sertifika için bir özel durum oluşturmanız gerekebilir.

> [!NOTE]
> [Yalnızca kendi kodum](/visualstudio/debugger/just-my-code) ve derleyici Iyileştirmeleriyle yayın derleme yapılandırması hata ayıklaması, düzeyi düşürülmüş bir deneyimle sonuçlanır. Örneğin, kesme noktaları isabet edilmez.

## <a name="additional-resources"></a>Ek kaynaklar

* [IIS 'de IIS Yöneticisi 'Ni kullanmaya başlama](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* <xref:security/enforcing-ssl>
