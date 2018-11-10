---
title: ASP.NET Core Modülü
author: guardrex
description: ASP.NET Core modülü Kestrel web sunucusu, bir ters proxy sunucusu olarak IIS veya IIS Express kullanacak şekilde nasıl olanak tanıdığını öğrenin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 39c1b364f9dab635c79e00561d212c858c0c4395
ms.sourcegitcommit: 09affee3d234cb27ea6fe33bc113b79e68900d22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191262"
---
# <a name="aspnet-core-module"></a>ASP.NET Core Modülü

Tarafından [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), ve [Chris Ross](https://github.com/Tratcher)

::: moniker range=">= aspnetcore-2.2"

ASP.NET Core modülü ASP.NET Core uygulamalarını bir IIS çalışan işlemindeki çalışmasına izin verir. (*işlem içi*) veya bir ters proxy yapılandırması IIS'de arkasında (*işlem dışı*). IIS, Gelişmiş bir web uygulaması güvenlik ve yönetilebilirlik özellikleri sağlar.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

ASP.NET Core, ASP.NET Core modülü IIS bir ters proxy yapılandırmasında çalıştırılacak uygulamaları sağlar. IIS, Gelişmiş bir web uygulaması güvenlik ve yönetilebilirlik özellikleri sağlar.

::: moniker-end

Desteklenen Windows sürümleri:

* Windows 7 veya üzeri
* Windows Server 2008 R2 veya üzeri

::: moniker range=">= aspnetcore-2.2"

İşlem içi barındırırken modülü, kendi sunucu uygulamaya sahip `IISHttpServer`.

İşlem dışı barındırırken, modülü yalnızca Kestrel ile çalışır. Uyumlu bir modüldür [HTTP.sys](xref:fundamentals/servers/httpsys) (eski adıyla [WebListener](xref:fundamentals/servers/weblistener)).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Modül yalnızca Kestrel ile çalışır. Uyumlu bir modüldür [HTTP.sys](xref:fundamentals/servers/httpsys) (eski adıyla [WebListener](xref:fundamentals/servers/weblistener)).

::: moniker-end

## <a name="aspnet-core-module-description"></a>ASP.NET Core modülü açıklaması

::: moniker range=">= aspnetcore-2.2"

ASP.NET Core modülü ya da IIS kanala takılan yerel bir IIS modülüdür:

* IIS çalışan işlemi içinde ASP.NET Core uygulaması ana bilgisayar (`w3wp.exe`) adlı [işlem içi barındırma modeli](#in-process-hosting-model).

* Arka uç, çalışan bir ASP.NET Core uygulaması için web istekleri iletmek [Kestrel sunucu](xref:fundamentals/servers/kestrel)adlı [işlem dışı barındırma modeli](#out-of-process-hosting-model).

### <a name="in-process-hosting-model"></a>İşlem içi barındırma modeli

İşlemdeki barındırma, bir ASP.NET Core kullanarak uygulama IIS çalışan işlemi ile aynı işlemde çalıştırır. Bu proxy isteklerin performans cezası geri döngü bağdaştırıcısından giden işlem barındırma modeli kullanılırken kaldırır. IIS işleme Süreci Yönetimi ile [Windows İşlem Etkinleştirme Hizmeti (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

ASP.NET Core Modülü:

* Uygulama başlatmayı gerçekleştirir.
  * Yükleri [CoreCLR](/dotnet/standard/glossary#coreclr).
  * Çağrıları `Program.Main`.
* Yerel IIS istek ömrünü işler.

Aşağıdaki diyagram IIS, ASP.NET Core modülü arasındaki ilişkiyi gösterir ve uygulama işlemde barındırılan:

![ASP.NET Core Modülü](aspnet-core-module/_static/ancm-inprocess.png)

Bir istek için çekirdek modu HTTP.sys sürücüsünü Web'den ulaşır. Sürücü IIS Web sitesinin yapılandırılan bağlantı noktası, genellikle 80 (HTTP) veya 443 (HTTPS) üzerinde yerel istek yönlendirir. Modülü yerel isteği alır ve denetimi geçirir `IISHttpServer`, olduğu ne isteği Yerelden yönetilene dönüştürür.

Sonra `IISHttpServer` çekme isteği, istek ASP.NET Core ara yazılım ardışık düzende gönderildi. Ara yazılım ardışık düzenini isteği işler ve olarak geçirir bir `HttpContext` örneği uygulama mantığına. Uygulamanın yanıt IIS, yeniden istek başlatılan HTTP istemcisi için hangi bildirim geçirilir.

### <a name="out-of-process-hosting-model"></a>İşlem dışı barındırma modeli

Bir işlem içinde çalıştırmak, ASP.NET Core uygulamaları IIS çalışan işleminden ayrı olduğundan, işlem yönetimi modül işler. İlk istek ulaştığında ve kapatılır veya çöküyor uygulama yeniden başlatmalarını modülü ASP.NET Core uygulaması için bir işlem başlar. Bu aslında aynı işlemde çalışan tarafından yönetilen uygulamalarla görüldüğü gibi davranıştır [Windows İşlem Etkinleştirme Hizmeti (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Aşağıdaki diyagram IIS, ASP.NET Core modülü arasındaki ilişkiyi gösterir ve uygulama barındırılan işlem dışı:

![ASP.NET Core Modülü](aspnet-core-module/_static/ancm-outofprocess.png)

İstekleri için çekirdek modu HTTP.sys sürücüsünü Web'den ulaşır. Sürücü istekler IIS Web sitesinin yapılandırılan bağlantı noktası, genellikle 80 (HTTP) veya 443 (HTTPS) üzerinde yönlendirir. Modülün istekleri Kestrel rastgele bir bağlantı için bağlantı noktası değil uygulamanın iletir 80/443'tür.

Modülü başlatma sırasında bir ortam değişkeni aracılığıyla bağlantı noktasını belirtir ve IIS tümleştirme ara yazılımı üzerinde dinlemek üzere yapılandırır `http://localhost:{port}`. Ek denetimler gerçekleştirilir ve modülünden değilsiniz kaynaklı istekler reddedilir. İstekler HTTP üzerinden HTTPS üzerinden IIS tarafından alınan bile iletilir modülü HTTPS iletmeyi desteklemez.

Modül istekten Kestrel seçer sonra ASP.NET Core ara yazılım ardışık düzende isteği gönderilir. Ara yazılım ardışık düzenini isteği işler ve olarak geçirir bir `HttpContext` örneği uygulama mantığına. IIS tümleştirme tarafından eklenen bir ara yazılım istek için Kestrel iletmek için hesap için şema, uzak IP ve pathbase güncelleştirir. Uygulamanın yanıt IIS, yeniden istek başlatılan HTTP istemcisi için hangi bildirim geçirilir.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

ASP.NET Core modülü, uygulama arka ucu ASP.NET Core web isteklerini iletmek için IIS ardışık düzende takılan yerel bir IIS modülüdür.

Bir işlem içinde çalıştırmak, ASP.NET Core uygulamaları IIS çalışan işleminden ayrı olduğundan, modül işlem yönetimi da işler. İlk istek ulaştığında ve onu bir çökme gerçekleşirse, uygulama yeniden başlatmalarını modülü ASP.NET Core uygulaması için bir işlem başlar. Bu aslında aynı işlemde çalışan ASP.NET 4.x uygulamalar ile görüldüğü gibi davranıştır tarafından yönetildiği IIS'de [Windows İşlem Etkinleştirme Hizmeti (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Aşağıdaki diyagramda, IIS, ASP.NET Core modülü ve uygulama arasındaki ilişki gösterilmektedir:

![ASP.NET Core Modülü](aspnet-core-module/_static/ancm-outofprocess.png)

İstekleri için çekirdek modu HTTP.sys sürücüsünü Web'den ulaşır. Sürücü istekler IIS Web sitesinin yapılandırılan bağlantı noktası, genellikle 80 (HTTP) veya 443 (HTTPS) üzerinde yönlendirir. Modülün istekleri Kestrel rastgele bir bağlantı için bağlantı noktası değil uygulamanın iletir 80/443'tür.

Modülü başlatma sırasında bir ortam değişkeni aracılığıyla bağlantı noktasını belirtir ve IIS tümleştirme ara yazılımı üzerinde dinlemek üzere yapılandırır `http://localhost:{port}`. Ek denetimler gerçekleştirilir ve modülünden değilsiniz kaynaklı istekler reddedilir. İstekler HTTP üzerinden HTTPS üzerinden IIS tarafından alınan bile iletilir modülü HTTPS iletmeyi desteklemez.

Modül istekten Kestrel seçer sonra ASP.NET Core ara yazılım ardışık düzende isteği gönderilir. Ara yazılım ardışık düzenini isteği işler ve olarak geçirir bir `HttpContext` örneği uygulama mantığına. IIS tümleştirme tarafından eklenen bir ara yazılım istek için Kestrel iletmek için hesap için şema, uzak IP ve pathbase güncelleştirir. Uygulamanın yanıt IIS, yeniden istek başlatılan HTTP istemcisi için hangi bildirim geçirilir.

::: moniker-end

Windows kimlik doğrulaması gibi birçok yerel modül etkin kalır. IIS modüllerini etkin modülüyle hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/iis/modules>.

ASP.NET Core modülü birkaç diğer işlevlere sahiptir. Modül yapabilirsiniz:

* Çalışan işlemi için ortam değişkenlerini ayarlayın.
* Çıkış başlatma sorunlarını gidermek için dosya depolama için stdout'u günlüğe kaydeder.
* Windows kimlik doğrulama belirteçlerinizi iletin.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>Yükleme ve ASP.NET Core modülü kullanın

Yükleme ve ASP.NET Core modülü kullanma konusunda ayrıntılı yönergeler için bkz <xref:host-and-deploy/iis/index>. Modül yapılandırma hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module>.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* [ASP.NET Core modülü GitHub deposu (kaynak kodu)](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
